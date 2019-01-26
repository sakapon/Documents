## ASP.NET SignalR でデバイスの回転状態を同期する

前回の [transform と deviceorientation における回転の表現 (HTML)](HTML-Device-Orientation.md) では、そのデバイスのブラウザー上で回転の状態を表示していましたが、今回は他のデバイスのブラウザーにネットワーク経由で同期するようにしました。

WebSocket で同期するためのフレームワークとして ASP.NET SignalR を利用し、Azure Web App に GitHub からの継続的デプロイを設定しています。

https://www.youtube.com/watch?v=616reMOovi8

これを実装する方法を以下に示します。

Visual Studio で空の ASP.NET Web プロジェクトを作成し、NuGet で [Microsoft.AspNet.SignalR](https://www.nuget.org/packages/Microsoft.AspNet.SignalR/) をインストールします。  
まずサーバー側の C# コードとして、次のクラスを実装します。

https://gist.github.com/sakapon/732b69c502e629f9386da1f29e67310f

Startup.Configuration メソッドの中で ASP.NET SignalR を有効にします。  
そして、送受信をするためのハブとして SensorHub クラスを作成しています。  
今回は、JavaScript の deviceorientation イベントで取得できる `alpha, beta, gamma` の値を引数で受け取ってクライアントに通知するだけです。  
なお、`Clients.Others` は送信者以外のクライアントを表します。

次に、クライアントとなる HTML を実装します。

https://gist.github.com/sakapon/bd27820c79c487ae43c7b709282cf312

センサーのデータを送信する sensor.html と、それを受信して表示する viewer.html に分かれています。  
各 JS ファイルを `<script>` で読み込み、`$.connection` から各機能にアクセスします。

前回: [transform と deviceorientation における回転の表現 (HTML)](HTML-Device-Orientation.md)

### 作成したサンプル
- [DeviceSignal](https://github.com/sakapon/DeviceSignal) (GitHub)

### バージョン情報
- .NET Framework 4.7
- ASP.NET SignalR 2.4.0
