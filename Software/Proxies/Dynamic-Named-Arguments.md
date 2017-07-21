C# で、メソッドに引数として「キーと値のペア」を渡す方法を考えてみます。
例えば HTTP で GET でアクセスするときに URL でクエリ文字列を指定する場合が挙げられます。

よく使われているのは、メソッドの引数に Dictionary や匿名型オブジェクトを渡す方法かと思います。
以下の HttpHelper クラスのように実装します。
なお、[WebClient クラス](https://msdn.microsoft.com/ja-jp/library/system.net.webclient.aspx)ではクエリ文字列 (QueryString プロパティ) は NameValueCollection 型であるため、
受け取った情報を NameValueCollection 型に変換しています。
