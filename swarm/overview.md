---
advisory: swarm-standalone
hide_from_sitemap: true
description: 'Swarm: Docker のネイティブなクラスタリングシステム'
keywords: docker, swarm, clustering
title: Docker Swarm 概要
---

{% comment %}
Docker Swarm is native clustering for Docker. It turns a pool of Docker hosts
into a single, virtual Docker host. Because Docker Swarm serves the standard
Docker API, any tool that already communicates with a Docker daemon can use
Swarm to transparently scale to multiple hosts. Supported tools include, but
are not limited to, the following:
{% endcomment %}
Docker Swarm は Docker のネイティブなクラスタリング機能です。
これは複数の Docker ホストを 1 つの Docker ホストへ仮想的に作り変えます。
Docker Swarm はスタンドアロンの Docker API を提供するので、Docker デーモンとのやり取りを行っているツール類は、何も気にせず複数のホストへ Swarm を拡張することができます。
サポートされるツールは以下のものです。
ただしこれだけに限定されるものではありません。

- Dokku
- Docker Compose
- Docker Machine
- Jenkins

{% comment %}
And of course, the Docker client itself is also supported.
{% endcomment %}
そして当然のことながら、Docker クライアントそのものもサポートされます。

{% comment %}
Like other Docker projects, Docker Swarm follows the "swap, plug, and play"
principle. As initial development settles, an API develops to enable
pluggable backends. This means you can swap out the scheduling backend
Docker Swarm uses out-of-the-box with a backend you prefer. Swarm's swappable design provides a smooth out-of-box experience for most use cases, and allows large-scale production deployments to swap for more powerful backends, like Mesos.
{% endcomment %}
他の Docker プロジェクトでも同じことですが、Docker Swarm は「交換、取り付け、実行」（swap, plug, and play）の原則に従っています。
初期開発からやがて API は、交換可能なバックエンドを有効にするように開発されています。
これはたとえば、Docker Swarm が利用するスケジュールバックエンドを、好みのバックエンドにすぐに切り替えられることを意味します。

Swarm's swappable design provides a smooth out-of-box experience for most use cases, and allows large-scale production deployments to swap for more powerful backends, like Mesos.

{% comment %}
## Understand Swarm cluster creation
{% endcomment %}
## Swarm クラスタ生成への理解
{: #understand-swarm-cluster-creation }

{% comment %}
The first step to creating a swarm cluster on your network is to pull the Docker Swarm image. Then, using Docker, you configure the swarm manager and all the nodes to run Docker Swarm. This method requires that you:
{% endcomment %}
The first step to creating a swarm cluster on your network is to pull the Docker Swarm image. Then, using Docker, you configure the swarm manager and all the nodes to run Docker Swarm. This method requires that you:

{% comment %}
* open a TCP port on each node for communication with the swarm manager
* install Docker on each node
* create and manage TLS certificates to secure your cluster
{% endcomment %}
* open a TCP port on each node for communication with the swarm manager
* install Docker on each node
* create and manage TLS certificates to secure your cluster

{% comment %}
Docker Swarm の
As a starting point, the manual method is best suited for experienced
administrators or programmers contributing to Docker Swarm. The alternative is
to use `docker-machine` to install a cluster.
{% endcomment %}
As a starting point, the manual method is best suited for experienced
administrators or programmers contributing to Docker Swarm. The alternative is
to use `docker-machine` to install a cluster.

{% comment %}
Using Docker Machine, you can quickly install a Docker Swarm on cloud providers
or inside your own data center. If you have VirtualBox installed on your local
machine, you can quickly build and explore Docker Swarm in your local
environment. This method automatically generates a certificate to secure your
cluster.
{% endcomment %}
Using Docker Machine, you can quickly install a Docker Swarm on cloud providers
or inside your own data center. If you have VirtualBox installed on your local
machine, you can quickly build and explore Docker Swarm in your local
environment. This method automatically generates a certificate to secure your
cluster.

{% comment %}
Using Docker Machine is the best method for users getting started with Swarm for the first time. To try the recommended method of getting started, see [Get Started with Docker Swarm](install-w-machine.md).
{% endcomment %}
Using Docker Machine is the best method for users getting started with Swarm for the first time. To try the recommended method of getting started, see [Get Started with Docker Swarm](install-w-machine.md).

{% comment %}
If you are interested in manually installing or interested in contributing, see [Build a swarm cluster for production](install-manual.md).
{% endcomment %}
If you are interested in manually installing or interested in contributing, see [Build a swarm cluster for production](install-manual.md).

{% comment %}
## Discovery services
{% endcomment %}
## Discovery services

{% comment %}
To dynamically configure and manage the services in your containers, you use a discovery backend with Docker Swarm. For information on which backends are available, see the [Discovery service](discovery.md) documentation.
{% endcomment %}
To dynamically configure and manage the services in your containers, you use a discovery backend with Docker Swarm. For information on which backends are available, see the [Discovery service](discovery.md) documentation.

{% comment %}
## Advanced scheduling
{% endcomment %}
## Advanced scheduling
{: #advanced-scheduling }

{% comment %}
To learn more about advanced scheduling, see the
[strategies](scheduler/strategy.md) and [filters](scheduler/filter.md)
documents.
{% endcomment %}
To learn more about advanced scheduling, see the
[strategies](scheduler/strategy.md) and [filters](scheduler/filter.md)
documents.

## Swarm API

{% comment %}
The [Docker Swarm API](swarm-api.md) is compatible with the
[Docker remote API](/engine/api/index.md), and extends it with some new
endpoints.
{% endcomment %}
The [Docker Swarm API](swarm-api.md) is compatible with the
[Docker remote API](/engine/api/index.md), and extends it with some new
endpoints.

{% comment %}
## Getting help
{% endcomment %}
## Getting help

{% comment %}
Docker Swarm is still in its infancy and under active development. If you need
help, would like to contribute, or simply want to talk about the project with
like-minded individuals, we have a number of open channels for communication.
{% endcomment %}
Docker Swarm is still in its infancy and under active development. If you need
help, would like to contribute, or simply want to talk about the project with
like-minded individuals, we have a number of open channels for communication.

{% comment %}
* To report bugs or file feature requests, use the [issue tracker on Github](https://github.com/docker/swarm/issues).
{% endcomment %}
* To report bugs or file feature requests, use the [issue tracker on Github](https://github.com/docker/swarm/issues).

{% comment %}
* To talk about the project with people in real time, join the `#docker-swarm` channel on IRC.
{% endcomment %}
* To talk about the project with people in real time, join the `#docker-swarm` channel on IRC.

{% comment %}
* To contribute code or documentation changes, submit a [pull request on Github](https://github.com/docker/swarm/pulls).
{% endcomment %}
* To contribute code or documentation changes, submit a [pull request on Github](https://github.com/docker/swarm/pulls).

{% comment %}
For more information and resources, visit the [Getting Help project page](/opensource/get-help/).
{% endcomment %}
For more information and resources, visit the [Getting Help project page](/opensource/get-help/).
