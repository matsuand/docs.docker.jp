---
description: ログドライバーを設定します。
keywords: docker, logging, driver
redirect_from:
- /engine/reference/logging/overview/
- /engine/reference/logging/
- /engine/admin/reference/logging/
- /engine/admin/logging/overview/
title: ログドライバーの設定
---

{% comment %}
Docker includes multiple logging mechanisms to help you
[get information from running containers and services](index.md).
These mechanisms are called logging drivers.
{% endcomment %}
Docker にはログ出力のメカニズムが複数あるため、[コンテナーまたはサービスのログ確認](index.md) を行うことができます。
このメカニズムのことをログドライバー（logging driver）と呼びます。

{% comment %}
Each Docker daemon has a default logging driver, which each container uses
unless you configure it to use a different logging driver.
{% endcomment %}
Docker デーモンにはデフォルトのログドライバーが設定されています。
別のログドライバーを利用するような設定を行わないかぎり、各コンテナーはこのデフォルトログドライバーを利用します。

{% comment %}
In addition to using the logging drivers included with Docker, you can also
implement and use [logging driver plugins](plugins.md).
{% endcomment %}
Docker が提供するログドライバーの利用だけでなく、[ログドライバープラグイン](plugins.md) を導入して利用することもできます。

{% comment %}
## Configure the default logging driver
{% endcomment %}
{: #configure-the-default-logging-driver }
## デフォルトのログドライバー設定

{% comment %}
To configure the Docker daemon to default to a specific logging driver, set the
value of `log-driver` to the name of the logging driver in the `daemon.json`
file, which is located in `/etc/docker/` on Linux hosts or
`C:\ProgramData\docker\config\` on Windows server hosts. Note that you should create `daemon.json`
file, if the file does not exist.
The default logging driver is `json-file`. The following example explicitly sets the default logging driver to `syslog`:
{% endcomment %}
特定のログドライバーを Docker デーモンのデフォルトとして設定するには、`daemon.json` ファイルにおいて `log-driver` にそのログドライバー名を指定します。
`daemon.json` ファイルは Linux ホストの場合は `/etc/docker/`、Windows サーバーホストの場合は `C:\ProgramData\docker\config\` にあります。
この `daemon.json` ファイルが存在していない場合は、生成する必要があります。
デフォルトのログドライバーは `json-file` です。
以下に示す例では、デフォルトのログドライバーを明示的に `syslog` に設定します。

```json
{
  "log-driver": "syslog"
}
```

{% comment %}
If the logging driver has configurable options, you can set them in the
`daemon.json` file as a JSON object with the key `log-opts`. The following
example sets two configurable options on the `json-file` logging driver:
{% endcomment %}
ログドライバーに設定変更可能なオプションがある場合、`daemon.json` ファイル内において `log-opts` キーを使って JSON オブジェクトとして指定することができます。
以下の例は、ログドライバー `json-file` において 2 つの設定オプションを指定します。

```json
{
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "10m",
    "max-file": "3",
    "labels": "production_status",
    "env": "os,customer"
  }
}
```

{% comment %}
> **Note**
>
> `log-opts` configuration options in the `daemon.json` configuration file must
> be provided as strings. Boolean and numeric values (such as the value for
> `max-file` in the example above) must therefore be enclosed in quotes (`"`).
{% endcomment %}
> **メモ**
>
> 設定ファイル `daemon.json` 内の `log-opts` オプションは文字列として指定する必要があります。
> したがってブール値や数値（上の例でいうと `max-file` の設定値）はクォート（`"`）で囲む必要があります。

{% comment %}
If you do not specify a logging driver, the default is `json-file`. Thus,
the default output for commands such as `docker inspect <CONTAINER>` is JSON.
{% endcomment %}
ログドライバーを指定しなかった場合、デフォルトは `json-file` です。
したがってたとえば `docker inspect <CONTAINER>` のようなコマンドの出力は JSON 形式となります。

{% comment %}
To find the current default logging driver for the Docker daemon, run
`docker info` and search for `Logging Driver`. You can use the following
command on Linux, macOS, or PowerShell on Windows:
{% endcomment %}
Docker デーモンにおいて設定されている、その時点でのデフォルトのログドライバーが何であるかは、`docker info` を実行して `Logging Driver` の項目を見ればわかります。
以下のコマンドは Linux、macOS、Windows 上の PowerShell において実行することができます。

{% raw %}
```bash
$ docker info --format '{{.LoggingDriver}}'

json-file
```
{% endraw %}

{% comment %}
## Configure the logging driver for a container
{% endcomment %}
{: #configure-the-logging-driver-for-a-container }
## コンテナーのログドライバー設定

{% comment %}
When you start a container, you can configure it to use a different logging
driver than the Docker daemon's default, using the `--log-driver` flag. If the
logging driver has configurable options, you can set them using one or more
instances of the `--log-opt <NAME>=<VALUE>` flag. Even if the container uses the
default logging driver, it can use different configurable options.
{% endcomment %}
コンテナーの起動時には、Docker デーモンのデフォルトとは異なる、別のログドライバーを設定することができます。
これは `--log-driver` フラグを使います。
ログドライバーに設定変更可能なオプションがある場合、`--log-opt <NAME>=<VALUE>` フラグを必要な分だけ用いて指定することができます。
コンテナーがデフォルトのログドライバーを利用しているとしても、設定変更可能なオプションはさまざまなものがあります。

{% comment %}
The following example starts an Alpine container with the `none` logging driver.
{% endcomment %}
以下の例は Alpine コンテナーの起動時に、ログドライバーとして `none` を指定しています。

```bash
$ docker run -it --log-driver none alpine ash
```

{% comment %}
To find the current logging driver for a running container, if the daemon
is using the `json-file` logging driver, run the following `docker inspect`
command, substituting the container name or ID for `<CONTAINER>`:
{% endcomment %}
デーモンがログドライバー `json-file` を利用している場合に、実行コンテナーに設定されているログドライバーが何であるかを確認するには、以下のように `docker inspect` を実行します。
実行にあたっては `<CONTAINER>` の部分をコンテナー名か、あるいはコンテナー ID とします。

{% raw %}
```bash
$ docker inspect -f '{{.HostConfig.LogConfig.Type}}' <CONTAINER>

json-file
```
{% endraw %}

{% comment %}
## Configure the delivery mode of log messages from container to log driver
{% endcomment %}
{: #Configure-the-delivery-mode-of-log-messages-from-container-to-log-driver }
## Configure the delivery mode of log messages from container to log driver

{% comment %}
Docker provides two modes for delivering messages from the container to the log
driver:
{% endcomment %}
Docker provides two modes for delivering messages from the container to the log
driver:

{% comment %}
* (default) direct, blocking delivery from container to driver
* non-blocking delivery that stores log messages in an intermediate per-container
  ring buffer for consumption by driver
{% endcomment %}
* (default) direct, blocking delivery from container to driver
* non-blocking delivery that stores log messages in an intermediate per-container
  ring buffer for consumption by driver

{% comment %}
The `non-blocking` message delivery mode prevents applications from blocking due
to logging back pressure. Applications are likely to fail in unexpected ways when
STDERR or STDOUT streams block.
{% endcomment %}
The `non-blocking` message delivery mode prevents applications from blocking due
to logging back pressure. Applications are likely to fail in unexpected ways when
STDERR or STDOUT streams block.

{% comment %}
> **WARNING**
> When the buffer is full and a new message is enqueued, the oldest message in
> memory is dropped.  Dropping messages is often preferred to blocking the
> log-writing process of an application.
{: .warning}
{% endcomment %}
> **WARNING**
> When the buffer is full and a new message is enqueued, the oldest message in
> memory is dropped.  Dropping messages is often preferred to blocking the
> log-writing process of an application.
{: .warning}

{% comment %}
The `mode` log option controls whether to use the `blocking` (default) or
`non-blocking` message delivery.
{% endcomment %}
The `mode` log option controls whether to use the `blocking` (default) or
`non-blocking` message delivery.

{% comment %}
The `max-buffer-size` log option controls the size of the ring buffer used for
intermediate message storage when `mode` is set to `non-blocking`. `max-buffer-size`
defaults to 1 megabyte.
{% endcomment %}
The `max-buffer-size` log option controls the size of the ring buffer used for
intermediate message storage when `mode` is set to `non-blocking`. `max-buffer-size`
defaults to 1 megabyte.

{% comment %}
The following example starts an Alpine container with log output in non-blocking
mode and a 4 megabyte buffer:
{% endcomment %}
The following example starts an Alpine container with log output in non-blocking
mode and a 4 megabyte buffer:

```bash
$ docker run -it --log-opt mode=non-blocking --log-opt max-buffer-size=4m alpine ping 127.0.0.1
```

{% comment %}
### Use environment variables or labels with logging drivers
{% endcomment %}
### Use environment variables or labels with logging drivers

{% comment %}
Some logging drivers add the value of a container's `--env|-e` or `--label`
flags to the container's logs. This example starts a container using the Docker
daemon's default logging driver (let's assume `json-file`) but sets the
environment variable `os=ubuntu`.
{% endcomment %}
Some logging drivers add the value of a container's `--env|-e` or `--label`
flags to the container's logs. This example starts a container using the Docker
daemon's default logging driver (let's assume `json-file`) but sets the
environment variable `os=ubuntu`.

```bash
$ docker run -dit --label production_status=testing -e os=ubuntu alpine sh
```

{% comment %}
If the logging driver supports it, this adds additional fields to the logging
output. The following output is generated by the `json-file` logging driver:
{% endcomment %}
If the logging driver supports it, this adds additional fields to the logging
output. The following output is generated by the `json-file` logging driver:

```json
"attrs":{"production_status":"testing","os":"ubuntu"}
```

{% comment %}
## Supported logging drivers
{% endcomment %}
## Supported logging drivers

{% comment %}
The following logging drivers are supported. See the link to each driver's
documentation for its configurable options, if applicable. If you are using
[logging driver plugins](plugins.md), you may
see more options.
{% endcomment %}
The following logging drivers are supported. See the link to each driver's
documentation for its configurable options, if applicable. If you are using
[logging driver plugins](plugins.md), you may
see more options.

{% comment %}
| Driver                        | Description                                                                                                   |
|:------------------------------|:--------------------------------------------------------------------------------------------------------------|
| `none`                        | No logs are available for the container and `docker logs` does not return any output.                         |
| [`local`](local.md)           | Logs are stored in a custom format designed for minimal overhead.                                             |
| [`json-file`](json-file.md)   | The logs are formatted as JSON. The default logging driver for Docker.                                        |
| [`syslog`](syslog.md)         | Writes logging messages to the `syslog` facility. The `syslog` daemon must be running on the host machine.    |
| [`journald`](journald.md)     | Writes log messages to `journald`. The `journald` daemon must be running on the host machine.                 |
| [`gelf`](gelf.md)             | Writes log messages to a Graylog Extended Log Format (GELF) endpoint such as Graylog or Logstash.             |
| [`fluentd`](fluentd.md)       | Writes log messages to `fluentd` (forward input). The `fluentd` daemon must be running on the host machine.   |
| [`awslogs`](awslogs.md)       | Writes log messages to Amazon CloudWatch Logs.                                                                |
| [`splunk`](splunk.md)         | Writes log messages to `splunk` using the HTTP Event Collector.                                               |
| [`etwlogs`](etwlogs.md)       | Writes log messages as Event Tracing for Windows (ETW) events. Only available on Windows platforms.           |
| [`gcplogs`](gcplogs.md)       | Writes log messages to Google Cloud Platform (GCP) Logging.                                                   |
| [`logentries`](logentries.md) | Writes log messages to Rapid7 Logentries.                                                                     |
{% endcomment %}
| ドライバー                    | 内容説明                                                                                                      |
|:------------------------------|:--------------------------------------------------------------------------------------------------------------|
| `none`                        | No logs are available for the container and `docker logs` does not return any output.                         |
| [`local`](local.md)           | Logs are stored in a custom format designed for minimal overhead.                                             |
| [`json-file`](json-file.md)   | The logs are formatted as JSON. The default logging driver for Docker.                                        |
| [`syslog`](syslog.md)         | Writes logging messages to the `syslog` facility. The `syslog` daemon must be running on the host machine.    |
| [`journald`](journald.md)     | Writes log messages to `journald`. The `journald` daemon must be running on the host machine.                 |
| [`gelf`](gelf.md)             | Writes log messages to a Graylog Extended Log Format (GELF) endpoint such as Graylog or Logstash.             |
| [`fluentd`](fluentd.md)       | Writes log messages to `fluentd` (forward input). The `fluentd` daemon must be running on the host machine.   |
| [`awslogs`](awslogs.md)       | Writes log messages to Amazon CloudWatch Logs.                                                                |
| [`splunk`](splunk.md)         | Writes log messages to `splunk` using the HTTP Event Collector.                                               |
| [`etwlogs`](etwlogs.md)       | Writes log messages as Event Tracing for Windows (ETW) events. Only available on Windows platforms.           |
| [`gcplogs`](gcplogs.md)       | Writes log messages to Google Cloud Platform (GCP) Logging.                                                   |
| [`logentries`](logentries.md) | Writes log messages to Rapid7 Logentries.                                                                     |

{% comment %}
## Limitations of logging drivers
{% endcomment %}
{: #limitations-of-logging-drivers }
## ログドライバーの制約

- Users of Docker Enterprise can make use of "dual logging", which enables you
  to use the `docker logs` command for any logging driver. Refer to
  [reading logs when using remote logging drivers](dual-logging.md) for
  information about using `docker logs` to read container logs locally for many
  third party logging solutions, including:
    - `syslog`
    - `gelf`
    - `fluentd`
    - `awslogs`
    - `splunk`
    - `etwlogs`
    - `gcplogs`
    - `Logentries`
- When using Docker Community Engine, the `docker logs` command is only available
  on the following drivers:
    - `local`
    - `json-file`
    - `journald`
- Reading log information requires decompressing rotated log files, which causes
  a temporary increase in disk usage (until the log entries from the rotated
  files are read) and an increased CPU usage while decompressing.
{% comment %}
{% endcomment %}
- The capacity of the host storage where the Docker data directory resides
  determines the maximum size of the log file information.
