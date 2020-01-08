---
title: Docker Trusted Registry 概要
description: Docker Trusted Registry のインストール、設定、利用について学びます。
keywords: registry, repository, images
---

>{% include enterprise_label_shortform.md %}

{% comment %}
Docker Trusted Registry (DTR) is the enterprise-grade image storage solution
from Docker. You install it behind your firewall so that you can securely store
and manage the Docker images you use in your applications.
{% endcomment %}
Docker Trusted Registry (DTR) は Docker が提供するエンタープライズレベルのイメージストレージソリューションです。
ファイアーウォール内にこれをインストールすれば、アプリケーションが利用する Docker イメージを安全に保持し管理することができます。

{% comment %}
## Image and job management
{% endcomment %}
## イメージ管理とジョブ管理
{: #image-and-job-management }

{% comment %}
DTR can be installed on-premises, or on a virtual private
cloud. And with it, you can store your Docker images securely, behind your
firewall.
{% endcomment %}
DTR は自社サーバー上でも仮想プライベートクラウド上でもインストールすることができます。
どちらでも Docker イメージを安全に保存することができ、しかもファイアーウォール内に置くことができます。

{% comment %}
You can use DTR as part of your continuous integration, and continuous
delivery processes to build, ship, and run your applications.
{% endcomment %}
DTR は継続的インテグレーションや継続的開発によるアプリケーションの構築、導入、実行を行うプロセスの一部として利用可能です。

{% comment %}
DTR has a web user interface that allows authorized users in your
organization to browse Docker images and [review repository events](/ee/dtr/user/audit-repository-events/). It even allows you to see what Dockerfile
lines were used to produce the image and, if security scanning is enabled, to
see a list of all of the software installed in your images. Additionally, you can now [review and audit jobs on the web interface](/ee/dtr/admin/manage-jobs/audit-jobs-via-ui/).
{% endcomment %}
DTR ではウェブユーザーインターフェースを提供しています。
開発チーム内の権限を有するユーザーであれば、Docker イメージをブラウズでき、[リポジトリイベントのレビュー](/ee/dtr/user/audit-repository-events/) を行うこともできます。
また Dockerfile の各行からどのようにしてイメージが生成されているかを知ることができます。
セキュリティスキャニングが有効になっていれば、イメージ内にインストールされているソフトウェアをすべて一覧確認することもできます。
さらに最新においては [ウェブインターフェイス上にてジョブをレビューし監査する](/ee/dtr/admin/manage-jobs/audit-jobs-via-ui/) ことができます。

{% comment %}
## Availability
{% endcomment %}
## 可用性
{: #availability }

{% comment %}
DTR is highly available through the use of multiple replicas of all containers
and metadata such that if a machine fails, DTR continues to operate and can be repaired.
{% endcomment %}
DTR はどのようなコンテナーでも複数のレプリカやメタデータを利用することができます。
これによりマシンが修復不能になっても、DTR は稼動を続けることができ、修復することができます。

{% comment %}
## Efficiency
{% endcomment %}
## 効率性
{: #efficiency }

{% comment %}
DTR has the ability to [cache images closer to users](admin/configure/deploy-caches/index.md)
to reduce the amount of bandwidth used when pulling Docker images.
{% endcomment %}
DTR has the ability to [cache images closer to users](admin/configure/deploy-caches/index.md)
to reduce the amount of bandwidth used when pulling Docker images.

{% comment %}
DTR has the ability to [clean up unreferenced manifests and layers](admin/configure/garbage-collection.md).
{% endcomment %}
DTR has the ability to [clean up unreferenced manifests and layers](admin/configure/garbage-collection.md).

{% comment %}
## Built-in access control
{% endcomment %}
## ビルトインのアクセス制御
{: #built-in-access-control }

{% comment %}
DTR uses the same authentication mechanism as Docker Universal Control Plane.
Users can be managed manually or synchronized from LDAP or Active Directory. DTR
uses [Role Based Access Control](admin/manage-users/index.md) (RBAC) to allow you to implement fine-grained
access control policies for your Docker images.
{% endcomment %}
DTR uses the same authentication mechanism as Docker Universal Control Plane.
Users can be managed manually or synchronized from LDAP or Active Directory. DTR
uses [Role Based Access Control](admin/manage-users/index.md) (RBAC) to allow you to implement fine-grained
access control policies for your Docker images.

{% comment %}
## Security scanning
{% endcomment %}
## セキュリティスキャニング
{: #security-scanning

{% comment %}
DTR has a built-in security scanner that can be used to discover what versions
of software are used in your images. It scans each layer and aggregates the
results to give you a complete picture of what you are shipping as a part of
your stack. Most importantly, it correlates this information with a
vulnerability database that is kept up to date through [periodic
updates](admin/configure/set-up-vulnerability-scans.md). This
gives you [unprecedented insight into your exposure to known security
threats](user/manage-images/scan-images-for-vulnerabilities.md).
{% endcomment %}
DTR has a built-in security scanner that can be used to discover what versions
of software are used in your images. It scans each layer and aggregates the
results to give you a complete picture of what you are shipping as a part of
your stack. Most importantly, it correlates this information with a
vulnerability database that is kept up to date through [periodic
updates](admin/configure/set-up-vulnerability-scans.md). This
gives you [unprecedented insight into your exposure to known security
threats](user/manage-images/scan-images-for-vulnerabilities.md).

{% comment %}
## Image signing
{% endcomment %}
## Image signing
{: #image-signing }

DTR ships with [Notary](/notary/getting_started.md)
built in so that you can use
[Docker Content Trust](/engine/security/trust/content_trust.md) to sign
and verify images. For more information about managing Notary data in DTR see
the [DTR-specific notary documentation](user/manage-images/sign-images/index.md).

{% comment %}
## Where to go next
{% endcomment %}
## 次に進む
{: #where-to-go-next }

* [DTR architecture](architecture.md)
* [Install DTR](admin/install/index.md)
