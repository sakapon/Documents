# XAML 系の Tips 集
WPF をはじめとする XAML 系技術についての Tips を集めたものです。

### XAML の記述
- [x:Null マークアップ拡張](https://learn.microsoft.com/dotnet/desktop/xaml-services/xnull-markup-extension)
  - null 参照は `{x:Null}` で表せます。
- [x:Static マークアップ拡張](https://learn.microsoft.com/dotnet/desktop/xaml-services/xstatic-markup-extension)
  - 定数、静的フィールド、静的プロパティ、列挙型の値を `{x:Static local:MainWindow.Constant1}` のような形式で表せます。
- [x:Array マークアップ拡張](https://learn.microsoft.com/dotnet/desktop/xaml-services/xarray-markup-extension)
  - 配列の型を `Type` 属性で指定します。
- [x:TypeArguments ディレクティブ](https://learn.microsoft.com/dotnet/desktop/xaml-services/xtypearguments-directive)
  - List や Dictionary の型を `x:TypeArguments` ディレクティブで指定します。

### その他
- マウスなどのイベントを下の階層の UI 要素で発生させたい場合、[IsHitTestVisible プロパティ](https://learn.microsoft.com/dotnet/api/system.windows.uielement.ishittestvisible)を False に設定します。
- 同じプロパティに対して、データ バインディングとアニメーションの両方を設定してはいけません。
- ClickOnce インストーラーは、そのままローカルのインストーラーとしても使えます。
  - `.application` または `setup.exe` をローカルで直接実行すればよいです。

次回: [XAML のデザイン モード](XAML-Design-Mode.md)

### 参照
- [たまに利用する .NET Tips 集](https://sakapon.wordpress.com/2011/05/23/tips/)
