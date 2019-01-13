# Build your own debugger using Roslyn's syntax analysis
// This is the 23rd article of [C# Advent Calendar 2018 in Japan](https://qiita.com/advent-calendar/2018/c-sharp).

I tried to make something like a debugger.

### Motivation
- I normally use Visual Studio, but it is troublesome to step by step manually at debugging
  - When the number of steps is large in a loop or the like
  - When I want to check the state of branching and the state of variables lightly

### Solution
- Specify only the time interval of the step and let the debugger run automatically
  - A list of variables is displayed
  - Time interval can be adjusted in real time
- Using the syntax analysis of .NET Compiler Platform (Roslyn), it seems to be realizable if we insert debugging code between each step

### Result
So, as a result of trying to make prototype "Tick-tack Debugger" with WPF, it looked like this.  
As an example, we are seeking a square root by the Newton's method. (Click to enlarge)

![](https://github.com/sakapon/Samples-2018/blob/master/Images/SyntaxTreeSample/TickTackDebugger.gif)

### 解説
以下は概略の技術解説です。  
WPF アプリを作成する前に、まず .NET Framework 上のコンソール アプリで実験してみます。  
C# の構文解析を使うには、NuGet で [Microsoft.CodeAnalysis.CSharp](https://www.nuget.org/packages/Microsoft.CodeAnalysis.CSharp) をインストールします。

デバッグ対象となるソースコードにデバッグ コードを挿入し、それを動的にコンパイルして実行する、という方針です。  
コンソール アプリのソースコードを以下に示します (全体のソリューションは [SyntaxTreeSample](https://github.com/sakapon/Samples-2018/tree/master/SyntaxTreeSample) にあります)。

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
- [Microsoft.CodeAnalysis.CSharp](https://www.nuget.org/packages/Microsoft.CodeAnalysis.CSharp) 2.10.0
- [ReactiveProperty](https://www.nuget.org/packages/ReactiveProperty/) 5.3.2

### 参照
- [.NET Compiler Platform SDK (Roslyn API)](https://docs.microsoft.com/ja-jp/dotnet/csharp/roslyn-sdk/)
- [WPF4.5入門 その24 「DataGridコントロール その2」](https://blog.okazuki.jp/entry/20130224/1361693816)
