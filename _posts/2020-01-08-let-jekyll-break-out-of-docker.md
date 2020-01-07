---
layout: post
title: "Let Jekyll break out of Docker"
description: "Jekyll を Docker で動かしていたけど、遅くて話にならないのでやめた。"
thumb_image: "documentation/chalk-docker-before-after.png"
tags: [jekyll, web]
---

{% include image.html path="documentation/chalk-docker-before-after.png" path-detail="documentation/chalk-docker-before-after@2x.png" alt="Chalk intro" %}

#### Table of Contents
{:.no_toc}

* A markdown unordered list which will be replaced with the ToC, excluding the "Contents header" from above
{:toc}

## Conclusion

- 普通に mac で build するようにした。取り敢えず、超快適。
  - 環境を portable にしたくて Docker で動かしていたのだけど、次の理由でその意味が判らなくなった。
    - blog の環境を丸ごと git で管理できている。
    - そもそも環境を作るのが大した手間ではない。
    - 仮に Docker を経由させることにどんな利点があったとしても、その遅さを我慢するのに釣り合うものではない。
      - 後述するけど、とにかく遅くて書く気にならない…。
  - Jekyll に限らず Docker で動かすと遅いという話は色々あるようで、原因のいくつかには対処方法もあるらしいのだけど、それを試してまでこの環境を維持する動機がない。
    - 何故なら、そもそも本当はもう Jekyll をやめたい。[Gatsby](https://www.gatsbyjs.org/) で作り直したいのだが、それが出来るまで blog を書かない、ということにしてしまうと次の entry がまた 1 年後になってしまうので、移行計画は進めつつ当面は Jekyll でしのぐことにした。
- 別方向の知見として、分割出来ないひとかたまりの処理に長い時間がかかるのはやはりとにかく駄目、ということを再確認した。
  - ひとつの処理に長い時間がかかると試行錯誤もままならないし、最終的な検証をする気もなくなる。本当に良いことがない。

## Before and After

- 冒頭の画像は左右それぞれ Docker を経由した時とそうでない時の、local preview を起動するまでにかかる時間の比較なんだけど、mac で普通に動かすと 5 秒以内で終わるところ、Docker を経由させると 127 秒かかっている。酷い。
- とは言え、遅いのが最初だけなら対処のしようはあるのだけど、その後も遅い様子が次の動画。
  - この動画では上側で動いているのがすべて Docker 経由、下側で動いているのがすべて mac で直に動かしているもの。
  - 左の terminal では、それぞれの環境の css を同時に変更して保存している。
  - 中央の terminal では、それぞれの Jekyll の実行時の log が流れている。
  - 右の browser では live reload されて変更が反映される様子が見える。

<div class="embed-responsive embed-responsive-16by9">
  <iframe src="https://www.youtube.com/embed/NwCtfyIP48w" frameborder="0" allow="accelerometer" allowfullscreen></iframe>
</div>

- 酷い。これ、文章を書いている時は Markdown を書くだけだから頻繁に preview する必要もなくまだ我慢のしようもあるんだけど、css の値を試行錯誤しながら調整する、みたいなことはまったく出来ない。やる気を無くする。

## Memorandum

Docker 抜きの環境に変更した際の手順の記録。

#### Environment
- macOS 10.14.6
- [Chalk](https://github.com/nielsenramon/chalk) [960d99f](https://github.com/nielsenramon/chalk/commit/960d99faa3e9ce5bee7854623807408b04e6da79)
  - 今回の移行では実際には、この commit 時点での Chalk を取り込んだ [自分の repository](https://github.com/cloudeleven/regrets) を利用した。

#### Variables
以降登場する次の値は任意なので状況 (例えば別 blog を作成するなど) に応じて置き換え。
- `~/dev/jekyll/regrets_anyenv`: blog の contents 用の directory 。

### Clone repository
- 以下では既存の自分の repository から clone しているが、素の Chalk から始める場合は、Chalk の repository から。その場合も以降はほぼ同じ手順でいけるはず。

{% highlight shell_session %}
$ cd ~/dev/jekyll
$ git clone git@github.com:cloudeleven/regrets.git regrets_anyenv
{% endhighlight %}

### Install Ruby and Node, then setup Chalk
- ruby と node の install はそれぞれ rbenv, nodenv を使い、rbenv, nodenv は anyenv から入れる前提。
- ruby の version を指定しているのは、なるべく同じ条件で比較したかったから。
  - なのに node の version には気が回らなかった…。12.7.0 というのはたぶんこの作業をした時の最新。

{% highlight shell_session %}
$ anyenv update
$ anyenv git pull
$ rbenv install 2.5.1
$ # まだやってなければ
$ anyenv install nodenv
$ # まだやってなければ
$ git clone https://github.com/pine/nodenv-yarn-install.git "$(nodenv root)/plugins/nodenv-yarn-install"
$ # plugin の力で yarn を入れたいので、使おうとしている version が既に install 済みなら
$ nodenv uninstall 12.7.0
$ nodenv install 12.7.0
$ exec $SHELL -l
$ cd regrets_anyenv
$ rbenv local 2.5.1
$ rbenv versions       # 確認
$ nodenv local 12.7.0
$ nodenv versions      # 確認
$ vi .env              # あるいは既にどこかにあるならそこから持ってくる
$ # 素の Chalk から始めた場合等、以下がまだ行なわれていない場合、.bundle に path を書く
$ # mkdir .bundle
$ # echo 'BUNDLE_PATH: "vendor/bundle"' >> .bundle/config 
$ # 以下では既存の gem が install された際の bundler の version を調べ、同じ version の bundler を install しているが、
$ # 素の Chalk から始めた場合等、初めて npm run setup する場合は単に gem install bundler すれば良い
$ tail -n2 Gemfile.lock
BUNDLED WITH
   2.1.2
$ gem install bundler -v 2.1.2
$ npm run setup
$ bundle exec jekyll serve --livereload           # local preview 環境が立ち上がることを確認
$ JEKYLL_ENV=production bundle exec jekyll build  # build 結果を確認
{% endhighlight %}

### Install Java for s3_website
- s3_website は Java に依存しているので、mac に Java の導入が必要。
- 最初、無邪気に `brew cask install java` とかやったら version 13 が入って、それだと s3_website が動かなかったので、Docker で動かしていた時に合わせて version 8 を入れている。
- project 毎に Java の version を使い分けやすいように jenv を使った。
- s3_website や、それが依存する AWS の設定に関しては [Docker 利用を前提として Jekyll を導入した際の entry]({% post_url 2019-01-19-building-a-blog %}) を参照。この設定は Docker 利用の有無に影響されない。

{% highlight terminal %}
$ brew install jenv
$ echo 'export PATH="$HOME/.jenv/bin:$PATH"' >> ~/.zshrc
$ echo 'eval "$(jenv init -)"' >> ~/.zshrc
$ exec $SHELL -l
$ brew tap AdoptOpenJDK/openjdk
$ brew cask install adoptopenjdk8
$ /usr/libexec/java_home -V        # 現状確認
Matching Java Virtual Machines (2):
    13.0.1, x86_64:     "OpenJDK 13.0.1"        /Library/Java/JavaVirtualMachines/openjdk-13.0.1.jdk/Contents/Home
    1.8.0_232, x86_64:  "AdoptOpenJDK 8"        /Library/Java/JavaVirtualMachines/adoptopenjdk-8.jdk/Contents/Home

/Library/Java/JavaVirtualMachines/openjdk-13.0.1.jdk/Contents/Home
$ jenv add /Library/Java/JavaVirtualMachines/adoptopenjdk-8.jdk/Contents/Home
$ jenv local 1.8.0.232
$ exec $SHELL -l
$ jenv versions      # 確認
  system (set by /Users/m4c/.jenv/version)
  1.8
* 1.8.0.232
  13.0
  13.0.1
  openjdk64-1.8.0.232
  openjdk64-13.0.1
$ echo ${JAVA_HOME}  # 確認
/Users/m4c/.jenv/versions/1.8.0.232
$ gem install --no-document s3_website
$ # 現状で publish して良い状態なら
$ JEKYLL_ENV=production bundle exec jekyll build
$ s3_website push
{% endhighlight %}


## Digression

完全に余談だけど、冒頭の画像の結果を出すための script を書くのにとても苦労したので、その記録を残しておく。

どちらの script もやりたいことは同じで、Jekyll を実行した際に流れる log の 1 行毎に経過時間を合わせて表示する、というもの。

### Script for after

Docker を使わない場合の script 。

{% highlight terminal %}
printf "\n\n\n%0*d after %0*d\n\n" 17 0 17 0 | sed "s/0/$(echo -ne '\U1F407')/g" \
&& START_TIME=$(gdate +%s.%3N) \
&& while IFS= read line; do
  lap_time=$(($(gdate +%s.%3N)-$START_TIME))
  printf "\e[33m%07.3f\e[m %s\n" $lap_time $line 
done < <(bundle exec jekyll serve --drafts --livereload)
{% endhighlight %}

こちらは特に難しいことはなかったが、細かい点では

- milli 秒まで表示したかったので、GNU 拡張の入った `date` command が必要だった。
  - `brew install coreutils` してやると `g` の prefix 付きで install される。つまり `date` は `gdate` 。
  - 参考: [date コマンドで日時のミリ秒単位まで表示する](https://qiita.com/niwasawa/items/9502e97b6c4d28d24042 "Qiita - qiita.com")
- 今となっては、何故 Jekyll の出力を普通に pipe で渡していないのか判らない。今試してみると pipe で渡しても普通に動くし…。恐らく最初は lap_time を loop 毎に加算していく作りにしたかったんじゃないかな。その時に変数の値がうまく共有されずに pipe 以外の渡し方を探ったのではないか。
  - 参考: [パイプ出力を現在のシェル上のwhileに喰わせる上手いやり方](https://qiita.com/kawaz/items/6fd4cd86ca98af644a05 "Qiita - qiita.com")
- 参考: [printfで右詰め左詰め](https://qiita.com/DQNEO/items/1d4584b538cd662dfaf6 "Qiita - qiita.com")
- 参考: [コンソール上で回数を指定して文字列を繰り返し出力(リピート)させる](https://orebibou.com/2017/08/%E3%82%B3%E3%83%B3%E3%82%BD%E3%83%BC%E3%83%AB%E4%B8%8A%E3%81%A7%E5%9B%9E%E6%95%B0%E3%82%92%E6%8C%87%E5%AE%9A%E3%81%97%E3%81%A6%E6%96%87%E5%AD%97%E5%88%97%E3%82%92%E7%B9%B0%E3%82%8A%E8%BF%94%E3%81%97/ "俺的備忘録 〜なんかいろいろ〜 - orebibou.com")
- 参考: [sedでユニコードを指定して置換を行う](https://orebibou.com/2016/11/sed%E3%81%A7%E3%83%A6%E3%83%8B%E3%82%B3%E3%83%BC%E3%83%89%E3%82%92%E6%8C%87%E5%AE%9A%E3%81%97%E3%81%A6%E7%BD%AE%E6%8F%9B%E3%82%92%E8%A1%8C%E3%81%86/ "俺的備忘録 〜なんかいろいろ〜 - orebibou.com")


### Script for before

Docker で動かす場合の script 。

{% highlight terminal %}
printf "\n\n\n%0*d before %0*d\n\n" 17 0 17 0 | sed "s/0/$(echo -ne '\U1F422')/g" \
&& START_TIME=$(gdate +%s.%3N) \
&& while IFS= read line; do
  lap_time=$(($(gdate +%s.%3N)-$START_TIME))
  line=$(echo $line | tr -d "\r")
  printf "%-68s \e[33m%07.3f\e[m" $line $lap_time
done < <(docker-compose run --rm -w /var/jekyll/regrets --service-ports jekyll bundle exec jekyll serve --port 8496 --livereload_port 8497 --host 0.0.0.0 --drafts --livereload)
{% endhighlight %}

これがもう本当に大変で。

- Docker を使わない場合と同様の script で、lap_time を左寄せから右寄せに変えるだけだろ、と思ったのだけど、Docker 経由で流れてくる文字列をどうしても思い通りに format 出来なくて、めちゃくちゃ悩んだ。
  - もうあまり覚えていないし、再度検証するのもうんざりなのでしないのだけど、結果的に Docker からの文字列にはどうも carriage return が付いてるっぽいっということが判り、それを削除してやることで何とか思い通りに表示させられたのだと思う。
  - 本当はこれでも駄目で、これ、実はまったく改行されておらず、単に折り返しているだけ。terminal の幅を変えると表示が崩れる。cr を lf に変換してやれば良かったのかなぁ。でもそんなの試したと思うんだよな…。
- 結果の画像には `formatted_lap_time=$(printf "%0.3f" $lap_time)` という debug 時の残骸が残ってしまっているが気にしない。
