---
description: アクティブなマシンを識別します。
keywords: machine, active, subcommand
title: docker-machine active
hide_from_sitemap: true
---

{% comment %}
See which machine is "active" (a machine is considered active if the
`DOCKER_HOST` environment variable points to it).
{% endcomment %}
どのマシンが「アクティブ」であるかを確認します。
（環境変数 `DOCKER_HOST` が設定されている場合は、そのマシンがアクティブとみなされます。）

```bash
$ docker-machine ls

NAME      ACTIVE   DRIVER         STATE     URL
dev       -        virtualbox     Running   tcp://192.168.99.103:2376
staging   *        digitalocean   Running   tcp://203.0.113.81:2376

$ echo $DOCKER_HOST
tcp://203.0.113.81:2376

$ docker-machine active
staging
```
