# セキュリティ

### 記事
- [My docomoはパスワードのコピペを禁止するな](http://sho.tdiary.net/20121111.html)
- [IE10にはパスワード表示ボタンが付いている](http://tumblr.tokumaru.org/post/35538308213/ie10)
- [「Googleグループ」問題から考える“シャドーIT”の潜在的リスク](http://japan.zdnet.com/communication/analysis/35034735/)
- [そろそろ「ZIPで暗号化」の謎文化をなくしたい](http://d.hatena.ne.jp/teruyastar/20130623/1371978600)
- [「使ってはいけないパスワード」2012年版トップ25、「password」「123456」が上位譲らず](http://gigazine.net/news/20121029-worst-passwords-2012/)
- [パスワード認証を脆弱にする10の方法とアンチパターン](http://causeless.seesaa.net/article/388566941.html)

## 暗号化アルゴリズム
- ハッシュ (不可逆)
  - ソルト (鍵付きハッシュ)
  - HMAC-SHA 256
- 共通鍵 (対称) 暗号化
  - AES 256
- 公開鍵 (非対称) 暗号化
  - RSA 2048
- ハイブリッド暗号方式
  - SSL
  - [SSLの基本を押さえる](http://thinkit.co.jp/free/article/0706/3/6/)

### 記事
- [「パスワードでの保護は限界」と結論したGoogleが評価するセキュリティ技術](http://itpro.nikkeibp.co.jp/article/COLUMN/20130502/474661/)
- [YubiKey](http://331arc.net/2012/01/28/002101/)
- [“日本の標準暗号”が10年ぶり大改定](http://itpro.nikkeibp.co.jp/article/Watcher/20130426/474102/)
- [暗号も国際標準化の時代へ～政府・ISO/IEC・インターネット標準](http://www.atmarkit.co.jp/ait/articles/0604/07/news119.html)
- [世界初！60次元の格子暗号を解読](https://www.sci.kyushu-u.ac.jp/koho/qrinews/qrinews_161020.html)
- [シマンテック、ハッシュアルゴリズム「SHA-1」利用停止までのロードマップを解説](http://www.atmarkit.co.jp/ait/articles/1402/05/news117.html)
- [パスワード問合せシステムを作る](http://qiita.com/kawasima/items/ef75f317605ce800a839)

## アプリケーションにおける実装
### ユーザー管理
- 認証 (Authentication)
- 承認 (Authorization)

ASP.NET では、それぞれ Membership、Role として抽象化されており、具象クラス (Provider) を差し替えられる
- [ASP.NET SQL Server 登録ツール (Aspnet_regsql.exe)](http://msdn.microsoft.com/ja-jp/library/ms229862.aspx)
  - ソルトも組み込まれている

```
aspnet_regsql -E -S .\SQLExpress -d Test1 -A mr
```

- 認証 Cookie
  - ユーザー名や有効期限など (FormsAuthenticationTicket) を暗号化したもの
  - SSL で通信することが望ましい (つまり、実質すべてのページで SSL)

### OAuth
- [RFCとなった「OAuth 2.0」――その要点は？](http://www.atmarkit.co.jp/ait/articles/1209/10/news105.html)
- [OAuth.jp](http://oauth.jp/)

## 攻撃の種類
[エクスプロイト](https://t.co/X0E2xocfko)のページの下部の表にまとまっている

### クロスサイト
- クロスサイト スクリプティング (XSS)
  - セッション ハイジャックを招く
  - 対策：表示時にサニタイジング (エスケープ)
  - フレームワークで解決 (MVC 3 Razor)
- クロスサイト リクエスト フォージェリ (CSRF)
  - 対策：フォームにワンタイムトークンを設定する
  - AntiForgery ヘルパー (MVC 3 Razor)

### インジェクション
- SQL インジェクション
  - 対策：保存時にサニタイジング (エスケープ)
  - フレームワークで解決 (ADO.NET)
    - 組み込みストアド プロシージャを利用
    - パラメトライズド クエリ
- XML ファイルに保存する場合、適切にエスケープしないとデータ破損
  - フレームワークで解決

### Secure By Default
- 現代のフレームワークは Secure By Default
- ASP.NET や ADO.NET を利用する限り、XSS と SQL インジェクションに対しては自動で耐性を持つ
- Windows などの UAC もこの考え方

### 悪い対策例
- 固定の禁則文字・禁則ワード
  - sys, delete, ...
  - 記号も一般のユーザーが利用する文字である
  - 禁則ワードを含む会社名が存在する

### 記事
- [モテるセキュ女子力を磨くための4つの心得「SQLインジェクションができない女をアピールせよ」等](http://d.hatena.ne.jp/ockeghem/20110518/p1)
- [HTML5時代のWeb開発者が知らないとガチヤバな3つの未来予測と6つの脆弱性対策 (1/2)](http://www.atmarkit.co.jp/ait/articles/1309/05/news042.html)
- [ASP.NET のセキュリティ対策について考える](http://shiba-yan.hatenablog.jp/entry/20120526/1338001863)

### ネットワークキャプチャツール
- Wireshark
- Fiddler
- Network Monitor

### プロジェクト
- セキュリティは、専門性の高い知識である
- 要件定義の段階で具体的な実装方法まで決めてはならない

### 学習
- [体系的に学ぶ 安全なWebアプリケーションの作り方](http://www.amazon.co.jp/dp/4797361190)
- [暗号技術 ライブラリ](http://dev.sbins.co.jp/cryptography/04_menu.html)
