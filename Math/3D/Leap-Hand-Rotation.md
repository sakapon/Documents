## Leap Motion で手の回転状態を取得する

Leap Motion Controller で手の回転の状態を取得する方法として、Hand.Direction (Vector クラス) の Yaw, Pitch, Roll プロパティが用意されています。
ただし、[Hand クラスの説明](https://developer.leapmotion.com/documentation/csharp/api/Leap.Hand.html)を参照すると、
ロールについては Direction.Roll ではなく PalmNormal.Roll を使うように書かれています。

```c#
float pitch = hand.Direction.Pitch;
float yaw = hand.Direction.Yaw;
float roll = hand.PalmNormal.Roll;
```

しかし、これらの値を使って実装してみても、期待通りの動作にはなりません。

そこで、前回の [3D における回転の表現と相互変換](3D-Rotation-Conversion.md)の内容をもとに、手の回転の状態を取得する機能を自作しました。

Hand.Direction と Hand.PalmNormal はともに長さ 1 で直交しているため、これらをそれぞれ (0, 0, -1) と (0, -1, 0) の回転後のベクトルと見なして、前回の Rotation3DHelper クラスを利用すれば OK です。

[HandRotationLeap](https://gist.github.com/sakapon/97659608cd8e63f27277451fec2b3a8c)

全体のソースコードは [HandRotationLeap (GitHub)](https://github.com/sakapon/Samples-2016/tree/master/LeapSample/HandRotationLeap) にあります。

前回: [3D における回転の表現と相互変換](3D-Rotation-Conversion.md)

**バージョン情報**  
.NET Framework 4.5  
Leap Motion SDK 2.3.1

**参照**  
[Hand クラス](https://developer.leapmotion.com/documentation/csharp/api/Leap.Hand.html)
