## インターフェイスに対する透過プロキシ

前回の [DLR で名前付き引数を使う](Dynamic-Named-Arguments.md)では、メソッドに引数として「キーと値のペア」を渡す方法を考え、  
動的言語ランタイム (DLR) を利用する方法を紹介しました。  
しかし、同じ処理を複数の場所で呼び出す必要があるなど、再利用性を重視する場合は、  
なるべく静的にシグネチャを解決する方法がよいでしょう。

そこで今回は、呼び出すサービスをインターフェイスとして定義し、透過プロキシ (transparent proxy) を利用してみます。  
先にコードを示します。

https://gist.github.com/sakapon/fac58772e6c6e148d54880c0106e50b3

このコードでは、[RealProxy クラス](https://msdn.microsoft.com/ja-jp/library/system.runtime.remoting.proxies.realproxy.aspx)を継承した HttpProxy<IService> クラスを作成しています。  
RealProxy はプロキシの実体となるものであり、  
その外層である透過プロキシを RealProxy.GetTransparentProxy メソッドで取得できます。

一方、利用する側の Program.cs では、ICgisService インターフェイスを定義しておきます。  
その透過プロキシを生成すれば、ICgisService インターフェイスを実装するクラスが存在しなくてもメソッドを呼び出すことができ、  
実体は HttpProxy<IService> クラスの Invoke メソッドとなります。

また、BaseUriAttribute クラスを定義しており、  
呼び出される Web API のベースとなる URI を属性で指定できるようにしています。

WCF における契約プログラミングでは、クライアント側とサーバー側で同一のインターフェイスを利用し、  
クライアント側からのアクセスを上記のように透過プロキシで実装する方法があります。  
[方法 : ChannelFactory を使用する](https://msdn.microsoft.com/ja-jp/library/ms734681.aspx) にある通り、ChannelFactory.CreateChannel メソッドで透過プロキシを生成します。
