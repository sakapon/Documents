# C# で演算子を実装する (4)

[前回の記事](CSharp-Operators-3.md)では、算術演算子のオーバーロードについて説明しました。  
今回は例として、.NET の基本クラスライブラリにある BitArray クラスや BitVector32 構造体のような、整数をビットの配列として扱えるものを実装します。
インデクサー、キャスト演算子、インクリメント演算子 (算術演算子の一部) をオーバーロードしており、その他にもいろいろな観点が詰め込まれています。

まずはソースコードを示します。

https://gist.github.com/sakapon/ef775da2dab677534d60c8b2717713ea

以下、それぞれについて説明していきます。

### キャスト演算子
型変換 (キャスト) には暗黙的 (implicit) な型変換と明示的 (explicit) な型変換が存在し、ユーザー定義型でキャスト演算子を実装するときにいずれかを選べます。
基本的な方針として、情報の消失がない場合は暗黙的な型変換を実装可能である、と考えます。  
ただし、一方が他方のインスタンスをラップしている関係の場合には、ラップする操作を暗黙的な型変換として、ラップを解除する操作を明示的な型変換として定義することが多いです。
今回の例では、BitArray が int をラップしています。

### Parse メソッド
ToString メソッドの逆の操作として、インスタンスの文字列表現からインスタンスを復元するための Parse メソッドを用意することがあります。
Parse メソッドを実装する場合は、少なくとも ToString メソッドで得られた文字列をそのまま Parse メソッドで解析でき、元と同等のインスタンスが得られることが望ましいでしょう。

```cs
[TestMethod]
public void Parse()
{
	BitArray b1 = 65535; // implicit conversion
	Assert.AreEqual("65535", b1.ToString());

	var b2 = BitArray.Parse(b1.ToString());
	Assert.AreEqual(65535, (int)b2); // explicit conversion
	Assert.AreEqual(b1, b2);
}
```

### インデクサー
`this[]` という特殊な形式のプロパティを実装することで、配列のように `[]` でアクセスできるようになります。  
多次元配列と同様に、複数の引数を受け付けることもできます。

この例では setter でビットを書き換えています。  
構造体は、基本的には不変 (immutable) な値として扱うことが多いのですが、値を書き換えることもできます。  
このインデクサーの setter を実装することで、キーと値を指定する[コレクション初期化子](https://docs.microsoft.com/dotnet/csharp/programming-guide/classes-and-structs/object-and-collection-initializers)を利用できるようになります。  
他にも、Add メソッドを定義することで、List<T> のように値を追加するコレクション初期化子を利用できます。

また、C# 8.0 で Index および Range の機能が導入されたため、インデクサーのオーバーロードで [Index 型](https://docs.microsoft.com/dotnet/api/system.index)を引数として使えるようにしています。
これにより、逆順のインデックスでもアクセスできるようになります。  
[Range 型](https://docs.microsoft.com/dotnet/api/system.range)は、例えば数列の部分和を求めるというケースで使えるでしょう。

```cs
[TestMethod]
public void Indexer()
{
	BitArray b = 10;
	Assert.AreEqual(false, b[0]);
	Assert.AreEqual(true, b[1]);
	Assert.AreEqual(false, b[2]);
	Assert.AreEqual(true, b[3]);
	Assert.AreEqual(false, b[4]);
	Assert.AreEqual(false, b[5]);
	Assert.AreEqual(false, b[^27]);
	Assert.AreEqual(10, (int)b);

	b[5] = true;
	Assert.AreEqual(true, b[5]);
	Assert.AreEqual(true, b[^27]);
	Assert.AreEqual(42, (int)b);
}

[TestMethod]
public void Initializer()
{
	var b = new BitArray
	{
		[3] = true,
		[6] = true,
	};
	Assert.AreEqual(72, b.Value);
}
```

### インクリメント演算子
インクリメント演算子 `++` およびデクリメント演算子 `--` は、引数の型も戻り値の型も定義元と同じでなければなりません (派生型はOK)。

次回は論理演算子についてです。

前回: [C# で演算子を実装する (3)](CSharp-Operators-3.md)  
次回: [C# で演算子を実装する (5)](CSharp-Operators-5.md)

### 作成したサンプル
- [OperatorsSample (GitHub)](https://github.com/sakapon/Samples-2020/tree/master/OperatorsSample)

### バージョン情報
- C# 8.0
- .NET Standard 2.1
- .NET Core 3.1

### 参照
- [C# 演算子と式 (C# リファレンス)](https://docs.microsoft.com/dotnet/csharp/language-reference/operators/)
- [演算子のオーバーロード (C# リファレンス)](https://docs.microsoft.com/dotnet/csharp/language-reference/operators/operator-overloading)
- [インデクサー (C# プログラミング ガイド)](https://docs.microsoft.com/dotnet/csharp/programming-guide/indexers/)
- [オブジェクト初期化子とコレクション初期化子 (C# プログラミング ガイド)](https://docs.microsoft.com/dotnet/csharp/programming-guide/classes-and-structs/object-and-collection-initializers)
- [ユーザー定義の変換演算子 (C# リファレンス)](https://docs.microsoft.com/dotnet/csharp/language-reference/operators/user-defined-conversion-operators)
- [算術演算子 (C# リファレンス)](https://docs.microsoft.com/dotnet/csharp/language-reference/operators/arithmetic-operators)
- [BitArray クラス](https://docs.microsoft.com/dotnet/api/system.collections.bitarray)
- [BitVector32 構造体](https://docs.microsoft.com/dotnet/api/system.collections.specialized.bitvector32)
- [DebuggerDisplayAttribute クラス](https://docs.microsoft.com/dotnet/api/system.diagnostics.debuggerdisplayattribute)
- [演算子のオーバーロード](https://ufcpp.net/study/csharp/oo_operator.html)
- [.NETのクラスライブラリ設計](https://amzn.to/3kLf0R8) (書籍)
