---
title: ブリッジネットワークの利用
description: All about using user-defined bridge networks and the default bridge
keywords: network, bridge, user-defined, standalone
redirect_from:
- /engine/userguide/networking/default_network/custom-docker0/
- /engine/userguide/networking/default_network/dockerlinks/
- /engine/userguide/networking/default_network/build-bridges/
- /engine/userguide/networking/work-with-networks/
- /config/containers/bridges/
---

{% comment %}
In terms of networking, a bridge network is a Link Layer device
which forwards traffic between network segments. A bridge can be a hardware
device or a software device running within a host machine's kernel.
{% endcomment %}
ネットワーク技術において、ブリッジネットワークとはリンク層のデバイスのことであり、ネットワークセグメントに向けてトラフィックを送信します。
ブリッジはハードウェアデバイス、ソフトウェアデバイスのいずれも可能であり、ホストマシンのカーネル内で動作します。

{% comment %}
In terms of Docker, a bridge network uses a software bridge which allows
containers connected to the same bridge network to communicate, while providing
isolation from containers which are not connected to that bridge network. The
Docker bridge driver automatically installs rules in the host machine so that
containers on different bridge networks cannot communicate directly with each
other.
{% endcomment %}
Docker の用語におけるブリッジネットワークは、同一のブリッジネットワークに接続されたコンテナーが、互いに通信を行うためのソフトウェアブリッジを利用します。
そしてそのブリッジネットワークに接続していないコンテナーからは隔離されます。
ホストマシン内には Docker ブリッジドライバーのルールが自動的にインストールされます。
このルールにより、別のブリッジネットワーク上のコンテナーは、直接通信することはできなくなります。

{% comment %}
Bridge networks apply to containers running on the **same** Docker daemon host.
For communication among containers running on different Docker daemon hosts, you
can either manage routing at the OS level, or you can use an
[overlay network](overlay.md).
{% endcomment %}
ブリッジネットワークは、**同一** の Docker デーモンホスト上に稼動しているコンテナーに適用されます。
別の Docker デーモンホスト上に稼動するコンテナーとの間で通信を行うためには、OS レベルでのネットワーク管理を行うか、あるいは [オーバーレイネットワーク](overlay.md) を利用します。

{% comment %}
When you start Docker, a [default bridge network](#use-the-default-bridge-network) (also
called `bridge`) is created automatically, and newly-started containers connect
to it unless otherwise specified. You can also create user-defined custom bridge
networks. **User-defined bridge networks are superior to the default `bridge`
network.**
{% endcomment %}
Docker を起動すると [デフォルトブリッジネットワーク](#use-the-default-bridge-network)（または単にブリッジ）が自動的に生成されます。
そして特に指定がない限り、この後に生成されるコンテナーは、このネットワークに接続されます。
もちろんユーザー定義によるカスタムブリッジネットワークを生成することもできます。
**ユーザー定義によるブリッジネットワークは、デフォルトのブリッジネットワークよりも優先されます。**

{% comment %}
## Differences between user-defined bridges and the default bridge
{% endcomment %}
{: ### Differences between user-defined bridges and the default bridge }
## ユーザー定義ブリッジとデフォルトブリッジの違い

{% comment %}
- **User-defined bridges provide automatic DNS resolution between containers**.
{% endcomment %}
- **ユーザー定義ブリッジは、コンテナー間において自動的に DNS 解決を提供します。**

  {% comment %}
  Containers on the default bridge network can only access each other by IP
  addresses, unless you use the [`--link` option](links.md), which is
  considered legacy. On a user-defined bridge network, containers can resolve
  each other by name or alias.
  {% endcomment %}
  デフォルトブリッジネットワーク上にあるコンテナーは、IP アドレスによって互いにアクセスすることができます。
  ただしこれは [`--link` オプション](links.md) を使った場合であり、古い機能とされています。
  ユーザー定義のブリッジネットワークにおいて、コンテナーは名前またはエイリアスにより互いを識別します。

  {% comment %}
  Imagine an application with a web front-end and a database back-end. If you call
  your containers `web` and `db`, the web container can connect to the db container
  at `db`, no matter which Docker host the application stack is running on.
  {% endcomment %}
  今、フロントエンドにウェブ、バックエンドにデータベースを持つアプリケーションがあるとします。
  コンテナーをそれぞれ `web`、`db` とすると、web コンテナーから db コンテナーは `db` として接続します。
  そのアプリケーションスタックがどの Docker ホスト上で動作していても同様です。

  {% comment %}
  If you run the same application stack on the default bridge network, you need
  to manually create links between the containers (using the legacy `--link`
  flag). These links need to be created in both directions, so you can see this
  gets complex with more than two containers which need to communicate.
  Alternatively, you can manipulate the `/etc/hosts` files within the containers,
  but this creates problems that are difficult to debug.
  {% endcomment %}
  上と同じアプリケーションスタックを、デフォルトブリッジネットワーク上において実行したとします。
  その場合は（古い `--link` フラグを使い）コンテナー間に手動でリンクを生成する必要があります。
  このリンクは双方向に生成しなければなりません。
  したがって通信すべきコンテナーが 2 つ以上になってくると、このやり方は面倒なものに思うかもしれません。
  それならコンテナーすべてに `/etc/hosts` を設定する方法もありますが、今度はデバッグがしづらくなるという問題が発生します。

{% comment %}
- **User-defined bridges provide better isolation**.
{% endcomment %}
- **ユーザー定義ブリッジ上のコンテナーは、さらに隔離されます。**

  {% comment %}
  All containers without a `--network` specified, are attached to the default bridge network. This can be a risk, as unrelated stacks/services/containers are then able to communicate.
  {% endcomment %}
  `--network` の指定を一切行わなければ、すべてのコンテナーがデフォルトブリッジネットワークに接続されます。
  これにはリスクがあります。
  無関係なスタック、サービス、コンテナーが通信できてしまうからです。

  {% comment %}
  Using a user-defined network provides a scoped network in which only containers attached to that network are able to communicate.
  {% endcomment %}
  ユーザー定義ネットワークを使うとネットワーク範囲が限定され、そのネットワークに接続しているコンテナーだけが、互いに通信を行うことができます。

{% comment %}
- **Containers can be attached and detached from user-defined networks on the fly**.
{% endcomment %}
- **ユーザー定義ネットワークであれば、コンテナーの動作中にアタッチ、デタッチを行うことができます。**

  {% comment %}
  During a container's lifetime, you can connect or disconnect it from
  user-defined networks on the fly. To remove a container from the default
  bridge network, you need to stop the container and recreate it with different
  network options.
  {% endcomment %}
  コンテナーの稼働中であれば、ユーザー定義ネットワークとの接続や切断は、その場ですぐに行うことができます。
  一方デフォルトブリッジネットワークの場合、コンテナーをネットワークから切り離すには、コンテナーを停止させ、別のネットワークオプションを使ってコンテナーを再生成しなければなりません。

{% comment %}
- **Each user-defined network creates a configurable bridge**.
{% endcomment %}
- **ユーザー定義ネットワークは、設定変更可能なブリッジを生成します。**

  {% comment %}
  If your containers use the default bridge network, you can configure it, but
  all the containers use the same settings, such as MTU and `iptables` rules.
  In addition, configuring the default bridge network happens outside of Docker
  itself, and requires a restart of Docker.
  {% endcomment %}
  デフォルトブリッジネットワークを利用している場合に、ネットワーク設定を変更することはできますが、MTU や `iptables` ルールといった設定は全コンテナーに適用されることになります。
  さらにデフォルトブリッジネットワークの設定変更は Docker 外部で処理されるため、Docker デーモンの再起動が必要になります。

  {% comment %}
  User-defined bridge networks are created and configured using
  `docker network create`. If different groups of applications have different
  network requirements, you can configure each user-defined bridge separately,
  as you create it.
  {% endcomment %}
  ユーザー定義ブリッジネットワークは `docker network create` を使って生成され設定されます。
  アプリケーションがグループ分けされていて、ネットワーク要件が異なっているとします。
  ユーザー定義ブリッジは、個々に設定を変えて生成することができます。

{% comment %}
- **Linked containers on the default bridge network share environment variables**.
{% endcomment %}
- **デフォルトブリッジネットワーク上のリンクコンテナーは、環境変数を共有します。**

  {% comment %}
  Originally, the only way to share environment variables between two containers
  was to link them using the [`--link` flag](links.md). This type of
  variable sharing is not possible with user-defined networks. However, there
  are superior ways to share environment variables. A few ideas:
  {% endcomment %}
  もともと 2 つのコンテナー間で環境変数を共有するには、[`--link` フラグ](links.md) を使って互いをリンクするのが唯一の方法でした。
  この変数共有は、ユーザー定義ネットワークでは利用できません。
  ただし変数共有を行う優れた方法はあります。
  以下がその考え方です。

  {% comment %}
  - Multiple containers can mount a file or directory containing the shared
    information, using a Docker volume.
  {% endcomment %}
  - 複数コンテナーからはファイルやディレクトリをマウントすることができます。
    これには Docker ボリュームを利用します。
    そしてそこに共有したい情報を含めます。

  {% comment %}
  - Multiple containers can be started together using `docker-compose` and the
    compose file can define the shared variables.
  {% endcomment %}
  - 複数コンテナーは `docker-compose` を利用して同時に起動することができます。
    その compose ファイルには共有変数を定義することができます。

  {% comment %}
  - You can use swarm services instead of standalone containers, and take
    advantage of shared [secrets](../engine/swarm/secrets.md) and
    [configs](../engine/swarm/configs.md).
  {% endcomment %}
  - スタンドアロンなコンテナーではなく swarm サービスを利用することができます。
    これにより共有化した [secrets](../engine/swarm/secrets.md) や [configs](../engine/swarm/configs.md) を利用することができます。

{% comment %}
Containers connected to the same user-defined bridge network effectively expose all ports
to each other. For a port to be accessible to containers or non-Docker hosts on
different networks, that port must be _published_ using the `-p` or `--publish`
flag.
{% endcomment %}
同一のユーザー定義ブリッジネットワークに接続するコンテナーは、効率性により全ポートが互いに公開されます。
コンテナーへの接続ポート、あるいは別のネットワーク上の Docker でないホストへの接続ポートは、**公開されている** 必要があり、`-p` または `--publish` を指定します。

{% comment %}
## Manage a user-defined bridge
{% endcomment %}
{: #manage-a-user-defined-bridge }
## ユーザー定義ブリッジの管理

{% comment %}
Use the `docker network create` command to create a user-defined bridge
network.
{% endcomment %}
ユーザー定義ブリッジネットワークを生成するには、コマンド `docker network create` を実行します。
network.

```bash
$ docker network create my-net
```

{% comment %}
You can specify the subnet, the IP address range, the gateway, and other
options. See the
[docker network create](../engine/reference/commandline/network_create.md#specify-advanced-options)
reference or the output of `docker network create --help` for details.
{% endcomment %}
サブネット、IP アドレス範囲、ゲートウェイ、その他のオプションを指定することもできます。
詳しくは [docker network create](../engine/reference/commandline/network_create.md#specify-advanced-options) リファレンスを参照してください。
あるいは `docker network create --help` の出力を確認してください。

{% comment %}
{% endcomment %}
Use the `docker network rm` command to remove a user-defined bridge
network. If containers are currently connected to the network,
[disconnect them](#disconnect-a-container-from-a-user-defined-bridge)
first.

```bash
$ docker network rm my-net
```

{% comment %}
{% endcomment %}
> **What's really happening?**
>
> When you create or remove a user-defined bridge or connect or disconnect a
> container from a user-defined bridge, Docker uses tools specific to the
> operating system to manage the underlying network infrastructure (such as adding
> or removing bridge devices or configuring `iptables` rules on Linux). These
> details should be considered implementation details. Let Docker manage your
> user-defined networks for you.

{% comment %}
{% endcomment %}
## Connect a container to a user-defined bridge

{% comment %}
{% endcomment %}
When you create a new container, you can specify one or more `--network` flags.
This example connects a Nginx container to the `my-net` network. It also
publishes port 80 in the container to port 8080 on the Docker host, so external
clients can access that port. Any other container connected to the `my-net`
network has access to all ports on the `my-nginx` container, and vice versa.

```bash
$ docker create --name my-nginx \
  --network my-net \
  --publish 8080:80 \
  nginx:latest
```

{% comment %}
{% endcomment %}
To connect a **running** container to an existing user-defined bridge, use the
`docker network connect` command. The following command connects an already-running
`my-nginx` container to an already-existing `my-net` network:

```bash
$ docker network connect my-net my-nginx
```

{% comment %}
{% endcomment %}
## Disconnect a container from a user-defined bridge

{% comment %}
{% endcomment %}
To disconnect a running container from a user-defined bridge, use the `docker
network disconnect` command. The following command disconnects the `my-nginx`
container from the `my-net` network.

```bash
$ docker network disconnect my-net my-nginx
```

{% comment %}
{% endcomment %}
## Use IPv6

{% comment %}
{% endcomment %}
If you need IPv6 support for Docker containers, you need to
[enable the option](../config/daemon/ipv6.md) on the Docker daemon and reload its
configuration, before creating any IPv6 networks or assigning containers IPv6
addresses.

{% comment %}
{% endcomment %}
When you create your network, you can specify the `--ipv6` flag to enable
IPv6. You can't selectively disable IPv6 support on the default `bridge` network.

{% comment %}
{% endcomment %}
## Enable forwarding from Docker containers to the outside world

{% comment %}
{% endcomment %}
By default, traffic from containers connected to the default bridge network is
**not** forwarded to the outside world. To enable forwarding, you need to change
two settings. These are not Docker commands and they affect the Docker host's
kernel.

{% comment %}
{% endcomment %}
1.  Configure the Linux kernel to allow IP forwarding.

    ```bash
    $ sysctl net.ipv4.conf.all.forwarding=1
    ```

{% comment %}
{% endcomment %}
2.  Change the policy for the `iptables` `FORWARD` policy from `DROP` to
    `ACCEPT`.

    ```bash
    $ sudo iptables -P FORWARD ACCEPT
    ```

{% comment %}
{% endcomment %}
These settings do not persist across a reboot, so you may need to add them to a
start-up script.

{% comment %}
{% endcomment %}
## Use the default bridge network

{% comment %}
{% endcomment %}
The default `bridge` network is considered a legacy detail of Docker and is not
recommended for production use. Configuring it is a manual operation, and it has
[technical shortcomings](#differences-between-user-defined-bridges-and-the-default-bridge).

{% comment %}
{% endcomment %}
### Connect a container to the default bridge network

{% comment %}
{% endcomment %}
If you do not specify a network using the `--network` flag, and you do specify a
network driver, your container is connected to the default `bridge` network by
default. Containers connected to the default `bridge` network can communicate,
but only by IP address, unless they are linked using the
[legacy `--link` flag](links.md).

{% comment %}
{% endcomment %}
### Configure the default bridge network

{% comment %}
{% endcomment %}
To configure the default `bridge` network, you specify options in `daemon.json`.
Here is an example `daemon.json` with several options specified. Only specify
the settings you need to customize.

```json
{
  "bip": "192.168.1.5/24",
  "fixed-cidr": "192.168.1.5/25",
  "fixed-cidr-v6": "2001:db8::/64",
  "mtu": 1500,
  "default-gateway": "10.20.1.1",
  "default-gateway-v6": "2001:db8:abcd::89",
  "dns": ["10.20.1.2","10.20.1.3"]
}
```

{% comment %}
{% endcomment %}
Restart Docker for the changes to take effect.

{% comment %}
{% endcomment %}
### Use IPv6 with the default bridge network

{% comment %}
{% endcomment %}
If you configure Docker for IPv6 support (see [Use IPv6](#use-ipv6)), the
default bridge network is also configured for IPv6 automatically. Unlike
user-defined bridges, you can't selectively disable IPv6 on the default bridge.

{% comment %}
## Next steps
{% endcomment %}
{: #next-steps }
## 次のステップ

{% comment %}
{% endcomment %}
- Go through the [standalone networking tutorial](network-tutorial-standalone.md)
- Learn about [networking from the container's point of view](../config/containers/container-networking.md)
- Learn about [overlay networks](overlay.md)
- Learn about [Macvlan networks](macvlan.md)
