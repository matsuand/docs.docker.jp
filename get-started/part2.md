---
title: "はじめよう 2 部: コンテナー"
keywords: containers, python, code, coding, build, push, run
description: Docker 風に簡単なアプリを書き、ビルドし実行する方法を学びます。
---

{% include_relative nav.html selected="2" %}

{% comment %}
## Prerequisites
{% endcomment %}
{: #prerequisites }
## 前提条件

{% comment %}
- [Install Docker version 1.13 or higher](/engine/installation/).
- Read the orientation in [Part 1](index.md).
- Give your environment a quick test run to make sure you're all set up:
{% endcomment %}
- [Docker バージョン 1.13 またはそれ以上をインストールしていること](/engine/installation/)。
- [1 部](index.md) の概要を読んでいること。
- 以下のような簡単なコマンドを通じて、環境のセットアップ確認ができていること。

  ```shell
  docker run hello-world
  ```

{% comment %}
## Introduction
{% endcomment %}
{: #introduction }
## はじめに

{% comment %}
It's time to begin building an app the Docker way. We start at the bottom of the hierarchy of such app, a container, which this page covers. Above this level is a service, which defines how containers behave in
production, covered in [Part 3](part3.md). Finally, at the top level is the
stack, defining the interactions of all the services, covered in
[Part 5](part5.md).
{% endcomment %}
Docker におけるアプリケーション構築を始めましょう。
アプリケーション階層の最下部となるところから始めることにします。
これがコンテナーであり、このページにて取り扱います。
このレベルの上位にあるのがサービスです。
サービスは、アプリケーション実行中のコンテナーの動きを定義します。
このことは [3 部](part3.md) で扱います。
最後に、最上位にあるのがスタックです。
サービス間の対話方法を定義します。
これは [5 部](part5.md) で扱います。

{% comment %}
- Stack
- Services
- **Container** (you are here)
{% endcomment %}
- スタック
- サービス
- **コンテナー** (いまここにいます)

{% comment %}
## Your new development environment
{% endcomment %}
## 新しい開発環境
{: #your-new-development-environment }

{% comment %}
In the past, if you were to start writing a Python app, your first
order of business was to install a Python runtime onto your machine. But,
that creates a situation where the environment on your machine needs to be
perfect for your app to run as expected, and also needs to match your production
environment.
{% endcomment %}
Python アプリケーションを書き始めるにあたり、自分のマシン上に Python ランタイムをインストールするのが、これまでは一番初めの仕事でした。しかし、サーバー上でもアプリケーションが期待するとおりに問題なく動作するには、マシンと同じ環境を作成しなくてはいけません。

{% comment %}
With Docker, you can just grab a portable Python runtime as an image, no
installation necessary. Then, your build can include the base Python image
right alongside your app code, ensuring that your app, its dependencies, and the
runtime, all travel together.
{% endcomment %}
Docker であれば、移動可能な Python ランタイムをイメージ内に収容しているため、インストールは不要です。
そして、ベース Python イメージにはアプリのコードも一緒に構築できますし、アプリを確実に動かすための依存関係やランタイムもすべて運べます。

{% comment %}
These portable images are defined by something called a `Dockerfile`.
{% endcomment %}
移動可能なイメージは `Dockerfile` と呼ばれるファイルにより定義します。

{% comment %}
## Define a container with `Dockerfile`
{% endcomment %}
## `Dockerfile` によるコンテナー定義
{: #define-a-container-with-Dockerfile }

{% comment %}
`Dockerfile` defines what goes on in the environment inside your
container. Access to resources like networking interfaces and disk drives is
virtualized inside this environment, which is isolated from the rest of your
system, so you need to map ports to the outside world, and
be specific about what files you want to "copy in" to that environment. However,
after doing that, you can expect that the build of your app defined in this
`Dockerfile` behaves exactly the same wherever it runs.
{% endcomment %}
`Dockerfile` では、コンテナー内の環境で何をするかを定義します。
ネットワークインターフェースとディスクドライバーのようなリソースは、システム上の他の環境からは隔離された環境内に仮想化されています。
このようなリソースに接続するには、ポートを外の世界にマッピング（割り当て）する必要がありますし、どのファイルを環境に「複製」（copy in）するか指定する必要もあります。
しかしながら、これらの作業を `Dockerfile` における構築時の定義で済ませておけば、どこで実行しても同じ挙動となります。

### `Dockerfile`
{: #dockerfile }

{% comment %}
Create an empty directory on your local machine. Change directories (`cd`) into the new directory,
create a file called `Dockerfile`, copy-and-paste the following content into
that file, and save it. Take note of the comments that explain each statement in
your new Dockerfile.
{% endcomment %}
空ディレクトリを作成します。
新しいディレクトリ内にディレクトリを変更（`cd`）し、`Dockerfile` という名前のファイルを作成し、以降の内容をファイルにコピー＆ペーストし、保存します。
なお、Dockerfile のコメントは、各命令文に対する説明です。

```dockerfile
# 公式 Python ランタイムを親イメージとして使用
FROM python:2.7-slim

# 作業ディレクトリを /app に設定
WORKDIR /app

# 現在のディレクトリの内容を、コンテナー内の /app にコピー
COPY . /app

# requirements.txt で指定された必要なパッケージをすべてインストール
RUN pip install --trusted-host pypi.python.org -r requirements.txt

# ポート 80 番をコンテナーの外の世界でも利用可能に
EXPOSE 80

# 環境変数の定義
ENV NAME World

# コンテナー起動時に app.py を実行
CMD ["python", "app.py"]
```

{% comment %}
This `Dockerfile` refers to a couple of files we haven't created yet, namely
`app.py` and `requirements.txt`. Let's create those next.
{% endcomment %}
この `Dockerfile` は、`app.py` と `requirements.txt` といった、まだ作成していないファイルを参照しています。
次はこれらを作りましょう。

{% comment %}
## The app itself
{% endcomment %}
## アプリそのもの
{: #the-app-itself }

{% comment %}
Create two more files, `requirements.txt` and `app.py`, and put them in the same
folder with the `Dockerfile`. This completes our app, which as you can see is
quite simple. When the above `Dockerfile` is built into an image, `app.py` and
`requirements.txt` is present because of that `Dockerfile`'s `COPY` command,
and the output from `app.py` is accessible over HTTP thanks to the `EXPOSE`
command.
{% endcomment %}
さらに２つのファイルを作成します。
`requirements.txt` と `app.py` です。
これらを `Dockerfile` と同じフォルダに入れます。
アプリは見てのとおり、極めて単純になります。
先ほどの `Dockerfile` でイメージの構築時、 `Dockerfile` の `COPY` 命令で `app.py` と `requirements.txt` をイメージの中に組み込みます。

### `requirements.txt`
{: #requirementstxt }

```
Flask
Redis
```

### `app.py`
{: #apppy }

```python
from flask import Flask
from redis import Redis, RedisError
import os
import socket

# Redis に接続
redis = Redis(host="redis", db=0, socket_connect_timeout=2, socket_timeout=2)

app = Flask(__name__)

@app.route("/")
def hello():
    try:
        visits = redis.incr("counter")
    except RedisError:
        visits = "<i>cannot connect to Redis, counter disabled</i>"

    html = "<h3>Hello {name}!</h3>" \
           "<b>Hostname:</b> {hostname}<br/>" \
           "<b>Visits:</b> {visits}"
    return html.format(name=os.getenv("NAME", "world"), hostname=socket.gethostname(), visits=visits)

if __name__ == "__main__":
    app.run(host='0.0.0.0', port=80)
```

{% comment %}
Now we see that `pip install -r requirements.txt` installs the Flask and Redis
libraries for Python, and the app prints the environment variable `NAME`, as
well as the output of a call to `socket.gethostname()`. Finally, because Redis
isn't running (as we've only installed the Python library, and not Redis
itself), we should expect that the attempt to use it here fails and produces
the error message.
{% endcomment %}
先ほどの `pip install -r requirements.txt` で Python 用の Flask と Redis ライブラリをインストールします。
そして、アプリは環境変数 `NAME` を表示し、また `socket.gethostname()` を呼び出した結果も出力します。
しかしながら、 Redis は実行できないため（Python ライブラリをインストールしただけであり、 Redis 自身は入っていません）、実行を試みても失敗し、エラーメッセージを表示するでしょう。

{% comment %}
> **Note**: Accessing the name of the host when inside a container retrieves the
container ID, which is like the process ID for a running executable.
{% endcomment %}
> **メモ**: コンテナー内でホスト名の取得を試みると、コンテナー ID を返します。
コンテナー ID は実行バイナリにおけるプロセス ID のようなものです。

{% comment %}
That's it! You don't need Python or anything in `requirements.txt` on your
system, nor does building or running this image install them on your system. It
doesn't seem like you've really set up an environment with Python and Flask, but
you have.
{% endcomment %}
以上です！
システム上に Python や `requirements.txt` に書かれているどれもが不要であり、それどころか、システム上にイメージの構築や実行も不要なのです。
一見しますと環境に Python と Flask をインストールしていませんが、すでに持っているのです。

{% comment %}
## Build the app
{% endcomment %}
## アプリの構築
{: #build-the-app }

{% comment %}
We are ready to build the app. Make sure you are still at the top level of your
new directory. Here's what `ls` should show:
{% endcomment %}
アプリを構築する準備が整いました。
まだ、新しく作成したディレクトリのトップレベルにいるのを確認します。
ここでは `ls` は次のようになるでしょう。

```shell
$ ls
Dockerfile		app.py			requirements.txt
```

{% comment %}
Now run the build command. This creates a Docker image, which we're going to
name using the `--tag` option. Use `-t` if you want to use the shorter option.
{% endcomment %}
次は構築コマンドを実行します。
これは Docker イメージを作成します。
イメージには分かりやすい名前として `--tag` オプションによりタグを指定します。
短いオプションとして `-t` を使っても構いません。

```shell
docker build --tag=friendlyhello .
```

{% comment %}
Where is your built image? It's in your machine's local Docker image registry:
{% endcomment %}
構築したイメージはどこにあるのでしょうか？
マシン上のローカルにある Docker イメージレジストリの中です。

```shell
$ docker image ls

REPOSITORY            TAG                 IMAGE ID
friendlyhello         latest              326387cea398

```

{% comment %}
Note how the tag defaulted to `latest`. The full syntax for the tag option would
be something like `--tag=friendlyhello:v0.0.1`.
{% endcomment %}
タグ名がデフォルトの `latest` となったことに注意してください。
タグオプションの適切な文法は `--tag=friendlyhello:v0.0.1` というものです。


{% comment %}
>  Troubleshooting for Linux users
>
> _Proxy server settings_
>
> Proxy servers can block connections to your web app once it's up and running.
> If you are behind a proxy server, add the following lines before `RUN pip` in your
> Dockerfile, using the `ENV` command to specify the host and port for your
> proxy servers:
>
> ```conf
> # Set proxy server, replace host:port with values for your servers
> ENV http_proxy host:port
> ENV https_proxy host:port
> ```
>
> _DNS settings_
>
> DNS misconfigurations can generate problems with `pip`. You need to set your
> own DNS server address to make `pip` work properly. You might want
> to change the DNS settings of the Docker daemon. You can edit (or create) the
> configuration file at `/etc/docker/daemon.json` with the `dns` key, as following:
>
> ```json
>{
>   "dns": ["your_dns_address", "8.8.8.8"]
>}
> ```
>
> In the example above, the first element of the list is the address of your DNS
> server. The second item is Google's DNS which can be used when the first one is
> not available.
>
> Before proceeding, save `daemon.json` and restart the docker service.
>
> `sudo service docker restart`
>
> Once fixed, retry to run the `build` command.
>
> _MTU settings_
>
> If the MTU (default is 1500) on the default bridge network is greater than the MTU of the host external network, then `pip` fails. Set the MTU of the docker bridge network to match that of the host by editing (or creating) the configuration file at `/etc/docker/daemon.json` with the `mtu` key, as follows:
>
> ```json
>{
>   "mtu": 1450
>}
> ```
> Before proceeding, save `daemon.json` and restart the docker service.
>
> `sudo systemctl restart docker`
>
> Re-run the `build` command.
{% endcomment %}
>  Linux ユーザー向けのトラブルシューティング
>
> _Proxy サーバー設定_
>
> プロキシーサーバーが稼動していると、ウェブアプリへの接続がブロックされることがあります。
> プロキシーサーバーを動かしているなら、Dockerfile における `RUN pip` の記述行より前に、以下の記述を追加してください。
> これは `ENV` コマンドを使って、プロキシーサーバーのホストとポートを指定するものです。
>
> ```conf
> # プロキシーサーバー設定、host:port 部分は各自のサーバー向けに置き換える
> ENV http_proxy host:port
> ENV https_proxy host:port
> ```
>
> _DNS セッティング_
>
> DNS 設定に誤りがあると `pip` において問題が発生することがあります。
> `pip` を正しく動作させるためには、DNS サーバーアドレスを正しく設定する必要があります。
> Docker デーモンの DNS 設定を変更することになります。
> 設定ファイル `/etc/docker/daemon.json` を編集（または新規生成）し、以下のように `dns` キーを加えます。
>
> ```json
>{
>   "dns": ["your_dns_address", "8.8.8.8"]
>}
> ```
>
> 上の例においてリストの 1 つめは DNS サーバーのアドレスです。
> また 2 つめは Google の DNS サーバーのアドレスであり、1 つめが利用できないときに利用されます。
>
> 設定を有効にするには `daemon.json` を保存し、docker サービスを再起動します。
>
> `sudo service docker restart`
>
> 設定ができたら、再度 `build` コマンドを実行します。
>
> _MTU 設定_
>
> デフォルトブリッジネットワーク上の MTU（デフォルトは 1500）がホストの外部ネットワークにおける MTU を越えている場合には `pip` が失敗します。
> この場合は Docker ブリッジネットワークの MTU の値をホストに合わせるようにしてください。
> これは設定ファイル `/etc/docker/daemon.json` を編集（または新規生成）して、キー項目 `mtu` を以下のように設定します。
>
> ```json
>{
>   "mtu": 1450
>}
> ```
> 処理を進めるには、まずこの `daemon.json` を保存した上で Docker サービスを再起動します。
>
> `sudo systemctl restart docker`
>
> そしてもう一度 `build` コマンドを実行してください。

{% comment %}
## Run the app
{% endcomment %}
## アプリの実行
{: #run-the-app }

{% comment %}
Run the app, mapping your machine's port 4000 to the container's published port
80 using `-p`:
{% endcomment %}
アプリの実行にあたり、マシン側のポート 4000 をコンテナーの公開ポート 80 に割り当てるには `-p` を使います。

```shell
docker run -p 4000:80 friendlyhello
```

{% comment %}
You should see a message that Python is serving your app at `http://0.0.0.0:80`.
But that message is coming from inside the container, which doesn't know you
mapped port 80 of that container to 4000, making the correct URL
`http://localhost:4000`.
{% endcomment %}
Python がアプリに提供するのは `http://0.0.0.0:80` であることに注意して下さい。
しかしこれはコンテナー内で表示されるメッセージであり、コンテナー内からはコンテナーのポート 80 番からポート 4000 への割り当ては分かりません。
適切な URL は `http://localhost:4000` です。

{% comment %}
Go to that URL in a web browser to see the display content served up on a
web page.
{% endcomment %}
ウェブブラウザーで URL を開いて、出力内容がウェブページに表示されていることを確認してください。

![Hello World in browser](images/app-in-browser.png)

{% comment %}
> **Note**: If you are using Docker Toolbox on Windows 7, use the Docker Machine IP
> instead of `localhost`. For example, http://192.168.99.100:4000/. To find the IP
> address, use the command `docker-machine ip`.
{% endcomment %}
> **メモ**: Windows 7 上でDocker Toolbox を利用している場合は、`localhost` ではなく Docker Machine の IP アドレスを利用してください。
> たとえば http://192.168.99.100:4000/ といった具合です。
> IP アドレスを調べるには `docker-machine ip` コマンドを実行します。

{% comment %}
You can also use the `curl` command in a shell to view the same content.
{% endcomment %}
シェル上で `curl` コマンドを実行しても、同じ内容を表示します。

```shell
$ curl http://localhost:4000

<h3>Hello World!</h3><b>Hostname:</b> 8fc990912a14<br/><b>Visits:</b> <i>cannot connect to Redis, counter disabled</i>
```

{% comment %}
This port remapping of `4000:80` demonstrates the difference
between `EXPOSE` within the `Dockerfile` and what the `publish` value is set to when running
`docker run -p`. In later steps, map port 4000 on the host to port 80
in the container and use `http://localhost`.
{% endcomment %}
このポート `4000:80` の再割り当ては、`Dockerfile`の `EXPOSE` での指定とは異なるポートを指定できるデモです。
ここでは、`docker run -p` で何を公開（`publish`）するかを指定しました。
後の手順では、ホストのポート 80 をコンテナー内のポート 80 に割り当て、`http://localhost` で接続します。

{% comment %}
Hit `CTRL+C` in your terminal to quit.
{% endcomment %}
ターミナル上で `CTRL+C` を実行し、終了します。

 {% comment %}
 > On Windows, explicitly stop the container
 >
 > On Windows systems, `CTRL+C` does not stop the container. So, first
 type `CTRL+C` to get the prompt back (or open another shell), then type
 `docker container ls` to list the running containers, followed by
 `docker container stop <Container NAME or ID>` to stop the
 container. Otherwise, you get an error response from the daemon
 when you try to re-run the container in the next step.
 {% endcomment %}
 > Windows においてはコンテナーを明示的に停止
 >
 > Windows システムでは `CTRL+C` を入力してもコンテナーを止めることはできません。
 > このため、まず `CTRL+C` を入力してプロンプトに戻り（あるいは別のシェルを起動して）、
 > `docker container ls` を入力して実行中のコンテナーを確認します。
 > そして `docker container stop <コンテナー名 または コンテナー ID>` を入力してコンテナーを停止させます。
 > こうしておかないと、次にコンテナーを再起動しようとしたときに、デーモンがエラーを出力することになります。

{% comment %}
Now let's run the app in the background, in detached mode:
{% endcomment %}
次はアプリをバックグラウンドで起動するため、デタッチモード（detached mode）で実行します。

```shell
docker run -d -p 4000:80 friendlyhello
```

{% comment %}
You get the long container ID for your app and then are kicked back to your
terminal. Your container is running in the background. You can also see the
abbreviated container ID with `docker container ls` (and both work interchangeably when
running commands):
{% endcomment %}
コマンドを実行しますと、アプリの長いコンテナー ID を表示し、ターミナルに戻ります。
コンテナーはバックグラウンドで実行中です。
なお、`docker container ls` で短縮コンテナー ID を確認できます（コマンド実行時は、長いコンテナー ID と短縮 ID のどちらも利用できます）。

```shell
$ docker container ls
CONTAINER ID        IMAGE               COMMAND             CREATED
1fa4ab2cf395        friendlyhello       "python app.py"     28 seconds ago
```

{% comment %}
Notice that `CONTAINER ID` matches what's on `http://localhost:4000`.
{% endcomment %}
このように `http://localhost:4000` で表示したものと同じコンテナー ID （`CONTAINER ID`）が表示されます。

{% comment %}
Now use `docker container stop` to end the process, using the `CONTAINER ID`, like so:
{% endcomment %}
あとは、プロセスを停止するために `docker stop` コマンドでコンテナー ID を次のように指定します。

```shell
docker container stop 1fa4ab2cf395
```

{% comment %}
## Share your image
{% endcomment %}
## イメージの共有
{: #share-your-image}

{% comment %}
To demonstrate the portability of what we just created, let's upload our built
image and run it somewhere else. After all, you need to know how to push to
registries when you want to deploy containers to production.
{% endcomment %}
生成したばかりのイメージの可搬性を示すため、このイメージを別のところにアップロードしてみます。
なお、コンテナーを本番環境へデプロイするには、レジストリへ送信する方法を知っておく必要があります。

{% comment %}
A registry is a collection of repositories, and a repository is a collection of
images&#8212;sort of like a GitHub repository, except the code is already built.
An account on a registry can create many repositories. The `docker` CLI uses
Docker's public registry by default.
{% endcomment %}
レジストリというものはレポジトリの集まりのことです。
そのレポジトリとはイメージの集まりです。
GitHub リポジトリのようなものです。
ただしコードのビルドがすでに済んでいるというところが GitHub とは違います。
レジストリのアカウントからはレポジトリをたくさん作ることができます。
`docker` CLI はデフォルトとして Docker 公開リポジトリを利用します。

{% comment %}
> **Note**: We use Docker's public registry here just because it's free
and pre-configured, but there are many public ones to choose from, and you can
even set up your own private registry using [Docker Trusted
Registry](/datacenter/dtr/2.2/guides/).
{% endcomment %}
> **メモ**: Docker 公開リポジトリをここで利用するのは、これが無償であり事前に設定されているものだからです。
> ただし公開リポジトリは他にもあるので自由に選んでください。
> あるいは [Docker Trusted Registry](/datacenter/dtr/2.2/guides/) を利用すれば、独自のプライベートレジストリを設定することもできます。

{% comment %}
### Log in with your Docker ID
{% endcomment %}
### Docker ID でログイン
{: #log-in-with-your-docker-id }

{% comment %}
If you don't have a Docker account, sign up for one at
[hub.docker.com](https://hub.docker.com){: target="_blank" class="_" }.
Make note of your username.
{% endcomment %}
Docker アカウントを持っていない方は、[hub.docker.com](https://hub.docker.com){: target="_blank" class="_" }
にサインアップして取得してください。
ユーザー名は控えておいてください。

{% comment %}
Log in to the Docker public registry on your local machine.
{% endcomment %}
ローカルマシンから Docker の公開レジストリへログインします。

```shell
$ docker login
```

{% comment %}
### Tag the image
{% endcomment %}
### イメージへのタグづけ
{: #tag-the-image }

{% comment %}
The notation for associating a local image with a repository on a registry is
`username/repository:tag`. The tag is optional, but recommended, since it is
the mechanism that registries use to give Docker images a version. Give the
repository and tag meaningful names for the context, such as
`get-started:part2`. This puts the image in the `get-started` repository and
tags it as `part2`.
{% endcomment %}
レジストリ内のレポジトリをローカルイメージに関連づけるには `username/repository:tag` という書き方をします。
タグ（tag）はオプションですが、指定することが推奨されます。
というのも、これによってレジストリが Docker イメージに対してバージョンを与える方法となるからです。
状況に合わせてレポジトリとタグには、たとえば `get-started:part2` といったように意味を持つ名前をつけてください。
これによりそのイメージが `get-started` レポジトリ内に `part2` というタグがつけられた上で保存されます。

{% comment %}
Now, put it all together to tag the image. Run `docker tag image` with your
username, repository, and tag names so that the image uploads to your
desired destination. The syntax of the command is:
{% endcomment %}
そこでイメージにタグをつけます。
`docker tag image` の実行時にユーザー名、リポジトリ名、タグ名を指定します。
イメージは指定した名前でアップロードされます。
このコマンドの文法は以下のとおりです。

```shell
docker tag image username/repository:tag
```

{% comment %}
For example:
{% endcomment %}
例：

```shell
docker tag friendlyhello gordon/get-started:part2
```

{% comment %}
Run [docker image ls](/engine/reference/commandline/image_ls/) to see your newly
tagged image.
{% endcomment %}
[docker image ls](/engine/reference/commandline/image_ls/) を実行して、タグづけした新しいイメージを確認してみます。

```shell
$ docker image ls

REPOSITORY               TAG                 IMAGE ID            CREATED             SIZE
friendlyhello            latest              d9e555c53008        3 minutes ago       195MB
gordon/get-started         part2               d9e555c53008        3 minutes ago       195MB
python                   2.7-slim            1c7128a655f6        5 days ago          183MB
...
```

{% comment %}
### Publish the image
{% endcomment %}
### イメージの公開
{: #publish-the-image }

{% comment %}
Upload your tagged image to the repository:
{% endcomment %}
タグづけされたイメージをリポジトリにアップロードします。

```shell
docker push username/repository:tag
```

{% comment %}
Once complete, the results of this upload are publicly available. If you log in
to [Docker Hub](https://hub.docker.com/), you see the new image there, with
its pull command.
{% endcomment %}
上のコマンドが成功すれば、アップロードした結果が公開され利用可能となります。
[Docker Hub](https://hub.docker.com/) にログインすれば、新しいイメージがアップロードされているのを確認できます。
これは pull コマンドで取得可能です。

{% comment %}
### Pull and run the image from the remote repository
{% endcomment %}
### リモートリポジトリからのイメージの取得と実行
{: #pull-and-run-the-image-from-the-remote-repository }

{% comment %}
From now on, you can use `docker run` and run your app on any machine with this
command:
{% endcomment %}
ここからは `docker run` を利用していきます。
どのマシン上であっても、アプリの実行にはこのコマンドを使います。

```shell
docker run -p 4000:80 username/repository:tag
```

{% comment %}
If the image isn't available locally on the machine, Docker pulls it from
the repository.
{% endcomment %}
ローカルマシン上にそのイメージがない場合、Docker はリポジトリからイメージを取得します。

```shell
$ docker run -p 4000:80 gordon/get-started:part2
Unable to find image 'gordon/get-started:part2' locally
part2: Pulling from gordon/get-started
10a267c67f42: Already exists
f68a39a6a5e4: Already exists
9beaffc0cf19: Already exists
3c1fe835fb6b: Already exists
4c9f1fa8fcb8: Already exists
ee7d8f576a14: Already exists
fbccdcced46e: Already exists
Digest: sha256:0601c866aab2adcc6498200efd0f754037e909e5fd42069adeff72d1e2439068
Status: Downloaded newer image for gordon/get-started:part2
 * Running on http://0.0.0.0:80/ (Press CTRL+C to quit)
```

{% comment %}
No matter where `docker run` executes, it pulls your image, along with Python
and all the dependencies from `requirements.txt`, and runs your code. It all
travels together in a neat little package, and you don't need to install
anything on the host machine for Docker to run it.
{% endcomment %}
`docker run` をどこで実行してもイメージが取得できます。
しかも `requirements.txt` に書かれた Python や依存パッケージもすべて含まれていて、アプリコードも実行できます。
すっきりとまとまったこのパッケージはどこにでも持ち運びできて、
Docker を実行するからといって、ホストマシンには何もインストールする必要がないのです。

{% comment %}
## Conclusion of part two
{% endcomment %}
## 2 部のまとめ
{: #conclusion-of-part-two }

{% comment %}
That's all for this page. In the next section, we learn how to scale our
application by running this container in a **service**.
{% endcomment %}
このページの内容はここまでです。
次はアプリケーションをスケールアップして、コンテナーを**サービス**として起動することを学びます。

{% comment %}
[Continue to Part 3 >>](part3.md){: class="button outline-btn"}
{% endcomment %}
[3 部へ >>](part3.md){: class="button outline-btn"}

{% comment %}
Or, learn how to [launch your container on your own machine using DigitalOcean](https://docs.docker.com/machine/examples/ocean/){: target="_blank" class="_" }.
{% endcomment %}
あるいは [DigitalOcean を使って自マシン内でコンテナーを起動する](https://docs.docker.com/machine/examples/ocean/){: target="_blank" class="_" } 方法もあります。

{% comment %}
## Recap and cheat sheet (optional)
{% endcomment %}
## まとめと早見表（おまけ）
{: #recap-and-cheat-sheet-optional }

{% comment %}
Here's [a terminal recording of what was covered on this
page](https://asciinema.org/a/blkah0l4ds33tbe06y4vkme6g):
{% endcomment %}
[このページで扱った端末操作の録画](https://asciinema.org/a/blkah0l4ds33tbe06y4vkme6g) がこちらです。

<script type="text/javascript"
src="https://asciinema.org/a/blkah0l4ds33tbe06y4vkme6g.js"
id="asciicast-blkah0l4ds33tbe06y4vkme6g" speed="2" async></script>

{% comment %}
Here is a list of the basic Docker commands from this page, and some related
ones if you'd like to explore a bit before moving on.
{% endcomment %}
このページで扱った基本的な Docker コマンドの一覧を示します。
またこの先に進むにあたって必要となりそうな関連コマンドも示します。

```shell
docker build -t friendlyhello .                 # Dockerfileのディレクトリにてイメージ生成
docker run -p 4000:80 friendlyhello  # "friendlyname" の実行; ポート 4000 を 80 に割り当て
docker run -d -p 4000:80 friendlyhello                        # 同上、ただしデタッチモード
docker container ls                                         # 実行中のコンテナー一覧の表示
docker container ls -a                      # 実行していないものを含めコンテナー一覧の表示
docker container stop <hash>                                  # 指定コンテナーを適切に停止
docker container kill <hash>                          # 指定コンテナーを強制シャットダウン
docker container rm <hash>                                # マシンから指定コンテナーを削除
docker container rm $(docker container ls -a -q)                  # コンテナーをすべて削除
docker image ls -a                                    # マシン上のイメージすべての一覧表示
docker image rm <image id>                                  # マシン上の指定イメージを削除
docker image rm $(docker image ls -a -q)                # マシン上からイメージすべてを削除
docker login                                         # CLIセッションによりDockerへログイン
docker tag <image> username/repository:tag   # レジストリアップロードにて<image>にタグづけ
docker push username/repository:tag             # タグつきイメージをレジストリアップロード
docker run username/repository:tag                            # レジストリからイメージ実行
```
