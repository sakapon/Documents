## How to Make Certificates

次の 2 種類の証明書を作成します。

* 自己署名証明書 (ルート証明書)
  * 証明書ストアの [信頼されたルート証明機関] に配置します。
  * いわゆる「オレオレ証明書」です。
* ルート証明書で署名された証明書
  * 証明書ストアの [個人]、[信頼された発行元] などに配置します。

各コマンドライン ツールを利用するため、Visual Studio 開発者コマンド プロンプトを使用すると便利です。

### 自己署名証明書 (ルート証明書) を作成する

必須項目のみでルート証明書を作成するには、次のコマンドを実行します。  
表示されるダイアログで、秘密キーを保護するためのパスワード (Subject Key) を指定します。  
.pvk に秘密キーがエクスポートされます。

```
makecert -n "CN=abc" -r -sv abc.pvk abc.cer
```

実際には、次のようにオプションを指定するとよいでしょう。

```
makecert -n "CN=Abc Root CA,O=Abc Company,C=JP" -a sha256 -b 01/01/2000 -e 01/01/2100 -r -sv abc-root.pvk abc-root.cer
```

主なスイッチの説明は以下の通りです。

* **-r** 自己署名であることを示します。  
  -r も親の証明書も指定されない場合、"Root Agency" というルート証明機関の下に証明書が作成されます。
* **-n** サブジェクト名を指定します。CN (Common Name) には "." を使用できますが、"," を使用できません。

.pfx を作成するには、次のコマンドを実行します。  
P@ssw0rd の部分には、先ほど設定したパスワードが入ります。

```
pvk2pfx -pvk abc-root.pvk -spc abc-root.cer -pfx abc-root.pfx -f -pi P@ssw0rd
```

### ルート証明書で署名された証明書を作成する

-iv, -ic にルート証明書を指定します。  
表示されるダイアログで、作成する証明書のパスワード (Subject Key) を指定するほか、ルート証明書のパスワード (Issuer Signature) も入力します。

```
makecert -n "CN=Abc Test,O=Abc Company,C=JP" -a sha256 -b 01/01/2000 -e 01/01/2100 -iv abc-root.pvk -ic abc-root.cer -sv abc-test.pvk abc-test.cer
```

.pfx を作成する方法は先ほどと同じです。

```
pvk2pfx -pvk abc-test.pvk -spc abc-test.cer -pfx abc-test.pfx -f -pi P@ssw0rd
```

### 証明書を実行してインストールする
ストアを指定しない場合、.pfx は [個人] に、.cer は [ほかの人] にインストールされます。  
.pfx をインストールするには、パスワードを入力します。

### 証明書をコマンドでインストールする
Certmgr.exe を使います。  
(.pfx には使えない？)

```
certmgr -add -c abc-root.cer -s root  
certmgr -add -c abc-test.cer -s my  
certmgr -add -c abc-test.cer -s trustedpublisher
```

### 証明書の一覧を表示する
Certmgr.exe を実行します。  

### 参照
[Makecert.exe (証明書作成ツール)](https://msdn.microsoft.com/library/bfsktky3.aspx)  
[Certmgr.exe (証明書マネージャー ツール)](https://msdn.microsoft.com/library/e78byta0.aspx)  
[Pvk2Pfx](https://msdn.microsoft.com/library/windows/hardware/ff550672.aspx)  
[方法 : 開発中に使用する一時的な証明書を作成する](https://msdn.microsoft.com/library/ms733813.aspx)  
証明書ストアの名前: [X509Store.Name プロパティ](https://msdn.microsoft.com/library/system.security.cryptography.x509certificates.x509store.name.aspx)

### その他
不明確なものも含まれます。

#### Makecert.exe のスイッチ

* **-pe** 証明書をエクスポート可能にします。
* **-sk** Subject Key。代わりに -sv を指定していれば不要？
* **-sky signature** ???
* **-sr** 同時にインストールさせる場合、「現在のユーザー」「コンピューター」を指定します。
* **-ss** 同時にインストールさせる場合、ストアの種類を指定します。
