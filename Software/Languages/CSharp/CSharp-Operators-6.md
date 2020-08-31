# C# で演算子を実装する (6)

[前回の記事](CSharp-Operators-5.md)では、論理演算子のオーバーロードについて説明しました。  
今回はその他の注意点や設計について書いていきます。

### タプル型との相互変換
C# 7.0 以降の機能で、ユーザー定義型でも Deconstruct メソッドを追加することにより、タプル型と同様に分解を利用できます (後付けの拡張メソッドでも可)。
次のコードは、タプル型とのキャスト演算子と Deconstruct メソッドにより、ユーザー定義の構造体をタプル型に近い形で扱うことを目指した実装例です。

https://gist.github.com/sakapon/ac87a691cdb62b1debfdfe551de321f7

### KeyValuePair
[(1) のタプル型など](CSharp-Operators-1.md)の補足となりますが、タプル型 (ValueTuple) が登場する以前は、2つの値を値型で保持するために [KeyValuePair<TKey,TValue> 構造体](https://docs.microsoft.com/dotnet/api/system.collections.generic.keyvaluepair-2)を使うことがありました。
この KeyValuePair<TKey,TValue> では、等値演算子のオーバーロードも Equals メソッドのオーバーライドもないため、いちおう Equals メソッドで等価性評価はできるもののパフォーマンスは最適化されていません。

なお、最近のバージョンの基本クラスライブラリでは KeyValuePair<TKey,TValue> に [Deconstruct メソッド](https://docs.microsoft.com/dotnet/api/system.collections.generic.keyvaluepair-2.deconstruct)が追加されているため、例えば Dictionary<TKey,TValue> を使うときに次のように分解を利用できます。

```cs
[TestMethod]
public void KeyValuePair_Deconstruct()
{
	var d = Enumerable.Range(1, 100).ToDictionary(i => i, i => i / 2.0);
	foreach (var (i, value) in d)
		Console.WriteLine($"{i} {value}");
}
```

### 構造体の既定のコンストラクター
構造体では引数付きのコンストラクターを追加しても、引数のない暗黙的な既定のコンストラクターの存在が残ります。  
構造体の既定のコンストラクターは、既定値 `default(T)` を得るために使われます (クラスにおいては null に相当する)。  
開発者が明示的に既定のコンストラクターを宣言することはできません。

構造体では、この暗黙的な既定のコンストラクターが呼び出されるときはすべてのフィールドが型の既定値で初期化されます。  
したがって、明示的なコンストラクターと暗黙的なコンストラクターの間で齟齬が発生しないように注意しなければなりません。  
例えば、明示的なコンストラクターの中で、引数で渡された値を単純にフィールドに設定することの他に何らかの処理がある場合や、0 や null などの既定値を不正 (invalid) な値として扱っている場合などが該当します。  
次のコードは Norm とAngle を初期化時に計算して設定している2次元ベクトルの例です。

https://gist.github.com/sakapon/588f5d94c7bee663746a91dc63e3b171

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
- [タプルとその他の型の分解](https://docs.microsoft.com/dotnet/csharp/deconstruct)
- [KeyValuePair<TKey,TValue> 構造体](https://docs.microsoft.com/dotnet/api/system.collections.generic.keyvaluepair-2)
- [演算子のオーバーロード](https://ufcpp.net/study/csharp/oo_operator.html)
- [.NETのクラスライブラリ設計](https://amzn.to/3kLf0R8) (書籍)
