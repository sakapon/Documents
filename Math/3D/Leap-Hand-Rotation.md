Leap Motion Controller で手の回転の状態を取得する方法として、Hand.Direction (Vector クラス) の Yaw, Pitch, Roll プロパティが用意されています。
ただし、[Hand クラスの説明](https://developer.leapmotion.com/documentation/csharp/api/Leap.Hand.html)を参照すると、
ロールについては Direction.Roll ではなく PalmNormal.Roll を使うように書かれています。

```c#
float pitch = hand.Direction.Pitch;
float yaw = hand.Direction.Yaw;
float roll = hand.PalmNormal.Roll;
```

しかし、これらの値を使って実装してみても、期待通りの動作にはなりません。

**バージョン情報**  
.NET Framework 4.5  
Leap Motion SDK 2.3.1

**参照**  
[Hand クラス](https://developer.leapmotion.com/documentation/csharp/api/Leap.Hand.html)
