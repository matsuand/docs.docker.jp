---
title: リファレンス
notoc: true
---

{% comment %}
This section includes the reference documentation for the Docker platform's
various APIs, CLIs, and file formats.
{% endcomment %}
This section includes the reference documentation for the Docker platform's
various APIs, CLIs, and file formats.

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
| [Dockerfile]({{ site.baseurl }}/engine/reference/builder/)                            | Defines the contents and startup behavior of a single container |
| [Compose file]({{ site.baseurl }}/compose/compose-file/)                              | Defines a multi-container application                           |


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
| CLI                                                           | 内容説明                                                                                                        |
|:--------------------------------------------------------------|:----------------------------------------------------------------------------------------------------------------|
| [Docker CLI]({{ site.baseurl }}/engine/reference/commandline/cli/)              | The main CLI for Docker, includes all `docker` commands |
| [Compose CLI]({{ site.baseurl }}/compose/reference/overview/)                   | The CLI for Docker Compose, which allows you to build and run multi-container applications                      |
| [Daemon CLI (dockerd)]({{ site.baseurl }}/engine/reference/commandline/dockerd/)                            | Persistent process that manages containers                                                 |


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
| API                                                   | 内容説明                                                                               |
|:------------------------------------------------------|:---------------------------------------------------------------------------------------|
| [Engine API]({{ site.baseurl }}/engine/api/)                            | The main API for Docker, provides programmatic access to a daemon |
| [Registry API]({{ site.baseurl }}/registry/spec/api/)                   | Facilitates distribution of images to the engine                                       |
| [Template API]({{ site.baseurl }}/app-template/api-reference)| Allows users to create new Docker applications by using a library of templates.|

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
| ドライバー                                             | 内容説明                                                                           |
|:-------------------------------------------------------|:-----------------------------------------------------------------------------------|
| [Image specification]({{ site.baseurl }}/registry/spec/manifest-v2-2/)   | Describes the various components of a Docker image                                 |
| [Registry token authentication]({{ site.baseurl }}/registry/spec/auth/)  | Outlines the Docker registry authentication scheme                                 |
| [Registry storage drivers]({{ site.baseurl }}/registry/storage-drivers/) | Enables support for given cloud providers when storing images with Registry        |
