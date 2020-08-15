---
description: Download rate limit
keywords: Docker, pull requests, download, limit,
title: ダウンロードレート制限
---

{% comment %}
Docker has enabled download rate limits for downloads and pull requests on Docker Hub.
{% endcomment %}
Docker では Docker Hub におけるダウンロードやプルリクエストに対して、ダウンロードレート制限を有効にしています。

{% comment %}
A Docker image contains multiple layers. Each layer in a pull request represents a download object. For example, when you download the latest Python image from Docker Hub, you’ll be downloading eight layers and indexes. The download rate limit introduced by Docker caps the number of objects that users can download within a specified timeframe. Any downloads beyond this limit will result in the `429 Too Many Requests` error message.
{% endcomment %}
1 つの Docker イメージには複数のレイヤーがあります。
プルリクエスト内の各レイヤーは、1 つのダウンロードオブジェクトとなります。
たとえば Docker Hub から最新の Python イメージをダウンロードするとします。
その場合、8 つのレイヤーとインデックスがダウンロードされます。
Docker が導入しているダウンロードレート制限は、指定された時間内にユーザーがダウンロードできるオブジェクト数を制限するものです。
この制限数を越えてダウンロードを行った場合は、`429 Too Many Requests` のエラーメッセージが返されます。

{% comment %}
Docker will gradually impose download rate limits with an eventual limit of  300 downloads per six hours for anonymous users.
{% endcomment %}
Docker は匿名ユーザーのダウンロードに対しては、ダウンロードレート制限を段階的に増やしていき、最終的には 6 時間あたり 100 回のプルまでの制限とします。

{% comment %}
Logged in users will not be affected at this time. Therefore, we recommend that you log into [Docker Hub](https://hub.docker.com/){: target="_blank" class="_"} as an authenticated user. For more information, see the following section [How do I authenticate pull requests](#how-do-i-authenticate-pull-requests).
{% endcomment %}
ログインしているユーザーには、この時間は影響しません。
したがって利用する際には、認証されたユーザーとして [Docker Hub](https://hub.docker.com/){: target="_blank" class="_"} にログインしておくことをお勧めします。
詳しくは以下の [プルリクエストの認証方法](#how-do-i-authenticate-pull-requests) の節を参照してください。

{% comment %}
## How do I authenticate pull requests
{% endcomment %}
{: #how-do-I-authenticate-pull-requests }
## プルリクエストの認証方法

{% comment %}
The following section contains information on how to log into on Docker Hub to authenticate pull requests.
{% endcomment %}
以下の節では、Docker Hub にログインして、プルリクエストを認証する方法を説明します。

### Docker Desktop

{% comment %}
If you are using Docker Desktop, you can log into Docker Hub from the Docker Desktop menu.
{% endcomment %}
Docker Desktop を利用している場合、Docker Desktop メニューから Docker Hub にログインすることができます。

{% comment %}
Click **Sign in / Create Docker ID** from the Docker Desktop menu and follow the on-screen instructions to complete the sign-in process.
{% endcomment %}
Docker Desktop メニューから **Sign in / Create Docker ID** をクリックして、画面内の指示に従って、サインイン操作を完了させます。

### Docker Engine

{% comment %}
If you are using a standalone version of Docker Engine, run the `docker login` command from a terminal to authenticate with Docker Hub. For information on how to use the command, see [docker login](../engine/reference/commandline/login.md).
{% endcomment %}
Docker Engine のスタンドアロン版を利用している場合は、ターミナル画面から `docker login` コマンドを実行して Docker Hub の認証を取得します。
このコマンドの使い方については [docker login](../engine/reference/commandline/login.md) を参照してください。

### Docker Swarm

{% comment %}
If you are running Docker Swarm, you must use the `-- with-registry-auth` flag to authenticate with Docker Hub. For more information, see [docker service create](../engine/reference/commandline/service_create.md/#create-a-service). If you are using a Docker Compose file to deploy an application stack, see [docker stack deploy](../engine/reference/commandline/stack_deploy.md).
{% endcomment %}
Docker Swarm を実行している場合、Docker Hub における認証を得るためには `-- with-registry-auth` フラグを用いる必要があります。
詳しくは [docker service create](../engine/reference/commandline/service_create.md/#create-a-service) を参照してください。
Docker Compose ファイルを使ってアプリケーションをデプロイしている場合は [docker stack deploy](../engine/reference/commandline/stack_deploy.md) を参照してください。

{% comment %}
### GitHub Actions
{% endcomment %}
{: #gitHub-actions }
### GitHub Action

{% comment %}
If you are using GitHub Actions to build and push Docker images to Docker Hub, see [username](https://github.com/docker/build-push-action#username){: target="_blank" class="_"}. If you are using another Action, you must add your username and access token in a similar way for authentication.
{% endcomment %}
GitHub の Action を利用して、Docker Hub における Docker イメージのビルドとプッシュを行っている場合は、[username](https://github.com/docker/build-push-action#username){: target="_blank" class="_"} を参照してください。
これとは別の Action を利用している場合、認証と同じようにユーザー名とアクセストークンを追加する必要があります。

### Kubernetes

{% comment %}
If you are running Kubernetes, follow the instructions in [Pull an Image from a Private Registry](https://kubernetes.io/docs/tasks/configure-pod-container/pull-image-private-registry/){: target="_blank" class="_"} for information on authentication.
{% endcomment %}
Kubernetes を実行している場合、認証に関する情報は [Pull an Image from a Private Registry](https://kubernetes.io/docs/tasks/configure-pod-container/pull-image-private-registry/){: target="_blank" class="_"} に示されている手順に従ってください。

{% comment %}
### Third-party platforms
{% endcomment %}
{: #third-party-platforms }
### サードパーティー製のプラットフォーム

{% comment %}
If you are using any third-party platforms, follow your provider’s instructions on using registry authentication.
{% endcomment %}
サードパーティー製のプラットフォームを利用している場合は、各プロバイダーから提供される、レジストリ認証を利用する手順に従ってください。

{% comment %}
{% endcomment %}
- [CircleCI](https://circleci.com/docs/2.0/private-images/){: target="_blank" class="_"}
- [Drone.io](https://docs.drone.io/pipeline/docker/syntax/images/#pulling-private-images){: target="_blank" class="_"}
- [Codefresh](https://codefresh.io/docs/docs/docker-registries/external-docker-registries/docker-hub/){: target="_blank" class="_"}
- [AWS ECS/Fargate](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/private-auth.html){: target="_blank" class="_"}
- [AWS CodeBuild](https://aws.amazon.com/blogs/devops/how-to-use-docker-images-from-a-private-registry-in-aws-codebuild-for-your-build-environment/){: target="_blank" class="_"}
