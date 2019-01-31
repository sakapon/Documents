## ライブラリの .NET Standard 化
- 対象のプロジェクト
  - .csproj ファイルを .NET Core フォーマットにする
  - Properties/AssemblyInfo.cs を削除する
  - InternalsVisibleTo が必要な場合、AssemblyInfo.cs を追加する
- 単体テスト プロジェクト
  - .csproj ファイルを .NET Core フォーマットにする
  - Properties/AssemblyInfo.cs を削除する
- NuGet Packup を For .NET Core に切り替える場合、次のファイルを削除する
  - NuGetPackup.exe
  - Package.nuspec.xml
  - packages.config

例
- https://github.com/sakapon/Blaze/commit/70cd681e117d2320ae4d7cdd36ab77852dea5291
- https://github.com/sakapon/Harmonia/commit/fd4fc6c69da83da4cc19bdca893ff8c71ea27cc3
