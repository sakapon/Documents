# transform と deviceorientation における回転の表現 (HTML)
HTML 関連で 3D の回転を扱う場面として、CSS の transform プロパティと JavaScript の deviceorientation イベントがあります。
deviceorientation は、デバイスのジャイロ センサーが回転状態を通知することで発生するイベントです。

HTML の 3 次元座標系では、2 次元スクリーン座標系の x 軸および y 軸に、スクリーンに垂直な z 軸が追加されます。
デバイス (スマートフォンなど) を水平に持ち、北を向いた状態を基準に考えます。

このとき、CSS の transform プロパティと JavaScript の deviceorientation イベントにおける、回転に関する性質の違いを下の表にまとめました。  
例えば z 軸を中心とする回転の角度は、現在は北を向いていたとしたら、transform では東側 (時計回り) を向くと正、deviceorientation では西側を向くと正になります。

| | transform | deviceorientation |
-|-|-
| 座標系 | **左手系** | **右手系** |
| x 軸 | 右が正 | 右が正 |
| y 軸 | **下 (手前) が正** | **上 (奥) が正** |
| z 軸 | (画面が水平のとき) 鉛直の上が正 | 鉛直の上が正 |
| 回転角度 | 回転軸の方向に**左ねじ**を回す場合が正 | 回転軸の方向に**右ねじ**を回す場合が正 |

### 回転状態の同期
CSS の transform プロパティと JavaScript の deviceorientation イベントを利用して、デバイスの回転状態を画面上の立方体オブジェクトに同期させるサンプルを作成しました。

(図)

ソースコードはこちらです。

https://gist.github.com/sakapon/168ebc510f375af192cd3fc6d44d6a02

以下は、各技術についての説明です。

### transform プロパティ
transform プロパティで rotateX などを利用して回転状態を指定する場合、  
CSS では
```CSS
transform: rotateX(45deg) rotateY(30deg) rotateZ(60deg);
```

JavaScript では
```JS
element.style.transform = "rotateX(45deg) rotateY(30deg) rotateZ(60deg)";
```

のように、複数の回転を重ね合わせることができます。

ただし、**座標系ごと回転させながら**左から順に適用します。
これは、[3D における回転の表現と相互変換](https://sakapon.wordpress.com/2017/01/15/3d-rotation-conversion/)で書いた通り、**元の座標系のまま**右から順に適用する、と考えても同じです。

以下に `rotateX(45deg)` と `rotateY(45deg)` を組み合わせた例を載せておきます。

(図)

### 作成したサンプル
- [DeviceOrientation](https://github.com/sakapon/JS-Test/tree/master/DeviceOrientation)

### 参照
- [3D における回転の表現と相互変換](https://sakapon.wordpress.com/2017/01/15/3d-rotation-conversion/)

transform
- [perspective](https://developer.mozilla.org/ja/docs/Web/CSS/perspective)

deviceorientation
- [デバイスの方向の検出](https://developer.mozilla.org/ja/docs/Web/API/Detecting_device_orientation)
- [方向および動きとして示されるデータ](https://developer.mozilla.org/ja/docs/DOM/Orientation_and_motion_data_explained)
- [端末画面の向きと端末のモーション](https://developers.google.com/web/fundamentals/native-hardware/device-orientation/?hl=ja)
