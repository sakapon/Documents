# Roslyn の構文解析を使ってデバッガーを自作する
// [C# Advent Calendar 2018](https://qiita.com/advent-calendar/2018/c-sharp) の 23 日目の記事です。

デバッガーのようなものを自作してみました。

### 動機
- 普段は Visual Studio を使っているが、デバッグ時に手動でステップ実行するのが面倒
  - ステップ数が多い場合
  - 分岐の様子や変数の状態を軽くチェックしたい場合

### 解決案
- ステップの時間間隔だけを指定して、デバッガーを自動で実行させる
  - 変数の一覧が表示される
  - 時間間隔をリアルタイムで調節できる
- Roslyn の構文解析の機能を使い、各ステップの間にデバッグ用のコードを差し込めば実現できそう

### 結果
というわけで、WPF でプロトタイプ「Tick-tack Debugger」を作ってみた結果、このようになりました (クリックで拡大)。

![](https://github.com/sakapon/Samples-2018/blob/master/Images/SyntaxTreeSample/TickTackDebugger.gif)

### 解説
以下は技術解説です。  
WPF アプリを作成する前に、まず .NET Framework 上のコンソール アプリで実験してみます。  
構文解析を使うには、NuGet で [Microsoft.CodeAnalysis](https://www.nuget.org/packages/Microsoft.CodeAnalysis) をインストールします。

先にソースコードを示しておきます (全体のソリューションは [SyntaxTreeSample](https://github.com/sakapon/Samples-2018/tree/master/SyntaxTreeSample) にあります)。

https://gist.github.com/sakapon/f6366ea8353c565757073fbd3727598e

SyntaxHelper クラスでは、元のソースコードを構文ツリー (SyntaxTree) に変換して解析し、各ステートメントの間にデバッグ用のコード行を挿入していきます。

なお、メソッド、ステートメント、式など、すべてのノードを表す親クラスは SyntaxNode クラスで、
- Parent プロパティ: 親
- Ancestors メソッド: 祖先
- ChildNodes メソッド: 子
- DescendantNodes メソッド: 子孫

が存在することを知っておけば、だいたいの探索ができるでしょう。

この他に、デバッグ用のコードから呼び出されるメソッドを定義するクラス ライブラリとして DebuggerLib を作成しています。
各ステートメントの位置、およびそのとき存在する変数とその値を通知するために、このライブラリを経由させます。

Program クラスでは、生成されたデバッグ用のソースコードをファイルに保存したら、System.CodeDom.Compiler 名前空間の CodeDomProvider を使ってこれをコンパイルし、エントリ ポイント (Main メソッド) を呼び出します。
また、デバッグ コードが実行されたときのイベントハンドラーを登録し、Thread.Sleep メソッドを使って、指定した時間だけ停止させます。

これで、デバッグ対象の元のソースコードが下の Program.cs だとすると、デバッグ用のソースコードとして Program.g.cs が生成されます。

https://gist.github.com/sakapon/e418ec76781dcff701bd61692f03890c

作成したコンソール アプリを実行すると、次の図のようになります (時間間隔は 0.3 秒)。

![](https://github.com/sakapon/Samples-2018/blob/master/Images/SyntaxTreeSample/DebuggerConsole.gif)