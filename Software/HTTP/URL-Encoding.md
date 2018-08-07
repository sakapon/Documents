# URL エンコーディング

## パーセント エンコーディングと URL エンコーディング
パーセント エンコーディングとは、文字をパーセント記号 `%` と UTF-8 でエンコードしたときの 16 進数を用いて表すことです。例えば、`/` は `%2F` に、`あ` は `%E3%81%82` に変換されます。  
URL エンコーディングとは、URI の中で使われている記号と混在しないように一部の文字列をパーセント エンコーディングにより変換することです。  
両者の言葉を区別せずに使うこともあります。

RFC 3986 では、文字は次のように分類されます。
- 非予約文字
  - エンコードしなくても利用できる文字
  - アルファベット、数字、および 4 種類の記号 `-._~`
- 予約文字
  - URI で意味を持つ記号
  - 18  種類の記号 `!#$&'()*+,/:;=?@[]`
- その他の文字
  - エンコードが必要な文字
  - 11  種類の記号 `` "%<>\^`{|}``、その他のすべての文字 (日本語など)

URL エンコーディングは、主に次の 2 通りで利用されます。
- URI の各セグメント (クエリ文字列を除く)
  - `https://tempuri.org/messages/Hello%20World%21` の `messages` や `Hello%20World%21` の部分
  - 非予約文字以外をパーセント エンコーディング
    - ただし、Web フレームワーク個別の仕様により、パーセント エンコーディングしても使用を制限されることがある
- URI のクエリ文字列や、POST などで送信するときの本文 (フォーム)
  - `key=value&message=Hello+World%21` の `value` や `Hello+World%21` の部分
  - 非予約文字以外をパーセント エンコーディングし、さらに `%20` (スペース) を `+` に変換
  - MIME タイプ `application/x-www-form-urlencoded` と呼ばれる

### 作成したサンプル
- [ConversionSample (GitHub)](https://github.com/sakapon/Samples-2018/blob/master/ConversionSample/UnitTest/UriTest.cs)
- [AspNetWebApiSample (GitHub)](https://github.com/sakapon/Samples-2018/blob/master/AspNetWebApiSample/UnitTest/Client/UriQueryTest.cs)

### バージョン情報
- .NET Framework 4.5

### 参照
- [Percent-encoding](https://en.wikipedia.org/wiki/Percent-encoding)
- [URIで使用できる文字](http://www.asahi-net.or.jp/~ax2s-kmtn/ref/uric.html)
- [URLエンコードについておさらいしてみた](https://qiita.com/sisisin/items/3efeb9420cf77a48135d)
- [URLエンコード、URLデコードを行う](https://dobon.net/vb/dotnet/internet/urlencode.html)
