---
description: Compose CLI environment variables
keywords: fig, composition, compose, docker, orchestration, cli, reference
title: Compose CLI 環境変数
---

{% comment %}
Several environment variables are available for you to configure the Docker Compose command-line behaviour.
{% endcomment %}
Docker Compose コマンドラインの動作を設定するものとして、数種類の環境変数が利用できます。

{% comment %}
Variables starting with `DOCKER_` are the same as those used to configure the
Docker command-line client. If you're using `docker-machine`, then the `eval "$(docker-machine env my-docker-vm)"` command should set them to their correct values. (In this example, `my-docker-vm` is the name of a machine you created.)
{% endcomment %}
`DOCKER_` が先頭につく変数は、Docker コマンドラインクライアントの設定に用いられる環境変数と同じです。
`docker-machine` を利用している場合は、`eval "$(docker-machine env my-docker-vm)"` コマンドを実行することで、各環境変数に適切な値が設定されます。
（この例では `my-docker-vm` が生成されているマシン名です。）

{% comment %}
> **Note**: Some of these variables can also be provided using an
> [environment file](../env-file.md).
{% endcomment %}
> **メモ**： ここに示す環境変数の中には、[環境ファイル](../env-file.md) を用いて設定できるものもあります。

## COMPOSE\_PROJECT\_NAME

{% comment %}
Sets the project name. This value is prepended along with the service name to
the container on start up. For example, if your project name is `myapp` and it
includes two services `db` and `web`, then Compose starts containers named
`myapp_db_1` and `myapp_web_1` respectively.
{% endcomment %}
プロジェクト名を設定します。
この値は、コンテナーの起動時にサービス名の先頭につけられます。
たとえばプロジェクト名が `myapp` であり、2 つのサービス `db` と `web` があるとします。
Compose がコンテナーを起動したときにつける名前は、それぞれ `myapp_db_1`、`myapp_web_1` です。

{% comment %}
Setting this is optional. If you do not set this, the `COMPOSE_PROJECT_NAME`
defaults to the `basename` of the project directory. See also the `-p`
[command-line option](overview.md).
{% endcomment %}
この変数を設定するのは任意です。
変数を設定しなかった場合 `COMPOSE_PROJECT_NAME` のデフォルトは、プロジェクトディレクトリの `basename` となります。
[コマンドラインオプション](overview.md) の `-p` も参照してください。

## COMPOSE\_FILE

{% comment %}
Specify the path to a Compose file. If not provided, Compose looks for a file named
`docker-compose.yml` in the current directory and then each parent directory in
succession until a file by that name is found.
{% endcomment %}
Compose ファイルへのパスを指定します。
指定されなかった場合、Compose はカレントディレクトリ内の `docker-compose.yml` というファイルを探します。
そしてファイルが見つからなければ、この名前のファイルを見つけるまで親ディレクトリを順にたどって探します。

{% comment %}
This variable supports multiple Compose files separated by a path separator (on
Linux and macOS the path separator is `:`, on Windows it is `;`). For example:
`COMPOSE_FILE=docker-compose.yml:docker-compose.prod.yml`. The path separator
can also be customized using `COMPOSE_PATH_SEPARATOR`.
{% endcomment %}
この変数は複数の Compose ファイルの指定をサポートしています。
複数のパスはセパレーターで区切ります（パスセパレーターは Linux や macOS では `:`、Windows では `;`）。
たとえば `COMPOSE_FILE=docker-compose.yml:docker-compose.prod.yml` とします。
パスセパレーターは `COMPOSE_PATH_SEPARATOR` を使って変更することもできます。

{% comment %}
See also the `-f` [command-line option](overview.md).
{% endcomment %}
[コマンドラインオプション](overview.md) `-f` も参照してください。

## COMPOSE\_API\_VERSION

{% comment %}
The Docker API only supports requests from clients which report a specific
version. If you receive a `client and server don't have same version` error using
`docker-compose`, you can workaround this error by setting this environment
variable. Set the version value to match the server version.
{% endcomment %}
Docker API は、クライアントが特定のバージョンを返す場合に限って、クライアントからのリクエストに応じます。
`docker-compose` を利用する際に `client and server don't have same version` （クライアントとサーバーのバージョンが一致しません）というエラーが発生した場合は、その回避策として、本環境変数を設定する方法があります。
サーバーのバージョンに合致するようなバージョン値をこの変数に設定することです。

{% comment %}
Setting this variable is intended as a workaround for situations where you need
to run temporarily with a mismatch between the client and server version. For
example, if you can upgrade the client but need to wait to upgrade the server.
{% endcomment %}
クライアントとサーバーのバージョンが一致しないときであっても、実行が必要になる状況があります。
この変数を用いるのは、そういった状況を一時的に解決するためです。
具体的には、クライアントをアップグレードしたものの、サーバーをまだアップグレードしていないような状況です。

{% comment %}
Running with this variable set and a known mismatch does prevent some Docker
features from working properly. The exact features that fail would depend on the
Docker client and server versions. For this reason, running with this variable
set is only intended as a workaround and it is not officially supported.
{% endcomment %}
この変数を設定したとしても、既知のバージョン不一致に該当していれば、Docker の機能が正常に動作しないことがあります。
動作しない機能は、Docker クライアントやサーバーのバージョンによって異なります。
このことから、本変数を設定して実行するのはあくまで一時的な回避策であり、公式にサポートされるものではありません。

{% comment %}
If you run into problems running with this set, resolve the mismatch through
upgrade and remove this setting to see if your problems resolve before notifying
support.
{% endcomment %}
本変数を設定することで問題が発生する場合は、アップグレードを行ってバージョンの不一致を解消してください。
そしてこの変数の定義を行わなかったらどうなるかを確認してください。
それでも問題が解決しない場合はサポートに問い合わせてください。

## DOCKER\_HOST

{% comment %}
Sets the URL of the `docker` daemon. As with the Docker client, defaults to `unix:///var/run/docker.sock`.
{% endcomment %}
`docker` デーモンの URL を設定します。
Docker クライアントと同じように、このデフォルト値は `unix:///var/run/docker.sock` です。

## DOCKER\_TLS\_VERIFY

{% comment %}
When set to anything other than an empty string, enables TLS communication with
the `docker` daemon.
{% endcomment %}
この変数が空文字以外であれば、`docker` デーモンとの TLS 通信を有効にします。

## DOCKER\_CERT\_PATH

{% comment %}
Configures the path to the `ca.pem`, `cert.pem`, and `key.pem` files used for TLS verification. Defaults to `~/.docker`.
{% endcomment %}
TLS 検証に用いられる各種ファイル、`ca.pem`, `cert.pem`, `key.pem` のパスを設定します。
デフォルトは `~/.docker` です。

## COMPOSE\_HTTP\_TIMEOUT

{% comment %}
Configures the time (in seconds) a request to the Docker daemon is allowed to hang before Compose considers
it failed. Defaults to 60 seconds.
{% endcomment %}
Docker デーモンへの処理要求にあたって、Compose の処理は失敗していなくても、デーモンをハングアップさせる所要時間を（秒単位で）指定します。
デフォルトは 60 秒です。

## COMPOSE\_TLS\_VERSION

{% comment %}
Configure which TLS version is used for TLS communication with the `docker`
daemon. Defaults to `TLSv1`.
Supported values are: `TLSv1`, `TLSv1_1`, `TLSv1_2`.
{% endcomment %}
`docker` デーモンとの TLS 通信に用いられる TLS バージョンを指定します。
デフォルトは `TLSv1` です。
対応する値は `TLSv1`, `TLSv1_1`, `TLSv1_2` です。

## COMPOSE\_CONVERT\_WINDOWS\_PATHS

{% comment %}
Enable path conversion from Windows-style to Unix-style in volume definitions.
Users of Docker Machine and Docker Toolbox on Windows should always set this. Defaults to `0`.
Supported values: `true` or `1` to enable, `false` or `0` to disable.
{% endcomment %}
ボリュームの定義において、パスの記述に関して Windows 書式と Unix 書式の変換を有効にします。
Windows 上の Docker Machine と Docker Toolbox を利用するユーザーは、必ず本変数を設定しておきます。
デフォルトは `0` です。
対応する値は `true` または `1` が有効を意味し、`false` または `0` が無効を意味します。

## COMPOSE\_PATH\_SEPARATOR

{% comment %}
If set, the value of the `COMPOSE_FILE` environment variable is separated
using this character as path separator.
{% endcomment %}
これが設定されている場合に、環境変数 `COMPOSE_FILE` の設定値に対して、パスを区切る文字として本変数の設定値を用います。

## COMPOSE\_FORCE\_WINDOWS\_HOST

{% comment %}
If set, volume declarations using the [short syntax](../compose-file/#short-syntax-3)
are parsed assuming the host path is a Windows path, even if Compose is
running on a UNIX-based system.
Supported values: `true` or `1` to enable, `false` or `0` to disable.
{% endcomment %}
これが設定されていて、[短い文法](../compose-file/index.md#short-syntax-3) によってボリュームの宣言を行うと、ホストのパスは Windows パスとして扱われます。
Compose が UNIX ベースのシステム上で動作していても、そのようになります。
対応する値は `true` または `1` が有効を意味し、また `false` または `0` が無効を意味します。

## COMPOSE\_IGNORE\_ORPHANS

{% comment %}
If set, Compose doesn't try to detect orphaned containers for the project.
Supported values: `true` or `1` to enable, `false` or `0` to disable.
{% endcomment %}
これが設定されている場合に、そのプロジェクト内で孤立したコンテナーを Compose が検出しないようにします。
対応する値は `true` または `1` が有効を意味し、`false` または `0` が無効を意味します。

## COMPOSE\_PARALLEL\_LIMIT

{% comment %}
Sets a limit for the number of operations Compose can execute in parallel. The
default value is `64`, and may not be set lower than `2`.
{% endcomment %}
Compose が並列で処理する操作数を設定します。
デフォルト値は `64` です。
`2` よりも小さな値を設定することはできません。

## COMPOSE\_INTERACTIVE\_NO\_CLI

{% comment %}
If set, Compose doesn't attempt to use the Docker CLI for interactive `run`
and `exec` operations. This option is not available on Windows where the CLI
is required for the aforementioned operations.
Supported: `true` or `1` to enable, `false` or `0` to disable.
{% endcomment %}
これが設定されている場合に、Docker CLI の `run` や `exec` の処理において Compose は対話的処理を行わないようにします。
前もって操作を定めておく必要がある Windows の CLI では、このオプションを利用することはできません。
対応する値は `true` または `1` が有効を意味し、`false` または `0` が無効を意味します。

{% comment %}
## Related information
{% endcomment %}
{: #related-information }
## 関連情報

{% comment %}
- [User guide](../index.md)
- [Installing Compose](../install.md)
- [Compose file reference](../compose-file/index.md)
- [Environment file](../env-file.md)
{% endcomment %}
- [ユーザーガイド](../index.md)
- [Compose のインストール](../install.md)
- [Compose ファイルリファレンス](../compose-file/index.md)
- [環境ファイル](../env-file.md)
