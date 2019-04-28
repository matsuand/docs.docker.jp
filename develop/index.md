---
title: Docker を用いた開発
description: Overview of developer resources
keywords: developer, developing, apps, api, sdk
---

<!--
This page lists resources for application developers using Docker.
-->
このページは Docker アプリケーションの開発者に向けた情報を示します。

<!--
## Develop new apps on Docker
-->
## Docker 上での新たなアプリ開発

<!--
If you're just getting started developing a brand new app on Docker, check out
these resources to understand some of the most common patterns for getting the
most benefits from Docker.
-->
Docker を使って新しいアプリを開発しようとしている方は、以下のような情報を確認し、Docker を効率よく利用する開発パターンについて理解してください。

<!--
- Learn to [build an image from a Dockerfile](/get-started/part2.md){: target="_blank" class="_"}
- Use [multistage builds](/engine/userguide/eng-image/multistage-build.md){: target="_blank" class="_"} to keep your images lean
- Manage application data using [volumes](/engine/admin/volumes/volumes.md) and [bind mounts](/engine/admin/volumes/bind-mounts.md){: target="_blank" class="_"}
- [Scale your app](/get-started/part3.md){: target="_blank" class="_"} as a swarm service
- [Define your app stack](/get-started/part5.md){: target="_blank" class="_"} using a compose file
- General application development best practices
-->
- [Dockerfile からイメージをビルドする](/get-started/part2.md){: target="_blank" class="_"}方法について学ぶ。
- Use [multistage builds](/engine/userguide/eng-image/multistage-build.md){: target="_blank" class="_"} to keep your images lean
- Manage application data using [volumes](/engine/admin/volumes/volumes.md) and [bind mounts](/engine/admin/volumes/bind-mounts.md){: target="_blank" class="_"}
- [Scale your app](/get-started/part3.md){: target="_blank" class="_"} as a swarm service
- [Define your app stack](/get-started/part5.md){: target="_blank" class="_"} using a compose file
- General application development best practices

<!--
## Learn about language-specific app development with Docker
-->
## 特定言語でのDocker アプリ開発について学ぶ

- [Docker for Java developers](https://github.com/docker/labs/tree/master/developer-tools/java/){: target="_blank" class="_"} lab
- [Port a node.js app to Docker](https://github.com/docker/labs/tree/master/developer-tools/nodejs/porting){: target="_blank" class="_"}
- [Ruby on Rails app on Docker](https://github.com/docker/labs/tree/master/developer-tools/ruby){: target="_blank" class="_"} lab
- [Dockerize a .Net Core application](/engine/examples/dotnetcore/){: target="_blank" class="_"}
- [Dockerize an ASP.NET Core application with SQL Server on Linux](/compose/aspnet-mssql-compose/){: target="_blank" class="_"} using Docker Compose

## Advanced development with the SDK or API

After you can write Dockerfiles or Compose files and use Docker CLI, take it to the next level by using Docker Engine SDK for Go/Python or use the HTTP API directly.
