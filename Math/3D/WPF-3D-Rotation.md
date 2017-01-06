## WPF で 3D オブジェクトを回転させる

前回の [WPF で 3D オブジェクトを表示する](WPF-3D-Model.md)に引き続いて、今回は 3D オブジェクトを回転させます。  
図のようにボタンを配置して、6 方向の回転ができるように実装します。

![Dice Rotation](https://github.com/sakapon/Samples-2016/raw/master/Images/Wpf3DSample/DiceRotationWpf-0.png)

次のようにコードを追加・変更します。

[DiceRotationWpf](https://gist.github.com/sakapon/635dca42214a603f0f014d876e38eed9)

ボタンとして RepeatButton を配置しています。  
RepeatButton は、押したままにしておけば断続的に Click イベントが発生します。  
また回転の状態を表すために、ModelVisual3D.Transform の中で [MatrixTransform3D](https://msdn.microsoft.com/ja-jp/library/system.windows.media.media3d.matrixtransform3d.aspx) を使います。

回転には、回転軸と回転角度が必要です。  
- 回転軸は、ベクトルで表されます。  
- 回転角度は、回転軸の方向に右ねじを押し込む場合を正とします。

6 つのボタンはそれぞれ、x, y, z 軸を回転軸とした正方向または負方向の回転を表します。  
1 回の Click イベントにつき 5° ずつ回転させています。  
なお、カメラは z 軸上の正の位置から原点方向を見下ろし、右側が x 軸の正、上側が y 軸の正を表しています。

Click イベントハンドラーの中で、[Matrix3D.Rotate メソッド](https://msdn.microsoft.com/ja-jp/library/system.windows.media.media3d.matrix3d.rotate.aspx)を呼び出すことでオブジェクトを回転させます。  
引数には、回転軸と回転角度を表す [Quaternion](https://msdn.microsoft.com/ja-jp/library/system.windows.media.media3d.quaternion.aspx) (四元数) を指定します。  
このように、回転軸と回転角度がわかっている場合は比較的簡単に実装ができます。

下図は、最初の状態から x 軸の負の方向に 60° 回転させたところです。  
![Dice Rotation](https://github.com/sakapon/Samples-2016/raw/master/Images/Wpf3DSample/DiceRotationWpf-x-60.png)

全体のソースコードは [DiceRotationWpf (GitHub)](https://github.com/sakapon/Samples-2016/tree/master/Wpf3DSample/DiceRotationWpf) にあります。  
マウスまたはタッチのドラッグ操作でも回転できるようになっています。  
![Dice Rotation](https://github.com/sakapon/Samples-2016/raw/master/Images/Wpf3DSample/DiceRotationWpf-Play.gif)

前回: [WPF で 3D オブジェクトを表示する](WPF-3D-Model.md)

**作成したサンプル**  
[DiceRotationWpf (GitHub)](https://github.com/sakapon/Samples-2016/tree/master/Wpf3DSample/DiceRotationWpf)

**バージョン情報**  
.NET Framework 4.5

**参照**  
[3-D グラフィックスの概要](https://msdn.microsoft.com/ja-jp/library/ms747437.aspx)  
[3-D 変換の概要](https://msdn.microsoft.com/ja-jp/library/ms753347.aspx)
