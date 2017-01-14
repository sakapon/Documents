## 3D における回転の表現と相互変換

以前に投稿した [WPF で 3D オブジェクトを回転させる](WPF-3D-Rotation.md)ではオブジェクトの回転の状態を行列で表していましたが、  
3 次元空間における回転を表現する方法は、次のように何通りか考えられます。  
なお、原点を中心に回転させるものとし、回転角度は、回転軸を表すベクトルの方向に右ねじを押し込む場合を正とします。

1. 行列
  - 3 次正方行列。とくに、回転を表すものは直交行列であり、`P^{-1} = {}^t P` が成り立つ
  - WPF では [Matrix3D 構造体](https://msdn.microsoft.com/ja-jp/library/system.windows.media.media3d.matrix3d.aspx)
1. 四元数 (クォータニオン)
  - 回転軸と回転角度から構成される値。詳細の定義は[四元数と三次元空間における回転](http://mathtrain.jp/quaternion)を参照
  - WPF では [Quaternion 構造体](https://msdn.microsoft.com/ja-jp/library/system.windows.media.media3d.quaternion.aspx)
1. オイラー角 (ロール、ピッチ、ヨー)
  - 任意の回転を 3 回の単純な回転の合成により表す。詳細の定義は[ロール・ピッチ・ヨー ― Kinectで学ぶ数学](http://www.buildinsider.net/small/bookkinectv2/0804)を参照
  - **座標系ごと**回転させる
  - ロール、ピッチ、ヨーをそれぞれどの軸に対応させるか、どの順番で作用させるかで結果が変わってしまうため注意が必要
1. 特定の 2 点の回転前後の座標
  - とくに、直交する 2 つのベクトルを用いるとよい

3D のプログラミングをしていると、API によってどの表現を利用するかが異なることがあります。  
以下では、これらの表現を互いに変換する方法について考えます。

### 行列 → 2 点の座標
任意のベクトルに対して行列を作用させれば回転後のベクトルが求められます。  
WPF では[演算子 *](https://msdn.microsoft.com/ja-jp/library/ms603921.aspx) が用意されています。

### 四元数 → 行列
四元数の成分から行列の各成分を計算できます (詳細は[四元数による回転行列の表現](http://physmath.main.jp/src/quaternion-rotation.html)を参照)。  
WPF では [Matrix3D.Rotate メソッド](https://msdn.microsoft.com/ja-jp/library/system.windows.media.media3d.matrix3d.rotate.aspx)として用意されています。

### オイラー角 → 行列または四元数
ここでは、ヨーは y 軸、ピッチは x 軸、ロールは z 軸のまわりの回転を表し、この順に適用されるとします。  
(元の座標系における) ヨー、ピッチ、ロールを表す回転行列をそれぞれ `R_y, R_p, R_r` とし、それぞれの回転角度を `\theta_y, \theta_p, \theta_r` とします。
すなわち、

```
R_y = \left( \begin{array}{ccc} \cos \theta_y & 0 & \sin \theta_y \\ 0 & 1 & 0 \\ -\sin \theta_y & 0 & \cos \theta_y \end{array} \right), 
R_p = \left( \begin{array}{ccc} 1 & 0 & 0 \\ 0 & \cos \theta_p & -\sin \theta_p \\ 0 & \sin \theta_p & \cos \theta_p \end{array} \right), 
R_r = \left( \begin{array}{ccc} \cos \theta_r & -\sin \theta_r & 0 \\ \sin \theta_r & \cos \theta_r & 0 \\ 0 & 0 & 1 \end{array} \right)
```

このとき、座標系ごとヨー、ピッチ、ロールの順に適用した回転は、**元の座標系で** `R_y R_p R_r` で表されます (適用の順序が逆になる)。
証明は次のようにできますが、実際の回転をイメージするとわかりやすいと思います。

#### 証明
ベクトル `{\bf x}` にヨーを作用させると、`R_y {\bf x}`。  
次に、これにピッチを作用させるには、いったん座標系を戻して `R_p` を作用させるから、  
`R_y R_p R_y^{-1} \cdot R_y {\bf x} = R_y R_p {\bf x}`  
同様に、これにロールを作用させると、  
`(R_y R_p) R_r (R_y R_p)^{-1} \cdot R_y R_p {\bf x} = R_y R_p R_r {\bf x}`  
(証明終)

WPF での実際の演算では四元数を使うとよいでしょう。

### 2 点の座標 → オイラー角
ここでは、2 点として `{\bf e}_z = (0, 0, 1), {\bf e}_y = (0, 1, 0)` を選びます。  
これらの回転後のベクトルがそれぞれ `{\bf u}, {\bf t}` で与えられたとすると、オイラー角は以下の手続きにより求められます。

#### 解
まず、`{\bf u}` の x 要素と z 要素の比はピッチおよびロールの影響を受けないから、  
`\tan \theta_y = \frac{{\bf u}_x}{{\bf u}_z}`
により、`\theta_y, R_y` が決まる。

ピッチおよびロールの決め方から、  
`\tan \theta_p = \frac{- (R_p {\bf e}_z)_y}{(R_p {\bf e}_z)_z}`
`\tan \theta_r = \frac{- (R_r {\bf e}_y)_x}{(R_r {\bf e}_y)_y}`

また、前項と同様に考えて、  
`{\bf u} = R_y R_p {\bf e}_z`
`{\bf t} = R_y R_p R_r {\bf e}_y`
であるから、  
`R_p {\bf e}_z = R_y^{-1} {\bf u}`
`R_r {\bf e}_y = R_p^{-1} R_y^{-1} {\bf t}`

したがって、`\theta_p, \theta_r` も順に決まる。  
なお、この決め方の場合、回転角度の範囲は、  
`- \pi < \theta_y \le \pi, - \pi / 2 \le \theta_p \le \pi / 2, - \pi < \theta_r \le \pi`  
となる。  
(終)

arctan を求めるには、[Math.Atan2 メソッド](https://msdn.microsoft.com/ja-jp/library/system.math.atan2.aspx)を使うとよいでしょう。

以上により、行列 → 2 点の座標 → オイラー角 → 四元数 → 行列と変換する方法が与えられたので、回転の表現を相互に変換できるようになります。

では、これらを実装してみます。  
ベクトル、行列、四元数を扱うための [System.Numerics.Vectors](https://www.nuget.org/packages/System.Numerics.Vectors/) という、WPF のライブラリよりも高機能なライブラリもあるのですが、今回は WPF のライブラリのみを利用して実装したいと思います。

[Rotation3DHelper](https://gist.github.com/sakapon/9ab43c8b90fd266ae61d764c307a3f86)

全体のソースコードは [RotationTest (GitHub)](https://github.com/sakapon/Samples-2016/tree/master/Wpf3DSample/RotationTest) にあります。

前回: [WPF で 3D オブジェクトを回転させる](WPF-3D-Rotation.md)

**バージョン情報**  
.NET Framework 4.5

#### 参照
- [四元数と三次元空間における回転](http://mathtrain.jp/quaternion)
- [四元数による回転行列の表現](http://physmath.main.jp/src/quaternion-rotation.html)
- [ロールピッチヨー角による回転行列の表現](http://physmath.main.jp/src/roll-pitch-yaw.html)
- [ロール・ピッチ・ヨー ― Kinectで学ぶ数学](http://www.buildinsider.net/small/bookkinectv2/0804)
- [NuGet Gallery | System.Numerics.Vectors](https://www.nuget.org/packages/System.Numerics.Vectors/)
- [System.Numerics 名前空間](https://msdn.microsoft.com/ja-jp/library/system.numerics.aspx)
