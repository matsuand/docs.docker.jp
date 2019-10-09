---
title: ウェブベースのアクセス
description: ウェブブラウザーから Docker Universal Control Plane にアクセスする方法について学びます。
keywords: ucp, web, administration
redirect_from:
  - /ee/ucp/user/access-ucp/
---

{% comment %}
Docker Universal Control Plane allows you to manage your cluster in a visual
way, from your browser.
{% endcomment %}
Docker Universal Control Plane におけるクラスター管理は、ウェブブラウザー上からわかりやすく行うことができます。

![](../images/web-based-access-1.png){: .with-border}


{% comment %}
Docker UCP secures your cluster by using
[role-based access control](../authorization/index.md).
From the browser, administrators can:
{% endcomment %}
Docker UCP では [ロールベースのアクセス制御](../authorization/index.md) を利用して安全にクラスターを取り扱います。
ブラウザーから管理者は以下を行います。

{% comment %}
* Manage cluster configurations,
* Manage the permissions of users, teams, and organizations,
* See all images, networks, volumes, and containers.
* Grant permissions to users for scheduling tasks on specific nodes
  (with the Docker Enterprise license).
{% endcomment %}
* クラスター設定を管理します。
* ユーザー、チーム、組織のパーミッションを管理します。
* イメージ、ネットワーク、ボリューム、コンテナーをすべて監視します。
* 特定ノードのスケジュールタスクに対して、ユーザーへの権限を付与します。
  （Docker Enterprise ライセンスを利用します。）

![](../images/web-based-access-2.png){: .with-border}

{% comment %}
Non-admin users can only see and change the images, networks, volumes, and
containers, and only when they're granted access by an administrator.
{% endcomment %}
管理者ではない一般ユーザーは、イメージ、ネットワーク、ボリューム、コンテナーを参照し、場合により変更することができます。
これは管理者によって権限が与えられた場合のみです。

{% comment %}
## Where to go next
{% endcomment %}
{: #where-to-go-next }
## 次に読むものは

{% comment %}
- [Authorization](../authorization.md)
- [Access UCP from the CLI](cli.md)
{% endcomment %}
- [アクセス権限](../authorization.md)
- [CLI からの UCP アクセス](cli.md)
