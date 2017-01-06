## WPF で 3D オブジェクトを表示する

WPF の 3D グラフィックスの機能を使って、3D オブジェクトを表示する方法を示します。  
XAML でさいころのような立方体を描画することを目指します。

![Dice (XAML)](https://github.com/sakapon/Samples-2016/raw/master/Images/Wpf3DSample/DiceXamlWpf.png)

先に XAML を示します。この後に簡単な説明が続きます。

[DiceXamlWpf](https://gist.github.com/sakapon/bdec458e3e528ef85ae37aed68d3cd31)

まず、Viewport3D を配置します。  
3D オブジェクトを描画するには次の 3 種類のものが必要で、これらを Viewport3D に設定します。

**カメラ**  
Viewport3D.Camera プロパティに指定します。
PerspectiveCamera を使うと、遠近感が出ます。  
既定では、`LookDirection="0,0,-1" UpDirection="0,1,0"` となっており、つまり z 軸の逆方向を向いており、y 軸の正方向が上になっています。

**照明**  
通常のオブジェクトと同様に、Viewport3D.Children プロパティにモデルとして追加します。  
AmbientLight を使うと、影がなく、一様な明るさでオブジェクトが表示されます。

**オブジェクト**  
主に ModelVisual3D として定義し、Viewport3D.Children プロパティに追加します。

以下では、ModelVisual3D の設定について書いていきます。  
GeometryModel3D でオブジェクトを定義し、ModelVisual3D.Content プロパティにそれを指定します。  
複数のオブジェクトをグループ化するには、Model3DGroup を使います。  
GeometryModel3D には、マテリアルとジオメトリを指定します。

### マテリアル
GeometryModel3D.Material プロパティで、何を表示するかを指定します。  
ここでは、さいころの面を描くために 2D の TextBlock を用意しておき、それをテクスチャとして指定しています。  
2D の Brush をテクスチャとして利用するには、DiffuseMaterial を使います。

また、BackMaterial プロパティに、Material プロパティと同じものを指定すれば、裏面には表面を反転したものが表示されます。  
つまり、「3」の裏面は「ε」のようになります。

### ジオメトリ
GeometryModel3D.Geometry プロパティで、どこに表示するかを指定します。  
具体的には、MeshGeometry3D を利用して、次のものを指定します。

**点の集合 (Positions プロパティ)**  
メッシュを構成する三角形に分割した場合に、頂点となる点の集合を指定します。

**三角形の集合 (TriangleIndices プロパティ)**  
Positions プロパティで指定した点に0からインデックスを付けて、メッシュを構成する三角形の頂点を列挙します。  
三角形の頂点の順番は、表側から見て反時計回りになるように指定します。

**テクスチャの位置 (TextureCoordinates プロパティ)**  
Positions プロパティで指定した各点が、マテリアルの中のどの点に対応するかを指定します。  
マテリアルは 2D なので、左上が (0, 0)、右下が (1, 1) です。

例えば、

```
<MeshGeometry3D Positions="-1,-1,1 1,-1,1 1,1,1 -1,1,1" TriangleIndices="0,1,2 0,2,3" TextureCoordinates="0,1 1,1 1,0 0,0"/>
```

とした場合は、P0(-1, -1, 1), P1(1, -1, 1), P2(1, 1, 1), P3(-1, 1, 1) の 4 点があり、  
メッシュは P0P1P2 と P0P2P3 の 2 つの三角形で構成され、  
P0 が左下、P1 が右下、P2 が右上、P3 が左上となるようにテクスチャを描画するという意味になります。  
なお、これらの値を XAML 上で列挙するときの区切り文字としては、カンマまたはスペースの両方が使えます。

### アフィン変換 (移動、スケール、回転など) について
3D オブジェクトのアフィン変換は、ModelVisual3D.Transform プロパティで指定します。

### コードでの記述について
XAML の代わりに C# などの .NET のコードで記述することもできます。  
同一の処理が繰り返される場合には、コードで生成したほうが簡単に表現できることもあります。

次回: [WPF で 3D オブジェクトを回転させる](WPF-3D-Rotation.md)

**作成したサンプル**  
[Wpf3DSample (GitHub)](https://github.com/sakapon/Samples-2016/tree/wpf-3d/Wpf3DSample): コードで描画した例もあります。

**バージョン情報**  
.NET Framework 4.5

**参照**  
[3-D グラフィックスの概要](https://msdn.microsoft.com/ja-jp/library/ms747437.aspx)  
[3-D 変換の概要](https://msdn.microsoft.com/ja-jp/library/ms753347.aspx)
