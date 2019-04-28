---
advisory: toolbox
description: Toolbox の概要を説明
keywords: docker, documentation, about, technology, kitematic, gui, toolbox
title: Docker Toolbox 概要
---

<!--
Docker Toolbox is an installer for quick setup and launch of a Docker environment on older Mac and Windows systems that do not meet the requirements of the new [Docker Desktop for Mac](/docker-for-mac/index.md) and [Docker Desktop for Windows](/docker-for-windows/index.md) apps.
-->
Docker Toolbox は Mac や Windows システム向けの Docker インストーラーです。
これにより Docker 環境をすばやくセットアップして起動できるようになります。
ただし Mac や Windows のやや古いバージョン向けのものであって、そのようなシステムでは、新しい [Docker Desktop for Mac](/docker-for-mac/index.md) や [Docker Desktop for Windows](/docker-for-windows/index.md) の利用要件を満たしません。

<!--
![Toolbox installer](images/toolbox-installer.png)
-->
![Toolbox インストーラー](images/toolbox-installer.png)

<!--
## What's in the box
-->
## 中に何があるのか

<!--
Toolbox includes these Docker tools:
-->
Toolbox には以下のような Docker ツールがあります。

<!--
* Docker Machine for running `docker-machine` commands
-->
* コマンド `docker-machine` が実行できる Docker Machine

<!--
* Docker Engine for running the `docker` commands
-->
* コマンド `docker` が実行できる Docker Engine

<!--
* Docker Compose for running the `docker-compose` commands
-->
* コマンド `docker-compose` が実行できる Docker Compose

<!--
* Kitematic, the Docker GUI
-->
* Docker GUI である Kitematic

<!--
* a shell preconfigured for a Docker command-line environment
-->
* Docker コマンドライン環境として用意されているシェル

* Oracle VirtualBox

<!--
You can find various versions of the tools on [Toolbox Releases](https://github.com/docker/toolbox/releases) or run them with the `--version` flag in the terminal, for example, `docker-compose --version`.
-->
[Toolbox リリース](https://github.com/docker/toolbox/releases) には、各種ツールのさまざまなバージョンがあります。
またターミナル画面上から `-version` フラグをつけて実行すればバージョンが分かります。
たとえば `docker-compose --version` などとします。


<!--
## Ready to get started?
-->
## 準備はできている？

<!--
1. Get the latest Toolbox installer for your platform:
-->
1. 各プラットフォーム向けの最新の Toolbox インストーラーを入手します。

    <table style="width:100%">
      <tr>
        <th style="font-size: medium; font-family: arial;  text-align: center">
        Toolbox for Mac</th>
        <th style="font-size: medium; font-family: arial; text-align: center">
        Toolbox for Windows</th>
      </tr>
      <tr valign="top">
        <td width="50%" style="font-size: medium; font-family: arial;  text-align: center">
        <a class="button outline-btn" href="https://download.docker.com/mac/stable/DockerToolbox.pkg">Docker Toolbox for Mac の入手</a>
        </td>
        <td width="50%" style="font-size: medium; font-family: arial;  text-align: center">
        <a class="button outline-btn" href="https://download.docker.com/win/stable/DockerToolbox.exe">Docker Toolbox for Windows の入手</a>
        </td>
      </tr>
    </table>

   <!--
   2. Choose the install instructions for your platform, and follow the steps:
   -->
2. それぞれのプラットフォームに応じたインストール手順を選んで、その手順を進めます。

    <!--
    //* [Install Docker Toolbox on macOS](toolbox_install_mac.md)
    -->
    * [Docker Toolbox on macOS のインストール](toolbox_install_mac.md)

    <!--
    //* [Install Docker Toolbox for Windows](toolbox_install_windows.md)
    -->
    * [Docker Toolbox for Windows のインストール](toolbox_install_windows.md)

<!--
## Next steps
-->
## 次のステップ

<!--
* Try the [Get started](/get-started/) tutorial.
-->
* チュートリアル [「はじめよう」](/get-started/) を試す。

<!--
* Dig in deeper with [more tutorials and examples](/engine/tutorials/index.md) on building images, running containers, networking, managing data, and storing images on Docker Hub.
-->
* [さらなるチュートリアルと例](/engine/tutorials/index.md)を読んで、イメージ構築、コンテナー実行、ネットワーク、データ管理、Docker Hub へのイメージ保存に進む。

<!--
* [Learn about Kitematic](/kitematic/userguide.md)
-->
* [Kitematic について学ぶ](/kitematic/userguide.md)

<!--
* [Learn about Docker Machine](/machine/overview.md)
-->
* [Docker Machine について学ぶ](/machine/overview.md)

<!--
* [Learn about Docker Compose](/compose/overview.md)
-->
* [Docker Compose について学ぶ](/compose/overview.md)
