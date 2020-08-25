---
description: Getting Started
keywords: windows, edge, tutorial, run, docker, local, machine
redirect_from:
- /winkit/getting-started/
- /winkit/
- /windows/
- /windows/started/
- /docker-for-windows/started/
- /installation/windows/
- /engine/installation/windows/
- /docker-for-windows/index/
title: Docker Desktop for Windows ユーザーマニュアル
toc_min: 1
toc_max: 2
---

{% comment %}
Welcome to Docker Desktop! The Docker Desktop for Windows user manual provides information on how to configure and manage your Docker Desktop settings.
{% endcomment %}
Docker Desktop へようこそ！
Docker Desktop for Windows ユーザーマニュアルでは Docker Desktop 設定をどのように行い、管理していくのかを説明しています。

{% comment %}
For information about Docker Desktop download, system requirements, and installation instructions, see [Install Docker Desktop](install.md).
{% endcomment %}
Docker Desktop のダウンロード、システム要件、インストール手順については [Docker Desktop のインストール](install.md) を参照してください。

{% comment %}
>**Note**
>
> This page contains information about the Docker Desktop Stable release. For information about features available in Edge releases, see the [Edge release notes](edge-release-notes.md).
{% endcomment %}
>**メモ**
>
> 本ページでは Docker Desktop 安定版（Stable）についての情報を示します。
最新版（Edge）において利用可能な機能については [Edge リリースノート](edge-release-notes.md) を参照してください。

{% comment %}
## Settings
{% endcomment %}
{: #settings }
## Settings ダイアログ

{% comment %}
The **Docker Desktop** menu allows you to configure your Docker settings such as installation, updates, version channels, Docker Hub login,
and more.
{% endcomment %}
The **Docker Desktop** menu allows you to configure your Docker settings such as installation, updates, version channels, Docker Hub login,
and more.

{% comment %}
This section explains the configuration options accessible from the **Settings** dialog.
{% endcomment %}
This section explains the configuration options accessible from the **Settings** dialog.

{% comment %}
1. Open the Docker Desktop menu by clicking the Docker icon in the Notifications area (or System tray):
{% endcomment %}
1. Open the Docker Desktop menu by clicking the Docker icon in the Notifications area (or System tray):

    {% comment %}
    ![Showing hidden apps in the taskbar](images/whale-icon-systray-hidden.png){:width="250px"}
    {% endcomment %}
    ![Showing hidden apps in the taskbar](images/whale-icon-systray-hidden.png){:width="250px"}

{% comment %}
2. Select **Settings** to open the Settings dialog:
{% endcomment %}
2. Select **Settings** to open the Settings dialog:

    {% comment %}
    ![Docker Desktop popup menu](images/docker-menu-settings.png){:width="300px"}
    {% endcomment %}
    ![Docker Desktop ポップアップメニュー](images/docker-menu-settings.png){:width="300px"}

{% comment %}
### General
{% endcomment %}
{: #general }
### General タブ

{% comment %}
On the **General** tab of the Settings dialog, you can configure when to start and update Docker.
{% endcomment %}
Settings ダイアログの **General** タブにおいて、Docker の起動や更新をいつ行うのかを設定します。

![Settings](images/settings-general.png){:width="750px"}

{% comment %}
* **Start Docker when you log in** - Automatically start Docker Desktop upon Windows system login.
{% endcomment %}
* **Start Docker when you log in**（ログイン時に Docker を起動） - Windows にログインしたときに自動的に Docker Desktop を起動します。

{% comment %}
* **Automatically check for updates** - By default, Docker Desktop automatically checks for updates and notifies you when an update is available.
Click **OK** to accept and install updates (or cancel to keep the current
version). You can manually update by choosing **Check for Updates** from the
main Docker menu.
{% endcomment %}
* **Automatically check for updates**（アップデートの自動更新） - デフォルトでは、Docker Desktop の更新チェックは自動的に行われ、利用可能な更新がある場合は通知されます。
  **OK** のクリックにより決定し、更新をインストールします。
  （または現行バージョンのままとする場合はキャンセルします。）
  Docker のメインメニューにある **Check for Updates** を実行すれば、いつでも手動による更新ができます。

{% comment %}
* **Expose daemon on tcp://localhost:2375 without TLS** - Click this option to enable legacy clients to connect to the Docker daemon. You must use this option with caution as exposing the daemon without TLS can result in remote code execution attacks.
{% endcomment %}
* **Expose daemon on tcp://localhost:2375 without TLS** - Click this option to enable legacy clients to connect to the Docker daemon. You must use this option with caution as exposing the daemon without TLS can result in remote code execution attacks.

{% comment %}
* **Send usage statistics** - By default, Docker Desktop sends diagnostics,
crash reports, and usage data. This information helps Docker improve and
troubleshoot the application. Clear the check box to opt out. Docker may periodically prompt you for more information.
{% endcomment %}
* **Send usage statistics** - By default, Docker Desktop sends diagnostics,
crash reports, and usage data. This information helps Docker improve and
troubleshoot the application. Clear the check box to opt out. Docker may periodically prompt you for more information.

  {% comment %}
  Click **Switch to the Edge version** to learn more about Docker Desktop Edge releases.
  {% endcomment %}
  Click **Switch to the Edge version** to learn more about Docker Desktop Edge releases.

{% comment %}
### Resources
{% endcomment %}
{: #resources }
### Resources タブ

{% comment %}
The **Resources** tab allows you to configure CPU, memory, disk, proxies,
network, and other resources. Different settings are available for
configuration depending on whether you are using Linux containers in WSL 2
mode, Linux containers in Hyper-V mode, or Windows containers.
{% endcomment %}
**Resources** タブは、CPU、メモリ、ディスク、プロキシー、ネットワークといったリソースを設定します。
動作させているコンテナーが WSL 2 モードでの Linux コンテナーか、Hyper-V モードでの Linux コンテナーか、Windows コンテナーかによって、設定可能な項目は異なります。

{% comment %}
![Resources](images/settings-resources.png){:width="750px"}
{% endcomment %}
![Resources タブ](images/settings-resources.png){:width="750px"}

{% comment %}
#### Advanced
{% endcomment %}
{: #advanced }
#### Advanced タブ

{% comment %}
> **Note**
>
> The Advanced tab is only available in Hyper-V mode, because in WSL 2 mode and
> Windows container mode these resources are managed by Windows. In WSL 2
> mode, you can configure limits on the memory, CPU, and swap size allocated
> to the [WSL 2 utility VM](https://docs.microsoft.com/en-us/windows/wsl/release-notes#build-18945).
{% endcomment %}
> **メモ**
>
> The Advanced tab is only available in Hyper-V mode, because in WSL 2 mode and
> Windows container mode these resources are managed by Windows. In WSL 2
> mode, you can configure limits on the memory, CPU, and swap size allocated
> to the [WSL 2 utility VM](https://docs.microsoft.com/en-us/windows/wsl/release-notes#build-18945).

{% comment %}
Use the **Advanced** tab to limit resources available to Docker.
{% endcomment %}
Advanced タブでは、Docker におけるリソースの利用制限を設定します。

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
allocated from the total available memory on your machine. To increase the RAM, set this to a higher number. To decrease it, lower the number.
{% endcomment %}
**メモリ**  デフォルトにおいて Docker Desktop は、実行時メモリとして `2` GB を利用するものとして設定されています。
この値はマシン上において利用可能な全メモリ容量の中から割り当てられます。
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
> **Note**
>
> The File sharing tab is only available in Hyper-V mode, because in WSL 2 mode
> and Windows container mode all files are automatically shared by Windows.
{% endcomment %}
> **メモ**
>
> The File sharing tab is only available in Hyper-V mode, because in WSL 2 mode
> and Windows container mode all files are automatically shared by Windows.

{% comment %}
Use File sharing to allow local directories on Windows to be shared with Linux containers.
This is especially useful for
editing source code in an IDE on the host while running and testing the code in a container.
Note that configuring file sharing is not necessary for Windows containers, only [Linux containers](#switch-between-windows-and-linux-containers).
 If a directory is not shared with a Linux container you may get `file not found` or `cannot start service` errors at runtime. See [Volume mounting requires shared folders for Linux containers](troubleshoot.md#volume-mounting-requires-shared-folders-for-linux-containers).
{% endcomment %}
File sharing（ファイル共有）を利用すると、Windows 内のローカルディレクトリを Linux コンテナー間で共有できるようになります。
たとえばホストにある IDE 環境上でソースコードを編集し、コードの実行やテストはコンテナー内で行うような場合に、大変便利なものです。
Note that configuring file sharing is not necessary for Windows containers, only [Linux containers](#switch-between-windows-and-linux-containers).
 If a directory is not shared with a Linux container you may get `file not found` or `cannot start service` errors at runtime. See [Volume mounting requires shared folders for Linux containers](troubleshoot.md#volume-mounting-requires-shared-folders-for-linux-containers).

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
> Tips on shared folders, permissions, and volume mounts
>
 * Shared folders are designed to allow application code to be edited on the host while being executed in containers. For non-code items
 such as cache directories or databases, the performance will be much better if they are stored in
 the Linux VM, using a [data volume](../storage/volumes.md)
 (named volume) or [data container](../storage/volumes.md).
>
 * Docker Desktop sets permissions to read/write/execute for users, groups and others [0777 or a+rwx](http://permissions-calculator.org/decode/0777/).
   This is not configurable. See [Permissions errors on data directories for shared volumes](troubleshoot.md#permissions-errors-on-data-directories-for-shared-volumes).
>
 * Windows presents a case-insensitive view of the filesystem to applications while Linux is case-sensitive. On Linux it is possible to create 2 separate files: `test` and `Test`, while on Windows these filenames would actually refer to the same underlying file. This can lead to problems where an app works correctly on a developer Windows machine (where the file contents are shared) but fails when run in Linux in production (where the file contents are distinct). To avoid this, Docker Desktop insists that all shared files are accessed as their original case. Therefore if a file is created called `test`, it must be opened as `test`. Attempts to open `Test` will fail with "No such file or directory". Similarly once a file called `test` is created, attempts to create a second file called `Test` will fail.
{% endcomment %}
> Tips on shared folders, permissions, and volume mounts
>
 * Shared folders are designed to allow application code to be edited on the host while being executed in containers. For non-code items
 such as cache directories or databases, the performance will be much better if they are stored in
 the Linux VM, using a [data volume](../storage/volumes.md)
 (named volume) or [data container](../storage/volumes.md).
>
 * Docker Desktop sets permissions to read/write/execute for users, groups and others [0777 or a+rwx](http://permissions-calculator.org/decode/0777/).
   This is not configurable. See [Permissions errors on data directories for shared volumes](troubleshoot.md#permissions-errors-on-data-directories-for-shared-volumes).
>
 * Windows presents a case-insensitive view of the filesystem to applications while Linux is case-sensitive. On Linux it is possible to create 2 separate files: `test` and `Test`, while on Windows these filenames would actually refer to the same underlying file. This can lead to problems where an app works correctly on a developer Windows machine (where the file contents are shared) but fails when run in Linux in production (where the file contents are distinct). To avoid this, Docker Desktop insists that all shared files are accessed as their original case. Therefore if a file is created called `test`, it must be opened as `test`. Attempts to open `Test` will fail with "No such file or directory". Similarly once a file called `test` is created, attempts to create a second file called `Test` will fail.

{% comment %}
#### Shared folders on demand
{% endcomment %}
#### Shared folders on demand

{% comment %}
You can share a folder "on demand" the first time a particular folder is used by a container.
{% endcomment %}
You can share a folder "on demand" the first time a particular folder is used by a container.

{% comment %}
If you run a Docker command from a shell with a volume mount (as shown in the
example below) or kick off a Compose file that includes volume mounts, you get a
popup asking if you want to share the specified folder.
{% endcomment %}
If you run a Docker command from a shell with a volume mount (as shown in the
example below) or kick off a Compose file that includes volume mounts, you get a
popup asking if you want to share the specified folder.

{% comment %}
You can select to **Share it**, in which case it is added your Docker Desktop Shared Folders list and available to
containers. Alternatively, you can opt not to share it by selecting **Cancel**.
{% endcomment %}
You can select to **Share it**, in which case it is added your Docker Desktop Shared Folders list and available to
containers. Alternatively, you can opt not to share it by selecting **Cancel**.

{% comment %}
![Shared folder on demand](images/shared-folder-on-demand.png){:width="600px"}
{% endcomment %}
![Shared folder on demand](images/shared-folder-on-demand.png){:width="600px"}

{% comment %}
#### Proxies
{% endcomment %}
{: #proxies }
#### Proxies タブ

{% comment %}
Docker Desktop lets you configure HTTP/HTTPS Proxy Settings and
automatically propagates these to Docker. For example, if you set your proxy
settings to `http://proxy.example.com`, Docker uses this proxy when pulling containers.
{% endcomment %}
Docker Desktop lets you configure HTTP/HTTPS Proxy Settings and
automatically propagates these to Docker. For example, if you set your proxy
settings to `http://proxy.example.com`, Docker uses this proxy when pulling containers.

{% comment %}
Your proxy settings, however, will not be propagated into the containers you start.
If you wish to set the proxy settings for your containers, you need to define
environment variables for them, just like you would do on Linux, for example:
{% endcomment %}
しかしそのプロキシー設定は、起動したコンテナーには伝えられません。
コンテナーに対してプロキシー設定を行いたい場合は、Linux 上において行うのと同じように、環境変数を使って定義することが必要です。
たとえば以下のとおりです。

```ps
> docker run -e HTTP_PROXY=http://proxy.example.com:3128 alpine env

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
> **Note**
>
> The Network tab is not available in Windows container mode because networking is
> managed by Windows.
{% endcomment %}
> **メモ**
>
> Network タブは Windows コンテナーモードでは利用できません。
> ネットワークは Windows によって管理されます。

{% comment %}
You can configure Docker Desktop networking to work on a virtual private network (VPN). Specify a network address translation (NAT) prefix and subnet mask to enable Internet connectivity.
{% endcomment %}
Docker Desktop のネットワーク設定により、仮想プライベートネットワーク（VPN）上で動作するように設定することができます。
インターネットへの接続を有効にするには、ネットワークアドレス変換（NAT）のプリフィックスとサブネットマスクを設定していください。

{% comment %}
**DNS Server**: You can configure the DNS server to use dynamic or static IP addressing.
{% endcomment %}
**DNS Server**: You can configure the DNS server to use dynamic or static IP addressing.

{% comment %}
> **Note**
>
> Some users reported problems connecting to Docker Hub on Docker Desktop Stable version. This would manifest as an error when trying to run
`docker` commands that pull images from Docker Hub that are not already
downloaded, such as a first time run of `docker run hello-world`. If you
encounter this, reset the DNS server to use the Google DNS fixed address:
`8.8.8.8`. For more information, see
[Networking issues](troubleshoot.md#networking-issues) in Troubleshooting.
{% endcomment %}
> **Note**
>
> Some users reported problems connecting to Docker Hub on Docker Desktop Stable version. This would manifest as an error when trying to run
`docker` commands that pull images from Docker Hub that are not already
downloaded, such as a first time run of `docker run hello-world`. If you
encounter this, reset the DNS server to use the Google DNS fixed address:
`8.8.8.8`. For more information, see
[Networking issues](troubleshoot.md#networking-issues) in Troubleshooting.

{% comment %}
Updating these settings requires a reconfiguration and reboot of the Linux VM.
{% endcomment %}
Updating these settings requires a reconfiguration and reboot of the Linux VM.

{% comment %}
#### WSL Integration
{% endcomment %}
{: #wsl-integration }
#### WSL との統合

{% comment %}
In WSL 2 mode, you can configure which WSL 2 distributions will have the Docker
WSL integration.
{% endcomment %}
In WSL 2 mode, you can configure which WSL 2 distributions will have the Docker
WSL integration.

{% comment %}
By default, the integration will be enabled on your default WSL distribution.
To change your default WSL distro, run `wsl --set-default <distro name>`. (For example,
to set Ubuntu as your default WSL distro, run `wsl --set-default ubuntu`).
{% endcomment %}
By default, the integration will be enabled on your default WSL distribution.
To change your default WSL distro, run `wsl --set-default <distro name>`. (For example,
to set Ubuntu as your default WSL distro, run `wsl --set-default ubuntu`).

{% comment %}
You can also select any additional distributions you would like to enable the WSL 2
integration on.
{% endcomment %}
You can also select any additional distributions you would like to enable the WSL 2
integration on.

{% comment %}
For more details on configuring Docker Desktop to use WSL 2, see
[Docker Desktop WSL 2 backend](wsl.md).
{% endcomment %}
For more details on configuring Docker Desktop to use WSL 2, see
[Docker Desktop WSL 2 backend](wsl.md).

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

{% comment %}
On both Docker Desktop Edge and Stable releases, you can toggle the experimental features on and off. If you toggle the experimental features off, Docker Desktop uses the current generally available release of Docker Engine.
{% endcomment %}
Docker Desktop の最新版（Edge）と安定版（Stable）の両方において、試験的機能は有効無効を切り替えることができます。
試験的機能を無効にした場合、Docker Desktop は、Docker Engine の安定版を利用することになります。

{% comment %}
{% endcomment %}
#### Experimental features

{% comment %}
{% endcomment %}
Docker Desktop Edge releases have the experimental version
of Docker Engine enabled by default, described in the [Docker Experimental Features README](https://github.com/docker/cli/blob/master/experimental/README.md) on GitHub.

{% include experimental.md %}

{% comment %}
{% endcomment %}
Run `docker version` to verify whether you have enabled experimental features. Experimental mode
is listed under `Server` data. If `Experimental` is `true`, then Docker is
running in experimental mode, as shown here:

```shell
> docker version

Client: Docker Engine - Community
 Version:           19.03.1
 API version:       1.40
 Go version:        go1.12.5
 Git commit:        74b1e89
 Built:             Thu Jul 25 21:17:08 2019
 OS/Arch:           windows/amd64
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
> **Note**
>
> The Kubernetes tab is not available in Windows container mode.
{% endcomment %}
> **メモ**
>
> Kubernetes タブは Windows コンテナーモードでは利用できません。

{% comment %}
Docker Desktop includes a standalone Kubernetes server that runs on your Windows host, so that you can test deploying your Docker workloads on Kubernetes.
{% endcomment %}
Docker Desktop には、Windows ホスト上で稼動するスタンドアロンの Kubernetes サーバーが含まれます。
したがって Kubernetes 上に構築した Docker アプリをデプロイするテストができます。

{% comment %}
![Enable Kubernetes](images/settings-kubernetes.png){:width="750px"}
{% endcomment %}
![Kubernetes の有効化](images/settings-kubernetes.png){:width="750px"}

{% comment %}
The Kubernetes client command, `kubectl`, is included and configured to connect
to the local Kubernetes server. If you have `kubectl` already installed and
pointing to some other environment, such as `minikube` or a GKE cluster, be sure
to change context so that `kubectl` is pointing to `docker-desktop`:
{% endcomment %}
Kubernetes のクライアントコマンドである `kubectl` が提供されていて、ローカルの Kubernetes サーバーへの接続するように設定されています。
`kubectl` をすでにインストールしていて、`minikube` や GKE クラスターといった別の環境に向いている場合は、その内容を変更して、`kubectl` が `docker-desktop` を向くようにしてください。

```bash
> kubectl config get-contexts
> kubectl config use-context docker-desktop
```

 {% comment %}
 To enable Kubernetes support and install a standalone instance of Kubernetes
  running as a Docker container, select **Enable Kubernetes**.
 {% endcomment %}
 Kubernetes サポートを有効にし、Docker コンテナーとして起動するスタンドアロンの Kubernetes インスタンスをインストールするには、**Enable Kubernetes** を選択します。

{% comment %}
To set Kubernetes as the
  [default orchestrator](/docker-for-windows/kubernetes/#override-the-default-orchestrator), select **Deploy Docker Stacks to Kubernetes by default**.
{% endcomment %}
To set Kubernetes as the
  [default orchestrator](/docker-for-windows/kubernetes/#override-the-default-orchestrator), select **Deploy Docker Stacks to Kubernetes by default**.

{% comment %}
By default, Kubernetes containers are hidden from commands like `docker
service ls`, because managing them manually is not supported. To make them
visible, select **Show system containers (advanced)**. Most users do not need this option.
{% endcomment %}
デフォルトで Kubernetes コンテナーは `docker service ls` などのコマンドには現れません。
手動で管理することがサポートされていないためです。
コマンド上に表示させるためには、**Show system containers (advanced)** を選択します。
ただしたいていのユーザーにとって、このオプションは不要です。

{% comment %}
Click **Apply & Restart** to save the settings. This instantiates images required to run the Kubernetes server as containers, and installs the `kubectl.exe` command in the path.
{% endcomment %}
Click **Apply & Restart** to save the settings. This instantiates images required to run the Kubernetes server as containers, and installs the `kubectl.exe` command in the path.

{% comment %}
- When Kubernetes is enabled and running, an additional status bar item displays
at the bottom right of the Docker Desktop Settings dialog. The status of Kubernetes shows in the Docker menu and the context points to
  `docker-desktop`.
{% endcomment %}
- Kubernetes が有効になって稼動していると、Docker Desktop の Settings 画面の下段右側に、新たにステータスバーが表示されます。
  Kubernetes の状態は Docker メニューに表示され、context が `docker-desktop` を指します。

{% comment %}
- To disable Kubernetes support at any time, clear the **Enable Kubernetes** check box.
  The Kubernetes containers are stopped and removed, and the
  `/usr/local/bin/kubectl` command is removed.
{% endcomment %}
- どの時点でも Kubernetes サポートを無効にするには、**Enable Kubernetes** チェックボックスをオフにします。
  Kubernetes コンテナーが停止して削除されます。
  そして `/usr/local/bin/kubectl` コマンドも削除されます。

{% comment %}
- To delete all stacks and Kubernetes resources, select **Reset Kubernetes Cluster**.
{% endcomment %}
- To delete all stacks and Kubernetes resources, select **Reset Kubernetes Cluster**.

{% comment %}
- If you installed `kubectl` by another method, and
experience conflicts, remove it.
{% endcomment %}
- If you installed `kubectl` by another method, and
experience conflicts, remove it.

  {% comment %}
  For more information on using the Kubernetes integration with Docker Desktop, see [Deploy on Kubernetes](kubernetes.md).
  {% endcomment %}
  Docker Desktop における Kubernetes 利用の詳細については [Kubernetes へのデプロイ](kubernetes.md) を参照してください。

{% comment %}
### Reset
{% endcomment %}
### Reset タブ

{% comment %}
The **Restart Docker Desktop** and **Reset to factory defaults** options are now available on the **Troubleshoot** menu. For information, see [Logs and Troubleshooting](troubleshoot.md).
{% endcomment %}
**Restart Docker Desktop**、**Reset to factory defaults**、その他のリセットオプションは、**Troubleshoot** メニューにあります。
詳細は [ログ機能とトラブルシューティング](troubleshoot.md) を参照してください。

{% comment %}
### Troubleshoot
{% endcomment %}
{: #troubleshoot }
### トラブルシューティング

{% comment %}
Visit our [Logs and Troubleshooting](troubleshoot.md) guide for more details.
{% endcomment %}
Visit our [Logs and Troubleshooting](troubleshoot.md) guide for more details.

{% comment %}
Log on to our [Docker Desktop for Windows forum](https://forums.docker.com/c/docker-for-windows) to get help from the community, review current user topics, or join a discussion.
{% endcomment %}
Log on to our [Docker Desktop for Windows forum](https://forums.docker.com/c/docker-for-windows) to get help from the community, review current user topics, or join a discussion.

{% comment %}
Log on to [Docker Desktop for Windows issues on GitHub](https://github.com/docker/for-win/issues) to report bugs or problems and review community reported issues.
{% endcomment %}
Log on to [Docker Desktop for Windows issues on GitHub](https://github.com/docker/for-win/issues) to report bugs or problems and review community reported issues.

{% comment %}
For information about providing feedback on the documentation or update it yourself, see [Contribute to documentation](/opensource/).
{% endcomment %}
For information about providing feedback on the documentation or update it yourself, see [Contribute to documentation](/opensource/).

{% comment %}
## Switch between Windows and Linux containers
{% endcomment %}
## Switch between Windows and Linux containers

{% comment %}
From the Docker Desktop menu, you can toggle which daemon (Linux or Windows)
the Docker CLI talks to. Select **Switch to Windows containers** to use Windows
containers, or select **Switch to Linux containers** to use Linux containers
(the default).
{% endcomment %}
From the Docker Desktop menu, you can toggle which daemon (Linux or Windows)
the Docker CLI talks to. Select **Switch to Windows containers** to use Windows
containers, or select **Switch to Linux containers** to use Linux containers
(the default).

{% comment %}
For more information on Windows containers, refer to the following documentation:
{% endcomment %}
For more information on Windows containers, refer to the following documentation:

{% comment %}
- Microsoft documentation on [Windows containers](https://docs.microsoft.com/en-us/virtualization/windowscontainers/about/index).
{% endcomment %}
- Microsoft documentation on [Windows containers](https://docs.microsoft.com/en-us/virtualization/windowscontainers/about/index).

{% comment %}
- [Build and Run Your First Windows Server Container (Blog Post)](https://blog.docker.com/2016/09/build-your-first-docker-windows-server-container/)
  gives a quick tour of how to build and run native Docker Windows containers on Windows 10 and Windows Server 2016 evaluation releases.
{% endcomment %}
- [Build and Run Your First Windows Server Container (Blog Post)](https://blog.docker.com/2016/09/build-your-first-docker-windows-server-container/)
  gives a quick tour of how to build and run native Docker Windows containers on Windows 10 and Windows Server 2016 evaluation releases.

{% comment %}
- [Getting Started with Windows Containers (Lab)](https://github.com/docker/labs/blob/master/windows/windows-containers/README.md)
  shows you how to use the [MusicStore](https://github.com/aspnet/MusicStore/blob/dev/README.md)
  application with Windows containers. The MusicStore is a standard .NET application and,
  [forked here to use containers](https://github.com/friism/MusicStore), is a good example of a multi-container application.
{% endcomment %}
- [Getting Started with Windows Containers (Lab)](https://github.com/docker/labs/blob/master/windows/windows-containers/README.md)
  shows you how to use the [MusicStore](https://github.com/aspnet/MusicStore/blob/dev/README.md)
  application with Windows containers. The MusicStore is a standard .NET application and,
  [forked here to use containers](https://github.com/friism/MusicStore), is a good example of a multi-container application.

{% comment %}
- To understand how to connect to Windows containers from the local host, see
  [Limitations of Windows containers for `localhost` and published ports](troubleshoot.md#limitations-of-windows-containers-for-localhost-and-published-ports)
{% endcomment %}
- To understand how to connect to Windows containers from the local host, see
  [Limitations of Windows containers for `localhost` and published ports](troubleshoot.md#limitations-of-windows-containers-for-localhost-and-published-ports)

{% comment %}
> Settings dialog changes with Windows containers
>
> When you switch to Windows containers, the Settings dialog only shows those tabs that are active and apply to your Windows containers:
>
{% endcomment %}
> Settings dialog changes with Windows containers
>
> When you switch to Windows containers, the Settings dialog only shows those tabs that are active and apply to your Windows containers:
>

  {% comment %}
  * [General](#general)
  * [Proxies](#proxies)
  * [Daemon](#docker-daemon)
  * [Reset](#reset)
  {% endcomment %}
  * [General タブ](#general)
  * [Proxies タブ](#proxies)
  * [Daemon タブ](#docker-daemon)
  * [Reset タブ](#reset)

{% comment %}
If you set proxies or daemon configuration in Windows containers mode, these
apply only on Windows containers. If you switch back to Linux containers,
proxies and daemon configurations return to what you had set for Linux
containers. Your Windows container settings are retained and become available
again when you switch back.
{% endcomment %}
If you set proxies or daemon configuration in Windows containers mode, these
apply only on Windows containers. If you switch back to Linux containers,
proxies and daemon configurations return to what you had set for Linux
containers. Your Windows container settings are retained and become available
again when you switch back.

{% comment %}
## Dashboard
{% endcomment %}
{: #dashboard }
## Dashboard

{% comment %}
{% endcomment %}
The Docker Desktop Dashboard enables you to interact with containers and applications and manage the lifecycle of your applications directly from your machine. The Dashboard UI shows all running, stopped, and started containers with their state. It provides an intuitive interface to perform common actions to inspect and manage containers and Docker Compose applications. For more information, see [Docker Desktop Dashboard](../desktop/dashboard.md).

{% comment %}
{% endcomment %}
## Docker Hub

{% comment %}
{% endcomment %}
Select **Sign in /Create Docker ID** from the Docker Desktop menu to access your [Docker Hub](https://hub.docker.com/){: target="_blank" class="_" } account. Once logged in, you can access your Docker Hub repositories directly from the Docker Desktop menu.

{% comment %}
{% endcomment %}
For more information, refer to the following [Docker Hub topics](../docker-hub/index.md){: target="_blank" class="_" }:

{% comment %}
{% endcomment %}
* [Organizations and Teams in Docker Hub](../docker-hub/orgs.md){: target="_blank" class="_" }
* [Builds and Images](../docker-hub/builds/index.md){: target="_blank" class="_" }

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
![Docker Desktop 2FA](images/desktop-win-2fa.png){:width="500px"}
{% endcomment %}
![Docker Desktop の 2 要素認証](images/desktop-win-2fa.png){:width="500px"}

{% comment %}
After you have successfully authenticated, you can access your organizations and repositories directly from the Docker Desktop menu.
{% endcomment %}
認証が正常に行われたら、Docker Desktop メニューから組織やリポジトリに直接アクセスできるようになります。

{% comment %}
## Adding TLS certificates
{% endcomment %}
{: #adding-tls-certificates }
## TLS 証明書の追加

{% comment %}
You can add trusted **Certificate Authorities (CAs)** to your Docker daemon to verify registry server
certificates, and **client certificates**, to authenticate to registries. For more information, see [How do I add custom CA certificates?](faqs.md#how-do-i-add-custom-ca-certificates)
and [How do I add client certificates?](faqs.md#how-do-i-add-client-certificates)
in the FAQs.
{% endcomment %}
You can add trusted **Certificate Authorities (CAs)** to your Docker daemon to verify registry server
certificates, and **client certificates**, to authenticate to registries. For more information, see [How do I add custom CA certificates?](faqs.md#how-do-i-add-custom-ca-certificates)
and [How do I add client certificates?](faqs.md#how-do-i-add-client-certificates)
in the FAQs.

{% comment %}
### How do I add custom CA certificates?
{% endcomment %}
### How do I add custom CA certificates?

{% comment %}
Docker Desktop supports all trusted Certificate Authorities (CAs) (root or
intermediate). Docker recognizes certs stored under Trust Root
Certification Authorities or Intermediate Certification Authorities.
{% endcomment %}
Docker Desktop supports all trusted Certificate Authorities (CAs) (root or
intermediate). Docker recognizes certs stored under Trust Root
Certification Authorities or Intermediate Certification Authorities.

{% comment %}
Docker Desktop creates a certificate bundle of all user-trusted CAs based on
the Windows certificate store, and appends it to Moby trusted certificates. Therefore, if an enterprise SSL certificate is trusted by the user on the host, it is trusted by Docker Desktop.
{% endcomment %}
Docker Desktop creates a certificate bundle of all user-trusted CAs based on
the Windows certificate store, and appends it to Moby trusted certificates. Therefore, if an enterprise SSL certificate is trusted by the user on the host, it is trusted by Docker Desktop.

{% comment %}
To learn more about how to install a CA root certificate for the registry, see
[Verify repository client with certificates](../engine/security/certificates.md)
in the Docker Engine topics.
{% endcomment %}
To learn more about how to install a CA root certificate for the registry, see
[Verify repository client with certificates](../engine/security/certificates.md)
in the Docker Engine topics.

{% comment %}
### How do I add client certificates?
{% endcomment %}
### How do I add client certificates?

{% comment %}
You can add your client certificates
in `~/.docker/certs.d/<MyRegistry>:<Port>/client.cert` and
`~/.docker/certs.d/<MyRegistry>:<Port>/client.key`. You do not need to push your certificates with `git` commands.
{% endcomment %}
You can add your client certificates
in `~/.docker/certs.d/<MyRegistry>:<Port>/client.cert` and
`~/.docker/certs.d/<MyRegistry>:<Port>/client.key`. You do not need to push your certificates with `git` commands.

{% comment %}
When the Docker Desktop application starts, it copies the
`~/.docker/certs.d` folder on your Windows system to the `/etc/docker/certs.d`
directory on Moby (the Docker Desktop virtual machine running on Hyper-V).
{% endcomment %}
When the Docker Desktop application starts, it copies the
`~/.docker/certs.d` folder on your Windows system to the `/etc/docker/certs.d`
directory on Moby (the Docker Desktop virtual machine running on Hyper-V).

{% comment %}
You need to restart Docker Desktop after making any changes to the keychain
or to the `~/.docker/certs.d` directory in order for the changes to take effect.
{% endcomment %}
You need to restart Docker Desktop after making any changes to the keychain
or to the `~/.docker/certs.d` directory in order for the changes to take effect.

{% comment %}
The registry cannot be listed as an _insecure registry_ (see
[Docker Daemon](#docker-engine)). Docker Desktop ignores
certificates listed under insecure registries, and does not send client
certificates. Commands like `docker run` that attempt to pull from the registry
produce error messages on the command line, as well as on the registry.
{% endcomment %}
The registry cannot be listed as an _insecure registry_ (see
[Docker Daemon](#docker-engine)). Docker Desktop ignores
certificates listed under insecure registries, and does not send client
certificates. Commands like `docker run` that attempt to pull from the registry
produce error messages on the command line, as well as on the registry.

{% comment %}
To learn more about how to set the client TLS certificate for verification, see
[Verify repository client with certificates](../engine/security/certificates.md)
in the Docker Engine topics.
{% endcomment %}
To learn more about how to set the client TLS certificate for verification, see
[Verify repository client with certificates](../engine/security/certificates.md)
in the Docker Engine topics.

{% comment %}
## Where to go next
{% endcomment %}
{: #where-to-go-next }
## 次に読むものは

{% comment %}
* Try out the walkthrough at [Get Started](../get-started/index.md){: target="_blank" class="_"}.
{% endcomment %}
* [はじめよう](../get-started/index.md){: target="_blank" class="_"} に示されているウォークスルーを試してみてください。

{% comment %}
* Dig in deeper with [Docker Labs](https://github.com/docker/labs/) example walkthroughs and source code.
{% endcomment %}
* [Docker Labs](https://github.com/docker/labs/) にあるウォークスルーやソースコードを参照して、理解を深めてください。

{% comment %}
* Refer to the [Docker CLI Reference Guide](/engine/reference/commandline/cli/){: target="_blank" class="_"}.
{% endcomment %}
* [Docker CLI リファレンスガイド](/engine/reference/commandline/cli/){: target="_blank" class="_"} を参照してください。
