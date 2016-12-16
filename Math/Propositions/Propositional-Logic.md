## 命題論理を実装する (C#)

// この投稿は [数学 Advent Calendar 2016](http://qiita.com/advent-calendar/2016/math) の 16 日目の記事です。

命題といえば高校数学で「かつ」「または」「ならば」などを学習することと思いますが、
この投稿では、これらを一般化、形式化した命題論理をプログラミングで表現することを目指します。

用語や定義については、主に [Raymond Smullyan「記号論理学: 一般化と記号化」](https://www.amazon.co.jp/dp/4621085727)の第 7 章「命題論理入門」に記載されているものを使い、解説と実装をしていきます。
プログラミング言語としては C# (.NET) を利用します。

[Raymond Smullyan「記号論理学: 一般化と記号化」](https://www.amazon.co.jp/dp/4621085727)

### 論理結合子、論理式
任意の命題 p は、真偽値 (truth value) を持ちます。真偽値とは、真 (T) または偽 (F) です。
そして、「かつ」「または」「ならば」などを一般化した概念を論理結合子 (logical connective) と呼び、次のものがあります。

1. 否定 (～p)  
真偽値を反転させます。
1. 論理積 (p ∧ q)  
p および q がともに真のときに限り真です。
1. 論理和 (p ∨ q)  
p または q のうち、少なくとも一方が真のときに真です。
1. 含意 (p ⇒ q)  
～p ∨ q と同義です。
1. 同値 (p ≡ q)  
(p ⇒ q) ∧ (q ⇒ p) と同義です。計算機においては、XOR 演算の否定と同義です。

さらに、論理式を次のように定義します。

- 命題変数 (propositional variable): 命題を表す変数。値は、真または偽となりうる。
- 命題定数 (propositional constant): 真を表す *t* および偽を表す *f*。
- 論理式 (formula): 命題変数、命題定数、論理結合子により構成される式。

ここで以上の内容をもとに、論理式を実装します。
論理結合子は、否定のみ単項式で、他は二項式です。

[Formula.cs](https://gist.github.com/sakapon/fa4a0bf84a702e6b066b093be055b201)

式を簡単に記述できるようにするため、静的メソッドを用意します。

[Formula.cs](https://gist.github.com/sakapon/f32b48c1aea3357ae3d37460552043da)

次回は、このライブラリを利用してもう少し高度な問題を解いてみます。

次回: 12/23 (予定)

**作成したライブラリ**  
[Blaze (GitHub)](https://github.com/sakapon/Blaze)

**作成したサンプル**  
[PropositionsConsole (GitHub)](https://github.com/sakapon/Samples-2016/tree/master/MathSample/PropositionsConsole)

**バージョン情報**  
.NET Framework 4.5

**参照**  
[Raymond Smullyan「記号論理学: 一般化と記号化」](https://www.amazon.co.jp/dp/4621085727)
