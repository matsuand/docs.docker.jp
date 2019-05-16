---
title: scratch
keywords: library, sample, scratch
repo: scratch
layout: docs
permalink: /samples/library/scratch/
redirect_from:
- /samples/scratch/
description: |
  意図的な空のイメージ。「一から」のイメージ構築用。
---

{% comment %}
an explicitly empty image, especially for building images "FROM scratch"
{% endcomment %}
意図的な空のイメージ。
「一から」のイメージ構築用。

{% comment %}
> **Library reference**
>
> This content is imported from
> [the official Docker Library docs](https://github.com/docker-library/docs/tree/master/{{ page.repo}}/),
> and is provided by the original uploader. You can view the Docker Hub page for this image at
> [https://hub.docker.com/images/{{ page.repo }}](https://hub.docker.com/images/{{ page.repo }})
{% endcomment %}
> **ライブラリリファレンス**
>
> このイメージは [公式の Docker ライブラリドキュメント](https://github.com/docker-library/docs/tree/master/{{ page.repo}}/) からインポートできるものであり、オリジナルのアップロード者が提供します。
> Docker Hub の [https://hub.docker.com/images/{{ page.repo }}](https://hub.docker.com/images/{{ page.repo }}) においてこのイメージを確認することができます。

<!-- content begin -->
{% raw %}
# `FROM scratch`

このイメージは（[`debian`](https://registry.hub.docker.com/_/debian/)や[`busybox`](https://registry.hub.docker.com/_/busybox/)のような）ベースイメージの構築、あるいは（[`hello-world`](https://registry.hub.docker.com/_/hello-world/))のように単一の実行バイナリだけが含まれているような）最大限に小さくしたイメージの構築に対して有用なものです。

Docker 1.5.0 から（特に[`docker/docker#8827`](https://github.com/docker/docker/pull/8827)から）、`Dockerfile` 内の `FROM scratch` の行は何も処理をしないものとなりました。したがってイメージ内に追加のレイヤーを生成しません。
（つまりこれまで 2 レイヤーからなるイメージであったものが、今は 1 レイヤーのイメージとなります。）

[https://docs.docker.com/engine/userguide/eng-image/baseimages/](https://docs.docker.com/engine/userguide/eng-image/baseimages/#creating-a-simple-parent-image-using-scratch)からの引用。

> Docker が規定する最小イメージ `scratch` は、コンテナーを構築するベースイメージとして利用できます。
> `scratch` を利用すると、「イメージ」は、`Dockerfile`内の次に実行したいコマンドの構築プロセスに対して、最初のファイルシステムレイヤーとなるように指示を出します。
>
> Docker Hub 上の Docker リポジトリとして `scratch` が登場したことにより、`scratch` という名前を使ったイメージのアップロード、実行、タグづけはできなくなりました。
> そのかわり`Dockerfile` 内での参照のみが可能です。
> たとえば `scratch` を利用した最小コンテナーの生成は以下のようになります。

```dockerfile
FROM scratch
COPY hello /
CMD ["/hello"]
```
{% endraw %}
