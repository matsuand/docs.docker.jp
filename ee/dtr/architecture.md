---
title: DTR アーキテクチャー
description: Learn about the architecture of Docker Trusted Registry.
keywords: registry, dtr, architecture
---

>{% include enterprise_label_shortform.md %}

{% comment %}
Docker Trusted Registry (DTR) is a containerized application that runs on a
Docker Universal Control Plane cluster.
{% endcomment %}
Docker Trusted Registry (DTR) はコンテナー化されたアプリケーションであり、Docker Universal Control Plane によるクラスター上で動作します。

![](images/architecture-1.svg)

{% comment %}
Once you have DTR deployed, you use your Docker CLI client to login, push, and
pull images.
{% endcomment %}
Once you have DTR deployed, you use your Docker CLI client to login, push, and
pull images.

{% comment %}
## Under the hood
{% endcomment %}
{: #under-the-hood }
## Under the hood

{% comment %}
For high-availability you can deploy multiple DTR replicas, one on each UCP
worker node.
{% endcomment %}
For high-availability you can deploy multiple DTR replicas, one on each UCP
worker node.

![](images/architecture-2.svg)

{% comment %}
All DTR replicas run the same set of services and changes to their configuration
are automatically propagated to other replicas.
{% endcomment %}
All DTR replicas run the same set of services and changes to their configuration
are automatically propagated to other replicas.

{% comment %}
## DTR internal components
{% endcomment %}
{: #dtr-internal-components }
## DTR の内部コンポーネント

{% comment %}
When you install DTR on a node, the following containers are started:
{% endcomment %}
ノード上に DTR をインストールすると、以下に示すコンテナーが起動します。

{% comment %}
| Name                                 | Description                                                                                                                        |
|:-------------------------------------|:-----------------------------------------------------------------------------------------------------------------------------------|
| dtr-api-&lt;replica_id&gt;           | Executes the DTR business logic. It serves the DTR web application, and API                                                        |
| dtr-garant-&lt;replica_id&gt;        | Manages DTR authentication                                                                                                         |
| dtr-jobrunner-&lt;replica_id&gt;     | Runs cleanup jobs in the background                                                                                                |
| dtr-nginx-&lt;replica_id&gt;         | Receives http and https requests and proxies them to other DTR components. By default it listens to ports 80 and 443 of the host   |
| dtr-notary-server-&lt;replica_id&gt; | Receives, validates, and serves content trust metadata, and is consulted when pushing or pulling to DTR with content trust enabled |
| dtr-notary-signer-&lt;replica_id&gt; | Performs server-side timestamp and snapshot signing for content trust metadata                                                     |
| dtr-registry-&lt;replica_id&gt;      | Implements the functionality for pulling and pushing Docker images. It also handles how images are stored                          |
| dtr-rethinkdb-&lt;replica_id&gt;     | A database for persisting repository metadata                                                                                      |
| dtr-scanningstore-&lt;replica_id&gt; | Stores security scanning data                                                                                                      |
{% endcomment %}
| 名称                                 | 内容説明                                                                                                                           |
|:-------------------------------------|:-----------------------------------------------------------------------------------------------------------------------------------|
| dtr-api-&lt;replica_id&gt;           | Executes the DTR business logic. It serves the DTR web application, and API                                                        |
| dtr-garant-&lt;replica_id&gt;        | Manages DTR authentication                                                                                                         |
| dtr-jobrunner-&lt;replica_id&gt;     | Runs cleanup jobs in the background                                                                                                |
| dtr-nginx-&lt;replica_id&gt;         | Receives http and https requests and proxies them to other DTR components. By default it listens to ports 80 and 443 of the host   |
| dtr-notary-server-&lt;replica_id&gt; | Receives, validates, and serves content trust metadata, and is consulted when pushing or pulling to DTR with content trust enabled |
| dtr-notary-signer-&lt;replica_id&gt; | Performs server-side timestamp and snapshot signing for content trust metadata                                                     |
| dtr-registry-&lt;replica_id&gt;      | Implements the functionality for pulling and pushing Docker images. It also handles how images are stored                          |
| dtr-rethinkdb-&lt;replica_id&gt;     | A database for persisting repository metadata                                                                                      |
| dtr-scanningstore-&lt;replica_id&gt; | セキュリティスキャニングデータを保持します。                                                                                       |

{% comment %}
All these components are for internal use of DTR. Don't use them in your applications.
{% endcomment %}
このコンポーネントはすべて DTR 内部で用いられるものです。
アプリケーションからこれらを利用しないでください。

{% comment %}
## Networks used by DTR
{% endcomment %}
{: #networks-used-by-dtr }
## DTR が利用するネットワーク

{% comment %}
To allow containers to communicate, when installing DTR the following networks
are created:
{% endcomment %}
コンテナーによるネットワーク通信を可能とするために、DTR のインストール時には以下のネットワークが生成されます。

{% comment %}
| Name   | Type    | Description                                                                            |
|:-------|:--------|:---------------------------------------------------------------------------------------|
| dtr-ol | overlay | Allows DTR components running on different nodes to communicate, to replicate DTR data |
{% endcomment %}
| 名称   | タイプ  | 内容説明                                                                               |
|:-------|:--------|:---------------------------------------------------------------------------------------|
| dtr-ol | overlay | Allows DTR components running on different nodes to communicate, to replicate DTR data |


{% comment %}
## Volumes used by DTR
{% endcomment %}
{: #volumes-used-by-dtr }
## DTR が利用するボリューム

{% comment %}
DTR uses these named volumes for persisting data:
{% endcomment %}
DTR はデータ保存のために以下の名前つきボリュームを利用します。

{% comment %}
| Volume name                         | Description                                                                      |
|:------------------------------------|:---------------------------------------------------------------------------------|
| dtr-ca-&lt;replica_id&gt;           | Root key material for the DTR root CA that issues certificates                   |
| dtr-notary-&lt;replica_id&gt;       | Certificate and keys for the Notary components                                   |
| dtr-postgres-&lt;replica_id&gt;     | Vulnerability scans data                                                         |
| dtr-registry-&lt;replica_id&gt;     | Docker images data, if DTR is configured to store images on the local filesystem |
| dtr-rethink-&lt;replica_id&gt;      | Repository metadata                                                              |
| dtr-nfs-registry-&lt;replica_id&gt; | Docker images data, if DTR is configured to store images on NFS                  |
{% endcomment %}
| ボリューム名                        | 内容説明                                                                         |
|:------------------------------------|:---------------------------------------------------------------------------------|
| dtr-ca-&lt;replica_id&gt;           | Root key material for the DTR root CA that issues certificates                   |
| dtr-notary-&lt;replica_id&gt;       | Certificate and keys for the Notary components                                   |
| dtr-postgres-&lt;replica_id&gt;     | Vulnerability scans data                                                         |
| dtr-registry-&lt;replica_id&gt;     | Docker images data, if DTR is configured to store images on the local filesystem |
| dtr-rethink-&lt;replica_id&gt;      | Repository metadata                                                              |
| dtr-nfs-registry-&lt;replica_id&gt; | Docker images data, if DTR is configured to store images on NFS                  |

{% comment %}
You can customize the volume driver used for these volumes, by creating the
volumes before installing DTR. During the installation, DTR checks which volumes
don't exist in the node, and creates them using the default volume driver.
{% endcomment %}
You can customize the volume driver used for these volumes, by creating the
volumes before installing DTR. During the installation, DTR checks which volumes
don't exist in the node, and creates them using the default volume driver.

{% comment %}
By default, the data for these volumes can be found at
`/var/lib/docker/volumes/<volume-name>/_data`.
{% endcomment %}
By default, the data for these volumes can be found at
`/var/lib/docker/volumes/<volume-name>/_data`.

{% comment %}
## Image storage
{% endcomment %}
{: #image-storage }
## イメージストレージ

{% comment %}
By default, Docker Trusted Registry stores images on the filesystem of the node
where it is running, but you should configure it to use a centralized storage
backend.
{% endcomment %}
By default, Docker Trusted Registry stores images on the filesystem of the node
where it is running, but you should configure it to use a centralized storage
backend.

![](images/architecture-3.svg)

{% comment %}
DTR supports these storage backends:
{% endcomment %}
DTR supports these storage backends:

{% comment %}
* NFS
* Amazon S3
* Cleversafe
* Google Cloud Storage
* OpenStack Swift
* Microsoft Azure
{% endcomment %}
* NFS
* Amazon S3
* Cleversafe
* Google Cloud Storage
* OpenStack Swift
* Microsoft Azure

{% comment %}
## How to interact with DTR
{% endcomment %}
{: #how-to-interact-with-dtr }
## How to interact with DTR

{% comment %}
DTR has a web UI where you can manage settings and user permissions.
{% endcomment %}
DTR has a web UI where you can manage settings and user permissions.

![](images/architecture-4.svg)

{% comment %}
You can push and pull images using the standard Docker CLI client or other tools
that can interact with a Docker registry.
{% endcomment %}
You can push and pull images using the standard Docker CLI client or other tools
that can interact with a Docker registry.

{% comment %}
## Where to go next
{% endcomment %}
{: #where-to-go-next }
## 次に読むものは

{% comment %}
* [System requirements](admin/install/system-requirements.md)
* [Install DTR](admin/install/index.md)
{% endcomment %}
* [システム要件](admin/install/system-requirements.md)
* [DTR のインストール](admin/install/index.md)
