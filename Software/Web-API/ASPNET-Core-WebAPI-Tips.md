# ASP.NET Core Web API の Tips
ASP.NET Core で Web API を利用する際の注意点や備忘録です。ほぼ箇条書きです。  
以前に書いた [ASP.NET Web API](ASPNET-WebAPI-Tips-1.md) と細かい差異はありますが、コントローラー作成などの基本的な説明は省略しています。

### CORS
- NuGet で Microsoft.AspNetCore.Cors をインストールする
- Startup.cs で AddCors メソッドおよび UseCors メソッドを呼び出すことで機能を有効にする
  - AddMvc メソッドおよび UseMvc メソッドの前で呼び出す必要がある
  - コントローラー、アクションの単位では `[EnableCors]` を指定する

https://gist.github.com/sakapon/779fa14ee7e62682da43909b0fe0b39d

CORS が機能しているかどうかをテストするには、現在実行中のものとは異なるドメインを Origin ヘッダーに付加して API を呼び出します。応答に Access-Control-Allow-Origin ヘッダーが含まれていれば OK です。  
ツールとしては [Advanced REST client](https://chrome.google.com/webstore/detail/advanced-rest-client/hgmloofddffdnphfgcellkdfbfbjeloo) などを使えばよいでしょう。

要求ヘッダー
```
Origin: https://tempuri.org
```

応答ヘッダー
```
Access-Control-Allow-Origin: *
```

公式解説: [ASP.NET Core でのクロス オリジン要求 (CORS) を有効にする](https://docs.microsoft.com/ja-jp/aspnet/core/security/cors)

### Swagger (Swashbuckle)
TBD

### バージョン情報
- Microsoft.AspNetCore.All 2.0.8
- Microsoft.AspNetCore.Cors 2.0.3
- Swashbuckle.AspNetCore 2.5.0

### 参照
- [ASP.NET Core でのクロス オリジン要求 (CORS) を有効にする](https://docs.microsoft.com/ja-jp/aspnet/core/security/cors)
- [Swashbuckle と ASP.NET Core の概要](https://docs.microsoft.com/ja-jp/aspnet/core/tutorials/getting-started-with-swashbuckle)
