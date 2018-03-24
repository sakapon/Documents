# ASP.NET Web API の Tips (2)
ASP.NET Web API を利用する際の注意点や備忘録です。ほぼ箇条書きです。  
基本的な説明は省略しています。

## 例外処理
- 戻り値が HttpResponseMessage または IHttpActionResult の場合、Request.CreateErrorResponse メソッドなどで HttpResponseMessage を生成する
- 戻り値が HttpResponseMessage でも IHttpActionResult でもない場合、HttpResponseException を返すことで同様の効果が得られる
- HttpError を利用して、JSON のエラー メッセージを含めることができる

https://gist.github.com/sakapon/7641318950b9e61a4537ecc9feff397b

公式解説: [Exception Handling in ASP.NET Web API](https://docs.microsoft.com/en-us/aspnet/web-api/overview/error-handling/exception-handling)

## Help Page
コードの XML ドキュメントから、ユーザー向けのヘルプ ページを自動的に生成する機能です。  
Visual Studio でプロジェクトを作成するときに Web API を選択すると、Help Page もインストールされます。  
あとから追加するには、NuGet で [Microsoft.AspNet.WebApi.HelpPage](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.HelpPage/) をインストールします。

ただし、既定では機能が有効になっていません。
- 有効にする手順：
  - プロジェクトのプロパティで、XML ドキュメントの出力を有効にする
  - HelpPageConfig.cs のコメントアウトを解除する ( // を取る)

![SetDocumentationProvider](https://github.com/sakapon/Samples-2018/blob/master/Images/AspNetWebApiSample/SetDocumentationProvider.png)

- アクション メソッドの戻り値が HttpResponseMessage または IHttpActionResult の場合、[ResponseType(typeof(string))] のように属性でデータの型を指定する
- Areas\HelpPage にソースコードがあるため、カスタマイズ可能
- ヘルプ ページ (Help/Index) を既定のページに設定するには、HelpPageAreaRegistration.cs でルーティングの設定を追加するとよい
- ASP.NET Core Web API では、Help Page を使えない
  - Swashbuckle (Swagger の .NET 向け実装) を使う

![HelpPage](https://github.com/sakapon/Samples-2018/blob/master/Images/AspNetWebApiSample/HelpPage.png)

公式解説: [Creating Help Pages for ASP.NET Web API](https://docs.microsoft.com/en-us/aspnet/web-api/overview/getting-started-with-aspnet-web-api/creating-api-help-pages)

## Web API の呼び出し
.NET アプリケーションから Web API を呼び出すには、HttpClient クラスを利用するとよいでしょう。  
また、サービス側で実装されたカスタム データ型も、サービス コントラクトとして利用できます。
すなわち、応答メッセージに対して `response.Content.ReadAsAsync<T>()` を呼び出せば T 型としてデシリアライズできます。

https://gist.github.com/sakapon/7c81d432b2a886fbcf1dd4a6c3dbe108

公式解説: [Call a Web API From a .NET Client (C#)](https://docs.microsoft.com/en-us/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client)

## CORS
未検証。
- ASP.NET Web API CORS を利用する

公式解説: [Enabling Cross-Origin Requests in ASP.NET Web API 2](https://docs.microsoft.com/en-us/aspnet/web-api/overview/security/enabling-cross-origin-requests-in-web-api)

## JSONP
未検証。
- MediaTypeFormatter を利用する
  - WebApiContrib.Formatting.Jsonp など

## Entity Framework, OData
未検証。

公式解説: [Using Web API 2 with Entity Framework 6](https://docs.microsoft.com/en-us/aspnet/web-api/overview/data/using-web-api-with-entity-framework/)

## 認証
未検証。

前回: [ASP.NET Web API の Tips (1)](ASPNET-WebAPI-Tips-1.md)

### 作成したサンプル
- [AspNetWebApiSample (GitHub)](https://github.com/sakapon/Samples-2018/tree/master/AspNetWebApiSample)

### バージョン情報
- .NET Framework 4.5
- ASP.NET Web API 5.2.3
- ASP.NET MVC 5.2.3
- ASP.NET Web API Help Page 5.2.3
- ASP.NET Web API CORS 5.2.3

### 参照
- [ASP.NET Web API](https://docs.microsoft.com/en-us/aspnet/web-api/)
