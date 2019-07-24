---
title: Docker Assemble (試験的)
description: Docker Assemble のインストール。
keywords: Assemble, Docker Enterprise, plugin, Spring Boot, .NET, c#, F#
---

{% comment %}
>This is an experimental feature.
>
>{% include experimental.md %}
{% endcomment %}
>これは試験的な機能です。
>
>{% include experimental.md %}

{% comment %}
## Overview
{% endcomment %}
{: #overview }
## 概要

{% comment %}
Docker Assemble (`docker assemble`) is a plugin which provides a language and framework-aware tool that enables users to build an application into an optimized Docker container. With Docker Assemble, users can quickly build Docker images without providing configuration information (like Dockerfile) by auto-detecting the required information from existing framework configuration.
{% endcomment %}
Docker Assemble (`docker assemble`) は言語指向（language-aware）あるいはフレームワーク指向（framework-aware）のツールを提供するプラグインです。
これを用いることで、最適化した Docker コンテナー内にアプリケーションを構築することができます。
Docker Assemble では（Dockerfile のような）設定情報がなくても、現状のフレームワーク設定から必要な情報を自動的に取得して、すばやく Docker イメージが構築できるようになります。

{% comment %}
Docker Assemble supports the following application frameworks:
{% endcomment %}
Docker Assemble では以下のアプリケーションフレームワークをサポートしています。

{% comment %}
- [Spring Boot](https://spring.io/projects/spring-boot) when using the [Maven](https://maven.apache.org/) build system
{% endcomment %}
- [Spring Boot](https://spring.io/projects/spring-boot)、[Maven](https://maven.apache.org/) ビルドシステムの利用時。

{% comment %}
- [ASP.NET Core](https://docs.microsoft.com/en-us/aspnet/core) (with C# and F#)
{% endcomment %}
- [ASP.NET Core](https://docs.microsoft.com/en-us/aspnet/core) (C# または F#)

{% comment %}
## System requirements
{% endcomment %}
{: #system-requirements }
## システム要件

{% comment %}
Docker Assemble requires a Linux, Windows, or a macOS Mojave with the Docker Engine installed.
{% endcomment %}
Docker Assemble を利用するには Docker Engine がインストールされている Linux、Windows、macOS Mojave が必要です。

{% comment %}
## Install
{% endcomment %}
{: #install }
## インストール

{% comment %}
Docker Assemble requires its own buildkit instance to be running in a Docker container on the local system. You can start and manage the backend using the `backend` subcommand of `docker assemble`.
{% endcomment %}
Docker Assemble は、ローカルシステム上の Docker コンテナーにて稼動する独自の buildkit インスタンスを必要とします。
`docker assemble` のサブコマンド `backend` を使えば、バックエンドの起動と管理を行うことができます。

{% comment %}
To start the backend, run:
{% endcomment %}
バックエンドを起動するには以下を実行します。

```
~$ docker assemble backend start`
Pulling image «…»: Success
Started backend container "docker-assemble-backend-username" (3e627bb365a4)
```

{% comment %}
When the backend is running, it can be used for multiple builds and does not need to be restarted.
{% endcomment %}
バックエンドが起動してればマルチビルドが利用できるため、再起動は必要なくなります。

{% comment %}
> **Note:** For instructions on running a remote backend, accessing logs, saving the build cache in a named volume, accessing a host port, and for information about the buildkit instance, see `--help` .
{% endcomment %}
> **メモ:** リモートバックエンドの起動方法、ログのアクセス、名前つきボリューム内のビルドキャッシュの保存、ホストポートへのアクセス、buildkit インストールに関する情報などに関しては `--help` を確認してください。

{% comment %}
For advanced backend user information, see [Advanced Backend Management](/assemble/adv-backend-manage/).
{% endcomment %}
バックエンドのユーザー情報については [応用的なバックエンド管理](/assemble/adv-backend-manage/) を参照してください。
