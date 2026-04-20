[開発/サブプロジェクト](../サブプロジェクト.md)

# MultiPacketerとは
簡単に言えば、汎用的なパケット送受信ツールです。<br>
JavaScriptでシーケンスとパケットを定義することにより、どんなパケット(テキストベース)でも、送受信可能です。(SIPでもHTTPでもなんでもOKです。)<br>
この特性を利用して、簡単なシミュレーションやテストに使用することが出来ます。


# 使用法
1. [取得先](../../#l9c1685f.md)から、MultiPacketerを取得します。
1. [必要なライブラリ](../../#t4f251ca.md)を取得します。
1. 上記を、適当なディレクトリに展開します。
1. [JavaScriptファイルの作り方](../../#ve1de087.md)を参考にして、実行したいシーケンスのJavaScriptファイルを作成します。
1. [実行法](../../#hf1eba12.md)に従って実行します。

## 必要なライブラリ
- J2SE5.0以上
- [rhino1_6R2](http://www.mozilla-japan.org/rhino/index.html)
- [junit-3.8](http://www.junit.org/index.htm)
- [log4j-1.2.8](http://logging.apache.org/log4j/docs/)

## ビルドに必要なツール
- [Maven2](http://maven.apache.org/)

## 取得先
### ソースファイル
<!-- -SVNリポジトリ：https://svn.sourceforge.net/svnroot/siprop/trunk/subProject/MultiPacketer -->
- ファイル：[MultiPacketer1.0.src.zip](http://www.siprop.org/ja/1.0/download/stable/MultiPacketer1.0.src.zip)


### バイナリファイル
- [MultiPacketer1.0.zip](http://www.siprop.org/ja/1.0/download/stable/MultiPacketer1.0.zip)

## 実行法
### コマンドライン
1. 下記のライブラリに対して、classpathを通します。
    1. [必要なライブラリ](../../#t4f251ca.md)のライブラリ
    1. ./lib/内のライブラリ
    1. ./target/multiPacketer.jar
1. ./js/init.js のパラメータを環境に合わせて変更します。
    1. このファイルの代わりにコマンドの引数として渡すことも出来ます。状況に応じて使い分けてください。
1. コマンドラインから、下記のコマンドを実行します。
    1. java org.siprop.simulator.multiPacketer.ConsoleMain -Dmultipacket.config=./conf/multipacket.properties {実行したいJavaScriptファイル}

### Maven2
1. Maven2をインストールします。
1. ./install-path.xxx を参考に、必要なライブラリをMaven2にインストールします。
1. ./build.properties を作成して、必要なパラメータを設定します。
    1. localIP="ローカルIP(待ち受けIP)"
    1. localPort="ローカルポート(待ち受けポート)"
    1. remoteIP="リモートIP(送信先IP)"
    1. remotePort="リモートポート(送信先ポート)"
    1. transportType="トランスポートタイプ：[TCP|UDP]"
    1. ipType="IP種別：[IP4|IP6]"
    1. js="実行したいJavaScriptファイル"
1. コマンドラインから、下記のコマンドを実行します。
    1. mvn install


## ディレクトリ構造
```
-conf
 -multipacket.properties     ・・・log4jなどのシステム用設定ファイル
-js                          ・・・サンプルJavaScriptファイル
 -init.js                    ・・・初期パラメータ設定用
 -func_lib_call.js           ・・・SIPメッセージ生成用関数ライブラリ
 -basic_var.js               ・・・基本的なSIPヘッダー変数用ライブラリ
 -basic_message.js           ・・・基本的なSIPメッセージ変数用ライブラリ
 -standard_reg_uac_auth.js   ・・・一般的なREGISTERシーケンス・認証あり
 -standard_call_uac.js       ・・・一般的なUAC通話シーケンス
 -standard_call_uac_auth.js  ・・・一般的なUAC通話シーケンス・認証あり
 -standard_call_uas.js       ・・・一般的なUAS通話シーケンス
-lib
 -JainSipApi1.1.jar          ・・・Jain-SIPライブラリ
 -nist-sdp-1.0.jar           ・・・Jain-SIPライブラリ
 -nist-sip-1.2.jar           ・・・Jain-SIPライブラリ
-log                         ・・・log4j用のログファイル出力先ディレクトリ
-src
 -main                       ・・・本体のソースファイル
 -test                       ・・・test用ソースファイル
-pom.xml                     ・・・Maven2用設定ファイル
-build.xml                   ・・・実行用設定ファイル(ant用ファイル)
-install-path.bat            ・・・Maven2用の外部ライブラリインストール用バッチ
-install-path.sh             ・・・Maven2用の外部ライブラリインストール用バッチ
```

## JavaScriptファイルの作り方
1. 必要となるメッセージやヘッダーを作成し、変数にセットします。
    1. ヘッダーやパラメータ(変数)の定義：RequestURIの例<br>
下記のように定義します。jsEngine.setVar(String,String)で、設定した変数は、{変数名}とすることにより展開されます。<br>
下記例では、requestLine は、「INVITE sip:req@siprop.org SIP/2.0\r\n」と展開されます。<br>
また、システムが最初から保持している、[システム変数](../../#ece60a8e.md)もあります。


```
var requestUri = "sip:req@siprop.org";
jsEngine.setVar("REQUEST_URI", requestUri);
```
 
```
var requestLine = "INVITE {REQUEST_URI} SIP/2.0\r\n";
jsEngine.setVar("REQUEST_LINE", requestLine);
```

    1. メッセージの定義：INVITEリクエストの例<br>
上記のヘッダーやパラメータを設定し、下記のような形で定義します。<br>
下記のすべての変数は、事前に定義しておく必要がありますが、本プログラムにおいては「basic_var.js」にて定義済みとなっています。

```
var INVITE_REQUEST = "INVITE {REQUEST_URI} SIP/2.0\r\n" +
                      "Call-ID:{CALL_ID}\r\n" +
                      "CSeq:{CSEQ_NUM} {METHOD}\r\n" +
                      "VIA:SIP/2.0/{TRANSPORT_TYPE} {LOCAL_IP}:{LOCAL_PORT};branch={BRANCH}\r\n" +
                      "From:\"{FROM_DISPLAYNAME}\" <{FROM_URI}>;tag={FROM_TAG}\r\n" +
                      "To:\"{TO_DISPLAYNAME}\" <{TO_URI}>\r\n" +
                      "Contact:<sip:{LOCAL_IP}:{LOCAL_PORT}>\r\n" +
                      "Allow: INVITE,ACK,CANCEL,BYE,NOTIFY,REFER,OPTIONS,MESSAGE\r\n" +
                      "Max-Forwards:70\r\n" +
                      "Content-Type: application/sdp\r\n" +
                      "Content-Length:{CONTENT_LENGTH}\r\n\r\n" +
                      "{STANDARD_SDP}";
```

1. 続いて、シーケンスを定義します。<br>
下記例は、「INVITE送信-200OK受信-ACK送信」という一般的なSIPシーケンスとなります。<br>
※jsEngine.importScript()で、外部で定義したJavaScriptを呼びだし可能です。<br>
このほかにも、[JavaScript内で使用可能なシステム関数](../../#p7377b8c.md)があります。

```
// createACK()関数が定義されたライブラリの呼び出し
jsEngine.importScript("./js/func_lib_call.js"); 
```
 
```
// INVITEを送信
jsEngine.send(INVITE_REQUEST);
// 200OKを受信して、OK_RESPONSEにその内容をセットする。
var OK_RESPONSE = jsEngine.receive();
// 200OKから、ACKリクエストを生成する。
var ACK_REQUEST = createACK(OK_RESPONSE);
// 上記で、生成したACKを送信する。
jsEngine.send(ACK_REQUEST);
```

1. 以上で、終了です。あとは、実行してください。<br>

1. Tips<br>
    1. 上記例では、ほとんど、JavaScriptの関数などを利用していませんが、例えば、下記のようにすることにより、1xx応答が複数回来ることに対応可能となります。<br>
このように、JavaScriptの関数や機能を使うことにより、さまざまな状況に対応できます。

```
// 200OKパケットであるかをチェック。
var OK_RESPONSE = new String();
while(OK_RESPONSE.indexOf("SIP/2.0 200") != 1) {
  OK_RESPONSE = jsEngine.receive();
}
```

## JavaScript内で使用可能なシステム関数
JavaScript内で使用できるシステム関数です。最新情報は、[こちら](http://www.siprop.org/ja/1.0/javadoc/MultiPacketer/org/siprop/simulator/multiPacketer/JSEngine.html)を参照してください。jsFunction_XXXとなっているものが、上記の関数に相当します。<br>
|  |  |
| :---: | :---: |
| メソッド | 意味 |
| [void jsEngine.send(String);](http://www.siprop.org/ja/1.0/javadoc/MultiPacketer/org/siprop/simulator/multiPacketer/JSEngine.html#jsFunction_send(java.lang.String)) | Stringを送信します。 |
| [String jsEngine.receive();](http://www.siprop.org/ja/1.0/javadoc/MultiPacketer/org/siprop/simulator/multiPacketer/JSEngine.html#jsFunction_receive()) | 受信待ちをします。受信したメッセージをStringで返します。受信できなかった場合、変数TIMEOUT時間後、タイムアウトします。 |
| [void jsEngine.importScript(String);](http://www.siprop.org/ja/1.0/javadoc/MultiPacketer/org/siprop/simulator/multiPacketer/JSEngine.html#jsFunction_importScript(java.lang.String)) | Stringで指定したJavaScriptファイルをimportします。 |
| [void jsEngine.print(String);](http://www.siprop.org/ja/1.0/javadoc/MultiPacketer/org/siprop/simulator/multiPacketer/JSEngine.html#jsFunction_print(java.lang.String)) | StringをTracerに表示します。 |
| [String1 jsEngine.replaceVariables(String2);](http://www.siprop.org/ja/1.0/javadoc/MultiPacketer/org/siprop/simulator/multiPacketer/JSEngine.html#jsFunction_replaceVariables(java.lang.String)) | String2を変数に従って変換したString1を返します。 |
| [void jsEngine.setVar(String1,String2);](http://www.siprop.org/ja/1.0/javadoc/MultiPacketer/org/siprop/simulator/multiPacketer/JSEngine.html#jsFunction_setVar(java.lang.String,java.lang.String)) | 変数String1にString2をセットします。 |
| [String1 jsEngine.getVar(String2);](http://www.siprop.org/ja/1.0/javadoc/MultiPacketer/org/siprop/simulator/multiPacketer/JSEngine.html#jsFunction_getVar(java.lang.String)) | 変数String2を取得します。 |
| [void jsEngine.setVariablesForSIPByPacket(String);](http://www.siprop.org/ja/1.0/javadoc/MultiPacketer/org/siprop/simulator/multiPacketer/JSEngine.html#jsFunction_setVariablesForSIPByPacket(java.lang.String)) | 変数StringからSIPヘッダーを取得し、ヘッダー名の大文字を変数名としてセットします。例：Toヘッダーは、変数TOにセットされます。 |
| [void jsEngine.sleep(int);](http://www.siprop.org/ja/1.0/javadoc/MultiPacketer/org/siprop/simulator/multiPacketer/JSEngine.html#jsFunction_sleep(double)) | intミリ秒停止します。 |
| [String jsEngine.calcAuth(String authResponse, String requestUri, String username, String password);](http://www.siprop.org/ja/1.0/javadoc/MultiPacketer/org/siprop/simulator/multiPacketer/JSEngine.html#jsFunction_calcAuth(String,String,String,String)) | 401,407応答、request-URI、ユーザ名、パスワードを渡すと、StringにAuthヘッダーを返します。 |
| [void jsEngine.setReceiveMode(Boolean b);](http://www.siprop.org/ja/1.0/javadoc/MultiPacketer/org/siprop/simulator/multiPacketer/JSEngine.html#jsFunction_setReceiveMode(Boolean)) | trueをセットするとjsEngine.receive()がsent-byのポートで待ち受け、falseにするとjsEngine.receive()がLocalポートで待ち受けをします。 |


## システム変数
システムが最初から用意している変数一覧です。最新情報は、[こちら](http://www.siprop.org/ja/1.0/javadoc/MultiPacketer/org/siprop/simulator/multiPacketer/SystemVariables.html)を参照してください。<br>
|  |  |
| :---: | :---: |
| 変数名 | 意味 |
| [ORIGINAL_PACKET](http://www.siprop.org/ja/1.0/javadoc/MultiPacketer/org/siprop/simulator/multiPacketer/SystemVariables.html#ORIGINAL_PACKET) | 最初に送受信したパケット |
| [CURRENT_PACKET](http://www.siprop.org/ja/1.0/javadoc/MultiPacketer/org/siprop/simulator/multiPacketer/SystemVariables.html#CURRENT_PACKET) | 現在の送受信したパケット |
| [RECEIVE_PACKET](http://www.siprop.org/ja/1.0/javadoc/MultiPacketer/org/siprop/simulator/multiPacketer/SystemVariables.html#RECEIVE_PACKET) | 最後に受信したパケット |
| [SEND_PACKET](http://www.siprop.org/ja/1.0/javadoc/MultiPacketer/org/siprop/simulator/multiPacketer/SystemVariables.html#SEND_PACKET) | 最後に送信したパケット |
| [LOCAL_IP](http://www.siprop.org/ja/1.0/javadoc/MultiPacketer/org/siprop/simulator/multiPacketer/SystemVariables.html#LOCAL_IP) | ローカルIP(待ち受けIP) |
| [LOCAL_PORT](http://www.siprop.org/ja/1.0/javadoc/MultiPacketer/org/siprop/simulator/multiPacketer/SystemVariables.html#LOCAL_PORT) | ローカルポート(待ち受けポート) |
| [REMOTE_IP](http://www.siprop.org/ja/1.0/javadoc/MultiPacketer/org/siprop/simulator/multiPacketer/SystemVariables.html#REMOTE_IP) | リモートIP(送信先IP) |
| [REMOTE_PORT](http://www.siprop.org/ja/1.0/javadoc/MultiPacketer/org/siprop/simulator/multiPacketer/SystemVariables.html#REMOTE_PORT) | リモートポート(送信先ポート) |
| [RECEIVE_IP](http://www.siprop.org/ja/1.0/javadoc/MultiPacketer/org/siprop/simulator/multiPacketer/SystemVariables.html#RECEIVE_IP) | VIAのsent-by相当のIP |
| [RECEIVE_PORT](http://www.siprop.org/ja/1.0/javadoc/MultiPacketer/org/siprop/simulator/multiPacketer/SystemVariables.html#RECEIVE_PORT) | VIAのsent-by相当のポート |
| [TRANSPORT_TYPE](http://www.siprop.org/ja/1.0/javadoc/MultiPacketer/org/siprop/simulator/multiPacketer/SystemVariables.html#TRANSPORT_TYPE) | トランスポートタイプ |
| [IS_RECEIVE_MODE](http://www.siprop.org/ja/1.0/javadoc/MultiPacketer/org/siprop/simulator/multiPacketer/SystemVariables.html#IS_RECEIVE_MODE) | ローカルIPを使用するかsent-byを使用するか |
| [TIMEOUT](http://www.siprop.org/ja/1.0/javadoc/MultiPacketer/org/siprop/simulator/multiPacketer/SystemVariables.html#TIMEOUT) | Socketの待ち受けタイムアウト時間(ms) |

## 拡張するには
[JSControllerクラス](http://www.siprop.org/ja/1.0/javadoc/MultiPacketer/org/siprop/simulator/multiPacketer/JSController.html)が外部とのインタフェースを担っています。

```
～～～～～いろいろな処理～～～～～
String result = "いろいろな処理の結果";
String jsCode = "jsEngine.setVar(\"RESULT\","\" + result  + "\");" +
               "jsEngine.importScript(\"./mainFunc.js\");";
```
 
```
JSController jsCon = new JSController();
jsCon.execByText(jsCode);
```

とすることにより、アプリ上からJavaScriptを実行することが出来ます。<br>
mainFunc.jsには、変数RESULTを処理する内容が書かれているものとします。


# 各種資料
## ドキュメント
- [Javadoc](http://www.siprop.org/ja/1.0/javadoc/MultiPacketer/)

## Maven2コマンド
|  |  |
| :---: | :---: |
| コマンド | 意味 |
| mvn compile | ソースをコンパイルします。 |
| mvn test | テストを実行します。 |
| mvn package | jarファイルを作成します。 |
| mvn install | build.xmlに沿って、実行します。 |
| mvn site | javadocを作成します。 |
