---
description: Compose に関するサンプルの概要。
keywords: documentation, docs, docker, compose, samples
title: Compose を使ったサンプルアプリ
---

{% comment %}
The following samples show the various aspects of how to work with Docker
Compose. As a prerequisite, be sure to [install Docker
Compose](/compose/install/) if you have not already done so.
{% endcomment %}
The following samples show the various aspects of how to work with Docker
Compose. As a prerequisite, be sure to [install Docker
Compose](/compose/install/) if you have not already done so.

{% comment %}
## Key concepts these samples cover
{% endcomment %}
{: #key-concepts-these-samples-cover }
## サンプルが示す重要な考え方

{% comment %}
The samples should help you to:
{% endcomment %}
このサンプルでは以下のことを行います。

{% comment %}
- define services based on Docker images using
  [Compose files](/compose/compose-file.md) `docker-compose.yml` and
  `docker-stack.yml` files
- understand the relationship between `docker-compose.yml` and
  [Dockerfiles](/engine/reference/builder.md)
- learn how to make calls to your application services from Compose files
- learn how to deploy applications and services to a [swarm](/engine/swarm.md)
{% endcomment %}
- define services based on Docker images using
  [Compose files](/compose/compose-file.md) `docker-compose.yml` and
  `docker-stack.yml` files
- understand the relationship between `docker-compose.yml` and
  [Dockerfiles](/engine/reference/builder.md)
- learn how to make calls to your application services from Compose files
- learn how to deploy applications and services to a [swarm](/engine/swarm.md)

{% comment %}
## Samples tailored to demo Compose
{% endcomment %}
{: #samples-tailored-to-demo-compose }
## Compose のデモを兼ねたサンプル

{% comment %}
These samples focus specifically on Docker Compose:
{% endcomment %}
以下のサンプルは特に Docker Compose に着目しています。

{% comment %}
- [Quickstart: Compose and Django](/compose/django.md) - Shows how to use Docker Compose to set up and run a simple Django/PostgreSQL app.
{% endcomment %}
- [クイックスタート: Compose と Django](/compose/django.md) - Docker Compose を使って、簡単な Django/PostgreSQL アプリのセットアップと実行方法を示します。

{% comment %}
- [Quickstart: Compose and Rails](/compose/rails.md) - Shows how to use
Docker Compose to set up and run a Rails/PostgreSQL app.
{% endcomment %}
- [クイックスタート: Compose と Rails](/compose/rails.md) - Docker Compose を使って、Rails/PostgreSQL アプリのセットアップと実行方法を示します。

{% comment %}
- [Quickstart: Compose and WordPress](/compose/wordpress.md) - Shows how to
use Docker Compose to set up and run WordPress in an isolated environment
with Docker containers.
{% endcomment %}
- [クイックスタート: Compose と WordPress](/compose/wordpress.md) - Docker Compose を使って、独立した環境内にて WordPress をセットアップし実行する方法を示します。

{% comment %}
## Samples that include Compose in the workflows
{% endcomment %}
{: #samples-that-include-compose-in-the-workflows }
## Samples that include Compose in the workflows

{% comment %}
These samples include working with Docker Compose as part of broader learning
goals:
{% endcomment %}
These samples include working with Docker Compose as part of broader learning
goals:

{% comment %}
- [Get Started with Docker](/get-started/index.md) - This multi-part tutorial covers writing your first app, data storage, networking, and swarms,
and ends with your app running on production servers in the cloud.
{% endcomment %}
- [Docker をはじめよう](/get-started/index.md) - This multi-part tutorial covers writing your first app, data storage, networking, and swarms,
and ends with your app running on production servers in the cloud.

{% comment %}
- [Deploying an app to a Swarm](https://github.com/docker/labs/blob/master/beginner/chapters/votingapp.md) - This tutorial from [Docker Labs](https://github.com/docker/labs/blob/master/README.md) shows you how to create and customize a sample voting app, deploy it to a [swarm](/engine/swarm.md), test it, reconfigure the app, and redeploy.
{% endcomment %}
- [スウォームへのアプリのデプロイ](https://github.com/docker/labs/blob/master/beginner/chapters/votingapp.md) - This tutorial from [Docker Labs](https://github.com/docker/labs/blob/master/README.md) shows you how to create and customize a sample voting app, deploy it to a [swarm](/engine/swarm.md), test it, reconfigure the app, and redeploy.
