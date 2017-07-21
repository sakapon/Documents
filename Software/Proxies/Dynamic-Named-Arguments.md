## DLR で名前付き引数を使う

C# で、メソッドに引数として「キーと値のペア」を渡す方法を考えてみます。
例えば HTTP で GET でアクセスするときに URL でクエリ文字列を指定する場合が挙げられます。

よく使われているのは、メソッドの引数に Dictionary や匿名型オブジェクトを渡す方法かと思います。
以下の HttpHelper クラスのように実装します。
なお、[WebClient クラス](https://msdn.microsoft.com/ja-jp/library/system.net.webclient.aspx)ではクエリ文字列 (QueryString プロパティ) は NameValueCollection 型であるため、
受け取った情報を NameValueCollection 型に変換しています。

https://gist.github.com/sakapon/d07be0cbf35ddd90eaeffa2b989e2b02

なお、ここでは題材として [CGI's 郵便番号検索 API](http://zip.cgis.biz/) を利用しています。

さて、[動的言語ランタイム (DLR)](https://msdn.microsoft.com/library/dd233052.aspx) と[名前付き引数](https://docs.microsoft.com/ja-jp/dotnet/csharp/programming-guide/classes-and-structs/named-and-optional-arguments)を利用して、
引数の情報を実行時に解決できないかと考えると、次のような方法を思いつきます。

```C#
dynamic http = new DynamicHttpProxy();
var result = http.Get(Uri_Cgis_Xml, zn: "402", ver: 1);
```

実際、[DynamicObject クラス](https://msdn.microsoft.com/ja-jp/library/system.dynamic.dynamicobject.aspx)を継承した DynamicHttpProxy クラスを次のように作れば可能です。

https://gist.github.com/sakapon/3d0726245f7319d26913045637a6e6cd

TryInvokeMember メソッドの中で、引数の名前は binder.CallInfo.ArgumentNames で取得できます。
ただし、引数の名前を指定せずに渡された分はここに含まれない (コレクションの長さが変わる) ため注意が必要です。

また、C# 7.0 で追加された ValueTuple を利用して、

```C#
var result = HttpHelper.Get(Uri_Cgis_Xml, (zn: "6050073"));
```

とする案もありましたが、
- 要素が 1 つ以下の場合、タプル リテラルを記述できない
- コンパイル後はフィールド名が残らないため、実行時に動的に取得できない

という制約により実現できませんでした。

次回：[インターフェイスに対する透過プロキシ](Transparent-Proxy-Interface.md)

**作成したサンプル**
- [DynamicHttpConsole (GitHub)](https://github.com/sakapon/Samples-2017/tree/master/ProxySample/DynamicHttpConsole)

**バージョン情報**
- C# 7.0
- .NET Framework 4.5

**参照**
- [動的言語ランタイムの概要](https://msdn.microsoft.com/library/dd233052.aspx)
- [名前付き引数と省略可能な引数](https://docs.microsoft.com/ja-jp/dotnet/csharp/programming-guide/classes-and-structs/named-and-optional-arguments)
- [タプル – C# によるプログラミング入門](http://ufcpp.net/study/csharp/datatype/tuples/)
