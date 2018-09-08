# T4 でクラスやプロパティを自動生成する
Text Template Transformation Toolkit (T4) はテンプレート エンジンの一つで、主に Visual Studio で使われているものです。  
これを使うと、ソースコードやデータの集合などのファイルを自動生成できます。

今回は例として、次のような Markdown を記述したら、それに対応するクラスやプロパティを C# のソースコードとして生成することを考えます。

https://gist.github.com/sakapon/f06c910251724dd293c7f130688278dc

この Markdown の仕様を次のように定めます。
- 空行は無視
- 箇条書きでない行がクラス名
- その下に続く箇条書きはプロパティ
  - `- Type PropertyName` の形式
  - プロパティ名が省略された場合は、型名と同じ
- クラス名およびプロパティ名は、PascalCase, camelCase のどちらを指定してもよい

以下では、このような .md ファイルを入力として、プロパティおよびコンストラクターを持つ部分クラスを .cs ファイルに出力するように T4 で実装していきます。

まず、プロジェクトに上記の .md ファイルを追加しておきます。  
そしてプロジェクトに「テキスト テンプレート (.tt)」 を追加します。

![](https://github.com/sakapon/Samples-2018/blob/master/Images/TextTemplateSample/AddNewItem.png)

追加された .tt ファイルを、仕様に従って次のように実装します。

https://gist.github.com/sakapon/c6574bc3c84e1da5f8ef2255a2f4315e

注意点は以下の通りです。
- 初期状態では出力の拡張子が .txt になっているため、.cs に変更する
- プロジェクト内のファイルのパスを取得するには、`hostspecific="true"` を指定して `Host.ResolvePath` メソッドを使う
- `<#= #>` ：テキストの出力
- `<# #>` ：コードを書ける、変数を使える
- `<#+ #>` ：メソッド、クラスなどを定義できる

.tt ファイルを保存したときに処理が実行されます。  
または、.tt ファイルを右クリックして `カスタム ツールの実行` を選択すれば実行されます。

![](https://github.com/sakapon/Samples-2018/blob/master/Images/TextTemplateSample/RunCustomTool.png)

これで、以下のように RecordTypes.cs が生成されます。

https://gist.github.com/sakapon/fe8652bdbdcce776cc95be6e701426f3

### 作成したサンプル
- [RecordGenConsole (GitHub)](https://github.com/sakapon/Samples-2018/tree/master/TextTemplateSample/RecordGenConsole)

### テストしたバージョン
- Visual Studio 2017

### 参照
- [コード生成と T4 テキスト テンプレート](https://docs.microsoft.com/ja-jp/visualstudio/modeling/code-generation-and-t4-text-templates)
- [Text Template Transformation Toolkit - Wikipedia](https://ja.wikipedia.org/wiki/Text_Template_Transformation_Toolkit)
- [Visual Studio × T4 × 属性で Entity コード大量生成](https://qiita.com/matatabi_ux/items/f02b2dd6bbb92d39553f)
