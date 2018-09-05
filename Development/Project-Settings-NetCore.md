## Project Settings (.NET Core Project Format)

### Project Property
- アプリケーション
  - アセンブリ名
  - 既定の名前空間
- ビルド
  - 詳細設定 - デバッグ情報: なし (Release のみ)
  - XML ドキュメント ファイル: `Lib1.xml` (Release のみ)

### Assembly Info (Project File)
For .exe
```xml
    <TargetFramework>net45</TargetFramework>
    <Version>1.0.0</Version>
    <AssemblyTitle>The Tool</AssemblyTitle>
    <Product>KTools</Product>
    <Authors>Keiho Sakapon</Authors>
    <Copyright>© 2018 Keiho Sakapon</Copyright>
```

For ASP.NET
```xml
    <TargetFramework>netcoreapp2.0</TargetFramework>
    <Version>1.0.0</Version>
    <AssemblyTitle>The Web API</AssemblyTitle>
    <Product>The Web APIs</Product>
    <Description>Provides a Web API.</Description>
    <Authors>Keiho Sakapon</Authors>
    <Copyright>© 2018 Keiho Sakapon</Copyright>
```

For .dll, using TargetFrameworks
```xml
    <TargetFrameworks>netstandard2.0;net40</TargetFrameworks>
    <Version>1.0.0</Version>
    <Title>The Lib</Title>
    <AssemblyTitle>The Lib</AssemblyTitle>
    <Product>The Tools</Product>
    <Description>A tool.</Description>
    <Authors>Keiho Sakapon</Authors>
    <Copyright>© 2018 Keiho Sakapon</Copyright>
    <PackageProjectUrl>https://github.com/sakapon/KLibrary</PackageProjectUrl>
    <PackageLicenseUrl>https://github.com/sakapon/KLibrary/blob/master/LICENSE</PackageLicenseUrl>
    <PackageTags>Lib Sample</PackageTags>
    <PackageReleaseNotes>The first release.</PackageReleaseNotes>
```

### Tools
- [Build Release](https://github.com/sakapon/Build-Release)
