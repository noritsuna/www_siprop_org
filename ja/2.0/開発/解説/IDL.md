[開発/解説](../解説.md)

# 意味
オブジェクト指向プログラミング言語におけるオブジェクトについて、オブジェクトを他のプログラムから利用するための接点（インターフェース）を定義するために用いられる言語のことである。<br>

以上、[BINARY](http://www.sophia-it.com/content/IDL)より抜粋。<br>

## SIPropでの意味
SIPropにおいても、言語汎用的なインタフェースの定義言語として使用しますが、これを元に特定言語へコンパイルすることは考慮しません。


# 詳細解説
IDLを記述する上で、必要最小限のことを解説します。<br>
また、マッピング対象としては、Javaを想定しています。

## 文法
サンプルコード：
```
module SIProp {
  const long typename = 1;
  interface SIPropClass1 {
    long operation(in long id);
  }
  exception e1 {
    string reason;
  };
}
```

対応表：
|  |  |
| --- | --- |
| IDL | Java |
| module | パッケージ |
| interface | クラス |
| operation | メソッド |
| exception | 例外 |
| const | 定数宣言 |

### 注意点
- 大文字小文字の区別はない
- moduleとinterfaceに同一名称は利用できない
- Cと同じように前方宣言しないと未定義エラーとなる
- #pragma prefix で、using と同様の効果を得ることが出来る
    - JDK1.4以降は、コマンドラインから指定する
- 引数の型の前に付ける「パラメータ引き渡しモード」は、「in」のみ使用する


## 型
型の対応表:
|  |  |
| --- | --- |
| IDL | Java |
| boolean | boolean |
| char | char |
| octet | byte |
| string | String |
| short | short |
| unsigned short | short |
| long | int |
| unsigned long | int |
| long long | long |
| unsigned long long | long |
| float | float |
| double | double |
| struct | クラス |
| enum | クラス |
| union | クラス |
| typedef sequence<string> ArrayString | ArrayString[] (配列の指定法) |

### 注意点
- int と long の関係が、直感的ではない
- octet という見慣れない型がある


# 参考資料
- [JavaIDLドキュメント](http://java.sun.com/j2se/1.4/ja/docs/ja/guide/idl/index.html)
- [Java mets IDL](http://www.02.246.ne.jp/~torutk/javacorba/javameetsidl.html)
- [TECHSCORE-CORBA](http://www.techscore.com/tech/CORBA/chapter5.html)
