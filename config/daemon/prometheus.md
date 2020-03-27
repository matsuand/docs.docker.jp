---
description: Prometheus を用いた Docker メトリックスの収集。
keywords: prometheus, metrics
title: Prometheus を用いた Docker メトリックスの収集
redirect_from:
- /engine/admin/prometheus/
- /config/thirdparty/monitoring/
- /config/thirdparty/prometheus/
---

{% comment %}
[Prometheus](https://prometheus.io/) is an open-source systems monitoring and
alerting toolkit. You can configure Docker as a Prometheus target. This topic
shows you how to configure Docker, set up Prometheus to run as a Docker
container, and monitor your Docker instance using Prometheus.
{% endcomment %}
[Prometheus](https://prometheus.io/) はシステム監視や警告を行うオープンソースのツールキットです。
この Prometheus の対象として Docker を設定することができます。
ここでは Docker の設定、Docker コンテナーとしての Prometheus の設定、Prometheus を使った Docker インスタンスの監視について示します。

{% comment %}
> **Warning**: The available metrics and the names of those metrics are in
> active development and may change at any time.
{% endcomment %}
> **警告**: 利用可能なメトリックスおよびその名称は、現在開発中のものであるため、随時変更されます。

{% comment %}
Currently, you can only monitor Docker itself. You cannot currently monitor your
application using the Docker target.
{% endcomment %}
現時点において監視できる対象は Docker そのものです。
Docker ターゲットとしてアプリケーションを監視することは、今のところできません。


{% comment %}
## Configure Docker
{% endcomment %}
{: #configure-docker }
## Docker の設定

{% comment %}
To configure the Docker daemon as a Prometheus target, you need to specify the
`metrics-address`. The best way to do this is via the `daemon.json`, which is
located at one of the following locations by default. If the file does not
exist, create it.
{% endcomment %}
Docker デーモンを Prometheus のターゲットとして設定するには、`metrics-address` を指定する必要があります。
これを行う一番良い方法は `daemon.json` に記述することです。
デフォルトにおいて `daemon.json` は以下に示すいずれかのディレクトリにあります。
もしこのファイルが存在していない場合は、新規に生成します。

{% comment %}
- **Linux**: `/etc/docker/daemon.json`
- **Windows Server**: `C:\ProgramData\docker\config\daemon.json`
- **Docker Desktop for Mac / Docker Desktop for Windows**: Click the Docker icon in the toolbar,
  select **Preferences**, then select **Daemon**. Click **Advanced**.
{% endcomment %}
- **Linux**: `/etc/docker/daemon.json`
- **Windows Server**: `C:\ProgramData\docker\config\daemon.json`
- **Docker Desktop for Mac / Docker Desktop for Windows**:
  ツールバーの Docker アイコンをクリック、**Preferences**、**Daemon** を選択。**Advanced** をクリック。

{% comment %}
If the file is currently empty, paste the following:
{% endcomment %}
このファイルが空であった場合は、以下の内容を貼り付けます。

```json
{
  "metrics-addr" : "127.0.0.1:9323",
  "experimental" : true
}
```

{% comment %}
If the file is not empty, add those two keys, making sure that the resulting
file is valid JSON. Be careful that every line ends with a comma (`,`) except
for the last line.
{% endcomment %}
このファイルが空でなかった場合は、上の 2 つのキーを追加します。
書き加えた結果は正しい JSON フォーマットでなければなりません。
最終行を除き、各行の終わりはカンマ（`,`）が必要です。

{% comment %}
Save the file, or in the case of Docker Desktop for Mac or Docker Desktop for Windows, save the
configuration. Restart Docker.
{% endcomment %}
ファイルを保存します。
また Docker Desktop for Mac や Docker Desktop for Windows を利用している場合は、設定を保存します。
そして Docker を再起動します。

{% comment %}
Docker now exposes Prometheus-compatible metrics on port 9323.
{% endcomment %}
これにより Docker は、Prometheus 互換メトリックスをポート 9323 番にて公開することになります。

{% comment %}
## Configure and run Prometheus
{% endcomment %}
{: #configure-and-run-prometheus }
## Prometheus の設定と実行

{% comment %}
Prometheus runs as a Docker service on a Docker swarm.
{% endcomment %}
Docker swarm 上の Docker サービスとして Prometheus を実行します。

{% comment %}
> **Prerequisites**
>
> 1.  One or more Docker engines are joined into a Docker swarm, using `docker swarm init`
>     on one manager and `docker swarm join` on other managers and worker nodes.
>
> 2.  You need an internet connection to pull the Prometheus image.
{% endcomment %}
> **前提条件**
>
> 1.  One or more Docker engines are joined into a Docker swarm, using `docker swarm init`
>     on one manager and `docker swarm join` on other managers and worker nodes.
>
> 2.  You need an internet connection to pull the Prometheus image.


{% comment %}
Copy one of the following configuration files and save it to
`/tmp/prometheus.yml` (Linux or Mac) or `C:\tmp\prometheus.yml` (Windows). This
is a stock Prometheus configuration file, except for the addition of the Docker
job definition at the bottom of the file. Docker Desktop for Mac and Docker Desktop for Windows
need a slightly different configuration.
{% endcomment %}
Copy one of the following configuration files and save it to
`/tmp/prometheus.yml` (Linux or Mac) or `C:\tmp\prometheus.yml` (Windows). This
is a stock Prometheus configuration file, except for the addition of the Docker
job definition at the bottom of the file. Docker Desktop for Mac and Docker Desktop for Windows
need a slightly different configuration.

<ul class="nav nav-tabs">
<li class="active"><a data-toggle="tab" data-target="#linux-config" data-group="linux">Docker for Linux</a></li>
<li><a data-toggle="tab" data-target="#mac-config" data-group="mac">Docker Desktop for Mac</a></li>
<li><a data-toggle="tab" data-target="#win-config" data-group="win">Docker Desktop for Windows</a></li>
</ul>

<div class="tab-content">
<div id="linux-config" class="tab-pane fade in active" markdown="1">

```yml
# my global config
global:
  scrape_interval:     15s # Set the scrape interval to every 15 seconds. Default is every 1 minute.
  evaluation_interval: 15s # Evaluate rules every 15 seconds. The default is every 1 minute.
  # scrape_timeout is set to the global default (10s).

  # Attach these labels to any time series or alerts when communicating with
  # external systems (federation, remote storage, Alertmanager).
  external_labels:
      monitor: 'codelab-monitor'

# Load rules once and periodically evaluate them according to the global 'evaluation_interval'.
rule_files:
  # - "first.rules"
  # - "second.rules"

# A scrape configuration containing exactly one endpoint to scrape:
# Here it's Prometheus itself.
scrape_configs:
  # The job name is added as a label `job=<job_name>` to any timeseries scraped from this config.
  - job_name: 'prometheus'

    # metrics_path defaults to '/metrics'
    # scheme defaults to 'http'.

    static_configs:
      - targets: ['localhost:9090']

  - job_name: 'docker'
         # metrics_path defaults to '/metrics'
         # scheme defaults to 'http'.

    static_configs:
      - targets: ['localhost:9323']
```

</div><!-- linux -->
<div id="mac-config" class="tab-pane fade" markdown="1">

```yml
# my global config
global:
  scrape_interval:     15s # Set the scrape interval to every 15 seconds. Default is every 1 minute.
  evaluation_interval: 15s # Evaluate rules every 15 seconds. The default is every 1 minute.
  # scrape_timeout is set to the global default (10s).

  # Attach these labels to any time series or alerts when communicating with
  # external systems (federation, remote storage, Alertmanager).
  external_labels:
      monitor: 'codelab-monitor'

# Load rules once and periodically evaluate them according to the global 'evaluation_interval'.
rule_files:
  # - "first.rules"
  # - "second.rules"

# A scrape configuration containing exactly one endpoint to scrape:
# Here it's Prometheus itself.
scrape_configs:
  # The job name is added as a label `job=<job_name>` to any timeseries scraped from this config.
  - job_name: 'prometheus'

    # metrics_path defaults to '/metrics'
    # scheme defaults to 'http'.

    static_configs:
      - targets: ['docker.for.mac.localhost:9090']

  - job_name: 'docker'
         # metrics_path defaults to '/metrics'
         # scheme defaults to 'http'.

    static_configs:
      - targets: ['docker.for.mac.host.internal:9323']
```

</div><!-- mac -->
<div id="win-config" class="tab-pane fade" markdown="1">

```yml
# my global config
global:
  scrape_interval:     15s # Set the scrape interval to every 15 seconds. Default is every 1 minute.
  evaluation_interval: 15s # Evaluate rules every 15 seconds. The default is every 1 minute.
  # scrape_timeout is set to the global default (10s).

  # Attach these labels to any time series or alerts when communicating with
  # external systems (federation, remote storage, Alertmanager).
  external_labels:
      monitor: 'codelab-monitor'

# Load rules once and periodically evaluate them according to the global 'evaluation_interval'.
rule_files:
  # - "first.rules"
  # - "second.rules"

# A scrape configuration containing exactly one endpoint to scrape:
# Here it's Prometheus itself.
scrape_configs:
  # The job name is added as a label `job=<job_name>` to any timeseries scraped from this config.
  - job_name: 'prometheus'

    # metrics_path defaults to '/metrics'
    # scheme defaults to 'http'.

    static_configs:
      - targets: ['docker.for.win.localhost:9090']

  - job_name: 'docker'
         # metrics_path defaults to '/metrics'
         # scheme defaults to 'http'.

    static_configs:
      - targets: ['192.168.65.1:9323']
```

</div><!-- windows -->
</div><!-- tabs -->


{% comment %}
Next, start a single-replica Prometheus service using this configuration.
{% endcomment %}
Next, start a single-replica Prometheus service using this configuration.

<ul class="nav nav-tabs">
<li class="active"><a data-toggle="tab" data-target="#linux-run" data-group="linux">Docker for Linux</a></li>
<li><a data-toggle="tab" data-target="#mac-run" data-group="mac">Docker Desktop for Mac</a></li>
<li><a data-toggle="tab" data-target="#win-run" data-group="win">Docker Desktop for Windows or Windows Server</a></li>
</ul>

<div class="tab-content">

<div id="linux-run" class="tab-pane fade in active" markdown="1">

```bash
$ docker service create --replicas 1 --name my-prometheus \
    --mount type=bind,source=/tmp/prometheus.yml,destination=/etc/prometheus/prometheus.yml \
    --publish published=9090,target=9090,protocol=tcp \
    prom/prometheus
```

</div><!-- linux -->
<div id="mac-run" class="tab-pane fade" markdown="1">

```bash
$ docker service create --replicas 1 --name my-prometheus \
    --mount type=bind,source=/tmp/prometheus.yml,destination=/etc/prometheus/prometheus.yml \
    --publish published=9090,target=9090,protocol=tcp \
    prom/prometheus
```

</div><!-- mac -->
<div id="win-run" class="tab-pane fade" markdown="1">

```powershell
PS C:\> docker service create --replicas 1 --name my-prometheus
    --mount type=bind,source=C:/tmp/prometheus.yml,destination=/etc/prometheus/prometheus.yml
    --publish published=9090,target=9090,protocol=tcp
    prom/prometheus
```

</div><!-- windows -->
</div><!-- tabs -->

{% comment %}
Verify that the Docker target is listed at http://localhost:9090/targets/.
{% endcomment %}
http://localhost:9090/targets/ において Docker ターゲットが一覧表示されていることを確認します。

{% comment %}
![Prometheus targets page](images/prometheus-targets.png)
{% endcomment %}
![Prometheus ターゲットページ](images/prometheus-targets.png)

{% comment %}
You can't access the endpoint URLs directly if you use Docker Desktop
for Mac or Docker Desktop for Windows.
{% endcomment %}
You can't access the endpoint URLs directly if you use Docker Desktop
for Mac or Docker Desktop for Windows.

{% comment %}
## Use Prometheus
{% endcomment %}
{: #use-prometheus }
## Prometheus の利用

{% comment %}
Create a graph. Click the **Graphs** link in the Prometheus UI. Choose a metric
from the combo box to the right of the **Execute** button, and click
**Execute**. The screenshot below shows the graph for
`engine_daemon_network_actions_seconds_count`.
{% endcomment %}
グラフを生成します。
Prometheus UI 画面の **Graphs** リンクをクリックします。
そして **Execute** ボタンの右にあるコンボボックスからメトリックを選び **Execute** をクリックします。
以下に示すスクリーンショットは `engine_daemon_network_actions_seconds_count` に対するグラフを示しています。

{% comment %}
![Prometheus engine_daemon_network_actions_seconds_count report](images/prometheus-graph_idle.png)
{% endcomment %}
![Prometheus engine_daemon_network_actions_seconds_count report](images/prometheus-graph_idle.png)

{% comment %}
The above graph shows a pretty idle Docker instance. Your graph might look
different if you are running active workloads.
{% endcomment %}
The above graph shows a pretty idle Docker instance. Your graph might look
different if you are running active workloads.

{% comment %}
To make the graph more interesting, create some network actions by starting
a service with 10 tasks that just ping Docker non-stop (you can change the
ping target to anything you like):
{% endcomment %}
To make the graph more interesting, create some network actions by starting
a service with 10 tasks that just ping Docker non-stop (you can change the
ping target to anything you like):

```bash
$ docker service create \
  --replicas 10 \
  --name ping_service \
  alpine ping docker.com
```

{% comment %}
Wait a few minutes (the default scrape interval is 15 seconds) and reload
your graph.
{% endcomment %}
Wait a few minutes (the default scrape interval is 15 seconds) and reload
your graph.

{% comment %}
![Prometheus engine_daemon_network_actions_seconds_count report](images/prometheus-graph_load.png)
{% endcomment %}
![Prometheus engine_daemon_network_actions_seconds_count report](images/prometheus-graph_load.png)

{% comment %}
When you are ready, stop and remove the `ping_service` service, so that you
are not flooding a host with pings for no reason.
{% endcomment %}
When you are ready, stop and remove the `ping_service` service, so that you
are not flooding a host with pings for no reason.

```bash
$ docker service remove ping_service
```

{% comment %}
Wait a few minutes and you should see that the graph falls back to the idle
level.
{% endcomment %}
Wait a few minutes and you should see that the graph falls back to the idle
level.


{% comment %}
## Next steps
{% endcomment %}
{: #next-steps }
## 次のステップ

{% comment %}
{% endcomment %}
- [Prometheus のドキュメント](https://prometheus.io/docs/introduction/overview/){: target="_blank" class="_" } を読む。
- [警告](https://prometheus.io/docs/alerting/overview/){: target="_blank" class="_" } を設定してみる。
