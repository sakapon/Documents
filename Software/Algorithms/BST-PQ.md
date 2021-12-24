# 平衡二分探索木を優先度付きキューとして使う
.NET 6 では、優先度付きキューを表す [PriorityQueue\<TElement,TPriority\> クラス](https://docs.microsoft.com/dotnet/api/system.collections.generic.priorityqueue-2)が基本クラスライブラリ (BCL) に追加されました。  
言い換えると、.NET 5 以前の BCL には優先度付きキューが存在しません。  
そこで、.NET 5 以前の環境において、平衡二分探索木のクラスを利用して優先度付きキューに相当する処理を実現する方法を考えてみます。

## 優先度付きキューの要件
まず、優先度付きキューに必要な機能を整理します：
- Count: 現在の要素の数を取得する
- Peek: 優先度が最も高い要素を取得する。削除はしない
- Pop: 優先度が最も高い要素を取得し、同時に削除する
- Push: 新たな要素を追加する

すなわち、初めから全ての要素がわかっている (静的データ) とは限らず、要素の動的な追加に対応します。  
また、オプション機能として次のものが考えられます：
- 一般的に、要素の重複を許す
- 昇順・降順を指定できる
- 要素に対して、優先度を表すキーを指定できる

例えば、1組の生徒たち、2組の生徒たち、・・・のような順でデータを取得する要件に対応するには、優先度を表すキーを指定できる機能があると便利です。

## 平衡二分探索木に関連するクラス
ここでは、.NET 5 以前の BCL に存在する平衡二分探索木およびその周辺のクラスについてまとめます。  
以下の3種類があり、「追加した要素が自動的にキーの順序でソートされる」「キーの重複は不許可」という点はこれらに共通です。  
なお、SortedList\<TKey,TValue\> クラスは平衡二分探索木ではありません (この記事の後半では使いません)。

- [SortedSet\<T\> クラス](https://docs.microsoft.com/dotnet/api/system.collections.generic.sortedset-1)
  - 要素をキーとして使用する
  - 要素の動的な追加および削除の時間計算量は O(log n)
  - HashSet\<T\> が順序付きになったと考えてよい
- [SortedDictionary\<TKey,TValue\> クラス](https://docs.microsoft.com/dotnet/api/system.collections.generic.sorteddictionary-2)
  - キーと値のペアを格納する
  - 要素の動的な追加および削除の時間計算量は O(log n)
  - Dictionary\<TKey,TValue\> が順序付きになったと考えてよい
- [SortedList\<TKey,TValue\> クラス](https://docs.microsoft.com/dotnet/api/system.collections.generic.sortedlist-2)
  - キーと値のペアを格納する
  - 要素の動的な追加および削除の時間計算量は O(n)
  - キーからインデックスの取得、インデックスから要素の取得の時間計算量は O(log n)
  - 通常のリストのような構造であり、空間計算量は O(n)
    - 平衡二分探索木ではない
  - ソート済みの静的データで初期化するだけの場合や、末尾にくるデータを追加するだけであれば速い

## 実装
では、平衡二分探索木を利用して、優先度付きキューを実装していきます。  
要件に合わせて以下の3種類を用意しました。

(1) 要素を重複させない場合
- SortedSet\<T\> をほぼそのまま利用する
- Min プロパティおよび Add, Remove メソッドを呼び出せばよい

(2) 要素の重複を許す場合 (一般的な優先度付きキュー)
- SortedDictionary\<T, int\> を利用する
- 要素ごとの個数を管理する

(3) 要素に対して優先度を表すキーを指定する場合 (重複を許可)
- SortedDictionary\<TKey, Queue\<T\>\> を利用する
- キーごとにキューで要素を管理する

ソースコードは次の通りです。言語は C# です。

### 作成したサンプル
- [AlgorithmSample (GitHub)](https://github.com/sakapon/Samples-2020/tree/master/AlgorithmSample/AlgorithmLab/DataTrees)

### 検証したバージョン
- C# 8.0
- .NET Standard 2.1

### 参照
- [PriorityQueue\<TElement,TPriority\> クラス](https://docs.microsoft.com/dotnet/api/system.collections.generic.priorityqueue-2)
- [SortedSet\<T\> クラス](https://docs.microsoft.com/dotnet/api/system.collections.generic.sortedset-1)
- [SortedDictionary\<TKey,TValue\> クラス](https://docs.microsoft.com/dotnet/api/system.collections.generic.sorteddictionary-2)
