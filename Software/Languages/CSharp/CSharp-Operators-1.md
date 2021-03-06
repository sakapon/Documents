# C# で演算子を実装する (1)

C# では、ユーザー定義型 (自作の型) においても、組込み型と同様に `==` や `+` などの演算子を定義することができます。  
例えば数式を表現する型を使う場合などにおいて、簡潔に記述できて便利です。

ユーザー定義のクラスまたは構造体に演算子を実装するときの注意点について、全6回にわたって書いていきます。  
ただし、すべての演算子を扱うわけではなく、個別の文法について細かくは説明しません。
- (1) 素のクラスおよび構造体、タプル型など (本記事)
- [(2) 等値演算子、比較演算子](CSharp-Operators-2.md)
- [(3) 算術演算子](CSharp-Operators-3.md)
- [(4) キャスト演算子、インデクサー](CSharp-Operators-4.md)
- [(5) 論理演算子](CSharp-Operators-5.md)
- [(6) 補足](CSharp-Operators-6.md)

### 素のクラスおよび構造体
演算子を実装する前に、まず何も演算子を実装しなかったらどのような動作をするのか確認しておきましょう。  
こちらはプロパティを定義しただけのクラス (参照型) です。
このコードでオブジェクトの等価性を確認します。

https://gist.github.com/sakapon/a592069d354ce6229c3bb259044c3e56

等値演算の方法がはじめからいくつか存在しており、実行結果はすべて、参照が同じであれば true、それ以外は false です。

次に、こちらはプロパティを定義しただけの構造体 (値型) です。

https://gist.github.com/sakapon/b104bcf4af76f5131fb2cfdb25cc4da0

構造体の場合はなんと、Equals メソッドでプロパティごと (正確にはフィールドごと) の等価性評価が機能します。  
これは、構造体は暗黙的に ValueType クラスを継承していることにより [ValueType.Equals メソッド](https://docs.microsoft.com/dotnet/api/system.valuetype.equals)が呼び出され、すべてのフィールドの値が等しいかどうか判定されるためです。

この仕組みにより、何もしなくても構造体のインスタンスを Array.IndexOf や LINQ の Distinct、さらに Dictionary のキーとしても使うことができます。
ただし、その内部ではリフレクションが利用されているため、パフォーマンスを最適化するには等値演算をカスタム実装したほうがよいでしょう ([.NETのクラスライブラリ設計](https://amzn.to/3kLf0R8)より)。
それほどパフォーマンスを気にせず、楽な実装で等値演算を実現したいのであれば、上記のようなコードだけで済ませることもできます。

### タプル型など
ついでに、[匿名型](https://docs.microsoft.com/dotnet/csharp/programming-guide/classes-and-structs/anonymous-types) (参照型)、[Tuple](https://docs.microsoft.com/dotnet/api/system.tuple) (参照型) および [ValueTuple](https://docs.microsoft.com/dotnet/csharp/language-reference/builtin-types/value-tuples) (値型) の動作にも触れておきます。

これらはいずれも Equals メソッドで各要素の等価性が評価されます。  
C# 7.3 以降の ValueTuple では言語の機能として `==` および `!=` 演算子が使え、コンパイラにより各要素の等値演算に展開されます。  
なお、内部でフィールド名は無視され、フィールドの定義順により Item1, Item2, ・・・となります (下図は ILSpy)。

https://gist.github.com/sakapon/c00fe5960bba2d60d11ddd3c4b1dc74d

![](https://github.com/sakapon/Samples-2020/blob/master/Images/OperatorsSample/ValueTuple-ILSpy.png)

さらに、Tuple および ValueTuple はともに IComparable インターフェイスを実装しており、そのまま Array.Sort や LINQ のソートにおいてキーとして利用できます。評価は Item1, Item2, ・・・の順に優先されます。  
なお、IComparable インターフェイスを実装しているものの、比較演算子 (`<`, `>`, `<=`, `>=`) は定義されていません。

https://gist.github.com/sakapon/951a8f4b29347dbcdee1f77767ce1ff9

![](https://github.com/sakapon/Samples-2020/blob/master/Images/OperatorsSample/ValueTuple-Sort.png)

したがって、ユーザー定義型を作成しなくても ValueTuple で済んでしまうケースもあるでしょう。

さて、次回はユーザー定義型に等値演算子および比較演算子を実装します。

次回: [C# で演算子を実装する (2)](CSharp-Operators-2.md)

### 作成したサンプル
- [OperatorsSample (GitHub)](https://github.com/sakapon/Samples-2020/tree/master/OperatorsSample)

### バージョン情報
- C# 8.0
- .NET Standard 2.1
- .NET Core 3.1

### 参照
- [C# 演算子と式 (C# リファレンス)](https://docs.microsoft.com/dotnet/csharp/language-reference/operators/)
- [タプル型 (C# リファレンス)](https://docs.microsoft.com/dotnet/csharp/language-reference/builtin-types/value-tuples)
- [Tuple クラス](https://docs.microsoft.com/dotnet/api/system.tuple)
- [匿名型 (C# プログラミング ガイド)](https://docs.microsoft.com/dotnet/csharp/programming-guide/classes-and-structs/anonymous-types)
- [.NETのクラスライブラリ設計](https://amzn.to/3kLf0R8) (書籍)
