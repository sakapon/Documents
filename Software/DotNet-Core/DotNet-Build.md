# dotnet コマンドによるビルド
前回の [.NET Core と .NET Standard](DotNet-Core-Standard.md) に引き続き、今回はコマンドラインでアプリやライブラリをビルドする方法を検証しました。  
まず、ビルドに関連する dotnet コマンドの一覧を挙げます。  
基本的にはプロジェクト フォルダー上で実行しますが、build や pack などは ソリューション フォルダー上でも実行できます。

- dotnet restore
  - NuGet 参照を解決する
- dotnet build
  - MSBuild.exe を実行する
  - 内部で restore する (ソースコードしかない状態でも実行できる)
- dotnet msbuild
  - MSBuild.exe と同じ引数を指定する
  - 内部で restore しない (ソースコードしかない状態では失敗)
- dotnet publish
  - publish フォルダーに発行する
  - 内部で build する (ソースコードしかない状態でも実行できる)
- dotnet pack
  - NuGet パッケージを作成する
  - 参照先の DLL は含まれず、依存関係が設定される
  - 内部で build しない (ソースコードしかない状態では失敗)
- dotnet clean
  - 前回のビルド結果を消去する
  - restore の結果は残る
- dotnet run
  - ソースコードからアプリを実行する
  - 内部で build する
- dotnet App1.dll
  - ビルド済みのアプリを実行する

以下、詳細について記述していきます。

### dotnet msbuild と msbuild
dotnet msbuild と msbuild の動作は同じです。
```
dotnet msbuild /p:Configuration=Release /t:Rebuild
msbuild /p:Configuration=Release /t:Rebuild
```

ただし、msbuild は環境変数の `PATH` に設定されていないため、cmd や PowerShell で実行するにはそのパスを指定しなければなりませんが、dotnet は `PATH` に設定されているため cmd や PowerShell でそのまま実行できて便利です。

### アセンブリのビルド・発行
リビルドするには `--no-incremental` を指定します。
```
dotnet build -c Release --no-incremental
```

ただし build では、.NET Core を対象とする場合、NuGet 参照の DLL がコピーされません。  
build では開発環境が想定されており、.dev.json ファイルに NuGet 参照が記述されます。
(.NET Framework を対象とする場合は NuGet 参照の DLL もコピーされます。)

配置用にすべての DLL を含めるには publish を使います。  
プロジェクトに対象のフレームワークが複数ある場合、`-f` で一つだけ指定します。
```
dotnet clean -c Release
dotnet publish -c Release -f netcoreapp2.0
```

なお、publish 単独ではリビルドができないため、先に clean を実行しています。

### NuGet パッケージ作成
出力先のディレクトリを変更するには `-o` を指定します。
```
dotnet pack -c Release -o pkg
```

または、
```
dotnet msbuild /p:Configuration=Release /t:pack
```

`構築時に NuGet パッケージを生成する` (.csproj では `GeneratePackageOnBuild`) を設定して build する方法もあります。
```
dotnet build -c Release --no-incremental
```

![GeneratePackageOnBuild](https://github.com/sakapon/Samples-2018/blob/master/Images/NetStandardSample/GeneratePackageOnBuild.png)

前回: [.NET Core と .NET Standard](DotNet-Core-Standard.md)

### 作成したサンプル
- [NetStandardSample (GitHub)](https://github.com/sakapon/Samples-2018/tree/master/NetStandardSample)

### バージョン情報
- .NET Core 2.0

### 参照
- [dotnet build コマンド](https://docs.microsoft.com/ja-jp/dotnet/core/tools/dotnet-build)
- [dotnet publish コマンド](https://docs.microsoft.com/ja-jp/dotnet/core/tools/dotnet-publish)
- [dotnet pack コマンド](https://docs.microsoft.com/ja-jp/dotnet/core/tools/dotnet-pack)
- [.NET Coreでコンソールアプリを配置する](https://www.buildinsider.net/language/dotnetcore/04)
