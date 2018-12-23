# Roslyn の構文解析を使ってデバッガーを自作する
// [C# Advent Calendar 2018](https://qiita.com/advent-calendar/2018/c-sharp) の 23 日目の記事です。

デバッガーのようなものを自作してみました。

### 動機
- 普段は Visual Studio を使っているが、デバッグ時に手動でステップ実行するのが面倒
  - ステップ数が多い場合
  - 分岐の様子や変数の状態を軽くチェックしたい場合

### 解決案
- ステップの時間間隔だけを指定して、デバッガーを自動で実行させる
  - 変数の値をウォッチできる
  - 時間間隔をリアルタイムで調節できる
- Roslyn の構文解析の機能を使い、各ステップの間にデバッグ用のコードを差し込めば実現できそう

### 結果
というわけで、WPF でプロトタイプ「Tick-tack Debugger」を作ってみた結果、このようになりました (クリックで拡大)。

![](https://github.com/sakapon/Samples-2018/blob/master/Images/SyntaxTreeSample/TickTackDebugger.gif)
