# URL エンコーディング
RFC 3986 では、文字は次のように分類されます。
- 非予約文字
  - エンコードしなくても利用できる文字
  - アルファベット、数字、および 4 種類の記号 `-._~`
- 予約文字
  - URI で意味を持つ記号
  - 18  種類の記号 `!#$&'()*+,/:;=?@[]`
- その他の文字
  - 11  種類の記号 ` "%<>\^``{|}`、その他のすべての文字 (日本語など)

### 参照
- [Percent-encoding](https://en.wikipedia.org/wiki/Percent-encoding)
- [URIで使用できる文字](http://www.asahi-net.or.jp/~ax2s-kmtn/ref/uric.html)
- [URLエンコードについておさらいしてみた](https://qiita.com/sisisin/items/3efeb9420cf77a48135d)
- [URLエンコード、URLデコードを行う](https://dobon.net/vb/dotnet/internet/urlencode.html)
