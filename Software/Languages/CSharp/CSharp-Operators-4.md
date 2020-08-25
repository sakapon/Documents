# C# で演算子を実装する (4)

[前回の記事](CSharp-Operators-3.md)では、算術演算子のオーバーロードについて説明しました。  
今回は例として、.NET の基本クラスライブラリにある BitArray クラスや BitVector32 構造体のような、整数をビットの配列として扱えるものを実装します。
キャスト演算子、インデクサー、インクリメント演算子 (算術演算子の一部) をオーバーロードしており、その他にもいろいろな観点が詰め込まれています。

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

https://gist.github.com/sakapon/5a62d055ec735cde2575f2b1f91c91ce

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

https://gist.github.com/sakapon/26dcf40b8daa77a891a6c8db2275100d

### インクリメント演算子
インクリメント演算子 `++` およびデクリメント演算子 `--` は、引数の型も戻り値の型も定義元と同じでなければなりません (派生型はOK)。

### bit 全探索
以上のように実装すると、いわゆる bit 全探索のアルゴリズムが次のようなコードでできます。

https://gist.github.com/sakapon/06d170b712990531022ed577ba051a37

(図)

なお、.NET の基本クラスライブラリの [BitArray クラス](https://docs.microsoft.com/dotnet/api/system.collections.bitarray)では `new BitArray(new[] { x })` のようにコンストラクターで元の整数を配列で渡し、[BitVector32 構造体](https://docs.microsoft.com/dotnet/api/system.collections.specialized.bitvector32)ではインデクサーで `b[i]` ではなく `b[1 << i]` のようにマスクを表す整数を指定する、という違いがあります。

### デバッグ時の表示
Visual Studio において、デバッグ時の `[ローカル]` ウィンドウや `[ウォッチ]` ウィンドウで値を表示するとき、既定では ToString メソッドを呼び出した結果が利用されます。
これを ToString と異なるものにしたい場合、型に [[DebuggerDisplay] 属性](https://docs.microsoft.com/dotnet/api/system.diagnostics.debuggerdisplayattribute)を追加します。  
コンストラクターに指定する文字列では、`{}` の中にコードを記述することができます。  
上の実装例では、整数を16進数形式で表示させています。

(図)

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
