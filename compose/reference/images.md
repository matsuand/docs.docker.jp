---
description: プロジェクトにおいて用いられているイメージの一覧を表示します。
keywords: fig, composition, compose, docker, orchestration, cli, images
title: docker-compose images
notoc: true
---

{% comment %}
```none
Usage: images [options] [SERVICE...]

Options:
    -q, --quiet  Only display IDs
```
{% endcomment %}
```none
利用方法: images [オプション] [SERVICE...]

オプション:
    -q, --quiet  ID のみを表示します。
```

{% comment %}
List images used by the created containers.
{% endcomment %}
生成済コンテナーによって利用されているイメージの一覧を表示します。
