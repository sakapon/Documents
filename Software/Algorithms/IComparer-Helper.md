# IComparer\<T\> の補助クラス
.NET で配列やコレクションをソートするときに、ソートの条件を指定する方法がいくつかあります。  
それらを調べ、ソート条件の指定を補助するためのクラスを作成しました。

まず、配列やコレクションをソートするために呼び出す主なメソッドを挙げてみます。
- [Array.Sort メソッド](https://docs.microsoft.com/dotnet/api/system.array.sort)
- [List\<T\>.Sort メソッド](https://docs.microsoft.com/dotnet/api/system.collections.generic.list-1.sort)
- [SortedSet\<T\> コンストラクター](https://docs.microsoft.com/dotnet/api/system.collections.generic.sortedset-1.-ctor)
- [Enumerable.OrderBy メソッド](https://docs.microsoft.com/dotnet/api/system.linq.enumerable.orderby) など

そして、これらのメソッドのオーバーロードでソートの条件を指定する方法をまとめると、次のようになります。

- [IComparable\<T\>](https://docs.microsoft.com/dotnet/api/system.icomparable-1)
  - 対象となる型 T が IComparable\<T\> インターフェイスを実装している場合、引数を指定しなくてもソート可能
  - ただし、これだけでは降順にできない
- [Comparison\<T\>](https://docs.microsoft.com/dotnet/api/system.comparison-1) または Func\<T, T, int\>
  - 引数 (x, y) に対し、次の値を返す比較関数
    - x が小さいならば、負の値
    - 等しいならば、0
    - x が大きいならば、正の値
- [IComparer\<T\>](https://docs.microsoft.com/dotnet/api/system.collections.generic.icomparer-1)
  - Comparison\<T\> をインターフェイスにしたもの
  - [Comparer\<T\>.Create メソッド](https://docs.microsoft.com/dotnet/api/system.collections.generic.comparer-1.create)により、Comparison\<T\> から生成できる
- 各要素に対応するキーの配列
  - 主に配列をソートするとき
  - 各キーが IComparable\<TKey\> インターフェイスを実装していることが必要
- Func\<T, TKey\> によるキーの指定、および昇順・降順の指定
  - 主に LINQ の Enumerable.OrderBy メソッドなど
  - 第2キー以降を指定するには Enumerable.ThenBy メソッドなど
  - 各キーが IComparable\<TKey\> インターフェイスを実装していることが必要

普段これらを使っていると、次のような実感があります。
- 対象となる型 T が IComparable\<T\> インターフェイスを実装しており、その昇順でソートしたい場合は、引数のないオーバーロードを呼び出すだけであり簡単
- それ以外の少し複雑な条件の場合、LINQ のように、Func\<T, TKey\> によりキーを指定したいことが多い
  - しかし、このオーバーロードは用意されていないことが多い
- IComparer\<T\> オブジェクトを引数に指定するオーバーロードであれば、だいたいどのソート関数でも備えている

そこで、「Func\<T, TKey\> によるキーの指定、および昇順・降順の指定」から IComparer\<T\> オブジェクトを生成するためのヘルパー クラスを作成しました。
言語は C# です。