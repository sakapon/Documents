## 透過プロキシでアスペクト指向プログラミング (1)

前回の[インターフェイスに対する透過プロキシ](Transparent-Proxy-Interface.md)では、[RealProxy クラス](https://msdn.microsoft.com/ja-jp/library/system.runtime.remoting.proxies.realproxy.aspx)を利用してインターフェイスに対する透過プロキシを生成し、  
その実装クラスが存在していなくてもメソッドに処理を割り当てることができました。

改めて、透過プロキシ (transparent proxy) の主な特徴を整理すると次のようになります。
- 対象となる型は、[MarshalByRefObject](https://msdn.microsoft.com/ja-jp/library/system.marshalbyrefobject.aspx) を継承したクラス、またはインターフェイス
- 各メンバーが呼び出されたときの挙動を上書きできる

今回は、MarshalByRefObject を継承したクラスの既存の処理を透過プロキシで拡張して、  
[アスペクト指向プログラミング (AOP)](https://t.co/K3PluHqMbh) を実践してみます。

一般的にアプリケーション設計においては、  
ログ出力やデータベース トランザクションなど、いろいろなビジネス ロジックに共通する横断的関心事があります。  
例えばデータベース トランザクションであれば、以前に[ファントム リードとその解決方法](https://sakapon.wordpress.com/2011/12/14/phantomread2/)などで書いている通り、  
ビジネス ロジックごとに TransactionScope に関する同じようなコードを繰り返し記述することになります。  
この部分をアスペクト (側面) として分離し、属性として記述できるようにすることで再利用性を高めることを目指します。

まず次のコードで示す通り、RealProxy を継承した CrossCuttingProxy クラスと、アスペクトを表す属性を定義します。

https://gist.github.com/sakapon/5d61eedc9580f6dba2bbefcb4373839e

ログ出力を表す TraceLogAttribute クラスとデータベース トランザクションを表す TransactionScopeAttribute クラスを用意し、  
既存の処理の前後に割り込むようにしてそれぞれの処理を追加しています。

以上のようにすれば、ビジネス ロジックを次のように記述するだけで済むようになります。

https://gist.github.com/sakapon/54ef60b0d084e0f259a973fc119f5425

実行結果です。ログが出力されています：

![CrossCuttingConsole](https://github.com/sakapon/Samples-2017/blob/master/Images/ProxySample/CrossCuttingConsole.png)

前回：[インターフェイスに対する透過プロキシ](Transparent-Proxy-Interface.md)  
次回：[透過プロキシでアスペクト指向プログラミング (2)](Transparent-Proxy-AOP-2.md)

**作成したサンプル**  
[CrossCuttingConsole (GitHub)](https://github.com/sakapon/Samples-2017/tree/master/ProxySample/CrossCuttingConsole)

**バージョン情報**  
C# 7.0  
.NET Framework 4.5

#### 参照
- [RealProxy クラス](https://msdn.microsoft.com/ja-jp/library/system.runtime.remoting.proxies.realproxy.aspx)
- [アスペクト指向プログラミング](https://t.co/K3PluHqMbh) (Wikipedia)
- [RealProxy クラスによるアスペクト指向プログラミング](https://docs.microsoft.com/ja-jp/archive/msdn-magazine/2014/february/aspect-oriented-programming-aspect-oriented-programming-with-the-realproxy-class) (MSDN Magazine)
