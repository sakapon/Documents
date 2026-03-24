# Python セットアップ

### Python
- 最新版の Python 3.14 は Install Manager でインストール
- Python 3.13 以前は Microsoft Store でインストール
  - 公式サイトにも、単独のインストーラーがあるかもしれない

### ワークスペース
- ルート フォルダーに、ワークスペース ファイルおよびソース フォルダーを置く

### Visual Studio Code
- 拡張機能
  - Python
- venv
  - 右下の `Select interpreter` (または `3.13`) をクリック
  - 上で `Create virtual environment` をクリック
  - 使用する python.exe を選択
  - requirements.txt を選択

### venv で実行する
そのままでは venv では実行できない  
1回のみ、PowerShell を管理者権限で起動し、次を実行する
- `Set-ExecutionPolicy RemoteSigned`

```
.\.venv\Scripts\activate.bat
pip install -r requirements.txt
pip install xyz.whl (Rust)
```
