# Azure Static Web Apps と GitHub で継続的デプロイ
Azure Static Web Apps (Azure 静的 Web アプリ) のコンテンツを、  
GitHub のリポジトリから継続的デプロイ (CD) するように構成する手順のメモです。

## リポジトリの作成
GitHub にリポジトリを新規作成し、トップページである `index.html` を追加します。  
リポジトリのルートにこれを配置してもよいのですが、今回はあえて `root-dir` フォルダーを作成し、その下に置いてみましょう。

![](https://raw.githubusercontent.com/sakapon/Documents/refs/heads/master/Development/CD-2026/Images-ASG/Azure-Static-01.png)

## Azure Static Web Apps の作成
Azure ポータルで Azure Static Web Apps を新規作成します。  
名前やリソースグループなどを指定し、デプロイ元となるリポジトリを指定します。  
すると、リポジトリから自動的に `index.html` の場所が検出されます。  
この情報をもとに、継続的デプロイのための GitHub Actions ワークフロー ファイル (YAML) が生成されることになります。

![](https://raw.githubusercontent.com/sakapon/Documents/refs/heads/master/Development/CD-2026/Images-ASG/Azure-Static-11.png)

![](https://raw.githubusercontent.com/sakapon/Documents/refs/heads/master/Development/CD-2026/Images-ASG/Azure-Static-12.png)

現状では、日本のリージョンは選択できないようなので、East Asia リージョンを選択します。

![](https://raw.githubusercontent.com/sakapon/Documents/refs/heads/master/Development/CD-2026/Images-ASG/Azure-Static-13.png)

以上の情報で Azure Static Web Apps を作成します。

![](https://raw.githubusercontent.com/sakapon/Documents/refs/heads/master/Development/CD-2026/Images-ASG/Azure-Static-21.png)

![](https://raw.githubusercontent.com/sakapon/Documents/refs/heads/master/Development/CD-2026/Images-ASG/Azure-Static-22.png)

これで Web アプリが作られました。  
このとき、GitHub のリポジトリでは次のものが保存されます：

- GitHub Actions ワークフロー ファイル (YAML)
  - `index.html` のパスが `app_location` 変数に設定されます
- Azure にデプロイするためのシークレット

![](https://raw.githubusercontent.com/sakapon/Documents/refs/heads/master/Development/CD-2026/Images-ASG/Azure-Static-31.png)

![](https://raw.githubusercontent.com/sakapon/Documents/refs/heads/master/Development/CD-2026/Images-ASG/Azure-Static-32.png)

さらに、ワークフロー ファイルがコミットされたことがトリガーとなり、GitHub Actions が開始され、初回デプロイが実行されます。

![](https://raw.githubusercontent.com/sakapon/Documents/refs/heads/master/Development/CD-2026/Images-ASG/Azure-Static-23.png)

なお、別の分岐 (ブランチ) からプルリクエストを出すとプレビュー環境が作られ、Web アプリをテストできます。  
そのプルリクエストがマージされると、運用環境にデプロイされます。これは便利な機能だと思います。

Azure Static Web Apps では、指定した名前が URL に使われるわけではありません。  
`abcde-vwxyz.0.azurestaticapps.net` のようなサブドメインがランダムで割り当てられる、ということに注意が必要です。  
したがって、次回の記事で示すように、カスタムドメインで運用するのがよいでしょう。

![](https://raw.githubusercontent.com/sakapon/Documents/refs/heads/master/Development/CD-2026/Images-ASG/Azure-Static-33.png)

**注意:** リポジトリに `index.html` が存在しない場合、ワークフロー ファイルは作成されますが、  
次のエラー メッセージとともにデプロイが失敗します。  
この後でも `index.html` を追加すれば継続的デプロイは成功します。

```
Failed to find a default file in the app artifacts folder (/). Valid default files: index.html,Index.html.
If your application contains purely static content, please verify that the variable 'app_location' in your workflow file points to the root of your application.
```

## 継続的デプロイ (CD)
リポジトリでコンテンツを変更すると、GitHub Actions のワークフローが開始され、  
新しいバージョンが Azure Static Web Apps にデプロイされます。

![](https://raw.githubusercontent.com/sakapon/Documents/refs/heads/master/Development/CD-2026/Images-ASG/Azure-Static-41.png)

![](https://raw.githubusercontent.com/sakapon/Documents/refs/heads/master/Development/CD-2026/Images-ASG/Azure-Static-42.png)

![](https://raw.githubusercontent.com/sakapon/Documents/refs/heads/master/Development/CD-2026/Images-ASG/Azure-Static-43.png)

**注意:** Azure Static Web Apps を削除しても、GitHub リポジトリのワークフロー ファイルおよびシークレットは削除されません。  
この状態でリポジトリが変更されると、デプロイしようとして失敗します。

## 参照
- [クイック スタート: 静的 Web アプリを初めてビルドする](https://learn.microsoft.com/ja-jp/azure/static-web-apps/get-started-portal)
