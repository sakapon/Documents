# WPF の Tips 集
WPF についての Tips を集めたものです。

### デザイン モード
Visual Studio や Blend において XAML の編集画面が表示されているとき、  
内部で実行されるコードでは [DesignerProperties.GetIsInDesignMode メソッド](https://learn.microsoft.com/dotnet/api/system.componentmodel.designerproperties.getisindesignmode)の戻り値は true となるため、  
デザイン時と非デザイン時でアプリの挙動を切り替えることができます。  
ただし、この方法では引数に DependencyObject を受け付けるため、Model 層では使いづらいです。

そこで、デザイン時のみ有効となる `d:` を使います。  
例えば Model 層のクラスに bool 型の IsForDesign プロパティを用意しておき、Window などの DataContext に  
`<local:AppModel d:IsForDesign="True"/>`  
と設定することで、挙動を切り替えることができます。

利用例： [DQBG: DQ'S BATTLEGROUNDS](https://github.com/kcg-edu-future-lab/DQBG/blob/main/DQBG/DQBG/MainWindow.xaml)

### その他
- マウスなどのイベントを下の階層の UI 要素で発生させたい場合、[IsHitTestVisible プロパティ](https://learn.microsoft.com/dotnet/api/system.windows.uielement.ishittestvisible)を False に設定します。
- XAML 上で null 参照は `{x:Null}` で表せます。
  - [x:Null マークアップ拡張](https://learn.microsoft.com/dotnet/desktop/xaml-services/xnull-markup-extension)
- 同じプロパティに対して、データ バインディングとアニメーションの両方を設定してはいけません。
- ClickOnce インストーラーは、そのままローカルのインストーラーとしても使えます。
  - `.application` または `setup.exe` をローカルで直接実行すればよいです。

### 参照
- [たまに利用する .NET Tips 集](https://sakapon.wordpress.com/2011/05/23/tips/)
