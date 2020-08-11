---
description: ネットワーク機能。
keywords: windows, networking
title: Docker Desktop for Windows のネットワーク機能
---
{% assign Arch = 'Windows' %}

{% comment %}
Docker Desktop provides several networking features to make it easier to
use.
{% endcomment %}
Docker Desktop では、より簡単に利用できるネットワーク機能をいくつも提供しています。

{% comment %}
## Features
{% endcomment %}
{: #features }
## 機能

{% comment %}
### VPN Passthrough
{% endcomment %}
{: #vpn-passthrough }
### VPN パススルー

{% comment %}
Docker Desktop networking can work when attached to a VPN. To do this,
Docker Desktop intercepts traffic from the containers and injects it into
{{Arch}} as if it originated from the Docker application.
{% endcomment %}
Docker Desktop のネットワークは VPN に接続して利用することができます。
このとき Docker Desktop は、コンテナーからのトラフィックを捕捉し {{Arch}} へ受け渡します。
まるで Docker アプリケーションから発信されたかのように扱います。

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
Docker Desktop makes whatever is running on port 80 in the container (in
this case, `nginx`) available on port 80 of `localhost`. In this example, the
host and container ports are the same. What if you need to specify a different
host port? If, for example, you already have something running on port 80 of
your host machine, you can connect the container to a different port:
{% endcomment %}
Docker Desktop は、コンテナー上のポート 80 を使って稼動するものすべて（この場合は `nginx`）、`localhost` のポート 80 から利用できるようにします。
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
Now, connections to `localhost:8000` are sent to port 80 in the container. The
syntax for `-p` is `HOST_PORT:CLIENT_PORT`.

{% comment %}
### HTTP/HTTPS Proxy Support
{% endcomment %}
{: #httphttps-proxy-support }
### HTTP/HTTPS プロキシーサポート

{% comment %}
See [Proxies](index.md#proxies).
{% endcomment %}
[Proxies タブ](index.md#proxies) を参照してください。

{% comment %}
## Known limitations, use cases, and workarounds
{% endcomment %}
{: #known-limitations-use-cases-and-workarounds }
## 既知の制約、利用状況、回避策

{% comment %}
Following is a summary of current limitations on the Docker Desktop for {{Arch}}
networking stack, along with some ideas for workarounds.
{% endcomment %}
以下では、Docker Desktop for {{Arch}} の現状のネットワーク機能における制約と回避方法についてまとめます。

{% comment %}
### There is no docker0 bridge on {{Arch}}
{% endcomment %}
{: #there-is-no-docker0-bridge-on-macos }
### {{Arch}} 上に docker0 ブリッジがない

{% comment %}
Because of the way networking is implemented in Docker Desktop for {{Arch}}, you cannot
see a `docker0` interface on the host.  This interface is actually within the
virtual machine.
{% endcomment %}
Docker Desktop for {{Arch}} におけるネットワーク機能の実装方法により、ホスト上から `docker0` インターフェースを見ることはできません。
このインターフェースは仮想マシン内にあります。

{% comment %}
### I cannot ping my containers
{% endcomment %}
{: #i-cannot-ping-my-containers }
### コンテナーに ping ができない

{% comment %}
Docker Desktop for Windows can't route traffic to Linux containers.  However, you can
ping the Windows containers.
{% endcomment %}
Docker Desktop for Windows が Linux コンテナーに対して、トラフィックをルーティングできません。
ただし Windows コンテナーであれば ping を通すことができます。

{% comment %}
### Per-container IP addressing is not possible
{% endcomment %}
{: #per-container-ip-addressing-is-not-possible }
### コンテナー単位の IP アドレス割り当てができない

{% comment %}
The docker (Linux) bridge network is not reachable from the Windows host.
However, it works with Windows containers.
{% endcomment %}
Docker の（Linux における）ブリッジネットワークが、Windows ホストからアクセスできません。
ただし Windows コンテナーでは動作します。

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
The host has a changing IP address (or none if you have no network access). We recommend that you connect to the special DNS name
`host.docker.internal` which resolves to the internal IP address used by the
host. This is for development purpose and will not work in a production environment outside of Docker Desktop for Windows.
{% endcomment %}
ホストには可変の IP アドレスがあります（もっともネットワークを利用しなければ何もありません）。
推奨されるのは、特別な DNS 名 `host.docker.internal` に接続することです。
この DNS は、ホストが利用する内部 IP アドレスを名前解決します。
これは開発環境において用いられるものであり、Docker Desktop for Windows の範囲外にある本番環境では動作しません。

{% comment %}
You can also reach the gateway using `gateway.docker.internal`.
{% endcomment %}
ゲートウェイも `gateway.docker.internal` を利用してアクセス可能です。

{% comment %}
If you have installed Python on your machine, use the following instructions as an example to connect from a container to a service on the host:
{% endcomment %}
マシン上に Python をインストールしている場合は、例として以下の手順に従い、コンテナーからホスト上のサービスに接続します。

{% comment %}
1. Run the following command to start a simple HTTP server on port 8000.
{% endcomment %}
1. 以下のコマンドを実行して、単純の HTTP サーバーをポート 8000 で起動します。

    `python -m http.server 8000`

    {% comment %}
    If you have installed Python 2.x, run `python -m SimpleHTTPServer 8000`.
    {% endcomment %}
    Python 2.x をインストールしている場合は、`python -m SimpleHTTPServer 8000` を実行します。

{% comment %}
2. Now, run a container, install `curl`, and try to connect to the host using the following commands:
{% endcomment %}
2. そしてコンテナーの実行と `curl` のインストールを行って、以下のコマンドのようにホストへの接続を試してみます。

    ```console
    $ docker run --rm -it alpine sh
    # apk add curl
    # curl http://host.docker.internal:8000
    # exit
    ```

{% comment %}
#### I want to connect to a container from Windows
{% endcomment %}
{: #i-want-to-connect-to-a-container-from-the-windows }
#### Windows からコンテナーに接続したい

{% comment %}
Port forwarding works for `localhost`; `--publish`, `-p`, or `-P` all work.
Ports exposed from Linux are forwarded to the host.
{% endcomment %}
`localhost` に対するポート転送（port forwarding）が動作します。
つまり `--publish`、`-p`、`-P` はすべて動きます。
Linux コンテナーから公開されたポートは、ホストに転送されます。

{% comment %}
Our current recommendation is to publish a port, or to connect from another
container. This is what you need to do even on Linux if the container is on an
overlay network, not a bridge network, as these are not routed.
{% endcomment %}
今のところ推奨される方法は、ポートを公開するか、あるいはもう 1 つ別のコンテナーから接続することです。
Linux 上であっても、コンテナーがブリッジネットワーク上ではなく、オーバーレイネットワーク上にある場合には、こういった方法が必要になります。
その場合にはルートが解決されないからです。

{% comment %}
The command to run the `nginx` webserver shown in [Getting Started](index.md#explore-the-application)
is an example of this.
{% endcomment %}
以下は、[はじめよう](index.md#explore-the-application) の節に示している `nginx` ウェブサーバーの起動コマンドです。

```bash
$ docker run -d -p 80:80 --name webserver nginx
```

{% comment %}
To clarify the syntax, the following two commands both publish container's port `80` to host's port `8000`:
{% endcomment %}
文法を明確にする目的で、以下の 2 つのコマンドを実行します。
両方ともコンテナー上のポート `80` を、ホスト上のポート `8000` に向けて公開するものです。

```bash
$ docker run --publish 8000:80 --name webserver nginx

$ docker run -p 8000:80 --name webserver nginx
```

{% comment %}
To publish all ports, use the `-P` flag. For example, the following command
starts a container (in detached mode) and the `-P` flag publishes all exposed ports of the
container to random ports on the host.
{% endcomment %}
ポートをすべて公開するには `-P` フラグを使います。
たとえば以下のコマンドは（デタッチモードで）コンテナーを起動し、`-P` フラグによってコンテナー上の全ポートを、ホスト上のランダムなポートとして公開します。

```bash
$ docker run -d -P --name webserver nginx
```

{% comment %}
See the [run command](../engine/reference/commandline/run.md) for more details on
publish options used with `docker run`.
{% endcomment %}
`docker run` コマンドにて利用できる、ポート公開に関するオプションの詳細については [run コマンド](../engine/reference/commandline/run.md) を参照してください。