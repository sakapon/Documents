## 透過プロキシでアスペクト指向プログラミング (2)

[前回の記事](Transparent-Proxy-AOP-1.md)で、NorthwindBusiness クラスの透過プロキシを生成するときに、

```c#
var nw = CrossCuttingProxy.CreateProxy<NorthwindBusiness>();
```

というコードを記述していましたが、  
MarshalByRefObject の代わりに [ContextBoundObject クラス](https://msdn.microsoft.com/ja-jp/library/system.contextboundobject.aspx)を継承させると、通常のコンストラクターを利用することができます。  
ただし、クラスに属性を付ける必要があります。  
次のように実装します。

https://gist.github.com/sakapon/89b097430f95b3d0adb0ddf17863c3c2

以上で、

```c#
var nw = new NorthwindBusiness();
```

と記述できるようになりました。  
なお、上記のコードには現れていませんが、  
コンストラクターが呼び出されたときに、CrossCuttingProxy クラスの Invoke メソッドが呼び出されます。

前回：[透過プロキシでアスペクト指向プログラミング (1)](Transparent-Proxy-AOP-1.md)
次回：[メソッドチェーンでアスペクト指向プログラミング](Method-Chain-AOP.md)

**作成したサンプル**  
[CrossCuttingConsole (GitHub)](https://github.com/sakapon/Samples-2017/tree/master/ProxySample/CrossCuttingConsole)

**バージョン情報**  
C# 7.0  
.NET Framework 4.5

#### 参照
- [RealProxy クラス](https://msdn.microsoft.com/ja-jp/library/system.runtime.remoting.proxies.realproxy.aspx)
- [アスペクト指向プログラミング](https://t.co/K3PluHqMbh)
