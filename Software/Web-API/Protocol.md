# Web API Protocol

## アーキテクチャ
### REST スタイル
データベース操作の概念に近い。

```
URI: api/users/1
```

- GET: 取得
- POST: 挿入
- PUT: 更新
- DELETE: 削除

POST, PUT, DELETE の場合、送信するエンティティ データの中に ID があれば URI の ID は必要ないが、通常は指定する。

`GET api/users` で全件を取得する。

### RPC スタイル
メソッドの概念に近い。
- GET
  - サービス側の状態を変えない操作
  - 値を取得する
- POST
  - サービス側の状態を変える操作

## 要求
ヘッダー
- Accept: application/json
- Accept: application/xml

HTTP メソッドおよび引数の渡し方により場合分けする。
URL のパス (ルーティング) と組み合わせてもよい。

### (1) GET (URL-encoded)
引数を URL のクエリ文字列で指定する。
```
Services/LogIn/user01?p=password
```

### (2) GET (JSON)
引数を URL のクエリ文字列で指定する。
```
Services/LogIn?q={"u":"user01","p":"password"}
```

### (3) POST, PUT, DELETE (URL-encoded)
引数を本文で指定する。
```
u=user01&p=password
Content-Type: application/x-www-form-urlencoded; charset=utf-8
```

### (4) POST, PUT, DELETE (JSON)
引数を本文で指定する。
```
{"u":"user01","p":"password"}
Content-Type: application/json; charset=utf-8
```

## 応答
ヘッダー
- Content-Type: application/json; charset=utf-8
- Content-Type: application/xml; charset=utf-8

## 実装方法
### サーバー側
- (ASP.NET Web サービス)
- WCF サービス
- (ASP.NET MVC)
- [ASP.NET Web API](https://docs.microsoft.com/en-us/aspnet/web-api/)

### クライアント側
- Ajax
- WCF プロキシ
- [HttpClient](https://docs.microsoft.com/en-us/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client)
