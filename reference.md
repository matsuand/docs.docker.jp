---
title: Reference documentation
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
| ファイルフォーマット                                                | 内容                                                            |
|:--------------------------------------------------------------------|:----------------------------------------------------------------|
| [Dockerfile](/engine/reference/builder/)                            | Defines the contents and startup behavior of a single container |
| [Compose file](/compose/compose-file/)                              | Defines a multi-container application                           |


{% comment %}
## Command-line interfaces (CLIs)
{% endcomment %}
{: #command-line-interfaces-clis }
## コマンドラインインターフェース（Command-line interfaces; CLI）

{% comment %}
| CLI                                                           | Description                                                                                                     |
|:--------------------------------------------------------------|:----------------------------------------------------------------------------------------------------------------|
| [Docker CLI](/engine/reference/commandline/cli/)              | The main CLI for Docker, includes all `docker` commands |
| [Compose CLI](/compose/reference/overview/)                   | The CLI for Docker Compose, which allows you to build and run multi-container applications                      |
| [Daemon CLI (dockerd)](/engine/reference/commandline/dockerd/)                            | Persistent process that manages containers                                                 |
{% endcomment %}
| CLI                                                           | Description                                                                                                     |
|:--------------------------------------------------------------|:----------------------------------------------------------------------------------------------------------------|
| [Docker CLI](/engine/reference/commandline/cli/)              | The main CLI for Docker, includes all `docker` commands |
| [Compose CLI](/compose/reference/overview/)                   | The CLI for Docker Compose, which allows you to build and run multi-container applications                      |
| [Daemon CLI (dockerd)](/engine/reference/commandline/dockerd/)                            | Persistent process that manages containers                                                 |


{% comment %}
## Application programming interfaces (APIs)
{% endcomment %}
{: #application-programming-interfaces-APIs }
## アプリケーションプログラミングインターフェース（API）

| API                                                   | Description                                                                            |
|:------------------------------------------------------|:---------------------------------------------------------------------------------------|
| [Engine API](/engine/api/)                            | The main API for Docker, provides programmatic access to a daemon |
| [Registry API](/registry/spec/api/)                   | Facilitates distribution of images to the engine                                       |
| [Template API](app-template/api-reference)| Allows users to create new Docker applications by using a library of templates.|

{% comment %}
## Drivers and specifications
{% endcomment %}
{: #drivers-and-specifications }
## ドライバーとその仕様

| Driver                                                 | Description                                                                        |
|:-------------------------------------------------------|:-----------------------------------------------------------------------------------|
| [Image specification](/registry/spec/manifest-v2-2/)   | Describes the various components of a Docker image                                 |
| [Registry token authentication](/registry/spec/auth/)  | Outlines the Docker registry authentication scheme                                 |
| [Registry storage drivers](/registry/storage-drivers/) | Enables support for given cloud providers when storing images with Registry        |
