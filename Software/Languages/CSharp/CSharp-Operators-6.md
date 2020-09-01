# C# で演算子を実装する (6)

[前回の記事](CSharp-Operators-5.md)では、論理演算子のオーバーロードについて説明しました。  
今回は補足として、その他の注意点や設計について書いていきます。

### タプル型との相互変換
C# 7.0 以降の機能で、ユーザー定義型でも Deconstruct メソッドを追加することにより、タプル型と同様に分解を利用できます (後付けの拡張メソッドでも可)。
次のコードは、タプル型とのキャスト演算子と Deconstruct メソッドにより、ユーザー定義の構造体をタプル型に近い形で扱うことを目指した実装例です。

https://gist.github.com/sakapon/ac87a691cdb62b1debfdfe551de321f7

### KeyValuePair
[(1) のタプル型など](CSharp-Operators-1.md) の補足となりますが、タプル型 (ValueTuple) が登場する以前は、2つの値を値型で保持するために [KeyValuePair<TKey,TValue> 構造体](https://docs.microsoft.com/dotnet/api/system.collections.generic.keyvaluepair-2)を使うことがありました。
この KeyValuePair<TKey,TValue> には等値演算子のオーバーロードも Equals メソッドのオーバーライドもないため、いちおう Equals メソッドで Key および Value に対する等価比較はできるもののパフォーマンスは最適化されていません。

なお、最近のバージョンの基本クラスライブラリでは KeyValuePair<TKey,TValue> に [Deconstruct メソッド](https://docs.microsoft.com/dotnet/api/system.collections.generic.keyvaluepair-2.deconstruct)が追加されているため、例えば Dictionary<TKey,TValue> の要素を列挙するときに次のように分解を利用できます。

https://gist.github.com/sakapon/8752a4c7d5c07e6c88d317197f36139e

### ValueType のような抽象クラスを作る
[(1) の記事](CSharp-Operators-1.md)で、構造体は暗黙的に ValueType クラスを継承するため最初から Equals メソッドでフィールドごとの等価性評価ができると書きました。  
これのクラス版で、パフォーマンスは気にしないけど簡単な実装で等値演算を備えたクラスを実装したいという場合、ValueType と同様にリフレクションでフィールドごとの等価性評価をする抽象クラスを次のコードで作ることができます。  
なお、ValueType クラスを直接継承することはできません。

https://gist.github.com/sakapon/dca0caed09eb950c714c96fffa9c7aec

### 構造体の既定のコンストラクター
構造体では引数付きのコンストラクターを追加しても、引数のない暗黙的な既定のコンストラクターの存在が残ります。  
構造体の既定のコンストラクターは、既定値 `default(T)` を得るために使われます (クラスにおいては null に相当)。  
開発者が明示的に既定のコンストラクターを宣言することはできません。

構造体では、この暗黙的な既定のコンストラクターが呼び出されるときはすべてのフィールドが型の既定値で初期化されます。  
したがって、明示的なコンストラクターと暗黙的なコンストラクターの間で齟齬が発生しないように注意しなければなりません。  
例えば、明示的なコンストラクターの中で、引数で渡された値を単純にフィールドに設定することの他に何らかの処理がある場合や、0 や null などの既定値を不正 (invalid) な値として扱っている場合などが該当します。  
次のコードは Norm とAngle を初期化時に計算して設定している2次元ベクトルの例です。

https://gist.github.com/sakapon/588f5d94c7bee663746a91dc63e3b171

### 演算子の優先順位
演算子が実行される優先順位は [C# 演算子と式 (C# リファレンス)](https://docs.microsoft.com/dotnet/csharp/language-reference/operators/) に載っています。  
優先順位の低い演算を先に実行するには、丸括弧 `()` で囲みます。
この表はなかなかすべて覚えられるものではないため、優先順位が高くてもあまり馴染みのない演算の組合せの場合には丸括弧で囲むことがあります。  
Visual Studio などのエディターに省略を推奨されるのであれば丸括弧を省略してよいでしょう (下図で丸括弧が灰色になっています)。とくに省略を推奨されない (どちらでもよい) 組合せもあります。

(図)

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
