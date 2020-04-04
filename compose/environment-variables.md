---
title: Compose における環境変数
description: Compose において環境変数を設定、利用、管理する方法について説明。
keywords: compose, orchestration, environment, env file
---

{% comment %}
There are multiple parts of Compose that deal with environment variables in one
sense or another. This page should help you find the information you need.
{% endcomment %}
Compose の複数の場面において環境変数がさまざまに用いられています。
このページでは環境変数について必要となる情報を示します。


{% comment %}
## Substitute environment variables in Compose files
{% endcomment %}
## Compose ファイル内での環境変数の利用
{: #substitute-environment-variables-in-compose-files }

{% comment %}
It's possible to use environment variables in your shell to populate values
inside a Compose file:
{% endcomment %}
シェル内にて環境変数を設定し、その値を Compose ファイルにおいて読み込ませることができます。

```yaml
web:
  image: "webapp:${TAG}"
```

{% comment %}
For more information, see the
[Variable substitution](/compose/compose-file/index.md#variable-substitution) section in the
Compose file reference.
{% endcomment %}
詳しくは Compose ファイルリファレンスの[変数の置換](/compose/compose-file/index.md#variable-substitution) の項を参照してください。


{% comment %}
## Set environment variables in containers
{% endcomment %}
## コンテナー内での環境変数の設定
{: #set-environment-variables-in-containers }

{% comment %}
You can set environment variables in a service's containers with the
['environment' key](/compose/compose-file/index.md#environment), just like with
`docker run -e VARIABLE=VALUE ...`:
{% endcomment %}
サービスコンテナーにおいて、たとえば`docker run -e VARIABLE=VALUE ...`のように[`environment`キー](/compose/compose-file/index.md#environment) を使って、環境変数を設定することができます。

```yaml
web:
  environment:
    - DEBUG=1
```

{% comment %}
## Pass environment variables to containers
{% endcomment %}
## コンテナーへの環境変数の受け渡し
{: #pass-environment-variables-to-containers }

{% comment %}
You can pass environment variables from your shell straight through to a
service's containers with the ['environment' key](/compose/compose-file/index.md#environment)
by not giving them a value, just like with `docker run -e VARIABLE ...`:
{% endcomment %}
シェル内の環境変数を[`environment`キー](/compose/compose-file/index.md#environment) を使って、直接サービスコンテナーに受け渡すことができます。この場合には値を渡すのではなく`docker run -e 変数名 ...`のようにできます。

```yaml
web:
  environment:
    - DEBUG
```

{% comment %}
The value of the `DEBUG` variable in the container is taken from the value for
the same variable in the shell in which Compose is run.
{% endcomment %}
コンテナー内の`DEBUG`変数は、シェル内の`DEBUG`変数の値が用いられます。
このシェルとは Compose が起動しているシェルのことです。

{% comment %}
## The “env_file” configuration option
{% endcomment %}
## 設定オプション``env_file``
{: #the-env_file-configuration-option }

{% comment %}
You can pass multiple environment variables from an external file through to
a service's containers with the ['env_file' option](/compose/compose-file/index.md#env_file),
just like with `docker run --env-file=FILE ...`:
{% endcomment %}
外部ファイルから複数の環境変数をサービスコンテナーに受け渡すには [``env_file``オプション](/compose/compose-file/index.md#env_file) を利用することができます。
`docker run --env-file=FILE ...`のようにすることもできます。

```yaml
web:
  env_file:
    - web-variables.env
```

{% comment %}
## Set environment variables with 'docker-compose run'
{% endcomment %}
## ``docker-compose run``実行時の環境変数の設定
{: #set-environment-variables-with-docker-compose-run }

{% comment %}
Just like with `docker run -e`, you can set environment variables on a one-off
container with `docker-compose run -e`:
{% endcomment %}
`docker run -e`と同じように、`docker-compose run -e`の実行によるコンテナーに対しても環境変数を実行することができます。

```bash
docker-compose run -e DEBUG=1 web python console.py
```

{% comment %}
You can also pass a variable through from the shell by not giving it a value:
{% endcomment %}
シェル変数を受け渡す際には、値は直接受け渡さずに以下のようにできます。

```bash
docker-compose run -e DEBUG web python console.py
```

{% comment %}
The value of the `DEBUG` variable in the container is taken from the value for
the same variable in the shell in which Compose is run.
{% endcomment %}
コンテナー内の`DEBUG`変数は、シェル内の`DEBUG`変数の値が用いられます。
このシェルとは Compose が起動しているシェルのことです。


{% comment %}
## The “.env” file
{% endcomment %}
## ``.env``ファイル
{: #the-env-file }

{% comment %}
You can set default values for any environment variables referenced in the
Compose file, or used to configure Compose, in an [environment file](env-file.md)
named `.env`:
{% endcomment %}
Compose ファイルが参照する環境変数、あるいは Compose の設定に用いられる環境変数のデフォルト値を設定することができます。これは`.env`という[環境ファイル](env-file.md)にて行います。

```bash
$ cat .env
TAG=v1.5

$ cat docker-compose.yml
version: '3'
services:
  web:
    image: "webapp:${TAG}"
```

{% comment %}
When you run `docker-compose up`, the `web` service defined above uses the
image `webapp:v1.5`. You can verify this with the
[config command](reference/config.md), which prints your resolved application
config to the terminal:
{% endcomment %}
`docker-compose up`を実行すると、上で定義されている`web`サービスは`webapp:v1.5`というイメージを利用します。
このことは [config コマンド](reference/config.md)を使って確認できます。
このコマンドは変数を置換した後のアプリケーション設定を端末画面に出力します。

```bash
$ docker-compose config

version: '3'
services:
  web:
    image: 'webapp:v1.5'
```

{% comment %}
Values in the shell take precedence over those specified in the `.env` file.
If you set `TAG` to a different value in your shell, the substitution in `image`
uses that instead:
{% endcomment %}
シェル内にて設定される値は、`.env`ファイル内のものよりも優先されます。
たとえばシェル上において`TAG`を異なる値に設定していたら、それを使って変数置換された`image`が用いられることになります。

```bash
$ export TAG=v2.0
$ docker-compose config

version: '3'
services:
  web:
    image: 'webapp:v2.0'
```

{% comment %}
When you set the same environment variable in multiple files, here's the
priority used by Compose to choose which value to use:
{% endcomment %}
環境変数を複数のファイルなどに設定していた場合に、Compose が値を採用する優先順位は以下のとおりです。

{% comment %}
1. Compose file
2. Shell environment variables
3. Environment file
4. Dockerfile
5. Variable is not defined
{% endcomment %}
1. Compose ファイル
2. シェル内の環境変数
3. 環境ファイル
4. Dockerfile
5. 未定義の変数

{% comment %}
In the example below, we set the same environment variable on an Environment
file, and the Compose file:
{% endcomment %}
以下の例では同一の環境変数を、環境ファイルと Compose ファイルに設定しています。

```bash
$ cat ./Docker/api/api.env
NODE_ENV=test

$ cat docker-compose.yml
version: '3'
services:
  api:
    image: 'node:6-alpine'
    env_file:
     - ./Docker/api/api.env
    environment:
     - NODE_ENV=production
```

{% comment %}
When you run the container, the environment variable defined in the Compose
file takes precedence.
{% endcomment %}
コンテナーを実行すると Compose ファイルに定義された環境変数が優先されます。

```bash
$ docker-compose exec api node

> process.env.NODE_ENV
'production'
```

{% comment %}
Having any `ARG` or `ENV` setting in a `Dockerfile` evaluates only if there is
no Docker Compose entry for `environment` or `env_file`.
{% endcomment %}
`Dockerfile`ファイル内の`ARG`や`ENV`は、`environment`や`env_file`による Docker Composeの設定がある場合は評価されません。

{% comment %}
> Specifics for NodeJS containers
>
> If you have a `package.json` entry for `script:start` like
> `NODE_ENV=test node server.js`, then this overrules any setting in your
> `docker-compose.yml` file.
{% endcomment %}
> NodeJS コンテナーの仕様
>
> `script:start`に対して`package.json`のエントリーを含む場合、たとえば`NODE_ENV=test node server.js`のような場合には、`docker-compose.yml`ファイルでの設定よりもこちらの設定が優先されます。

{% comment %}
## Configure Compose using environment variables
{% endcomment %}
## 環境変数を用いた Compose の設定
{: #configure-compose-using-environment-variables }

{% comment %}
Several environment variables are available for you to configure the Docker
Compose command-line behavior. They begin with `COMPOSE_` or `DOCKER_`, and are
documented in [CLI Environment Variables](reference/envvars.md).
{% endcomment %}
Docker Compose のコマンドラインからの処理設定を行うことができる環境変数がいくつかあります。
そういった変数は先頭が`COMPOSE_`や`DOCKER_`で始まります。
詳しくは [CLI 環境変数](reference/envvars.md)を参照してください。

{% comment %}
## Environment variables created by links
{% endcomment %}
## リンクから生成される環境変数
{: #environment-variables-created-by-links }

{% comment %}
When using the ['links' option](/compose/compose-file/index.md#links) in a
[v1 Compose file](/compose/compose-file/index.md#version-1), environment variables are created
for each link. They are documented in
the [Link environment variables reference](link-env-deprecated.md).
{% endcomment %}
[Compose ファイルバージョン 1](/compose/compose-file/index.md#version-1) における[`links`オプション](/compose/compose-file/index.md#links) を用いると、各リンクに対する環境変数が生成されます。
このことは [リンク環境変数リファレンス](link-env-deprecated.md) において説明しています。

{% comment %}
However, these variables are deprecated. Use the link alias as a hostname instead.
{% endcomment %}
ただしこの変数は廃止予定となっています。
リンクはホスト名として利用するようにしてください。
