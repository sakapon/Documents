# C# で演算子を実装する (2)

ユーザー定義型で演算子を実装することを「演算子をオーバーロードする」と呼びます。  
オーバーロードできる演算子の一覧は、[演算子のオーバーロード (C# リファレンス)](https://docs.microsoft.com/dotnet/csharp/language-reference/operators/operator-overloading) に載っています。

今回は、実装の頻度が高く、かつ最もややこしい等値演算子 (`==`, `!=`) と比較演算子 (`<`, `>`, `<=`, `>=`) を扱います。

### インターフェイスの実装について
等値演算子をオーバーロードするときは、[IEquatable\<T\> インターフェイス](https://docs.microsoft.com/dotnet/api/system.iequatable-1)を実装します。  
また、比較演算子をオーバーロードするときは、[IComparable\<T\> インターフェイス](https://docs.microsoft.com/dotnet/api/system.icomparable-1)を実装します。

原理上はこれらのインターフェイスを実装しなくても演算子のオーバーロードはできるのですが、これらのジェネリックなインターフェイスを実装することで、各種のライブラリ (Array.IndexOf\<T\>, Array.Sort\<T\> など) からは型変換を伴わずに Equals(T) メソッドおよび CompareTo(T) メソッドが呼び出されます。
構造体であれば値のボックス化を避けることができます。  
これは、各種のライブラリが [EqualityComparer\<T\>.Default](https://docs.microsoft.com/dotnet/api/system.collections.generic.equalitycomparer-1.default) および [Comparer\<T\>.Default](https://docs.microsoft.com/dotnet/api/system.collections.generic.comparer-1.default
) を通じて各インターフェイスにアクセスする仕組みによるものです。
したがって、逆に型引数 T に対して等値演算・比較演算を呼び出すライブラリを作る立場のときは、EqualityComparer\<T\>.Default および Comparer\<T\>.Default を使いましょう。

また、各種ライブラリからの等値演算・比較演算さえできればよいというケース (ソートに使うだけ、など) では、演算子をオーバーロードせずにインターフェイスを実装するだけ、という選択肢もあります。

前回: [C# で演算子を実装する (1)](CSharp-Operators-1.md)  
次回: [C# で演算子を実装する (3)](CSharp-Operators-3.md)

### 作成したサンプル
- [OperatorsSample (GitHub)](https://github.com/sakapon/Samples-2020/tree/master/OperatorsSample)

### バージョン情報
- C# 8.0
- .NET Standard 2.1
- .NET Core 3.1

### 参照
- [C# 演算子と式 (C# リファレンス)](https://docs.microsoft.com/dotnet/csharp/language-reference/operators/)
- [演算子のオーバーロード (C# リファレンス)](https://docs.microsoft.com/dotnet/csharp/language-reference/operators/operator-overloading)
- [等値演算子 (C# プログラミング ガイド)](https://docs.microsoft.com/dotnet/csharp/language-reference/operators/equality-operators)
- [比較演算子 (C# プログラミング ガイド)](https://docs.microsoft.com/dotnet/csharp/language-reference/operators/comparison-operators)
- [.NETのクラスライブラリ設計](https://amzn.to/3kLf0R8) (書籍)
- [演算子のオーバーロード](https://ufcpp.net/study/csharp/oo_operator.html)
- [C# 7のタプルが一般的なガイドラインに沿わずに書き換え可能な構造体である背景](https://www.buildinsider.net/column/iwanaga-nobuyuki/016)
