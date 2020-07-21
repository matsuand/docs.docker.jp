---
title: リファレンス
notoc: true
---

{% comment %}
This section includes the reference documentation for the Docker platform's
various APIs, CLIs, and file formats.
{% endcomment %}
この節では Docker プラットフォームにおけるさまざまな API、CLI、ファイルフォーマットのリファレンスを示します。

{% comment %}
## File formats
{% endcomment %}
{: #file-formats }
## ファイルフォーマット

{% comment %}
| File format                                                         | Description                                                     |
|:--------------------------------------------------------------------|:----------------------------------------------------------------|
| [Dockerfile](/engine/reference/builder/)                            | Defines the contents and startup behavior of a single container |
| [Compose file](/compose/compose-file/)                              | Defines a multi-container application                           |
{% endcomment %}
| ファイルフォーマット                                                | 内容説明                                                        |
|:--------------------------------------------------------------------|:----------------------------------------------------------------|
| [Dockerfile]({{ site.baseurl }}/engine/reference/builder/)          | 1 つのコンテナーの内容と起動時の動作を定義します。              |
| [Compose file]({{ site.baseurl }}/compose/compose-file/)            | 複数コンテナーによるアプリケーションを定義します。              |


{% comment %}
## Command-line interfaces (CLIs)
{% endcomment %}
{: #command-line-interfaces-clis }
## コマンドラインインターフェース（CLI）

{% comment %}
| CLI                                                           | Description                                                                                                     |
|:--------------------------------------------------------------|:----------------------------------------------------------------------------------------------------------------|
| [Docker CLI](/engine/reference/commandline/cli/)              | The main CLI for Docker, includes all `docker` commands |
| [Compose CLI](/compose/reference/overview/)                   | The CLI for Docker Compose, which allows you to build and run multi-container applications                      |
| [Daemon CLI (dockerd)](/engine/reference/commandline/dockerd/)                            | Persistent process that manages containers                                                 |
{% endcomment %}
| CLI                                                                              | 内容説明                                                                                        |
|:---------------------------------------------------------------------------------|:------------------------------------------------------------------------------------------------|
| [Docker CLI]({{ site.baseurl }}/engine/reference/commandline/cli/)               | Docker の主要な CLI です。`docker` コマンドすべてがここにあります。                             |
| [Compose CLI]({{ site.baseurl }}/compose/reference/overview/)                    | Docker Compose の CLI です。これにより複数コンテナーのアプリケーションのビルドと実行ができます。|
| [Daemon CLI (dockerd)]({{ site.baseurl }}/engine/reference/commandline/dockerd/) | コンテナーを管理する常駐プロセスです。                                                          |


{% comment %}
## Application programming interfaces (APIs)
{% endcomment %}
{: #application-programming-interfaces-APIs }
## アプリケーションプログラミングインターフェース（API）

{% comment %}
| API                                                   | Description                                                                            |
|:------------------------------------------------------|:---------------------------------------------------------------------------------------|
| [Engine API](/engine/api/)                            | The main API for Docker, provides programmatic access to a daemon |
| [Registry API](/registry/spec/api/)                   | Facilitates distribution of images to the engine                                       |
| [Template API](app-template/api-reference)| Allows users to create new Docker applications by using a library of templates.|
{% endcomment %}
| API                                                           | 内容説明                                                                               |
|:--------------------------------------------------------------|:---------------------------------------------------------------------------------------|
| [Engine API]({{ site.baseurl }}/engine/api/)                  | Docker の主要な API です。プログラムを通じてデーモンにアクセスします。|
| [Registry API]({{ site.baseurl }}/registry/spec/api/)         | Docker Engine に対してイメージ配布の機能を提供します。                                 |

{% comment %}
## Drivers and specifications
{% endcomment %}
{: #drivers-and-specifications }
## ドライバーとその仕様

{% comment %}
| Driver                                                 | Description                                                                        |
|:-------------------------------------------------------|:-----------------------------------------------------------------------------------|
| [Image specification](/registry/spec/manifest-v2-2/)   | Describes the various components of a Docker image                                 |
| [Registry token authentication](/registry/spec/auth/)  | Outlines the Docker registry authentication scheme                                 |
| [Registry storage drivers](/registry/storage-drivers/) | Enables support for given cloud providers when storing images with Registry        |
{% endcomment %}
| ドライバー                                                                     | 内容説明                                                                              |
|:-------------------------------------------------------------------------------|:--------------------------------------------------------------------------------------|
| [イメージ仕様]({{ site.baseurl }}/registry/spec/manifest-v2-2/)                | Docker イメージのさまざまなコンポーネントを説明しています。                           |
| [レジストリトークン認証]({{ site.baseurl }}/registry/spec/auth/)               | Docker レジストリの認証スキームについて概観しています。                               |
| [レジストリストレージドライバー]({{ site.baseurl }}/registry/storage-drivers/) | レジストリにイメージを保存するための、クラウドプロバイダー向けサポートを可能にします。|
