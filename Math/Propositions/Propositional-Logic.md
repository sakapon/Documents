## 命題論理を実装する (C#)

// この投稿は [数学 Advent Calendar 2016](http://qiita.com/advent-calendar/2016/math) の 16 日目の記事です。

命題といえば高校数学で「かつ」「または」「ならば」などを学習すると思いますが、
この投稿では、これらを一般化、形式化した命題論理をプログラミングで表現することを目指します。
プログラミングをしない方も、適宜飛ばせばなんとか読めると思います。

用語や定義については、主に [Raymond Smullyan「記号論理学: 一般化と記号化」](https://www.amazon.co.jp/dp/4621085727)の第 7 章「命題論理入門」に記載されているものを使い、解説と実装をしていきます。
プログラミング言語としては C# (.NET) を利用します。

[Raymond Smullyan「記号論理学: 一般化と記号化」](https://www.amazon.co.jp/dp/4621085727)

### 論理結合子
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

### 論理式
さらに、論理式を次のように定義します。

- 命題変数 (propositional variable): 命題を表す変数。値は、真または偽となりうる。
- 命題定数 (propositional constant): 真を表す *t* および偽を表す *f*。
- 論理式 (formula): 命題変数、命題定数、論理結合子により構成される式。

ここまでの内容をもとに、論理式を実装します。
論理結合子は、否定のみが単項で、他はすべて二項です。

[Formula classes](https://gist.github.com/sakapon/fa4a0bf84a702e6b066b093be055b201)

式を簡単に記述できるようにするため、静的メソッドを用意します。

[Formula facade](https://gist.github.com/sakapon/f32b48c1aea3357ae3d37460552043da)

### 恒真式および矛盾
論理式のうち、含まれる命題変数がどのような真偽値の組合せになっても真であるものを恒真式 (tautology) と呼びます。
逆に、含まれる命題変数がどのような真偽値の組合せになっても偽であるものを矛盾論理式または矛盾 (contradiction) と呼びます。
簡単な例を挙げると、p ∨ ～p は恒真式で、p ∧ ～p は矛盾です。

論理式が恒真式あるいは矛盾であるかどうかを判定するためのメソッドは、次のように実装できます。

[Tautology](https://gist.github.com/sakapon/769cbcfa1fbb4bf89dc6432e8ac57699)

以上のコードをまとめて、[Blaze (GitHub)](https://github.com/sakapon/Blaze) というライブラリとして公開しています。
NuGet でインストールできます。

### ライブラリを使ってみる
さて、このライブラリを利用すると、命題論理に関する定理を証明できるようになります。
実際にいくつかやってみましょう。
Visual Studio で新規のコンソール アプリケーション プロジェクトを作成して、NuGet で [Blaze](https://www.nuget.org/packages/Blaze/) をインストールします。
そして次のようなコードを記述します。

[PropositionsConsole](https://gist.github.com/sakapon/7d1a9b3ec24c442e2b161dfb6da3d1ad)

恒真式であるということは、その論理式がつねに成り立つということです。
このコードを実行することで、三段論法、背理法、対偶などを証明できます。

![PropositionsConsole](https://github.com/sakapon/Samples-2016/raw/master/Images/MathSample/PropositionsConsole.png)

次回は、このライブラリを利用してもう少し高度な問題を解いてみます。

次回: [命題論理の複雑な問題を解く (C#)](Solve-Propositions.md)

**作成したライブラリ**  
[Blaze (GitHub)](https://github.com/sakapon/Blaze)  
[Blaze (NuGet Gallery)](https://www.nuget.org/packages/Blaze/)

**作成したサンプル**  
[PropositionsConsole (GitHub)](https://github.com/sakapon/Samples-2016/tree/master/MathSample/PropositionsConsole)

**バージョン情報**  
.NET Framework 4.5

**参照**  
[Raymond Smullyan「記号論理学: 一般化と記号化」](https://www.amazon.co.jp/dp/4621085727)
