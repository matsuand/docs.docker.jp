---
description: Compose はコンテナー間のネットワークをどのように構築するか。
keywords: documentation, docs, docker, compose, orchestration, containers, networking
title: Compose におけるネットワーク機能
---

{% comment %}
> This page applies to Compose file formats [version 2](/compose/compose-file/compose-file-v2.md) and [higher](/compose/compose-file/index.md). Networking features are not supported for Compose file [version 1 (legacy)](/compose/compose-file/compose-file-v1.md).
{% endcomment %}
> このページに示す内容は Compose ファイルフォーマットの [バージョン 2](/compose/compose-file/compose-file-v2.md) と [それ以降](/compose/compose-file/index.md) に適用されます。
ネットワーク機能は[（古い）バージョン 1](/compose/compose-file/compose-file-v1.md) ではサポートされません。

{% comment %}
By default Compose sets up a single
[network](/engine/reference/commandline/network_create.md) for your app. Each
container for a service joins the default network and is both *reachable* by
other containers on that network, and *discoverable* by them at a hostname
identical to the container name.
{% endcomment %}
デフォルトで Compose は、アプリ向けに単一の [ネットワーク](/engine/reference/commandline/network_create.md) を設定します。
1 つのサービスを構成する各コンテナーは、そのデフォルトのネットワークに参加するので、ネットワーク上の他のコンテナーからのアクセスが可能です。
さらにコンテナー名と同等のホスト名を用いてコンテナーの識別が可能となります。

{% comment %}
> **Note**: Your app's network is given a name based on the "project name",
> which is based on the name of the directory it lives in. You can override the
> project name with either the [`--project-name` flag](/compose/reference/overview.md)
> or the [`COMPOSE_PROJECT_NAME` environment variable](/compose/reference/envvars.md#compose_project_name).
{% endcomment %}
> **メモ**: アプリのネットワークには「プロジェクト名」に基づいた名前がつけられます。
> そしてプロジェクト名はこれが稼動しているディレクトリ名に基づいて定まります。
> プロジェクト名は [`--project-name` フラグ](/compose/reference/overview.md) あるいは [`COMPOSE_PROJECT_NAME` 環境変数](/compose/reference/envvars.md#compose_project_name) を使って上書きすることができます。

{% comment %}
For example, suppose your app is in a directory called `myapp`, and your `docker-compose.yml` looks like this:
{% endcomment %}
たとえばアプリが `myapp` というディレクトリにあって、`docker-compose.yml` が以下のような内容であるとします。

    version: "3"
    services:
      web:
        build: .
        ports:
          - "8000:8000"
      db:
        image: postgres
        ports:
          - "8001:5432"

{% comment %}
When you run `docker-compose up`, the following happens:
{% endcomment %}
`docker-compose up` を実行すると以下の結果になります。

{% comment %}
1.  A network called `myapp_default` is created.
2.  A container is created using `web`'s configuration. It joins the network
    `myapp_default` under the name `web`.
3.  A container is created using `db`'s configuration. It joins the network
    `myapp_default` under the name `db`.
{% endcomment %}
1.  `myapp_default` というネットワークが生成されます。
2.  `web` に関する設定に従って 1 つのコンテナーが生成されます。
    そしてそのコンテナーは `web` という名前でネットワーク `myapp_default` に参加します。
3.  `db` に関する設定に従って 1 つのコンテナーが生成されます。
    そしてそのコンテナーは `db` という名前でネットワーク `myapp_default` に参加します。

{% comment %}
> **In v2.1+, overlay networks are always `attachable`**
>
> Starting in Compose file format 2.1, overlay networks are always created as
> `attachable`, and this is not configurable. This means that standalone
> containers can connect to overlay networks.
>
> In Compose file format 3.x, you can optionally set the `attachable` property
> to `false`.
{% endcomment %}
> **v2.1 以降、overlay ネットワークは常に `attachable` に**
>
> Compose ファイルフォーマット 2.1 から、overlay ネットワークは常に `attachable` として生成されるようになりましたが、これを変更することはできません。
> `attachable` として生成されるため、スタンドアロンのコンテナーでも overlay ネットワークに接続することができます。
> Compose ファイルフォーマット 3.x においては、必要に応じて `attachable` プロパティに `false` を設定できます。

{% comment %}
Each container can now look up the hostname `web` or `db` and
get back the appropriate container's IP address. For example, `web`'s
application code could connect to the URL `postgres://db:5432` and start
using the Postgres database.
{% endcomment %}
各コンテナーはこれ以降、ホスト名 `web` と `db` を認識できるようになり、コンテナーの IP アドレスも適切に取得できるようになります。
たとえば `web` のアプリケーションコードでは、URL `postgres://db:5432` を使ってのアクセスが可能となり、Postgres データベースの利用ができるようになります。

{% comment %}
It is important to note the distinction between `HOST_PORT` and `CONTAINER_PORT`.
In the above example, for `db`, the `HOST_PORT` is `8001` and the container port is
`5432` (postgres default). Networked service-to-service
communication use the `CONTAINER_PORT`. When `HOST_PORT` is defined,
the service is accessible outside the swarm as well.
{% endcomment %}
`HOST_PORT` と `CONTAINER_PORT` の違いについては理解しておくことが重要です。
上の例の `db` では、`HOST_PORT` が `8001`、コンテナーポートが `5432`（postgres のデフォルト） になっています。
ネットワークにより接続されているサービス間の通信は `CONTAINER_PORT` を利用します。
`HOST_PORT` を定義すると、このサービスはスウォームの外からもアクセスが可能になります。

{% comment %}
Within the `web` container, your connection string to `db` would look like
`postgres://db:5432`, and from the host machine, the connection string would
look like `postgres://{DOCKER_IP}:8001`.
{% endcomment %}
`web` コンテナー内では、`db` への接続文字列は `postgres://db:5432` といったものになります。
そしてホストマシン上からは、その接続文字列は `postgres://{DOCKER_IP}:8001` となります。

{% comment %}
## Update containers
{% endcomment %}
## コンテナーの更新
{: #update-containers }

{% comment %}
If you make a configuration change to a service and run `docker-compose up` to update it, the old container is removed and the new one joins the network under a different IP address but the same name. Running containers can look up that name and connect to the new address, but the old address stops working.
{% endcomment %}
サービスに対する設定を変更して `docker-compose up` により更新を行うと、それまでのコンテナーは削除されて新しいコンテナーがネットワークに接続されます。
このとき IP アドレスは異なることになりますが、ホスト名は変わりません。
コンテナーの実行によってホスト名による名前解決を行い、新たな IP アドレスへ接続します。
それまでの古い IP アドレスは利用できなくなります。

{% comment %}
If any containers have connections open to the old container, they are closed. It is a container's responsibility to detect this condition, look up the name again and reconnect.
{% endcomment %}
古いコンテナーに対して接続を行っていたコンテナーがあれば、その接続は切断されます。
この状況を検出するのは各コンテナーの責任であって、ホスト名を探して再接続が行われます。

## Links

{% comment %}
Links allow you to define extra aliases by which a service is reachable from another service. They are not required to enable services to communicate - by default, any service can reach any other service at that service's name. In the following example, `db` is reachable from `web` at the hostnames `db` and `database`:
{% endcomment %}
links は自サービスが他のサービスからアクセスできるように、追加でエイリアスを定義するものです。
これはサービス間の通信を行うために必要となるわけではありません。
デフォルトにおいてサービスは、サービス名を使って他サービスにアクセスできます。
以下の例においては、`db` は `web` からアクセス可能であり、ホスト名 `db` あるいは `database` を使ってアクセスできます。

    version: "3"
    services:

      web:
        build: .
        links:
          - "db:database"
      db:
        image: postgres

{% comment %}
See the [links reference](/compose/compose-file/compose-file-v2.md#links) for more information.
{% endcomment %}
詳細は [links リファレンス](/compose/compose-file/compose-file-v2.md#links) を参照してください。

{% comment %}
## Multi-host networking
{% endcomment %}
## 複数ホストによるネットワーク
{: #Multi-host networking }

{% comment %}
> **Note**: The instructions in this section refer to [legacy Docker Swarm](swarm.md) operations, and only work when targeting a legacy Swarm cluster. For instructions on deploying a compose project to the newer integrated swarm mode, consult the [Docker Stacks](/engine/reference/commandline/stack_deploy.md) documentation.
{% endcomment %}
> **メモ**: ここに示す手順は、[かつての Docker Swarm](swarm.md) の操作に基づいています。
> したがってかつてのスウォームクラスターを対象とする場合にのみ動作します。
> Compose によるプロジェクトを、最新の統合されたスウォームモードにデプロイするには、[Docker Stacks](/engine/reference/commandline/stack_deploy.md) に示すドキュメントを参照してください。

{% comment %}
When [deploying a Compose application to a Swarm cluster](swarm.md), you can make use of the built-in `overlay` driver to enable multi-host communication between containers with no changes to your Compose file or application code.
{% endcomment %}
[Compose アプリケーションをスウォームクラスターにデプロイする](swarm.md) 際には、ビルトインの `overlay` ドライバーを利用して、コンテナー間で複数ホストによる通信を行うことが可能です。
Compose ファイルやアプリケーションコードへの変更は必要ありません。

{% comment %}
Consult the [Getting started with multi-host networking](/network/network-tutorial-overlay.md) to see how to set up a Swarm cluster. The cluster uses the `overlay` driver by default, but you can specify it explicitly if you prefer - see below for how to do this.
{% endcomment %}
[複数ホストによるネットワークをはじめよう](/network/network-tutorial-overlay.md) を参考に、スウォームクラスターの構築方法を確認してください。
デフォルトでクラスターは `overlay` ドライバーを用います。
ただし明示的にこれを指定することもできます。
詳しくは後述します。

{% comment %}
## Specify custom networks
{% endcomment %}
## 独自のネットワーク設定
{: #specify-custom-networks }

{% comment %}
Instead of just using the default app network, you can specify your own networks with the top-level `networks` key. This lets you create more complex topologies and specify [custom network drivers](/engine/extend/plugins_network/) and options. You can also use it to connect services to externally-created networks which aren't managed by Compose.
{% endcomment %}
デフォルトのアプリ用ネットワークを利用するのではなく、独自のネットワークを指定することができます。
これは最上位の `networks` キーを使って行います。
これを使えば、より複雑なネットワークトポロジーを生成したり、[独自のネットワークドライバー](/engine/extend/plugins_network/) とそのオプションを設定したりすることができます。
さらには、Compose が管理していない、外部に生成されたネットワークに対してサービスを接続することもできます。

{% comment %}
Each service can specify what networks to connect to with the *service-level* `networks` key, which is a list of names referencing entries under the *top-level* `networks` key.
{% endcomment %}
サービスレベルの定義となる `networks` キーを利用すれば、サービスごとにどのネットワークに接続するかを指定できます。
指定する値はサービス名のリストであり、最上位の `networks` キーに指定されている値を参照するものです。

{% comment %}
Here's an example Compose file defining two custom networks. The `proxy` service is isolated from the `db` service, because they do not share a network in common - only `app` can talk to both.
{% endcomment %}
以下において Compose ファイルは、独自のネットワークを 2 つ定義しています。
`proxy` サービスは `db` サービスから切り離されています。
というのも両者はネットワークを共有しないためです。
そして `app` だけがその両者と通信を行います。

    version: "3"
    services:

      proxy:
        build: ./proxy
        networks:
          - frontend
      app:
        build: ./app
        networks:
          - frontend
          - backend
      db:
        image: postgres
        networks:
          - backend

    networks:
      frontend:
        # 独自ドライバーの利用
        driver: custom-driver-1
      backend:
        # 所定のオプションを用いる独自ドライバーの利用
        driver: custom-driver-2
        driver_opts:
          foo: "1"
          bar: "2"

{% comment %}
Networks can be configured with static IP addresses by setting the [ipv4_address and/or ipv6_address](/compose/compose-file/compose-file-v2.md#ipv4_address-ipv6_address) for each attached network.
{% endcomment %}
接続するネットワークのそれぞれは、[ipv4_address または ipv6_address](/compose/compose-file/compose-file-v2.md#ipv4_address-ipv6_address) を使ってスタティック IP アドレスを設定することができます。

{% comment %}
Networks can also be given a [custom name](/compose/compose-file/index.md#network-configuration-reference) (since version 3.5):
{% endcomment %}
ネットワークには [独自の名前](/compose/compose-file/index.md#network-configuration-reference) も設定することができます。
（バージョン 3.5 から）

    version: "3.5"
    networks:
      frontend:
        name: custom_frontend
        driver: custom-driver-1

{% comment %}
For full details of the network configuration options available, see the following references:
{% endcomment %}
ネットワーク設定に関して利用可能なオプションについては、以下のリファレンスを参照してください。

{% comment %}
- [Top-level `networks` key](/compose/compose-file/compose-file-v2.md#network-configuration-reference)
- [Service-level `networks` key](/compose/compose-file/compose-file-v2.md#networks)
{% endcomment %}
- [最上位の `networks` キー](/compose/compose-file/compose-file-v2.md#network-configuration-reference)
- [service レベルの `networks` キー](/compose/compose-file/compose-file-v2.md#networks)

{% comment %}
## Configure the default network
{% endcomment %}
## デフォルトネットワークの設定
{: #configure-the-default-network }

{% comment %}
Instead of (or as well as) specifying your own networks, you can also change the settings of the app-wide default network by defining an entry under `networks` named `default`:
{% endcomment %}
独自のネットワーク設定は行わずに、あるいはそれを行った上でさらに、アプリに対するデフォルトネットワークの設定を変更することができます。
これは `networks` のもとに `default` という項目を定義して行います。

    {% comment %}
    version: "3"
    services:

      web:
        build: .
        ports:
          - "8000:8000"
      db:
        image: postgres

    networks:
      default:
        # Use a custom driver
        driver: custom-driver-1
    {% endcomment %}
    version: "3"
    services:

      web:
        build: .
        ports:
          - "8000:8000"
      db:
        image: postgres

    networks:
      default:
        # 独自のドライバーを利用
        driver: custom-driver-1

{% comment %}
## Use a pre-existing network
{% endcomment %}
## 既存ネットワークの利用
{: #use-a-pre-existing-network }

{% comment %}
If you want your containers to join a pre-existing network, use the [`external` option](/compose/compose-file/compose-file-v2.md#network-configuration-reference):
{% endcomment %}
コンテナーを既存のネットワークに接続したい場合は [`external` オプション](/compose/compose-file/compose-file-v2.md#network-configuration-reference) を利用します。

    networks:
      default:
        external:
          name: my-pre-existing-network

{% comment %}
Instead of attempting to create a network called `[projectname]_default`, Compose looks for a network called `my-pre-existing-network` and connect your app's containers to it.
{% endcomment %}
Compose は `[projectname]_default` という名前のネットワークを生成しようとはせず、`my-pre-existing-network` というネットワークを探し出して、アプリのコンテナーをそこに接続します。
