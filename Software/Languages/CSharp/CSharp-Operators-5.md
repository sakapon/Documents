# C# で演算子を実装する (5)

[前回の記事](CSharp-Operators-4.md)では、キャスト演算子、インデクサーなどについて説明しました。  
今回は論理演算子を扱います。

ユーザー定義型で直接オーバーロードできる論理演算子には、`!`, `&`, `^`, `|` さらに `true` および `false` 演算子があります。  
ただし整数型においては、`&`, `^`, `|` はビット演算を表します。  
また、否定 `!` は、整数のビット演算においては補数 `~` に相当します。

論理演算については、bool 型や bool? 型の状態で扱えば実務上は事足りることが多く、ユーザー定義型でこれらの演算子をオーバーロードして使う機会はあまりないと思います。  
そこで、今回は `&&` および `||` 演算子による短絡評価 (ショート サーキット) を検証してみました。

### 短絡評価 (ショート サーキット)
ユーザー定義型で `&&` および `||` 演算子による短絡評価を利用するには、次の条件を満たす必要があります。
- `&` および `|` について、2つの引数と戻り値の型はすべて、定義元の型
- `true` および `false` 演算子が宣言されている

そしてこのとき、`x && y` は x が偽を表すならば y には何もせず x を返し、そうでなければ `x & y` を評価します。
形式的に書くと `T.false(x) ? x : (x & y)` のようになります。  
同様に、`x || y` は x が真を表すならば y には何もせず x を返し、そうでなければ `x | y` を評価します。
形式的に書くと `T.true(x) ? x : (x | y)` のようになります。

bool 型における短絡評価も上記の法則に従っていると見なすことができ、短絡評価の概念を bool 型以外に拡張したことになります。
なお、bool? 型では `&&` および `||` 演算子を利用できません。

さて、上記の仕組みから考えると、短絡評価を適用できる論理体系とは、x と y のうち1つ以上が偽を表すのであれば `x & y` も偽を表し、x と y のうち1つ以上が真を表すのであれば `x | y` も真を表すものである必要があります。  
これは、真・偽のほかに第3の値の存在を考える[3値論理](https://t.co/WRXPzaVG1D)で体系を構成する場合、クリーネの3値論理に相当します。  
bool? 型における論理演算 (`!`, `&`, `^`, `|`) もこのクリーネの3値論理に従います。  
3値論理については記事の後半で補足します。

### 実装例
今回の実装例では、与えられた文字列が真を表すのか、偽を表すのか、それ以外かの3値を判定する構造体を作成し、各論理演算子をオーバーロードしています。  
以下にソースコードを示します。

https://gist.github.com/sakapon/9d92680e9c68f3d53bb8e0c6b66ee443

コンストラクターの処理で bool? に変換することもできますが、この例ではあえて Value プロパティで入力の文字列を保持することとし、論理演算を自作しています。
上記のように実装することで、下記のコードのように短絡評価を使えるようになります。

https://gist.github.com/sakapon/70c0df78061d3e6a6b28623c12040031

このコードにより、下図のような真理値表が得られます。  
短絡になったケースは小文字始まりで表示されています。

![](https://github.com/sakapon/Samples-2020/blob/master/Images/OperatorsSample/StringBool-Tables.png)

### クリーネの3値論理
情報技術で多く使われているクリーネの3値論理における、真でも偽でもない3つ目の値 (C# では bool? 型の null) とは、「真でも偽でもない、もう一つの異なる固定値」ではなく「本来は真または偽の値を持つが、現在はわかっていない状態」と考えたほうが意味論的には理解しやすいと思います。  
(命題論理の考え方については、以前に[命題論理を実装する (C#)](https://github.com/sakapon/Documents/blob/master/Math/Propositions/Propositional-Logic.md) という記事で書きました。)

そこで、null の代わりに unknown という名前を使うことにします。  
具体例として、`unknown & false` は、左側のオペランドが実際に true とわかっても false とわかっても式全体では false であることが確定しています。また、`unknown & true` は、左側のオペランドが true とわかるか false とわかるかで式全体の結果が変わるため現在は unknown です。すると、`x & y` が現時点で true と確定するのは x と y がともに true のときだけです。  
残りの `!`, `^`, `|` についても同様に考えて体系を構成できます。

bool? 型における論理演算もこれと同じになります。
演算結果の真理値表はなかなか丸暗記できないと思いますが、上記のように短絡評価と関連付けることで導けるようになるはずです。

また、`==` および `!=` 演算子を等価性ではなく命題論理における同値性として扱いたい場合、これらの演算子の戻り値の型を bool ではなく定義元の型とし、`x == y` は `!(x ^ y)` と同じ意味に、`x != y` は `x ^ y` と同じ意味になるようにオーバーロードします。

もし null を unknown ではなく「もう一つの異なる固定値」と考えて「一方が null ならば `x & y` は null」のような体系を立てる場合であっても、各論理演算子をオーバーロードすれば実現できるでしょう。ただし、短絡評価はできません。

### 制御条件式
`true` 演算子が宣言されていることで、if ステートメントなどにおける制御条件式として `if (obj)` のように使えます。  
また、`true` 演算子の代わりに bool 型への暗黙的型変換を宣言することででも、制御条件式として使えるようになります。  
両方を宣言した場合は、この暗黙的型変換が優先されるようです。

```cs
public static implicit operator bool(StringBool v) => v.IsTrue;
```

次回はその他の注意点や設計についてです。

前回: [C# で演算子を実装する (4)](CSharp-Operators-4.md)  
次回: [C# で演算子を実装する (6)](CSharp-Operators-6.md)

### 作成したサンプル
- [OperatorsSample (GitHub)](https://github.com/sakapon/Samples-2020/tree/master/OperatorsSample)

### バージョン情報
- C# 8.0
- .NET Standard 2.1
- .NET Core 3.1

### 参照
- [C# 演算子と式 (C# リファレンス)](https://docs.microsoft.com/dotnet/csharp/language-reference/operators/)
- [演算子のオーバーロード (C# リファレンス)](https://docs.microsoft.com/dotnet/csharp/language-reference/operators/operator-overloading)
- [ブール論理演算子 (C# リファレンス)](https://docs.microsoft.com/dotnet/csharp/language-reference/operators/boolean-logical-operators)
- [true 演算子と false 演算子 (C# リファレンス)](https://docs.microsoft.com/dotnet/csharp/language-reference/operators/true-false-operators)
- [演算子のオーバーロード](https://ufcpp.net/study/csharp/oo_operator.html)
- [.NETのクラスライブラリ設計](https://amzn.to/3kLf0R8) (書籍)
- [3値論理 - Wikipedia](https://t.co/WRXPzaVG1D)
- [命題論理を実装する (C#)](https://github.com/sakapon/Documents/blob/master/Math/Propositions/Propositional-Logic.md)
