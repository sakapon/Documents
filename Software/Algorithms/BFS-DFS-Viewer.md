# BFS と DFS の探索順を視覚化する
2次元グリッド上の**幅優先探索** (breadth-first search; **BFS**) および**深さ優先探索** (depth-first search; **DFS**) について、どのような順序で探索が進行するのかを視覚化してみます。

最初に、視覚化した結果を載せておきます。
グリッドの中央のマスを始点とし、マスの色はそれぞれ

- 赤： 探索処理中
- 橙： 保留中
- 緑： 完了

を表しています。

![BfsDfsViewer-1.0.3-Rotation.gif](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/9649/9b6db4ae-3b14-832d-9c54-354ab9d4d974.gif)

3種類のアニメーションがあり、それぞれ

- 左上： 幅優先探索 (キューを利用)
- 左下： 深さ優先探索 (スタックを利用)
- 右下： 深さ優先探索 (再帰関数を利用)

を示しています。

幅優先探索を実装するには主にキューを利用しますが、深さ優先探索を実装するには、スタックを利用する方法と再帰関数を利用する方法の2つが挙げられます。
以下では、これら3種類の実装方法について記述していきます。

## 幅優先探索 (キューを利用)
キューを利用して**幅優先探索 (BFS)** を実装します。
ここでは、[幅優先探索 (Wikipedia)](https://ja.wikipedia.org/wiki/幅優先探索) に記載されている方法と同様に、同じ頂点 (マス) をキューに二度追加しない (既に探索済ならキューに追加しない) ような実装としています。

また、グリッドの高さを `h`、幅を `w` とし、左上の頂点を原点 $(0, 0)$ として、`i` 行目 `j` 列目の頂点の ID を `w * i + j` と定めます。

```csharp:BFS_Queue.cs
// h: 高さ、w: 幅
int h, w;
// n: マスの数 (h * w)
int n;

// sv: 始点の ID
public int[] BFSByQueue(int sv)
{
	var d = new int[n];
	Array.Fill(d, -1);
	d[sv] = 0;

	var q = new Queue<int>();
	q.Enqueue(sv);

	while (q.Count > 0)
	{
		var v = q.Dequeue();

		foreach (var nv in GetNexts(v))
		{
			if (d[nv] != -1) continue;
			d[nv] = d[v] + 1;
			q.Enqueue(nv);
		}
	}
	return d;
}

IEnumerable<int> GetNexts(int v)
{
	var (i, j) = (v / w, v % w);
	if (i > 0) yield return v - w;
	if (j > 0) yield return v - 1;
	if (i + 1 < h) yield return v + w;
	if (j + 1 < w) yield return v + 1;
}
```

変数 `d` には、始点からの移動手数 (距離) を記録します。
BFS の場合はこれで最短経路が求められます。

`GetNexts` メソッドでは、隣接する頂点を上、左、下、右の順に返しています。

## 深さ優先探索 (スタックを利用)
前節のコードで、キューをスタックに変更するだけで**深さ優先探索 (DFS)** になります。
DFS の実装方法についても、[深さ優先探索 (Wikipedia)](https://ja.wikipedia.org/wiki/深さ優先探索) に記載があります。

今回の3種類の実装のうち、保留中の頂点の管理が最も複雑となり、探索の様子を直感的に予想しづらいのではと思います。

```csharp:DFS_Stack.cs
public int[] DFSByStack(int sv)
{
	var d = new int[n];
	Array.Fill(d, -1);
	d[sv] = 0;

	var q = new Stack<int>();
	q.Push(sv);

	while (q.Count > 0)
	{
		var v = q.Pop();

		foreach (var nv in GetNexts(v))
		{
			if (d[nv] != -1) continue;
			d[nv] = d[v] + 1;
			q.Push(nv);
		}
	}
	return d;
}
```

なお、DFS では始点からの最短経路を求めることができません。
そのため実際には `int[]` ではなく `bool[]` を用意し、障害物のあるグリッドや非連結なグラフにおいて、単に始点から到達可能かどうかを調べるために使われることが多いです。
(ただし、木構造の場合には最短経路となります。)

## 深さ優先探索 (再帰関数を利用)
再帰関数を利用した DFS では、各頂点において2個目以降の分岐を調べる前に次の頂点の処理を開始するため、遠く離れた頂点に早く到達することがあります。

```csharp:DFS_Rec.cs
public int[] DFSByRec(int sv)
{
	var d = new int[n];
	Array.Fill(d, -1);
	d[sv] = 0;

	Rec(sv);
	return d;

	void Rec(int v)
	{
		foreach (var nv in GetNexts(v))
		{
			if (d[nv] != -1) continue;
			d[nv] = d[v] + 1;
			Rec(nv);
		}
	}
}
```

## 隣接頂点
`GetNexts` メソッドにおいて隣接頂点の列挙順を変更すると、探索の様子も少し変わります。
次の例では、隣接する頂点を左、右、上、下の順に返します。

```csharp
IEnumerable<int> GetNexts(int v)
{
	var (i, j) = (v / w, v % w);
	if (j > 0) yield return v - 1;
	if (j + 1 < w) yield return v + 1;
	if (i > 0) yield return v - w;
	if (i + 1 < h) yield return v + w;
}
```

![BfsDfsViewer-1.0.3-Cross.gif](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/9649/3c70f39e-3f8c-04cb-dd36-13a919696a3e.gif)

以上のように、作成したコードが実際にどのように動くのかを視覚化して確認するのもよい練習となるでしょう。
なお、このビューアーは .NET の [WPF](https://learn.microsoft.com/dotnet/desktop/wpf/) で作成されています。

## 作成したビューアー
- [BfsDfsViewer (GitHub)](https://github.com/sakapon/Tools-2024/tree/main/BfsDfs)

## 利用したバージョン
- C# 10, .NET 6

## 参照
1. [幅優先探索 (Wikipedia)](https://ja.wikipedia.org/wiki/幅優先探索)
1. [深さ優先探索 (Wikipedia)](https://ja.wikipedia.org/wiki/深さ優先探索)
