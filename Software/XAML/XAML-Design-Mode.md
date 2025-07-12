# XAML のデザイン モード
Visual Studio や Blend において XAML の編集画面が表示されているとき、  
内部で実行されるコードでは [DesignerProperties.GetIsInDesignMode メソッド](https://learn.microsoft.com/dotnet/api/system.componentmodel.designerproperties.getisindesignmode)の戻り値は true となるため、  
デザイン時と非デザイン時でアプリの挙動を切り替えることができます。  
ただし、この方法では引数に DependencyObject を受け付けるため、Model 層では使いづらいです。

そこで、デザイン時のみ有効となる `d:` を使います。  
例えば Model 層のクラスに bool 型の IsForDesign プロパティを用意しておき、Window などの DataContext に  
`<local:AppModel d:IsForDesign="True"/>`  
と設定することで、挙動を切り替えることができます。

https://gist.github.com/sakapon/4eda2d33b1068463c6899578775ea919

利用例： [DQBG: DQ'S BATTLEGROUNDS](https://github.com/kcg-edu-future-lab/DQBG/blob/main/DQBG/DQBG/MainWindow.xaml)

### 参照
- [Visual Studio の XAML デザイナーでデザイン時のデータを使用する](https://learn.microsoft.com/visualstudio/xaml-tools/xaml-designtime-data)
