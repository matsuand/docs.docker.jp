---
title: UCP アーキテクチャー
description: Docker Universal Control Plane について学ぶ。
keywords: ucp, architecture
---

{% comment %}
Universal Control Plane is a containerized application that runs on
[Docker Enterprise Edition](/ee/index.md), extending its functionality
to simplify the deployment, configuration, and monitoring of your applications at scale.
{% endcomment %}
Universal Control Plane は [Docker Enterprise Edition](/ee/index.md) 上に稼動する、コンテナー化されたアプリケーションです。
Docker Enterprise Edition 機能の拡張によって、デプロイや設定を単純化し、アプリケーションの規模に応じて監視を行えるようにします。

{% comment %}
UCP also secures Docker with role-based access control so that only authorized
users can make changes and deploy applications to your Docker cluster.
{% endcomment %}
UCP はまたロールベースアクセス制御（role-based access control）により Docker を保護します。
Docker クラスターに対しては承認されたユーザーしか、アプリケーションへの変更とデプロイができないようにします。

![](images/ucp-architecture-1.svg){: .with-border}

{% comment %}
Once Universal Control Plane (UCP) instance is deployed, developers and IT
operations no longer interact with Docker Engine directly, but interact with
UCP instead. Since UCP exposes the standard Docker API, this is all done
transparently, so that you can use the tools you already know and love, like
the Docker CLI client and Docker Compose.
{% endcomment %}
Universal Control Plane (UCP) のインスタンスをデプロイすると、開発者や IT オペレーターは Docker Engine との直接の対話は行わない形になります。
それにかわって UCP とのやり取りとなります。
ただし UCP は標準的な Docker API を公開しているため、すべての操作はわかりやすいものになっています。
Docker CLI クライアントや Docker Compose などを通じて、すでに慣れ親しんできたツール類を利用していくことができます。


{% comment %}
## Under the hood
{% endcomment %}
{: #under-the-hood }
## Under the hood

{% comment %}
Docker UCP leverages the clustering and orchestration functionality provided
by Docker.
{% endcomment %}
Docker UCP は Docker が提供するクラスター機能やオーケストレーション機能を活用しています。

![](images/ucp-architecture-2.svg){: .with-border}

{% comment %}
A swarm is a collection of nodes that are in the same Docker cluster.
[Nodes](/engine/swarm/key-concepts.md) in a Docker swarm operate in one of two
modes: Manager or Worker. If nodes are not already running in a swarm when
installing UCP, nodes will be configured to run in swarm mode.
{% endcomment %}
スウォームというものはノードの集まりであって、そのノードはすべて同一の Docker クラスター内にあるものです。
Docker スウォーム内の [ノード](/engine/swarm/key-concepts.md) は 2 つあるモード、つまりマネージャーかワーカーのいずれかにより動作します。
UCP をインストールする際にノードがまだ起動していなかった場合は、ノードはスウォームモード内にて起動するように設定されます。

{% comment %}
When you deploy UCP, it starts running a globally scheduled service called
`ucp-agent`. This service monitors the node where it's running and starts
and stops UCP services, based on whether the node is a
[manager or a worker node](/engine/swarm/key-concepts.md).
{% endcomment %}
UCP をデプロイすると、グローバルなスケジュールサービス `ucp-agent` が起動します。
このサービスは起動中のノードを監視したり、UCP サービスの起動、停止を行ったりします。
このサービスは [マネージャーあるいはワーカーノード](/engine/swarm/key-concepts.md) において起動されます。

{% comment %}
If the node is a:
{% endcomment %}
ノード別に以下のようになります。

{% comment %}
* **Manager**: the `ucp-agent` service automatically starts serving all UCP
  components, including the UCP web UI and data stores used by UCP. The
  `ucp-agent` accomplishes this by
  [deploying several containers](#ucp-components-in-manager-nodes)
  on the node. By promoting a node to manager, UCP automatically becomes
  highly available and fault tolerant.
* **Worker**: on worker nodes, the `ucp-agent` service starts serving a proxy
  service that ensures only authorized users and other UCP services can run
  Docker commands in that node. The `ucp-agent` deploys a
  [subset of containers](#ucp-components-in-worker-nodes) on worker nodes.
{% endcomment %}
* **マネージャー**： `ucp-agent` サービスはすべての UCP コンポーネント提供を自動的に開始します。
  UCP の ウェブ UI 画面や UCP が利用するデータストアが含まれます。
  `ucp-agent` はこれらの機能を、ノード上の [複数コンテナーのデプロイ](#ucp-components-in-manager-nodes) によって実現します。
  ノードをマネージャーに格上げすると、UCP は自動的に高可用性と耐障害性を持つことになります。
* **ワーカー**： ワーカーノードの `ucp-agent` サービスは、プロキシーサービスの提供を開始します。
  そのノード上においては、承認されたユーザーと他の UCP サービスが Docker コマンドを実行できるようになります。
  `ucp-agent` は [コンテナーのサブセット](#ucp-components-in-worker-nodes) をワーカーノード上にデプロイします。

{% comment %}
## UCP internal components
{% endcomment %}
{: #ucp-internal-components }
## UCP 内部コマンド

{% comment %}
The core component of UCP is a globally scheduled service called `ucp-agent`.
When you install UCP on a node, or join a node to a swarm that's being managed
by UCP, the `ucp-agent` service starts running on that node.
{% endcomment %}
UCP コンポーネントの中心に位置するのが `ucp-agent` というグローバルなスケジュールサービスです。
ノード上に UCP をインストールするか、あるいはすでに UCP により管理されているスウォーム上にノードを追加すると、そのノード上に `ucp-agent` サービスが稼動し始めます。

{% comment %}
Once this service is running, it deploys containers with other UCP components,
and it ensures they keep running. The UCP components that are deployed
on a node depend on whether the node is a manager or a worker.
{% endcomment %}
このサービスが起動すると、コンテナー内に他の UCP コンポーネントをデプロイし、継続して起動させます。
ノード上にデプロイされる UCP コンポーネントは、そのノードがマネージャーなのかワーカーなのかによって変わります。

{% comment %}
> OS-specific component names
>
> Some UCP component names depend on the node's operating system. For example,
> on Windows, the `ucp-agent` component is named `ucp-agent-win`.
> [Learn about architecture-specific images](admin/install/architecture-specific-images.md).
{% endcomment %}
> OS 固有のコンポーネント名
>
> UCP コンポーネント名において、ノードのオペレーティングシステムに応じて命名されているものがあります。
> たとえば Windows においてコンポーネント `ucp-agent` は `ucp-agent-win` となっています。
> [アーキテクチャー固有のイメージについて学ぶ](admin/install/architecture-specific-images.md) を参照してください。

{% comment %}
### UCP components in manager nodes
{% endcomment %}
{: #ucp-components-in-manager-nodes }
### マネージャーノードにおける UCP コンポーネント

{% comment %}
Manager nodes run all UCP services, including the web UI and data stores that
persist the state of UCP. These are the UCP services running on manager nodes:
{% endcomment %}
マネージャーノードは UCP の全サービスを起動します。
ウェブ UI 画面があり、また UCP の状態を保持するデータストアがあります。
以下に示すものが、マネージャーノード上にて起動する UCP サービスです。

{% comment %}
| UCP component                   | Description                                                                                                                                                                                                                                                                                                                                                     |
|:--------------------------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| k8s_calico-kube-controllers     | A cluster-scoped Kubernetes controller used to coordinate Calico networking. Runs on one manager node only.                                                                                                                                                                                                                                                     |
| k8s_calico-node                 | The Calico node agent, which coordinates networking fabric according to the cluster-wide Calico configuration. Part of the `calico-node` daemonset. Runs on all nodes. Configure the container network interface (CNI) plugin by using the `--cni-installer-url` flag. If this flag isn't set, UCP uses Calico as the default CNI plugin.                                                     |
| k8s_install-cni_calico-node     | A container that's responsible for installing the Calico CNI plugin binaries and configuration on each host. Part of the `calico-node` daemonset. Runs on all nodes.                                                                                                                                                                                            |
| k8s_POD_calico-node             | Pause container for the `calico-node` pod.                                                                                                                                                                                                                                                                                                                      |
| k8s_POD_calico-kube-controllers | Pause container for the `calico-kube-controllers` pod.                                                                                                                                                                                                                                                                                                          |
| k8s_POD_compose                 | Pause container for the `compose` pod.                                                                                                                                                                                                                                                                                                                          |
| k8s_POD_kube-dns                | Pause container for the `kube-dns` pod.                                                                                                                                                                                                                                                                                                                         |
| k8s_ucp-dnsmasq-nanny           | A dnsmasq instance used in the Kubernetes DNS Service. Part of the `kube-dns` deployment. Runs on one manager node only.                                                                                                                                                                                                                                        |
| k8s_ucp-kube-compose            | A custom Kubernetes resource component that's responsible for translating Compose files into Kubernetes constructs. Part of the `compose` deployment. Runs on one manager node only.                                                                                                                                                                            |
| k8s_ucp-kube-dns                | The main Kubernetes DNS Service, used by pods to [resolve service names](https://v1-11.docs.kubernetes.io/docs/concepts/services-networking/dns-pod-service/). Part of the `kube-dns` deployment. Runs on one manager node only. Provides service discovery for Kubernetes services and pods. A set of three containers deployed via Kubernetes as a single pod. |
| k8s_ucp-kubedns-sidecar         | Health checking and metrics daemon of the Kubernetes DNS Service. Part of the `kube-dns` deployment. Runs on one manager node only.                                                                                                                                                                                                                             |
| ucp-agent                       | Monitors the node and ensures the right UCP services are running.                                                                                                                                                                                                                                                                                               |
| ucp-auth-api                    | The centralized service for identity and authentication used by UCP and DTR.                                                                                                                                                                                                                                                                                    |
| ucp-auth-store                  | Stores authentication configurations and data for users, organizations, and teams.                                                                                                                                                                                                                                                                              |
| ucp-auth-worker                 | Performs scheduled LDAP synchronizations and cleans authentication and authorization data.                                                                                                                                                                                                                                                                      |
| ucp-client-root-ca              | A certificate authority to sign client bundles.                                                                                                                                                                                                                                                                                                                 |
| ucp-cluster-root-ca             | A certificate authority used for TLS communication between UCP components.                                                                                                                                                                                                                                                                                      |
| ucp-controller                  | The UCP web server.                                                                                                                                                                                                                                                                                                                                             |
| ucp-dsinfo                      | Docker system information collection script to assist with troubleshooting.                                                                                                                                                                                                                                                                                     |
| ucp-interlock                   | Monitors swarm workloads configured to use Layer 7 routing. Only runs when you enable Layer 7 routing.                                                                                                                                                                                                                                                          |
| ucp-interlock-proxy             | A service that provides load balancing and proxying for swarm workloads. Only runs when you enable Layer 7 routing.                                                                                                                                                                                                                                             |
| ucp-kube-apiserver              | A master component that serves the Kubernetes API. It persists its state in `etcd` directly, and all other components communicate with API server directly.                                                                                                                                                                                                     |
| ucp-kube-controller-manager     | A master component that manages the desired state of controllers and other Kubernetes objects. It monitors the API server and performs background tasks when needed.                                                                                                                                                                                            |
| ucp-kubelet                     | The Kubernetes node agent running on every node, which is responsible for running Kubernetes pods, reporting the health of the node, and monitoring resource usage.                                                                                                                                                                                             |
| ucp-kube-proxy                  | The networking proxy running on every node, which enables pods to contact Kubernetes services and other pods, via cluster IP addresses.                                                                                                                                                                                                                         |
| ucp-kube-scheduler              | A master component that handles scheduling of pods. It communicates with the API server only to obtain workloads that need to be scheduled.                                                                                                                                                                                                                     |
| ucp-kv                          | Used to store the UCP configurations. Don't use it in your applications, since it's for internal use only. Also used by Kubernetes components.                                                                                                                                                                                                                  |
| ucp-metrics                     | Used to collect and process metrics for a node, like the disk space available.                                                                                                                                                                                                                                                                                  |
| ucp-proxy                       | A TLS proxy. It allows secure access to the local Docker Engine to UCP components.                                                                                                                                                                                                                                                                              |
| ucp-reconcile                   | When ucp-agent detects that the node is not running the right UCP components, it starts the ucp-reconcile container to converge the node to its desired state. It is expected for the ucp-reconcile container to remain in an exited state when the node is healthy.                                                                                            |
| ucp-swarm-manager               | Used to provide backwards-compatibility with Docker Swarm.                                                                                                                                                                                                                                                                                                      |
{% endcomment %}
| UCP コンポーネント              | 内容説明                                                                                                                                                                                                                                                                                                                                                        |
|:--------------------------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| k8s_calico-kube-controllers     | A cluster-scoped Kubernetes controller used to coordinate Calico networking. Runs on one manager node only.                                                                                                                                                                                                                                                     |
| k8s_calico-node                 | The Calico node agent, which coordinates networking fabric according to the cluster-wide Calico configuration. Part of the `calico-node` daemonset. Runs on all nodes. Configure the container network interface (CNI) plugin by using the `--cni-installer-url` flag. If this flag isn't set, UCP uses Calico as the default CNI plugin.                                                     |
| k8s_install-cni_calico-node     | A container that's responsible for installing the Calico CNI plugin binaries and configuration on each host. Part of the `calico-node` daemonset. Runs on all nodes.                                                                                                                                                                                            |
| k8s_POD_calico-node             | Pause container for the `calico-node` pod.                                                                                                                                                                                                                                                                                                                      |
| k8s_POD_calico-kube-controllers | Pause container for the `calico-kube-controllers` pod.                                                                                                                                                                                                                                                                                                          |
| k8s_POD_compose                 | Pause container for the `compose` pod.                                                                                                                                                                                                                                                                                                                          |
| k8s_POD_kube-dns                | Pause container for the `kube-dns` pod.                                                                                                                                                                                                                                                                                                                         |
| k8s_ucp-dnsmasq-nanny           | A dnsmasq instance used in the Kubernetes DNS Service. Part of the `kube-dns` deployment. Runs on one manager node only.                                                                                                                                                                                                                                        |
| k8s_ucp-kube-compose            | A custom Kubernetes resource component that's responsible for translating Compose files into Kubernetes constructs. Part of the `compose` deployment. Runs on one manager node only.                                                                                                                                                                            |
| k8s_ucp-kube-dns                | The main Kubernetes DNS Service, used by pods to [resolve service names](https://v1-11.docs.kubernetes.io/docs/concepts/services-networking/dns-pod-service/). Part of the `kube-dns` deployment. Runs on one manager node only. Provides service discovery for Kubernetes services and pods. A set of three containers deployed via Kubernetes as a single pod. |
| k8s_ucp-kubedns-sidecar         | Health checking and metrics daemon of the Kubernetes DNS Service. Part of the `kube-dns` deployment. Runs on one manager node only.                                                                                                                                                                                                                             |
| ucp-agent                       | Monitors the node and ensures the right UCP services are running.                                                                                                                                                                                                                                                                                               |
| ucp-auth-api                    | The centralized service for identity and authentication used by UCP and DTR.                                                                                                                                                                                                                                                                                    |
| ucp-auth-store                  | Stores authentication configurations and data for users, organizations, and teams.                                                                                                                                                                                                                                                                              |
| ucp-auth-worker                 | Performs scheduled LDAP synchronizations and cleans authentication and authorization data.                                                                                                                                                                                                                                                                      |
| ucp-client-root-ca              | A certificate authority to sign client bundles.                                                                                                                                                                                                                                                                                                                 |
| ucp-cluster-root-ca             | A certificate authority used for TLS communication between UCP components.                                                                                                                                                                                                                                                                                      |
| ucp-controller                  | The UCP web server.                                                                                                                                                                                                                                                                                                                                             |
| ucp-dsinfo                      | Docker system information collection script to assist with troubleshooting.                                                                                                                                                                                                                                                                                     |
| ucp-interlock                   | Monitors swarm workloads configured to use Layer 7 routing. Only runs when you enable Layer 7 routing.                                                                                                                                                                                                                                                          |
| ucp-interlock-proxy             | A service that provides load balancing and proxying for swarm workloads. Only runs when you enable Layer 7 routing.                                                                                                                                                                                                                                             |
| ucp-kube-apiserver              | A master component that serves the Kubernetes API. It persists its state in `etcd` directly, and all other components communicate with API server directly.                                                                                                                                                                                                     |
| ucp-kube-controller-manager     | A master component that manages the desired state of controllers and other Kubernetes objects. It monitors the API server and performs background tasks when needed.                                                                                                                                                                                            |
| ucp-kubelet                     | The Kubernetes node agent running on every node, which is responsible for running Kubernetes pods, reporting the health of the node, and monitoring resource usage.                                                                                                                                                                                             |
| ucp-kube-proxy                  | The networking proxy running on every node, which enables pods to contact Kubernetes services and other pods, via cluster IP addresses.                                                                                                                                                                                                                         |
| ucp-kube-scheduler              | A master component that handles scheduling of pods. It communicates with the API server only to obtain workloads that need to be scheduled.                                                                                                                                                                                                                     |
| ucp-kv                          | Used to store the UCP configurations. Don't use it in your applications, since it's for internal use only. Also used by Kubernetes components.                                                                                                                                                                                                                  |
| ucp-metrics                     | Used to collect and process metrics for a node, like the disk space available.                                                                                                                                                                                                                                                                                  |
| ucp-proxy                       | A TLS proxy. It allows secure access to the local Docker Engine to UCP components.                                                                                                                                                                                                                                                                              |
| ucp-reconcile                   | When ucp-agent detects that the node is not running the right UCP components, it starts the ucp-reconcile container to converge the node to its desired state. It is expected for the ucp-reconcile container to remain in an exited state when the node is healthy.                                                                                            |
| ucp-swarm-manager               | Used to provide backwards-compatibility with Docker Swarm.                                                                                                                                                                                                                                                                                                      |


{% comment %}
### UCP components in worker nodes
{% endcomment %}
{: #ucp-components-in-worker-nodes }
### ワーカーノードにおける UCP コンポーネント

{% comment %}
Worker nodes are the ones where you run your applications. These are the UCP
services running on worker nodes:
{% endcomment %}
Worker nodes are the ones where you run your applications. These are the UCP
services running on worker nodes:

{% comment %}
| UCP component               | Description                                                                                                                                                                                                                                                          |
|:----------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| k8s_calico-node             | The Calico node agent, which coordinates networking fabric according to the cluster-wide Calico configuration. Part of the `calico-node` daemonset. Runs on all nodes.                                                                                               |
| k8s_install-cni_calico-node | A container that's responsible for installing the Calico CNI plugin binaries and configuration on each host. Part of the `calico-node` daemonset. Runs on all nodes.                                                                                                 |
| k8s_POD_calico-node         | Pause container for the Calico-node pod. By default, this container is hidden, but you can see it by running `docker ps -a`.                                                                                                                                       |
| ucp-agent                   | Monitors the node and ensures the right UCP services are running                                                                                                                                                                                                     |
| ucp-interlock-extension     | Helper service that reconfigures the ucp-interlock-proxy service based on the swarm workloads that are running.                                                                                                                                                      |
| ucp-interlock-proxy         | A service that provides load balancing and proxying for swarm workloads. Only runs when you enable Layer 7 routing.                                                                                                                                                  |
| ucp-dsinfo                  | Docker system information collection script to assist with troubleshooting                                                                                                                                                                                           |
| ucp-kubelet                 | The kubernetes node agent running on every node, which is responsible for running Kubernetes pods, reporting the health of the node, and monitoring resource usage                                                                                                   |
| ucp-kube-proxy              | The networking proxy running on every node, which enables pods to contact Kubernetes services and other pods, via cluster IP addresses                                                                                                                               |
| ucp-reconcile               | When ucp-agent detects that the node is not running the right UCP components, it starts the ucp-reconcile container to converge the node to its desired state. It is expected for the ucp-reconcile container to remain in an exited state when the node is healthy. |
| ucp-proxy                   | A TLS proxy. It allows secure access to the local Docker Engine to UCP components                                                                                                                                                                                    |
{% endcomment %}
| UCP コンポーネント          | 内容説明                                                                                                                                                                                                                                                             |
|:----------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| k8s_calico-node             | The Calico node agent, which coordinates networking fabric according to the cluster-wide Calico configuration. Part of the `calico-node` daemonset. Runs on all nodes.                                                                                               |
| k8s_install-cni_calico-node | A container that's responsible for installing the Calico CNI plugin binaries and configuration on each host. Part of the `calico-node` daemonset. Runs on all nodes.                                                                                                 |
| k8s_POD_calico-node         | Pause container for the Calico-node pod. By default, this container is hidden, but you can see it by running `docker ps -a`.                                                                                                                                       |
| ucp-agent                   | Monitors the node and ensures the right UCP services are running                                                                                                                                                                                                     |
| ucp-interlock-extension     | Helper service that reconfigures the ucp-interlock-proxy service based on the swarm workloads that are running.                                                                                                                                                      |
| ucp-interlock-proxy         | A service that provides load balancing and proxying for swarm workloads. Only runs when you enable Layer 7 routing.                                                                                                                                                  |
| ucp-dsinfo                  | Docker system information collection script to assist with troubleshooting                                                                                                                                                                                           |
| ucp-kubelet                 | The kubernetes node agent running on every node, which is responsible for running Kubernetes pods, reporting the health of the node, and monitoring resource usage                                                                                                   |
| ucp-kube-proxy              | The networking proxy running on every node, which enables pods to contact Kubernetes services and other pods, via cluster IP addresses                                                                                                                               |
| ucp-reconcile               | When ucp-agent detects that the node is not running the right UCP components, it starts the ucp-reconcile container to converge the node to its desired state. It is expected for the ucp-reconcile container to remain in an exited state when the node is healthy. |
| ucp-proxy                   | A TLS proxy. It allows secure access to the local Docker Engine to UCP components                                                                                                                                                                                    |

{% comment %}
## Pause containers
{% endcomment %}
{: #pause-containers }
## Pause containers

{% comment %}
{% endcomment %}
Every pod in Kubernetes has a _pause_ container, which is an "empty" container
that bootstraps the pod to establish all of the namespaces. Pause containers
hold the cgroups, reservations, and namespaces of a pod before its individual
containers are created. The pause container's image is always present, so the
allocation of the pod's resources is instantaneous.

{% comment %}
{% endcomment %}
By default, pause containers are hidden, but you can see them by running
`docker ps -a`.

```
docker ps -a | grep -I pause

8c9707885bf6        dockereng/ucp-pause:3.0.0-6d332d3        "/pause"                 47 hours ago        Up 47 hours                                                                                               k8s_POD_calico-kube-controllers-559f6948dc-5c84l_kube-system_d00e5130-1bf4-11e8-b426-0242ac110011_0
258da23abbf5        dockereng/ucp-pause:3.0.0-6d332d3        "/pause"                 47 hours ago        Up 47 hours                                                                                               k8s_POD_kube-dns-6d46d84946-tqpzr_kube-system_d63acec6-1bf4-11e8-b426-0242ac110011_0
2e27b5d31a06        dockereng/ucp-pause:3.0.0-6d332d3        "/pause"                 47 hours ago        Up 47 hours                                                                                               k8s_POD_compose-698cf787f9-dxs29_kube-system_d5866b3c-1bf4-11e8-b426-0242ac110011_0
5d96dff73458        dockereng/ucp-pause:3.0.0-6d332d3        "/pause"                 47 hours ago        Up 47 hours                                                                                               k8s_POD_calico-node-4fjgv_kube-system_d043a0ea-1bf4-11e8-b426-0242ac110011_0
```

{% comment %}
{% endcomment %}
## Volumes used by UCP

{% comment %}
{% endcomment %}
Docker UCP uses these named volumes to persist data in all nodes where it runs:

{% comment %}
{% endcomment %}
| Volume name                 | Description                                                                              |
|:----------------------------|:-----------------------------------------------------------------------------------------|
| ucp-auth-api-certs          | Certificate and keys for the authentication and authorization service                    |
| ucp-auth-store-certs        | Certificate and keys for the authentication and authorization store                      |
| ucp-auth-store-data         | Data of the authentication and authorization store, replicated across managers           |
| ucp-auth-worker-certs       | Certificate and keys for authentication worker                                           |
| ucp-auth-worker-data        | Data of the authentication worker                                                        |
| ucp-client-root-ca          | Root key material for the UCP root CA that issues client certificates                    |
| ucp-cluster-root-ca         | Root key material for the UCP root CA that issues certificates for swarm members         |
| ucp-controller-client-certs | Certificate and keys used by the UCP web server to communicate with other UCP components |
| ucp-controller-server-certs | Certificate and keys for the UCP web server running in the node                          |
| ucp-kv                      | UCP configuration data, replicated across managers                                       |
| ucp-kv-certs                | Certificates and keys for the key-value store                                            |
| ucp-metrics-data            | Monitoring data gathered by UCP                                                          |
| ucp-metrics-inventory       | Configuration file used by the ucp-metrics service                                       |
| ucp-node-certs              | Certificate and keys for node communication                                              |


{% comment %}
{% endcomment %}
You can customize the volume driver used for these volumes, by creating
the volumes before installing UCP. During the installation, UCP checks which
volumes don't exist in the node, and creates them using the default volume
driver.

{% comment %}
{% endcomment %}
By default, the data for these volumes can be found at
`/var/lib/docker/volumes/<volume-name>/_data`.

{% comment %}
## Configurations used by UCP
{% endcomment %}
{: #configurations-used-by-ucp }
## UCP にて用いられる設定

{% comment %}
| Configuration name             | Description                                                                                      |
|:-------------------------------|:-------------------------------------------------------------------------------------------------|
| com.docker.interlock.extension | Configuration for the Interlock extension service that monitors and configures the proxy service |
| com.docker.interlock.proxy     | Configuration for the service responsible for handling user requests and routing them            |
| com.docker.license             | The Docker EE license                                                                            |
| com.docker.ucp.interlock.conf  | Configuration for the core Interlock service                                                     |
{% endcomment %}
| 設定名                         | 内容説明                                                                                         |
|:-------------------------------|:-------------------------------------------------------------------------------------------------|
| com.docker.interlock.extension | Configuration for the Interlock extension service that monitors and configures the proxy service |
| com.docker.interlock.proxy     | Configuration for the service responsible for handling user requests and routing them            |
| com.docker.license             | The Docker EE license                                                                            |
| com.docker.ucp.interlock.conf  | Configuration for the core Interlock service                                                     |

{% comment %}
## How you interact with UCP
{% endcomment %}
{: #how-you-interact-with-ucp }
## UCP の操作方法

{% comment %}
There are two ways to interact with UCP: the web UI or the CLI.
{% endcomment %}
UCP を操作する方法は 2 とおりあります。
ウェブ UI 画面と CLI です。

{% comment %}
You can use the UCP web UI to manage your swarm, grant and revoke user
permissions, deploy, configure, manage, and monitor your applications.
{% endcomment %}
You can use the UCP web UI to manage your swarm, grant and revoke user
permissions, deploy, configure, manage, and monitor your applications.

![](images/ucp-architecture-3.svg){: .with-border}

{% comment %}
UCP also exposes the standard Docker API, so you can continue using existing
tools like the Docker CLI client. Since UCP secures your cluster with role-based
access control, you need to configure your Docker CLI client and other client
tools to authenticate your requests using
[client certificates](user-access/index.md) that you can download
from your UCP profile page.
{% endcomment %}
UCP also exposes the standard Docker API, so you can continue using existing
tools like the Docker CLI client. Since UCP secures your cluster with role-based
access control, you need to configure your Docker CLI client and other client
tools to authenticate your requests using
[client certificates](user-access/index.md) that you can download
from your UCP profile page.

{% comment %}
## Where to go next
{% endcomment %}
{: #where-to-go-next }
## Where to go next

{% comment %}
- [System requirements](admin/install/system-requirements.md)
- [Plan your installation](admin/install/plan-installation.md)
{% endcomment %}
- [System requirements](admin/install/system-requirements.md)
- [Plan your installation](admin/install/plan-installation.md)
