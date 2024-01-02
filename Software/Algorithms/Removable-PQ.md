# 削除可能な優先度付きキュー

// [競技プログラミング Advent Calendar 2023](https://qiita.com/advent-calendar/2023/kyopro) の 23 日目の記事です。

```csharp:RemovableListHeapQueue.cs
// ImplicitUsings: enable
[System.Diagnostics.DebuggerDisplay(@"Count = {Count}")]
public class RemovableListHeapQueue<T>
{
	readonly IComparer<T> c;
	readonly List<T> l = new List<T>();
	int n;
	readonly Dictionary<T, int> counts = new Dictionary<T, int>();

	public RemovableListHeapQueue(IEnumerable<T> items = null, IComparer<T> comparer = null)
	{
		c = comparer ?? Comparer<T>.Default;
		if (items != null) foreach (var x in items) Push(x);
	}

	public IComparer<T> Comparer => c;
	public int Count => n;
	public T First => n != 0 ? l[0] : throw new InvalidOperationException("No items.");

	public void Clear()
	{
		l.Clear();
		n = 0;
		counts.Clear();
	}

	public int GetCount(T item) => counts.GetValueOrDefault(item);

	bool TrySwap(int i)
	{
		var p = (i - 1) / 2;
		if (c.Compare(l[p], l[i]) <= 0) return false;
		(l[i], l[p]) = (l[p], l[i]);
		return true;
	}

	void UpHeap()
	{
		for (var i = l.Count - 1; i > 0; i = (i - 1) / 2)
		{
			if (!TrySwap(i)) break;
		}
	}

	void DownHeap()
	{
		for (var i = 1; i < l.Count; i = i * 2 + 1)
		{
			if (i + 1 < l.Count && c.Compare(l[i + 1], l[i]) < 0) ++i;
			if (!TrySwap(i)) break;
		}
	}

	public void Push(T item)
	{
		l.Add(item);
		UpHeap();
		++n;
		counts[item] = counts.GetValueOrDefault(item) + 1;
	}

	public T Pop()
	{
		if (n == 0) throw new InvalidOperationException("No items.");
		var item = l[0];
		Remove(item);
		return item;
	}

	public bool Remove(T item)
	{
		if (!counts.TryGetValue(item, out var count)) return false;
		if (--count == 0) counts.Remove(item); else counts[item] = count;
		--n;
		EnsureFirst();
		return true;
	}

	void EnsureFirst()
	{
		while (l.Count > 0 && !counts.ContainsKey(l[0]))
		{
			l[0] = l[l.Count - 1];
			l.RemoveAt(l.Count - 1);
			DownHeap();
		}
	}
}
```

## 作成したサンプル
- [AlgorithmSample (GitHub)](https://github.com/sakapon/Samples-2020/tree/master/AlgorithmSample/AlgorithmLib10/DataTrees/PQ)

## 検証したバージョン
- C# 10
- .NET 6

## 問題集
- [ABC 330 E - Mex and Update](https://atcoder.jp/contests/abc330/tasks/abc330_e)
- [ABC 194 E - Mex Min](https://atcoder.jp/contests/abc194/tasks/abc194_e)
- [ABC 306 E - Best Performances](https://atcoder.jp/contests/abc306/tasks/abc306_e)
- [ABC 281 E - Least Elements](https://atcoder.jp/contests/abc281/tasks/abc281_e)

## 参照
1. [優先度付きキュー (Wikipedia)](https://t.co/hRIyIbXAmC)
1. [PriorityQueue\<TElement,TPriority\> クラス](https://learn.microsoft.com/dotnet/api/system.collections.generic.priorityqueue-2)
