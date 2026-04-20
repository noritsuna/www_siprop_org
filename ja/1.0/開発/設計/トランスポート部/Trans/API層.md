[開発/設計](../../../設計.md)

# 目的
- プロトコルやAPIの差異を吸収する

# 実装方針
- [メッセージ変換部](../../制御モジュール/メッセージ変換部.md)とプロトコルまたはAPIが連携する形とする
- アダプタパターンとして、[FlatSIPメッセージ](../../オブジェクト/FlatSIPメッセージ.md)を入出力する

# 定義方法

# 実装クラス
- [Transport](http://www.siprop.org/ja/javadoc/prototype_design/org/siprop/prototype/j2se/transport/Transport.html)
- [DirectConnecter](http://www.siprop.org/ja/javadoc/prototype_design/org/siprop/prototype/j2se/transport/DirectConnecter.html)

# 機能
- メッセージの自動判定
    - [メッセージ変換部](../../制御モジュール/メッセージ変換部.md)のチェック関数に順次メッセージを投げる
