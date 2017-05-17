## Project Settings

### プロジェクトのプロパティ
- アプリケーション
  - [アセンブリ名] を設定
  - [既定の名前空間] を設定
- ビルド
  - 条件付きコンパイル シンボル: SILVERLIGHT (Silverlight の場合)
  - 条件付きコンパイル シンボル: SILVERLIGHT;WINDOWS_PHONE (Windows Phone の場合)
  - [詳細設定] → [デバッグ情報]: none (Release のみ)
  - [出力パス]: ..\Release\Current\ (Release のみ)
  - [XML ドキュメント ファイル] : ..\Release\Current\Project1.xml (Release のみ)
- デバッグ
  - [Visual Studio ホスティング プロセスを有効にする] : オフ (Release のみ)

### アセンブリ情報
- プロジェクトに SharedAssemblyInfo.cs をリンクとして追加
- AssemblyInfo.cs に、以下を追加
  - [assembly: CLSCompliant(true)]

### Local.testsettings
- [データと診断] → [コード カバレッジ]: 有効
- [構成] をクリックし、テスト対象のアセンブリを選択
