---
description: Docker Desktop WSL 2 backend
keywords: WSL, WSL 2 Tech Preview, Windows Subsystem for Linux, WSL 2 backend Docker
redirect_from:
- /docker-for-windows/wsl-tech-preview/
title: Docker Desktop WSL 2 バックエンド
toc_min: 2
toc_max: 3
---

{% comment %}
Windows Subsystem for Linux (WSL) 2 introduces a significant architectural change as it is a full Linux kernel built by Microsoft, allowing Linux containers to run natively without emulation. With Docker Desktop running on WSL 2, users can leverage Linux workspaces and avoid having to maintain both Linux and Windows build scripts. In addition, WSL 2 provides improvements to file system sharing, boot time, and allows access to some cool new features for Docker Desktop users.
{% endcomment %}
Windows Subsystem for Linux (WSL) 2 は、Microsoft がビルドした完全な Linux カーネルを導入する、画期的な構造変化をもたらします。
これにより Linux コンテナーはエミュレーションとしてではなく、ネイティブな実行が可能になります。
WSL 2 上において Docker Desktop を実行すれば、Linux ワークスペースの活用が可能となり、Linux と Windows の双方に対するビルドスクリプトを準備する必要はなくなります。
また WSL 2 では、ファイルシステム共有や起動時間に関する改善がなされていて、Docker Desktop ユーザーにとって、優れた機能を利用できるものになりました。

{% comment %}
Docker Desktop uses the dynamic memory allocation feature in WSL 2 to greatly improve the resource consumption. This means, Docker Desktop only uses the required amount of CPU and memory resources it needs, while enabling CPU and memory-intensive tasks such as building a container to run much faster.
{% endcomment %}
Docker Desktop では WSL 2 内の動的メモリ割り当て機能を活用して、リソース消費を劇的に改善しています。
つまり Docker Desktop は、求められている CPU やメモリリソースだけを利用するようになったということです。
そしてコンテナーをビルドする処理のように、CPU やメモリへの負荷が高いタスクは、より速く処理実行できます。

{% comment %}
Additionally, with WSL 2, the time required to start a Docker daemon after a cold start is significantly faster. It takes less than 10 seconds to start the Docker daemon when compared to almost a minute in the previous version of Docker Desktop.
{% endcomment %}
さらに、コールドスタート後の Docker デーモン起動にかかる時間は、かなり速くなります。
Docker デーもの起動時間は、従来の Docker Desktop ではおよそ 1 分は要していたものが、今は 10 秒以下に抑えられています。

{% comment %}
## Prerequisites
{% endcomment %}
{: #prerequisites }
## 前提条件

{% comment %}
Before you install the Docker Desktop WSL 2 backend, you must complete the following steps:
{% endcomment %}
Docker Desktop WSL 2 バックエンドをインストールするにあたっては、事前に以下の手順を行っておく必要があります。

{% comment %}
1. Install Windows 10, version 1903 or higher.
2. Enable WSL 2 feature on Windows. For detailed instructions, refer to the [Microsoft documentation](https://docs.microsoft.com/en-us/windows/wsl/install-win10).
3. Download and install the [Linux kernel update package](https://docs.microsoft.com/windows/wsl/wsl2-kernel).
{% endcomment %}
1. Windows 10、バージョン 1903 またはそれ以上をインストールします。
2. Windows 上において WSL 2 機能を有効にします。
   詳しい手順については [Microsoft のドキュメント](https://docs.microsoft.com/en-us/windows/wsl/install-win10) を参照してください。
3. [Linux カーネル更新プログラムパッケージ](https://docs.microsoft.com/windows/wsl/wsl2-kernel) をダウンロードしてインストールします。

{% comment %}
## Download
{% endcomment %}
{: #download }
## ダウンロード

{% comment %}
Download [Docker Desktop Stable 2.3.0.2](https://hub.docker.com/editions/community/docker-ce-desktop-windows/) or a later release.
{% endcomment %}
[Docker Desktop 安定版 2.3.0.2](https://hub.docker.com/editions/community/docker-ce-desktop-windows/) またはこれよりも最新版をダウンロードします。

{% comment %}
## Install
{% endcomment %}
{: #install }
## インストール

{% comment %}
Ensure you have completed the steps described in the Prerequisites section **before** installing the Docker Desktop Stable 2.3.0.2 release.
{% endcomment %}
Docker Desktop 安定版 2.3.0.2 をインストールする **前に**、上の前提条件の節に示した各手順をすべて終えていることを確認してください。

{% comment %}
1. Follow the usual installation instructions to install Docker Desktop. If you are running a supported system, Docker Desktop prompts you to enable WSL 2 during installation. Read the information displayed on the screen and enable WSL 2 to continue.
2. Start Docker Desktop from the Windows Start menu.
3. From the Docker menu, select **Settings** > **General**.
{% endcomment %}
1. Docker Desktop のインストールは、通常のインストール手順に従います。
   サポートされているシステム上では、Docker Desktop のインストールの際に WSL 2 を有効にするかどうかが尋ねられます。
   画面上に示される説明をよく読み、WSL 2 を有効化した上で操作を続けます。
2. Windows スタートメニューから Docker Desktop を起動します。
3. Docker メニューから **Settings** > **General** を実行します。

    {% comment %}
    ![Enable WSL 2](images/wsl2-enable.png){:width="750px"}
    {% endcomment %}
    ![WSL 2 の有効化](images/wsl2-enable.png){:width="750px"}

{% comment %}
4. Select the **Use WSL 2 based engine** check box.
{% endcomment %}
4. チェックボックス **Use WSL 2 based engine** を選択します。

    {% comment %}
    If you have installed Docker Desktop on a system that supports WSL 2, this option will be enabled by default.
    {% endcomment %}
    WSL 2 をサポートするシステム上に Docker Desktop をインストールした場合、このオプションはデフォルトで有効になっています。
{% comment %}
5. Click **Apply & Restart**.
6. Ensure the distribution runs in WSL 2 mode. WSL can run distributions in both v1 or v2 mode.
{% endcomment %}
5. **Apply & Restart** をクリックします。
6. ディストリビューションが WSL 2 モードで動作していることを確認します。
   WSL が実行されるモードには v1 モードと v2 モードがあります。

    {% comment %}
    To check the WSL mode, run:
    {% endcomment %}
    WSL モードを確認するには以下を実行します。

     `wsl.exe -l -v`

    {% comment %}
    To upgrade your existing Linux distro to v2, run:
    {% endcomment %}
    現在の Linux ディストリビューションを v2 にアップグレードするには、以下を実行します。

    {% comment %}
    `wsl.exe --set-version (distro name) 2`
    {% endcomment %}
    `wsl.exe --set-version (ディストリビューション名) 2`

    {% comment %}
    To set v2 as the default version for future installations, run:
    {% endcomment %}
    今後のインストールを考慮して、デフォルトバージョンを v2 に設定しておくには、以下を実行します。

    `wsl.exe --set-default-version 2`

{% comment %}
7. When Docker Desktop restarts, go to **Settings** > **Resources** > **WSL Integration**.
{% endcomment %}
7. Docker Desktop が再起動したら、**Settings** > **Resources** > **WSL Integration** を実行します。

    {% comment %}
    WSL Integration will be enabled on your default WSL distribution. To change your default WSL distro, run `wsl --set-default <distro name>`.
    {% endcomment %}
    WSL 統合環境が、デフォルトの WSL ディストリビューション上において有効になります。
    デフォルトの WSL ディストリビューションを変更するには `wsl --set-default <ディストリビューション名>` を実行します。

    {% comment %}
    For example, to set Ubuntu as your default WSL distro, run `wsl --set-default ubuntu`.
    {% endcomment %}
    たとえばデフォルトの WSL ディストリビューションとして Ubuntu を設定する場合は `wsl --set-default ubuntu` を実行します。

    {% comment %}
    Optionally, select any additional distributions you would like to enable WSL 2 on.
    {% endcomment %}
    他に WSL 2 を有効にしたいディストリビューションがあれば、その設定を行います。

    {% comment %}
    ![WSL 2 Choose Linux distro](images/wsl2-choose-distro.png)
    {% endcomment %}
    ![WSL 2 による Linux ディストリビューションの決定](images/wsl2-choose-distro.png)

{% comment %}
8. Click **Apply & Restart**.
{% endcomment %}
8. **Apply & Restart** をクリックします。

{% comment %}
## Develop with Docker and WSL 2
{% endcomment %}
## Docker と WSL 2 を用いた開発

{% comment %}
The following section describes how to start developing your applications using Docker and WSL 2. We recommend that you have your code in your default Linux distribution for the best development experience using Docker and WSL 2. After you have enabled WSL 2 on Docker Desktop, you can start working with your code inside the Linux distro and ideally with your IDE still in Windows. This workflow can be pretty straightforward if you are using [VSCode](https://code.visualstudio.com/download).
{% endcomment %}
The following section describes how to start developing your applications using Docker and WSL 2. We recommend that you have your code in your default Linux distribution for the best development experience using Docker and WSL 2. After you have enabled WSL 2 on Docker Desktop, you can start working with your code inside the Linux distro and ideally with your IDE still in Windows. This workflow can be pretty straightforward if you are using [VSCode](https://code.visualstudio.com/download).

{% comment %}
1. Open VSCode and install the [Remote - WSL](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl) extension. This extension allows you to work with a remote server in the Linux distro and your IDE client still on Windows.
2. Now, you can start working in VSCode remotely. To do this, open your terminal and type:
{% endcomment %}
1. Open VSCode and install the [Remote - WSL](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl) extension. This extension allows you to work with a remote server in the Linux distro and your IDE client still on Windows.
2. Now, you can start working in VSCode remotely. To do this, open your terminal and type:

    `wsl`

    `code .`

    {% comment %}
    This opens a new VSCode connected remotely to your default Linux distro which you can check in the bottom corner of the screen.
    {% endcomment %}
    This opens a new VSCode connected remotely to your default Linux distro which you can check in the bottom corner of the screen.

    {% comment %}
    Alternatively, you can type the name of your default Linux distro in your Start menu, open it, and then run `code` .
    {% endcomment %}
    Alternatively, you can type the name of your default Linux distro in your Start menu, open it, and then run `code` .
{% comment %}
3. When you are in VSCode, you can use the terminal in VSCode to pull your code and start working natively from your Windows machine.
{% endcomment %}
3. When you are in VSCode, you can use the terminal in VSCode to pull your code and start working natively from your Windows machine.

{% comment %}
## Best practices
{% endcomment %}
{: #best-practices }
## ベストプラクティス

{% comment %}
- To get the best out of the file system performance when bind-mounting files:
    - Store source code and other data that is bind-mounted into Linux containers
      (i.e., with `docker run -v <host-path>:<container-path>`) in the Linux
      filesystem, rather than the Windows filesystem.
    - Linux containers only receive file change events ("inotify events") if the
      original files are stored in the Linux filesystem.
    - Performance is much higher when files are bind-mounted from the Linux
      filesystem, rather than remoted from the Windows host. Therefore avoid
      `docker run -v /mnt/c/users:/users` (where `/mnt/c` is mounted from Windows).
    - Instead, from a Linux shell use a command like `docker run -v ~/my-project:/sources <my-image>`
      where `~` is expanded by the Linux shell to `$HOME`.
- If you have concerns about the size of the docker-desktop-data VHDX, or need to change it, take a look at the [WSL tooling built into Windows](https://docs.microsoft.com/en-us/windows/wsl/wsl2-ux-changes#understanding-wsl-2-uses-a-vhd-and-what-to-do-if-you-reach-its-max-size).
- If you have concerns about CPU or memory usage, you can configure limits on the memory, CPU, Swap size allocated to the [WSL 2 utility VM](https://docs.microsoft.com/en-us/windows/wsl/release-notes#build-18945).
- To avoid any potential conflicts with using WSL 2 on Docker Desktop, you must [uninstall any previous versions of Docker Engine](https://docs.docker.com/install/linux/docker-ce/ubuntu/#uninstall-docker-engine---community) and CLI installed directly through Linux distributions before installing Docker Desktop.
{% endcomment %}
- To get the best out of the file system performance when bind-mounting files:
    - Store source code and other data that is bind-mounted into Linux containers
      (i.e., with `docker run -v <host-path>:<container-path>`) in the Linux
      filesystem, rather than the Windows filesystem.
    - Linux containers only receive file change events ("inotify events") if the
      original files are stored in the Linux filesystem.
    - Performance is much higher when files are bind-mounted from the Linux
      filesystem, rather than remoted from the Windows host. Therefore avoid
      `docker run -v /mnt/c/users:/users` (where `/mnt/c` is mounted from Windows).
    - Instead, from a Linux shell use a command like `docker run -v ~/my-project:/sources <my-image>`
      where `~` is expanded by the Linux shell to `$HOME`.
- If you have concerns about the size of the docker-desktop-data VHDX, or need to change it, take a look at the [WSL tooling built into Windows](https://docs.microsoft.com/en-us/windows/wsl/wsl2-ux-changes#understanding-wsl-2-uses-a-vhd-and-what-to-do-if-you-reach-its-max-size).
- If you have concerns about CPU or memory usage, you can configure limits on the memory, CPU, Swap size allocated to the [WSL 2 utility VM](https://docs.microsoft.com/en-us/windows/wsl/release-notes#build-18945).
- To avoid any potential conflicts with using WSL 2 on Docker Desktop, you must [uninstall any previous versions of Docker Engine](https://docs.docker.com/install/linux/docker-ce/ubuntu/#uninstall-docker-engine---community) and CLI installed directly through Linux distributions before installing Docker Desktop.

{% comment %}
## Feedback
{% endcomment %}
{: #feedback }
## フィードバック

{% comment %}
Your feedback is very important to us. Please let us know your feedback by creating an issue in the [Docker Desktop for Windows GitHub](https://github.com/docker/for-win/issues) repository and adding the **WSL 2** label.
{% endcomment %}
Your feedback is very important to us. Please let us know your feedback by creating an issue in the [Docker Desktop for Windows GitHub](https://github.com/docker/for-win/issues) repository and adding the **WSL 2** label.
