# Roslyn の構文解析を使ってデバッガーを自作する
// [C# Advent Calendar 2018](https://qiita.com/advent-calendar/2018/c-sharp) の 23 日目の記事です。

デバッガーのようなものを自作してみました。

### 動機
- 普段は Visual Studio を使っているが、デバッグ時に手動でステップ実行するのが面倒
  - ループなどでステップ数が多い場合
  - 分岐の様子や変数の状態を軽くチェックしたい場合

### 解決案
- ステップの時間間隔だけを指定して、デバッガーを自動で実行させる
  - 変数の一覧が表示される
  - 時間間隔をリアルタイムで調節できる
- .NET Compiler Platform (Roslyn) の構文解析の機能を使い、各ステップの間にデバッグ用のコードを差し込めば実現できそう

### 結果
というわけで、WPF でプロトタイプ「Tick-tack Debugger」を作ってみた結果、このようになりました。  
例として、ニュートン法で平方根を求めています (クリックで拡大)。

![](https://github.com/sakapon/Samples-2018/blob/master/Images/SyntaxTreeSample/TickTackDebugger.gif)

### 解説
以下は概略の技術解説です。  
WPF アプリを作成する前に、まず .NET Framework 上のコンソール アプリで実験してみます。  
構文解析を使うには、NuGet で [Microsoft.CodeAnalysis](https://www.nuget.org/packages/Microsoft.CodeAnalysis) をインストールします。

デバッグ対象となるソースコードにデバッグ コードを挿入し、それを動的にコンパイルして実行する、という方針です。  
先にソースコードを示しておきます (全体のソリューションは [SyntaxTreeSample](https://github.com/sakapon/Samples-2018/tree/master/SyntaxTreeSample) にあります)。

https://gist.github.com/sakapon/f6366ea8353c565757073fbd3727598e

SyntaxHelper クラスでは、デバッグ対象の C# ソースコードを構文ツリー (SyntaxTree) に変換して走査し、各ステートメントの前にデバッグ用のコード行を挿入していきます。

CSharpSyntaxTree.ParseText メソッドを使うことで、ソースコードを構文ツリーに変換できます。  
また、メソッド・ステートメント・式など、すべてのノードを表す親クラスは SyntaxNode クラスであり、
- Parent プロパティ: 親
- Ancestors メソッド: 祖先
- ChildNodes メソッド: 子
- DescendantNodes メソッド: 子孫

が存在することを知っておけば、だいたいの探索ができるでしょう。

この他に、デバッグ用のコードから呼び出されるメソッドを定義するクラス ライブラリとして DebuggerLib を作成しています。
各ステートメントの位置、およびその直前で存在する変数とその値を通知するために、このライブラリを経由させます。

Program クラスでは、生成されたデバッグ用のソースコードをファイルに保存したら、System.CodeDom.Compiler 名前空間の CodeDomProvider を使ってこれをコンパイルし、そのエントリ ポイント (Main メソッド) を呼び出します。  
また、デバッグ コードが実行されたときのイベントハンドラーを登録しておき、Thread.Sleep メソッドを使って、指定した時間だけ停止させます。

これで、デバッグ対象の元のソースコードが次の Program.cs だとすると、デバッグ用のソースコードとして下の Program.g.cs が生成されます。

https://gist.github.com/sakapon/e418ec76781dcff701bd61692f03890c

作成したコンソール アプリを実行すると、次の図のようになります (時間間隔は 0.3 秒)。

![](https://github.com/sakapon/Samples-2018/blob/master/Images/SyntaxTreeSample/DebuggerConsole.gif)

以上をもとに、WPF アプリでデバッグ ツールを作成しました。  
左側の C# ソースコードの部分は TextBox で、編集もできます。
デバッグ実行時は、各ステートメントを選択状態にすることでハイライトしています。  
右側の変数一覧が表示される部分は DataGrid です。

![](https://github.com/sakapon/Samples-2018/blob/master/Images/SyntaxTreeSample/TickTackDebugger-Pi.png)

(図は円周率を求める例)

今回は上記の方法でプロトタイプを作ってみましたが、デバッグ コードの挿入やコンパイルに関しては、よりスマートな方法があるのではないかと思います。

### 注意点
- 考えられうるすべてのステートメントには対応できていません。また、Main メソッドしか構文解析していません。
- コンパイル時に生成されるアセンブリ (EXE) は、`%TEMP%` フォルダー (ユーザーの `AppData\Local\Temp`) に保存されていきます。
- TextBox で、IsInactiveSelectionHighlightEnabled を True に設定しても利かないことがあります。  
  また、選択状態のハイライトがずれることがあります。  
  RichTextBox で Run などを使うのがよいかもしれません。

### 作成したサンプル
- [SyntaxTreeSample (GitHub)](https://github.com/sakapon/Samples-2018/tree/master/SyntaxTreeSample)

### バージョン情報
- .NET Framework 4.7
- [Microsoft.CodeAnalysis](https://www.nuget.org/packages/Microsoft.CodeAnalysis) 2.10.0
- [ReactiveProperty](https://www.nuget.org/packages/ReactiveProperty/) 5.3.2

### 参照
- [.NET Compiler Platform SDK (Roslyn API)](https://docs.microsoft.com/ja-jp/dotnet/csharp/roslyn-sdk/)
- [WPF4.5入門 その24 「DataGridコントロール その2」](https://blog.okazuki.jp/entry/20130224/1361693816)
