# SIPropって何？
世界的に問題となっているVoIP(SIP((Session Initiation Protocol。VoIP用プロトコルとして広く採用されている。)))相互接続問題を解決するミドルウェアである。<br>
これにより、SIPが本来目指していたセッション層の汎用プロトコルとしての地位を取り戻すためのものであり、国内初(？)のSIPオープンソースプロジェクトである。

# 背景
開発予算や期間の都合により、下記のような制約を持った相互接続性の低いSIP-UAが、作成されている。
これにより、相互接続テストが世界的に問題となっている。
- 接続対象の網を制約する
    - 発注元のキャリア網のみ
- 接続対象のUAを制約する
    - 自社のSIP-UAのみ
    - キャリア指定のSIP-UAのみ
- 機能(サービス)を制約する
    - 通話のみ
    - キャリア指定の機能のみ

## 現在の解決策
- 下図のような仕様差を吸収するB2BUAを網間に設置する
    - 各キャリアから、それぞれの網に特化したSIP-UAとして動作する
![b2buaFig.png](http%3A/www.siprop.org/ja/1.0/image_attachment_b2buaFig.png)

# 特長
- クライアントサイドで動作するB2BUAモジュール
- B2BUA((Back To Back User Agent。))の動作をモジュールにて定義可能なモデル
- 各UAや各網のSIPを、標準化されたSIPに変換する([標準化SIP(FlatSIP)](開発/設計/オブジェクト/FlatSIPメッセージ.md))
- モバイル端末向けの実装
![sipropFig.png](http%3A/www.siprop.org/ja/1.0/image_attachment_sipropFig.png)

# 利点
- 処理をクライアントに分散可能
    - 特に、メディア変換の負荷分散
- 世界的な関心事項となっているVoIP相互接続問題の解決
- 独自機能を追加可能
    - 対向UA自動判別も可能なことにより、SIProp非対応端末も対応可能
- HTTP上のWebアプリのように、SIP上のコミュニケーションアプリの開発の容易化
- 今後、世界的に主流となるNGN((Next Generation Network。IPをベースとした次世代基幹ネットワーク網。))やIMS((IP Multimedia Subsystem。NGNで、各サービスを行うためのベースシステム。))、FMC((Fixed Mobile Convergence。固定網とモバイル網の融合したネットワークやサービスなど。))への早期対応

# 目標
- SIP-UAやコミュニケーションアプリのセッション層として、デファクトスタンダードとなる。<br>
これにより、すべてのノードとセッションを張ることが可能となり、シームレスな通信が可能となる。
    - ライブラリとしてのデファクトスタンダード化
    - コードとしてのリファレンス実装化
- [標準化SIP(FlatSIP)](開発/設計/オブジェクト/FlatSIPメッセージ.md)を策定し、Internet-Draftのexamplesとして提案する
    - SIP-UAとの接続におけるベストプラクティス集<br>
最大公約数的な「[パターン化したSIP](開発/設計/オブジェクト/FlatSIPメッセージ/パターン化したSIP.md)」と最小公倍数的な「[SIP条件](開発/設計/オブジェクト/FlatSIPメッセージ/SIP条件.md)」を探し出すことを目標とする。
        - Fromヘッダーにおける「user=phone」パラメータなどは、ないと動作しないUAとあると動作しないUAの矛盾したUAが存在する。<br>
この場合、「[パターン化したSIP](開発/設計/オブジェクト/FlatSIPメッセージ/パターン化したSIP.md)」として、「user=phone」があるものとないものを用意し、「Request-URI」や「User-Agent」などの「[SIP条件](開発/設計/オブジェクト/FlatSIPメッセージ/SIP条件.md)」により、どのパターンを使用するかを定めること。


# SIProp/PJ体制
- オープンソース＆バザールモデル
    - 皆様の参加をお待ちしています。<(_ _)>

# ライセンス
- [Apache License Version 2.0](http://www.apache.org/licenses/LICENSE-2.0.txt)

# 名前の由来
- 読み：しっぷろっぷ
- 意味：SIP と  Interop（Interoperability、相互接続性）の造語
- ロゴマーク：
![Logo_SIProp.jpg](http%3A/www.siprop.org/ja/1.0/image_attachment_Logo_SIProp.jpg)
