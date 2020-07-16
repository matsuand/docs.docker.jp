---
description: Deploying Notary
keywords: trust, security, notary, deployment
title: Compose を使った Notary サーバーのデプロイ
---

{% comment %}
The easiest way to deploy Notary Server is by using Docker Compose. To follow the procedure on this page, you must have already [installed Docker Compose](../../../compose/install.md).
{% endcomment %}
Notary サーバーをデプロイする一番簡単な方法は Docker Compose を利用することです。
本ページに示す手順に従って進めるためには、[Docker Compose のインストール](../../../compose/install.md) が必要です。

{% comment %}
1. Clone the Notary repository.
{% endcomment %}
1. Notary リポジトリをクローンします。

       git clone https://github.com/theupdateframework/notary.git

{% comment %}
2. Build and start Notary Server with the sample certificates.
{% endcomment %}
2. サンプルの証明書を利用して Notary サーバーをビルドし起動します。

       docker-compose up -d


{% comment %}
  For more detailed documentation about how to deploy Notary Server, see the [instructions to run a Notary service](../../../notary/running_a_service.md) as well as [the Notary repository](https://github.com/theupdateframework/notary) for more information.
{% endcomment %}
  Notary サーバーのデプロイに関する詳細は [Notary サービスの実行方法](../../../notary/running_a_service.md) や [Notary リポジトリ](https://github.com/theupdateframework/notary) を参照してください。
{% comment %}
3. Make sure that your Docker or Notary client trusts Notary Server's certificate before you try to interact with the Notary server.
{% endcomment %}
3. Notary サーバーとのやり取りを行う前に、Docker クライアントあるいは Notary クライアントが Notary サーバーの証明書を信頼している状態にあることを確認してください。

{% comment %}
See the instructions for [Docker](../../reference/commandline/cli.md#notary) or
for [Notary](https://github.com/docker/notary#using-notary) depending on which one you are using.
{% endcomment %}
[Docker クライアント](../../reference/commandline/cli.md#notary) あるいは [Notary クライアント](https://github.com/docker/notary#using-notary) のいずれを用いるかにより、その手順を参照してください。

{% comment %}
## If you want to use Notary in production
{% endcomment %}
{: #if-you-want-to-use-Notary-in-production }
## Notary を本番環境で利用する場合

{% comment %}
Check back here for instructions after Notary Server has an official
stable release. To get a head start on deploying Notary in production, see
[the Notary repository](https://github.com/theupdateframework/notary).
{% endcomment %}
Notary サーバーに公式安定版を適用したら、ここに戻って手順を再確認してください。
本番環境において Notary のデプロイを適切に行うには [Notary リポジトリ](https://github.com/theupdateframework/notary) を参照してください。
