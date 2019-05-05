---
title: "はじめよう 6部: アプリのデプロイ"
keywords: deploy, production, datacenter, cloud, aws, azure, provider, admin, enterprise
description: Docker CE または EE を使ってアプリを本番環境にデプロイする。
---
{% include_relative nav.html selected="6" %}

<!--
## Prerequisites
-->
## 前提条件
{: #prerequisites }

<!--
- [Install Docker](/install/index.md).
- Get [Docker Compose](/compose/overview.md) as described in [Part 3 prerequisites](/get-started/part3.md#prerequisites).
- Get [Docker Machine](/machine/overview.md) as described in [Part 4 prerequisites](/get-started/part4.md#prerequisites).
- Read the orientation in [Part 1](index.md).
- Learn how to create containers in [Part 2](part2.md).
-->
- [Docker をインストールしていること](/install/index.md)。
- [3 部の前提条件](/get-started/part3.md#prerequisites)で説明した [Docker Compose](/compose/overview.md) を入手していること。
- [4 部の前提条件](/get-started/part4.md#prerequisites)で説明した [Docker Machine](/machine/overview.md) を入手していること。
- [1 部](index.md)の概要説明を読んでいること。
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
slotting in your info for `username`, `repo`, and `tag`: `docker run -p 80:80
username/repo:tag`, then visit `http://localhost/`.
-->
- デプロイしたコンテナーとしてイメージが動作することを確認します。
  以下のコマンドを実行してください。
  `docker run -p 80:80 username/repo:tag`
  ここで username、repo、tag の部分は各環境に合わせて書き換えてください。
  そして `http://localhost/` にアクセスします。

<!--
- Have [the final version of `docker-compose.yml` from Part 5](/get-started/part5.md#persist-the-data) handy.
-->
- [5 部における最終の `docker-compose.yml` ](/get-started/part5.md#persist-the-data) が用意できていること。

<!--
## Introduction
-->
## はじめに
{: #introduction }

<!--
You've been editing the same Compose file for this entire tutorial. Well, we
have good news. That Compose file works just as well in production as it does
on your machine. In this section, we will go through some options for running your
Dockerized application.
-->
このチュートリアルを通しては、ずっと同じ Compose ファイルを用いてきています。
うれしい話ですが、この Compose ファイルはローカルマシンと同じく、本番環境においても同様に動作します。
ここでは Docker 化したアプリケーションを動かす上での選択肢を示します。

<!--
## Choose an option
-->
## いずれかを選ぶ
{: #choose-an-option }

{% capture community %}

<!--
### Install Docker Engine --- Community
-->
### Docker Engine --- Community のインストール
{: #install-docker-engine-----community }

<!--
Find the [install instructions](/install/#supported-platforms) for Docker Engine --- Community on the platform of your choice.
-->
動作環境に合わせて Docker Engine の[インストール手順](/install/#supported-platforms)を参照してください。

<!--
### Create your swarm
-->
### スウォームの生成
{: #create-your-swarm }

<!--
Run `docker swarm init` to create a swarm on the node.
-->
ノード上から `docker swarm init` を実行してスウォームを生成します。

<!--
### Deploy your app
-->
### アプリのデプロイ
{: #deploy-your-app }

<!--
Run `docker stack deploy -c docker-compose.yml getstartedlab` to deploy
the app on the cloud hosted swarm.
-->
`docker stack deploy -c docker-compose.yml getstartedlab` を実行して、クラウド上のスウォームにアプリをデプロイします。

```shell
docker stack deploy -c docker-compose.yml getstartedlab

Creating network getstartedlab_webnet
Creating service getstartedlab_web
Creating service getstartedlab_visualizer
Creating service getstartedlab_redis
```

<!--
Your app is now running on your cloud provider.
-->
クラウドプロバイダー上においてアプリが起動し始めました。

<!--
#### Run some swarm commands to verify the deployment
-->
#### スウォームコマンドを実行してデプロイ結果を確認
{: #run-some-swarm-commands-to-verify-the-deployment }

<!--
You can use the swarm command line, as you've done already, to browse and manage
the swarm. Here are some examples that should look familiar by now:
-->
スウォームに対するコマンドはすでに実行してきましたが、これを使ってスウォームを確認したり管理したりしていきます。
今までにも見てきたような例をいくつか示します。

<!--
* Use `docker node ls` to list the nodes in your swarm.
-->
* `docker node ls` により、スウォーム上のノード一覧を表示します。

```shell
[getstartedlab] ~ $ docker node ls
ID                            HOSTNAME                                      STATUS              AVAILABILITY        MANAGER STATUS
n2bsny0r2b8fey6013kwnom3m *   ip-172-31-20-217.us-west-1.compute.internal   Ready               Active              Leader
```

<!--
* Use `docker service ls` to list services.
-->
* `docker service ls` によりサービス一覧を表示します。

```shell
[getstartedlab] ~/sandbox/getstart $ docker service ls
ID                  NAME                       MODE                REPLICAS            IMAGE                             PORTS
ioipby1vcxzm        getstartedlab_redis        replicated          0/1                 redis:latest                      *:6379->6379/tcp
u5cxv7ppv5o0        getstartedlab_visualizer   replicated          0/1                 dockersamples/visualizer:stable   *:8080->8080/tcp
vy7n2piyqrtr        getstartedlab_web          replicated          5/5                 sam/getstarted:part6    *:80->80/tcp
```

<!--
* Use `docker service ps <service>` to view tasks for a service.
-->
* `docker service ps <サービス名>` により、サービスに対するタスクを確認します。

```shell
[getstartedlab] ~/sandbox/getstart $ docker service ps vy7n2piyqrtr
ID                  NAME                  IMAGE                            NODE                                          DESIRED STATE       CURRENT STATE            ERROR               PORTS
qrcd4a9lvjel        getstartedlab_web.1   sam/getstarted:part6   ip-172-31-20-217.us-west-1.compute.internal   Running             Running 20 seconds ago
sknya8t4m51u        getstartedlab_web.2   sam/getstarted:part6   ip-172-31-20-217.us-west-1.compute.internal   Running             Running 17 seconds ago
ia730lfnrslg        getstartedlab_web.3   sam/getstarted:part6   ip-172-31-20-217.us-west-1.compute.internal   Running             Running 21 seconds ago
1edaa97h9u4k        getstartedlab_web.4   sam/getstarted:part6   ip-172-31-20-217.us-west-1.compute.internal   Running             Running 21 seconds ago
uh64ez6ahuew        getstartedlab_web.5   sam/getstarted:part6   ip-172-31-20-217.us-west-1.compute.internal   Running             Running 22 seconds ago
```

<!--
#### Open ports to services on cloud provider machines
-->
#### クラウドプロバイダーマシン上のサービスのためのポート
{: #open-ports-to-services-on-cloud-provider-machines }

<!--
At this point, your app is deployed as a swarm on your cloud provider servers,
as evidenced by the `docker` commands you just ran. But, you still need to
open ports on your cloud servers in order to:
-->
この時点でアプリは、クラウドプロバイダーサーバー上のスウォームとしてデプロイされています。
それは `docker` コマンドを実行してみればわかります。
ただしクラウドサーバーに対してポートを開く作業は行う必要があります。
それは以下のためです。

<!--
* if using many nodes, allow communication between the `redis` service and `web` service
-->
* 複数のノードがある場合、`redis` サービスと `web` サービスの間での通信を可能とします。

<!--
* allow inbound traffic to the `web` service on any worker nodes so that
Hello World and Visualizer are accessible from a web browser.
-->
* どのワーカーノードにおいても `web` サービスに対するインバウンドトラフィックを許可します。
  これにより Hello World やビジュアライザーがウェブブラウザーからアクセス可能になります。

<!--
* allow inbound SSH traffic on the server that is running the `manager` (this may be already set on your cloud provider)
-->
* `manager` として稼動しているサーバーに対するインバウンド SSH トラフィックを許可します。
  （この設定は、クラウドプロバイダー上ですでに行われているかもしれません。）

{: id="table-of-ports"}

<!--
These are the ports you need to expose for each service:
-->
各サービスにおいて公開するべきポートは以下のとおりです。

| サービス       | タイプ  | プロトコル |  ポート |
| :---           | :---    | :---       | :---    |
| `web`          | HTTP    | TCP        |  80     |
| `visualizer`   | HTTP    | TCP        |  8080   |
| `redis`        | TCP     | TCP        |  6379   |

<!--
Methods for doing this vary depending on your cloud provider.
-->
ポートの公開方法はクラウドサーバーによりさまざまです。

<!--
We use Amazon Web Services (AWS) as an example.
-->
ここでは例として Amazon Web Services (AWS) を取り上げます。

<!--
> What about the redis service to persist data?
>
> To get the `redis` service working, you need to `ssh` into
the cloud server where the `manager` is running, and make a `data/`
directory in `/home/docker/` before you run `docker stack deploy`.
Another option is to change the data path in the `docker-stack.yml` to
a pre-existing path on the `manager` server. This example does not
include this step, so the `redis` service is not up in the example output.
-->
> redis サービスが保存するデータはどうなるのか？
>
> `redis` サービスを稼動させるには、`manager` が稼動しているクラウドサーバーに `ssh` でログインし、`/home/docker/` ディレクトリ内に `data/` を作成する必要があります。これを行った後に `docker stack deploy` を実行します。
別のやり方として `docker-stack.yml` 内のデータパスを変更する方法があります。
`manager` サーバーにあらかじめ存在しているパスを指定するものです。
ここに示す例ではその手順を説明しませんので、`redis` サービスは稼動していない状態で出力結果を示します。

<!--
### Iteration and cleanup
-->
### 繰り返しの操作とクリーンアップ
{: #iteration-and-cleanup }

<!--
From here you can do everything you learned about in previous parts of the
tutorial.
-->
ここからは、これまでチュートリアルで学んだことをすべて実施していきます。

<!--
* Scale the app by changing the `docker-compose.yml` file and redeploy
on-the-fly with the `docker stack deploy` command.
-->
* アプリをスケーリングするには `docker-compose.yml` ファイルを変更し `docker stack deploy` コマンドを実行します。
  これによって再デプロイがすぐにできます。

<!--
* Change the app behavior by editing code, then rebuild, and push the new image.
(To do this, follow the same steps you took earlier to [build the
app](part2.md#build-the-app) and [publish the
image](part2.md#publish-the-image)).
-->
* アプリの機能を変更するには、コードを編集して再ビルドし、新たなイメージをプッシュします。
  （これを行うには、以前も行った手順と同様に [アプリをビルド](part2.md#build-the-app)し、[イメージを公開](part2.md#publish-the-image)します。）

<!--
* You can tear down the stack with `docker stack rm`. For example:
-->
* `docker stack rm` によりスタックを削除することができます。
  たとえば以下のとおりです。

  ```
  docker stack rm getstartedlab
  ```

<!--
Unlike the scenario where you were running the swarm on local Docker machine
VMs, your swarm and any apps deployed on it continue to run on cloud
servers regardless of whether you shut down your local host.
-->
ローカルの Docker マシン においてスウォームを実行してきたことと、今回は異なります。
今回のスウォームとデプロイしたアプリはクラウドサーバー上に稼動しています。
つまりローカルホストマシンをシャットダウンしても稼動し続けます。

{% endcapture %}
{% capture enterpriseboilerplate %}
<!--
Customers of Docker Enterprise Edition run a stable, commercially-supported
version of Docker Engine, and as an add-on they get our first-class management
software, Docker Datacenter. You can manage every aspect of your application
through the interface using Universal Control Plane, run a private image registry with Docker
Trusted Registry, integrate with your LDAP provider, sign production images with
Docker Content Trust, and many other features.
-->
Docker Enterprise エディションを利用すると、安定的な Docker Engine の商用サポート版が実行できます。
またそのアドオン機能として、最高レベルの管理ソフトウェア Docker Datacenter も利用できます。
アプリケーションのあやゆることを管理するインターフェースとして Universal Control Plane があります。
Docker Trusted Registry ではプライベートイメージを実行でき、LDAP プロバイダーとの統合も可能です。
また Docker Content Trust により本番環境イメージに電子サインを行うことができます。
この他にも機能はさまざまです。

{% endcapture %}
{% capture enterprisedeployapp %}
<!--
Once you're all set up and Docker Enterprise is running, you can [deploy your Compose
file from directly within the UI](/ee/ucp/swarm/deploy-multi-service-app/){: onclick="ga('send', 'event', 'Get Started Referral', 'Enterprise', 'Deploy app in UI');"}.
-->
セットアップをすべて行って Docker Enterprise を起動したら、[UI 上から直接 Compose ファイルをデプロイします](/ee/ucp/swarm/deploy-multi-service-app/){: onclick="ga('send', 'event', 'Get Started Referral', 'Enterprise', 'Deploy app in UI');"}。

<!--
![Deploy an app on Docker Enterprise](/ee/ucp/images/deploy-multi-service-app-2.png)
-->
![Docker Enterprise からアプリをデプロイ](/ee/ucp/images/deploy-multi-service-app-2.png)

<!--
After that, you can see it running, and can change any aspect of the application
you choose, or even edit the Compose file itself.
-->
アプリが実行したことはすぐにわかるはずです。
アプリケーションに対して必要となる変更はすぐに行うことができ、Compose ファイルそのものも編集することができます。

<!--
![Managing app on Docker Enterprise](/ee/ucp/images/deploy-multi-service-app-4.png)
-->
![Docker Enterprise 上でアプリを管理](/ee/ucp/images/deploy-multi-service-app-4.png)
{% endcapture %}
{% capture enterprise %}
{{ enterpriseboilerplate }}

<!--
Bringing your own server to Docker Enterprise and setting up Docker Datacenter
essentially involves two steps:
-->
それまでのサーバーを Docker Enterprise に移行し、Docker Datacenter 上にセットアップするには、基本的に以下の手順を進めます。

<!--
1. [Get Docker Enterprise for your server's OS from Docker Hub](https://hub.docker.com/search?offering=enterprise&type=edition){: onclick="ga('send', 'event', 'Get Started Referral', 'Enterprise', 'Get Docker EE for your OS');"}.
2. Follow the [instructions to install Docker Enterprise on your own host](/datacenter/install/linux/){: onclick="ga('send', 'event', 'Get Started Referral', 'Enterprise', 'BYOH setup guide');"}.
-->
1. [Docker Hub から、利用するサーバー OS に対する Docker Enterprise を入手します](https://hub.docker.com/search?offering=enterprise&type=edition){: onclick="ga('send', 'event', 'Get Started Referral', 'Enterprise', 'Get Docker EE for your OS');"}。
2. [ホスト上に Docker Enterprise をインストールする手順](/datacenter/install/linux/){: onclick="ga('send', 'event', 'Get Started Referral', 'Enterprise', 'BYOH setup guide');"}に従います。

<!--
> **Note**: Running Windows containers? View our [Windows Server setup guide](/install/windows/docker-ee.md){: onclick="ga('send', 'event', 'Get Started Referral', 'Enterprise', 'Windows Server setup guide');"}.
-->
> **メモ**: Windows コンテナーを実行？
>
その場合は [Windows Server セットアップガイド](/install/windows/docker-ee.md){: onclick="ga('send', 'event', 'Get Started Referral', 'Enterprise', 'Windows Server setup guide');"}を参照してください。

{{ enterprisedeployapp }}
{% endcapture %}

<ul class="nav nav-tabs">
  <li class="active"><a data-toggle="tab" href="#enterprise">Docker Enterprise</a></li>
  <li><a data-toggle="tab" href="#community">Docker Engine - Community</a></li>
</ul>
<div class="tab-content">
  <div id="enterprise" class="tab-pane fade in active" markdown="1">{{ enterprise }}</div>
  <div id="community" class="tab-pane fade" markdown="1">{{ community }}</div>
</div>

<!--
## Congratulations!
-->
## おつかれさま
{: #congratulations }

<!--
You've taken a full-stack, dev-to-deploy tour of the entire Docker platform.
-->
Docker のすべてにわたって開発からデプロイまでの流れを学びました。

<!--
There is much more to the Docker platform than what was covered here, but you
have a good idea of the basics of containers, images, services, swarms, stacks,
scaling, load-balancing, volumes, and placement constraints.
-->
Docker プラットフォームには、これまでに示してきたもの以上のことが数多くあります。
しかしここまで基本的な知識として、コンテナー、イメージ、サービス、スウォーム、スタック、スケーリング、負荷分散、ボリューム、placement 制約は、十分に理解できたと思います。

<!--
Want to go deeper? Here are some resources we recommend:
-->
より詳細に進みますか？
この先のお勧めする情報を以下に示します。

<!--
- [Samples](/samples/): Our samples include multiple examples of popular software
  running in containers, and some good labs that teach best practices.
- [User Guide](/engine/userguide/): The user guide has several examples that
  explain networking and storage in greater depth than was covered here.
- [Admin Guide](/engine/admin/): Covers how to manage a Dockerized production
  environment.
- [Training](https://training.docker.com/): Official Docker courses that offer
  in-person instruction and virtual classroom environments.
- [Blog](https://blog.docker.com): Covers what's going on with Docker lately.
-->
- [サンプル](/samples/): このサンプルにはポピュラーなソフトウェアをコンテナー上で実現する例を数多く提供しています。
  またベストプラクティスが学べるラボがあります。
- [ユーザーガイド](/engine/userguide/): ユーザーガイドにはいくつかの例が含まれていて、ここで示してきたものに比べて、より深いレベルでネットワークやストレージについて説明しています。
- [管理者ガイド](/engine/admin/): Docker 化された本番環境を管理する方法について説明します。
- [トレーニング](https://training.docker.com/): 公式の Docker 学習コースであり、対面での指導や仮想的な教室が提供されます。
- [ブログ](https://blog.docker.com): Docker の最新動向などを提供しています。
