---
description: CentOS 上に Docker Engine - Enterprise をインストールする手順を説明。
keywords: requirements, apt, installation, centos, rpm, install, uninstall, upgrade, update
redirect_from:
- /engine/installation/centos/
- /engine/installation/linux/docker-ee/centos/
- /install/linux/docker-ee/centos/
title: Docker Engine - Enterprise の入手（CentOS 向け）
---

{% assign linux-dist = "centos" %}
{% assign linux-dist-cap = "CentOS" %}
{% assign linux-dist-url-slug = "centos" %}
{% assign linux-dist-long = "Centos" %}
{% assign package-format = "RPM" %}
{% assign gpg-fingerprint = "77FE DA13 1A83 1D29 A418  D3E8 99E5 FF2E 7668 2BC9" %}

>{% include enterprise_label_shortform.md %}

{% comment %}
There are two ways to install and upgrade [Docker Enterprise](https://www.docker.com/enterprise-edition/){: target="_blank" class="_" }
on {{ linux-dist-long }}:
{% endcomment %}
{{ linux-dist-long }} 向けの [Docker Enterprise](https://www.docker.com/enterprise-edition/){: target="_blank" class="_" } をインストールおよびアップグレードするには、以下の 2 つの方法があります。

{% comment %}
- [YUM repository](#repo-install-and-upgrade): Set up a Docker repository and install Docker Engine - Enterprise from it. This is the recommended approach because installation and upgrades are managed with YUM and easier to do.
{% endcomment %}
- [YUM リポジトリ](#repo-install-and-upgrade): Set up a Docker repository and install Docker Engine - Enterprise from it. This is the recommended approach because installation and upgrades are managed with YUM and easier to do.

{% comment %}
- [RPM package](#package-install-and-upgrade): Download the {{ package-format }} package, install it manually, and manage upgrades manually. This is useful when installing Docker Engine - Enterprise on air-gapped systems with no access to the internet.
{% endcomment %}
- [RPM パッケージ](#package-install-and-upgrade): Download the {{ package-format }} package, install it manually, and manage upgrades manually. This is useful when installing Docker Engine - Enterprise on air-gapped systems with no access to the internet.

<!---
Shared between centOS.md, rhel.md, oracle.md
--->

{% comment %}
For Docker Community Edition on {{ linux-dist-cap }}, see [Get Docker Engine - Community for CentOS](/install/linux/docker-ce/centos.md).
{% endcomment %}
{{ linux-dist-cap }} 向けの Docker Community Edition については [Docker Engine - Community の入手（CentOS 向け）](/install/linux/docker-ce/centos.md) を参照してください。

{% comment %}
## Prerequisites
{% endcomment %}
{: #prerequisites }
## 前提条件

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

{% comment %}
To install Docker Enterprise, you will need the URL of the Docker Enterprise repository associated with your trial or subscription:
{% endcomment %}
Docker Enterprise をインストールするには、Docker Enterprise リポジトリ上のお試し用（trial） URL、あるいは購入者用（subscription）URL を必要とします。

{% comment %}
1.  Go to [https://hub.docker.com/my-content](https://hub.docker.com/my-content){: target="_blank" class="_" }. All of your subscriptions and trials are listed.
2.  Click the **Setup** button for **Docker Enterprise Edition for {{ linux-dist-long }}**.
3.  Copy the URL from **Copy and paste this URL to download your Edition** and save it for later use.
{% endcomment %}
1.  [https://hub.docker.com/my-content](https://hub.docker.com/my-content){: target="_blank" class="_" } にアクセスします。
    お試し用、購入者用が一覧に示されています。
2.  **Docker Enterprise Edition for {{ linux-dist-long }}** における **Setup** ボタンをクリックします。
3.  **Copy and paste this URL to download your Edition** により URL をコピーして保存しておきます。これを後に用います。

{% comment %}
You will use this URL in a later step to create a variable called, `DOCKERURL`.
{% endcomment %}
上の URL は後の作業において `DOCKERURL` という変数を生成する際に利用します。

<!---
Shared between centOS.md, rhel.md, oracle.md
--->

{% comment %}
### Uninstall old Docker versions
{% endcomment %}
{: #uninstall-old-docker-versions }
### Docker の旧バージョンのアンインストール

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
{: #repo-install-and-upgrade }
## リポジトリからのインストールとアップグレード

{% comment %}
The advantage of using a repository from which to install Docker Engine - Enterprise (or any software) is that it provides a certain level of automation. RPM-based distributions such as {{ linux-dist-long }}, use a tool called YUM that work with your repositories to manage dependencies and provide automatic updates.
{% endcomment %}
リポジトリを使って Docker Engine - Enterprise を（あるいはどんなソフトウェアでも）インストールする利点は、ある程度、自動的に操作を進めることができるところです。
{{ linux-dist-long }} のような RPM ベースのディストリビューションでは、YUM というツールを使ってリポジトリを操作し、依存パッケージの管理や自動アップデート機能を提供しています。

<!---
Shared between centOS.md, rhel.md, oracle.md
--->

{% comment %}
### Set up the repository
{% endcomment %}
{: #set-up-the-repository }
### リポジトリのセットアップ

{% comment %}
You only need to set up the repository once, after which you can install Docker Engine - Enterprise _from_ the repo and repeatedly upgrade as necessary.
{% endcomment %}
リポジトリをセットアップする必要があるのは一回だけです。
その後は Docker Engine - Enterprise のインストールを、必要であれば何度でも行うことができます。

<!---
Shared between centOS.md, rhel.md, oracle.md
--->


{% comment %}
1.  Remove existing Docker repositories from `/etc/yum.repos.d/`:
{% endcomment %}
1.  `/etc/yum.repos.d/` にすでにある Docker リポジトリを削除します。

    ```bash
    $ sudo rm /etc/yum.repos.d/docker*.repo
    ```

{% comment %}
2.  Temporarily store the URL (that you [copied above](#find-your-docker-ee-repo-url)) in an environment variable. Replace `<DOCKER-EE-URL>` with your URL in the following command. This variable assignment does not persist when the session ends:
{% endcomment %}
2.  （[上でコピーした](#find-your-docker-ee-repo-url)）URL を一時的に環境変数に設定します。
    以下のコマンドにおいて `<DOCKER-EE-URL>` の部分は、その URL に置き換えてください。
    この変数を使うのは、ここで行う作業を終えるまでの一時的なものです。

    ```bash
    $ export DOCKERURL="<DOCKER-EE-URL>"
    ```

{% comment %}
3.  Store the value of the variable, `DOCKERURL` (from the previous step), in a `yum` variable in `/etc/yum/vars/`:
{% endcomment %}
3.  （前の手順における）`DOCKERURL` の変数値を、`/etc/yum/vars/` 内の `yum` 変数に保存します。

    ```bash
    $ sudo -E sh -c 'echo "$DOCKERURL/{{ linux-dist-url-slug }}" > /etc/yum/vars/dockerurl'
    ```

{% comment %}
4.  Install required packages: `yum-utils` provides the _yum-config-manager_ utility, and `device-mapper-persistent-data` and `lvm2` are required by the _devicemapper_ storage driver:
{% endcomment %}
4.  必要なパッケージをインストールします。
    `yum-utils` は _yum-config-manager_ ユーティリティーを提供します。
    `device-mapper-persistent-data` と `lvm2` はストレージドライバー _devicemapper_ が必要とします。

    ```bash
    $ sudo yum install -y yum-utils \
      device-mapper-persistent-data \
      lvm2
    ```

<!---
Shared between centOS.md, oracle.md
--->

{% comment %}
5.  Add the Docker Engine - Enterprise **stable** repository:
{% endcomment %}
5.  Add the Docker Engine - Enterprise **stable** repository:

    ```bash
    $ sudo -E yum-config-manager \
        --add-repo \
        "$DOCKERURL/{{ linux-dist-url-slug }}/docker-ee.repo"
    ```

<!---
Shared between centOS.md, oracle.md
--->

{% comment %}
### Install from the repository
{% endcomment %}
{: #install-from-the-repository }
### リポジトリからのインストール


{% comment %}
> **Note**: If you need to run Docker Engine - Enterprise 2.0, please see the following instructions:
> * [18.03](https://docs.docker.com/v18.03/ee/supported-platforms/) - Older Docker Engine - Enterprise Engine only release
> * [17.06](https://docs.docker.com/v17.06/engine/installation/) - Docker Enterprise Edition 2.0 (Docker Engine,
> UCP, and DTR).
{% endcomment %}
> **メモ**: Docker Engine - Enterprise 2.0 を利用する必要がある場合は、以下の手順を参照してください。
> * [18.03](https://docs.docker.com/v18.03/ee/supported-platforms/) - かつての Docker Engine - Enterprise のみリリース
> * [17.06](https://docs.docker.com/v17.06/engine/installation/) - Docker Enterprise エディション 2.0（Docker Engine、UCP、DTR）

{% comment %}
1.  Install the latest patch release, or go to the next step to install a specific version:
{% endcomment %}
1.  最新のパッチリリースをインストールします。
    特定のバージョンをインストールする場合は、この次の手順に進んでください。

    ```bash
    $ sudo yum -y install docker-ee docker-ee-cli containerd.io
    ```

    {% comment %}
    If prompted to accept the GPG key, verify that the fingerprint matches `{{ gpg-fingerprint }}`, and if so, accept it.
    {% endcomment %}
    GPG 鍵を受け入れるかどうか問われたら、鍵の指紋が `{{ gpg-fingerprint }}` に間違いないことを確認し、鍵を受け入れてください。


{% comment %}
2.  To install a _specific version_ of Docker Engine - Enterprise (recommended in production), list versions and install:
{% endcomment %}
2.  Docker Engine - Enterprise の特定バージョンをインストールする場合（本番環境向けにはこれを推奨する）、リポジトリにある利用可能なバージョンの一覧を確認し、いずれかを選んでインストールします。

    {% comment %}
    a.  List and sort the versions available in your repo. This example sorts results by version number, highest to lowest, and is truncated:
    {% endcomment %}
    a. リポジトリ内にある利用可能なバージョンを並びかえて一覧表示します。
       以下の例では出力結果をバージョン番号によりソートします。
       一覧は最新のものが上に並びます。バージョンは簡略に表示されます。

    ```bash
    $ sudo yum list docker-ee  --showduplicates | sort -r

    docker-ee.x86_64      {{ site.docker_ee_version }}.ee.2-1.el7.{{ linux-dist }}      docker-ee-stable-18.09
    ```

    {% comment %}
    The list returned depends on which repositories you enabled, and is specific to your version of {{ linux-dist-long }} (indicated by `.el7` in this example).
    {% endcomment %}
    この一覧内容は、どのリポジトリを有効にしているかによって変わります。
    また利用している {{ linux-dist-long }} のバージョンに応じたものになります（この例では ``.e17`` というサフィックスにより示されるバージョンです）。

    {% comment %}
    b.  Install a specific version by its fully qualified package name, which is the package name (`docker-ee`) plus the version string (2nd column) starting at the first colon (`:`), up to the first hyphen, separated by a hyphen (`-`). For example, `docker-ee-18.09.1`.
    {% endcomment %}
    b. 特定のバージョンをインストールする場合は、有効なパッケージ名を用います。
       これはパッケージ名（`docker-ce`）に加えて、バージョン文字列（２項目め）の初めのコロン（`:`）から初めのハイフン（`-`）までを、ハイフンでつなげます。
       たとえば `docker-ce-18.09.1` となります。

    ```bash
    $ sudo yum -y install docker-ee-<VERSION_STRING> docker-ee-cli-<VERSION_STRING> containerd.io
    ```

    {% comment %}
    For example, if you want to install the 18.09 version run the following:
    {% endcomment %}
    たとえばバージョン 18.09 をインストールする場合は、以下のコマンドになります。

    ```bash
    sudo yum-config-manager --enable docker-ee-stable-18.09
    ```

    {% comment %}
    Docker is installed but not started. The `docker` group is created, but no users are added to the group.
    {% endcomment %}
    Docker はインストールされましたが、まだ起動はしていません。
    グループ ``docker`` が追加されていますが、このグループにはまだユーザーが存在していない状態です。

{% comment %}
3.  Start Docker:
{% endcomment %}
3. Docker を起動します。

    {% comment %}
    > If using `devicemapper`, ensure it is properly configured before starting Docker, per the [storage guide](/storage/storagedriver/device-mapper-driver/){: target="_blank" class="_" }.
    {% endcomment %}
    > `devicemapper` を利用する場合は、各[ストレージドライバー](/storage/storagedriver/device-mapper-driver/){: target="_blank" class="_" }に応じて `devicemapper` が適切に設定できていることを確認してから、Docker を起動してください。

    ```bash
    $ sudo systemctl start docker
    ```

{% comment %}
4.  Verify that Docker Engine - Enterprise is installed correctly by running the `hello-world`
    image. This command downloads a test image, runs it in a container, prints
    an informational message, and exits:
{% endcomment %}
4.  Docker Engine - Enterprise が正しくインストールされているのを確認するため、`hello-world` イメージを実行します。
    このコマンドはテストイメージをダウンロードし、コンテナー内で実行します。
    コンテナーが起動すると、メッセージを表示して終了します。

    ```bash
    $ sudo docker run hello-world
    ```

    {% comment %}
    Docker Engine - Enterprise is installed and running. Use `sudo` to run Docker commands. See
    [Linux postinstall](/install/linux/linux-postinstall.md){: target="_blank" class="_" } to allow
    non-privileged users to run Docker commands.
    {% endcomment %}
    Docker Engine - Enterprise がインストールされ、実行できました。
    Docker コマンドの実行には `sudo` が必要になります。
    続いて [Linux のインストール後](/engine/installation/linux/linux-postinstall.md)に進み、非特権ユーザーでも Docker コマンドが実行できる設定方法を参照してください。

<!---
Shared between centOS.md, rhel.md, oracle.md
--->

{% comment %}
### Upgrade from the repository
{% endcomment %}
{: #upgrade-from-the-repository }
### リポジトリからのアップグレード

{% comment %}
1.  [Add the new repository](#set-up-the-repository).
{% endcomment %}
1.  [新たなリポジトリを追加します](#set-up-the-repository)。

{% comment %}
2.  Follow the [installation instructions](#install-from-the-repository) and install a new version.
{% endcomment %}
2.  [インストール手順](#install-from-the-repository)に従って、新たなバージョンをインストールします。

<!---
Shared between centOS.md, rhel.md, oracle.md
--->

{% comment %}
## Package install and upgrade
{% endcomment %}
{: #package-install-and-upgrade }
## パッケージのインストールとアップグレード

{% comment %}
To manually install Docker Enterprise, download the `.{{ package-format | downcase }}` file for your release. You need to download a new file each time you want to upgrade Docker Enterprise.
{% endcomment %}
Docker Enterprise を手動でインストールするには、ディストリビューションに応じた `.{{ package-format | downcase }}` ファイルをダウンロードします。
Docker Enterprise をアップグレードする際には、常に新しいファイルをダウンロードすることが必要です。

<!---
Shared between centOS.md, rhel.md, oracle.md
--->

{% comment %}
### Install with a package
{% endcomment %}
{: #install-with-a-package }
### パッケージのインストール

{% comment %}
1.  Go to the Docker Engine - Enterprise repository URL associated with your trial or subscription
    in your browser. Go to `{{ linux-dist-url-slug }}/7/x86_64/stable-<VERSION>/Packages`
    and download the `.{{ package-format | downcase }}` file for the Docker version you want to install.
{% endcomment %}
1.  ブラウザー上にてお試し用（trial）あるいは購入者用 の URL により Docker Engine - Enterprise リポジトリにアクセスします。
    `{{ linux-dist-url-slug }}/7/x86_64/stable-<バージョン>/Packages` に移動して、ダウンロードしたいバージョンの `.{{ package-format | downcase }}` ファイルをダウンロードします。

<!---
Not shared
--->

{% comment %}
2.  Install Docker Enterprise, changing the path below to the path where you downloaded
    the Docker package.
{% endcomment %}
2.  Install Docker Enterprise, changing the path below to the path where you downloaded
    the Docker package.

    ```bash
    $ sudo yum install /path/to/package.rpm
    ```

    {% comment %}
    Docker is installed but not started. The `docker` group is created, but no
    users are added to the group.
    {% endcomment %}
    Docker is installed but not started. The `docker` group is created, but no
    users are added to the group.

{% comment %}
3.  Start Docker:
{% endcomment %}
3. Docker を起動します。

    {% comment %}
    > If using `devicemapper`, ensure it is properly configured before starting Docker, per the [storage guide](/storage/storagedriver/device-mapper-driver/){: target="_blank" class="_" }.
    {% endcomment %}
    > If using `devicemapper`, ensure it is properly configured before starting Docker, per the [storage guide](/storage/storagedriver/device-mapper-driver/){: target="_blank" class="_" }.

    ```bash
    $ sudo systemctl start docker
    ```

{% comment %}
4.  Verify that Docker Engine - Enterprise is installed correctly by running the `hello-world`
    image. This command downloads a test image, runs it in a container, prints
    an informational message, and exits:
{% endcomment %}
4.  Verify that Docker Engine - Enterprise is installed correctly by running the `hello-world`
    image. This command downloads a test image, runs it in a container, prints
    an informational message, and exits:

    ```bash
    $ sudo docker run hello-world
    ```

    {% comment %}
    Docker Engine - Enterprise is installed and running. Use `sudo` to run Docker commands. See
    [Linux postinstall](/install/linux/linux-postinstall.md){: target="_blank" class="_" } to allow
    non-privileged users to run Docker commands.
    {% endcomment %}
    Docker Engine - Enterprise is installed and running. Use `sudo` to run Docker commands. See
    [Linux postinstall](/install/linux/linux-postinstall.md){: target="_blank" class="_" } to allow
    non-privileged users to run Docker commands.

<!---
Shared between centOS.md, rhel.md, oracle.md
--->

{% comment %}
### Upgrade with a package
{% endcomment %}
{: #upgrade-with-a-package }
### パッケージのアップグレード

{% comment %}
1.  Download the newer package file.
{% endcomment %}
1.  新しいパッケージファイルをダウンロードします。

{% comment %}
2.  Repeat the [installation procedure](#install-with-a-package), using
    `yum -y upgrade` instead of `yum -y install`, and point to the new file.
{% endcomment %}
2.  [インストール手順](#install-with-a-package)をもう一度行います。
    その際には `yum -y install` でなく `yum -y upgrade` を実行します。
    またパッケージには新しいものを指定します。

<!---
Shared between centOS.md, rhel.md, oracle.md
--->

{% comment %}
## Uninstall Docker Engine - Enterprise
{% endcomment %}
{: #uninstall-docker-engine-enterprise }
## Docker Engine - Enterprise のアンインストール

{% comment %}
1.  Uninstall the Docker Engine - Enterprise package:
{% endcomment %}
1.  Docker Engine - Enterprise パッケージをアンインストールします。

    ```bash
    $ sudo yum -y remove docker-ee
    ```

{% comment %}
2.  Delete all images, containers, and volumes (because these are not automatically removed from your host):
{% endcomment %}
2.  イメージ、コンテナー、ボリュームをすべて削除します。
    （これらはホストマシンから自動的には削除されません。）

    ```bash
    $ sudo rm -rf /var/lib/docker
    ```

{% comment %}
3.  Delete other Docker related resources:
{% endcomment %}
3.  Docker に関連するその他のリソースを削除します。
    ```bash
    $ sudo rm -rf /run/docker
    $ sudo rm -rf /var/run/docker
    $ sudo rm -rf /etc/docker
    ```

{% comment %}
4.  If desired, remove the `devicemapper` thin pool and reformat the block
    devices that were part of it.
{% endcomment %}
4.  必要であれば `devicemapper` のシンプール（thin pool）を削除して、その一部であるブロックデバイスをフォーマットし直します。

{% comment %}
You must delete any edited configuration files manually.
{% endcomment %}
編集した設定ファイルはすべて手動で削除する必要があります。

<!---
Shared between centOS.md, rhel.md, oracle.md
--->

{% comment %}
## Next steps
{% endcomment %}
{: #next-steps }
## 次のステップ


{% comment %}
- Continue to [Post-installation steps for Linux](/install/linux/linux-postinstall.md){: target="_blank" class="_" }
{% endcomment %}
- [Linux のインストール後](/install/linux/linux-postinstall.md){: target="_blank" class="_" }へ進む

{% comment %}
- Continue with user guides on [Universal Control Plane (UCP)](/ee/ucp/){: target="_blank" class="_" } and [Docker Trusted Registry (DTR)](/ee/dtr/){: target="_blank" class="_" }
{% endcomment %}
- [Universal Control Plane (UCP)](/ee/ucp/){: target="_blank" class="_" } や [Docker Trusted Registry (DTR)](/ee/dtr/){: target="_blank" class="_" } のユーザーガイドへ進む
