# C# で演算子を実装する (6)

[前回の記事](CSharp-Operators-5.md)では、論理演算子のオーバーロードについて説明しました。  
今回はその他の設計や注意点を書いていきます。

前回: [C# で演算子を実装する (5)](CSharp-Operators-5.md)

### 作成したサンプル
- [OperatorsSample (GitHub)](https://github.com/sakapon/Samples-2020/tree/master/OperatorsSample)

### バージョン情報
- C# 8.0
- .NET Standard 2.1
- .NET Core 3.1

### 参照
- [C# 演算子と式 (C# リファレンス)](https://docs.microsoft.com/dotnet/csharp/language-reference/operators/)
- [演算子のオーバーロード (C# リファレンス)](https://docs.microsoft.com/dotnet/csharp/language-reference/operators/operator-overloading)
- [構造体型 (C# リファレンス)](https://docs.microsoft.com/dotnet/csharp/language-reference/builtin-types/struct)
- [KeyValuePair<TKey,TValue> 構造体](https://docs.microsoft.com/dotnet/api/system.collections.generic.keyvaluepair-2)
- [演算子のオーバーロード](https://ufcpp.net/study/csharp/oo_operator.html)
- [.NETのクラスライブラリ設計](https://amzn.to/3kLf0R8) (書籍)
