# .NET Core と .NET Standard
.NET Core が登場したことにより、Visual Studio で新しいプロジェクトを作成しようとすると、従来の .NET Framework のほかに .NET Core と .NET Standard 向けのプロジェクト テンプレートが現れます。

(図)

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
- NuGet パッケージを簡単な設定で作成できる
  - ビルド時に作成する設定もできる

クラス ライブラリの .csproj ファイルを開くと、
```xml
<TargetFramework>netstandard2.0</TargetFramework> 
```

のようになっています。  
`TargetFramework` を `TargetFrameworks` に変更すれば、セミコロン区切りで対象のフレームワークを複数指定できます。
ここで指定する `netstandard2.0` や `net40` は、Target Framework Moniker と呼ばれます。
```xml
<TargetFrameworks>netstandard2.0;net40</TargetFrameworks>
```

このように指定することで、複数のフレームワークを対象にしたアセンブリを一度にビルドできます。

(図)

.NET Framework 向けアセンブリを作成したい場合でも、.NET Core 向けのテンプレートから作成して `TargetFramework` を変更するのもよいでしょう。

また、`OutputType` に `Exe` を指定すればコンソール アプリになります。何も指定しなければクラス ライブラリです。
```xml
<OutputType>Exe</OutputType>
<TargetFrameworks>netcoreapp2.0;net45</TargetFrameworks>
```

コンソール アプリのビルドでは、.NET Framework では .exe が生成されますが、.NET Core では .dll となります。
.NET Core 向けコンソール アプリを実行するには、dotnet コマンドを実行します。
```
dotnet ConsoleApp1.dll
```

#### その他の注意点
- 例えば System.Security.Cryptography 名前空間は .NET Standard で利用可能ですが、.NET Framework と .NET Core ではクラス構成に差があります。
ビルドできても実行時にエラーとなることもあります。

### 作成したサンプル
- [NetStandardSample (GitHub)](https://github.com/sakapon/Samples-2018/tree/master/NetStandardSample)

### バージョン情報
- Visual Studio 2017
- .NET Core 2.0
