# ASP.NET Web API の Tips (2)
ASP.NET Web API を利用する際の注意点や備忘録です。ほぼ箇条書きです。  
基本的な説明は省略しています。

## 例外処理
- 戻り値が HttpResponseMessage または IHttpActionResult の場合、Request.CreateErrorResponse メソッドなどで HttpResponseMessage を生成する
- 戻り値が HttpResponseMessage でも IHttpActionResult でもない場合、HttpResponseException を返すことで同様の効果が得られる
- HttpError を利用して、JSON のエラー メッセージを含めることができる

https://gist.github.com/sakapon/7641318950b9e61a4537ecc9feff397b

## Help Page
コードの XML ドキュメントから、ユーザー向けのヘルプ ページを自動的に生成する機能です。  
Visual Studio でプロジェクトを作成するときに Web API を選択すると、Help Page もインストールされます。
あとから追加するには、NuGet で [Microsoft.AspNet.WebApi.HelpPage](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.HelpPage/) をインストールします。

ただし、既定では機能が有効になっていません。
- 有効にする手順：
  - プロジェクトのプロパティで、XML ドキュメントの出力を有効にする
  - HelpPageConfig.cs のコメントアウトを解除する ( // を取る)

(画像)

- アクション メソッドの戻り値が HttpResponseMessage や IHttpActionResult の場合、[ResponseType(typeof(string))] のようにデータの型を指定する
- Areas/HelpPage にソースコードがあるため、カスタマイズ可能
- ヘルプ ページ (Help/Index) を既定のページに設定するには、HelpPageAreaRegistration.cs でルーティングの設定を追加するとよい
- ASP.NET Core Web API では、Help Page を使えない
  - Swashbuckle (Swagger の .NET 向け実装) を使う

## Entity Framework, OData
未検証。

## 認証
未検証。

前回: [ASP.NET Web API の Tips (1)](ASPNET-WebAPI-Tips-1.md)

### 作成したサンプル
- [AspNetWebApiSample (GitHub)](https://github.com/sakapon/Samples-2018/tree/master/AspNetWebApiSample)

### バージョン情報
- .NET Framework 4.5
- ASP.NET Web API 5.2.3
- ASP.NET Web API Help Page 5.2.3
- ASP.NET Web API CORS 5.2.3

### 参照
- [ASP.NET Web API](https://docs.microsoft.com/en-us/aspnet/web-api/)
