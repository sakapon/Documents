# XAML 系の Tips 集
WPF をはじめとする XAML 系技術についての Tips を集めたものです。

### XAML の記述
- [x:Null マークアップ拡張](https://learn.microsoft.com/dotnet/desktop/xaml-services/xnull-markup-extension)
  - XAML 上で null 参照は `{x:Null}` で表せます。
- [x:Static マークアップ拡張](https://learn.microsoft.com/dotnet/desktop/xaml-services/xstatic-markup-extension)
- [x:Array マークアップ拡張](https://learn.microsoft.com/dotnet/desktop/xaml-services/xarray-markup-extension)
- [x:TypeArguments ディレクティブ](https://learn.microsoft.com/dotnet/desktop/xaml-services/xtypearguments-directive)

### その他
- マウスなどのイベントを下の階層の UI 要素で発生させたい場合、[IsHitTestVisible プロパティ](https://learn.microsoft.com/dotnet/api/system.windows.uielement.ishittestvisible)を False に設定します。
- 同じプロパティに対して、データ バインディングとアニメーションの両方を設定してはいけません。
- ClickOnce インストーラーは、そのままローカルのインストーラーとしても使えます。
  - `.application` または `setup.exe` をローカルで直接実行すればよいです。

### 参照
- [たまに利用する .NET Tips 集](https://sakapon.wordpress.com/2011/05/23/tips/)
