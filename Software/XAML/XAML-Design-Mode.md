# XAML のデザイン モード
WPF をはじめとする XAML 系のアプリを Visual Studio や Blend で開発するとき、  
例えば、レイアウトの作業時はサンプルのデータを表示させたい、ということがあります。

このような場合に、[DesignerProperties.GetIsInDesignMode メソッド](https://learn.microsoft.com/dotnet/api/system.componentmodel.designerproperties.getisindesignmode)を使うことが考えられます。  
このメソッドは XAML の編集画面でアプリが実行されているときに戻り値が true となるため、  
デザイン時と非デザイン時でアプリの挙動を切り替えることができます。  
ただし、このメソッドでは引数に DependencyObject を受け付けるため、Model 層では使いづらいです。

そこで、デザイン時のみ有効となる `d:` を使います。  
例えば Model 層のクラスに bool 型の IsForDesign プロパティを用意しておき、XAML 上で  
`<local:AppModel d:IsForDesign="True"/>`  
と設定することで、挙動を切り替えることができます。  
また、`d:` は UI 要素自体にも指定することができ、デザイン時のみ配置することができます。

https://gist.github.com/sakapon/4eda2d33b1068463c6899578775ea919

利用例： [DQBG: DQ'S BATTLEGROUNDS](https://github.com/kcg-edu-future-lab/DQBG/blob/main/DQBG/DQBG/MainWindow.xaml)

### 参照
- [DesignerProperties.GetIsInDesignMode メソッド](https://learn.microsoft.com/dotnet/api/system.componentmodel.designerproperties.getisindesignmode)
- [Visual Studio の XAML デザイナーでデザイン時のデータを使用する](https://learn.microsoft.com/visualstudio/xaml-tools/xaml-designtime-data)
