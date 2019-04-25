---
title: "はじめよう 3部: サービス"
keywords: services, replicas, scale, ports, compose, compose file, stack, networking
description: Learn how to define load-balanced and scalable service that runs containers.
---
{% include_relative nav.html selected="3" %}

## 前提条件

<!--
- [Install Docker version 1.13 or higher](/engine/installation/index.md).
-->
- [Docker バージョン 1.13 またはそれ以上をインストールすること](/engine/installation/index.md)。

<!--
- Get [Docker Compose](/compose/overview.md). On [Docker Desktop for
Mac](/docker-for-mac/index.md) and [Docker Desktop for
Windows](/docker-for-windows/index.md) it's pre-installed, so you're good-to-go.
On Linux systems you need to [install it
directly](https://github.com/docker/compose/releases). On pre Windows 10 systems
_without Hyper-V_, use [Docker
Toolbox](/toolbox/overview.md).
-->
* [Docker Compose](/compose/overview.md) を入手していること。
  [Docker for Mac](/docker-for-mac/index.md) と [Docker for Windows](/docker-for-windows/index.md) ではすでにインストールされているので、このまま読み進めてください。
  Linux システムでは[直接インストール](https://github.com/docker/compose/releases>)が必要です。
  Widows 10 システム上で Hyper-V が入っていない場合は [Docker Toolbox](/toolbox/overview.md) を利用してください。

<!--
- Read the orientation in [Part 1](index.md).
-->
- [1 部](index.md)の概要説明を読んでいること。

<!--
- Learn how to create containers in [Part 2](part2.md).
-->
- [2 部](part2.md)で説明した、コンテナーの生成方法を理解していること。

<!--
- Make sure you have published the `friendlyhello` image you created by
[pushing it to a registry](/get-started/part2.md#share-your-image). We use that
shared image here.
-->
- [レジストリに送信](/get-started/part2.md#share-your-image)して作成した `friendlyhello` イメージが共有可能であることを確認します。
  ここではその共有イメージを使います。

<!--
- Be sure your image works as a deployed container. Run this command,
slotting in your info for `username`, `repo`, and `tag`: `docker run -p 4000:80
username/repo:tag`, then visit `http://localhost:4000/`.
-->
- デプロイしたコンテナーとしてイメージが動作することを確認します。
  以下のコマンドを実行してください。
  `docker run -p 80:80 username/repo:tag`
  ここで username、repo、tag の部分は各環境に合わせて書き換えてください。
  そして `http://localhost/` にアクセスします。

## はじめに

<!--
In part 3, we scale our application and enable load-balancing. To do this, we
must go one level up in the hierarchy of a distributed application: the
**service**.
-->
3 部ではアプリケーションをスケールアップして、負荷分散（ロードバランシング）を有効にします。
こうするためには、分散アプリケーションのレベルを一段あげる必要があります。
**サービス化**するということです。

<!--
- Stack
- **Services** (you are here)
- Container (covered in [part 2](part2.md))
-->
- スタック
- **サービス** (ここにいます)
- コンテナー ([2 部](part2.md)で説明済)

## サービスについて

<!--
In a distributed application, different pieces of the app are called "services".
For example, if you imagine a video sharing site, it probably includes a service
for storing application data in a database, a service for video transcoding in
the background after a user uploads something, a service for the front-end, and
so on.
-->
分散アプリケーションにおいては、その中に他の要素とは性格が異なる「サービス」と呼ばれるものがあります。
たとえば動画共有サイトを考えてみてください。
おそらくはアプリケーションデータをデータベースに保存するためのサービスがあり、ユーザーがデータをアップロードしたときにバックグラウンドでビデオ変換するサービスがあり、フロントエンドのサービスもあるでしょう。

<!--
Services are really just "containers in production." A service only runs one
image, but it codifies the way that image runs&#8212;what ports it should use,
how many replicas of the container should run so the service has the capacity it
needs, and so on. Scaling a service changes the number of container instances
running that piece of software, assigning more computing resources to the
service in the process.
-->
サービスとは正に「稼動するコンテナー」なのです。
どのサービスも実行するイメージはただ一つですが、そこにはイメージの動作方法がコーディングされています。
ポートは何番を使うか、サービスが持つべき性能を発揮するにはレプリカをいくつ用いればよいか、などです。
サービスの規模を大きくすると、このソフトウエアを実行するコンテナーインスタンスの数が変わります。
またプロセス内で実行されるこのサービスへのコンピューターリソース割り当てが増えます。

<!--
Luckily it's very easy to define, run, and scale services with the Docker
platform -- just write a `docker-compose.yml` file.
-->
うれしいことに Docker では簡単にサービスの定義、実行、規模変更を行うことができます。
ただ `docker-compose.yml` ファイルを書くだけです。

<!--
## Your first `docker-compose.yml` file
-->
## 初めての `docker-compose.yml` ファイル

<!--
A `docker-compose.yml` file is a YAML file that defines how Docker containers
should behave in production.
-->
`docker-compose.yml` ファイルは YAML ファイルであり、Docker コンテナーがどのように動作するかを定義します。

### `docker-compose.yml`

<!--
Save this file as `docker-compose.yml` wherever you want. Be sure you have
[pushed the image](/get-started/part2.md#share-your-image) you created in [Part
2](part2.md) to a registry, and update this `.yml` by replacing
`username/repo:tag` with your image details.
-->
以下の内容を `docker-commpose.yml` というファイル名で任意の場所に保存します。
[2 部](part2.md)で行ったレジストリへの[送信イメージ](/get-started/part2.md#share-your-image)を確認し、`.yml` ファイルの `username/repo:tag` の部分を各自の環境のものへ書き換えます。

```yaml
version: "3"
services:
  web:
    # replace username/repo:tag with your name and image details
    image: username/repo:tag
    deploy:
      replicas: 5
      resources:
        limits:
          cpus: "0.1"
          memory: 50M
      restart_policy:
        condition: on-failure
    ports:
      - "4000:80"
    networks:
      - webnet
networks:
  webnet:
```

<!--
This `docker-compose.yml` file tells Docker to do the following:
-->
この `docker-compose.yml` ファイルが Docker に対して以下の指示を行います。

<!--
- Pull [the image we uploaded in step 2](part2.md) from the registry.
-->
- [2 部でアップロードしたイメージ](part2.md)をレジストリから取得。

<!--
- Run 5 instances of that image as a service
  called `web`, limiting each one to use, at most, 10% of a single core of
  CPU time (this could also be e.g. "1.5" to mean 1 and half core for each),
  and 50MB of RAM.
-->
- イメージのインスタンスを 5 つ実行し `web` という名前のサービスとして実行。
  それぞれのインスタンスは（全てのコアを通じて）最大で CPU の 10% の利用までに制限し、RAM は 50MB とする。

<!--
- Immediately restart containers if one fails.
-->
- コンテナーが停止したときは、すぐに再起動。

<!--
- Map port 4000 on the host to `web`'s port 80.
-->
- ホスト側のポート 4000 を `web` のポート 80 に割り当て。

<!--
- Instruct `web`'s containers to share port 80 via a load-balanced network
  called `webnet`. (Internally, the containers themselves publish to
  `web`'s port 80 at an ephemeral port.)
-->
- `web` のコンテナーに対し、`webnet` という名前の負荷分散ネットワークを経由してポート 80 を共有するよう命令（内部では、コンテナー自身の一時的なポートとして `web` のポート 80 を公開 ）。

<!--
- Define the `webnet` network with the default settings (which is a
  load-balanced overlay network).
-->
デフォルトの設定として `webnet` ネットワークを定義（負荷分散されるオーバレイネットワーク）。


<!--
## Run your new load-balanced app
-->
## 新しい負荷分散アプリケーションの実行

<!--
Before we can use the `docker stack deploy` command we first run:
-->
`docker stack deploy` コマンドを実行する前に、以下を実行します。

```shell
docker swarm init
```

<!--
>**Note**: We get into the meaning of that command in [part 4](part4.md).
> If you don't run `docker swarm init` you get an error that "this node is not a swarm manager."
-->
>**メモ**
>
このコマンドの意味については [4 部](part4.md)で説明します。
`docker swarm init` コマンドを実行していないと、"this node is not a swarm manager" （このノードはスウォームマネージャーではありません）というエラーが出ることになります。

<!--
Now let's run it. You need to give your app a name. Here, it is set to
`getstartedlab`:
-->
次のコマンドを実行します。
アプリには名前をつける必要があるので、ここでは `getstartedlab` と指定します。

```shell
docker stack deploy -c docker-compose.yml getstartedlab
```

<!--
Our single service stack is running 5 container instances of our deployed image
on one host. Let's investigate.
-->
1 つのサービススタックから、ホストにデプロイしたイメージに対するコンテナーインスタンスが５つ稼動しました。
中身を確認してみましょう。

<!--
Get the service ID for the one service in our application:
-->
アプリケーション内に稼動しているサービスの ID を取得します。

```shell
docker service ls
```

<!--
Look for output for the `web` service, prepended with your app name. If you
named it the same as shown in this example, the name is
`getstartedlab_web`. The service ID is listed as well, along with the number of
replicas, image name, and exposed ports.
-->
`web` サービスに関する情報を見てください。
アプリ名が行先頭に表示されます。
上で示した例と同じ名前をつけていれば `getstartedlab_web` が表示されたはずです。
サービス ID をはじめ、レプリカ数、イメージ名、公開ポートもともに一覧表示されます。

<!--
Alternatively, you can run `docker stack services`, followed by the name of
your stack. The following example command lets you view all services associated with the
`getstartedlab` stack:
-->
`docker stack services` を実行することもできます。
コマンドにはスタック名も指定します。
以下のコマンド例では、`getstartedlab` スタックに関連づいたサービスがすべて一覧表示されます。

```bash
docker stack services getstartedlab
ID                  NAME                MODE                REPLICAS            IMAGE               PORTS
bqpve1djnk0x        getstartedlab_web   replicated          5/5                 username/repo:tag   *:4000->80/tcp
```

<!--
A single container running in a service is called a **task**. Tasks are given unique
IDs that numerically increment, up to the number of `replicas` you defined in
`docker-compose.yml`. List the tasks for your service:
-->
1 つのサービス内に稼動している単一のコンテナーのことを**タスク**と呼びます。
タスクにはユニークな ID がつけられます。
この ID は `docker-compose.yml` 内に定義された `replicas` の値までの数値が加算されて割り当てられます。
サービスに対するタスクの一覧を表示してみます。

```bash
docker service ps getstartedlab_web
```

<!--
Tasks also show up if you just list all the containers on your system, though that
is not filtered by service:
-->
タスクは、システム全体のコンテナー一覧を表示することでも確認することができます。
ただしサービスごとに表示されるわけではありません。

```bash
docker container ls -q
```

<!--
You can run `curl -4 http://localhost:4000` several times in a row, or go to that URL in
your browser and hit refresh a few times.
-->
`curl http://localhost` コマンドを順に数回実行するか、あるいはブラウザーでこの URL を表示して何回か再読み込みをしてみてください。

<!--
![Hello World in browser](images/app80-in-browser.png)
-->
![ブラウザー上の Hello World](images/app80-in-browser.png)

<!--
Either way, the container ID changes, demonstrating the
load-balancing; with each request, one of the 5 tasks is chosen, in a
round-robin fashion, to respond. The container IDs match your output from
the previous command (`docker container ls -q`).
-->
どちらの方法でもコンテナー ID が変化して、各リクエストごとに５つのレプリカのうちの１つがラウンドロビン方式により選ばれて応答します。
これにより負荷分散が機能していることが分かります。
コンテナー ID は先ほどのコマンド（`docker container ls -q`）の出力に合致しているはずです。

<!--
To view all tasks of a stack, you can run `docker stack ps` followed by your app name, as shown in the following example:
-->
スタック内のタスクすべてを見るには `docker stack ps` を実行します。
このコマンドにはアプリ名をつけます。
以下がその実行例です。

```bash
docker stack ps getstartedlab
ID                  NAME                  IMAGE               NODE                DESIRED STATE       CURRENT STATE           ERROR               PORTS
uwiaw67sc0eh        getstartedlab_web.1   username/repo:tag   docker-desktop      Running             Running 9 minutes ago
sk50xbhmcae7        getstartedlab_web.2   username/repo:tag   docker-desktop      Running             Running 9 minutes ago
c4uuw5i6h02j        getstartedlab_web.3   username/repo:tag   docker-desktop      Running             Running 9 minutes ago
0dyb70ixu25s        getstartedlab_web.4   username/repo:tag   docker-desktop      Running             Running 9 minutes ago
aocrb88ap8b0        getstartedlab_web.5   username/repo:tag   docker-desktop      Running             Running 9 minutes ago
```

<!--
> Running Windows 10?
>
> Windows 10 PowerShell should already have `curl` available, but if not you can
> grab a Linux terminal emulator like
> [Git BASH](https://git-for-windows.github.io/){: target="_blank" class="_"},
> or download
> [wget for Windows](http://gnuwin32.sourceforge.net/packages/wget.htm)
> which is very similar.
-->
> Windows 10 を利用していたら？
>
> Windows 10 の PowerShell では `curl` が利用可能となっています。
> 利用できない場合には [Git BASH](https://git-for-windows.github.io/){: target="_blank" class="_"}
> のような Linux ターミナルエミュレーターを使う方法があります。
> あるいは [wget for Windows](http://gnuwin32.sourceforge.net/packages/wget.htm) を使うと `curl` と同様の作業ができます。

<!--
> Slow response times?
>
> Depending on your environment's networking configuration, it may take up to 30
> seconds for the containers
> to respond to HTTP requests. This is not indicative of Docker or
> swarm performance, but rather an unmet Redis dependency that we
> address later in the tutorial. For now, the visitor counter isn't working
> for the same reason; we haven't yet added a service to persist data.
-->
> レスポンスが遅い？
>
> 利用している環境でのネットワーク設定によりますが、コンテナーが HTTP リクエストに応答するのに 30 秒くらいかかることがあります。
> これは Docker や スウォームの性能によるものではなく、Redis の依存関係が満たされていないためです。
> このことはチュートリアル内で後に説明します。
> ですから今の時点でアクセスカウンターは動作しません。
> まだデータを維持するためのサービスを作り出していないということです。


<!--
## Scale the app
-->
## アプリのスケーリング

<!--
You can scale the app by changing the `replicas` value in `docker-compose.yml`,
saving the change, and re-running the `docker stack deploy` command:
-->
`docker-compose.yml` の `replicas` の値を変更すれば、アプリのスケールを変更できます。
変更を保存したら、`docker stack deploy` コマンドを再度実行します。

```shell
docker stack deploy -c docker-compose.yml getstartedlab
```

<!--
Docker performs an in-place update, no need to tear the stack down first or kill
any containers.
-->
Docker は直接書き換える（in-place）更新を行うので、スタックやコンテナーをあらかじめ停止しておく必要はありません。

<!--
Now, re-run `docker container ls -q` to see the deployed instances reconfigured.
If you scaled up the replicas, more tasks, and hence, more containers, are
started.
-->
もう一度 `docker container ls -q` を実行してみると、デプロイしたインスタンスが再設定されたことが確認できます。
レプリカをスケールアップしていれば、より多くのタスクが起動するので、つまりより多くのコンテナーが起動します。

<!--
### Take down the app and the swarm
-->
### アプリとスウォームの停止

<!--
* Take the app down with `docker stack rm`:
-->
* `docker stack rm` を実行してアプリを停止します。

  ```shell
  docker stack rm getstartedlab
  ```

<!--
* Take down the swarm.
-->
* スウォームを停止します。

  ```
  docker swarm leave --force
  ```

<!--
It's as easy as that to stand up and scale your app with Docker. You've taken a
huge step towards learning how to run containers in production. Up next, you
learn how to run this app as a bonafide swarm on a cluster of Docker
machines.
-->
Docker においてはアプリケーションの起動もスケールアップも非常に簡単です。
ここまでにコンテナーを実稼動させる方法を学びました。
大きく前進しました。
次に学ぶのは、複数の Docker マシンによるクラスター上にて、本当の意味でスウォームとしてのアプリを実行する方法です。

<!--
> **Note**: Compose files like this are used to define applications with Docker, and can be uploaded to cloud providers using [Docker
Cloud](/docker-cloud/), or on any hardware or cloud provider you choose with
[Docker Enterprise Edition](https://www.docker.com/enterprise-edition).
-->
> **メモ**
今回使ったような Compose ファイルは Docker においてアプリケーションを定義するために用います。
そして [Docker Cloud](/docker-cloud/) を用いてクラウドプロバイダーへのアップロードを行います。
つまり他のハードウェアや、[Docker エンタープライズエディション](https://www.docker.com/enterprise-edition)において選定したクラウドプロバイダーへのアップロードを行うものです。

<!--
[On to "Part 4" >>](part4.md){: class="button outline-btn"}
-->
[4 部へ >>](part4.md){: class="button outline-btn"}

<!--
## Recap and cheat sheet (optional)
-->
## まとめと早見表（おまけ）

<!--
Here's [a terminal recording of what was covered on this page](https://asciinema.org/a/b5gai4rnflh7r0kie01fx6lip):
-->
[このページで扱った端末操作の録画](https://asciinema.org/a/b5gai4rnflh7r0kie01fx6lip)がこちらです。

<script type="text/javascript" src="https://asciinema.org/a/b5gai4rnflh7r0kie01fx6lip.js" id="asciicast-b5gai4rnflh7r0kie01fx6lip" speed="2" async></script>

<!--
To recap, while typing `docker run` is simple enough, the true implementation
of a container in production is running it as a service. Services codify a
container's behavior in a Compose file, and this file can be used to scale,
limit, and redeploy our app. Changes to the service can be applied in place, as
it runs, using the same command that launched the service:
`docker stack deploy`.
-->
要するに `docker run` と入力するのが非常に簡単なことなのですが、実稼動させるコンテナーの真の実現方法は、これをサービスとして稼動させることです。
サービスは Compose ファイルにおいてコンテナーの動作を定義します。
このファイルによってアプリのスケールアップ、制限、再デプロイを実現します。
サービスへの変更は、稼動中であろうとも適切に反映されます。
その際のコマンドはサービスを起動させたときの `docker stack deploy` と同じようにして実現できます。

<!--
Some commands to explore at this stage:
-->
ここまでのコマンドをまとめます。

```shell
docker stack ls                                         # スタックやアプリの一覧
docker stack deploy -c <composefile> <appname> # 指定する Compose ファイルの実行
docker service ls                           # アプリに関連する実行中サービス一覧
docker service ps <service>                         # アプリに関連するタスク一覧
docker inspect <task or container>                # タスクまたはコンテナーの調査
docker container ls -q                                    # コンテナー ID の一覧
docker stack rm <appname>                               # アプリケーションの停止
docker swarm leave --force          # マネージャーから単一ノードスウォームを停止
```
