---
title: logentries ログドライバー
description: logentries ログドライバーの利用方法について説明します。
keywords: logentries, docker, logging, driver
redirect_from:
- /engine/admin/logging/logentries/
---

{% comment %}
The `logentries` logging driver sends container logs to the
[Logentries](https://logentries.com/) server.
{% endcomment %}
`logentries` ログドライバーは、コンテナーログを [Logentries](https://logentries.com/) サーバーに送信します。

{% comment %}
## Usage
{% endcomment %}
{: #usage }
## 利用方法

{% comment %}
Some options are supported by specifying `--log-opt` as many times as needed:
{% endcomment %}
Some options are supported by specifying `--log-opt` as many times as needed:

 {% comment %}
 {% endcomment %}
 - `logentries-token`: specify the logentries log set token
 - `line-only`: send raw payload only

{% comment %}
{% endcomment %}
Configure the default logging driver by passing the
`--log-driver` option to the Docker daemon:

```bash
$ dockerd --log-driver=logentries
```

{% comment %}
{% endcomment %}
To set the logging driver for a specific container, pass the
`--log-driver` option to `docker run`:

```bash
$ docker run --log-driver=logentries ...
```

{% comment %}
{% endcomment %}
Before using this logging driver, you need to create a new Log Set in the
Logentries web interface and pass the token of that log set to Docker:

```bash
$ docker run --log-driver=logentries --log-opt logentries-token=abcd1234-12ab-34cd-5678-0123456789ab
```

{% comment %}
{% endcomment %}
## Options

{% comment %}
{% endcomment %}
Users can use the `--log-opt NAME=VALUE` flag to specify additional Logentries logging driver options.

{% comment %}
{% endcomment %}
### logentries-token

{% comment %}
{% endcomment %}
You need to provide your log set token for logentries driver to work:

```bash
$ docker run --log-driver=logentries --log-opt logentries-token=abcd1234-12ab-34cd-5678-0123456789ab
```

{% comment %}
{% endcomment %}
### line-only

{% comment %}
{% endcomment %}
You could specify whether to send log message wrapped into container data (default) or to send raw log line

```bash
$ docker run --log-driver=logentries --log-opt logentries-token=abcd1234-12ab-34cd-5678-0123456789ab --log-opt line-only=true
```
