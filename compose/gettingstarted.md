---
description: Docker Compose をはじめよう。
keywords: documentation, docs, docker, compose, orchestration, containers
title: Docker Compose をはじめよう
---

{% comment %}
On this page you build a simple Python web application running on Docker
Compose. The application uses the Flask framework and maintains a hit counter in
Redis. While the sample uses Python, the concepts demonstrated here should be
understandable even if you're not familiar with it.
{% endcomment %}
このページでは、簡単な Python ウェブアプリケーションを Docker Compose 上に作り上げます。
このアプリケーションは Flask フレームワークを利用し、Redis によりアクセスカウンターを管理します。
サンプルでは Python を用いていますが、ここで示す考え方は Python に不慣れな方でも理解できるようにしています。

{% comment %}
## Prerequisites
{% endcomment %}
{: #prerequisites }
## 前提条件

{% comment %}
Make sure you have already installed both [Docker Engine](../get-docker.md)
and [Docker Compose](install.md). You don't need to install Python or Redis, as
both are provided by Docker images.
{% endcomment %}
[Docker Engine](../get-docker.md) と [Docker Compose](install.md) をインストールしておいてください。
Python や Redis はインストールする必要はありません。
これはいずれも Docker イメージとして提供されます。

{% comment %}
## Step 1: Setup
{% endcomment %}
{: #step-1-setup }
## ステップ 1: セットアップ

{% comment %}
Define the application dependencies.
{% endcomment %}
アプリケーションの依存パッケージを定義します。

{% comment %}
1.  Create a directory for the project:
{% endcomment %}
1.  プロジェクト用のディレクトリを生成します。

        $ mkdir composetest
        $ cd composetest

{% comment %}
2.  Create a file called `app.py` in your project directory and paste this in:
{% endcomment %}
2.  プロジェクトディレクトリ内に `app.py` というファイルを生成して、以下の記述を書き写してください。

        import time

        import redis
        from flask import Flask

        app = Flask(__name__)
        cache = redis.Redis(host='redis', port=6379)


        def get_hit_count():
            retries = 5
            while True:
                try:
                    return cache.incr('hits')
                except redis.exceptions.ConnectionError as exc:
                    if retries == 0:
                        raise exc
                    retries -= 1
                    time.sleep(0.5)


        @app.route('/')
        def hello():
            count = get_hit_count()
            return 'Hello World! I have been seen {} times.\n'.format(count)


      {% comment %}
      In this example, `redis` is the hostname of the redis container on the
      application's network. We use the default port for Redis, `6379`.
      {% endcomment %}
      この例において `redis` とは、このアプリケーションネットワーク上の redis コンテナーのホスト名です。
      Redis のデフォルトポートとして `6379` を利用します。

      {% comment %}
      > Handling transient errors
      >
      > Note the way the `get_hit_count` function is written. This basic retry
      > loop lets us attempt our request multiple times if the redis service is
      > not available. This is useful at startup while the application comes
      > online, but also makes our application more resilient if the Redis
      > service needs to be restarted anytime during the app's lifetime. In a
      > cluster, this also helps handling momentary connection drops between
      > nodes.
      {% endcomment %}
      > 一時的なエラーの取り扱い
      >
      > `get_hit_count` という関数がどのように書かれているかを見てください。
      > この単純なリトライのループにより、redis サービスが起動していなかったとしても、リクエストを何度でも送信できます。
      > アプリケーションが起動する最中に対して、この方法が適していますが、さらにはこのアプリの動作中に redis サービスを再起動する必要が発生した場合も、アプリが柔軟に対応できる方法です。
      > クラスターを構成している場合、ノード間でのネットワークの瞬断を制御することもできます。


{% comment %}
3.  Create another file called `requirements.txt` in your project directory and
paste this in:
{% endcomment %}
3.  プロジェクト用のディレクトリにもう一つ ``requirements.txt`` という名称のファイルを作成し、次のようにします。

        flask
        redis

{% comment %}
## Step 2: Create a Dockerfile
{% endcomment %}
{: #step-2-create-a-dockerfile }
## ステップ 2: Dockerfile の生成

{% comment %}
In this step, you write a Dockerfile that builds a Docker image. The image
contains all the dependencies the Python application requires, including Python
itself.
{% endcomment %}
このステップでは、Docker イメージを構築する Dockerfile を作ります。
そのイメージには依存するすべてのもの、つまり Python と Python アプリケーションが含まれます。

{% comment %}
In your project directory, create a file named `Dockerfile` and paste the
following:
{% endcomment %}
プロジェクト用のディレクトリ内で `Dockerfile` という名称のファイルを作成し、次の内容にします。

    FROM python:3.7-alpine
    WORKDIR /code
    ENV FLASK_APP app.py
    ENV FLASK_RUN_HOST 0.0.0.0
    RUN apk add --no-cache gcc musl-dev linux-headers
    COPY requirements.txt requirements.txt
    RUN pip install -r requirements.txt
    EXPOSE 5000
    COPY . .
    CMD ["flask", "run"]

{% comment %}
This tells Docker to:
{% endcomment %}
これは Docker に対して以下の指示を行います。

{% comment %}
* Build an image starting with the Python 3.7 image.
* Set the working directory to `/code`.
* Set environment variables used by the `flask` command.
* Install gcc and other dependencies
* Copy `requirements.txt` and install the Python dependencies.
* Add metadata to the image to describe that the container is listening on port 5000
* Copy the current directory `.` in the project to the workdir `.` in the image.
* Set the default command for the container to `flask run`.
{% endcomment %}
* Python 3.7 イメージを使って当イメージを構築する。
* 作業用ディレクトリを `/code` に指定する。
* `flask`コマンドにより用いられる環境変数を設定する。
* gcc や依存パッケージをインストールする。
* `requirements.txt`をコピーして Python 依存パッケージをインストールする。
* イメージにメタデータを追加して、コンテナーがポート 5000 をリッスンするように記述する。
* このプロジェクトのカレントディレクトリ`.`を、イメージ内のワークディレクトリ`.`にコピーする。
* コンテナーに対するデフォルトのコマンドを `flask run` にする。

{% comment %}
For more information on how to write Dockerfiles, see the
[Docker user guide](../develop/index.md)
and the [Dockerfile reference](/engine/reference/builder/).
{% endcomment %}
Dockerfile の書き方の詳細については、[Docker ユーザーガイド](../develop/index.md)や [Dockerfile リファレンス](/engine/reference/builder/) をご覧ください。


{% comment %}
## Step 3: Define services in a Compose file
{% endcomment %}
{: #step-3-define-services-in-a-compose-file }
## ステップ 3: Compose ファイル内でのサービス定義

{% comment %}
Create a file called `docker-compose.yml` in your project directory and paste
the following:
{% endcomment %}
プロジェクト用のディレクトリ内で `Dockerfile` という名称のファイルを作成し、次の内容にします。

    version: '3'
    services:
      web:
        build: .
        ports:
          - "5000:5000"
      redis:
        image: "redis:alpine"

{% comment %}
This Compose file defines two services: `web` and `redis`.
{% endcomment %}
この Compose ファイルは `web` と `redis` という 2 つのサービスを定義します。

{% comment %}
### Web service
{% endcomment %}
{: #web-service }
### ウェブサービス

{% comment %}
The `web` service uses an image that's built from the `Dockerfile` in the current directory.
It then binds the container and the host machine to the exposed port, `5000`. This example service uses the default port for
the Flask web server, `5000`.
{% endcomment %}
`web` サービスは、カレントディレクトリ内の `Dockerfile` からビルドされたイメージを利用します。
そしてコンテナーとホストマシンを、公開用ポート `5000` でつなぎます。
このサービス例では、Flask ウェブサーバーのデフォルトポートである `5000` を利用するものです。

{% comment %}
### Redis service
{% endcomment %}
{: #redis-service }
### Redis サービス

{% comment %}
The `redis` service uses a public [Redis](https://registry.hub.docker.com/_/redis/)
image pulled from the Docker Hub registry.
{% endcomment %}
`redis` サービスには Docker Hub レジストリから取得した、公開の [Redis](https://registry.hub.docker.com/_/redis/) イメージを利用します。

{% comment %}
## Step 4: Build and run your app with Compose
{% endcomment %}
{: #step-4-build-and-run-your-app-with-compose }
## ステップ 4: Compose によるアプリケーションの構築と実行

{% comment %}
1.  From your project directory, start up your application by running `docker-compose up`.
{% endcomment %}
1.  プロジェクト用のディレクトリで `docker-compose up` によりアプリケーションを起動します。

    ```
    $ docker-compose up
    Creating network "composetest_default" with the default driver
    Creating composetest_web_1 ...
    Creating composetest_redis_1 ...
    Creating composetest_web_1
    Creating composetest_redis_1 ... done
    Attaching to composetest_web_1, composetest_redis_1
    web_1    |  * Running on http://0.0.0.0:5000/ (Press CTRL+C to quit)
    redis_1  | 1:C 17 Aug 22:11:10.480 # oO0OoO0OoO0Oo Redis is starting oO0OoO0OoO0Oo
    redis_1  | 1:C 17 Aug 22:11:10.480 # Redis version=4.0.1, bits=64, commit=00000000, modified=0, pid=1, just started
    redis_1  | 1:C 17 Aug 22:11:10.480 # Warning: no config file specified, using the default config. In order to specify a config file use redis-server /path/to/redis.conf
    web_1    |  * Restarting with stat
    redis_1  | 1:M 17 Aug 22:11:10.483 * Running mode=standalone, port=6379.
    redis_1  | 1:M 17 Aug 22:11:10.483 # WARNING: The TCP backlog setting of 511 cannot be enforced because /proc/sys/net/core/somaxconn is set to the lower value of 128.
    web_1    |  * Debugger is active!
    redis_1  | 1:M 17 Aug 22:11:10.483 # Server initialized
    redis_1  | 1:M 17 Aug 22:11:10.483 # WARNING you have Transparent Huge Pages (THP) support enabled in your kernel. This will create latency and memory usage issues with Redis. To fix this issue run the command 'echo never > /sys/kernel/mm/transparent_hugepage/enabled' as root, and add it to your /etc/rc.local in order to retain the setting after a reboot. Redis must be restarted after THP is disabled.
    web_1    |  * Debugger PIN: 330-787-903
    redis_1  | 1:M 17 Aug 22:11:10.483 * Ready to accept connections
    ```

    {% comment %}
    Compose pulls a Redis image, builds an image for your code, and starts the
    services you defined. In this case, the code is statically copied into the image at build time.
    {% endcomment %}
    Compose は Redis イメージを取得し、コードが動作するイメージを構築した上で、定義されているサービスを開始します。
    この例ではビルド時において、コードがイメージ内に静的にコピーされます。

{% comment %}
2.  Enter http://localhost:5000/ in a browser to see the application running.
{% endcomment %}
2.  ブラウザーで `http://localhost:5000/` を開き、アプリケーションの動作を確認します。

    {% comment %}
    If you're using Docker natively on Linux, Docker Desktop for Mac, or Docker Desktop for
    Windows, then the web app should now be listening on port 5000 on your
    Docker daemon host. Point your web browser to `http://localhost:5000` to
    find the `Hello World` message. If this doesn't resolve, you can also try
    `http://0.0.0.0:5000`.
    {% endcomment %}
    Docker を Linux、Docker Desktop for Mac、Docker Desktop for Windows で直接使っている場合、ウェブアプリは Docker デーモンのホスト上でポート 5000 を開いています。
    ブラウザーから http://localhost:5000 にアクセスして、`Hello World` メッセージが表示されることを確認してください。
    接続できなければ `http://127.0.0.1:5000` も試してください。

    {% comment %}
    If you're using Docker Machine on a Mac or Windows, use `docker-machine ip
    MACHINE_VM` to get the IP address of your Docker host. Then, open
    `http://MACHINE_VM_IP:5000` in a browser.
    {% endcomment %}
    Docker Machine on Mac や Docker Machine on Windows を利用している場合は、`docker-machine ip 仮想マシン名` を実行して Docker ホスト上の IP アドレスを取得します。
    そしてブラウザーから `http://仮想マシンのIP:5000` を開きます。

    {% comment %}
    You should see a message in your browser saying:
    {% endcomment %}
    ブラウザーには以下のメッセージが表示されます。

    ```
    Hello World! I have been seen 1 times.
    ```

    {% comment %}
    ![hello world in browser](images/quick-hello-world-1.png)
    {% endcomment %}
    ![ブラウザー上の hello world](images/quick-hello-world-1.png)

    {% comment %}
    3.  Refresh the page.
    {% endcomment %}
3.  ページを更新します。

    {% comment %}
    The number should increment.
    {% endcomment %}
    数値が更新されたはずです。

    ```
    Hello World! I have been seen 2 times.
    ```

    {% comment %}
    ![hello world in browser](images/quick-hello-world-2.png)
    {% endcomment %}
    ![ブラウザー上の hello world](images/quick-hello-world-2.png)

{% comment %}
4.  Switch to another terminal window, and type `docker image ls` to
    list local images.
{% endcomment %}
4.  別の端末画面を開いて `docker image ls` を実行し、ローカルのイメージ一覧を表示します。

    {% comment %}
    Listing images at this point should return `redis` and `web`.
    {% endcomment %}
    この時点で一覧表示されるイメージに `redis` と `web` が含まれます。

    ```
    $ docker image ls
    REPOSITORY              TAG                 IMAGE ID            CREATED             SIZE
    composetest_web         latest              e2c21aa48cc1        4 minutes ago       93.8MB
    python                  3.4-alpine          84e6077c7ab6        7 days ago          82.5MB
    redis                   alpine              9d8fa9aa0e5b        3 weeks ago         27.5MB
    ```

    {% comment %}
    You can inspect images with `docker inspect <tag or id>`.
    {% endcomment %}
    `docker inspect <tag または id>` によってイメージを確認することもできます。

{% comment %}
5.  Stop the application, either by running `docker-compose down`
from within your project directory in the second terminal, or by
hitting CTRL+C in the original terminal where you started the app.
{% endcomment %}
5.  アプリケーションを停止させます。
    2 つめに開いた端末画面上のプロジェクトディレクトリにおいて ``docker-compose down`` を実行します。
    またはアプリを開始したはじめの端末画面上において CTRL+C を入力します。

{% comment %}
## Step 5: Edit the Compose file to add a bind mount
{% endcomment %}
{: #step-5-edit-the-compose-file-to-add-a-bind-mount }
## ステップ 5: Compose ファイルにバインドマウントを追加

{% comment %}
Edit `docker-compose.yml` in your project directory to add a
[bind mount](../storage/bind-mounts.md) for the `web` service:
{% endcomment %}
プロジェクトディレクトリ内にある `docker-compose.yml` を編集して、`web` サービスへの [バインドマウント](../storage/bind-mounts.md) を追加します。

    version: '3'
    services:
      web:
        build: .
        ports:
          - "5000:5000"
        volumes:
          - .:/code
        environment:
          FLASK_ENV: development
      redis:
        image: "redis:alpine"

{% comment %}
The new `volumes` key mounts the project directory (current directory) on the
host to `/code` inside the container, allowing you to modify the code on the
fly, without having to rebuild the image. The `environment` key sets the
`FLASK_ENV` environment variable, which tells `flask run` to run in development
mode and reload the code on change. This mode should only be used in development.
{% endcomment %}
新しい `volumes` というキーは、ホスト上のプロジェクトディレクトリ（カレントディレクトリ）を、コンテナー内にある `/code` ディレクトリにマウントします。
こうすることで、イメージを再構築することなく、実行中のコードを修正できるようになります。
`environment`キーには環境変数`FLASK_ENV`を設定しています。
これは開発モードで`flask run`を実行し、コード変更時にリロードするよう指示します。
このモードは開発時にのみ用いるようにします。

{% comment %}
## Step 6: Re-build and run the app with Compose
{% endcomment %}
## ステップ 6: Compose によるアプリの再構築と実行
{: #step-6-re-build-and-run-the-app-with-compose }

{% comment %}
From your project directory, type `docker-compose up` to build the app with the updated Compose file, and run it.
{% endcomment %}
プロジェクトディレクトリにて `docker-compose up` を入力する際に、Compose ファイルが更新されていると、アプリは再構築され実行されます。

```
$ docker-compose up
Creating network "composetest_default" with the default driver
Creating composetest_web_1 ...
Creating composetest_redis_1 ...
Creating composetest_web_1
Creating composetest_redis_1 ... done
Attaching to composetest_web_1, composetest_redis_1
web_1    |  * Running on http://0.0.0.0:5000/ (Press CTRL+C to quit)
...
```

{% comment %}
Check the `Hello World` message in a web browser again, and refresh to see the
count increment.
{% endcomment %}
`Hello World` メッセージをもう一度確認してみます。
再読み込みをすると、さらにカウンターが増えるはずです。

{% comment %}
> Shared folders, volumes, and bind mounts
>
> * If your project is outside of the `Users` directory (`cd ~`), then you
need to share the drive or location of the Dockerfile and volume you are using.
If you get runtime errors indicating an application file is not found, a volume
mount is denied, or a service cannot start, try enabling file or drive sharing.
Volume mounting requires shared drives for projects that live outside of
`C:\Users` (Windows) or `/Users` (Mac), and is required for _any_ project on
Docker Desktop for Windows that uses [Linux containers](../docker-for-windows/index.md#switch-between-windows-and-linux-containers).
For more information, see [File sharing](../docker-for-mac/index.md#file-sharing) on Docker
for Mac, and the general examples on how to
> [Manage data in containers](../storage/volumes.md).
>
> * If you are using Oracle VirtualBox on an older Windows OS, you might encounter an issue with shared folders as described in this [VB trouble
ticket](https://www.virtualbox.org/ticket/14920). Newer Windows systems meet the
requirements for [Docker Desktop for Windows](../docker-for-windows/install.md) and do not
need VirtualBox.
{: .important}
{% endcomment %}
> フォルダ、ボリューム、バインドマウントの共有
>
> * プロジェクトを `Users` ディレクトリ（`cd ~`）以外に置いている場合、利用している Dockerfile やボリュームのドライブ、ディレクトリは、共有できるようにしておく必要があります。
>   実行時に、アプリケーションファイルが見つからない、ボリュームマウントが拒否される、サービスが起動できない、といったランタイムエラーが発生した場合は、ファイルやドライブを共有にすることを試してください。
>   `C:\Users`（Windows の場合）または `/Users`（Mac の場合）ではないディレクトリにあるプロジェクトがある場合は、ボリュームマウントを共有ドライブにする必要があります。
>   これはまた、[Linux コンテナー](../docker-for-windows/index.md#switch-between-windows-and-linux-containers) を利用する Docker Desktop  for Windows のプロジェクトでも同様です。
>   詳しくは Docker for Mac における [ファイル共有](../docker-for-mac/index.md#file-sharing) を参照してください。
>   また一般的な利用例に関しては [コンテナーでデータ管理](../storage/volumes.md) を参照してください。
>
> * 比較的古い Windows OS 上において Oracle VirtualBox を利用している場合は、[VB trouble ticket](https://www.virtualbox.org/ticket/14920) に示されている共有フォルダに関する問題が起こるかもしれません。
>   より新しい Windows システムであれば、[Docker Desktop for Windows](../docker-for-windows/install.md) の要件を満たすため、VirtualBox は必要としません。
{: .important}

{% comment %}
## Step 7: Update the application
{% endcomment %}
{: #step-7-update-the-application }
## ステップ 7: アプリケーションの更新

{% comment %}
Because the application code is now mounted into the container using a volume,
you can make changes to its code and see the changes instantly, without having
to rebuild the image.
{% endcomment %}
アプリケーションのコードは、ボリュームを利用してコンテナー内にマウントされたため、コードへの変更とその確認はすぐにできます。
イメージを再構築することは必要なくなりました。

{% comment %}
1.  Change the greeting in `app.py` and save it. For example, change the `Hello World!` message to `Hello from Docker!`:
{% endcomment %}
1.  `app.py` 内のメッセージを変更して保存します。
    たとえば `Hello World!` メッセージを `Hello from Docker!` に変更することにします。

    ```
    return 'Hello from Docker! I have been seen {} times.\n'.format(count)
    ```

{% comment %}
2.  Refresh the app in your browser. The greeting should be updated, and the
    counter should still be incrementing.
{% endcomment %}
2.  ブラウザーにてアプリを再読み込みします。
    メッセージは更新され、カウンターも加算されているはずです。

    {% comment %}
    ![hello world in browser](images/quick-hello-world-3.png)
    {% endcomment %}
    ![ブラウザー上の hello world](images/quick-hello-world-3.png)

{% comment %}
## Step 8: Experiment with some other commands
{% endcomment %}
{: #step-8-experiment-with-some-other-commands }
## ステップ 8: その他のコマンドを試す

{% comment %}
If you want to run your services in the background, you can pass the `-d` flag
(for "detached" mode) to `docker-compose up` and use `docker-compose ps` to
see what is currently running:
{% endcomment %}
サービスをバックグラウンドで実行したい場合は `docker-compose up` に `-d` フラグ（"デタッチ"モード用のフラグ）をつけます。
`docker-compose ps` を実行して、現在動いているものを確認します。

    $ docker-compose up -d
    Starting composetest_redis_1...
    Starting composetest_web_1...

    $ docker-compose ps
    Name                 Command            State       Ports
    -------------------------------------------------------------------
    composetest_redis_1   /usr/local/bin/run         Up
    composetest_web_1     /bin/sh -c python app.py   Up      5000->5000/tcp

{% comment %}
The `docker-compose run` command allows you to run one-off commands for your
services. For example, to see what environment variables are available to the
`web` service:
{% endcomment %}
`docker-compose run` コマンドを使えば、サービスに対してのコマンド実行を行うことができます。
たとえば `web` サービス上でどのような環境変数が利用可能であるかは、以下のコマンドを実行します。

    $ docker-compose run web env

{% comment %}
See `docker-compose --help` to see other available commands. You can also install [command completion](completion.md) for the bash and zsh shell, which also shows you available commands.
{% endcomment %}
`docker-compose --help` を実行すれば、その他のコマンドを確認できます。
bash や zsh シェルにおいて [コマンド補完](completion.md)をインストールしている場合は、利用可能なコマンドを確認することもできます。

{% comment %}
If you started Compose with `docker-compose up -d`, stop
your services once you've finished with them:
{% endcomment %}
`docker-compose up -d` により Compose を起動していた場合は、サービスを停止させるために以下のコマンドを実行します。

    $ docker-compose stop

{% comment %}
You can bring everything down, removing the containers entirely, with the `down`
command. Pass `--volumes` to also remove the data volume used by the Redis
container:
{% endcomment %}
コンテナーも完全に削除し、すべてを終わらせる場合には `down` コマンドを使います。
`--volumes` を指定すれば Redis コンテナーにおいて利用されているデータボリュームも削除することができます。

    $ docker-compose down --volumes

{% comment %}
At this point, you have seen the basics of how Compose works.
{% endcomment %}
ここまで Compose の基本動作について見てきました。


{% comment %}
## Where to go next
{% endcomment %}
{: #where-to-go-next }
## 次に読むものは

{% comment %}
- Next, try the quick start guide for [Django](django.md),
  [Rails](rails.md), or [WordPress](wordpress.md)
- [Explore the full list of Compose commands](reference/index.md)
- [Compose configuration file reference](compose-file/index.md)
- To learn more about volumes and bind mounts, see [Manage data in Docker](../storage/index.md)
{% endcomment %}
- 次は [Django](django.md)、[Rails](rails.md)、[WordPress](wordpress.md) のクイックスタートを試してみましょう。
- [Compose コマンドの全一覧](reference/index.md)
- [Compose ファイルリファレンス](compose-file/index.md)
- ボリュームやバインドマウントについての詳細は、[Docker でのデータ管理](../storage/index.md) を参照してください。
