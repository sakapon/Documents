# BFS と DFS の探索順を視覚化する
2次元グリッド上の幅優先探索 (breadth-first search; BFS) および深さ優先探索 (depth-first search; DFS) について、どのような順序で探索が進行するのかを視覚化してみます。

最初に、視覚化した結果を載せておきます。
マスの色は、赤は処理中、橙は保留中、緑は完了を表しています。
3種類のアニメーションがありますが、それぞれどのような処理手順により実現されているのかを予想してみてください。

![](https://github.com/sakapon/Tools-2024/blob/main/Images/BfsDfs/BfsDfsViewer-1.0.3-Rotation.gif)

BFS を実装するには主にキューを利用しますが、
DFS を実装するには主に、スタックを利用する方法と再帰関数を利用する方法の2つが挙げられます。
以下では、これら3通りの実装方法を記述していきます。

## BFS (キュー)
キューを利用して幅優先探索 (BFS) を実装します。
ここでは、同じ頂点をキューに二度入れない (既に探索済ならキューに入れない) ような実装としています。

変数 d には、始点からの移動手数 (距離) が格納されます。
BFS では最短経路が求められます。
GetNexts メソッドでは、隣接する頂点を上、左、下、右の順に返しています。

```csharp
public int[] ShortestByBFS(int n, int sv)
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
			if (d[nv] == -1) continue;
			d[nv] = d[v] + 1;
			q.Enqueue(nv);
		}
	}
	return d;
}

// h: 高さ、w: 幅
IEnumerable<int> GetNexts(int v)
{
	var (i, j) = (v / w, v % w);
	if (i > 0) yield return v - w;
	if (j > 0) yield return v - 1;
	if (i + 1 < h) yield return v + w;
	if (j + 1 < w) yield return v + 1;
}
```

## DFS (スタック)
前節のコードで、キューをスタックに変更するだけで


## DFS (再帰関数)

## 視覚化

![](https://github.com/sakapon/Tools-2024/blob/main/Images/BfsDfs/BfsDfsViewer-1.0.3-Cross.gif)

## 作成したサンプル
- [BfsDfsViewer (GitHub)](https://github.com/sakapon/Tools-2024/tree/main/BfsDfs)

## 検証したバージョン
- C# 10, .NET 6

## 参照
1. [幅優先探索 (Wikipedia)](https://ja.wikipedia.org/wiki/幅優先探索)
1. [深さ優先探索 (Wikipedia)](https://ja.wikipedia.org/wiki/深さ優先探索)
