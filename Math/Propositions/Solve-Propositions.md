## 命題論理の複雑な問題を解く (C#)

// この投稿は [数学 Advent Calendar 2016](http://qiita.com/advent-calendar/2016/math) の 23 日目の記事です。

前回の[命題論理を実装する (C#)](Propositional-Logic.md) では命題論理を扱うためのライブラリを作成しました。
今回は、このライブラリを利用して複雑な問題をいくつか解いてみます。

まず、前回でも紹介した書籍 [Raymond Smullyan「記号論理学: 一般化と記号化」](https://www.amazon.co.jp/dp/4621085727)から、問題を 2 つ引用します。

```
すべての住民は騎士または悪漢のいずれかであり、騎士はつねに本当のことを言い、悪漢はつねに噓をつく。

問題 1.3 住民 A, B がおり、A は「私たちは二人とも悪漢だ」と言った。
A, B はそれぞれ騎士か悪漢か？

問題 1.5 住民 A, B がおり、A は「私たちは同種 (二人とも騎士または二人とも悪漢のいずれか) だ」と言った。
A, B はそれぞれ騎士か悪漢か？
```

普通に考えて解くこともできますが、この状況を記号で表すことを考えます。  
住民 X が騎士であるという命題を `k_X` とし、X が命題 p を主張したとすると、`k_X` と p の真偽性は等しいということになり、`k_X ≡ p` が成り立ちます。
したがって、問題 1.3 では `k_A ≡ (～k_A ∧ ～k_B)` が、問題 1.5 では `k_A ≡ (k_A ≡ k_B)` が成り立ちます。

前回作成したライブラリを使って、次のように解くことができます。  
[PropositionsConsole Knights](https://gist.github.com/sakapon/82ab1ad2b5c2834d01c7076442fd7727)

実行すると、問題 1.3 では A が悪漢、B が騎士と確定します。  
問題 1.5 では B が騎士と確定しますが、A は確定できません。  
![Knights](https://github.com/sakapon/Samples-2016/raw/master/Images/MathSample/PropositionsConsole-Knights.png)

[David Gale「Tracking the Automatic ANT: And Other Mathematical Explorations」](https://www.amazon.com/dp/1461274532)

![人型ロボットと数当てゲームをする人のイラスト](https://github.com/sakapon/Samples-2016/raw/master/Images/MathSample/NumberGuess-8-9-dark.jpg)

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
