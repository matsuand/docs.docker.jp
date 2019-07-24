---
description: Compose ファイルリファレンス
keywords: fig, composition, compose, docker
redirect_from:
- /compose/yml
- /compose/compose-file-v3.md
title: Compose ファイル バージョン 3 リファレンス
toc_max: 4
toc_min: 1
---

{% comment %}
## Reference and guidelines
{% endcomment %}
{: id="reference-and-guidelines" }
## リファレンスとガイドライン

{% comment %}
These topics describe version 3 of the Compose file format. This is the newest
version.
{% endcomment %}
ここに示す内容は Compose ファイルフォーマット、バージョン 3 です。
これが最新バージョンです。

{% comment %}
## Compose and Docker compatibility matrix
{% endcomment %}
{: id="compose-and-docker-compatibility-matrix" }
## Compose と Docker の互換マトリックス

{% comment %}
There are several versions of the Compose file format – 1, 2, 2.x, and 3.x. The
table below is a quick look. For full details on what each version includes and
how to upgrade, see **[About versions and upgrading](compose-versioning.md)**.
{% endcomment %}
Compose ファイルフォーマットには 1、2、2.x、3.x という複数のバージョンがあります。
その様子は以下の一覧表に見ることができます。
各バージョンにて何が増えたのか、どのようにアップグレードしたのか、といった詳細については **[バージョンとアップグレードについて](compose-versioning.md)**を参照してください。

{% include content/compose-matrix.md %}

{% comment %}
## Compose file structure and examples
{% endcomment %}
{: id="compose-file-structure-and-examples" }
## Compose ファイルの構造と記述例

<div class="panel panel-default">
    <div class="panel-heading collapsed" data-toggle="collapse" data-target="#collapseSample1" style="cursor: pointer">
    Compose ファイル バージョン 3 の記述例
    <i class="chevron fa fa-fw"></i></div>
    <div class="collapse block" id="collapseSample1">
<pre><code>
version: "{{ site.compose_file_v3 }}"
services:

  redis:
    image: redis:alpine
    ports:
      - "6379"
    networks:
      - frontend
    deploy:
      replicas: 2
      update_config:
        parallelism: 2
        delay: 10s
      restart_policy:
        condition: on-failure

  db:
    image: postgres:9.4
    volumes:
      - db-data:/var/lib/postgresql/data
    networks:
      - backend
    deploy:
      placement:
        constraints: [node.role == manager]

  vote:
    image: dockersamples/examplevotingapp_vote:before
    ports:
      - "5000:80"
    networks:
      - frontend
    depends_on:
      - redis
    deploy:
      replicas: 2
      update_config:
        parallelism: 2
      restart_policy:
        condition: on-failure

  result:
    image: dockersamples/examplevotingapp_result:before
    ports:
      - "5001:80"
    networks:
      - backend
    depends_on:
      - db
    deploy:
      replicas: 1
      update_config:
        parallelism: 2
        delay: 10s
      restart_policy:
        condition: on-failure

  worker:
    image: dockersamples/examplevotingapp_worker
    networks:
      - frontend
      - backend
    deploy:
      mode: replicated
      replicas: 1
      labels: [APP=VOTING]
      restart_policy:
        condition: on-failure
        delay: 10s
        max_attempts: 3
        window: 120s
      placement:
        constraints: [node.role == manager]

  visualizer:
    image: dockersamples/visualizer:stable
    ports:
      - "8080:8080"
    stop_grace_period: 1m30s
    volumes:
      - "/var/run/docker.sock:/var/run/docker.sock"
    deploy:
      placement:
        constraints: [node.role == manager]

networks:
  frontend:
  backend:

volumes:
  db-data:
</code></pre>
    </div>
</div>

{% comment %}
The topics on this reference page are organized alphabetically by top-level key
to reflect the structure of the Compose file itself. Top-level keys that define
a section in the configuration file such as `build`, `deploy`, `depends_on`,
`networks`, and so on, are listed with the options that support them as
sub-topics. This maps to the `<key>: <option>: <value>` indent structure of the
Compose file.
{% endcomment %}
このリファレンスページで取り上げているトピックは、Compose ファイルの構造に合わせて、最上位のキー項目をアルファベット順に示しています。
最上位のキー項目とは、設定ファイルにおいてのセクションを定義するものであり、`build`、`deploy`、`depends_on`、`networks`などのことです。
そのキー項目ごとに、それをサポートするオプションをサブトピックとして説明しています。
これは Compose ファイルにおいて `<key>: <option>: <value>` という形式のインデント構造に対応します。

{% comment %}
A good place to start is the [Getting Started](/get-started/index.md) tutorial
which uses version 3 Compose stack files to implement multi-container apps,
service definitions, and swarm mode. Here are some Compose files used in the
tutorial.
{% endcomment %}
理解しやすいのは、[はじめよう](/get-started/index.md)にて示しているチュートリアルです。
そこでは Compose ファイルのバージョン 3 を使って、マルチコンテナーアプリケーション、サービス定義、スウォームモードを実現しています。
チュートリアルで利用している Compose ファイルは以下のものです。

{% comment %}
- [Your first docker-compose.yml File](/get-started/part3.md#your-first-docker-composeyml-file)
{% endcomment %}
- [はじめての docker-compose.yml ファイル](/get-started/part3.md#your-first-docker-composeyml-file)

{% comment %}
- [Add a new service and redeploy](/get-started/part5.md#add-a-new-service-and-redeploy)
{% endcomment %}
- [サービスの新規追加と再デプロイ](/get-started/part5.md#add-a-new-service-and-redeploy)

{% comment %}
Another good reference is the Compose file for the voting app sample used in the
[Docker for Beginners lab](https://github.com/docker/labs/tree/master/beginner/)
topic on [Deploying an app to a
Swarm](https://github.com/docker/labs/blob/master/beginner/chapters/votingapp.md). This is also shown on the accordion at the top of this section.
{% endcomment %}
別のリファレンスとして [Deploying an app to a
Swarm](https://github.com/docker/labs/blob/master/beginner/chapters/votingapp.md) の中のトピック [Docker for Beginners lab](https://github.com/docker/labs/tree/master/beginner/) において利用されている投票アプリのサンプルの Compose ファイルが参考になります。
これも本節の上部にあるプルダウンのコードの中に示しています。

{% comment %}
## Service configuration reference
{% endcomment %}
{: id="service-configuration-reference" }
## サービス設定リファレンス

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
>**Tip**: You can use either a `.yml` or `.yaml` extension for this file.
They both work.
{% endcomment %}
>**ヒント**: このファイルの拡張子は `.yml` と `.yaml` のどちらでも構いません。
いずれでも動作します。

{% comment %}
A service definition contains configuration that is applied to each
container started for that service, much like passing command-line parameters to
`docker container create`. Likewise, network and volume definitions are analogous to
`docker network create` and `docker volume create`.
{% endcomment %}
サービスの定義とは、そのサービスを起動する各コンテナーに適用される設定を行うことです。
コマンドラインから `docker container create` のパラメーターを受け渡すことと、非常によく似ています。
同様に、ネットワークの定義、ボリュームの定義は、それぞれ `docker network create` と `docker volume create` のコマンドに対応づくものです。

{% comment %}
As with `docker container create`, options specified in the Dockerfile, such as `CMD`,
`EXPOSE`, `VOLUME`, `ENV`, are respected by default - you don't need to
specify them again in `docker-compose.yml`.
{% endcomment %}
`docker container create` に関しても同じことが言えますが、Dockerfile にて指定された `CMD`、`EXPOSE`、`VOLUME`、`ENV` のようなオプションはデフォルトでは維持されます。したがって `docker-compose.yml` の中で再度設定する必要はありません。

{% comment %}
You can use environment variables in configuration values with a Bash-like
`${VARIABLE}` syntax - see
[variable substitution](#variable-substitution) for full details.
{% endcomment %}
設定を記述する際には環境変数を用いることができます。
環境変数は Bash 風に `${VARIABLE}` のように記述します。
詳しくは[変数の置換](#variable-substitution)を参照してください。

{% comment %}
This section contains a list of all configuration options supported by a service
definition in version 3.
{% endcomment %}
このセクションでは、バージョン 3 のサービス定義においてサポートされている設定オプションをすべて説明しています。

### build

{% comment %}
Configuration options that are applied at build time.
{% endcomment %}
この設定オプションはビルド時に適用されます。

{% comment %}
`build` can be specified either as a string containing a path to the build
context:
{% endcomment %}
`build` の指定方法の 1 つは、ビルドコンテキストへのパスを表わす文字列を指定します。

```yaml
version: "{{ site.compose_file_v3 }}"
services:
  webapp:
    build: ./dir
```

{% comment %}
Or, as an object with the path specified under [context](#context) and
optionally [Dockerfile](#dockerfile) and [args](#args):
{% endcomment %}
あるいは [context](#context) の指定のもとにパスを指定し、オプションとして [Dockerfile](#dockerfile) や [args](#args) を記述する方法をとります。

```yaml
version: "{{ site.compose_file_v3 }}"
services:
  webapp:
    build:
      context: ./dir
      dockerfile: Dockerfile-alternate
      args:
        buildno: 1
```

{% comment %}
If you specify `image` as well as `build`, then Compose names the built image
with the `webapp` and optional `tag` specified in `image`:
{% endcomment %}
`build` に加えて `image` も指定した場合、Compose はビルドイメージに名前をつけます。
たとえば以下のように `image` を指定すると、イメージ名を `webapp`、オプションのタグを `tag` という名前にします。

```yaml
build: ./dir
image: webapp:tag
```

{% comment %}
This results in an image named `webapp` and tagged `tag`, built from `./dir`.
{% endcomment %}
結果としてイメージ名は `webapp` であり `tag` というタグづけが行われます。
そしてこのイメージは `./dir` から作り出されます。

{% comment %}
> **Note**: This option is ignored when
> [deploying a stack in swarm mode](/engine/reference/commandline/stack_deploy.md)
> with a (version 3) Compose file. The `docker stack` command accepts only pre-built images.
{% endcomment %}
> **メモ**: Compose ファイルバージョン 3 においてこのオプションは、[スウォームモードでのスタックのデプロイ](/engine/reference/commandline/stack_deploy.md)を行う場合には無視されます。
> `docker stack` コマンドは、ビルド済のイメージのみを受け付けるためです。

#### context

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
Compose builds and tags it with a generated name, and uses that image
thereafter.
{% endcomment %}
Compose は指定された名前により、イメージのビルドとタグづけを行い、後々これを利用します。

```yaml
build:
  context: ./dir
```

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

```yaml
build:
  context: .
  dockerfile: Dockerfile-alternate
```

#### args

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

```Dockerfile
ARG buildno
ARG gitcommithash

RUN echo "Build number: $buildno"
RUN echo "Based on commit: $gitcommithash"
```

{% comment %}
Then specify the arguments under the `build` key. You can pass a mapping
or a list:
{% endcomment %}
そして `build` キーのもとにその引数を指定します。
指定は個々をマッピングする形式か、リストとする形式が可能です。

```yaml
build:
  context: .
  args:
    buildno: 1
    gitcommithash: cdc3b19
```

```yaml
build:
  context: .
  args:
    - buildno=1
    - gitcommithash=cdc3b19
```

{% comment %}
> **Note**: In your Dockerfile, if you specify `ARG` before the `FROM` instruction,
> `ARG` is not available in the build instructions under `FROM`.
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

```yaml
args:
  - buildno
  - gitcommithash
```

{% comment %}
> **Note**: YAML boolean values (`true`, `false`, `yes`, `no`, `on`, `off`) must
> be enclosed in quotes, so that the parser interprets them as strings.
{% endcomment %}
> **メモ**: YAML のブール値 (`true`, `false`, `yes`, `no`, `on`, `off`) を用いる場合は、クォートで囲む必要があります。
> そうすることで、これらの値は文字列として解釈されます。

#### cache_from

{% comment %}
> **Note**: This option is new in v3.2
{% endcomment %}
> **メモ**: このオプションはバージョン 3.2 において新たに追加されました。

{% comment %}
A list of images that the engine uses for cache resolution.
{% endcomment %}
エンジンがキャッシュ解決のために利用するイメージを設定します。

```yaml
build:
  context: .
  cache_from:
    - alpine:latest
    - corp/web_app:3.14
```

#### labels

{% comment %}
> **Note**: This option is new in v3.3
{% endcomment %}
> **メモ**: このオプションはバージョン 3.3 において新たに追加されました。

{% comment %}
Add metadata to the resulting image using [Docker labels](/engine/userguide/labels-custom-metadata.md).
You can use either an array or a dictionary.
{% endcomment %}
[Docker labels](/engine/userguide/labels-custom-metadata.md) を使ってイメージにメタデータを追加します。
配列形式と辞書形式のいずれかにより指定します。

{% comment %}
We recommend that you use reverse-DNS notation to prevent your labels from conflicting with
those used by other software.
{% endcomment %}
ここでは逆 DNS 記法とすることをお勧めします。
この記法にしておけば、他のソフトウェアが用いるラベルとの競合が避けられるからです。

```yaml
build:
  context: .
  labels:
    com.example.description: "Accounting webapp"
    com.example.department: "Finance"
    com.example.label-with-empty-value: ""
```

```yaml
build:
  context: .
  labels:
    - "com.example.description=Accounting webapp"
    - "com.example.department=Finance"
    - "com.example.label-with-empty-value"
```

#### shm_size

{% comment %}
> Added in [version 3.5](compose-versioning.md#version-35) file format
{% endcomment %}
> ファイルフォーマット[バージョン 3.5](compose-versioning.md#version-35) において追加されました。

{% comment %}
Set the size of the `/dev/shm` partition for this build's containers. Specify
as an integer value representing the number of bytes or as a string expressing
a [byte value](#specifying-byte-values).
{% endcomment %}
このビルドコンテナーにおける `/dev/shm` パーティションのサイズを設定します。
指定する値は、バイト数を表わす整数値か、あるいは [バイト表現](#specifying-byte-values) によって表わされる文字列とします。

```yaml
build:
  context: .
  shm_size: '2gb'
```

```yaml
build:
  context: .
  shm_size: 10000000
```

#### target

{% comment %}
> Added in [version 3.4](compose-versioning.md#version-34) file format
{% endcomment %}
> ファイルフォーマット[バージョン 3.4](compose-versioning.md#version-34) において追加されました。

{% comment %}
Build the specified stage as defined inside the `Dockerfile`. See the
[multi-stage build docs](/engine/userguide/eng-image/multistage-build.md) for
details.
{% endcomment %}
`Dockerfile` 内部に定義されている特定のステージをビルドする方法は、[マルチステージビルド](/engine/userguide/eng-image/multistage-build.md)を参照してください。

```yaml
build:
  context: .
  target: prod
```

### cap_add, cap_drop

{% comment %}
Add or drop container capabilities.
See `man 7 capabilities` for a full list.
{% endcomment %}
コンテナーケーパビリティーの機能を追加または削除します。
詳細な一覧は `man 7 capabilities` を参照してください。

```yaml
cap_add:
  - ALL

cap_drop:
  - NET_ADMIN
  - SYS_ADMIN
```

{% comment %}
> **Note**: These options are ignored when
> [deploying a stack in swarm mode](/engine/reference/commandline/stack_deploy.md)
> with a (version 3) Compose file.
{% endcomment %}
> **メモ**: Compose ファイルバージョン 3 においてこのオプションは、[スウォームモードでのスタックのデプロイ](/engine/reference/commandline/stack_deploy.md)を行う場合には無視されます。

### cgroup_parent

{% comment %}
Specify an optional parent cgroup for the container.
{% endcomment %}
コンテナーに対して、オプションで指定する親の cgroup を指定します。

```yaml
cgroup_parent: m-executor-abcd
```

{% comment %}
> **Note**: This option is ignored when
> [deploying a stack in swarm mode](/engine/reference/commandline/stack_deploy.md)
> with a (version 3) Compose file.
{% endcomment %}
> **メモ**: Compose ファイルバージョン 3 においてこのオプションは、[スウォームモードでのスタックのデプロイ](/engine/reference/commandline/stack_deploy.md)を行う場合には無視されます。

### command

{% comment %}
Override the default command.
{% endcomment %}
デフォルトコマンドを上書きします。

```yaml
command: bundle exec thin -p 3000
```

{% comment %}
The command can also be a list, in a manner similar to
[dockerfile](/engine/reference/builder.md#cmd):
{% endcomment %}
このコマンドは [dockerfile](/engine/reference/builder.md#cmd) の場合と同じように、リスト形式により指定することもできます。

```yaml
command: ["bundle", "exec", "thin", "-p", "3000"]
```

### configs

{% comment %}
Grant access to configs on a per-service basis using the per-service `configs`
configuration. Two different syntax variants are supported.
{% endcomment %}
サービスごとの `configs` を利用して、サービス単位での configs 設定を許可します。
2 つの異なる文法がサポートされています。

{% comment %}
> **Note**: The config must already exist or be
> [defined in the top-level `configs` configuration](#configs-configuration-reference)
> of this stack file, or stack deployment fails.
{% endcomment %}
> **メモ**: config は既に定義済であるか、あるいは [最上位の `configs` が定義済](#configs-configuration-reference) である必要があります。
> そうでない場合、このファイルによるデプロイが失敗します。

{% comment %}
For more information on configs, see [configs](/engine/swarm/configs.md).
{% endcomment %}
configs に関する詳細は [configs](/engine/swarm/configs.md) を参照してください。

{% comment %}
#### Short syntax
{% endcomment %}
{: id="short-syntax" }
#### 短い文法

{% comment %}
The short syntax variant only specifies the config name. This grants the
container access to the config and mounts it at `/<config_name>`
within the container. The source name and destination mountpoint are both set
to the config name.
{% endcomment %}
短い文法の場合には config 名を指定するのみです。
これを行うと、コンテナー内において `/<config_name>` というディレクトリをマウントしてアクセス可能とします。
マウント元の名前とマウント名はともに config 名となります。

{% comment %}
The following example uses the short syntax to grant the `redis` service
access to the `my_config` and `my_other_config` configs. The value of
`my_config` is set to the contents of the file `./my_config.txt`, and
`my_other_config` is defined as an external resource, which means that it has
already been defined in Docker, either by running the `docker config create`
command or by another stack deployment. If the external config does not exist,
the stack deployment fails with a `config not found` error.
{% endcomment %}
以下の例では短い文法を用います。
`redis` サービスが 2 つの configs ファイル `my_config` と `my_other_config` にアクセスできるようにするものです。
`my_config` へは、ファイル `./my_config.txt` の内容を設定しています。
そして `my_other_config` は外部リソースとして定義します。
これはつまり Docker によりすでに定義されていることを意味し、`docker config create` の実行により、あるいは別のスタックデプロイメントにより指定されます。
外部 config が存在していない場合は、スタックデプロイメントは失敗し `config not found` というエラーになります。

{% comment %}
> **Note**: `config` definitions are only supported in version 3.3 and higher
>  of the compose file format.
{% endcomment %}
> **メモ**: `config` 定義は、Compose ファイルフォーマットのバージョン 3.3 またはそれ以上においてサポートされています。

```yaml
version: "{{ site.compose_file_v3 }}"
services:
  redis:
    image: redis:latest
    deploy:
      replicas: 1
    configs:
      - my_config
      - my_other_config
configs:
  my_config:
    file: ./my_config.txt
  my_other_config:
    external: true
```

{% comment %}
#### Long syntax
{% endcomment %}
{: id="long-syntax" }
#### 長い文法

{% comment %}
The long syntax provides more granularity in how the config is created within
the service's task containers.
{% endcomment %}
長い文法は、サービスのタスクコンテナー内にて生成する config をより細かく設定します。

{% comment %}
- `source`: The name of the config as it exists in Docker.
- `target`: The path and name of the file to be mounted in the service's
  task containers. Defaults to `/<source>` if not specified.
- `uid` and `gid`: The numeric UID or GID that owns the mounted config file
  within in the service's task containers. Both default to `0` on Linux if not
  specified. Not supported on Windows.
- `mode`: The permissions for the file that is mounted within the service's
  task containers, in octal notation. For instance, `0444`
  represents world-readable. The default is `0444`. Configs cannot be writable
  because they are mounted in a temporary filesystem, so if you set the writable
  bit, it is ignored. The executable bit can be set. If you aren't familiar with
  UNIX file permission modes, you may find this
  [permissions calculator](http://permissions-calculator.org/){: target="_blank" class="_" }
  useful.
{% endcomment %}
- `source`: Docker 内に設定する config 名。
- `target`: サービスのタスクコンテナー内にマウントされるファイルパス名。
  指定されない場合のデフォルトは `/<source>`。
- `uid` と `gid`: サービスのタスクコンテナー内にマウントされる config ファイルの所有者を表わす UID 値および GID 値。
  指定されない場合のデフォルトは、Linux においては `0`。
  Windows においてはサポートされません。
- `mode`: サービスのタスクコンテナー内にマウントされる config ファイルのパーミッション。
  8 進数表記。
  たとえば `0444` であればすべて読み込み可。
  デフォルトは `0444`。
  Configs は、テンポラリなファイルシステム上にマウントされるため、書き込み可能にはできません。
  したがって書き込みビットを設定しても無視されます。
  実行ビットは設定できます。
  UNIX のファイルパーミッションモードに詳しくない方は、[パーミッション計算機](http://permissions-calculator.org/){: target="_blank" class="_" }を参照してください。

{% comment %}
The following example sets the name of `my_config` to `redis_config` within the
container, sets the mode to `0440` (group-readable) and sets the user and group
to `103`. The `redis` service does not have access to the `my_other_config`
config.
{% endcomment %}
以下の例では `my_config` という名前の config を、コンテナー内では `redis_config` として設定します。
そしてモードを `0440`（グループが読み込み可能）とし、ユーザーとグループは `103` とします。
`redis` サービスは `my_other_config` へはアクセスしません。

```yaml
version: "{{ site.compose_file_v3 }}"
services:
  redis:
    image: redis:latest
    deploy:
      replicas: 1
    configs:
      - source: my_config
        target: /redis_config
        uid: '103'
        gid: '103'
        mode: 0440
configs:
  my_config:
    file: ./my_config.txt
  my_other_config:
    external: true
```

{% comment %}
You can grant a service access to multiple configs and you can mix long and
short syntax. Defining a config does not imply granting a service access to it.
{% endcomment %}
1 つのサービスが複数の configs にアクセスする設定とすることもできます。
また短い文法、長い文法を混在することも可能です。
config を定義しただけでは、サービスに対して config へのアクセスを許可するものにはなりません。

### container_name

{% comment %}
Specify a custom container name, rather than a generated default name.
{% endcomment %}
デフォルトのコンテナー名ではない、独自のコンテナー名を設定します。

```yaml
container_name: my-web-container
```

{% comment %}
Because Docker container names must be unique, you cannot scale a service beyond
1 container if you have specified a custom name. Attempting to do so results in
an error.
{% endcomment %}
Docker コンテナー名はユニークである必要があります。
そこで独自のコンテナー名を設定したときは、サービスをスケールアップして複数コンテナーとすることはできません。
これを行うとエラーが発生します。

{% comment %}
> **Note**: This option is ignored when
> [deploying a stack in swarm mode](/engine/reference/commandline/stack_deploy.md)
> with a (version 3) Compose file.
{% endcomment %}
> **メモ**: Compose ファイルバージョン 3 においてこのオプションは、[スウォームモードでのスタックのデプロイ](/engine/reference/commandline/stack_deploy.md)を行う場合には無視されます。

### credential_spec

{% comment %}
> **Note**: This option was added in v3.3. Using group Managed Service Account (gMSA) configurations with compose files is supported in Compose version 3.8.
{% endcomment %}
> **メモ**: このオプションは v3.3 で追加されました。
> Compopse ファイルにおける group Managed Service Account (gMSA) の設定は Compose バージョン 3.8 においてサポートされています。

{% comment %}
Configure the credential spec for managed service account. This option is only
used for services using Windows containers. The `credential_spec` must be in the
format `file://<filename>` or `registry://<value-name>`.
{% endcomment %}
管理サービスアカウントに対する資格情報スペック（`credential_spec`）を設定します。
このオプションは Windows コンテナーを利用するサービスにおいてのみ用いられます。
`credential_spec` の書式は `file://<filename>` または `registry://<value-name>` でなければなりません。

{% comment %}
When using `file:`, the referenced file must be present in the `CredentialSpecs`
subdirectory in the Docker data directory, which defaults to `C:\ProgramData\Docker\`
on Windows. The following example loads the credential spec from a file named
`C:\ProgramData\Docker\CredentialSpecs\my-credential-spec.json`:
{% endcomment %}
`file:` を用いるとき、参照するファイルは実際に存在するファイルでなければならず、Docker データディレクトリ配下のサブディレクトリ `CredentialSpecs` になければなりません。
Windows における Docker データディレクトリのデフォルトは `C:\ProgramData\Docker\` です。
以下の例は `C:\ProgramData\Docker\CredentialSpecs\my-credential-spec.json` というファイルから資格情報スペックを読み込みます。

```yaml
credential_spec:
  file: my-credential-spec.json
```

{% comment %}
When using `registry:`, the credential spec is read from the Windows registry on
the daemon's host. A registry value with the given name must be located in:
{% endcomment %}
`registry:` を用いるとき資格情報スペックは、デーモンホスト内の Windows レジストリから読み込まれます。
指定された名称のレジストリ値は、以下に定義されている必要があります。

    HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Virtualization\Containers\CredentialSpecs

{% comment %}
The following example load the credential spec from a value named `my-credential-spec`
in the registry:
{% endcomment %}
以下の例は、レジストリ内の `my-credential-spec` という値から資格情報スペックを読み込みます。

```yaml
credential_spec:
  registry: my-credential-spec
```

{% comment %}
#### Example gMSA configuration
{% endcomment %}
{: #example-gmsa-configuration }
#### gMSA の設定例
{% comment %}
When configuring a gMSA credential spec for a service, you only need
to specify a credential spec with `config`, as shown in the following example:
{% endcomment %}
サービスに対して credential spec における gMSA を設定する場合、credential spec の `config` を設定するだけです。
その例を以下に示します。
```
version: "3.8"
services:
  myservice:
    image: myimage:latest
    credential_spec:
      config: my_credential_spec

configs:
  my_credentials_spec:
    file: ./my-credential-spec.json|
```

### depends_on

{% comment %}
Express dependency between services, Service dependencies cause the following
behaviors:
{% endcomment %}
サービス間の依存関係を表わします。
サービスに依存関係があると、以下の動作になります。

{% comment %}
- `docker-compose up` starts services in dependency order. In the following
  example, `db` and `redis` are started before `web`.
{% endcomment %}
- `docker-compose up` は依存関係の順にサービスを起動します。
  以下の例において `db` と `redis` は `web` の後に起動します。

{% comment %}
- `docker-compose up SERVICE` automatically includes `SERVICE`'s
  dependencies. In the following example, `docker-compose up web` also
  creates and starts `db` and `redis`.
{% endcomment %}
- `docker-compose up SERVICE` を実行すると `SERVICE` における依存関係をもとに動作します。
  以下の例において `docker-compose up web` を実行すると `db` と `redis` を生成して起動します。

{% comment %}
- `docker-compose stop` stops services in dependency order. In the following
  example, `web` is stopped before `db` and `redis`.
{% endcomment %}
- `docker-compose stop` は依存関係の順にサービスを停止します。
  以下の例においては `web` が停止されてから `db` と `redis` が停止されます。

{% comment %}
Simple example:
{% endcomment %}
以下がその簡単な例です。

```yaml
version: "{{ site.compose_file_v3 }}"
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
```

{% comment %}
> There are several things to be aware of when using `depends_on`:
>
> - `depends_on` does not wait for `db` and `redis` to be "ready" before
>   starting `web` - only until they have been started. If you need to wait
>   for a service to be ready, see [Controlling startup order](/compose/startup-order.md)
>   for more on this problem and strategies for solving it.
>
> - Version 3 no longer supports the `condition` form of `depends_on`.
>
> - The `depends_on` option is ignored when
>   [deploying a stack in swarm mode](/engine/reference/commandline/stack_deploy.md)
>   with a version 3 Compose file.
{% endcomment %}
> `depends_on` の利用にあたっては、気をつけておくべきことがあります。
>
> - `depends_on` では `db` や `redis` が「準備」状態になるのを待たずに、つまりそれらを開始したらすぐに `web` を起動します。
>   準備状態になるのを待ってから次のサービスを起動することが必要な場合は、[Compose における起動順の制御](/compose/startup-order.md)にて示す内容と解決方法を確認してください。
>
> - バージョン 3 では `depends_on` の `condition` 形式はサポートされなくなりました。
>
> - Compose ファイルバージョン 3 において `depends_on` オプションは、[スウォームモードでのスタックのデプロイ](/engine/reference/commandline/stack_deploy.md)を行う場合には無視されます。

### deploy

{% comment %}
> **[Version 3](compose-versioning.md#version-3) only.**
{% endcomment %}
> **[バージョン 3](compose-versioning.md#version-3) のみ。**

{% comment %}
Specify configuration related to the deployment and running of services. This
only takes effect when deploying to a [swarm](/engine/swarm/index.md) with
[docker stack deploy](/engine/reference/commandline/stack_deploy.md), and is
ignored by `docker-compose up` and `docker-compose run`.
{% endcomment %}
サービスのデプロイや起動に関する設定を行います。
この設定が有効になるのは[スウォーム](/engine/swarm/index.md)に対して [docker stack deploy](/engine/reference/commandline/stack_deploy.md) コマンドを実行したときであって、`docker-compose up` や `docker-compose run` を実行したときには無視されます。

```yaml
version: "{{ site.compose_file_v3 }}"
services:
  redis:
    image: redis:alpine
    deploy:
      replicas: 6
      update_config:
        parallelism: 2
        delay: 10s
      restart_policy:
        condition: on-failure
```

{% comment %}
Several sub-options are available:
{% endcomment %}
利用可能なサブオプションがあります。

#### endpoint_mode

{% comment %}
Specify a service discovery method for external clients connecting to a swarm.
{% endcomment %}
スウォームに接続する外部クライアントのサービスディスカバリー方法を指定します。

{% comment %}
> **[Version 3.3](compose-versioning.md#version-3) only.**
{% endcomment %}
> **[バージョン 3.3](compose-versioning.md#version-3) でのみ利用可能です。**

{% comment %}
* `endpoint_mode: vip` - Docker assigns the service a virtual IP (VIP)
that acts as the front end for clients to reach the service on a
network. Docker routes requests between the client and available worker
nodes for the service, without client knowledge of how many nodes
are participating in the service or their IP addresses or ports.
(This is the default.)
{% endcomment %}
* `endpoint_mode: vip` - Docker はサービスに対して仮想 IP（virtual IP; VIP）を割り当てます。
  これはネットワーク上のサービスに対して、クライアントがアクセスできるようにするためのフロントエンドとして機能します。
  Docker がサービスに参加する稼動中のワーカーノードやクライアントの間でリクエストを受け渡ししている際に、クライアントはそのサービス上にどれだけの数のノードが参加しているのか、その IP アドレスやポートが何なのか、一切分かりません。
  （この設定がデフォルトです。）

{% comment %}
* `endpoint_mode: dnsrr` -  DNS round-robin (DNSRR) service discovery does
not use a single virtual IP. Docker sets up DNS entries for the service
such that a DNS query for the service name returns a list of IP addresses,
and the client connects directly to one of these. DNS round-robin is useful
in cases where you want to use your own load balancer, or for Hybrid
Windows and Linux applications.
{% endcomment %}
* `endpoint_mode: dnsrr` -  DNS ラウンドロビン（DNS round-robin; DNSRR）によるサービスディスカバリーでは、仮想 IP は単一ではありません。
  Docker はサービスに対する DNS エントリーを設定し、サービス名に対する DNS クエリーが IP アドレスのリストを返すようにします。
  クライアントはそのうちの 1 つに対して直接アクセスします。
  DNS ラウンドロビンが有効なのは、独自のロードバランサーを利用したい場合や、Windows と Linux が統合されたアプリケーションに対して利用する場合です。

```yaml
version: "{{ site.compose_file_v3 }}"

services:
  wordpress:
    image: wordpress
    ports:
      - "8080:80"
    networks:
      - overlay
    deploy:
      mode: replicated
      replicas: 2
      endpoint_mode: vip

  mysql:
    image: mysql
    volumes:
       - db-data:/var/lib/mysql/data
    networks:
       - overlay
    deploy:
      mode: replicated
      replicas: 2
      endpoint_mode: dnsrr

volumes:
  db-data:

networks:
  overlay:
```

{% comment %}
The options for `endpoint_mode` also work as flags on the swarm mode CLI command
[docker service create](/engine/reference/commandline/service_create.md). For a
quick list of all swarm related `docker` commands, see [Swarm mode CLI
commands](/engine/swarm.md#swarm-mode-key-concepts-and-tutorial).
{% endcomment %}
`endpoint_mode` に対する設定は、スウォームモードの CLI コマンド [docker service create](/engine/reference/commandline/service_create.md) におけるフラグとしても動作します。
スウォームに関連する `docker` コマンドの一覧は [スウォームモード CLI コマンド](/engine/swarm.md#swarm-mode-key-concepts-and-tutorial) を参照してください。

{% comment %}
To learn more about service discovery and networking in swarm mode, see
[Configure service
discovery](/engine/swarm/networking.md#configure-service-discovery) in the swarm
mode topics.
{% endcomment %}
スウォームモードにおけるサービスディスカバリーやネットワークに関しての詳細は、スウォームモードのトピックにある [サービスディスカバリーの設定](/engine/swarm/networking.md#configure-service-discovery)を参照してください。


#### labels

{% comment %}
Specify labels for the service. These labels are *only* set on the service,
and *not* on any containers for the service.
{% endcomment %}
サービスに対してのラベルを設定します。
このラベルはサービスに対してのみ設定するものであって、サービスのコンテナーに設定するものでは**ありません**。

```yaml
version: "{{ site.compose_file_v3 }}"
services:
  web:
    image: web
    deploy:
      labels:
        com.example.description: "This label will appear on the web service"
```

{% comment %}
To set labels on containers instead, use the `labels` key outside of `deploy`:
{% endcomment %}
コンテナーにラベルを設定するのであれば、`deploy` の外に `labels` キーを記述してください。

```yaml
version: "{{ site.compose_file_v3 }}"
services:
  web:
    image: web
    labels:
      com.example.description: "This label will appear on all containers for the web service"
```

#### mode

{% comment %}
Either `global` (exactly one container per swarm node) or `replicated` (a
specified number of containers). The default is `replicated`. (To learn more,
see [Replicated and global
services](/engine/swarm/how-swarm-mode-works/services/#replicated-and-global-services)
in the [swarm](/engine/swarm/) topics.)
{% endcomment %}
`global`（スウォームノードごとに 1 つのコンテナーとする）か、`replicated`（指定されたコンテナー数とするか）のいずれかを設定します。
デフォルトは `replicated` です。
（詳しくは [スウォーム](/engine/swarm/)のトピックにある [サービスの Replicated と global](/engine/swarm/how-swarm-mode-works/services/#replicated-and-global-services) を参照してください。）


```yaml
version: "{{ site.compose_file_v3 }}"
services:
  worker:
    image: dockersamples/examplevotingapp_worker
    deploy:
      mode: global
```

#### placement

{% comment %}
Specify placement of constraints and preferences. See the docker service create documentation for a full description of the syntax and available types of [constraints](/engine/reference/commandline/service_create.md#specify-service-constraints-constraint) and [preferences](/engine/reference/commandline/service_create.md#specify-service-placement-preferences-placement-pref).
{% endcomment %}
制約（constraints）とプリファレンス（preferences）の記述場所を指定します。
docker service create のドキュメントには、[制約](/engine/reference/commandline/service_create.md#specify-service-constraints-constraint) と [プリファレンス](/engine/reference/commandline/service_create.md#specify-service-placement-preferences-placement-pref) に関する文法と設定可能な型について、詳細に説明しているので参照してください。

```yaml
version: "{{ site.compose_file_v3 }}"
services:
  db:
    image: postgres
    deploy:
      placement:
        constraints:
          - node.role == manager
          - engine.labels.operatingsystem == ubuntu 14.04
        preferences:
          - spread: node.labels.zone
```

#### replicas

{% comment %}
If the service is `replicated` (which is the default), specify the number of
containers that should be running at any given time.
{% endcomment %}
サービスを `replicated`（デフォルト）に設定しているときに、起動させるコンテナー数を指定します。

```yaml
version: "{{ site.compose_file_v3 }}"
services:
  worker:
    image: dockersamples/examplevotingapp_worker
    networks:
      - frontend
      - backend
    deploy:
      mode: replicated
      replicas: 6
```

#### resources

{% comment %}
Configures resource constraints.
{% endcomment %}
リソースの制約を設定します。

{% comment %}
> **Note**: This replaces the [older resource constraint options](compose-file-v2.md#cpu-and-other-resources) for non swarm mode in
Compose files prior to version 3 (`cpu_shares`, `cpu_quota`, `cpuset`,
`mem_limit`, `memswap_limit`, `mem_swappiness`), as described in [Upgrading
version 2.x to 3.x](/compose/compose-file/compose-versioning.md#upgrading).
{% endcomment %}
> **メモ**: Compose ファイルバージョン 3 より前には、[リソースに対する古い制約オプション](compose-file-v2.md#cpu-and-other-resources) があって、非スウォームモードで用いられていました。
（`cpu_shares`, `cpu_quota`, `cpuset`, `mem_limit`, `memswap_limit`, `mem_swappiness`）
> ここに示すオプションはそれに替わるものです。
> このことは [バージョン 2.x から 3.x へのアップグレード](/compose/compose-file/compose-versioning.md#upgrading) にて説明しています。

{% comment %}
Each of these is a single value, analogous to its [docker service
create](/engine/reference/commandline/service_create.md) counterpart.
{% endcomment %}
個々の設定には単一の値を指定します。
これは [docker service create](/engine/reference/commandline/service_create.md) のオプションに対応づきます。

{% comment %}
In this general example, the `redis` service is constrained to use no more than
50M of memory and `0.50` (50% of a single core) of available processing time (CPU),
and has `20M` of memory and `0.25` CPU time reserved (as always available to it).
{% endcomment %}
以下の一般的な例は `redis` サービスに制約をつけるものです。
メモリ利用は 50M まで、利用可能なプロセス時間（CPU）は `0.50`（シングルコアの 50%）まで。
最低メモリは `20M` 確保し（常時利用可能な） CPU 時間は `0.25` とします。

```yaml
version: "{{ site.compose_file_v3 }}"
services:
  redis:
    image: redis:alpine
    deploy:
      resources:
        limits:
          cpus: '0.50'
          memory: 50M
        reservations:
          cpus: '0.25'
          memory: 20M
```

{% comment %}
The topics below describe available options to set resource constraints on
services or containers in a swarm.
{% endcomment %}
以下では、スウォーム内のサービスやコンテナーに設定できるリソース制約を説明します。

{% comment %}
> Looking for options to set resources on non swarm mode containers?
>
> The options described here are specific to the
`deploy` key and swarm mode. If you want to set resource constraints
on non swarm deployments, use
[Compose file format version 2 CPU, memory, and other resource
options](compose-file-v2.md#cpu-and-other-resources).
If you have further questions, refer to the discussion on the GitHub
issue [docker/compose/4513](https://github.com/docker/compose/issues/4513){: target="_blank" class="_"}.
{: .important}
{% endcomment %}
> スウォームモードではないコンテナーでのリソース設定を探していますか？
>
> ここに説明している設定は、スウォームモードで利用する `deploy` キーにおけるオプションです。
> スウォームモードではないデプロイメントにおけるリソース制約を設定したい場合は、[Compose ファイルバージョン 2 における CPU、メモリなどに関するリソースオプション](compose-file-v2.md#cpu-and-other-resources) を参照してください。
> それでもよくわからない場合は、GitHub 上にあげられている議論 [docker/compose/4513](https://github.com/docker/compose/issues/4513){: target="_blank" class="_"} を参照してください。
{: .important}

##### Out Of Memory Exceptions (OOME)

{% comment %}
If your services or containers attempt to use more memory than the system has
available, you may experience an Out Of Memory Exception (OOME) and a container,
or the Docker daemon, might be killed by the kernel OOM killer. To prevent this
from happening, ensure that your application runs on hosts with adequate memory
and see [Understand the risks of running out of
memory](/engine/admin/resource_constraints.md#understand-the-risks-of-running-out-of-memory).
{% endcomment %}
サービスやコンテナーが、システムにおいて利用可能なメモリ容量以上を利用しようとして、Out Of Memory Exception (OOME) が発生するということがあります。
コンテナーあるいは Docker デーモンは、このときカーネルの OOM killer によって強制終了させられます。
これが発生しないようにするためには、ホスト上で動作させるアプリケーションのメモリ利用を適切に行うことが必要です。
[メモリ不足のリスクへの理解](/engine/admin/resource_constraints.md#understand-the-risks-of-running-out-of-memory) を参照してください。


#### restart_policy

{% comment %}
Configures if and how to restart containers when they exit. Replaces
[`restart`](compose-file-v2.md#orig-resources).
{% endcomment %}
コンテナーが終了した後に、いつどのようにして再起動するかを設定します。
[`restart`](compose-file-v2.md#orig-resources) に置き換わるものです。

{% comment %}
- `condition`: One of `none`, `on-failure` or `any` (default: `any`).
- `delay`: How long to wait between restart attempts, specified as a
  [duration](#specifying-durations) (default: 0).
- `max_attempts`: How many times to attempt to restart a container before giving
  up (default: never give up). If the restart does not succeed within the configured
  `window`, this attempt doesn't count toward the configured `max_attempts` value.
  For example, if `max_attempts` is set to '2', and the restart fails on the first
  attempt, more than two restarts may be attempted.
- `window`: How long to wait before deciding if a restart has succeeded,
  specified as a [duration](#specifying-durations) (default:
  decide immediately).
{% endcomment %}
- `condition`: `none`, `on-failure`, `any` のいずれかを指定します。（デフォルト: `any`）
- `delay`: 再起動までどれだけの間隔をあけるかを [時間](#specifying-durations) として指定します。
  （デフォルト: 0）
- `max_attempts`: 再起動に失敗しても何回までリトライするかを指定します。
  （デフォルト: 無制限）
  設定した `window` 時間内に再起動が成功しなかったとしても、そのときの再起動リトライは、`max_attempts` の値としてカウントされません。
  たとえば `max_attempts` が `2` として設定されていて、1 回めの再起動が失敗したとします。
  この場合、2 回以上の再起動が発生する可能性があります。
- `window`: 再起動が成功したかどうかを決定するためにどれだけ待つかを [時間](#specifying-durations) として指定します。
  （デフォルト: 即時）

```yaml
version: "{{ site.compose_file_v3 }}"
services:
  redis:
    image: redis:alpine
    deploy:
      restart_policy:
        condition: on-failure
        delay: 5s
        max_attempts: 3
        window: 120s
```

#### rollback_config

{% comment %}
> [Version 3.7 file format](compose-versioning.md#version-37) and up
{% endcomment %}
> [ファイルフォーマットバージョン 3.7](compose-versioning.md#version-37) とそれ以上において利用可能です。

{% comment %}
Configures how the service should be rollbacked in case of a failing
update.
{% endcomment %}
サービスの更新が失敗した場合に、どのようにしてロールバックするかを設定します。

{% comment %}
- `parallelism`: The number of containers to rollback at a time. If set to 0, all containers rollback simultaneously.
- `delay`: The time to wait between each container group's rollback (default 0s).
- `failure_action`: What to do if a rollback fails. One of `continue` or `pause` (default `pause`)
- `monitor`: Duration after each task update to monitor for failure `(ns|us|ms|s|m|h)` (default 0s).
- `max_failure_ratio`: Failure rate to tolerate during a rollback (default 0).
- `order`: Order of operations during rollbacks. One of `stop-first` (old task is stopped before starting new one), or `start-first` (new task is started first, and the running tasks briefly overlap) (default `stop-first`).
{% endcomment %}
- `parallelism`: 一度にロールバックするコンテナー数を指定します。
  0 を指定した場合、コンテナーすべてが同時にロールバックされます。
- `delay`: コンテナーグループごとのロールバックの間をどれだけ待つかを指定します。（デフォルト 0s）
- `failure_action`: ロールバックが失敗したときにどうするかを指定します。
  `continue`、 `pause` のいずれかです。（デフォルト `pause`）
- `monitor`: 各タスク更新後に失敗を監視する時間 `(ns|us|ms|s|m|h)` を設定します。
  （デフォルト： 0s）
- `max_failure_ratio`: ロールバック時の失敗許容率を設定します。（デフォルト 0）
- `order`: ロールバック時の処理順を設定します。
  `stop-first` （古いタスクを停止してから新しいタスクを実行する）、あるいは `start-first` （新しいタスクをまず起動させ、タスク起動を一時的に重複させる）のいずれかを設定します。
  （デフォルト： `stop-first`）

#### update_config

{% comment %}
Configures how the service should be updated. Useful for configuring rolling
updates.
{% endcomment %}
どのようにサービスを更新するかを設定します。
Rolling update を設定する際に有効です。

{% comment %}
- `parallelism`: The number of containers to update at a time.
- `delay`: The time to wait between updating a group of containers.
- `failure_action`: What to do if an update fails. One of `continue`, `rollback`, or `pause`
  (default: `pause`).
- `monitor`: Duration after each task update to monitor for failure `(ns|us|ms|s|m|h)` (default 0s).
- `max_failure_ratio`: Failure rate to tolerate during an update.
- `order`: Order of operations during updates. One of `stop-first` (old task is stopped before starting new one), or `start-first` (new task is started first, and the running tasks briefly overlap) (default `stop-first`) **Note**: Only supported for v3.4 and higher.
{% endcomment %}
- `parallelism`： 一度に更新を行うコンテナー数を設定します。
- `delay`： 一連のコンテナーを更新する時間を設定します。
- `failure_action`： 更新に失敗したときの動作を設定します。
  `continue`, `rollback`, `pause` のいずれかです。
  （デフォルト： `pause`）
- `monitor`: 各タスク更新後に失敗を監視する時間 `(ns|us|ms|s|m|h)` を設定します。
  （デフォルト： 0s）
- `max_failure_ratio`: 更新時の失敗許容率を設定します。
- `order`: 更新時の処理順を設定します。
  `stop-first` （古いタスクを停止してから新しいタスクを実行する）、あるいは `start-first` （新しいタスクをまず起動させ、タスク起動を一時的に重複させる）のいずれかを設定します。
  （デフォルト： `stop-first`）
   **メモ**: v3.4 またはそれ以上においてのみサポートされます。

{% comment %}
> **Note**: `order` is only supported for v3.4 and higher of the compose
file format.
{% endcomment %}
> **メモ**: `order` は Compose ファイルフォーマット v3.4 およびそれ以上においてのみサポートされます。

```yaml
version: "{{ site.compose_file_v3 }}"
services:
  vote:
    image: dockersamples/examplevotingapp_vote:before
    depends_on:
      - redis
    deploy:
      replicas: 2
      update_config:
        parallelism: 2
        delay: 10s
        order: stop-first
```

{% comment %}
#### Not supported for `docker stack deploy`
{% endcomment %}
{: id="not-supported-for-docker-stack-deploy" }
#### `docker stack deploy` でサポートされないオプション

{% comment %}
The following sub-options (supported for `docker-compose up` and `docker-compose run`) are _not supported_ for `docker stack deploy` or the `deploy` key.
{% endcomment %}
以下に示すサブオプション（`docker-compose up` と `docker-compose run` においてサポートされるオプション）は、`docker stack deploy` または `deploy` キーにおいては **サポートされません** 。

- [build](#build)
- [cgroup_parent](#cgroup_parent)
- [container_name](#container_name)
- [devices](#devices)
- [tmpfs](#tmpfs)
- [external_links](#external_links)
- [links](#links)
- [network_mode](#network_mode)
- [restart](#restart)
- [security_opt](#security_opt)
- [sysctls](#sysctls)
- [userns_mode](#userns_mode)

{% comment %}
>**Tip:** See the section on [how to configure volumes
for services, swarms, and docker-stack.yml
files](#volumes-for-services-swarms-and-stack-files). Volumes _are_ supported
but to work with swarms and services, they must be configured
as named volumes or associated with services that are constrained to nodes with
access to the requisite volumes.
{% endcomment %}
>**ヒント:** [サービス、スウォーム、docker-stack.yml ファイルにおけるボリューム設定方法](#volumes-for-services-swarms-and-stack-files) にある説明を確認してください。
> ボリュームはサポートされますが、これはスウォームとサービスに対してです。
> ボリュームは名前つきとして設定されるか、あるいは必要なボリュームにアクセスするノードのみから構成されるサービスに関連づけられている必要があります。

### devices

{% comment %}
List of device mappings.  Uses the same format as the `--device` docker
client create option.
{% endcomment %}
デバイスのマッピングをリスト形式で設定します。
Docker クライアントの create オプションと同じ書式にします。

```yaml
devices:
  - "/dev/ttyUSB0:/dev/ttyUSB0"
```

{% comment %}
> **Note**: This option is ignored when
> [deploying a stack in swarm mode](/engine/reference/commandline/stack_deploy.md)
> with a (version 3) Compose file.
{% endcomment %}
> **メモ**: Compose ファイルバージョン 3 においてこのオプションは、[スウォームモードでのスタックのデプロイ](/engine/reference/commandline/stack_deploy.md)を行う場合には無視されます。

### depends_on

{% comment %}
Express dependency between services, Service dependencies cause the following
behaviors:
{% endcomment %}
サービス間の依存関係を表わします。
サービスに依存関係があると、以下の動作になります。

{% comment %}
- `docker-compose up` starts services in dependency order. In the following
  example, `db` and `redis` are started before `web`.
{% endcomment %}
- `docker-compose up` は依存関係の順にサービスを起動します。
  以下の例において `db` と `redis` は `web` の後に起動します。

{% comment %}
- `docker-compose up SERVICE` automatically includes `SERVICE`'s
  dependencies. In the following example, `docker-compose up web` also
  creates and starts `db` and `redis`.
{% endcomment %}
- `docker-compose up SERVICE` を実行すると `SERVICE` における依存関係をもとに動作します。
  以下の例において `docker-compose up web` を実行すると `db` と `redis` を生成して起動します。

{% comment %}
- `docker-compose stop` stops services in dependency order. In the following
  example, `web` is stopped before `db` and `redis`.
{% endcomment %}
- `docker-compose stop` は依存関係の順にサービスを停止します。
  以下の例においては `web` が停止されてから `db` と `redis` が停止されます。

{% comment %}
Simple example:
{% endcomment %}
以下がその簡単な例です。

```yaml
version: "{{ site.compose_file_v3 }}"
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
```

{% comment %}
> There are several things to be aware of when using `depends_on`:
>
> - `depends_on` does not wait for `db` and `redis` to be "ready" before
>   starting `web` - only until they have been started. If you need to wait
>   for a service to be ready, see [Controlling startup order](/compose/startup-order.md)
>   for more on this problem and strategies for solving it.
>
> - Version 3 no longer supports the `condition` form of `depends_on`.
>
> - The `depends_on` option is ignored when
>   [deploying a stack in swarm mode](/engine/reference/commandline/stack_deploy.md)
>   with a version 3 Compose file.
{% endcomment %}
> `depends_on` の利用にあたっては、気をつけておくべきことがあります。
>
> - `depends_on` では `db` や `redis` が「準備」状態になるのを待たずに、つまりそれらを開始したらすぐに `web` を起動します。
>   準備状態になるのを待ってから次のサービスを起動することが必要な場合は、[Compose における起動順の制御](/compose/startup-order.md)にて示す内容と解決方法を確認してください。
>
> - バージョン 3 では `depends_on` の `condition` 形式はサポートされなくなりました。
>
> - Compose ファイルバージョン 3 において `depends_on` オプションは、[スウォームモードでのスタックのデプロイ](/engine/reference/commandline/stack_deploy.md)を行う場合には無視されます。

### dns

{% comment %}
Custom DNS servers. Can be a single value or a list.
{% endcomment %}
DNS サーバーを設定します。
設定は 1 つだけとするか、リストにすることができます。

```yaml
dns: 8.8.8.8
```

```yaml
dns:
  - 8.8.8.8
  - 9.9.9.9
```

### dns_search

{% comment %}
Custom DNS search domains. Can be a single value or a list.
{% endcomment %}
DNS 検索ドメインを設定します。
設定は 1 つだけとするか、リストにすることができます。

```yaml
dns_search: example.com
```

```yaml
dns_search:
  - dc1.example.com
  - dc2.example.com
```

### entrypoint

{% comment %}
Override the default entrypoint.
{% endcomment %}
デフォルトのエントリーポイントを上書きします。

```yaml
entrypoint: /code/entrypoint.sh
```

{% comment %}
The entrypoint can also be a list, in a manner similar to
[dockerfile](/engine/reference/builder.md#entrypoint):
{% endcomment %}
エントリーポイントはリスト形式で設定することができます。
その指定方法は [Dockerfile](/engine/reference/builder.md#entrypoint) と同様です。

```yaml
entrypoint:
    - php
    - -d
    - zend_extension=/usr/local/lib/php/extensions/no-debug-non-zts-20100525/xdebug.so
    - -d
    - memory_limit=-1
    - vendor/bin/phpunit
```

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

```yaml
env_file: .env
```

```yaml
env_file:
  - ./common.env
  - ./apps/web.env
  - /opt/secrets.env
```

{% comment %}
Compose expects each line in an env file to be in `VAR=VAL` format. Lines
beginning with `#` are treated as comments and are ignored. Blank lines are
also ignored.
{% endcomment %}
env ファイルの各行は `VAR=VAL` の書式とします。
行先頭に `#` があると、コメント行となり無視されます。
空行も無視されます。

```bash
# Set Rails/Rack environment
RACK_ENV=development
```

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

```bash
# a.env
VAR=1
```

{% comment %}
and
{% endcomment %}

```bash
# b.env
VAR=hello
```

{% comment %}
`$VAR` is `hello`.
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

```yaml
environment:
  RACK_ENV: development
  SHOW: 'true'
  SESSION_SECRET:
```

```yaml
environment:
  - RACK_ENV=development
  - SHOW=true
  - SESSION_SECRET
```

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

```yaml
expose:
 - "3000"
 - "8000"
```

### external_links

{% comment %}
Link to containers started outside this `docker-compose.yml` or even outside of
Compose, especially for containers that provide shared or common services.
`external_links` follow semantics similar to the legacy option `links` when
specifying both the container name and the link alias (`CONTAINER:ALIAS`).
{% endcomment %}
今の `docker-compose.yml` からではない別のところから起動されたコンテナーをリンクします。
あるいは Compose の外から、特に共有サービスや汎用サービスとして提供されるコンテナーをリンクします。
`external_links` の文法は、かつてのオプション `links` と同様です。
つまりコンテナー名とリンクのエイリアス名（`CONTAINER:ALIAS`）を同時に指定します。

```yaml
external_links:
 - redis_1
 - project_db_1:mysql
 - project_db_1:postgresql
```

{% comment %}
> **Notes:**
>
> If you're using the [version 2 or above file format](compose-versioning.md#version-2), the externally-created  containers
must be connected to at least one of the same networks as the service that is
linking to them. [Links](compose-file-v2#links) are a
legacy option. We recommend using [networks](#networks) instead.
>
> This option is ignored when [deploying a stack in swarm mode](/engine/reference/commandline/stack_deploy.md)
with a (version 3) Compose file.
{% endcomment %}
> **メモ:**
>
> [バージョン 2 またはそれ以上のファイルフォーマット](compose-versioning.md#version-2)を利用しているときに、外部にて生成されたコンテナーをネットワークに接続する場合は、そのコンテナーがサービスとしてリンクしているネットワークのうちの 1 つでなければなりません。
[Links](compose-file-v2#links) は古いオプションです。
これではなく [networks](#networks) を用いるようにしてください。
>
> Compose ファイルバージョン 3 においてこのオプションは、[スウォームモードでのスタックのデプロイ](/engine/reference/commandline/stack_deploy.md)を行う場合には無視されます。

### extra_hosts

{% comment %}
Add hostname mappings. Use the same values as the docker client `--add-host` parameter.
{% endcomment %}
ホスト名のマッピングを追加します。
Docker Client の `--add-host` パラメーターと同じ値を設定してください。

```yaml
extra_hosts:
 - "somehost:162.242.195.82"
 - "otherhost:50.31.209.229"
```

{% comment %}
An entry with the ip address and hostname is created in `/etc/hosts` inside containers for this service, e.g:
{% endcomment %}
ホスト名と IP アドレスによるこの設定内容は、サービスコンテナー内の `/etc/hosts` に追加されます。
たとえば以下のとおりです。

```none
162.242.195.82  somehost
50.31.209.229   otherhost
```

### healthcheck

{% comment %}
> [Version 2.1 file format](compose-versioning.md#version-21) and up.
{% endcomment %}
> [ファイルフォーマットバージョン 2.1](compose-versioning.md#version-21) またはそれ以上。

{% comment %}
Configure a check that's run to determine whether or not containers for this
service are "healthy". See the docs for the
[HEALTHCHECK Dockerfile instruction](/engine/reference/builder.md#healthcheck)
for details on how healthchecks work.
{% endcomment %}
このサービスを起動させているコンテナーが「健康」（healthy）かどうかを確認する処理を設定します。
ヘルスチェックがどのように動作するのかの詳細は [Dockerfile の HEALTHCHECK 命令](/engine/reference/builder.md#healthcheck) を参照してください。

```yaml
healthcheck:
  test: ["CMD", "curl", "-f", "http://localhost"]
  interval: 1m30s
  timeout: 10s
  retries: 3
  start_period: 40s
```

{% comment %}
`interval`, `timeout` and `start_period` are specified as [durations](#specifying-durations).
{% endcomment %}
`interval`, `timeout`, `start_period` は [時間](#specifying-durations) を設定します。

{% comment %}
> **Note**: `start_period` is only supported for v3.4 and higher of the compose
file format.
{% endcomment %}
> **メモ**: `start_period` は Compose ファイルフォーマットバージョン 3.4 またはそれ以上においてのみサポートされます。

{% comment %}
`test` must be either a string or a list. If it's a list, the first item must be
either `NONE`, `CMD` or `CMD-SHELL`. If it's a string, it's equivalent to
specifying `CMD-SHELL` followed by that string.
{% endcomment %}
`test` は 1 つの文字列かリスト形式である必要があります。
リスト形式の場合、第 1 要素は必ず `NONE`, `CMD`, `CMD-SHELL` のいずれかとします。
文字列の場合は、`CMD-SHELL` に続けてその文字列を指定することと同じになります。

{% comment %}
```yaml
# Hit the local web app
test: ["CMD", "curl", "-f", "http://localhost"]
```
{% endcomment %}
```yaml
# ローカルのウェブアプリにアクセスします。
test: ["CMD", "curl", "-f", "http://localhost"]
```

{% comment %}
As above, but wrapped in `/bin/sh`. Both forms below are equivalent.
{% endcomment %}
上と同様ですが、次は `/bin/sh` によってラップされます。
なお以下の 2 つの書式は同じことです。

```yaml
test: ["CMD-SHELL", "curl -f http://localhost || exit 1"]
```

```yaml
test: curl -f https://localhost || exit 1
```

{% comment %}
To disable any default healthcheck set by the image, you can use `disable:
true`. This is equivalent to specifying `test: ["NONE"]`.
{% endcomment %}
イメージが設定するデフォルトのヘルスチェックを無効にするには、`disable: true` を指定します。
これは `test: ["NONE"]` と指定することと同じです。

```yaml
healthcheck:
  disable: true
```

### image

{% comment %}
Specify the image to start the container from. Can either be a repository/tag or
a partial image ID.
{% endcomment %}
コンテナーを起動させるイメージを設定します。
リポジトリ/タグの形式か、あるいは部分イメージ ID により指定します。

    image: redis
    image: ubuntu:14.04
    image: tutum/influxdb
    image: example-registry.com:4000/postgresql
    image: a4bc65fd

{% comment %}
If the image does not exist, Compose attempts to pull it, unless you have also
specified [build](#build), in which case it builds it using the specified
options and tags it with the specified tag.
{% endcomment %}
イメージが存在しなかった場合、[build](#build) を指定していなければ Compose はイメージを取得しようとします。
取得する際には、指定されたオプションを使ってビルドを行い、指定されたタグ名によりタグづけを行います。

### init

{% comment %}
> [Added in version 3.7 file format](compose-versioning.md#version-37).
{% endcomment %}
> [ファイルフォーマットバージョン 3.7](compose-versioning.md#version-37) において追加。

{% comment %}
Run an init inside the container that forwards signals and reaps processes.
Set this option to `true` to enable this feature for the service.
{% endcomment %}
コンテナー内にて init を実行し、シグナルを送信してプロセスを停止させます。
このオプションに `true` を設定することで、サービスに対するこの機能を有効にします。

```yaml
version: "{{ site.compose_file_v3 }}"
services:
  web:
    image: alpine:latest
    init: true
```

{% comment %}
> The default init binary that is used is [Tini](https://github.com/krallin/tini),
> and is installed in `/usr/libexec/docker-init` on the daemon host. You can
> configure the daemon to use a custom init binary through the
> [`init-path` configuration option](/engine/reference/commandline/dockerd/#daemon-configuration-file).
{% endcomment %}
> 利用されるデフォルトの init の実行モジュールは [Tini](https://github.com/krallin/tini) であり、デーモンホストの `/usr/libexec/docker-init` にインストールされています。
> デーモンに対して独自の init 実行モジュールを設定するには、[`init-path` 設定オプション](/engine/reference/commandline/dockerd/#daemon-configuration-file) を利用します。

### isolation

{% comment %}
Specify a container’s isolation technology. On Linux, the only supported value
is `default`. On Windows, acceptable values are `default`, `process` and
`hyperv`. Refer to the
[Docker Engine docs](/engine/reference/commandline/run.md#specify-isolation-technology-for-container---isolation)
for details.
{% endcomment %}
コンテナーの分離技術（isolation technology）を設定します。
Linux においてサポートされるのは `default` のみです。
Windows では `default`, `process`, `hyperv` の設定が可能です。
詳しくは [Docker Engine ドキュメント](/engine/reference/commandline/run.md#specify-isolation-technology-for-container---isolation) を参照してください。

### labels

{% comment %}
Add metadata to containers using [Docker labels](/engine/userguide/labels-custom-metadata.md). You can use either an array or a dictionary.
{% endcomment %}
[Docker labels](/engine/userguide/labels-custom-metadata.md) を使ってコンテナーにメタデータを追加します。
配列形式と辞書形式のいずれかにより指定します。

{% comment %}
It's recommended that you use reverse-DNS notation to prevent your labels from conflicting with those used by other software.
{% endcomment %}
他のソフトウェアが用いるラベルとの競合を避けるため、逆 DNS 記法とすることをお勧めします。

```yaml
labels:
  com.example.description: "Accounting webapp"
  com.example.department: "Finance"
  com.example.label-with-empty-value: ""
```

```yaml
labels:
  - "com.example.description=Accounting webapp"
  - "com.example.department=Finance"
  - "com.example.label-with-empty-value"
```

### links

{% comment %}
>**Warning**: The `--link` flag is a legacy feature of Docker. It
may eventually be removed. Unless you absolutely need to continue using it, we
recommend that you use [user-defined networks](/engine/userguide/networking//#user-defined-networks)
to facilitate communication between two containers instead of using `--link`.
One feature that user-defined networks do not support that you can do with
`--link` is sharing environmental variables between containers. However, you can
use other mechanisms such as volumes to share environment variables between
containers in a more controlled way.
{:.warning}
{% endcomment %}
>**警告**: `--link` フラグはすでに古い機能であり、そのうち削除されるかもしれません。
> このフラグを利用し続ける必要が明確にないのであれば、[ユーザー定義のネットワーク](/engine/userguide/networking//#user-defined-networks) を利用することをお勧めします。
> そうすれば `--link` を用いなくても、2 つのコンテナー間での通信を実現できます。
> ただしユーザー定義のネットワークではサポートされない機能が 1 つあります。
> それは `--link` であれば、コンテナー間で環境変数を共有できるという点です。
> もっともボリュームなどの他のメカニズムを利用すれば、コンテナー間での環境変数の共有は可能であり、その方が、より制御のしやすい方法になります。
{:.warning}

{% comment %}
Link to containers in another service. Either specify both the service name and
a link alias (`SERVICE:ALIAS`), or just the service name.
{% endcomment %}
他サービスのコンテナーをリンクします。
サービス名とリンクのエイリアス名（`SERVICE:ALIAS`）を指定するか、直接サービス名を指定します。

```yaml
web:
  links:
   - db
   - db:database
   - redis
```

{% comment %}
Containers for the linked service are reachable at a hostname identical to
the alias, or the service name if no alias was specified.
{% endcomment %}
リンクされたサービスのコンテナーは、エイリアスと同等のホスト名により到達可能になります。
エイリアスが設定されていない場合はサービス名により到達可能です。

{% comment %}
Links are not required to enable services to communicate - by default,
any service can reach any other service at that service’s name. (See also, the
[Links topic in Networking in Compose](/compose/networking.md#links).)
{% endcomment %}
Links はサービスを通信可能とするために必要になるものではありません。
デフォルトで各サービスは、サービス名を使って他サービスにアクセスすることができます。
（[Compose ネットワークにおける Links のトピック](/compose/networking.md#links) も参照してください。）

{% comment %}
Links also express dependency between services in the same way as
[depends_on](#depends_on), so they determine the order of service startup.
{% endcomment %}
Links は [depends_on](#depends_on) と同様にサービス間の依存関係を表わします。
したがってサービスの起動順を設定するものになります。

{% comment %}
> **Notes**
>
> * If you define both links and [networks](#networks), services with
> links between them must share at least one network in common to
> communicate.
>
> *  This option is ignored when
> [deploying a stack in swarm mode](/engine/reference/commandline/stack_deploy.md)
> with a (version 3) Compose file.
{% endcomment %}
> **メモ**
>
> * links と [networks](#networks) をともに設定する場合、リンクするサービスは、少なくとも 1 つのネットワークが共有され通信ができるようにする必要があります。
>
> * Compose ファイルバージョン 3 においてこのオプションは、[スウォームモードでのスタックのデプロイ](/engine/reference/commandline/stack_deploy.md)を行う場合には無視されます。

### logging

{% comment %}
Logging configuration for the service.
{% endcomment %}
サービスに対するログ記録の設定をします。

```yaml
logging:
  driver: syslog
  options:
    syslog-address: "tcp://192.168.0.42:123"
```

{% comment %}
The `driver`  name specifies a logging driver for the service's
containers, as with the ``--log-driver`` option for docker run
([documented here](/engine/admin/logging/overview.md)).
{% endcomment %}
`driver` 名にはサービスコンテナーにおけるロギングドライバーを指定します。
これは docker run コマンドに対する `--log-driver` オプションと同じです。
（[ドキュメントはこちら](/engine/admin/logging/overview.md)）

{% comment %}
The default value is json-file.
{% endcomment %}
デフォルトは json-file です。

    driver: "json-file"
    driver: "syslog"
    driver: "none"

{% comment %}
> **Note**: Only the `json-file` and `journald` drivers make the logs
available directly from `docker-compose up` and `docker-compose logs`.
Using any other driver does not print any logs.
{% endcomment %}
> **メモ**: ドライバーのうち `json-file` と `journald` だけが、`docker-compose up` と `docker-compose logs` によって直接ログ参照ができます。
その他のドライバーではログ表示は行われません。

{% comment %}
Specify logging options for the logging driver with the ``options`` key, as with the ``--log-opt`` option for `docker run`.
{% endcomment %}
ロギングドライバーへのロギングオプションの設定は ``options`` キーにより行います。
これは `docker run` コマンドの ``--log-opt`` オプションと同じです。

{% comment %}
Logging options are key-value pairs. An example of `syslog` options:
{% endcomment %}
ロギングオプションはキーバリューのペアで指定します。
たとえば `syslog` オプションは以下のようになります。

```yaml
driver: "syslog"
options:
  syslog-address: "tcp://192.168.0.42:123"
```

{% comment %}
The default driver [json-file](/engine/admin/logging/overview.md#json-file), has options to limit the amount of logs stored. To do this, use a key-value pair for maximum storage size and maximum number of files:
{% endcomment %}
デフォルトドライバーである [json-file](/engine/admin/logging/overview.md#json-file) には、保存するログの容量を制限するオプションがあります。
これを利用する場合は、キーバリューのペアを使って、最大保存容量（max-size）と最大ファイル数（max-file）を指定します。

```yaml
options:
  max-size: "200k"
  max-file: "10"
```

{% comment %}
The example shown above would store log files until they reach a `max-size` of
200kB, and then rotate them. The amount of individual log files stored is
specified by the `max-file` value. As logs grow beyond the max limits, older log
files are removed to allow storage of new logs.
{% endcomment %}
上に示した例では、`max-size` に指定された 200 KB に達するまでログファイルへの出力を行います。
最大値に達するとログをローテートします。
保存するログファイル数は `max-file` により指定します。
ログ出力が上限数を越えた場合、古いログファイルは削除され、新たなログファイルへの保存が行われます。

{% comment %}
Here is an example `docker-compose.yml` file that limits logging storage:
{% endcomment %}
以下に示すのは `docker-compose.yml` ファイルにおいてログ保存の制限を行う例です。

```yaml
version: "{{ site.compose_file_v3 }}"
services:
  some-service:
    image: some-service
    logging:
      driver: "json-file"
      options:
        max-size: "200k"
        max-file: "10"
```

{% comment %}
> Logging options available depend on which logging driver you use
>
> The above example for controlling log files and sizes uses options
specific to the [json-file driver](/engine/admin/logging/overview.md#json-file).
These particular options are not available on other logging drivers.
For a full list of supported logging drivers and their options, see
[logging drivers](/engine/admin/logging/overview.md).
{% endcomment %}
> 利用可能なロギングオプションは、利用しているロギングドライバーによって変わります。
>
> 上で示した例においては、ログファイルや容量を制御するために [json-file ドライバー](/engine/admin/logging/overview.md#json-file) に固有のオプションを利用しました。
このようなオプションはその他のロギングドライバーでは利用できません。
サポートされるロギングドライバーと個々のオプションについては [ロギングドライバー](/engine/admin/logging/overview.md) を参照してください。

### network_mode

{% comment %}
Network mode. Use the same values as the docker client `--network` parameter, plus
the special form `service:[service name]`.
{% endcomment %}
ネットワークモードを設定します。
Docker クライアントの `--network` パラメーターと同じ値を設定します。
これに加えて `service:[service name]` という特別な書式も指定可能です。

    network_mode: "bridge"
    network_mode: "host"
    network_mode: "none"
    network_mode: "service:[service name]"
    network_mode: "container:[container name/id]"

{% comment %}
> **Notes**
>
>* This option is ignored when
[deploying a stack in swarm
 mode](/engine/reference/commandline/stack_deploy.md) with a (version 3) Compose
 file.
>
>* `network_mode: "host"` cannot be mixed with [links](#links).
{% endcomment %}
> **メモ**
>
>* Compose ファイルバージョン 3 においてこのオプションは、[スウォームモードでのスタックのデプロイ](/engine/reference/commandline/stack_deploy.md)を行う場合には無視されます。
>
>* `network_mode: "host"` とした場合、[links](#links) を同時に指定することはできません。

### networks

{% comment %}
Networks to join, referencing entries under the
[top-level `networks` key](#network-configuration-reference).
{% endcomment %}
ネットワークへの参加を設定します。
設定には [最上位の `networks` キー](#network-configuration-reference) に設定された値を用います。

```yaml
services:
  some-service:
    networks:
     - some-network
     - other-network
```

#### aliases

{% comment %}
Aliases (alternative hostnames) for this service on the network. Other containers on the same network can use either the service name or this alias to connect to one of the service's containers.
{% endcomment %}
ネットワーク上のサービスに対して、ホスト名の別名となるエイリアスを設定します。
同じネットワーク上にある他のコンテナーは、この 1 つのサービスコンテナーに対して、サービス名か、あるいはそのエイリアスを使ってアクセスすることができます。

{% comment %}
Since `aliases` is network-scoped, the same service can have different aliases on different networks.
{% endcomment %}
`aliases` はネットワーク範囲内において有効です。
ネットワークが異なれば、同一サービスに違うエイリアスを持たせることができます。

{% comment %}
> **Note**: A network-wide alias can be shared by multiple containers, and even by multiple services. If it is, then exactly which container the name resolves to is not guaranteed.
{% endcomment %}
> **メモ**: ネットワーク全体にわたってのエイリアスを複数コンテナー間で共有することができます。
> それは複数サービス間でも可能です。
> ただしこの場合、名前解決がどのコンテナーに対して行われるかは保証されません。

{% comment %}
The general format is shown here.
{% endcomment %}
一般的な書式は以下のとおりです。

```yaml
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
```

{% comment %}
In the example below, three services are provided (`web`, `worker`, and `db`),
along with two networks (`new` and `legacy`). The `db` service is reachable at
the hostname `db` or `database` on the `new` network, and at `db` or `mysql` on
the `legacy` network.
{% endcomment %}
以下の例では 3 つのサービス（`web`, `worker`, `db`）と 2 つのネットワーク（`new` と `legacy`）を提供します。
`db` サービスは `new` ネットワーク上では、ホスト名 `db` あるいは `database` としてアクセスできます。
一方 `legacy` ネットワーク上では `db` あるいは `mysql` としてアクセスできます。

```yaml
version: "{{ site.compose_file_v3 }}"

services:
  web:
    image: "nginx:alpine"
    networks:
      - new

  worker:
    image: "my-worker-image:latest"
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
```

#### ipv4_address, ipv6_address

{% comment %}
Specify a static IP address for containers for this service when joining the network.
{% endcomment %}
サービスをネットワークに参加させる際、そのコンテナーに対してスタティック IP アドレスを設定します。

{% comment %}
The corresponding network configuration in the
[top-level networks section](#network-configuration-reference) must have an
`ipam` block with subnet configurations covering each static address.
{% endcomment %}
[最上位の networks セクション](#network-configuration-reference) の対応するネットワーク設定においては、`ipam` ブロックが必要です。
そこでは各スタティックアドレスに応じたサブネットの設定が必要になります。

{% comment %}
> If IPv6 addressing is desired, the [`enable_ipv6`](compose-file-v2.md##enable_ipv6)
> option must be set, and you must use a [version 2.x Compose file](compose-file-v2.md#ipv4_address-ipv6_address).
> _IPv6 options do not currently work in swarm mode_.
{% endcomment %}
> IPv6 アドレスが必要である場合は、[`enable_ipv6`](compose-file-v2.md##enable_ipv6) オプションの設定が必要になります。
> この場合は [Compose ファイルバージョン 2.x ](compose-file-v2.md#ipv4_address-ipv6_address) を利用しなければなりません。
> **IPv6 オプションは、現時点ではスウォームモード内で動作しません**。

{% comment %}
An example:
{% endcomment %}
以下に例を示します。

```yaml
version: "{{ site.compose_file_v3 }}"

services:
  app:
    image: nginx:alpine
    networks:
      app_net:
        ipv4_address: 172.16.238.10
        ipv6_address: 2001:3984:3989::10

networks:
  app_net:
    ipam:
      driver: default
      config:
        - subnet: "172.16.238.0/24"
        - subnet: "2001:3984:3989::/64"
```

### pid

    pid: "host"

{% comment %}
Sets the PID mode to the host PID mode.  This turns on sharing between
container and the host operating system the PID address space.  Containers
launched with this flag can access and manipulate other
containers in the bare-metal machine's namespace and vice versa.
{% endcomment %}
PID モードをホスト PID モードに設定します。
これはコンテナーとホストオペレーティングシステムとの間で、PID アドレス空間の共有を開始します。
このフラグを使って起動したコンテナーは、ベアメタルマシンの名前空間にあるコンテナーにアクセスし、操作することが可能になります。
逆もまた可能です。

### ports

{% comment %}
Expose ports.
{% endcomment %}
公開用ポートを設定します。

{% comment %}
> **Note**: Port mapping is incompatible with `network_mode: host`
{% endcomment %}
> **メモ**: ポートマッピングは `network_mode: host` とは互換性がありません。

{% comment %}
#### Short syntax
{% endcomment %}
{: id="short-syntax" }
#### 短い文法

{% comment %}
Either specify both ports (`HOST:CONTAINER`), or just the container
port (an ephemeral host port is chosen).
{% endcomment %}
ホスト側とコンテナー側の両方のポートを指定する（`HOST:CONTAINER`）か、あるいはコンテナー側のポートを指定します（ホストポートはエフェメラルに設定されます）。

{% comment %}
> **Note**: When mapping ports in the `HOST:CONTAINER` format, you may experience
> erroneous results when using a container port lower than 60, because YAML
> parses numbers in the format `xx:yy` as a base-60 value. For this reason,
> we recommend always explicitly specifying your port mappings as strings.
{% endcomment %}
> **メモ**: `HOST:CONTAINER` の書式によってポートをマッピングした場合に、コンテナー側のポートが 60 番未満であるとエラーになることがあります。
> これは YAML パーサーが `xx:yy` の書式内にある数値を 60 進数値として解釈するからです。
> このことからポートマッピングを指定する際には、常に文字列として設定することをお勧めします。

```yaml
ports:
 - "3000"
 - "3000-3005"
 - "8000:8000"
 - "9090-9091:8080-8081"
 - "49100:22"
 - "127.0.0.1:8001:8001"
 - "127.0.0.1:5000-5010:5000-5010"
 - "6060:6060/udp"
```

{% comment %}
#### Long syntax
{% endcomment %}
{: id="long-syntax" }
#### 長い文法

{% comment %}
The long form syntax allows the configuration of additional fields that can't be
expressed in the short form.
{% endcomment %}
長い文法は追加の設定項目が加えられていて、短い文法では表現できないものです。

{% comment %}
- `target`: the port inside the container
- `published`: the publicly exposed port
- `protocol`: the port protocol (`tcp` or `udp`)
- `mode`: `host` for publishing a host port on each node, or `ingress` for a swarm
   mode port to be load balanced.
{% endcomment %}
- `target`: コンテナー内部のポート。
- `published`: 公開ポート。
- `protocol`: ポートプロトコル。（`tcp` または `udp`）
- `mode`: `host` は各ノード向けにホストポートを公開、また `ingress` はロードバランスを行うためのスウォームモードポート。

```yaml
ports:
  - target: 80
    published: 8080
    protocol: tcp
    mode: host
```

{% comment %}
> **Note**: The long syntax is new in v3.2
{% endcomment %}
> **メモ**: 長い文法は v3.2 から導入されました。

### restart

{% comment %}
`no` is the default restart policy, and it does not restart a container under
any circumstance. When `always` is specified, the container always restarts. The
`on-failure` policy restarts a container if the exit code indicates an
on-failure error.
{% endcomment %}
再起動ポリシー（restart policy）のデフォルトは `no` です。
この場合はどういう状況であってもコンテナーは再起動しません。
`always` を指定した場合、コンテナーは常に再起動することになります。
また `on-failure` ポリシーでは、終了コードが on-failure エラーを表わしている場合にコンテナーが再起動します。

    restart: "no"
    restart: always
    restart: on-failure
    restart: unless-stopped

{% comment %}
> **Note**: This option is ignored when
> [deploying a stack in swarm mode](/engine/reference/commandline/stack_deploy.md)
> with a (version 3) Compose file. Use [restart_policy](#restart_policy) instead.
{% endcomment %}
> **メモ**: Compose ファイルバージョン 3 においてこのオプションは、[スウォームモードでのスタックのデプロイ](/engine/reference/commandline/stack_deploy.md)を行う場合には無視されます。
> 代わりに [restart_policy](#restart_policy) を利用してください。

### secrets

{% comment %}
Grant access to secrets on a per-service basis using the per-service `secrets`
configuration. Two different syntax variants are supported.
{% endcomment %}
各サービスごとの `secrets` 設定を用いて、個々のサービスごとに secrets へのアクセスを許可します。
2 つの異なる文法がサポートされています。

{% comment %}
> **Note**: The secret must already exist or be
> [defined in the top-level `secrets` configuration](#secrets-configuration-reference)
> of this stack file, or stack deployment fails.
{% endcomment %}
> **メモ**: secrets は既に定義済であるか、あるいは [最上位の `secrets` が定義済](#configs-configuration-reference) である必要があります。
> そうでない場合、このファイルによるデプロイが失敗します。

{% comment %}
For more information on secrets, see [secrets](/engine/swarm/secrets.md).
{% endcomment %}
secrets に関する詳細は [secrets](/engine/swarm/secrets.md) を参照してください。

{% comment %}
#### Short syntax
{% endcomment %}
{: id="secrets-short-syntax" }
#### Short syntax

{% comment %}
The short syntax variant only specifies the secret name. This grants the
container access to the secret and mounts it at `/run/secrets/<secret_name>`
within the container. The source name and destination mountpoint are both set
to the secret name.
{% endcomment %}
短い文法では secret 名を指定することだけができます。
これを行うと、コンテナーが secret にアクセスできるようになり、secret はコンテナー内の `/run/secrets/<secret_name>` にマウントされます。
ソース元となる名前とマウントポイント名は、ともに secret 名となります。

{% comment %}
The following example uses the short syntax to grant the `redis` service
access to the `my_secret` and `my_other_secret` secrets. The value of
`my_secret` is set to the contents of the file `./my_secret.txt`, and
`my_other_secret` is defined as an external resource, which means that it has
already been defined in Docker, either by running the `docker secret create`
command or by another stack deployment. If the external secret does not exist,
the stack deployment fails with a `secret not found` error.
{% endcomment %}
以下の例では短い文法を使って、`redis` サービスが 2 つの secret、つまり `my_secret` と `my_other_secret` にアクセスできるようにします。
`my_secret` の値には `./my_secret.txt` ファイルに含まれる内容を設定します。
`my_other_secret` は外部リソースとして定義し、それはつまり Docker において定義済の内容を用います。
これは `docker secret create` コマンドの実行か、あるいは別のスタックデプロイメントにより与えられます。
外部 secret が存在しなかった場合、スタックデプロイメントは失敗し `secret not found` といったエラーが発生します。

```yaml
version: "{{ site.compose_file_v3 }}"
services:
  redis:
    image: redis:latest
    deploy:
      replicas: 1
    secrets:
      - my_secret
      - my_other_secret
secrets:
  my_secret:
    file: ./my_secret.txt
  my_other_secret:
    external: true
```

{% comment %}
#### Long syntax
{% endcomment %}
{: id="secrets-long-syntax" }
#### 長い文法

{% comment %}
The long syntax provides more granularity in how the secret is created within
the service's task containers.
{% endcomment %}
長い文法ではさらに細かな設定として、サービスタスクのコンテナー内にて secret をどのように生成するかを設定します。

{% comment %}
- `source`: The name of the secret as it exists in Docker.
- `target`: The name of the file to be mounted in `/run/secrets/` in the
  service's task containers. Defaults to `source` if not specified.
- `uid` and `gid`: The numeric UID or GID that owns the file within
  `/run/secrets/` in the service's task containers. Both default to `0` if not
  specified.
- `mode`: The permissions for the file to be mounted in `/run/secrets/`
  in the service's task containers, in octal notation. For instance, `0444`
  represents world-readable. The default in Docker 1.13.1 is `0000`, but is
  be `0444` in newer versions. Secrets cannot be writable because they are mounted
  in a temporary filesystem, so if you set the writable bit, it is ignored. The
  executable bit can be set. If you aren't familiar with UNIX file permission
  modes, you may find this
  [permissions calculator](http://permissions-calculator.org/){: target="_blank" class="_" }
  useful.
{% endcomment %}
- `source`: Docker 内に存在している secret 名。
- `target`: サービスのタスクコンテナーにおいて `/run/secrets/` にマウントされるファイル名。
  指定されなかった場合のデフォルトは `source` となる。
- `uid` と `gid`: サービスのタスクコンテナーにおいて `/run/secrets/` 内のファイルを所有する UID 値と GID 値。

  指定されなかった場合のデフォルトはいずれも `0`。
- `mode`: サービスのタスクコンテナーにおいて `/run/secrets/` にマウントされるファイルのパーミッション。
  8 進数表記。
  たとえば `0444` であればすべて読み込み可。
  Docker 1.13.1 におけるデフォルトは `0000` でしたが、それ以降では `0444` となりました。
  secrets はテンポラリなファイルシステム上にマウントされるため、書き込み可能にはできません。
  したがって書き込みビットを設定しても無視されます。
  実行ビットは設定できます。
  UNIX のファイルパーミッションモードに詳しくない方は、[パーミッション計算機](http://permissions-calculator.org/){: target="_blank" class="_" }を参照してください。

{% comment %}
The following example sets name of the `my_secret` to `redis_secret` within the
container, sets the mode to `0440` (group-readable) and sets the user and group
to `103`. The `redis` service does not have access to the `my_other_secret`
secret.
{% endcomment %}
以下の例では `my_secret` という名前の secret を、コンテナー内では `redis_secret` として設定します。
そしてモードを `0440`（グループが読み込み可能）とし、ユーザーとグループは `103` とします。
`redis` サービスは `my_other_secret` へはアクセスしません。

```yaml
version: "{{ site.compose_file_v3 }}"
services:
  redis:
    image: redis:latest
    deploy:
      replicas: 1
    secrets:
      - source: my_secret
        target: redis_secret
        uid: '103'
        gid: '103'
        mode: 0440
secrets:
  my_secret:
    file: ./my_secret.txt
  my_other_secret:
    external: true
```

{% comment %}
You can grant a service access to multiple secrets and you can mix long and
short syntax. Defining a secret does not imply granting a service access to it.
{% endcomment %}
1 つのサービスが複数の secrets にアクセスする設定とすることもできます。
また短い文法、長い文法を混在することも可能です。
secret を定義しただけでは、サービスに対して secret へのアクセスを許可するものにはなりません。

### security_opt

{% comment %}
Override the default labeling scheme for each container.
{% endcomment %}
各コンテナーにおけるデフォルトのラベリングスキーム（labeling scheme）を上書きします。

```yaml
security_opt:
  - label:user:USER
  - label:role:ROLE
```

{% comment %}
> **Note**: This option is ignored when
> [deploying a stack in swarm mode](/engine/reference/commandline/stack_deploy.md)
> with a (version 3) Compose file.
{% endcomment %}
> **メモ**: Compose ファイルバージョン 3 においてこのオプションは、[スウォームモードでのスタックのデプロイ](/engine/reference/commandline/stack_deploy.md)を行う場合には無視されます。

### stop_grace_period

{% comment %}
Specify how long to wait when attempting to stop a container if it doesn't
handle SIGTERM (or whatever stop signal has been specified with
[`stop_signal`](#stopsignal)), before sending SIGKILL. Specified
as a [duration](#specifying-durations).
{% endcomment %}
コンテナーが SIGKILL を送信するまでに、SIGTERM（あるいは [`stop_signal`](#stopsignal) によって設定されたストップシグナル）をどれだけ待つかを設定します。
指定には [時間](#specifying-durations) を用います。

    stop_grace_period: 1s
    stop_grace_period: 1m30s

{% comment %}
By default, `stop` waits 10 seconds for the container to exit before sending
SIGKILL.
{% endcomment %}
デフォルトで、コンテナーが SIGKILL を送信する前に `stop` は 10 秒待ちます。

### stop_signal

{% comment %}
Sets an alternative signal to stop the container. By default `stop` uses
SIGTERM. Setting an alternative signal using `stop_signal` causes
`stop` to send that signal instead.
{% endcomment %}
コンテナーに対して別の停止シグナルを設定します。
デフォルトにおいて `stop` は SIGTERM を用います。
`stop_signal` を使って別のシグナルを設定すると `stop` にはそのシグナルが代わりに送信されます。

```yaml
stop_signal: SIGUSR1
```

### sysctls

{% comment %}
Kernel parameters to set in the container. You can use either an array or a
dictionary.
{% endcomment %}
コンテナーに設定するカーネルパラメーターを設定します。
配列または辞書形式での指定ができます。

```yaml
sysctls:
  net.core.somaxconn: 1024
  net.ipv4.tcp_syncookies: 0
```

```yaml
sysctls:
  - net.core.somaxconn=1024
  - net.ipv4.tcp_syncookies=0
```

{% comment %}
> **Note**: This option is ignored when
> [deploying a stack in swarm mode](/engine/reference/commandline/stack_deploy.md)
> with a (version 3) Compose file.
{% endcomment %}
> **メモ**: Compose ファイルバージョン 3 においてこのオプションは、[スウォームモードでのスタックのデプロイ](/engine/reference/commandline/stack_deploy.md)を行う場合には無視されます。

### tmpfs

{% comment %}
> [Version 2 file format](compose-versioning.md#version-2) and up.
{% endcomment %}
> [ファイルフォーマットバージョン 2](compose-versioning.md#version-2) またはそれ以上。

{% comment %}
Mount a temporary file system inside the container. Can be a single value or a list.
{% endcomment %}
コンテナー内においてテンポラリファイルシステムをマウントします。
設定は 1 つだけとするか、リストにすることができます。

```yaml
tmpfs: /run
```

```yaml
tmpfs:
  - /run
  - /tmp
```

{% comment %}
> **Note**: This option is ignored when
> [deploying a stack in swarm mode](/engine/reference/commandline/stack_deploy.md)
> with a (version 3-3.5) Compose file.
{% endcomment %}
> **メモ**: Compose ファイルバージョン 3 ～ 3.5 においてこのオプションは、[スウォームモードでのスタックのデプロイ](/engine/reference/commandline/stack_deploy.md)を行う場合には無視されます。

{% comment %}
> [Version 3.6 file format](compose-versioning.md#version-3) and up.
{% endcomment %}
> [ファイルフォーマットバージョン 3.6 ](compose-versioning.md#version-3) またはそれ以上。

{% comment %}
Mount a temporary file system inside the container. Size parameter specifies the size
of the tmpfs mount in bytes. Unlimited by default.
{% endcomment %}
コンテナー内部にてテンポラリファイルシステムをマウントします。
size パラメーターは tmpfs マウントのサイズをバイト単位で指定します。
デフォルトは無制限です。

```yaml
 - type: tmpfs
     target: /app
     tmpfs:
       size: 1000
```

### ulimits

{% comment %}
Override the default ulimits for a container. You can either specify a single
limit as an integer or soft/hard limits as a mapping.
{% endcomment %}
コンテナーにおけるデフォルトの ulimits を上書きします。
1 つの limit を整数値として指定するか、ソフト、ハードの limit をマッピングとして指定することができます。


```yaml
ulimits:
  nproc: 65535
  nofile:
    soft: 20000
    hard: 40000
```

### userns_mode

```yaml
userns_mode: "host"
```

{% comment %}
Disables the user namespace for this service, if Docker daemon is configured with user namespaces.
See [dockerd](/engine/reference/commandline/dockerd.md#disable-user-namespace-for-a-container) for
more information.
{% endcomment %}
Docker デーモンにおいてユーザー名前空間が設定されていても、サービスに対してユーザー名前空間を無効にします。
詳しくは [dockerd](/engine/reference/commandline/dockerd.md#disable-user-namespace-for-a-container) を参照してください。

{% comment %}
> **Note**: This option is ignored when
> [deploying a stack in swarm mode](/engine/reference/commandline/stack_deploy.md)
> with a (version 3) Compose file.
{% endcomment %}
> **メモ**: Compose ファイルバージョン 3 においてこのオプションは、[スウォームモードでのスタックのデプロイ](/engine/reference/commandline/stack_deploy.md)を行う場合には無視されます。

### volumes

{% comment %}
Mount host paths or named volumes, specified as sub-options to a service.
{% endcomment %}
マウントホストパスや名前つきボリュームを、サービスに対するサブオプションとして指定します。

{% comment %}
You can mount a host path as part of a definition for a single service, and
there is no need to define it in the top level `volumes` key.
{% endcomment %}
ホストパスのマウントは、単一サービスの定義の一部として行うことができます。
これは最上位の `volumes` キーにて定義する必要はありません。

{% comment %}
But, if you want to reuse a volume across multiple services, then define a named
volume in the [top-level `volumes` key](#volume-configuration-reference). Use
named volumes with [services, swarms, and stack
files](#volumes-for-services-swarms-and-stack-files).
{% endcomment %}
ただし複数のサービスにわたってボリュームを再利用したい場合は、[最上位の `volumes` キー](#volume-configuration-reference) において名前つきボリュームを定義してください。
名前つきボリュームは [サービス、スウォーム、スタックファイル](#volumes-for-services-swarms-and-stack-files) において用いられます。

{% comment %}
> **Note**: The top-level
> [volumes](#volume-configuration-reference) key defines
> a named volume and references it from each service's `volumes` list. This replaces `volumes_from` in earlier versions of the Compose file format. See [Use volumes](/engine/admin/volumes/volumes.md) and [Volume
Plugins](/engine/extend/plugins_volume.md) for general information on volumes.
{% endcomment %}
> **メモ**: 最上位の [volumes](#volume-configuration-reference) キーは名前つきボリュームを定義し、各サービスの `volumes` リストからこれを参照します。
> これは Compose ファイルフォーマットのかつてのバージョンにおける `volumes_from` に置き換わるものです。
> ボリュームに関する一般的な情報については [ボリュームの利用](/engine/admin/volumes/volumes.md) や [ボリュームプラグイン](/engine/extend/plugins_volume.md) を参照してください。

{% comment %}
This example shows a named volume (`mydata`) being used by the `web` service,
and a bind mount defined for a single service (first path under `db` service
`volumes`). The `db` service also uses a named volume called `dbdata` (second
path under `db` service `volumes`), but defines it using the old string format
for mounting a named volume. Named volumes must be listed under the top-level
`volumes` key, as shown.
{% endcomment %}
以下の例では名前つきボリューム（`mydata`）が `web` サービスにおいて利用されています。
またバインドマウントが単一のサービス（`db` サービスの `volumes` にある最初のパス）に対して定義されています。
`db` サービスはさらに `dbdata` という名前つきボリュームを利用しています（`db` サービスの `volumes` の 2 つめのパス）が、その定義の仕方は、名前つきボリュームをマウントする古い文字列書式を使っています。
名前つきボリュームは以下に示しているように、最上位の `volumes` キーのもとに列記されていなければなりません。

```yaml
version: "{{ site.compose_file_v3 }}"
services:
  web:
    image: nginx:alpine
    volumes:
      - type: volume
        source: mydata
        target: /data
        volume:
          nocopy: true
      - type: bind
        source: ./static
        target: /opt/app/static

  db:
    image: postgres:latest
    volumes:
      - "/var/run/postgres/postgres.sock:/var/run/postgres/postgres.sock"
      - "dbdata:/var/lib/postgresql/data"

volumes:
  mydata:
  dbdata:
```

{% comment %}
> **Note**: See [Use volumes](/engine/admin/volumes/volumes.md) and [Volume
> Plugins](/engine/extend/plugins_volume.md) for general information on volumes.
{% endcomment %}
> **メモ**: ボリュームに関する一般的な情報については [ボリュームの利用](/engine/admin/volumes/volumes.md) や [ボリュームプラグイン](/engine/extend/plugins_volume.md) を参照してください。


{% comment %}
#### Short syntax
{% endcomment %}
{: id="volumes-short-syntax" }
#### 短い文法

{% comment %}
Optionally specify a path on the host machine
(`HOST:CONTAINER`), or an access mode (`HOST:CONTAINER:ro`).
{% endcomment %}
設定方法として、ホストマシンのパスを指定する方法（`HOST:CONTAINER`）や、さらにアクセスモードを指定する方法（`HOST:CONTAINER:ro`）があります。

{% comment %}
You can mount a relative path on the host, that expands relative to
the directory of the Compose configuration file being used. Relative paths
should always begin with `.` or `..`.
{% endcomment %}
ホスト上の相対パスをマウントすることができます。
これは、用いられている Compose 設定ファイルのディレクトリからの相対パスとして展開されます。
相対パスは `.` または `..` で書き始める必要があります。

{% comment %}
```yaml
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
```
{% endcomment %}
```yaml
volumes:
  # パス指定のみ。Engine にボリュームを生成させます。
  - /var/lib/mysql

  # 絶対パスのマッピングを指定。
  - /opt/data:/var/lib/mysql

  # ホストからのパス指定。Compose ファイルからの相対パス。
  - ./cache:/tmp/cache

  # ユーザーディレクトリからの相対パス。
  - ~/configs:/etc/configs/:ro

  # 名前つきボリューム。
  - datavolume:/var/lib/mysql
```

{% comment %}
#### Long syntax
{% endcomment %}
{: id="volumes-long-syntax" }
#### 長い文法

{% comment %}
The long form syntax allows the configuration of additional fields that can't be
expressed in the short form.
{% endcomment %}
長い文法は追加の設定項目が加えられていて、短い文法では表現できないものです。

{% comment %}
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
- `consistency`: the consistency requirements of the mount, one of `consistent` (host and container have identical view), `cached` (read cache, host view is authoritative) or `delegated` (read-write cache, container's view is authoritative)
{% endcomment %}
- `type`: マウントタイプを表わす `volume`, `bind`, `tmpfs`, `npipe` のいずれかを指定します。
- `source`: マウント元。バインドマウントにおいてはホスト上のパスを指定します。
  また [最上位の `volumes` キー](#volume-configuration-reference) で定義したボリューム名を指定します。
  tmpfs マウントはできません。
- `target`: ボリュームがマウントされるコンテナー内のパスを指定します。
- `read_only`: ボリュームを読み込み専用に設定します。
- `bind`: 追加のバインドオプションを設定します。
  - `propagation`: バインドにおいて伝播モード（propagation mode）を利用します。
- `volume`: 追加のボリュームオプションを設定します。
  - `nocopy`: ボリュームの生成時にはコンテナーからのデータコピーを無効にします。
- `tmpfs`: 追加の tmpfs オプションを設定します。
  - `size`: tmpfs マウントのサイズをバイト数で指定します。
- `consistency`: マウントに求める一貫性を指定します。以下のいずれか： `consistent` （ホストとコンテナーは同一ビューを持ちます）、 `cached` （読み込みキャッシュ、ホストビューに権限あり）、`delegated` (読み書きキャッシュ、コンテナービューに権限あり）

```yaml
version: "{{ site.compose_file_v3 }}"
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

{% comment %}
> **Note**: The long syntax is new in v3.2
{% endcomment %}
> **メモ**: 長い文法は v3.2 から導入されました。


{% comment %}
#### Volumes for services, swarms, and stack files
{% endcomment %}
{: id="volumes-for-services-swarms-and-stack-files" }
#### サービス、スウォーム、スタックファイルに対するボリューム設定

{% comment %}
When working with services, swarms, and `docker-stack.yml` files, keep in mind
that the tasks (containers) backing a service can be deployed on any node in a
swarm, and this may be a different node each time the service is updated.
{% endcomment %}
サービス、スウォーム、`docker-stack.yml` ファイルを扱っている際には気をつけておくべきことがあります。
サービスのもとにあるタスク（コンテナー）はスォーム内のどのノードにでもデプロイされます。
サービスは毎回、異なるノードとなり得るということです。

{% comment %}
In the absence of having named volumes with specified sources, Docker creates an
anonymous volume for each task backing a service. Anonymous volumes do not
persist after the associated containers are removed.
{% endcomment %}
ボリュームに名前をつけずに利用したとすると、Docker はサービスのもとにある各タスクに対して、名前のない匿名のボリュームを生成します。
匿名ボリュームは、関連コンテナーが削除された後は持続されません。

{% comment %}
If you want your data to persist, use a named volume and a volume driver that
is multi-host aware, so that the data is accessible from any node. Or, set
constraints on the service so that its tasks are deployed on a node that has the
volume present.
{% endcomment %}
データを維持しておきたい場合は、名前つきボリュームを設定し、複数ホストに対応したボリュームドライバーを利用してください。
そうすればデータはどのノードからでもアクセスできます。
あるいはサービスに対する指定として、ボリュームが存在しているノードへタスクをデプロイするようにしてください。

{% comment %}
As an example, the `docker-stack.yml` file for the
[votingapp sample in Docker
Labs](https://github.com/docker/labs/blob/master/beginner/chapters/votingapp.md) defines a service called `db` that runs a `postgres` database. It is
configured as a named volume to persist the data on the swarm,
_and_ is constrained to run only on `manager` nodes. Here is the relevant snip-it from that file:
{% endcomment %}
例として [Docker Labs にある投票アプリ](https://github.com/docker/labs/blob/master/beginner/chapters/votingapp.md) では、`docker-stack.yml` にて `postgres` データベースを起動する `db` サービスが定義されています。
そして名前つきボリュームを設定して、スウォーム内のデータを失わないようにしています。
**さらに**それは `manager` ノードでのみ稼動するように限定しています。
以下は該当するファイル部分の抜粋です。

```yaml
version: "{{ site.compose_file_v3 }}"
services:
  db:
    image: postgres:9.4
    volumes:
      - db-data:/var/lib/postgresql/data
    networks:
      - backend
    deploy:
      placement:
        constraints: [node.role == manager]
```

{% comment %}
#### Caching options for volume mounts (Docker Desktop for Mac)
{% endcomment %}
{: id="caching-options-for-volume-mounts-docker-desktop-for-mac" }
#### ボリュームマウントに対するキャッシュオプション（Docker for Mac）

{% comment %}
On Docker 17.04 CE Edge and up, including 17.06 CE Edge and Stable, you can
configure container-and-host consistency requirements for bind-mounted
directories in Compose files to allow for better performance on read/write of
volume mounts. These options address issues specific to `osxfs` file sharing,
and therefore are only applicable on Docker Desktop for Mac.
{% endcomment %}
Docker 17.04 CE Edge とそれ以上の 17.06 CE Edge や Stable においては、Compose ファイル内にてバインドマウントするディレクトリの、コンテナーホスト間の一貫性を設定することができます。
これによってボリュームの読み書き性能を向上させることができます。
これを実現するオプションは `osxfs` ファイル共有に対する問題に対処しているため、Docker Desktop for Mac においてのみ利用可能です。

{% comment %}
The flags are:
{% endcomment %}
フラグとして以下があります。

{% comment %}
* `consistent`: Full consistency. The container runtime and the
host maintain an identical view of the mount at all times.  This is the default.
{% endcomment %}
* `consistent`: 完全な一貫性を持ちます。
  起動しているコンテナーとホストは、常にマウント上を同一に見ることができます。
  これがデフォルトです。

{% comment %}
* `cached`: The host's view of the mount is authoritative. There may be
delays before updates made on the host are visible within a container.
{% endcomment %}
* `cached`: ホスト側マウントが優先されます。
  ホスト上の更新が、コンテナー内で確認できるまでには遅延が起こりえます。

{% comment %}
* `delegated`: The container runtime's view of the mount is
authoritative. There may be delays before updates made in a container
are visible on the host.
{% endcomment %}
* `delegated`: コンテナー実行時のコンテナー側マウントが優先されます。
  コンテナー内での更新が、ホスト上で確認できるまでには遅延が起こりえます。

{% comment %}
Here is an example of configuring a volume as `cached`:
{% endcomment %}
以下はボリュームに `cached` を設定した例です。

```yaml
version: "{{ site.compose_file_v3 }}"
services:
  php:
    image: php:7.1-fpm
    ports:
      - "9000"
    volumes:
      - .:/var/www/project:cached
```

{% comment %}
Full detail on these flags, the problems they solve, and their
`docker run` counterparts is in the Docker Desktop for Mac topic [Performance tuning for
volume mounts (shared filesystems)](/docker-for-mac/osxfs-caching.md).
{% endcomment %}
このフラグの詳細、これにより解決される諸問題、`docker run` での対応オプションについては Docker Desktop for Mac のトピック、[ボリュームマウント（共有ファイルシステム）でのパフォーマンスチューニング](/docker-for-mac/osxfs-caching.md) を参照してください。

### domainname, hostname, ipc, mac\_address, privileged, read\_only, shm\_size, stdin\_open, tty, user, working\_dir

{% comment %}
Each of these is a single value, analogous to its
[docker run](/engine/reference/run.md) counterpart. Note that `mac_address` is a legacy option.
{% endcomment %}
ここに示すオプションはいずれも、値 1 つを設定するものであり、[docker run](/engine/reference/run.md) のオプションに対応づいています。
なお `mac_address` は古くなったオプションです。

```yaml
user: postgresql
working_dir: /code

domainname: foo.com
hostname: foo
ipc: host
mac_address: 02:42:ac:11:65:43

privileged: true


read_only: true
shm_size: 64M
stdin_open: true
tty: true
```

{% comment %}
## Specifying durations
{% endcomment %}
{: id="specifying-durations" }
## 時間の指定

{% comment %}
Some configuration options, such as the `interval` and `timeout` sub-options for
[`check`](#healthcheck), accept a duration as a string in a
format that looks like this:
{% endcomment %}
[`check`](#healthcheck) のサブオプション `interval`、`timeout` のように、時間を設定するオプションがあります。
これは以下のような書式による文字列を時間として受け付けるものです。

    2.5s
    10s
    1m30s
    2h32m
    5h34m56s

{% comment %}
The supported units are `us`, `ms`, `s`, `m` and `h`.
{% endcomment %}
サポートされる単位は `us`, `ms`, `s`, `m`, `h` です。


{% comment %}
## Specifying byte values
{% endcomment %}
{: #specifying-byte-values }
## バイト値の表現

{% comment %}
Some configuration options, such as the `shm_size` sub-option for
[`build`](#build), accept a byte value as a string in a format
that looks like this:
{% endcomment %}
[`build`](#build) のサブオプション `shm_size` のようにバイト値を設定するオプションがあります。
バイト値は文字列として指定するものであり、以下のようになります。

    2b
    1024kb
    2048k
    300m
    1gb

{% comment %}
The supported units are `b`, `k`, `m` and `g`, and their alternative notation `kb`,
`mb` and `gb`. Decimal values are not supported at this time.
{% endcomment %}
サポートされる単位は `b`, `k`, `m`, `g` とそれに応じた `kb`, `mb`, `gb` です。
現時点にて 10 進数値の指定はサポートされていません。


{% comment %}
## Volume configuration reference
{% endcomment %}
{: id="volume-configuration-reference" }
## ボリューム設定リファレンス

{% comment %}
While it is possible to declare [volumes](#volumes) on the file as part of the
service declaration, this section allows you to create named volumes (without
relying on `volumes_from`) that can be reused across multiple services, and are
easily retrieved and inspected using the docker command line or API. See the
[docker volume](/engine/reference/commandline/volume_create.md) subcommand
documentation for more information.
{% endcomment %}
サービスの宣言の一部として、ファイル上に [ボリューム](#volumes) を宣言することが可能ですが、このセクションでは（`volumes_from` を利用せずに）名前つきボリュームを生成する方法を説明します。
このボリュームは、複数のサービスにわたっての再利用が可能であり、docker コマンドラインや API を使って簡単に抽出したり確認したりすることができます。
詳しくは [docker volume](/engine/reference/commandline/volume_create.md) のサブコマンドを確認してください。

{% comment %}
See [Use volumes](/engine/admin/volumes/volumes.md) and [Volume
Plugins](/engine/extend/plugins_volume.md) for general information on volumes.
{% endcomment %}
ボリュームに関する一般的な情報については [ボリュームの利用](/engine/admin/volumes/volumes.md) や [ボリュームプラグイン](/engine/extend/plugins_volume.md) を参照してください。

{% comment %}
Here's an example of a two-service setup where a database's data directory is
shared with another service as a volume so that it can be periodically backed
up:
{% endcomment %}
以下の例では 2 つのサービスを用います。
データベースのデータディレクトリは、もう一方のサービスに対してボリュームとして共有させます。
これによりデータが定期的に反映されます。

```yaml
version: "{{ site.compose_file_v3 }}"

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
```

{% comment %}
An entry under the top-level `volumes` key can be empty, in which case it
uses the default driver configured by the Engine (in most cases, this is the
`local` driver). Optionally, you can configure it with the following keys:
{% endcomment %}
最上位の `volumes` キーは指定しないようにすることもできます。
その場合は Engine によってデフォルトで設定されているドライバーが用いられます。
（たいていは `local` ドライバーとなります。）
さらに追加で、以下のようなキーを設定することができます。

### driver

{% comment %}
Specify which volume driver should be used for this volume. Defaults to whatever
driver the Docker Engine has been configured to use, which in most cases is
`local`. If the driver is not available, the Engine returns an error when
`docker-compose up` tries to create the volume.
{% endcomment %}
どのボリュームドライバーを現在のボリュームに対して用いるかを指定します。
デフォルトは Docker Engine が利用するものとして設定されているドライバーになります。
たいていは `local` です。
ドライバーが利用できない場合、`docker-compose up` によってボリューム生成が行われる際に Engine がエラーを返します。

```yaml
driver: foobar
```

### driver_opts

{% comment %}
Specify a list of options as key-value pairs to pass to the driver for this
volume. Those options are driver-dependent - consult the driver's
documentation for more information. Optional.
{% endcomment %}
このボリュームが利用するドライバーに対して、受け渡したいオプションをキーバリューペアのリストとして設定します。
このオプションは各ドライバーによって異なります。
詳しくは各ドライバーのドキュメントを参照してください。
設定は任意です。

```yaml
volumes:
  example:
    driver_opts:
      type: "nfs"
      o: "addr=10.40.0.199,nolock,soft,rw"
      device: ":/docker/example"
```

### external

{% comment %}
If set to `true`, specifies that this volume has been created outside of
Compose. `docker-compose up` does not attempt to create it, and raises
an error if it doesn't exist.
{% endcomment %}
このオプションを `true` に設定することにより、Compose の外部において生成されているボリュームを設定します。
`docker-compose up` はボリュームを生成しないようになりますが、ボリュームが存在しなければエラーとなります。

{% comment %}
For version 3.3 and below of the format, `external` cannot be used in
conjunction with other volume configuration keys (`driver`, `driver_opts`,
`labels`). This limitation no longer exists for
[version 3.4](compose-versioning.md#version-34) and above.
{% endcomment %}
バージョン 3.3 およびそれ以前にて、`external` は他のボリューム設定キー（`driver`, `driver_opts`, `labels`）と同時に用いることはできませんでした。
この制約は [バージョン 3.4](compose-versioning.md#version-34) 以降においてはなくなりました。

{% comment %}
In the example below, instead of attempting to create a volume called
`[projectname]_data`, Compose looks for an existing volume simply
called `data` and mount it into the `db` service's containers.
{% endcomment %}
以下の例では `[projectname]_data` というボリュームは生成されることはなく、Compose はすでに存在している `data` という単純な名前のボリュームを探しにいきます。
そしてこれを `db` サービスコンテナー内にマウントします。

```yaml
version: "{{ site.compose_file_v3 }}"

services:
  db:
    image: postgres
    volumes:
      - data:/var/lib/postgresql/data

volumes:
  data:
    external: true
```

{% comment %}
> [external.name was deprecated in version 3.4 file format](compose-versioning.md#version-34)
> use `name` instead.
{% endcomment %}
> [ファイルフォーマットバージョン 3.4 において external.name は廃止予定となりました](compose-versioning.md#version-34)。
> 代わりに `name` を用いてください。

{% comment %}
You can also specify the name of the volume separately from the name used to
refer to it within the Compose file:
{% endcomment %}
ボリューム名として指定する名前は、Compose ファイル内で参照されている名前以外でも指定することができます。

```yaml
volumes:
  data:
    external:
      name: actual-name-of-volume
```

{% comment %}
> External volumes are always created with docker stack deploy
>
External volumes that do not exist _are created_ if you use [docker stack
deploy](#deploy) to launch the app in [swarm mode](/engine/swarm/index.md)
(instead of [docker compose up](/compose/reference/up.md)). In swarm mode, a
volume is automatically created when it is defined by a service. As service
tasks are scheduled on new nodes,
[swarmkit](https://github.com/docker/swarmkit/blob/master/README.md) creates the
volume on the local node. To learn more, see
[moby/moby#29976](https://github.com/moby/moby/issues/29976).
{% endcomment %}
> external ボリュームは docker stack deploy により常に生成されます。
>
external ボリュームが存在しない場合に、[docker stack deploy](#deploy) を実行してアプリを [スウォームモード](/engine/swarm/index.md) 内に導入すると、ボリュームが**生成されます**。
（[docker compose up](/compose/reference/up.md) とは異なります。）
スウォームモードにおいて、サービスとして定義されているボリュームは自動生成されます。
サービスタスクは新たなノード上においてスケジューリングされるので、[swarmkit](https://github.com/docker/swarmkit/blob/master/README.md) がローカルノード上にボリュームを生成します。
詳しくは [moby/moby#29976](https://github.com/moby/moby/issues/29976) を参照してください。

### labels

{% comment %}
Add metadata to containers using
[Docker labels](/engine/userguide/labels-custom-metadata.md). You can use either
an array or a dictionary.
{% endcomment %}
[Docker labels](/engine/userguide/labels-custom-metadata.md) を使ってコンテナーにメタデータを追加します。
配列形式と辞書形式のいずれかにより指定します。

{% comment %}
It's recommended that you use reverse-DNS notation to prevent your labels from
conflicting with those used by other software.
{% endcomment %}
ここでは逆 DNS 記法とすることをお勧めします。
この記法にしておけば、他のソフトウェアが用いるラベルとの競合が避けられるからです。

```yaml
labels:
  com.example.description: "Database volume"
  com.example.department: "IT/Ops"
  com.example.label-with-empty-value: ""
```

```yaml
labels:
  - "com.example.description=Database volume"
  - "com.example.department=IT/Ops"
  - "com.example.label-with-empty-value"
```

### name

{% comment %}
> [Added in version 3.4 file format](compose-versioning.md#version-34)
{% endcomment %}
> [ファイルフォーマットバージョン 3.4](compose-versioning.md#version-34) において追加。

{% comment %}
Set a custom name for this volume. The name field can be used to reference
volumes that contain special characters. The name is used as is
and will **not** be scoped with the stack name.
{% endcomment %}
ボリュームに対して独自の名前を設定します。
name は、特殊文字を含むボリュームを参照する際に用いることができます。
name は記述されたとおりに用いられ、スタック名によるスコープは**行われません**。

```yaml
version: "{{ site.compose_file_v3 }}"
volumes:
  data:
    name: my-app-data
```

{% comment %}
It can also be used in conjunction with the `external` property:
{% endcomment %}
これは `external` プロパティと同時に利用することができます。

```yaml
version: "{{ site.compose_file_v3 }}"
volumes:
  data:
    external: true
    name: my-app-data
```

{% comment %}
## Network configuration reference
{% endcomment %}
{: id="network-configuration-reference" }
## ネットワーク設定リファレンス

{% comment %}
The top-level `networks` key lets you specify networks to be created.
{% endcomment %}
最上位の `networks` キーは、生成するネットワークを指定します。

{% comment %}
* For a full explanation of Compose's use of Docker networking features and all
network driver options, see the [Networking guide](../networking.md).
{% endcomment %}
* Compose が利用する Docker ネットワーク機能やネットワークドライバーのオプションに関して、詳細は [ネットワークガイド](../networking.md) を参照してください。

{% comment %}
* For [Docker Labs](https://github.com/docker/labs/blob/master/README.md)
tutorials on networking, start with [Designing Scalable, Portable Docker
Container
Networks](https://github.com/docker/labs/blob/master/networking/README.md)
{% endcomment %}
* [Docker Labs](https://github.com/docker/labs/blob/master/README.md) にあるネットワークのチュートリアルとして、[Designing Scalable, Portable Docker Container Networks](https://github.com/docker/labs/blob/master/networking/README.md) を試してみてください。

### driver

{% comment %}
Specify which driver should be used for this network.
{% endcomment %}
現在のネットワークにおいて利用するドライバーを設定します。

{% comment %}
The default driver depends on how the Docker Engine you're using is configured,
but in most instances it is `bridge` on a single host and `overlay` on a
Swarm.
{% endcomment %}
デフォルトとなるドライバーは、Docker Engine においてどのドライバーを用いているかによって変わります。
たいていの場合、単一ホストであれば `bridge`、スウォーム上では `overlay` となります。

{% comment %}
The Docker Engine returns an error if the driver is not available.
{% endcomment %}
ドライバーが利用できない場合、Docker Engine はエラーを返します。

```yaml
driver: overlay
```

#### bridge

{% comment %}
Docker defaults to using a `bridge` network on a single host. For examples of
how to work with bridge networks, see the Docker Labs tutorial on [Bridge
networking](https://github.com/docker/labs/blob/master/networking/A2-bridge-networking.md).
{% endcomment %}
単一ホストの場合、Docker はデフォルトとして `bridge` ネットワークを利用します。
bridge ネットワークがどのように動作するかは、Docker Labs のチュートリアルである [ブリッジネットワーク](https://github.com/docker/labs/blob/master/networking/A2-bridge-networking.md) の例を参照してください。

#### overlay

{% comment %}
The `overlay` driver creates a named network across multiple nodes in a
[swarm](/engine/swarm/).
{% endcomment %}
`overlay` ドライバーは、[スウォーム](/engine/swarm/) 内での複数ノードにわたって、名前づけされたネットワークを生成します。

{% comment %}
* For a working example of how to build and use an
`overlay` network with a service in swarm mode, see the Docker Labs tutorial on
[Overlay networking and service
discovery](https://github.com/docker/labs/blob/master/networking/A3-overlay-networking.md).
{% endcomment %}
* スウォームモードにて `overlay` ネットワークによるサービスを構築し利用する例として、Docker Labs のチュートリアル [Overlay networking and service discovery](https://github.com/docker/labs/blob/master/networking/A3-overlay-networking.md) を参照してください。

{% comment %}
* For an in-depth look at how it works under the hood, see the
networking concepts lab on the [Overlay Driver Network
Architecture](https://github.com/docker/labs/blob/master/networking/concepts/06-overlay-networks.md).
{% endcomment %}
* さらに詳しく内部動作を知るためには、networking concepts lab にある [Overlay Driver Network
Architecture](https://github.com/docker/labs/blob/master/networking/concepts/06-overlay-networks.md) を参照してください。

{% comment %}
#### host or none
{% endcomment %}
{: id="host-or-none" }
#### host または none

{% comment %}
Use the host's networking stack, or no networking. Equivalent to
`docker run --net=host` or `docker run --net=none`. Only used if you use
`docker stack` commands. If you use the `docker-compose` command,
use [network_mode](#network_mode) instead.
{% endcomment %}
ホストのネットワークスタックを利用する場合、あるいはネットワークを利用しない場合に指定します。
`docker run --net=host` あるいは `docker run --net=none` を実行することと同じです。
`docker stack` コマンドを用いる場合にのみ利用します。
`docker-compose` コマンドを用いる場合は、これではなく [network_mode](#network_mode) を利用してください。

{% comment %}
If you want to use a particular network on a common build, use [network] as
mentioned in the second yaml file example.
{% endcomment %}
通常のビルドではあるものの、ネットワークには特定のものを利用したい場合は、2 つめの yaml ファイル例に示しているように [network](#network) を利用してください。

{% comment %}
The syntax for using built-in networks such as `host` and `none` is a little
different. Define an external network with the name `host` or `none` (that
Docker has already created automatically) and an alias that Compose can use
(`hostnet` or `nonet` in the following examples), then grant the service access to that
network using the alias.
{% endcomment %}
`host` や `none` のようにビルトインネットワークを利用する場合の文法は、少々違います。
外部ネットワークを定義する場合は `host` や `none` （Docker ではすでに自動的に生成済）を用い、また Compose が利用可能なエイリアス（以下の例における `hostnet` や `nonet`）を定義してください。
そしてサービスが、そのエイリアスを使ってネットワークにアクセスできるようにしてください。

```yaml
version: "{{ site.compose_file_v3 }}"
services:
  web:
    networks:
      hostnet: {}

networks:
  hostnet:
    external: true
    name: host
```

```yaml
services:
  web:
    ...
    build:
      ...
      network: host
      context: .
      ...
```

```yaml
services:
  web:
    ...
    networks:
      nonet: {}

networks:
  nonet:
    external: true
    name: none
```

### driver_opts

{% comment %}
Specify a list of options as key-value pairs to pass to the driver for this
network. Those options are driver-dependent - consult the driver's
documentation for more information. Optional.
{% endcomment %}
このネットワークが利用するドライバーに対して、受け渡したいオプションをキーバリューペアのリストとして設定します。
このオプションは各ドライバーによって異なります。
詳しくは各ドライバーのドキュメントを参照してください。
設定は任意です。

```yaml
driver_opts:
  foo: "bar"
  baz: 1
```

### attachable

{% comment %}
> **Note**: Only supported for v3.2 and higher.
{% endcomment %}
> **メモ**: v3.2 またはそれ以上においてのみサポートされています。

{% comment %}
Only used when the `driver` is set to `overlay`. If set to `true`, then
standalone containers can attach to this network, in addition to services. If a
standalone container attaches to an overlay network, it can communicate with
services and standalone containers that are also attached to the overlay
network from other Docker daemons.
{% endcomment %}
`driver` が `overlay` に設定されている場合にのみ用います。
これが `true` に設定されている場合、スタンドアロンのコンテナーがネットワークに、そしてサービスにアタッチされます。
スタンドアロンのコンテナーが overlay ネットワークにアタッチしていると、サービス間での通信が可能になります。
さらに他の Docker デーモンにより overlay ネットワークが構成されている場合に、そこにアタッチされたスタンドアロンコンテナーとも通信が可能になります。

```yaml
networks:
  mynet1:
    driver: overlay
    attachable: true
```

### enable_ipv6

{% comment %}
Enable IPv6 networking on this network.
{% endcomment %}
現在のネットワークにおいて IPv6 ネットワークを有効にします。

{% comment %}
> Not supported in Compose File version 3
>
> `enable_ipv6` requires you to use a version 2 Compose file, as this directive
> is not yet supported in Swarm mode.
{: .warning }
{% endcomment %}
> Compose ファイルバージョン 3 ではサポートされません。
>
> `enable_ipv6` は Compose ファイルバージョン 2 を必要とします。
> したがってこのディレクティブはまだ、スウォームモードに対してはサポートされていません。
{: .warning }

### ipam

{% comment %}
Specify custom IPAM config. This is an object with several properties, each of
which is optional:
{% endcomment %}
独自の IPAM 設定を行います。
いくつかのプロパティにより表わされるオブジェクトであり、それぞれの指定は任意です。

{% comment %}
-   `driver`: Custom IPAM driver, instead of the default.
-   `config`: A list with zero or more config blocks, each containing any of
    the following keys:
    - `subnet`: Subnet in CIDR format that represents a network segment
{% endcomment %}
-   `driver`: デフォルトではない独自の IPAM ドライバーを指定します。
-   `config`: 設定ブロックを指定します。要素数はゼロでも複数でも可です。
    以下のキーを用いることができます。
    - `subnet`: ネットワークセグメントを表わす CIDR 形式のサブネットを指定します。

{% comment %}
A full example:
{% endcomment %}
すべてを利用した例が以下です。

```yaml
ipam:
  driver: default
  config:
    - subnet: 172.28.0.0/16
```

{% comment %}
> **Note**: Additional IPAM configurations, such as `gateway`, are only honored for version 2 at the moment.
{% endcomment %}
> **メモ**: `gateway` のような設定キーは、, 現時点ではバージョン 2 においてのみ利用できます。

### internal

{% comment %}
By default, Docker also connects a bridge network to it to provide external
connectivity. If you want to create an externally isolated overlay network,
you can set this option to `true`.
{% endcomment %}
デフォルトにおいて Docker はブリッジネットワークに接続する際に、外部接続機能も提供します。
外部に独立した overlay ネットワークを生成したい場合、本オプションを `true` にします。

### labels

{% comment %}
Add metadata to containers using
[Docker labels](/engine/userguide/labels-custom-metadata.md). You can use either
an array or a dictionary.
{% endcomment %}
[Docker labels](/engine/userguide/labels-custom-metadata.md) を使ってコンテナーにメタデータを追加します。
配列形式と辞書形式のいずれかにより指定します。

{% comment %}
It's recommended that you use reverse-DNS notation to prevent your labels from
conflicting with those used by other software.
{% endcomment %}
ここでは逆 DNS 記法とすることをお勧めします。
この記法にしておけば、他のソフトウェアが用いるラベルとの競合が避けられるからです。

```yaml
labels:
  com.example.description: "Financial transaction network"
  com.example.department: "Finance"
  com.example.label-with-empty-value: ""
```

```yaml
labels:
  - "com.example.description=Financial transaction network"
  - "com.example.department=Finance"
  - "com.example.label-with-empty-value"
```

### external

{% comment %}
If set to `true`, specifies that this network has been created outside of
Compose. `docker-compose up` does not attempt to create it, and raises
an error if it doesn't exist.
{% endcomment %}
このオプションを `true` に設定することにより、Compose の外部において生成されているボリュームを設定します。
`docker-compose up` はボリュームを生成しないようになりますが、ボリュームが存在しなければエラーとなります。

{% comment %}
For version 3.3 and below of the format, `external` cannot be used in
conjunction with other network configuration keys (`driver`, `driver_opts`,
`ipam`, `internal`). This limitation no longer exists for
[version 3.4](compose-versioning.md#version-34) and above.
{% endcomment %}
バージョン 3.3 およびそれ以前にて、`external` は他のボリューム設定キー（`driver`, `driver_opts`, `ipam`, `internal`）と同時に用いることはできませんでした。
この制約は [バージョン 3.4](compose-versioning.md#version-34) 以降においてはなくなりました。

{% comment %}
In the example below, `proxy` is the gateway to the outside world. Instead of
attempting to create a network called `[projectname]_outside`, Compose
looks for an existing network simply called `outside` and connect the `proxy`
service's containers to it.
{% endcomment %}
以下の例において `proxy` は外部ネットワークとの間のゲートウェイです。
`[projectname]_outside` というネットワークは生成されることはなく、Compose はすでに存在している `outside` という単純な名前のネットワークを探しにいって、`proxy` サービスのコンテナーに接続します。

```yaml
version: "{{ site.compose_file_v3 }}"

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
```

{% comment %}
> [external.name was deprecated in version 3.5 file format](compose-versioning.md#version-35)
> use `name` instead.
{% endcomment %}
> [ファイルフォーマットバージョン 3.5 において external.name は廃止予定となりました](compose-versioning.md#version-35)。
> 代わりに `name` を用いてください。

{% comment %}
You can also specify the name of the network separately from the name used to
refer to it within the Compose file:
{% endcomment %}
ネットワーク名として指定する名前は、Compose ファイル内で参照されている名前以外でも指定することができます。

```yaml
version: "{{ site.compose_file_v3 }}"
networks:
  outside:
    external:
      name: actual-name-of-network
```

### name

{% comment %}
> [Added in version 3.5 file format](compose-versioning.md#version-35)
{% endcomment %}
> [Added in version 3.5 file format](compose-versioning.md#version-35)

{% comment %}
Set a custom name for this network. The name field can be used to reference
networks which contain special characters. The name is used as is
and will **not** be scoped with the stack name.
{% endcomment %}
ネットワークに対して独自の名前を設定します。
name は、特殊文字を含むネットワークを参照する際に用いることができます。
name は記述されたとおりに用いられ、スタック名によるスコープは**行われません**。

```yaml
version: "{{ site.compose_file_v3 }}"
networks:
  network1:
    name: my-app-net
```

{% comment %}
It can also be used in conjunction with the `external` property:
{% endcomment %}
これは `external` プロパティと同時に利用することができます。

```yaml
version: "{{ site.compose_file_v3 }}"
networks:
  network1:
    external: true
    name: my-app-net
```

{% comment %}
## configs configuration reference
{% endcomment %}
{: id="configs-configuration-reference" }
## configs 設定リファレンス

{% comment %}
The top-level `configs` declaration defines or references
[configs](/engine/swarm/configs.md) that can be granted to the services in this
stack. The source of the config is either `file` or `external`.
{% endcomment %}
最上位の `configs` の宣言では、このスタックファイル内のサービスに対して適用する [configs](/engine/swarm/configs.md) を定義し参照します。
config の値となるのは `file` か `external` です。

{% comment %}
- `file`: The config is created with the contents of the file at the specified
  path.
- `external`: If set to true, specifies that this config has already been
  created. Docker does not attempt to create it, and if it does not exist, a
  `config not found` error occurs.
- `name`: The name of the config object in Docker. This field can be used to
   reference configs that contain special characters. The name is used as is
   and will **not** be scoped with the stack name. Introduced in version 3.5
   file format.
{% endcomment %}
- `file`: config は、指定されたパスにあるファイルの内容に従って生成されます。
- `external`: true に設定されている場合、config がすでに定義済であることを設定します。
  Dockder はこれを生成しないようになりますが、config が存在しなければ `config not found` というエラーが発生します。
- `name`: Docker における config オブジェクト名を設定します。
  この設定は、特殊文字を含む config を参照する際に用いることができます。
  name はそのまま用いられ、スタック名によるスコープは**行われません**。
  これはファイルフォーマットバージョン 3.5 において導入されたものです。

{% comment %}
In this example, `my_first_config` is created (as
`<stack_name>_my_first_config)`when the stack is deployed,
and `my_second_config` already exists in Docker.
{% endcomment %}
以下の例においては、スタックがデプロイされる際に（`<stack_name>_my_first_config` として）`my_first_config` が生成されます。
また `my_second_config` は Docker にすでに定義済のものです。

```yaml
configs:
  my_first_config:
    file: ./config_data
  my_second_config:
    external: true
```

{% comment %}
Another variant for external configs is when the name of the config in Docker
is different from the name that exists within the service. The following
example modifies the previous one to use the external config called
`redis_config`.
{% endcomment %}
別の状況として、外部にある config を参照する際に、Docker における config 名と、サービス内にある config 名が異なる場合があります。
以下は、前の例における config を、外部に定義されている `redis_config` というものに変更した例です。

```yaml
configs:
  my_first_config:
    file: ./config_data
  my_second_config:
    external:
      name: redis_config
```

{% comment %}
You still need to [grant access to the config](#configs) to each service in the
stack.
{% endcomment %}
スタック内の各サービスに対しては、[config へのアクセス許可](#configs) を行う必要があります。



{% comment %}
## secrets configuration reference
{% endcomment %}
{: id="secrets-configuration-reference" }
## secrets 設定リファレンス

{% comment %}
The top-level `secrets` declaration defines or references
[secrets](/engine/swarm/secrets.md) that can be granted to the services in this
stack. The source of the secret is either `file` or `external`.
{% endcomment %}
最上位の `secrets` の宣言では、このスタックファイル内のサービスに対して適用する [secrets](/engine/swarm/secrets.md) を定義し参照します。
secret の値となるのは `file` か `external` です。

{% comment %}
- `file`: The secret is created with the contents of the file at the specified
  path.
- `external`: If set to true, specifies that this secret has already been
  created. Docker does not attempt to create it, and if it does not exist, a
  `secret not found` error occurs.
- `name`: The name of the secret object in Docker. This field can be used to
   reference secrets that contain special characters. The name is used as is
   and will **not** be scoped with the stack name. Introduced in version 3.5
   file format.
{% endcomment %}
- `file`: secret は、指定されたパスにあるファイルの内容に従って生成されます。
- `external`: true に設定されている場合、secret がすでに定義済であることを設定します。
  Dockder はこれを生成しないようになりますが、secret が存在しなければ `secret not found` というエラーが発生します。
- `name`: Docker における secret オブジェクト名を設定します。
  この設定は、特殊文字を含む secret を参照する際に用いることができます。
  name はそのまま用いられ、スタック名によるスコープは**行われません**。
  これはファイルフォーマットバージョン 3.5 において導入されたものです。

{% comment %}
In this example, `my_first_secret` is created as
`<stack_name>_my_first_secret `when the stack is deployed,
and `my_second_secret` already exists in Docker.
{% endcomment %}
以下の例においては、スタックがデプロイされる際に（`<stack_name>_my_first_secret` として）`my_first_secret` が生成されます。
また `my_second_secret` は Docker にすでに定義済のものです。

```yaml
secrets:
  my_first_secret:
    file: ./secret_data
  my_second_secret:
    external: true
```

{% comment %}
Another variant for external secrets is when the name of the secret in Docker
is different from the name that exists within the service. The following
example modifies the previous one to use the external secret called
`redis_secret`.
{% endcomment %}
別の状況として、外部にある secret を参照する際に、Docker における secret 名と、サービス内にある secret 名が異なる場合があります。
以下は、前の例における secret を、外部に定義されている `redis_secret` というものに変更した例です。

{% comment %}
### Compose File v3.5 and above
{% endcomment %}
{: id="compose-file-v35-and-above" }
### Compose ファイル v3.5 またはそれ以降の場合

```yaml
secrets:
  my_first_secret:
    file: ./secret_data
  my_second_secret:
    external: true
    name: redis_secret
```

{% comment %}
### Compose ファイル v3.4 またそれ以前の場合
{% endcomment %}
{: id="compose-file-v34-and-under" }
### Compose File v3.4 and under

```yaml
  my_second_secret:
    external:
      name: redis_secret
```

{% comment %}
You still need to [grant access to the secrets](#secrets) to each service in the
stack.
{% endcomment %}
スタック内の各サービスに対しては、[secret へのアクセス許可](#secrets) を行う必要があります。

{% comment %}
## Variable substitution
{% endcomment %}
{: id="variable-substitution" }
## 変数の置換

{% include content/compose-var-sub.md %}

{% comment %}
## Extension fields
{% endcomment %}
{: id="extension-fields" }
## 拡張項目

{% comment %}
> [Added in version 3.4 file format](compose-versioning.md#version-34).
{% endcomment %}
> [ファイルフォーマットバージョン 3.4](compose-versioning.md#version-34) において追加されました。

{% include content/compose-extfields-sub.md %}

{% comment %}
## Compose documentation
{% endcomment %}
{: id="compose-documentation" }
## Compose ドキュメント

{% comment %}
- [User guide](/compose/index.md)
- [Installing Compose](/compose/install/)
- [Compose file versions and upgrading](compose-versioning.md)
- [Get started with Docker](/get-started/)
- [Samples](/samples/)
- [Command line reference](/compose/reference/)
{% endcomment %}
- [ユーザーガイド](/compose/index.md)
- [Compose のインストール](/compose/install/)
- [Compose ファイルのバージョンとアップグレード](compose-versioning.md)
- [Docker をはじめよう](/get-started/)
- [サンプル](/samples/)
- [コマンドラインリファレンス](/compose/reference/)
