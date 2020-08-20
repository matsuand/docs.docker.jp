---
description: High-level overview of the Registry
keywords: registry, on-prem, images, tags, repository, distribution
redirect_from:
- /registry/overview/
title: Docker Registry
---

{% include registry.md %}

{% comment %}
## What it is
{% endcomment %}
{: #what-it-is }
## これは何か

{% comment %}
The Registry is a stateless, highly scalable server side application that stores
and lets you distribute Docker images. The Registry is open-source, under the
permissive [Apache license](http://en.wikipedia.org/wiki/Apache_License).
{% endcomment %}
Registory とは、ステートレスで拡張性の高いサーバーサイドのアプリケーションであり、Docker イメージを保存し提供するものです。
Registry はオープンソースであり [Apache ライセンス](http://en.wikipedia.org/wiki/Apache_License) のもとに提供されています。

{% comment %}
## Why use it
{% endcomment %}
{: #why-use-it }
## なぜ利用するのか

{% comment %}
You should use the Registry if you want to:
{% endcomment %}
Registry は以下の状況において利用することができます。

 {% comment %}
 * tightly control where your images are being stored
 * fully own your images distribution pipeline
 * integrate image storage and distribution tightly into your in-house development workflow
 {% endcomment %}
 * イメージの保存場所を厳密に管理したい場合。
 * 独自のイメージ配布場所を確保したい場合。
 * イメージの保存と配布を、社内の開発ワークフローに緊密に連携したい場合。

{% comment %}
## Alternatives
{% endcomment %}
{: #alternatives }
## 他の選択肢

{% comment %}
Users looking for a zero maintenance, ready-to-go solution are encouraged to
head-over to the [Docker Hub](https://hub.docker.com), which provides a
free-to-use, hosted Registry, plus additional features (organization accounts,
automated builds, and more).
{% endcomment %}
メンテナンス不要ですぐに活用できるソリューションを求めるユーザーであれば、
[Docker Hub](https://hub.docker.com) をお勧めします。
これは無償で利用できる、ホスト提供込みの Registry であり、追加機能がさまざまにあります（組織アカウント、自動ビルドなど）。

{% comment %}
## Requirements
{% endcomment %}
{: #requirements }
## システム要件

{% comment %}
The Registry is compatible with Docker engine **version 1.6.0 or higher**.
{% endcomment %}
Registry は Docker engine **バージョン 1.6.0 またはそれ以降** と互換性があります。

{% comment %}
## Basic commands
{% endcomment %}
{: #basic-commands }
## 基本的なコマンド

{% comment %}
Start your registry
{% endcomment %}
レジストリを起動します。

    docker run -d -p 5000:5000 --name registry registry:2

{% comment %}
Pull (or build) some image from the hub
{% endcomment %}
Docker Hub からイメージをプル（またはビルド）します。

    docker pull ubuntu

{% comment %}
Tag the image so that it points to your registry
{% endcomment %}
イメージにタグづけを行って、レジストリを指し示すようにします。

    docker image tag ubuntu localhost:5000/myfirstimage

{% comment %}
Push it
{% endcomment %}
イメージをプッシュします。

    docker push localhost:5000/myfirstimage

{% comment %}
Pull it back
{% endcomment %}
もう一度プルします。

    docker pull localhost:5000/myfirstimage

{% comment %}
Now stop your registry and remove all data
{% endcomment %}
レジストリを停止して全データを削除します。

    docker container stop registry && docker container rm -v registry

{% comment %}
## Next
{% endcomment %}
{: #next }
## 次は

{% comment %}
You should now read the [detailed introduction about the registry](introduction.md),
or jump directly to [deployment instructions](deploying.md).
{% endcomment %}
次に読むのは [レジストリの導入](introduction.md) です。
あるいは [デプロイ手順](deploying.md) に進んでください。
