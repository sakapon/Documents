# 数論・素数に関するメソッドを実装する
数論・素数に関するメソッドを C# で一通り実装してまとめました。  
通常の数値計算プログラミングにも、また競技プログラミング用のライブラリとしても使えます。

機能は次の通りです。

### Euclid 互除法
- 最大公約数 (GCD)
- 最小公倍数 (LCM)
- Euclid 互除法の拡張
  - `ax + by = 1` の解

### 素数
下の O(x) は計算量のオーダーを表します。
- 素因数分解
  - O(√n)
- 約数の列挙
  - O(√n)
- 素数判定
  - O(√n)
- n 以下の素数の列挙
  - およそ O(n)
- m 以上 M 以下の素数の列挙
  - およそ O(max(√M, M - m))

ソースコードは以下の通りです。  
ここではシンプルな実装とし、例外処理も全体的に省略しています。  
さらに無駄な演算処理を除くように改良することで、もう少し速くすることはできるはずです。

https://gist.github.com/sakapon/9ee745bd460ae03045ee406a3de6a90c

使用例を次に示します。

https://gist.github.com/sakapon/62e6644f0c0208007325896c03642552

前回: [二分探索のライブラリ化](Binary-Search.md)

### 作成したサンプル
- [AlgorithmSample (GitHub)](https://github.com/sakapon/Samples-2020/tree/master/AlgorithmSample/AlgorithmLib)

### バージョン情報
- C# 7.0
- .NET Standard 2.0

### 参照
- [プログラミングコンテストチャレンジブック](https://www.amazon.co.jp/dp/B00CY9256C)
- [エラトステネスのふるいとその計算量](https://mathtrain.jp/eratosthenes)
- [LINQ で素数を求める (C#)](https://sakapon.wordpress.com/2014/08/18/linq-prime-numbers/)
