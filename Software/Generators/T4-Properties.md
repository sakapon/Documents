例えば、次のような Markdown を記述したらそれに対応する .cs ファイルを生成することができます。
https://gist.github.com/sakapon/f06c910251724dd293c7f130688278dc

この Markdown の仕様を次のように定めます。
- 空行は無視
- 箇条書きでない行がクラス名
- その下に続く箇条書きはプロパティ
  - `- Type PropertyName` の形式
  - プロパティ名が省略された場合は、型名と同じ
- クラス名およびプロパティ名は、PascalCase, camelCase のどちらを指定してもよい

このような .md ファイルを入力として、プロパティおよびコンストラクターを持つ部分クラスが .cs ファイルに出力されるというものを考えます。

### 作成したサンプル
- [RecordGenConsole (GitHub)](https://github.com/sakapon/Samples-2018/tree/master/TextTemplateSample/RecordGenConsole)

### テストしたバージョン
- Visual Studio 2017

### 参照
- [コード生成と T4 テキスト テンプレート](https://docs.microsoft.com/ja-jp/visualstudio/modeling/code-generation-and-t4-text-templates)
- [Text Template Transformation Toolkit - Wikipedia](https://ja.wikipedia.org/wiki/Text_Template_Transformation_Toolkit)
