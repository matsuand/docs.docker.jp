---
description: Introduction and Overview of Compose
keywords: documentation, docs, docker, compose, orchestration, containers
title: Docker Compose 概要
---

<!--
>**Looking for Compose file reference?** [Find the latest version here](/compose/compose-file/index.md).
-->
>**Compose ファイルリファレンスをお探しですか？**
>
その場合は[最新版がこちら](/compose/compose-file/index.md)。

<!--
Compose is a tool for defining and running multi-container Docker applications.
With Compose, you use a YAML file to configure your application's services.
Then, with a single command, you create and start all the services
from your configuration. To learn more about all the features of Compose,
see [the list of features](overview.md#features).
-->
Compose とは、複数のコンテナーを定義し実行する Docker アプリケーションのためのツールです。
Compose においては YAML ファイルを使ってアプリケーションサービスの設定を行います。
コマンドを 1 つ実行するだけで、設定内容に基づいたアプリケーションサービスの生成、起動を行います。
Compose の機能一覧については、[機能一覧](overview.md#features)をご覧ください。

<!--
Compose works in all environments: production, staging, development, testing, as
well as CI workflows. You can learn more about each case in [Common Use
Cases](overview.md#common-use-cases).
-->
Compose は本番環境、ステージング環境、開発環境において動作し、CI ワークフローとしても利用することができます。
それぞれの使い方については、[一般的な利用例](overview.md#common-use-cases)を確認してください。

<!--
Using Compose is basically a three-step process:
-->
Compose を使うには、基本的に 3 つのステップを踏みます。

<!--
1. Define your app's environment with a `Dockerfile` so it can be reproduced
anywhere.
-->
1. アプリケーション環境を `Dockerfile` に定義します。
   これによりその環境は再構築が可能となります。

   <!--
   2. Define the services that make up your app in `docker-compose.yml`
   so they can be run together in an isolated environment.
   -->
2. アプリケーションを構成するサービスを `docker-compose.yml` ファイル内に定義します。
   各サービスは独立した環境において起動することになります。

   <!--
   3. Run `docker-compose up` and Compose starts and runs your entire app.
   -->
3. 最後に `docker-compose up` を実行したら、Compose はアプリケーション全体を起動、実行します。

<!--
A `docker-compose.yml` looks like this:
-->
`docker-compose.yml` は次のように記述します。

    version: '3'
    services:
      web:
        build: .
        ports:
        - "5000:5000"
        volumes:
        - .:/code
        - logvolume01:/var/log
        links:
        - redis
      redis:
        image: redis
    volumes:
      logvolume01: {}

<!--
For more information about the Compose file, see the
[Compose file reference](compose-file/index.md).
-->
Compose ファイルに関するさらに詳しい情報は、[Compose ファイルリファレンス](compose-file/index.md)をご覧ください。

<!--
Compose has commands for managing the whole lifecycle of your application:
-->
Compose には、アプリケーションのライフサイクルを管理するコマンドがあります。

<!--
 * Start, stop, and rebuild services
 * View the status of running services
 * Stream the log output of running services
 * Run a one-off command on a service
-->
* サービスの開始、停止、再構築
* 実行中のサービスの状態を表示
* 実行中のサービスのストリームログ出力
* サービス上で１回限りのコマンドを実行

<!--
## Compose documentation
-->
## Compose のドキュメント

<!--
- [Installing Compose](install.md)
- [Getting Started](gettingstarted.md)
- [Get started with Django](django.md)
- [Get started with Rails](rails.md)
- [Get started with WordPress](wordpress.md)
- [Frequently asked questions](faq.md)
- [Command line reference](./reference/index.md)
- [Compose file reference](compose-file/index.md)
-->
- [Compose のインストール](install.md)
- [Compose をはじめよう](gettingstarted.md)
- [Django とともにはじめる](django.md)
- [Rails とともにはじめる](rails.md)
- [WordPress とともにはじめる](wordpress.md)
- [よくたずねられる質問](faq.md)
- [コマンドラインインターフェース](./reference/index.md)
- [Compose ファイルリファレンス](compose-file/index.md)

<!--
## Features
-->
## 機能

<!--
The features of Compose that make it effective are:
-->
Compose には特徴的な以下の機能があります。

* [1 つのホスト上で分離された環境を複数実現](overview.md#Multiple-isolated-environments-on-a-single-host)
* [コンテナー生成時はボリュームデータを維持](overview.md#preserve-volume-data-when-containers-are-created)
* [変更のあったコンテナーのみ再作成](overview.md#only-recreate-containers-that-have-changed)
* [Variables and moving a composition between environments](overview.md#variables-and-moving-a-composition-between-environments)

<!--
### Multiple isolated environments on a single host
-->
<a name="Multiple-isolated-environments-on-a-single-host"></a>
### 1 つのホスト上で分離された環境を複数実現

<!--
Compose uses a project name to isolate environments from each other. You can make use of this project name in several different contexts:
-->
Compose はプロジェクト名というものを用いて各環境を分離します。
このプロジェクト名はさまざまに異なる用途に利用することができます。

<!--
* on a dev host, to create multiple copies of a single environment, such as when you want to run a stable copy for each feature branch of a project
* on a CI server, to keep builds from interfering with each other, you can set
  the project name to a unique build number
* on a shared host or dev host, to prevent different projects, which may use the
  same service names, from interfering with each other
-->
* 開発ホスト上では 1 つの環境に対して複数のコピー作成に使います（例：プロジェクトの機能ブランチごとに、安定版のコピーを実行したい場合）。
* CI サーバー上では、お互いのビルドが干渉しないようにするため、プロジェクト名にユニークなビルド番号をセットできます。
* 共有ホストまたは開発ホスト上では、異なるプロジェクトが同じサービス名を使わないようにし、お互いを干渉しないようにします。

<!--
The default project name is the basename of the project directory. You can set
a custom project name by using the
[`-p` command line option](./reference/overview.md) or the
[`COMPOSE_PROJECT_NAME` environment variable](./reference/envvars.md#compose-project-name).
-->
プロジェクト名はデフォルトでは、プロジェクトが存在するディレクトリ名となります。
プロジェクト名を指定するには、[コマンドラインオプション](./reference/overview.md)の `-p` を指定するか、[環境変数 `COMPOSE_PROJECT_NAME`](./reference/envvars.md#compose-project-name) を使って指定します。

<!--
### Preserve volume data when containers are created
-->
<a name="preserve-volume-data-when-containers-are-created"></a>
### コンテナー生成時はボリュームデータを維持

<!--
Compose preserves all volumes used by your services. When `docker-compose up`
runs, if it finds any containers from previous runs, it copies the volumes from
the old container to the new container. This process ensures that any data
you've created in volumes isn't lost.
-->
Compose は、サービスによって利用されているボリュームをすべて維持します。
`docker-compose up` が実行されたときに、コンテナーがそれ以前に実行されていれば、以前のコンテナーから現在のコンテナーに向けてボリュームをコピーします。
この処理において、ボリューム内に作り出されていたデータは失われることはありません。

<!--
If you use `docker-compose` on a Windows machine, see
[Environment variables](reference/envvars.md) and adjust the necessary environment
variables for your specific needs.
-->
Windows 上において `docker-compose` を利用している場合には、[環境変数のページ](reference/envvars.md)を参考にし、状況に応じて必要となる環境変数を定めてください。


<!--
### Only recreate containers that have changed
-->
<a name="only-recreate-containers-that-have-changed"></a>
### 変更のあったコンテナーのみ再作成

<!--
Compose caches the configuration used to create a container. When you
restart a service that has not changed, Compose re-uses the existing
containers. Re-using containers means that you can make changes to your
environment very quickly.
-->
Compose はコンテナーが生成されたときの設定情報をキャッシュに保存します。
設定内容に変更のないサービスが再起動された場合、Compose はすでにあるコンテナーを再利用します。
再利用されるということは、全体として環境への変更がすばやくできることを意味します。


<!--
### Variables and moving a composition between environments
-->
<a name="variables-and-moving-a-composition-between-environments"></a>
### 変数と環境間の移行

<!--
Compose supports variables in the Compose file. You can use these variables
to customize your composition for different environments, or different users.
See [Variable substitution](compose-file.md#variable-substitution) for more
details.
-->
Compose は Compose ファイル内での変数の利用をサポートしています。
環境変数を使い、さまざまな環境、さまざまなユーザー向けに構成をカスタマイズできます。
詳細は[環境変数のページ](compose-file.md#variable-substitution)をご覧ください。

<!--
You can extend a Compose file using the `extends` field or by creating multiple
Compose files. See [extends](extends.md) for more details.
-->
Compose ファイルは `extends` フィールドを使うことで、複数の Compose ファイルを作成できるように拡張できます。
[extends](extends.md) をご覧ください。


<!--
## Common use cases
-->
<a name="common-use-cases"></a>
## 一般的な利用例

<!--
Compose can be used in many different ways. Some common use cases are outlined
below.
-->
Compose はさまざまな使い方があります。
一般的な利用例は、以下のとおりです。

<!--
### Development environments
-->
### 開発環境

<!--
When you're developing software, the ability to run an application in an
isolated environment and interact with it is crucial. The Compose command
line tool can be used to create the environment and interact with it.
-->
ソフトウェアを開発する上で、アプリケーションを分離された環境内にて実行させ、しかも正しくアクセスできるようにすることが極めて重要です。
Compose のコマンドラインツールを用いることで、環境生成と環境へのアクセスを行うことができます。

<!--
The [Compose file](compose-file.md) provides a way to document and configure
all of the application's service dependencies (databases, queues, caches,
web service APIs, etc). Using the Compose command line tool you can create
and start one or more containers for each dependency with a single command
(`docker-compose up`).
-->
[Compose ファイル](compose-file.md)は、アプリケーションにおけるサービスの依存関係（データベース、キュー、キャッシュ、ウェブ・サービス API など）を設定するものです。
Compose コマンドラインツールを使うと、いくつでもコンテナーを生成、起動でき、しかもコマンド（`docker-compose up`）を１つ実行するだけで、依存関係も正しく考慮してくれます。

<!--
Together, these features provide a convenient way for developers to get
started on a project. Compose can reduce a multi-page "developer getting
started guide" to a single machine readable Compose file and a few commands.
-->
さらにこういった機能は、プロジェクトに取りかかろうとしている開発者にとっても便利なものです。
Compose は、分厚く仕上がっている「開発者向け導入手順書」のページ数を減らすものになり、ただ 1 つの Compose ファイルと数えるほどのコマンドだけになります。

<!--
### Automated testing environments
-->
### 自動テスト環境

<!--
An important part of any Continuous Deployment or Continuous Integration process
is the automated test suite. Automated end-to-end testing requires an
environment in which to run tests. Compose provides a convenient way to create
and destroy isolated testing environments for your test suite. By defining the full environment in a [Compose file](compose-file.md), you can create and destroy these environments in just a few commands:
-->
継続的デプロイや継続的インテグレーションのプロセスにおいて、自動テストスイートは極めて重要です。
もれることなくテストを自動化させるためには、そのためのテスト環境が必要になるものです。
Compose ではテストスイートに対応して、分離されたテスト環境の生成とデプロイを便利に行う機能を提供しています。
[Compose ファイル](compose-file.md)内に必要な環境定義を行っておけば、テスト環境の生成と削除は、ごく簡単なコマンドだけで実現できます。

    $ docker-compose up -d
    $ ./run_tests
    $ docker-compose down

<!--
### Single host deployments
-->
### ただ 1 つのホストからのデプロイ

<!--
Compose has traditionally been focused on development and testing workflows,
but with each release we're making progress on more production-oriented features. You can use Compose to deploy to a remote Docker Engine. The Docker Engine may be a single instance provisioned with
[Docker Machine](/machine/overview.md) or an entire
[Docker Swarm](/engine/swarm/index.md) cluster.
-->
Compose はこれまで、開発環境やテスト環境でのワークフローに注目してきました。
しかしリリースを重ねるにつれて、本番環境を意識した機能を充実させるように進化しています。
Compose はリモートにある Docker Engine に対してもデプロイすることができます。
Docker Engine とは、[Docker Machine](/machine/overview.md) で提供される単一インスタンスであったり、[Docker Swarm](/engine/swarm/index.md) クラスター一式である場合もあります。

<!--
For details on using production-oriented features, see
[compose in production](production.md) in this documentation.
-->
本番環境向け機能の使い方については、[本番環境における Compose](production.md) をご覧ください。


<!--
## Release notes
-->
## リリースノート

<!--
To see a detailed list of changes for past and current releases of Docker
Compose, refer to the
[CHANGELOG](https://github.com/docker/compose/blob/master/CHANGELOG.md).
-->
Docker Compose の過去から現在に至るまでの詳細な変更一覧は、[CHANGELOG](https://github.com/docker/compose/blob/master/CHANGELOG.md) をご覧ください。

<!--
## Getting help
-->
## ヘルプを得るには

<!--
Docker Compose is under active development. If you need help, would like to
contribute, or simply want to talk about the project with like-minded
individuals, we have a number of open channels for communication.
-->
Docker Compose は活発に開発中です。
ヘルプが必要な場合、貢献したい場合、あるいはプロジェクトの仲間と話をしたい場合、コミュニケーションができるオープンチャネルがたくさんあｒます。

<!--
* To report bugs or file feature requests: use the [issue tracker on Github](https://github.com/docker/compose/issues).
-->
* バグ報告や機能リクエストは [GitHub の issue トラッカー](https://github.com/docker/compose/issues)をご利用ください。

<!--
* To talk about the project with people in real time: join the
  `#docker-compose` channel on freenode IRC.
-->
* プロジェクトのメンバーとリアルタイムに会話したければ、IRC の `#docker-compose` チャネルに参加してください。

<!--
* To contribute code or documentation changes: submit a [pull request on Github](https://github.com/docker/compose/pulls).
-->
* コードやドキュメントの変更に貢献したい場合は、[GitHub にプルリクエスト](https://github.com/docker/compose/pulls)を送ってください。

<!--
For more information and resources, visit the [Getting Help project page](/opensource/get-help/).
-->
より詳細な情報やリソースについては、[ヘルプ用ページ](https://docs.docker.com/project/get-help/) を参照してください。
