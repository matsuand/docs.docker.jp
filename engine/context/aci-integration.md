---
title: Azure での Docker コンテナーのデプロイ
description: Azure において Docker コンテナーをデプロイします。
keywords: Docker, Azure, Integration, ACI, context, Compose, cli, deploy, containers, cloud
toc_min: 1
toc_max: 2
---

{% comment %}
## Overview
{% endcomment %}
{: #overview }
## 概要

{% comment %}
The Docker Azure Integration enables developers to use native Docker commands to run applications in Azure Container Instances (ACI) when building cloud-native applications. The new experience provides a tight integration between Docker Desktop and Microsoft Azure allowing developers to quickly run applications using the Docker CLI or VS Code extension, to switch seamlessly from local development to cloud deployment.
{% endcomment %}
Docker Azure 統合は、Azure コンテナーインスタンス（Azure Container Instances; ACI） の中で、ネイティブな Docker コマンドの利用を可能にします。
これを使って、クラウドに適したアプリケーションを構築し実行します。
この新たな仕組みによって、Docker Desktop と Microsoft Azure の緊密な統合が実現され、開発者にとっては、Docker CLI や VS Code 拡張機能を使ったアプリケーション実行が即座にできるようになり、またローカル開発環境とクラウドデプロイ環境をシームレスに切り替えることができます。

{% comment %}
In addition, the integration between Docker and Microsoft developer technologies allow developers to use the Docker CLI to:
{% endcomment %}
さらに Docker と Microsoft の開発技術の統合により、開発者が Docker CLI を用いる際には、以下のことが可能になります。

{% comment %}
- Easily log into Azure
- Set up an ACI context in one Docker command allowing you to switch from a local context to a cloud context and run applications quickly and easily
- Simplify single container and multi-container application development using the Compose specification, allowing a developer to invoke fully Docker-compatible commands seamlessly for the first time natively within a cloud container service
{% endcomment %}
- Azure に簡単にログインすることができます。
- 1 つの Docker コマンドに対して ACI コンテキストを設定することができます。
  これによってローカルコンテキストとクラウドコンテキストを切り替えられるようになり、アプリケーションの実行をすばやく簡単に行うことができます。
- Compose を利用すれば、単一コンテナーアプリケーション、複数コンテナーアプリケーションの開発を単純化できます。
  開発者にとってはクラウドコンテナーサービスに対して、シームレスな Docker 完全互換コマンドの実行が初めて可能になります。

{% comment %}
>**Note**
>
> Docker Azure Integration is currently a beta release. The commands and flags are subject to change in subsequent releases.
{:.important}
{% endcomment %}
>**メモ**
>
> Docker Azure 統合は、現在ベータ版として提供されます。
> 提供されるコマンドやフラグは、将来のリリースにおいて変更されることがあります。
{:.important}

{% comment %}
## Prerequisites
{% endcomment %}
{: #prerequisites }
## 前提条件

{% comment %}
To deploy Docker containers on Azure, you must meet the following requirements:
{% endcomment %}
Docker コンテナーを Azure にデプロイするには、以下の条件を満たしていることが必要です。

{% comment %}
1. Download and install Docker Desktop Edge version 2.3.2.0 or later.
{% endcomment %}
1. Docker Desktop 最新版（Edge）2.3.2.0 またはそれ以降をダウンロードしインストールしていることが必要です。

    - [Download for Mac](https://desktop.docker.com/mac/edge/Docker.dmg){: target="_blank" class="_"}
    - [Download for Windows](https://desktop.docker.com/win/edge/Docker%20Desktop%20Installer.exe){: target="_blank" class="_"}

    {% comment %}
    Alternatively, install the [Docker ACI Integration for Linux](#install-the-docker-aci-integration-cli-on-linux).
    {% endcomment %}
    あるいは [Docker ACI Integration for Linux](#install-the-docker-aci-integration-cli-on-linux) をインストールしていることが必要です。

{% comment %}
2. Ensure you have an Azure subscription. You can get started with an [Azure free account](https://aka.ms/AA8r2pj){: target="_blank" class="_"}.
{% endcomment %}
2. Azure サブスクリプションを持っていることが必要です。
   [Azure free account](https://aka.ms/AA8r2pj){: target="_blank" class="_"} にアクセスして取得操作を進めることができます。

{% comment %}
## Run Docker containers on ACI
{% endcomment %}
{: #run-docker-containers-on-aci }
## ACI での Docker コンテナー実行

{% comment %}
Docker not only runs containers locally, but also enables developers to seamlessly deploy Docker containers on ACI using `docker run` or deploy multi-container applications defined in a Compose file using the `docker compose up` command.
{% endcomment %}
Docker は、単にローカルのコンテナーを実行するだけのものではなくなります。
`docker run` を使って ACI 上の Docker コンテナーをシームレスにデプロイできます。
あるいは Compose ファイルに定義されたマルチコンテナーのアプリケーションを `docker compose up` コマンドを使ってデプロイできるようになります。

{% comment %}
The following sections contain instructions on how to deploy your Docker containers on ACI.
{% endcomment %}
以下の節では ACI 上において Docker コンテナーをデプロイする手順を説明します。

{% comment %}
### Log into Azure
{% endcomment %}
{: #log-into-azure }
### Azure へのログイン

{% comment %}
Run the following commands to log into Azure:
{% endcomment %}
以下のコマンドを実行して Azure にログインします。

```console
docker login azure
```

{% comment %}
This opens your web browser and prompts you to enter your Azure login credentials.
{% endcomment %}
コマンドを実行するとウェブブラウザーが開き、Azure のログイン情報の入力が求められます。

{% comment %}
### Create an ACI context
{% endcomment %}
{: #create-an-aci-context }
### ACI コンテキストの生成

{% comment %}
After you have logged in, you need to create a Docker context associated with ACI to deploy containers in ACI. For example, let us create a new context called `myacicontext`:
{% endcomment %}
ログインした後は、ACI においてコンテナーをデプロイできるようにするために、ACI に関する Docker コンテキストを生成することが必要です。
たとえば新たなコンテキストとして `myacicontext` を生成することにします。

```console
docker context create aci myacicontext
```

{% comment %}
This command automatically uses your Azure login credentials to identify your subscription IDs and resource groups. You can then interactively select the subscription and group that you would like to use. If you prefer, you can specify these options in the CLI using the following flags: `--subscription-id`,
`--resource-group`, and `--location`.
{% endcomment %}
このコマンドは自動的に Azure ログイン情報を利用して、サブスクリプション ID やリソースグループを識別するものです。
コマンド実行においては、利用するサブスクリプションやグループを対話的に選択します。
必要であれば CLI の以下のようなフラグ `--subscription-id`、`--resource-group`、`--location` を使って指定することもできます。

{% comment %}
If you don't have any existing resource groups in your Azure account, the `docker context create aci myacicontext` command creates one for you. You don’t have to specify any additional options to do this.
{% endcomment %}
Azure アカウントにおいてリソースグループを 1 つも生成していない場合、`docker context create aci myacicontext` コマンドの実行によって、リソースグループが生成されます。
その際には特別なオプションを設定する必要はありません。

{% comment %}
After you have created an ACI context, you can list your Docker contexts by running the `docker context ls` command:
{% endcomment %}
ACI コンテキストを生成したら、`docker context ls` コマンドを実行して Docker コンテキストの一覧を確認します。

```console
NAME                TYPE                DESCRIPTION                               DOCKER ENDPOINT                KUBERNETES ENDPOINT   ORCHESTRATOR
myacicontext        aci                 myResourceGroupGTA@eastus
default *           moby              Current DOCKER_HOST based configuration   unix:///var/run/docker.sock                          swarm
```

{% comment %}
> **Note**
>
> If you need to change the subscription and create a new context, you must 
execute the `docker login azure` command again.
{% endcomment %}
> **メモ**
>
> サブスクリプションの切り替えが必要となって、新たなコンテキストを生成した場合には、`docker login azure` コマンドを再び実行する必要があります。

{% comment %}
### Run a container
{% endcomment %}
{: #run-a-container }
### コンテナーの実行

{% comment %}
Now that you've logged in and created an ACI context, you can start using Docker commands to deploy containers on ACI.
{% endcomment %}
ログインを済ませて ACI コンテキストも生成したので、ACI 上にコンテナーをデプロイするための Docker コマンド操作を進めていきます。

{% comment %}
There are two ways to use your new ACI context. You can use the `--context` flag with the Docker command to specify that you would like to run the command using your newly created ACI context.
{% endcomment %}
新しく生成した ACI コンテキストを利用する方法は 2 つあります。
1 つは Docker コマンドにおいて `--context` フラグを利用する方法です。
新たに生成した ACI コンテキストをここに指定して、コマンド実行を行います。

```console
docker --context myacicontext run -p 80:80 nginx
```

{% comment %}
Or, you can change context using `docker context use` to select the ACI context to be your focus for running Docker commands. For example, we can use the `docker context use` command to deploy an ngnix container:
{% endcomment %}
もう 1 つは `docker context use` を使ってコンテキストを切り替える方法です。
Docker コマンドの実行にあたって、対象とするコンテキストを ACI コンテキストとする方法です。
たとえば `docker context use` コマンドを利用して、nginx コンテナーをデプロイしてみます。

```console
docker context use myacicontext
docker run -p 80:80 nginx
```

{% comment %}
After you've switched to the `myacicontext` context, you can use docker ps to list your containers running on ACI.
{% endcomment %}
コンテキストを `myacicontext` に切り替えたら、docker ps を使って、ACI 上に動作するコンテナー一覧を確認することができます。

{% comment %}
To view logs from your container, run:
{% endcomment %}
コンテナーからログを確認するには以下を実行します。

{% comment %}
```console
docker logs <CONTAINER_ID>
```
{% endcomment %}
```console
docker logs <コンテナーID>
```

{% comment %}
To execute a command in a running container, run:
{% endcomment %}
実行中のコンテナーにおいてコマンドを実行するには、以下のようにします。

{% comment %}
```console
docker exec -t <CONTAINER_ID> COMMAND
```
{% endcomment %}
```console
docker exec -t <コンテナーID> COMMAND
```

{% comment %}
To stop and remove a container from ACI, run:
{% endcomment %}
ACI 上のコンテナーを停止して削除するには、以下のようにします。

{% comment %}
```console
docker rm <CONTAINER_ID>
```
{% endcomment %}
```console
docker rm <コンテナーID>
```

{% comment %}
## Running Compose applications
{% endcomment %}
{: #running-compose-applications }
## Compose アプリケーションの実行

{% comment %}
You can also deploy and manage multi-container applications defined in Compose files to ACI using the `docker compose` command. To do this:
{% endcomment %}
ACI に対しては、Compose ファイルにて定義されたマルチコンテナーアプリケーションのデプロイと管理も可能です。
その際には`docker compose` コマンドを利用します。
そしてこれを実現するにあたっては、以下が必要です。

{% comment %}
1. Ensure you are using your ACI context. You can do this either by specifying the `--context myacicontext` flag or by setting the default context using the command  `docker context use myacicontext`.
{% endcomment %}
1. ACI コンテキストを利用していることが必要です。
   これは `--context myacicontext` フラグを指定するか、あるいはデフォルトのコンテキストを設定するコマンド  `docker context use myacicontext` を実行します。

{% comment %}
2. Run `docker compose up` and `docker compose down` to start and then stop a full Compose application.
{% endcomment %}
2. `docker compose up` と `docker compose down` を実行して、Compose アプリケーションの開始、停止ができることが必要です。

  {% comment %}
  By default, `docker compose up` uses the `docker-compose.yaml` file in the current folder. You can specify the working directory using the  --workdir  flag or specify the Compose file directly using the `--file` flag.
  {% endcomment %}
  デフォルトにおいて `docker compose up` は、カレントフォルダーの `docker-compose.yaml` ファイルを利用します。
  ワーキングディレクトリは --workdir フラグにより指定することができます。
  また Compose ファイルを直接 `--file` フラグを使って指定することもできます。

  {% comment %}
  You can also specify a name for the Compose application using the `--project-name` flag during deployment. If no name is specified, a name will be derived from the working directory.
  {% endcomment %}
  Compose アプリケーションのデプロイの際に `--project-name` フラグを使って、アプリケーション名を指定することもできます。
  アプリケーション名の指定がなかった場合は、ワーキングディレクトリから名前が定められます。

  {% comment %}
  You can view logs from containers that are part of the Compose application using the command `docker logs <CONTAINER_ID>`. To know the container ID, run `docker ps`.
  {% endcomment %}
  Compose アプリケーションを構成するコンテナーに対しては、コマンド `docker logs <コンテナーID>` を実行してそれぞれのログを確認できます。
  コンテナー ID を調べるには `docker ps` を実行します。

{% comment %}
> **Note**
>
> The current Docker Azure integration does not allow fetching a combined log stream from all the containers that make up the Compose application.
{% endcomment %}
> **メモ**
>
> 現時点の Docker Azure 統合では、Compose アプリケーションを構成するコンテナーからのログを、すべて集めて取得するようなことはできません。

{% comment %}
## Using ACI resource groups as namespaces
{% endcomment %}
{: #using-aci-resource-groups-as-namespaces }
## 名前空間としての ACI リソースグループの利用

{% comment %}
You can create several Docker contexts associated with ACI. Each context must be associated with a unique Azure resource group. This allows you to use Docker contexts as namespaces. You can switch between namespaces using `docker context use <CONTEXT>`.
{% endcomment %}
ACI に関連づける Docker コンテキストは、複数用意することができます。
個々のコンテキストは、一意の Azure リソースグループに関連づいていることが必要です。
こうしておくと Docker コンテキストを名前空間として利用することができます。
`docker context use <CONTEXT>` を実行することで、複数の名前空間を切り替えることができます。

{% comment %}
When you run the `docker ps` command, it only lists containers in your current Docker context. There won’t be any contention in container names or Compose application names between two Docker contexts.
{% endcomment %}
`docker ps` コマンドを実行すると、現在の Docker コンテキスト内にあるコンテナーのみが一覧表示されます。
Docker コンテキストが異なれば、コンテナー名や Compose アプリケーション名が同一であっても、競合することにはなりません。

{% comment %}
## Install the Docker ACI Integration CLI on Linux
{% endcomment %}
{: #install-the-docker-aci-integration-cli-on-linux }
## Linux における Docker ACI 統合 CLI のインストール

{% comment %}
The Docker ACI Integration CLI adds support for running and managing containers on Azure Container Instances (ACI).
{% endcomment %}
Docker ACI 統合の CLI は、Azure コンテナーインスタンス（ACI）上でのコンテナー実行と管理をサポートします。

{% comment %}
>**Note**
>
> **Docker Azure Integration is a beta release**. The installation process, commands, and flags will change in future releases.
{:.important}
{% endcomment %}
>**メモ**
>
> **Docker Azure 統合はベータ版です**。
> インストール処理、コマンド、フラグは、将来のリリースにおいて変更されることがあります。
{:.important}

{% comment %}
### Prerequisites
{% endcomment %}
{: #prerequisites }
### 前提条件

{% comment %}
* [Docker 19.03 or later](https://docs.docker.com/get-docker/)
{% endcomment %}
* [Docker 19.03 またはそれ以降](https://docs.docker.com/get-docker/)

{% comment %}
### Install script
{% endcomment %}
{: #install-script }
### インストールスクリプト

{% comment %}
You can install the new CLI using the install script:
{% endcomment %}
新たな CLI は、インストールスクリプトを利用してインストールします。

```console
curl -L https://raw.githubusercontent.com/docker/aci-integration-beta/main/scripts/install_linux.sh | sh
```

{% comment %}
### Manual install
{% endcomment %}
{: #manual-install }
### 手動インストール

{% comment %}
You can download the Docker ACI Integration CLI from the 
[latest release](https://github.com/docker/aci-integration-beta/releases/latest){: target="_blank" class="_"} page.
{% endcomment %}
Docker ACI 統合の CLI は、[最新版](https://github.com/docker/aci-integration-beta/releases/latest){: target="_blank" class="_"} ページからダウンロードすることができます。

{% comment %}
You will then need to make it executable:
{% endcomment %}
入手したらこれを実行可能にします。

```console
chmod +x docker-aci
```

{% comment %}
To enable using the local Docker Engine and to use existing Docker contexts, you
must have the existing Docker CLI as `com.docker.cli` somewhere in your
`PATH`. You can do this by creating a symbolic link from the existing Docker
CLI:
{% endcomment %}
ローカルの Docker Engine の利用を有効にするため、そして既存の Docker コンテキストを利用可能にするため、既存の Docker CLI は `com.docker.cli` として、パス上のどこかに配置する必要があります。

```console
ln -s /path/to/existing/docker /directory/in/PATH/com.docker.cli
```

{% comment %}
> **Note**
>
> The `PATH` environment variable is a colon-separated list of
> directories with priority from left to right. You can view it using
> `echo $PATH`. You can find the path to the existing Docker CLI using
> `which docker`. You may need root permissions to make this link.
{% endcomment %}
> **メモ**
>
> 環境変数 `PATH` は、複数ディレクトリをコロンで区切るものであり、左にあるものが優先されます。
> 内容を確認するには `echo $PATH` を実行します。
> 既存の Docker CLI へのパスは `which docker` を使って確認できます。
> なお上のリンクを生成する際には root 権限を要するかもしれません。

{% comment %}
On a fresh install of Ubuntu 20.04 with Docker Engine
[already installed](https://docs.docker.com/engine/install/ubuntu/):
{% endcomment %}
Docker Engine が [インストール](https://docs.docker.com/engine/install/ubuntu/) されている Ubuntu 20.04 の場合、初期状態では以下のようになっています。

```console
$ echo $PATH
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin
$ which docker
/usr/bin/docker
$ sudo ln -s /usr/bin/docker /usr/local/bin/com.docker.cli
```

{% comment %}
You can verify that this is working by checking that the new CLI works with the
default context:
{% endcomment %}
新しい CLI が動作しているかどうかを確認するには、デフォルトのコンテキストを使って CLI を実行します。

```console
$ ./docker-aci --context default ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
$ echo $?
0
```

{% comment %}
To make this CLI with ACI integration your default Docker CLI, you must move it
to a directory in your `PATH` with higher priority than the existing Docker CLI.
{% endcomment %}
ACI と統合された CLI を、デフォルトの Docker CLI とするためには、`PATH` 上のディレクトリにおいて、既存の Docker CLI よりも優先されるようなディレクトリに移動させる必要があります。

{% comment %}
Again, on a fresh Ubuntu 20.04:
{% endcomment %}
再び Ubuntu 20.04 の例を見ます。

```console
$ which docker
/usr/bin/docker
$ echo $PATH
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin
$ sudo mv docker-aci /usr/local/bin/docker
$ which docker
/usr/local/bin/docker
$ docker version
...
 Azure integration  0.1.4
...
```

{% comment %}
### Supported commands
{% endcomment %}
{: #supported-commands }
### サポートされているコマンド

{% comment %}
After you have installed the Docker ACI Integration CLI, run `--help` to see the current list of commands.
{% endcomment %}
Docker ACI 統合の CLI をインストールしたら、`--help` をつけてコマンド実行し、この時点でのコマンド一覧を確認します。

{% comment %}
> **Note**
>
> Docker Azure Integration is a beta release. The commands and flags will change in future releases.
{:.important}
{% endcomment %}
> **メモ**
>
> Docker Azure 統合はベータ版です。
> コマンドやフラグは、将来リリースにおいて変更されることがあります。
{:.important}

{% comment %}
### Uninstall
{% endcomment %}
{: #uninstall }
### アンインストール

{% comment %}
To remove the Docker Azure Integration CLI, you need to remove the binary you downloaded and `com.docker.cli` from your `PATH`. If you installed using the script, this can be done as follows:
{% endcomment %}
Docker Azure 統合の CLI を削除する場合は、ダウンロードしたバイナリと `com.docker.cli` を `PATH` 上から削除します。
スクリプトを通じてインストールを行った場合は、以下により削除することができます。

```console
sudo rm /usr/local/bin/docker /usr/local/bin/com.docker.cli
```

{% comment %}
## Feedback
{% endcomment %}
{: #feedback }
## フィードバック

{% comment %}
Thank you for trying out the Docker Azure Integration beta release. Your feedback is very important to us. Let us know your feedback by creating an issue in the [aci-integration-beta](https://github.com/docker/aci-integration-beta){: target="_blank" class="_"} GitHub repository.
{% endcomment %}
Docker Azure 統合ベータ版を利用していただき、ありがとうございます。
みなさんからのフィードバックが大変重要です。
フィードバックをいただくには、Github レポジトリ [aci-integration-beta](https://github.com/docker/aci-integration-beta){: target="_blank" class="_"} に issue をあげてください。