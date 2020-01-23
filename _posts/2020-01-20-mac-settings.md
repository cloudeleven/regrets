---
layout: post
title: "128 settings required minimumly to start using a new mac."
description: "新しい mac を使い始めるための最低限の設定 128 。"
thumb_image: "documentation/chalk-docker-before-after.png"
tags: [mac]
---

後述するけど、とにかく全員、人前で使う可能性のある mac では最低限、key repeat を最速に、repeat までの時間を最短にしていただきたい。それだけ判っていただいたら後は自由に立ち去っていただいて結構です。

{% include image.html path="documentation/mac-keyboard-settings.png" path-detail="documentation/mac-keyboard-settings@2x.png" alt="key repeat setting" %}

#### Table of Contents
{:.no_toc}

* A markdown unordered list which will be replaced with the ToC, excluding the "Contents header" from above
{:toc}


## Preface

- 既存の端末から設定をまるっと引き継ぐようなことはしない。だって、どこが変わったかを知っておきたいし、自分の求めることの変化も知っておきたい。それに何より、新しい mac を買って setup している時間が一番楽しいでしょう?
- とは言え、新しい mac を setup するのは大抵いつも久々で割と手間取る。設定したいことは頭の中にあるとしても、順番を間違えて同じ画面を何度も行き来する羽目になったり運用を始めてから漏れていた設定に気付いて苛ついたりする羽目になるので手順書が必要。今迄はその手順 memo を自宅内の git で管理していたのだけど、後述するように Terminal.app を使えるようにするまでに済ませておきたい設定も多々あり何かと不便だったので web に置くことにした。
- 基本的に default の設定から変更する必要があるもののみ記載。

### Environment
- MacBook Pro (16-inch, 2019)
- macOS Catalina Version 10.15.2


## Minimum Settings

最低限の設定。

### System Preferences

setup 中は `System Preferences.app` を頻繁に使うので、最初に使いやすくしておく。

#### System Preferences > menu 'View' > 

- check `Organize Alphabetically`
  - Catalina になったらますます探せなくなったので。検索窓で incremental に絞り込めるのは素晴らしいけど、typing せずに済ませたい時もあるわけで。

### Trackpad

まず入力系を使えるようにしておかないとずっと苛々するゆえ。

#### System Preferences > Trackpad > Point & Click >

- `Look up & data detectors` を `Tap with three fingers` に。
  - 辞書を引く手間は少なければ少ないほど良い。
- Check `Tap to click`
  - trackpad が tap に反応するようにする。超重要:bangbang:
  - 最近の MacBook の trackpad は click もかなり軽いけど、それでも tap の方が楽。押し込むのは疲れる。文字入力中に意図せず trackpad が反応するということももう何年も経験していないので、これを設定しない理由がない。

#### Accessibility > Pointer Control > Mouse & Trackpad > Trackpad Options... >
- check `Enable dragging` and select `three finger drag`
  - 3 本指 drag で window を移動出来るように。とにかく trackpad を押し込みたくないんです。超重要:bangbang:


### Keyboard

まず入力系を

#### System Preferences > Keyboard >

##### Keyboard >

- `Key Repeat` を最速に。
  - あらゆる application が、例えば "cursor から行頭までの文字列を削除" の shortcut key を提供していて、我々がそれらをすべて覚えられるなら必要ないが、世の中も我々もそうなっていないのだから、backspace を押しっ放しにしなければならない時が必ずくる。設定しておくしかない。超重要:bangbang:
- `Delay Until Repeat` を最短に。
  - 同上。超重要:bangbang:
- check `Turn keyboard backlight off after 5 secs of inactivity`
  - ここは好みで。
- check `Use F1, F2, etc. keys as standard function keys`
  - 俺は Touch Bar を基本的に default の設定で使っているので、この設定は実質 Touch Bar のない keyboard 使う場合の設定。
  - ただ、この設定は未だに悩んでいて…。Touch Bar のある keyboard を使う時は Fn を押した時に Touch Bar に F1 ~ が現われるように設定しているのだけど、それだと Touch Bar のある keyboard で使う時と Touch Bar の無い keyboard で使う時とで F1 ~ を押すための操作が違ってしまっているので、間違えることがある…。
- `Modifier Keys...` >
  - `Caps Lock Key:` を `Control` に。
    - 入れ替えるんじゃない。どちらも Control にする。Caps Lock Key などこの世に必要ない。憎い。この世のすべての keyboard から Caps Lock が無くなれば良いのに。超重要:bangbang:

##### Text >

  - uncheck `Correct spelling automatically`
    - とにかく余計なことはしてほしくない。
  - uncheck `Capitalize words automatically`
    - とにかく余計なことは
  - uncheck `Add period with double-space`
    - とに
  - `Spelling` >
    - 選択肢から `Set Up...` を選択し、使わない言語を残してすべて uncheck 。
      - これ、記憶は曖昧だけど、昔は spell check を無効にしても何故か赤い波線が表示されてしまうことがあった気がして、これをしたらそれを解消できた気がするんだけど、もしかしたらしなくても良かったのかも知れない。あるいは今はしなくても良くなったかも知れない。確認はしない。
      - 使わない言語の check を残すのは、すべての check を外すことが出来ないから。

##### Shortcuts >
- Input Sources >
  - check `Select the previous input source`
  - check `Select next source in Input menu`
- check `Use keyboard navigation to move focus between controls`
  - Tab key による focus 移動をあらゆる control 要素間で可能にする。これをしないと移動出来る要素が限られる。特に button 要素へ focus を移せないのは本当に困るので設定するしかない。超重要:bangbang:
  - これ、Mojave までは `Ful Keyboard Access: In windows and dialogs, press Tab to move keyboard focus between:` という項目で選択肢は `Text boxes and lists only` (こちらが default) と `All controls` だった。これを `All controls` に設定していたのだけど、現状この項目に check を入れることがまったく同じ結果をもたらすのかどうかは判らない。でも入れるしかないよ。そうしないと button に focus を移せないんだから。
    
### Host Name

#### System Preferences > Sharing >

- `Computer Name:` を任意の名前に。
  - これ、後からゆっくりやっても良いのだろうけど、default の host name は自分の名前を元に勝手に付けられてしまうのでださい以上に気持ち悪すぎるし、それが Terminal.app を立ち上げたときに prompt に現れたり、Bonjour で local network に advertise されたりして気持ち悪いので早めに済ませたい。

### Lock

画面と睨めっこしながら長考していたり、pointer 操作を誤って screen saver が発動してしまった時にそれを cancel するために 5 秒の猶予が欲しい。

#### System Preferences > Security & Privacy > General >

- `Require password` を `5 seconds` に。
  - Mac mini 等、外に持ち出すことのない端末ではもう少し長めにしている。Touch ID なくて面倒だし。

#### System Preferences > Desktop & Screen Saver > Screen Saver >

- `Start after` を `5 Minutes` に。
- check `Show with clock`
- `Hot Corners...`
  - 右上を `Start Screen Saver` に。


### Local Account

#### System Preferences > Users & Groups >

- icon 画像部分に設定したい画像を drag & drop 。
  - これ、どうでも良いのだけどやっておかないと AirDrop を使う時にがっかりするので。

### Launcher

Spotlight を launcher として使う。Spotlight 最高。

#### Spotlight > Search Results >
次のもののみ check を残して他をすべて uncheck 。重要:heavy_exclamation_mark:

- Applications
  - launcher として使うのはこれが肝。
- Calculator
  - 簡単な計算なら Calculator.app を立ち上げるまでもない。
- Conversion
  - 単位変換。うまく行けば google を開かずに済む。
- Definition
  - 辞書。どの辞書を使うかは Dictionary.app で設定する。後述。
- Documents
  - File を探すのはほぼ Finder.app か Terminal.app でするし、要らないかも。やめようかな。text file の中身を Spotlight 内で preview 出来ることだけがたまに嬉しいんだよなぁ。
- Folders
  - 同上。
- Spotlight Suggestions
  - これ、あんまり良い提案してこないからやめるかも。というか俺が基本的に欲しいものを明確に入力するので意味がない。
- System Preferences
  - System Preferences の個々の項目に直接行きたい。


### Dock

launcher としては使わないので不要なものはすべて削除。Dock 自体も隠す。

#### Remove ALL application from Dock

- Dock 上のすべての application icon 上で context menu を表示し `Options` > `Remove from Dock` 。あるいは icon を Trash に drag 。
  - application を何も起動していない時はこうあるべき。Finder はたぶん消せないよね。Downloads は消せるけど割と便利なので。
{% include image.html path="documentation/mac-settings-dock.png" path-detail="documentation/mac-settings-dock@2x.png" alt="Dock" %}


#### System Preferences > Dock >

- `Position on screen` を `Left` に。
  - 左か右かの二択だけど、俺は右端に Terminal.app を配置するし、右上で screen saver が起動するようにしているので左。下にしておくと何か不便なことがあったと思うんだけど忘れた。
- check `Automatically hide and show the Dock`
  - 滅茶苦茶大きい display で使っているなら出しっぱなしでも良いのかも知れないけど、出しておいたところでたいして意味ないので隠すが吉。俺は 43 inch の display に繋いでいる Mac mini でも隠してる。


### Others of System Preferences

その他 System Preferences でやっておくこと。

#### System Preferences > Date & Time > Clock >

これ、default がどうなっていたか忘れてしまったんだけど結果として次のようになっていれば良い。

- check `Show date and time in menu bar`
- check `Use a 24-hour clock`
- check `Show date` of `Date Options`

#### System Preferences > Sound >
- check `Show volume in menu bar`
  - ここから音量調節したいというよりは、今 mute されているかどうかを一瞥で確認したいことが多々ある。あと speaker と headphone を切り替えたり。

#### System Preferences > Apple ID > 

左上に表示されている sign in 済みの account が iCloud 用として望み通りのものであるかどうかをまず確認。App Store などで支払いの為に使う account と iCloud の account を分けている場合は特に注意。期待と違った場合は `Overview` > `Sign Out...` してから `Sign In` し直す。

##### iCloud >
次のものだけが check されている状態にする。依存すると Android で困るようなものは使わない。使えない。
- `Safari`
- `Notes`
- `Siri`
- `Keychain`
- `Find My Mac`
- `Home`

##### Media & Purchases >

購入用として望み通りの account で sign in されているかを確認。期待と違った場合は (`Manage...` button から行っても良いんだろうけど) 、App Store.app を立ち上げて menu の `Store` から一度 `Sign Out` して再度 `Sign in` 。

- check `Use Touch ID for Purchases`

#### System Preferences > Energy Saver >
MacBook の場合のみ
- check `Show battery status in menu bar`
- menu bar に表示された battery icon を tap して `Show Percentage` を選択。


### Dictionary.app

ここでの設定が Spotlight での Definition にも共有される。

- `Preferences` で必要な辞書だけに check 。俺の場合は以下のみ。
  - New Oxford American Dictionary (American English)
  - スーパー大辞林 (Japanese)
  - ウィズダム英和辞典/ウィズダム和英辞典 (Japanese-English)

- ちなみに、特定の環境では、特定の辞書が使えない問題があるようで、俺の端末でも再現していた。comment や追記を併せて読むと判るように、download して展開して rename して配置してやったら取り敢えずは使えるなる。けど、Spotlight では出てきてくれない…。
  - [MacBook Pro (16-inch, 2019)で辞書.app（Dictionary.app）の英和/和英辞典が使えなくなった話。](https://qiita.com/mybdesign/items/72a09ccbbb38a3ec72e2)
  - [The macOS dictionary app cannot add new build in dictionaries](https://discussions.apple.com/thread/250861549) を読むと、16inch MacBook Pro でばかり起きているっぽいが、端末依存というのは考えにくいので、Catalina が pre-install (あるいは clean install) された端末で起きるということなのかな?

### Finder.app

#### Preferences... >
##### General >
- `Show these items on the desktop` の全項目を uncheck 。
- `New Finder windows show` を home directory に。

##### Sidebar >
- `Favorites` 以下の item だけが check されている状態にする
  - `AirDrop`
  - `Applications`
  - `Documents`
  - `Downloads`
  - home directory
- `iCloud` 以下の次の項目を uncheck
  - `iCloud Drive`
- `Locations` 以下の全項目を check

##### Advanced >
- check `Show all filename extensions`
  - 拡張子を見せない設定が default なのはまじで意味が解らない。重要:heavy_exclamation_mark:
- `When performing a search` を `Search the Current Folder` に。

#### menu 'View' >
- `Show Status Bar` を選択、あるいは `⌘+/` で status bar を表示。
  - これが default じゃない意味が解r

### Safari.app
#### Preferences >
##### General >
- `Safari opens with` を `All windows from last session` に。
- uncheck `Open "safe" files after downloading`
##### Tabs >
- `Open pages in tabs instead of windows` を `Always` に
- check `When a new tab or window opens, make it active`
- check `Show website icons in tabs`
##### Advanced >
- check `Show full website address`
- check `Show Develop menu in menu bar`

#### menu 'View' >
- `Show Status Bar` を選択、あるいは `⌘+/` で status bar を表示。
  - これが default じゃn


### AquaSKK
この世のあらゆる連文節変換 input method は使い物にならないので mac で動く [SKK](https://ja.wikipedia.org/wiki/SKK) 実装であるところの [AquaSKK](https://github.com/codefirst/aquaskk) を使うしかない。

install の最後で logout (= 全 application 終了) を要求されるので、止められたら困る application は予め終わらせておく。

- https://github.com/codefirst/aquaskk/releases から最新を install
- `System preferences` > `Keyboard` > `Input Sources` で `+` を tap し、`English` から AquaSKK の `ASCII` を、Japanese から AquaSKK の `カタカナ`, `ひらかな`, `全角英数` を追加。
- どの timing で出来るようになるか判らないけど、既存の `ABC` も消せるようなら消す。一生使わないし邪魔なので。でももしかしたら今は消せないのかも知れない…。
- user 辞書の設定。既存の辞書がある場合のみ。AquaSKK は望みの変換を簡単に登録出来るのでそこまで重要じゃないけど、俺の場合は default 状態で自分の名前を変換するのが異様に面倒なので。
  1. 他の端末から user 辞書を持ってきて任意の場所に配置。
  2. menu bar の AquaSKK icon > `環境設定` > `辞書` > `ユーザー辞書` > で path を set 。
    - ただしこれには罠があって、`変更...` button から file を選択すると `file:/` から始まる path が入力されるのだけど、これだと読んでくれないようなので、file:/ を削除。
      - 参考: [AquaSKK で辞書が保存できなかった理由](https://shammerism.hatenadiary.com/entry/20141023/p1)
- menu bar の AquaSKK icon > `環境設定` > `互換性` >
  - `com.google.android.studio` を追加し、全 check 。
    - Android Studio を使う端末の場合のみ。
  - `jp.naver.line.mac` を追加し、`露払後確定` のみ check 。
    - もちろん line を使う端末の場合のみ。


### App Store.app

- とりあえず update 出来るものは update 。
- 続いて必要なものを install 。
  - まずは setup の途中で必要になる以下を install してしまう。
    - [Xcode](https://apps.apple.com/jp/app/xcode/id497799835?l=en&mt=12)
      - 開発する端末なら。
    - [Enpass](https://apps.apple.com/jp/app/enpass-password-manager/id732710998?l=en&mt=12)
      - password manager 。昔は 1Password を使っていたのだけど、(少なくとも当時は) local network 内で各端末を同期させるには export した file をやりとりするしか手段がなくて、暗号化されているとは言え password が詰まった file を internet 上の Dropbox や iCloud 等に置く気になれないため mac 4 台と iPhone, iPad, Android を運用している身には同期が恐しく面倒な割に、version up の度に金を取られる気がしてうんざりしたので乗り換えた。今は家の中の webdav を介して同期している。超便利。
    - [The Unarchiver](https://apps.apple.com/jp/app/the-unarchiver/id425424353?l=en&mt=12)
      - いつも新しい端末を setup する時には "もう RAR とか 7-Zip とか download することもないし要らないだろ" と思うのだけど、使っていくうちに結局必要になって入れることになる。
  - 以下は端末の用途次第で任意の timing で install 。
    - [Bluetooth MIDI Connect](https://apps.apple.com/jp/app/bluetooth-midi-connect/id1074606480?l=en&mt=12)
    - [Kindle](https://apps.apple.com/jp/app/kindle/id405399194?l=en&mt=12)
    - [VOX](https://apps.apple.com/jp/app/vox-mp3-flac-music-player/id461369673?l=en&mt=12)
      - 音楽 player 。MacBook で音楽を聴くことはほぼ無いのだけど、ripping したり download した音源を確認したい時に便利なので。見栄えも良いし。だけどいつからか有料の機能を subscribe しろとうるさくなったので、他に良いのがあれば乗り換えたい。CD の内容をひとつの file にした flac 音源及び cuesheet/tag を正しく扱える player でなければいけないので探すのが面倒。
      - media server として使っている Mac mini では [Audirvana](https://audirvana.com/) の license を買って使っている。
    - [Blackmagic Disk Speed Test](https://apps.apple.com/jp/app/blackmagic-disk-speed-test/id425264550?l=en&mt=12)
      - 別にこれじゃなくても良いんだけど、新しい MacBook を買ったら内蔵 SSD の速さを可視化して喜びを倍増させる。
    - [Pixelmator](https://apps.apple.com/jp/app/pixelmator/id407963104?l=en&mt=12)
      - もう入れなくて良いかな。昔は安い Photoshop 代替として良い感じだったのだけど、いつの間にか [Pixelmator Pro](https://apps.apple.com/jp/app/pixelmator-pro/id1289583905?l=en&mt=12) とかいうのが出ていて今更 Pro 版じゃないものを使う気にならないし、俺は俺でもう Adobe Creative Cloud の complete plan を subscribe しているからなぁ。でも Adobe CC は複数端末での同時使用は出来ない license だった気がするから、必要が生じれば。
    - [Logic Pro X](https://apps.apple.com/jp/app/logic-pro-x/id634148309?l=en&mt=12)
      - ただでさえ Pro 版としてはかなり安いのに、複数端末で使えるのは本当に有り難い。
  - [Witch](https://apps.apple.com/jp/app/witch/id412485838?l=en&mt=12) はここでは入れない。App Store には古い version しか無いので、別途 download して install する。後述。

### Enpass.app

- `Restore from Cloud` から既存の sync 先を設定する。
- `Preferences` > `Security` >
  - check `Touch ID`


### Delete unnecessary apps
端末の用途に応じて以下の app を削除。update がうざい。
- `iMovie.app`
- `Numbers.app`
- `Pages.app`
- `Keynote.app`



### Browsers

#### [Chrome.app](https://www.google.com/intl/ja_ALL/chrome/)

- download して install して login 。それで設定も同期されるけど、何らかの都合で同期しない場合でも最低限以下を設定する。
  - `Preferences` > `On startup` で `Continue where you left off` を選択。

#### [Firefox](https://www.mozilla.org/ja/firefox/new/)

- download して install して login 。それで設定も同期されるけど、何らかの都合で同期しない場合でも最低限以下を設定する。
  - `Preferences` > `一般` > `起動` で `前回のセッションを復元する` を check 。

#### [Brave](https://brave.com/)
- download して install 。特に何も設定しない。


### Xcode.app
端末の用途によっては必要ないけど、Homebrew の install で一部の component が必要だったりするので、storage 容量がよほど逼迫しているのでない限り入れてしまう。せっかく毎年 license 買っているのだし。
- 初回起動で追加 component の install を迫られるので従う。

### Homebrew
- https://brew.sh/ に従って install 。
- 以下を install
  - git
    - mac には pre-install されているけど、段々古くなるので。
  - [tmux](https://github.com/tmux/tmux)
    - 少し前までは [GNU Screen](https://www.gnu.org/software/screen/) を使っていたのだけど tmux への乗り換えを試行中。ほぼ同じ使い勝手で使えているし、より便利な点もあるので、もう完全に乗り換えても良いかな。ひとつだけまだ慣れないのが copy mode で範囲選択を終えるための key が enter であるところ。screen は space だった。
  - zsh
    - pre-install されているし、Catalina からは default にさえなったけど、以前に次の問題に遭遇してからずっと Homebrew で入れたものを使っている。たぶんこの問題は今は直っているんだけど。
      - [Auto completion not work for command 'du'](https://stackoverflow.com/questions/33817282/auto-completion-not-work-for-command-du)
  - rsync
    - pre-install されているけど、同期中に流れる log で日本語 file 名が化けるので。 
    {% include image.html path="documentation/mac-settings-rsync.png" path-detail="documentation/mac-settings-rsync@2x.png" alt="Dock" %}
  - wget
  - openssl
  - lv
    - 昔からの馴染みで。でももうこれを使う理由って無いのかな?
  - jq
    - JSON 返す API を扱っている職場で質問してきた奴の端末にこれが入ってないと「だから何やっても遅えんだよ…」と言いたくなる…。
  - flac
  - mkvtooknix
  - ffmpeg

### Dot files

git で管理しているので、次のようにして配置。

{% highlight shell_session %}
$ cd
$ git clone https://github.com/cloudeleven/dotfiles.git
$ cd dotfiles
$ ./link.sh
{% endhighlight %}


### Terminal.app

下記は既存端末から設定 profile を export して import するのででも良し。

- `Preferences` > `Profiles` > `Pro` を選択状態にして歯車 icon から `Duplicate Profile` を選択、任意の名前を付ける。
- 名前を付けた profile が選択された状態で `Default` button を tap 。
- 同じく profile が選択された状態のまま各 tab で以下を設定。
  - `Text`
    - `Background` の `Color & Effects` を tap して `Opacity` を 100% に。
    - check `Antialias text`
    - uncheck `Use fold fonts`
  - `Window`
    - `Window Size` を任意に。MacBook Pro (16-inch, 2019) だと 100 x 76 とか。
    - `Scrollback` で `Limit number of rows to` を check して値を 0 に設定。 
  - `Shell`
    - `Run command` を check して内容を `/usr/local/bin/tmux -u` に。参考: [tmux使用時に日本語が表示されないとき(アンダースコアになってしまう）の対処法](https://www.wazalab.com/2018/09/10/tmux%E4%BD%BF%E7%94%A8%E6%99%82%E3%81%AB%E6%97%A5%E6%9C%AC%E8%AA%9E%E3%81%8C%E8%A1%A8%E7%A4%BA%E3%81%95%E3%82%8C%E3%81%AA%E3%81%84%E3%81%A8%E3%81%8D%E3%82%A2%E3%83%B3%E3%83%80%E3%83%BC%E3%82%B9/ "Waza Lab - www.wazalab.com")
      - GNU Screen の場合は同様に内容を `/usr/local/bin/screen -U -O -s /usr/local/bin/zsh` に。
    - `When the shell exits` を `Close if the shell exited cleanly` に変更。
  - `Keyboard`
    - check `Use Option as Meta key`
  - `Advanced`
    - check `Delete sends Control-H`
      - 何故これをしているのか覚えていない。MacBook の keyboard を使っている限りは関係ないかも。
    - uncheck `Audible bell`
    - uncheck `Set locale environment variables on startup`
      - 何故こうしたのか確かな記憶がないが、たぶんこの辺は全部 .zshrc や .tmux.conf (あるいは .screenrc) で制御したかったんじゃないかな。
- `Preferences` の `General` の `On startup, open` において `New window with profile` が check され、上記で作成した profile が選択されていることを確認。


### Change the path of Screen Capture saved
以下は Screen Capture 画像の保存先を `~/Pictures/ss` にしたい場合。どこでも良いけど Desktop は有り得ない。
{% highlight shell_session %}
$ mkdir ~/Pictures/ss/
$ defaults write com.apple.screencapture location ~/Pictures/ss/ && killall SystemUIServer
{% endhighlight %}

### [Visual Studio Code](https://code.visualstudio.com)
- download して install 。
- 既存の設定をどこからから持ってきて、 {% highlight shell_session %}
$ mv -i ~/Library/Application\ Support/Code/User ~/Library/Application\ Support/Code/User.bak # backup
$ ln -s ~/dotfiles/vscode ~/Library/Application\ Support/Code/User
{% endhighlight %}
- command line から起動出来るようにする
  - [Launching from the command line](https://code.visualstudio.com/docs/setup/mac#_launching-from-the-command-line)
  - path の設定は前述の Dotfiles 内の .zshrc には入れてある。
- 参考
  - [Visual Studio Codeで設定ファイル・キーバインディング・拡張機能を共有する](https://qiita.com/mottox2/items/581869563ce5f427b5f6)
  - [どこでも同じ設定でVisualStudioCodeを使う方法](https://qiita.com/canpok1/items/a7c4c96e3c1c2a1cc532)


### Create case-sensitive volume
file 名の大文字小文字を区別する領域は持っておきたい。Linux 等 default の file system が case-sensitive な環境から持ってきた tar を展開したくなったりすることがあるので。
#### volume を作って mount
{% highlight shell_session %}
$ hdiutil create -size 50g -fs "Case-sensitive APFS" -type SPARSEBUNDLE -volname "dev" dev.sparsebundle
$ # volume が mount 可能であることを確認。
$ hdiutil attach dev.sparsebundle
$ ln -s /Volumes/dev ~/dev
{% endhighlight %}

#### 作成した volume を mount するための script を用意
{% highlight shell_session %}
$ mkdir -p ~/bin
$ vi ~/bin/mount_dev.sh
$ cat ~/bin/mount_dev.sh
#!/bin/bash

IMAGE="/Users/m4c/dev.sparsebundle"
if [ -d "${IMAGE}" ]; then
    /usr/bin/hdiutil attach "${IMAGE}"
else
    echo "image '${IMAGE}' is NOT exists."
fi
$ chmod 700 ~/bin/mount_dev.sh
{% endhighlight %}

#### mac への login 時に、上記 script を自動的に呼び出すための準備
- 参考
  - [[Mac] ユーザログイン時にスクリプトを実行する方法](https://dev.classmethod.jp/tool/mac-launch-agents/)
  - [launchdで定期的にスクリプトを実行](https://qiita.com/rsahara/items/7d37a4cb6c73329d4683)

{% highlight shell_session %}
$ vi ~/Library/LaunchAgents/space.cloudeleven.m4c.mount_dev.plist
$ cat ~/Library/LaunchAgents/space.cloudeleven.m4c.mount_dev.plist
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
  <dict>
    <key>Label</key>
    <string>space.cloudeleven.m4c.mount_dev</string>
    <key>ProgramArguments</key>
    <array>
      <string>/bin/bash</string>
      <string>/Users/m4c/bin/mount_dev.sh</string>
    </array>
    <key>RunAtLoad</key>
    <true/>
  </dict>
</plist>
$ # lint
$ plutil -lint ~/Library/LaunchAgents/space.cloudeleven.m4c.mount_dev.plist
$ # 一旦 unmount して、
$ hdiutil detach /Volumes/dev
$ # plist の内容で mount 出来ることを確認
$ launchctl load ~/Library/LaunchAgents/space.cloudeleven.m4c.mount_dev.plist
$ # もしやり直す必要がある場合は一旦 unload する
$ launchctl unload ~/Library/LaunchAgents/space.cloudeleven.m4c.mount_dev.plist
{% endhighlight %}

### git
- git status などで日本語を含む file 名が読める形で表示されるように。開発だと基本的に日本語名の file なんて作らないけど、text file はとりあえず何でも git に放り込んだりしているので。
  - 参考: [[小ネタ][git] 日本語ファイルの文字化けを回避する](https://dev.classmethod.jp/tool/git/git-avoid-illegal-charactor-tips/)
{% highlight shell_session %}
$ git config --global core.quotepath false
{% endhighlight %}
- github で使う名前を global に設定しておく。
  - github に限らず git 全体に影響する設定だけど、うっかり push して恥をかくのは github であることが多いので、github に push されても恥ずかしくない設定を git の global に設定してしまう。ま、mail address は隠蔽されるように github 側で設定すると思うんだけど。
{% highlight shell_session %}
$ git config --global user.name "任意の user name"
$ git config --global user.email "任意の mail address"
{% endhighlight %}
- 必要な repository を clone
  - github や bitbucket, 自宅の network などから必要そうな repository を clone しておく。


### anyenv

**env 系は全部 anyenv のお世話になる。

#### install anyenv

{% highlight shell_session %}
$ git clone https://github.com/anyenv/anyenv ~/.anyenv
$ echo 'export PATH="$HOME/.anyenv/bin:$PATH"' >> ~/.zshrc
$ echo 'eval "$(anyenv init -)"' >> ~/.zshrc
$ exec $SHELL -l # ここで warning が出るので次で対応する
$ anyenv install --init
$ # update に便利な plugin も入れてしまう。
$ mkdir -p ~/.anyenv/plugins
$ git clone https://github.com/znz/anyenv-update.git ~/.anyenv/plugins/anyenv-update
$ git clone https://github.com/znz/anyenv-git.git ~/.anyenv/plugins/anyenv-git
$ # update してみる
$ anyenv update
$ anyenv git pull
{% endhighlight %}


#### install rbenv, pyenv, nodenv, jenv

{% highlight shell_session %}
$ anyenv install -l # install 可能な **env を確認
$ anyenv install rbenv
$ anyenv install pyenv
$ anyenv install nodenv
$ anyenv install jenv
$ exec $SHELL -l
$ anyenv versions # 確認
$ # rbenv で `bundle exec` を省略出来るように
$ git clone https://github.com/ianheggie/rbenv-binstubs.git "$(rbenv root)/plugins/rbenv-binstubs"
{% endhighlight %}
- 参考: [ianheggie/rbenv-binstubs](https://github.com/ianheggie/rbenv-binstubs)


### Witch
基本的には `⌥+Tab` で (application ではなく) window を切り替えるための tool 。

Microsoft Windows での `Alt+Tab` に近い操作感になるのだけど、それだけじゃなく、window 内の各 tab を直接 active にしたり、application/window/tab 名を入力しながら incremental に active 化する対象を絞り込むことも可能。Spaces を跨いでいる window にも一発で focus 出来るので便利。というかこれが普通であるべき。

例えば、四本指 swipe で Spaces を切り替えて、⌘+Tab で application を切り替えて、⌥+` で window を切り替えて… とかやってられなくない?

mac に乗り換えてかなり経つけど、application/window の切り替えだけはどう考えても Windows の方がまとも。とは言え Witch のおかげでやっぱり Mac の方が便利ということになるのだけど。

- Many Tricks の [Witch](https://manytricks.com/witch/ "Witch - Many Tricks") の page から download して install 。必要に応じて license を購入。
- 起動すると色々権限を要求されるので与えてやる。具体的には以下の項目に check が入った状態になれば良いはず。
  - `System Preferences` > `Security & Privacy` > `Privacy` >
    - `Accessibility` が選択された状態で `Allow the apps below to control your computer.` 内の
      - `witchdaemon.app`
    - `Screen Recording` が選択された状態での
      - `witchdaemon.app`


### [Android Studio](https://developer.android.com/studio?hl=ja)
これも端末の用途によっては必須ではないのだけど、mac に Android 端末を繋ぎたくなる時はいずれ訪れるし、その時に `adb` 等の commands が使えると便利なので、storage 容量がよほど逼迫しているのでなければ入れてしまう。
- 既存の別端末にて menu `File` > `Export Settings...` したものを新端末で `Import Settings...` する。
  - たぶん Keymap しか変えていない。^H で backspace 出来ないと死ぬほど苛つくので死なないように変更したり、AquaSKK で使う ^J に割り当てられている機能を別の key bind に変えたり。
- `SDK Manager` を立ち上げて `SDK Tools` で必要そうな項目を check して install しておく。例えば次の項目とか。
  - Android Emulator
  - Google Play Instant Development SDK
  - Google Play services
- Emulator を使う場合は `ADV Manager` で virtual device を作っておく。
  - これは完全に備忘録だけど、emulator を立ち上げる command 。自宅以外では問題ない気もするけど virtual device が network に繋がらなくて毎回はまるので。
    - local の DNS を使いたい場合は `8.8.8.8` の部分を適当に変えて。
{% highlight shell_session %}
$ ~/Library/Android/sdk/emulator/emulator -avd Nexus_5X_API_28 -dns-server 8.8.8.8
{% endhighlight %}
- 前述の case-sensitive な領域にある project を開くと表示される警告に対応する。
  - 参考: [Filesystem Case-Sensitivity Mismatch](https://confluence.jetbrains.com/display/IDEADEV/Filesystem+Case-Sensitivity+Mismatch)
{% highlight shell_session %}
$ # 一応既存の設定を確認
$ cat /Applications/Android\ Studio.app/Contents/bin/idea.properties
$ echo 'idea.case.sensitive.fs=true' >> /Applications/Android\ Studio.app/Contents/bin/idea.properties
{% endhighlight %}


## Other Settings

端末の用途によって必要だったり不必要だったりする設定。個人的 memo 色強め。

### [Docker Desktop for Mac](https://docs.docker.com/docker-for-mac/)
- document に従って install するだけ。昔は docker-compose を別途 install する必要があった気がするけど、今は全部含まれてるし。

### [VirtualBox](https://www.virtualbox.org/)
- 本体を install して、Extension Pack を install 。

### [Adobe Creative Cloud](https://creativecloud.adobe.com/)
- Creative Cloud Installer を download して install 。


### Media related

#### [VLC media player](https://www.videolan.org/)
動画・音声、何でもお気軽再生用。

#### [HandBrake](https://handbrake.fr/)
動画のお気軽 encode 用。
- `Preferences...` > `General`
  - `At Launch`
    - uncheck `Show Open Source panel`
  - `When Done`
    - select `Do Nothing`
    - uncheck `Play System Alert Sound`
- menu `View` > `Customize Toolbar...` で `Add Titles To Queue` を toolbar に追加。
  - これの存在を知るまで file をひとつひとつ add していた…。
- 自分で調整したお気に入りの preset があるなら、別の端末でそれを export して、新端末で import 。
  - HandBrake はいつからか audio track の扱いの設定が変わったのか、(古い version から preset を持ってきた場合のみ?) audio track を読み込んでくれない場合があるので、自分で作った preset を使う場合は preset の `Audio` tab の `Selection Behavior...` button から適当に set する。
    - `Track Selection Behavior` を `All Matching Selected Languages` に。
    - `Languages` では `Show All` button を押して全選択肢を表示させ `Any` のみが check されている状態にする。
    - `Audio encoder settings for each selected track` では `Mixdown` を `Stereo` に変更しておく。



### Optical Disc related
光学 disc drive を繋ぐ予定がある端末の場合。

#### [X Lossless Decoder](https://tmkk.undo.jp/xld/)
CD ripping 用。基本的に 1 枚の CD を 1 file の flac に書き出すために使っている。
##### Preferences >
- `General`
  - `Output format` を `FLAC` に。
  - `Output directory` で `Specify` を選択。path は必要に応じて任意のものに変更。
  - `Character encoding of cuesheet` を `Unicode (UTF-8)` に。
- `File Naming`
  - `Format of filename` で `Custom` を選択し、値を `%A - %T` に。
- `CDDB`
  - `FreeDB` の `Server` を `freedbtest.dyndns.org` に変更。
    - これ、確か本家の [freedb.org](http://www.freedb.org/) は日本語に対応していなかったからこの設定にしていたのだけど、本家は 2020/03/31 で shut down するらしい。[freedbtest.dyndns.org/](https://freedbtest.dyndns.org/) がますますありがたい。

#### [MakeMKV](https://www.makemkv.com/)
Blu-ray や DVD の内容を無劣化に抽出して mkv として保存するための tool 。

日本では暗号化されたものを抽出したり保存したりしてはいけません。

#### Allow HandBrake to read DVD
現状の日本では駄目なんだけど、法が変わったり合法な国に移住するなど条件が整った場合、`brew install libdvdcss` して HandBrake を再起動すれば良いらしい。

昔、まだ日本でも合法だったころは libdvdcss を自分で探して download してきて、自分でどこかに配置した記憶があるが今時は便利な模様。

#### Never Let Disc Play Automatically
光学 media を挿入すると自動的に再生されるのがうざいので止める。

以下の `CDs & DVDs` はもしかすると光学 disc drive を繋がないと表示されないかも知れない。
##### System Preferences > CDs & DVDs
- `When you insert a blank` から始まる 3 つはすべて `Ignore` に変更
- `When you insert a music CD` は `Open other application...` から上記で install した `Open XLD.app` を set 。
  - CD はこのためにしか使わないので。
- `When you insert a video DVD` は最初から `Ignore` になってるけど、それでも DVD を入れたら勝手に DVD Player が起動してしまっていたので、一度 `Open DVD Player` に変更した後で再度 `Ignore` に変更したら、DVD を挿入しても DVD Player が立ち上がらなくなった。気がする。再現性があるかどうかは判らない。


