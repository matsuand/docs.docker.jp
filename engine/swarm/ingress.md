---
description: Use the routing mesh to publish services externally to a swarm
keywords: guide, swarm mode, swarm, network, ingress, routing mesh
title: Swarm モードにおけるルーティングメッシュの利用
---

{% comment %}
Docker Engine swarm mode makes it easy to publish ports for services to make
them available to resources outside the swarm. All nodes participate in an
ingress **routing mesh**. The routing mesh enables each node in the swarm to
accept connections on published ports for any service running in the swarm, even
if there's no task running on the node. The routing mesh routes all
incoming requests to published ports on available nodes to an active container.
{% endcomment %}
Docker Engine の Swarm モードでは、サービスに対してのポート公開が簡単にできるので、Swarm 外部にあるリソースがサービスに対してアクセスすることを可能にします。
ノードはすべて、ingress の **ルーティングメッシュ**（routing mesh）に参加します。
ルーティングメッシュがあることによって Swarm 内の各ノードは、同じく Swarm 内で稼動するどのようなサービスに対しても、公開ポートを通じて接続することができます。
たとえノード上にタスクが実行されていなくてもかまいません。
ルーティングメッシュは、利用可能ノードの公開ポートに入ってきたリクエストすべてを、アクティブなコンテナーにルーティングします。

{% comment %}
To use the ingress network in the swarm, you need to have the following
ports open between the swarm nodes before you enable swarm mode:
{% endcomment %}
Swarm において ingress ネットワークを利用するには、Swarm モードを有効にする前に、Swarm ノード間において以下のポートを開放しておく必要があります。

{% comment %}
* Port `7946` TCP/UDP for container network discovery.
* Port `4789` UDP for the container ingress network.
{% endcomment %}
* ポート `7946` TCP/UDP、コンテナーのネットワーク検出のため。
* ポート `4789` UDP、コンテナーの ingress ネットワークのため。

{% comment %}
You must also open the published port between the swarm nodes and any external
resources, such as an external load balancer, that require access to the port.
{% endcomment %}
これに加えて、たとえば外部のロードバランサーなどの外部リソースが、特定ポートへのアクセスを必要とする場合には、Swarm ノード間においてその公開ポートを開放しておくことも必要です。

{% comment %}
You can also [bypass the routing mesh](#bypass-the-routing-mesh) for a given
service.
{% endcomment %}
あるいは指定したサービスに対しては [ルーティングメッシュの無効化](#bypass-the-routing-mesh) を実施することもできます。

{% comment %}
## Publish a port for a service
{% endcomment %}
{: #publish-a-port-for-a-service }
## サービスにおけるポート公開

{% comment %}
Use the `--publish` flag to publish a port when you create a service. `target`
is used to specify the port inside the container, and `published` is used to
specify the port to bind on the routing mesh. If you leave off the `published`
port, a random high-numbered port is bound for each service task. You
need to inspect the task to determine the port.
{% endcomment %}
サービス生成時にポートを公開するには `--publish` フラグを利用します。
その際にはコンテナー内部のポート指定に `target` を用い、ルーティングメッシュ上に割り当てるポートの指定に `published` を用います。
`published` の指定がなかった場合は、各サービスタスクにおいてランダムに高位のポート番号が割り振られます。
ポート番号がどの番号に割り振られたかを知るには、タスクの確認が必要です。

{% comment %}
```bash
$ docker service create \
  --name <SERVICE-NAME> \
  --publish published=<PUBLISHED-PORT>,target=<CONTAINER-PORT> \
  <IMAGE>
```
{% endcomment %}
```bash
$ docker service create \
  --name <サービス名> \
  --publish published=<公開ポート>,target=<コンテナーポート> \
  <イメージ>
```

{% comment %}
> **Note**: The older form of this syntax is a colon-separated string, where
> the published port is first and the target port is second, such as
> `-p 8080:80`. The new syntax is preferred because it is easier to read and
> allows more flexibility.
{% endcomment %}
> **メモ**: 上のコマンドの古い書式として、コロン区切りの文字列を用いるものがあります。
> その場合、1 つめが公開ポート、2 つめがターゲットとなるポートとなり、たとえば `-p 8080:80` と指定します。
> 好ましいのは新たな書式です。
> その方が読みやすく、より柔軟性があるからです。

{% comment %}
The `<PUBLISHED-PORT>` is the port where the swarm makes the service available.
If you omit it, a random high-numbered port is bound.
The `<CONTAINER-PORT>` is the port where the container listens. This parameter
is required.
{% endcomment %}
`<公開ポート>` は、Swarm がサービスを利用可能とするポートです。
これを省略すると、高位のポート番号が割り振られます。
`<コンテナーポート>` は、コンテナーが待ち受けるポートです。
このパラメーターは必須です。

{% comment %}
For example, the following command publishes port 80 in the nginx container to
port 8080 for any node in the swarm:
{% endcomment %}
たとえば以下のコマンドは nginx コンテナー上のポート 80 を、Swarm 内の全ノード上のポート 8080 に向けて公開します。

```bash
$ docker service create \
  --name my-web \
  --publish published=8080,target=80 \
  --replicas 2 \
  nginx
```

{% comment %}
When you access port 8080 on any node, Docker routes your request to an active
container. On the swarm nodes themselves, port 8080 may not actually be bound,
but the routing mesh knows how to route the traffic and prevents any port
conflicts from happening.
{% endcomment %}
どのノードにおいてもポート 8080 へのアクセスが行われると、Docker はそのリクエストをアクティブコンテナーに転送します。
Swarm 内のノードそのものには、実際にはポート 8080 が割り振られていない場合もあります。
しかしルーティングメッシュは、トラフィックをどこに転送すべきかがわかっているので、ポートの競合は発生しません。

{% comment %}
The routing mesh listens on the published port for any IP address assigned to
the node. For externally routable IP addresses, the port is available from
outside the host. For all other IP addresses the access is only available from
within the host.
{% endcomment %}
ルーティングメッシュは、ノードに割り当てられているどのような IP アドレスに対しても、公開ポートを待ち受けます。
外部にルーティングできる IP アドレスの場合、そのポートはホスト外部から利用できます。
これ以外の IP アドレスの場合は、すべてホスト内部からしかアクセスできません。

{% comment %}
![service ingress image](images/ingress-routing-mesh.png)
{% endcomment %}
![ingress サービスのイメージ](images/ingress-routing-mesh.png)

{% comment %}
You can publish a port for an existing service using the following command:
{% endcomment %}
既存のサービスに対しては、以下のコマンドによってポートを公開することができます。

{% comment %}
```bash
$ docker service update \
  --publish-add published=<PUBLISHED-PORT>,target=<CONTAINER-PORT> \
  <SERVICE>
```
{% endcomment %}
```bash
$ docker service update \
  --publish-add published=<公開ポート>,target=<コンテナーポート> \
  <サービス>
```

{% comment %}
You can use `docker service inspect` to view the service's published port. For
instance:
{% endcomment %}
サービスの公開ポートの確認には `docker service inspect` を使います。
たとえば以下のとおりです。

{% raw %}
```bash
$ docker service inspect --format="{{json .Endpoint.Spec.Ports}}" my-web

[{"Protocol":"tcp","TargetPort":80,"PublishedPort":8080}]
```
{% endraw %}

{% comment %}
The output shows the `<CONTAINER-PORT>` (labeled `TargetPort`) from the containers and the
`<PUBLISHED-PORT>` (labeled `PublishedPort`) where nodes listen for requests for the service.
{% endcomment %}
上の出力結果では、コンテナー側に `<コンテナーポート>`（ラベル `TargetPort` の部分）、サービスへのリクエストを待ち受けるノード側に `<公開ポート>`（ラベル `PublishedPort` の部分）があるのがわかります。

{% comment %}
### Publish a port for TCP only or UDP only
{% endcomment %}
{: #publish-a-port-for-tcp-only-or-udp-only }
### TCP のみ、UDP のみのポート公開

{% comment %}
By default, when you publish a port, it is a TCP port. You can
specifically publish a UDP port instead of or in addition to a TCP port. When
you publish both TCP and UDP ports, If you omit the protocol specifier,
the port is published as a TCP port. If you use the longer syntax (recommended
  for Docker 1.13 and higher), set the `protocol` key to either `tcp` or `udp`.
{% endcomment %}
ポートを公開すると、デフォルトでは TCP ポートとなります。
このかわりに、明示的に UDP ポートを指定するか、TCP ポートに UDP ポートを加えた指定とすることができます。
TCP と UDP の両方を公開するとします。
プロトコルの指定を省略してしまうと、TCP ポートとしてしか公開されません。
長い文法（Docker 1.13 またはそれ以降において推奨される）を使って、`protocol` キーに `tcp` または `udp` を設定します。

{% comment %}
#### TCP only
{% endcomment %}
{: #tcp-only }
#### TCP のみの指定

{% comment %}
**Long syntax:**
{% endcomment %}
**長い文法の場合**

```bash
$ docker service create --name dns-cache \
  --publish published=53,target=53 \
  dns-cache
```

{% comment %}
**Short syntax:**
{% endcomment %}
**短い文法の場合**

```bash
$ docker service create --name dns-cache \
  -p 53:53 \
  dns-cache
```

{% comment %}
#### TCP and UDP
{% endcomment %}
{: #tcp-and-udp }
#### TCP と UDP の指定

{% comment %}
**Long syntax:**
{% endcomment %}
**長い文法の場合**

```bash
$ docker service create --name dns-cache \
  --publish published=53,target=53 \
  --publish published=53,target=53,protocol=udp \
  dns-cache
```

{% comment %}
**Short syntax:**
{% endcomment %}
**短い文法の場合**

```bash
$ docker service create --name dns-cache \
  -p 53:53 \
  -p 53:53/udp \
  dns-cache
```

{% comment %}
#### UDP only
{% endcomment %}
{: #udp-only }
#### UDP のみの指定

{% comment %}
**Long syntax:**
{% endcomment %}
**長い文法の場合**

```bash
$ docker service create --name dns-cache \
  --publish published=53,target=53,protocol=udp \
  dns-cache
```

{% comment %}
**Short syntax:**
{% endcomment %}
**短い文法の場合**

```bash
$ docker service create --name dns-cache \
  -p 53:53/udp \
  dns-cache
```

{% comment %}
## Bypass the routing mesh
{% endcomment %}
{: #bypass-the-routing-mesh }
## ルーティングメッシュの無効化

{% comment %}
You can bypass the routing mesh, so that when you access the bound port on a
given node, you are always accessing the instance of the service running on
that node. This is referred to as `host` mode. There are a few things to keep
in mind.
{% endcomment %}
ルーティングメッシュは無効化することができます。
無効化した場合、特定のノード上に割り当てられているポートにアクセスすると、常にそのノード上に稼動しているサービスインスタンスにアクセスすることになります。
これは `host` モードと呼ばれます。
この場合には注意しておくべき点がいくつかあります。

{% comment %}
- If you access a node which is not running a service task, the service does not
  listen on that port. It is possible that nothing is listening, or
  that a completely different application is listening.
{% endcomment %}
- アクセスしたノードにサービスタスクが稼動していない場合、そのポートは待ち受けされていないことになります。
  この場合、そのポートが待ち受けされていないか、あるいはまったく別のアプリケーションがそのポートを待ち受けている可能性があります。

{% comment %}
- If you expect to run multiple service tasks on each node (such as when you
  have 5 nodes but run 10 replicas), you cannot specify a static target port.
  Either allow Docker to assign a random high-numbered port (by leaving off the
  `published`), or ensure that only a single instance of the service runs on a
  given node, by using a global service rather than a replicated one, or by
  using placement constraints.
{% endcomment %}
- 各ノードにおいて複数のサービスタスクを実行したい場合（たとえば 5 つのノードに対して 10 個のレプリカを実行させたい場合）、対象ポートを固定することはできません。
  この場合には、ランダムな高位のポート番号を割り当てるようにする（`published` を指定しない）か、または対象ノード上では単一のサービスインスタンスのみが稼動しているようにするかのいずれかを行います。
  つまり、サービスのレプリカは行わずにグローバルとするか、あるいはノード配置に関する制約（placement constraint）を利用します。

{% comment %}
To bypass the routing mesh, you must use the long `--publish` service and
set `mode` to `host`. If you omit the `mode` key or set it to `ingress`, the
routing mesh is used. The following command creates a global service using
`host` mode and bypassing the routing mesh.
{% endcomment %}
ルーティングメッシュを無効化するには、サービス生成時に長い文法による `--publish` を使って、`mode` を `host` に設定する必要があります。
`mode` キーを省略するか、`ingress` に設定した場合、ルーティングメッシュが有効になります。
以下のコマンドでは、`host` モードを使ってグローバルなサービスを生成し、ルーティングメッシュは無効化します。

```bash
$ docker service create --name dns-cache \
  --publish published=53,target=53,protocol=udp,mode=host \
  --mode global \
  dns-cache
```

{% comment %}
## Configure an external load balancer
{% endcomment %}
{: #configure-an-external-load-balancer }
## 外部ロードバランサーの設定

{% comment %}
You can configure an external load balancer for swarm services, either in
combination with the routing mesh or without using the routing mesh at all.
{% endcomment %}
Swarm サービスに対して、外部のロードバランサーを設定することができます。
その場合には、ルーティングメッシュと組み合わせる方法と、ルーティングメッシュを利用しない方法があります。

{% comment %}
### Using the routing mesh
{% endcomment %}
{: #using-the-routing-mesh }
### ルーティングメッシュを利用する方法

{% comment %}
You can configure an external load balancer to route requests to a swarm
service. For example, you could configure [HAProxy](http://www.haproxy.org) to
balance requests to an nginx service published to port 8080.
{% endcomment %}
外部のロードバランサーを利用して Swarm サービスに対するリクエストの転送設定を行うことができます。
たとえば [HAProxy](http://www.haproxy.org) を用いた設定により、nginx サービスの公開ポートを 8080 へのリクエストとして分散することができます。

{% comment %}
![ingress with external load balancer image](images/ingress-lb.png)
{% endcomment %}
![外部ロードバランサーを使った ingress イメージ](images/ingress-lb.png)

{% comment %}
In this case, port 8080 must be open between the load balancer and the nodes in
the swarm. The swarm nodes can reside on a private network that is accessible to
the proxy server, but that is not publicly accessible.
{% endcomment %}
この場合ロードバランサーと Swarm 内ノード間において、ポート 8080 が解放されている必要があります。
Swarm ノードは、プロキシーサーバーにアクセスできるのであれば、プライベートネットワーク内に置くことができます。
ただし外部からアクセスすることはできません。

{% comment %}
You can configure the load balancer to balance requests between every node in
the swarm even if there are no tasks scheduled on the node. For example, you
could have the following HAProxy configuration in `/etc/haproxy/haproxy.cfg`:
{% endcomment %}
ロードバランサーによって Swarm ノード間にリクエストを分散する際には、タスクがスケジューリングされていないノードであってもかまいません。
たとえば以下のように `/etc/haproxy/haproxy.cfg` に HAProxy 設定を行うことができます。

{% comment %}
```bash
global
        log /dev/log    local0
        log /dev/log    local1 notice
...snip...

# Configure HAProxy to listen on port 80
frontend http_front
   bind *:80
   stats uri /haproxy?stats
   default_backend http_back

# Configure HAProxy to route requests to swarm nodes on port 8080
backend http_back
   balance roundrobin
   server node1 192.168.99.100:8080 check
   server node2 192.168.99.101:8080 check
   server node3 192.168.99.102:8080 check
```
{% endcomment %}
```bash
global
        log /dev/log    local0
        log /dev/log    local1 notice
...省略...

# HAProxy がポート 80 を待ち受ける設定
frontend http_front
   bind *:80
   stats uri /haproxy?stats
   default_backend http_back

# HAProxy がリクエストを Swarm ノードのポート 8080 にルーティングする設定
backend http_back
   balance roundrobin
   server node1 192.168.99.100:8080 check
   server node2 192.168.99.101:8080 check
   server node3 192.168.99.102:8080 check
```

{% comment %}
When you access the HAProxy load balancer on port 80, it forwards requests to
nodes in the swarm. The swarm routing mesh routes the request to an active task.
If, for any reason the swarm scheduler dispatches tasks to different nodes, you
don't need to reconfigure the load balancer.
{% endcomment %}
HAProxy ロードバランサーに対してポート 80 でアクセスすると、Swarm 内のポートにリクエストが送信されます。
Swarm のルーティングメッシュは、そのリクエストをアクティブタスクに対して転送します。
このとき、何らかの理由により Swarm スケジューラーが、別のノードにタスクを移動させたとしても、ロードバランサーを再設定する必要はありません。

{% comment %}
You can configure any type of load balancer to route requests to swarm nodes.
To learn more about HAProxy, see the [HAProxy documentation](https://cbonte.github.io/haproxy-dconv/).
{% endcomment %}
Swarm ノードへのリクエストを送信するロードバランサーは、どのような種類のものでも設定できます。
HAProxy に関する詳細は [HAProxy のドキュメント](https://cbonte.github.io/haproxy-dconv/) を参照してください。

{% comment %}
### Without the routing mesh
{% endcomment %}
{: #without-the-routing-mesh }
### ルーティングメッシュを利用しない方法

{% comment %}
To use an external load balancer without the routing mesh, set `--endpoint-mode`
to `dnsrr` instead of the default value of `vip`. In this case, there is not a
single virtual IP. Instead, Docker sets up DNS entries for the service such that
a DNS query for the service name returns a list of IP addresses, and the client
connects directly to one of these. You are responsible for providing the list of
IP addresses and ports to your load balancer. See
[Configure service discovery](networking.md#configure-service-discovery).
{% endcomment %}
ルーティングメッシュは利用せずに外部のロードバランサーを用いるには、`--endpoint-mode` に設定する値を、デフォルトの `vip` ではなく `dnsrr` にします。
この場合、単一の仮想 IP はありません。
その代わりに Docker は、サービスの DNS エントリーを作り出します。
そしてサービス名に対する DNS クエリーが IP アドレス一覧を返すので、クライアントはその中の 1 つに直接接続するようになります。
ロードバランサーには、その IP アドレス一覧とポートを設定することが必要になります。
[サービス検出の設定](networking.md#configure-service-discovery) を参照してください。

{% comment %}
## Learn more
{% endcomment %}
{: #learn-more }
## さらに詳しくは

{% comment %}
* [Deploy services to a swarm](services.md)
{% endcomment %}
* [Swarm へのサービスのデプロイ](services.md)
