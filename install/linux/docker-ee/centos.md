---
description: CentOS 上に Docker Engine - Enterprise をインストールする手順を説明。
keywords: requirements, apt, installation, centos, rpm, install, uninstall, upgrade, update
redirect_from:
- /engine/installation/centos/
- /engine/installation/linux/docker-ee/centos/
title: Docker Engine - Enterprise の入手（CentOS 向け）
---

{% assign linux-dist = "centos" %}
{% assign linux-dist-cap = "CentOS" %}
{% assign linux-dist-url-slug = "centos" %}
{% assign linux-dist-long = "Centos" %}
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
- Use {{ linux-dist-cap }} 64-bit 7.1 and higher on `x86_64`.
- Use storage driver `overlay2` or `devicemapper` (`direct-lvm` mode in
  production).
- Find the URL for your Docker Engine - Enterprise repo at [Docker Hub](https://hub.docker.com/my-content){: target="_blank" class="_" }.
- Uninstall old versions of Docker.
- Remove old Docker repos from `/etc/yum.repos.d/`.
{% endcomment %}
- {{ linux-dist-cap }} 64 ビット 7.1 またはそれ以上 `x86_64` を利用。
- ストレージドライバーとして `overlay2` または `devicemapper`（本番環境では `direct-lvm` モード）を利用。
- [Docker Hub](https://hub.docker.com/my-content){: target="_blank" class="_" } から Docker Engine - Enterprise リポジトリ の URL を検索。
- Docker の旧バージョンはアンインストール。
- `/etc/yum.repos.d/` から古い Docker リポジトリは削除。

{% comment %}
### Architectures and storage drivers
{% endcomment %}
{: #architectures-and-storage-drivers }
### アーキテクチャーとストレージドライバー

{% comment %}
Docker Engine - Enterprise supports {{ linux-dist-long }} 64-bit, latest
version, running on  `x86_64`.
{% endcomment %}
Docker Engine - Enterprise は {{ linux-dist-long }} の 64 ビット最新版、`x86_64` をサポートします。

{% comment %}
On {{ linux-dist-long }}, Docker Engine - Enterprise supports storage drivers,
`overlay2` and `devicemapper`. In Docker Engine - Enterprise 17.06.2-ee-5 and
higher, `overlay2` is the recommended storage driver. The following limitations
apply:
{% endcomment %}
{{ linux-dist-long }} において Docker Engine - Enterprise はストレージドライバー `overlay2` と `devicemapper` をサポートします。
Docker Engine - Enterprise 17.06.2-ee-5 およびこれ以上においては、`overlay2` の利用が推奨されています。
利用にあたっては以下の制約があります。

{% comment %}
- [OverlayFS](/storage/storagedriver/overlayfs-driver){: target="_blank" class="_" }:
  If `selinux` is enabled, the `overlay2` storage driver is supported on
  {{ linux-dist-cap }} 7.4 or higher. If `selinux` is disabled, `overlay2` is
  supported on {{ linux-dist-cap }} 7.2 or higher with kernel version 3.10.0-693
  and higher.
{% endcomment %}
- [OverlayFS](/storage/storagedriver/overlayfs-driver){: target="_blank" class="_" }: `selinux` が有効な場合、ストレージドライバー `overlay2` は {{ linux-dist-cap }} 7.4 またはそれ以上においてサポートされます。`selinux` が無効な場合、`overlay2` は {{ linux-dist-cap }} 7.2 またはそれ以上、ただしカーネルバージョンは 3.10.0-693 またはそれ以上においてサポートされます。

{% comment %}
- [Device Mapper](/storage/storagedriver/device-mapper-driver/){: target="_blank" class="_" }:
  On production systems using `devicemapper`, you must use `direct-lvm` mode,
  which requires one or more dedicated block devices. Fast storage such as
  solid-state media (SSD) is recommended. Do not start Docker until properly
  configured per the [storage guide](/storage/storagedriver/device-mapper-driver/){: target="_blank" class="_" }.
{% endcomment %}
- [Device Mapper](/storage/storagedriver/device-mapper-driver/){: target="_blank" class="_" }: 本番環境において `devicemapper` を利用する場合は、`direct-lvm` モードを利用するようにしてください。
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
including images, containers, volumes, and networks. If you are upgrading from
Docker Engine - Community to Docker Engine - Enterprise, remove the Docker
Engine - Community package as well.
{% endcomment %}
Docker Engine - Enterprise パッケージは `docker-ee` という名称です。
古いバージョンでは `docker` あるいは `docker-engine` というものでした。
古いバージョンと関連する依存パッケージはすべてアンインストールします。
`/var/lib/docker/` にはイメージ、コンテナー、ボリューム、ネットワークが含まれていて、それは保持されたまま残ります。
Docker Engine - Community から Docker Engine - Enterprise へのアップグレードを行っている場合は、Docker Engine - Community パッケージも同じく削除します。

```bash
$ sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-selinux \
                  docker-engine-selinux \
                  docker-engine
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
