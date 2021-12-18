# IComparer\<T\> の補助クラス
.NET で配列やコレクションをソートするときに、ソートの条件を指定する方法がいくつかあります。  
それらを調べ、ソート条件の指定を補助するためのクラスを作成しました。

まず、配列やコレクションをソートするために呼び出す主なメソッドを挙げてみます。
- [Array.Sort メソッド](https://docs.microsoft.com/dotnet/api/system.array.sort)
- [List\<T\>.Sort メソッド](https://docs.microsoft.com/dotnet/api/system.collections.generic.list-1.sort)
- [SortedSet\<T\> コンストラクター](https://docs.microsoft.com/dotnet/api/system.collections.generic.sortedset-1.-ctor)
- [Enumerable.OrderBy メソッド](https://docs.microsoft.com/dotnet/api/system.linq.enumerable.orderby) など
