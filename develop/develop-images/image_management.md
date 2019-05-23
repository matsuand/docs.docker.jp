---
redirect_from:
- /reference/api/hub_registry_spec/
- /userguide/image_management/
- /engine/userguide/eng-image/image_management/
description: Docker Registry や Registry API についての説明。
keywords: docker, registry, api,  hub
title: イメージの管理
---

{% comment %}
The easiest way to make your images available for use by others inside or
outside your organization is to use a Docker registry, such as [Docker Hub](#docker-hub),
[Docker Trusted Registry](#docker-trusted-registry), or
by running your own [private registry](#docker-registry).
{% endcomment %}
構築したイメージを、組織の内外を問わず他の方に提供しようと思ったら、Docker registry を利用するのが最も手っ取り早い方法です。
Docker registry には [Docker Hub](#docker-hub)、[Docker Trusted Registry](#docker-trusted-registry) があり、[独自のレジストリ](#docker-registry)を運用することもできます。


## Docker Hub

{% comment %}
[Docker Hub](/docker-hub/) is a public registry managed by Docker, Inc. It
centralizes information about organizations, user accounts, and images. It
includes a web UI, authentication and authorization using organizations, CLI and
API access using commands such as `docker login`, `docker pull`, and `docker
push`, comments, stars, search, and more.
{% endcomment %}
[Docker Hub](/docker-hub/) は Docker 社が管理する公開レジストリです。
ここに組織情報、ユーザーアカウント、イメージが一元管理できます。
このレジストリではウェブ UI 画面、組織情報を使った認証と承認、`docker login`、`docker pull`、`docker
push`といったコマンドを使った CLI と API によるアクセス、コメント、スター付け、検索など、数多くの機能を提供します。

## Docker Registry

{% comment %}
The Docker Registry is a component of Docker's ecosystem. A registry is a
storage and content delivery system, holding named Docker images, available in
different tagged versions. For example, the image `distribution/registry`, with
tags `2.0` and `latest`. Users interact with a registry by using docker push and
pull commands such as `docker pull myregistry.com/stevvooe/batman:voice`.
{% endcomment %}
Docker Registry は Docker エコシステムのコンポーネントの 1 つです。
そもそもレジストリとはストレージとデータ内容を提供するシステムです。
名前づけされた Docker イメージを保持し、さまざまなタグバージョンの利用を可能にします。
たとえば`distribution/registry`というイメージに、タグ`2.0`と`latest`をつけることができます。
レジストリを利用する場合は`docker pull myregistry.com/stevvooe/batman:voice`などのようにして Docker のプッシュおよびプルコマンドを使います。

{% comment %}
Docker Hub is an instance of a Docker Registry.
{% endcomment %}
Docker Hub は Docker Registry の 1 つです。

## Docker Trusted Registry

{% comment %}
[Docker Trusted Registry](/datacenter/dtr/2.1/guides/index.md) is part of
Docker Enterprise Edition, and is a private, secure Docker registry which
includes features such as image signing and content trust, role-based access
controls, and other Enterprise-grade features.
{% endcomment %}
[Docker Trusted Registry](/datacenter/dtr/2.1/guides/index.md) は Docker Enterprise エディションの一部であり、プライベートでセキュアな Docker レジストリです。
ここにはイメージへのサイン機能、Content Trust、ロールベースのアクセス制御、その他さまざまなエンタープライズレベルの機能が提供されています。


## Content Trust

{% comment %}
When transferring data among networked systems, *trust* is a central concern. In
particular, when communicating over an untrusted medium such as the internet, it
is critical to ensure the integrity and publisher of all of the data a system
operates on. You use Docker to push and pull images (data) to a registry.
Content trust gives you the ability to both verify the integrity and the
publisher of all the data received from a registry over any channel.
{% endcomment %}
ネットワークシステム内においてデータを転送する際には、*信頼*（trust）というものが最も重要です。
特にインターネットのような信頼性の乏しいシステム上においてデータのやり取りを行う場合、システムが取り扱うあらゆるデータの整合性とそのアプリ発行者情報を確実にすることが極めて重要になります。
Docker を使ってイメージデータをレジストリの間でプッシュしたりプルしたりすることができます。
Content Trust はレジストリのどのチャネルからでも、全データの整合性とアプリ発行者の情報を検証する機能を提供します。

{% comment %}
See [Content trust](/engine/security/trust/index.md) for information about
configuring and using this feature on Docker clients.
{% endcomment %}
Docker クライアント上のこの機能を設定し利用する情報は [Content trust](/engine/security/trust/index.md) を参照してください。
