# 削除可能な優先度付きキュー

## はじめに
前回投稿した [mex のライブラリ化](Mex-Multiset.md)で、「削除機能を持った優先度付きキュー」について触れました。
前回の記事では、
- キーは、範囲が制限された非負整数
- .NET の [`PriorityQueue<TElement, TPriority>`](https://learn.microsoft.com/dotnet/api/system.collections.generic.priorityqueue-2) クラスを利用する

という場合のソースコードを示しましたが、今回は、
- キーは、任意の `T` 型の値
- [優先度付きキュー](https://t.co/hRIyIbXAmC)の主要機能である up-heap や down-heap などを自作する

という場合の実装を考えていきます。

なお、前回の記事の続きとなるため、[競技プログラミング Advent Calendar 2023](https://qiita.com/advent-calendar/2023/kyopro) の 23 日目の記事として、あとから登録しました。

## 実装 1
基本的な方針は[前回](Mex-Multiset.md)と同様で、各要素の重複数を保持しておき、先頭の要素が削除済 (重複数が 0) になっていれば二分ヒープから削除します (遅延評価)。
任意の `T` 型の各要素の重複数を管理するために、連想配列 (.NET では `Dictionary<T, int>`) を利用します。

ソースコードは次のようになります (C#)。

```csharp:RemovableListHeapQueue.cs
// ImplicitUsings: enable
[System.Diagnostics.DebuggerDisplay("Count = {Count}")]
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

`List<T>` で二分ヒープを構成し、インデックスは 0 から使っています。
`Dictionary<T, int>` が重いという印象があるかもしれませんが、呼び出される回数が少ないため、性能にはほとんど影響ありません。

`Pop` や `Remove` が呼び出されたときに、最後に `EnsureFirst` を呼び出すことで、先頭の要素がつねに未削除の要素となるように保ちます。
時間計算量は、サイズを $n$ としたとき `First` は $O(1)$ 、`Push` は $O( \log n)$ 、そして `Pop` と `Remove` は最悪 $O(n \log n)$ ですが償却で $O( \log n)$ となります。

## 実装 2
ここで、`EnsureFirst` メソッドをもう少し軽くできないかを考えてみます。

例えば、先頭の要素のみ未削除で他の要素がすべて削除済のときに `Pop` を呼び出すと、計算量 $O(n \log n)$ で二分ヒープから要素が削除されていき、最終的に空になります。
そこで `EnsureFirst` メソッドを、末尾の要素が削除済の場合は先頭に移動させずにそのまま二分ヒープから削除するという実装に変更することで、このような例に対して $O(n)$ にできます。

```csharp
void EnsureFirst()
{
	while (l.Count > 0 && !counts.ContainsKey(l[0]))
	{
		while (l.Count > 0 && !counts.ContainsKey(l[l.Count - 1])) l.RemoveAt(l.Count - 1);
		if (l.Count == 0) break;
		l[0] = l[l.Count - 1];
		l.RemoveAt(l.Count - 1);
		DownHeap();
	}
}
```

`PriorityQueue<TElement, TPriority>` クラスを利用せずに二分ヒープを自作することで、このような処理も可能となります。

## 性能テスト
最大 150 万件のデータを扱う [ABC 194 E - Mex Min](https://atcoder.jp/contests/abc194/tasks/abc194_e) で実行時間を比較してみます。
最も重いテストケース `answer_n_00` が、実装 1 では 873 ms、実装 2 では 522 ms となりました。

https://atcoder.jp/contests/abc194/submissions/48987254

https://atcoder.jp/contests/abc194/submissions/48987280

また、連想配列を利用して各要素のインデックスを管理するなど、遅延評価せずに直接削除できる実装もありますが、こちらはかなり性能が落ちてしまいます。

## 作成したサンプル
- [AlgorithmSample (GitHub)](https://github.com/sakapon/Samples-2020/tree/master/AlgorithmSample/AlgorithmLib10/DataTrees/PQ)

## 検証したバージョン
- 開発環境: C# 10, .NET 6
- 実行環境: C# 11, .NET 7

## 問題集
- [ABC 194 E - Mex Min](https://atcoder.jp/contests/abc194/tasks/abc194_e)
- [ABC 330 E - Mex and Update](https://atcoder.jp/contests/abc330/tasks/abc330_e)
- [ABC 306 E - Best Performances](https://atcoder.jp/contests/abc306/tasks/abc306_e)
- [ABC 281 E - Least Elements](https://atcoder.jp/contests/abc281/tasks/abc281_e)

## 参照
1. [優先度付きキュー (Wikipedia)](https://t.co/hRIyIbXAmC)
1. [PriorityQueue\<TElement,TPriority\> クラス](https://learn.microsoft.com/dotnet/api/system.collections.generic.priorityqueue-2)

前回: [mex のライブラリ化](Mex-Multiset.md)
