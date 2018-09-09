## How to Release

### Create a ZIP File (for .exe)
- Run `Zip Release`
- Save the ZIP file to `Downloads` or `zip` folder

### Create a NuGet Package (for .dll)
- Run CreatePackage.ps1

#### In Case of Prerelease Version
例えば、1.0.3.0 を 1.0.3-alpha としてリリースする場合。

* .nupkg ファイルの名前を変更する
* .nupkg ファイルを開き、**Version** を変更する

### Pull Request
* **Title** 「vX.Y.Z」と入力する
* **Comment** Release Notes の内容を転記する

### Merge
* そのまま [Merge pull request] をクリックする

### Release
* **Tag** 「vX.Y.Z」と入力する
* **Title** 「vX.Y.Z」と入力する
* **Description** Release Notes の内容を転記する

### Upload the NuGet Package
* Upload the created NuGet package to [NuGet Gallery](https://www.nuget.org/).

### Update Documents
* Update README.md.
* Update Wiki.
