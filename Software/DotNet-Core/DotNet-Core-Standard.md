# .NET Core と .NET Standard
.NET Core が登場したことにより、Visual Studio で新しいプロジェクトを作成しようとすると、従来の .NET Framework のほかに .NET Core と .NET Standard 向けのプロジェクト テンプレートが現れます。

(図)

それぞれのプロジェクトにおけるアセンブリの参照可否をまとめると次のようになります。
- .NET Framework コンソール アプリ
  - .NET Framework アセンブリを参照可能
  - .NET Core アセンブリを参照不可
  - .NET Standard アセンブリを参照可能
- .NET Core コンソール アプリ
  - .NET Framework アセンブリを参照不可
  - .NET Core アセンブリを参照可能
  - .NET Standard アセンブリを参照可能

.NET Framework と .NET Core はランタイムが異なるため、両方に対応するクロスプラットフォームのクラス ライブラリを作成するには .NET Standard をターゲットにします。
なお、.NET Standard 2.0 アセンブリは、.NET Framework 4.6.1 以上で参照可能です。
