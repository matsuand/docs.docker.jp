---
title: Universal Control Plane 概要
description: |
  Docker Universal Control Plane、つまり Docker が提供するエンタープライズレベルのクラスター管理ソリューションについて学びます。
keywords: ucp, overview, orchestration, cluster
redirect_from:
  - /ucp/
---

>{% include enterprise_label_shortform.md %}

{% comment %}
Docker Universal Control Plane (UCP) is the enterprise-grade cluster management
solution from Docker. You install it on-premises or in your virtual private
cloud, and it helps you manage your Docker cluster and applications through a
single interface.
{% endcomment %}
Docker Universal Control Plane (UCP) は Docker が提供するエンタープライズレベルのクラスター管理ソリューションです。
これを自社サーバーやプライベートな仮想クラウドにインストールすることができます。
そして Docker クラスターやアプリケーションを、単一のインターフェースを通じて管理することができます。

![](images/v32dashboard.png){: .with-border}

{% comment %}
## Centralized cluster management
{% endcomment %}
{: #centralized-cluster-management }
## 集中クラスター管理

{% comment %}
With Docker, you can join up to thousands of physical or virtual machines
together to create a container cluster that allows you to deploy your
applications at scale. UCP extends the functionality provided by Docker to make it easier to manage your cluster from a centralized place.
{% endcomment %}
Docker を使えば、物理マシン、仮想マシンを問わず何千ものマシンを集約して、コンテナークラスターを実現することができます。
これによって相当規模のアプリケーションをデプロイできます。
UCP は Docker が提供する機能を拡張して、クラスターを一元管理する集中型システムを実現します。

{% comment %}
You can manage and monitor your container cluster using a graphical UI.
{% endcomment %}
コンテナークラスターの管理や監視は、グラフィック UI から操作することができます。

![](images/v32nodes.png){: .with-border}

{% comment %}
## Deploy, manage, and monitor
{% endcomment %}
{: #deploy-manage-and-monitor }
## デプロイ、管理、監視

{% comment %}
With UCP, you can manage from a centralized place all of the computing
resources you have available, like nodes, volumes, and networks.
{% endcomment %}
UCP を用いると、ノード、ボリューム、ネットワークといった利用可能なコンピューターリソースを集中して管理することができます。

{% comment %}
You can also deploy and monitor your applications and services.
{% endcomment %}
またアプリケーションやサービスのデプロイや監視も行うことができます。

{% comment %}
## Built-in security and access control
{% endcomment %}
{: #built-in-security-and-access-control }
## ビルトインのセキュリティ制御、アクセス制御

{% comment %}
UCP has its own built-in authentication mechanism and integrates with
LDAP services. It also has role-based access control (RBAC), so that you can
control who can access and make changes to your cluster and applications.
[Learn about role-based access control](authorization/index.md).
{% endcomment %}
UCP にはビルトインの認証メカニズムがあり、LDAP サービスと統合しています。
さらにロールベースアクセス制御（role-based access control; RBAC）も含んでいるため、クラスターやアプリケーションへのアクセスや変更を誰が行うことができるかを制御することができます。
[ロールベースアクセス制御を学ぶ](authorization/index.md) を参照してください。

![](images/v32users.png){: .with-border}

{% comment %}
UCP integrates with Docker Trusted Registry (DTR) so that you can keep the
Docker images you use for your applications behind your firewall, where they
are safe and can't be tampered with.
{% endcomment %}
UCP は Docker Trusted Registry (DTR) と統合することで、アプリケーションを実現している Docker イメージを、ファイアーウォール内に安全に改ざんされることなく運用することができます。

{% comment %}
You can also enforce security policies and only allow running applications
that use Docker images you know and trust.
{% endcomment %}
そしてセキュリティポリシーを適用して、信頼できる Docker イメージだけを使ってアプリケーションを実行することもできます。

{% comment %}
## Use the Docker CLI client
{% endcomment %}
{: #use-the-docker-cli-client }
## Docker CLI クライアントの利用

{% comment %}
Because UCP exposes the standard Docker API, you can continue using the tools
you already know, including the Docker CLI client, to deploy and manage your
applications.
{% endcomment %}
UCP は標準的な Docker API を公開しているので、Docker CLI クライアントのようにこれまでのツールを使って、アプリケーションのデプロイや管理を行うことができます。

{% comment %}
For example, you can use the `docker info` command to check the status of a
cluster that's managed by UCP:
{% endcomment %}
たとえば `docker info` コマンドを使うと、UCP により管理されているクラスターの状況を確認することができます。

```bash
docker info
```

{% comment %}
This command produces the output that you expect from Docker Enterprise:
{% endcomment %}
上のコマンドの実行により、Docker EE Engine からの期待どおりの出力が行われます。

```bash
Containers: 38
Running: 23
Paused: 0
Stopped: 15
Images: 17
Server Version: 17.06
...
Swarm: active
NodeID: ocpv7el0uz8g9q7dmw8ay4yps
Is Manager: true
ClusterID: tylpv1kxjtgoik2jnrg8pvkg6
Managers: 1
…
```

{% comment %}
## Where to go next
{% endcomment %}
{: #where-to-go-next }
## 次に読むものは

{% comment %}
- [Install UCP](admin/install/index.md)
- [Docker Enterprise architecture](../docker-ee-architecture.md)
{% endcomment %}
- [UCP のインストール](admin/install/index.md)
- [Docker Enterprise アーキテクチャー](../docker-ee-architecture.md)
