## Project Settings (.NET Core Project Format)

### Project Property
- アプリケーション
  - アセンブリ名
  - 既定の名前空間
- ビルド
  - 詳細設定 - デバッグ情報: なし (Release のみ)
  - XML ドキュメント ファイル: `..\Release\Project1.xml` (Release のみ)

### Assembly Info (Project File)
For .exe
```
    <TargetFramework>net45</TargetFramework>
    <Version>1.0.0</Version>
    <AssemblyTitle>The Tool</AssemblyTitle>
    <Product>The Tools</Product>
    <Authors>Keiho Sakapon</Authors>
    <Copyright>© 2018 Keiho Sakapon</Copyright>
```

For ASP.NET
```
    <TargetFramework>netcoreapp2.0</TargetFramework>
    <Version>1.0.0</Version>
    <AssemblyTitle>The Tool</AssemblyTitle>
    <Product>The Tools</Product>
    <Description>A tool.</Description>
    <Authors>Keiho Sakapon</Authors>
    <Copyright>© 2018 Keiho Sakapon</Copyright>
```

For .dll, using TargetFrameworks
```
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
