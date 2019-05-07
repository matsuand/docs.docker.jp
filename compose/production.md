---
description: 本番環境で Compose を利用するためのガイド。
keywords: compose, orchestration, containers, production
title: 本番環境での Compose の利用
---

<!--
When you define your app with Compose in development, you can use this
definition to run your application in different environments such as CI,
staging, and production.
-->
Compose を用いたアプリの定義を開発環境で行っていたら、その定義を別の環境に、たとえば継続インテグレーション（CI）、ステージング環境、本番環境に適用することができます。

<!--
The easiest way to deploy an application is to run it on a single server,
similar to how you would run your development environment. If you want to scale
up your application, you can run Compose apps on a Swarm cluster.
-->
アプリケーションをデプロイする一番簡単な方法は、一つのサーバー上で動作させることです。
ちょうど開発環境で動作させている方法に近いものです。
アプリケーションをスケールアップしたいなら、Compose アプリをスウォームクラスター上で実行する方法もあります。

<!--
### Modify your Compose file for production
-->
### 本番環境向け Compose ファイルの修正
{: #modify-your-compose-file-for-production }

<!--
You probably need to make changes to your app configuration to make it ready for
production. These changes may include:
-->
アプリケーションの設定を本番環境向けにするには、変更を要するかもしれません。
その変更とは以下のようなものです。

<!--
- Removing any volume bindings for application code, so that code stays inside
  the container and can't be changed from outside
- Binding to different ports on the host
- Setting environment variables differently, such as when you need to decrease the verbosity of
  logging, or to enable email sending)
- Specifying a restart policy like `restart: always` to avoid downtime
- Adding extra services such as a log aggregator
-->
- アプリケーションコードからボリュームバインディングは削除します。
  コードは内部のみで実現するようにし、外部から変更されないようにします。
- ホスト上のポートは別のものを割り当てます。
- 環境変数は区別して割り当てます。
  （たとえば冗長なログ出力とならないようにログレベルを下げる、メール送信を可能にする、など。）
- 再起動ポリシーを設定します。
  例えば `restart: always` とすることで、システムダウンの時間を減らします。
- ログ収集といったような追加サービスを設定します。

<!--
For this reason, consider defining an additional Compose file, say
`production.yml`, which specifies production-appropriate
configuration. This configuration file only needs to include the changes you'd
like to make from the original Compose file. The additional Compose file
can be applied over the original `docker-compose.yml` to create a new configuration.
-->
このようなことがあるので、追加の Compose ファイルとしてたとえば `production.yml` といったものを用意して、そこに本番環境固有の設定を行うことを考えておきましょう。
この設定ファイルには、元の Compose ファイルに対して変更したい内容のみを含めておけばよいことになります。
つまりこの追加ファイルは、元々の `docker-compose.yml` にさらに設定を追加して、新たな設定を作り出すものとなるわけです。

<!--
Once you've got a second configuration file, tell Compose to use it with the
`-f` option:
-->
このような 2 つめの設定ファイルを用意したら Compose に対してこれを利用するために `-f` オプションを用います。

    docker-compose -f docker-compose.yml -f production.yml up -d

<!--
See [Using multiple compose files](extends.md#different-environments) for a more
complete example.
-->
より詳細な例は、[複数 compose ファイルの利用](extends.md#different-environments)を参照してください。

<!--
### Deploying changes
-->
### アプリ変更後のデプロイ
{: #deploying-changes }

<!--
When you make changes to your app code, remember to rebuild your image and
recreate your app's containers. To redeploy a service called
`web`, use:
-->
アプリコードに変更を加えたら、イメージを再ビルドし、アプリのコンテナーを再生成することを忘れないでください。
`web` というサービスを再デプロイするには以下のようにします。

    $ docker-compose build web
    $ docker-compose up --no-deps -d web

<!--
This first rebuilds the image for `web` and then stop, destroy, and recreate
*just* the `web` service. The `--no-deps` flag prevents Compose from also
recreating any services which `web` depends on.
-->
はじめに `web` のイメージを再ビルドし、次に `web` サービス**のみ**を停止、破棄、再生成します。
`--no-deps` フラグは `web` サービスが依存している他のサービスを再生成しないことを指示しています。

<!--
### Running Compose on a single server
-->
### 単一サーバー上での Compose の実行
{: #running-compose-on-a-single-server }

<!--
You can use Compose to deploy an app to a remote Docker host by setting the
`DOCKER_HOST`, `DOCKER_TLS_VERIFY`, and `DOCKER_CERT_PATH` environment variables
appropriately. For tasks like this,
[Docker Machine](/machine/overview.md) makes managing local and
remote Docker hosts very easy, and is recommended even if you're not deploying
remotely.
-->
Compose を使って、リモートの Docker ホストへアプリをデプロイすることができます。
これを行うには `DOCKER_HOST`、`DOCKER_TLS_VERIFY`、`DOCKER_CERT_PATH` という各環境変数を適切に設定します。
この作業を行うにあたっては  [Docker Machine](/machine/overview.md) を用いれば、Docker ホストがローカルでもリモートでも簡単に管理することができます。
リモートへのデプロイを行うことがない場合でも、このツールを用いることをお勧めします。

<!--
Once you've set up your environment variables, all the normal `docker-compose`
commands work with no further configuration.
-->
環境変数を設定していれば、他になにかを設定する必要はなく、通常の `docker-compose` コマンドを使っていくことができます。

<!--
### Running Compose on a Swarm cluster
-->
### スウォームクラスター上での Compose の実行
{: #running-compose-on-a-swarm-cluster }

<!--
[Docker Swarm](/swarm/overview.md), a Docker-native clustering
system, exposes the same API as a single Docker host, which means you can use
Compose against a Swarm instance and run your apps across multiple hosts.
-->
[Docker Swarm](/swarm/overview.md) は Docker のネイティブなクラスターシステムです。
単一 Docker ホストと同様の API を提供します。
つまりスウォームインタンスに対しても Compose を利用することができ、また複数ホストにわたってアプリを実行することができるということです。

<!--
Read more about the Compose/Swarm integration in the
[integration guide](swarm.md).
-->
Compose と Swarm の統合に関しては、[統合に関する説明](swarm.md)を確認してください。

<!--
## Compose documentation
-->
## Compose ドキュメント
{: #compose-documentation }

<!--
- [Installing Compose](install.md)
- [Command line reference](./reference/index.md)
- [Compose file reference](compose-file.md)
-->
- [Compose のインストール](install.md)
- [コマンドラインリファレンス](./reference/index.md)
- [Compose ファイルリファレンス](compose-file.md)
