---
description: RHEL 上に Docker Engine - Enterprise をインストールする手順を説明。
keywords: requirements, installation, rhel, rpm, install, uninstall, upgrade, update
redirect_from:
- /engine/installation/rhel/
- /installation/rhel/
- /engine/installation/linux/rhel/
- /engine/installation/linux/docker-ee/rhel/
title: Docker Engine - Enterprise の入手（Red Hat Enterprise Linux 向け）
---

{% assign linux-dist = "rhel" %}
{% assign linux-dist-cap = "RHEL" %}
{% assign linux-dist-url-slug = "rhel" %}
{% assign linux-dist-long = "Red Hat Enterprise Linux" %}
{% assign package-format = "RPM" %}
{% assign gpg-fingerprint = "77FE DA13 1A83 1D29 A418  D3E8 99E5 FF2E 7668 2BC9" %}


{% include ee-linux-install-reuse.md section="ee-install-intro" %}

{% comment %}
## Prerequisites
{% endcomment %}
## 前提条件
{: #prerequisites }

{% comment %}
This section lists what you need to consider before installing Docker EE. Items that require action are explained below.
{% endcomment %}
ここでは Docker EE をインストールする前に考慮しておくべきことを説明します。
作業を必要とする項目を以下に示します。

{% comment %}
- Use {{ linux-dist-cap }} 64-bit 7.4 and higher on `x86_64`, or `s390x`.
- Use storage driver `overlay2` or `devicemapper` (`direct-lvm` mode in production).
- Find the URL for your Docker EE repo at [Docker Hub](https://hub.docker.com/my-content){: target="_blank" class="_" }.
- Uninstall old versions of Docker.
- Remove old Docker repos from `/etc/yum.repos.d/`.
- Disable SELinux on `s390x` (IBM Z) systems before install/upgrade.
{% endcomment %}
- {{ linux-dist-cap }} 64 ビット 7.4 またはそれ以上、`x86_64`または`s390x`を利用。
- ストレージドライバーとして `overlay2` または `devicemapper`（本番環境では `direct-lvm` モード）を利用。
- [Docker Hub](https://hub.docker.com/my-content){: target="_blank" class="_" } から Docker EE リポジトリを検索。
- Docker の旧バージョンはアンインストール。
- `/etc/yum.repos.d/` から古い Docker リポジトリは削除。
- `s390x` (IBM Z) システムではインストール、アップグレード前に SELinux を無効化。

{% comment %}
### Architectures and storage drivers
{% endcomment %}
### アーキテクチャーとストレージドライバー
{: #architectures-and-storage-drivers }

{% comment %}
Docker EE supports {{ linux-dist-long }} 64-bit, versions 7.4 and higher running on one of the following architectures: `x86_64`, or `s390x` (IBM Z). See [Compatability Matrix](https://success.docker.com/article/compatibility-matrix){: target="_blank" class="_" }) for specific details.
{% endcomment %}
Docker EE では {{ linux-dist-long }} 64 ビット、バージョン 7.4 またはそれ以上、`x86_64`または`s390x` (IBM Z) のアーキテクチャーをサポートします。詳細については[互換性マトリックス](https://success.docker.com/article/compatibility-matrix){: target="_blank" class="_" }（Compatability Matrix）を参照してください。

{% comment %}
> Little-endian format only
>
> On IBM Power systems, Docker Engine - Enterprise only supports little-endian format, `ppc64le`, even though {{ linux-dist-cap }} 7 ships both big and little-endian versions.
{% endcomment %}
> リトルエンディアン方式のみ
>
> {{ linux-dist-cap }} 7 にはビッグエンディアン、リトルエンディアンの両バージョンがありますが、IBM Power システムにおいて Docker Engine - Enterprise はリトルエンディアン方式、つまり `ppc64le` のみをサポートします。

{% comment %}
On {{ linux-dist-long }}, Docker EE supports storage drivers, `overlay2` and `devicemapper`. In Docker EE 17.06.2-ee-5 and higher, `overlay2` is the recommended storage driver. The following limitations apply:
{% endcomment %}
{{ linux-dist-long }} において Docker EE はストレージドライバー `overlay2` と `devicemapper` をサポートします。
Docker EE 17.06.2-ee-5 およびこれ以上においては、`overlay2` の利用が推奨されています。
利用にあたっては以下の制約があります。

{% comment %}
- [OverlayFS](/storage/storagedriver/overlayfs-driver){: target="_blank" class="_" }: If `selinux` is enabled, the `overlay2` storage driver is supported on {{ linux-dist-cap }} 7.4 or higher. If `selinux` is disabled, `overlay2` is supported on {{ linux-dist-cap }} 7.2 or higher with kernel version 3.10.0-693 and higher.
{% endcomment %}
- [OverlayFS](/storage/storagedriver/overlayfs-driver){: target="_blank" class="_" }: `selinux` が有効な場合、ストレージドライバー `overlay2` は {{ linux-dist-cap }} 7.4 またはそれ以上においてサポートされます。
`selinux` が無効な場合、`overlay2` は {{ linux-dist-cap }} 7.2 またはそれ以上、ただしカーネルバージョンは 3.10.0-693 またはそれ以上においてサポートされます。

{% comment %}
- [Device Mapper](/storage/storagedriver/device-mapper-driver/){: target="_blank" class="_" }: On production systems using `devicemapper`, you must use `direct-lvm` mode, which requires one or more dedicated block devices. Fast storage such as solid-state media (SSD) is recommended. Do not start Docker until properly configured per the [storage guide](/storage/storagedriver/device-mapper-driver/){: target="_blank" class="_" }.
{% endcomment %}
- [Device Mapper](/storage/storagedriver/device-mapper-driver/){: target="_blank" class="_" }: 本番環境において `devicemapper` を利用する場合は、`direct-lvm` モードを利用するようにしてください。
  これは専用のブロックデバイスをいくつか必要とします。
  SSD（solid-state media）のような高速なストレージが推奨されます。
  各[ストレージドライバー](/storage/storagedriver/device-mapper-driver/){: target="_blank" class="_" }に応じた設定が適切にできていることを確認してから、Docker を起動してください。

{% comment %}
### FIPS 140-2 cryptographic module support
{% endcomment %}
### FIPS 140-2 暗号化モジュールのサポート
{: #fips-140-2-cryptographic-module-support }

{% comment %}
[Federal Information Processing Standards (FIPS) Publication 140-2](https://csrc.nist.gov/csrc/media/publications/fips/140/2/final/documents/fips1402.pdf)
is a United States Federal security requirement for cryptographic modules.
{% endcomment %}
[連邦情報処理標準（Federal Information Processing Standards; FIPS）文書 140-2](https://csrc.nist.gov/csrc/media/publications/fips/140/2/final/documents/fips1402.pdf) は、米国連邦政府が定める暗号化モジュールのセキュリティ要件です。

{% comment %}
With Docker Engine - Enterprise Basic license for versions 18.03 and later,
Docker provides FIPS 140-2 support in RHEL 7.3, 7.4 and 7.5. This includes a
FIPS supported cryptographic module. If the RHEL implementation already has FIPS
support enabled, FIPS is also automatically enabled in the Docker engine. If
FIPS support is not already enabled in your RHEL implementation, visit the
[Red Hat Product Documentation](https://access.redhat.com/documentation/en-us/)
for instructions on how to enable it.
{% endcomment %}
Docker では、Docker Engine - Enterprise 基本ライセンスバージョン 18.03 またはそれ以上にて、RHEL 7.3、7.4、7.5 に対する FIPS 140-2 サポートを提供しています。
ここには FIPS がサポートする暗号化モジュールも含まれます。
RHEL におけるモジュールがすでに FIPS サポートを可能としていれば、FIPS は Docker エンジンにおいて自動的に有効となっています。
RHEL 実装内にて FIPS サポートが有効になっていない場合は、[Red Hat Product Documentation](https://access.redhat.com/documentation/en-us/) を確認し、これを有効にする手順に従ってください。

{% comment %}
To verify the FIPS-140-2 module is enabled in the Linux kernel, confirm the file
`/proc/sys/crypto/fips_enabled` contains `1`.
{% endcomment %}
Linux カーネルにおいて FIPS-140-2 モジュールが有効かどうかは、`/proc/sys/crypto/fips_enabled` ファイルに `1` が設定されているかどうかでわかります。

```
$ cat /proc/sys/crypto/fips_enabled
1
```

{% comment %}
> **Note**: FIPS is only supported in the Docker Engine Engine - Enterprise. UCP
> and DTR currently do not have support for FIPS-140-2.
{% endcomment %}
> **メモ**: FIPS は Docker Engine - Enterprise でのみサポートされます。
UCP と DTR は今のところ FIPS-140-2 をサポートしていません。

{% comment %}
You can override FIPS 140-2 compliance on a system that is not in FIPS 140-2
mode. Note, this **does not** change FIPS 140-2 mode on the system. To override
the FIPS 140-2 mode, follow ths steps below.
{% endcomment %}
FIPS 140-2 モードになっていないシステムにおいて FIPS 140-2 コンプライアンスを有効にすることができます。
これはシステム内の FIPS 140-2 モードを変更するものでは **ありません**。
FIPS 140-2 モードを上書き設定するには、以下の手順に従います。

{% comment %}
Create a file called `/etc/systemd/system/docker.service.d/fips-module.conf`.
Add the following:
{% endcomment %}
`/etc/systemd/system/docker.service.d/fips-module.conf` というファイルを生成します。
そこに以下を記述します。

```
[Service]
Environment="DOCKER_FIPS=1"
```

{% comment %}
Reload the Docker configuration to systemd.
{% endcomment %}
Docker の設定を systemd に再読み込みさせます。

`$ sudo systemctl daemon-reload`

{% comment %}
Restart the Docker service as root.
{% endcomment %}
root ユーザーにより Docker サービスを再起動します。

`$ sudo systemctl restart docker`

{% comment %}
To confirm Docker is running with FIPS-140-2 enabled, run the `docker info`
command:
{% endcomment %}
FIPS-140-2 を有効にした Docker が稼動しているかを確認するため `docker info` コマンドを実行します。

{% raw %}
```
docker info --format {{.SecurityOptions}}
[name=selinux name=fips]
```
{% endraw %}

{% comment %}
### Disabling FIPS-140-2
{% endcomment %}
### FIPS-140-2 の無効化
{: #disabling-fips-140-2 }

{% comment %}
If the system has the FIPS 140-2 cryptographic module installed on the operating
system, it is possible to disable FIPS-140-2 compliance.
{% endcomment %}
オペレーティングシステム内に FIPS 140-2 暗号化モジュールがインストールされている場合に、FIPS-140-2 コンプライアンスを無効にすることもできます。

{% comment %}
To disable FIPS 140-2 in Docker but not the operating system, set the value
`DOCKER_FIPS=0` in the `/etc/systemd/system/docker.service.d/fips-module.conf`.
{% endcomment %}
FIPS 140-2 を Docker 上では無効とし、オペレーティングシステム上は有効のままとする場合は、`/etc/systemd/system/docker.service.d/fips-module.conf` 内において `DOCKER_FIPS=0` と設定します。

{% comment %}
Reload the Docker configuration to systemd.
{% endcomment %}
Docker の設定を systemd に再読み込みさせます。

`$ sudo systemctl daemon-reload`

{% comment %}
Restart the Docker service as root.
{% endcomment %}
root ユーザーにより Docker サービスを再起動します。

`$ sudo systemctl restart docker`

{% comment %}
### Find your Docker Engine - Enterprise repo URL
{% endcomment %}
### Docker Engine - Enterprise のリポジトリ URL
{: #find-your-docker-engine-enterprise-repo-url }

{% include ee-linux-install-reuse.md section="find-ee-repo-url" %}

{% comment %}
### Uninstall old Docker versions
{% endcomment %}
### Docker の旧バージョンのアンインストール
{: #uninstall-old-docker-versions }

{% comment %}
The Docker EE package is called `docker-ee`. Older versions were called `docker` or `docker-engine`. Uninstall all older versions and associated dependencies. The contents of `/var/lib/docker/` are preserved, including images, containers, volumes, and networks.
{% endcomment %}
Docker EE パッケージは `docker-ee` という名称です。
古いバージョンでは `docker` あるいは `docker-engine` というものでした。
古いバージョンと関連する依存パッケージはすべてアンインストールします。
`/var/lib/docker/` にはイメージ、コンテナー、ボリューム、ネットワークが含まれていて、それは保持されたまま残ります。

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
{% capture selinux-warning %}
> Disable SELinux before installing Docker EE on IBM Z systems
>
> There is currently no support for `selinux` on IBM Z systems. If you attempt to install or upgrade Docker EE on an IBM Z system with `selinux` enabled, an error is thrown that the `container-selinux` package is not found. Disable `selinux` before installing or upgrading Docker on IBM Z.
{:.warning}
{% endcapture %}
{{ selinux-warning }}
{% endcomment %}
{% capture selinux-warning %}
> Docker EE を IBM Z システムへのインストール時は SELinux を無効に
>
> 現在のところ IBM Z システムにおいては `selinux` をサポートしていません。
> Docker EE を IBM Z システム上にてインストールまたはアップグレードする際に `selinux` を有効にしていると、`container-selinux` パッケージが見つからないというエラーが発生します。
> IBM Z システム上にてインストールまたはアップグレードする際には `selinux` を無効にしてください。
{:.warning}
{% endcapture %}
{{ selinux-warning }}

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
## Docker Engine - Enterprise のアンインストール
{: #uninstall-docker-engine-enterprise }

{% include ee-linux-install-reuse.md section="yum-uninstall" %}

{% comment %}
## Next steps
{% endcomment %}
## 次のステップ
{: #next-steps }

{% include ee-linux-install-reuse.md section="linux-install-nextsteps" %}
