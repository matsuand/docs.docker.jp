---
description: Forces running containers to stop.
keywords: fig, composition, compose, docker, orchestration, cli,  kill
title: docker-compose kill
notoc: true
---

{% comment %}
```none
Usage: kill [options] [SERVICE...]

Options:
    -s SIGNAL         SIGNAL to send to the container.
                      Default signal is SIGKILL.
```
{% endcomment %}
```none
利用方法: kill [オプション] [SERVICE...]

オプション:
    -s SIGNAL         SIGNAL to send to the container.
                      Default signal is SIGKILL.
```

{% comment %}
Forces running containers to stop by sending a `SIGKILL` signal. Optionally the
signal can be passed, for example:
{% endcomment %}
Forces running containers to stop by sending a `SIGKILL` signal. Optionally the
signal can be passed, for example:

    docker-compose kill -s SIGINT
