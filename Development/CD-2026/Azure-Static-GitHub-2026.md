# Azure Static Web Apps と GitHub で継続的デプロイ
Azure Static Web Apps (Azure 静的 Web アプリ) のコンテンツを、GitHub のリポジトリから継続的デプロイ (CD) するように構成する手順のメモです。

## リポジトリの作成
GitHub にリポジトリを新規作成し、トップページである `index.html` を追加します。  
リポジトリのルートにこれを配置してもよいのですが、今回はあえて `root-dir` フォルダーを作成し、その下に置いてみましょう。

![](https://github.com/sakapon/Documents/blob/master/Development/CD-2026/Images-ASG/Azure-Static-01.png)

## Azure Static Web Apps の作成
Azure ポータルで Azure Static Web Apps を新規作成します。  
名前やリソースグループなどを指定し、デプロイ元となるリポジトリを指定します。  
すると、リポジトリから自動的に `index.html` の場所が検出されます。  
この情報をもとに、GitHub Actions のワークフロー ファイル (YAML) が生成されることになります。

![](https://github.com/sakapon/Documents/blob/master/Development/CD-2026/Images-ASG/Azure-Static-11.png)

![](https://github.com/sakapon/Documents/blob/master/Development/CD-2026/Images-ASG/Azure-Static-12.png)

現状では、日本のリージョンは選択できないようなので、East Asia リージョンを選択します。

![](https://github.com/sakapon/Documents/blob/master/Development/CD-2026/Images-ASG/Azure-Static-13.png)

以上の情報で Azure Static Web Apps を作成します。

![](https://github.com/sakapon/Documents/blob/master/Development/CD-2026/Images-ASG/Azure-Static-21.png)

![](https://github.com/sakapon/Documents/blob/master/Development/CD-2026/Images-ASG/Azure-Static-22.png)

![](https://github.com/sakapon/Documents/blob/master/Development/CD-2026/Images-ASG/Azure-Static-31.png)

![](https://github.com/sakapon/Documents/blob/master/Development/CD-2026/Images-ASG/Azure-Static-32.png)

![](https://github.com/sakapon/Documents/blob/master/Development/CD-2026/Images-ASG/Azure-Static-23.png)

![](https://github.com/sakapon/Documents/blob/master/Development/CD-2026/Images-ASG/Azure-Static-33.png)

![](https://github.com/sakapon/Documents/blob/master/Development/CD-2026/Images-ASG/Azure-Static-41.png)

![](https://github.com/sakapon/Documents/blob/master/Development/CD-2026/Images-ASG/Azure-Static-42.png)

![](https://github.com/sakapon/Documents/blob/master/Development/CD-2026/Images-ASG/Azure-Static-43.png)
