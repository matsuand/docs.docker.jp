---
description: Docker Desktop の概要。
keywords: Desktop, Docker, GUI, run, docker, local, machine
title: Docker Desktop 概要
toc_min: 1
toc_max: 2
---

{% comment %}
Docker Desktop is an easy-to-install application for your Mac or Windows environment that enables you to build and share containerized applications and microservices. Docker Desktop includes [Docker Engine](../engine/), Docker CLI client, [Docker Compose](../compose/), [Notary](../notary/getting_started/), [Kubernetes](https://github.com/kubernetes/kubernetes/), and [Credential Helper](https://github.com/docker/docker-credential-helpers/).
{% endcomment %}
Docker Desktop は Mac や Windows に簡単にインストールできるアプリケーションです。
これにより、コンテナー化アプリケーションやマイクロサービスを構築し共有することができます。
Docker Desktop には [Docker Engine](../engine/)、Docker CLI クライアント、[Docker Compose](../compose/)、[Notary](../notary/getting_started/)、[Kubernetes](https://github.com/kubernetes/kubernetes/)、[Credential Helper](https://github.com/docker/docker-credential-helpers/) が含まれます。

{% comment %}
Docker Desktop works with your choice of development tools and languages and gives you access to a vast library of certified images and templates in [Docker Hub](https://hub.docker.com/). This enables development teams to extend their environment to rapidly auto-build, continuously integrate and collaborate using a secure repository.
{% endcomment %}
Docker Desktop は、選択した開発ツールやプログラミング言語を使って動作します。
そして [Docker Hub](https://hub.docker.com/) にある認定イメージやテンプレートに対しての、数多くのライブラリにアクセスできるようになります。
これにより開発チームの環境を充足させ、セキュアなリポジトリを使ってすばやく自動ビルドを行って、継続的な開発や共同開発を行えるようになります。

{% comment %}
Some of the key features of Docker Desktop include:
{% endcomment %}
Docker Desktop の主な特徴としては、以下のものがあります。

{% comment %}
* Ability to containerize and share any application on any cloud platform, in multiple languages and frameworks
* Easy installation and setup of a complete Docker development environment
* Includes the latest version of Kubernetes
* Automatic updates to keep you up to date and secure
* On Windows, the ability to toggle between Linux and Windows Server environments to build applications
* Fast and reliable performance with native Windows Hyper-V virtualization
* Ability to work natively on Linux through WSL 2 on Windows machines
* Volume mounting for code and data, including file change notifications and easy access to running containers on the localhost network
* In-container development and debugging with supported IDEs
{% endcomment %}
* どのようなクラウドプラットフォーム上においても、いろいろなアプリケーションをコンテナー化し共有することができます。複数のプログラミング言語やフレームワークにも対応します。
* 完全な Docker 開発環境を、簡単にインストールし設定することができます。
* 最新版の Kubernetes を提供します。
* 自動アップデートを通じて、最新で安全な開発環境を確保します。
* Windows においは、Linux と Windows Server を切り替えてアプリケーションを構築できます。
* Windows のネイティブな Hyper-V 仮想技術を利用して、高速かつ信頼できる性能を提供します。
* Windows マシン上の WSL 2 を通じて、Linux ネイティブのように機能します。
* ソースコードやデータに対してボリュームマウントを可能にします。
  ファイル更新の通知機能や、ローカルホストのネットワーク上で稼動するコンテナーへの簡単なアクセス機能を提供します。
* サポートされている統合開発環境を使って、コンテナー内での開発とデバッグを実現します。

{% comment %}
## Download and install
{% endcomment %}
{: #download-and-install }
## ダウンロードとインストール

{% comment %}
Docker Desktop is available for Mac and Windows. For download information, system requirements, and installation instructions, see:
{% endcomment %}
Docker Desktop は Mac と Windows において利用できます。
ダウンロード情報、システム要件、インストール手順は、以下を参照してください。

{% comment %}
* [Install Docker Desktop on Mac](../docker-for-mac/install.md)
* [Install Docker Desktop on Windows](../docker-for-windows/install.md)
{% endcomment %}
* [Docker Desktop on Mac のインストール](../docker-for-mac/install.md)
* [Docker Desktop on Windows のインストール](../docker-for-windows/install.md)

{% comment %}
## Get started
{% endcomment %}
{: #get-started }
## はじめよう

{% comment %}
For information on how to get to get started with Docker Desktop and to learn about various UI options and their usage, see:
{% endcomment %}
Docker Desktop をはじめるための情報や、そのさまざまな UI の利用方法を学ぶには、以下を参照してください。

{% comment %}
* [Get started with Docker Desktop on Mac](../docker-for-mac/index.md)
* [Get started with Docker Desktop on Windows](../docker-for-windows/index.md)
{% endcomment %}
* [Docker Desktop on Mac をはじめよう](../docker-for-mac/index.md)
* [Docker Desktop on Windows をはじめよう](../docker-for-windows/index.md)

{% comment %}
## Stable and Edge versions
{% endcomment %}
{: #stable-and-edge-versions }
## 安定版と最新版

{% comment %}
Docker Desktop offers Stable and Edge download channels.
{% endcomment %}
Docker Desktop には安定版（stable）と最新版（edge）のダウンロードチャネルがあります。

{% comment %}
The Stable release provides a general availability release-ready installer for a fully baked and tested, more reliable app. The Stable version of Docker Desktop includes the latest released version of Docker Engine. The release schedule is synced every three months for major releases, with patch releases to fix minor issues, and to stay up to date with Docker Engine as required. You can choose to opt out of the usage statistics and telemetry data on the Stable channel.
{% endcomment %}
安定版（stable）リリースは
The Stable release provides a general availability release-ready installer for a fully baked and tested, more reliable app. The Stable version of Docker Desktop includes the latest released version of Docker Engine. The release schedule is synced every three months for major releases, with patch releases to fix minor issues, and to stay up to date with Docker Engine as required. You can choose to opt out of the usage statistics and telemetry data on the Stable channel.

{% comment %}
Docker Desktop Edge release is our preview version. It offers an installer with the latest features and comes with the experimental features turned on. When using the Edge release, bugs, crashes, and issues can occur as the new features may not be fully tested. However, you get a chance to preview new functionality, experiment, and provide feedback as Docker Desktop evolves. Edge releases are typically more frequent than Stable releases. Telemetry data and usage statistics are sent by default on the Edge version.
{% endcomment %}
Docker Desktop Edge release is our preview version. It offers an installer with the latest features and comes with the experimental features turned on. When using the Edge release, bugs, crashes, and issues can occur as the new features may not be fully tested. However, you get a chance to preview new functionality, experiment, and provide feedback as Docker Desktop evolves. Edge releases are typically more frequent than Stable releases. Telemetry data and usage statistics are sent by default on the Edge version.

{% comment %}
## Release notes
{% endcomment %}
{: #release-notes }
## リリースノート

{% comment %}
For information about new features, improvements, and bug fixes in Docker Desktop Stable releases, see:
{% endcomment %}
Docker Desktop の安定版リリースにおける新機能、修正、バグフィックスについては、以下を参照してください。

{% comment %}
* [Docker Desktop for Mac Stable Release notes](../docker-for-mac/release-notes.md)
* [Docker Desktop for Windows Stable Release notes](../docker-for-windows/release-notes.md)
{% endcomment %}
* [Docker Desktop for Mac 安定版リリースノート](../docker-for-mac/release-notes.md)
* [Docker Desktop for Windows 安定版リリースノート](../docker-for-windows/release-notes.md)

{% comment %}
For information about new features, improvements, and bug fixes in Docker Desktop Edge releases, see:
{% endcomment %}
Docker Desktop の最新版リリースにおける新機能、修正、バグフィックスについては、以下を参照してください。

{% comment %}
* [Docker Desktop for Mac Edge Release notes](../docker-for-mac/edge-release-notes.md)
* [Docker Desktop for Windows Edge Release notes](../docker-for-windows/edge-release-notes.md)
{% endcomment %}
* [Docker Desktop for Mac 最新版リリースノート](../docker-for-mac/edge-release-notes.md)
* [Docker Desktop for Windows 最新版リリースノート](../docker-for-windows/edge-release-notes.md)
