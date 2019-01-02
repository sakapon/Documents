# transform と deviceorientation における回転の表現 (HTML)

| | transform | deviceorientation |
-|-|-
| 座標系 | **左手系** | **右手系** |
| x 軸 | 右が正 | 右が正 |
| y 軸 | **下 (手前) が正** | **上 (奥) が正** |
| z 軸 | 鉛直の上が正 | 鉛直の上が正 |
| 回転角度 | 回転軸の方向に**左ねじ**を回す場合が正 | 回転軸の方向に**右ねじ**を回す場合が正 |

### 参照
- [3D における回転の表現と相互変換](https://sakapon.wordpress.com/2017/01/15/3d-rotation-conversion/)

transform
- [perspective](https://developer.mozilla.org/ja/docs/Web/CSS/perspective)

deviceorientation
- [デバイスの方向の検出](https://developer.mozilla.org/ja/docs/Web/API/Detecting_device_orientation)
- [方向および動きとして示されるデータ](https://developer.mozilla.org/ja/docs/DOM/Orientation_and_motion_data_explained)
- [端末画面の向きと端末のモーション](https://developers.google.com/web/fundamentals/native-hardware/device-orientation/?hl=ja)
