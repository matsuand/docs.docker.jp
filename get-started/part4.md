---
title: "はじめよう 4部: スウォーム"
keywords: swarm, scale, cluster, machine, vm, manager, worker, deploy, ssh, orchestration
description: Learn how to create clusters of Dockerized machines.
---
{% include_relative nav.html selected="4" %}

## 前提条件
{: #prerequisites }

<!--
- [Install Docker version 1.13 or higher](/engine/installation/index.md).
-->
- [Docker バージョン 1.13 またはそれ以上をインストールしていること](/engine/installation/index.md)。

<!--
- Get [Docker Compose](/compose/overview.md) as described in [Part 3 prerequisites](/get-started/part3.md#prerequisites).
-->
- [3 部の前提条件](/get-started/part3.md#prerequisites)で説明した [Docker Compose](/compose/overview.md) を入手していること。

<!--
- Get [Docker Machine](/machine/overview.md), which is pre-installed with
[Docker Desktop for Mac](/docker-for-mac/index.md) and [Docker Desktop for
Windows](/docker-for-windows/index.md), but on Linux systems you need to
[install it directly](/machine/install-machine/#installing-machine-directly). On pre Windows 10 systems _without Hyper-V_, as well as Windows 10 Home, use
[Docker Toolbox](/toolbox/overview.md).
-->
- [Docker Machine](/machine/overview.md) を入手していること。
  [Docker Desktop for Mac](/docker-for-mac/index.md) と [Docker Desktop for Windows](/docker-for-windows/index.md) ではインストール済みであるが Linux システムでは[直接インストール](/machine/install-machine/#installing-machine-directly)が必要。
  Widows 10 Home のように Windows 10 システム上に Hyper-V が入っていない場合は [Docker Toolbox](/toolbox/overview.md) が必要。

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
slotting in your info for `username`, `repo`, and `tag`: `docker run -p 80:80
username/repo:tag`, then visit `http://localhost/`.
-->
- デプロイしたコンテナーとしてイメージが動作することを確認します。
  以下のコマンドを実行してください。
  `docker run -p 80:80 username/repo:tag`
  ここで username、repo、tag の部分は各環境に合わせて書き換えてください。
  そして `http://localhost/` にアクセスします。

<!--
- Have a copy of your `docker-compose.yml` from [Part 3](part3.md) handy.
-->
- [3 部](part3)で扱った `docker-compose.yml` ファイルが用意できていること。

<!--
## Introduction
-->
## はじめに
{: #introduction }

<!--
In [part 3](part3.md), you took an app you wrote in [part 2](part2.md), and
defined how it should run in production by turning it into a service, scaling it
up 5x in the process.
-->
[3 部](part3.md)では [2 部](part2.md)で書いたアプリをもとに、本番環境において実行可能なサービスとして調整したものを定義し、プロセスを 5 倍にスケールアップしました。

<!--
Here in part 4, you deploy this application onto a cluster, running it on
multiple machines. Multi-container, multi-machine applications are made possible
by joining multiple machines into a "Dockerized" cluster called a **swarm**.
-->
この 4 部では、デプロイするアプリケーションをクラスターにして、複数のマシン上で実行します。
複数コンテナー、複数マシンによるアプリケーションは、各マシンを「Docker化」（Dockerized）したクラスター、すなわち**スウォーム**（swarm）と呼ばれるものに集約することによって実現されます。

<!--
## Understanding Swarm clusters
-->
## スウォームクラスターの理解
{: #understanding-swarm-clusters }

<!--
A swarm is a group of machines that are running Docker and joined into
a cluster. After that has happened, you continue to run the Docker commands
you're used to, but now they are executed on a cluster by a **swarm manager**.
The machines in a swarm can be physical or virtual. After joining a swarm, they
are referred to as **nodes**.
-->
スウォームとは Docker の動作するマシンがひとまとまりとなってクラスターを構成するものです。
スウォームを使うようになると、これまで使ってきた Docker コマンドは引き続き用いることにはなりますが、今度はクラスターに対しての**スウォームマネージャー**として処理操作を行うものとなります。
スウォーム内のマシンは物理マシン、仮想マシンのいずれでも構いません。
マシンをスウォームに含めた後は**ノード**として参照されます。

<!--
Swarm managers can use several strategies to run containers, such as "emptiest
node" -- which fills the least utilized machines with containers. Or "global",
which ensures that each machine gets exactly one instance of the specified
container. You instruct the swarm manager to use these strategies in the Compose
file, just like the one you have already been using.
-->
スウォームマネージャーでは、コンテナーの実行にあたってストラテジーというものが指定できます。
たとえば「emptiest node」（最も空いているノード）です。
これは最も使われていないマシンをコンテナーに割り当てます。
「global」というものは、個々のマシンには、指定されたコンテナーの１インスタンスのみを割り当てます。
スウォームマネージャーに対してのストラテジー指定は Compose ファイルにて行います。
既に利用してきたファイルです。

<!--
Swarm managers are the only machines in a swarm that can execute your commands,
or authorize other machines to join the swarm as **workers**. Workers are just
there to provide capacity and do not have the authority to tell any other
machine what it can and cannot do.
-->
スウォームマネージャーは、スウォーム内でコマンド実行ができる唯一のマシンです。
そして他のマシンのスウォームへの参加を認証します。
スウォームに参加したマシンは**ワーカー**（worker）と呼ばれます。
ワーカーは処理提供するために加えられたわけですが、他のマシンの機能を制約する権限を持つわけではありません。

<!--
Up until now, you have been using Docker in a single-host mode on your local
machine. But Docker also can be switched into **swarm mode**, and that's what
enables the use of swarms. Enabling swarm mode instantly makes the current
machine a swarm manager. From then on, Docker runs the commands you execute
on the swarm you're managing, rather than just on the current machine.
-->
これまではローカルマシン上において、シングルホストモードにより Docker を利用してきました。
Docker は**スウォームモード**に切り替えることが可能であり、このモードにすることでスウォームが利用できるようになります。
スウォームを有効にした時点で現在のマシンがスウォームマネージャーとなります。
これ以降の Docker に対するコマンドはスウォームに対して実行されます。
もうそれまでのマシンに対して実行するものではなくなります。

<!--
## Set up your swarm
-->
## スウォームのセットアップ
{: #set-up-your-swarm }

<!--
A swarm is made up of multiple nodes, which can be either physical or virtual
machines. The basic concept is simple enough: run `docker swarm init` to enable
swarm mode and make your current machine a swarm manager, then run
`docker swarm join` on other machines to have them join the swarm as workers.
Choose a tab below to see how this plays out in various contexts. We use VMs
to quickly create a two-machine cluster and turn it into a swarm.
-->
スウォームは複数のノードにより構成されます。
それは物理マシン、仮想マシンのどちらでも構いません。
基本的な考え方はとても簡単です。
`docker swarm init` の実行によりスウォームモードが有効となり、現在のマシンがスウォームマネージャーになります。
その後に他のマシンから `docker swarm join` を実行すると、そのマシンはワーカーとしてスウォームに参加できます。
この仕組みがさまざまな環境においてどのように動作するか、以下のタブを切り替えて確認してください。
以下では VM を用いて、２つのマシンによるクラスターをさっと作り出して、これをスウォームに切り替えます。

<!--
### Create a cluster
-->
### クラスターの生成
{: #create-a-cluster }

<ul class="nav nav-tabs">
  <li class="active"><a data-toggle="tab" href="#local">ローカル VM (Mac, Linux, Windows 7 または 8)</a></li>
  <li><a data-toggle="tab" href="#localwin">ローカル VM (Windows 10/Hyper-V)</a></li>
</ul>
<div class="tab-content">
  <div id="local" class="tab-pane fade in active">
{% capture local-content %}

<!--
#### VMs on your local machine (Mac, Linux, Windows 7 and 8)
-->
#### ローカルマシン上の VM（Mac, Linux, Windows 7 または 8）
{: #vms-on-your-local-machine-mac-linux-windows-7-and-8 }

<!--
You need a hypervisor that can create virtual machines (VMs), so
[install Oracle VirtualBox](https://www.virtualbox.org/wiki/Downloads) for your
machine's OS.
-->
仮想マシン（virtual machine; VM）を生成するにはハイパーバイザーが必要です。
利用している OS に [Oracle VirtualBox](https://www.virtualbox.org/wiki/Downloads) をインストールしてください。

> **メモ**
>
Windows 10 のように Hyper-V が搭載された Windows システムの場合、VirtualBox のインストールは不要です。
かわりに Hyper-V を利用してください。
上記の Hyper-V に関するタブをクリックして Hyper-V システムの手順を参照してください。
[Docker Toolbox](/toolbox/overview.md) を利用している場合は、その一部としてすでに VirtualBox がインストールされるため、このまま先に進んでください。

<!--
Now, create a couple of VMs using `docker-machine`, using the VirtualBox driver:
-->
次に `docker-machine` を使って 2 つの仮想マシンを作成します。
ここでは VirtualBox ドライバーを使います。

```shell
docker-machine create --driver virtualbox myvm1
docker-machine create --driver virtualbox myvm2
```

{% endcapture %}
{{ local-content | markdownify }}

</div>
<div id="localwin" class="tab-pane fade" markdown="1">
{% capture localwin-content %}

<!--
#### VMs on your local machine (Windows 10)
-->
#### ローカルマシン上の VM（Windows 10）
{: #vms-on-your-local-machine-windows-10 }

<!--
First, quickly create a virtual switch for your virtual machines (VMs) to share,
so they can connect to each other.
-->
はじめに、仮想マシン（virtual machine; VM）が共有する仮想スイッチを作成して、仮想マシンが互いに接続できるようにします。

<!--
1. Launch Hyper-V Manager
2. Click **Virtual Switch Manager** in the right-hand menu
3. Click **Create Virtual Switch** of type **External**
4. Give it the name `myswitch`, and check the box to share your host machine's
   active network adapter
-->
1. Hyper-V マネージャーを起動
2. 右側のメニューにある **仮想スイッチ マネージャー** をクリック
3. **仮想スイッチの作成** のタイプ **外部** をクリック
4. `myswitch` という名称に設定し、ホストマシンのアクティブネットワークアダプタとの共有ボックスにチェックを入れる

<!--
Now, create a couple of VMs using our node management tool,
`docker-machine`:
-->
次にノード管理ツール `docker-machine` を使い 2 つの仮想マシンを作成します。

<!--
> **Note**: you need to run the following as administrator or else you don't have the permission to create hyperv VMs!
-->
> **メモ**
>
以下のコマンドは管理者権限で実行する必要があります。
管理者権限がないと Hyper-V の仮想マシンを生成することができません。

```shell
docker-machine create -d hyperv --hyperv-virtual-switch "myswitch" myvm1
docker-machine create -d hyperv --hyperv-virtual-switch "myswitch" myvm2
```

{% endcapture %}
{{ localwin-content | markdownify }}
</div>
<hr>
</div>

<!--
#### List the VMs and get their IP addresses
-->
#### VM の一覧および各 IP アドレスの取得
{: #list-the-vms-and-get-their-ip-addresses }

<!--
You now have two VMs created, named `myvm1` and `myvm2`.
-->
2 つ作り上げた VM の名前はそれぞれ `myvm1`、`myvm2` です。

<!--
Use this command to list the machines and get their IP addresses.
-->
以下のコマンドを使って、マシンおよび IP アドレスの一覧を得ます。

<!--
> **Note**: you need to run the following as administrator or else you don't get any resonable output (only "UNKNOWN").
-->
> **メモ**
>
以下のコマンドは管理者権限で実行する必要があります。
管理者権限がないと有効な情報を得ることができません（"UNKNOWN" が出力されるだけです）。

```shell
docker-machine ls
```

<!--
Here is example output from this command.
-->
以下はこのコマンドの出力例です。

```shell
$ docker-machine ls
NAME    ACTIVE   DRIVER       STATE     URL                         SWARM   DOCKER        ERRORS
myvm1   -        virtualbox   Running   tcp://192.168.99.100:2376           v17.06.2-ce
myvm2   -        virtualbox   Running   tcp://192.168.99.101:2376           v17.06.2-ce
```

<!--
#### Initialize the swarm and add nodes
-->
#### スウォームの初期化とノードの追加
{: #initialize-the-swarm-and-add-nodes }

<!--
The first machine acts as the manager, which executes management commands
and authenticates workers to join the swarm, and the second is a worker.
-->
1 つめのマシンはマネージャーとして動作します。
そして管理コマンドの実行と、このスウォームへ参加しようとするワーカーを認証します。
2 つめのマシンがそのワーカーです。

<!--
You can send commands to your VMs using `docker-machine ssh`. Instruct `myvm1`
to become a swarm manager with `docker swarm init` and look for output like
this:
-->
`docker-machine ssh` を利用すると VM に対してコマンドを送ることができます。
`myvm1` に対してスウォームマネージャーとなるように指示するために `docker swarm init` を実行します。
その出力は以下のようになります。

```shell
$ docker-machine ssh myvm1 "docker swarm init --advertise-addr <myvm1 ip>"
Swarm initialized: current node <node ID> is now a manager.

To add a worker to this swarm, run the following command:

  docker swarm join \
  --token <token> \
  <myvm ip>:<port>

To add a manager to this swarm, run 'docker swarm join-token manager' and follow the instructions.
```

<!--
> Ports 2377 and 2376
>
> Always run `docker swarm init` and `docker swarm join` with port 2377
> (the swarm management port), or no port at all and let it take the default.
>
> The machine IP addresses returned by `docker-machine ls` include port 2376,
> which is the Docker daemon port. Do not use this port or
> [you may experience errors](https://forums.docker.com/t/docker-swarm-join-with-virtualbox-connection-error-13-bad-certificate/31392/2){: target="_blank" class="_"}.
-->
> ポート 2377 と 2376
>
> `docker swarm init` と `docker swarm join` は必ずポート 2377 で実行してください。
> 他にポートはないので、これをデフォルトとしてください。
>
> `docker-machine ls` の出力結果にあるマシン IP アドレスは、ポート 2376 と示されています。
> これは Docker デーモン用のポートです。
> このポートを他に利用しないでください。
> 利用してしまうと[エラーが発生します](https://forums.docker.com/t/docker-swarm-join-with-virtualbox-connection-error-13-bad-certificate/31392/2){: target="_blank" class="_"}。

<!--
> Having trouble using SSH? Try the --native-ssh flag
>
> Docker Machine has [the option to let you use your own system's SSH](/machine/reference/ssh/#different-types-of-ssh), if
> for some reason you're having trouble sending commands to your Swarm manager. Just specify the
> `--native-ssh` flag when invoking the `ssh` command:
>
> ```
> docker-machine --native-ssh ssh myvm1 ...
> ```
-->
> SSH でトラブル？ フラグ --native-ssh を試す
>
> Docker Machine には、[システム内の SSH の利用を指示するオプション](/machine/reference/ssh/#different-types-of-ssh)があります。
> スウォームマネージャーへコマンドを送る際に、何らかの理由で処理に失敗した場合は、`ssh` コマンドを実行するときに `--native-ssh` フラグを指定して実行してください。
>
> ```
> docker-machine --native-ssh ssh myvm1 ...
> ```

<!--
As you can see, the response to `docker swarm init` contains a pre-configured
`docker swarm join` command for you to run on any nodes you want to add. Copy
this command, and send it to `myvm2` via `docker-machine ssh` to have `myvm2`
join your new swarm as a worker:
-->
上で見たように `docker swarm init` の出力結果に `docker swarm join` コマンドの準備ができていることが示されています。
このスウォームに加えたいノード上で実行できます。
コマンドをコピーしたら `docker-machine ssh` コマンド経由で `myvm2` にコマンドを送ってください。
`myvm2` がスウォームにおけるワーカーとして加わることになります。

```shell
$ docker-machine ssh myvm2 "docker swarm join \
--token <token> \
<ip>:2377"

This node joined a swarm as a worker.
```

<!--
Congratulations, you have created your first swarm!
-->
おめでとうございます。はじめてのスウォーム作りができました。

<!--
Run `docker node ls` on the manager to view the nodes in this swarm:
-->
マネージャー上で `docker node ls` を実行すると、スウォーム内のノードを見ることができます。

```shell
$ docker-machine ssh myvm1 "docker node ls"
ID                            HOSTNAME            STATUS              AVAILABILITY        MANAGER STATUS
brtu9urxwfd5j0zrmkubhpkbd     myvm2               Ready               Active
rihwohkh3ph38fhillhhb84sk *   myvm1               Ready               Active              Leader
```

<!--
> Leaving a swarm
>
> If you want to start over, you can run `docker swarm leave` from each node.
-->
> スウォームから去る（leave）
>
> もう一度やり直したい場合は、個々のノードから `docker swarm leave` を実行します。

<!--
## Deploy your app on the swarm cluster
-->
## スウォームクラスター上にアプリをデプロイ
{: #deploy-your-app-on-the-swarm-cluster }

<!--
The hard part is over. Now you just repeat the process you used in [part
3](part3.md) to deploy on your new swarm. Just remember that only swarm managers
like `myvm1` execute Docker commands; workers are just for capacity.
-->
面倒な部分はここまでです。
ここからは [3 部](part3.md)で行ったことを繰り返して、今度はスウォームをデプロイします。
Docker コマンドを実行できるのは `myvm1` のようなスウォームマネージャーだけです。
ワーカーは単に処理性能をあげるためにあるだけです。

<!--
### Configure a `docker-machine` shell to the swarm manager
-->
### Configure a `docker-machine` shell to the swarm manager
{: #configure-a-docker-machine-shell-to-the-swarm-manager }

So far, you've been wrapping Docker commands in `docker-machine ssh` to talk to
the VMs. Another option is to run `docker-machine env <machine>` to get
and run a command that configures your current shell to talk to the Docker
daemon on the VM. This method works better for the next step because it allows
you to use your local `docker-compose.yml` file to deploy the app
"remotely" without having to copy it anywhere.

Type `docker-machine env myvm1`, then copy-paste and run the command provided as
the last line of the output to configure your shell to talk to `myvm1`, the
swarm manager.

The commands to configure your shell differ depending on whether you are Mac,
Linux, or Windows, so examples of each are shown on the tabs below.

<ul class="nav nav-tabs">
  <li class="active"><a data-toggle="tab" href="#mac-linux-machine">Mac, Linux</a></li>
  <li><a data-toggle="tab" href="#win-machine">Windows</a></li>
</ul>
<div class="tab-content">
  <div id="mac-linux-machine" class="tab-pane fade in active">
  {% capture mac-linux-machine-content %}

<!--
#### Docker machine shell environment on Mac or Linux
-->
#### Docker machine shell environment on Mac or Linux
{: #docker-machine-shell-environment-on-mac-or-linux }

Run `docker-machine env myvm1` to get the command to configure your shell to
talk to `myvm1`.

```shell
$ docker-machine env myvm1
export DOCKER_TLS_VERIFY="1"
export DOCKER_HOST="tcp://192.168.99.100:2376"
export DOCKER_CERT_PATH="/Users/sam/.docker/machine/machines/myvm1"
export DOCKER_MACHINE_NAME="myvm1"
# Run this command to configure your shell:
# eval $(docker-machine env myvm1)
```

Run the given command to configure your shell to talk to `myvm1`.

```shell
eval $(docker-machine env myvm1)
```

Run `docker-machine ls` to verify that `myvm1` is now the active machine, as
indicated by the asterisk next to it.

```shell
$ docker-machine ls
NAME    ACTIVE   DRIVER       STATE     URL                         SWARM   DOCKER        ERRORS
myvm1   *        virtualbox   Running   tcp://192.168.99.100:2376           v17.06.2-ce
myvm2   -        virtualbox   Running   tcp://192.168.99.101:2376           v17.06.2-ce
```

{% endcapture %}
{{ mac-linux-machine-content | markdownify }}

</div>
<div id="win-machine" class="tab-pane fade">
{% capture win-machine-content %}

<!--
#### Docker machine shell environment on Windows
-->
#### Docker machine shell environment on Windows
{: #docker-machine-shell-environment-on-windows }

Run `docker-machine env myvm1` to get the command to configure your shell to
talk to `myvm1`.

```shell
PS C:\Users\sam\sandbox\get-started> docker-machine env myvm1
$Env:DOCKER_TLS_VERIFY = "1"
$Env:DOCKER_HOST = "tcp://192.168.203.207:2376"
$Env:DOCKER_CERT_PATH = "C:\Users\sam\.docker\machine\machines\myvm1"
$Env:DOCKER_MACHINE_NAME = "myvm1"
$Env:COMPOSE_CONVERT_WINDOWS_PATHS = "true"
# Run this command to configure your shell:
# & "C:\Program Files\Docker\Docker\Resources\bin\docker-machine.exe" env myvm1 | Invoke-Expression
```

Run the given command to configure your shell to talk to `myvm1`.

```shell
& "C:\Program Files\Docker\Docker\Resources\bin\docker-machine.exe" env myvm1 | Invoke-Expression
```

Run `docker-machine ls` to verify that `myvm1` is the active machine as indicated by the asterisk next to it.

```shell
PS C:PATH> docker-machine ls
NAME    ACTIVE   DRIVER   STATE     URL                          SWARM   DOCKER        ERRORS
myvm1   *        hyperv   Running   tcp://192.168.203.207:2376           v17.06.2-ce
myvm2   -        hyperv   Running   tcp://192.168.200.181:2376           v17.06.2-ce
```

  {% endcapture %}
  {{ win-machine-content | markdownify }}
  </div>
  <hr>
</div>

<!--
### Deploy the app on the swarm manager
-->
### Deploy the app on the swarm manager
{: #deploy-the-app-on-the-swarm-manager }

Now that you have `myvm1`, you can use its powers as a swarm manager to
deploy your app by using the same `docker stack deploy` command you used in part
3 to `myvm1`, and your local copy of `docker-compose.yml.`. This command may take a few seconds
to complete and the deployment takes some time to be available. Use the
`docker service ps <service_name>` command on a swarm manager to verify that
all services have been redeployed.

You are connected to `myvm1` by means of the `docker-machine` shell
configuration, and you still have access to the files on your local host. Make
sure you are in the same directory as before, which includes the
[`docker-compose.yml` file you created in part
3](/get-started/part3/#docker-composeyml).

Just like before, run the following command to deploy the app on `myvm1`.

```bash
docker stack deploy -c docker-compose.yml getstartedlab
```

And that's it, the app is deployed on a swarm cluster!

> **Note**: If your image is stored on a private registry instead of Docker Hub,
> you need to be logged in using `docker login <your-registry>` and then you
> need to add the `--with-registry-auth` flag to the above command. For example:
>
> ```bash
> docker login registry.example.com
>
> docker stack deploy --with-registry-auth -c docker-compose.yml getstartedlab
> ```
>
> This passes the login token from your local client to the swarm nodes where the
> service is deployed, using the encrypted WAL logs. With this information, the
> nodes are able to log into the registry and pull the image.
>

Now you can use the same [docker commands you used in part
3](/get-started/part3.md#run-your-new-load-balanced-app). Only this time notice
that the services (and associated containers) have been distributed between
both `myvm1` and `myvm2`.

```bash
$ docker stack ps getstartedlab

ID            NAME                  IMAGE                   NODE   DESIRED STATE
jq2g3qp8nzwx  getstartedlab_web.1   gordon/get-started:part2  myvm1  Running
88wgshobzoxl  getstartedlab_web.2   gordon/get-started:part2  myvm2  Running
vbb1qbkb0o2z  getstartedlab_web.3   gordon/get-started:part2  myvm2  Running
ghii74p9budx  getstartedlab_web.4   gordon/get-started:part2  myvm1  Running
0prmarhavs87  getstartedlab_web.5   gordon/get-started:part2  myvm2  Running
```

> Connecting to VMs with `docker-machine env` and `docker-machine ssh`
>
> * To set your shell to talk to a different machine like `myvm2`, simply re-run
`docker-machine env` in the same or a different shell, then run the given
command to point to `myvm2`. This is always specific to the current shell. If
you change to an unconfigured shell or open a new one, you need to re-run the
commands. Use `docker-machine ls` to list machines, see what state they are in,
get IP addresses, and find out which one, if any, you are connected to. To learn
more, see the [Docker Machine getting started topics](/machine/get-started.md#create-a-machine).
>
> * Alternatively, you can wrap Docker commands in the form of
`docker-machine ssh <machine> "<command>"`, which logs directly into
the VM but doesn't give you immediate access to files on your local host.
>
> * On Mac and Linux, you can use `docker-machine scp <file> <machine>:~`
to copy files across machines, but Windows users need a Linux terminal emulator
like [Git Bash](https://git-for-windows.github.io/){: target="_blank" class="_"} for this to work.
>
> This tutorial demos both `docker-machine ssh` and
`docker-machine env`, since these are available on all platforms via the `docker-machine` CLI.

<!--
### Accessing your cluster
-->
### Accessing your cluster
{: #accessing-your-cluster }

You can access your app from the IP address of **either** `myvm1` or `myvm2`.

The network you created is shared between them and load-balancing. Run
`docker-machine ls` to get your VMs' IP addresses and visit either of them on a
browser, hitting refresh (or just `curl` them).

![Hello World in browser](images/app-in-browser-swarm.png)

There are five possible container IDs all cycling by randomly, demonstrating
the load-balancing.

The reason both IP addresses work is that nodes in a swarm participate in an
ingress **routing mesh**. This ensures that a service deployed at a certain port
within your swarm always has that port reserved to itself, no matter what node
is actually running the container. Here's a diagram of how a routing mesh for a
service called `my-web` published at port `8080` on a three-node swarm would
look:

![routing mesh diagram](/engine/swarm/images/ingress-routing-mesh.png)

> Having connectivity trouble?
>
> Keep in mind that to use the ingress network in the swarm,
> you need to have the following ports open between the swarm nodes
> before you enable swarm mode:
>
> - Port 7946 TCP/UDP for container network discovery.
> - Port 4789 UDP for the container ingress network.
>
> Double check what you have in the ports section under your web
> service and make sure the ip addresses you enter in your browser
> or curl reflects that

<!--
## Iterating and scaling your app
-->
## Iterating and scaling your app
{: #iterating-and-scaling-your-app }

From here you can do everything you learned about in parts 2 and 3.

Scale the app by changing the `docker-compose.yml` file.

Change the app behavior by editing code, then rebuild, and push the new image.
(To do this, follow the same steps you took earlier to [build the
app](part2.md#build-the-app) and [publish the
image](part2.md#publish-the-image)).

In either case, simply run `docker stack deploy` again to deploy these changes.

You can join any machine, physical or virtual, to this swarm, using the
same `docker swarm join` command you used on `myvm2`, and capacity is added
to your cluster. Just run `docker stack deploy` afterwards, and your app can
take advantage of the new resources.

<!--
## Cleanup and reboot
-->
## Cleanup and reboot
{: #cleanup-and-reboot }

<!--
### Stacks and swarms
-->
### Stacks and swarms
{: #stacks-and-swarms }

You can tear down the stack with `docker stack rm`. For example:

```
docker stack rm getstartedlab
```

> Keep the swarm or remove it?
>
> At some point later, you can remove this swarm if you want to with
> `docker-machine ssh myvm2 "docker swarm leave"` on the worker
> and `docker-machine ssh myvm1 "docker swarm leave --force"` on the
> manager, but _you need this swarm for part 5, so keep it
> around for now_.

### Unsetting docker-machine shell variable settings

You can unset the `docker-machine` environment variables in your current shell
with the given command.

  On **Mac or Linux** the command is:

  ```shell
  eval $(docker-machine env -u)
  ```

  On **Windows** the command is:

  ```shell
  & "C:\Program Files\Docker\Docker\Resources\bin\docker-machine.exe" env -u | Invoke-Expression
  ```

This disconnects the shell from `docker-machine` created virtual machines,
and allows you to continue working in the same shell, now using native `docker`
commands (for example, on Docker Desktop for Mac or Docker Desktop for Windows). To learn more,
see the [Machine topic on unsetting environment variables](/machine/get-started/#unset-environment-variables-in-the-current-shell).

### Restarting Docker machines

If you shut down your local host, Docker machines stops running. You can check the status of machines by running `docker-machine ls`.

```
$ docker-machine ls
NAME    ACTIVE   DRIVER       STATE     URL   SWARM   DOCKER    ERRORS
myvm1   -        virtualbox   Stopped                 Unknown
myvm2   -        virtualbox   Stopped                 Unknown
```

To restart a machine that's stopped, run:

```
docker-machine start <machine-name>
```

For example:

```
$ docker-machine start myvm1
Starting "myvm1"...
(myvm1) Check network to re-create if needed...
(myvm1) Waiting for an IP...
Machine "myvm1" was started.
Waiting for SSH to be available...
Detecting the provisioner...
Started machines may have new IP addresses. You may need to re-run the `docker-machine env` command.

$ docker-machine start myvm2
Starting "myvm2"...
(myvm2) Check network to re-create if needed...
(myvm2) Waiting for an IP...
Machine "myvm2" was started.
Waiting for SSH to be available...
Detecting the provisioner...
Started machines may have new IP addresses. You may need to re-run the `docker-machine env` command.
```

[5 部へ >>](part5.md){: class="button outline-btn"}

## Recap and cheat sheet (optional)

Here's [a terminal recording of what was covered on this
page](https://asciinema.org/a/113837):

<script type="text/javascript" src="https://asciinema.org/a/113837.js" id="asciicast-113837" speed="2" async></script>

In part 4 you learned what a swarm is, how nodes in swarms can be managers or
workers, created a swarm, and deployed an application on it. You saw that the
core Docker commands didn't change from part 3, they just had to be targeted to
run on a swarm master. You also saw the power of Docker's networking in action,
which kept load-balancing requests across containers, even though they were
running on different machines. Finally, you learned how to iterate and scale
your app on a cluster.

Here are some commands you might like to run to interact with your swarm and your VMs a bit:

```shell
docker-machine create --driver virtualbox myvm1 # Create a VM (Mac, Win7, Linux)
docker-machine create -d hyperv --hyperv-virtual-switch "myswitch" myvm1 # Win10
docker-machine env myvm1                # View basic information about your node
docker-machine ssh myvm1 "docker node ls"         # List the nodes in your swarm
docker-machine ssh myvm1 "docker node inspect <node ID>"        # Inspect a node
docker-machine ssh myvm1 "docker swarm join-token -q worker"   # View join token
docker-machine ssh myvm1   # Open an SSH session with the VM; type "exit" to end
docker node ls                # View nodes in swarm (while logged on to manager)
docker-machine ssh myvm2 "docker swarm leave"  # Make the worker leave the swarm
docker-machine ssh myvm1 "docker swarm leave -f" # Make master leave, kill swarm
docker-machine ls # list VMs, asterisk shows which VM this shell is talking to
docker-machine start myvm1            # Start a VM that is currently not running
docker-machine env myvm1      # show environment variables and command for myvm1
eval $(docker-machine env myvm1)         # Mac command to connect shell to myvm1
& "C:\Program Files\Docker\Docker\Resources\bin\docker-machine.exe" env myvm1 | Invoke-Expression   # Windows command to connect shell to myvm1
docker stack deploy -c <file> <app>  # Deploy an app; command shell must be set to talk to manager (myvm1), uses local Compose file
docker-machine scp docker-compose.yml myvm1:~ # Copy file to node's home dir (only required if you use ssh to connect to manager and deploy the app)
docker-machine ssh myvm1 "docker stack deploy -c <file> <app>"   # Deploy an app using ssh (you must have first copied the Compose file to myvm1)
eval $(docker-machine env -u)     # Disconnect shell from VMs, use native docker
docker-machine stop $(docker-machine ls -q)               # Stop all running VMs
docker-machine rm $(docker-machine ls -q) # Delete all VMs and their disk images
```
