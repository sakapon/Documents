## Web API Protocol

### 要求
HTTP メソッドおよび引数の渡し方により場合分けする。
URL のパス (ルーティング) と組み合わせてもよい。

#### (1) GET (URL-encoded)
引数を URL のクエリ文字列で指定する。
```
Services/LogIn/user01?p=password
```

#### (2) GET (JSON)
引数を URL のクエリ文字列で指定する。
```
Services/LogIn?q={"u":"user01","p":"password"}
```

#### (3) POST, PUT, DELETE (URL-encoded)
引数を本文で指定する。
```
u=user01&p=password
Content-Type: application/x-www-form-urlencoded; charset=utf-8
```

#### (4) POST, PUT, DELETE (JSON)
引数を本文で指定する。
```
{"u":"user01","p":"password"}
Content-Type: application/json; charset=utf-8
```

### 応答
- Content-Type: application/json; charset=utf-8
- Content-Type: application/xml; charset=utf-8

### 実装方法

#### クライアント側
- Ajax
- WCF プロキシ
- [HttpClient](https://docs.microsoft.com/en-us/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client)

#### サーバー側
- (ASP.NET Web サービス)
- WCF サービス
- (ASP.NET MVC)
- ASP.NET Web API
