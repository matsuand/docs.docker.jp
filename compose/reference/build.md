---
description: Build or rebuild services.
keywords: fig, composition, compose, docker, orchestration, cli, build
title: docker-compose build
notoc: true

---

```
Usage: build [options] [--build-arg key=val...] [SERVICE...]

Options:
    --compress              Compress the build context using gzip.
    --force-rm              Always remove intermediate containers.
    --no-cache              Do not use cache when building the image.
    --pull                  Always attempt to pull a newer version of the image.
    -m, --memory MEM        Sets memory limit for the build container.
    --build-arg key=val     Set build-time variables for services.
    --parallel              Build images in parallel.
```

<!-- v17.06 ja: 原文修正未対応 -->
サービスは `プロジェクト名_サービス` として構築時にタグ付けられます。例： `composetest_db` 。サービスの Dockerfile や構築ディレクトリの内容に変更を加える場合は、 `docker-compose build` で再構築を実行します。

<!-- v17.06 ja: 原文修正未対応 -->
If you change a service's Dockerfile or the contents of its
build directory, run `docker-compose build` to rebuild it.
