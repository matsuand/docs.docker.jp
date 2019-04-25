---
description: Get started with Docker Machine and a local VM
keywords: docker, machine, virtualbox, local
title: Docker Machine をローカル VM で始めるには
---

Let's take a look at using `docker-machine` to create, use and manage a
Docker host inside of a local virtual machine.

## 前提条件

With the advent of [Docker Desktop for Mac](/docker-for-mac/index.md) and [Docker Desktop for
Windows](/docker-for-windows/index.md) as replacements for [Docker
Toolbox](/toolbox/overview.md), we recommend that you use these for your primary
Docker workflows. You can use these applications to run Docker natively on your
local system without using Docker Machine at all. (See [Docker Desktop for Mac vs.
Docker Toolbox](/docker-for-mac/docker-toolbox.md) for an explanation on the Mac
side.)

For now, however, if you want to create _multiple_ local machines, you still
need Docker Machine to create and manage machines for multi-node
experimentation. Both Docker Desktop for Mac and Docker Desktop for Windows include the newest
version of Docker Machine, so when you install either of these, you get
`docker-machine`.

The new solutions come with their own native virtualization solutions rather
than Oracle VirtualBox, so keep the following considerations in mind when using
Machine to create local VMs.

* **Docker Desktop for Mac** - You can use `docker-machine create` with the `virtualbox` driver to create additional local machines.

* **Docker Desktop for Windows** - You can use `docker-machine create` with the `hyperv` driver to create additional local machines.

#### Docker Desktop for Windows を使っている場合

Docker Desktop for Windows uses [Microsoft
Hyper-V](https://msdn.microsoft.com/en-us/virtualization/hyperv_on_windows/windows_welcome)
for virtualization, and Hyper-V is not compatible with Oracle VirtualBox.
Therefore, you cannot run the two solutions simultaneously. But you can still
use `docker-machine` to create more local VMs by using the Microsoft Hyper-V
driver.

The prerequisites are:

* Have Docker Desktop for Windows installed, and running (which requires that virtualization and Hyper-V are enabled, as described in [What to know before you install Docker Desktop for Windows](/docker-for-windows/install.md#what-to-know-before-you-install)).

* Set up the Hyper-V driver to use an external virtual network switch See
the [Docker Machine driver for Microsoft Hyper-V](drivers/hyper-v.md) topic,
which includes an [example](/machine/drivers/hyper-v.md#example) of how to do this.

#### Docker Desktop for Mac を使っている場合

Docker Desktop for Mac uses [HyperKit](https://github.com/docker/HyperKit/), a
lightweight macOS virtualization solution built on top of the
[Hypervisor.framework](https://developer.apple.com/reference/hypervisor).

Currently, there is no `docker-machine create` driver for HyperKit, so
use the `virtualbox` driver to create local machines. (See the [Docker Machine
driver for Oracle VirtualBox](drivers/virtualbox.md).) You can run
both HyperKit and Oracle VirtualBox on the same system. To learn more, see
[Docker Desktop for Mac vs. Docker Toolbox](/docker-for-mac/docker-toolbox/).

* [最新版の VirtualBox](https://www.virtualbox.org/wiki/Downloads){: target="_blank" class="_"} を正しくインストールできていることを確認します。（すでに示した ToolBox の一部としてインストールしているか、直接インストールしているかのどちらでも構いません。）

#### Docker Toolbox を使っている場合

Docker Desktop for Mac and Docker Desktop for Windows both require newer versions of their
respective operating systems, so users with older OS versions must use Docker
Toolbox.

* If you are using Docker Toolbox on either Mac or an older version Windows system (without Hyper-V), use the `virtualbox` driver to create a local
machine based on Oracle [VirtualBox](https://www.virtualbox.org/){:
target="_blank" class="_"}.  (See the [Docker Machine driver for Oracle
VirtualBox](drivers/virtualbox.md).)

* If you are using Docker Toolbox on a Windows system that has Hyper-V but cannot run Docker Desktop for Windows (for example Windows 8 Pro), you must use the
`hyperv` driver to create local machines. (See the [Docker Machine driver for
Microsoft Hyper-V](drivers/hyper-v.md).)

* Make sure you have [the latest VirtualBox](https://www.virtualbox.org/wiki/Downloads){: target="_blank" class="_"}
  correctly installed on your system. If you used
  [Toolbox](https://www.docker.com/products/docker-toolbox){: target="_blank" class="_"}
  or [Docker Desktop for Windows](/docker-for-windows/index.md){: target="_blank" class="_"}
  to install Docker Machine, VirtualBox is
  automatically installed.

* If you used the Quickstart Terminal to launch your first machine and set your
  terminal environment to point to it, a default machine was automatically
  created. If so, you can still follow along with these steps, but
  create another machine and name it something other than `default`.

##  Use Machine to run Docker containers

Docker コンテナを実行するには、

* 新しい Docker 仮想マシンを生成します（あるいは既存の仮想マシンを開始します）。
* 環境変数を新しい仮想マシンに切り替えます。
* docker クライアントを使ってコンテナの作成、読み込み、管理を行います。

Docker Machine で作成したマシンは、必要に応じて何度も再利用できます。マシンは VirtualBox 上の仮想マシンと同じ環境であり、どちらでも同じ設定が使われます。

<!-- v17.09
The examples here show how to create and start a machine, run Docker commands, and work with containers.
-->
以下の例で、マシンの作成・起動方法、 Docker コマンドの実行方法、コンテナの使い方を見ていきます。

## マシンの作成

1. コマンドシェルやターミナル画面を開きます。

    以下の例では Bash シェルを扱います。 C シェルのような他のシェルでは、いくつかのコマンドが動作しない可能性がありますので、ご注意ください。

2. `docker-machine ls` を使って利用可能なマシンの一覧を表示します。

    以下の例では、マシンがまだ１台も作成されていないことが分かります。

        $ docker-machine ls
        NAME   ACTIVE   DRIVER   STATE   URL   SWARM   DOCKER   ERRORS

3. マシンを作成します。

<!-- v17.09 ja: 原文相違
    Run the `docker-machine create` command, passing the string virtualbox to the
--driver flag. The final argument is the name of the machine. If this is your first machine, name
it `default`. If you already have a “default” machine,
choose another name for this new machine.
-->
    コマンド ``docker-machine create`` の実行時、 ``--driver`` フラグに ``virtualbox`` の文字列を指定します。そして、最後の引数がマシン名になります。これが初めてのマシンであれば、名前を ``default`` にしましょう。既に「default｣という名前のマシンが存在している場合は、別の新しいマシン名を指定します。

    * If you are using Toolbox on Mac, Toolbox on older Windows systems without Hyper-V, or Docker Desktop for Mac, use `virtualbox` as the driver, as shown in this example. (The Docker Machine VirtualBox driver reference is [here](drivers/virtualbox.md).) (See [prerequisites](get-started.md#prerequisite-information) above to learn more.)

    * On Docker Desktop for Windows systems that support Hyper-V, use the `hyperv` driver as shown in the [Docker Machine Microsoft Hyper-V driver reference](drivers/hyper-v.md) and follow the [example](/machine/drivers/hyper-v.md#example), which shows how to use an external network switch and provides the flags for the full command. (See [prerequisites](get-started.md#prerequisite-information) above to learn more.)

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

      このコマンドは Docker デーモンをインストールする軽量 Linux ディストリビューション ([boot2docker](https://github.com/boot2docker/boot2docker){: target="_blank" class="_"}) をダウンロードし、Docker を動かすための VirtualBox 仮想マシンを作成・起動します。

4. 再び利用可能なマシン一覧表示したら、新しいマシンが出てきます。

        $ docker-machine ls
        NAME      ACTIVE   DRIVER       STATE     URL                         SWARM   DOCKER   ERRORS
        default   *        virtualbox   Running   tcp://192.168.99.187:2376           v1.9.1

5. コマンドの環境変数を新しい仮想マシンに設定します。

    コマンド ``docker-machine create`` を実行しても、そのまま新しいマシンを操作できないので注意が必要です。新しいマシンの操作には ``docker-machine env`` コマンドを使います。

        $ docker-machine env default
        export DOCKER_TLS_VERIFY="1"
        export DOCKER_HOST="tcp://172.16.62.130:2376"
        export DOCKER_CERT_PATH="/Users/<yourusername>/.docker/machine/machines/default"
        export DOCKER_MACHINE_NAME="default"
        # Run this command to configure your shell:
        # eval "$(docker-machine env default)"

6. シェルを新しいマシンに接続します。

        $ eval "$(docker-machine env default)"

      **メモ**: `fish` や Powershell 、あるいは `cmd.exe` のような Windows シェルでは、先ほどのコマンドは実行できません。自分の使っているシェルで環境変数を有効にする方法は、 [the `env` command's documentation](/machine/reference/env.md){: target="_blank" class="_"} をご覧ください。

<!-- v17.06 ja: 原文相違
    This sets environment variables for the current shell that the Docker
    client will read which specify the TLS settings. You need to do this
    each time you open a new shell or restart your machine.
-->
    このシェル上で指定した環境変数を使えば、クライアントは指定された  TLS 設定を読み込みます。新しいシェルの起動時やマシン再起動時には、再度指定する必要があります。

    あとはホスト上で Docker コマンドを実行できます。

## Machine コマンドを使ってコンテナを実行

セットアップが完了したことを確認するため、`docker run` コマンドを使ってコンテナを起動しましょう。

1. `docker run` コマンドを使い、 `busybox` イメージをダウンロードし、 簡単な 'echo' コマンドを実行します。

        $ docker run busybox echo hello world
        Unable to find image 'busybox' locally
        Pulling repository busybox
        e72ac664f4f0: Download complete
        511136ea3c5a: Download complete
        df7546f9f060: Download complete
        e433a6c5b276: Download complete
        hello world

2. ホストの IP アドレスを確認します。

    Docker ホスト上でポート番号が利用可能な IP アドレスの確認は、 ``docker-machine ip`` コマンドを使います。

        $ docker-machine ip default
        192.168.99.100

3. コンテナで[Nginx](https://www.nginx.com/){: target="_blank" class="_"} を実行するため、次のコマンドを実行します。

        $ docker run -d -p 8000:80 nginx

    イメージの取得が完了したら、 ``docker-machine ip`` で確認した IP アドレス上のポート 8000 でサーバにアクセスできます。実行例：

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

  あとは、好きなだけ実行したいローカル仮想マシンを作成・管理できます。そのためには ``docker-machine create`` を実行するだけです。作成されたマシン全ての情報を確認するには ``docker-machine ls`` を使います。

## マシンの起動と停止

ホストを使い終わり、しばらく使わないのであれば、 `docker-machine stop` を実行して停止できます。あとで起動したい場合は `docker-machine start`  を実行します。

        $ docker-machine stop default
        $ docker-machine start default

## マシンの名前を指定せずに操作するには

いくつかの `docker-machine` コマンドは、マシン名を明示しれなければ `default` という名称のマシン（が存在している場合）に対して処理を行います。そのため、 `default` ローカル仮想マシンは一般的なパターンとして、頻繁に利用できるでしょう。

実行例：

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

コマンドは以下の形式でも利用可能です。

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

`default` 以外のマシンでは、常に特定のマシン名をコマンドの引数として明示する必要があります。

## Unset environment variables in the current shell

You might want to use the current shell to connect to a different Docker Engine.
This would be the case if, for example, you are [running Docker Desktop for Mac
concurrent with Docker Toolbox](/docker-for-mac/docker-toolbox.md) and want to
talk to two different Docker Engines.
In both scenarios, you have the option to switch the environment for the current
shell to talk to different Docker engines.

1.  Run `env|grep DOCKER` to check whether DOCKER environment variables are set.

    ```none
    $ env | grep DOCKER
    DOCKER_HOST=tcp://192.168.99.100:2376
    DOCKER_MACHINE_NAME=default
    DOCKER_TLS_VERIFY=1
    DOCKER_CERT_PATH=/Users/<your_username>/.docker/machine/machines/default
    ```

    If it returns output (as shown in the example), you can unset the `DOCKER` environment variables.

2.  Use one of two methods to unset DOCKER environment variables in the current shell.

    * Run the `unset` command on the following `DOCKER` environment variables.

      ```none
      unset DOCKER_TLS_VERIFY
      unset DOCKER_CERT_PATH
      unset DOCKER_MACHINE_NAME
      unset DOCKER_HOST
      ```

    * Alternatively, run a shortcut command `docker-machine env -u` to show the command you need to run to unset all DOCKER variables:

      ```none
      $ docker-machine env -u
      unset DOCKER_TLS_VERIFY
      unset DOCKER_HOST
      unset DOCKER_CERT_PATH
      unset DOCKER_MACHINE_NAME
      # Run this command to configure your shell:
      # eval $(docker-machine env -u)
      ```

      Run `eval $(docker-machine env -u)` to unset all DOCKER variables in the current shell.

3. Now, after running either of the above commands, this command should return no output.

    ```
    $ env | grep DOCKER
    ```

    If you are running Docker Desktop for Mac, you can run Docker commands to talk
    to the Docker Engine installed with that app.

    Since [Docker Desktop for Windows is incompatible with
    Toolbox](/docker-for-windows/install.md#what-to-know-before-you-install),
    this scenario isn't applicable because Docker Desktop for Windows uses the Docker
    Engine and Docker Machine that come with it.

## 起動時にローカル・マシンの自動起動

To ensure that the Docker client is automatically configured at the start of
each shell session, you can embed `eval $(docker-machine env default)` in your
shell profiles, by adding it to the `~/.bash_profile` file or the equivalent
configuration file for your shell. However, this fails if a machine called
`default` is not running. You can configure your system to start the `default`
machine automatically. The following example shows how to do this in macOS.


<!-- v17.09 ja: 原文相違
Create a file called `com.docker.machine.default.plist` under
`~/Library/LaunchAgents` with the following content:
-->
`~/Library/LaunchAgents` 以下に `com.docker.machine.default.plist` ファイルを作成します。内容は次の通りです。

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

この中にある ``LaunchAgent`` の ``default``  を書き換えれば、任意のマシン（群）を起動できます。

## 次はどこへ行きますか

-   Provision multiple Docker hosts [on your cloud provider](get-started-cloud.md)
-   [Understand Machine concepts](concepts.md)
- [Docker Machine list of reference pages for all supported drivers](/machine/drivers/index.md)
- [Docker Machine ドライバー Oracle VirtualBox 用](drivers/virtualbox.md)
- [Docker Machine ドライバー Microsoft Hyper-V 用](drivers/hyper-v.md)
- [`docker-machine` コマンドラインリファレンス](reference/index.md)
