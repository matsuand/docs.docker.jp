---
description: 実行中のプロセスを表示します。
keywords: fig, composition, compose, docker, orchestration, cli, top
title: docker-compose top
notoc: true
---

{% comment %}
```none
Usage: top [SERVICE...]

```
{% endcomment %}
```none
利用方法: top [SERVICE...]

```

{% comment %}
Displays the running processes.
{% endcomment %}
実行中のプロセスを表示します。

```bash
$ docker-compose top
compose_service_a_1
PID    USER   TIME   COMMAND
----------------------------
4060   root   0:00   top

compose_service_b_1
PID    USER   TIME   COMMAND
----------------------------
4115   root   0:00   top
```
