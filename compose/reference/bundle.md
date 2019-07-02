---
description: Compose ファイルから分散アプリケーションバンドル（Distributed Application Bundle; DAB）を生成します。
keywords: fig, composition, compose, docker, orchestration, cli, bundle
title: docker-compose bundle
notoc: true
---

{% comment %}
```
Usage: bundle [options]

Options:
    --push-images              Automatically push images for any services
                               which have a `build` option specified.

    -o, --output PATH          Path to write the bundle file to.
                               Defaults to "<project name>.dab".
```
{% endcomment %}
```
利用方法: bundle [オプション]

オプション:
    --push-images              `build` オプションが指定されているサービス
                               のイメージをすべて自動的にプッシュします。

    -o, --output PATH          バンドルファイルの出力パスを指定します。
```

{% comment %}
Generate a Distributed Application Bundle (DAB) from the Compose file.
{% endcomment %}
Compose ファイルから分散アプリケーションバンドル（Distributed Application Bundle; DAB）を生成します。

{% comment %}
Images must have digests stored, which requires interaction with a
Docker registry. If digests aren't stored for all images, you can fetch
them with `docker-compose pull` or `docker-compose push`. To push images
automatically when bundling, pass `--push-images`. Only services with
a `build` option specified have their images pushed.
{% endcomment %}
イメージにはダイジェスト値が保存されていなければなりません。
この値は Docker レジストリとのやり取りにおいて必要になります。
ダイジェスト値がイメージすべてに保存されていないときは、`docker-compose pull` または `docker-compose push` の実行によって取得することができます。
バンドルを生成すると同時に、自動的にイメージをプッシュするには `--push-images` を指定してください。
`build` オプションが指定されているサービスだけが、イメージをプッシュすることができます。
