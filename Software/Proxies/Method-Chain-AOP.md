## メソッドチェーンでアスペクト指向プログラミング

[透過プロキシでアスペクト指向プログラミング (1)](Transparent-Proxy-AOP-1.md) では、ログ出力やデータベース トランザクションなどの横断的関心事を、

```c#
public class NorthwindBusiness : MarshalByRefObject
{
    [TraceLog]
    [TransactionScope]
    public short SelectUnitsInStock()
    {
    }
}
```

のように、属性として記述することができました。  
今回は、これを属性を使わずに、LINQ などのようにメソッドチェーンで横断的関心事を記述してみます。

まず、戻り値を持つ (`Func<TResult>` に相当する) 処理を `IProxyable<TResult>` インターフェイスとして定義して、  
後ろにメソッドチェーンを追加することで処理を上書きできるようにします。

https://gist.github.com/sakapon/32e1a0d760558a951117c66c2aab7ecb

これを実行すると出力は次のようになり、元の処理の前後に処理が追加されていることがわかります。

```
Before
Body
After
```

次に、戻り値を持たない (`Action` に相当する) 処理を表す `IProxyable` インターフェイスを、
`IProxyable<TResult>` インターフェイスの特別な場合とみなして継承させます。

https://gist.github.com/sakapon/7235fb5310caa0437efe1d8c12572c40

あとは、ログ出力やデータベース トランザクションなどの横断的関心事を拡張メソッドとして作成すれば完成です。

https://gist.github.com/sakapon/608a817c849bd5a2cf3cba473aa52cef

実行結果です：  
(図)

- .NET で透過プロキシを使いたくない (処理速度を上げたいなど)
- .NET の属性のような仕組みを持たない別のプラットフォーム
などの場合に使うとよいでしょう。

前回：[透過プロキシでアスペクト指向プログラミング (2)](Transparent-Proxy-AOP-2.md)

**作成したサンプル**  
[ProxyableConsole (GitHub)](https://github.com/sakapon/Samples-2017/tree/master/ProxySample/ProxyableConsole)

**バージョン情報**  
C# 7.0  
.NET Framework 4.5

#### 参照
- [アスペクト指向プログラミング](https://t.co/K3PluHqMbh)
