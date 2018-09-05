## Project Settings (.NET Core Project Format)

### Project Property
- アプリケーション
  - アセンブリ名
  - 既定の名前空間
- ビルド
  - 詳細設定 - デバッグ情報: なし (Release のみ)
  - XML ドキュメント ファイル: `..\Release\Project1.xml` (Release のみ)

### Assembly Info (Project File)
TargetFramework
```
    <TargetFramework>netcoreapp2.0</TargetFramework>
```

or, TargetFrameworks
```
    <TargetFrameworks>netstandard2.0;net40</TargetFrameworks>
```

Assembly Info
```
    <Version>1.0.0</Version>
    <AssemblyTitle>The Tool</AssemblyTitle>
    <Product>The Tools</Product>
    <Description>A tool.</Description>
    <Authors>Keiho Sakapon</Authors>
    <Copyright>© 2018 Keiho Sakapon</Copyright>
```

### Tools
- [Build Release](https://github.com/sakapon/Build-Release)
