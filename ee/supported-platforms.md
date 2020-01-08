---
title: 概要
description: Docker Enterprise の製品情報
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

>{% include enterprise_label_shortform.md %}

{% comment %}
Docker Enterprise is designed for enterprise development as well as IT teams who build, share, and run business-critical
applications at scale in production. Docker Enterprise is an integrated container platform that includes
Docker Desktop Enterprise, a secure image registry, advanced management control plane, and Docker Engine - Enterprise.
Docker Engine - Enterprise is a certified and supported container runtime that is also available as a standalone
solution  to provide enterprises with the most secure container engine in the industry. For more information
about Docker Enterprise and Docker Engine - Enterprise, including purchasing options,
see [Docker Enterprise](https://www.docker.com/enterprise-edition/).
{% endcomment %}
Docker Enterprise はエンタープライズ開発向けとして設計されています。
これは IT チームが開発する重要なビジネスアプリケーションを、相当規模の本番環境において構築、導入、実行するためのものです。
Docker Enterprise is an integrated container platform that includes
Docker Desktop Enterprise, a secure image registry, advanced management control plane, and Docker Engine - Enterprise.
Docker Engine - Enterprise is a certified and supported container runtime that is also available as a standalone
solution  to provide enterprises with the most secure container engine in the industry.
より詳細な Docker Enterprise の情報や、その購入オプションなどに関しては [Docker Enterprise](https://www.docker.com/enterprise-edition/) を参照してください。

{% comment %}
> Compatibility Matrix
>
> Refer to the [Compatibility Matrix](https://success.docker.com/article/compatibility-matrix)
> for the latest list of supported platforms.
{: .important}
{% endcomment %}
> 互換マトリックス
>
> サポート対応しているプラットフォームについての最新一覧は [互換マトリックス（Compatibility Matrix）](https://success.docker.com/article/compatibility-matrix) を参照してください。
{: .important}

{% comment %}
## Docker Enterprise products
{% endcomment %}
## Docker Enterprise 製品
{: #docker-enterprise-products }

{% include docker_ee.md %}

{% comment %}
> Note
>
> Starting with Docker Enterprise 2.1, Docker Enterprise - Basic is now Docker Engine - Enterprise,
> and both Docker Enterprise - Standard and Docker Enterprise - Advanced are now called Docker Enterprise.
{% endcomment %}
> メモ
>
> Docker Enterprise 2.1 をはじめとする、Docker Enterprise - Basic は現在は Docker Engine - Enterprise として、また
Docker Enterprise - Standard、Docker Enterprise - Advanced は Docker Enterprise と呼び表わしています。

{% comment %}
### Docker Enterprise
{% endcomment %}
### Docker Enterprise
{: #docker-enterprise }

{% comment %}
With Docker Enterprise, you can manage container workloads on Windows, Linux, on site, or on the cloud
in a flexible way.
{% endcomment %}
Docker Enterprise を用いると、コンテナー作業をより柔軟に管理できるようになります。
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
### New licensing for Docker Enterprise
{% endcomment %}
### New licensing for Docker Enterprise
{: #new-licensing-for-docker-enterprise }

{% comment %}
Starting in version 18.09, Docker Enterprise is aware of the license applied on
the system. The license summary is available in the `docker info` output on
standalone or manager nodes.
{% endcomment %}
Starting in version 18.09, Docker Enterprise is aware of the license applied on
the system. The license summary is available in the `docker info` output on
standalone or manager nodes.

{% comment %}
For Docker Enterprise customers, when you license Universal Control Plane
(UCP), this same license is applied to the underlying engines in the cluster.
Docker recommends that Enterprise customers use UCP to manage their license.
{% endcomment %}
For Docker Enterprise customers, when you license Universal Control Plane
(UCP), this same license is applied to the underlying engines in the cluster.
Docker recommends that Enterprise customers use UCP to manage their license.

{% comment %}
Standalone Docker Engine - Enterprise users can license engines using `docker engine activate`.
{% endcomment %}
Standalone Docker Engine - Enterprise users can license engines using `docker engine activate`.

{% comment %}
Offline activation of standalone enterprise engines can be performed by downloading the license and using the command `docker engine activate --license filename.lic`.
{% endcomment %}
Offline activation of standalone enterprise engines can be performed by downloading the license and using the command `docker engine activate --license filename.lic`.

{% comment %}
Additionally, Docker is now distributing the CLI as a separate installation package. This gives Docker Enterprise users the ability to install as many CLI packages as needed without using the Engine node licenses for client-only systems.
{% endcomment %}
Additionally, Docker is now distributing the CLI as a separate installation package. This gives Docker Enterprise users the ability to install as many CLI packages as needed without using the Engine node licenses for client-only systems.

{% comment %}
[Learn more about Docker Enterprise](/ee/index.md).
{% endcomment %}
[Learn more about Docker Enterprise](/ee/index.md).


{% comment %}
> When using Docker Enterprise
> Microsoft Windows Server is not supported as a manager. Microsoft Windows
> Server 1803 is not supported as a worker.
{% endcomment %}
> When using Docker Enterprise
> Microsoft Windows Server is not supported as a manager. Microsoft Windows
> Server 1803 is not supported as a worker.

{% comment %}
### Docker Certified Infrastructure
{% endcomment %}
### Docker Certified Infrastructure
{: #docker-certified-infrastructure }

{% comment %}
Docker Certified Infrastructure is Docker’s prescriptive approach to deploying Docker Enterprise
on a variety of infrastructures. Each Docker Certified Infrastructure option includes a reference architecture,
a CLI plugin for automated deployment and configuration, and third-party ecosystem solution briefs.
{% endcomment %}
Docker Certified Infrastructure is Docker’s prescriptive approach to deploying Docker Enterprise
on a variety of infrastructures. Each Docker Certified Infrastructure option includes a reference architecture,
a CLI plugin for automated deployment and configuration, and third-party ecosystem solution briefs.

{% comment %}
| Platform  | Docker Enterprise support |
:----------------------------------------------------------------------------------------|:-------------------------:|
| [Amazon Web Services](..\cluster\aws.md) |  {{ page.green-check }}   |
| [Azure](..\cluster\azure.md) |  {{ page.green-check }}   |
| VMware  |  coming soon  |
{% endcomment %}
| プラットフォーム | Docker Enterprise エディション |
:----------------------------------------------------------------------------------------|:-------------------------:|
| [Amazon Web Services](..\cluster\aws.md) |  {{ page.green-check }}   |
| [Azure](..\cluster\azure.md) |  {{ page.green-check }}   |
| VMware  |  coming soon  |

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
The Docker API version is independent of the Docker platform version. We
maintain careful API backward compatibility and deprecate APIs and features
slowly and conservatively. We remove features after deprecating them for a
period of three stable releases. Docker 1.13 introduced improved
interoperability between clients and servers using different API versions,
including dynamic feature negotiation.
{% endcomment %}
Docker API のバージョンは Docker バージョンとは独立しています。
API の保守においては注意深く下位互換性を維持し、API や機能の廃止に関しては慎重にゆっくりと行っています。
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
Docker supports Docker Enterprise minor releases for 24 months. Upgrades to the
latest minor release of Docker Enterprise are not required, however we
recommend staying on the latest maintenance release of the supported minor
release you are on. Please see [Maintenance
Lifecycle](https://success.docker.com/article/maintenance-lifecycle) for more
details on EOL of minor and major versions of Docker Enterprise.
{% endcomment %}
Docker supports Docker Enterprise minor releases for 24 months. Upgrades to the
latest minor release of Docker Enterprise are not required, however we
recommend staying on the latest maintenance release of the supported minor
release you are on. Please see [Maintenance
Lifecycle](https://success.docker.com/article/maintenance-lifecycle) for more
details on EOL of minor and major versions of Docker Enterprise.

{% comment %}
## Where to go next
{% endcomment %}
{: #where-to-go-next }
## Where to go next

{% comment %}
- [Install Docker Engine - Enterprise for RHEL](/install/linux/docker-ee/rhel/)
- [Install Docker Engine - Enterprise for Ubuntu](/install/linux/docker-ee/ubuntu/)
- [Install Docker Engine - Enterprise for Windows Server](/install/windows/docker-ee/)
{% endcomment %}
- [Install Docker Engine - Enterprise for RHEL](/install/linux/docker-ee/rhel/)
- [Install Docker Engine - Enterprise for Ubuntu](/install/linux/docker-ee/ubuntu/)
- [Install Docker Engine - Enterprise for Windows Server](/install/windows/docker-ee/)
