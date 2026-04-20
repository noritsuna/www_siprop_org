[開発](開発.md)


# SIProp

## 必要なライブラリ
- J2ME PersonalProfile

## 取得先
### ソースファイル
- ファイル：[SIProp1.0.src.zip](http://www.siprop.org/ja/1.0/download/stable/SIProp1.0.src.zip)

## 実行法
### コマンドライン
1. 下記のライブラリに対して、classpathを通します。
    1. [必要なライブラリ](#t4f251ca.md)のライブラリ
    1. ./lib/内のライブラリ
1. コマンドラインから、下記のコマンドを実行します。
    1. java org.siprop.B2BUAMain -Dsiprop.config=./conf/siprop.properties


## ディレクトリ構造
```
-conf
 -siprop.properties     ・・・log4jなどのシステム用設定ファイル
-lib
 -JainSipApi1.1.jar          ・・・Jain-SIPライブラリ
-src
 -main                       ・・・本体のソースファイル
```

# 各種資料
## ドキュメント
- [Javadoc](http://www.siprop.org/ja/1.0/javadoc/SIProp/)
