# BFS と DFS の探索順を視覚化する
2次元グリッド上の**幅優先探索** (breadth-first search; **BFS**) および**深さ優先探索** (depth-first search; **DFS**) について、どのような順序で探索が進行するのかを視覚化してみます。

最初に、視覚化した結果を載せておきます。
マスの色は、赤は処理中、橙は保留中、緑は完了を表しています。
3種類のアニメーションがありますが、それぞれどのような処理手順により実現されているのかを予想してみてください。

![](https://github.com/sakapon/Tools-2024/blob/main/Images/BfsDfs/BfsDfsViewer-1.0.3-Rotation.gif)

BFS を実装するには主にキューを利用しますが、
DFS を実装するには主に、スタックを利用する方法と再帰関数を利用する方法の2つが挙げられます。
以下では、これら3通りの実装方法を記述していきます。

## 幅優先探索 (キュー)
キューを利用して**幅優先探索 (BFS)** を実装します。
ここでは、同じ頂点をキューに二度入れない (既に探索済ならキューに入れない) ような実装としています。

```csharp
// h: 高さ、w: 幅
int h, w;
// n: h * w
int n;

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

変数 `d` には、始点からの移動手数 (距離) が格納されます。
BFS では最短経路が求められます。
`GetNexts` メソッドでは、隣接する頂点を上、左、下、右の順に返しています。

## 深さ優先探索 (スタック)
前節のコードで、キューをスタックに変更するだけで**深さ優先探索 (DFS)** になります。
今回の3種類の実装のうち、探索の様子が最も複雑となり予想しづらいのではないかと思います。

```csharp
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

DFS では始点からの最短経路を求めることができません。
そのため実際には `int[]` ではなく `bool[]` を用意し、障害物のあるグリッドや非連結なグラフにおいて、始点から到達可能かどうかを調べるために使われることが多いです。

## 深さ優先探索 (再帰関数)

```csharp
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

## 視覚化

```csharp
IEnumerable<int> GetNexts(int v)
{
	var (i, j) = (v / w, v % w);
	if (i > 0) yield return v - w;
	if (i + 1 < h) yield return v + w;
	if (j > 0) yield return v - 1;
	if (j + 1 < w) yield return v + 1;
}
```

![](https://github.com/sakapon/Tools-2024/blob/main/Images/BfsDfs/BfsDfsViewer-1.0.3-Cross.gif)

なお、このビューアーは .NET の WPF で作成されています。

## 作成したビューアー
- [BfsDfsViewer (GitHub)](https://github.com/sakapon/Tools-2024/tree/main/BfsDfs)

## 利用したバージョン
- C# 10, .NET 6

## 参照
1. [幅優先探索 (Wikipedia)](https://ja.wikipedia.org/wiki/幅優先探索)
1. [深さ優先探索 (Wikipedia)](https://ja.wikipedia.org/wiki/深さ優先探索)
