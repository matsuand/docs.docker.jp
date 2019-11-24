---
description: Oracle Linux 上に Docker Engine - Enterprise をインストールする手順を説明。
keywords: requirements, installation, oracle, ol, rpm, install, uninstall, upgrade, update
redirect_from:
- /engine/installation/oracle/
- /engine/installation/linux/oracle/
- /engine/installation/linux/docker-ee/oracle/
title: Docker Engine - Enterprise の入手（Oracle Linux 向け）
---

{% assign linux-dist = "oraclelinux" %}
{% assign linux-dist-cap = "OL" %}
{% assign linux-dist-url-slug = "oraclelinux" %}
{% assign linux-dist-long = "Oracle Linux" %}
{% assign package-format = "RPM" %}
{% assign gpg-fingerprint = "77FE DA13 1A83 1D29 A418  D3E8 99E5 FF2E 7668 2BC9" %}


{% include ee-linux-install-reuse.md section="ee-install-intro" %}

{% comment %}
## Prerequisites
{% endcomment %}
## 前提条件
{: #prerequisites }

{% comment %}
This section lists what you need to consider before installing Docker Engine -
Enterprise. Items that require action are explained below.
{% endcomment %}
ここでは Docker Engine - Enterprise をインストールする前に考慮しておくべきことを説明します。
作業を必要とする項目を以下に示します。

{% comment %}
- Use {{ linux-dist-cap }} 64-bit 7.3 or higher on RHCK 3.10.0-514 or higher.
- Use the `devicemapper` storage driver only (`direct-lvm` mode in production).
- Find the URL for your Docker Engine - Enterprise repo at [Docker Hub](https://hub.docker.com/my-content){: target="_blank" class="_" }.
- Uninstall old versions of Docker.
- Remove old Docker repos from `/etc/yum.repos.d/`.
- Disable SELinux if installing or upgrading Docker Engine - Enterprise 17.06.1
  or newer.
{% endcomment %}
- {{ linux-dist-cap }} 64 ビット 7.3 またはそれ以上、RHCK 3.10.0-514 またはそれ以上を利用。
- ストレージドライバーとして `devicemapper` のみ（本番環境では `direct-lvm` モード）を利用。
- [Docker Hub](https://hub.docker.com/my-content){: target="_blank" class="_" } から Docker Engine - Enterprise リポジトリの URL を検索。
- Docker の旧バージョンはアンインストール。
- `/etc/yum.repos.d/` から古い Docker リポジトリは削除。
- Docker Engine - Enterprise 17.06.1 またはそれ以上においてインストールまたはアップグレードを行う場合は SELinux を無効に。

{% comment %}
### Architectures and storage drivers
{% endcomment %}
### アーキテクチャーとストレージドライバー
{: #architectures-and-storage-drivers }

{% comment %}
Docker Engine - Enterprise supports {{ linux-dist-long }} 64-bit, versions 7.3
and higher, running the Red Hat Compatible kernel (RHCK) 3.10.0-514 or higher.
Older versions of {{ linux-dist-long }} are not supported.
{% endcomment %}
Docker Engine - Enterprise は {{ linux-dist-long }} の 64 ビット、バージョン 7.3 またはそれ以上、Red Hat Compatible kernel (RHCK) 3.10.0-514 またはそれ以上をサポートします。
{{ linux-dist-long }} の古いバージョンはサポート対象外です。

{% comment %}
On {{ linux-dist-long }}, Docker Engine - Enterprise only supports the
`devicemapper` storage driver. In production, you must use it in `direct-lvm`
mode, which requires one or more dedicated block devices. Fast storage such as
solid-state media (SSD) is recommended. Do not start Docker until properly
configured per the [storage guide](/storage/storagedriver/device-mapper-driver/){: target="_blank" class="_" }.
{% endcomment %}
{{ linux-dist-long }} において Docker Engine - Enterprise は、ストレージドライバー `devicemapper` のみをサポートします。
本番環境では `direct-lvm` モードを利用するようにしてください。
これは専用のブロックデバイスをいくつか必要とします。
SSD（solid-state media）のような高速なストレージが推奨されます。
各[ストレージドライバー](/storage/storagedriver/device-mapper-driver/){: target="_blank" class="_" }に応じた設定が適切にできていることを確認してから、Docker を起動してください。

{% comment %}
### Find your Docker Engine - Enterprise repo URL
{% endcomment %}
{: #find-your-docker-engine-enterprise-repo-url }
### Docker Engine - Enterprise のリポジトリ URL

{% include ee-linux-install-reuse.md section="find-ee-repo-url" %}

{% comment %}
### Uninstall old Docker versions
{% endcomment %}
### Docker の旧バージョンのアンインストール
{: #uninstall-old-docker-versions }

{% comment %}
The Docker Engine - Enterprise package is called `docker-ee`. Older versions
were called `docker` or `docker-engine`. Uninstall all older versions and
associated dependencies. The contents of `/var/lib/docker/` are preserved,
including images, containers, volumes, and networks.
{% endcomment %}
Docker Engine - Enterprise パッケージは `docker-ee` という名称です。
古いバージョンでは `docker` あるいは `docker-engine` というものでした。
古いバージョンと関連する依存パッケージはすべてアンインストールします。
`/var/lib/docker/` にはイメージ、コンテナー、ボリューム、ネットワークが含まれていて、それは保持されたまま残ります。

```bash
$ sudo yum remove docker \
                  docker-engine \
                  docker-engine-selinux
```

{% comment %}
## Repo install and upgrade
{% endcomment %}
## リポジトリからのインストールとアップグレード
{: #repo-install-and-upgrade }

{% include ee-linux-install-reuse.md section="using-yum-repo" %}

{% comment %}
### Set up the repository
{% endcomment %}
### リポジトリのセットアップ
{: #set-up-the-repository }

{% include ee-linux-install-reuse.md section="set-up-yum-repo" %}

{% comment %}
### Install from the repository
{% endcomment %}
### リポジトリからのインストール
{: #install-from-the-repository }

{% include ee-linux-install-reuse.md section="install-using-yum-repo" %}

{% comment %}
### Upgrade from the repository
{% endcomment %}
### リポジトリからのアップグレード
{: #upgrade-from-the-repository }

{% include ee-linux-install-reuse.md section="upgrade-using-yum-repo" %}



{% comment %}
## Package install and upgrade
{% endcomment %}
## パッケージのインストールとアップグレード
{: #package-install-and-upgrade }

{% include ee-linux-install-reuse.md section="package-installation" %}

{{ selinux-warning }}

{% comment %}
### Install with a package
{% endcomment %}
### パッケージのインストール
{: #install-with-a-package }

{% include ee-linux-install-reuse.md section="install-using-yum-package" %}

{% comment %}
### Upgrade with a package
{% endcomment %}
### パッケージのアップグレード
{: #upgrade-with-a-package }

{% include ee-linux-install-reuse.md section="upgrade-using-yum-package" %}


{% comment %}
## Uninstall Docker Engine - Enterprise
{% endcomment %}
{: #uninstall-docker-engine-enterprise }
## Docker Engine - Enterprise のアンインストール

{% include ee-linux-install-reuse.md section="yum-uninstall" %}

{% comment %}
## Next steps
{% endcomment %}
## 次のステップ
{: #next-steps }

{% include ee-linux-install-reuse.md section="linux-install-nextsteps" %}
