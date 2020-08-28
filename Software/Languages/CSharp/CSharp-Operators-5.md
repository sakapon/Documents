# C# で演算子を実装する (5)

[前回の記事](CSharp-Operators-4.md)では、キャスト演算子、インデクサーなどについて説明しました。  
今回は論理演算子を扱います。

論理演算子には、`!`, `&`, `^`, `|` さらに `true` および `false` 演算子があります。  
整数型においては、`!`, `&`, `^` はビット演算を表します。  
`true` 演算子が宣言されていることで、if ステートメントなどにおける制御条件式として `if (obj)` のように使えます。

論理演算については、bool 型や bool? 型の状態で扱えば実務上は事足りることが多く、ユーザー定義型でこれらの演算子をオーバーロードして使う機会はあまりないと思います。
そこで、今回は `&&` および `||` 演算子による短絡評価 (ショート サーキット) を検証してみました。

次回はその他についてです。

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
- [命題論理を実装する (C#)](https://sakapon.wordpress.com/2016/12/16/propositional-logic/)
