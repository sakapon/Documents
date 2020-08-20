# C# で演算子を実装する (3)

前回は、等値演算子と比較演算子のオーバーロードについて説明しました。  
今回は、算術演算子を扱います。
算術演算子には、単項演算子 (`+`, `-`, `++`, `--`) と二項演算子 (`+`, `-`, `*`, `/`, `%`) があります。

### 実装例 (構造体)
例として、2次元ベクトルを表す構造体を実装してみます。  
等値演算子に加え、単項演算子 (`+`, `-`) と二項演算子 (`+`, `-`, `*`, `/`) をオーバーロードします。

https://gist.github.com/sakapon/57ab029a06b76b9f0d19977a457e3e88

これで、ベクトルの基本的な四則演算が定義され、数式に近い記述ができるようになりました。  
また、この例では、二次的な計算結果である Norm, Angle プロパティと、静的メソッド Area を追加しています。
インスタンスに付随する概念や演算子で表現することが難しい概念に対しては、このようにプロパティや静的メソッドで実装します。

次回はインデクサー、キャスト演算子などのオーバーロードについてです。

前回: [C# で演算子を実装する (2)](CSharp-Operators-2.md)  
次回: [C# で演算子を実装する (4)](CSharp-Operators-4.md)

### 作成したサンプル
- [OperatorsSample (GitHub)](https://github.com/sakapon/Samples-2020/tree/master/OperatorsSample)

### バージョン情報
- C# 8.0
- .NET Standard 2.1
- .NET Core 3.1

### 参照
- [C# 演算子と式 (C# リファレンス)](https://docs.microsoft.com/dotnet/csharp/language-reference/operators/)
- [演算子のオーバーロード (C# リファレンス)](https://docs.microsoft.com/dotnet/csharp/language-reference/operators/operator-overloading)
- [算術演算子 (C# リファレンス)](https://docs.microsoft.com/dotnet/csharp/language-reference/operators/arithmetic-operators)
- [構造体型 (C# リファレンス)](https://docs.microsoft.com/dotnet/csharp/language-reference/builtin-types/struct)
- [BigInteger 構造体](https://docs.microsoft.com/dotnet/api/system.numerics.biginteger)
- [Complex 構造体](https://docs.microsoft.com/dotnet/api/system.numerics.complex)
- [演算子のオーバーロード](https://ufcpp.net/study/csharp/oo_operator.html)
- [.NETのクラスライブラリ設計](https://amzn.to/3kLf0R8) (書籍)
