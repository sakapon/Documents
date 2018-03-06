## How to Publish App by ClickOnce

デスクトップ アプリケーションを ClickOnce で発行するための手順を示します。

### 証明書の準備
署名に利用する証明書は、ルート証明書でもルート証明書でなくてもかまいません。  
ただし、.pfx であることが必要です (なりすましを防ぐためです)。  
.pfx を証明書ストアの個人 (My) にインストールしておきます。

### プロジェクトの設定

#### [署名]
証明書を指定する方法として、以下が挙げられます。

* 証明書ファイルの本体をプロジェクトに含める
  * [ファイルから選択]
  * パスワードで認証する
* 証明書ファイルの本体をプロジェクトに含めない
  * [ストアから選択]
  * 拇印 (Thumbprint) のみが含まれる
  * ファイルは別の場所で管理する

後者の方法が推奨されます。  
開発者と発行者が一致するとは限らないためです。

#### [発行]
* **インストール フォルダーの URL** 自動更新を有効にする場合は設定します。

#### [発行] - [更新]
自動更新を有効にする場合は設定します。

#### [発行] - [オプション]
* **発行者名** インストール時のルート フォルダーの名前となります。
* **スイート名** ルート フォルダーとアプリケーションの間のフォルダーの名前となります。省略可能です。
* **製品名** アプリケーションの名前となります。

### 発行
* ソリューション構成を [Release] に切り替えます。
* [今すぐ発行] をクリックします。

MSBuild で発行することもできます。

```
msbuild WpfApp1.csproj /p:Configuration=Release /t:Publish
```

### Git の設定
発行されたファイルを Git 経由で共有する場合は注意が必要です。  
(例えば、GitHub Pages でホストする場合など。)

Git は既定で、コミット時にテキスト ファイルの改行コードを LF に変換します。  
しかし、ClickOnce はインストール時にファイルのハッシュを検証するため、  
ファイルが変更されてしまうとエラーが発生します。

#### config
core.autoCRLF を false に設定します。

```
$ git config --global core.autocrlf false
```

設定を確認するためのコマンド:

```
$ git config --list
```

#### .gitattributes
config よりも、各リポジトリの .gitattributes が優先されます。  
Visual Studio が生成する .gitattributes を次のように編集します。

```
text=false
```

### 参照
[連載 ClickOnceの真実：第7回 ClickOnceが持つセキュリティ機構とは？](http://www.atmarkit.co.jp/ait/articles/0612/02/news015.html)

[Windowsでgitを使う場合の改行コード自動変換がうざい](http://www.seeds-std.co.jp/seedsblog/2551.html)  
[git での改行コード](http://qiita.com/shuhei/items/2da839de8803cb335f86)  
[Gitのcore.autocrlfについて](http://hack.aipo.com/archives/5841/)  
[WindowsのGitクライアントSourceTreeのインストールと初期設定](http://www.karakaram.com/windows-git-source-tree)

### その他

#### Mage.exe
手動で発行するには、Mage.exe または MageUI.exe を利用します。

[Mage.exe (マニフェストの生成および編集ツール)](https://msdn.microsoft.com/ja-jp/library/acz3y3te.aspx)  
[チュートリアル : ClickOnce アプリケーションを手動で配置する](https://msdn.microsoft.com/ja-jp/library/xc3tc5xx.aspx)  
[アプリケーションの発行後に、アプリケーション設定ファイルの内容を編集するには](http://blogs.msdn.com/b/jpvsblog/archive/2011/05/26/clickonce-tips.aspx)  
[ClickOnceの配布手順がよくわからなかったのでメモ](http://d.hatena.ne.jp/masakitk/20110111/1294751018)  
[csc.exeでClickOnceアプリケーションをコンパイルし、mage.exeで配置する](http://symfoware.blog68.fc2.com/blog-entry-1135.html)
