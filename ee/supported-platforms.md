---
title: Docker Enterprise について
description: Docker Enterprise 2.1 に関する情報
keywords: Docker Enterprise, enterprise, enterprise edition, ee, docker ee, docker enterprise edition, lts, commercial, cs engine, commercially supported
redirect_from:
  - /enterprise/supported-platforms/
  - /cs-engine/
  - /cs-engine/1.12/
  - /cs-engine/1.12/upgrade/
  - /cs-engine/1.13/
  - /cs-engine/1.13/upgrade/
green-check: '![yes](/install/images/green-check.svg){: style="height: 14px; margin:auto;"}'
install-prefix-ee: '/install/linux/docker-ee'
---

{% comment %}
Docker Enterprise is designed for enterprise development as well as IT teams who build, ship, and run business-critical
applications in production and at scale. Docker Enterprise is integrated, certified,
and supported to provide enterprises with the most secure container platform
in the industry. For more info about Docker Enterprise, including purchasing
options, see [Docker Enterprise](https://www.docker.com/enterprise-edition/).
{% endcomment %}
Docker Enterprise はエンタープライズ開発向けとして設計されています。
これは IT チームが開発する重要なビジネスアプリケーションを、相当規模の本番環境において構築、導入、実行するためのものです。
Docker Enterprise は
Docker Enterprise is integrated, certified,
and supported to provide enterprises with the most secure container platform
in the industry.
より詳細な Docker Enterprise の情報や、その購入オプションなどに関しては [Docker Enterprise](https://www.docker.com/enterprise-edition/) を参照してください。

{% comment %}
> Compatibility Matrix
>
> Refer to the [Compatibility Matrix](https://success.docker.com/article/compatibility-matrix) for the latest list of supported platforms.
{: .important}
{% endcomment %}
> 互換マトリックス
>
> サポート対応しているプラットフォームについての最新一覧は [互換マトリックス（Compatibility Matrix）](https://success.docker.com/article/compatibility-matrix) を参照してください。
{: .important}

{% comment %}
## Docker EE tiers
{% endcomment %}
## Docker EE の各階層
{: #docker-ee-tiers }

{% include docker_ce_ee.md %}

{% comment %}
> Note
>
> Starting with Docker Enterprise 2.1, Docker Enterprise --- Basic, Docker Enterprise --- Standard,
> and Docker Enterprise --- Advanced are all now called Docker Enterprise.
{% endcomment %}
> メモ
>
> Docker Enterprise 2.1 をはじめとする、Docker Enterprise --- Basic、Docker Enterprise --- Standard、Docker Enterprise --- Advanced は、現在はすべて Docker Enterprise と呼び表わしています。

{% comment %}
### Docker Enterprise
{% endcomment %}
### Docker Enterprise
{: #docker-enterprise }

{% comment %}
With Docker Enterprise, you can deploy Docker Engine --- Enterprise
to manage your container workloads in a flexible way. You can manage workloads
on Windows, Linux, on site, or on the cloud.
{% endcomment %}
Docker Enterprise を用いると、Docker Engine --- Enterprise をデプロイして、コンテナー作業をより柔軟に管理できるようになります。
同じ作業は Windows でも Linux でも行うことができ、さらには自サイト上でもクラウド上でも可能です。

{% comment %}
Docker Enterprise has private image management, integrated image signing policies, and cluster
management with support for Kubernetes and Swarm orchestrators. It allows you to implement
node-based RBAC policies, image promotion policies, image mirroring, and
scan your images for vulnerabilities. It also has support with defined SLAs and extended
maintenance cycles for patches for up to 24 months.
{% endcomment %}
Docker Enterprise にはプライベートなイメージ管理機能があり、イメージへの認証ポリシーの統合が可能です。
またクラスター管理においては、Kubernetes サポートや Swarm によるオーケストレーションもあります。
It allows you to implement
node-based RBAC policies, image promotion policies, image mirroring, and
scan your images for vulnerabilities. It also has support with defined SLAs and extended
maintenance cycles for patches for up to 24 months.

{% comment %}
### New Licensing for Docker Enterprise
{% endcomment %}
### New Licensing for Docker Enterprise
{: #new-licensing-for-docker-enterprise }

{% comment %}
In version 18.09, the Docker Enterprise --- Engine is aware of the license applied on the system. The
license summary is available in the `docker info` output on standalone or manager nodes.
{% endcomment %}
In version 18.09, the Docker Enterprise --- Engine is aware of the license applied on the system. The
license summary is available in the `docker info` output on standalone or manager nodes.

{% comment %}
For EE platform customers, when you license UCP, this same license is applied to the underlying
engines in the cluster. Docker recommends platform customers use UCP to manage their license.
{% endcomment %}
For EE platform customers, when you license UCP, this same license is applied to the underlying
engines in the cluster. Docker recommends platform customers use UCP to manage their license.

{% comment %}
Standalone EE engines can be licensed using `docker engine activate`.
{% endcomment %}
Standalone EE engines can be licensed using `docker engine activate`.

{% comment %}
Offline activation of standalone EE engines can be performed by downloading the license and
using the command `docker engine activate --license filename.lic`.
{% endcomment %}
Offline activation of standalone EE engines can be performed by downloading the license and
using the command `docker engine activate --license filename.lic`.

{% comment %}
Additionally, Docker is now distributing the CLI as a separate installation package.
This gives Enterprise users the ability to install as many CLI packages as needed
without using the Engine node licenses for client-only systems.
{% endcomment %}
Additionally, Docker is now distributing the CLI as a separate installation package.
This gives Enterprise users the ability to install as many CLI packages as needed
without using the Engine node licenses for client-only systems.

{% comment %}
[Learn more about Docker Enterprise](/ee/index.md).
{% endcomment %}
[Learn more about Docker Enterprise](/ee/index.md).


{% comment %}
> When using Docker Enterprise
>
> IBM Power is not supported as managers or workers.
> Microsoft Windows Server is not supported as a manager. Microsoft Windows
> Server 1803 is not supported as a worker.
{% endcomment %}
> When using Docker Enterprise
>
> IBM Power is not supported as managers or workers.
> Microsoft Windows Server is not supported as a manager. Microsoft Windows
> Server 1803 is not supported as a worker.

{% comment %}
### Docker Certified Infrastructure
{% endcomment %}
### Docker Certified Infrastructure
{: #docker-certified-infrastructure }

{% comment %}
Docker Certified Infrastructure is Docker’s prescriptive approach to deploying
Docker Enterprise on a range of infrastructure choices. Each Docker
Certified Infrastructure includes a reference architecture, automation templates,
and third-party ecosystem solution briefs.
{% endcomment %}
Docker Certified Infrastructure is Docker’s prescriptive approach to deploying
Docker Enterprise on a range of infrastructure choices. Each Docker
Certified Infrastructure includes a reference architecture, automation templates,
and third-party ecosystem solution briefs.

{% comment %}
| Platform                                                                                | Docker Enterprise Edition |
|:----------------------------------------------------------------------------------------|:-------------------------:|
| [VMware](https://success.docker.com/article/certified-infrastructures-vmware-vsphere)   |  {{ page.green-check }}   |
| [Amazon Web Services](https://success.docker.com/article/certified-infrastructures-aws) |  {{ page.green-check }}   |
| [Microsoft Azure](https://success.docker.com/article/certified-infrastructures-azure)   |  {{ page.green-check }}   |
| IBM Cloud                                                                               |        Coming soon        |
{% endcomment %}
| プラットフォーム                                                                        | Docker Enterprise エディション |
|:----------------------------------------------------------------------------------------|:------------------------------:|
| [VMware](https://success.docker.com/article/certified-infrastructures-vmware-vsphere)   |  {{ page.green-check }}        |
| [Amazon Web Services](https://success.docker.com/article/certified-infrastructures-aws) |  {{ page.green-check }}        |
| [Microsoft Azure](https://success.docker.com/article/certified-infrastructures-azure)   |  {{ page.green-check }}        |
| IBM Cloud                                                                               |        Coming soon             |


{% comment %}
## Docker Enterprise release cycles
{% endcomment %}
## Docker Enterprise のリリースサイクル
{: #docker-enterprise-release-cycles }

{% comment %}
Each Docker Enterprise release is supported and maintained for 24 months, and
receives security and critical bug fixes during this period.
{% endcomment %}
Docker Enterprise の各リリースは 24 ヶ月間、サポートおよび保守が行われます。
この期間内においては、セキュリティフィックスや重大なバグフィックスも受けることができます。

{% comment %}
The Docker API version is independent of the Docker platform version. We maintain
careful API backward compatibility and deprecate APIs and features slowly and
conservatively. We remove features after deprecating them for a period of
three stable releases. Docker 1.13 introduced improved interoperability
between clients and servers using different API versions, including dynamic
feature negotiation.
{% endcomment %}
Docker API のバージョンは Docker プラットフォームのバージョンとは独立しています。
API の保守においては注意深く後方互換性を維持し、API や機能の廃止に関しては慎重にゆっくりと行っています。
機能の削除は、廃止予定としてから後に、3 回の安定版リリースを経た上で行うものとしています。
Docker 1.13 introduced improved interoperability
between clients and servers using different API versions, including dynamic
feature negotiation.

{% comment %}
## Upgrades and support
{% endcomment %}
## アップグレードとサポート
{: #upgrades-and-support }

{% comment %}
If you're a Docker DDC or CS Engine customer, you don't need to upgrade to
Docker Enterprise to continue to get support. We will continue to support customers
with valid subscriptions whether the subscription covers Docker EE or
Commercially Supported Docker. You can choose to stay with your current
deployed version, or you can upgrade to the latest Docker EE version. For
more info, see [Scope of Coverage and Maintenance
Lifecycle](https://success.docker.com/Policies/Scope_of_Support).
{% endcomment %}
If you're a Docker DDC or CS Engine customer, you don't need to upgrade to
Docker Enterprise to continue to get support. We will continue to support customers
with valid subscriptions whether the subscription covers Docker EE or
Commercially Supported Docker. You can choose to stay with your current
deployed version, or you can upgrade to the latest Docker EE version. For
more info, see [Scope of Coverage and Maintenance
Lifecycle](https://success.docker.com/Policies/Scope_of_Support).

{% comment %}
## Where to go next
{% endcomment %}
## Where to go next
{: #where-to-go-next }

{% comment %}
- [Install Docker](/engine/installation/index.md)
- [Get Started with Docker](/get-started/index.md)
{% endcomment %}
- [Docker のインストール](/engine/installation/index.md)
- [Docker をはじめよう](/get-started/index.md)
