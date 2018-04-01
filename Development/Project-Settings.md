## Project Settings

### プロジェクトのプロパティ
- アプリケーション
  - [アセンブリ名] を設定
  - [既定の名前空間] を設定
- ビルド
  - 条件付きコンパイル シンボル: SILVERLIGHT (Silverlight の場合)
  - 条件付きコンパイル シンボル: SILVERLIGHT;WINDOWS_PHONE (Windows Phone の場合)
  - **[詳細設定] → [デバッグ情報] : none (Release のみ)**
  - [出力パス] : `..\Release\Current\` (Release のみ)
  - [XML ドキュメント ファイル] : `..\Release\Current\Project1.xml` (Release のみ)
- デバッグ
  - [Visual Studio ホスティング プロセスを有効にする] : オフ (Release のみ)

### アセンブリ情報
- (SharedAssemblyInfo.cs)
  - プロジェクトにリンクとして追加
- AssemblyInfo.cs
  - AssemblyTitle
  - AssemblyCompany
  - AssemblyProduct
  - AssemblyCopyright
  - AssemblyFileVersion: 1.0.0
  - `[assembly: CLSCompliant(true)]` を追加

### ツール
- [ZIP Release](https://www.nuget.org/packages/KTools.ZipRelease/) または [Version Increment](https://www.nuget.org/packages/KTools.VersionIncrement/) をインストール

### Local.testsettings
- [データと診断] → [コード カバレッジ]: 有効
- [構成] をクリックし、テスト対象のアセンブリを選択
