---
description: Docker Desktop for Mac のインストール方法。
keywords: mac, beta, alpha, install, download
title: Docker Desktop for Mac のインストール
---

{% comment %}
To download Docker Desktop for Mac, head to Docker Hub.
{% endcomment %}
Docker Desktop for Mac のダウンロードには Docker Hub を利用します。

{% comment %}
[Download from Docker Hub](https://hub.docker.com/editions/community/docker-ce-desktop-mac){: .button .outline-btn}
{% endcomment %}
[Docker Hub からのダウンロード](https://hub.docker.com/editions/community/docker-ce-desktop-mac){: .button .outline-btn}

{% comment %}
##  What to know before you install
{% endcomment %}
##  インストールの前に
{: #what-to-know-before-you-install }

{% comment %}
> README FIRST for Docker Toolbox and Docker Machine users
>
>If you are already running Docker on your machine, first read
[Docker Desktop for Mac vs. Docker Toolbox](docker-toolbox.md) to understand the
impact of this installation on your existing setup, how to set your environment
for Docker Desktop for Mac, and how the two products can coexist.
{% endcomment %}
> Docker Toolbox と Docker Machine のユーザーはまずはじめに読んでください。
>
>すでにマシン上で Docker を利用している方は、まずはじめに [Docker Desktop for Mac vs. Docker Toolbox](docker-toolbox.md) をよく読んでください。
>そしてすでに Docker があるマシンに新たにインストールするとどうなるのか、Docker Desktop for Mac の環境設定をどうするのか、2 つの製品を並行して利用するにはどうするのか、こういったことを十分に理解してください。

{% comment %}
* **Relationship to Docker Machine**: Installing Docker Desktop for Mac does not affect
  machines you created with Docker Machine. You have the option to copy
  containers and images from your local `default` machine (if one exists) to the
  new Docker Desktop for Mac [HyperKit](https://github.com/docker/HyperKit/) VM. When
  you are running Docker Desktop for Mac, you do not need Docker Machine nodes running
  at all locally (or anywhere else). With Docker Desktop for Mac, you have a new, native
  virtualization system running (HyperKit) which takes the place of the
  VirtualBox system. To learn more, see
  [Docker Desktop for Mac vs. Docker Toolbox](docker-toolbox.md).
{% endcomment %}
* **Docker Machine との関係**: Docker Desktop for Mac をインストールしても、Docker Machine を使って生成していたマシンへの影響はありません。
  You have the option to copy
  containers and images from your local `default` machine (if one exists) to the
  new Docker Desktop for Mac [HyperKit](https://github.com/docker/HyperKit/) VM. When
  you are running Docker Desktop for Mac, you do not need Docker Machine nodes running
  at all locally (or anywhere else). With Docker Desktop for Mac, you have a new, native
  virtualization system running (HyperKit) which takes the place of the
  VirtualBox system. To learn more, see
  [Docker Desktop for Mac vs. Docker Toolbox](docker-toolbox.md).

{% comment %}
* **System Requirements**: Docker Desktop for Mac launches only if all of these
  requirements are met.
{% endcomment %}
* **システム要件**: Docker Desktop for Mac は、以下のシステム要件がすべて満たされている場合にのみ利用することができます。

  {% comment %}
  - Mac hardware must be a 2010 or newer model, with Intel's hardware support for memory
    management unit (MMU) virtualization, including Extended Page Tables (EPT) and
    Unrestricted Mode. You can check to see if your machine has this support by
    running the following command  in a terminal: `sysctl kern.hv_support`
  {% endcomment %}
  - Mac ハードウェアは 2010 かそれより新しいモデルがであり、
    with Intel's hardware support for memory
    management unit (MMU) virtualization, including Extended Page Tables (EPT) and
    Unrestricted Mode. You can check to see if your machine has this support by
    running the following command  in a terminal: `sysctl kern.hv_support`

  {% comment %}
  - macOS Sierra 10.12 and newer macOS releases are supported. We recommend
    upgrading to the latest version of macOS.
  {% endcomment %}
  - macOS Sierra 10.12 and newer macOS releases are supported. We recommend
    upgrading to the latest version of macOS.

  {% comment %}
  - At least 4GB of RAM
  {% endcomment %}
  - RAM 容量は最低でも 4GB

  {% comment %}
  - VirtualBox prior to version 4.3.30 must NOT be installed (it is incompatible
    with Docker Desktop for Mac). If you have a newer version of VirtualBox installed, it's fine.
  {% endcomment %}
  - VirtualBox バージョン 4.3.30 以前はインストールしないでください。
    （Docker Desktop for Mac との互換性がありません。）
    より最新の VirtualBox をインストールするようにしてください。

  {% comment %}
  > **Note**: If your system does not satisfy these requirements, you can
  > install [Docker Toolbox](/toolbox/overview.md), which uses Oracle VirtualBox
  > instead of HyperKit.
  {% endcomment %}
  > **メモ**: 利用しているシステムが上の要件を満たしていない場合は、[Docker Toolbox](/toolbox/overview.md) をインストールすることができます。
  > その場合は HyperKit のかわりに Oracle VirtualBox を利用することになります。

{% comment %}
* **What the install includes**: The installation provides
  [Docker Engine](/engine/userguide/), Docker CLI client,
  [Docker Compose](/compose/overview/), [Docker Machine](/machine/overview/), and [Kitematic](/kitematic/userguide.md).
{% endcomment %}
* **インストールに含まれるもの**: インストールにより以下のものが利用できます。
  [Docker Engine](/engine/userguide/)、Docker CLI クライアント、[Docker Compose](/compose/overview/)、[Docker Machine](/machine/overview/)、[Kitematic](/kitematic/userguide.md)。

{% comment %}
## Install and run Docker Desktop for Mac
{% endcomment %}
## Docker Desktop for Mac のインストールと実行
{: #Install and run Docker Desktop for Mac }

{% comment %}
1.  Double-click `Docker.dmg` to open the installer, then drag Moby the whale to
    the Applications folder.
{% endcomment %}
1.  `Docker.dmg` をダブルクリックしてインストーラーを開きます。
    クジラアイコン（"Moby the whale"）をアプリケーションフォルダーへドラッグします。

      {% comment %}
      ![Install Docker app](images/docker-app-drag.png)
      {% endcomment %}
      ![Docker アプリのインストール](images/docker-app-drag.png)

{% comment %}
2.  Double-click `Docker.app` in the Applications folder to start Docker. (In the example below, the Applications folder is in "grid" view mode.)
{% endcomment %}
2.  アプリケーションフォルダー内の `Docker.app` をダブルクリックして Docker を起動します。
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
      The whale in the top status bar indicates that Docker is running, and accessible from a terminal.
      {% endcomment %}
      The whale in the top status bar indicates that Docker is running, and accessible from a terminal.

      {% comment %}
      ![Whale in menu bar](images/whale-in-menu-bar.png)
      {% endcomment %}
      ![メニューバー内のクジラアイコン](images/whale-in-menu-bar.png)

    {% comment %}
    If you just installed the app, you also get a success message with suggested
    next steps and a link to this documentation. Click the whale (![whale
    menu](images/whale-x.png){: .inline}) in the status bar to
    dismiss this popup.
    {% endcomment %}
    If you just installed the app, you also get a success message with suggested
    next steps and a link to this documentation. Click the whale (![whale
    menu](images/whale-x.png){: .inline}) in the status bar to
    dismiss this popup.

      {% comment %}
      ![Startup information](images/mac-install-success-docker-cloud.png)
      {% endcomment %}
      ![Startup information](images/mac-install-success-docker-cloud.png)

{% comment %}
3.  Click the whale (![whale menu](images/whale-x.png){: .inline}) to get
Preferences and other options.
4.  Select **About Docker** to verify that you have the latest version.
{% endcomment %}
3.  クジラアイコン (![クジラメニュー](images/whale-x.png){: .inline}) をクリックして、設定ファイルやその他のオプションを設定します。
4.  **About Docker** をクリックして、最新版を入手していることを確認します。

{% comment %}
Congratulations! You are up and running with Docker Desktop for Mac.
{% endcomment %}
おめでとうございます！
Docker Desktop for Mac の設定と実行ができました。

{% comment %}
## Where to go next
{% endcomment %}
## 次は何を読むか
{: #where-to-go-next }

{% comment %}
* [Getting started](index.md) provides an overview of Docker Desktop for Mac, basic
  Docker command examples, how to get help or give feedback, and links to all
  topics in the Docker Desktop for Mac guide.
* [Troubleshooting](troubleshoot.md) describes common problems, workarounds, how
  to run and submit diagnostics, and submit issues.
* [FAQs](faqs.md) provides answers to frequently asked questions.
* [Release Notes](release-notes.md) lists component updates, new features, and
  improvements associated with Stable releases (or [Edge Release
  Notes](edge-release-notes.md)).
* [Get Started with Docker](/get-started/) provides a general Docker tutorial.
{% endcomment %}
* [はじめよう](index.md)では Docker Desktop for Mac の概要、基本的な Docker コマンド例、how to get help or give feedback, and links to all
  topics in the Docker Desktop for Mac guide.
* [トラブルシューティング](troubleshoot.md) describes common problems, workarounds, how
  to run and submit diagnostics, and submit issues.
* [FAQ](faqs.md) はよくたずねられる質問とその回答を示します。
* [リリースノート](release-notes.md) lists component updates, new features, and
  improvements associated with Stable releases (or [Edge Release
  Notes](edge-release-notes.md)).
* [Docker をはじめよう](/get-started/)は、Docker の全般的なチュートリアルです。
