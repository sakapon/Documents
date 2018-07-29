# .NET Core と .NET Standard
.NET Core の登場以降、Visual Studio で新しいプロジェクトを作成しようとすると、従来の .NET Framework のほかに .NET Core と .NET Standard 向けのプロジェクト テンプレートが現れます。

![NewProject](https://github.com/sakapon/Samples-2018/blob/master/Images/NetStandardSample/NewProject.png)

それぞれのプロジェクトにおけるアセンブリの参照可否をまとめると次のようになります。
- .NET Framework 向けプロジェクト
  - .NET Framework アセンブリを参照可能
  - .NET Core アセンブリを参照不可
  - .NET Standard アセンブリを参照可能
- .NET Core 向けプロジェクト
  - .NET Framework アセンブリを参照不可
  - .NET Core アセンブリを参照可能
  - .NET Standard アセンブリを参照可能

.NET Framework と .NET Core はランタイムが異なるため、両方に対応するクロスプラットフォームのクラス ライブラリを作成するには .NET Standard をターゲットにします。
なお、.NET Standard 2.0 アセンブリは、.NET Framework 4.6.1 以上で参照可能です。

.NET Core および .NET Standard のプロジェクト テンプレートには、次の特徴があります。
- .csproj ファイルの記述が簡略化されている
  - アセンブリ情報は .csproj ファイルに含まれ、AssemblyInfo.cs は不要
- NuGet パッケージを簡単に作成できる
  - ビルド時に作成する設定もできる

クラス ライブラリの .csproj ファイルの内容は次のようになっています。
```xml
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <TargetFramework>netstandard2.0</TargetFramework>
  </PropertyGroup>

</Project>
```

`TargetFramework` を `TargetFrameworks` に変更すれば、セミコロン区切りで対象のフレームワークを複数指定できます。
ここで指定する `netstandard2.0` や `net40` は、Target Framework Moniker と呼ばれます。
```xml
<TargetFrameworks>netstandard2.0;net40</TargetFrameworks>
```

これで複数のフレームワークを対象にしたアセンブリを一度にビルドできます。

![TargetFrameworks](https://github.com/sakapon/Samples-2018/blob/master/Images/NetStandardSample/TargetFrameworks.png)

.NET Framework 向けのみのアセンブリを作成したい場合であっても、.NET Core 向けのテンプレートから作成して `TargetFramework` を変更する方法が有効です。

また、`OutputType` に `Exe` を指定すればコンソール アプリになります。何も指定がなければクラス ライブラリです。
```xml
<OutputType>Exe</OutputType>
<TargetFrameworks>netcoreapp2.0;net45</TargetFrameworks>
```

コンソール アプリをビルドすると、.NET Framework 向けでは .exe が生成されますが、.NET Core 向けでは .dll となります。

![NetCoreConsole](https://github.com/sakapon/Samples-2018/blob/master/Images/NetStandardSample/NetCoreConsole.png)

.NET Core 向けコンソール アプリを実行するには、dotnet コマンドを実行します。
```
dotnet ConsoleApp1.dll
```

#### その他の注意点
- 例えば System.Security.Cryptography 名前空間は .NET Standard で利用可能ですが、.NET Framework と .NET Core ではクラス構成に差があります。
ビルドできても実行時にエラーとなることもあります (例えば `HashAlgorithm.Create` メソッド)。

### 作成したサンプル
- [NetStandardSample (GitHub)](https://github.com/sakapon/Samples-2018/tree/master/NetStandardSample)

### バージョン情報
- Visual Studio 2017
- .NET Core 2.0

### 参照
- [クロス プラットフォーム ツールによるライブラリの開発](https://docs.microsoft.com/ja-jp/dotnet/core/tutorials/libraries)
- [ターゲット フレームワーク](https://docs.microsoft.com/ja-jp/dotnet/standard/frameworks)
- [.NET Standardなライブラリプロジェクトを作成して参照する](https://www.buildinsider.net/language/dotnetcore/05)
- [dotnet run](https://docs.microsoft.com/ja-jp/dotnet/core/tools/dotnet-run)
