## 命題論理の複雑な問題を解く (C#)

// この投稿は [数学 Advent Calendar 2016](http://qiita.com/advent-calendar/2016/math) の 23 日目の記事です。

前回の[命題論理を実装する (C#)](Propositional-Logic.md) では命題論理を扱うためのライブラリを作成しました。
今回は、このライブラリを利用して複雑な問題をいくつか解いてみます。

### 騎士と悪漢
まず、前回でも紹介した書籍 [Raymond Smullyan「記号論理学: 一般化と記号化」](https://www.amazon.co.jp/dp/4621085727)から、問題を 2 つ引用します。

```
すべての住民は騎士または悪漢のいずれかであり、騎士はつねに本当のことを言い、悪漢はつねに噓をつく。

問題 1.3
住民 A, B がおり、A は「私たちは二人とも悪漢だ」と言った。
A, B はそれぞれ騎士か悪漢か？

問題 1.5
住民 A, B がおり、A は「私たちは同種 (二人とも騎士または二人とも悪漢のいずれか) だ」と言った。
A, B はそれぞれ騎士か悪漢か？
```

普通に考えて解くこともできますが、この状況を記号で表すことを考えます。  
住民 X が騎士であるという命題を `k_X` とし、X が命題 p を主張したとすると、`k_X` と p の真偽性は等しいということになり、`k_X ≡ p` が成り立ちます。
したがって、問題 1.3 では `k_A ≡ (～k_A ∧ ～k_B)` が、問題 1.5 では `k_A ≡ (k_A ≡ k_B)` が成り立ちます。

前回作成したライブラリ [Blaze](https://github.com/sakapon/Blaze) を使って、次のように解くことができます。  
[PropositionsConsole Knights](https://gist.github.com/sakapon/82ab1ad2b5c2834d01c7076442fd7727)

実行すると、問題 1.3 では A が悪漢、B が騎士と確定します。  
問題 1.5 では B が騎士と確定しますが、A は確定できません。

![Knights](https://github.com/sakapon/Samples-2016/raw/master/Images/MathSample/PropositionsConsole-Knights.png)

### 数当てゲーム
さて次に、[David Gale「Tracking the Automatic ANT: And Other Mathematical Explorations」](https://www.amazon.com/dp/1461274532)という書籍から、数当てゲームを紹介します。

```
・プレイヤーは 2 人
・それぞれのプレイヤーに数が割り当てられており、自身の数を知っているが、相手の数は知らない
・2 人の持つ数は、連続する 2 つの自然数 (つまり、差は 1)
・ターン制で、相手の数を当てれば終了 (わかったら宣言し、わからなければ発声しない)
・先攻・後攻の区別はなく、いずれが宣言してもよい

どちらのプレイヤーが何ターン目に相手の数を当てることができるか？
```

![人型ロボットと数当てゲームをする人のイラスト](https://github.com/sakapon/Samples-2016/raw/master/Images/MathSample/NumberGuess-8-9-dark.jpg)

自分の数が 9 だとすると、与えられた条件から相手の数は 8 または 10 の 2 通りに絞られます。  
ここまではすぐにわかるのですが、いずれかを確定させるには相手の将来の行動あるいは実際の行動の原因を推測する必要があります。
相手がどう考えているかを考える、という日常生活においても重要な能力を問われているようでもあります。

小さい数値でいくつか試してみましょう。  
2 人のプレイヤーには対称性がありますが、ここでは大小関係を固定して、プレイヤー A が n、プレイヤー B が n+1 を持つとします。
以下では、各プレイヤーの心理状況を書くことにします。

n=1 の場合、

- A「B=2 しかありえない。」
- B「A=1 または A=3。A=1 ならば、A は 1 ターン目に当てるはず。A=3 ならば、A は 1 ターン目に静観するはず。」

したがって、A が 1 ターン目で当てて終了します。  
n=2 の場合、

- A「B=1 または B=3。B=1 ならば、B は 1 ターン目に当てるはず。B=3 ならば、B は 1 ターン目に静観するはず。」
- B「A=2 または A=4。A=2 ならば、A は 2 ターン目に当てるはず (B が 1 ターン目に静観するため)。A=4 ならば、A は 2 ターン目まで静観するはず。」

したがって、1 ターン目は両方静観し、A が 2 ターン目で当てて終了します。  
このように、相手がどのように行動してくるかをあらかじめシミュレートしておけば、実際の行動に応じて相手の数を確定できることになります。

この方針でプログラミングしたものが [NumberGuessConsole (GitHub)](https://github.com/sakapon/Samples-2016/tree/master/MathSample/NumberGuessConsole) です (コードが長いため、ここには記載しません)。
このプログラムを A=8, B=9 で実行した結果です。A が 8 ターン目に当てました。  
(「X @ n」は X が n ターン目に当てることを表します。)

![NumberGuessConsole](https://github.com/sakapon/Samples-2016/raw/master/Images/MathSample/NumberGuessConsole-8-9.png)

各プレイヤーの脳内であらかじめシミュレーターを実行することで、値を確定させるための条件を得ます。  
A の Knowledge には `((B = 7)≢(B = 9))∧((B = 7)⇒(B @ 7))∧((B = 9)⇒～(B @ 7))` と入っています。  
B が 7 ターン目に静観したため、A は B=9 だとわかりました。

![NumberGuessConsole](https://github.com/sakapon/Samples-2016/raw/master/Images/MathSample/NumberGuessConsole-8-9-Debug.png)

結果として、小さいほうの数 n を持つプレイヤーが n ターン目に相手の数 n+1 を当てることになります。  
(プログラミングをしなくても、同様の方針により少し複雑な数学的帰納法で証明できます。)

前回: [命題論理を実装する (C#)](Propositional-Logic.md)

**作成したライブラリ**  
[Blaze (GitHub)](https://github.com/sakapon/Blaze)  
[Blaze (NuGet Gallery)](https://www.nuget.org/packages/Blaze/)

**作成したサンプル**  
[PropositionsConsole (GitHub)](https://github.com/sakapon/Samples-2016/tree/master/MathSample/PropositionsConsole)  
[NumberGuessConsole (GitHub)](https://github.com/sakapon/Samples-2016/tree/master/MathSample/NumberGuessConsole)

**素材**  
[不気味の谷現象のイラスト (いらすとや)](http://www.irasutoya.com/2016/02/blog-post_858.html)

**バージョン情報**  
.NET Framework 4.5

**参照**  
[Raymond Smullyan「記号論理学: 一般化と記号化」](https://www.amazon.co.jp/dp/4621085727)  
[David Gale「Tracking the Automatic ANT: And Other Mathematical Explorations」](https://www.amazon.com/dp/1461274532)
