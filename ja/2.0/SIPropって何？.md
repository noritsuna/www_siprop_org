[FrontPage](FrontPage.md)

# SIPropって何？
SIPropには、2つの意味があり、下記にて解説しているというB2BUAミドルウェアとしてのSIPropと本コミュニティーを示す[SIPropプロジェクト](SIPropプロジェクト.md)としてのSIPropがある。

## [SIPropプロジェクト](SIPropプロジェクト.md)について
コミュニティーとしての[SIPropプロジェクト](SIPropプロジェクト.md)。

### 詳細
- [SIPropプロジェクト](SIPropプロジェクト.md)を参照のこと。


## SIProp Ver.2.0について
[SIProp Ver.1.0](http://www.siprop.org/ja/1.0/)の反省点を踏まえて、フルスクラッチで全てを新規に実装し直したSIPropである。

### 概要
- 世界的に問題となっているVoIP(SIP((Session Initiation Protocol。VoIP用プロトコルとして広く採用されている。)))相互接続問題を解決するB2BUA&UAフレームワークである。
- これにより、SIPが本来目指していたセッション層の汎用プロトコルとしての地位を取り戻すためのものであり、国内初(？)のSIPオープンソースプロジェクトである。


### 特徴
- B2BUA((Back To Back User Agent。))やUAを作成するためのフレームワーク
    - B2BUAやUAを容易に開発できるように機能単位で実装が行える
- B2BUAやUAの動作をモジュールにて定義可能なモデル
    - UAを機能単位で定義し、組み合わせて使用する
        - 認証UAやアーリートーキUAなどの単位でUAがある

### 利点
- HTTP上のWebアプリのように、SIP上のコミュニケーションアプリの開発の容易化
- 今後、世界的に主流となるNGN((Next Generation Network。IPをベースとした次世代基幹ネットワーク網。))やIMS((IP Multimedia Subsystem。NGNで、各サービスを行うためのベースシステム。))、FMC((Fixed Mobile Convergence。固定網とモバイル網の融合したネットワークやサービスなど。))への早期対応

### 目標
- B2BUAやUAの開発の容易化
- [SIP IX](http://www.sipix.jp/)と[Asterisk](http://www.asterisk.org/)との連携による相互接続問題の解決
    - [SIP IX](http://www.sipix.jp/)のVoIP網による相互接続網の構築
    - [Asterisk](http://www.asterisk.org/)のデファクトスタンダード化による相互接続問題の解決

### Ver.1.0からの変更点
- [変更点](開発/設計/変更.md)

### *ライセンス
- [Apache License Version 2.0](http://www.apache.org/licenses/LICENSE-2.0.txt)

# [SIProp/PJ体制](http://www.siprop.org/ja/1.0/index.php?%A5%E1%A5%F3%A5%D0%A1%BC)
- オープンソース＆バザールモデル
    - 皆様の参加をお待ちしています。<(_ _)>
- [体制一覧](http://www.siprop.org/ja/1.0/index.php?%A5%E1%A5%F3%A5%D0%A1%BC)

# 名前の由来
- 読み：しっぷろっぷ
- 意味：SIP と  Interop（Interoperability、相互接続性）の造語
- ロゴマーク：
![Logo_SIProp.jpg](http%3A/www.siprop.org/ja/2.0/image_attachment_Logo_SIProp.jpg)
