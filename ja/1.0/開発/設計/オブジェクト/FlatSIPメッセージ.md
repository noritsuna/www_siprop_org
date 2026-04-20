[開発/設計](../../設計.md)

# 位置づけ
- 機能単位でのSIPメッセージ&シーケンスのベストプラクティス集

# 実装クラス
- [FlatSIPMessage](http://www.siprop.org/ja/javadoc/prototype_design/org/siprop/prototype/j2se/message/FlatSIPMessage.html)

# 機能
- 最大公約数的な「パターン化したSIP」
- 最小公倍数的な「SIP条件」
    - 例えば、Fromヘッダーにおける「user=phone」パラメータは、「あると動作しないUA」と「ないと動作しないUA」の矛盾したUAが存在する。<br>
この場合、「パターン化したSIP」として、「user=phone」があるものとないものを用意し、「Request-URI」や「User-Agent」などの「SIP条件」により、どの「パターン化したSIP」を使用するかを定める。

# 最終目的
- Internet-DraftのExamplesを定義し、提案する
