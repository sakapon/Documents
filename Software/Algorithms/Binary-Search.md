# 二分探索のライブラリ化
// [Competitive Programming (2) Advent Calendar 2019](https://adventar.org/calendars/4587) の 20 日目の記事です。

競技プログラミングでたびたび利用する二分探索について考察してみます。  
といっても、とくに競技プログラミングに限らず活用できると思います。

まず二分探索の基本的な例として、昇順にソートされた整数のリスト `a` に対して「値 `v` をリストに挿入するときのインデックス」を求めてみます。
これは「値 `v` より大きい値が最初に現れるインデックス」と考え、最後の要素が `v` 以下のときはリストの長さ `a.Count` を返すことにすれば、次の IndexForInsert メソッドとして実装できます。

https://gist.github.com/sakapon/056daac8eee07fd36d52c3bef0e7f5a1

同じ考え方を利用して、[Array.BinarySearch メソッド](https://docs.microsoft.com/ja-jp/dotnet/api/system.array.binarysearch)のように「値が一致するインデックスを求める。一致しない場合は、それより大きな値のインデックスの補数」という仕様にするには、上の IndexOf メソッドのように書けます。

注意点:
- `IList<T>` インターフェイスは、 `T[]` や `List<T>` クラスを含みます。
- リスト内の値は重複してもかまいません。
- 例外処理は除いています。
- `(l + r - 1) / 2` とするとオーバーフローすることがあるため、 `l + (r - l - 1) / 2` としています。

上記のメソッドは、リストのみ、しかも昇順にソート済みという制約の下で適用することができます。  
そこで、さらに汎用的に使えるように、抽象度を上げてライブラリを作ることを考えます。

二分探索により実現できることは、見方を変えると「状態が変化する境界を見つけること」、厳密には「与えられた条件に対し、ある境界が存在して、その前方と後方で真偽が逆転する場合にその境界を見つけること」だと考えます。
この考え方で先ほどのコードから抽象化した部分を抜き出すと、次の First メソッドのようになります。

https://gist.github.com/sakapon/ba3c310c476b65619b4342ee9117f638

この形にすれば、降順のリストにも、リスト以外の数値の範囲にも適用できます。  
例として、[ABC 146 C - Buy an Integer](https://atcoder.jp/contests/abc146/tasks/abc146_c) を解いてみます。  
先ほどの First メソッドに対して Last メソッドを用意し (コメントは省略)、価格が所持金以下となる最後の整数を求めればよいです。

https://gist.github.com/sakapon/de28afc51ca773275f3ed3f1137884c8

だいぶ簡潔に書けました。  
次に小数向けのメソッドを用意して、二分法で平方根を求めてみます。  
誤差を表す桁数を指定できるようにしています。

https://gist.github.com/sakapon/bb272a7bc19cdcaee42dc47b45e0bdea

### まとめ
というわけで、今後は二分探索の問題を意味論的に「与えられた条件を満たす最初または最後の値を求める」と考えれば、今回作成したライブラリを使って実装ができます。
- [作成したライブラリとサンプル コード (GitHub)](https://github.com/sakapon/Samples-2019/tree/master/AlgorithmSample)

### その他
- これまでは二分探索を実装するときに再帰を使っていたのですが、おそらく過去に読んだ解説に再帰を使うものが多かったためだと思います。競技プログラミングを始めてから自然に `while (l < r)` を使うようになりました。
- [Array.BinarySearch メソッド](https://docs.microsoft.com/ja-jp/dotnet/api/system.array.binarysearch)や [List<T>.BinarySearch メソッド](https://docs.microsoft.com/ja-jp/dotnet/api/system.collections.generic.list-1.binarysearch)では、重複する値を検索する場合、その最初のインデックスが返ってくるとは限りません。

前回: [競技プログラミングでも C# で簡潔に書きたい](../Languages/CSharp/Competitive-Short-Code.md)

### 参照
- [二分探索 - Wikipedia](https://bit.ly/2Ssg0dx)
- [本当に不足しているIT人材を知るために、本当に必要なもの | ウェブ電通報](https://dentsu-ho.com/articles/6878)
