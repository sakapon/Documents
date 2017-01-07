## 3D における回転の表現と相互変換

3 次元空間における回転を表現する方法は、次のように何通りか考えられます。

- 行列
  - 3 次正方行列。とくに、回転を表すものは直交行列であり、`A^{-1} = {}^t A` が成り立つ
  - WPF では [Matrix3D 構造体](https://msdn.microsoft.com/ja-jp/library/system.windows.media.media3d.matrix3d.aspx)
- 四元数 (クォータニオン)
  - 回転軸と回転角度から構成される値。詳細の定義は[四元数と三次元空間における回転](http://mathtrain.jp/quaternion)を参照
  - WPF では [Quaternion 構造体](https://msdn.microsoft.com/ja-jp/library/system.windows.media.media3d.quaternion.aspx)
- オイラー角 (ロール、ピッチ、ヨー)
  - 任意の回転を 3 回の単純な回転の合成により表す。詳細の定義は[ロール・ピッチ・ヨー ― Kinectで学ぶ数学](http://www.buildinsider.net/small/bookkinectv2/0804)を参照
  - **座標系ごと**回転させる
  - ロール、ピッチ、ヨーをそれぞれどの軸に対応させるか、どの順番で作用させるかで結果が変わってしまうため注意が必要
- 特定の 2 点の回転前後の座標
  - とくに、直交する 2 つのベクトルを用いるとよい

以下では、これらの表現を互いに変換する方法について考えます。  
なお、原点を中心に回転するものとします。

#### 参照
- [四元数と三次元空間における回転](http://mathtrain.jp/quaternion)
- [四元数による回転行列の表現](http://physmath.main.jp/src/quaternion-rotation.html)
- [ロールピッチヨー角による回転行列の表現](http://physmath.main.jp/src/roll-pitch-yaw.html)
- [ロール・ピッチ・ヨー ― Kinectで学ぶ数学](http://www.buildinsider.net/small/bookkinectv2/0804)
- [NuGet Gallery | System.Numerics.Vectors](https://www.nuget.org/packages/System.Numerics.Vectors/)
- [System.Numerics 名前空間](https://msdn.microsoft.com/ja-jp/library/system.numerics.aspx)
