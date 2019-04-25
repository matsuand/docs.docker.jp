---
title: "はじめよう 1部: 概要とセットアップ"
keywords: get started, setup, orientation, quickstart, intro, concepts, containers
description: Get oriented on some basics of Docker before diving into the walkthrough.
redirect_from:
- /getstarted/
- /get-started/part1/
- /engine/getstarted/
- /learn/
- /engine/getstarted/step_one/
- /engine/getstarted/step_two/
- /engine/getstarted/step_three/
- /engine/getstarted/step_four/
- /engine/getstarted/step_five/
- /engine/getstarted/step_six/
- /engine/getstarted/last_page/
- /engine/getstarted-voting-app/
- /engine/getstarted-voting-app/node-setup/
- /engine/getstarted-voting-app/create-swarm/
- /engine/getstarted-voting-app/deploy-app/
- /engine/getstarted-voting-app/test-drive/
- /engine/getstarted-voting-app/customize-app/
- /engine/getstarted-voting-app/cleanup/
- /engine/userguide/intro/
- /mac/started/
- /windows/started/
- /linux/started/
- /getting-started/
- /mac/step_one/
- /windows/step_one/
- /linux/step_one/
- /engine/tutorials/dockerizing/
- /mac/step_two/
- /windows/step_two/
- /linux/step_two/
- /mac/step_three/
- /windows/step_three/
- /linux/step_three/
- /engine/tutorials/usingdocker/
- /mac/step_four/
- /windows/step_four/
- /linux/step_four/
- /engine/tutorials/dockerimages/
- /userguide/dockerimages/
- /engine/userguide/dockerimages/
- /mac/last_page/
- /windows/last_page/
- /linux/last_page/
- /mac/step_six/
- /windows/step_six/
- /linux/step_six/
- /engine/tutorials/dockerrepos/
- /userguide/dockerrepos/
- /engine/userguide/containers/dockerimages/
---

{% include_relative nav.html selected="1" %}

<!--
Welcome! We are excited that you want to learn Docker. The _Docker Get Started Tutorial_
teaches you how to:
-->
ようこそ！  皆さんが Docker の使い方を学ぼうとしており、私たちは嬉しく思います。
**Docker をはじめよう** では以下のことを学んでいきます。

<!--
1. Set up your Docker environment (on this page)
2. [Build an image and run it as one container](part2.md)
3. [Scale your app to run multiple containers](part3.md)
4. [Distribute your app across a cluster](part4.md)
5. [Stack services by adding a backend database](part5.md)
6. [Deploy your app to production](part6.md)
-->
1. Docker 環境のセットアップ（このページ）
2. [イメージの構築とコンテナーを一つ実行](part2.md)
3. [アプリをスケールアップして複数コンテナーを実行](part3.md)
4. [クラスターにアプリを投入](part4.md)
5. [バックエンドにデータベースを加えてスタックサービス](part5.md)
6. [アプリを本番環境へデプロイ](part6.md)

## Docker の考え方

<!--
Docker is a platform for developers and sysadmins to **develop, deploy, and run**
applications with containers. The use of Linux containers to deploy applications
is called _containerization_. Containers are not new, but their use for easily
deploying applications is.
-->
Docker は開発者やシステム管理者が、コンテナーを使ってアプリケーションを**開発、デプロイ、実行**するためのプラットフォームです。
Linux のコンテナーを利用してアプリケーションをデプロイすることを「コンテナー化」（_containerization_）と呼びます。
コンテナーは新たな技術というものではありませんが、これを使えばアプリケーションを簡単にデプロイできます。

<!--
Containerization is increasingly popular because containers are:
-->
コンテナー化は、よりポピュラーなものになっています。
その理由は以下です。

<!--
- Flexible: Even the most complex applications can be containerized.
- Lightweight: Containers leverage and share the host kernel.
- Interchangeable: You can deploy updates and upgrades on-the-fly.
- Portable: You can build locally, deploy to the cloud, and run anywhere.
- Scalable: You can increase and automatically distribute container replicas.
- Stackable: You can stack services vertically and on-the-fly.
-->
- 柔軟性: より複雑なアプリケーションであるほど、コンテナー化ができます。
- 軽量: コンテナーはホストシステムのカーネルを活用し共有します。
- 交換可能: アップデートやアップグレードがすばやくできます。
- 可搬性: ローカルでビルド、クラウドにデプロイ、どこでも動きます。
- スケーラブル: コンテナーのレプリカを増やしたり自動的に提供することができます。
- スタッカブル: サービスをスケールアップし、すばやく対応できます。

<!--
![Containers are portable](images/laurel-docker-containers.png){:width="100%"}
-->
![コンテナーは可搬性に優れる](images/laurel-docker-containers.png){:width="100%"}

### イメージとコンテナー

<!--
A container is launched by running an image. An **image** is an executable
package that includes everything needed to run an application--the code, a
runtime, libraries, environment variables, and configuration files.
-->
コンテナーというものはイメージを実行することによって動き始めます。
**イメージ** とは、アプリケーションの実行に必要なものをすべて含んだ、実行可能なパッケージのことです。
つまりそこには、コード、ランタイム、ライブラリ、環境変数、設定ファイルをすべて含みます。

<!--
A **container** is a runtime instance of an image--what the image becomes in
memory when executed (that is, an image with state, or a user process). You can
see a list of your running containers with the command, `docker ps`, just as you
would in Linux.
-->
**コンテナー** とは、イメージが実行されたインスタンスのことです。
つまりイメージの実行とともに（状態あるいはユーザープロセスをともなって）メモリにロードされたものです。
実行中のコンテナーの一覧は `docker ps` というコマンドを実行すれば見ることができます。
ちょうど Linux 上で行うコマンド実行と同様です。

### コンテナーと仮想マシン

<!--
A **container** runs _natively_ on Linux and shares the kernel of the host
machine with other containers. It runs a discrete process, taking no more memory
than any other executable, making it lightweight.
-->
**コンテナー**は Linux 上においてネイティブに実行され、他のコンテナーも含めてホストマシン上のカーネルを共有します。
そのプロセスは分離されていて、通常の実行モジュールよりも少ないメモリで動作します。
つまり軽量化されています。

<!--
By contrast, a **virtual machine** (VM) runs a full-blown "guest" operating
system with _virtual_ access to host resources through a hypervisor. In general,
VMs provide an environment with more resources than most applications need.
-->
これとは対照的に**仮想マシン（virtual machine; VM）**は、本格的な"ゲスト"オペレーティングシステムを稼動させ、仮想的なアクセスはハイパーバイザーを通じてホストリソースを操作します。
一般に仮想マシンが提供する環境は、通常のアプリケーションに比べて多くのリソースを必要とします。

![コンテナースタックの例](/images/Container%402x.png){:width="300px"} | ![仮想マシンスタックの例](/images/VM%402x.png){:width="300px"}

## Docker 環境の用意

<!--
Install a [maintained version](/engine/installation/#updates-and-patches){: target="_blank" class="_"}
of Docker Community Edition (CE) or Enterprise Edition (EE) on a
[supported platform](/ee/supported-platforms/){: target="_blank" class="_"}.
-->
Docker コミュニティエディション（Community Edition; CE）または Docker エンタープライズエディション（Enterprise Edition; EE）をインストールします。
[対応するプラットフォーム](/ee/supported-platforms/){: target="_blank" class="_"}および [メンテナンスされているバージョン](/engine/installation/#updates-and-patches){: target="_blank" class="_"}をそれぞれ確認してください。

<!--
> For full Kubernetes Integration
>
> - [Kubernetes on Docker Desktop for Mac](/docker-for-mac/kubernetes/){: target="_blank" class="_"}
is available in [17.12 Edge (mac45)](/docker-for-mac/edge-release-notes/#docker-community-edition-17120-ce-mac45-2018-01-05){: target="_blank" class="_"} or
[17.12 Stable (mac46)](/docker-for-mac/release-notes/#docker-community-edition-17120-ce-mac46-2018-01-09){: target="_blank" class="_"} and higher.
> - [Kubernetes on Docker Desktop for Windows](/docker-for-windows/kubernetes/){: target="_blank" class="_"}
is available in
[18.02 Edge (win50)](/docker-for-windows/edge-release-notes/#docker-community-edition-18020-ce-rc1-win50-2018-01-26){: target="_blank" class="_"} and higher edge channels only.
-->
> Kubernetes 統合について
>
> - [Docker Desktop for Mac における Kubernetes](/docker-for-mac/kubernetes/){: target="_blank" class="_"}
は [17.12 Edge (mac45)](/docker-for-mac/edge-release-notes/#docker-community-edition-17120-ce-mac45-2018-01-05){: target="_blank" class="_"} において、または [17.12 Stable (mac46)](/docker-for-mac/release-notes/#docker-community-edition-17120-ce-mac46-2018-01-09){: target="_blank" class="_"} 以上において利用可能です。
> - [Docker Desktop for Windows における Kubernetes](/docker-for-windows/kubernetes/){: target="_blank" class="_"}
は [18.02 Edge (win50)](/docker-for-windows/edge-release-notes/#docker-community-edition-18020-ce-rc1-win50-2018-01-26){: target="_blank" class="_"} またはそれ以上のエッジチャネルのみで利用可能です。

[Docker のインストール](/engine/installation/index.md){: class="button outline-btn"}
<div style="clear:left"></div>

### Docker バージョンの確認

<!--
1.  Run `docker --version` and ensure that you have a supported version of Docker:
-->
1.  `docker --version` を実行して、サポートされているバージョンの Docker であることを確認します。

    ```shell
    docker --version

    Docker version 17.12.0-ce, build c97c6d6
    ```

<!--
2.  Run `docker info` (or `docker version` without `--`) to view even more details about your Docker installation:
-->
2.  `docker info` （または `--` なしで `docker version`）を実行して、Docker のインストール状況について細かな情報を表示します。

    ```shell
    docker info

    Containers: 0
     Running: 0
     Paused: 0
     Stopped: 0
    Images: 0
    Server Version: 17.12.0-ce
    Storage Driver: overlay2
    ...
    ```

<!--
> To avoid permission errors (and the use of `sudo`), add your user to the `docker` group. [Read more](/engine/installation/linux/linux-postinstall/){: target="_blank" class="_"}.
-->
> パーミッションエラーにならないようにする（`sudo` は使わない場合）には、`docker` グループにユーザーを追加します。
[詳しくはこちら](/engine/installation/linux/linux-postinstall/){: target="_blank" class="_"}。

### Docker インストールの確認

<!--
1.  Test that your installation works by running the simple Docker image,
[hello-world](https://hub.docker.com/_/hello-world/){: target="_blank" class="_"}:
-->
1.  簡単な Docker イメージ [hello-world](https://hub.docker.com/_/hello-world/){: target="_blank" class="_"} を実行させて、インストールが適切にできていることを確認します。

    ```shell
    docker run hello-world

    Unable to find image 'hello-world:latest' locally
    latest: Pulling from library/hello-world
    ca4f61b1923c: Pull complete
    Digest: sha256:ca0eeb6fb05351dfc8759c20733c91def84cb8007aa89a5bf606bc8b315b9fc7
    Status: Downloaded newer image for hello-world:latest

    Hello from Docker!
    This message shows that your installation appears to be working correctly.
    ...
    ```

    <!--
    2.  List the `hello-world` image that was downloaded to your machine:
    -->
2.  マシンにダウンロードされた `hello-world` イメージを表示します。

    ```shell
    docker image ls
    ```

    <!--
    3.  List the `hello-world` container (spawned by the image) which exits after
        displaying its message. If it were still running, you would not need the `--all` option:
    -->
3.  `hello-world` コンテナー（イメージから実行されたもの）を表示します。
    これはメッセージが出力された後には終了します。
    実行されている場合は `--all` オプションをつける必要はありません。

    ```shell
    docker container ls --all

    CONTAINER ID     IMAGE           COMMAND      CREATED            STATUS
    54f4984ed6a8     hello-world     "/hello"     20 seconds ago     Exited (0) 19 seconds ago
    ```

## まとめと早見表

```shell
## Docker CLI のコマンド一覧
docker
docker container --help

## Docker バージョンと情報の出力
docker --version
docker version
docker info

## Docker イメージの実行
docker run hello-world

## Docker イメージの一覧
docker image ls

## Docker コンテナーの一覧（実行中, すべて (-all), quiet モードにあるすべて (-aq)）
docker container ls
docker container ls --all
docker container ls -aq
```

## 1 部を終えて

<!--
Containerization makes [CI/CD](https://www.docker.com/solutions/cicd){: target="_blank" class="_"} seamless. For example:
-->
コンテナー化（containerization）は [CI/CD](https://www.docker.com/solutions/cicd){: target="_blank" class="_"} をシームレスにします。
たとえば以下のとおりです。

<!--
- applications have no system dependencies
- updates can be pushed to any part of a distributed application
- resource density can be optimized.
-->
- アプリケーションはシステム依存性がありません。
- 提供しているアプリケーションのどんな部分に対しても、更新を容易に行うことができます。
- リソースの集約は最適化することができます。

<!--
With Docker, scaling your application is a matter of spinning up new
executables, not running heavy VM hosts.
-->
Docker においてアプリケーションのスケールアップを行うことは、いかに素早く実行モジュールを提供できるかの問題であって、重々しい VM ホストを実行させることではないのです。

[2 部へ >>](part2.md){: class="button outline-btn" style="margin-bottom: 30px; margin-right: 100%"}
