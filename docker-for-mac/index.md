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
title: Docker Desktop for Mac ユーザーマニュアル
toc_min: 1
toc_max: 2
---

{% comment %}
Welcome to Docker Desktop! The Docker Desktop for Mac user manual provides information on how to configure and manage your Docker Desktop settings.
{% endcomment %}
Docker Desktop へようこそ！
Docker Desktop for Mac ユーザーマニュアルでは Docker Desktop 設定をどのように行い、管理していくのかを説明しています。

{% comment %}
For information about Docker Desktop download, system requirements, and installation instructions, see [Install Docker Desktop](install.md).
{% endcomment %}
Docker Desktop のダウンロード、システム要件、インストール手順については [Docker Desktop のインストール](install.md) を参照してください。

{% comment %}
>**Note**
>
> This page contains information about the Docker Desktop Stable release. For information about features available in Edge releases, see the [Edge release notes](edge-release-notes/).
{% endcomment %}
>**メモ**
>
> 本ページでは Docker Desktop 安定版（Stable）についての情報を示します。
最新版（Edge）において利用可能な機能については [Edge リリースノート](edge-release-notes/) を参照してください。

{% comment %}
## Preferences
{% endcomment %}
{: #preferences }
## Preferences メニュー

{% comment %}
The Docker **Preferences** menu allows you to configure your Docker settings such as installation, updates, version channels, Docker Hub login,
and more.
{% endcomment %}
Docker の **Preferences** メニューでは Docker に対する設定として、インストール設定、アップデート、バージョンチャネル、Docker Hub ログインなどを行うことができます。

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
- [Volume mounting requires file sharing for any project directories outside of `/Users`](troubleshoot.md#volume-mounting-requires-file-sharing-for-any-project-directories-outside-of-users).

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
Docker Desktop では macOS の HTTP/HTTPS プロキシー設定を検出して、その内容を Docker に対して自動的に伝えます。
たとえばプロキシー設定を `http://proxy.example.com` としている場合、Docker はこのプロキシー情報を利用して、コンテナーのプル処理を行います。

{% comment %}
Your proxy settings, however, will not be propagated into the containers you start.
If you wish to set the proxy settings for your containers, you need to define
environment variables for them, just like you would do on Linux, for example:
{% endcomment %}
しかしそのプロキシー設定は、起動したコンテナーには伝えられません。
コンテナーに対してプロキシー設定を行いたい場合は、Linux 上において行うのと同じように、環境変数を使って定義することが必要です。
たとえば以下のとおりです。

```
$ docker run -e HTTP_PROXY=http://proxy.example.com:3128 alpine env

PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
HOSTNAME=b7edf988b2b5
TERM=xterm
HOME=/root
HTTP_PROXY=http://proxy.example.com:3128
```

{% comment %}
For more information on setting environment variables for running containers,
see [Set environment variables](/engine/reference/commandline/run/#set-environment-variables--e---env---env-file).
{% endcomment %}
実行中のコンテナーに対して環境変数を設定する方法については [環境変数の設定](/engine/reference/commandline/run/#set-environment-variables--e---env---env-file) を参照してください。

{% comment %}
#### Network
{% endcomment %}
{: #network }
#### Network タブ

{% comment %}
You can configure Docker Desktop networking to work on a virtual private network (VPN). Specify a network address translation (NAT) prefix and subnet mask to enable Internet connectivity.
{% endcomment %}
Docker Desktop のネットワーク設定により、仮想プライベートネットワーク（VPN）上で動作するように設定することができます。
インターネットへの接続を有効にするには、ネットワークアドレス変換（NAT）のプリフィックスとサブネットマスクを設定していください。

{% comment %}
### Docker Engine
{% endcomment %}
### Docker Engine タブ

{% comment %}
The Docker Engine page allows you to configure the Docker daemon to determine how your containers run.
{% endcomment %}
Docker Engine のページでは、Docker デーモンに対して、コンテナーを実行させる方法を設定することができます。

{% comment %}
Type a JSON configuration file in the box to configure the daemon settings. For a full list of options, see the Docker Engine
[dockerd commandline reference](/engine/reference/commandline/dockerd/){:target="_blank"
class="_"}.
{% endcomment %}
テキスト入力欄に JSON 設定ファイルを入力して、デーモンを設定します。
オプションの全一覧についてはDocker Engine の [dockerd コマンドラインリファレンス](/engine/reference/commandline/dockerd/){:target="_blank" class="_"} を参照してください。

{% comment %}
Click **Apply & Restart** to save your settings and restart Docker Desktop.
{% endcomment %}
**Apply & Restart** をクリックして設定内容を保存し、Docker Desktop を再起動します。

{% comment %}
### Command Line
{% endcomment %}
{: #command-line }
### Command Line タブ

{% comment %}
On the Command Line page, you can specify whether or not to enable experimental features.
{% endcomment %}
Command Line のページでは、試験的機能を有効にするかどうかを設定することができます。

{% include experimental.md %}

{% comment %}
On both Docker Desktop Edge and Stable releases, you can toggle the experimental features on and off. If you toggle the experimental features off, Docker Desktop uses the current generally available release of Docker Engine.
{% endcomment %}
Docker Desktop の最新版（Edge）と安定版（Stable）の両方において、試験的機能は有効無効を切り替えることができます。
試験的機能を無効にした場合、Docker Desktop は、Docker Engine の安定版を利用することになります。

{% comment %}
You can see whether you are running experimental mode at the command line. If
`Experimental` is `true`, then Docker is running in experimental mode, as shown
here. (If `false`, Experimental mode is off.)
{% endcomment %}
試験的機能モードを有効にしているかどうかは、コマンドラインから確認できます。
以下に示すように、`Experimental` が `true` となっていれば、試験的機能モードが有効です。
（`false` であれば、試験的機能モードはオフです。）

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

{% comment %}
### Kubernetes
{% endcomment %}
### Kubernetes タブ

{% comment %}
Docker Desktop includes a standalone Kubernetes server that runs on your Mac, so
that you can test deploying your Docker workloads on Kubernetes.
{% endcomment %}
Docker Desktop には、Mac 上で稼動するスタンドアロンの Kubernetes サーバーが含まれます。
したがって Kubernetes 上に構築した Docker アプリをデプロイするテストができます。

{% comment %}
The Kubernetes client command, `kubectl`, is included and configured to connect
to the local Kubernetes server. If you have `kubectl` already installed and
pointing to some other environment, such as `minikube` or a GKE cluster, be sure
to change context so that `kubectl` is pointing to `docker-desktop`:
{% endcomment %}
Kubernetes のクライアントコマンドである `kubectl` が提供されていて、ローカルの Kubernetes サーバーへの接続するように設定されています。
`kubectl` をすでにインストールしていて、`minikube` や GKE クラスターといった別の環境に向いている場合は、その内容を変更して、`kubectl` が `docker-desktop` を向くようにしてください。

```bash
$ kubectl config get-contexts
$ kubectl config use-context docker-desktop
```

{% comment %}
If you installed `kubectl` with Homebrew, or by some other method, and
experience conflicts, remove `/usr/local/bin/kubectl`.
{% endcomment %}
Homebrew あるいは別の方法により `kubectl` をインストールしていて、衝突が起きていたら `/usr/local/bin/kubectl` を削除してください。

{% comment %}
- To enable Kubernetes support and install a standalone instance of Kubernetes
  running as a Docker container, select **Enable Kubernetes**. To set Kubernetes as the
  [default orchestrator](kubernetes.md#override-the-default-orchestrator), select **Deploy Docker Stacks to Kubernetes by default**.
{% endcomment %}
- Kubernetes サポートを有効にし、Docker コンテナーとして起動するスタンドアロンの Kubernetes インスタンスをインストールするには、**Enable Kubernetes** を選択します。
  Kubernetes を [デフォルトのオーケストレーター](kubernetes.md#override-the-default-orchestrator) として設定する場合は、**Deploy Docker Stacks to Kubernetes by default** を選択します。

   {% comment %}
   Click **Apply & Restart** to save the settings. This instantiates images required to run the Kubernetes server as containers, and installs the
  `/usr/local/bin/kubectl` command on your Mac.
   {% endcomment %}
   **Apply & Restart** をクリックして設定を保存します。
   これによって、Kubernetes サーバーをコンテナーとして起動するためのイメージがインスタンス化されます。
   そして Mac 内に `/usr/local/bin/kubectl` コマンドがインストールされます。

  {% comment %}
  ![Enable Kubernetes](images/kubernetes/kube.png){:width="750px"}
  {% endcomment %}
  ![Kubernetes の有効化](images/kubernetes/kube.png){:width="750px"}

  {% comment %}
  When Kubernetes is enabled and running, an additional status bar item displays
  at the bottom right of the Docker Desktop Settings dialog.
  {% endcomment %}
  Kubernetes が有効になって稼動していると、Docker Desktop の Settings 画面の下段右側に、新たにステータスバーが表示されます。

  {% comment %}
  The status of Kubernetes shows in the Docker menu and the context points to
  `docker-desktop`.
  {% endcomment %}
  Kubernetes の状態は Docker メニューに表示され、context が `docker-desktop` を指します。

  {% comment %}
  ![Docker Menu with Kubernetes](images/kubernetes/kube-context.png){: .with-border
  width="400px"}
  {% endcomment %}
  ![Docker メニュー上の Kubernetes](images/kubernetes/kube-context.png){: .with-border
  width="400px"}

{% comment %}
- By default, Kubernetes containers are hidden from commands like `docker
  service ls`, because managing them manually is not supported. To make them
  visible, select **Show system containers (advanced)** and click **Apply and
  Restart**. Most users do not need this option.
{% endcomment %}
- デフォルトで Kubernetes コンテナーは `docker service ls` などのコマンドには現れません。
  手動で管理することがサポートされていないためです。
  コマンド上に表示させるためには、**Show system containers (advanced)** を選択して **Apply and Restart** をクリックします。
  ただしたいていのユーザーにとって、このオプションは不要です。

{% comment %}
- To disable Kubernetes support at any time, clear the **Enable Kubernetes** check box. The
  Kubernetes containers are stopped and removed, and the
  `/usr/local/bin/kubectl` command is removed.
{% endcomment %}
- どの時点でも Kubernetes サポートを無効にするには、**Enable Kubernetes** チェックボックスをオフにします。
  Kubernetes コンテナーが停止して削除されます。
  そして `/usr/local/bin/kubectl` コマンドも削除されます。

  {% comment %}
  For more about using the Kubernetes integration with Docker Desktop, see
  [Deploy on Kubernetes](kubernetes.md){:target="_blank" class="_"}.
  {% endcomment %}
  Docker Desktop における Kubernetes 利用の詳細については [Kubernetes へのデプロイ](kubernetes.md){:target="_blank" class="_"} を参照してください。

{% comment %}
### Reset
{% endcomment %}
### Reset タブ

{% comment %}
> Reset and Restart options
>
> On Docker Desktop Mac, the **Restart Docker Desktop**, **Reset to factory defaults**, and other reset options are available from the **Troubleshoot** menu.
{% endcomment %}
> Reset と Restart オプション
>
> Docker Desktop Mac において **Restart Docker Desktop**、**Reset to factory defaults**、その他のリセットオプションは、**Troubleshoot** メニューにあります。

{% comment %}
For information about the reset options, see [Logs and Troubleshooting](troubleshoot.md).
{% endcomment %}
リセットオプションに関する詳細は [ログ機能とトラブルシューティング](troubleshoot.md) を参照してください。

{% comment %}
## Dashboard
{% endcomment %}
{: #dashboard }
## ダッシュボード

{% comment %}
The Docker Desktop Dashboard enables you to interact with containers and applications and manage the lifecycle of your applications directly from your machine. The Dashboard UI shows all running, stopped, and started containers with their state. It provides an intuitive interface to perform common actions to inspect and manage containers and existing Docker Compose applications. For more information, see [Docker Desktop Dashboard](../desktop/dashboard.md).
{% endcomment %}
Docker Desktop ダッシュボードを利用すると、コンテナーやアプリケーションとのやりとりが行えるようになり、アプリケーションのライフサイクルを手元のマシンから管理することができます。
ダッシュボードの UI にはすべてのコンテナーが表示され、実行中、停止中、開始中といった状態が示されます。
提供されている UI は直感的になっていて、コンテナーや Docker Compose アプリケーションを確認したり管理したりといった通常操作を行うことができます。
詳しくは [Docker Desktop ダッシュボード](../desktop/dashboard.md) を参照してください。

{% comment %}
## Add TLS certificates
{% endcomment %}
{: #add-tls-certificates }
## TLS 証明書の追加

{% comment %}
You can add trusted Certificate Authorities (CAs) (used to verify registry
server certificates) and client certificates (used to authenticate to
registries) to your Docker daemon.
{% endcomment %}
Docker デーモンに対しては、信頼できる認証局（Certificate Authorities; CAs）（レジストリサーバー証明書の確認のため）やクライアント証明書（レジストリの認証のため）を追加することができます。

{% comment %}
### Add custom CA certificates (server side)
{% endcomment %}
{: #add-custom-ca-certificates-server-side }
### カスタム CA 証明書の追加（サーバー側）

{% comment %}
All trusted CAs (root or intermediate) are supported. Docker Desktop creates a
certificate bundle of all user-trusted CAs based on the Mac Keychain, and
appends it to Moby trusted certificates. So if an enterprise SSL certificate is
trusted by the user on the host, it is trusted by Docker Desktop.
{% endcomment %}
信頼できる CA （ルート証明書や中間 CA 証明書）はすべてサポートされます。
Docker Desktop では Mac キーチェーンに基づいて、信頼できる CA による証明書バンドルが生成されます。
そしてこれを Moby の信頼できる証明書に追加します。
したがってエンタープライズ向けの SSL 証明書が、ホスト上のユーザーによって認証されていると、それは Docker Desktop によって認証されているものとなります。

{% comment %}
To manually add a custom, self-signed certificate, start by adding the
certificate to the macOS keychain, which is picked up by Docker Desktop. Here is
an example:
{% endcomment %}
カスタマイズした自己署名の証明書を手動で追加するには、まずその証明書を macOS キーチェーンに追加してください。
こうしておけば Docker Desktop が証明書を認識してくれます。
たとえば以下のとおりです。

```bash
$ sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain ca.crt
```

{% comment %}
Or, if you prefer to add the certificate to your own local keychain only (rather
than for all users), run this command instead:
{% endcomment %}
あるいは、その証明書をローカルな独自キーチェーンにのみ追加したい（全ユーザー向けとはしたくない）場合は、以下のコマンドを実行します。

```
$ security add-trusted-cert -d -r trustRoot -k ~/Library/Keychains/login.keychain ca.crt
```

{% comment %}
See also, [Directory structures for
certificates](#directory-structures-for-certificates).
{% endcomment %}
[証明書のディレクトリ構造](#directory-structures-for-certificates) も参照してください。

{% comment %}
> **Note**: You need to restart Docker Desktop after making any changes to the
> keychain or to the `~/.docker/certs.d` directory in order for the changes to
> take effect.
{% endcomment %}
> **メモ**: キーチェーンや `~/.docker/certs.d` ディレクトリに変更を加えたら、これを有効にするために Docker Desktop を再起動することが必要です。

{% comment %}
For a complete explanation of how to do this, see the blog post [Adding
Self-signed Registry Certs to Docker & Docker Desktop for
Mac](http://container-solutions.com/adding-self-signed-registry-certs-docker-mac/){:target="_blank"
class="_"}.
{% endcomment %}
これを実現するための方法として、ブログ投稿 [Adding
Self-signed Registry Certs to Docker & Docker Desktop for
Mac](http://container-solutions.com/adding-self-signed-registry-certs-docker-mac/){:target="_blank"
class="_"} に説明が網羅されているので参照してください。

{% comment %}
### Add client certificates
{% endcomment %}
{: #add-client-certificates }
### クライアント証明書の追加

{% comment %}
You can put your client certificates in
`~/.docker/certs.d/<MyRegistry>:<Port>/client.cert` and
`~/.docker/certs.d/<MyRegistry>:<Port>/client.key`.
{% endcomment %}
クライアント証明書は `~/.docker/certs.d/<MyRegistry>:<Port>/client.cert` と `~/.docker/certs.d/<MyRegistry>:<Port>/client.key` に置きます。

{% comment %}
When the Docker Desktop application starts, it copies the `~/.docker/certs.d`
folder on your Mac to the `/etc/docker/certs.d` directory on Moby (the Docker
Desktop `xhyve` virtual machine).
{% endcomment %}
Docker Desktop のアプリケーションが起動すると、Mac 上の `~/.docker/certs.d` フォルダーの内容が、Moby（Docker Desktop の仮想マシン `xhyve`）の `/etc/docker/certs.d` ディレクトリにコピーされます。

{% comment %}
> * You need to restart Docker Desktop after making any changes to the keychain
>   or to the `~/.docker/certs.d` directory in order for the changes to take
>   effect.
>
> * The registry cannot be listed as an _insecure registry_ (see [Docker
>   Engine](#docker-engine). Docker Desktop ignores certificates listed
>   under insecure registries, and does not send client certificates. Commands
>   like `docker run` that attempt to pull from the registry produce error
>   messages on the command line, as well as on the registry.
{% endcomment %}
> * キーチェーンや `~/.docker/certs.d` ディレクトリに変更を加えたら、これを有効にするために Docker Desktop を再起動することが必要です。
>
> * レジストリ一覧において、レジストリは **安全な** レジストリの扱いになります。
>   （[Docker Engine](#docker-engine) を参照してください。）
>   Docker Desktop では、安全でない（insecure）レジストリのもとにある証明書は無視します。
>   そしてクライアント証明書を送信することはなくなります。
>   `docker run` などのコマンドを通じてそのレジストリからプルを行おうとすると、コマンドライン上にエラーメッセージが出力されます。
>   レジストリ上でもエラー発生します。

{% comment %}
### Directory structures for certificates
{% endcomment %}
{: #directory-structures-for-certificates }
### 証明書のディレクトリ構造

{% comment %}
If you have this directory structure, you do not need to manually add the CA
certificate to your Mac OS system login:
{% endcomment %}
以下のようなディレクトリ構造がすでにある場合は、Mac OS へのログインにあたって、CA を手動で追加する必要はありません。

```
/Users/<user>/.docker/certs.d/
└── <MyRegistry>:<Port>
   ├── ca.crt
   ├── client.cert
   └── client.key
```

{% comment %}
The following further illustrates and explains a configuration with custom
certificates:
{% endcomment %}
さらに以下では、カスタム証明書を利用する場合の設定状況を示しています。

{% comment %}
```
/etc/docker/certs.d/        <-- Certificate directory
└── localhost:5000          <-- Hostname:port
   ├── client.cert          <-- Client certificate
   ├── client.key           <-- Client key
   └── ca.crt               <-- Certificate authority that signed
                                the registry certificate
```
{% endcomment %}
```
/etc/docker/certs.d/        <-- 証明書ディレクトリ
└── localhost:5000          <-- ホスト名:ポート
   ├── client.cert          <-- クライアント証明書
   ├── client.key           <-- クライアント鍵
   └── ca.crt               <-- レジストリ証明書により署名された CA
```

{% comment %}
You can also have this directory structure, as long as the CA certificate is
also in your keychain.
{% endcomment %}
以下のようなディレクトリ構造にすることもできます。
CA 証明書がキーチェーンにも存在しているものとします。

```
/Users/<user>/.docker/certs.d/
└── <MyRegistry>:<Port>
    ├── client.cert
    └── client.key
```

{% comment %}
To learn more about how to install a CA root certificate for the registry and
how to set the client TLS certificate for verification, see
[Verify repository client with certificates](../engine/security/certificates.md)
in the Docker Engine topics.
{% endcomment %}
レジストリにおける CA ルート証明書のインストール方法、あるいはクライアント TLS 証明書の設定方法については、Docker Engine の説明内にある [証明書を使ったリポジトリクライアントの確認](../engine/security/certificates.md) を参照してください。

{% comment %}
## Install shell completion
{% endcomment %}
{: #install-shell-completion }
## シェル補完のインストール

{% comment %}
Docker Desktop comes with scripts to enable completion for the `docker` and `docker-compose` commands. The completion scripts may be
found inside `Docker.app`, in the `Contents/Resources/etc/` directory and can be
installed both in Bash and Zsh.
{% endcomment %}
Docker Desktop には、`docker` や`docker-compose` コマンドにおいて、入力補完を可能とするスクリプトがあります。
補完スクリプトは `Contents/Resources/etc/` ディレクトリの `Docker.app` 内にあります。
これは Bash および Zsh においてインストールすることができます。

### Bash

{% comment %}
Bash has [built-in support for
completion](https://www.debian-administration.org/article/316/An_introduction_to_bash_completion_part_1){:target="_blank"
class="_"} To activate completion for Docker commands, these files need to be
copied or symlinked to your `bash_completion.d/` directory. For example, if you
installed bash via [Homebrew](http://brew.sh/):
{% endcomment %}
Bash には [入力補完のためのビルトインサポート](https://www.debian-administration.org/article/316/An_introduction_to_bash_completion_part_1){:target="_blank" class="_"} があります。
Docker コマンドに対して入力補完を有効にするには、`bash_completion.d/` ディレクトリに上記ファイルをコピーするかシンボリックリンクを張ります。
たとえば [Homebrew](http://brew.sh/) から Bash をインストールしている場合は、以下のようにします。

```bash
etc=/Applications/Docker.app/Contents/Resources/etc
ln -s $etc/docker.bash-completion $(brew --prefix)/etc/bash_completion.d/docker
ln -s $etc/docker-compose.bash-completion $(brew --prefix)/etc/bash_completion.d/docker-compose
```

{% comment %}
Add the following to your `~/.bash_profile`:
{% endcomment %}
そして以下の記述を `~/.bash_profile` に追加します。

```shell
[ -f /usr/local/etc/bash_completion ] && . /usr/local/etc/bash_completion
```

{% comment %}
OR
{% endcomment %}
または以下を追加します。

```shell
if [ -f $(brew --prefix)/etc/bash_completion ]; then
. $(brew --prefix)/etc/bash_completion
fi
```

### Zsh

{% comment %}
In Zsh, the [completion
system](http://zsh.sourceforge.net/Doc/Release/Completion-System.html){:target="_blank"
class="_"} takes care of things. To activate completion for Docker commands,
these files need to be copied or symlinked to your Zsh `site-functions/`
directory. For example, if you installed Zsh via [Homebrew](http://brew.sh/):
{% endcomment %}
Zsh においては、[入力補完システム](http://zsh.sourceforge.net/Doc/Release/Completion-System.html){:target="_blank" class="_"} というものが処理を行ってくれます。
Docker コマンドに対する入力補完を有効にするには、上記ファイルを Zsh のディレクトリ `site-functions/` にコピーするかシンボリックリンクを張ります。
たとえば [Homebrew](http://brew.sh/) から Zsh をインストールしている場合は、以下のようにします。

```bash
etc=/Applications/Docker.app/Contents/Resources/etc
ln -s $etc/docker.zsh-completion /usr/local/share/zsh/site-functions/_docker
ln -s $etc/docker-compose.zsh-completion /usr/local/share/zsh/site-functions/_docker-compose
```

### Fish-Shell

{% comment %}
Fish-shell also supports tab completion [completion
system](https://fishshell.com/docs/current/#tab-completion){:target="_blank"
class="_"}. To activate completion for Docker commands,
these files need to be copied or symlinked to your Fish-shell `completions/`
directory.
{% endcomment %}
Fish-shell でも、タブ入力による [入力補完システム](https://fishshell.com/docs/current/#tab-completion){:target="_blank" class="_"} をサポートしています。
Docker コマンドに対する入力補完を有効にするには、上記ファイルを Fish-shel のディレクトリ `completions/` にコピーするかシンボリックリンクを張ります。

{% comment %}
Create the `completions` directory:
{% endcomment %}
まず `completions` ディレクトリを生成します。

```bash
mkdir -p ~/.config/fish/completions
```

{% comment %}
Now add fish completions from docker.
{% endcomment %}
Docker の Fish 入力補完を追加します。

```bash
ln -shi /Applications/Docker.app/Contents/Resources/etc/docker.fish-completion ~/.config/fish/completions/docker.fish
ln -shi /Applications/Docker.app/Contents/Resources/etc/docker-compose.fish-completion ~/.config/fish/completions/docker-compose.fish
```

{% comment %}
## Give feedback and get help
{% endcomment %}
{: #give-feedback-and-get-help }
## フィードバックやヘルプ

{% comment %}
To get help from the community, review current user topics, join or start a
discussion, log on to our [Docker Desktop for Mac
forum](https://forums.docker.com/c/docker-for-mac){:target="_blank" class="_"}.
{% endcomment %}
コミュニティのヘルプを必要とする場合は、最新のユーザートピックを確認し、ディスカッションへの参加、開始をしてみてください。
[Docker Desktop for Mac フォーラム](https://forums.docker.com/c/docker-for-mac){:target="_blank" class="_"} にログインして行います。

{% comment %}
To report bugs or problems, log on to Docker Desktop [for Mac issues on
GitHub](https://github.com/docker/for-mac/issues){:target="_blank" class="_"},
where you can review community reported issues, and file new ones.  See
[Logs and Troubleshooting](troubleshoot.md) for more details.
{% endcomment %}
バグや問題を報告するには、[Docker Desktop for Mac 向けの GitHub issue](https://github.com/docker/for-mac/issues){:target="_blank" class="_"} にログインして、コミュニティによって報告済みの issue を確認してください。
そして新たな issue をあげてください。
詳しくは [ログ機能とトラブルシューティング](troubleshoot.md) を参照してください。

{% comment %}
For information about providing feedback on the documentation or update it yourself, see [Contribute to documentation](/opensource/).
{% endcomment %}
ドキュメントに関するフィードバックや更新提案については [ドキュメントへの貢献](/opensource/) を参照してください。

## Docker Hub

{% comment %}
Select **Sign in /Create Docker ID** from the Docker Desktop menu to access your [Docker Hub](https://hub.docker.com/){: target="_blank" class="_" } account. Once logged in, you can access your Docker Hub repositories and organizations directly from the Docker Desktop menu.
{% endcomment %}
Docker Desktop メニューから **Sign in /Create Docker ID** を選択すれば [Docker Hub](https://hub.docker.com/){: target="_blank" class="_" } アカウントを使ってアクセスすることができます。
ログインした後は、Docker Desktop メニューから、Docker Hub リポジトリや組織に直接アクセスすることができます。

{% comment %}
For more information, refer to the following [Docker Hub topics](../docker-hub/index.md){:target="_blank"
class="_"}:
{% endcomment %}
詳しい情報については、以下の [Docker Hub トピック](../docker-hub/index.md){:target="_blank" class="_"} を参照してください。

{% comment %}
* [Organizations and Teams in Docker Hub](../docker-hub/orgs.md){:target="_blank" class="_"}
* [Builds](../docker-hub/builds/index.md){:target="_blank" class="_"}
{% endcomment %}
* [Docker Hub における組織とチーム](../docker-hub/orgs.md){:target="_blank" class="_"}
* [ビルド](../docker-hub/builds/index.md){:target="_blank" class="_"}

{% comment %}
### Two-factor authentication
{% endcomment %}
{: #two-factor-authentication }
### 2 要素認証

{% comment %}
Docker Desktop enables you to sign into Docker Hub using two-factor authentication. Two-factor authentication provides an extra layer of security when accessing your Docker Hub account.
{% endcomment %}
Docker Desktop では Docker Hub へのサインイン時に 2 要素認証（two-factor authentication）を利用することができます。
2 要素認証は Docker Hub アカウントへのアクセス時に、二重のセキュリティを提供するものです。

{% comment %}
You must enable two-factor authentication in Docker Hub before signing into your Docker Hub account through Docker Desktop. For instructions, see [Enable two-factor authentication for Docker Hub](/docker-hub/2fa/).
{% endcomment %}
Docker Desktop 経由で Docker Hub アカウントへのサインインを行う前には、あらかじめ 2 要素認証を有効にしておく必要があります。
その手順については [Docker Hub における 2 要素認証の有効化](/docker-hub/2fa/) を参照してください。

{% comment %}
After you have enabled two-factor authentication:
{% endcomment %}
2 要素認証の有効化が済んだら、以下を行います。

{% comment %}
1. Go to the Docker Desktop menu and then select **Sign in / Create Docker ID**.
{% endcomment %}
1. Docker Desktop メニューから **Sign in / Create Docker ID** を実行します。

{% comment %}
2. Enter your Docker ID and password and click **Sign in**.
{% endcomment %}
2. Docker ID とパスワードを入力して **Sign in** をクリックします。

{% comment %}
3. After you have successfully signed in, Docker Desktop prompts you to enter the authentication code. Enter the six-digit code from your phone and then click **Verify**.
{% endcomment %}
3. サインインに成功したら、Docker Desktop が認証コードの入力を求めてきます。
   電話に届いた 6 桁のコードを入力して **Verify** をクリックします。

{% comment %}
![Docker Desktop 2FA](images/desktop-mac-2fa.png){:width="500px"}
{% endcomment %}
![Docker Desktop の 2 要素認証](images/desktop-mac-2fa.png){:width="500px"}

{% comment %}
After you have successfully authenticated, you can access your organizations and repositories directly from the Docker Desktop menu.
{% endcomment %}
認証が正常に行われたら、Docker Desktop メニューから組織やリポジトリに直接アクセスできるようになります。

{% comment %}
## Where to go next
{% endcomment %}
{: #where-to-go-next }
## 次に読むものは

{% comment %}
* Try out the walkthrough at [Get Started](/get-started/){: target="_blank"
  class="_"}.
{% endcomment %}
* [はじめよう](/get-started/){: target="_blank" class="_"} に示されているウォークスルーを試してみてください。

{% comment %}
* Dig in deeper with [Docker Labs](https://github.com/docker/labs/) example
  walkthroughs and source code.
{% endcomment %}
* [Docker Labs](https://github.com/docker/labs/) にあるウォークスルーやソースコードを参照して、理解を深めてください。

{% comment %}
* For a summary of Docker command line interface (CLI) commands, see
  [Docker CLI Reference Guide](../engine/api/index.md){: target="_blank" class="_"}.
{% endcomment %}
* コマンドラインインターフェース（CLI）コマンドのまとめについては [Docker CLI リファレンスガイド](../engine/api/index.md){: target="_blank" class="_"} を参照してください。

{% comment %}
* Check out the blog post, [What’s New in Docker 17.06 Community Edition
  (CE)](https://blog.docker.com/2017/07/whats-new-docker-17-06-community-edition-ce/){:
  target="_blank" class="_"}.
{% endcomment %}
* ブログ投稿 [What’s New in Docker 17.06 Community Edition
  (CE)](https://blog.docker.com/2017/07/whats-new-docker-17-06-community-edition-ce/){:
  target="_blank" class="_"} を確認してみてください。
