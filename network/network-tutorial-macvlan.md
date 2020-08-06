---
title: Macvlan ネットワークのチュートリアル
description: Tutorials for networking using a macvlan bridge network and 802.1q trunk bridge network
keywords: networking, macvlan, 802.1q, standalone
---

{% comment %}
This series of tutorials deals with networking standalone containers which
connect to `macvlan` networks. In this type of network, the Docker host accepts
requests for multiple MAC addresses at its IP address, and routes those requests
to the appropriate container. For other networking topics, see the
[overview](index.md).
{% endcomment %}
ここに示すチュートリアルは、`macvlan` ネットワークに接続するスタンドアロンコンテナーのネットワークを扱います。
この種類のネットワークでは、Docker ホストは IP アドレスにおいて、複数の MAC アドレスへのリクエストを受けつけます。
そしてそのリクエストを適切なコンテナーにルーティングします。
これ以外のネットワークに関しては、[ネットワーク概要](index.md) を参照してください。

{% comment %}
## Goal
{% endcomment %}
{: #goal }
## 目指すこと

{% comment %}
The goal of these tutorials is to set up a bridged `macvlan` network and attach
a container to it, then set up an 802.1q trunked `macvlan` network and attach a
container to it.
{% endcomment %}
本チュートリアルの目標は、ブリッジネットワークである `macvlan` を設定し、そこにコンテナーをアタッチすることです。
その後に 802.1q によってトランク設定された `macvlan` ネットワークを設定し、そこにコンテナーをアタッチします。

{% comment %}
## Prerequisites
{% endcomment %}
{: #prerequisites }
## 前提条件

{% comment %}
- Most cloud providers block `macvlan` networking. You may need physical access
  to your networking equipment.
{% endcomment %}
- クラウドプロバイダーでは、ほとんどが `macvlan` ネットワークをブロックします。
  したがってネットワーク機器からは、物理アクセスが必要となる場合があります。

{% comment %}
- The `macvlan` networking driver only works on Linux hosts, and is not supported
  on Docker Desktop for Mac, Docker Desktop for Windows, or Docker EE for Windows Server.
{% endcomment %}
- `macvlan` 用のネットワークドライバーは Linux ホスト上でのみ動作します。
  一方、Docker Desktop for Mac、Docker Desktop for Windows、Docker EE for Windows Server ではサポートされていません。

{% comment %}
- You need at least version 3.9 of the Linux kernel, and version 4.0 or higher
  is recommended.
{% endcomment %}
- Linux カーネルは最低でも 3.9 が必要です。
  4.0 またはそれ以降が推奨されます。

{% comment %}
- The examples assume your ethernet interface is `eth0`. If your device has a
  different name, use that instead.
{% endcomment %}
- 本例において、イーサネットインターフェースは `eth0` であるとします。
  利用するデバイス名が異なる場合は、その名前を用いてください。

{% comment %}
## Bridge example
{% endcomment %}
{: #bridge-example }
## ブリッジ例

{% comment %}
In the simple bridge example, your traffic flows through `eth0` and Docker
routes traffic to your container using its MAC address. To network devices
on your network, your container appears to be physically attached to the network.
{% endcomment %}
この単純なブリッジ例では、トラフィックは `eth0` を通じて送受信されます。
そして Docker はコンテナーへのトラフィックを、MAC アドレスを使ってルーティングします。
ネットワーク上のデバイスにとって、コンテナーは物理的にネットワークにアタッチされたものとして見えます。

{% comment %}
1.  Create a `macvlan` network called `my-macvlan-net`. Modify the `subnet`, `gateway`,
    and `parent` values to values that make sense in your environment.
{% endcomment %}
1.  `my-macvlan-net` という名前の `macvlan` ネットワークを生成します。
    `subnet`、`gateway`、`parent` の値は、利用環境に合わせて修正してください。

    ```bash
    $ docker network create -d macvlan \
      --subnet=172.16.86.0/24 \
      --gateway=172.16.86.1 \
      -o parent=eth0 \
      my-macvlan-net
    ```

    {% comment %}
    You can use `docker network ls` and `docker network inspect my-macvlan-net`
    commands to verify that the network exists and is a `macvlan` network.
    {% endcomment %}
    `docker network ls` や `docker network inspect my-macvlan-net` を実行すれば、ネットワークが確認でき、それが `macvlan` ネットワークであることがわかります。

{% comment %}
2.  Start an `alpine` container and attach it to the `my-macvlan-net` network. The
    `-dit` flags start the container in the background but allow you to attach
    to it. The `--rm` flag means the container is removed when it is stopped.
{% endcomment %}
2.  `alpine` コンテナーを起動し `my-macvlan-net` ネットワークにアタッチします。
    `-dit` フラグは、コンテナーをバックグラウンドで起動します。
    そしてコンテナーへのアクセスを可能とします。
    `--rm` フラグは、コンテナーが停止されたときに削除するためのものです。

    ```bash
    $ docker run --rm -dit \
      --network my-macvlan-net \
      --name my-macvlan-alpine \
      alpine:latest \
      ash
    ```

{% comment %}
3.  Inspect the `my-macvlan-alpine` container and notice the `MacAddress` key
    within the `Networks` key:
{% endcomment %}
3.  `my-macvlan-alpine` コンテナーを確認します。
    `Networks` キーの中に `MacAddress` キーがあるのがわかります。

    ```none
    $ docker container inspect my-macvlan-alpine

    ...省略...
    "Networks": {
      "my-macvlan-net": {
          "IPAMConfig": null,
          "Links": null,
          "Aliases": [
              "bec64291cd4c"
          ],
          "NetworkID": "5e3ec79625d388dbcc03dcf4a6dc4548644eb99d58864cf8eee2252dcfc0cc9f",
          "EndpointID": "8caf93c862b22f379b60515975acf96f7b54b7cf0ba0fb4a33cf18ae9e5c1d89",
          "Gateway": "172.16.86.1",
          "IPAddress": "172.16.86.2",
          "IPPrefixLen": 24,
          "IPv6Gateway": "",
          "GlobalIPv6Address": "",
          "GlobalIPv6PrefixLen": 0,
          "MacAddress": "02:42:ac:10:56:02",
          "DriverOpts": null
      }
    }
    ...省略
    ```

{% comment %}
4.  Check out how the container sees its own network interfaces by running a
    couple of `docker exec` commands.
{% endcomment %}
4.  コンテナーからネットワークインターフェースがどのように見えているかを確認します。
    以下のように `docker exec` コマンドを何回か実行します。

    ```bash
    $ docker exec my-macvlan-alpine ip addr show eth0

    9: eth0@tunl0: <BROADCAST,MULTICAST,UP,LOWER_UP,M-DOWN> mtu 1500 qdisc noqueue state UP
    link/ether 02:42:ac:10:56:02 brd ff:ff:ff:ff:ff:ff
    inet 172.16.86.2/24 brd 172.16.86.255 scope global eth0
       valid_lft forever preferred_lft forever
    ```

    ```bash
    $ docker exec my-macvlan-alpine ip route

    default via 172.16.86.1 dev eth0
    172.16.86.0/24 dev eth0 scope link  src 172.16.86.2
    ```

{% comment %}
5.  Stop the container (Docker removes it because of the `--rm` flag), and remove
    the network.
{% endcomment %}
5.  コンテナーを停止します（`--rm` フラグを用いていたため Docker はコンテナーを削除します）。
    そしてネットワークを停止します。

    ```bash
    $ docker container stop my-macvlan-alpine

    $ docker network rm my-macvlan-net
    ```

{% comment %}
## 802.1q trunked bridge example
{% endcomment %}
{: #8021q-trunked-bridge-example }
## 802.1q トランク設定によるブリッジ例

{% comment %}
In the 802.1q trunked bridge example, your traffic flows through a sub-interface
of `eth0` (called `eth0.10`) and Docker routes traffic to your container using
its MAC address. To network devices on your network, your container appears to
be physically attached to the network.
{% endcomment %}
802.1q トランク設定によるブリッジ例では、`eth0` のサブインターフェース（`eth0.10`）を通じてトラフィックがやりとりされます。
そして Docker はコンテナーへのトラフィックを、MAC アドレスを使ってルーティングします。
ネットワーク上のデバイスにとって、コンテナーは物理的にネットワークにアタッチされたものとして見えます。

{% comment %}
1.  Create a `macvlan` network called `my-8021q-macvlan-net`. Modify the
    `subnet`, `gateway`, and `parent` values to values that make sense in your
    environment.
{% endcomment %}
1.  `my-8021q-macvlan-net` という名前の `macvlan` ネットワークを生成します。
    `subnet`、`gateway`、`parent` の値は、利用環境に合わせて修正してください。

    ```bash
    $ docker network create -d macvlan \
      --subnet=172.16.86.0/24 \
      --gateway=172.16.86.1 \
      -o parent=eth0.10 \
      my-8021q-macvlan-net
    ```

    {% comment %}
    You can use `docker network ls` and `docker network inspect my-8021q-macvlan-net`
    commands to verify that the network exists, is a `macvlan` network, and
    has parent `eth0.10`. You can use `ip addr show` on the Docker host to
    verify that the interface `eth0.10` exists and has a separate IP address
    {% endcomment %}
    `docker network ls` や `docker network inspect my-macvlan-net` を実行すれば、ネットワークが確認でき、それが `macvlan` ネットワークであることがわかります。
    また親として `eth0.10` があることもわかります。
    Docker ホスト上で `ip addr show` を実行すると、インターフェース `eth0.10` が存在し、別の IP アドレスを持つことが確認できます。

{% comment %}
2.  Start an `alpine` container and attach it to the `my-8021q-macvlan-net`
    network. The `-dit` flags start the container in the background but allow
    you to attach to it. The `--rm` flag means the container is removed when it
    is stopped.
{% endcomment %}
2.  `alpine` コンテナーを起動し `my-8021q-macvlan-net` ネットワークにアタッチします。
    `-dit` フラグは、コンテナーをバックグラウンドで起動します。
    そしてコンテナーへのアクセスを可能とします。
    `--rm` フラグは、コンテナーが停止されたときに削除するためのものです。

    ```bash
    $ docker run --rm -itd \
      --network my-8021q-macvlan-net \
      --name my-second-macvlan-alpine \
      alpine:latest \
      ash
    ```

{% comment %}
3.  Inspect the `my-second-macvlan-alpine` container and notice the `MacAddress`
    key within the `Networks` key:
{% endcomment %}
3.  `my-second-macvlan-alpine` コンテナーを確認します。
    `Networks` キーの中に `MacAddress` キーがあるのがわかります。

    ```none
    $ docker container inspect my-second-macvlan-alpine

    ...省略...
    "Networks": {
      "my-8021q-macvlan-net": {
          "IPAMConfig": null,
          "Links": null,
          "Aliases": [
              "12f5c3c9ba5c"
          ],
          "NetworkID": "c6203997842e654dd5086abb1133b7e6df627784fec063afcbee5893b2bb64db",
          "EndpointID": "aa08d9aa2353c68e8d2ae0bf0e11ed426ea31ed0dd71c868d22ed0dcf9fc8ae6",
          "Gateway": "172.16.86.1",
          "IPAddress": "172.16.86.2",
          "IPPrefixLen": 24,
          "IPv6Gateway": "",
          "GlobalIPv6Address": "",
          "GlobalIPv6PrefixLen": 0,
          "MacAddress": "02:42:ac:10:56:02",
          "DriverOpts": null
      }
    }
    ...省略
    ```

{% comment %}
4.  Check out how the container sees its own network interfaces by running a
    couple of `docker exec` commands.
{% endcomment %}
4.  コンテナーからネットワークインターフェースがどのように見えているかを確認します。
    以下のように `docker exec` コマンドを何回か実行します。

    ```bash
    $ docker exec my-second-macvlan-alpine ip addr show eth0

    11: eth0@if10: <BROADCAST,MULTICAST,UP,LOWER_UP,M-DOWN> mtu 1500 qdisc noqueue state UP
    link/ether 02:42:ac:10:56:02 brd ff:ff:ff:ff:ff:ff
    inet 172.16.86.2/24 brd 172.16.86.255 scope global eth0
       valid_lft forever preferred_lft forever
    ```

    ```bash
    $ docker exec my-second-macvlan-alpine ip route

    default via 172.16.86.1 dev eth0
    172.16.86.0/24 dev eth0 scope link  src 172.16.86.2
    ```

{% comment %}
5.  Stop the container (Docker removes it because of the `--rm` flag), and remove
    the network.
{% endcomment %}
5.  コンテナーを停止します（`--rm` フラグを用いていたため Docker はコンテナーを削除します）。
    そしてネットワークを停止します。

    ```bash
    $ docker container stop my-second-macvlan-alpine

    $ docker network rm my-8021q-macvlan-net
    ```

{% comment %}
## Other networking tutorials
{% endcomment %}
{: #other-networking-tutorials }
## その他のネットワークチュートリアル

{% comment %}
Now that you have completed the networking tutorial for `macvlan` networks,
you might want to run through these other networking tutorials:
{% endcomment %}
`macvlan` ネットワークのチュートリアルをここに終えました。
以下に示すような別のネットワークチュートリアルも見てください。

{% comment %}
- [Standalone networking tutorial](network-tutorial-standalone.md)
- [Overlay networking tutorial](network-tutorial-overlay.md)
- [Host networking tutorial](network-tutorial-host.md)
{% endcomment %}
- [スタンドアロンネットワークのチュートリアル](network-tutorial-standalone.md)
- [オーバーレイネットワークのチュートリアル](network-tutorial-overlay.md)
- [ホストネットワークのチュートリアル](network-tutorial-host.md)

