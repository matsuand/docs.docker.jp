---
title: Docker 開発のベストプラクティス
description: Docker アプリケーションの開発をより簡単に進めるための経験則を示します。
keywords: application, development
---

{% comment %}
The following development patterns have proven to be helpful for people
building applications with Docker. If you have discovered something we should
add,
[let us know](https://github.com/docker/docker.github.io/issues/new){: target="_blank" class="_"}.
{% endcomment %}
Docker を使ってアプリケーション開発を行う方々にとって、以下に示す開発パターンが有効であることが明らかになっています。
何かを見つけている場合は [お知らせください](https://github.com/docker/docker.github.io/issues/new){: target="_blank" class="_"}。

{% comment %}
## How to keep your images small
{% endcomment %}
## どうやってイメージを小さく保つか

{% comment %}
Small images are faster to pull over the network and faster to load into
memory when starting containers or services. There are a few rules of thumb to
keep image size small:
{% endcomment %}
イメージが小さければネットワークからの取得が速くなります。
またコンテナーやサービスの起動時に、メモリへのロードも速くなります。
イメージを小さく保つ経験則をいくつか示します。

{% comment %}
- Start with an appropriate base image. For instance, if you need a JDK,
  consider basing your image on the official `openjdk` image, rather than
  starting with a generic `ubuntu` image and installing `openjdk` as part of the
  Dockerfile.
{% endcomment %}
- 適切なイメージをベースとして始めます。
  たとえば JDK を必要とするのであれば、公式イメージ `openjdk` をベースとしたイメージ作りとします。
  逆に、汎用的な `ubuntu` イメージから始めて、Dockerfile 内に `openjdk` をインストールするような方法は取らないようにします。

{% comment %}
- [Use multistage builds](/engine/userguide/eng-image/multistage-build.md). For
  instance, you can use the `maven` image to build your Java application, then
  reset to the `tomcat` image and copy the Java artifacts into the correct
  location to deploy your app, all in the same Dockerfile. This means that your
  final image doesn't include all of the libraries and dependencies pulled in by
  the build, but only the artifacts and the environment needed to run them.
{% endcomment %}
- [マルチステージビルド](/engine/userguide/eng-image/multistage-build.md) を利用します。
  たとえば `maven` イメージを使うと Java アプリケーションを構築することができます。
  これを `tomcat` イメージとして作り直して、Java アプリのコード類を適切な場所に配置してデプロイできるようにします。
  これをすべて同一の Dockerfile 内で行います。
  これはつまり最終的なイメージがビルドされたら、そのイメージ内には、元々のイメージ取得時に存在していたライブラリや依存パッケージがすべて含まれるわけではなく、実行時に必要なモジュールや環境のみが含まれるということを意味します。

{% comment %}
  - If you need to use a version of Docker that does not include multistage
    builds, try to reduce the number of layers in your image by minimizing the
    number of separate `RUN` commands in your Dockerfile. You can do this by
    consolidating multiple commands into a single `RUN` line and using your
    shell's mechanisms to combine them together. Consider the following two
    fragments. The first creates two layers in the image, while the second
    only creates one.
{% endcomment %}
  - マルチステージビルドの機能を持たない Docker バージョンを使う必要があるときには、イメージ内に作られるレイヤー数を減らすようにしてください。
    これは Dockerfile 内での `RUN` コマンドの実行が、できるだけ分断されないように、その実行数を最小化します。
    これを実現するには、複数の `RUN` コマンドはできるだけ 1 つの `RUN` コマンドとなるように、シェルの機能を使って互いに連結させます。
    たとえば以下のような 2 つのコマンド実行例があったとします。
    1 つめのコマンドは、イメージ内に 2 つのレイヤーを生成しますが、2 つめのコマンドはレイヤーが 1 つで済みます。

    ```conf
    RUN apt-get -y update
    RUN apt-get install -y python
    ```

    ```conf
    RUN apt-get -y update && apt-get install -y python
    ```

{% comment %}
- If you have multiple images with a lot in common, consider creating your own
  [base image](/engine/userguide/eng-image/baseimages.md) with the shared
  components, and basing your unique images on that. Docker only needs to load
  the common layers once, and they are cached. This means that your
  derivative images use memory on the Docker host more efficiently and load more
  quickly.
{% endcomment %}
- 共有するイメージがたくさんある場合は、[ベースイメージ](/engine/userguide/eng-image/baseimages.md) を作ることを考えてみてください。
  これを用いてコンポーネントを共有し、これをベースとした独自のイメージを作っていくことができます。
  Docker は共通するレイヤーであれば 1 度しかロードする必要がなく、ロードした内容はキャッシュされます。
  つまりベースイメージから派生させたイメージは、Docker ホスト上でのメモリ利用が効率よく行われ、ロードもすばやく行われることになります。

{% comment %}
- To keep your production image lean but allow for debugging, consider using the
  production image as the base image for the debug image. Additional testing or
  debugging tooling can be added on top of the production image.
{% endcomment %}
- 本番環境向けのイメージはスリムにしたいものの、デバッグは可能にしたいといった場合は、デバッグ環境向けとして、本番環境イメージをベースイメージとすることを考えてみてください。
  さらにテストやデバッグツールを加えたい場合でも、本番環境イメージの上に追加ができます。

{% comment %}
- When building images, always tag them with useful tags which codify version
  information, intended destination (`prod` or `test`, for instance), stability,
  or other information that is useful when deploying the application in
  different environments. Do not rely on the automatically-created `latest` tag.
{% endcomment %}
- イメージをビルドする場合には、常にわかりやすいタグをつけるようにします。
  このタグを用いて、バージョン情報をコード化したり、目的とする用途（たとえば `prod` や `test` など）や安定性など、いろいろな情報を付与したりします。
  こうしておけば、アプリケーションをさまざまな環境にデプロイする際にわかりやすくなります。
  自動的に生成される `latest` タグには頼らないようにします。

{% comment %}
## Where and how to persist application data
{% endcomment %}
{: #where-and-how-to-persist-application-data }
## アプリケーションデータはどこにどう保存するか

{% comment %}
- **Avoid** storing application data in your container's writable layer using
  [storage drivers](/engine/userguide/storagedriver.md). This increases the
  size of your container and is less efficient from an I/O perspective than
  using volumes or bind mounts.
- Instead, store data using [volumes](/engine/admin/volumes/volumes.md).
- One case where it is appropriate to use
  [bind mounts](/engine/admin/volumes/bind-mounts.md) is during development,
  when you may want to mount your source directory or a binary you just built
  into your container. For production, use a volume instead, mounting it into
  the same location as you mounted a bind mount during development.
- For production, use [secrets](/engine/swarm/secrets.md) to store sensitive
  application data used by services, and use [configs](/engine/swarm/configs.md)
  for non-sensitive data such as configuration files. If you currently use
  standalone containers, consider migrating to use single-replica services, so
  that you can take advantage of these service-only features.
{% endcomment %}
- [ストレージドライバー](/engine/userguide/storagedriver.md) によって、コンテナーの書き込み可能レイヤーへデータ保存を行うことができますが、アプリケーションデータの保存を行うことは**避けます**。
  これを行ってしまうと、コンテナーのサイズが増えることになり、I/O 観点で言えば、ボリュームやバインドマウントを用いることに比べて非効率なものになります。
- そのかわりに、データ保存は [ボリューム](/engine/admin/volumes/volumes.md) を利用します。
- [バインドマウント](/engine/admin/volumes/bind-mounts.md) を用いるのが適当な例として、開発時での利用が考えられます。
  開発時には、ソースディレクトリや生成したばかりのバイナリを、コンテナー内にマウントしたくなります。
  本番環境ではボリュームを利用しますが、本番環境がマウントする同じ場所を、開発環境時はバインドマウントによりマウントします。
- 本番環境において、サービスが機密情報を利用している場合、その保存には [secrets](/engine/swarm/secrets.md) を利用します。そして機密情報ではない設定ファイルなどの情報は [configs](/engine/swarm/configs.md) を利用します。
  今利用しているコンテナーがスタンドアロンである場合、
  consider migrating to use single-replica services, so
  that you can take advantage of these service-only features.

{% comment %}
## Use swarm services when possible
{% endcomment %}
{: #use-swarm-services-when-possible }
## 可能であればスウォームサービスを利用する

{% comment %}
- When possible, design your application with the ability to scale using swarm
  services.
- Even if you only need to run a single instance of your application, swarm
  services provide several advantages over standalone containers. A service's
  configuration is declarative, and Docker is always working to keep the
  desired and actual state in sync.
- Networks and volumes can be connected and disconnected from swarm services,
  and Docker handles redeploying the individual service containers in a
  non-disruptive way. Standalone containers need to be manually stopped, removed,
  and recreated to accommodate configuration changes.
- Several features, such as the ability to store
  [secrets](/engine/swarm/secrets.md) and [configs](/engine/swarm/configs.md),
  are only available to services rather than standalone containers. These
  features allow you to keep your images as generic as possible and to avoid
  storing sensitive data within the Docker images or containers themselves.
- Let `docker stack deploy` handle any image pulls for you, instead of using
  `docker pull`. This way, your deployment doesn't try to pull from nodes
  that are down. Also, when new nodes are added to the swarm, images are
  pulled automatically.
{% endcomment %}
- 可能であればスウォームサービスを利用し、スケールが可能なアプリケーション設計とします。
- アプリケーションの実行インスタンスがただ 1 つあれば良い場合であっても、スタンドアロンコンテナーに比べてスウォームサービスには有用な機能が提供されます。
  サービスの設定は宣言によって行われるため、Docker は常に、設定された内容と実際が同期して動作します。
- Networks and volumes can be connected and disconnected from swarm services,
  and Docker handles redeploying the individual service containers in a
  non-disruptive way. Standalone containers need to be manually stopped, removed,
  and recreated to accommodate configuration changes.
- Several features, such as the ability to store
  [secrets](/engine/swarm/secrets.md) and [configs](/engine/swarm/configs.md),
  are only available to services rather than standalone containers. These
  features allow you to keep your images as generic as possible and to avoid
  storing sensitive data within the Docker images or containers themselves.
- Let `docker stack deploy` handle any image pulls for you, instead of using
  `docker pull`. This way, your deployment doesn't try to pull from nodes
  that are down. Also, when new nodes are added to the swarm, images are
  pulled automatically.

{% comment %}
There are limitations around sharing data amongst nodes of a swarm service.
If you use [Docker for AWS](/docker-for-aws/persistent-data-volumes.md) or
[Docker for Azure](/docker-for-azure/persistent-data-volumes.md), you can use the
Cloudstor plugin to share data amongst your swarm service nodes. You can also
write your application data into a separate database which supports simultaneous
updates.
{% endcomment %}
There are limitations around sharing data amongst nodes of a swarm service.
If you use [Docker for AWS](/docker-for-aws/persistent-data-volumes.md) or
[Docker for Azure](/docker-for-azure/persistent-data-volumes.md), you can use the
Cloudstor plugin to share data amongst your swarm service nodes. You can also
write your application data into a separate database which supports simultaneous
updates.

{% comment %}
## Use CI/CD for testing and deployment
{% endcomment %}
## Use CI/CD for testing and deployment

{% comment %}
- When you check a change into source control or create a pull request, use
  [Docker Hub](/docker-hub/builds/automated-build.md) or
  another CI/CD pipeline to automatically build and tag a Docker image and test
  it.
{% endcomment %}
- When you check a change into source control or create a pull request, use
  [Docker Hub](/docker-hub/builds/automated-build.md) or
  another CI/CD pipeline to automatically build and tag a Docker image and test
  it.

{% comment %}
- Take this even further with [Docker Engine - Enterprise](/ee/index.md) by requiring
  your development, testing, and security teams to sign images before they can
  be deployed into production. This way, you can be sure that before an image is
  deployed into production, it has been tested and signed off by, for instance,
  development, quality, and security teams.
{% endcomment %}
- Take this even further with [Docker Engine - Enterprise](/ee/index.md) by requiring
  your development, testing, and security teams to sign images before they can
  be deployed into production. This way, you can be sure that before an image is
  deployed into production, it has been tested and signed off by, for instance,
  development, quality, and security teams.

{% comment %}
## Differences in development and production environments
{% endcomment %}
## Differences in development and production environments

{% comment %}
| Development                                                         | Production                                                                                                                                                                                                                                       |
|:--------------------------------------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Use bind mounts to give your container access to your source  code. | Use volumes to store container data.                                                                                                                                                                                                             |
| Use Docker Desktop for Mac or Docker Desktop for Windows.                           | Use Docker EE if possible, with [userns mapping](/engine/security/userns-remap.md) for greater isolation of Docker processes from host processes.                                                                                                |
| Don't worry about time drift.                                       | Always run an NTP client on the Docker host and within each container process and sync them all to the same NTP server. If you use swarm services, also ensure that each Docker node syncs its clocks to the same time source as the containers. |
{% endcomment %}
| Development                                                         | Production                                                                                                                                                                                                                                       |
|:--------------------------------------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Use bind mounts to give your container access to your source  code. | Use volumes to store container data.                                                                                                                                                                                                             |
| Use Docker Desktop for Mac or Docker Desktop for Windows.                           | Use Docker EE if possible, with [userns mapping](/engine/security/userns-remap.md) for greater isolation of Docker processes from host processes.                                                                                                |
| Don't worry about time drift.                                       | Always run an NTP client on the Docker host and within each container process and sync them all to the same NTP server. If you use swarm services, also ensure that each Docker node syncs its clocks to the same time source as the containers. |
