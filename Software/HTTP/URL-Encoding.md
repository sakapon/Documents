# URL エンコーディング
URL エンコーディングの定義と、それを扱うための .NET Framework のライブラリを検証しました。

## パーセント エンコーディングと URL エンコーディング
パーセント エンコーディングとは、文字列を UTF-8 でエンコードし、各バイトをパーセント記号 `%` とその 16 進数を用いて表すことです。例えば、`/` は `%2F` に、`あ` は `%E3%81%82` に変換されます。  
URL エンコーディングとは、URI の中で使われている記号と混在しないように一部の文字列をパーセント エンコーディングにより変換することです。  
両者の言葉を区別せずに使うこともあります。

## URL エンコーディング
RFC 3986 では、文字は次のように分類されます。
- 非予約文字
  - エンコードしなくても利用できる文字
  - アルファベット、数字、および 4 種類の記号 `-._~`
- 予約文字
  - URI で意味を持つ記号
  - 18  種類の記号 `!#$&'()*+,/:;=?@[]`
- その他の文字
  - エンコードが必要な文字
  - 11  種類の記号 ``" %<>\^`{|}``、その他のすべての文字 (日本語など)

URL エンコーディングは、主に次の 2 通りで利用されます。
- URI の各セグメント (クエリ文字列を除く)
  - `https://tempuri.org/messages/Hello%20World%21` の `messages` や `Hello%20World%21` の部分
  - 非予約文字以外をパーセント エンコーディング
    - ただし、Web フレームワーク個別の仕様により、パーセント エンコーディングしても使用を制限されることがある
- URI のクエリ文字列や、POST などで送信するときの本文 (フォーム)
  - `key=value&message=Hello+World%21` の `key` や `Hello+World%21` の部分
  - 非予約文字以外をパーセント エンコーディングし、さらに `%20` (スペース) を `+` に変換
  - MIME タイプ `application/x-www-form-urlencoded` と定義されている

## .NET Framework のライブラリ
.NET Framework では、URL エンコーディングのために次の方法が用意されています。
- System.Uri.EscapeDataString メソッド
  - RFC 3986 に従って非予約文字以外をパーセント エンコーディング
- System.Uri.EscapeUriString メソッド
  - RFC 3986 に従って非予約文字・予約文字以外をパーセント エンコーディング
  - 既に全体が URI の形式になっているときに利用する
    - クエリ文字列も同様の規則で変換される。`application/x-www-form-urlencoded` には変換されない
- System.Uri インスタンスの AbsoluteUri プロパティ
  - 基本的に Uri.EscapeUriString メソッドと同じだが、下記の点が異なる
  - `%XX` の形式になっているかどうかで扱いが異なる
    - `https://tempuri.org/%2` は `https://tempuri.org/%252` に
    - `https://tempuri.org/%25` は `https://tempuri.org/%25` のまま
  - クエリ文字列でない部分の `\` は `/` に変換される
- System.Net.WebUtility.UrlEncode メソッド
  - RFC 2396 (旧版) に近い仕様で非予約文字以外をパーセント エンコーディングし、さらに `%20` (スペース) を `+` に変換
- System.Web.HttpUtility.UrlEncode メソッド
  - System.Net.WebUtility.UrlEncode メソッドと同じだが、小文字になる
- System.Net.Http.FormUrlEncodedContent クラス
  - key-value データをまとめて `application/x-www-form-urlencoded` に変換

![](https://github.com/sakapon/Samples-2018/blob/master/Images/ConversionSample/Uri.AbsoluteUri.png)

.NET では [System.Uri.EscapeDataString メソッド](https://msdn.microsoft.com/ja-jp/library/system.uri.escapedatastring.aspx)、[System.Uri.EscapeUriString メソッド](https://msdn.microsoft.com/ja-jp/library/system.uri.escapeuristring.aspx)、[System.Net.Http.FormUrlEncodedContent クラス](https://msdn.microsoft.com/ja-jp/library/system.net.http.formurlencodedcontent(v=vs.110).aspx)を使えばよいでしょう。

アプリケーションから HTTP 接続をするために [System.Net.Http.HttpClient クラス](https://msdn.microsoft.com/ja-jp/library/system.net.http.httpclient(v=vs.110).aspx)を使うことが多いと思いますが、接続先の URI を string で渡しても、HttpClient の内部では Uri インスタンスで扱われます。
したがって、URI を HttpClient に渡す前に、セグメントもクエリ文字列も URL エンコーディングしておくのがよさそうです。

https://gist.github.com/sakapon/d0d1f80395740c2488a57d812588e9c0

### 作成したサンプル
- [ConversionSample (GitHub)](https://github.com/sakapon/Samples-2018/blob/master/ConversionSample/UnitTest/UriTest.cs)
- [AspNetWebApiSample (GitHub)](https://github.com/sakapon/Samples-2018/blob/master/AspNetWebApiSample/UnitTest/Client/UriQueryTest.cs)

### バージョン情報
- .NET Framework 4.5

### 参照
- [Percent-encoding](https://en.wikipedia.org/wiki/Percent-encoding)
- [URIで使用できる文字](http://www.asahi-net.or.jp/~ax2s-kmtn/ref/uric.html)
- [URLエンコードについておさらいしてみた](https://qiita.com/sisisin/items/3efeb9420cf77a48135d)
- [application/x-www-form-urlencoded](https://wiki.suikawiki.org/n/application%2Fx-www-form-urlencoded)
- [URLエンコード、URLデコードを行う](https://dobon.net/vb/dotnet/internet/urlencode.html)
