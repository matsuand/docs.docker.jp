---
description: Rails を使って Docker Compose をはじめる。
keywords: documentation, docs, docker, compose, orchestration, containers
title: "クィックスタート: Compose と Rails"
---

{% comment %}
This Quickstart guide shows you how to use Docker Compose to set up and run
a Rails/PostgreSQL app. Before starting, [install Compose](install.md).
{% endcomment %}
このクィックスタートガイドでは Docker Compose を使って、簡単な Rails/PostgreSQL アプリを設定し実行する手順を示します。
はじめるには [Compose のインストール](install.md) が必要です。

{% comment %}
### Define the project
{% endcomment %}
### プロジェクトの定義
{: #define-the-project }


{% comment %}
Start by setting up the files needed to build the app. The app will run inside a Docker container containing its dependencies. Defining dependencies is done using a file called `Dockerfile`. To begin with, the
Dockerfile consists of:
{% endcomment %}
アプリのビルドに必要となるファイルを作るところから始めます。
アプリの依存パッケージも含め、アプリは Docker コンテナー内で実行するものとします。
依存パッケージは `Dockerfile` というファイル内に定義します。
まずは Dockerfile を以下のようにします。

    FROM ruby:2.5
    RUN apt-get update -qq && apt-get install -y nodejs postgresql-client
    RUN mkdir /myapp
    WORKDIR /myapp
    COPY Gemfile /myapp/Gemfile
    COPY Gemfile.lock /myapp/Gemfile.lock
    RUN bundle install
    COPY . /myapp

    # コンテナー起動時に毎回実行されるスクリプトを追加
    COPY entrypoint.sh /usr/bin/
    RUN chmod +x /usr/bin/entrypoint.sh
    ENTRYPOINT ["entrypoint.sh"]
    EXPOSE 3000

    # メインプロセスの起動
    CMD ["rails", "server", "-b", "0.0.0.0"]

{% comment %}
That'll put your application code inside an image that builds a container
with Ruby, Bundler and all your dependencies inside it. For more information on
how to write Dockerfiles, see the [Docker user guide](/get-started/index.md)
and the [Dockerfile reference](/engine/reference/builder/).
{% endcomment %}
上の設定はイメージ内部にアプリケーションコードを置きます。
このイメージは Ruby、Bundler などの依存パッケージすべてをコンテナー内部に含めてビルドされます。
Dockerfile の記述方法の詳細は [Docker ユーザーガイド](/get-started/index.md) や [Dockerfile リファレンス](/engine/reference/builder/) を参照してください。

{% comment %}
Next, create a bootstrap `Gemfile` which just loads Rails. It'll be overwritten
in a moment by `rails new`.
{% endcomment %}
次にブートストラップを行うファイル `Gemfile` を生成して、Rails をロードできるようにします。
このファイルは `rails new` を行ったタイミングで書き換わります。

    source 'https://rubygems.org'
    gem 'rails', '~>5'

{% comment %}
Create an empty `Gemfile.lock` to build our `Dockerfile`.
{% endcomment %}
空のファイル `Gemfile.lock` を生成して `Dockerfile` のビルドができるようにします。

    touch Gemfile.lock

{% comment %}
Next, provide an entrypoint script to fix a Rails-specific issue that
prevents the server from restarting when a certain `server.pid` file pre-exists.
This script will be executed every time the container gets started.
`entrypoint.sh` consists of:
{% endcomment %}
次にエントリーポイントの実行スクリプトを生成して、Rails 特有の問題へ対処します。
これはサーバー内に `server.pid` というファイルが先に存在していたときに、サーバーが再起動できなくなる問題を回避するものです。
このスクリプトは、コンテナーが起動されるたびに実行されることになります。
`entrypoint.sh` というこのスクリプトを、以下のように生成します。

{% comment %}
```bash
#!/bin/bash
set -e

# Remove a potentially pre-existing server.pid for Rails.
rm -f /myapp/tmp/pids/server.pid

# Then exec the container's main process (what's set as CMD in the Dockerfile).
exec "$@"
```
{% endcomment %}
```bash
#!/bin/bash
set -e

# Rails に対応したファイル server.pid が存在しているかもしれないので削除する。
rm -f /myapp/tmp/pids/server.pid

# コンテナーのプロセスを実行する。（Dockerfile 内の CMD に設定されているもの。）
exec "$@"
```

{% comment %}
Finally, `docker-compose.yml` is where the magic happens. This file describes
the services that comprise your app (a database and a web app), how to get each
one's Docker image (the database just runs on a pre-made PostgreSQL image, and
the web app is built from the current directory), and the configuration needed
to link them together and expose the web app's port.
{% endcomment %}
最後に `docker-compose.yml` が取りまとめてくれます。
このファイルには、データベースとウェブという 2 つのアプリを含んだサービスが定義されています。
そしてそれぞれの Docker イメージをどう作るかが示されています。
（データベースは既存の PostgreSQL イメージにより動作します。ウェブアプリはカレントディレクトリ内に生成されます。）
また、リンクによってそれを結び合わせることが設定されていて、ウェブアプリのポートは外部に公開されています。

    version: '3'
    services:
      db:
        image: postgres
        volumes:
          - ./tmp/db:/var/lib/postgresql/data
      web:
        build: .
        command: bash -c "rm -f tmp/pids/server.pid && bundle exec rails s -p 3000 -b '0.0.0.0'"
        volumes:
          - .:/myapp
        ports:
          - "3000:3000"
        depends_on:
          - db

{% comment %}
>**Tip**: You can use either a `.yml` or `.yaml` extension for this file.
{% endcomment %}
>**ヒント**: このファイルの拡張子は`.yml`と`.yaml`のどちらでも構いません。

{% comment %}
### Build the project
{% endcomment %}
### プロジェクトのビルド
{: #build-the-project }

{% comment %}
With those files in place, you can now generate the Rails skeleton app
using [docker-compose run](/compose/reference/run.md):
{% endcomment %}
ここまでのファイルを使って [docker-compose run](/compose/reference/run.md) を実行し、Rails アプリのひながたを生成します。

    docker-compose run web rails new . --force --no-deps --database=postgresql

{% comment %}
First, Compose builds the image for the `web` service using the
`Dockerfile`. Then it runs `rails new` inside a new container, using that
image. Once it's done, you should have generated a fresh app.
{% endcomment %}
最初に Compose は `Dockerfile` を用いて `web` サービスに対するイメージをビルドします。
そしてこのイメージを利用して、新たに生成されたコンテナー内にて `rails new` を実行します。
処理が完了すれば、できたてのアプリが生成されているはずです。

{% comment %}
List the files.
{% endcomment %}
ファイル一覧を見てみます。

```bash
$ ls -l
total 64
-rw-r--r--   1 vmb  staff   222 Jun  7 12:05 Dockerfile
-rw-r--r--   1 vmb  staff  1738 Jun  7 12:09 Gemfile
-rw-r--r--   1 vmb  staff  4297 Jun  7 12:09 Gemfile.lock
-rw-r--r--   1 vmb  staff   374 Jun  7 12:09 README.md
-rw-r--r--   1 vmb  staff   227 Jun  7 12:09 Rakefile
drwxr-xr-x  10 vmb  staff   340 Jun  7 12:09 app
drwxr-xr-x   8 vmb  staff   272 Jun  7 12:09 bin
drwxr-xr-x  14 vmb  staff   476 Jun  7 12:09 config
-rw-r--r--   1 vmb  staff   130 Jun  7 12:09 config.ru
drwxr-xr-x   3 vmb  staff   102 Jun  7 12:09 db
-rw-r--r--   1 vmb  staff   211 Jun  7 12:06 docker-compose.yml
-rw-r--r--   1 vmb  staff   184 Jun  7 12:08 entrypoint.sh
drwxr-xr-x   4 vmb  staff   136 Jun  7 12:09 lib
drwxr-xr-x   3 vmb  staff   102 Jun  7 12:09 log
-rw-r--r--   1 vmb  staff    63 Jun  7 12:09 package.json
drwxr-xr-x   9 vmb  staff   306 Jun  7 12:09 public
drwxr-xr-x   9 vmb  staff   306 Jun  7 12:09 test
drwxr-xr-x   4 vmb  staff   136 Jun  7 12:09 tmp
drwxr-xr-x   3 vmb  staff   102 Jun  7 12:09 vendor
```

{% comment %}
If you are running Docker on Linux, the files `rails new` created are owned by
root. This happens because the container runs as the root user. If this is the
case, change the ownership of the new files.
{% endcomment %}
Linux 上で Docker を利用している場合、`rails new` により生成されたファイルの所有者は root になります。
これはコンテナーが root ユーザーにより実行されているためです。
この場合は、生成されたファイルの所有者を以下のように変更してください。

```bash
sudo chown -R $USER:$USER .
```

{% comment %}
If you are running Docker on Mac or Windows, you should already have ownership
of all files, including those generated by `rails new`.
{% endcomment %}
Docker on Mac あるいは Docker on Windows を利用している場合、`rails new` により生成されたファイルも含め、すべてのファイルに対しての所有権は、正しく設定されているはずです。

{% comment %}
Now that you’ve got a new Gemfile, you need to build the image again. (This, and
changes to the `Gemfile` or the Dockerfile, should be the only times you’ll need
to rebuild.)
{% endcomment %}
ここに新たな Gemfile が作成されたので、イメージを再ビルドすることが必要です。
（再ビルドが必要になるのは、今の時点、あるいは `Gemfile` や Dockerfile を修正したときだけです。）

    docker-compose build


{% comment %}
### Connect the database
{% endcomment %}
### データベースの接続設定
{: #connect-the-database }

{% comment %}
The app is now bootable, but you're not quite there yet. By default, Rails
expects a database to be running on `localhost` - so you need to point it at the
`db` container instead. You also need to change the database and username to
align with the defaults set by the `postgres` image.
{% endcomment %}
アプリは実行可能ですが、実行するのはまだです。
デフォルトで Rails は `localhost` において実行されているデータベースを用います。
したがってここでは `db` コンテナーを用いるように書き換える必要があります。
また `postgres` イメージにおいて設定されているデフォルトのデータベース名、ユーザー名を変更することも必要です。

{% comment %}
Replace the contents of `config/database.yml` with the following:
{% endcomment %}
`config/database.yml` の記述内容を以下のように書き換えます。

```none
default: &default
  adapter: postgresql
  encoding: unicode
  host: db
  username: postgres
  password:
  pool: 5

development:
  <<: *default
  database: myapp_development


test:
  <<: *default
  database: myapp_test
```

{% comment %}
You can now boot the app with [docker-compose up](/compose/reference/up.md):
{% endcomment %}
[docker-compose up](/compose/reference/up.md) によりアプリを起動します。

    docker-compose up

{% comment %}
If all's well, you should see some PostgreSQL output.
{% endcomment %}
正常に動作すれば、PostgreSQL による出力が確認できるはずです。

```bash
rails_db_1 is up-to-date
Creating rails_web_1 ... done
Attaching to rails_db_1, rails_web_1
db_1   | PostgreSQL init process complete; ready for start up.
db_1   |
db_1   | 2018-03-21 20:18:37.437 UTC [1] LOG:  listening on IPv4 address "0.0.0.0", port 5432
db_1   | 2018-03-21 20:18:37.437 UTC [1] LOG:  listening on IPv6 address "::", port 5432
db_1   | 2018-03-21 20:18:37.443 UTC [1] LOG:  listening on Unix socket "/var/run/postgresql/.s.PGSQL.5432"
db_1   | 2018-03-21 20:18:37.726 UTC [55] LOG:  database system was shut down at 2018-03-21 20:18:37 UTC
db_1   | 2018-03-21 20:18:37.772 UTC [1] LOG:  database system is ready to accept connections
```

{% comment %}
Finally, you need to create the database. In another terminal, run:
{% endcomment %}
最後にデータベースを生成することが必要です。
別の端末から以下を実行します。

    docker-compose run web rake db:create

{% comment %}
Here is an example of the output from that command:
{% endcomment %}
コマンドから出力される結果は、たとえば以下のようになります。

```none
vmb at snapair in ~/sandbox/rails
$ docker-compose run web rake db:create
Starting rails_db_1 ... done
Created database 'myapp_development'
Created database 'myapp_test'
```

{% comment %}
### View the Rails welcome page!
{% endcomment %}
### Rails の「ようこそ」ページの確認
{: # }

{% comment %}
That's it. Your app should now be running on port 3000 on your Docker daemon.
{% endcomment %}
以上です。
Docker デーモンを通じて、アプリがポート 3000 番を使って実行されています。

{% comment %}
On Docker Desktop for Mac and Docker Desktop for Windows, go to `http://localhost:3000` on a web
browser to see the Rails Welcome.
{% endcomment %}
Docker Desktop for Mac や Docker Desktop for Windows の場合は、ウェブブラウザーから `http://localhost:3000` にアクセスすれば Rails のようこそページを確認できます。

{% comment %}
If you are using [Docker Machine](/machine/overview.md), then `docker-machine ip
MACHINE_VM` returns the Docker host IP address, to which you can append the port
(`<Docker-Host-IP>:3000`).
{% endcomment %}
[Docker Machine](/machine/overview.md) を利用している場合は、`docker-machine ip MACHINE_VM` を実行すると Docker ホストの IP アドレスを得ることができます。
これにポート番号をつけて利用します。
（`<Docker-Host-IP>:3000`）

{% comment %}
![Rails example](images/rails-welcome.png)
{% endcomment %}
![Rails の例](images/rails-welcome.png)

{% comment %}
### Stop the application
{% endcomment %}
### アプリケーションの停止
{: #stop-the-application }

{% comment %}
To stop the application, run [docker-compose down](/compose/reference/down.md) in
your project directory. You can use the same terminal window in which you
started the database, or another one where you have access to a command prompt.
This is a clean way to stop the application.
{% endcomment %}
アプリケーションを停止するには、プロジェクトディレクトリにおいて [docker-compose down](/compose/reference/down.md) を実行します。
この場合に用いる端末画面は、データベースを起動したときと同じものを用いるか、あるいはコマンドプロンプトにアクセスできる別画面であっても構いません。
これがアプリケーションを適切に停止する方法です。

```none
vmb at snapair in ~/sandbox/rails
$ docker-compose down
Stopping rails_web_1 ... done
Stopping rails_db_1 ... done
Removing rails_web_run_1 ... done
Removing rails_web_1 ... done
Removing rails_db_1 ... done
Removing network rails_default

```

{% comment %}
### Restart the application
{% endcomment %}
### アプリケーションの再起動
{: #restart-the-application }

{% comment %}
To restart the application run `docker-compose up` in the project directory.
{% endcomment %}
アプリケーションを再起動する場合は、プロジェクトディレクトリにて `docker-compose up` を実行します。

{% comment %}
### Rebuild the application
{% endcomment %}
### アプリケーションの再ビルド
{: #rebuild-the-application }

{% comment %}
If you make changes to the Gemfile or the Compose file to try out some different
configurations, you need to rebuild. Some changes require only
`docker-compose up --build`, but a full rebuild requires a re-run of
`docker-compose run web bundle install` to sync changes in the `Gemfile.lock` to
the host, followed by `docker-compose up --build`.
{% endcomment %}
Gemfile や Compose ファイルを編集して、いろいろと別の設定とした場合には、再ビルドが必要になります。
変更内容によっては `docker-compose up --build` だけで済む場合もあります。
しかし完全に再ビルドを行うには、`docker-compose run web bundle install` を再度実行して、ホストにおける `Gemfile.lock` の変更と同期を取ることが必要になります。
その後に `docker-compose up --build` を実行します。

{% comment %}
Here is an example of the first case, where a full rebuild is not necessary.
Suppose you simply want to change the exposed port on the local host from `3000`
in our first example to `3001`. Make the change to the Compose file to expose
port `3000` on the container through a new port, `3001`, on the host, and save
the changes:
{% endcomment %}
以下に示すのは前者、つまり完全な再ビルドは必要としない例です。
ローカルホスト側の公開ポートを `3000` から `3001` に変更する場合を取り上げます。
Compose ファイルにおいて、コンテナー側にて `3000` としているポートを新たなポート `3001` に変更します。
そしてこの変更を保存します。

```none
ports: - "3001:3000"
```

{% comment %}
Now, rebuild and restart the app with `docker-compose up --build`.
{% endcomment %}
再ビルドとアプリの再起動は  `docker-compose up --build` により行います。

{% comment %}
Inside the container, your app is running on the same port as before `3000`, but
the Rails Welcome is now available on `http://localhost:3001` on your local
host.
{% endcomment %}
コンテナー内部において、アプリはそれまでと変わらないポート `3000` で稼動していますが、ローカルホスト上から Rails ようこそページにアクセスするのは `http://localhost:3001` となります。

{% comment %}
## More Compose documentation
{% endcomment %}
## Compose ドキュメント
{: #more-compose-documentation }

{% comment %}
- [User guide](index.md)
- [Installing Compose](install.md)
- [Getting Started](gettingstarted.md)
- [Get started with Django](django.md)
- [Get started with WordPress](wordpress.md)
- [Command line reference](/compose/reference/index.md)
- [Compose file reference](/compose/compose-file/index.md)
{% endcomment %}
- [ユーザーガイド](index.md)
- [Compose のインストール](install.md)
- [はじめよう](gettingstarted.md)
- [Django を使ってはじめよう](django.md)
- [WordPress を使ってはじめよう](wordpress.md)
- [コマンドラインリファレンス](/compose/reference/index.md)
- [Compose ファイルリファレンス](/compose/compose-file/index.md)
