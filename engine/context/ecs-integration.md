---
title: ECS での Docker コンテナーのデプロイ
description: ECS において Docker コンテナーをデプロイします。
keywords: Docker, AWS, ECS, Integration, context, Compose, cli, deploy, containers, cloud
toc_min: 1
toc_max: 2
---

{% comment %}
## Overview
{% endcomment %}
{: #overview }
## 概要

{% comment %}
The Docker ECS Integration enables developers to use native Docker commands to run applications in Amazon EC2 Container Service (ECS) when building cloud-native applications.
{% endcomment %}
Docker ECS 統合は、Amazon EC2 コンテナーサービス（Amazon EC2 Container Service; ECS）の中で、ネイティブな Docker コマンドの利用を可能にします。
これを使って、クラウドに適したアプリケーションを構築し実行します。

{% comment %}
The integration between Docker and Amazon ECS allow developers to use the Docker CLI to:
{% endcomment %}
Docker と Amazon ECS の統合により、開発者が Docker CLI を用いる際には、以下のことが可能になります。

{% comment %}
Set up an AWS context in one Docker command, allowing you to switch from a local context to a cloud context and run applications quickly and easily
Simplify multi-container application development on Amazon ECS using the Compose specification
{% endcomment %}
1 つの Docker コマンドに対して AWS コンテキストを設定することができます。
これによってローカルコンテキストとクラウドコンテキストを切り替えられるようになり、アプリケーションの実行をすばやく簡単に行うことができます。
Compose を使って、Amazon ECS 上の複数コンテナーアプリケーションの開発を単純化できます。

{% comment %}
>**Note**
>
> Docker ECS Integration is currently a beta release. The commands and flags are subject to change in subsequent releases.
{:.important}
{% endcomment %}
>**メモ**
>
> Docker ECS 統合は、現在ベータ版として提供されます。
> 提供されるコマンドやフラグは、将来のリリースにおいて変更されることがあります。
{:.important}

{% comment %}
## Prerequisites
{% endcomment %}
{: #prerequisites }
## 前提条件

{% comment %}
To deploy Docker containers on ECS, you must meet the following requirements:
{% endcomment %}
Docker コンテナーを ECS にデプロイするには、以下の条件を満たしていることが必要です。

{% comment %}
1. Download and install Docker Desktop Edge version 2.3.3.0 or later.
{% endcomment %}
1. Docker Desktop 最新版（Edge）2.3.2.0 またはそれ以降をダウンロードしインストールしていることが必要です。

    - [Download for Mac](https://desktop.docker.com/mac/edge/Docker.dmg){: target="_blank" class="_"}
    - [Download for Windows](https://desktop.docker.com/win/edge/Docker%20Desktop%20Installer.exe){: target="_blank" class="_"}

    {% comment %}
    Alternatively, install [Docker ECS Integration for Linux](#install-the-docker-ecs-integration-cli-on-linux).
    {% endcomment %}
    あるいは [Docker ECS Integration for Linux](#install-the-docker-ecs-integration-cli-on-linux) をインストールしていることが必要です。

{% comment %}
2. Ensure you have an AWS account.
{% endcomment %}
2. AWS アカウントを持っていることが必要です。

  {% comment %}
  > **Note**
  >
  > If you had previously installed a Docker Desktop Stable release and now switched to Edge, ensure you turn on the experimental features flag. 
  >
  > From the Docker Desktop menu, click **Settings** (Preferences on macOS) > **Command Line** and then turn on the **Enable experimental features** toggle. Click **Apply & Restart** for the changes to take effect.
  {% endcomment %}
  > **メモ**
  >
  > これまで Docker Desktop 安定版（Stable）をインストールしていて、これを最新版（Edge）に切り替えている場合、試験的機能フラグがオンになっていることを確認してください。
  >
  > Docker Desktop メニューから **Settings**（macOS の場合は **Preferences**）> **Command Line** をクリックして、**Enable experimental features**（試験的機能の有効化）トグルをオンにしてください。
  > そして **Apply & Restart** をクリックして変更を適用します。

{% comment %}
Check your installation by running the command `docker ecs version`.
{% endcomment %}
インストール結果を確認するには、コマンド `docker ecs version` を実行します。

{% comment %}
Docker not only runs multi-container applications locally, but also enables developers to seamlessly deploy Docker containers on Amazon ECS using a Compose file with the `docker ecs compose up` command. The following sections contain instructions on how to deploy your Compose application on Amazon ECS.
{% endcomment %}
Docker は、単にローカルのマルチコンテナーを実行するだけのものではなくなります。
`docker ecs compose up` により Compose ファイルを使って Amazon ECS 上の Docker コンテナーをシームレスにデプロイできます。
以下の節では Amazon ECS 上において Docker コンテナーをデプロイする手順を説明します。

{% comment %}
### Create AWS context
{% endcomment %}
{: #create-aws-context }
### AWS コンテキストの生成

{% comment %}
Run the `docker ecs setup` command to create an AWS docker context. If you have already installed and configured the AWS CLI, the setup command lets you select an existing AWS profile to connect to Amazon. Otherwise, you can create a new profile by passing an [AWS access key ID and a secret access key](https://docs.aws.amazon.com/general/latest/gr/aws-sec-cred-types.html#access-keys-and-secret-access-keys){: target="_blank" class="_"}.
{% endcomment %}
`docker ecs setup` コマンドを実行して AWS コンテキストを生成します。
すでに AWS CLI をインストールし設定を行っているのであれば、このセットアップコマンドにより、既存の AWS プロファイルを選んで Amazon への接続を行います。
これがまだできていない場合は、[AWS access key ID and a secret access key](https://docs.aws.amazon.com/general/latest/gr/aws-sec-cred-types.html#access-keys-and-secret-access-keys){: target="_blank" class="_"} を通じて、新たなプロファイルを生成します。

{% comment %}
The `docker ecs setup` command will let you select an existing AWS configuration, or create one with provided secrets and tokens.
{% endcomment %}
`docker ecs setup` コマンドは、既存の AWS 設定の選択を行うことができます。
あるいは指定された機密情報やトークンを使って生成することができます。

{% comment %}
After you have created an AWS context, you can list your Docker contexts by running the `docker context ls` command:
{% endcomment %}
AWS コンテキストを生成したら、`docker context ls` コマンドを実行して Docker コンテキストの一覧を表示します。

```console
NAME   DESCRIPTION  DOCKER ENDPOINT  KUBERNETES ENDPOINT ORCHESTRATOR
aws *
default  Current DOCKER_HOST based configuration   unix:///var/run/docker.sock     swarm
```

{% comment %}
## Run Compose applications
{% endcomment %}
{: #run-compose-applications }
## Compose アプリケーションの実行

{% comment %}
You can deploy and manage multi-container applications defined in Compose files to Amazon ECS using the `docker ecs compose` command. To do this:
{% endcomment %}
Compose ファイルに定義したマルチコンテナーによるアプリケーションを、Amazon ECS に対してデプロイし管理するためには、`docker ecs compose` コマンドを実行します。
これを行うためには以下のことが必要です。

{% comment %}
- Ensure you are using your AWS context. You can do this either by specifying the `--context aws` flag with your command, or by setting the current context using the command `docker context use aws`.
{% endcomment %}
- AWS コンテキストを利用していることが必要です。
  これはコマンド実行時に `--context aws` フラグを指定するか、あるいはカレントなコンテキストを設定するコマンド  `docker context use aws` を実行します。

{% comment %}
- Run `docker ecs compose up` and `docker ecs compose down` to start and then stop a full Compose application.
{% endcomment %}
- `docker ecs compose up` と `docker ecs compose down` を実行して、Compose アプリケーションの開始、停止ができることが必要です。

  {% comment %}
  By default, `docker ecs compose up` uses the `docker-compose.yaml` file in the current folder. You can specify the Compose file directly using the `--file` flag.
  {% endcomment %}
  デフォルトにおいて `docker ecs compose up` は、カレントフォルダーの `docker-compose.yaml` ファイルを利用します。
  Compose ファイルを直接 `--file` フラグを使って指定することもできます。

  {% comment %}
  You can also specify a name for the Compose application using the `--project-name` flag during deployment. If no name is specified, a name will be derived from the working directory.
  {% endcomment %}
  Compose アプリケーションのデプロイの際に `--project-name` フラグを使って、アプリケーション名を指定することもできます。
  アプリケーション名の指定がなかった場合は、ワーキングディレクトリから名前が定められます。

{% comment %}
- You can view services created for the Compose application on Amazon ECS and their state using the `docker ecs compose ps` command.
{% endcomment %}
- Amazon ECS 上の Compose アプリケーションに対して生成されたサービスの情報およびその状態を確認するには `docker ecs compose ps` コマンドを実行します。

{% comment %}
- You can view logs from containers that are part of the Compose application using the `docker ecs compose logs` command.
{% endcomment %}
- Compose アプリケーションを構成するコンテナーに対しては、コマンド `docker ecs logs` を実行してそれぞれのログを確認できます。

{% comment %}
## Private Docker images
{% endcomment %}
{: #private-docker-images }
## プライベートな Docker イメージ

{% comment %}
The Docker ECS integration automatically configures authorization so you can pull private images from the Amazon ECR registry on the same AWS account. To pull private images from another registry, including Docker Hub, you’ll have to create a Username + Password (or a Username + Token) secret on the [Amazon SSM service](https://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-parameter-store.html){: target="_blank" class="_"}.
{% endcomment %}
Docker ECS 統合では自動的に認証が設定されます。
したがって Amazon ECR レジストリにあるプライベートイメージは、同じ AWS アカウントを使ってプルすることができます。
一方 Docker Hub も含め、別のレジストリからプライベートイメージをプルするには、[Amazon SSM サービス](https://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-parameter-store.html){: target="_blank" class="_"} 上にユーザー名とパスワード（あるいはユーザー名とトークン）を生成する必要があります。

{% comment %}
For your convenience, Docker ECS integration offers the `docker ecs secret` command, so you can manage secrets created on AWS SMS without having to install the AWS CLI.
{% endcomment %}
Docker ECS 統合では、便利なコマンドとして `docker ecs secret` が提供されています。
したがって AWS CLI をインストールしていなくても、AWS SMS において生成した機密情報を管理することができます。

```console
docker ecs secret create dockerhubAccessToken --username <dockerhubuser>  --password <dockerhubtoken>
arn:aws:secretsmanager:eu-west-3:12345:secret:DockerHubAccessToken
```

{% comment %}
Once created, you can use this ARN in you Compose file using using `x-aws-pull_credentials` custom extension with the Docker image URI for your service.
{% endcomment %}
この ARN を生成したら Compose ファイル内において、カスタム拡張 `x-aws-pull_credentials` を利用して、そのサービスに対する Docker イメージ URI を指定することができます。

```console
version: 3.8
services:
  worker:
    image: mycompany/privateimage
    x-aws-pull_credentials: "arn:aws:secretsmanager:eu-west-3:12345:secret:DockerHubAccessToken"
```

{% comment %}
>**Note**
>
> If you set the Compose file version to 3.8 or later, you can use the same Compose file for local deployment using `docker-compose`. Custom extensions will be ignored in this case.
{% endcomment %}
>**メモ**
>
> Compose ファイルのバージョンを 3.8 またはそれ以降に指定すると、この Compose ファイルをそのまま、`docker-compose` を使ってローカル開発環境向けに利用することができます。
その場合、カスタム拡張は無視されます。

{% comment %}
## Service discovery
{% endcomment %}
{: #service-discovery }
## サービスの検出

{% include jatrans.md jatrans_pattern="1" %}

{% comment %}
Service-to-service communication is implemented by the [Security Groups](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-security-groups.html){: target="_blank" class="_"} rules, allowing services sharing a common Compose file “network” to communicate together. This allows individual services to run with distinct constraints (memory, cpu) and replication rules. However, it comes with a constraint that Docker images have to handle service discovery and wait for dependent services to be available.
{% endcomment %}
サービス間の通信は [Security Groups](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-security-groups.html){: target="_blank" class="_"} ルールによって実現されています。
サービス間では共通の Compose ファイル「network」が共有され、互いに通信を行います。
そこでは特定のサービスに対して、異なる（メモリ、CPU の）制約やレプリカに関するルールにもとづいて実行することが可能です。
ただしこれを行うための条件として、Docker イメージがサービスを検出できて、従属サービスの利用可能状態を待つことができなければなりません。

{% comment %}
### Service names
{% endcomment %}
{: #service-names }
### サービス名

{% comment %}
Services are registered by the Docker ECS integration on [AWS Cloud Map](https://docs.aws.amazon.com/cloud-map/latest/dg/what-is-cloud-map.html){: target="_blank" class="_"} during application deployment. They are declared as fully qualified domain names of the form: `<service>.<compose_project_name>.local`. Services can retrieve their dependencies using this fully qualified name, or can just use a short service name (as they do with docker-compose) as Docker ECS integration automatically injects the `LOCALDOMAIN` variable. This works out of the box if your Docker image fully implements domain name resolution standards, otherwise (typically, when using Alpine-based Docker images), you’ll have to include an [entrypoint script](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/#entrypoint) in your Docker image to force this option:
{% endcomment %}
Docker ECS 統合においてアプリケーションデプロイを行うサービスは、[AWS Cloud Map](https://docs.aws.amazon.com/cloud-map/latest/dg/what-is-cloud-map.html){: target="_blank" class="_"} 上に登録されます。
これは `<サービス>.<composeプロジェクト名>.local` という形の完全修飾ドメイン名として表わされています。
サービスが、依存するサービスを引き出す際には、この完全修飾ドメイン名を利用するか、あるいは短いサービス名（docker-compose において用いられるもの）が利用されます。
完全修飾ドメイン名は Docker ECS が自動的に、環境変数 `LOCALDOMAIN` に設定します。
これを使った処理は、Docker イメージにドメイン名解決の機能が実装されていれば、即座に動作します。
（Alpine ベースの Docker イメージを利用している場合など）その機能が実装されていない場合は、Docker イメージに [エンドポイントスクリプト]({{ site.baseurl }}/develop/develop-images/dockerfile_best-practices/#entrypoint) を用意して、そのオプションを有効にする必要があります。

```console
#! /bin/sh

if [ "${LOCALDOMAIN}x" != "x" ]; then echo "search ${LOCALDOMAIN}" >> /etc/resolv.conf; fi
exec "$@"
```

{% comment %}
### Dependent service startup time and DNS resolution
{% endcomment %}
{: #dependent-service-startup-time-and-dns-resolution }
### 依存サービスの起動タイミングと DNS 名前解決

{% comment %}
Services get concurrently scheduled on ECS when a Compose file is deployed. AWS Cloud Map introduces an initial delay for DNS service to be able to resolve your services domain names. As a result, your code needs to be adjusted to support this delay by waiting for dependent services to be ready, or by adding a wait-script as the entrypoint to your Docker image, as documented in [Control startup order](https://docs.docker.com/compose/startup-order/).
{% endcomment %}
ECS 上におけるサービスは、Compose ファイルがデプロイされたと同時にスケジュールされます。
AWS Cloud Map は DNS サービスに対して初期の待機時間を導入していて、デプロイされたサービスのドメイン名をDNS サービスが解決できるまで待機します。
このため、開発コードにはこの待機時間への対応が必要であり、依存サービスが準備状態になるまで待つ、あるいはエントリーポイントにウェイトを行うスクリプトを Docker イメージに追加するなどして、調整を行う必要があります。
このことは [Compose における起動、停止順の制御]({{ site.baseurl }}/compose/startup-order/) で述べています。

{% comment %}
Alternatively, you can use the [depends_on](https://github.com/compose-spec/compose-spec/blob/master/spec.md#depends_on){: target="_blank" class="_"} feature of the Compose file format. By doing this, dependent service will be created first, and application deployment will wait for it to be up and running before starting the creation of the dependent services.
{% endcomment %}
あるいは Compose ファイルにおける [depends_on](https://github.com/compose-spec/compose-spec/blob/master/spec.md#depends_on){: target="_blank" class="_"} の機能を利用することもできます。
これを利用すると、依存サービスがまず初めに生成され、これが起動されるのを待って、アプリケーションのデプロイが行われるようになります。

{% comment %}
## Tuning the CloudFormation template
{% endcomment %}
{: #tuning-the-cloudformation-template }
## CloudFormation テンプレートの調整

{% comment %}
The Docker ECS integration relies on [Amazon CloudFormation](https://docs.aws.amazon.com/cloudformation/){: target="_blank" class="_"} to manage the application deployment. To get more control on the created resources, you can use `docker ecs compose convert` to generate a CloudFormation stack file from your Compose file. This allows you to inspect resources it defines, or customize the template for your needs, and then apply the template to AWS using the AWS CLI, or the AWS web console.
{% endcomment %}
Docker ECS 統合では [Amazon CloudFormation](https://docs.aws.amazon.com/cloudformation/){: target="_blank" class="_"} を活用して、アプリケーションのデプロイ管理を行っています。
生成済みのリソースをより的確に制御するには、`docker ecs compose convert` を使い、Compose ファイルから CloudFormation スタックファイルを生成します。
スタックファイルを生成すると、そこに定義されたリソースの確認や、必要に応じたテンプレートのカスタマイズ、AWS CLI や AWS ウェブコンソールからのテンプレートの適用を行うことができます。

{% comment %}
By default, the Docker ECS integration creates an ECS cluster for your Compose application, a Security Group per network in your Compose file on your AWS account’s default VPC, and a LoadBalancer to route traffic to your services. If your AWS account does not have [permissions](https://github.com/docker/ecs-plugin/blob/master/docs/requirements.md#permissions){: target="_blank" class="_"} to create such resources, or you want to manage these yourself, you can use the following custom Compose extensions:
{% endcomment %}
Docker ECS 統合では、Compose アプリケーションに対して、デフォルトで以下のものを生成します。
1 つは Compose アプリケーション用の ECS クラスターです。
もう 1 つはネットワークごとの SecurityGroup です。
これは AWS でのデフォルト VPC 上において、Compose ファイルによって定義されるネットワークごとのものです。
そしてサービスのトラフィックをルーティングする LoadBlancer です。
AWS アカウントに、そのようなリソースを生成する [パーミッション](https://github.com/docker/ecs-plugin/blob/master/docs/requirements.md#permissions){: target="_blank" class="_"} が与えられていない場合、あるいは独自にこれらを管理したい場合は、以下のカスタム Compose 拡張機能を利用することができます。

{% comment %}
- Use `x-aws-cluster` as a top-level element in your Compose file to set the ARN
of an ECS cluster when deploying a Compose application. Otherwise, a 
cluster will be created for the Compose project.
{% endcomment %}
- Compose ファイルの最上位項目として `x-aws-cluster` を利用します。
  これは Compose アプリケーションのデプロイ時に利用する ECS クラスターの ARN を設定するものです。
  これがない場合、クラスターは Compose プロジェクトに対して生成されます。

{% comment %}
- Use `x-aws-vpc` as a top-level element in your Compose file to set the ARN 
of a VPC when deploying a Compose application.
{% endcomment %}
- Compose ファイルの最上位項目として `x-aws-vpc` を利用します。
  これは Compose アプリケーションのデプロイ時に利用する VPC の ARN を設定します。

{% comment %}
- Use `x-aws-loadbalancer` as a top-level element in your Compose file to set
the ARN of an existing LoadBalancer.
{% endcomment %}
- Compose ファイルの最上位項目として `x-aws-loadbalancer` を利用します。
  これは既存の LoadBalancer の ARN を設定します。

{% comment %}
- Use `x-aws-securitygroup` inside a network definition in your Compose file to
set the ARN of an existing SecurityGroup used to implement network connectivity
between services.
{% endcomment %}
- Compose ファイルのネットワーク定義内において `x-aws-securitygroup` を利用します。
  これは、サービス間のネットワーク接続のために用意されている既存の SecurityGroup の ARN を設定します。

{% comment %}
## Install the Docker ECS Integration CLI on Linux
{% endcomment %}
{: #install-the-docker-ecs-integration-cli-on-linux }
## Linux における Docker ECS 統合 CLI のインストール

{% comment %}
The Docker ECS Integration CLI adds support for running and managing containers on ECS.
{% endcomment %}
Docker ECS 統合の CLI は、ECS 上のコンテナー実行と管理をサポートします。

{% comment %}
>**Note**
>
> Docker ECS Integration is a beta release. The installation process, commands, and flags will change in future releases.
{:.important}
{% endcomment %}
>**メモ**
>
> **Docker ECS 統合はベータ版です**。
> インストール処理、コマンド、フラグは、将来のリリースにおいて変更されることがあります。
{:.important}

{% comment %}
### Prerequisites
{% endcomment %}
{: #prerequisites }
### 前提条件

{% comment %}
[Docker 19.03 or later](https://docs.docker.com/get-docker/)
{% endcomment %}
* [Docker 19.03 またはそれ以降](https://docs.docker.com/get-docker/)

{% comment %}
### Download the plugin
{% endcomment %}
{: #download-the-plugin }
### プラグインのダウンロード

{% comment %}
You can download the Docker ECS plugin from the [docker/ecs-plugin](https://github.com/docker/ecs-plugin){: target="_blank" class="_"} GitHub repository using the following command:
{% endcomment %}
GitHub リポジトリ [docker/ecs-plugin](https://github.com/docker/ecs-plugin){: target="_blank" class="_"} から Docker ECS プラグインをダウンロードすることができます。
ダウンロードには以下のコマンドを実行します。

```console
$ curl -LO https://github.com/docker/ecs-plugin/releases/latest/download/docker-ecs-linux-amd64
```

{% comment %}
You will then need to make it an executable:
{% endcomment %}
入手したらこれを実行可能にします。

```console
$ chmod +x docker-ecs-linux-amd64
```

{% comment %}
### Install the plugin
{% endcomment %}
{: #install-the-plugin }
### プラグインのインストール

{% comment %}
Move the plugin you’ve downloaded to the right place so the Docker CLI can use it:
{% endcomment %}
ダウンロードしたプラグインを Docker CLI が利用できるように、適切な場所に移動させます。

```console
$ mkdir -p /usr/local/lib/docker/cli-plugins

$ mv docker-ecs-linux-amd64 /usr/local/lib/docker/cli-plugins/docker-ecs
```

{% comment %}
You can move the CLI plugin into any of the following directories:
{% endcomment %}
CLI プラグインを移動するディレクトリは、以下の場所でもかまいません。

- `/usr/local/lib/docker/cli-plugins`
- `/usr/local/libexec/docker/cli-plugins`
- `/usr/lib/docker/cli-plugins`
- `/usr/libexec/docker/cli-plugins`

{% comment %}
Finally, you must enable the experimental features on the CLI. You can do this by setting the environment variable `DOCKER_CLI_EXPERIMENTAL=enabled`, or by setting experimental to `enabled` in your Docker config file located at `~/.docker/config.json`:
{% endcomment %}
最後に CLI において試験的機能を有効にする必要があります。
これを行うには、環境変数として `DOCKER_CLI_EXPERIMENTAL=enabled` を設定するか、Docker 設定ファイル `~/.docker/config.json` 内に試験的機能を `enabled` とする設定を行います。

```console
$ export DOCKER_CLI_EXPERIMENTAL=enabled

$ DOCKER_CLI_EXPERIMENTAL=enabled docker help

$ cat ~/.docker/config.json
{
  "experimental" : "enabled",
  "auths" : {
    "https://index.docker.io/v1/" : {

    }
  }
}
```

{% comment %}
You can verify whether the CLI plugin installation is successful by checking whether it appears in the CLI help output, or by checking the plugin version. For example:
{% endcomment %}
CLI プラグインのインストールが適切に行われたかどうかは、CLI のヘルプを表示するか、あるいはプラグインのバージョンを表示して確認します。

```console
$ docker help | grep ecs
  ecs*        Docker ECS (Docker Inc., 0.0.1)

$ docker ecs version
Docker ECS plugin 0.0.1
```

## FAQ

{% comment %}
**What does the error `this tool requires the "new ARN resource ID format"` mean?**
{% endcomment %}
**`this tool requires the "new ARN resource ID format"` というエラーはどんな意味？**

{% comment %}
This error message means that your integration requires the new ARN resource ID format for ECS. To learn more, see [Migrating your Amazon ECS deployment to the new ARN and resource ID format](https://aws.amazon.com/blogs/compute/migrating-your-amazon-ecs-deployment-to-the-new-arn-and-resource-id-format-2/){: target="_blank" class="_"}.
{% endcomment %}
このエラーメッセージは、統合環境において ECS 向けの新たな ARN リソース ID フォーマットが必要であることを示しています。

詳しくは [Migrating your Amazon ECS deployment to the new ARN and resource ID format](https://aws.amazon.com/blogs/compute/migrating-your-amazon-ecs-deployment-to-the-new-arn-and-resource-id-format-2/){: target="_blank" class="_"} を参照してください。

{% comment %}
## Feedback
{% endcomment %}
{: #feedback }
## フィードバック

{% comment %}
{% endcomment %}
Docker ECS 統合ベータ版を利用していただき、ありがとうございます。
みなさんからのフィードバックが大変重要です。
フィードバックをいただくには、Github レポジトリ [ecs-plugin](https://github.com/docker/ecs-plugin){: target="_blank" class="_"} に issue をあげてください。
