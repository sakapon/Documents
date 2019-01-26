## transform と deviceorientation における回転の表現 (HTML)
CSS の transform プロパティと JavaScript の deviceorientation イベントではともに 3D の回転状態 (姿勢、傾き) が登場しますが、その扱い方に差があるため検証しました。  
deviceorientation は、デバイスのジャイロ センサーが回転状態を通知することで発生するイベントです。

HTML の 3 次元座標系では、2 次元スクリーン座標系の x 軸および y 軸に加えて、スクリーンに垂直な z 軸が存在します。
デバイス (スマートフォンなど) を水平に持ち、北を向いた状態を基準に考えます。

このとき、CSS の transform プロパティと JavaScript の deviceorientation イベントにおける、回転に関する性質の違いを下の表にまとめました。  
例えば z 軸を中心とする回転の角度は、現在は北を向いていたとしたら、transform では東側 (時計回り) を向くと正、deviceorientation では西側を向くと正になります。

| | transform | deviceorientation |
-|-|-
| 座標系 | **左手系** | **右手系** |
| x 軸 | 右が正 | 右が正 |
| y 軸 | **下 (手前) が正** | **上 (奥) が正** |
| z 軸 | (画面が水平のとき) 鉛直の上が正 | 鉛直の上が正 |
| 回転角度 | 回転軸の正方向に**左ねじ**を進める場合が正 | 回転軸の正方向に**右ねじ**を進める場合が正 |

検証のため、CSS の transform プロパティと JavaScript の deviceorientation イベントを利用して、デバイスの回転状態を画面内の立方体オブジェクトに同期させるサンプルを作成しました。

![](https://github.com/sakapon/JS-Test/blob/master/images/DeviceOrientation/DeviceOrientation.gif)

ジャイロ センサーを搭載した端末であれば、[こちらのテストページ](https://sakapon.github.io/JS-Test/DeviceOrientation/sync)で確認できます。  
HTML のソースは以下の通りです。

https://gist.github.com/sakapon/168ebc510f375af192cd3fc6d44d6a02

以下は、各技術についての説明です。

### transform プロパティ
transform プロパティで rotateX などを利用して回転状態を指定する場合、次に示すように複数の回転を重ね合わせることができます。

CSS:
```CSS
transform: rotateX(45deg) rotateY(30deg) rotateZ(60deg);
```

JavaScript:
```JS
element.style.transform = "rotateX(45deg) rotateY(30deg) rotateZ(60deg)";
```

ただし、**座標系ごと回転させながら**左から順に適用します ([オイラー角](https://t.co/4WbYGmDCfa))。  
これは、以前に [3D における回転の表現と相互変換](3D-Rotation-Conversion.md)で書いた通り、**元の座標系のまま**右から順に適用する、と考えても同じです。

以下に `rotateX(45deg)` と `rotateY(45deg)` を組み合わせた例を載せておきます。

初期状態  
![](https://github.com/sakapon/JS-Test/blob/master/images/DeviceOrientation/transform/rotate-0.png)

`rotateX(45deg)` (左) `rotateX(45deg) rotateY(45deg)` (右)  
![](https://github.com/sakapon/JS-Test/blob/master/images/DeviceOrientation/transform/rotate-x45.png)
![](https://github.com/sakapon/JS-Test/blob/master/images/DeviceOrientation/transform/rotate-x45-y45.png)

`rotateY(45deg)` (左) `rotateY(45deg) rotateX(45deg)` (右)  
![](https://github.com/sakapon/JS-Test/blob/master/images/DeviceOrientation/transform/rotate-y45.png)
![](https://github.com/sakapon/JS-Test/blob/master/images/DeviceOrientation/transform/rotate-y45-x45.png)

### deviceorientation イベント
`window.addEventListener` で `deviceorientation` に対するイベントリスナーを登録します。
デバイスの回転状態が変化すると、イベントリスナーが呼び出されます。

引数の `alpha, beta, gamma` はそれぞれ z 軸、x 軸、y 軸を中心とした回転の角度を表し、**座標系ごと回転させながら**この順に重ね合わせたものが回転状態を表します。  
それぞれの値の範囲は次の通りです。
- z 軸: 0 ≦ alpha < 360
  - 北を向いたとき、alpha = 0
- x 軸: -180 ≦ beta < 180
- y 軸: -90 ≦ gamma < 90

### 回転状態の同期
以上から、デバイスの回転状態を画面内のオブジェクトに同期させるには次のようにします。
```JS
cubeEl.style.transform = `rotateZ(${-e.alpha}deg) rotateX(${-e.beta}deg) rotateY(${e.gamma}deg)`;
```

正負の符号に注意します。  
結果として、z 軸および x 軸における回転角度の正負は異なり、y 軸では同じになります。

次回: [ASP.NET SignalR でデバイスの回転状態を同期する](SignalR-Device-Orientation.md)

### 作成したサンプル
- [DeviceOrientation](https://github.com/sakapon/JS-Test/tree/master/DeviceOrientation) (GitHub)

### 参照
- [3D における回転の表現と相互変換](3D-Rotation-Conversion.md)
- [Leap Motion で手の回転状態を取得する](Leap-Hand-Rotation.md)

#### transform
- [transform](https://developer.mozilla.org/ja/docs/Web/CSS/transform)
- [perspective](https://developer.mozilla.org/ja/docs/Web/CSS/perspective)
- [CSS3 のみでキューブ(立方体)を作る！](https://cartman0.hatenablog.com/entry/2015/05/29/173343)
- [CSS3: 3次元空間に立方体をつくって回す ー transformプロパティ](http://www.fumiononaka.com/Business/html5/FN1404001.html)

#### deviceorientation
- [デバイスの方向の検出](https://developer.mozilla.org/ja/docs/Web/API/Detecting_device_orientation)
- [方向および動きとして示されるデータ](https://developer.mozilla.org/ja/docs/DOM/Orientation_and_motion_data_explained)
- [Using device orientation with 3D transforms](https://developer.mozilla.org/en-US/docs/Web/Guide/Events/Using_device_orientation_with_3D_transforms)
- [端末画面の向きと端末のモーション](https://developers.google.com/web/fundamentals/native-hardware/device-orientation/?hl=ja)
