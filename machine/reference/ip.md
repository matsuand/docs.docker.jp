---
description: Show client configuration
keywords: machine, ip, subcommand
title: docker-machine ip
hide_from_sitemap: true
---

{% comment %}
Get the IP address of one or more machines.
{% endcomment %}
1 つまたは複数のマシンの IP アドレスを取得します。

```bash
$ docker-machine ip dev

192.168.99.104

$ docker-machine ip dev dev2

192.168.99.104
192.168.99.105
```
