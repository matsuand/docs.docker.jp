---
description: サービスのビルドまたは再ビルド。
keywords: fig, composition, compose, docker, orchestration, cli, build
title: docker-compose build
notoc: true

---

<!--
Usage: build [options] [--build-arg key=val...] [SERVICE...]

Options:
    --compress              Compress the build context using gzip.
    --force-rm              Always remove intermediate containers.
    --no-cache              Do not use cache when building the image.
    --pull                  Always attempt to pull a newer version of the image.
    -m, --memory MEM        Sets memory limit for the build container.
    --build-arg key=val     Set build-time variables for services.
    --parallel              Build images in parallel.
-->
```
利用方法: build [オプション] [--build-arg key=val...] [SERVICE...]

オプション:
    --compress              gzip を使ってビルドコンテキストを圧縮します。
    --force-rm              常に中間コンテナーは削除します。
    --no-cache              イメージビルド時にキャッシュを使用しません。
    --pull                  プルを行う際に常に最新版のイメージの取得を試みます。
    -m, --memory MEM        ビルドコンテナーにおけるメモリ上限を設定します。
    --build-arg key=val     サービスに対してビルド時の変数を設定します。
    --parallel              並行的にイメージをビルドします。
```

<!--
Services are built once and then tagged, by default as `project_service`. For
example, `composetest_db`. If the Compose file specifies an
[image](/compose/compose-file/index.md#image) name, the image is
tagged with that name, substituting any variables beforehand. See [variable
substitution](/compose/compose-file/#variable-substitution).
-->
サービスは `プロジェクト名_サービス` として構築時にタグ付けられます。
例えば `composetest_db` です。
Compose ファイルが[イメージ](/compose/compose-file/index.md#image) 名を指定している場合、イメージはその名称によってタグづけされます。変数が用いられている場合は、あらかじめ置換されます。
これについては[変数置換](/compose/compose-file/#variable-substitution)を参照してください。

<!--
If you change a service's Dockerfile or the contents of its
build directory, run `docker-compose build` to rebuild it.
-->
サービスの Dockerfile やビルドディレクトリの内容を変更する場合は、`docker-compose build` を実行して再ビルドします。
