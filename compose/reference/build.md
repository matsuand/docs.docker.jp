---
description: サービスのビルドまたは再ビルド。
keywords: fig, composition, compose, docker, orchestration, cli, build
title: docker-compose build
notoc: true

---

{% comment %}
```none
Usage: build [options] [--build-arg key=val...] [SERVICE...]

Options:
    --build-arg key=val     Set build-time variables for services.
    --compress              Compress the build context using gzip.
    --force-rm              Always remove intermediate containers.
    -m, --memory MEM        Set memory limit for the build container.
    --no-cache              Do not use cache when building the image.
    --no-rm                 Do not remove intermediate containers after a successful build.
    --parallel              Build images in parallel.
    --progress string       Set type of progress output (`auto`, `plain`, `tty`).
                            `EXPERIMENTAL` flag for native builder.
                            To enable, run with `COMPOSE_DOCKER_CLI_BUILD=1`)
    --pull                  Always attempt to pull a newer version of the image.
    -q, --quiet             Don't print anything to `STDOUT`.
```
{% endcomment %}
```none
利用方法: build [オプション] [--build-arg key=val...] [SERVICE...]

オプション:
    --build-arg key=val     サービスに対してビルド時の変数を設定します。
    --compress              gzip を使ってビルドコンテキストを圧縮します。
    --force-rm              常に中間コンテナーは削除します。
    -m, --memory MEM        ビルドするコンテナーのメモリ上限を設定します。
    --no-cache              イメージビルド時にキャッシュを使用しません。
    --no-rm                 ビルド生成後に中間コンテナーを削除しません。
    --parallel              並行的にイメージをビルドします。
    --progress string       処理経過の出力タイプ。（`auto`、`plain`、`tty`）
                            ネイティブビルダー用に `EXPERIMENTAL` フラグがあり、これを有効
                            にするには `COMPOSE_DOCKER_CLI_BUILD=1` を指定して実行。
    --pull                  プルを行う際に常に最新版のイメージの取得を試みます。
    -q, --quiet             `STDOUT` に何も出力しません。
```

{% comment %}
Services are built once and then tagged, by default as `project_service`. For
example, `composetest_db`. If the Compose file specifies an
[image](../compose-file/index.md#image) name, the image is
tagged with that name, substituting any variables beforehand. See
[variable substitution](../compose-file/index.md#variable-substitution).
{% endcomment %}
サービスは `プロジェクト名_サービス` として構築時にタグ付けられます。
例えば `composetest_db` です。
Compose ファイルが[イメージ](../compose-file/index.md#image) 名を指定している場合、イメージはその名称によってタグづけされます。変数が用いられている場合は、あらかじめ置換されます。
これについては[変数置換](../compose-file/index.md#variable-substitution) を参照してください。

{% comment %}
If you change a service's Dockerfile or the contents of its
build directory, run `docker-compose build` to rebuild it.
{% endcomment %}
サービスの Dockerfile やビルドディレクトリの内容を変更する場合は、`docker-compose build` を実行して再ビルドします。
