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
    Alternatively, install the [Docker ECS Integration for Linux](#install-the-docker-ecs-integration-cli-on-linux).
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
Docker not only runs multi-container applications locally, but also enables 
developers to seamlessly deploy Docker containers on Amazon ECS using a 
Compose file with the `docker compose up` command. The following sections 
contain instructions on how to deploy your Compose application on Amazon ECS.
{% endcomment %}
Docker は、単にローカルのマルチコンテナーを実行するだけのものではなくなります。
`docker compose up` により Compose ファイルを使って Amazon ECS 上の Docker コンテナーをシームレスにデプロイできます。
以下の節では Amazon ECS 上において Docker コンテナーをデプロイする手順を説明します。

{% comment %}
### Create AWS context
{% endcomment %}
{: #create-aws-context }
### AWS コンテキストの生成

{% comment %}
Run the `docker context create ecs myecscontext` command to create an Amazon ECS docker 
context named `myecscontext`. If you have already installed and configured the AWS CLI, 
the setup command lets you select an existing AWS profile to connect to Amazon. 
Otherwise, you can create a new profile by passing an 
[AWS access key ID and a secret access key](https://docs.aws.amazon.com/general/latest/gr/aws-sec-cred-types.html#access-keys-and-secret-access-keys){: target="_blank" class="_"}.
{% endcomment %}
`docker context create ecs myecscontext` コマンドを実行して AWS コンテキストを生成します。
すでに AWS CLI をインストールし設定を行っているのであれば、このセットアップコマンドにより、既存の AWS プロファイルを選んで Amazon への接続を行います。
これがまだできていない場合は、[AWS access key ID and a secret access key](https://docs.aws.amazon.com/general/latest/gr/aws-sec-cred-types.html#access-keys-and-secret-access-keys){: target="_blank" class="_"} を通じて、新たなプロファイルを生成します。

{% comment %}
After you have created an AWS context, you can list your Docker contexts by running the `docker context ls` command:
{% endcomment %}
AWS コンテキストを生成したら、`docker context ls` コマンドを実行して Docker コンテキストの一覧を表示します。

```console
NAME   DESCRIPTION  DOCKER ENDPOINT  KUBERNETES ENDPOINT ORCHESTRATOR
myecscontext *
default  Current DOCKER_HOST based configuration   unix:///var/run/docker.sock     swarm
```

{% comment %}
## Run Compose applications
{% endcomment %}
{: #run-compose-applications }
## Compose アプリケーションの実行

{% comment %}
You can deploy and manage multi-container applications defined in Compose files
to Amazon ECS using the `docker compose` command. To do this:
{% endcomment %}
Compose ファイルに定義したマルチコンテナーによるアプリケーションを、Amazon ECS に対してデプロイし管理するためには、`docker compose` コマンドを実行します。
これを行うためには以下のことが必要です。

{% comment %}
- Ensure you are using your ECS context. You can do this either by specifying 
the `--context myecscontext` flag with your command, or by setting the 
current context using the command `docker context use myecscontext`.
{% endcomment %}
- AWS コンテキストを利用していることが必要です。
  これはコマンド実行時に `--context myecscontext` フラグを指定するか、あるいはカレントなコンテキストを設定するコマンド  `docker context use myecscontext` を実行します。

{% comment %}
- Run `docker compose up` and `docker compose down` to start and then 
stop a full Compose application.
{% endcomment %}
- `docker compose up` と `docker compose down` を実行して、Compose アプリケーションの開始、停止ができることが必要です。

  {% comment %}
  By default, `docker compose up` uses the `docker-compose.yaml` file in 
  the current folder. You can specify the Compose file directly using the 
  `--file` flag.
  {% endcomment %}
  デフォルトにおいて `docker compose up` は、カレントフォルダーの `docker-compose.yaml` ファイルを利用します。
  Compose ファイルを直接 `--file` フラグを使って指定することもできます。

  {% comment %}
  You can also specify a name for the Compose application using the `--project-name` flag during deployment. If no name is specified, a name will be derived from the working directory.
  {% endcomment %}
  Compose アプリケーションのデプロイの際に `--project-name` フラグを使って、アプリケーション名を指定することもできます。
  アプリケーション名の指定がなかった場合は、ワーキングディレクトリから名前が定められます。

{% comment %}
- You can view services created for the Compose application on Amazon ECS and 
their state using the `docker compose ps` command.
{% endcomment %}
- Amazon ECS 上の Compose アプリケーションに対して生成されたサービスの情報およびその状態を確認するには `docker compose ps` コマンドを実行します。

{% comment %}
- You can view logs from containers that are part of the Compose application 
using the `docker compose logs` command.
{% endcomment %}
- Compose アプリケーションを構成するコンテナーに対しては、コマンド `docker compose logs` を実行してそれぞれのログを確認できます。

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
For your convenience, Docker ECS integration offers the `docker secret` command, so you can manage secrets created on AWS SMS without having to install the AWS CLI.
{% endcomment %}
Docker ECS 統合では、便利なコマンドとして `docker secret` が提供されています。
したがって AWS CLI をインストールしていなくても、AWS SMS において生成した機密情報を管理することができます。

```console
docker secret create dockerhubAccessToken --username <dockerhubuser>  --password <dockerhubtoken>
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

### Secrets

{% comment %}
You can pass secrets to your ECS services using Docker model to bind sensitive 
data as files under `/run/secrets`. If your Compose file declares a secret as 
file, such a secret will be created as part of your application deployment on 
ECS. If you use an existing secret as `external: true` reference in your 
Compose file, use the ECS Secrets Manager full ARN as the secret name:
{% endcomment %}
Docker モデルを使って ECS サービスに Secret 情報を受け渡すことにより、`/run/secrets` 配下にあるファイルを機密データとして結びつけることができます。
Compose ファイルに Secret 情報を宣言している場合、ECS 上へのアプリケーションのデプロイにあたって、デプロイの一部としてその情報が生成されます。
Compose ファイル内にて既存の Secret 情報を `external: true` として参照している場合、Secret 名として ECS Secrets Manager の 完全リソース名を使うことができます。
```yaml
services:
  webapp:
    image: ...
    secrets:
      - foo

secrets:
  foo:
    name: "arn:aws:secretsmanager:eu-west-3:1234:secret:foo-ABC123"
```

{% comment %}
Secrets will be available at runtime for your service as a plain text file `/run/secrets/foo`.
{% endcomment %}
Secret はサービス実行時には、`/run/secrets/foo` というプレーンなテキストファイルとしてアクセスできます。

{% comment %}
The AWS Secrets Manager allows you to store sensitive data either as a plain 
text (like Docker secret does), or as a hierarchical JSON document. You can 
use the latter with ECS integration by using custom field `x-asw-keys` to 
define which entries in the JSON document to bind as a secret in your service 
container.
{% endcomment %}
AWS Secrets マネージャーにおいて機密情報を保存する際には、（Docker の Secret と同じように）プレーンテキストとして保存する方法と、階層化した JSON ドキュメントとして保存する方法があります。
ECS 統合環境内にて後者の方法を利用する場合には、カスタム項目 `x-asw-keys` を利用して、サービスコンテナー内の Secret として、JSON ドキュメント内のどの項目を結びつけるかを定義します。

```yaml
services:
  webapp:
    image: ...
    secrets:
      - foo

secrets:
  foo:
    name: "arn:aws:secretsmanager:eu-west-3:1234:secret:foo-ABC123"
    keys: 
      - "bar"
```

{% comment %}
By doing this, the secret for `bar` key will be available at runtime for your 
service as a plain text file `/run/secrets/foo/bar`. You can use the special 
value `*` to get all keys bound in your container. 
{% endcomment %}
このようにするとキー `bar` に対する機密データは、サービスの実行時に `/run/secrets/foo/bar` というプレーンなテキストファイルとしてアクセスできます。
特別な値として `*` を用いると、コンテナー内のキーすべてを得ることができます。

{% comment %}
### Logging
{% endcomment %}
{: #logging }
### ログ処理

{% comment %}
The ECS integration configures AWS CloudWatch Logs service for your containers. 
A log group is created for the application as `docker-compose/<application_name>`, 
and log streams are created for each service and container in your application 
as `<application_name>/<service_name>/<container_ID>`.
{% endcomment %}
ECS 統合環境では、コンテナーに対して AWS CloudWatch ログサービスを設定します。
アプリケーションにおいて、ロググループ `docker-compose/<アプリケーション名>` が生成され、またアプリケーション内の各サービスとコンテナーにおいては、ログストリーム `<アプリケーション名>/<サービス名>/<コンテナーID>` が生成されます。

{% comment %}
You can fine tune AWS CloudWatch Logs using extension field `x-aws-logs_retention` 
in your Compose file to set the number of retention days for log events. The 
default behaviour is to keep logs forever.
{% endcomment %}
AWS CloudWatch ログに対しては Compose ファイル内の拡張項目 `x-aws-logs_retention` を使って、イベントの保存日数を調整することができます。
デフォルトは無期限に保存します。

{% comment %}
You can also pass `awslogs` driver parameters to your container as standard 
Compose file `logging.driver_opts` elements.
{% endcomment %}
標準的な Compose ファイルの項目 `logging.driver_opts` を使えば、コンテナーに対して `awslogs` ドライバーのパラメーターを指定することができます。

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
### Rolling update
{% endcomment %}
{: #rolling-update }
### ローリングアップデート

{% comment %}
Your ECS services are created with rolling update configuration. As you run 
`docker compose up` with a modified Compose file, the stack will be 
updated to reflect changes, and if required, some services will be replaced. 
This replacement process will follow the rolling-update configuration set by 
your services [`deploy.update_config`](https://docs.docker.com/compose/compose-file/#update_config) 
configuration. 
{% endcomment %}
ECS サービスはローリングアップデート設定を含めて生成されます。
Compose ファイルを修正した上で `docker compose up` を実行すると、その修正に応じてアップデートが行われ、必要なサービスは置き換えられます。
この置き換え処理は、サービスの [`deploy.update_config`](https://docs.docker.com/compose/compose-file/#update_config)  設定によって定められるローリングアップデート設定に従います。

{% comment %}
AWS ECS uses a percent-based model to define the number of containers to be 
run or shut down during a rolling update. The ECS integration computes 
rolling update configuration according to the `prallelism` and `replicas` 
fields. However, you might prefer to directly configure a rolling update 
using the extension fields `x-aws-min_percent` and `x-aws-max_percent`. 
The former sets the minimum percent of containers to run for service, and the 
latter sets the maximum percent of additional containers to start before 
previous versions are removed.
{% endcomment %}
AWS ECS ではパーセントベースモデル（percent-based model）を採用して、ローリングアップデート時に起動または停止するコンテナー数を定義しています。
ECS 統合環境では項目 `prallelism` または `replicas` に従って、ローリングアップデート設定を算出しています。
ローリングアップデートの設定を、項目 `x-aws-min_percent` や `x-aws-max_percent` を使って設定したい場合があります。
`x-aws-min_percent` はサービスに対して、起動させるコンテナーの最小パーセントを設定します。
`x-aws-max_percent` は、それまでのバージョンのコンテナーを削除する前に、追加で起動するコンテナーの最大パーセントを設定します。

{% comment %}
By default, the ECS rolling update is set to run twice the number of 
containers for a service (200%), and has the ability to shut down 100% 
containers during the update.
{% endcomment %}
デフォルトにおいて ECS のローリングアップデートは、コンテナー数の 2 倍（200%）の数だけ起動されるように設定されます。
またアップデート時にコンテナー停止を行う程度は 100 % として設定されます。


{% comment %}
### IAM roles
{% endcomment %}
{: #iam-roles }
### IAM ロール

{% comment %}
Your ECS Tasks are executed with a dedicated IAM role, granting access 
to AWS Managed policies[`AmazonECSTaskExecutionRolePolicy`](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/task_execution_IAM_role.html) 
and [`AmazonEC2ContainerRegistryReadOnly`](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/ecr_managed_policies.html). 
In addition, if your service uses secrets, IAM Role gets additional 
permissions to read and decrypt secrets from the AWS Secret Manager.
{% endcomment %}
ECS タスクは、AWS 管理ポリシー[`AmazonECSTaskExecutionRolePolicy`](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/task_execution_IAM_role.html) と [`AmazonEC2ContainerRegistryReadOnly`](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/ecr_managed_policies.html) へのアクセスが可能な、専用の IAM ロールによって実行されます。
さらにサービスが Secret を利用している場合、IAM ロールは追加の権限によって AWS Secret マネージャーから Secret の読み込みと復号化を行います。

{% comment %}
You can grant additional managed policies to your service execution 
by using `x-aws-policies` inside a service definition:
{% endcomment %}
サービスの実行にあたって管理ポリシーを追加で利用するには、サービス定義の内部において `x-aws-policies` を用います。

```yaml
services:
  foo:
    x-aws-policies:
      - "arn:aws:iam::aws:policy/AmazonS3FullAccess"
```

{% comment %}
You can also write your own [IAM Policy Document](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/task-iam-roles.html) 
to fine tune the IAM role to be applied to your ECS service, and use 
`x-aws-role` inside a service definition to pass the 
yaml-formatted policy document.
{% endcomment %}
また独自に [IAM ポリシードキュメント](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/task-iam-roles.html) を記述して、ECS サービスに適用する IAM ロールを調整することができます。
そしてサービス定義内に `x-aws-role` を用いることで、YAML 書式のポリシードキュメントを受け渡すことができます。

```yaml
services:
  foo:
    x-aws-role:
      Version: "2012-10-17"
      Statement: 
        - Effect: "Allow"
          Action: 
            - "some_aws_service"
          Resource": 
            - "*"
```

{% comment %}
## Tuning the CloudFormation template
{% endcomment %}
{: #tuning-the-cloudformation-template }
## CloudFormation テンプレートの調整

{% comment %}
The Docker ECS integration relies on [Amazon CloudFormation](https://docs.aws.amazon.com/cloudformation/){: target="_blank" class="_"} to manage the application deployment. To get more control on the created resources, you can use `docker compose convert` to generate a CloudFormation stack file from your Compose file. This allows you to inspect resources it defines, or customize the template for your needs, and then apply the template to AWS using the AWS CLI, or the AWS web console.
{% endcomment %}
Docker ECS 統合では [Amazon CloudFormation](https://docs.aws.amazon.com/cloudformation/){: target="_blank" class="_"} を活用して、アプリケーションのデプロイ管理を行っています。
生成済みのリソースをより的確に制御するには、`docker compose convert` を使い、Compose ファイルから CloudFormation スタックファイルを生成します。
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
### Install script
{% endcomment %}
{: #install-script }
### インストールスクリプト

{% comment %}
You can install the new CLI using the install script:
{% endcomment %}
インストールスクリプトを使えば、新たな CLI をインストールできます。

```console
curl -L https://raw.githubusercontent.com/docker/aci-integration-beta/main/scripts/install_linux.sh | sh
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
