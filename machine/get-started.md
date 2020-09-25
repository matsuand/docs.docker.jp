---
description: Get started with Docker Machine and a local VM
keywords: machine, virtualbox
title: Docker Machine とローカル VM を使ってはじめよう
hide_from_sitemap: true
---

{% comment %}
Let's take a look at using `docker-machine` to create, use, and manage a
Docker host inside of a local virtual machine.
{% endcomment %}
`docker-machine`コマンドを使って、ローカルの仮想マシン内部に Docker ホストを生成して管理する方法を見ていきます。

{% comment %}
## Prerequisite information
{% endcomment %}
{: #prerequisite-information }
## 前提条件

{% comment %}
With the advent of [Docker Desktop for Mac](../docker-for-mac/index.md) and
[Docker Desktop for Windows](../docker-for-windows/index.md) as replacements for
[Docker Toolbox](../toolbox/overview.md), we recommend that you use these for your
primary Docker workflows. You can use these applications to run Docker natively
on your local system without using Docker Machine at all. (See
[Docker Desktop for Mac vs. Docker Toolbox](../docker-for-mac/docker-toolbox.md)
for an explanation on the Mac side.)
{% endcomment %}
[Docker Toolbox](../toolbox/overview.md) に代わるものとして [Docker Desktop for Mac](../docker-for-mac/index.md) や [Docker Desktop for Windows](../docker-for-windows/index.md) が開発されているので、Docker を使った作業には、これらの新しいアプリケーションを利用することをお勧めします。
このアプリケーションを利用すれば Docker Machine を用いることなく、ローカルシステム上において Docker をネイティブに実行することができます。
（Mac 上における説明については [Docker Desktop for Mac と Docker Toolbox](../docker-for-mac/docker-toolbox.md) を参照してください。）

{% comment %}
For now, however, if you want to create _multiple_ local machines, you still
need Docker Machine to create and manage machines for multi-node
experimentation. Both Docker Desktop for Mac and Docker Desktop for Windows include the newest
version of Docker Machine, so when you install either of these, you get
`docker-machine`.
{% endcomment %}
もっとも現在のところ、ローカルマシンを **複数** 生成するには Docker Machine がまだ必要になります。
Docker Machine では、複数ノードを用いたマシンの生成や管理を行うことができます。
Docker Desktop for Mac や Docker Desktop for Windows には、いずれも最新版の Docker Machine が含まれるため、どちらかをインストールしていれば`docker-machine`は入手できていることになります。

{% comment %}
The new solutions come with their own native virtualization solutions rather
than Oracle VirtualBox, so keep the following considerations in mind when using
Machine to create local VMs.
{% endcomment %}
この新たなソリューションでは、Oracle VirtualBox を用いることなく、独自にネイティブな仮想技術を導入しています。
したがって Docker Machine を使ってローカル VM を生成していく上では、以下のことを覚えておいてください。

{% comment %}
* **Docker Desktop for Mac** - You can use `docker-machine create` with the `virtualbox` driver to create additional local machines.
{% endcomment %}
* **Docker Desktop for Mac** - `docker-machine create`コマンドによって追加のローカルマシンを生成する際には、`virtualbox`ドライバーが用いられます。

{% comment %}
* **Docker Desktop for Windows** - You can use `docker-machine create` with the `hyperv` driver to create additional local machines.
{% endcomment %}
* **Docker Desktop for Windows** - `docker-machine create`コマンドによって追加のローカルマシンを生成する際には`hyperv`ドライバーが用いられます。

{% comment %}
#### If you are using Docker Desktop for Windows
{% endcomment %}
{: #if-you-are-using-docker-desktop-for-windows }
#### Docker Desktop for Windows を使う場合

{% comment %}
Docker Desktop for Windows uses [Microsoft
Hyper-V](https://msdn.microsoft.com/en-us/virtualization/hyperv_on_windows/windows_welcome)
for virtualization, and Hyper-V is not compatible with Oracle VirtualBox.
Therefore, you cannot run the two solutions simultaneously. But you can still
use `docker-machine` to create more local VMs by using the Microsoft Hyper-V
driver.
{% endcomment %}
Docker Desktop for Windows では、仮想化技術として [Microsoft Hyper-V](https://msdn.microsoft.com/en-us/virtualization/hyperv_on_windows/windows_welcome) を用います。
Hyper-V は Oracle VirtualBox と互換性はありません。
したがってこの 2 つのソリューションを同時に実行することはできません。
`docker-machine`コマンドが Microsoft Hyper-V ドライバーを用いていれば、ローカル VM をさらに作り出していくことができます。

{% comment %}
The prerequisites are:
{% endcomment %}
前提条件は以下のとおりです。

{% comment %}
* Have Docker Desktop for Windows installed, and running (which requires that virtualization and Hyper-V are enabled, as described in [What to know before you install Docker Desktop for Windows](../docker-for-windows/install.md#what-to-know-before-you-install)).
{% endcomment %}
* Docker Desktop for Windows がインストールされていて、実行していることが必要です。
  （実行するためには仮想化環境および Hyper-V が有効であることが必要です。
   このことは [Docker Desktop for Windows をインストール前に確認すべきこと](../docker-for-windows/install.md#what-to-know-before-you-install) において説明されています。）

{% comment %}
* Set up the Hyper-V driver to use an external virtual network switch See
the [Docker Machine driver for Microsoft Hyper-V](drivers/hyper-v.md) topic,
which includes an [example](drivers/hyper-v.md#example) of how to do this.
{% endcomment %}
* Hyper-V ドライバーを、外部の仮想ネットワークスイッチとして設定していることが必要です。
  [Docker Machine の Microsoft Hyper-V 向けドライバー](drivers/hyper-v.md) のトピックを参照してください。
  そこではこのドライバーをどのように用いるかの [利用例](drivers/hyper-v.md#example) を示しています。

{% comment %}
#### If you are using Docker Desktop for Mac
{% endcomment %}
{: #if-you-are-using-docker-desktop-for-mac }
#### Docker Desktop for Mac を使う場合

{% comment %}
Docker Desktop for Mac uses [HyperKit](https://github.com/docker/HyperKit/), a
lightweight macOS virtualization solution built on top of the
[Hypervisor.framework](https://developer.apple.com/reference/hypervisor).
{% endcomment %}
Docker Desktop for Mac では [HyperKit](https://github.com/docker/HyperKit/) が用いられます。
これは macOS の軽量な仮想化ソリューションであり、[Hypervisor.framework](https://developer.apple.com/reference/hypervisor) の最上位に構築されています。
.

{% comment %}
Currently, there is no `docker-machine create` driver for HyperKit, so
use the `virtualbox` driver to create local machines. (See the
[Docker Machine driver for Oracle VirtualBox](drivers/virtualbox.md).) You can
run both HyperKit and Oracle VirtualBox on the same system. To learn more, see
[Docker Desktop for Mac vs. Docker Toolbox](../docker-for-mac/docker-toolbox.md).
{% endcomment %}
現時点で`docker-machine create`が利用する HyperKit 用のドライバーはありません。
したがってローカルマシンの生成には`virtualbox`ドライバーが用いられます。
（[Docker Machine の Oracle VirtualBox 向けドライバー](drivers/virtualbox.md) を参照してください。）
HyperKit と Oracle VirtualBox は 1 つのシステム上に稼動させることができます。
詳しくは [Docker Desktop for Mac と Docker Toolbox](../docker-for-mac/docker-toolbox.md) を参照してください。

{% comment %}
* Make sure you have [the latest VirtualBox](https://www.virtualbox.org/wiki/Downloads){: target="_blank" class="_"}
  correctly installed on your system (either as part of an earlier Toolbox install,
  or manual install).
{% endcomment %}
* [最新版の VirtualBox](https://www.virtualbox.org/wiki/Downloads){: target="_blank" class="_"} が正しくインストールされていることを確認してください。
  （すでに示した ToolBox の一部としてインストールしているか、直接インストールしているかのどちらでもかまいません。）

{% comment %}
#### If you are using Docker Toolbox
{% endcomment %}
{: #if-you-are-using-docker-toolbox }
#### Docker Toolbox を使う場合

{% comment %}
Docker Desktop for Mac and Docker Desktop for Windows both require newer versions of their
respective operating systems, so users with older OS versions must use Docker
Toolbox.
{% endcomment %}
Docker Desktop for Mac と Docker Desktop for Windows では、それぞれのオペレーティングシステムの最新版を必要とします。
したがってシステムバージョンが古いユーザーは Docker Toolbox を利用しなければなりません。

{% comment %}
* If you are using Docker Toolbox on either Mac or an older version Windows
  system (without Hyper-V), use the `virtualbox` driver to create a local machine
  based on Oracle [VirtualBox](https://www.virtualbox.org/){:target="_blank" class="_"}.
  (See the [Docker Machine driver for Oracle VirtualBox](drivers/virtualbox.md).)
{% endcomment %}
* Docker Toolbox を Mac 上において、あるいは（Hyper-V がない）古い Windows 上において利用している場合は、Oracle [VirtualBox](https://www.virtualbox.org/){:target="_blank" class="_"} に基づいたローカルマシンを生成する際に`virtualbox`ドライバーが用いられます。
  （[Docker Machine の Oracle VirtualBox 向けドライバー](drivers/virtualbox.md) を参照してください。）

{% comment %}
* If you are using Docker Toolbox on a Windows system that has Hyper-V but cannot
  run Docker Desktop for Windows (for example Windows 8 Pro), you must use the
  `hyperv` driver to create local machines. (See th
   [Docker Machine driver for Microsoft Hyper-V](drivers/hyper-v.md).)
{% endcomment %}
* Docker Toolbox を Windows 上で利用している場合で、Hyper-V 機能はあるものの Docker Desktop for Windows は稼動させていない場合（たとえば Windows 8 pro の場合）には、ローカルマシンの生成に`hyperv`ドライバーが必要になります。
  （[Docker Machine の Microsoft Hyper-V 向けドライバー](drivers/hyper-v.md) を参照してください。）

{% comment %}
* Make sure you have [the latest VirtualBox](https://www.virtualbox.org/wiki/Downloads){: target="_blank" class="_"}
  correctly installed on your system. If you used
  [Toolbox](https://www.docker.com/products/docker-toolbox){: target="_blank" class="_"}
  or [Docker Desktop for Windows](../docker-for-windows/index.md){: target="_blank" class="_"}
  to install Docker Machine, VirtualBox is
  automatically installed.
{% endcomment %}
* [最新版の VirtualBox](https://www.virtualbox.org/wiki/Downloads){: target="_blank" class="_"} が正しくインストールされていることを確認してください。
  [Toolbox](https://www.docker.com/products/docker-toolbox){: target="_blank" class="_"} または [Docker Desktop for Windows](../docker-for-windows/index.md){: target="_blank" class="_"} を利用して Docker Machine がインストールできている場合、VirtualBox は自動的にインストールされています。

{% comment %}
* If you used the Quickstart Terminal to launch your first machine and set your
  terminal environment to point to it, a default machine was automatically
  created. If so, you can still follow along with these steps, but
  create another machine and name it something other than `default`.
{% endcomment %}
* Quickstart Terminal を利用してマシン起動を行っていて、ターミナル画面の環境を設定済みである場合、デフォルトのマシンは自動的に生成されています。
  その場合でも、ここに示す手順に従って進めることはできます。
  ただしマシンは別のものを生成して、その名前は`default`以外のものにしてください。

{% comment %}
##  Use Machine to run Docker containers
{% endcomment %}
{: #use-machine-to-run-docker-containers }
##  Docker Machine を使ったコンテナーの起動

{% comment %}
To run a Docker container, you:
{% endcomment %}
Docker コンテナーを実行するには、以下を行います。

{% comment %}
* create a new (or start an existing) Docker virtual machine
* switch your environment to your new VM
* use the docker client to create, load, and manage containers
{% endcomment %}
* 新しい Docker 仮想マシンを生成します（あるいは既存の仮想マシンを起動します）。
* 環境変数を新しい仮想マシンに切り替えます。
* docker クライアントを使ってコンテナーの作成、ロード、管理を行います。

{% comment %}
Once you create a machine, you can reuse it as often as you like. Like any VirtualBox VM, it maintains its configuration between uses.
{% endcomment %}
Docker Machine で生成したマシンは、必要に応じて何度でも再利用できます。
そのマシンは VirtualBox 上にあるそれまでの仮想マシンと同じものであり、どちらでも同じ設定が使われます。

{% comment %}
The examples here show how to create and start a machine, run Docker commands, and work with containers.
{% endcomment %}
以下の例では、マシンの生成、起動、 Docker コマンドの実行、コンテナーの使い方を見ていきます。

{% comment %}
## Create a machine
{% endcomment %}
{: #create-a-machine }
## マシンの生成

{% comment %}
1. Open a command shell or terminal window.
{% endcomment %}
1. コマンドシェルやターミナル画面を開きます。

    {% comment %}
    These command examples shows a Bash shell. For a different shell, such as C Shell, the same commands are the same except where noted.
    {% endcomment %}
    以下の例では Bash シェルを扱います。
    C シェルのような他のシェルでは、動作しないコマンドがあるかもしれないので注意してください。

{% comment %}
2. Use `docker-machine ls` to list available machines.
{% endcomment %}
2. `docker-machine ls`を使って利用可能なマシンの一覧を表示します。

    {% comment %}
    In this example, no machines have been created yet.
    {% endcomment %}
    以下の例では、マシンがまだ 1 台も生成されていないことがわかります。

        $ docker-machine ls
        NAME   ACTIVE   DRIVER   STATE   URL   SWARM   DOCKER   ERRORS

{% comment %}
3. Create a machine.
{% endcomment %}
3. マシンを生成します。

    {% comment %}
    Run the `docker-machine create` command, pass the appropriate driver to the
`--driver` flag and provide a machine name. If this is your first machine, name
it `default` as shown in the example. If you already have a "default" machine,
choose another name for this new machine.
    {% endcomment %}
    コマンド`docker-machine create`を実行します。
    引数として`--driver`フラグには適切なドライバー名を指定し、マシン名を与えます。
    マシンを生成するのはこれが初めての場合、例に示しているように名称は`default`としてください。
    すでに「default｣というマシンが存在している場合は、別のマシン名を指定します。

    {% comment %}
    * If you are using Toolbox on Mac, Toolbox on older Windows systems without Hyper-V, or Docker Desktop for Mac, use `virtualbox` as the driver, as shown in this example. (The Docker Machine VirtualBox driver reference is [here](drivers/virtualbox.md).) (See [prerequisites](get-started.md#prerequisite-information) above to learn more.)
    {% endcomment %}
    * 利用しているシステムが以下の場合、つまり Toolbox on Mac であるか、Hyper-V のない古い Windows 上でのToolbox か、Docker Desktop for Mac である場合、ドライバーとしては、例に示しているように`virtualbox`を指定します。
      （Docker Machine の VirtualBox 向けドライバーのリファレンスは [こちら](drivers/virtualbox.md) にあります。）
      （[prerequisites](get-started.md#prerequisite-information) を再度確認してください。）

    {% comment %}
    * On Docker Desktop for Windows systems that support Hyper-V, use the `hyperv` driver as shown in the [Docker Machine Microsoft Hyper-V driver reference](drivers/hyper-v.md) and follow the [example](drivers/hyper-v.md#example), which shows how to use an external network switch and provides the flags for the full command. (See [prerequisites](get-started.md#prerequisite-information) above to learn more.)
    {% endcomment %}
    * Hyper-V をサポートしている Windows 上において Docker Desktop for Windows を利用いている場合は`hyperv`ドライバーを指定します。
      このドライバーについては [Docker Machine の Microsoft Hyper-V 向けドライバーリファレンス](drivers/hyper-v.md) に示されています。
      そこにある [利用例](drivers/hyper-v.md#example) に従ってください。
      そこでは外部ネットワークスイッチの利用方法を示すとともに、各コマンドのフラグについて説明しています。
      （詳しくは上の [前提条件](get-started.md#prerequisite-information) を参照してください。）

            $ docker-machine create --driver virtualbox default
            Running pre-create checks...
            Creating machine...
            (staging) Copying /Users/ripley/.docker/machine/cache/boot2docker.iso to /Users/ripley/.docker/machine/machines/default/boot2docker.iso...
            (staging) Creating VirtualBox VM...
            (staging) Creating SSH key...
            (staging) Starting the VM...
            (staging) Waiting for an IP...
            Waiting for machine to be running, this may take a few minutes...
            Machine is running, waiting for SSH to be available...
            Detecting operating system of created instance...
            Detecting the provisioner...
            Provisioning with boot2docker...
            Copying certs to the local machine directory...
            Copying certs to the remote machine...
            Setting Docker configuration on the remote daemon...
            Checking connection to Docker...
            Docker is up and running!
            To see how to connect Docker to this machine, run: docker-machine env default

      {% comment %}
      This command downloads a lightweight Linux distribution ([boot2docker](https://github.com/boot2docker/boot2docker){: target="_blank" class="_"}) with the Docker daemon installed, and creates and starts a VirtualBox VM with Docker running.
      {% endcomment %}
      このコマンドは、インストールされている Docker デーモンを利用して、軽量 Linux ディストリビューション ([boot2docker](https://github.com/boot2docker/boot2docker){: target="_blank" class="_"}) をダウンロードします。
      そして稼動中の Docker を利用して VirtualBox 仮想マシンを生成し起動します。

{% comment %}
4. List available machines again to see your newly minted machine.
{% endcomment %}
4. 再びマシン一覧を確認すると、新たに生成されたマシンが表示されています。

        $ docker-machine ls
        NAME      ACTIVE   DRIVER       STATE     URL                         SWARM   DOCKER   ERRORS
        default   *        virtualbox   Running   tcp://192.168.99.187:2376           v1.9.1

{% comment %}
5. Get the environment commands for your new VM.
{% endcomment %}
5. 新しい仮想マシン用の、環境変数設定のためのコマンドを取得します。

    {% comment %}
    As noted in the output of the `docker-machine create` command, you need to tell Docker to talk to the new machine. You can do this with the `docker-machine env` command.
    {% endcomment %}
    `docker-machine create`コマンドの出力に示されているように、新たに生成したマシンとのやりとりを行うために、Docker に対する設定が必要です。
    これには`docker-machine env`コマンドを実行します。

        $ docker-machine env default
        export DOCKER_TLS_VERIFY="1"
        export DOCKER_HOST="tcp://172.16.62.130:2376"
        export DOCKER_CERT_PATH="/Users/<yourusername>/.docker/machine/machines/default"
        export DOCKER_MACHINE_NAME="default"
        # Run this command to configure your shell:
        # eval "$(docker-machine env default)"

{% comment %}
6. Connect your shell to the new machine.
{% endcomment %}
6. シェルから新しいマシンに接続します。

        $ eval "$(docker-machine env default)"

      {% comment %}
      **Note**: If you are using `fish`, or a Windows shell such as
      Powershell/`cmd.exe`, the above method does not work as described.
      Instead, see [the `env` command's documentation](reference/env.md){: target="_blank" class="_"}
      to learn how to set the environment variables for your shell.
      {% endcomment %}
      **メモ**: `fish`や Powershell、`cmd.exe`のような Windows シェルでは、先ほどのコマンドは実行できません。
      利用するシェルにおいて環境変数を有効にする方法は、[the `env` command's documentation](reference/env.md){: target="_blank" class="_"} を参照してください。

    {% comment %}
    This sets environment variables for the current shell that the Docker
    client will read which specify the TLS settings. You need to do this
    each time you open a new shell or restart your machine. (See also, how to
      [unset environment variables in the current shell](get-started.md#unset-environment-variables-in-the-current-shell).)
    {% endcomment %}
    カレントなシェル上でこの環境変数設定を使えば、クライアントはその TLS 設定を読み込みます。
    新しいシェルの起動時やマシン再起動時には、再度指定する必要があります。
    （[カレントシェルの環境変数クリア](get-started.md#unset-environment-variables-in-the-current-shell) も参照してください。）

    {% comment %}
    You can now run Docker commands on this host.
    {% endcomment %}
    こうしてホスト上において Docker コマンドを実行します。

{% comment %}
## Run containers and experiment with Machine commands
{% endcomment %}
{: #run-containers-and-experiment-with-machine-commands }
## Docker Machine のコマンドを使ったコンテナー実行

{% comment %}
Run a container with `docker run` to verify your set up.
{% endcomment %}
`docker run`コマンドによりコンテナーを起動することで、セットアップができていることを確認します。

{% comment %}
1. Use `docker run` to download and run `busybox` with a simple 'echo' command.
{% endcomment %}
1. `docker run`コマンドを実行して`busybox`イメージをダウンロードし、単純に「echo」コマンドを実行する`busybox`を起動します。

        $ docker run busybox echo hello world
        Unable to find image 'busybox' locally
        Pulling repository busybox
        e72ac664f4f0: Download complete
        511136ea3c5a: Download complete
        df7546f9f060: Download complete
        e433a6c5b276: Download complete
        hello world

{% comment %}
2. Get the host IP address.
{% endcomment %}
2. ホストの IP アドレスを確認します。

    {% comment %}
    Any exposed ports are available on the Docker host’s IP address, which you can get using the `docker-machine ip` command:
    {% endcomment %}
    Docker ホスト上の IP アドレスに対して、公開されるポートはすべて利用できます。
    これは`docker-machine ip`コマンドを実行して確認できます。

        $ docker-machine ip default
        192.168.99.100

{% comment %}
3. Run a [Nginx](https://www.nginx.com/){: target="_blank" class="_"} webserver in a container with the following command:
{% endcomment %}
3. コンテナー内においてウェブサーバー [Nginx](https://www.nginx.com/){: target="_blank" class="_"} を起動するために、以下のコマンドを実行します。

        $ docker run -d -p 8000:80 nginx

    {% comment %}
    When the image is finished pulling, you can hit the server at port 8000 on the IP address given to you by `docker-machine ip`. For instance:
    {% endcomment %}
    イメージのプルが完了したら、サーバーに対してポート 8000 によりアクセスします。
    IP アドレスは`docker-machine ip`で得られるものを利用します。

            $ curl $(docker-machine ip default):8000
            <!DOCTYPE html>
            <html>
            <head>
            <title>Welcome to nginx!</title>
            <style>
                body {
                    width: 35em;
                    margin: 0 auto;
                    font-family: Tahoma, Verdana, Arial, sans-serif;
                }
            </style>
            </head>
            <body>
            <h1>Welcome to nginx!</h1>
            <p>If you see this page, the nginx web server is successfully installed and
            working. Further configuration is required.</p>

            <p>For online documentation and support, refer to
            <a href="http://nginx.org/">nginx.org</a>.<br/>
            Commercial support is available at
            <a href="http://nginx.com/">nginx.com</a>.</p>

            <p><em>Thank you for using nginx.</em></p>
            </body>
            </html>

  {% comment %}
  You can create and manage as many local VMs running Docker as your local resources permit; just run `docker-machine create` again. All created machines appear in the output of `docker-machine ls`.
  {% endcomment %}
  ローカルのリソースが許す限り、Docker を起動するローカル VM はいくらでも生成して管理することができます。
  そのときには、再度`docker-machine create`を実行するだけです。
  生成したマシンは、`docker-machine ls`の出力結果にすべて表示されます。

{% comment %}
## Start and stop machines
{% endcomment %}
{: #start-and-stop-machines }
## マシンの起動と停止

{% comment %}
If you are finished using a host for the time being, you can stop it with `docker-machine stop` and later start it again with `docker-machine start`.
{% endcomment %}
ホストの利用が終わってしばらくたったものは、`docker-machine stop`を実行して停止しておきます。
後に起動したくなったら`docker-machine start`を実行します。

        $ docker-machine stop default
        $ docker-machine start default

{% comment %}
## Operate on machines without specifying the name
{% endcomment %}
{: #operate-on-machines-without-specifying-the-name }
## マシン名指定なしの操作

{% comment %}
Some `docker-machine` commands assume that the given operation should be run on a machine named `default` (if it exists) if no machine name is specified.  Because using a local VM named `default` is such a common pattern, this allows you to save some typing on the most frequently used Machine commands.
{% endcomment %}
`docker-machine`コマンドにおいてマシン名を指定しなかった場合、操作の対象を`default`という名のマシン（それが存在している場合に限り）であるものとして処理するものがあります。
ローカル VM は`default`という名前にするのが通常であるため、何度も利用する Docker Machine コマンドにおいては、入力を省くことができます。

{% comment %}
For example:
{% endcomment %}
たとえば以下のとおりです。

          $ docker-machine stop
          Stopping "default"....
          Machine "default" was stopped.

          $ docker-machine start
          Starting "default"...
          (default) Waiting for an IP...
          Machine "default" was started.
          Started machines may have new IP addresses.  You may need to re-run the `docker-machine env` command.

          $ eval $(docker-machine env)

          $ docker-machine ip
            192.168.99.100

{% comment %}
Commands that follow this style are:
{% endcomment %}
このような扱いとしているコマンドは、以下のものです。

        - `docker-machine config`
        - `docker-machine env`
        - `docker-machine inspect`
        - `docker-machine ip`
        - `docker-machine kill`
        - `docker-machine provision`
        - `docker-machine regenerate-certs`
        - `docker-machine restart`
        - `docker-machine ssh`
        - `docker-machine start`
        - `docker-machine status`
        - `docker-machine stop`
        - `docker-machine upgrade`
        - `docker-machine url`

{% comment %}
For machines other than `default`, and commands other than those listed above, you must always specify the name explicitly as an argument.
{% endcomment %}
`default`以外のマシンを扱う場合や、上記以外のコマンドを利用する場合は、コマンドの引数に必ずマシン名を指定する必要があります。

{% comment %}
## Unset environment variables in the current shell
{% endcomment %}
{: #unset-environment-variables-in-the-current-shell }
## カレントシェルにおける環境変数クリア

{% comment %}
You might want to use the current shell to connect to a different Docker Engine.
This would be the case if, for example, you are
[running Docker Desktop for Mac concurrent with Docker Toolbox](../docker-for-mac/docker-toolbox.md)
and want to talk to two different Docker Engines.
In both scenarios, you have the option to switch the environment for the current
shell to talk to different Docker engines.
{% endcomment %}
カレントなシェルから、今までとは違う Docker Engine に接続したい場合があります。
たとえば [Docker Desktop for Mac と Docker Toolbox を同時実行](../docker-for-mac/docker-toolbox.md) している場合であって、2 つの Docker Engine に接続する必要がある場合です。
こういった場合、カレントなシェルが別の Docker Engine に接続するように、環境を切り替えることができます。

{% comment %}
1.  Run `env|grep DOCKER` to check whether DOCKER environment variables are set.
{% endcomment %}
1.  `env|grep DOCKER`を実行して、DOCKER 関連の環境変数 が設定されているかどうかを確認します。

    ```none
    $ env | grep DOCKER
    DOCKER_HOST=tcp://192.168.99.100:2376
    DOCKER_MACHINE_NAME=default
    DOCKER_TLS_VERIFY=1
    DOCKER_CERT_PATH=/Users/<your_username>/.docker/machine/machines/default
    ```

    {% comment %}
    If it returns output (as shown in the example), you can unset the `DOCKER` environment variables.
    {% endcomment %}
    （例に示すように）出力結果が返されたら、この`DOCKER`関連の環境変数をクリアします。

{% comment %}
2.  Use one of two methods to unset DOCKER environment variables in the current shell.
{% endcomment %}
2.  カレントシェルにおいて DOCKER 関連の環境変数をクリアするには、以下のいずれかの方法をとります。

    {% comment %}
    * Run the `unset` command on the following `DOCKER` environment variables.
    {% endcomment %}
    * `unset`コマンドに続けて`DOCKER`関連の環境変数を指定して実行します。

      ```none
      unset DOCKER_TLS_VERIFY
      unset DOCKER_CERT_PATH
      unset DOCKER_MACHINE_NAME
      unset DOCKER_HOST
      ```

    {% comment %}
    * Alternatively, run a shortcut command `docker-machine env -u` to show the command you need to run to unset all DOCKER variables:
    {% endcomment %}
    * もう 1 つの方法は、ショートカットコマンド`docker-machine env -u`を実行して、DOCKER 関連の環境変数をクリアするコマンドを表示させます。

      ```none
      $ docker-machine env -u
      unset DOCKER_TLS_VERIFY
      unset DOCKER_HOST
      unset DOCKER_CERT_PATH
      unset DOCKER_MACHINE_NAME
      # Run this command to configure your shell:
      # eval $(docker-machine env -u)
      ```

      {% comment %}
      Run `eval $(docker-machine env -u)` to unset all DOCKER variables in the current shell.
      {% endcomment %}
      `eval $(docker-machine env -u)`を実行して、カレントなシェル上における DOCKER 関連の環境変数をクリアします。

{% comment %}
3. Now, after running either of the above commands, this command should return no output.
{% endcomment %}
3. そこでもう一度、先ほどのコマンドを実行してみます。
   何も出力されなくなったはずです。

    ```
    $ env | grep DOCKER
    ```

    {% comment %}
    If you are running Docker Desktop for Mac, you can run Docker commands to talk
    to the Docker Engine installed with that app.
    {% endcomment %}
    Docker Desktop for Mac を利用している場合、同時にインストールされる Docker Engine とのやりとりを Docker コマンドにより行います。

    {% comment %}
    Since [Docker Desktop for Windows is incompatible with Toolbox](../docker-for-windows/install.md#what-to-know-before-you-install),
    this scenario isn't applicable because Docker Desktop for Windows uses the Docker
    Engine and Docker Machine that come with it.
    {% endcomment %}
    [Docker Desktop for Windows は Toolbox と非互換](../docker-for-windows/install.md#what-to-know-before-you-install) であることから、ここに示した方法は、その場合には利用できません。
    Docker Desktop for Windows が利用するのは、同時にインストールされる Docker Engine と Docker Machine だからです。

{% comment %}
## Start local machines on startup
{% endcomment %}
{: #start-local-machines-on-startup }
## 起動時におけるローカルマシン起動

{% comment %}
To ensure that the Docker client is automatically configured at the start of
each shell session, you can embed `eval $(docker-machine env default)` in your
shell profiles, by adding it to the `~/.bash_profile` file or the equivalent
configuration file for your shell. However, this fails if a machine called
`default` is not running. You can configure your system to start the `default`
machine automatically. The following example shows how to do this in macOS.
{% endcomment %}
シェルを起動する際に、自動的に Docker クライアントが設定されるようにするには、シェルプロファイルに`eval $(docker-machine env default)`を書き入れます。
具体的には`~/.bash_profile`ファイルか、あるいはこれに相当する設定ファイルに記述します。
ただし`default`というマシンが実行中でない場合に、この記述はエラーとなります。
そこでシステム起動時に`default`マシンが自動的に起動されるように設定します。
以下の例は macOS において、それを実現するものです。


{% comment %}
Create a file called `com.docker.machine.default.plist` under
`~/Library/LaunchAgents` with the following content:
{% endcomment %}
`~/Library/LaunchAgents`の配下に`com.docker.machine.default.plist`というファイルを生成し、内容を以下のようにします。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
    <dict>
        <key>EnvironmentVariables</key>
        <dict>
            <key>PATH</key>
            <string>/usr/bin:/bin:/usr/sbin:/sbin:/usr/local/bin</string>
        </dict>
        <key>Label</key>
        <string>com.docker.machine.default</string>
        <key>ProgramArguments</key>
        <array>
            <string>/usr/local/bin/docker-machine</string>
            <string>start</string>
            <string>default</string>
        </array>
        <key>RunAtLoad</key>
        <true/>
    </dict>
</plist>
```

{% comment %}
You can change the `default` string above to make this `LaunchAgent` start a
different machine.
{% endcomment %}
上の`default`の記述を変更すれば、`LaunchAgent`から起動するマシンを別のものにすることがでｋます。

{% comment %}
## Where to go next
{% endcomment %}
{: #where-to-go-next }
## 次に読むものは

{% comment %}
-   Provision multiple Docker hosts [on your cloud provider](get-started-cloud.md)
-   [Understand Machine concepts](concepts.md)
- [Docker Machine list of reference pages for all supported drivers](drivers/index.md)
- [Docker Machine driver for Oracle VirtualBox](drivers/virtualbox.md)
- [Docker Machine driver for Microsoft Hyper-V](drivers/hyper-v.md)
- [`docker-machine` command line reference](reference/index.md)
{% endcomment %}
- [クラウドプロバイダー](get-started-cloud.md) 上に複数の Docker ホストをプロビジョニングします。
- [Machine の考え方](concepts.md)
- [Docker Machine に対応する全ドライバーのリファレンス一覧](drivers/index.md)
- [Docker Machine ドライバー Oracle VirtualBox 用](drivers/virtualbox.md)
- [Docker Machine ドライバー Microsoft Hyper-V 用](drivers/hyper-v.md)
- [`docker-machine`コマンドラインリファレンス](reference/index.md)
