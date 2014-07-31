## GitHub のアクセス トークン

Visual Studio や SourceTree などの Git クライアントから GitHub にアクセスできるように設定する方法です。
GitHub では 2 段階認証が推奨されるため、Git クライアントでアクセスするにはトークンを設定します。

### アクセス トークンを作成する
* GitHub の [Authorized applications](https://github.com/settings/applications) で、[Personal access tokens] の [Generate new token] をクリックする
* [Token description] には、アプリケーション名などを入力する
* [Select scopes] は、既定のままでよい
* 作成されたアクセス トークンは、一度しか表示されない

### SourceTree にアクセス トークンを設定する
* SourceTree の [ホストされたリポジトリ] で [アカウントを編集] をクリックする
* GitHub を選択して、ユーザー名を入力する
* [Password] には、アクセス トークンを設定する
  ![Access Tokens-1](Access%20Tokens-1.png)
  ![Access Tokens-2](Access%20Tokens-2.png)
