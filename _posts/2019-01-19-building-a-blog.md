---
layout: post
title: "Building a blog with Jekyll, Chalk, s3_website and Docker"
description: "Docker で Jekyll (template は Chalk) と s3_website を動かして build した blog を S3 に hosting して CloudFront で配信する環境を整える手順の記録。"
tags: [jekyll, docker, web]
updated: 2019-12-30
---

#### Table of Contents
{:.no_toc}

* A markdown unordered list which will be replaced with the ToC, excluding the "Contents header" from above
{:toc}

### Environment
- macOS 10.14.6
  - Docker の host として使うだけなのでほとんど関係ない
- [Docker Desktop for Mac](https://docs.docker.com/docker-for-mac/) 2.1.0.5 (40693) 
  - Jekyll の環境を container に閉じ込めるために利用。そうする理由は、Jekyll の環境をある程度 portable にするため、及び、host の環境に影響を与えないため。
- [Jekyll](https://jekyllrb.com/)
  - static な website を build する engine 。
  - 今回は Chalk に含まれているものを使ったので、Jekyll のみを独立して install することはおこなっていない。
- [Chalk](https://github.com/nielsenramon/chalk) [960d99f](https://github.com/nielsenramon/chalk/commit/960d99faa3e9ce5bee7854623807408b04e6da79)
  - Jekyll の template 。Jekyll そのものも含んでいるので今回はそれを使った。

### Variables
以降登場する次の値は任意なので状況 (例えば別 blog を作成するなど) に応じて置き換え。
- `~/dev/docker/jekyll`: Jekyll を動かす Docker container の Dockerfile と docker-compose.yml 用の directory 。'docker-compose' command はこの directory で入力する。
- `~/dev/jekyll/regrets`: blog の contents 用の directory 。
- `/var/jekyll/regrets`: 上記 `~/dev/jekyll/regrets` を mount する、Docker container 内での directory 。
  - 実際にはそのひとつ上の /dev/jekyll を /var/jekyll に mount している。この下に blog 毎に directory が出来ていく想定。
- `8496`: local での確認用に jekyll 組み込みの web server を動かす port 番号。
- `regrets.cloudeleven.space`: blog を公開する domain 。S3 の bucket もこの名前で作った。

## Build the docker image
{% highlight shell_session %}
$ mkdir -p ~/dev/docker
$ cd ~/dev/docker
$ git clone https://github.com/cloudeleven/docker-jekyll jekyll
$ cd jekyll
$ docker-compose build
{% endhighlight %}
Dockerfile の中で、package 'zlib1g-dev', 'npm', 'locales' を入れたり locale の care をしているのは、これら無しでは Chalk で error になったからであり、Jekyll を素で使うには (少なくとも使い始めるには) 不要。

## Set up Chalk
{% highlight terminal %}
$ mkdir -p ~/dev/jekyll
$ cd ~/dev/jekyll/
$ git clone https://github.com/nielsenramon/chalk.git regrets
$ cd regrets
$ mkdir .bundle
$ echo 'BUNDLE_PATH: "vendor/bundle"' >> .bundle/config
$ cd ~/dev/docker/jekyll
$ docker-compose run --rm -w /var/jekyll/regrets jekyll npm run setup
$ docker-compose run --rm -w /var/jekyll/regrets --service-ports jekyll bundle exec jekyll serve --port 8496 --host 0.0.0.0 --watch --force_polling --drafts --incremental
{% endhighlight %}

最後の command で web server が立ち上がるので、次の点を確認。
* browser で http://localhost:8496/ が見える。
* ~/dev/jekyll/regrets/_posts/2017-12-23-introducing-chalk.md の本文を適当に変更して少し待つと http://localhost:8496/posts/2017/12/23/introducing-chalk に変更が反映される。
問題なければ blog を書いて、local で preview する環境は整ったことになる。


### Remove all .html ext of posts whenever site built for production
`.html` という拡張子をつけたくないので、production 向けに build する時に拡張子が削除されるようにする。

- references
  - [Upload Jekyll blogs to S3 without .html ext](https://jcxia.com/blog/Hosting-Jekyll-blogs-on-S3-without-html-ext "JC writes - jcxia.com")


{% highlight console %}
$ cd ~/dev/jekyll/regrets
$ mkdir _plugins
$ vi _plugins/prettyurl.rb
$ cat _plugins/prettyurl.rb
require 'fileutils'

# Remove all .html ext of posts, for pretty url and S3 restriction
# Production only

Jekyll::Hooks.register :posts, :post_write do |post|
  if Jekyll.env == 'production'
    path = post.destination('/')
    FileUtils.mv(path, path.sub(/\.html$/, ''))
  end
end
$ cd ~/dev/docker/jekyll
$ docker-compose run --rm -w /var/jekyll/regrets -e JEKYLL_ENV=production jekyll bundle exec jekyll build
$ ls -l ~/dev/jekyll/regrets/_site/posts
total 104
-rw-r--r--  1 m4c  staff  12346 10 22 22:30 chalk-sample-post-with-all-elements
-rw-r--r--  1 m4c  staff  19563 10 22 22:30 configuring-chalk
-rw-r--r--  1 m4c  staff  12530 10 22 22:30 introducing-chalk
{% endhighlight %}

拡張子なしの file が出来ていれば ok 。


### Set up DISQUS

Jekyll で作る blog は static であるがゆえに独自の comment system を持たないため、外部のものを利用する。
Chalk は元々 [DISQUS](https://disqus.com/ "DISQUS - disqus.com") を integrate してあるので、DISQUS で site の設定を作って、作成された ID を _config.yml に書くだけ。

{% highlight console %}
$ vi _config.yml
$ git diff _config.yml
diff --git a/_config.yml b/_config.yml
index a77c5b9..7e4f410 100755
--- a/_config.yml
+++ b/_config.yml
@@ -10,7 +10,7 @@ url: # add your site url (format: https://example.com)
 
 about_enabled: false # Change to true if you wish to show an icon in the navigation that redirects to the about page
 baseurl: # Set if blog doesn't sit at the root of the domain (format: /blog)
-discus_identifier: # Add your Disqus identifier
+discus_identifier: regrets
 ga_analytics: # Add your GA Tracking Id
 local_fonts: false # Change to true if you wish to use local fonts
 rss_enabled: true # Change to false if not
{% endhighlight %}


## Set up AWS

S3 で hosting, CloudFront で配信するので、そのための準備。

- references
  - [Jekyll on S3 and CloudFront using s3_website](https://tjwallace.ca/2017-05-16/jekyll-on-s3-and-cloudfront/ "TJ WALLACE ー tjwallace.ca")


### IAM

以下、用途別にふたつの group を作成し、権限を attach 。それぞれに新規 user をひとつずつ追加した。

- s3_website 用
  - [Setting up AWS credentials](https://github.com/laurilehmijoki/s3_website/blob/master/additional-docs/setting-up-aws-credentials.md) に従って作った CloudFront に対する全権と S3 の blog 用の bucket に対する権限を持った group 。
  - 追加した user の access key を作成して access key ID と access key secret を控えておく。
- 作業用
  - blog 用の作業を実行するための group 。基本的には S3 と CloudFront の権限があれば良いはずだけど、今回は、他で運用していた既存の domain を移したり、新たに証明書を作ったりする必要があったので Route 53 と Certificate Manager の権限も付けた。
  - 以降の AWS での作業はこの user で行う。


### S3

bucket 作成。以下以外は default で。

- `bucket name`: regrets.cloudeleven.space
- `region`: Tokyo
- `Bucket Policy`: 以下

{% highlight json %}
{
  "Version":"2012-10-17",
  "Statement":[{
    "Sid": "PublicReadForGetBucketObjects",
    "Effect": "Allow",
    "Principal": "*",
    "Action": ["s3:GetObject"],
    "Resource": ["arn:aws:s3:::regrets.cloudeleven.space/*"]
  }]
}
{% endhighlight %}

Static website hosting の設定とかしたくなるけど、後で s3_website がやってくれるのでここではしなくて良い。


### Certificate Manager
- references 
  - [Zone Apex環境でワイルドカード証明書を適用するときの注意点](https://kosukety.org/notes-when-applying-the-wildcard-certificate-in-zone-apex/ "kosukety blog - kosukety.org")
  - [CloudFrontとACM連携でハマった話。](http://iryond.hatenablog.com/entry/2016/10/08/130918 "なるべくシンプルに - iryond.hatenablog.com")

今回は *.cloudeleven.space で証明書を作った。
region が `US East (N. Virginia)` であることを確認して次のように設定

- `Domain name`: *.cloudeleven.space
- `Another name`: cloudeleven.space

確認の mail が来るので対応。


### CloudFront

次の設定で distribution を作成。

- `Delivery Method`: Web
- `Origin Settings`
  - `Origin Domain Name`: regrets.cloudeleven.space.s3-website-ap-northeast-1.amazonaws.com
    - dropdown される list からは `選択しない` ことに注意。[ウェブサイトエンドポイント](https://docs.aws.amazon.com/ja_jp/AmazonS3/latest/dev/WebsiteEndpoints.html "aws - docs.aws.amazon.com") を参考に。
  - `Origin ID`: regrets.cloudeleven.space
- `Default Cache Behavior Settings`
  - `Viewer Protocol Policy`: Redirect HTTP to HTTPS
- `Distribution Settings`
  - `Price Class`: Use U.S., Canada, Europe, Asia and Africa
  - `Alternate Domain Names (CNAMEs)`: regrets.cloudeleven.space
  - `SSL Certificate`: Custom SSL Certificate (example.com): 作成した証明書を選択

Status が Deployed になるのを待つ。割とかかる:coffee:。

### Route 53

今回は既に [value-domain](https://www.value-domain.com/ "value-domain - www.value-domain.com") で購入・管理している domain の管理を Route 53 に移した上で運用したかったので、そのための作業を次の page を参考にして行なった。

- references
  - [Value Domainで購入したドメインをAWSのRoute53で使用する](http://yamada.daiji.ro/blog/?p=87 "しびら - yamada.daiji.ro")
  - [【ネームサーバ設定】ValueDomainのネームサーバからAWSのRoute53を使用するよう変更](https://qiita.com/azusanakano/items/2169e26c4d5de8447b96 "Qiita - qiita.com")
  - [はじめてのRoute 53](https://www.dondari.com/%E3%81%AF%E3%81%98%E3%82%81%E3%81%A6%E3%81%AERoute_53 "dondari - www.dondari.com")
  - [AWS Route53 各レコードの登録方法](https://qiita.com/mmmm/items/0feaf5bf6056614df5b8 "Qiita - qiita.com")
  - [set Google Apps SPF record in Amazon AWS Route 53](https://serverfault.com/questions/332019/set-google-apps-spf-record-in-amazon-aws-route-53 "serverfault - serverfault.com")


で、blog 用に作成した Hosted Zone の下に Record Set を作る。Type 以外は同じ設定で IPv4 用と IPv6 用のふたつ。

- `Name`: regrets.cloudeleven.space.
- `Type`:
  - IPv4: A – IPv4 address.
  - IPv6: AAAA – IPv6 address.
- `Alias`: Yes
- `Alias Target`: — CloudFront distributions — から CloudFront で作成したものを選ぶ
- `Routing Policy`： Simple
- `Evaluate Target Health`: No


## Set up s3_website

{% highlight console %}
$ cd ~/dev/docker/jekyll
$ docker-compose run --rm -w /var/jekyll/regrets jekyll s3_website cfg create
$ cd ~/dev/jekyll/regrets
$ cp -i s3_website.yml{,.orig}
$ vi s3_website.yml
$ diff s3_website.yml{.orig,}
4,6c4,6
< s3_id: YOUR_AWS_S3_ACCESS_KEY_ID
< s3_secret: YOUR_AWS_S3_SECRET_ACCESS_KEY
< s3_bucket: your.blog.bucket.com
---
> s3_id: <%= ENV['s3_id'] %>
> s3_secret: <%= ENV['s3_secret'] %>
> s3_bucket: <%= ENV['s3_bucket'] %>
30c30
< # s3_endpoint: ap-northeast-1
---
> s3_endpoint: ap-northeast-1
40c40
< # cloudfront_distribution_id: your-dist-id
---
> cloudfront_distribution_id: <%= ENV['cloudfront_distribution_id'] %>
42,48c42,50
< # cloudfront_distribution_config:
< #   default_cache_behavior:
< #     min_ttl: <%= 60 * 60 * 24 %>
< #   aliases:
< #     quantity: 1
< #     items:
< #       - your.website.com
---
> cloudfront_distribution_config:
>   default_cache_behavior:
>     min_ttl: <%= 60 * 60 * 24 %>
>   aliases:
>     quantity: 1
>     items:
>       - <%= ENV['cloudfront_alias1'] %>
> max_age: 120
> gzip: true

$ vi .env
$ cat .env
s3_id=IAM で s3_website 用に作った user の access key ID
s3_secret=IAM で s3_website 用に作った user の access key secret
s3_bucket=regrets.cloudeleven.space
cloudfront_distribution_id=CloudFront の distribution id
cloudfront_alias1=regrets.cloudeleven.space

$ cd ~/dev/docker/jekyll
$ docker-compose run --rm -w /var/jekyll/regrets jekyll s3_website cfg apply
{% endhighlight %}


不必要な file が upload されないための設定だけ済ませてから一旦試しに upload してみる。

{% highlight console %}
$ cd ~/dev/jekyll/regrets
$ cp -ip _config.yml{,.orig}
$ vi _config.yml
$ diff _config.yml{.orig,}
76a77,81
>   - .bundle/
>   - .env
>   - s3_website.yml
>   - s3_website.yml.orig
>   - _config.yml.orig
$ cd ~/dev/docker/jekyll
$ docker-compose run --rm -w /var/jekyll/regrets -e JEKYLL_ENV=production jekyll bundle exec jekyll build
$ docker-compose run --rm -w /var/jekyll/regrets jekyll s3_website push
{% endhighlight %}

browser で https://regrets.cloudeleven.space/ が見えることと、S3 に上がっちゃいけないものが上がってないことを確認。


## Change repository's origin

ひと通り基本的な準備が出来たので、blog を丸ごと git で管理出来るようにする。

Chalk を clone した directory がそのまま自分の blog の source になるので、commit 可能な remote を追加するだけでも良いのだけど、何となく気持ち悪いので自分の remote を origin にしておく。
Chalk の remote を別名で残しておくのは、Chalk の更新に追従しやすくするため。

先に remote 先となる repository を作成しておいてから、

{% highlight console %}
$ cd ~/dev/jekyll/regrets
$ git remote add chalk git@github.com:nielsenramon/chalk.git
$ git remote set-url origin git@github.com:cloudeleven/regrets.git
$ git remote -v
chalk   git@github.com:nielsenramon/chalk.git (fetch)
chalk   git@github.com:nielsenramon/chalk.git (push)
origin  git@github.com:cloudeleven/regrets.git (fetch)
origin  git@github.com:cloudeleven/regrets.git (push)
{% endhighlight %}

確認。念の為変更なしの状態で一旦試す。

{% highlight console %}
$ git stash save
$ git branch -a
* master
  remotes/origin/HEAD -> origin/master
  remotes/origin/gh-pages
  remotes/origin/master
  remotes/origin/website
$ git fetch chalk
$ git branch -a
* master
  remotes/chalk/gh-pages
  remotes/chalk/master
  remotes/chalk/website
  remotes/origin/HEAD -> origin/master
  remotes/origin/gh-pages
  remotes/origin/master
  remotes/origin/website
$ git push -u origin master
{% endhighlight %}

期待通りに動くことが確認出来たら、ここまでの変更を commit, push 。

{% highlight console %}
$ git stash pop
$ vi README.md # 内容を適当に変えておく
$ vi .gitignore
$ git diff .gitignore
diff --git a/.gitignore b/.gitignore
index e91e057..f0d223f 100644
--- a/.gitignore
+++ b/.gitignore
@@ -13,3 +13,8 @@ Gemfile.lock
 package-lock.json
 yarn.lock
 .idea
+.env
+_config.yml.orig
+s3_website.yml.orig
+vendor/
+_drafts/

$ git add -A
$ git commit -m 'add and modify files for bundler, plugins and w3_website.'
$ git push
{% endhighlight %}




## List of the commands

環境が出来たら、以下の command を繰り返して entry を書いたり、徐々に好みに近付けていくことを繰り返すだけ。

#### local preview
{% highlight terminal %}
docker-compose run --rm -w /var/jekyll/regrets --service-ports jekyll bundle exec jekyll serve --port 8496 --host 0.0.0.0 --watch --force_polling --drafts --incremental
{% endhighlight %}
`--draft` と `--incremental` は状況に応じて。

#### build
{% highlight terminal %}
docker-compose run --rm -w /var/jekyll/regrets -e JEKYLL_ENV=production jekyll bundle exec jekyll build
{% endhighlight %}

#### publish
{% highlight terminal %}
docker-compose run --rm -w /var/jekyll/regrets jekyll s3_website push
{% endhighlight %}


