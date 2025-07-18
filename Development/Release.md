## How to Release

### Create a ZIP File (for .exe)
- Update **Release Notes** (Optional)
- Run `Zip Release`
- Save the ZIP file to `Downloads` or `zip` folder

### Create a NuGet Package (for .dll)
- Update **Release Notes**
- Run `NuGet Packup`
- Save the .nupkg file to `Published` or `pkg` folder

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
* .exe をリリースする場合、ZIP ファイルを添付してもよい

### Upload the NuGet Package
* Upload the created NuGet package to [NuGet Gallery](https://www.nuget.org/).

### Update Documents
* Update README.md.
* Update Wiki.
