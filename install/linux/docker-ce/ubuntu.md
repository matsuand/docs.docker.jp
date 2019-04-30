---
description: Ubuntu 上に Docker CE をインストールする手順を説明。
keywords: requirements, apt, installation, ubuntu, install, uninstall, upgrade, update
redirect_from:
- /engine/installation/ubuntulinux/
- /installation/ubuntulinux/
- /engine/installation/linux/ubuntulinux/
- /engine/installation/linux/docker-ce/ubuntu/
- /install/linux/ubuntu/
- /engine/installation/linux/ubuntu/
title: Docker CE の入手（Ubuntu 向け）
toc_max: 4
---

<!--
To get started with Docker CE on Ubuntu, make sure you
[meet the prerequisites](#prerequisites), then
[install Docker](#install-docker-ce).
-->
Ubuntu 向けに Docker CE を始めるには、[前提条件を満たしているか](#prerequisites)を確認してから、[Docker をインストール](#install-docker-ce)してください。

<!--
## Prerequisites
-->
## 前提条件
{: #prerequisites }

<!--
### Docker EE customers
-->
### Docker EE を利用する方は
{: #docker-ee-customers }

<!--
To install Docker Enterprise Edition (Docker EE), go to
[Get Docker EE for Ubuntu](/install/linux/docker-ee/ubuntu.md)
**instead of this topic**.
-->
Docker エンタープライズエディション（Docker Enterprise Edition; EE）をインストールする場合は、
**このページではなく** [Docer EE の入手（Ubuntu 向け）](/install/linux/docker-ee/ubuntu.md)に進んでください。

<!--
To learn more about Docker EE, see
[Docker Enterprise Edition](https://www.docker.com/enterprise-edition/){: target="_blank" class="_" }.
-->
Docker EE の詳細を学ぶには、[Docker エンタープライズエディション](https://www.docker.com/enterprise-edition/)をご覧ください。

<!--
### OS requirements
-->
### OS 要件
{: #os-requirements }

<!--
To install Docker CE, you need the 64-bit version of one of these Ubuntu
versions:
-->
Docker CE をインストールするには、以下に示す Ubuntu の 64 ビットバージョンのいずれかが必要です。

- Cosmic 18.10
- Bionic 18.04 (LTS)
- Xenial 16.04 (LTS)

<!--
Docker CE is supported on `x86_64` (or `amd64`), `armhf`, `arm64`, `s390x`
(IBM Z), and `ppc64le` (IBM Power) architectures.
-->
Docker CE は `x86_64`（または `amd64`）、`armhf`、`arm64`、`s390x`（IBM Z）、`ppc64le`（IBM Power）の各アーキテクチャーをサポートします。

<!--
### Uninstall old versions
-->
### 古いバージョンのアンインストール
{: #uninstall-old-versions }

<!--
Older versions of Docker were called `docker`, `docker.io `, or `docker-engine`.
If these are installed, uninstall them:
-->
Docker のかつてのバージョンは、`docker`、`docker.io`、`docker-engine` と呼ばれていました。
これがインストールされている場合はアンインストールしてください。

```bash
$ sudo apt-get remove docker docker-engine docker.io containerd runc
```

<!--
It's OK if `apt-get` reports that none of these packages are installed.
-->
`apt-get` を実行したときに、上のパッケージがインストールされていないと表示されれば OK です。

<!--
The contents of `/var/lib/docker/`, including images, containers, volumes, and
networks, are preserved. The Docker CE package is now called `docker-ce`.
-->
`/var/lib/docker/` にはイメージ、コンテナー、ボリューム、ネットワークが含まれていて、それは保持されたまま残ります。
なお Docker CE パッケージは、今は `docker-ce` と呼ばれます。

<!--
### Supported storage drivers
-->
### サポートするストレージドライバー
{: #supported-storage-drivers }

<!--
Docker CE on Ubuntu supports `overlay2`, `aufs` and `btrfs` storage drivers.
> **Note**: In Docker Engine - Enterprise, `btrfs` is only supported on SLES. See the documentation on
> [btrfs](/engine/userguide/storagedriver/btrfs-driver.md) for more details.
-->
Ubuntu 向けの Docker CE では `overlay2`、`aufs`、`btrfs` の各ストレージドライバーをサポートします。
> **メモ**: Docker Engine - エンタープライズは SLES 上において `btrfs` のみをサポートします。
> 詳しい説明は [btrfs](/engine/userguide/storagedriver/btrfs-driver.md) を参照してください。

<!--
For new installations on version 4 and higher of the Linux kernel, `overlay2`
is supported and preferred over `aufs`. Docker CE uses the `overlay2`
storage driver by default. If you need to use `aufs` instead, you need to
configure it manually. See [aufs](/engine/userguide/storagedriver/aufs-driver.md)
-->
Linux カーネルのバージョンが 4 以上の最近のインストールでは `overlay2` がサポートされ `aufs` よりも選ばれています。
Docker CE はデフォルトで `overlay2` ストレージドライバーを採用しています。
`aufs` を利用する必要がある場合は、手動で設定しなければなりません。
[aufs](/engine/userguide/storagedriver/aufs-driver.md) を参照してください。

<!--
## Install Docker CE
-->
## Docker CE のインストール
{: #install-docker-ce }

<!--
You can install Docker CE in different ways, depending on your needs:
-->
Docker CE のインストール方法はいくつかあります。
必要に応じて選んでください。

<!--
- Most users
  [set up Docker's repositories](#install-using-the-repository) and install
  from them, for ease of installation and upgrade tasks. This is the
  recommended approach.
-->
- たいていのユーザーは [Docker のリポジトリをセットアップ](#install-using-the-repository)して、そこからインストールしています。
  インストールやアップグレードの作業が簡単だからです。
  この方法をお勧めします。

<!--
- Some users download the DEB package and
  [install it manually](#install-from-a-package) and manage
  upgrades completely manually. This is useful in situations such as installing
  Docker on air-gapped systems with no access to the internet.
-->
- ユーザーの中には RPM パッケージをダウンロードし、[手動でインストール](#install-from-a-package)している方もいます。
  アップグレードも完全に手動となります。
  この方法は、インターネットにアクセスできない環境で Docker をインストールするような場合には有用です。

<!--
- In testing and development environments, some users choose to use automated
  [convenience scripts](#install-using-the-convenience-script) to install Docker.
-->
- テスト環境や開発環境向けに、自動化された[便利なスクリプト](#install-using-the-convenience-script)を使って
  Docker のインストールを行うユーザーもいます。

<!--
### Install using the repository
-->
### リポジトリを利用したインストール
{: #install-using-the-repository }

<!--
Before you install Docker CE for the first time on a new host machine, you need
to set up the Docker repository. Afterward, you can install and update Docker
from the repository.
-->
新しいホストマシンに Docker CE を初めてインストールするときは、その前に Docker リポジトリをセットアップしておくことが必要です。
これを行った後に、リポジトリからの Docker のインストールやアップグレードができるようになります。

<!--
#### Set up the repository
-->
#### リポジトリのセットアップ
{: #set-up-the-repository }

{% assign download-url-base = "https://download.docker.com/linux/ubuntu" %}

<!--
1.  Update the `apt` package index:
-->
1.  `apt` のパッケージインデックスを更新します。

    ```bash
    $ sudo apt-get update
    ```

    <!--
    2.  Install packages to allow `apt` to use a repository over HTTPS:
    -->
2.  `apt` が HTTPS 経由でリポジトリにアクセスしパッケージをインストールできるようにします。

    ```bash
    $ sudo apt-get install \
        apt-transport-https \
        ca-certificates \
        curl \
        gnupg-agent \
        software-properties-common
    ```

    <!--
    3.  Add Docker's official GPG key:
    -->
3. Docker の公式 GPG 鍵を追加します。

    ```bash
    $ curl -fsSL {{ download-url-base }}/gpg | sudo apt-key add -
    ```

    <!--
    Verify that you now have the key with the fingerprint
    `9DC8 5822 9FC7 DD38 854A  E2D8 8D81 803C 0EBF CD88`, by searching for the
    last 8 characters of the fingerprint.
    -->
    鍵を取得し、その指紋が `9DC8 5822 9FC7 DD38 854A  E2D8 8D81 803C 0EBF CD88` であることを確認してください。
    最後の 8 文字の一致を確認します。

    ```bash
    $ sudo apt-key fingerprint 0EBFCD88

    pub   rsa4096 2017-02-22 [SCEA]
          9DC8 5822 9FC7 DD38 854A  E2D8 8D81 803C 0EBF CD88
    uid           [ unknown] Docker Release (CE deb) <docker@docker.com>
    sub   rsa4096 2017-02-22 [S]
    ```

    <!--
    //4.  Use the following command to set up the **stable** repository. To add the
        **nightly** or **test** repository, add the word `nightly` or `test` (or both)
        after the word `stable` in the commands below. [Learn about **nightly** and **test** channels](/install/index.md).
    -->
4.  以下のコマンドを使って**安定版**（stable）リポジトリをセットアップします。
    **最新版**（nightly）、**テスト版**（test）の各リポジトリを追加する場合は、以下のコマンドにおける `stable` の文字に続けて `nightly` や `test` の文字を加えてください。
    [**最新版**と**テスト版**チャンネルを学ぶにはこちら](/install/index.md)。

    <!--
    > **Note**: The `lsb_release -cs` sub-command below returns the name of your
    > Ubuntu distribution, such as `xenial`. Sometimes, in a distribution
    > like Linux Mint, you might need to change `$(lsb_release -cs)`
    > to your parent Ubuntu distribution. For example, if you are using
    >  `Linux Mint Tessa`, you could use `bionic`. Docker does not offer any guarantees on untested
    > and unsupported Ubuntu distributions.
    -->
    > **メモ**
    >
    > サブコマンド `lsb_release -cs` は Ubuntu ディストリビューションの名前、たとえば `xenial` といったものを返します。
    > Linux Mint のようなディストリビューションなどでは、`$(lsb_release -cs)` とする必要があります。
    > たとえば `Linux Mint Tessa` を使っている場合、`bionic` を利用することになります。
    > テスト対象外、サポート対象外の Ubuntu ディストリビューションに対して Docker は何ら動作保証しません。


    <ul class="nav nav-tabs">
      <li class="active"><a data-toggle="tab" data-target="#x86_64_repo">x86_64 / amd64</a></li>
      <li><a data-toggle="tab" data-target="#armhf_repo">armhf</a></li>
      <li><a data-toggle="tab" data-target="#arm64_repo">arm64</a></li>
      <li><a data-toggle="tab" data-target="#ppc64le_repo">ppc64le (IBM Power)</a></li>
      <li><a data-toggle="tab" data-target="#s390x_repo">s390x (IBM Z)</a></li>
    </ul>
    <div class="tab-content">
    <div id="x86_64_repo" class="tab-pane fade in active" markdown="1">

    ```bash
    $ sudo add-apt-repository \
       "deb [arch=amd64] {{ download-url-base }} \
       $(lsb_release -cs) \
       stable"
    ```

    </div>
    <div id="armhf_repo" class="tab-pane fade" markdown="1">

    ```bash
    $ sudo add-apt-repository \
       "deb [arch=armhf] {{ download-url-base }} \
       $(lsb_release -cs) \
       stable"
    ```

    </div>
    <div id="arm64_repo" class="tab-pane fade" markdown="1">

    ```bash
    $ sudo add-apt-repository \
       "deb [arch=arm64] {{ download-url-base }} \
       $(lsb_release -cs) \
       stable"
    ```

    </div>
    <div id="ppc64le_repo" class="tab-pane fade" markdown="1">

    ```bash
    $ sudo add-apt-repository \
       "deb [arch=ppc64el] {{ download-url-base }} \
       $(lsb_release -cs) \
       stable"
    ```

    </div>
    <div id="s390x_repo" class="tab-pane fade" markdown="1">

    ```bash
    $ sudo add-apt-repository \
       "deb [arch=s390x] {{ download-url-base }} \
       $(lsb_release -cs) \
       stable"
    ```

    </div>
    </div> <!-- tab-content -->

<!--
#### Install Docker CE
-->
#### Docker CE のインストール
{: #install-docker-ce }

<!--
1.  Update the `apt` package index.
-->
1.  `apt` のパッケージインデックスを更新します。

    ```bash
    $ sudo apt-get update
    ```

    <!--
    2.  Install the _latest version_ of Docker CE and containerd, or go to the next step to install a specific version:
    -->
2.  Docker CE と containerd の最新版をインストールします。
    あるいは次の手順に行って、特定のバージョンをインストールします。

    ```bash
    $ sudo apt-get install docker-ce docker-ce-cli containerd.io
    ```

    <!--
    > Got multiple Docker repositories?
    >
    > If you have multiple Docker repositories enabled, installing
    > or updating without specifying a version in the `apt-get install` or
    > `apt-get update` command always installs the highest possible version,
    > which may not be appropriate for your stability needs.
    -->
    > 複数リポジトリからの取得？
    >
    > Docker リポジトリを複数有効にしていて、バージョン指定をせずに `apt-get install`
    > によるインストール、または `apt-get update` によるアップデートを行うと、入手可能な最新版がインストールされます。
    > 安定した版が必要である場合には、適切でない場合があります。

    <!--
    3.  To install a _specific version_ of Docker CE, list the available versions in the repo, then select and install:
    -->
3.  特定バージョンの Docker CE をインストールする場合は、リポジトリにある利用可能なバージョンの一覧を確認し、いずれかを選んでインストールします。

    <!--
    a. List the versions available in your repo:
    -->
    a. リポジトリ内にある利用可能なバージョンを一覧表示します。

    ```bash
    $ apt-cache madison docker-ce

      docker-ce | 5:18.09.1~3-0~ubuntu-xenial | {{ download-url-base }}  xenial/stable amd64 Packages
      docker-ce | 5:18.09.0~3-0~ubuntu-xenial | {{ download-url-base }}  xenial/stable amd64 Packages
      docker-ce | 18.06.1~ce~3-0~ubuntu       | {{ download-url-base }}  xenial/stable amd64 Packages
      docker-ce | 18.06.0~ce~3-0~ubuntu       | {{ download-url-base }}  xenial/stable amd64 Packages
      ...
    ```

    <!--
    b. Install a specific version using the version string from the second column,
       for example, `5:18.09.1~3-0~ubuntu-xenial`.
    -->
    b. 特定のバージョンをインストールする場合は、2 項目めにあるバージョン文字列を使ってインストールします。
       たとえば `5:18.09.1~3-0~ubuntu-xenial` となります。

    ```bash
    $ sudo apt-get install docker-ce=<VERSION_STRING> docker-ce-cli=<VERSION_STRING> containerd.io
    ```

    <!--
    //4.  Verify that Docker CE is installed correctly by running the `hello-world`
        image.
    -->
4.  Docker CE が正しくインストールされているのを確認するため、`hello-world` イメージを実行します。

    ```bash
    $ sudo docker run hello-world
    ```

    <!--
    This command downloads a test image and runs it in a container. When the
    container runs, it prints an informational message and exits.
    -->
    このコマンドはテスト用イメージをダウンロードし、コンテナー内で実行します。
    コンテナーが起動すると、メッセージを表示して終了します。

<!--
Docker CE is installed and running. The `docker` group is created but no users
are added to it. You need to use `sudo` to run Docker commands.
Continue to [Linux postinstall](/install/linux/linux-postinstall.md) to allow
non-privileged users to run Docker commands and for other optional configuration
steps.
-->
Docker CE がインストールされ、実行できました。
グループ `docker` が生成されていますが、このグループにはまだユーザーが存在していない状態です。
Docker コマンドの実行には ``sudo`` が必要になります。
続いて [Linux のインストール後](/engine/installation/linux/linux-postinstall.md)に進み、非特権ユーザーでも Docker コマンドが実行できるように、またその他の追加の設定について見ていきます。

<!--
#### Upgrade Docker CE
-->
#### Docker CE のアップグレード
{: #upgrade-docker-ce }

<!--
To upgrade Docker CE, first run `sudo apt-get update`, then follow the
[installation instructions](#install-docker-ce), choosing the new version you want
to install.
-->
Docker CE をアップグレードするには、まず `sudo apt-get update` を実行します。
そして[インストール手順](#install-docker-ce)に従って、インストールしたい新たなバージョンを選んでください。

<!--
### Install from a package
-->
### パッケージからのインストール
{: #install-from-a-package }

<!--
If you cannot use Docker's repository to install Docker CE, you can download the
`.deb` file for your release and install it manually. You need to download
a new file each time you want to upgrade Docker.
-->
Docker リポジトリを利用した Docker インストールができない場合は、目的とするリリースの `.deb` ファイルをダウンロードして、手動でインストールする方法があります。この場合 Docker をアップグレードするには、毎回新たな `.deb` ファイルをダウンロードして利用することになります

<!--
1.  Go to [`{{ download-url-base }}/dists/`]({{ download-url-base }}/dists/){: target="_blank" class="_" },
    choose your Ubuntu version, browse to `pool/stable/`, choose `amd64`,
    `armhf`, `arm64`, `ppc64el`, or `s390x`, and download the `.deb` file for the
    Docker CE version you want to install.
-->
1.  [{{ download-url-base }}/dists/]({{ download-url-base }}/dists/){: target="_blank" class="_" }
    にアクセスして、インストールしたい Ubuntu バージョンを選びます。
    そして `pool/stable/` にアクセスし、`amd64`、`armhf`、`arm64`、`ppc64el`、`s390x` のいずれかを選び、インストールしたい `.deb` ファイルをダウンロードします。

    <!--
    > **Note**: To install a **nightly**  package, change the word
    > `stable` in the  URL to `nightly`.
    > [Learn about **nightly** and **test** channels](/install/index.md).
    -->
    > **メモ**
    >
    > **最新版**パッケージをインストールする場合は
    > URL 内の `stable` を `nightly` に変更してください。
    > [**最新版**と**テスト版**チャンネルを学ぶにはこちら](/install/index.md)。

    <!--
    2.  Install Docker CE, changing the path below to the path where you downloaded
        the Docker package.
    -->
2.  Docker CE をインストールします。
    以下に示すパス部分は、Docker パッケージをダウンロードしたパスに書き換えます。

    ```bash
    $ sudo dpkg -i /path/to/package.deb
    ```

    <!--
    The Docker daemon starts automatically.
    -->
    Docker デーモンは自動的に起動します。

    <!--
    3.  Verify that Docker CE is installed correctly by running the `hello-world`
        image.
    -->
3.  Docker CE が正しくインストールされているのを確認するため `hello-world` イメージを実行します。

    ```bash
    $ sudo docker run hello-world
    ```

    <!--
    This command downloads a test image and runs it in a container. When the
    container runs, it prints an informational message and exits.
    -->
    このコマンドはテスト用イメージをダウンロードし、コンテナー内で実行します。
    コンテナーが起動すると、メッセージを表示して終了します。

<!--
Docker CE is installed and running. The `docker` group is created but no users
are added to it. You need to use `sudo` to run Docker commands.
Continue to [Post-installation steps for Linux](/install/linux/linux-postinstall.md)
to allow non-privileged users to run Docker commands and for other optional
configuration steps.
-->
Docker CE がインストールされ、実行できました。
グループ `docker` が生成されていますが、このグループにはまだユーザーが存在していない状態です。
Docker コマンドの実行には ``sudo`` が必要になります。
続いて [Linux のインストール後](/engine/installation/linux/linux-postinstall.md)に進み、非特権ユーザーでも Docker コマンドが実行できるように、またその他の追加の設定について見ていきます。

<!--
#### Upgrade Docker CE
-->
#### Docker CE のアップグレード
{: #upgrade-docker-ce }

<!--
To upgrade Docker CE, download the newer package file and repeat the
[installation procedure](#install-from-a-package), pointing to the new file.
-->
Docker CE をアップグレードするには、[インストール手順](#install-from-a-package)に従って、インストールしたい新たなバージョンを選んでください。

{% include install-script.md %}

<!--
## Uninstall Docker CE
-->
## Docker CE のアンインストール
{: #uninstall-docker-ce }

<!--
1.  Uninstall the Docker CE package:
-->
1. the Docker CE パッケージをアンインストールします。

    ```bash
    $ sudo apt-get purge docker-ce
    ```

    <!--
    //2.  Images, containers, volumes, or customized configuration files on your host
        are not automatically removed. To delete all images, containers, and
        volumes:
    -->
2.  ホスト上のイメージ、コンテナー、ボリューム、カスタマイズした設定ファイルは自動的に削除されません。
    イメージ、コンテナー、ボリュームをすべて削除するには、以下を実行します。

    ```bash
    $ sudo rm -rf /var/lib/docker
    ```

<!--
You must delete any edited configuration files manually.
-->
編集した設定ファイルはすべて手動で削除する必要があります。

<!--
## Next steps
-->
## 次のステップ
{: #next-steps }

<!--
- Continue to [Post-installation steps for Linux](/install/linux/linux-postinstall.md)
-->
- [Linux のインストール後](/install/linux/linux-postinstall.md) へ進む

<!--
- Continue with the [User Guide](/get-started/index.md).
-->
- [ユーザーガイド](/get-started/index.md) へ進む
