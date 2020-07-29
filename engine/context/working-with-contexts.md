---
title: Docker Context
description: Docker コンテキストについて学びます。
keywords: engine, context, cli, kubernetes
---


{% comment %}
## Introduction
{% endcomment %}
{: #introduction }
## はじめに

{% comment %}
This guide shows how _contexts_ make it easy for a **single Docker CLI** to manage multiple Swarm clusters, multiple Kubernetes clusters, and multiple individual Docker nodes.
{% endcomment %}
本ガイドでは **コンテキスト**（context）というものが、いかに簡単に、**単一の Docker CLI** から複数 Swarm クラスターを、そして複数 Kubernetes クラスター、各 Docker ノードを操作できるかを示します。

{% comment %}
A single Docker CLI can have multiple contexts. Each context contains all of the endpoint and security information required to manage a different cluster or node. The `docker context` command makes it easy to configure these contexts and switch between them.
{% endcomment %}
1 つの Docker CLI に対しては、複数のコンテキスト（context）を持たせることができます。
各コンテキストには、さまざまなクラスターやノードの管理に必要となる、エンドポイント情報やセキュリティ情報が含まれています。
`docker context` コマンドを使うと、そういったコンテキストを簡単に設定したり切り替えたりすることができます。

{% comment %}
As an example, a single Docker client on your company laptop might be configured with two contexts; **dev-k8s** and **prod-swarm**. **dev-k8s** contains the endpoint data and security credentials to configure and manage a Kubernetes cluster in a development environment. **prod-swarm** contains everything required to manage a Swarm cluster in a production environment. Once these contexts are configured, you can use the top-level `docker context use <context-name>` to easily switch between them.
{% endcomment %}
その例として、会社で利用するノート PC 上において、1 つの Docker クライアントから 2 つのコンテキスト、**dev-k8s** と **prod-swarm** の利用設定が必要であるとします。
**dev-k8s** は、開発環境上の Kubernetes クラスターの設定管理を行うものであり、エンドポイントデータやセキュリティ情報が含まれています。
一方 **prod-swarm** は、本番環境上の Swarm クラスター管理を一手に引き受けているものとします。
コンテキストの設定を一度行ってしまえば、トップレベルのコマンド `docker context use <コンテキスト名>` を実行するだけで、コンテキストを簡単に切り替えられるようになります。

{% comment %}
For information on using Docker Context to deploy your apps to the cloud, see [Deploying Docker containers on Azure](aci-integration.md) and [Deploying Docker containers on ECS](ecs-integration.md).
{% endcomment %}
Docker Context を利用し、開発アプリをクラウド上にデプロイする方法については、[Azure 上の Docker コンテナーのデプロイ](aci-integration.md) や [ECS 上への Docker コンテナーのデプロイ](ecs-integration.md) を参照してください。

{% comment %}
## Prerequisites
{% endcomment %}
{: #prerequisites }
## 前提条件

{% comment %}
To follow the examples in this guide, you'll need:
{% endcomment %}
本ガイドの利用例を進めていくためには、以下が必要になります。

{% comment %}
- A Docker client that supports the top-level `context` command
{% endcomment %}
- トップレベルコマンド `context` をサポートしている Docker クライアント。

{% comment %}
Run `docker context` to verify that your Docker client supports contexts.
{% endcomment %}
`docker context` を実行して、Docker クライアントがコンテキストをサポートしていることを確認してください。

{% comment %}
You will also need one of the following:
{% endcomment %}
また以下のいずれかが必要です。

{% comment %}
- Docker Swarm cluster
- Single-engine Docker node
- Kubernetes cluster
{% endcomment %}
- Docker Swarm クラスター
- 単独エンジンによる Docker クラスター
- Kubernetes クラスター

{% comment %}
## The anatomy of a context
{% endcomment %}
{: #the-anatomy-of-a-context }
## コンテキストの意味

{% comment %}
A context is a combination of several properties. These include:
{% endcomment %}
コンテキストとは、複数プロパティが組み合わせれたものです。
そこには以下の情報が含まれます。

{% comment %}
- Name
- Endpoint configuration
- TLS info
- Orchestrator
{% endcomment %}
- 名前
- エンドポイント設定
- TLS 情報
- オーケストレーター

{% comment %}
The easiest way to see what a context looks like is to view the **default** context.
{% endcomment %}
コンテキストというものがどのようなものに見えるかは、**default** コンテキストを見てみれば一番よくわかります。

```
$ docker context ls
NAME          DESCRIPTION     DOCKER ENDPOINT                KUBERNETES ENDPOINT      ORCHESTRATOR
default *     Current...      unix:///var/run/docker.sock                             swarm
```

{% comment %}
This shows a single context called "default". It's configured to talk to a Swarm cluster through the local `/var/run/docker.sock` Unix socket. It has no Kubernetes endpoint configured.
{% endcomment %}
上の結果より、「default」という単一のコンテキストがあることがわかります。
これは Swarm クラスターとやりとりするものとして設定されていて、ローカルの Unix ソケット `/var/run/docker.sock` を利用して実現するものとなっています。
ここには Kubernetes のエンドポイントは設定されていません。

{% comment %}
The asterisk in the `NAME` column indicates that this is the active context. This means all `docker` commands will be executed against the "default" context unless overridden with environment variables such as `DOCKER_HOST` and `DOCKER_CONTEXT`, or on the command-line with the `--context` and `--host` flags.
{% endcomment %}
`NAME` 列の横に示されるアスタリスクは、そのコンテキストがアクティブであることを表わします。
これはつまり `docker` コマンドがすべて、この「default」コンテキストに対して実行されることを意味します。
`DOCKER_HOST` や `DOCKER_CONTEXT` といった環境変数を使うか、コマンドラインから `--context` や `--host` フラグを指定することで、その対象をオーバーライドすることができます。

{% comment %}
Dig a bit deeper with `docker context inspect`. In this example, we're inspecting the context called `default`.
{% endcomment %}
さらに詳しく見るには `docker context inspect` を実行します。
この例においては、`default` というコンテキストを確認しています。

```
$ docker context inspect default
[
    {
        "Name": "default",
        "Metadata": {
            "StackOrchestrator": "swarm"
        },
        "Endpoints": {
            "docker": {
                "Host": "unix:///var/run/docker.sock",
                "SkipTLSVerify": false
            }
        },
        "TLSMaterial": {},
        "Storage": {
            "MetadataPath": "\u003cIN MEMORY\u003e",
            "TLSPath": "\u003cIN MEMORY\u003e"
        }
    }
]
```

{% comment %}
This context is using "swarm" as the orchestrator (`metadata.stackOrchestrator`). It is configured to talk to an endpoint exposed on a local Unix socket at `/var/run/docker.sock` (`Endpoints.docker.Host`), and requires TLS verification (`Endpoints.docker.SkipTLSVerify`).
{% endcomment %}
このコンテキストでは、オーケストレーター（`metadata.stackOrchestrator`）として「swarm」を利用しています。
また `/var/run/docker.sock` にあるローカル Unix ソケットを使い、利用可能なエンドポイントとの間でやりとりを行うように設定されています（`Endpoints.docker.Host`）。
そして TLS 検証を必要とする設定もあります（`Endpoints.docker.SkipTLSVerify`）。

{% comment %}
### Create a new context
{% endcomment %}
{: #create-a-new-context }
### 新たなコンテキストの生成

{% comment %}
You can create new contexts with the `docker context create` command.
{% endcomment %}
新たなコンテキストを生成するには `docker context create` コマンドを実行します。

{% comment %}
The following example creates a new context called "docker-test" and specifies the following:
{% endcomment %}
以下の例では「docker-test」というコンテキストを生成するものとし、以下を設定するものとします。

{% comment %}
- Default orchestrator = Swarm
- Issue commands to the local Unix socket `/var/run/docker.sock`
{% endcomment %}
- デフォルトのオーケストレーターを Swarm とします。
- ローカル Unix ソケット `/var/run/docker.sock` に対してコマンド実行するものとします。

```
$ docker context create docker-test \
  --default-stack-orchestrator=swarm \
  --docker host=unix:///var/run/docker.sock

Successfully created context "docker-test"
```

{% comment %}
The new context is stored in a `meta.json` file below `~/.docker/contexts/`. Each new context you create gets its own `meta.json` stored in a dedicated sub-directory of `~/.docker/contexts/`.
{% endcomment %}
新たなコンテキストは `~/.docker/contexts/` 配下の `meta.json` 内に保存されます。
コンテキストを新規に生成すると、`~/.docker/contexts/` に専用のサブディレクトリが生成され、それぞれの `meta.json` が保存されるものです。

{% comment %}
> **Note:** The default context behaves differently than manually created contexts. It does not have a `meta.json` configuration file, and it dynamically updates based on the current configuration. For example, if you switch your current Kubernetes config using `kubectl config use-context`, the default Docker context will dynamically update itself to the new Kubernetes endpoint.
{% endcomment %}
> **メモ:** デフォルトのコンテキストは、手動で生成したコンテキストとは、多少違った動きをします。
デフォルトのコンテキストには、設定ファイル `meta.json` が存在しません。
そしてその時点の設定に基づいて、動的に設定が更新されます。
たとえばコマンド `kubectl config use-context` を実行して、現在の Kubernetes 設定を切り替えたとします。
この場合 Docker のデフォルトコンテキストは、新たに Kubernetes エンドポイントに対応して、動的にコンテキスト自体を更新します。

{% comment %}
You can view the new context with `docker context ls` and `docker context inspect <context-name>`.
{% endcomment %}
新たに生成したコンテキストは、`docker context ls` や `docker context inspect <コンテキスト名>` により確認することができます。

{% comment %}
The following can be used to create a config with Kubernetes as the default orchestrator using the existing kubeconfig stored in `/home/ubuntu/.kube/config`. For this to work, you will need a valid kubeconfig file in `/home/ubuntu/.kube/config`. If your kubeconfig has more than one context, the current context (`kubectl config current-context`) will be used.
{% endcomment %}
以下のコマンドは新たな設定を生成するものであり、`/home/ubuntu/.kube/config` に保存されている既存の kubeconfig を使って、デフォルトのオーケストレーターを Kubernetes とする設定を行うものです。
これを動作させるためには、`/home/ubuntu/.kube/config` に Kubernetes の適切な設定ファイルが存在している必要があります。
kubeconfig に複数のコンテキストが設定されている場合、現在のコンテキスト（`kubectl config current-context`）が用いられます。

```
$ docker context create k8s-test \
  --default-stack-orchestrator=kubernetes \
  --kubernetes config-file=/home/ubuntu/.kube/config \
  --docker host=unix:///var/run/docker.sock

Successfully created context "k8s-test"
```

{% comment %}
You can view all contexts on the system with `docker context ls`.
{% endcomment %}
システム上にあるコンテキストをすべて確認するには `docker context ls` を実行します。

```
$ docker context ls
NAME           DESCRIPTION   DOCKER ENDPOINT               KUBERNETES ENDPOINT               ORCHESTRATOR
default *      Current       unix:///var/run/docker.sock   https://35.226.99.100 (default)   swarm
k8s-test                     unix:///var/run/docker.sock   https://35.226.99.100 (default)   kubernetes
docker-test                  unix:///var/run/docker.sock                                     swarm
```

{% comment %}
The current context is indicated with an asterisk ("\*").
{% endcomment %}
カレントなコンテキストはアスタリスク（"\*"）により示されます。

{% comment %}
## Use a different context
{% endcomment %}
{: #use-a-different-context }
## 別のコンテキストへの切り替え

{% comment %}
You can use `docker context use` to quickly switch between contexts.
{% endcomment %}
`docker context use` コマンドを利用すれば、コンテキストの切り替えをすばやく行うことができます。

{% comment %}
The following command will switch the `docker` CLI to use the "k8s-test" context.
{% endcomment %}
以下のコマンドは `docker` CLI が「k8s-test」コンテキストを利用するように切り替えるものです。

```
$ docker context use k8s-test

k8s-test
Current context is now "k8s-test"
```

{% comment %}
Verify the operation by listing all contexts and ensuring the asterisk ("\*") is against the "k8s-test" context.
{% endcomment %}
処理結果を確認するため、コンテキストをすべて一覧表示して、「k8s-test」コンテキストにアスタリスク（"\*"）がついているかどうかを見てみます。

```
$ docker context ls
NAME            DESCRIPTION                               DOCKER ENDPOINT               KUBERNETES ENDPOINT               ORCHESTRATOR
default         Current DOCKER_HOST based configuration   unix:///var/run/docker.sock   https://35.226.99.100 (default)   swarm
docker-test                                               unix:///var/run/docker.sock                                     swarm
k8s-test *                                                unix:///var/run/docker.sock   https://35.226.99.100 (default)   kubernetes
```

{% comment %}
`docker` commands will now target endpoints defined in the "k8s-test" context.
{% endcomment %}
`docker` コマンドはこれ以降、「k8s-test」コンテキストに定義されたエンドポイントを操作対象とします。

{% comment %}
You can also set the current context using the `DOCKER_CONTEXT` environment variable. This overrides the context set with `docker context use`.
{% endcomment %}
コンテキストの設定は、環境変数 `DOCKER_CONTEXT` を使って行うこともできます。
これは `docker context use` によって設定されたコンテキストをオーバーライドします。

{% comment %}
Use the appropriate command below to set the context to `docker-test` using an environment variable.
{% endcomment %}
以下の中から適切なコマンドを選んで、環境変数を利用した `docker-test` コンテキストの設定を行います。

{% comment %}
Windows PowerShell:
{% endcomment %}
Windows PowerShell の場合

```
> $Env:DOCKER_CONTEXT=docker-test
```

{% comment %}
Linux:
{% endcomment %}
Linux の場合

```
$ export DOCKER_CONTEXT=docker-test
```

{% comment %}
Run a `docker context ls` to verify that the "docker-test" context is now the active context.
{% endcomment %}
`docker context ls` を実行すると、「docker-test」コンテキストが現在のアクティブコンテキストであることがわかります。

{% comment %}
You can also use the global `--context` flag to override the context specified by the `DOCKER_CONTEXT` environment variable. For example, the following will send the command to a context called "production".
{% endcomment %}
またグローバルな `--context` フラグを使うと `DOCKER_CONTEXT` 環境変数によるコンテキスト指定ををオーバーライドします。
たとえば以下は、コマンドに対して「production」というコンテキストを指定します。

```
$ docker --context production container ls
```

{% comment %}
## Exporting and importing Docker contexts
{% endcomment %}
{: #exporting-and-importing-docker-contexts }
## Docker Context のインポート、エクスポート

{% comment %}
The `docker context` command makes it easy to export and import contexts on different machines with the Docker client installed.
{% endcomment %}
`docker context` コマンドでは、Docker クライアントがインストールされている別のマシンにおいて、コンテキストを簡単にインポート、エクスポートすることができます。

{% comment %}
You can use the `docker context export` command to export an existing context to a file. This file can later be imported on another machine that has the `docker` client installed.
{% endcomment %}
`docker context export` コマンドを使うと、既存のコンテキストをファイルにエクスポートします。
そのファイルは、`docker` クライアントがインストールされている別のマシンにインポートすることができます。

{% comment %}
By default, contexts will be exported as a _native Docker contexts_. You can export and import these using the `docker context` command. If the context you are exporting includes a Kubernetes endpoint, the Kubernetes part of the context will be included in the `export` and `import` operations.
{% endcomment %}
デフォルトでコンテキストは **ネイティブな Docker Context** としてエクスポートされます。
これは `docker context` コマンドを使って、インポートエクスポートができるものです。
エクスポートしたコンテキストに Kubernetes エンドポイントが含まれている場合、その Kubernetes 部分のコンテキストも `export`、`import` 操作を通じて含まれることになります。

{% comment %}
There is also an option to export just the Kubernetes part of a context. This will produce a native kubeconfig file that can be manually merged with an existing `~/.kube/config` file on another host that has `kubectl` installed. You cannot export just the Kubernetes portion of a context and then import it with `docker context import`. The only way to import the exported Kubernetes config is to manually merge it into an existing kubeconfig file.
{% endcomment %}
エクスポートにあたっては Kubernetes 部分のコンテキストだけをエクスポートすることもできます。
これにより、ネイティブな kubeconfig ファイルが生成され、そのファイル内容は、`kubectl` がインストールされている他のホスト上において、`~/.kube/config` ファイルへ手動でマージすることができます。
Kubernetes 部分のコンテキストだけをエクスポートし、これをインポートする場合は、`docker context import` コマンドは使えません。
エクスポートされた Kubernetes 設定をインポートするには、既存の kubeconfig ファイルに対して、手動でマージする以外に方法はありません。

{% comment %}
Let's look at exporting and importing a native Docker context.
{% endcomment %}
ネイティブな Docker コンテキストのエクスポートとインポートを以降に示します。

{% comment %}
### Exporting and importing a native Docker context
{% endcomment %}
{: #exporting-and-importing-a-native-docker-context }
### ネイティブな Docker コンテキストのエクスポートとインポート

{% comment %}
The following example exports an existing context called "docker-test". It will be written to a file called `docker-test.dockercontext`.
{% endcomment %}
以下の例では「docker-test」という既存のコンテキストをエクスポートします。
これはコンテキストを `docker-test.dockercontext` というファイルに出力します。

```
$ docker context export docker-test
Written file "docker-test.dockercontext"
```

{% comment %}
Check the contents of the export file.
{% endcomment %}
エクスポートしたファイルの中身を確認します。

```
$ cat docker-test.dockercontext
meta.json0000644000000000000000000000022300000000000011023 0ustar0000000000000000{"Name":"docker-test","Metadata":{"StackOrchestrator":"swarm"},"Endpoints":{"docker":{"Host":"unix:///var/run/docker.sock","SkipTLSVerify":false}}}tls0000700000000000000000000000000000000000000007716 5ustar0000000000000000
```

{% comment %}
This file can be imported on another host using `docker context import`. The target host must have the Docker client installed.
{% endcomment %}
このファイルは、別のホスト上において `docker context import` を使ってインポートできます。
対象とするホストには、Docker クライアントがインストールされている必要があります。

```
$ docker context import docker-test docker-test.dockercontext
docker-test
Successfully imported context "docker-test"
```

{% comment %}
You can verify that the context was imported with `docker context ls`.
{% endcomment %}
コンテキストがインポートできたかどうかは `docker context ls` により確認します。

{% comment %}
The format of the import command is `docker context import <context-name> <context-file>`.
{% endcomment %}
インポートするコマンドの記述書式は `docker context import <コンテキスト名> <コンテキストファイル>` です。

{% comment %}
Now, let's look at exporting just the Kubernetes parts of a context.
{% endcomment %}
次は、コンテキストの Kubernetes 部分のみをエクスポートする例です。

{% comment %}
### Exporting a Kubernetes context
{% endcomment %}
{: #exporting-a-Kubernetes-context }
### Kubernetes コンテキストのエクスポート

{% comment %}
You can export a Kubernetes context only if the context you are exporting has a Kubernetes endpoint configured. You cannot import a Kubernetes context using `docker context import`.
{% endcomment %}
Kubernetes コンテキストがエクスポートできるのは、エクスポートするコンテキスト内に Kubernetes エンドポイントが設定されている場合に限ります。
Kubernetes コンテキストのインポートには `docker context import` を使うことはできません。

{% comment %}
These steps will use the `--kubeconfig` flag to export **only** the Kubernetes elements of the existing `k8s-test` context to a file called "k8s-test.kubeconfig". The `cat` command will then show that it's exported as a valid kubeconfig file.
{% endcomment %}
この手順では `--kubeconfig` フラグを使って Kubernetes 部分のコンテキスト **のみ** をエクスポートします。
コンテキスト名は既存の `k8s-test` であり、これを「k8s-test.kubeconfig」というファイルに出力します。
`cat` コマンドを実行すれば、適正な kubeconfig ファイルとして出力されていることがわかります。

```
$ docker context export k8s-test --kubeconfig
Written file "k8s-test.kubeconfig"
```

{% comment %}
Verify that the exported file contains a valid kubectl config.
{% endcomment %}
エクスポートしたファイルが、適正な kubectl 設定であることを確認します。

```
$ cat k8s-test.kubeconfig
apiVersion: v1
clusters:
- cluster:
    certificate-authority-data:
    <Snip>
    server: https://35.226.99.100
  name: cluster
contexts:
- context:
    cluster: cluster
    namespace: default
    user: authInfo
  name: context
current-context: context
kind: Config
preferences: {}
users:
- name: authInfo
  user:
    auth-provider:
      config:
        cmd-args: config config-helper --format=json
        cmd-path: /snap/google-cloud-sdk/77/bin/gcloud
        expiry-key: '{.credential.token_expiry}'
        token-key: '{.credential.access_token}'
      name: gcp
```

{% comment %}
You can merge this with an existing `~/.kube/config` file on another machine.
{% endcomment %}
上の内容を、別のマシン上の `~/.kube/config` ファイルにマージします。

{% comment %}
## Updating a context
{% endcomment %}
{: #updating-a-context }
## コンテキストの更新

{% comment %}
You can use `docker context update` to update fields in an existing context.
{% endcomment %}
既存のコンテキストにおいて項目を更新するには `docker context update` を実行します。

{% comment %}
The following example updates the "Description" field in the existing `k8s-test` context.
{% endcomment %}
以下の例は、既存の `k8s-test` コンテキストにおいて「Description」という項目を更新するものです。

```
$ docker context update k8s-test --description "Test Kubernetes cluster"
k8s-test
Successfully updated context "k8s-test"
```
