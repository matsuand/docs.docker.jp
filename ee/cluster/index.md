---
description: Docker Cluster の概要と導入。
keywords: documentation, docs, docker, cluster, infrastructure, automation
title: Docker Cluster 概要
---

>{% include enterprise_label_shortform.md %}

{% comment %}
Docker Cluster is a tool for lifecycle management of Docker clusters.
With Cluster, you use a YAML file to configure your provider's resources.
Then, with a single command, you provision and install all the resources
from your configuration.
{% endcomment %}
Docker Cluster は Docker によるクラスターのライフサイクル管理を行うツールです。
Docker Cluster では YAML ファイルを使って、プロバイダーリソースの設定を行います。
そして単純な 1 つのコマンドを使い、設定したリソースすべてを導入（provision）してインストールします。

{% comment %}
Using Docker Cluster is a three-step process:
{% endcomment %}
Docker Cluster を使うには 3 つの手順を行います。

{% comment %}
1. Ensure you have the credentials necessary to provision a cluster.
{% endcomment %}
1. クラスターの導入に必要となる認証情報を持っていることを確認します。

{% comment %}
2. Define the resources that make up your cluster in `cluster.yml`
{% endcomment %}
2. クラスターを構成するリソースを `cluster.yml` に定義します。

{% comment %}
3. Run `docker cluster create` to have Cluster provision resources and install Docker Enterprise on the resources.
{% endcomment %}
3. `docker cluster create` を実行して、Cluster がリソース上に Docker Enterprise の導入とインストールを行うようにします。

{% comment %}
A `cluster.yml` file resembles the following example:
{% endcomment %}
`cluster.yml` ファイルは以下の例のようになります。

{% raw %}
```yaml
variable:
  region: us-east-2
  ucp_password:
    type: prompt

provider:
  aws:
    region: ${region}

cluster:
  engine:
    version: "ee-stable-18.09.5"
  ucp:
    version: "docker/ucp:3.1.6"
    username: "admin"
    password: ${ucp_password}

resource:
  aws_instance:
    managers:
       quantity: 1
```
{% endraw %}

{% comment %}
For more information about Cluster files, refer to the
[Cluster file reference](cluster-file.md).
{% endcomment %}
Cluster ファイルに関する詳細は [Cluster ファイルリファレンス](cluster-file.md) を参照してください。

{% comment %}
Docker Cluster has commands for managing the whole lifecycle of your cluster:
{% endcomment %}
Docker Cluster には、クラスターの全ライフサイクルを管理するコマンドがあります。

 {% comment %}
 * Create and destroy clusters
 * Scale up or scale down clusters
 * Upgrade clusters
 * View the status of clusters
 * Backup and restore clusters
 {% endcomment %}
 * クラスターの生成と削除
 * クラスターのスケールアップまたはスケールダウン
 * クラスターの更新
 * クラスターの状態確認
 * クラスターのバックアップと復元

{% comment %}
## Export Docker Cluster artifacts
{% endcomment %}
{: #export-docker-cluster-artifacts }
## Export Docker Cluster artifacts

{% comment %}
You can export both Terraform and Ansible scripts to deploy certain components standalone or with custom configurations. Use the following commands to export those scripts:
{% endcomment %}
コンポーネントをスタンドアロンにデプロイするため、あるいは設定をカスタマイズするために、Terraform と Ansible によるスクリプトがエクスポートできるようになっています。
以下のコマンドによりそのようなスクリプトをエクスポートします。

```bash
docker container run --detach --name dci --entrypoint sh docker/cluster:latest
docker container cp dci:/cluster/terraform terraform
docker container cp dci:/cluster/ansible ansible
docker container stop dci
docker container rm dci
```

{% comment %}
## Where to go next
{% endcomment %}
{: #where-to-go-next }
## 次に読むものは

{% comment %}
- [Get started with Docker Cluster on AWS](aws.md)
- [Command line reference](/engine/reference/commandline/cluster/)
- [Cluster file reference](cluster-file.md)
{% endcomment %}
- [AWS 上にて Docker Cluster をはじめよう](aws.md)
- [コマンドラインリファレンス](/engine/reference/commandline/cluster/)
- [Cluster ファイルリファレンス](cluster-file.md)

