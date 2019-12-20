# 二分探索のライブラリ化
// [Competitive Programming (2) Advent Calendar 2019](https://adventar.org/calendars/4587) の 20 日目の記事です。

競技プログラミングでたびたび利用する二分探索について考察してみます。  
といっても、とくに競技プログラミングに限らないのですが。

まず二分探索の基本的な例として、昇順にソートされた整数のリストから、指定された値より大きい値が最初に現れるインデックスを求めてみます。
存在しない場合はリストの長さ (範囲外のインデックス) を返すことにすると、「その値をリストに挿入するときのインデックス」と考えても同じです。
これは次の IndexForInsert メソッドとして実装できます。

https://gist.github.com/sakapon/056daac8eee07fd36d52c3bef0e7f5a1

同じ考え方を利用して、[Array.BinarySearch メソッド](https://docs.microsoft.com/ja-jp/dotnet/api/system.array.binarysearch)のように「値が一致するインデックスを求める。一致しない場合は、それより大きな値のインデックスの補数」という仕様にするには、上の IndexOf メソッドのように書けます。

注意点:
- `IList<T>` インターフェイスは、 `T[]` や `List<T>` クラスを含みます。
- リスト内の値は重複してもかまいません。
- 例外処理は除いています。
- `(l + r - 1) / 2` とするとオーバーフローすることがあるため、 `l + (r - l - 1) / 2` としています。

上記のメソッドは、リストのみ、しかも昇順にソート済みという制約の下で適用することができます。
そこで、さらに汎用的に使えるように、抽象度を上げてライブラリを作ることを考えます。

### その他
- これまでは二分探索を実装するときに再帰を使っていたのですが、おそらく過去に読んだ解説に再帰を使うものが多かったためだと思います。競技プログラミングを始めてから自然に `while (l < r)` を使うようになりました。
- [Array.BinarySearch メソッド](https://docs.microsoft.com/ja-jp/dotnet/api/system.array.binarysearch)や [List<T>.BinarySearch メソッド](https://docs.microsoft.com/ja-jp/dotnet/api/system.collections.generic.list-1.binarysearch)では、重複する値を検索する場合、その最初のインデックスが返ってくるとは限りません。

### 参照
- [二分探索 - Wikipedia](https://bit.ly/2Ssg0dx)
- [本当に不足しているIT人材を知るために、本当に必要なもの | ウェブ電通報](https://dentsu-ho.com/articles/6878)
