---
description: ネットワーク機能。
keywords: mac, networking
redirect_from:
- /mackit/networking/
title: Docker Desktop for Mac のネットワーク機能
---
{% assign Arch = 'Mac' %}

{% comment %}
Docker Desktop for {{Arch}} provides several networking features to make it
easier to use.
{% endcomment %}
Docker Desktop for {{Arch}} では、より簡単に利用できるネットワーク機能をいくつも提供しています。

{% comment %}
## Features
{% endcomment %}
{: #features }
## 機能

{% comment %}
### VPN Passthrough
{% endcomment %}
{: #vpn-passthrough }
### VPN Passthrough

{% comment %}
Docker Desktop for {{Arch}}'s networking can work when attached to a VPN. To do this,
Docker Desktop for {{Arch}} intercepts traffic from the containers and injects it into
{{Arch}} as if it originated from the Docker application.
{% endcomment %}
Docker Desktop for {{Arch}} のネットワーク機能は VPN に接続して利用することができます。

To do this,
Docker Desktop for {{Arch}} intercepts traffic from the containers and injects it into
{{Arch}} as if it originated from the Docker application.

{% comment %}
### Port Mapping
{% endcomment %}
{: #port-mapping }
### ポートマッピング

{% comment %}
When you run a container with the `-p` argument, for example:
{% endcomment %}
コンテナーの起動時に、たとえば以下のように `-p` 引数をつけたとします。

```
$ docker run -p 80:80 -d nginx
```

{% comment %}
Docker Desktop for {{Arch}} makes whatever is running on port 80 in the container (in
this case, `nginx`) available on port 80 of `localhost`. In this example, the
host and container ports are the same. What if you need to specify a different
host port? If, for example, you already have something running on port 80 of
your host machine, you can connect the container to a different port:
{% endcomment %}
Docker Desktop for {{Arch}} は、コンテナー上のポート 80 を使って稼動するものすべて（この場合は `nginx`）、`localhost` のポート 80 から利用できるようにします。
この例では、ホストとコンテナーのそれぞれのポート番号は同一にしています。
ホストのポートを別のものに指定する必要があるとしたら、どうなるでしょう。
つまり、ホストマシン上においてすでにポート 80 を利用して起動しているアプリケーションがあるなら、コンテナーのポートを別のものに接続したらよいことになります。

```
$ docker run -p 8000:80 -d nginx
```

{% comment %}
Now, connections to `localhost:8000` are sent to port 80 in the container. The
syntax for `-p` is `HOST_PORT:CLIENT_PORT`.
{% endcomment %}
こうすると `localhost:8000` への接続が、コンテナー内のポート 80 へ接続されます。
`-p` の文法は `ホストポート:クライアントポート` です。

{% comment %}
### HTTP/HTTPS Proxy Support
{% endcomment %}
{: #httphttps-proxy-support }
### HTTP/HTTPS プロキシーサポート

{% comment %}
See [Proxies](index.md#proxies).
{% endcomment %}
[プロキシー](index.md#proxies) を参照してください。

{% comment %}
## Known limitations, use cases, and workarounds
{% endcomment %}
{: #known-limitations-use-cases-and-workarounds }
## 既知の制約、利用状況、回避策

{% comment %}
Following is a summary of current limitations on the Docker Desktop for {{Arch}}
networking stack, along with some ideas for workarounds.
{% endcomment %}
Following is a summary of current limitations on the Docker Desktop for {{Arch}}
networking stack, along with some ideas for workarounds.

{% comment %}
### There is no docker0 bridge on macOS
{% endcomment %}
{: #there-is-no-docker0-bridge-on-macos }
### macOS 上に docker0 ブリッジがない

{% comment %}
Because of the way networking is implemented in Docker Desktop for Mac, you cannot see a
`docker0` interface on the host. This interface is actually within the virtual
machine.
{% endcomment %}
Docker Desktop for Mac におけるネットワーク機能の実装方法により、ホスト上から `docker0` インターフェースを見ることはできません。
このインターフェースは仮想マシン内にあります。

{% comment %}
### I cannot ping my containers
{% endcomment %}
{: #i-cannot-ping-my-containers }
### コンテナーに ping ができない

{% comment %}
Docker Desktop for Mac can't route traffic to containers.
{% endcomment %}
Docker Desktop for Mac can't route traffic to containers.

{% comment %}
### Per-container IP addressing is not possible
{% endcomment %}
{: #per-container-ip-addressing-is-not-possible }
### コンテナー単位の IP アドレス割り当てができない

{% comment %}
The docker (Linux) bridge network is not reachable from the macOS host.
{% endcomment %}
The docker (Linux) bridge network is not reachable from the macOS host.

{% comment %}
### Use cases and workarounds
{% endcomment %}
{: #use-cases-and-workarounds }
### 利用状況と回避策

{% comment %}
There are two scenarios that the above limitations affect:
{% endcomment %}
上記の制約から影響を受ける利用状況が 2 つあります。

{% comment %}
#### I want to connect from a container to a service on the host
{% endcomment %}
{: #i-want-to-connect-from-a-container-to-a-service-on-the-host }
#### コンテナーからホスト上のサービスに接続したい

{% comment %}
The host has a changing IP address (or none if you have no network access). From
18.03 onwards our recommendation is to connect to the special DNS name
`host.docker.internal`, which resolves to the internal IP address used by the
host.
This is for development purpose and will not work in a production environment outside of Docker Desktop for Mac.
{% endcomment %}
The host has a changing IP address (or none if you have no network access). From
18.03 onwards our recommendation is to connect to the special DNS name
`host.docker.internal`, which resolves to the internal IP address used by the
host.
This is for development purpose and will not work in a production environment outside of Docker Desktop for Mac.

{% comment %}
The gateway is also reachable as `gateway.docker.internal`.
{% endcomment %}
The gateway is also reachable as `gateway.docker.internal`.

{% comment %}
#### I want to connect to a container from the Mac
{% endcomment %}
{: #i-want-to-connect-to-a-container-from-the-mac }
#### Mac からコンテナーに接続したい

{% comment %}
Port forwarding works for `localhost`; `--publish`, `-p`, or `-P` all work.
Ports exposed from Linux are forwarded to the host.
{% endcomment %}
Port forwarding works for `localhost`; `--publish`, `-p`, or `-P` all work.
Ports exposed from Linux are forwarded to the host.

{% comment %}
Our current recommendation is to publish a port, or to connect from another
container. This is what you need to do even on Linux if the container is on an
overlay network, not a bridge network, as these are not routed.
{% endcomment %}
Our current recommendation is to publish a port, or to connect from another
container. This is what you need to do even on Linux if the container is on an
overlay network, not a bridge network, as these are not routed.

{% comment %}
The command to run the `nginx` webserver shown in [Getting Started](index.md#explore-the-application)
is an example of this.
{% endcomment %}
The command to run the `nginx` webserver shown in [Getting Started](index.md#explore-the-application)
is an example of this.

```bash
$ docker run -d -p 80:80 --name webserver nginx
```

{% comment %}
To clarify the syntax, the following two commands both expose port `80` on the
container to port `8000` on the host:
{% endcomment %}
To clarify the syntax, the following two commands both expose port `80` on the
container to port `8000` on the host:

```bash
$ docker run --publish 8000:80 --name webserver nginx

$ docker run -p 8000:80 --name webserver nginx
```

{% comment %}
To expose all ports, use the `-P` flag. For example, the following command
starts a container (in detached mode) and the `-P` exposes all ports on the
container to random ports on the host.
{% endcomment %}
To expose all ports, use the `-P` flag. For example, the following command
starts a container (in detached mode) and the `-P` exposes all ports on the
container to random ports on the host.

```bash
$ docker run -d -P --name webserver nginx
```

{% comment %}
See the [run command](../engine/reference/commandline/run.md) for more details on
publish options used with `docker run`.
{% endcomment %}
See the [run command](../engine/reference/commandline/run.md) for more details on
publish options used with `docker run`.
