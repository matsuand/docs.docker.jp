---
description: Engine
keywords: Engine
redirect_from:
- /engine/misc/
title: Docker Engine について
---

{% comment %}
**Develop, Ship and Run Any Application, Anywhere**
{% endcomment %}
**あらゆるアプリケーションがどこでも開発、導入、実行できる**

{% comment %}
[**Docker**](https://www.docker.com) is a platform for developers and sysadmins
to develop, ship, and run applications.  Docker lets you quickly assemble
applications from components and eliminates the friction that can come when
shipping code. Docker lets you get your code tested and deployed into production
as fast as possible.
{% endcomment %}
[**Docker**](https://www.docker.com/) とは、開発者やシステム管理者がアプリケーションを開発、導入、実行するためのプラットフォームです。
Docker を使えば、アプリケーションをコンポーネントからすばやく組み立てることができ、コード導入時に発生するコード間の相違を軽減できます。
Docker はテストや本番投入も迅速に実現します。

{% comment %}
Docker consists of:
{% endcomment %}
Docker は以下によって構成されます。

{% comment %}
* The Docker Engine - our lightweight and powerful open source containerization
  technology combined with a work flow for building and containerizing your
  applications.
* [Docker Hub](https://hub.docker.com) - our SaaS service for
  sharing and managing your application stacks.
{% endcomment %}
* Docker Engine … 軽量かつ強力なオープンソースによりコンテナー化（containerization）を行う技術。
  アプリケーションの構築とコンテナー化を行うワークフローを実現します。
* [Docker Hub](https://hub.docker.com) … アプリケーション層を共有し管理するための Saas サービス。

{% comment %}
## Why Docker?
{% endcomment %}
## なぜ Docker なのか？
{: #why-docker }

{% comment %}
*Faster delivery of your applications*
{% endcomment %}
*迅速なアプリケーション配信*

{% comment %}
* We want your environment to work better. Docker containers,
      and the work flow that comes with them, help your developers,
      sysadmins, QA folks, and release engineers work together to get your code
      into production and make it useful. We've created a standard
      container format that lets developers care about their applications
      inside containers while sysadmins and operators can work on running the
      container in your deployment. This separation of duties streamlines and
      simplifies the management and deployment of code.
* We make it easy to build new containers, enable rapid iteration of
      your applications, and increase the visibility of changes. This
      helps everyone in your organization understand how an application works
      and how it is built.
* Docker containers are lightweight and fast! Containers have
      sub-second launch times, reducing the cycle
      time of development, testing, and deployment.
{% endcomment %}
* 私たちはみなさんの環境を良くしたいのです。
  Docker コンテナーおよびこれを利用したワークフローは、開発に関わるすべての人、つまり開発者、システム管理者、品質管理担当者、リリースエンジニアを含め、コードを本番環境へ適用し実運用させる作業すべてを手助けします。
  標準的なコンテナーフォーマットというものが作り出されているので、開発者にとってはコンテナー内にあるアプリケーションの開発に集中するだけでよく、システム管理者やオペレーターはデプロイされたコンテナーの運用に取り組むだけでよくなります。
  このように作業を分担することは、コード管理とデプロイを効率化し簡素化することを意味します。
* 新たなコンテナーの構築は容易にできます。
  さらにアプリケーションを迅速に繰り返して投入することや、変更がわかりやすくなるようにしてます。
  つまり開発する誰にとっても、アプリケーションがいかに作動し、どのようにして構築されるかを、簡単に理解できるようにもなっているわけです。
* Docker コンテナーは軽量かつ高速です！
  コンテナーの起動時間は数秒であり、開発、テスト、デプロイのサイクルにかかる時間を減らします。

{% comment %}
*Deploy and scale more easily*
{% endcomment %}
*デプロイやスケールをもっと簡単に*

{% comment %}
* Docker containers run (almost) everywhere. You can deploy
      containers on desktops, physical servers, virtual machines, into
      data centers, and up to public and private clouds.
* Since Docker runs on so many platforms, it's easy to move your
      applications around. You can easily move an application from a
      testing environment into the cloud and back whenever you need.
* Docker's lightweight containers also make scaling up and
      down fast and easy. You can quickly launch more containers when
      needed and then shut them down easily when they're no longer needed.
{% endcomment %}
* Docker コンテナーは（ほとんど）どこでも動きます。
  コンテナーのデプロイは、デスクトップ、物理サーバー、仮想マシンに対して行うことができます。
  さらにデータセンターやパブリッククラウド、プライベートクラウドにもデプロイできます。
* Docker は多くのプラットフォーム上で動作するので、開発したアプリケーションをあちこちに動かすことが簡単にできます。
  アプリケーションは、テスト環境からクラウド上に簡単に移動でき、必要に応じてすぐに戻すこともできます。
* Docker の軽量なコンテナーは、スケールアップやスケールダウンもすばやく簡単に実現します。
  必要なときに必要なだけコンテナーをすばやく起動でき、不要になったときには簡単に停止することができます。

{% comment %}
*Get higher density and run more workloads*
{% endcomment %}
*処理を集中させ負荷を高く*

{% comment %}
* Docker containers don't need a hypervisor, so you can pack more of
      them onto your hosts. This means you get more value out of every
      server and can potentially reduce what you spend on equipment and
      licenses.
{% endcomment %}
* Docker コンテナーはハイパーバイザーを必要としないため、ホスト上により多くを詰め込むことができます。
  つまり各サーバーの価値を十分に引き出し、機器やライセンスにかかる潜在的なコストを軽減する可能性を秘めています。

{% comment %}
*Faster deployment makes for easier management*
{% endcomment %}
*管理の容易さを目指してデプロイを迅速化*

{% comment %}
* As Docker speeds up your work flow, it gets easier to make lots
      of small changes instead of huge, big bang updates. Smaller
      changes mean reduced risk and more uptime.
{% endcomment %}
* Docker によってワークフローがスピードアップするため、とてつもなく大きな更新を行うのではなく、小さな更新を数多くこなすことが可能になります。
  小さな更新であればあるほど、リスクは減り更新タイミングを増やすことができます。

{% comment %}
## About this guide
{% endcomment %}
## このガイドについて
{: #about-this-guide }

{% comment %}
The [Understanding Docker section](understanding-docker.md) helps you:
{% endcomment %}
[Docker を理解するページ](understanding-docker.md)では以下が示されています。

{% comment %}
 - See how Docker works at a high level
 - Understand the architecture of Docker
 - Discover Docker's features;
 - See how Docker compares to virtual machines
 - See some common use cases.
{% endcomment %}
 - Docker がいかにして動作するかを詳細に
 - Docker のアーキテクチャーを理解する
 - Docker の機能を確認する
 - Docker と仮想マシンの違いを見る
 - 一般的な利用例を見る

{% comment %}
### Installation guides
{% endcomment %}
### インストールガイド
{: #installation-guides }

{% comment %}
The [installation section](installation/index.md) shows you how to install Docker
on a variety of platforms.
{% endcomment %}
[インストールのセクション](installation/index.md)では、さまざまなプラットフォームにおける Docker のインストール方法を示します。


{% comment %}
### Docker user guide
{% endcomment %}
### Docker ユーザーガイド
{: #docker-user-guide }

{% comment %}
To learn about Docker in more detail and to answer questions about usage and
implementation, check out the [Docker User Guide](userguide/index.md).
{% endcomment %}
Docker についての詳細、あるいは使い方や実装についての疑問を解消するには [Docker Engine ユーザーガイド](userguide/index.md)を確認してください。

{% comment %}
## Release notes
{% endcomment %}
## リリースノート
{: #release-notes }

{% comment %}
A summary of the changes in each release in the current series can now be found
on the separate [Release Notes page](/release-notes)
{% endcomment %}
各リリースにおける変更点の概要については、[リリースノートのページ](/release-notes)を確認してください。

{% comment %}
## Feature Deprecation Policy
{% endcomment %}
## 機能廃止に関する方針
{: #feature-deprecation-policy }

{% comment %}
As changes are made to Docker there may be times when existing features
need to be removed or replaced with newer features. Before an existing
feature is removed it is labeled as "deprecated" within the documentation
and remains in Docker for at least 3 stable releases. After that time it may be
removed.
{% endcomment %}
Docker の機能変更に際しては、既存機能を削除したり新たな機能に置き換えたりする必要があった場合には、時間をおくことが必要になります。
既存機能を削除するにあたっては、ドキュメント内に "deprecated"（廃止予定）とラベル付けするようにします。
そして Docker モジュール内には、最低でも３つの安定版がリリースされる間（およそ９ヶ月）は残すようにします。
この期間を過ぎたものは削除されることがあります。

{% comment %}
Users are expected to take note of the list of deprecated features each
release and plan their migration away from those features, and (if applicable)
towards the replacement features as soon as possible.
{% endcomment %}
ユーザーは最新リリースごとに、廃止予定の機能一覧を注意して見ていく必要があります。
最新版への移行にあたっては、廃止予定の機能は使わないようにして、（適用可能であれば）できるだけ早くに代替機能を用いるようにしてください。

{% comment %}
The complete list of deprecated features can be found on the
[Deprecated Features page](deprecated.md).
{% endcomment %}
廃止予定の機能一覧については、[廃止予定機能のページ](deprecated.md)を確認してください。

{% comment %}
## Licensing
{% endcomment %}
## ライセンス
{: #licensing }

{% comment %}
Docker is licensed under the Apache License, Version 2.0. See
[LICENSE](https://github.com/moby/moby/blob/master/LICENSE) for the full
license text.
{% endcomment %}
Docker のライセンスは Apache License, Version 2.0 です。
ライセンス条項の詳細は [LICENSE](https://github.com/moby/moby/blob/master/LICENSE) を確認してください。
