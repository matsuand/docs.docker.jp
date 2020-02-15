---
title: Docker を用いた開発
description: 開発リソースの概要。
keywords: developer, developing, apps, api, sdk
---

{% comment %}
This page contains a list of resources for application developers who would like to build new applications using Docker.
{% endcomment %}
This page contains a list of resources for application developers who would like to build new applications using Docker.

{% comment %}
## Prerequisites
{% endcomment %}
## Prerequisites

{% comment %}
Work through the learning modules in [Get started](/get-started/index.md) to understand how to build an image and run it as a containerized application.
{% endcomment %}
Work through the learning modules in [Get started](/get-started/index.md) to understand how to build an image and run it as a containerized application.

{% comment %}
## Develop new apps on Docker
{% endcomment %}
{: #develop-new-apps-on-docker }
## Docker 上での新たなアプリ開発

{% comment %}
If you're just getting started developing a brand new app on Docker, check out
these resources to understand some of the most common patterns for getting the
most benefits from Docker.
{% endcomment %}
Docker を使って新しいアプリを開発しようとしている方は、以下のような情報を確認し、Docker を効率よく利用する開発パターンについて理解してください。

{% comment %}
- Use [multistage builds](/engine/userguide/eng-image/multistage-build.md){: target="_blank" class="_"} to keep your images lean
- Manage application data using [volumes](/engine/admin/volumes/volumes.md) and [bind mounts](/engine/admin/volumes/bind-mounts.md){: target="_blank" class="_"}
- [Scale your app](/get-started/kube-deploy.md){: target="_blank" class="_"} with kubernetes
- [Scale your app](/get-started/swarm-deploy.md){: target="_blank" class="_"} as a swarm service
- [General application development best practices](/develop/dev-best-practices.md){: target="_blank" class="_"}
{% endcomment %}
- [Dockerfile からイメージをビルドする](/get-started/part2.md){: target="_blank" class="_"}方法について学ぶ。
- Use [multistage builds](/engine/userguide/eng-image/multistage-build.md){: target="_blank" class="_"} to keep your images lean
- Manage application data using [volumes](/engine/admin/volumes/volumes.md) and [bind mounts](/engine/admin/volumes/bind-mounts.md){: target="_blank" class="_"}
- [Scale your app](/get-started/kube-deploy.md){: target="_blank" class="_"} with kubernetes
- [Scale your app](/get-started/swarm-deploy.md){: target="_blank" class="_"} as a swarm service
- [General application development best practices](/develop/dev-best-practices.md){: target="_blank" class="_"}

{% comment %}
## Learn about language-specific app development with Docker
{% endcomment %}
{: #learn-about-language-specific-app-development-with-docker }
## 特定言語でのDocker アプリ開発について学ぶ

{% comment %}
- [Docker for Java developers](https://github.com/docker/labs/tree/master/developer-tools/java/){: target="_blank" class="_"} lab
- [Port a node.js app to Docker](https://github.com/docker/labs/tree/master/developer-tools/nodejs/porting){: target="_blank" class="_"}
- [Ruby on Rails app on Docker](https://github.com/docker/labs/tree/master/developer-tools/ruby){: target="_blank" class="_"} lab
- [Dockerize a .Net Core application](/engine/examples/dotnetcore/){: target="_blank" class="_"}
- [Dockerize an ASP.NET Core application with SQL Server on Linux](/compose/aspnet-mssql-compose/){: target="_blank" class="_"} using Docker Compose
{% endcomment %}
- [Docker for Java developers](https://github.com/docker/labs/tree/master/developer-tools/java/){: target="_blank" class="_"} lab
- [Port a node.js app to Docker](https://github.com/docker/labs/tree/master/developer-tools/nodejs/porting){: target="_blank" class="_"}
- [Ruby on Rails app on Docker](https://github.com/docker/labs/tree/master/developer-tools/ruby){: target="_blank" class="_"} lab
- [Dockerize a .Net Core application](/engine/examples/dotnetcore/){: target="_blank" class="_"}
- [Dockerize an ASP.NET Core application with SQL Server on Linux](/compose/aspnet-mssql-compose/){: target="_blank" class="_"} using Docker Compose

{% comment %}
## Advanced development with the SDK or API
{% endcomment %}
{: #advanced-development-with-the-sdk-or-api }
## Advanced development with the SDK or API

{% comment %}
After you can write Dockerfiles or Compose files and use Docker CLI, take it to the next level by using Docker Engine SDK for Go/Python or use the HTTP API directly.
{% endcomment %}
After you can write Dockerfiles or Compose files and use Docker CLI, take it to the next level by using Docker Engine SDK for Go/Python or use the HTTP API directly.
