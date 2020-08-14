# C# で演算子を実装する (1)

ユーザー定義型 (自作の型) においても、組込み型と同様に `==` や `+` などの演算子を定義することができます。
とくに数式を表現する場合などにおいて、簡潔に記述できて便利です。

しかしその前に、何も演算子を実装しなかったらどのような動作をするのか確認しておきましょう。
こちらはプロパティを定義しただけのクラス (参照型) です。

https://gist.github.com/sakapon/a592069d354ce6229c3bb259044c3e56

等値比較演算の方法が最初からいくつか用意されており、実行結果はすべて、参照が同じであれば true、それ以外は false です。

次に、こちらはプロパティを定義しただけの構造体 (値型) です。

https://gist.github.com/sakapon/b104bcf4af76f5131fb2cfdb25cc4da0


構造体の場合はなんと、Equals メソッドでプロパティごと (正確にはフィールドごと) の等値比較が機能します。
これは、構造体は暗黙的に ValueType クラスを継承することになり、[ValueType.Equals メソッド](https://docs.microsoft.com/dotnet/api/system.valuetype.equals)が呼び出され、すべてのフィールドの値が等しいかどうか判定されるためです。
これにより、Array.IndexOf や LINQ の Distinct などでもプロパティごとに比較され、Dictionary のキーとしても使うことができます。
ただし、その内部ではリフレクションが利用されているため、パフォーマンスを最適化するには等値比較演算を実装したほうがよいとされています ([.NETのクラスライブラリ設計](https://amzn.to/3kLf0R8)より)。

ついでに、匿名型 (参照型)、Tuple (参照型) および ValueTuple (値型) の動作にも触れておきます。
これらはいずれも Equals メソッドで各要素を比較できます。
C# 7.3 以降の ValueTuple では言語の機能として `==` および `!=` 演算子が使え、コンパイラにより各要素の等値比較に展開されます。

(図)

さらに、Tuple および ValueTuple はともに IComparable を実装しており、そのまま Array.Sort や LINQ のソートにおいてキーとして利用できます。第1要素、第2要素、・・・の順に優先されます。
なお、IComparable を実装しているものの、`<` などの比較演算子は定義されていません。

https://gist.github.com/sakapon/951a8f4b29347dbcdee1f77767ce1ff9

(図)

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
- [匿名型 (C# プログラミング ガイド)](https://docs.microsoft.com/dotnet/csharp/programming-guide/classes-and-structs/anonymous-types)
- [.NETのクラスライブラリ設計](https://amzn.to/3kLf0R8) (書籍)
