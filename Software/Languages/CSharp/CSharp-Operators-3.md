# C# で演算子を実装する (3)

[前回の記事](CSharp-Operators-2.md)では、等値演算子と比較演算子のオーバーロードについて説明しました。  
今回は算術演算子を扱います。
算術演算子には、単項演算子 (`+`, `-`, `++`, `--`) と二項演算子 (`+`, `-`, `*`, `/`, `%`) があります。

### 実装例 (構造体)
例として、2次元ベクトルを表す構造体を実装してみます。  
等値演算子に加え、単項演算子 (`+`, `-`) と二項演算子 (`+`, `-`, `*`, `/`) をオーバーロードします。

https://gist.github.com/sakapon/57ab029a06b76b9f0d19977a457e3e88

これで、ベクトルの基本的な四則演算が定義され、数式に近い記述ができるようになりました。  
またこの例では、二次的な計算結果である Norm, Angle プロパティと、静的メソッド Area を追加しています。  
インスタンスに付随する概念や演算子で表現することが難しい概念に対しては、このようにプロパティや静的メソッドで実装します。

単項演算子または二項演算子をオーバーロードするには、引数のいずれかに定義元の型 (ここでは Vector) が含まれていなければなりません。
戻り値については、void 以外の型を自由に設定できます。

この例では、ベクトルのドット積 (内積) を `*` 演算子で定義しています。  
しかし、戻り値が Vector である他の `*` 演算子とは意味が変わってしまうため、演算子ではなく静的メソッド (名前は Dot など) で定義することが多いです。  
(しかもベクトルには内積も外積もある、という事情もあります。)

また、数値を表す型で `^` 演算子を累乗として利用することも、同様の理由で推奨されていません。  
C# の組込み型では `^` 演算子が累乗の意味で使われていないため、累乗は静的メソッド (名前は Pow または Power) として定義することが多いです。

演算子の戻り値の型を自由に設定することは原理上は可能であり、個人の範囲で利用する分にはかまわないと思うのですが、他の開発者に公開するライブラリでは利用を避けます。  
演算子として利用できる記号の種類は決まっており、しかも組込み型で既に意味を与えられているものばかりです。
コンパイラは引数の型の組合せから呼び出すべきメソッドを解決している、と考えれば、演算子の「オーバーロード」と呼ぶことに納得できるでしょう。

### クラスとして実装する場合
構造体 (値型) の場合との大きな違いとして、クラス (参照型) の場合は引数で渡されるオブジェクトが null 値である可能性を考慮しなければなりません。
このため、数式のように演算子を多くオーバーロードする型を実装するときは構造体にしたほうが簡潔なのですが、例えば継承を利用するためにクラスとして実装したい、というケースもあります。  
実装やテストを少し簡単にするために、null 値を `new Vector()` と同一視する方針も考えられます。

### デバッグ時の表示
Visual Studio において、デバッグ時の `[ローカル]` ウィンドウや `[ウォッチ]` ウィンドウで値を表示するとき、既定では ToString メソッドを呼び出した結果が利用されます。
したがって、ToString メソッドをオーバーライドしていない場合は型名が表示されますが、オーバーライドしておくことで、下図のようにそのオブジェクトを表現する文字列を表示させることができます。

![](https://github.com/sakapon/Samples-2020/blob/master/Images/OperatorsSample/VS-Debug-ToString.png)

このデバッグ時の表示を、ToString メソッドの結果以外のものに設定する方法については次回で紹介します。

次回はキャスト演算子、インデクサーなどについてです。

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
