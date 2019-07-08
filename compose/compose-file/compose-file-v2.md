---
description: Compose ファイルリファレンス
keywords: fig, composition, compose version 3, docker
redirect_from:
- /compose/yml
title: Compose ファイル バージョン 2 リファレンス
toc_max: 4
toc_min: 1
---

{% comment %}
## Reference and guidelines
{% endcomment %}
## リファレンスとガイドライン
{: #reference-and-guidelines }

{% comment %}
These topics describe version 2 of the Compose file format.
{% endcomment %}
ここに示す内容は Compose ファイルフォーマット、バージョン 2 です。

{% comment %}
## Compose and Docker compatibility matrix
{% endcomment %}
## Compose と Docker の互換マトリックス
{: #compose-and-docker-compatibility-matrix }

{% comment %}
There are several versions of the Compose file format – 1, 2, 2.x, and 3.x The
table below is a quick look. For full details on what each version includes and
how to upgrade, see **[About versions and upgrading](compose-versioning.md)**.
{% endcomment %}
Compose ファイルフォーマットには 1、2、2.x、3.x という複数のバージョンがあります。
その様子は以下の一覧表に見ることができます。
各バージョンにて何が増えたのか、どのようにアップグレードしたのか、といった詳細については **[バージョンとアップグレードについて](compose-versioning.md)**を参照してください。

{% include content/compose-matrix.md %}

{% comment %}
## Service configuration reference
{% endcomment %}
## サービス設定リファレンス
{: #service-configuration-reference }

{% comment %}
The Compose file is a [YAML](http://yaml.org/) file defining
[services](#service-configuration-reference),
[networks](#network-configuration-reference) and
[volumes](#volume-configuration-reference).
The default path for a Compose file is `./docker-compose.yml`.
{% endcomment %}
Compose ファイルは [YAML](http://yaml.org/) 形式のファイルであり、[サービス（services）](#service-configuration-reference)、[ネットワーク（networks）](#network-configuration-reference)、[ボリューム（volumes）](#volume-configuration-reference)を定義します。
Compose ファイルのデフォルトパスは `./docker-compose.yml` です。

{% comment %}
>**Tip**: You can use either a `.yml` or `.yaml` extension for this file. They both work.
{% endcomment %}
>**ヒント**: このファイルの拡張子は `.yml` と `.yaml` のどちらでも構いません。
いずれでも動作します。

{% comment %}
A [container](/engine/reference/glossary.md#container) definition contains configuration which are applied to each
container started for that service, much like passing command-line parameters to
`docker run`. Likewise, network and volume definitions are analogous to
`docker network create` and `docker volume create`.
{% endcomment %}
[コンテナー](/engine/reference/glossary.md#container) の定義とは、対応するサービスを起動する各コンテナーに適用される設定を行うことです。
コマンドラインから `docker run` のパラメーターを受け渡すことと、非常によく似ています。
同様に、ネットワークの定義、ボリュームの定義は、それぞれ `docker network create` と `docker volume create` のコマンドに対応づくものです。

{% comment %}
As with `docker run`, options specified in the Dockerfile, such as `CMD`,
`EXPOSE`, `VOLUME`, `ENV`, are respected by default - you don't need to
specify them again in `docker-compose.yml`.
{% endcomment %}
`docker run` に関しても同じことが言えますが、Dockerfile にて指定された `CMD`、`EXPOSE`、`VOLUME`、`ENV` のようなオプションはデフォルトでは維持されます。したがって `docker-compose.yml` の中で再度設定する必要はありません。

{% comment %}
You can use environment variables in configuration values with a Bash-like
`${VARIABLE}` syntax - see [variable substitution](#variable-substitution) for
full details.
{% endcomment %}
設定を記述する際には環境変数を用いることができます。
環境変数は Bash 風に `${VARIABLE}` のように記述します。
詳しくは[変数の置換](#variable-substitution)を参照してください。

{% comment %}
This section contains a list of all configuration options supported by a service
definition in version 2.
{% endcomment %}
このセクションでは、バージョン 2 のサービス定義においてサポートされている設定オプションをすべて説明しています。

### blkio_config

{% comment %}
A set of configuration options to set block IO limits for this service.
{% endcomment %}
サービスのブロック IO に対する制限を行う設定オプションです。

    version: "{{ site.compose_file_v2 }}"
    services:
      foo:
        image: busybox
        blkio_config:
          weight: 300
          weight_device:
            - path: /dev/sda
              weight: 400
          device_read_bps:
            - path: /dev/sdb
              rate: '12mb'
          device_read_iops:
            - path: /dev/sdb
              rate: 120
          device_write_bps:
            - path: /dev/sdb
              rate: '1024k'
          device_write_iops:
            - path: /dev/sdb
              rate: 30

#### device_read_bps, device_write_bps

{% comment %}
Set a limit in bytes per second for read / write operations on a given device.
Each item in the list must have two keys:
{% endcomment %}
指定されたデバイス上での読み書き操作に対して、秒ごとのバイト上限値を設定します。
設定するリストの各項目には、以下の 2 つのキーが必要です。

{% comment %}
* `path`, defining the symbolic path to the affected device
* `rate`, either as an integer value representing the number of bytes or as
  a string expressing a [byte value](#specifying-byte-values).
{% endcomment %}
* `path`, 影響を受けるデバイスへのシンボリックパスを指定します。
* `rate`, バイト数を表わす整数値、または [バイト値](#specifying-byte-values) を表現する文字列を指定します。

#### device_read_iops, device_write_iops

{% comment %}
Set a limit in operations per second for read / write operations on a given
device. Each item in the list must have two keys:
{% endcomment %}
指定されたデバイス上での読み書き操作に対して、秒ごとの操作上限値を設定します。
設定するリストの各項目には、以下の 2 つのキーが必要です。

{% comment %}
* `path`, defining the symbolic path to the affected device
* `rate`, as an integer value representing the permitted number of operations
  per second.
{% endcomment %}
* `path`, 影響を受けるデバイスへのシンボリックパスを指定します。
* `rate`, 許容する操作数を表わす整数値を指定します。

#### weight

{% comment %}
Modify the proportion of bandwidth allocated to this service relative to other
services. Takes an integer value between 10 and 1000, with 500 being the
default.
{% endcomment %}
他のサービスと比べたときの、当サービスに割り当てる処理性能比を設定します。
10 から 1000 までの整数値を割り当てるもので、デフォルト値は 500 です。

#### weight_device

{% comment %}
Fine-tune bandwidth allocation by device. Each item in the list must have
two keys:
{% endcomment %}
デバイスへの処理割り当て量を調整します。
設定するリストの各項目には、以下の 2 つのキーが必要です。

{% comment %}
* `path`, defining the symbolic path to the affected device
* `weight`, an integer value between 10 and 1000
{% endcomment %}
* `path`, 影響を受けるデバイスへのシンボリックパスを指定します。
* `weight`, 10 から 1000 までの整数値を指定します。

### build

{% comment %}
Configuration options that are applied at build time.
{% endcomment %}
この設定オプションはビルド時に適用されます。

{% comment %}
`build` can be specified either as a string containing a path to the build
context, or an object with the path specified under [context](#context) and
optionally [dockerfile](#dockerfile) and [args](#args).
{% endcomment %}
`build` の指定は 1 つには、ビルドコンテキストへのパスを表わす文字列を指定します。
あるいは [context](#context) の指定のもとにパスを指定し、オプションとして [Dockerfile](#dockerfile) や [args](#args) を記述する方法をとります。

    build: ./dir

    build:
      context: ./dir
      dockerfile: Dockerfile-alternate
      args:
        buildno: 1

{% comment %}
If you specify `image` as well as `build`, then Compose names the built image
with the `webapp` and optional `tag` specified in `image`:
{% endcomment %}
`build` に加えて `image` も指定した場合、Compose はビルドイメージに名前をつけます。
たとえば以下のように `image` を指定すると、イメージ名を `webapp`、オプションのタグを `tag` という名前にします。

    build: ./dir
    image: webapp:tag

{% comment %}
This results in an image named `webapp` and tagged `tag`, built from `./dir`.
{% endcomment %}
結果としてイメージ名は `webapp` であり `tag` というタグづけが行われます。
そしてこのイメージは `./dir` から作り出されます。

#### cache_from

{% comment %}
> Added in [version 2.2](compose-versioning.md#version-22) file format
{% endcomment %}
> ファイルフォーマット[バージョン 2.2](compose-versioning.md#version-22) において追加されました。

{% comment %}
A list of images that the engine uses for cache resolution.
{% endcomment %}
エンジンがキャッシュ解決のために利用するイメージを設定します。

    build:
      context: .
      cache_from:
        - alpine:latest
        - corp/web_app:3.14

#### context

{% comment %}
> [Version 2 file format](compose-versioning.md#version-2) and up. In version 1, just use
> [build](#build).
{% endcomment %}
> [ファイルフォーマットバージョン 2](compose-versioning.md#version-2) またはそれ以降。
> バージョン 1 の場合は [build](#build) を用いてください。

{% comment %}
Either a path to a directory containing a Dockerfile, or a url to a git repository.
{% endcomment %}
Dockerfile を含むディレクトリへのパスか、あるいは git リポジトリの URL を設定します。

{% comment %}
When the value supplied is a relative path, it is interpreted as relative to the
location of the Compose file. This directory is also the build context that is
sent to the Docker daemon.
{% endcomment %}
設定された記述が相対パスを表わしている場合、Compose ファイルのあるディレクトリからの相対パスとして解釈されます。
このディレクトリはビルドコンテキストでもあり、Docker デーモンへ送信されるディレクトリです。

{% comment %}
Compose builds and tags it with a generated name, and use that image thereafter.
{% endcomment %}
Compose は指定された名前により、イメージのビルドとタグづけを行い、後々これを利用します。

    build:
      context: ./dir

#### dockerfile

{% comment %}
Alternate Dockerfile.
{% endcomment %}
別の Dockerfile を指定します。

{% comment %}
Compose uses an alternate file to build with. A build path must also be
specified.
{% endcomment %}
Compose は指定された別の Dockerfile を使ってビルドを行います。
このときは、ビルドパスを同時に指定しなければなりません。

    build:
      context: .
      dockerfile: Dockerfile-alternate

#### args

{% comment %}
> [Version 2 file format](compose-versioning.md#version-2) and up.
{% endcomment %}
> [ファイルフォーマットバージョン 2](compose-versioning.md#version-2) またはそれ以降。

{% comment %}
Add build arguments, which are environment variables accessible only during the
build process.
{% endcomment %}
ビルド引数を追加します。
これは環境変数であり、ビルド処理の間だけ利用可能なものです。

{% comment %}
First, specify the arguments in your Dockerfile:
{% endcomment %}
Dockerfile 内にてはじめにビルド引数を指定します。

    ARG buildno
    ARG gitcommithash

    RUN echo "Build number: $buildno"
    RUN echo "Based on commit: $gitcommithash"

{% comment %}
Then specify the arguments under the `build` key. You can pass a mapping
or a list:
{% endcomment %}
そして `build` キーのもとにその引数を指定します。
指定は個々をマッピングする形式か、リストとする形式が可能です。

    build:
      context: .
      args:
        buildno: 1
        gitcommithash: cdc3b19

    build:
      context: .
      args:
        - buildno=1
        - gitcommithash=cdc3b19

{% comment %}
> **Note**: In your Dockerfile, if you specify `ARG` before the `FROM` instruction,
> If you need an argument to be available in both places, also specify it under the `FROM` instruction.
> See [Understand how ARGS and FROM interact](/engine/reference/builder/#understand-how-arg-and-from-interact) for usage details.
{% endcomment %}
> **メモ**: Dockerfile にて `FROM` 命令の前に `ARG` 命令を指定した場合、`FROM` 以降のビルド命令において `ARG` の値は利用することができません。
> `FROM` の前後どこでも、そして特に `FROM` 命令の後でもその値を利用したい場合は、[ARG と FROM の関連について](/engine/reference/builder/#understand-how-arg-and-from-interact)を参照してください。

{% comment %}
You can omit the value when specifying a build argument, in which case its value
at build time is the value in the environment where Compose is running.
{% endcomment %}
ビルド引数の指定にあたって、その値設定を省略することができます。
この場合、ビルド時におけるその値は、Compose を起動している環境での値になります。

    args:
      - buildno
      - gitcommithash

{% comment %}
> **Note**: YAML boolean values (`true`, `false`, `yes`, `no`, `on`, `off`) must
> be enclosed in quotes, so that the parser interprets them as strings.
{% endcomment %}
> **メモ**: YAML のブール値 (`true`, `false`, `yes`, `no`, `on`, `off`) を用いる場合は、クォートで囲む必要があります。
> そうすることで、これらの値は文字列として解釈されます。

#### extra_hosts

{% comment %}
Add hostname mappings at build-time. Use the same values as the docker client `--add-host` parameter.
{% endcomment %}
ビルド時におけるホスト名のマッピングを追加します。
Docker Client の `--add-host` パラメーターと同じ値を設定してください。

    extra_hosts:
     - "somehost:162.242.195.82"
     - "otherhost:50.31.209.229"

{% comment %}
An entry with the ip address and hostname is created in `/etc/hosts` inside containers for this build, e.g:
{% endcomment %}
ホスト名と IP アドレスによるこの設定内容は、サービスコンテナー内の `/etc/hosts` に追加されます。
たとえば以下のとおりです。

    162.242.195.82  somehost
    50.31.209.229   otherhost

#### isolation

{% comment %}
> [Added in version 2.1 file format](compose-versioning.md#version-21).
{% endcomment %}
> [ファイルフォーマットバージョン 2.1](compose-versioning.md#version-21) において追加されました。

{% comment %}
Specify a build’s container isolation technology. On Linux, the only supported value
is `default`. On Windows, acceptable values are `default`, `process` and
`hyperv`. Refer to the
[Docker Engine docs](/engine/reference/commandline/run.md#specify-isolation-technology-for-container---isolation)
for details.
{% endcomment %}
ビルドにおけるコンテナーの分離技術（isolation technology）を設定します。
Linux においてサポートされるのは `default` のみです。
Windows では `default`, `process`, `hyperv` の設定が可能です。
詳しくは [Docker Engine ドキュメント](/engine/reference/commandline/run.md#specify-isolation-technology-for-container---isolation) を参照してください。

{% comment %}
If unspecified, Compose will use the `isolation` value found in the service's definition
to determine the value to use for builds.
{% endcomment %}
設定されていない場合、Compose は service 定義の中から `isolation` 値を見つけて、これをビルド時に利用する値とします。

#### labels

{% comment %}
> Added in [version 2.1](compose-versioning.md#version-21) file format
{% endcomment %}
> ファイルフォーマット[バージョン 2.1](compose-versioning.md#version-21) において追加されました。

{% comment %}
Add metadata to the resulting image using [Docker labels](/engine/userguide/labels-custom-metadata.md).
You can use either an array or a dictionary.
{% endcomment %}
[Docker labels](/engine/userguide/labels-custom-metadata.md) を使ってビルドされるイメージにメタデータを追加します。
配列形式と辞書形式のいずれかにより指定します。

{% comment %}
It's recommended that you use reverse-DNS notation to prevent your labels from conflicting with
those used by other software.
{% endcomment %}
他のソフトウェアが用いるラベルとの競合を避けるため、逆 DNS 記法とすることをお勧めします。

    build:
      context: .
      labels:
        com.example.description: "Accounting webapp"
        com.example.department: "Finance"
        com.example.label-with-empty-value: ""


    build:
      context: .
      labels:
        - "com.example.description=Accounting webapp"
        - "com.example.department=Finance"
        - "com.example.label-with-empty-value"

#### network

{% comment %}
> Added in [version 2.2](compose-versioning.md#version-22) file format
{% endcomment %}
> ファイルフォーマット[バージョン 2.2](compose-versioning.md#version-22) において追加されました。

{% comment %}
Set the network containers connect to for the `RUN` instructions during
build.
{% endcomment %}
ビルド時の `RUN` 命令において接続するネットワークコンテナーを設定します。

    build:
      context: .
      network: host


    build:
      context: .
      network: custom_network_1

#### shm_size

{% comment %}
> Added in [version 2.3](compose-versioning.md#version-23) file format
{% endcomment %}
> ファイルフォーマット[バージョン 2.3](compose-versioning.md#version-23) において追加されました。

{% comment %}
Set the size of the `/dev/shm` partition for this build's containers. Specify
as an integer value representing the number of bytes or as a string expressing
a [byte value](#specifying-byte-values).
{% endcomment %}
このビルドコンテナーにおける `/dev/shm` パーティションのサイズを設定します。
指定する値は、バイト数を表わす整数値か、あるいは[バイト表現](#specifying-byte-values)によって表わされる文字列とします。

    build:
      context: .
      shm_size: '2gb'


    build:
      context: .
      shm_size: 10000000

#### target

{% comment %}
> Added in [version 2.3](compose-versioning.md#version-23) file format
{% endcomment %}
> ファイルフォーマット[バージョン 2.3](compose-versioning.md#version-23) において追加されました。

{% comment %}
Build the specified stage as defined inside the `Dockerfile`. See the
[multi-stage build docs](/engine/userguide/eng-image/multistage-build.md) for
details.
{% endcomment %}
`Dockerfile` 内部に定義されている特定のステージをビルドする方法は、[マルチステージビルド](/engine/userguide/eng-image/multistage-build.md)を参照してください。

      build:
        context: .
        target: prod

### cap_add, cap_drop

{% comment %}
Add or drop container capabilities.
See `man 7 capabilities` for a full list.
{% endcomment %}
コンテナーケーパビリティーの機能を追加または削除します。
詳細な一覧は `man 7 capabilities` を参照してください。

    cap_add:
      - ALL

    cap_drop:
      - NET_ADMIN
      - SYS_ADMIN

### command

{% comment %}
Override the default command.
{% endcomment %}
デフォルトコマンドを上書きします。

    command: bundle exec thin -p 3000

{% comment %}
The command can also be a list, in a manner similar to
[dockerfile](/engine/reference/builder.md#cmd):
{% endcomment %}
このコマンドは [dockerfile](/engine/reference/builder.md#cmd) の場合と同じように、リスト形式により指定することもできます。

    command: ["bundle", "exec", "thin", "-p", "3000"]

### cgroup_parent

{% comment %}
Specify an optional parent cgroup for the container.
{% endcomment %}
コンテナーに対して、オプションで指定する親の cgroup を指定します。

    cgroup_parent: m-executor-abcd

### container_name

{% comment %}
Specify a custom container name, rather than a generated default name.
{% endcomment %}
デフォルトのコンテナー名ではない、独自のコンテナー名を設定します。

    container_name: my-web-container

{% comment %}
Because Docker container names must be unique, you cannot scale a service
beyond 1 container if you have specified a custom name. Attempting to do so
results in an error.
{% endcomment %}
Docker コンテナー名はユニークである必要があります。
そこで独自のコンテナー名を設定したときは、サービスをスケールアップして複数コンテナーとすることはできません。
これを行うとエラーが発生します。

### cpu_rt_runtime, cpu_rt_period

{% comment %}
> Added in [version 2.2](compose-versioning.md#version-22) file format
{% endcomment %}
> ファイルフォーマット[バージョン 2.2](compose-versioning.md#version-22) において追加されました。

{% comment %}
Configure CPU allocation parameters using the Docker daemon realtime scheduler.
{% endcomment %}
Docker デーモンのリアルタイムスケジューラーが利用する CPU 割り当てパラメーターを設定します。

    {% comment %}
    cpu_rt_runtime: '400ms'
    cpu_rt_period: '1400us'

    # Integer values will use microseconds as units
    cpu_rt_runtime: 95000
    cpu_rt_period: 11000
    {% endcomment %}
    cpu_rt_runtime: '400ms'
    cpu_rt_period: '1400us'

    # マイクロ秒単位で整数値を指定します。
    cpu_rt_runtime: 95000
    cpu_rt_period: 11000


### device_cgroup_rules

{% comment %}
> [Added in version 2.3 file format](compose-versioning.md#version-23).
{% endcomment %}
> ファイルフォーマット[バージョン 2.3](compose-versioning.md#version-23) において追加されました。

{% comment %}
Add rules to the cgroup allowed devices list.
{% endcomment %}
cgroup としてアクセス許可されているデバイスリストにルールを追加します。

    device_cgroup_rules:
      - 'c 1:3 mr'
      - 'a 7:* rmw'

### devices

{% comment %}
List of device mappings.  Uses the same format as the `--device` docker
client create option.
{% endcomment %}
デバイスのマッピングをリスト形式で設定します。
Docker クライアントの create オプションの `--device` と同じ書式にします。

    devices:
      - "/dev/ttyUSB0:/dev/ttyUSB0"

### depends_on

{% comment %}
> [Version 2 file format](compose-versioning.md#version-2) and up.
{% endcomment %}
> [ファイルフォーマットバージョン 2](compose-versioning.md#version-2) またはそれ以降。

{% comment %}
Express dependency between services, which has two effects:
{% endcomment %}
サービス間の依存関係を表わします。
これにより以下の 2 つの効果が表れます。

{% comment %}
- `docker-compose up` starts services in dependency order. In the following
  example, `db` and `redis` are started before `web`.
{% endcomment %}
- `docker-compose up` は依存関係の順にサービスを起動します。
  以下の例において `db` と `redis` は `web` の後に起動します。

{% comment %}
- `docker-compose up SERVICE` automatically include `SERVICE`'s
  dependencies. In the following example, `docker-compose up web` also
  create and start `db` and `redis`.
{% endcomment %}
- `docker-compose up SERVICE` を実行すると `SERVICE` における依存関係をもとに動作します。
  以下の例において `docker-compose up web` を実行すると `db` と `redis` を生成して起動します。

{% comment %}
Simple example:
{% endcomment %}
以下がその簡単な例です。

    version: "{{ site.compose_file_v2 }}"
    services:
      web:
        build: .
        depends_on:
          - db
          - redis
      redis:
        image: redis
      db:
        image: postgres

{% comment %}
> **Note**: `depends_on` does not wait for `db` and `redis` to be "ready" before
> starting `web` - only until they have been started. If you need to wait
> for a service to be ready, see [Controlling startup order](/compose/startup-order.md)
> for more on this problem and strategies for solving it.
{% endcomment %}
> **メモ**: `depends_on` では `db` や `redis` が「準備」状態になるのを待たずに、つまりそれらを開始したらすぐに `web` を起動します。
>   準備状態になるのを待ってから次のサービスを起動することが必要な場合は、[Compose における起動順の制御](/compose/startup-order.md)にて示す内容と解決方法を確認してください。


{% comment %}
> [Added in version 2.1 file format](compose-versioning.md#version-21).
{% endcomment %}
### healthcheck

> ファイルフォーマット[バージョン 2.1](compose-versioning.md#version-21) において追加されました。

{% comment %}
A healthcheck indicates that you want a dependency to wait
for another container to be "healthy" (as indicated by a successful state from
the healthcheck) before starting.
{% endcomment %}
A healthcheck indicates that you want a dependency to wait
for another container to be "healthy" (as indicated by a successful state from
the healthcheck) before starting.

{% comment %}
Example:
{% endcomment %}
Example:

    version: "{{ site.compose_file_v2 }}"
    services:
      web:
        build: .
        depends_on:
          db:
            condition: service_healthy
          redis:
            condition: service_started
      redis:
        image: redis
      db:
        image: redis
        healthcheck:
          test: "exit 0"

{% comment %}
In the above example, Compose waits for the `redis` service to be started
(legacy behavior) and the `db` service to be healthy before starting `web`.
{% endcomment %}
In the above example, Compose waits for the `redis` service to be started
(legacy behavior) and the `db` service to be healthy before starting `web`.

{% comment %}
See the [healthcheck section](#healthcheck) for complementary
information.
{% endcomment %}
See the [healthcheck section](#healthcheck) for complementary
information.

### dns

{% comment %}
Custom DNS servers. Can be a single value or a list.
{% endcomment %}
DNS サーバーを設定します。
設定は 1 つだけとするか、リストにすることができます。

    dns: 8.8.8.8
    dns:
      - 8.8.8.8
      - 9.9.9.9

### dns_opt

{% comment %}
List of custom DNS options to be added to the container's `resolv.conf` file.
{% endcomment %}
List of custom DNS options to be added to the container's `resolv.conf` file.

    dns_opt:
      - use-vc
      - no-tld-query

### dns_search

{% comment %}
Custom DNS search domains. Can be a single value or a list.
{% endcomment %}
DNS 検索ドメインを設定します。
設定は 1 つだけとするか、リストにすることができます。

    dns_search: example.com
    dns_search:
      - dc1.example.com
      - dc2.example.com

### tmpfs

{% comment %}
Mount a temporary file system inside the container. Can be a single value or a list.
{% endcomment %}
コンテナー内においてテンポラリファイルシステムをマウントします。
設定は 1 つだけとするか、リストにすることができます。

    tmpfs: /run
    tmpfs:
      - /run
      - /tmp

### entrypoint

{% comment %}
Override the default entrypoint.
{% endcomment %}
デフォルトのエントリーポイントを上書きします。

    entrypoint: /code/entrypoint.sh

{% comment %}
The entrypoint can also be a list, in a manner similar to
[dockerfile](/engine/reference/builder.md#entrypoint):
{% endcomment %}
エントリーポイントはリスト形式で設定することができます。
その指定方法は [Dockerfile](/engine/reference/builder.md#entrypoint) と同様です。

    entrypoint:
        - php
        - -d
        - zend_extension=/usr/local/lib/php/extensions/no-debug-non-zts-20100525/xdebug.so
        - -d
        - memory_limit=-1
        - vendor/bin/phpunit

{% comment %}
> **Note**: Setting `entrypoint` both overrides any default entrypoint set
> on the service's image with the `ENTRYPOINT` Dockerfile instruction, *and*
> clears out any default command on the image - meaning that if there's a `CMD`
> instruction in the Dockerfile, it is ignored.
{% endcomment %}
> **メモ**: `entrypoint` を設定すると、サービスイメージ内に Dockerfile 命令の `ENTRYPOINT` によって設定されているデフォルトのエントリーポイントは上書きされ、**さらに**イメージ内のあらゆるデフォルトコマンドもクリアされます。
> これはつまり、Dockerfile に `CMD` 命令があったとしたら無視されるということです。

### env_file

{% comment %}
Add environment variables from a file. Can be a single value or a list.
{% endcomment %}
ファイルを用いて環境変数を追加します。
設定は 1 つだけとするか、リストにすることができます。

{% comment %}
If you have specified a Compose file with `docker-compose -f FILE`, paths in
`env_file` are relative to the directory that file is in.
{% endcomment %}
Compose ファイルを `docker-compose -f FILE` という起動により指定している場合、`env_file` におけるパスは、Compose ファイルがあるディレクトリからの相対パスとします。

{% comment %}
Environment variables declared in the [environment](#environment) section
_override_ these values &ndash; this holds true even if those values are
empty or undefined.
{% endcomment %}
環境変数が [environment](#environment) の項に宣言されていれば、ここでの設定を**オーバーライド**します。
たとえ設定値が空や未定義であっても、これは変わりません。

    env_file: .env

    env_file:
      - ./common.env
      - ./apps/web.env
      - /opt/secrets.env

{% comment %}
Compose expects each line in an env file to be in `VAR=VAL` format. Lines
beginning with `#` are processed as comments and are ignored. Blank lines are
also ignored.
{% endcomment %}
env ファイルの各行は `VAR=VAL` の書式とします。
行先頭に `#` があると、コメント行となり無視されます。
空行も無視されます。

    # Set Rails/Rack environment
    RACK_ENV=development

{% comment %}
> **Note**: If your service specifies a [build](#build) option, variables
> defined in environment files are _not_ automatically visible during the
> build. Use the [args](#args) sub-option of `build` to define build-time
> environment variables.
{% endcomment %}
> **メモ**: サービスに [build](#build) オプションを指定している場合、env ファイル内に定義された変数は、ビルド時にこのままでは自動的に参照されません。
> その場合は `build` のサブオプション [args](#args) を利用して、ビルド時の環境変数を設定してください。

{% comment %}
The value of `VAL` is used as is and not modified at all. For example if the
value is surrounded by quotes (as is often the case of shell variables), the
quotes are included in the value passed to Compose.
{% endcomment %}
`VAL` の値は記述されたとおりに用いられ、一切修正はされません。
たとえば値がクォートにより囲まれている（よくシェル変数に対して行う）場合、クォートもそのまま値として Compose に受け渡されます。

{% comment %}
Keep in mind that _the order of files in the list is significant in determining
the value assigned to a variable that shows up more than once_. The files in the
list are processed from the top down. For the same variable specified in file
`a.env` and assigned a different value in file `b.env`, if `b.env` is
listed below (after), then the value from `b.env` stands. For example, given the
following declaration in `docker-compose.yml`:
{% endcomment %}
ファイルを複数用いる場合の順番には気をつけてください。
特に何度も出現する変数に対して、値がどのように決定されるかです。
ファイルが複数指定された場合、その処理は上から順に行われます。
たとえば `a.env` ファイルに変数が指定されていて、`b.env` ファイルには同じ変数が異なる値で定義されていたとします。
ここで `b.env` ファイルが下に（後に）指定されているとします。
このとき変数の値は `b.env` のものが採用されます。
さらに例として `docker-compose.yml` に以下のような宣言があったとします。

```yaml
services:
  some-service:
    env_file:
      - a.env
      - b.env
```

{% comment %}
And the following files:
{% endcomment %}
ファイルの内容は以下であるとします。

```none
# a.env
VAR=1
```

{% comment %}
and
{% endcomment %}

```none
# b.env
VAR=hello
```

{% comment %}
$VAR is `hello`.
{% endcomment %}
この結果 `$VAR` は `hello` になります。

### environment

{% comment %}
Add environment variables. You can use either an array or a dictionary. Any
boolean values; true, false, yes no, need to be enclosed in quotes to ensure
they are not converted to True or False by the YML parser.
{% endcomment %}
環境変数を追加します。
配列形式または辞書形式での指定が可能です。
ブール値 `true`, `false`, `yes`, `no` を用いる場合は、クォートで囲むことで YML パーサーによって True や False に変換されてしまうのを防ぐ必要があります。

{% comment %}
Environment variables with only a key are resolved to their values on the
machine Compose is running on, which can be helpful for secret or host-specific values.
{% endcomment %}
環境変数だけが記述されている場合は、Compose が起動しているマシン上にて定義されている値が設定されます。
これは機密情報やホスト固有の値を設定する場合に利用できます。

    environment:
      RACK_ENV: development
      SHOW: 'true'
      SESSION_SECRET:

    environment:
      - RACK_ENV=development
      - SHOW=true
      - SESSION_SECRET

{% comment %}
> **Note**: If your service specifies a [build](#build) option, variables
> defined in `environment` are _not_ automatically visible during the
> build. Use the [args](#args) sub-option of `build` to define build-time
> environment variables.
{% endcomment %}
> **メモ**: サービスに [build](#build) オプションを指定している場合、env ファイル内に定義された変数は、ビルド時にこのままでは自動的に参照されません。
> その場合は `build` のサブオプション [args](#args) を利用して、ビルド時の環境変数を設定してください。

### expose

{% comment %}
Expose ports without publishing them to the host machine - they'll only be
accessible to linked services. Only the internal port can be specified.
{% endcomment %}
ホストマシンにはポートを公開せずに、ポートを expose します。
これはリンクされたサービスのみアクセスが可能になります。
内部ポートのみが指定できます。

    expose:
     - "3000"
     - "8000"

### extends

{% comment %}
Extend another service, in the current file or another, optionally overriding
configuration.
{% endcomment %}
現ファイルや別のファイルにある他のサービスを拡張します。
必要に応じて設定を上書きすることもできます。

{% comment %}
You can use `extends` on any service together with other configuration keys.
The `extends` value must be a dictionary defined with a required `service`
and an optional `file` key.
{% endcomment %}
別のサービスを `extends` により拡張する際には、合わせて他の設定キーを指定することができます。
`extends` に設定する値は辞書形式であり、`service` キーが必須です。
また必要に応じて指定する `file` キーがあります。

    extends:
      file: common.yml
      service: webapp

{% comment %}
The `service` the name of the service being extended, for example
`web` or `database`. The `file` is the location of a Compose configuration
file defining that service.
{% endcomment %}
`service` は拡張するサービスの名前を指定します。
たとえば `web` や `database` です。
`file` は、サービスを定義する Compose 設定ファイルのパスを指定します。

{% comment %}
If you omit the `file` Compose looks for the service configuration in the
current file. The `file` value can be an absolute or relative path. If you
specify a relative path, Compose treats it as relative to the location of the
current file.
{% endcomment %}
`file` の指定を省略した場合、Compose はそのサービス設定を現ファイルの中から探します。
`file` に指定する値は絶対パス、相対パスのいずれでも構いません。
相対パスを指定した場合、Compose は現ファイルのあるディレクトリからの相対パスとして扱います。

{% comment %}
You can extend a service that itself extends another. You can extend
indefinitely. Compose does not support circular references and `docker-compose`
returns an error if it encounters one.
{% endcomment %}
拡張するサービスそのものが、他のサービスを拡張したものも指定可能です。
拡張の繰り返しはいくらでもできます。
ただし Compose は循環参照をサポートしていないため、そのような状況が発生した場合は `docker-compose` がエラーを返します。

{% comment %}
For more on `extends`, see the
[the extends documentation](/compose/extends.md#extending-services).
{% endcomment %}
`extends` に関する詳細は [extends ドキュメント](/compose/extends.md#extending-services) を参照してください。

### external_links

Link to containers started outside this `docker-compose.yml` or even outside
of Compose, especially for containers that provide shared or common services.
`external_links` follow semantics similar to `links` when specifying both the
container name and the link alias (`CONTAINER:ALIAS`).

    external_links:
     - redis_1
     - project_db_1:mysql
     - project_db_1:postgresql

> **Note**: For version 2 file format, the
> externally-created containers must be connected to at least one of the same
> networks as the service which is linking to them.

### extra_hosts

Add hostname mappings. Use the same values as the docker client `--add-host` parameter.

    extra_hosts:
     - "somehost:162.242.195.82"
     - "otherhost:50.31.209.229"

An entry with the ip address and hostname is created in `/etc/hosts` inside containers for this service, e.g:

    162.242.195.82  somehost
    50.31.209.229   otherhost

### group_add

Specify additional groups (by name or number) which the user inside the
container should be a member of. Groups must exist in both the container and the
host system to be added. An example of where this is useful is when multiple
containers (running as different users) need to all read or write the same
file on the host system. That file can be owned by a group shared by all the
containers, and specified in `group_add`. See the
[Docker documentation](/engine/reference/run.md#additional-groups) for more
details.

A full example:

```
version: "{{ site.compose_file_v2 }}"
services:
  myservice:
    image: alpine
    group_add:
      - mail
```

Running `id` inside the created container shows that the user belongs to
the `mail` group, which would not have been the case if `group_add` were not
used.

### healthcheck

> [Version 2.1 file format](compose-versioning.md#version-21) and up.

Configure a check that's run to determine whether or not containers for this
service are "healthy". See the docs for the
[HEALTHCHECK Dockerfile instruction](/engine/reference/builder.md#healthcheck)
for details on how healthchecks work.

    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost"]
      interval: 1m30s
      timeout: 10s
      retries: 3
      start_period: 40s

`interval`, `timeout` and `start_period` are specified as
[durations](#specifying-durations).

`test` must be either a string or a list. If it's a list, the first item must be
either `NONE`, `CMD` or `CMD-SHELL`. If it's a string, it's equivalent to
specifying `CMD-SHELL` followed by that string.

    # Hit the local web app
    test: ["CMD", "curl", "-f", "http://localhost"]

    # As above, but wrapped in /bin/sh. Both forms below are equivalent.
    test: ["CMD-SHELL", "curl -f http://localhost && echo 'cool, it works'"]
    test: curl -f https://localhost && echo 'cool, it works'

To disable any default healthcheck set by the image, you can use `disable:
true`. This is equivalent to specifying `test: ["NONE"]`.

    healthcheck:
      disable: true

> **Note**: The `start_period` option is a more recent feature and is only
> available with the [2.3 file format](compose-versioning.md#version-23).

### image

Specify the image to start the container from. Can either be a repository/tag or
a partial image ID.

    image: redis
    image: ubuntu:14.04
    image: tutum/influxdb
    image: example-registry.com:4000/postgresql
    image: a4bc65fd

If the image does not exist, Compose attempts to pull it, unless you have also
specified [build](#build), in which case it builds it using the specified
options and tags it with the specified tag.

### init

> [Added in version 2.2 file format](compose-versioning.md#version-22).

Run an init inside the container that forwards signals and reaps processes.
Set this option to `true` to enable this feature for the service.

    version: "{{ site.compose_file_v2 }}"
    services:
      web:
        image: alpine:latest
        init: true

> The default init binary that is used is [Tini](https://github.com/krallin/tini),
> and is installed in `/usr/libexec/docker-init` on the daemon host. You can
> configure the daemon to use a custom init binary through the
> [`init-path` configuration option](/engine/reference/commandline/dockerd/#daemon-configuration-file).


### isolation

> [Added in version 2.1 file format](compose-versioning.md#version-21).

Specify a container’s isolation technology. On Linux, the only supported value
is `default`. On Windows, acceptable values are `default`, `process` and
`hyperv`. Refer to the
[Docker Engine docs](/engine/reference/commandline/run.md#specify-isolation-technology-for-container---isolation)
for details.

### labels

Add metadata to containers using [Docker labels](/engine/userguide/labels-custom-metadata.md). You can use either an array or a dictionary.

It's recommended that you use reverse-DNS notation to prevent your labels from conflicting with those used by other software.

    labels:
      com.example.description: "Accounting webapp"
      com.example.department: "Finance"
      com.example.label-with-empty-value: ""

    labels:
      - "com.example.description=Accounting webapp"
      - "com.example.department=Finance"
      - "com.example.label-with-empty-value"

### links

Link to containers in another service. Either specify both the service name and
a link alias (`"SERVICE:ALIAS"`), or just the service name.

> Links are a legacy option. We recommend using
> [networks](#networks) instead.

    web:
      links:
       - "db"
       - "db:database"
       - "redis"

Containers for the linked service are reachable at a hostname identical to
the alias, or the service name if no alias was specified.

Links also express dependency between services in the same way as
[depends_on](#depends_on), so they determine the order of service startup.

> **Note**: If you define both links and [networks](#networks), services with
> links between them must share at least one network in common in order to
> communicate. We recommend using networks instead.

### logging


Logging configuration for the service.

    logging:
      driver: syslog
      options:
        syslog-address: "tcp://192.168.0.42:123"

The `driver`  name specifies a logging driver for the service's
containers, as with the ``--log-driver`` option for docker run
([documented here](/engine/admin/logging/overview.md)).

The default value is json-file.

    driver: "json-file"
    driver: "syslog"
    driver: "none"

> **Note**: Only the `json-file` and `journald` drivers make the logs available directly from
> `docker-compose up` and `docker-compose logs`. Using any other driver does not
> print any logs.

Specify logging options for the logging driver with the ``options`` key, as with the ``--log-opt`` option for `docker run`.

Logging options are key-value pairs. An example of `syslog` options:

    driver: "syslog"
    options:
      syslog-address: "tcp://192.168.0.42:123"

### network_mode

> [Version 2 file format](compose-versioning.md#version-2) and up. Replaces the version 1 [net](compose-file-v1.md#net) option.

Network mode. Use the same values as the docker client `--net` parameter, plus
the special form `service:[service name]`.

    network_mode: "bridge"
    network_mode: "host"
    network_mode: "none"
    network_mode: "service:[service name]"
    network_mode: "container:[container name/id]"

### networks

> [Version 2 file format](compose-versioning.md#version-2) and up. Replaces the version 1 [net](compose-file-v1.md#net) option.

Networks to join, referencing entries under the
[top-level `networks` key](#network-configuration-reference).

    services:
      some-service:
        networks:
         - some-network
         - other-network

#### aliases

Aliases (alternative hostnames) for this service on the network. Other containers on the same network can use either the service name or this alias to connect to one of the service's containers.

Since `aliases` is network-scoped, the same service can have different aliases on different networks.

> **Note**: A network-wide alias can be shared by multiple containers, and even by multiple services. If it is, then exactly which container the name resolves to is not guaranteed.

The general format is shown here.

    services:
      some-service:
        networks:
          some-network:
            aliases:
             - alias1
             - alias3
          other-network:
            aliases:
             - alias2

In the example below, three services are provided (`web`, `worker`, and `db`), along with two networks (`new` and `legacy`). The `db` service is reachable at the hostname `db` or `database` on the `new` network, and at `db` or `mysql` on the `legacy` network.

    version: "{{ site.compose_file_v2 }}"

    services:
      web:
        build: ./web
        networks:
          - new

      worker:
        build: ./worker
        networks:
          - legacy

      db:
        image: mysql
        networks:
          new:
            aliases:
              - database
          legacy:
            aliases:
              - mysql

    networks:
      new:
      legacy:

#### ipv4_address, ipv6_address

Specify a static IP address for containers for this service when joining the network.

The corresponding network configuration in the [top-level networks section](#network-configuration-reference) must have an `ipam` block with subnet and gateway configurations covering each static address. If IPv6 addressing is desired, the [`enable_ipv6`](#enableipv6) option must be set.

An example:

    version: "{{ site.compose_file_v2 }}"

    services:
      app:
        image: busybox
        command: ifconfig
        networks:
          app_net:
            ipv4_address: 172.16.238.10
            ipv6_address: 2001:3984:3989::10

    networks:
      app_net:
        driver: bridge
        enable_ipv6: true
        ipam:
          driver: default
          config:
          - subnet: 172.16.238.0/24
            gateway: 172.16.238.1
          - subnet: 2001:3984:3989::/64
            gateway: 2001:3984:3989::1

#### link_local_ips

> [Added in version 2.1 file format](compose-versioning.md#version-21).

Specify a list of link-local IPs. Link-local IPs are special IPs which belong
to a well known subnet and are purely managed by the operator, usually
dependent on the architecture where they are deployed. Therefore they are not
managed by docker (IPAM driver).

Example usage:

    version: "{{ site.compose_file_v2 }}"
    services:
      app:
        image: busybox
        command: top
        networks:
          app_net:
            link_local_ips:
              - 57.123.22.11
              - 57.123.22.13
    networks:
      app_net:
        driver: bridge

#### priority

Specify a priority to indicate in which order Compose should connect the
service's containers to its networks. If unspecified, the default value is `0`.

In the following example, the `app` service connects to `app_net_1` first
as it has the highest priority. It then connects to `app_net_3`, then
`app_net_2`, which uses the default priority value of `0`.

    version: "{{ site.compose_file_v2 }}"
    services:
      app:
        image: busybox
        command: top
        networks:
          app_net_1:
            priority: 1000
          app_net_2:

          app_net_3:
            priority: 100
    networks:
      app_net_1:
      app_net_2:
      app_net_3:

> **Note**: If multiple networks have the same priority, the connection order
> is undefined.

### pid

    pid: "host"
    pid: "container:custom_container_1"
    pid: "service:foobar"

If set to one of the following forms: `container:<container_name>`,
`service:<service_name>`, the service shares the PID address space of the
designated container or service.

If set to "host", the service's PID mode is the host PID mode.  This turns
on sharing between container and the host operating system the PID address
space. Containers launched with this flag can access and manipulate
other containers in the bare-metal machine's namespace and vice versa.

> **Note**: the `service:` and `container:` forms require
> [version 2.1](compose-versioning.md#version-21) or above

### pids_limit

> [Added in version 2.1 file format](compose-versioning.md#version-21).

Tunes a container's PIDs limit. Set to `-1` for unlimited PIDs.

    pids_limit: 10


### platform

> [Added in version 2.4 file format](compose-versioning.md#version-24).

Target platform containers for this service will run on, using the
`os[/arch[/variant]]` syntax, e.g.

    platform: osx
    platform: windows/amd64
    platform: linux/arm64/v8

This parameter determines which version of the image will be pulled and/or
on which platform the service's build will be performed.

### ports

Expose ports. Either specify both ports (`HOST:CONTAINER`), or just the container
port (an ephemeral host port is chosen).

> **Note**: When mapping ports in the `HOST:CONTAINER` format, you may experience
> erroneous results when using a container port lower than 60, because YAML
> parses numbers in the format `xx:yy` as a base-60 value. For this reason,
> we recommend always explicitly specifying your port mappings as strings.

    ports:
     - "3000"
     - "3000-3005"
     - "8000:8000"
     - "9090-9091:8080-8081"
     - "49100:22"
     - "127.0.0.1:8001:8001"
     - "127.0.0.1:5000-5010:5000-5010"
     - "6060:6060/udp"
     - "12400-12500:1240"

### runtime

> [Added in version 2.3 file format](compose-versioning.md#version-23)

Specify which runtime to use for the service's containers. Default runtime
and available runtimes are listed in the output of `docker info`.

    web:
      image: busybox:latest
      command: true
      runtime: runc


### scale

> [Added in version 2.2 file format](compose-versioning.md#version-22)

Specify the default number of containers to deploy for this service. Whenever
you run `docker-compose up`, Compose creates or removes containers to match
the specified number. This value can be overridden using the
[`--scale`](/compose/reference/up.md) flag.

    web:
      image: busybox:latest
      command: echo 'scaled'
      scale: 3

### security_opt

Override the default labeling scheme for each container.

    security_opt:
      - label:user:USER
      - label:role:ROLE

### stop_grace_period

Specify how long to wait when attempting to stop a container if it doesn't
handle SIGTERM (or whatever stop signal has been specified with
[`stop_signal`](#stopsignal)), before sending SIGKILL. Specified
as a [duration](#specifying-durations).

    stop_grace_period: 1s
    stop_grace_period: 1m30s

By default, `stop` waits 10 seconds for the container to exit before sending
SIGKILL.

### stop_signal

Sets an alternative signal to stop the container. By default `stop` uses
SIGTERM. Setting an alternative signal using `stop_signal` causes
`stop` to send that signal instead.

    stop_signal: SIGUSR1

### storage_opt

> [Added in version 2.1 file format](compose-versioning.md#version-21).

Set storage driver options for this service.

    storage_opt:
      size: '1G'

### sysctls

> [Added in version 2.1 file format](compose-versioning.md#version-21).

Kernel parameters to set in the container. You can use either an array or a
dictionary.

    sysctls:
      net.core.somaxconn: 1024
      net.ipv4.tcp_syncookies: 0

    sysctls:
      - net.core.somaxconn=1024
      - net.ipv4.tcp_syncookies=0

### ulimits

Override the default ulimits for a container. You can either specify a single
limit as an integer or soft/hard limits as a mapping.


    ulimits:
      nproc: 65535
      nofile:
        soft: 20000
        hard: 40000

### userns_mode

> [Added in version 2.1 file format](compose-versioning.md#version-21).

    userns_mode: "host"

Disables the user namespace for this service, if Docker daemon is configured with user namespaces.
See [dockerd](/engine/reference/commandline/dockerd.md#disable-user-namespace-for-a-container) for
more information.

### volumes

Mount host folders or named volumes. Named volumes need to be specified with the
[top-level `volumes` key](#volume-configuration-reference).

You can mount a relative path on the host, which expands relative to
the directory of the Compose configuration file being used. Relative paths
should always begin with `.` or `..`.

#### Short syntax

The short syntax uses the generic `[SOURCE:]TARGET[:MODE]` format, where
`SOURCE` can be either a host path or volume name. `TARGET` is the container
path where the volume is mounted. Standard modes are `ro` for read-only
and `rw` for read-write (default).

    volumes:
      # Just specify a path and let the Engine create a volume
      - /var/lib/mysql

      # Specify an absolute path mapping
      - /opt/data:/var/lib/mysql

      # Path on the host, relative to the Compose file
      - ./cache:/tmp/cache

      # User-relative path
      - ~/configs:/etc/configs/:ro

      # Named volume
      - datavolume:/var/lib/mysql

#### Long syntax

> [Added in version 2.3 file format](compose-versioning.md#version-23).

The long form syntax allows the configuration of additional fields that can't be
expressed in the short form.

- `type`: the mount type `volume`, `bind`, `tmpfs` or `npipe`
- `source`: the source of the mount, a path on the host for a bind mount, or the
  name of a volume defined in the
  [top-level `volumes` key](#volume-configuration-reference). Not applicable for a tmpfs mount.
- `target`: the path in the container where the volume is mounted
- `read_only`: flag to set the volume as read-only
- `bind`: configure additional bind options
  - `propagation`: the propagation mode used for the bind
- `volume`: configure additional volume options
  - `nocopy`: flag to disable copying of data from a container when a volume is
    created
- `tmpfs`: configure additional tmpfs options
  - `size`: the size for the tmpfs mount in bytes


```none
version: "{{ site.compose_file_v2 }}"
services:
  web:
    image: nginx:alpine
    ports:
      - "80:80"
    volumes:
      - type: volume
        source: mydata
        target: /data
        volume:
          nocopy: true
      - type: bind
        source: ./static
        target: /opt/app/static

networks:
  webnet:

volumes:
  mydata:
```

> **Note**: When creating bind mounts, using the long syntax requires the
> referenced folder to be created beforehand. Using the short syntax
> creates the folder on the fly if it doesn't exist.
> See the [bind mounts documentation](/engine/admin/volumes/bind-mounts.md/#differences-between--v-and---mount-behavior)
> for more information.

### volume\_driver

Specify a default volume driver to be used for all declared volumes on this
service.

    volume_driver: mydriver

> **Note**: In [version 2 files](compose-versioning.md#version-2), this
> option only applies to anonymous volumes (those specified in the image,
> or specified under `volumes` without an explicit named volume or host path).
> To configure the driver for a named volume, use the `driver` key under the
> entry in the [top-level `volumes` option](#volume-configuration-reference).


See [Docker Volumes](/engine/userguide/dockervolumes.md) and
[Volume Plugins](/engine/extend/plugins_volume.md) for more information.

### volumes_from

Mount all of the volumes from another service or container, optionally
specifying read-only access (``ro``) or read-write (``rw``). If no access level is specified,
then read-write is used.

    volumes_from:
     - service_name
     - service_name:ro
     - container:container_name
     - container:container_name:rw

> **Notes**
>
>* The `container:...` formats are only supported in the
> [version 2 file format](compose-versioning.md#version-2).
>
>* In [version 1](compose-versioning.md#version-1), you can use
> container names without marking them as such:
>
>     - `service_name`
>     - `service_name:ro`
>     - `container_name`
>     - `container_name:rw`

### restart

`no` is the default restart policy, and it doesn't restart a container under any circumstance. When `always` is specified, the container always restarts. The `on-failure` policy restarts a container if the exit code indicates an on-failure error.

      - restart: no
      - restart: always
      - restart: on-failure

{: id="cpu-and-other-resources"}

### cpu_count, cpu_percent, cpu\_shares, cpu\_period, cpu\_quota, cpus, cpuset, domainname, hostname, ipc, mac\_address, mem\_limit, memswap\_limit, mem\_swappiness, mem\_reservation, oom_kill_disable, oom_score_adj, privileged, read\_only, shm\_size, stdin\_open, tty, user, working\_dir

Each of these is a single value, analogous to its
[docker run](/engine/reference/run.md) counterpart.

> **Note**: The following options were added in [version 2.2](compose-versioning.md#version-22):
> `cpu_count`, `cpu_percent`, `cpus`.
> The following options were added in [version 2.1](compose-versioning.md#version-21):
> `oom_kill_disable`, `cpu_period`

    cpu_count: 2
    cpu_percent: 50
    cpus: 0.5
    cpu_shares: 73
    cpu_quota: 50000
    cpu_period: 20ms
    cpuset: 0,1

    user: postgresql
    working_dir: /code

    domainname: foo.com
    hostname: foo
    ipc: host
    mac_address: 02:42:ac:11:65:43

    mem_limit: 1000000000
    memswap_limit: 2000000000
    mem_reservation: 512m
    privileged: true

    oom_score_adj: 500
    oom_kill_disable: true

    read_only: true
    shm_size: 64M
    stdin_open: true
    tty: true

{: id="orig-resources" }

## Specifying durations

Some configuration options, such as the `interval` and `timeout` sub-options for
[`healthcheck`](#healthcheck), accept a duration as a string in a
format that looks like this:

    2.5s
    10s
    1m30s
    2h32m
    5h34m56s

The supported units are `us`, `ms`, `s`, `m` and `h`.

## Specifying byte values

Some configuration options, such as the `device_read_bps` sub-option for
[`blkio_config`](#blkioconfig), accept a byte value as a string in a format
that looks like this:

    2b
    1024kb
    2048k
    300m
    1gb

The supported units are `b`, `k`, `m` and `g`, and their alternative notation `kb`,
`mb` and `gb`. Decimal values are not supported at this time.

## Volume configuration reference

While it is possible to declare volumes on the fly as part of the service
declaration, this section allows you to create named volumes that can be
reused across multiple services (without relying on `volumes_from`), and are
easily retrieved and inspected using the docker command line or API.
See the [docker volume](/engine/reference/commandline/volume_create.md)
subcommand documentation for more information.

Here's an example of a two-service setup where a database's data directory is
shared with another service as a volume so that it can be periodically backed
up:

    version: "{{ site.compose_file_v2 }}"

    services:
      db:
        image: db
        volumes:
          - data-volume:/var/lib/db
      backup:
        image: backup-service
        volumes:
          - data-volume:/var/lib/backup/data

    volumes:
      data-volume:

An entry under the top-level `volumes` key can be empty, in which case it
uses the default driver configured by the Engine (in most cases, this is the
`local` driver). Optionally, you can configure it with the following keys:

### driver

Specify which volume driver should be used for this volume. Defaults to whatever
driver the Docker Engine has been configured to use, which in most cases is
`local`. If the driver is not available, the Engine returns an error when
`docker-compose up` tries to create the volume.

     driver: foobar

### driver_opts

Specify a list of options as key-value pairs to pass to the driver for this
volume. Those options are driver-dependent - consult the driver's
documentation for more information. Optional.

     driver_opts:
       foo: "bar"
       baz: 1

### external

If set to `true`, specifies that this volume has been created outside of
Compose. `docker-compose up` does not attempt to create it, and raises
an error if it doesn't exist.

For version 2.0 of the format, `external` cannot be used in
conjunction with other volume configuration keys (`driver`, `driver_opts`,
`labels`). This limitation no longer exists for
[version 2.1](compose-versioning.md#version-21) and above.

In the example below, instead of attempting to create a volume called
`[projectname]_data`, Compose looks for an existing volume simply
called `data` and mount it into the `db` service's containers.

    version: "{{ site.compose_file_v2 }}"

    services:
      db:
        image: postgres
        volumes:
          - data:/var/lib/postgresql/data

    volumes:
      data:
        external: true

You can also specify the name of the volume separately from the name used to
refer to it within the Compose file:

    volumes:
      data:
        external:
          name: actual-name-of-volume

> **Note**: In newer versions of Compose, the `external.name` property is
> deprecated in favor of simply using the `name` property.

### labels

> [Added in version 2.1 file format](compose-versioning.md#version-21).

Add metadata to containers using
[Docker labels](/engine/userguide/labels-custom-metadata.md). You can use either
an array or a dictionary.

It's recommended that you use reverse-DNS notation to prevent your labels from
conflicting with those used by other software.

    labels:
      com.example.description: "Database volume"
      com.example.department: "IT/Ops"
      com.example.label-with-empty-value: ""

    labels:
      - "com.example.description=Database volume"
      - "com.example.department=IT/Ops"
      - "com.example.label-with-empty-value"


### name

> [Added in version 2.1 file format](compose-versioning.md#version-21)

Set a custom name for this volume.

    version: "{{ site.compose_file_v2 }}"
    volumes:
      data:
        name: my-app-data

It can also be used in conjunction with the `external` property:

    version: "{{ site.compose_file_v2 }}"
    volumes:
      data:
        external: true
        name: my-app-data

## Network configuration reference

The top-level `networks` key lets you specify networks to be created. For a full
explanation of Compose's use of Docker networking features, see the
[Networking guide](/compose/networking.md).

### driver

Specify which driver should be used for this network.

The default driver depends on how the Docker Engine you're using is configured,
but in most instances it is `bridge` on a single host and `overlay` on a
Swarm.

The Docker Engine returns an error if the driver is not available.

    driver: overlay

Starting in Compose file format 2.1, overlay networks are always created as
`attachable`, and this is not configurable. This means that standalone
containers can connect to overlay networks.

### driver_opts

Specify a list of options as key-value pairs to pass to the driver for this
network. Those options are driver-dependent - consult the driver's
documentation for more information. Optional.

      driver_opts:
        foo: "bar"
        baz: 1

### enable_ipv6

> [Added in version 2.1 file format](compose-versioning.md#version-21).

Enable IPv6 networking on this network.

### ipam

Specify custom IPAM config. This is an object with several properties, each of
which is optional:

-   `driver`: Custom IPAM driver, instead of the default.
-   `config`: A list with zero or more config blocks, each containing any of
    the following keys:
    - `subnet`: Subnet in CIDR format that represents a network segment
    - `ip_range`: Range of IPs from which to allocate container IPs
    - `gateway`: IPv4 or IPv6 gateway for the master subnet
    - `aux_addresses`: Auxiliary IPv4 or IPv6 addresses used by Network driver,
      as a mapping from hostname to IP
-   `options`: Driver-specific options as a key-value mapping.

A full example:

    ipam:
      driver: default
      config:
        - subnet: 172.28.0.0/16
          ip_range: 172.28.5.0/24
          gateway: 172.28.5.254
          aux_addresses:
            host1: 172.28.1.5
            host2: 172.28.1.6
            host3: 172.28.1.7
      options:
        foo: bar
        baz: "0"

### internal

By default, Docker also connects a bridge network to it to provide external
connectivity. If you want to create an externally isolated overlay network,
you can set this option to `true`.

### labels

> [Added in version 2.1 file format](compose-versioning.md#version-21).

Add metadata to containers using
[Docker labels](/engine/userguide/labels-custom-metadata.md). You can use either
an array or a dictionary.

It's recommended that you use reverse-DNS notation to prevent your labels from
conflicting with those used by other software.

    labels:
      com.example.description: "Financial transaction network"
      com.example.department: "Finance"
      com.example.label-with-empty-value: ""

    labels:
      - "com.example.description=Financial transaction network"
      - "com.example.department=Finance"
      - "com.example.label-with-empty-value"

### external

If set to `true`, specifies that this network has been created outside of
Compose. `docker-compose up` does not attempt to create it, and raises
an error if it doesn't exist.

For version 2.0 of the format, `external` cannot be used in conjunction with
other network configuration keys (`driver`, `driver_opts`, `ipam`, `internal`).
This limitation no longer exists for
[version 2.1](compose-versioning.md#version-21) and above.

In the example below, `proxy` is the gateway to the outside world. Instead of
attempting to create a network called `[projectname]_outside`, Compose
looks for an existing network simply called `outside` and connect the `proxy`
service's containers to it.

    version: "{{ site.compose_file_v2 }}"

    services:
      proxy:
        build: ./proxy
        networks:
          - outside
          - default
      app:
        build: ./app
        networks:
          - default

    networks:
      outside:
        external: true

You can also specify the name of the network separately from the name used to
refer to it within the Compose file:

    networks:
      outside:
        external:
          name: actual-name-of-network


Not supported for version 2 `docker-compose` files. Use
[network_mode](#network_mode) instead.

### name

> [Added in version 2.1 file format](compose-versioning.md#version-21)

Set a custom name for this network.

    version: "{{ site.compose_file_v2 }}"
    networks:
      network1:
        name: my-app-net

It can also be used in conjunction with the `external` property:

    version: "{{ site.compose_file_v2 }}"
    networks:
      network1:
        external: true
        name: my-app-net

## Variable substitution

{% include content/compose-var-sub.md %}

## Extension fields

> [Added in version 2.1 file format](compose-versioning.md#version-21).

{% include content/compose-extfields-sub.md %}

## Compose documentation

- [User guide](/compose/index.md)
- [Installing Compose](/compose/install.md)
- [Compose file versions and upgrading](compose-versioning.md)
- [Get started with Django](/compose/django.md)
- [Get started with Rails](/compose/rails.md)
- [Get started with WordPress](/compose/wordpress.md)
- [Command line reference](/compose/reference/)
