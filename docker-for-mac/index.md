---
description: はじめよう。
keywords: mac, tutorial, run, docker, local, machine
redirect_from:
- /mackit/
- /mackit/getting-started/
- /mac/
- /mac/started/
- /docker-for-mac/started/
- /installation/mac/
- /engine/installation/mac/
- /docker-for-mac/index/
- /docker-for-mac/osx/
title: Docker Desktop for Mac をはじめよう
toc_min: 1
toc_max: 2
---

{% comment %}
Welcome to Docker Desktop!
{% endcomment %}
Docker Desktop へようこそ！

{% comment %}
The _Docker Desktop for Mac_ section contains information about the Docker Desktop Community Stable release. For information about features available in Edge releases, see the [Edge release notes](edge-release-notes/). For information about Docker Desktop Enterprise (DDE) releases, see [Docker Desktop Enterprise](/desktop/enterprise/).
{% endcomment %}
**Docker Desktop for Mac** に関するこのページでは、Docker Desktop Community 安定版リリースについて説明します。
Edge 版リリースにおいて利用可能な機能に関しては [Edge リリースノート](edge-release-notes/) を参照してください。
また Docker Desktop Enterprise (DDE) リリースについては [Docker Desktop Enterprise](/desktop/enterprise/) を参照してください。

{% comment %}
Docker is a full development platform to build, run, and share containerized applications. Docker Desktop is the best way to get started with Docker on Mac.
{% endcomment %}
Docker には、コンテナー化アプリケーションをビルド、実行し、共有するための開発プラットフォームがすべて含まれています。
Mac 上において Docker をはじめるには Docker Desktop が最適です。

{% comment %}
See [Install Docker Desktop](install.md){: target="_blank" class="_"} for download information, system requirements, and installation instructions.
{% endcomment %}
[Docker Desktop のインストール](install.md){: target="_blank" class="_"} を参照して、ダウンロード情報、システム前提条件、インストール手順を確認してください。

{% comment %}
## Check versions
{% endcomment %}
{: #check-versions }
## バージョンの確認

{% comment %}
Ensure your versions of `docker` and `docker-compose` are
up-to-date and compatible with `Docker.app`. Your output may differ if you are
running different versions.
{% endcomment %}
利用している `docker`、`docker-compose` のバージョンが最新であること、そして `Docker.app` との互換性があることを確認します。
以下に示すバージョン出力は、利用しているものにより異なるかもしれません。

```shell
$ docker --version
Docker version {{ site.docker_ce_version }}, build c97c6d6
```

{% comment %}
## Explore the application
{% endcomment %}
{: #explore-the-application }
## アプリケーションの確認

{% comment %}
1.  Open a command-line terminal and test that your installation works by
    running the simple Docker image,
    [hello-world](https://hub.docker.com/_/hello-world/){: target="_blank"
    class="_"}:
{% endcomment %}
1.  コマンドラインターミナルを開いて、単純な Docker イメージ [hello-world](https://hub.docker.com/_/hello-world/){: target="_blank" class="_"} が動作するかどうかを確認します。

    ```shell
    $ docker run hello-world

    {% comment %}
    Unable to find image 'hello-world:latest' locally
    latest: Pulling from library/hello-world
    ca4f61b1923c: Pull complete
    Digest: sha256:ca0eeb6fb05351dfc8759c20733c91def84cb8007aa89a5bf606bc8b315b9fc7
    Status: Downloaded newer image for hello-world:latest
    {% endcomment %}
    Unable to find image 'hello-world:latest' locally
    latest: Pulling from library/hello-world
    ca4f61b1923c: Pull complete
    Digest: sha256:ca0eeb6fb05351dfc8759c20733c91def84cb8007aa89a5bf606bc8b315b9fc7
    Status: Downloaded newer image for hello-world:latest

    Hello from Docker!
    This message shows that your installation appears to be working correctly.
    ...
    ```

{% comment %}
2.  Start a Dockerized web server. Like the `hello-world` image above, if the
    image is not found locally, Docker pulls it from Docker Hub.
{% endcomment %}
2.  Docker 化されたウェブサーバーを起動します。
    上の `hello-world` イメージの場合と同じように、ローカルにそのイメージが見つからないときは Docker Hub からプルされます。

    ```bash
    $ docker run --detach --publish=80:80 --name=webserver nginx
    ```

{% comment %}
3.  In a web browser, go to `http://localhost/` to view the nginx homepage.
    Because we specified the default HTTP port, it isn't necessary to append
    `:80` at the end of the URL.
{% endcomment %}
3.  ウェブブラウザーから `http://localhost/` にアクセスして nginx ホームページを開きます。
    デフォルトの HTTP ポートを指定しているので、URL の最後に `:80` をつける必要はありません。

    {% comment %}
    ![nginx home page](images/hello-world-nginx.png){:width="500px"}
    {% endcomment %}
    ![nginx ホームページ](images/hello-world-nginx.png){:width="500px"}

    {% comment %}
    > Early beta releases used `docker` as the hostname to build the URL. Now,
    > ports are exposed on the private IP addresses of the VM and forwarded to
    > `localhost` with no other host name set.
    {% endcomment %}
    > 初期のベータリリースにおいては、URL を構成するためにホスト名に `docker` を用いていました。
    > 現在は VM のプライベート IP アドレスを用いていて、ホスト名の設定をせずに `localhost` に対してフォワードを行います。

{% comment %}
4.  View the details on the container while your web server is running (with
    `docker container ls` or `docker ps`):
{% endcomment %}
4.  ウェブサーバーを起動している状態で、コンテナーの詳細を確認します。
    （`docker container ls` や `docker ps` を利用します。）

    ```none
    $ docker container ls
    CONTAINER ID   IMAGE   COMMAND                  CREATED              STATUS              PORTS                         NAMES
    56f433965490   nginx   "nginx -g 'daemon off"   About a minute ago   Up About a minute   0.0.0.0:80->80/tcp, 443/tcp   webserver
    ```

{% comment %}
5.  Stop and remove containers and images with the following commands. Use the
    "all" flag (`--all` or `-a`) to view stopped containers.
{% endcomment %}
5.  以下のコマンドにより、コンテナーとイメージの停止と削除を行います。
    停止しているコンテナーを確認するには「all」フラグ（`--all` または `-a`）を指定します。

    ```shell
    $ docker container ls
    $ docker container stop webserver
    $ docker container ls -a
    $ docker container rm webserver
    $ docker image ls
    $ docker image rm nginx
    ```

{% comment %}
## Preferences
{% endcomment %}
{: #preferences }
## Preferences メニューの実行

{% comment %}
Choose the Docker menu ![whale menu](images/whale-x.png){: .inline} > **Preferences** from the
menu bar and configure the runtime options described below.
{% endcomment %}
メニューバー上の Docker メニュー ![クジラメニュー](images/whale-x.png){: .inline} から **Preferences** を選び、以降に示す実行時オプションを設定します。

{% comment %}
![Docker context menu](images/menu/prefs.png){:width="250px"}
{% endcomment %}
![Docker コンテキストメニュー](images/menu/prefs.png){:width="250px"}

{% comment %}
### General
{% endcomment %}
{: #general }
### General タブ

{% comment %}
![Preferences](images/menu/prefs-general.png){:width="750px"}
{% endcomment %}
![Preferences](images/menu/prefs-general.png){:width="750px"}

{% comment %}
On the **General** tab, you can configure when to start and update Docker:
{% endcomment %}
**General** タブにおいて Docker の起動や更新をいつ行うのかを設定します。

{% comment %}
- **Start Docker Desktop when you log in**: Automatically starts Docker Desktop when you open your session.
{% endcomment %}
- **Start Docker Desktop when you log in**（ログイン時に Docker Desktop を起動） セッションを開始したときに自動的に Docker Desktop を起動します。

{% comment %}
- **Automatically check for updates**: By default, Docker Desktop automatically checks for updates and notifies you when an update is available. You can manually check for updates anytime by choosing **Check for Updates** from the main Docker menu.
{% endcomment %}
- **Automatically check for updates**（アップデートの自動更新） デフォルトでは、Docker Desktop の更新チェックは自動的に行われ、利用可能な更新がある場合は通知されます。
  Docker のメインメニューにある **Check for Updates** を実行すれば、いつでも手動による更新ができます。

{% comment %}
- **Include VM in Time Machine backups**: Select this option to back up the Docker Desktop virtual machine. This option is disabled by default.
{% endcomment %}
- **Include VM in Time Machine backups**（Time Machine バックアップに VM を含める） Docker Desktop 仮想マシンのバックアップに関するオプションを設定します。
  このオプションはデフォルトでは無効になっています。

{% comment %}
- **Securely store Docker logins in macOS keychain**: Docker Desktop stores your Docker login credentials in macOS keychain by default.
{% endcomment %}
- **Securely store Docker logins in macOS keychain**（macOS キーチェーンに Docker ログイン情報を安全に保存） Docker Desktop はデフォルトで、macOS キーチェーン内に Docker ログイン情報を保存します。

{% comment %}
- **Send usage statistics**: Docker Desktop sends diagnostics, crash reports, and usage data. This information helps Docker improve and troubleshoot the application. Clear the check box to opt out.
{% endcomment %}
- **Send usage statistics**（利用統計の送信） Docker Desktop は、診断情報、クラッシュレポート、利用状況の各情報を送信します。
  この情報を通じて Docker は改良を行い、アプリケーションのトラブルシューティングに役立てています。
  チェックボックスをオフにすれば、データ送信を行いません。

  **Switch to the Edge version**（最新版への切り替え）をクリックすると、Docker Desktop 最新版について確認できます。

{% comment %}
### Resources
{% endcomment %}
{: #resources }
### Resources タブ

{% comment %}
The **Resources** tab allows you to configure CPU, memory, disk, proxies, network, and other resources.
{% endcomment %}
**Resources** タブは、CPU、メモリ、ディスク、プロキシー、ネットワークといったリソースを設定します。

{% comment %}
#### Advanced
{% endcomment %}
{: #advanced }
#### Advanced タブ

{% comment %}
{% endcomment %}
Advanced タブでは、Docker におけるリソースの利用制限を設定します。

{% comment %}
![Advanced Preference
settings-advanced](images/menu/prefs-advanced.png){:width="750px"}
{% endcomment %}
![Advanced Preference
settings-advanced](images/menu/prefs-advanced.png){:width="750px"}

{% comment %}
Advanced settings are:
{% endcomment %}
この Advanced 設定には以下のものがあります。

{% comment %}
**CPUs**: By default, Docker Desktop is set to use half the number of processors
available on the host machine. To increase processing power, set this to a
higher number; to decrease, lower the number.
{% endcomment %}
**CPU** デフォルトにおいて Docker Desktop は、ホストマシン上で利用可能なプロセッサー数の半分を利用するものとして設定されています。
プロセッサー性能を向上させるには、この設定値を大きくします。
逆に抑止するには設定値を小さくします。

{% comment %}
**Memory**: By default, Docker Desktop is set to use `2` GB runtime memory,
allocated from the total available memory on your Mac. To increase the RAM, set this to a higher number. To decrease it, lower the number.
{% endcomment %}
**メモリ**  デフォルトにおいて Docker Desktop は、実行時メモリとして `2` GB を利用するものとして設定されています。
この値は Mac 上において利用可能な全メモリ容量の中から割り当てられます。
RAM 容量を増やすには、この設定値を大きくします。
逆に減らすには、この設定値を小さくします。

{% comment %}
**Swap**: Configure swap file size as needed. The default is 1 GB.
{% endcomment %}
**スワップ**: 必要に応じてスワップファイルサイズを設定します。
デフォルトは 1 GB です。

{% comment %}
**Disk image size**: Specify the size of the disk image.
{% endcomment %}
**ディスクイメージサイズ**: ディスクイメージのサイズを指定します。

{% comment %}
**Disk image location**: Specify the location of the Linux volume where containers and images are stored.
{% endcomment %}
**ディスクイメージの保存場所**: コンテナーやイメージが保存される Linux ボリュームの場所を指定します。

{% comment %}
You can also move the disk image to a different location. If you attempt to move a disk image to a location that already has one, you get a prompt asking if you want to use the existing image or replace it.
{% endcomment %}
ディスクイメージの保存場所は、別のところにすることができます。
移動させようとした保存場所に、すでにディスクイメージが存在していた場合は、プロンプトが表示され、既存のイメージを利用するか、イメージを置き換えるかが問われます。

{% comment %}
#### File sharing
{% endcomment %}
{: #file-sharing }
#### File sharing タブ

{% comment %}
Use File sharing to allow local directories on the Mac to be shared with Linux containers.
This is especially useful for
editing source code in an IDE on the host while running and testing the code in a container.
By default the `/Users`, `/Volume`, `/private`, `/tmp` and `/var/folders` directory are shared. If your project is outside this directory then it must be added
to the list. Otherwise you may get `Mounts denied` or `cannot start service` errors at runtime.
{% endcomment %}
File sharing（ファイル共有）を利用すると、Mac 内のローカルディレクトリを Linux コンテナー間で共有できるようになります。
たとえばホストにある IDE 環境上でソースコードを編集し、コードの実行やテストはコンテナー内で行うような場合に、大変便利なものです。
デフォルトにおいて `/Users`、`/Volume`、`/private`、`/tmp`、`/var/folders` というディレクトリが共有されています。
開発プロジェクトが上記以外のディレクトリにある場合、その一覧にディレクトリを追加する必要があります。
これを行っていないと、実行時エラーとして `Mounts denied`（マウントが拒否されました）や `cannot start service`（サービスを起動できません）が発生します。

{% comment %}
File share settings are:
{% endcomment %}
File share の設定には以下のものがあります。

{% comment %}
- **Add a Directory**: Click `+` and navigate to the directory you want to add.
{% endcomment %}
- **Add a Directory**（ディレクトリの追加）: `+` をクリックして、追加したいディレクトリを指定します。

{% comment %}
- **Apply & Restart** makes the directory available to containers using Docker's
  bind mount (`-v`) feature.
{% endcomment %}
- **Apply & Restart**（適用および再起動）:  Docker のバインドマウント（`-v`）機能を利用して、コンテナー間でのディレクトリ共有を可能にします。

  {% comment %}
  There are some limitations on the directories that can be shared:
  {% endcomment %}
  共有できるディレクトリには制約があります。

  {% comment %}
  - The directory must not exist inside of Docker.
  {% endcomment %}
  - ディレクトリは Docker 内に存在していないことが必要です。

{% comment %}
For more information, see:
{% endcomment %}
より詳しくは以下を参照してください。

{% comment %}
- [Namespaces](osxfs.md#namespaces){: target="_blank" class="_"} in the topic on
  [osxfs file system sharing](osxfs.md).
- [Volume mounting requires file sharing for any project directories outside of `/Users`](troubleshoot.md#volume-mounting-requires-file-sharing-for-any-project-directories-outside-of-users).)
{% endcomment %}
- [osxfs ファイルシステム共有](osxfs.md) のトピックである [名前空間](osxfs.md#namespaces){: target="_blank" class="_"}
- [Volume mounting requires file sharing for any project directories outside of `/Users`](troubleshoot.md#volume-mounting-requires-file-sharing-for-any-project-directories-outside-of-users).)

{% comment %}
#### Proxies
{% endcomment %}
{: #proxies }
#### Proxies タブ

{% comment %}
Docker Desktop detects HTTP/HTTPS Proxy Settings from macOS and automatically
propagates these to Docker. For example, if you set your
proxy settings to `http://proxy.example.com`, Docker uses this proxy when
pulling containers.
{% endcomment %}
Docker Desktop detects HTTP/HTTPS Proxy Settings from macOS and automatically
propagates these to Docker. For example, if you set your
proxy settings to `http://proxy.example.com`, Docker uses this proxy when
pulling containers.

{% comment %}
Your proxy settings, however, will not be propagated into the containers you start.
If you wish to set the proxy settings for your containers, you need to define
environment variables for them, just like you would do on Linux, for example:
{% endcomment %}
Your proxy settings, however, will not be propagated into the containers you start.
If you wish to set the proxy settings for your containers, you need to define
environment variables for them, just like you would do on Linux, for example:

```
$ docker run -e HTTP_PROXY=http://proxy.example.com:3128 alpine env

PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
HOSTNAME=b7edf988b2b5
TERM=xterm
HOME=/root
HTTP_PROXY=http://proxy.example.com:3128
```

{% comment %}
{% endcomment %}
For more information on setting environment variables for running containers,
see [Set environment variables](/engine/reference/commandline/run/#set-environment-variables--e---env---env-file).

{% comment %}
{% endcomment %}
#### Network

{% comment %}
{% endcomment %}
You can configure Docker Desktop networking to work on a virtual private network (VPN). Specify a network address translation (NAT) prefix and subnet mask to enable Internet connectivity.

### Docker Engine

{% comment %}
{% endcomment %}
The Docker Engine page allows you to configure the Docker daemon to determine how your containers run.

{% comment %}
{% endcomment %}
Type a JSON configuration file in the box to configure the daemon settings. For a full list of options, see the Docker Engine
[dockerd commandline reference](/engine/reference/commandline/dockerd/){:target="_blank"
class="_"}.

{% comment %}
{% endcomment %}
Click **Apply & Restart** to save your settings and restart Docker Desktop.

{% comment %}
{% endcomment %}
### Command Line

{% comment %}
{% endcomment %}
On the Command Line page, you can specify whether or not to enable experimental features.

{% include experimental.md %}

{% comment %}
{% endcomment %}
On both Docker Desktop Edge and Stable releases, you can toggle the experimental features on and off. If you toggle the experimental features off, Docker Desktop uses the current generally available release of Docker Engine.

{% comment %}
{% endcomment %}
You can see whether you are running experimental mode at the command line. If
`Experimental` is `true`, then Docker is running in experimental mode, as shown
here. (If `false`, Experimental mode is off.)

```bash
> docker version

Client: Docker Engine - Community
 Version:           19.03.1
 API version:       1.40
 Go version:        go1.12.5
 Git commit:        74b1e89
 Built:             Thu Jul 25 21:18:17 2019
 OS/Arch:           darwin/amd64
 Experimental:      true

Server: Docker Engine - Community
 Engine:
  Version:          19.03.1
  API version:      1.40 (minimum version 1.12)
  Go version:       go1.12.5
  Git commit:       74b1e89
  Built:            Thu Jul 25 21:17:52 2019
  OS/Arch:          linux/amd64
  Experimental:     true
 containerd:
  Version:          v1.2.6
  GitCommit:        894b81a4b802e4eb2a91d1ce216b8817763c29fb
 runc:
  Version:          1.0.0-rc8
  GitCommit:        425e105d5a03fabd737a126ad93d62a9eeede87f
 docker-init:
  Version:          0.18.0
  GitCommit:        fec3683
```

### Kubernetes

{% comment %}
{% endcomment %}
Docker Desktop includes a standalone Kubernetes server that runs on your Mac, so
that you can test deploying your Docker workloads on Kubernetes.

{% comment %}
{% endcomment %}
The Kubernetes client command, `kubectl`, is included and configured to connect
to the local Kubernetes server. If you have `kubectl` already installed and
pointing to some other environment, such as `minikube` or a GKE cluster, be sure
to change context so that `kubectl` is pointing to `docker-desktop`:

```bash
$ kubectl config get-contexts
$ kubectl config use-context docker-desktop
```

{% comment %}
{% endcomment %}
If you installed `kubectl` with Homebrew, or by some other method, and
experience conflicts, remove `/usr/local/bin/kubectl`.

{% comment %}
{% endcomment %}
- To enable Kubernetes support and install a standalone instance of Kubernetes
  running as a Docker container, select **Enable Kubernetes**. To set Kubernetes as the
  [default orchestrator](kubernetes.md#override-the-default-orchestrator), select **Deploy Docker Stacks to Kubernetes by default**.

   {% comment %}
   {% endcomment %}
   Click **Apply & Restart** to save the settings. This instantiates images required to run the Kubernetes server as containers, and installs the
  `/usr/local/bin/kubectl` command on your Mac.

  {% comment %}
  {% endcomment %}
  ![Enable Kubernetes](images/kubernetes/kube.png){:width="750px"}

  {% comment %}
  {% endcomment %}
  When Kubernetes is enabled and running, an additional status bar item displays
  at the bottom right of the Docker Desktop Settings dialog.

  {% comment %}
  {% endcomment %}
  The status of Kubernetes shows in the Docker menu and the context points to
  `docker-desktop`.

  {% comment %}
  {% endcomment %}
  ![Docker Menu with Kubernetes](images/kubernetes/kube-context.png){: .with-border
  width="400px"}

{% comment %}
{% endcomment %}
- By default, Kubernetes containers are hidden from commands like `docker
  service ls`, because managing them manually is not supported. To make them
  visible, select **Show system containers (advanced)** and click **Apply and
  Restart**. Most users do not need this option.

{% comment %}
{% endcomment %}
- To disable Kubernetes support at any time, clear the **Enable Kubernetes** check box. The
  Kubernetes containers are stopped and removed, and the
  `/usr/local/bin/kubectl` command is removed.

  {% comment %}
  {% endcomment %}
  For more about using the Kubernetes integration with Docker Desktop, see
  [Deploy on Kubernetes](kubernetes.md){:target="_blank" class="_"}.

### Reset

{% comment %}
{% endcomment %}
> Reset and Restart options
>
> On Docker Desktop Mac, the **Restart Docker Desktop**, **Reset to factory defaults**, and other reset options are available from the **Troubleshoot** menu.

{% comment %}
{% endcomment %}
For information about the reset options, see [Logs and Troubleshooting](troubleshoot.md).

{% comment %}
{% endcomment %}
## Dashboard

{% comment %}
{% endcomment %}
The Docker Desktop Dashboard enables you to interact with containers and applications and manage the lifecycle of your applications directly from your machine. The Dashboard UI shows all running, stopped, and started containers with their state. It provides an intuitive interface to perform common actions to inspect and manage containers and existing Docker Compose applications. For more information, see [Docker Desktop Dashboard](../desktop/dashboard.md).

{% comment %}
{% endcomment %}
## Add TLS certificates

{% comment %}
{% endcomment %}
You can add trusted Certificate Authorities (CAs) (used to verify registry
server certificates) and client certificates (used to authenticate to
registries) to your Docker daemon.

{% comment %}
{% endcomment %}
### Add custom CA certificates (server side)

{% comment %}
{% endcomment %}
All trusted CAs (root or intermediate) are supported. Docker Desktop creates a
certificate bundle of all user-trusted CAs based on the Mac Keychain, and
appends it to Moby trusted certificates. So if an enterprise SSL certificate is
trusted by the user on the host, it is trusted by Docker Desktop.

{% comment %}
{% endcomment %}
To manually add a custom, self-signed certificate, start by adding the
certificate to the macOS keychain, which is picked up by Docker Desktop. Here is
an example:

```bash
$ sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain ca.crt
```

{% comment %}
{% endcomment %}
Or, if you prefer to add the certificate to your own local keychain only (rather
than for all users), run this command instead:

```
$ security add-trusted-cert -d -r trustRoot -k ~/Library/Keychains/login.keychain ca.crt
```

{% comment %}
{% endcomment %}
See also, [Directory structures for
certificates](#directory-structures-for-certificates).

{% comment %}
{% endcomment %}
> **Note**: You need to restart Docker Desktop after making any changes to the
> keychain or to the `~/.docker/certs.d` directory in order for the changes to
> take effect.

{% comment %}
{% endcomment %}
For a complete explanation of how to do this, see the blog post [Adding
Self-signed Registry Certs to Docker & Docker Desktop for
Mac](http://container-solutions.com/adding-self-signed-registry-certs-docker-mac/){:target="_blank"
class="_"}.

{% comment %}
{% endcomment %}
### Add client certificates

{% comment %}
{% endcomment %}
You can put your client certificates in
`~/.docker/certs.d/<MyRegistry>:<Port>/client.cert` and
`~/.docker/certs.d/<MyRegistry>:<Port>/client.key`.

{% comment %}
{% endcomment %}
When the Docker Desktop application starts, it copies the `~/.docker/certs.d`
folder on your Mac to the `/etc/docker/certs.d` directory on Moby (the Docker
Desktop `xhyve` virtual machine).

{% comment %}
{% endcomment %}
> * You need to restart Docker Desktop after making any changes to the keychain
>   or to the `~/.docker/certs.d` directory in order for the changes to take
>   effect.
>
> * The registry cannot be listed as an _insecure registry_ (see [Docker
>   Engine](#docker-engine). Docker Desktop ignores certificates listed
>   under insecure registries, and does not send client certificates. Commands
>   like `docker run` that attempt to pull from the registry produce error
>   messages on the command line, as well as on the registry.

{% comment %}
{% endcomment %}
### Directory structures for certificates

{% comment %}
{% endcomment %}
If you have this directory structure, you do not need to manually add the CA
certificate to your Mac OS system login:

```
/Users/<user>/.docker/certs.d/
└── <MyRegistry>:<Port>
   ├── ca.crt
   ├── client.cert
   └── client.key
```

{% comment %}
{% endcomment %}
The following further illustrates and explains a configuration with custom
certificates:

```
/etc/docker/certs.d/        <-- Certificate directory
└── localhost:5000          <-- Hostname:port
   ├── client.cert          <-- Client certificate
   ├── client.key           <-- Client key
   └── ca.crt               <-- Certificate authority that signed
                                the registry certificate
```

{% comment %}
{% endcomment %}
You can also have this directory structure, as long as the CA certificate is
also in your keychain.

```
/Users/<user>/.docker/certs.d/
└── <MyRegistry>:<Port>
    ├── client.cert
    └── client.key
```

{% comment %}
{% endcomment %}
To learn more about how to install a CA root certificate for the registry and
how to set the client TLS certificate for verification, see
[Verify repository client with certificates](../engine/security/certificates.md)
in the Docker Engine topics.

{% comment %}
{% endcomment %}
## Install shell completion

{% comment %}
{% endcomment %}
Docker Desktop comes with scripts to enable completion for the `docker` and `docker-compose` commands. The completion scripts may be
found inside `Docker.app`, in the `Contents/Resources/etc/` directory and can be
installed both in Bash and Zsh.

### Bash

{% comment %}
{% endcomment %}
Bash has [built-in support for
completion](https://www.debian-administration.org/article/316/An_introduction_to_bash_completion_part_1){:target="_blank"
class="_"} To activate completion for Docker commands, these files need to be
copied or symlinked to your `bash_completion.d/` directory. For example, if you
installed bash via [Homebrew](http://brew.sh/):

```bash
etc=/Applications/Docker.app/Contents/Resources/etc
ln -s $etc/docker.bash-completion $(brew --prefix)/etc/bash_completion.d/docker
ln -s $etc/docker-compose.bash-completion $(brew --prefix)/etc/bash_completion.d/docker-compose
```

{% comment %}
{% endcomment %}
Add the following to your `~/.bash_profile`:

```shell
[ -f /usr/local/etc/bash_completion ] && . /usr/local/etc/bash_completion
```

{% comment %}
{% endcomment %}
OR

```shell
if [ -f $(brew --prefix)/etc/bash_completion ]; then
. $(brew --prefix)/etc/bash_completion
fi
```

### Zsh

{% comment %}
{% endcomment %}
In Zsh, the [completion
system](http://zsh.sourceforge.net/Doc/Release/Completion-System.html){:target="_blank"
class="_"} takes care of things. To activate completion for Docker commands,
these files need to be copied or symlinked to your Zsh `site-functions/`
directory. For example, if you installed Zsh via [Homebrew](http://brew.sh/):

```bash
etc=/Applications/Docker.app/Contents/Resources/etc
ln -s $etc/docker.zsh-completion /usr/local/share/zsh/site-functions/_docker
ln -s $etc/docker-compose.zsh-completion /usr/local/share/zsh/site-functions/_docker-compose
```

{% comment %}
{% endcomment %}
### Fish-Shell

{% comment %}
{% endcomment %}
Fish-shell also supports tab completion [completion
system](https://fishshell.com/docs/current/#tab-completion){:target="_blank"
class="_"}. To activate completion for Docker commands,
these files need to be copied or symlinked to your Fish-shell `completions/`
directory.

{% comment %}
{% endcomment %}
Create the `completions` directory:

```bash
mkdir -p ~/.config/fish/completions
```

{% comment %}
{% endcomment %}
Now add fish completions from docker.

```bash
ln -shi /Applications/Docker.app/Contents/Resources/etc/docker.fish-completion ~/.config/fish/completions/docker.fish
ln -shi /Applications/Docker.app/Contents/Resources/etc/docker-compose.fish-completion ~/.config/fish/completions/docker-compose.fish
```

{% comment %}
{% endcomment %}
## Give feedback and get help

{% comment %}
{% endcomment %}
To get help from the community, review current user topics, join or start a
discussion, log on to our [Docker Desktop for Mac
forum](https://forums.docker.com/c/docker-for-mac){:target="_blank" class="_"}.

{% comment %}
{% endcomment %}
To report bugs or problems, log on to Docker Desktop [for Mac issues on
GitHub](https://github.com/docker/for-mac/issues){:target="_blank" class="_"},
where you can review community reported issues, and file new ones.  See
[Logs and Troubleshooting](troubleshoot.md) for more details.

{% comment %}
{% endcomment %}
For information about providing feedback on the documentation or update it yourself, see [Contribute to documentation](/opensource/).

## Docker Hub

{% comment %}
{% endcomment %}
Select **Sign in /Create Docker ID** from the Docker Desktop menu to access your [Docker Hub](https://hub.docker.com/){: target="_blank" class="_" } account. Once logged in, you can access your Docker Hub repositories and organizations directly from the Docker Desktop menu.

{% comment %}
{% endcomment %}
For more information, refer to the following [Docker Hub topics](../docker-hub/index.md){:target="_blank"
class="_"}:

{% comment %}
{% endcomment %}
* [Organizations and Teams in Docker Hub](../docker-hub/orgs.md){:target="_blank" class="_"}
* [Builds](../docker-hub/builds/index.md){:target="_blank" class="_"}

{% comment %}
{% endcomment %}
### Two-factor authentication

{% comment %}
{% endcomment %}
Docker Desktop enables you to sign into Docker Hub using two-factor authentication. Two-factor authentication provides an extra layer of security when accessing your Docker Hub account.

{% comment %}
{% endcomment %}
You must enable two-factor authentication in Docker Hub before signing into your Docker Hub account through Docker Desktop. For instructions, see [Enable two-factor authentication for Docker Hub](/docker-hub/2fa/).

{% comment %}
{% endcomment %}
After you have enabled two-factor authentication:

{% comment %}
{% endcomment %}
1. Go to the Docker Desktop menu and then select **Sign in / Create Docker ID**.

{% comment %}
{% endcomment %}
2. Enter your Docker ID and password and click **Sign in**.

{% comment %}
{% endcomment %}
3. After you have successfully signed in, Docker Desktop prompts you to enter the authentication code. Enter the six-digit code from your phone and then click **Verify**.

{% comment %}
{% endcomment %}
![Docker Desktop 2FA](images/desktop-mac-2fa.png){:width="500px"}

{% comment %}
{% endcomment %}
After you have successfully authenticated, you can access your organizations and repositories directly from the Docker Desktop menu.

{% comment %}
## Where to go next
{% endcomment %}
{: #where-to-go-next }
## 次に読むものは

{% comment %}
{% endcomment %}
* Try out the walkthrough at [Get Started](/get-started/){: target="_blank"
  class="_"}.

{% comment %}
{% endcomment %}
* Dig in deeper with [Docker Labs](https://github.com/docker/labs/) example
  walkthroughs and source code.

{% comment %}
{% endcomment %}
* For a summary of Docker command line interface (CLI) commands, see
  [Docker CLI Reference Guide](../engine/api/index.md){: target="_blank" class="_"}.

{% comment %}
{% endcomment %}
* Check out the blog post, [What’s New in Docker 17.06 Community Edition
  (CE)](https://blog.docker.com/2017/07/whats-new-docker-17-06-community-edition-ce/){:
  target="_blank" class="_"}.
