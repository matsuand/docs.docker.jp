{% assign platform = include.platform %}

{% comment %}

Include a chunk of this file, using variables already set in the file
where you want to reuse the chunk.

Usage: {% include kubernetes-mac-win.md platform="mac" %}

{% endcomment %}

{% if platform == "mac" %}
  {% assign product = "Docker Desktop for Mac" %}

  {% capture min-version %}{{ product }} 18.06.0-ce-mac70 CE{% endcapture %}

  {% capture version-caveat %}
  **Kubernetes is only available in {{ min-version }} and higher.**
  {% endcapture %}

  {% capture local-kubectl-warning %}
> If you independently installed the Kubernetes CLI, `kubectl`, make sure that
> it is pointing to `docker-desktop` and not some other context such as
> `minikube` or a GKE cluster. Run: `kubectl config use-context docker-desktop`.
> If you experience conflicts with an existing `kubectl` installation, remove `/usr/local/bin/kubectl`.

  {% endcapture %}

  {% assign kubectl-path = "/usr/local/bin/kubectl" %}

{% elsif platform == "windows" %}
  {% assign product = "Docker Desktop for Windows" %}

  {% capture min-version %}{{ product }} 18.06.0-ce-win70 CE{% endcapture %}

  {% capture version-caveat %}
  **Kubernetes is only available in {{ min-version }} and higher.**
  {% endcapture %}

  {% capture local-kubectl-warning %}
  If you installed `kubectl` by another method, and experience conflicts, remove it.
  {% endcapture %}

  {% assign kubectl-path = "C:\>Program Files\Docker\Docker\Resources\bin\kubectl.exe" %}

{% endif %}

{% comment %}
Docker Desktop includes a standalone Kubernetes server and client,
as well as Docker CLI integration. The Kubernetes server runs locally within
your Docker instance, is not configurable, and is a single-node cluster.
{% endcomment %}
Docker Desktop には Docker CLI 統合環境に加えて、Kubernetes のスタンドアロンサーバーとクライアントが含まれます。
Kubernetes サーバーは Docker インスタンス内にローカルに実行されます。
設定変更することはできず、単一ノードクラスターとして動作します。

{% comment %}
The Kubernetes server runs within a Docker container on your local system, and
is only for local testing. When Kubernetes support is enabled, you can deploy
your workloads, in parallel, on Kubernetes, Swarm, and as standalone containers.
Enabling or disabling the Kubernetes server does not affect your other
workloads.
{% endcomment %}
Kubernetes サーバーは、ローカルシステム内の Docker コンテナー内部で稼動します。
ローカル環境でのテスト用として利用するものです。
Kubernetes サポートが有効である場合、Kubernetes と Swarm へ同時並行により開発内容をデプロイし、スタンドアロンコンテナーとすることができます。
Kubernetes サーバーの有効、無効は、他の開発内容へは影響しません。

{% comment %}
See [{{ product }} > Getting started](/docker-for-{{ platform }}/#kubernetes) to
enable Kubernetes and begin testing the deployment of your workloads on
Kubernetes.
{% endcomment %}
Kubernetes 有効化の方法、Kubernetes に開発内容をデプロイしてテストを行う方法については [{{ product }} > はじめよう](/docker-for-{{ platform }}/#kubernetes) を参照してください。

{{ kubectl-warning }}

{% comment %}
## Use Docker commands
{% endcomment %}
{: #use-docker-commands }
## Docker コマンドの利用

{% comment %}
You can deploy a stack on Kubernetes with `docker stack deploy`, the
`docker-compose.yml` file, and the name of the stack.
{% endcomment %}
Kubernetes に対して Stack をデプロイすることができます。
その際には `docker stack deploy`、`docker-compose.yml` ファイル、Stack 名を用います。

```bash
docker stack deploy --compose-file /path/to/docker-compose.yml mystack
docker stack services mystack
```

{% comment %}
You can see the service deployed with the `kubectl get services` command.
{% endcomment %}
`kubectl get services` コマンドを使うと、デプロイされているサービスを確認することができます。

{% comment %}
### Specify a namespace
{% endcomment %}
{: #specify-a-namespace }
### 名前空間の指定

{% comment %}
By default, the `default` namespace is used. You can specify a namespace with
the `--namespace` flag.
{% endcomment %}
デフォルトにおいて `default` という名前空間が用いられます。
名前空間は `--namespace` フラグを使って指定することができます。

```bash
docker stack deploy --namespace my-app --compose-file /path/to/docker-compose.yml mystack
```

{% comment %}
Run `kubectl get services -n my-app` to see only the services deployed in the
`my-app` namespace.
{% endcomment %}
`kubectl get services -n my-app` を実行すると、`my-app` 名前空間内にデプロイされたサービスのみを確認することができます。

{% comment %}
### Override the default orchestrator
{% endcomment %}
{: #override-the-default-orchestrator }
### デフォルトオーケストレーターのオーバーライド

{% comment %}
While testing Kubernetes, you may want to deploy some workloads in swarm mode.
Use the `DOCKER_STACK_ORCHESTRATOR` variable to override the default orchestrator for
a given terminal session or a single Docker command. This variable can be unset
(the default, in which case Kubernetes is the orchestrator) or set to `swarm` or
`kubernetes`. The following command overrides the orchestrator for a single
deployment, by setting the variable{% if platform == "mac"" %}
at the start of the command itself.

```bash
DOCKER_STACK_ORCHESTRATOR=swarm docker stack deploy --compose-file /path/to/docker-compose.yml mystack
```{% elsif platform == "windows" %}
before running the command.

```shell
set DOCKER_STACK_ORCHESTRATOR=swarm
docker stack deploy --compose-file /path/to/docker-compose.yml mystack
```

{% endif %}
{% endcomment %}
Kubernetes をテストする際に、開発内容を Swarm にデプロイしたいことがあります。
操作を行っているターミナルのセッション内、あるいは 1 つの Docker コマンド内において、環境変数 `DOCKER_STACK_ORCHESTRATOR` を使うと、デフォルトのオーケストレーターをオーバーライドすることができます。
この変数は未指定とする（これがデフォルトであり、Kubernetes がオーケストレーターとなる）か、あるいは `swarm` や `kubernetes` に指定します。
以下のコマンドは 1 つのデプロイメントにおいてオーケストレーターをオーバーライドします。
{% if platform == "mac"" %}これが行われるのはコマンドの実行時です。

```bash
DOCKER_STACK_ORCHESTRATOR=swarm docker stack deploy --compose-file /path/to/docker-compose.yml mystack
```{% elsif platform == "windows" %}
これはコマンド実行前に行われます。

```shell
set DOCKER_STACK_ORCHESTRATOR=swarm
docker stack deploy --compose-file /path/to/docker-compose.yml mystack
```

{% endif %}

{% comment %}
Alternatively, the `--orchestrator` flag may be set to `swarm` or `kubernetes`
when deploying to override the default orchestrator for that deployment.
{% endcomment %}
別のやり方として `--orchestrator` フラグを利用して `swarm` や `kubernetes` に設定することで、デプロイの際にデフォルトオーケストレーターをオーバーライドする方法もあります。

```bash
docker stack deploy --orchestrator swarm --compose-file /path/to/docker-compose.yml mystack
```

{% comment %}
> **Note**
>
> Deploying the same app in Kubernetes and swarm mode may lead to conflicts with
> ports and service names.
{% endcomment %}
> **メモ**
>
> 1 つのアプリを Kubernetes と Swarm の両方にデプロイしてしまうと、ポートやサービス名が衝突することがあります。

{% comment %}
## Use the kubectl command
{% endcomment %}
{: #use-the-kubectl-command }
## kubectl コマンドの利用

{% comment %}
The {{ platform }} Kubernetes integration provides the Kubernetes CLI command
at `{{ kubectl-path }}`. This location may not be in your shell's `PATH`
variable, so you may need to type the full path of the command or add it to
the `PATH`. For more information about `kubectl`, see the
[official `kubectl` documentation](https://kubernetes.io/docs/reference/kubectl/overview/).
You can test the command by listing the available nodes:
{% endcomment %}
{{ platform }} における Kubernetes 統合環境では、`{{ kubectl-path }}` に Kubernetes CLI コマンドが提供されています。
このパスは、利用しているシェルの `PATH` 変数には含まれていないかもしれません。
そこでコマンド実行時にはフルパスを指定するか、`PATH` 設定に加えることが必要になります。
`kubectl` に関する詳細は [公式の `kubectl` ドキュメント](https://kubernetes.io/docs/reference/kubectl/overview/) を参照してください。
以下のコマンドを実行すれば、利用可能なノード一覧が得られます。

```bash
kubectl get nodes

NAME                 STATUS    ROLES     AGE       VERSION
docker-desktop       Ready     master    3h        v1.8.2
```

{% comment %}
## Example app
{% endcomment %}
{: #example-app }
## サンプルアプリ

{% comment %}
Docker has created the following demo app that you can deploy to swarm mode or
to Kubernetes using the `docker stack deploy` command.
{% endcomment %}
Docker では以下のようなデモアプリを用意しています。
`docker stack deploy` コマンドを使って、Swarm へのデプロイ、Kubernetes へのデプロイを行うことができます。

```yaml
version: '3.3'

services:
  web:
    image: dockersamples/k8s-wordsmith-web
    ports:
     - "80:80"

  words:
    image: dockersamples/k8s-wordsmith-api
    deploy:
      replicas: 5
      endpoint_mode: dnsrr
      resources:
        limits:
          memory: 50M
        reservations:
          memory: 50M

  db:
    image: dockersamples/k8s-wordsmith-db
```

{% comment %}
If you already have a Kubernetes YAML file, you can deploy it using the
`kubectl` command.
{% endcomment %}
Kubernetes YAML ファイルがすでにある場合は、`kubectl` コマンドを使ってデプロイを行うことができます。
