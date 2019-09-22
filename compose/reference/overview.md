---
description: Overview of docker-compose CLI
keywords: fig, composition, compose, docker, orchestration, cli,  docker-compose
redirect_from:
- /compose/reference/docker-compose/
title: docker-compose コマンド概要
---

{% comment %}
This page provides the usage information for the `docker-compose` Command.
{% endcomment %}
このページは `docker-compose` コマンドの利用方法について示します。

{% comment %}
## Command options overview and help
{% endcomment %}
## コマンドオプション概要とヘルプ
{: #command-options-overview-and-help }

{% comment %}
You can also see this information by running `docker-compose --help` from the
command line.
{% endcomment %}
以下は、コマンドラインから `docker-compose --help` を実行した結果です。

```none
Docker で使う複数コンテナーアプリケーションを定義し実行する。

利用方法:
  docker-compose [-f <引数>...] [オプション] [コマンド] [引数...]
  docker-compose -h|--help

オプション:
  -f, --file FILE             Compose ファイルを指定
                              (デフォルト: docker-compose.yml)
  -p, --project-name NAME     プロジェクト名を指定
                              (デフォルト: ディレクトリ名)
  --verbose                   詳細情報を表示
  --log-level LEVEL           ログレベルを設定 (DEBUG, INFO, WARNING, ERROR, CRITICAL)
  --no-ansi                   ANSI コントロール文字を表示しない
  -v, --version               バージョンを表示して終了
  -H, --host HOST             接続先のデーモンソケット

  --tls                       TLS を利用; --tlsverify が暗に利用する
  --tlscacert CA_PATH         この CA で署名した証明書のみ信頼
  --tlscert CLIENT_CERT_PATH  TLS 証明書ファイルへのパスを指定
  --tlskey TLS_KEY_PATH       TLS 鍵ファイルのパスを指定
  --tlsverify                 TLS を利用しリモートを検証
  --skip-hostname-check       Don't check the daemon's hostname against the
                              name specified in the client certificate
  --project-directory PATH    ワーキングディレクトリを指定する
                              (デフォルト: Compose ファイルがあるディレクトリ)
  --compatibility             If set, Compose will attempt to convert deploy
                              keys in v3 files to their non-Swarm equivalent

コマンド:
  build              サービスの構築または再構築
  bundle             Compose ファイルから Docker bundle を生成
  config             Compose ファイルの検証と表示
  create             サービスの生成
  down               コンテナー、ネットワーク、イメージ、ボリュームの停止と削除
  events             コンテナーからのリアルタイムイベントの受信
  exec               起動中コンテナーでのコマンド実行
  help               コマンドのヘルプ表示
  images             イメージ一覧の表示
  kill               コンテナーの強制停止(kill)
  logs               コンテナーからの出力を表示
  pause              サービスの一時停止
  port               ポートフォワーディングのための公開用ポートを表示
  ps                 コンテナーの一覧表示
  pull               サービスイメージをプル
  push               サービスイメージをプッシュ
  restart            サービスの再起動
  rm                 停止中のコンテナーを削除
  run                Run a one-off command
  scale              サービスに割り当てるコンテナー数を設定
  start              サービスの起動
  stop               サービスの停止
  top                実行中プロセスの表示
  unpause            サービスの再開
  up                 コンテナーの生成と起動
  version            Docker Compose のバージョン情報の表示
```

{% comment %}
You can use Docker Compose binary, `docker-compose [-f <arg>...] [options]
[COMMAND] [ARGS...]`, to build and manage multiple services in Docker containers.
{% endcomment %}
Docker Compose の実行バイナリが `docker-compose [-f <引数>...] [オプション][コマンド] [引数...]` です。
これを使って Docker コンテナー内の複数サービスを生成、管理することができます。

{% comment %}
## Use `-f` to specify name and path of one or more Compose files
{% endcomment %}
## `-f` 利用による（複数）Compose ファイルパスの指定
{: #use--f-to-specify-name-and-path-of-one-or-more-compose-files }

{% comment %}
Use the `-f` flag to specify the location of a Compose configuration file.
{% endcomment %}
`-f` フラグを利用して Compose 設定ファイルの場所を指定します。

{% comment %}
### Specifying multiple Compose files
{% endcomment %}
### 複数 Compose ファイルの指定
{: #specifying-multiple-compose-files }

{% comment %}
You can supply multiple `-f` configuration files. When you supply multiple
files, Compose combines them into a single configuration. Compose builds the
configuration in the order you supply the files. Subsequent files override and
add to their predecessors.
{% endcomment %}
`-f` フラグを複数用いることで複数の設定ファイルを指定することができます。
複数ファイルを指定した場合、Compose はそれを 1 つの設定にまとめます。
まとめる際には、指定されたファイル順に行われます。
後から指定されたファイルは、前の設定に追加または上書きされます。

{% comment %}
For example, consider this command line:
{% endcomment %}
たとえば以下のようにコマンドラインから入力したとします。

```
$ docker-compose -f docker-compose.yml -f docker-compose.admin.yml run backup_db
```

{% comment %}
The `docker-compose.yml` file might specify a `webapp` service.
{% endcomment %}
`docker-compose.yml` ファイルは `webapp` サービスを定義していたとします。

```
webapp:
  image: examples/web
  ports:
    - "8000:8000"
  volumes:
    - "/data"
```

{% comment %}
If the `docker-compose.admin.yml` also specifies this same service, any matching
fields override the previous file. New values, add to the `webapp` service
configuration.
{% endcomment %}
`docker-compose.admin.yml` が同じサービスを定義していたとすると、合致する設定項目は後から指定されたものによって上書きされます。
新たな設定項目であれば、`webapp` サービスに加えられます。

```
webapp:
  build: .
  environment:
    - DEBUG=1
```

{% comment %}
Use a `-f` with `-` (dash) as the filename to read the configuration from
`stdin`. When `stdin` is used all paths in the configuration are
relative to the current working directory.
{% endcomment %}
`-f` に対してファイル名として `-`（ダッシュ）を指定すると、設定を標準入力から読み込むことになります。
標準入力での設定内容のパスは、すべてカレントワーキングディレクトリからの相対パスを指定します。

{% comment %}
The `-f` flag is optional. If you don't provide this flag on the command line,
Compose traverses the working directory and its parent directories looking for a
`docker-compose.yml` and a `docker-compose.override.yml` file. You must supply
at least the `docker-compose.yml` file. If both files are present on the same
directory level, Compose combines the two files into a single configuration.
{% endcomment %}
`-f` フラグはオプションです。
コマンドラインにこのフラグを指定しなかった場合、

{% comment %}
The configuration in the `docker-compose.override.yml` file is applied over and
in addition to the values in the `docker-compose.yml` file.
{% endcomment %}
`docker-compose.override.yml` ファイルに設定されている内容は
`docker-compose.yml` の内容を上書きするか、つけ加えられて適用されます。

{% comment %}
### Specifying a path to a single Compose file
{% endcomment %}
### 1 つの Compose ファイルのパスを指定
{: #specifying-a-path-to-a-single-compose-file }

{% comment %}
You can use the `-f` flag to specify a path to a Compose file that is not
located in the current directory, either from the command line or by setting up
a [COMPOSE_FILE environment variable](envvars.md#compose_file) in your shell or
in an environment file.
{% endcomment %}
`-f` フラグを利用すると、カレントディレクトリではない場所にある Compose ファイルを指定することができます。
これはコマンドラインから与えますが、シェル内あるいは環境ファイル内から[環境変数 COMPOSE_FILE](envvars.md#compose_file)
をセットすることでも与えることができます。

{% comment %}
For an example of using the `-f` option at the command line, suppose you are
running the [Compose Rails sample](/compose/rails/), and
have a `docker-compose.yml` file in a directory called `sandbox/rails`. You can
use a command like [docker-compose pull](/compose/reference/pull.md) to get the
postgres image for the `db` service from anywhere by using the `-f` flag as
follows: `docker-compose -f ~/sandbox/rails/docker-compose.yml pull db`
{% endcomment %}
コマンドラインから `-f` オプションを利用する例として、[Compose Rails サンプル](/compose/rails/)を利用しているとします。
そして `sandbox/rails` というディレクトリに `docker-compose.yml` があるとします。
[docker-compose pull](/compose/reference/pull.md) のようなコマンドを使って、`db` サービスにおける postgres イメージをどこからでも取得できるようにするには、`-f` フラグを使って以下のようにします。
`docker-compose -f ~/sandbox/rails/docker-compose.yml pull db`

{% comment %}
Here's the full example:
{% endcomment %}
以下に処理例の全体を示します。

```
$ docker-compose -f ~/sandbox/rails/docker-compose.yml pull db
Pulling db (postgres:latest)...
latest: Pulling from library/postgres
ef0380f84d05: Pull complete
50cf91dc1db8: Pull complete
d3add4cd115c: Pull complete
467830d8a616: Pull complete
089b9db7dc57: Pull complete
6fba0a36935c: Pull complete
81ef0e73c953: Pull complete
338a6c4894dc: Pull complete
15853f32f67c: Pull complete
044c83d92898: Pull complete
17301519f133: Pull complete
dcca70822752: Pull complete
cecf11b8ccf3: Pull complete
Digest: sha256:1364924c753d5ff7e2260cd34dc4ba05ebd40ee8193391220be0f9901d4e1651
Status: Downloaded newer image for postgres:latest
```

{% comment %}
## Use `-p` to specify a project name
{% endcomment %}
## `-p` を利用したプロジェクト名の指定
{: #use--p-to-specify-a-project-name }

{% comment %}
Each configuration has a project name. If you supply a `-p` flag, you can
specify a project name. If you don't specify the flag, Compose uses the current
directory name. See also the [COMPOSE_PROJECT_NAME environment variable](
envvars.md#compose_project_name).
{% endcomment %}
個々の設定ファイルにはプロジェクト名があります。
`-p` フラグを用いると、プロジェクト名を指定することができます。
フラグを指定しなかった場合、Compose はカレントディレクトリ名をプロジェクト名とします。
詳細は [環境変数 COMPOSE_PROJECT_NAME](envvars.md#compose_project_name) を参照してください。

{% comment %}
## Set up environment variables
{% endcomment %}
## 環境変数の設定
{: #set-up-environment-variables }

{% comment %}
You can set [environment variables](envvars.md) for various
`docker-compose` options, including the `-f` and `-p` flags.
{% endcomment %}
`-f` や `-p` フラグといった `docker-compose` のさまざまなオプションに対しては、[環境変数](envvars.md)を利用することができます。

{% comment %}
For example, the [COMPOSE_FILE environment variable](envvars.md#compose_file)
relates to the `-f` flag, and [COMPOSE_PROJECT_NAME environment
variable](envvars.md#compose_project_name) relates to the `-p` flag.
{% endcomment %}
たとえば[環境変数 COMPOSE_FILE](envvars.md#compose_file) は `-f` フラグに関連づいています。
また[環境変数 COMPOSE_PROJECT_NAME](envvars.md#compose_project_name) は `-p` フラグに関連づいています。

{% comment %}
Also, you can set some of these variables in an [environment
file](/compose/env-file.md).
{% endcomment %}
また環境変数の中には[環境ファイル](/compose/env-file.md)において設定できるものもあります。

{% comment %}
## Where to go next
{% endcomment %}
## 次は何を読むか
{: #where-to-go-next }

{% comment %}
* [CLI environment variables](envvars.md)
* [Declare default environment variables in file](/compose/env-file.md)
{% endcomment %}
* [CLI 環境変数](envvars.md)
* [環境変数のデフォルトをファイル内に定義する](/compose/env-file.md)
