---
description: Docker Desktop on Mac のインストール方法。
keywords: mac, beta, alpha, install, download
title: Docker Desktop on Mac のインストール
---

  {% comment %}
  To download Docker Desktop, go to Docker Hub and sign in with your Docker ID.
  {% endcomment %}
  Docker Desktop のダウンロードには Docker Hub へアクセスし、Docker ID によりサインインします。

{% comment %}
[Download from Docker Hub](https://hub.docker.com/?overlay=onboarding){: .button .outline-btn}
{% endcomment %}
[Docker Hub からのダウンロード](https://hub.docker.com/?overlay=onboarding){: .button .outline-btn}

{% comment %}
## What to know before you install
{% endcomment %}
{: #what-to-know-before-you-install }
## インストールの前に

{% comment %}
> README FIRST for Docker Toolbox and Docker Machine users
>
>If you are already running Docker on your machine, first read
[Docker Desktop for Mac vs. Docker Toolbox](docker-toolbox.md) to understand the
impact of this installation on your existing setup, how to set your environment
for Docker Desktop on Mac, and how the two products can coexist.
{% endcomment %}
> Docker Toolbox と Docker Machine のユーザーはまずはじめに読んでください。
>
>すでにマシン上で Docker を利用している方は、まずはじめに [Docker Desktop for Mac vs. Docker Toolbox](docker-toolbox.md) をよく読んでください。
>そしてすでに Docker があるマシンに新たにインストールするとどうなるのか、Docker Desktop on Mac の環境設定をどうするのか、2 つの製品を並行して利用するにはどうするのか、こういったことを十分に理解してください。

{% comment %}
**Relationship to Docker Machine**: Installing Docker Desktop on Mac does not affect machines you created with Docker Machine. You have the option to copy containers and images from your local `default` machine (if one exists) to the Docker Desktop [HyperKit](https://github.com/docker/HyperKit/) VM. When
you are running Docker Desktop, you do not need Docker Machine nodes running locally (or anywhere else). With Docker Desktop, you have a new, native
virtualization system running (HyperKit) which takes the place of the
VirtualBox system. To learn more, see [Docker Desktop for Mac vs. Docker Toolbox](docker-toolbox.md).
{% endcomment %}
**Docker Machine との関係**: Docker Desktop on Mac をインストールしても、Docker Machine を使って生成していたマシンへの影響はありません。You have the option to copy containers and images from your local `default` machine (if one exists) to the Docker Desktop [HyperKit](https://github.com/docker/HyperKit/) VM. When
you are running Docker Desktop, you do not need Docker Machine nodes running locally (or anywhere else). With Docker Desktop, you have a new, native
virtualization system running (HyperKit) which takes the place of the
VirtualBox system. To learn more, see [Docker Desktop for Mac vs. Docker Toolbox](docker-toolbox.md).

{% comment %}
## System requirements
{% endcomment %}
{: #system-requirements }
## システム要件

{% comment %}
Your Mac must meet the following requirements to successfully install Docker Desktop:
{% endcomment %}
Docker Desktop を Mac に正常にインストールするには、以下のシステム要件が満たされている必要があります。

{% comment %}
- Mac hardware must be a 2010 or newer model, with Intel’s hardware support for memory management unit (MMU) virtualization, including Extended Page Tables (EPT) and Unrestricted Mode. You can check to see if your machine has this support by running the following command in a terminal: `sysctl kern.hv_support`
{% endcomment %}
- Mac hardware must be a 2010 or newer model, with Intel’s hardware support for memory management unit (MMU) virtualization, including Extended Page Tables (EPT) and Unrestricted Mode. You can check to see if your machine has this support by running the following command in a terminal: `sysctl kern.hv_support`

{% comment %}
- macOS must be version 10.13 or newer. We recommend upgrading to the latest version of macOS.
{% endcomment %}
- macOS must be version 10.13 or newer. We recommend upgrading to the latest version of macOS.

  If you experience any issues after upgrading your macOS to version 10.15, you must install the latest version of Docker Desktop to be compatible with this version of macOS.

  {% comment %}
  {% endcomment %}
  **Note:** Docker supports Docker Desktop on the most recent versions of macOS. That is, the current release of macOS and the previous two releases. As new major versions of macOS are made generally available, Docker will stop supporting the oldest version and support the newest version of macOS (in addition to the previous two releases).

{% comment %}
- At least 4 GB of RAM.
{% endcomment %}
- RAM 容量は最低でも 4GB

{% comment %}
- VirtualBox prior to version 4.3.30 must not be installed as it is not compatible with Docker Desktop.
{% endcomment %}
- VirtualBox バージョン 4.3.30 以前はインストールしないでください。
  Docker Desktop との互換性がありません。

{% comment %}
**Note**: If your system does not satisfy these requirements, you can install [Docker Toolbox](/toolbox/overview.md), which uses Oracle VirtualBox instead of HyperKit.
{% endcomment %}
**メモ**: 利用しているシステムが上の要件を満たしていない場合は、[Docker Toolbox](/toolbox/overview.md) をインストールすることができます。
その場合は HyperKit のかわりに Oracle VirtualBox を利用することになります。

{% comment %}
## What's included in the installer
{% endcomment %}
## What's included in the installer

{% comment %}
The Docker Desktop installation includes
  [Docker Engine](/engine/userguide/), Docker CLI client,
  [Docker Compose](/compose/), [Docker Machine](/machine/overview/), and [Kitematic](/kitematic/userguide.md).
{% endcomment %}
* Docker Desktop のインストールにより以下のものが利用できます。
  [Docker Engine](/engine/userguide/)、Docker CLI クライアント、[Docker Compose](/compose/)、[Docker Machine](/machine/overview/)、[Kitematic](/kitematic/userguide.md)。

{% comment %}
## Install and run Docker Desktop on Mac
{% endcomment %}
{: #install-and-run-docker-desktop-on-Mac }
## Docker Desktop on Mac のインストールと実行

{% comment %}
1. Double-click `Docker.dmg` to open the installer, then drag the Docker icon to
    the Applications folder.
{% endcomment %}
1. `Docker.dmg` をダブルクリックしてインストーラーを開きます。
    Docker アイコンをアプリケーションフォルダーへドラッグします。

      {% comment %}
      ![Install Docker app](images/docker-app-drag.png)
      {% endcomment %}
      ![Docker アプリのインストール](images/docker-app-drag.png)

{% comment %}
2. Double-click `Docker.app` in the Applications folder to start Docker. (In the example below, the Applications folder is in "grid" view mode.)
{% endcomment %}
2. アプリケーションフォルダー内の `Docker.app` をダブルクリックして Docker を起動します。
    （以下の例において、アプリケーションフォルダーは「グリッド」表示モードにしています。）

    {% comment %}
    ![Docker app in Hockeyapp](images/docker-app-in-apps.png)
    {% endcomment %}
    ![Hockeyapp 内の Docker](images/docker-app-in-apps.png)

    {% comment %}
    You are prompted to authorize `Docker.app` with your system password after you launch it.
    Privileged access is needed to install networking components and links to the Docker apps.
    {% endcomment %}
    You are prompted to authorize `Docker.app` with your system password after you launch it.
    Privileged access is needed to install networking components and links to the Docker apps.

    {% comment %}
    The Docker menu in the top status bar indicates that Docker Desktop is running, and accessible from a terminal.
    {% endcomment %}
    The Docker menu in the top status bar indicates that Docker Desktop is running, and accessible from a terminal.

      {% comment %}
      ![Whale in menu bar](images/whale-in-menu-bar.png)
      {% endcomment %}
      ![メニューバー内のクジラアイコン](images/whale-in-menu-bar.png)

    {% comment %}
    If you just installed the app, you also get a message with suggested
    next steps and a link to the documentation. Click the Docker menu (![whale
    menu](images/whale-x.png){: .inline}) in the status bar to
    dismiss this pop-up notification.
    {% endcomment %}
    If you just installed the app, you also get a message with suggested
    next steps and a link to the documentation. Click the Docker menu (![whale
    menu](images/whale-x.png){: .inline}) in the status bar to
    dismiss this pop-up notification.

      {% comment %}
      ![Startup information](images/mac-install-success.png)
      {% endcomment %}
      ![Startup information](images/mac-install-success.png)

{% comment %}
3. Click the Docker menu (![whale menu](images/whale-x.png){: .inline}) to see
**Preferences** and other options.
{% endcomment %}
3.  Docker メニュー (![クジラメニュー](images/whale-x.png){: .inline}) をクリックして、**Preferences** やその他のオプションを確認します。

{% comment %}
4. Select **About Docker** to verify that you have the latest version.
{% endcomment %}
4.  **About Docker** をクリックして、最新版を入手していることを確認します。

{% comment %}
Congratulations! You are now successfully running Docker Desktop.
{% endcomment %}
おめでとうございます！
Docker Desktop を正常に実行することができました。

{% comment %}
## Where to go next
{% endcomment %}
## 次は何を読むか
{: #where-to-go-next }

{% comment %}
- [Getting started](index.md) provides an overview of Docker Desktop on Mac, basic Docker command examples, how to get help or give feedback, and links to other topics about Docker Desktop on Mac.
- [Troubleshooting](troubleshoot.md) describes common problems, workarounds, how
  to run and submit diagnostics, and submit issues.
- [FAQs](faqs.md) provide answers to frequently asked questions.
- [Release notes](release-notes.md) lists component updates, new features, and
  improvements associated with Stable releases. For information about Edge releases, see [Edge release
  notes](edge-release-notes.md).
- [Get started with Docker](/get-started/) provides a general Docker tutorial.
{% endcomment %}
* [はじめよう](index.md)では Docker Desktop on Mac の概要、基本的な Docker コマンド例、how to get help or give feedback, and links to other topics about Docker Desktop on Mac.
* [トラブルシューティング](troubleshoot.md) describes common problems, workarounds, how
  to run and submit diagnostics, and submit issues.
* [FAQ](faqs.md) はよくたずねられる質問とその回答を示します。
* [リリースノート](release-notes.md) lists component updates, new features, and
  improvements associated with Stable releases. For information about Edge releases, see [Edge release
  notes](edge-release-notes.md).
* [Docker をはじめよう](/get-started/) は Docker の全般的なチュートリアルです。
