[FrontPage](FrontPage.md)


# SIPropプロジェクトとは？
SIP系のOSSを、開発するプロジェクトです。<br>
[SIPropVer.2.0](http://www.siprop.org/ja/2.0/index.php?SIProp%A4%C3%A4%C6%B2%BF%A1%A9)の完成と[雷電](http://raiden.siprop.org/)の開発開始に合わせて、定義し直されました。

## 目的
<br>


<span style="font-size: 28px"> 「メディア (通信媒体)の世界を広げる」 </span>

という目的の元、SIPに関するのOSSアプリを提案・開発するプロジェクトである。<br>
この目的を果たすために、他プロジェクトと提携し、いろいろなプロダクトを立ち上げている。プロジェクト一覧は[こちら](SIPropプロジェクト#w2123ce8.md)。<br>

イメージ図：
![raiden_base_s.PNG](SIPropプロジェクト_attachment_raiden_base_s.PNG)


## SIPropプロジェクト が認識する問題とその解決
一般の IP 電話において、SIP は相手を呼び出す機能（シグナリングという）を実現する通信プロトコルとして利用されている。そのため、現状では「SIP は IP 電話のための通信プロトコルである」と誤解されがちである。

しかし、インターネットの通信プロトコル規約である RFC において、SIP の目的は次のように規定されている。<br>
```
SIP (Session Initiation Protocol) は、インターネットのエンドポイント(ユーザーエージェントと呼ばれる)が
お互いを発見し、共有を望むセッションの特性に合意することを可能にすることで、これらのプロトコルと
協調して動作する。セッションの参加者と見込まれるものの場所を特定するため、および他の機能のために、
SIPは、ユーザーエージェントが登録リクエストやセッションへの招待およびその他のリクエストを送ることが
できる、ネットワークホスト(プロキシサーバーと呼ばれる)のインフラを生成することを可能にする。
SIPは、下位のトランスポートプロトコルから独立し、確立されるセッションのタイプに依存せずに動作する、
セッションを生成・修正・終了するためのしなやかで多目的なツールである。
```
すなわち、SIPは<br>
**「（IP電話を含む）様々なネットワーク・アプリケーションのためにセッションの管理を行う、汎用セッション・プロトコル」**<br>
なのである。
<br>

本来、汎用セッション・プロトコルであるはずの SIP が、IP 電話に付随する通信プロトコルとして語られることが多いのは、IP 電話が SIP が提供するセッション管理機能を必要とする最もメジャーなアプリケーションであり、他に SIP を必要とするアプリケーションが見つからないことに原因がある。

さらに、近年の通信事業各社によるNGNやFMCと呼ばれる試みは、「シグナリング・プロトコルとしての SIP」を推し進めるものであり、SIP の本来の「汎用セッション・プロトコルとしてのSIP」という設計意図からの乖離を生み出している。

このような状況により SIP は、大きなジレンマを抱え込むことを意味している。「汎用セッション・プロトコルとしての SIP」では、特定のアプリケーションに依存しない汎用セッション・プロトコルとしての地位を保ち続けるべきであるが、「シグナリング・プロトコルとしての SIP」では、通信事業者のシステムも含めたシグナリングに必要な機能が拡充が求められるからである。
<br>

そこで、SIPropプロジェクト は、 SIP が抱えるジレンマをより望ましい形で解決する方策を見出す議論をするためのコミュニティとして、さまざまな活動を行っている。

そのアプローチは概ね２つの方向性に整理することができる。

1. シグナリング・プロトコルとしての SIP の拡張<br>
独自の追加規格が乱立している現状を抜本的に解決するためには、通信事業に利害を持たない中立の立場からの提案が有効である。<br>
既に SIPropプロジェクト は、通信事業者間の SIP による通信の相互接続性を確保を目標に規格提案や実装提供を行っている。
1. 汎用セッション・プロトコルとしての SIP の拡張<br>
SIP の汎用セッション・プロトコルとしての特性を堅持するためには、さまざまアプリケーションでのセッション管理の方策を検討し、共通のセッション管理機能を見出して規格化する必要がある。<br>
SIPropプロジェクト では IP 電話以外の SIP アプリケーションの提案も行う。

以上の２つのアプローチは垂直方向と水平方向の全く異なるベクトルを持っているが、実は相互に密接な関係を持つ。

例えば (1) でIP 電話のために開発された実装の一部は、(2) の新たなアプリケーションの獲得に役立つかもしれない。また逆に (2) の新たなアプリケーション獲得のために考えられた規格は、(1) の相互接続のための設計・実装のヒントになるかもしれない。<br>
<br>

上記のバイ・ディレクション・アプローチは、SIP の問題解決には有効であり、SIP の利用範囲を拡大に寄与すると考えられる。そして、汎用セッション・プロトコルとしての SIP の拡張こそが、[目的](http://www.siprop.org/ja/2.0/index.php?SIProp%A5%D7%A5%ED%A5%B8%A5%A7%A5%AF%A5%C8#sd8a223c)である **「メディア (通信媒体)の世界を広げる」** につながると SIPropプロジェクト は考えており、実践している。

## 汎用セッション・プロトコルとしての SIP とは
### OSI参照モデルの層構造(<span style="font-size: 12px">出典：Wikipediaの「[OSI参照モデル](http://ja.wikipedia.org/wiki/OSI%E5%8F%82%E7%85%A7%E3%83%A2%E3%83%87%E3%83%AB)」より</span>)
![osi_proto.png](SIPropプロジェクト_attachment_osi_proto.png)
- 第7層 - アプリケーション層 
    - 具体的な通信サービス（例えばファイル転送、メール転送、遠隔データベースアクセスなど）を提供。 
- 第6層 - プレゼンテーション層 
    - データの表現方法。 
- 第5層 - セッション層 
    - 通信プログラム間の通信の開始から終了までの手順。 
- 第4層 - トランスポート層 
    - ネットワークの端から端までの通信管理（エラー訂正、再送制御等）。 
- 第3層 - ネットワーク層 
    - ネットワークにおける通信経路の選択（ルーティング）。データ中継。 

### SIPropプロジェクトが考える層構造<br>
上記のOSI参照モデルに、SIPを適用しようとした場合、下記の構造が想定される。
- 第5層 - セッション層 
    - ~~通信プログラム間~~ <span style="color: red">セッション</span>の通信の開始から終了までの手順。
        - すなわち、<span style="font-size: 24px">SIPネットワーク</span>と考えられる。
- 第4層 - トランスポート層 
    - ~~ネットワーク~~ <span style="color: red">Peer(Node,Resource)</span>の端から端までの通信管理（エラー訂正、再送制御等）。 
        - すなわち、<span style="font-size: 24px">P2Pネットワークやオーバーレイネットワーク</span>と考えられる。

<br>

<br>

これらから、下記のような層構造をSIPropプロジェクトは、考えている。
- アプリケーション層
    - アプリケーションのための機能を提供する
        - HTTPやFTPなどのアプリケーション用プロトコル
        - NGN+IMSでいうところの「アプリケーション基盤」に相当する。
- セッション層
    - セッション管理や認証、パーミッションを提供する
        - SIPそのもの
        - NGN+IMSでいうところの「サービス・ストラタム」に相当する。
- トランスポート層
    - Peerやリソースへのリーチャビリティーを提供する
        - P2Pやオーバーレイネットワーク
        - NGN+IMSでいうところの「トランスポート・ストラタム」に相当する。
- ネットワーク層
    - ネットワークを提供する
        - 現在のTCPネットワーク
        - NGN+IMSでいうところの「物理レイヤー」に相当する。

<br>

<br>

これらの適用例：HTTPの場合
- アプリケーション層
    - HTTP通信をする
        - 送信先としては、Socket(TCP)ではなく、SIPスタックやSIPropなどのHTTPをSIPでラップするためのアプリを想定している。
- セッション層
    - HTTPをSIPに載せて、通信をし、認証やパーミッションチェックを行う。
        - SIP-URIベースのSIP IX網ルーティングを想定している。
- トランスポート層
    - SIPをオーバーレイネットワークに載せて、相手リソースへ、通信を行う。
        - DHTであれば、ハッシュによるリソースへのルーティングを想定している。この層により、リーチャビリティーが確保される。
- ネットワーク層
    - 現在のTCP/IPネットワークに相当する通信を行う。
        - Socketによる通信を想定している。


## 参考資料
- SIPropプロジェクト解説
    - PDF版：[SIPropProject_web.pdf](http%3A/www.siprop.org/ja/download_attachment_SIPropProject_web.pdf)
    - PPT版：[SIPropProject_web.ppt](http%3A/www.siprop.org/ja/download_attachment_SIPropProject_web.ppt)
- 汎用セッション・プロトコルとしての SIP を考える
    - PDF版：[Session_SIP_web.pdf](http%3A/www.siprop.org/ja/download_attachment_Session_SIP_web.pdf)
    - PPT版：[Session_SIP_web.ppt](http%3A/www.siprop.org/ja/download_attachment_Session_SIP_web.ppt)
- P2P SIP を考える(構想途中)
    - PDF版：[p2psip_world.pdf](http%3A/www.siprop.org/ja/download_attachment_p2psip_world.pdf)
    - PPT版：[p2psip_world.ppt](http%3A/www.siprop.org/ja/download_attachment_p2psip_world.ppt)
- VoIPと他の世界との橋渡しをするために
    - SIerさん向けの資料<br>
SIのオプションとしてのVoIPとは？という視点で書いた提案資料のネタとなる資料<br>
そういう提案書の書かなければならないときに参考になれば。。。<br>
        - PDF版：[web_with_pm_web.pdf](http%3A/www.siprop.org/ja/download_attachment_web_with_pm_web.pdf)
        - PPT版：[web_with_pm_web.ppt](http%3A/www.siprop.org/ja/download_attachment_web_with_pm_web.ppt)
    - 上記をベースにちゃんと体裁を整えてみたサンプル資料<br>
すでに、受注システムがあるという前提で、それ用のオプションとしてVoiceシステム(CTIシステム)を拡張開発しましょうという提案資料。。。<br>
        - PDF版：[voice_commerce_system.pdf](http%3A/www.siprop.org/ja/download_attachment_voice_commerce_system.pdf)
        - PPT版：[voice_commerce_system.ppt](http%3A/www.siprop.org/ja/download_attachment_voice_commerce_system.ppt)

# プロジェクト一覧

## プロジェクト分類
### シグナリング・プロトコルとしての SIP の拡張を考えたプロジェクト
- [SIPropVer.2.0](http://www.siprop.org/ja/2.0/index.php?SIProp%A4%C3%A4%C6%B2%BF%A1%A9)
    - 世界的に問題となっているVoIP相互接続問題を解決
        - この目的は、下記のプロジェクトに受け継がれている
- [HOTARUプロジェクト](http://www.luciola.net/)
    - SIPropと同様にVoIP相互接続問題を解決(トップダウン方式)
        - 最終的には、誰も利用できるクライアント側のSIPミドルウェア(SIP Stack)となる
        - 汎用セッション・プロトコルとしての SIP用のStackとして使用されることも考えられる
- [SIP IX](http://www.sipix.jp/)プロジェクト
    - SIPropの最終目的であるSIP網の形成を目指したプロジェクト
        - SIPパケットのルーティングをするための網となる
        - 汎用セッション・プロトコルとしての SIPをルーティングすることも考えられる

### 汎用セッション・プロトコルとしての SIP の拡張を考えたプロジェクト
- [雷電](http://raiden.siprop.org/)
    - クライアント間マッシュアップ用UA Servletアプリケーションサーバ
        - SIPやHTTPなどセッションやトランスポートに依存せずに、アプリケーションを書く基盤を作る
- [俺流プロトコル実装入門](http://pipbook.siprop.org/)
    - Web系技術者など普段プロトコルなどを意識する必要のない人に、プロトコルに興味を持ってもらうため
        - 多方面の方々から「汎用セッション・プロトコルとしての SIP」の新しい使い方が提案されることを期待している
- [P2P SIPの実装検討プロジェクト](http://www.p2psip.jp/)
    - P2P世界にSIPを適用したらどうなるか？を模索するプロジェクト
        - 「汎用セッション・プロトコルとしての SIP」の新しい定義を作成する
    - P2P SIP を考える(構想途中)
        - PDF版：[p2psip_world.pdf](http%3A/www.siprop.org/ja/download_attachment_p2psip_world.pdf)
        - PPT版：[p2psip_world.ppt](http%3A/www.siprop.org/ja/download_attachment_p2psip_world.ppt)

## プロジェクト推進団体
### 本家プロジェクト
- [SIPropVer.2.0](http://www.siprop.org/ja/2.0/index.php?SIProp%A4%C3%A4%C6%B2%BF%A1%A9)
- [雷電](http://raiden.siprop.org/)
- [俺流プロトコル実装入門](http://pipbook.siprop.org/)

### 共同プロジェクト
- [WIDEプロジェクト](http://www.wide.ad.jp/)様
    - [HOTARUプロジェクト](http://www.luciola.net/)
- [SIProp勉強会](http://www.siprop.org/ja/2.0/index.php?%B3%AB%C8%AF%2F%A5%B3%A5%DF%A5%E5%A5%CB%A5%C6%A5%A3%A1%BC%2F%CA%D9%B6%AF%B2%F1)の皆様
    - [P2P SIPの実装検討プロジェクト](http://www.p2psip.jp/)

### 提携プロジェクト
- [SIP IX](http://www.sipix.jp/)様
    - [SIP IX](http://www.sipix.jp/)プロジェクト


# 中の人の意見
### [noritsuna](http://www.siprop.org/ja/1.0/index.php?noritsuna)の主張
- blogエントリー
    - [生まれ変わるNTTの進むべき道](http://www.noritsuna.com/archives/2007/09/ntt_6.html)
        - NGNのあり方を語ったエントリー
    - [販売奨励金廃止の理由](http://www.noritsuna.com/archives/2007/09/post_91.html)
        - ケータイというものがどうあるべきかを語ったエントリー
    - [IPv6の使い道](http://www.noritsuna.com/archives/2007/09/ipv6_2.html)
        - IPv6をどう使うのが良さそうかを語ったエントリー

# 名前の由来
- 読み：しっぷろっぷ
- 意味：SIP と  Interop（Interoperability、相互接続性）の造語
- ロゴマーク：
![Logo_SIProp.jpg](http%3A/www.siprop.org/ja/2.0/image_attachment_Logo_SIProp.jpg)
