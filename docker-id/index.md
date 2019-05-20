---
description: Docker ID のサインアップとログイン。
keywords: accounts, docker ID, billing, paid plans, support, Hub, Store, Forums, knowledge base, beta access
title: Docker ID アカウント
redirect_from:
- /docker-cloud/dockerid/
- /docker-hub/accounts/
---

{% comment %}
Your free Docker ID grants you access to Docker Hub repositories, and some beta programs. All you need is an email address.
{% endcomment %}
フリーの Docker ID を取得すると Docker Hub リポジトリへアクセスでき、ベータプログラムのいくつかを利用できます。
Docker ID の取得には電子メールアドレスのみが必要です。

{% comment %}
This account also allows you to log in to services such as the Docker Support
Center, the Docker Forums, and the Docker Success portal.
{% endcomment %}
このアカウントがあると、たとえば Docker Support Center、Docker フォーラム、Docker Success ポータルのようなサービスにもログインできるようになります。


{% comment %}
## Register for a Docker ID
{% endcomment %}
## Docker ID の登録
{: #register-for-a-docker-id }

{% comment %}
Your Docker ID becomes your user namespace for hosted Docker services, and becomes your username on the Docker Forums.
{% endcomment %}
Docker ID は、提供されている Docker サービスにおける各ユーザー向けの名前となり、Docker フォーラムでのユーザー名にもなります。

{% comment %}
1. Go to the [Docker Hub signup page](https://hub.docker.com/signup/).
{% endcomment %}
1. [Docker Hub サインアップページ](https://hub.docker.com/signup/)にアクセスします。

{% comment %}
2. Enter a username that is also your Docker ID.
{% endcomment %}
2. ユーザー名を入力します。これが Docker ID となります。

    {% comment %}
    Your Docker ID must be between 4 and 30 characters long, and can only contain numbers and lowercase letters.
    {% endcomment %}
    Docker ID は 4 文字以上、30 文字までで、数字と英小文字のみを用います。

{% comment %}
3. Enter a unique, valid email address.
{% endcomment %}
3. ユニークで適正なメールアドレスを入力します。

{% comment %}
4. Enter a password between 6 and 128 characters long.
{% endcomment %}
4. パスワードを 6 文字以上 128 文字以下の範囲で入力します。

{% comment %}
3. Click **Sign up**.
{% endcomment %}
3. **Sign up** をクリックします。

   {% comment %}
   Docker sends a verification email to the address you provided.
   {% endcomment %}
   Docker からの確認メールが、入力されたメールアドレスに送信されます。

{% comment %}
4. Click the link in the email to verify your address.
{% endcomment %}
4. 受信したメール内のリンクをクリックしてメールアドレスを承認します。

{% comment %}
> **Note**: You cannot log in with your Docker ID until you verify your email address.
{% endcomment %}
> **メモ**: メールアドレスの承認処理を終えてからでないと Docker ID を使ったログインはできません。


{% comment %}
## Log in
{% endcomment %}
## ログイン
{: #log-in }

{% comment %}
Once you register and verify your Docker ID email address, you can log in
to [Docker Hub](https://hub.docker.com) and [Docker Support](https://support.docker.com).
{% endcomment %}
Docker ID の登録を行ってメールアドレスの承認を終えたら [Docker Hub](https://hub.docker.com) や [Docker Support](https://support.docker.com) にログインできるようになります。

{% comment %}
![Login](images/login.png)
{% endcomment %}
![ログイン](images/login.png)

{% comment %}
You can also log in using the `docker login` command. (You can read more about `docker login` [here](/engine/reference/commandline/login.md).)
{% endcomment %}
`docker login` コマンドを使ってログインすることもできます。
（`docker login`の詳細は[こちら](/engine/reference/commandline/login.md)を参照してください。）

{% comment %}
> **Warning**:
> When you use the `docker login` command, your credentials are
stored in your home directory in `.docker/config.json`. The password is base64
encoded in this file. If you require secure storage for this password, use the
[Docker credential helpers](https://github.com/docker/docker-credential-helpers).
{:.warning}
{% endcomment %}
> **警告**:
> `docker login` コマンドを使ってログインすると、認証情報がホームディレクトリ配下の`.docker/config.json`に保存されます。
そしてパスワードは base64 エンコードにより保存されます。
より安全なパスワード保存を必要とする場合は [Docker credential helpers](https://github.com/docker/docker-credential-helpers) を利用してください。
{:.warning}
