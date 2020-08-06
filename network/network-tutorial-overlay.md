---
title: オーバーレイネットワークのチュートリアル
description: Tutorials for networking with swarm services and standalone containers on multiple Docker daemons
keywords: networking, bridge, routing, ports, swarm, overlay
redirect_from:
- /engine/userguide/networking/get-started-overlay/
---

{% comment %}
This series of tutorials deals with networking for swarm services.
For networking with standalone containers, see
[Networking with standalone containers](network-tutorial-standalone.md). If you need to
learn more about Docker networking in general, see the [overview](index.md).
{% endcomment %}
ここに示すチュートリアルは、Swarm サービスに対するネットワークを扱います。
スタンドアロンコンテナーに対するネットワークについては、[スタンドアロンコンテナーのネットワークチュートリアル](network-tutorial-standalone.md) を参照してください。
Docker ネットワークの全般的なことを確認したい場合は [ネットワーク概要](index.md) を参照してください。

{% comment %}
This topic includes four different tutorials. You can run each of them on
Linux, Windows, or a Mac, but for the last two, you need a second Docker
host running elsewhere.
{% endcomment %}
このトピックには 4 つのチュートリアルがあります。
それぞれは Linux、Windows、Mac 上において実行することができます。
ただし Windows と Mac の場合は、2 つめの Docker ホストを、どこか別に用意することが必要になります。

{% comment %}
- [Use the default overlay network](#use-the-default-overlay-network) demonstrates
  how to use the default overlay network that Docker sets up for you
  automatically when you initialize or join a swarm. This network is not the
  best choice for production systems.
{% endcomment %}
- [デフォルトのオーバーレイネットワーク利用](#use-the-default-overlay-network) では、Swarm の初期化時、または Swarm への参加時に Docker が自動的に設定するデフォルトのオーバーレイネットワークの利用方法を示します。
  このネットワークは、本番環境向けには適していません。

{% comment %}
- [Use user-defined overlay networks](#use-a-user-defined-overlay-network) shows
  how to create and use your own custom overlay networks, to connect services.
  This is recommended for services running in production.
{% endcomment %}
- [ユーザー定義のオーバーレイネットワーク利用](#use-a-user-defined-overlay-network) では、独自にオーバーレイネットワークを生成し、サービスに接続して利用する方法を示します。
  本番環境においてサービスを稼動させる場合には、この方法が推奨されます。

{% comment %}
- [Use an overlay network for standalone containers](#use-an-overlay-network-for-standalone-containers)
  shows how to communicate between standalone containers on different Docker
  daemons using an overlay network.
{% endcomment %}
- [スタンドアロンコンテナーに対するオーバーレイネットワーク利用](#use-an-overlay-network-for-standalone-containers) では、異なる Docker デーモン上にあるスタンドアロンコンテナーに対して、オーバーレイネットワークを使って互いに通信する方法について説明します。

{% comment %}
- [Communicate between a container and a swarm service](#communicate-between-a-container-and-a-swarm-service)
  sets up communication between a standalone container and a swarm service,
  using an attachable overlay network. This is supported in Docker 17.06 and
  higher.
{% endcomment %}
- [コンテナー、Swarm サービス間の通信](#communicate-between-a-container-and-a-swarm-service) では、スタンドアロンコンテナーと Swarm サービスの間で通信を行うための設定を行います。
  そこではアタッチ可能なオーバーレイネットワークを用います。
  これは Docker 17.06 またはそれ以降においてサポートされます。

{% comment %}
## Prerequisites
{% endcomment %}
{: #prerequisites }
## 前提条件

{% comment %}
These requires you to have at least a single-node swarm, which means that
you have started Docker and run `docker swarm init` on the host. You can run
the examples on a multi-node swarm as well.
{% endcomment %}
最低でも単一ノードからなる Swarm が必要です。
つまり Docker ホスト上にデーモンが起動している状態で `docker swarm init` を実行します。
もちろん複数ノードの Swarm 上でも、利用例を試すことができます。

{% comment %}
The last example requires Docker 17.06 or higher.
{% endcomment %}
最後に示す利用例では Docker 17.06 またはそれ以降が必要です。

{% comment %}
## Use the default overlay network
{% endcomment %}
{: #use-the-default-overlay-network }
## デフォルトのオーバーレイネットワーク利用

{% comment %}
In this example, you start an `alpine` service and examine the characteristics
of the network from the point of view of the individual service containers.
{% endcomment %}
以下の例では `alpine` サービスを起動し、コンテナーの個々から見たネットワークの特徴を確認していきます。

{% comment %}
This tutorial does not go into operation-system-specific details about how
overlay networks are implemented, but focuses on how the overlay functions from
the point of view of a service.
{% endcomment %}
このチュートリアルでは、オーバーレイネットワークがどのように実装されているかといった、システム詳細については立ち入った説明はしません。
ただしサービスの観点から、オーバーレイ機能がどのようなものかは、詳しく見ていきます。

{% comment %}
### Prerequisites
{% endcomment %}
{: #prerequisites-1 }
### 前提条件

{% comment %}
This tutorial requires three physical or virtual Docker hosts which can all
communicate with one another, all running new installations of Docker 17.03 or
higher. This tutorial assumes that the three hosts are running on the same
network with no firewall involved.
{% endcomment %}
このチュートリアルでは、物理ホスト、仮想ホストは問わず Docker ホストを 3 つ利用して、互いに通信を行うようにします。
この 3 つには Docker 17.03 またはそれ以降がインストールされ、同一のネットワーク上にファイアウォールなしに稼動しているものとします。

{% comment %}
These hosts will be referred to as `manager`, `worker-1`, and `worker-2`. The
`manager` host will function as both a manager and a worker, which means it can
both run service tasks and manage the swarm. `worker-1` and `worker-2` will
function as workers only,
{% endcomment %}
各ホストは `manager`、`worker-1`、`worker-2` とします。
`manager` ホストは、マネージャーとワーカーの両方の役割を持つものです。
つまりこれは、サービスタスクが稼動すると同時に、Swarm の管理も行います。
`worker-1` と `worker-2` は、ワーカーとしてのみ動作します。

{% comment %}
If you don't have three hosts handy, an easy solution is to set up three
Ubuntu hosts on a cloud provider such as Amazon EC2, all on the same network
with all communications allowed to all hosts on that network (using a mechanism
such as EC2 security groups), and then to follow the
[installation instructions for Docker Engine - Community on Ubuntu](../engine/install/ubuntu.md).
{% endcomment %}
3 つのホストを手元で自由に使えないといったときには、簡単な策として Amazon EC2 などのクラウドプロバイダー上に 3 つの Ubuntu ホストを設定するという方法があります。
そうすれば同一のネットワーク上において、各ホストが確実に通信できるようになります（EC2 のセキュリティグループなどの機能を利用して実現されます）。
これを実行するなら、[Ubuntu 向け Docker Engine - Community のインストール](../engine/install/ubuntu.md) の手順に従ってください。

{% comment %}
### Walkthrough
{% endcomment %}
{: #walkthrough }
### ウォークスルー

{% comment %}
#### Create the swarm
{% endcomment %}
{: #create-the-swarm }
#### Swarm の生成

{% comment %}
At the end of this procedure, all three Docker hosts will be joined to the swarm
and will be connected together using an overlay network called `ingress`.
{% endcomment %}
この手順を進めることで、最終的には 3 つの Docker ホストが Swarm に参加し、`ingress` というオーバーレイネットワークを使って互いに通信できる状態になります。

{% comment %}
1.  On `manager`. initialize the swarm. If the host only has one network
    interface, the `--advertise-addr` flag is optional.
{% endcomment %}
1.  `manager` において Swarm を初期化します。
    このホストがただ 1 つのネットワークインターフェースしかない場合は `--advertise-addr` フラグの指定は任意です。

    {% comment %}
    ```bash
    $ docker swarm init --advertise-addr=<IP-ADDRESS-OF-MANAGER>
    ```
    {% endcomment %}
    ```bash
    $ docker swarm init --advertise-addr=<managerのIPアドレス>
    ```

    {% comment %}
    Make a note of the text that is printed, as this contains the token that
    you will use to join `worker-1` and `worker-2` to the swarm. It is a good
    idea to store the token in a password manager.
    {% endcomment %}
    出力される文字列は書きとめておいてください。
    そこにはトークンが出力されます。
    この後に `worker-1` と `worker-2` を Swarm に参加させる際に必要となります。
    パスワード管理ツールがあれば、そこにトークンを保存しておくのでもよいでしょう。

{% comment %}
2.  On `worker-1`, join the swarm. If the host only has one network interface,
    the `--advertise-addr` flag is optional.
{% endcomment %}
2.  `worker-1` 上から、これを Swarm に参加させます。
    このホストがただ 1 つのネットワークインターフェースしかない場合は `--advertise-addr` フラグの指定は任意です。

    {% comment %}
    ```bash
    $ docker swarm join --token <TOKEN> \
      --advertise-addr <IP-ADDRESS-OF-WORKER-1> \
      <IP-ADDRESS-OF-MANAGER>:2377
    ```
    {% endcomment %}
    ```bash
    $ docker swarm join --token <トークン> \
      --advertise-addr <worker-1のIPアドレス> \
      <managerのIPアドレス>:2377
    ```

{% comment %}
3.  On `worker-2`, join the swarm. If the host only has one network interface,
    the `--advertise-addr` flag is optional.
{% endcomment %}
2.  `worker-2` 上から、これを Swarm に参加させます。
    このホストがただ 1 つのネットワークインターフェースしかない場合は `--advertise-addr` フラグの指定は任意です。

    {% comment %}
    ```bash
    $ docker swarm join --token <TOKEN> \
      --advertise-addr <IP-ADDRESS-OF-WORKER-2> \
      <IP-ADDRESS-OF-MANAGER>:2377
    ```
    {% endcomment %}
    ```bash
    $ docker swarm join --token <トークン> \
      --advertise-addr <worker-2のIPアドレス> \
      <managerのIPアドレス>:2377
    ```

{% comment %}
4.  On `manager`, list all the nodes. This command can only be done from a
    manager.
{% endcomment %}
4.  `manager` においてノード一覧を確認します。
    このコマンドはマネージャーからしか実行することができません。

    ```bash
    $ docker node ls

    ID                            HOSTNAME            STATUS              AVAILABILITY        MANAGER STATUS
    d68ace5iraw6whp7llvgjpu48 *   ip-172-31-34-146    Ready               Active              Leader
    nvp5rwavvb8lhdggo8fcf7plg     ip-172-31-35-151    Ready               Active
    ouvx2l7qfcxisoyms8mtkgahw     ip-172-31-36-89     Ready               Active
    ```

    {% comment %}
    You can also use the `--filter` flag to filter by role:
    {% endcomment %}
    `--filter` フラグを使って役割（role）を絞ることができます。

    ```bash
    $ docker node ls --filter role=manager

    ID                            HOSTNAME            STATUS              AVAILABILITY        MANAGER STATUS
    d68ace5iraw6whp7llvgjpu48 *   ip-172-31-34-146    Ready               Active              Leader

    $ docker node ls --filter role=worker

    ID                            HOSTNAME            STATUS              AVAILABILITY        MANAGER STATUS
    nvp5rwavvb8lhdggo8fcf7plg     ip-172-31-35-151    Ready               Active
    ouvx2l7qfcxisoyms8mtkgahw     ip-172-31-36-89     Ready               Active
    ```

{% comment %}
5.  List the Docker networks on `manager`, `worker-1`, and `worker-2` and notice
    that each of them now has an overlay network called `ingress` and a bridge
    network called `docker_gwbridge`. Only the listing for `manager` is shown
    here:
{% endcomment %}
5.  `manager`、`worker-1`、`worker-2` のそれぞれにおいて、Docker ネットワークの一覧を表示します。
    そこにはいずれも `ingress` というオーバーレイネットワークがあり、`docker_gwbridge` というブリッジネットワークがあることを確認してください。
    なお以下では `manager` におけるネットワーク一覧のみを示します。

    ```bash
    $ docker network ls

    NETWORK ID          NAME                DRIVER              SCOPE
    495c570066be        bridge              bridge              local
    961c6cae9945        docker_gwbridge     bridge              local
    ff35ceda3643        host                host                local
    trtnl4tqnc3n        ingress             overlay             swarm
    c8357deec9cb        none                null                local
    ```

{% comment %}
The `docker_gwbridge` connects the `ingress` network to the Docker host's
network interface so that traffic can flow to and from swarm managers and
workers. If you create swarm services and do not specify a network, they are
connected to the `ingress` network. It is recommended that you use separate
overlay networks for each application or group of applications which will work
together. In the next procedure, you will create two overlay networks and
connect a service to each of them.
{% endcomment %}
`docker_gwbridge` は `ingress` ネットワークを Docker ホストのネットワークインターフェースに接続するものであり、これがあることで、Swarm マネージャーとワーカーとの間でのトラフィックが通信できるようになります。
Swarm サービスを生成する際にネットワーク指定を行わない場合は、そのサービスは `ingress` ネットワークに接続されます。
いくつかのアプリケーションがまとまって動作するような場合には、各アプリケーションごとにオーバーレイネットワークを用いることが推奨されます。
次の手順では、オーバーレイネットワークを 2 つ生成して、サービスをそこに接続します。

{% comment %}
#### Create the services
{% endcomment %}
{: #create-the-services }
#### サービスの生成

{% comment %}
1.  On `manager`, create a new overlay network called `nginx-net`:
{% endcomment %}
1.  `manager` において、`nginx-net` という名前の新たなオーバーレイネットワークを生成します。

    ```bash
    $ docker network create -d overlay nginx-net
    ```

    {% comment %}
    You don't need to create the overlay network on the other nodes, because it
    will be automatically created when one of those nodes starts running a
    service task which requires it.
    {% endcomment %}
    このオーバーレイネットワークは、他のノード上で生成する必要はありません。
    それぞれのノードにおいて、このネットワークを必要とするサービスタスクが起動した際に、ネットワークは自動生成されます。

{% comment %}
2.  On `manager`, create a 5-replica Nginx service connected to `nginx-net`. The
    service will publish port 80 to the outside world. All of the service
    task containers can communicate with each other without opening any ports.
{% endcomment %}
2.  `manager` において、`nginx-net` に接続する Nginx サービスをレプリカ数 5 で生成します。
    このサービスは外部に対してはポート 80 を公開します。
    サービスタスクを実行するコンテナーは、ポートを公開しなくても互いに通信できるようになっています。

    {% comment %}
    > **Note**: Services can only be created on a manager.
    {% endcomment %}
    > **メモ**: サービスはマネージャー上からしか生成できません。

    ```bash
    $ docker service create \
      --name my-nginx \
      --publish target=80,published=80 \
      --replicas=5 \
      --network nginx-net \
      nginx
      ```

      {% comment %}
      The default publish mode of `ingress`, which is used when you do not
      specify a `mode` for the `--publish` flag, means that if you browse to
      port 80 on `manager`, `worker-1`, or `worker-2`, you will be connected to
      port 80 on one of the 5 service tasks, even if no tasks are currently
      running on the node you browse to. If you want to publish the port using
      `host` mode, you can add `mode=host` to the `--publish` output. However,
      you should also use `--mode global` instead of `--replicas=5` in this case,
      since only one service task can bind a given port on a given node.
      {% endcomment %}
      `ingress` のデフォルトの公開モードは、`--publish` フラグに対して `mode` を指定しなかった場合に用いられます。
      そして `manager`、`worker-1`、`worker-2` のポート 80 にアクセスしたときに、5 つのサービスタスクのどれか 1 つのポート 80 に接続できるものであり、たとえ接続したノードそのものにおいて、その時点でサービスタスクが稼動していなくても接続が可能なものです。
      `host` モードを利用してポートを公開したい場合は、`--publish` において `mode=host` を追加します。
      ただしその場合は、`--replicas=5` を指定するのではなく `--mode global` としなければなりません。
      そのときには、指定されたノード上の指定されたポートに割り当てることができるサービスタスクは、ただ 1 つになるからです。

{% comment %}
3.  Run `docker service ls` to monitor the progress of service bring-up, which
    may take a few seconds.
{% endcomment %}
3.  `docker service ls` を実行して、サービスの稼働状況を確認してみます。
    これには数秒かかります。

{% comment %}
4.  Inspect the `nginx-net` network on `manager`, `worker-1`, and `worker-2`.
    Remember that you did not need to create it manually on `worker-1` and
    `worker-2` because Docker created it for you. The output will be long, but
    notice the `Containers` and `Peers` sections. `Containers` lists all
    service tasks (or standalone containers) connected to the overlay network
    from that host.
{% endcomment %}
4.  `manager`、`worker-1`、`worker-2` 上の `nginx-net` ネットワークを確認します。
    `worker-1` と `worker-2` においては、ネットワークを手動で生成する必要はなく、Docker が生成してくれるものであることは、前に説明しました。
    出力結果は長いものになりますが、`Containers` と `Peers` という項をよく確認してください。
    `Containers` には、ホストからオーバーレイネットワークに接続されたサービスタスク（あるいはスタンドアロンコンテナー）の一覧が出力されています。

{% comment %}
5.  From `manager`, inspect the service using `docker service inspect my-nginx`
    and notice the information about the ports and endpoints used by the
    service.
{% endcomment %}
5.  `manager` において、`docker service inspect my-nginx` を実行してサービスを確認します。
    サービスによって利用されているポートとエンドポイントの情報を確認してください。

{% comment %}
6.  Create a new network `nginx-net-2`, then update the service to use this
    network instead of `nginx-net`:
{% endcomment %}
6.  新たに `nginx-net-2` というネットワークを生成します。
    そしてそれまでの `nginx-net` の代わりとして、このネットワークをサービスが利用するようにアップデートします。

    ```bash
    $ docker network create -d overlay nginx-net-2
    ```

    ```bash
    $ docker service update \
      --network-add nginx-net-2 \
      --network-rm nginx-net \
      my-nginx
    ```

{% comment %}
7.  Run `docker service ls` to verify that the service has been updated and all
    tasks have been redeployed. Run `docker network inspect nginx-net` to verify
    that no containers are connected to it. Run the same command for
    `nginx-net-2` and notice that all the service task containers are connected
    to it.
{% endcomment %}
7.  `docker service ls` を実行して、サービスがアップデートされたことと、タスクがすべて再デプロイされたことを確認します。
    そして `docker network inspect nginx-net` を実行して、このネットワークに接続しているコンテナーは 1 つもないことを確認します。
    同様のコマンドを `nginx-net-2` に対しても実行し、サービスタスクコンテナーがすべて、そのネットワークに接続されていることを確認します。

    {% comment %}
    > **Note**: Even though overlay networks are automatically created on swarm
    > worker nodes as needed, they are not automatically removed.
    {% endcomment %}
    > **メモ**: Swarm ワーカーノードにおいては、処理状況に応じてオーバーレイネットワークが自動生成されますが、これは自動的には削除されません。

{% comment %}
8.  Clean up the service and the networks. From `manager`, run the following
    commands. The manager will direct the workers to remove the networks
    automatically.
{% endcomment %}
8.  サービスとネットワークを削除します。
    `manager` 上から、以下のコマンドを実行します。
    マネージャーからは、ワーカーに対してネットワークを削除するよう、自動的に指示が送られます。

    ```bash
    $ docker service rm my-nginx
    $ docker network rm nginx-net nginx-net-2
    ```

{% comment %}
## Use a user-defined overlay network
{% endcomment %}
{: #use-a-user-defined-overlay-network }
## ユーザー定義のオーバーレイネットワーク利用

{% comment %}
### Prerequisites
{% endcomment %}
{: #prerequisites-2 }
### 前提条件

{% comment %}
This tutorial assumes the swarm is already set up and you are on a manager.
{% endcomment %}
このチュートリアルでは、Swarm がすでに設定済みであり、マネージャーにアクセスできているものとします。

{% comment %}
### Walkthrough
{% endcomment %}
{: #walkthrough-1 }
### ウォークスルー

{% comment %}
1.  Create the user-defined overlay network.
{% endcomment %}
1.  ユーザー定義のオーバーレイネットワークを生成します。

    ```bash
    $ docker network create -d overlay my-overlay
    ```

{% comment %}
2.  Start a service using the overlay network and publishing port 80 to port
    8080 on the Docker host.
{% endcomment %}
2.  そのオーバーレイネットワークを使ってサービスを起動します。
    ポート 80 を Docker ホストのポート 8080 に公開します。

    ```bash
    $ docker service create \
      --name my-nginx \
      --network my-overlay \
      --replicas 1 \
      --publish published=8080,target=80 \
      nginx:latest
    ```

{% comment %}
3.  Run `docker network inspect my-overlay` and verify that the `my-nginx`
    service task is connected to it, by looking at the `Containers` section.
{% endcomment %}
3.  `docker network inspect my-overlay` を実行して、`my-nginx` サービスタスクが `my-overlay` に接続されていることを確認します。
    確認は `Containers` の項を見ます。

{% comment %}
4.  Remove the service and the network.
{% endcomment %}
4.  サービスとネットワークを削除します。

    ```bash
    $ docker service rm my-nginx

    $ docker network rm my-overlay
    ```

{% comment %}
## Use an overlay network for standalone containers
{% endcomment %}
{: #use-an-overlay-network-for-standalone-containers }
## スタンドアロンコンテナーに対するオーバーレイネットワーク利用

{% comment %}
This example demonstrates DNS container discovery -- specifically, how to
communicate between standalone containers on different Docker daemons using an
overlay network. Steps are:
{% endcomment %}
この例では、DNS によるコンテナー検出を試してみます。
特に、オーバーレイネットワークを利用する複数の Docker デーモン上に、スタンドアロンコンテナーが稼動していて、その間での通信情報を確認していきます。

{% comment %}
- On `host1`, initialize the node as a swarm (manager).
- On `host2`, join the node to the swarm (worker).
- On `host1`, create an attachable overlay network (`test-net`).
- On `host1`, run an interactive [alpine](https://hub.docker.com/_/alpine/) container (`alpine1`) on `test-net`.
- On `host2`, run an interactive, and detached, [alpine](https://hub.docker.com/_/alpine/) container (`alpine2`) on `test-net`.
- On `host1`, from within a session of `alpine1`, ping `alpine2`.
{% endcomment %}
- `host1` では、Swarm としてノードを初期化します。（マネージャー）
- `host2` では、Swarm に対してノード参加します。（ワーカー）
- `host1` では、アタッチ可能なオーバーレイネットワークを生成します。（`test-net`）
- `host1` では、対話的な [alpine](https://hub.docker.com/_/alpine/) コンテナー（`alpine1`）を `test-net` 上で実行します。
- `host2` では、対話的な [alpine](https://hub.docker.com/_/alpine/) コンテナー（`alpine2`）を、デタッチモードにより `test-net` 上で実行します。
- `host1` では、`alpine1` のセッション内部から `alpine2` に対して ping を行います。

{% comment %}
### Prerequisites
{% endcomment %}
{: #prerequisites-3 }
### 前提条件

{% comment %}
For this test, you need two different Docker hosts that can communicate with
each other. Each host must have Docker 17.06 or higher with the following ports
open between the two Docker hosts:
{% endcomment %}
このテストでは、Docker ホストが 2 つ、互いに通信できるものとして用意する必要があります。
両ホストは Docker 17.06 またはそれ以降がインストールされている必要があり、両ホスト間で以下のポートが公開されていることが必要です。

{% comment %}
- TCP port 2377
- TCP and UDP port 7946
- UDP port 4789
{% endcomment %}
- TCP ポート 2377
- TCP と UDP のポート 7946
- UDP ポート 4789

{% comment %}
One easy way to set this up is to have two VMs (either local or on a cloud
provider like AWS), each with Docker installed and running. If you're using AWS
or a similar cloud computing platform, the easiest configuration is to use a
security group that opens all incoming ports between the two hosts and the SSH
port from your client's IP address.
{% endcomment %}
このような環境を準備する簡単な方法として、2 つの VM（ローカルなもの、あるいは AWS のようなクラウドプロバイダー上のもの）を利用することが考えられます。
そこで Docker をインストールし起動させます。
AWS や同様のクラウドコンピューティングプラットフォームを利用している場合は、セキュリティグループを利用すれば、ごく簡単に設定ができます。
これを使えば、2 つのホスト間での入力ポートはすべて開放され、手元のクライアントからは IP アドレスを使って SSH 接続することができます。

{% comment %}
This example refers to the two nodes in our swarm as `host1` and `host2`. This
example also uses Linux hosts, but the same commands work on Windows.
{% endcomment %}
この例では、Swarm 上に配置する 2 つのノードを `host1`、`host2` とします。
またこの例では Linux ホストを用いますが、同じコマンドは Windows 上でも動作します。

{% comment %}
### Walk-through
{% endcomment %}
{: #walk-through }
### ウォークスルー

{% comment %}
1.  Set up the swarm.
{% endcomment %}
1.  Swarm を設定します。

    {% comment %}
    a.  On `host1`, initialize a swarm (and if prompted, use `--advertise-addr`
        to specify the IP address for the interface that communicates with other
        hosts in the swarm, for instance, the private IP address on AWS):
    {% endcomment %}
    a.  `host1` において Swarm を初期化します。
        （プロンプトが出る場合は `--advertise-addr` を指定します。
         これを使って、Swarm 内の別ホストの間で通信を行うインターフェースの IP アドレスを指定します。
         たとえば AWS 上であれば、プライベート IP アドレスを指定します。）


    ```bash
    $ docker swarm init
    Swarm initialized: current node (vz1mm9am11qcmo979tlrlox42) is now a manager.

    To add a worker to this swarm, run the following command:

        docker swarm join --token SWMTKN-1-5g90q48weqrtqryq4kj6ow0e8xm9wmv9o6vgqc5j320ymybd5c-8ex8j0bc40s6hgvy5ui5gl4gy 172.31.47.252:2377

    To add a manager to this swarm, run 'docker swarm join-token manager' and follow the instructions.
    ```

    {% comment %}
    b.  On `host2`, join the swarm as instructed above:
    {% endcomment %}
    b.  `host2` において、上のコマンド実行により出力された Swarm への参加を行います。

    {% comment %}
    ```bash
    $ docker swarm join --token <your_token> <your_ip_address>:2377
    This node joined a swarm as a worker.
    ```
    {% endcomment %}
    ```bash
    $ docker swarm join --token <トークン> <IPアドレス>:2377
    This node joined a swarm as a worker.
    ```

    {% comment %}
    If the node fails to join the swarm, the `docker swarm join` command times
    out. To resolve, run `docker swarm leave --force` on `host2`, verify your
    network and firewall settings, and try again.
    {% endcomment %}
    Swarm へのノード参加に失敗すると、`docker swarm join` コマンドはタイムアウトします。
    これを解消するには `host2` 上で `docker swarm leave --force` を実行して、ネットワークとファイアウォールの設定を正しくした上で、再度コマンド実行を試してみてください。

{% comment %}
2.  On `host1`, create an attachable overlay network called `test-net`:
{% endcomment %}
2.  `host1` において `test-net` という名前の、アタッチ可能なオーバーレイネットワークを生成します。

    ```bash
    $ docker network create --driver=overlay --attachable test-net
    uqsof8phj3ak0rq9k86zta6ht
    ```

    {% comment %}
    > Notice the returned **NETWORK ID** -- you will see it again when you connect to it from `host2`.
    {% endcomment %}
    > ここで出力される **ネットワーク ID** を覚えておいてください。
    > 同じものが `host2` から接続した際にも表示されます。

{% comment %}
3.  On `host1`, start an interactive (`-it`) container (`alpine1`) that connects to `test-net`:
{% endcomment %}
3.  `host1` において、`test-net` に接続する対話形式（`-it`）のコンテナー（`alpine1`）を起動します。

    ```bash
    $ docker run -it --name alpine1 --network test-net alpine
    / #
    ```

{% comment %}
4.  On `host2`, list the available networks -- notice that `test-net` does not yet exist:
{% endcomment %}
4.  `host2` において利用可能なネットワーク一覧を表示します。
    なお `test-net` はこの時点ではまだ存在しません。

    ```bash
    $ docker network ls
    NETWORK ID          NAME                DRIVER              SCOPE
    ec299350b504        bridge              bridge              local
    66e77d0d0e9a        docker_gwbridge     bridge              local
    9f6ae26ccb82        host                host                local
    omvdxqrda80z        ingress             overlay             swarm
    b65c952a4b2b        none                null                local
    ```

{% comment %}
5.  On `host2`, start a detached (`-d`) and interactive (`-it`) container (`alpine2`) that connects to `test-net`:
{% endcomment %}
5.  `host2` において、`test-net` に接続するデタッチモード（`-d`）かつ対話形式（`-it`）のコンテナー（`alpine2`）を起動します。

    ```bash
    $ docker run -dit --name alpine2 --network test-net alpine
    fb635f5ece59563e7b8b99556f816d24e6949a5f6a5b1fbd92ca244db17a4342
    ```

    {% comment %}
    > Automatic DNS container discovery only works with unique container names.
    {% endcomment %}
    > DNS のコンテナー自動検出の機能は、一意のコンテナー名に対してのみ動作します。

{% comment %}
6. On `host2`, verify that `test-net` was created (and has the same NETWORK ID as `test-net` on `host1`):
{% endcomment %}
6. `host2` において `test-net` が生成されていることを確認します。
   （そして `host1` 上から見た `test-net` と同じように、同一のネットワーク ID となっていることを確認します。）

    ```bash
    $ docker network ls
    NETWORK ID          NAME                DRIVER              SCOPE
    ...
    uqsof8phj3ak        test-net            overlay             swarm
    ```

{% comment %}
7.  On `host1`, ping `alpine2` within the interactive terminal of `alpine1`:
{% endcomment %}
7.  `host1` において、`alpine1` の対話型ターミナル内から `alpine2` に向けて ping を行います。

    ```bash
    / # ping -c 2 alpine2
    PING alpine2 (10.0.0.5): 56 data bytes
    64 bytes from 10.0.0.5: seq=0 ttl=64 time=0.600 ms
    64 bytes from 10.0.0.5: seq=1 ttl=64 time=0.555 ms

    --- alpine2 ping statistics ---
    2 packets transmitted, 2 packets received, 0% packet loss
    round-trip min/avg/max = 0.555/0.577/0.600 ms
    ```

    {% comment %}
    The two containers communicate with the overlay network connecting the two
    hosts. If you run another alpine container on `host2` that is _not detached_,
    you can ping `alpine1` from `host2` (and here we add the
    [remove option](https://docs.docker.com/engine/reference/run/#clean-up---rm) for automatic container cleanup):
    {% endcomment %}
    2 つのコンテナーは、2 つのホストに接続しているオーバーレイネットワークを使って、互いに通信します。
    `host2` からもう一つ別の alpine コンテナーを、**デタッチモードではなく** 起動すると、`hosts` から `alpine1` へ ping を行うことができます。
    （そしてここでは [rm オプション](https://docs.docker.com/engine/reference/run/#clean-up---rm) を指定に加えて、コンテナーの自動削除を行うことにします。）

    ```sh
    $ docker run -it --rm --name alpine3 --network test-net alpine
    / # ping -c 2 alpine1
    / # exit
    ```

{% comment %}
8.  On `host1`, close the `alpine1` session (which also stops the container):
{% endcomment %}
8.  `host1` において `alpine1` のセッションを閉じます。
    （さらにコンテナーも停止します。）

    ```bash
    / # exit
    ```

{% comment %}
9.  Clean up your containers and networks:
{% endcomment %}
9.  コンテナーとネットワークを削除します。

    {% comment %}
    You must stop and remove the containers on each host independently because
    Docker daemons operate independently and these are standalone containers.
    You only have to remove the network on `host1` because when you stop
    `alpine2` on `host2`, `test-net` disappears.
    {% endcomment %}
    コンテナーの停止と削除は、個々のホスト上においてそれぞれ行うことが必要です。
    Docker デーモンはそれぞれに動作しているからであり、コンテナーはすべてスタンドアロンだからです。
    ネットワークを削除するのは `host1` 上だけで十分です。
    `host2` 上にて `alpine2` を停止したら `test-net` はなくなります。

    {% comment %}
    a.  On `host2`, stop `alpine2`, check that `test-net` was removed, then remove `alpine2`:
    {% endcomment %}
    a.  `host2` において `alpine2` を停止させます。
        そして `test-net` が削除されていることを確認した上で `alpine2` を削除します。

    ```bash
    $ docker container stop alpine2
    $ docker network ls
    $ docker container rm alpine2
    ```

    {% comment %}
    a.  On `host1`, remove `alpine1` and `test-net`:
    {% endcomment %}
    a.  `host1` において `alpine1` と `test-net` を削除します。

    ```bash
    $ docker container rm alpine1
    $ docker network rm test-net
    ```

{% comment %}
## Communicate between a container and a swarm service
{% endcomment %}
{: #communicate-between-a-container-and-a-swarm-service }
## コンテナー、Swarm サービス間の通信

{% comment %}
### Prerequisites
{% endcomment %}
{: #prerequisites-4 }
### 前提条件

{% comment %}
You need Docker 17.06 or higher for this example.
{% endcomment %}
この例においては Docker 17.06 またはそれ以降が必要です。

{% comment %}
### Walkthrough
{% endcomment %}
{: #walkthrough-2 }
### ウォークスルー

{% comment %}
In this example, you start two different `alpine` containers on the same Docker
host and do some tests to understand how they communicate with each other. You
need to have Docker installed and running.
{% endcomment %}
この例では、2 つの `alpine` コンテナーを同一の Docker ホスト上に稼動させます。
そしてテストを行って、コンテナー間の通信がどのように行われるかを確認します。
Docker がインストール済みであり起動していることを確認いてください。

{% comment %}
1.  Open a terminal window. List current networks before you do anything else.
    Here's what you should see if you've never added a network or initialized a
    swarm on this Docker daemon. You may see different networks, but you should
    at least see these (the network IDs will be different):
{% endcomment %}
1.  ターミナル画面を開きます。
    まず初めに、現在のネットワーク一覧を確認しておきます。
    ネットワークをまったく追加せず、Docker デーモン上において Swarm の初期化も行っていなければ、以下のような表示になるはずです。
    複数のネットワークが表示されるはずであり、最低で以下のものがあるはずです。
    （ネットワーク ID は異なります。）

    ```bash
    $ docker network ls

    NETWORK ID          NAME                DRIVER              SCOPE
    17e324f45964        bridge              bridge              local
    6ed54d316334        host                host                local
    7092879f2cc8        none                null                local
    ```

    {% comment %}
    The default `bridge` network is listed, along with `host` and `none`. The
    latter two are not fully-fledged networks, but are used to start a container
    connected directly to the Docker daemon host's networking stack, or to start
    a container with no network devices. **This tutorial will connect two
    containers to the `bridge` network.**
    {% endcomment %}
    デフォルトの `bridge` ネットワークが一覧に表示されます。
    これとともに `host` と `none` があります。
    この 2 つは完全なネットワークではありませんが、コンテナーを起動して Docker デーモンホストのネットワークに直接接続するために、あるいはネットワークデバイスのないコンテナーを起動するために必要となります。
    **このチュートリアルでは、2 つのコンテナーを `bridge` ネットワークに接続します。**

{% comment %}
2.  Start two `alpine` containers running `ash`, which is Alpine's default shell
    rather than `bash`. The `-dit` flags mean to start the container detached
    (in the background), interactive (with the ability to type into it), and
    with a TTY (so you can see the input and output). Since you are starting it
    detached, you won't be connected to the container right away. Instead, the
    container's ID will be printed. Because you have not specified any
    `--network` flags, the containers connect to the default `bridge` network.
{% endcomment %}
2.  `alpine` コンテナーを 2 つ起動して `ash` を実行します。
    Alpine のデフォルトシェルが `bash` ではなく `ash` です。
    `-dit` フラグは、コンテナーをデタッチモードで（バックグラウンドで）実行し、対話を行い（入力を可能とし）、TTY を利用する（入出力が確認できる）ことを意味します。
    デタッチモードで起動するため、コンテナーに即座に接続されるわけではありません。
    その前にコンテナー ID が出力されます。
    `--network` フラグを何も指定しなかったので、コンテナーはデフォルトの `bridge` ネットワークに接続されます。

    ```bash
    $ docker run -dit --name alpine1 alpine ash

    $ docker run -dit --name alpine2 alpine ash
    ```

    {% comment %}
    Check that both containers are actually started:
    {% endcomment %}
    2 つのコンテナーが実際に開始されたことを確認します。

    ```bash
    $ docker container ls

    CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
    602dbf1edc81        alpine              "ash"               4 seconds ago       Up 3 seconds                            alpine2
    da33b7aa74b0        alpine              "ash"               17 seconds ago      Up 16 seconds                           alpine1
    ```

{% comment %}
3.  Inspect the `bridge` network to see what containers are connected to it.
{% endcomment %}
3.  `bridge` ネットワークを参照して、どのコンテナーがこれに接続しているかを確認します。

    ```bash
    $ docker network inspect bridge

    [
        {
            "Name": "bridge",
            "Id": "17e324f459648a9baaea32b248d3884da102dde19396c25b30ec800068ce6b10",
            "Created": "2017-06-22T20:27:43.826654485Z",
            "Scope": "local",
            "Driver": "bridge",
            "EnableIPv6": false,
            "IPAM": {
                "Driver": "default",
                "Options": null,
                "Config": [
                    {
                        "Subnet": "172.17.0.0/16",
                        "Gateway": "172.17.0.1"
                    }
                ]
            },
            "Internal": false,
            "Attachable": false,
            "Containers": {
                "602dbf1edc81813304b6cf0a647e65333dc6fe6ee6ed572dc0f686a3307c6a2c": {
                    "Name": "alpine2",
                    "EndpointID": "03b6aafb7ca4d7e531e292901b43719c0e34cc7eef565b38a6bf84acf50f38cd",
                    "MacAddress": "02:42:ac:11:00:03",
                    "IPv4Address": "172.17.0.3/16",
                    "IPv6Address": ""
                },
                "da33b7aa74b0bf3bda3ebd502d404320ca112a268aafe05b4851d1e3312ed168": {
                    "Name": "alpine1",
                    "EndpointID": "46c044a645d6afc42ddd7857d19e9dcfb89ad790afb5c239a35ac0af5e8a5bc5",
                    "MacAddress": "02:42:ac:11:00:02",
                    "IPv4Address": "172.17.0.2/16",
                    "IPv6Address": ""
                }
            },
            "Options": {
                "com.docker.network.bridge.default_bridge": "true",
                "com.docker.network.bridge.enable_icc": "true",
                "com.docker.network.bridge.enable_ip_masquerade": "true",
                "com.docker.network.bridge.host_binding_ipv4": "0.0.0.0",
                "com.docker.network.bridge.name": "docker0",
                "com.docker.network.driver.mtu": "1500"
            },
            "Labels": {}
        }
    ]
    ```

    {% comment %}
    Near the top, information about the `bridge` network is listed, including
    the IP address of the gateway between the Docker host and the `bridge`
    network (`172.17.0.1`). Under the `Containers` key, each connected container
    is listed, along with information about its IP address (`172.17.0.2` for
    `alpine1` and `172.17.0.3` for `alpine2`).
    {% endcomment %}
    上の方に `bridge` ネットワークに関する情報が一覧表示されます。
    Docker ホストと `bridge` ネットワーク間のゲートウェイに対する IP アドレス（`172.17.0.1`）も表示されています。
    `Containers` キーの配下に、接続されているコンテナーがそれぞれ表示されています。
    そこには IP アドレスの情報もあります（`alpine1` が `172.17.0.2`、`alpine2` が `172.17.0.3` となっています）。

{% comment %}
4.  The containers are running in the background. Use the `docker attach`
    command to connect to `alpine1`.
{% endcomment %}
4.  コンテナーはバックグラウンドで実行しています。
    `docker attach` コマンドを使って `alpine1` に接続してみます。

    ```bash
    $ docker attach alpine1

    / #
    ```

    {% comment %}
    The prompt changes to `#` to indicate that you are the `root` user within
    the container. Use the `ip addr show` command to show the network interfaces
    for `alpine1` as they look from within the container:
    {% endcomment %}
    プロンプトが `#` に変わりました。
    これはコンテナー内の `root` ユーザーであることを意味します。
    `ip addr show` コマンドを使って、コンテナー内部から `alpine1` のネットワークインターフェースを見てみます。

    ```bash
    # ip addr show

    1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN qlen 1
        link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
        inet 127.0.0.1/8 scope host lo
           valid_lft forever preferred_lft forever
        inet6 ::1/128 scope host
           valid_lft forever preferred_lft forever
    27: eth0@if28: <BROADCAST,MULTICAST,UP,LOWER_UP,M-DOWN> mtu 1500 qdisc noqueue state UP
        link/ether 02:42:ac:11:00:02 brd ff:ff:ff:ff:ff:ff
        inet 172.17.0.2/16 scope global eth0
           valid_lft forever preferred_lft forever
        inet6 fe80::42:acff:fe11:2/64 scope link
           valid_lft forever preferred_lft forever
    ```

    {% comment %}
    The first interface is the loopback device. Ignore it for now. Notice that
    the second interface has the IP address `172.17.0.2`, which is the same
    address shown for `alpine1` in the previous step.
    {% endcomment %}
    1 つめのインターフェースはループバックデバイスです。
    今はこれを無視します。
    2 つめのインターフェースの IP アドレスは `172.17.0.2` となっています。
    前の手順で確認した `alpine1` のアドレスと同じです。

{% comment %}
5.  From within `alpine1`, make sure you can connect to the internet by
    pinging `google.com`. The `-c 2` flag limits the command two two `ping`
    attempts.
{% endcomment %}
5.  `alpine1` の内部から `google.com` への ping を行って、インターネットに接続してみます。
    `-c 2` フラグにより 2 回だけ `ping` を行います。

    ```bash
    # ping -c 2 google.com

    PING google.com (172.217.3.174): 56 data bytes
    64 bytes from 172.217.3.174: seq=0 ttl=41 time=9.841 ms
    64 bytes from 172.217.3.174: seq=1 ttl=41 time=9.897 ms

    --- google.com ping statistics ---
    2 packets transmitted, 2 packets received, 0% packet loss
    round-trip min/avg/max = 9.841/9.869/9.897 ms
    ```

{% comment %}
6.  Now try to ping the second container. First, ping it by its IP address,
    `172.17.0.3`:
{% endcomment %}
6.  そこで 2 つめのコンテナーに対して ping してみます。
    最初は IP アドレス `172.17.0.3` を使って ping します。

    ```bash
    # ping -c 2 172.17.0.3

    PING 172.17.0.3 (172.17.0.3): 56 data bytes
    64 bytes from 172.17.0.3: seq=0 ttl=64 time=0.086 ms
    64 bytes from 172.17.0.3: seq=1 ttl=64 time=0.094 ms

    --- 172.17.0.3 ping statistics ---
    2 packets transmitted, 2 packets received, 0% packet loss
    round-trip min/avg/max = 0.086/0.090/0.094 ms
    ```

    {% comment %}
    This succeeds. Next, try pinging the `alpine2` container by container
    name. This will fail.
    {% endcomment %}
    成功しました。
    次に `alpine2` コンテナーに向けて、コンテナー名により ping をしてみます。
    これは失敗します。

    ```bash
    # ping -c 2 alpine2

    ping: bad address 'alpine2'
    ```

{% comment %}
7.  Detach from `alpine1` without stopping it by using the detach sequence,
    `CTRL` + `p` `CTRL` + `q` (hold down `CTRL` and type `p` followed by `q`).
    If you wish, attach to `alpine2` and repeat steps 4, 5, and 6 there,
    substituting `alpine1` for `alpine2`.
{% endcomment %}
7.  `alpine1` を停止させることなくデタッチします。
    これはデタッチを行うキー操作、つまり `CTRL` + `p`、`CTRL` + `q` により行います（`CTRL` を押したまま、`p` と `q` を順に押します）。
    この後 `alpine2` に対して同じことをするなら、手順の 4、5、6 をもう一度行います。
    `alpine1` のところは `alpine2` に変えて実施します。

{% comment %}
8.  Stop and remove both containers.
{% endcomment %}
8.  2 つのコンテナーを停止させ削除します。

    ```bash
    $ docker container stop alpine1 alpine2
    $ docker container rm alpine1 alpine2
    ```

{% comment %}
Remember, the default `bridge` network is not recommended for production. To
learn about user-defined bridge networks, continue to the
[next tutorial](#use-user-defined-bridge-networks).
{% endcomment %}
デフォルトの `bridge` ネットワークは、本番環境向けとしては推奨されない点を覚えておいてください。
ユーザー定義のブリッジネットワークについては、[次のチュートリアル](#use-user-defined-bridge-networks) に進んでください。

{% comment %}
## Other networking tutorials
{% endcomment %}
{: #other-networking-tutorials }
## その他のネットワークチュートリアル

{% comment %}
Now that you have completed the networking tutorials for overlay networks,
you might want to run through these other networking tutorials:
{% endcomment %}
オーバーレイネットワークのチュートリアルを終えたので、以下に示すような別のネットワークチュートリアルも見てください。

{% comment %}
- [Host networking tutorial](network-tutorial-host.md)
- [Standalone networking tutorial](network-tutorial-standalone.md)
- [Macvlan networking tutorial](network-tutorial-macvlan.md)
{% endcomment %}
- [ホストネットワークのチュートリアル](network-tutorial-host.md)
- [スタンドアロンネットワークのチュートリアル](network-tutorial-standalone.md)
- [Macvlan ネットワークのチュートリアル](network-tutorial-macvlan.md)
