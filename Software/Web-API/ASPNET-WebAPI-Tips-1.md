# ASP.NET Web API の Tips (1)
ASP.NET Web API を利用する際の注意点や備忘録です。ほぼ箇条書きです。  
基本的な説明は省略しています。

## ルーティング
WebApiConfig.cs にルーティングの設定が記述されています。  
既定のテンプレートは `api/{controller}/{id}` となっており、REST スタイルを想定したものとなっています。  
ただし、ASP.NET MVC と同様に `{action}` も利用可能であり、RPC スタイルの API も構成できます。

- [RoutePrefix], [Route], [ActionName] などの属性を利用することで柔軟に構成できる
- [Route] を複数設定できる

公式解説: [Web API Routing](https://docs.microsoft.com/en-us/aspnet/web-api/overview/web-api-routing-and-actions/)

## HTTP メソッド
- 主に REST スタイルの場合、Get、Post などのメソッド名で解決される (CoC)。この場合、[HttpGet] などの属性を指定する必要はない
- 主に RPC スタイルで任意のアクション名を利用するには、[HttpGet] などの属性を指定する

## 引数
- DateTime, Guid などの型も扱える
- 引数の [FromBody] は、エンティティ型なら付ける必要はない
- ルーティングで `{i:int:range(0,100)}` のような制約を追加できる
- 引数に既定値を指定すると、URL のパラメーターを省略可能
  - ルーティングで指定する場合は `{i:int?}` のようにする
  - range などを使う場合、省略可能にできない
    - int? と range は同時に指定できない
- 引数で / を使う場合、引数名の前に * を指定する (下の DateTime 型の例)

https://gist.github.com/sakapon/1e5d10ca0b5b7a5435ba2a8c52072348

公式解説: [Attribute Routing in ASP.NET Web API 2](https://docs.microsoft.com/en-us/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2)

## 戻り値
戻り値として、通常のデータ型以外に次のものを指定できます。
- HttpResponseMessage
  - 生の応答データ
- IHttpActionResult
  - HttpResponseMessage をラップしたインターフェイス

これらを利用することにより、応答データを柔軟に設定できます。
- JSON や XML に限らず、テキストや画像などの任意の形式のコンテンツを返せる
- 主な IHttpActionResult の実装は [System.Web.Http.Results 名前空間](https://msdn.microsoft.com/library/system.web.http.results.aspx)で定義されている

以下は、テキスト (Content-Type: text/plain) を返す例です。

https://gist.github.com/sakapon/8d0b561449af2d8103b1ee374b44376a

公式解説: [Action Results in Web API 2](https://docs.microsoft.com/en-us/aspnet/web-api/overview/getting-started-with-aspnet-web-api/action-results)

次回: [ASP.NET Web API の Tips (2)](ASPNET-WebAPI-Tips-2.md)

### 作成したサンプル
- [AspNetWebApiSample (GitHub)](https://github.com/sakapon/Samples-2018/tree/master/AspNetWebApiSample)

### バージョン情報
- .NET Framework 4.5
- ASP.NET Web API 5.2.3

### 参照
- [ASP.NET Web API](https://docs.microsoft.com/en-us/aspnet/web-api/)
