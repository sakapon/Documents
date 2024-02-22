# BFS と DFS の探索順を視覚化する
2次元グリッド上の幅優先探索 (breadth-first search; BFS) および深さ優先探索 (depth-first search; DFS) について、どのような順序で探索が進行するのかを視覚化してみます。

![](https://github.com/sakapon/Tools-2024/blob/main/Images/BfsDfs/BfsDfsViewer-1.0.3-Rotation.gif)

```csharp
public int[] ShortestByBFS(int n, int sv)
{
	var costs = new int[n];
	Array.Fill(costs, -1);
	costs[sv] = 0;

	var q = new Queue<int>();
	q.Enqueue(sv);

	while (q.Count > 0)
	{
		var v = q.Dequeue();

		foreach (var nv in GetNexts(v))
		{
			if (costs[nv] == -1) continue;
			costs[nv] = costs[v] + 1;
			q.Enqueue(nv);
		}
	}
	return costs;
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

![](https://github.com/sakapon/Tools-2024/blob/main/Images/BfsDfs/BfsDfsViewer-1.0.3-Cross.gif)
