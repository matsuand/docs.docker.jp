---
title: "はじめよう 5部: スタック"
keywords: stack, data, persist, dependencies, redis, storage, volume, port
description: Learn how to create a multi-container application that uses all the machines in a cluster.
---

{% include_relative nav.html selected="5" %}

{% comment %}
## Prerequisites
{% endcomment %}
## 前提条件
{: #prerequisites }

{% comment %}
- [Install Docker version 1.13 or higher](/engine/installation/).
- Get [Docker Compose](/compose/overview.md) as described in [Part 3 prerequisites](/get-started/part3.md#prerequisites).
- Get [Docker Machine](/machine/overview.md) as described in [Part 4 prerequisites](/get-started/part4.md#prerequisites).
- Read the orientation in [Part 1](index.md).
- Learn how to create containers in [Part 2](part2.md).
{% endcomment %}
- [Docker バージョン 1.13 またはそれ以上をインストールしていること](/engine/installation/index.md)。
- [3 部の前提条件](/get-started/part3.md#prerequisites)で説明した [Docker Compose](/compose/overview.md) を入手していること。
- [4 部の前提条件](/get-started/part4.md#prerequisites)で説明した [Docker Machine](/machine/overview.md) を入手していること。
- [1 部](index.md)の概要説明を読んでいること。
- [2 部](part2.md)で説明した、コンテナーの生成方法を理解していること。

{% comment %}
- Make sure you have published the `friendlyhello` image you created by
[pushing it to a registry](/get-started/part2.md#share-your-image). We
use that shared image here.
{% endcomment %}
- [レジストリに送信](/get-started/part2.md#share-your-image)して作成した `friendlyhello` イメージが共有可能であることを確認します。
  ここではその共有イメージを使います。

{% comment %}
- Be sure your image works as a deployed container. Run this command,
slotting in your info for `username`, `repo`, and `tag`: `docker run -p 80:80
username/repo:tag`, then visit `http://localhost/`.
{% endcomment %}
- デプロイしたコンテナーとしてイメージが動作することを確認します。
  以下のコマンドを実行してください。
  `docker run -p 80:80 username/repo:tag`
  ここで username、repo、tag の部分は各環境に合わせて書き換えてください。
  そして `http://localhost/` にアクセスします。

{% comment %}
- Have a copy of your `docker-compose.yml` from [Part 3](part3.md) handy.
{% endcomment %}
- [3 部](part3)で扱った `docker-compose.yml` ファイルが用意できていること。

{% comment %}
- Make sure that the machines you set up in [part 4](part4.md) are running
and ready. Run `docker-machine ls` to verify this. If the machines are
stopped, run `docker-machine start myvm1` to boot the manager, followed
by `docker-machine start myvm2` to boot the worker.
{% endcomment %}
- [4 部](part4.md)で作り上げたマシンが、稼動していて準備ができていること。
`docker-machine ls` を実行して確認することができます。
マシンが停止していたら `docker-machine start myvm1` を実行してマネージャーを起動し、さらに `docker-machine start myvm2` を実行してワーカーを起動します。

{% comment %}
- Have the swarm you created in [part 4](part4.md) running and ready. Run
`docker-machine ssh myvm1 "docker node ls"` to verify this. If the swarm is up,
both nodes report a `ready` status. If not, reinitialize the swarm and join
the worker as described in [Set up your
swarm](/get-started/part4.md#set-up-your-swarm).
{% endcomment %}
- [4 部](part4.md)で生成したスウォームが稼動していて準備ができていること。
`docker-machine ssh myvm1 "docker node ls"` を実行して確認することができます。
スウォームがまだ稼動していれば、2 つのノードが `ready` の状態として表示されます。
稼動していない場合はスウォームを再度初期化して、[スウォームのセットアップ](/get-started/part4.md#set-up-your-swarm)での説明に従ってワーカーを参加させます。

{% comment %}
## Introduction
{% endcomment %}
## はじめに
{: #introduction }

{% comment %}
In [part 4](part4.md), you learned how to set up a swarm, which is a cluster of
machines running Docker, and deployed an application to it, with containers
running in concert on multiple machines.
{% endcomment %}
[4 部](part4.md)ではスウォームの構築方法を学びました。
スウォームとは Docker が稼動するマシンで構成されたクラスターであり、アプリケーションをデプロイすることができます。
コンテナーが複数のマシン上において協力して動作するものです。

{% comment %}
Here in part 5, you reach the top of the hierarchy of distributed
applications: the **stack**. A stack is a group of interrelated services that
share dependencies, and can be orchestrated and scaled together. A single stack
is capable of defining and coordinating the functionality of an entire
application (though very complex applications may want to use multiple stacks).
{% endcomment %}
この 5 部では分散アプリケーションの最上位層である**スタック**（stack）を扱います。
スタックは、相互に関連づいたサービス群を指します。
これが依存パッケージを共有し、互いに協調してスケールに対応します。
ただ 1 つのスタックであっても、アプリケーション全体における機能を定義し調整を行います。
（複雑なアプリケーションでは複数のスタックが必要になるかもしれません。）

{% comment %}
Some good news is, you have technically been working with stacks since part 3,
when you created a Compose file and used `docker stack deploy`. But that was a
single service stack running on a single host, which is not usually what takes
place in production. Here, you can take what you've learned, make
multiple services relate to each other, and run them on multiple machines.
{% endcomment %}
気づいているかもしれませんが、3 部のときからすでにスタックは操作してきています。
Compose ファイルを作り `docker stack deploy` を実行していました。
ただしこのときは、1 つのホスト上に稼動するただ 1 つのサービススタックでした。
こういったものは本番環境では普通は用いません。
ここではこれまでに学んだことを踏まえて、互いに関連づいているサービスを複数作り出します。
それを複数のマシン上で動作させます。

{% comment %}
You're doing great, this is the home stretch!
{% endcomment %}
ここまで頑張ってきています。
あと一息です。

{% comment %}
## Add a new service and redeploy
{% endcomment %}
## 新たなサービス追加と再度のデプロイ
{: #add-a-new-service-and-redeploy }

{% comment %}
It's easy to add services to our `docker-compose.yml` file. First, let's add
a free visualizer service that lets us look at how our swarm is scheduling
containers.
{% endcomment %}
`docker-compose.yml` ファイルにサービスを追加するのは簡単です。
はじめに、自由に使えるビジュアライザー（visualizer）サービスを加えます。
スウォームがコンテナーをどのようにスケジューリングしているかが見えるものです。

{% comment %}
1.  Open up `docker-compose.yml` in an editor and replace its contents
with the following. Be sure to replace `username/repo:tag` with your image details.
{% endcomment %}
1.  エディターにより `docker-compose.yml` を開き、以下に示すような内容にします。
    `username/repo:tag` の部分は各イメージに合わせて置き換えてください。

    ```yaml
    version: "3"
    services:
      web:
        # replace username/repo:tag with your name and image details
        image: username/repo:tag
        deploy:
          replicas: 5
          restart_policy:
            condition: on-failure
          resources:
            limits:
              cpus: "0.1"
              memory: 50M
        ports:
          - "80:80"
        networks:
          - webnet
      visualizer:
        image: dockersamples/visualizer:stable
        ports:
          - "8080:8080"
        volumes:
          - "/var/run/docker.sock:/var/run/docker.sock"
        deploy:
          placement:
            constraints: [node.role == manager]
        networks:
          - webnet
    networks:
      webnet:
    ```

    {% comment %}
    The only thing new here is the peer service to `web`, named `visualizer`.
    Notice two new things here: a `volumes` key, giving the visualizer
    access to the host's socket file for Docker, and a `placement` key, ensuring
    that this service only ever runs on a swarm manager -- never a worker.
    That's because this container, built from [an open source project created by
    Docker](https://github.com/ManoMarks/docker-swarm-visualizer), displays
    Docker services running on a swarm in a diagram.
    {% endcomment %}
    ここでの新たな内容は `web` と対になるサービス `visualizer` です。
    そして 2 つの新しい項目があります。
    1 つは `volumes` キーです。
    これはビジュアライザーのアクセスを Docker ホストのソケットファイルに与えます。
    もう 1 つは `placement` キーです。
    サービスがスウォームマネージャー上でのみ動作するようにします。
    ワーカー上では動作しません。
    こうする理由は、このコンテナーが [Docker を用いて生成されているオープンソースプロジェクト](https://github.com/ManoMarks/docker-swarm-visualizer) から構築されていて、スウォーム上で稼動する Docker サービスを図に表現するものであるからです。

    {% comment %}
    We talk more about placement constraints and volumes in a moment.
    {% endcomment %}
    placement の制約とボリュームに関してはこの後に説明します。

{% comment %}
2.  Make sure your shell is configured to talk to `myvm1` (full examples are [here](part4.md#configure-a-docker-machine-shell-to-the-swarm-manager)).
{% endcomment %}
2.  シェル環境が `myvm1` と対話できるように設定されていることを確認します。
    （その操作例は[こちら](part4.md#configure-a-docker-machine-shell-to-the-swarm-manager)で説明済です。）

    {% comment %}
    * Run `docker-machine ls` to list machines and make sure you are connected to `myvm1`, as indicated by an asterisk next to it.
    {% endcomment %}
    * `docker-machine ls` を実行してマシン一覧を表示し、`myvm1` に接続していることを確認します。
      マシン名のとなりにアスタリスクが表示されているはずです。

    {% comment %}
    * If needed, re-run `docker-machine env myvm1`, then run the given command to configure the shell.
    {% endcomment %}
    * 必要であれば `docker-machine env myvm1` を再実行し、シェル設定を行うコマンドを実行します。

      {% comment %}
      On **Mac or Linux** the command is:
      {% endcomment %}
      **Mac または Linux** では以下のコマンドです。

      ```shell
      eval $(docker-machine env myvm1)
      ```

      {% comment %}
      On **Windows** the command is:
      {% endcomment %}
      **Windows** では以下のコマンドです。

      ```shell
      & "C:\Program Files\Docker\Docker\Resources\bin\docker-machine.exe" env myvm1 | Invoke-Expression
      ```

{% comment %}
3.  Re-run the `docker stack deploy` command on the manager, and
whatever services need updating are updated:
{% endcomment %}
3.  マネージャー上で `docker stack deploy` を再実行します。
    更新が必要なサービスは更新が行われます。

    ```shell
    $ docker stack deploy -c docker-compose.yml getstartedlab
    Updating service getstartedlab_web (id: angi1bf5e4to03qu9f93trnxm)
    Creating service getstartedlab_visualizer (id: l9mnwkeq2jiononb5ihz9u7a4)
    ```

{% comment %}
4.  Take a look at the visualizer.
{% endcomment %}
4.  ビジュアライザーを見てみます。

    {% comment %}
    You saw in the Compose file that `visualizer` runs on port 8080. Get the
    IP address of one of your nodes by running `docker-machine ls`. Go
    to either IP address at port 8080 and you can see the visualizer running:
    {% endcomment %}
    Compose ファイルでは `visualizer` をポート 8080 で稼動することにしていました。
    ノードの IP アドレスは `docker-machine ls` で確認できるので、いずれかの IP アドレスとポート 8080 を使って、ビジュアライザーの動作を確認します。


    {% comment %}
    ![Visualizer screenshot](images/get-started-visualizer1.png)
    {% endcomment %}
    ![ビジュアライザーのスクリーンショット](images/get-started-visualizer1.png)

    {% comment %}
    The single copy of `visualizer` is running on the manager as you expect, and
    the 5 instances of `web` are spread out across the swarm. You can
    corroborate this visualization by running `docker stack ps <stack>`:
    {% endcomment %}
    マネージャー上には、想定していたとおり `visualizer` のコピーが 1 つ動作しており、`web` サービスのインスタンスが 5 つ、スウォーム内にわたって稼動しています。
    この画面表示が正しいかどうかは `docker stack ps <スタック名>` を実行して確認できます。

    ```shell
    docker stack ps getstartedlab
    ```

    {% comment %}
    The visualizer is a standalone service that can run in any app
    that includes it in the stack. It doesn't depend on anything else.
    Now let's create a service that *does* have a dependency: the Redis
    service that provides a visitor counter.
    {% endcomment %}
    ビジュアライザーはスタンドアロンのサービスであるため、スタック内のどのようなアプリ上でも利用することができ、特別なものに依存することなく動作します。
    今度は依存関係を**持つ**サービスを生成します。
    アクセスカウンター機能を提供する Redis サービスです。

{% comment %}
## Persist the data
{% endcomment %}
## データの保持
{: #persist-the-data }

{% comment %}
Let's go through the same workflow once more to add a Redis database for storing
app data.
{% endcomment %}
これまでと同様の作業をもう一回、今度はアプリデータを保持する Redis データベースを加えていく作業を行います。

{% comment %}
1.  Save this new `docker-compose.yml` file, which finally adds a
Redis service. Be sure to replace `username/repo:tag` with your image details.
{% endcomment %}
1.  以下を `docker-compose.yml` ファイルとして新規に保存します。
    後半に Redis サービスを加えています。
    なお `username/repo:tag` の部分はイメージに合わせて書き換えてください。

    ```yaml
    version: "3"
    services:
      web:
        # replace username/repo:tag with your name and image details
        image: username/repo:tag
        deploy:
          replicas: 5
          restart_policy:
            condition: on-failure
          resources:
            limits:
              cpus: "0.1"
              memory: 50M
        ports:
          - "80:80"
        networks:
          - webnet
      visualizer:
        image: dockersamples/visualizer:stable
        ports:
          - "8080:8080"
        volumes:
          - "/var/run/docker.sock:/var/run/docker.sock"
        deploy:
          placement:
            constraints: [node.role == manager]
        networks:
          - webnet
      redis:
        image: redis
        ports:
          - "6379:6379"
        volumes:
          - "/home/docker/data:/data"
        deploy:
          placement:
            constraints: [node.role == manager]
        command: redis-server --appendonly yes
        networks:
          - webnet
    networks:
      webnet:
    ```

    {% comment %}
    Redis has an official image in the Docker library and has been granted the
    short `image` name of just `redis`, so no `username/repo` notation here. The
    Redis port, 6379, has been pre-configured by Redis to be exposed from the
    container to the host, and here in our Compose file we expose it from the
    host to the world, so you can actually enter the IP for any of your nodes
    into Redis Desktop Manager and manage this Redis instance, if you so choose.
    {% endcomment %}
    Redis は Docker ライブラリ内に公式イメージが提供されていて、イメージの簡略な名称として `redis` の利用が認められています。
    つまり`username/repo` という記述形式は必要としません。
    Redis のポート 6379 は、コンテナーからホストに向けて公開するようにあらかじめ設定されています。
    Compose ファイルでは、このポートをさらに外部に向けて公開しています。
    このことから、どのノードからでも IP アドレスを入力すれば Redis デスクトップマネージャーを起動することができ、Redis インスタンスを管理することもできます。

    {% comment %}
    Most importantly, there are a couple of things in the `redis` specification
    that make data persist between deployments of this stack:
    {% endcomment %}
    `redis` サービスの重要な処理として以下があります。
    それはこのスタックからデプロイを繰り返しても、データが維持されるということです。

    {% comment %}
    - `redis` always runs on the manager, so it's always using the
    same filesystem.
    - `redis` accesses an arbitrary directory in the host's file system
    as `/data` inside the container, which is where Redis stores data.
    {% endcomment %}
    - `redis` は常にマネージャー上で動作します。
      これはつまり、同一のファイルシステムを常に利用するということです。
    - `redis` はホストのファイルシステム内の任意のディレクトリとして、コンテナー内の `/data` にアクセスします。そこに Redis がデータを保存します。

    {% comment %}
    Together, this is creating a "source of truth" in your host's physical
    filesystem for the Redis data. Without this, Redis would store its data in
    `/data` inside the container's filesystem, which would get wiped out if that
    container were ever redeployed.
    {% endcomment %}
    上により、ホストの物理ファイルシステムには Redis のデータが、本当にそこに保存されます。
    これがなかったとしたら、Redis はコンテナー内部の `/data` ディレクトリにデータを保存することになります。
    そこに保存してしまうと、コンテナーが再デプロイされたときに、すべてが消えてしまうということです。

    {% comment %}
    This source of truth has two components:
    {% endcomment %}
    このデータ保存を実現するには、以下の 2 つの要件が必要になります。

    {% comment %}
    - The placement constraint you put on the Redis service, ensuring that it
      always uses the same host.
    - The volume you created that lets the container access `./data` (on the host) as `/data` (inside the Redis container). While containers come and go, the files stored on `./data` on the specified host persists, enabling continuity.
    {% endcomment %}
    - Redis サービスを配置する placement の制約（constraint）として、必ず同一ホストを用いるものとします。
    - volume には、コンテナーがアクセスする（ホスト上の）`./data` を（Redis コンテナー内部では）`/data` としてアクセスすることを指定します。コンテナーが動作し始めると、指定されたホスト上の `./data` に保存されたファイルは保持され続け、継続して利用が可能となります。

    {% comment %}
    You are ready to deploy your new Redis-using stack.
    {% endcomment %}
    Redis を利用する新たなスタックをデプロイする準備が整いました。

{% comment %}
2.  Create a `./data` directory on the manager:
{% endcomment %}
2.  マネージャー上に `./data` ディレクトリを生成します。

    ```shell
    docker-machine ssh myvm1 "mkdir ./data"
    ```

{% comment %}
3.  Make sure your shell is configured to talk to `myvm1` (full examples are [here](part4.md#configure-a-docker-machine-shell-to-the-swarm-manager)).
{% endcomment %}
3.  シェル環境が `myvm1` と対話できるように設定されていることを確認します。
    （その操作例は[こちら](part4.md#configure-a-docker-machine-shell-to-the-swarm-manager)で説明済です。）

    {% comment %}
    * Run `docker-machine ls` to list machines and make sure you are connected to `myvm1`, as indicated by an asterisk next to it.
    {% endcomment %}
    * `docker-machine ls` を実行してマシン一覧を表示し、`myvm1` に接続していることを確認します。
      マシン名のとなりにアスタリスクが表示されているはずです。

    {% comment %}
    * If needed, re-run `docker-machine env myvm1`, then run the given command to configure the shell.
    {% endcomment %}
    * 必要であれば `docker-machine env myvm1` を再実行し、シェル設定を行うコマンドを実行します。

      {% comment %}
      On **Mac or Linux** the command is:
      {% endcomment %}
      **Mac または Linux** では以下のコマンドです。

      ```shell
      eval $(docker-machine env myvm1)
      ```

      {% comment %}
      On **Windows** the command is:
      {% endcomment %}
      **Windows** では以下のコマンドです。

      ```shell
      & "C:\Program Files\Docker\Docker\Resources\bin\docker-machine.exe" env myvm1 | Invoke-Expression
      ```

{% comment %}
4.  Run `docker stack deploy` one more time.
{% endcomment %}
4.  `docker stack deploy` をもう一度実行します。

    ```shell
    $ docker stack deploy -c docker-compose.yml getstartedlab
    ```

{% comment %}
5.  Run `docker service ls` to verify that the three services are running as expected.
{% endcomment %}
5.  `docker service ls` を実行して、3 つのサービスが期待どおりに稼動していることを確認します。

    ```shell
    $ docker service ls
    ID                  NAME                       MODE                REPLICAS            IMAGE                             PORTS
    x7uij6xb4foj        getstartedlab_redis        replicated          1/1                 redis:latest                      *:6379->6379/tcp
    n5rvhm52ykq7        getstartedlab_visualizer   replicated          1/1                 dockersamples/visualizer:stable   *:8080->8080/tcp
    mifd433bti1d        getstartedlab_web          replicated          5/5                 gordon/getstarted:latest    *:80->80/tcp

    ```

{% comment %}
6.  Check the web page at one of your nodes, such as `http://192.168.99.101`, and take a look at the results of the visitor counter, which is now live and storing information on Redis.
{% endcomment %}
6.  ノードのいずれかから、たとえば `http://192.168.99.101` からウェブページを表示させます。
    そしてアクセスカウンターの値を確認します。
    カウンターが動作し、その値は Redis 上に保存されます。

    {% comment %}
    ![Hello World in browser with Redis](images/app-in-browser-redis.png)
    {% endcomment %}
    ![ブラウザー上での Redis を用いた Hello World](images/app-in-browser-redis.png)

    {% comment %}
    Also, check the visualizer at port 8080 on either node's IP address, and notice the `redis` service running along with the `web` and `visualizer` services.
    {% endcomment %}
    同じく、いずれかのノードの IP アドレスとポート 8080 を用いてビジュアライザーを確認します。
    そして `web` や `visualizer` サービスとともに `redis` サービスも稼動していることを確認します。

    {% comment %}
    ![Visualizer with redis screenshot](images/visualizer-with-redis.png)
    {% endcomment %}
    ![ビジュアライザー上の redis のスクリーンショット](images/visualizer-with-redis.png)


[6 部へ >>](part6.md){: class="button outline-btn"}

{% comment %}
## Recap (optional)
{% endcomment %}
## まとめ（おまけ）
{: #recap-optional }

{% comment %}
Here's [a terminal recording of what was covered on this page](https://asciinema.org/a/113840):
{% endcomment %}
[このページで扱った端末操作の録画](https://asciinema.org/a/113840)がこちらです。

<script type="text/javascript" src="https://asciinema.org/a/113840.js" speed="2" id="asciicast-113840" async></script>

{% comment %}
You learned that stacks are inter-related services all running in concert, and
that -- surprise! -- you've been using stacks since part three of this tutorial.
You learned that to add more services to your stack, you insert them in your
Compose file. Finally, you learned that by using a combination of placement
constraints and volumes you can create a permanent home for persisting data, so
that your app's data survives when the container is torn down and redeployed.
{% endcomment %}
ここではスタックというものが、互いに協調して動作するサービスであることを学びました。
しかも実は、このスタックをチュートリアルの 3 部からずっと使い続けていたのです。
スタックに新たなサービスを加えるには Compose ファイルに追加すればよいことも知りました。
そして placement の制約と volume の設定の組み合わせにより、データを保持し続ける方法がわかったので、コンテナーが停止され新たにデプロイされても、アプリのデータは失われることがありません。
