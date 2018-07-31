# dotnet コマンドによるビルド
前回の [.NET Core と .NET Standard](DotNet-Core-Standard.md) に引き続き、コマンドラインでアプリやライブラリをビルドする方法を検証してみました。  
まず、ビルドに関連する dotnet コマンドの一覧を挙げます。

- dotnet restore: NuGet 参照を読み込む
- dotnet build
 	- 内部で restore する (ソースコードしかない状態でも実行できる)
- dotnet msbuild
 	- 内部で restore しない (ソースコードしかない状態では失敗)
- dotnet publish
  - 内部で build する (ソースコードしかない状態でも実行できる)
  - publish フォルダーに発行される
  - `-f` で対象のフレームワークを指定する (複数ある場合)
- dotnet pack
 	- 内部で build しない (ソースコードしかない状態では失敗)
- dotnet clean
  - restore の結果は残る
- dotnet App1.dll: ビルド済みのアプリを実行
- dotnet run
  - 内部で build する (ソースコードしかない状態でも実行できる)

### 作成したサンプル
- [NetStandardSample (GitHub)](https://github.com/sakapon/Samples-2018/tree/master/NetStandardSample)

### バージョン情報
- .NET Core 2.0

### 参照
- [dotnet build コマンド](https://docs.microsoft.com/ja-jp/dotnet/core/tools/dotnet-build)
- [dotnet pack コマンド](https://docs.microsoft.com/ja-jp/dotnet/core/tools/dotnet-pack)
- [.NET Coreでコンソールアプリを配置する](https://www.buildinsider.net/language/dotnetcore/04)
