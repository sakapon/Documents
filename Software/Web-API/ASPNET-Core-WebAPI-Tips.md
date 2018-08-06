# ASP.NET Core Web API の Tips
ASP.NET Core で Web API を利用する際の注意点や備忘録です。ほぼ箇条書きです。  
[ASP.NET Web API 版](ASPNET-WebAPI-Tips-1.md)は以前に書きました。

## ルーティング、コントローラーなど
[ASP.NET Web API](ASPNET-WebAPI-Tips-1.md) と細かい差異はありますが、説明は省略します。  
公式解説を参照するとよいでしょう。
- [ASP.NET Core のルーティング](https://docs.microsoft.com/ja-jp/aspnet/core/fundamentals/routing)
- [ASP.NET Core Web API のコントローラー アクションの戻り値の型](https://docs.microsoft.com/ja-jp/aspnet/core/web-api/action-return-types)

## CORS
- NuGet で [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/) をインストールする
- Startup.cs で AddCors メソッドおよび UseCors メソッドを呼び出すことで機能を有効にする
  - AddMvc メソッドおよび UseMvc メソッドの前で呼び出す必要がある
  - コントローラー、アクションの単位では `[EnableCors]` を指定する

https://gist.github.com/sakapon/779fa14ee7e62682da43909b0fe0b39d

CORS が機能しているかどうかをテストするには、現在実行中のものとは異なるドメインを Origin ヘッダーに付加して API を呼び出します。応答に Access-Control-Allow-Origin ヘッダーが含まれていれば OK です。

要求ヘッダー
```
Origin: https://tempuri.org
```

応答ヘッダー
```
Access-Control-Allow-Origin: *
```

ツールとしては [Advanced REST client](https://chrome.google.com/webstore/detail/advanced-rest-client/hgmloofddffdnphfgcellkdfbfbjeloo) などを使えばよいでしょう。

![](https://github.com/sakapon/Samples-2018/blob/master/Images/AspNetCoreWebApiSample/CORS-ARC.png)

公式解説: [ASP.NET Core でのクロス オリジン要求 (CORS) を有効にする](https://docs.microsoft.com/ja-jp/aspnet/core/security/cors)

## ヘルプ ページ
コードの XML ドキュメントから、ユーザー向けのヘルプ ページを自動的に生成する機能です。  
ASP.NET Core では、OpenAPI (Swagger) の .NET 向け実装である Swashbuckle を利用します。

- NuGet で [Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore) をインストールする
- プロジェクトのプロパティで、XML ドキュメントの出力を有効にする
- Startup.cs で AddSwaggerGen メソッド、UseSwagger メソッドおよび UseSwaggerUI メソッドを呼び出すことで機能を有効にする

ソースコード: [Startup.cs](https://github.com/sakapon/Samples-2018/blob/master/AspNetCoreWebApiSample/SampleWebApi/Startup.cs)

- ヘルプ ページの URI は既定で `/swagger` となるが、ルートに変更するには、RoutePrefix を空文字列に設定する
- アクション メソッドの戻り値が IActionResult の場合、`[ProducesResponseType(200, Type = typeof(string))]` のように属性でデータの型を指定する
- このサンプルではアセンブリ情報の値をタイトルなどに設定している

![](https://github.com/sakapon/Samples-2018/blob/master/Images/AspNetCoreWebApiSample/Swagger-UI.png)

公式解説: [Swashbuckle と ASP.NET Core の概要](https://docs.microsoft.com/ja-jp/aspnet/core/tutorials/getting-started-with-swashbuckle)

## フォーマット
ASP.NET Core Web API では、既定でテキスト (text/plain) と JSON が有効になっています。  
テキストを無効にして XML を有効にするには、Startup.ConfigureServices メソッド内で次のようにします。

https://gist.github.com/sakapon/7e64aebcc538871e65c2f97c7ab88f69

![](https://github.com/sakapon/Samples-2018/blob/master/Images/AspNetCoreWebApiSample/Swagger-ContentType.png)

さらに、コントローラーまたはアクションに Produces 属性を指定することで、利用可能な Content-Type を制限することもできます。
```c#
[Produces("application/json", "application/xml")]
```

公式解説: [ASP.NET Core Web API の応答データの書式設定](https://docs.microsoft.com/ja-jp/aspnet/core/web-api/advanced/formatting)

前回: [dotnet コマンドによるビルド](../DotNet-Core/DotNet-Build.md)

### 作成したサンプル
- [AspNetCoreWebApiSample (GitHub)](https://github.com/sakapon/Samples-2018/tree/master/AspNetCoreWebApiSample)

### バージョン情報
- Microsoft.AspNetCore.All 2.0.8
- Microsoft.AspNetCore.Cors 2.0.3
- Swashbuckle.AspNetCore 2.5.0

### 参照
- [ASP.NET Core で Web API を構築する](https://docs.microsoft.com/ja-jp/aspnet/core/web-api/)
- [ASP.NET Web API の Tips (1)](ASPNET-WebAPI-Tips-1.md)
