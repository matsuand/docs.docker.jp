---
description: CentOS 上に Docker CE をインストールする手順を説明。
keywords: requirements, apt, installation, centos, rpm, install, uninstall, upgrade, update
redirect_from:
- /engine/installation/centos/
- /engine/installation/linux/docker-ce/centos/
- /install/linux/centos/
- /engine/installation/linux/centos/
title: Docker CE の入手（CentOS 向け）
toc_max: 4
---

<!--
To get started with Docker CE on CentOS, make sure you
[meet the prerequisites](#prerequisites), then
[install Docker](#install-docker-ce).
-->
CentOS 向けに Docker CE を始めるには、[前提条件を満たしているか](#prerequisites)を確認してから、[Docker をインストール](#install-docker-ce)してください。

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
[Get Docker EE for CentOS](/install/linux/docker-ee/centos/)
**instead of this topic**.
-->
Docker エンタープライズエディション（Docker Enterprise Edition; EE）をインストールする場合は、
**このページではなく** [Docer EE の入手（CentOS 向け）](/install/linux/docker-ee/centos/)に進んでください。

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
To install Docker CE, you need a maintained version of CentOS 7. Archived
versions aren't supported or tested.
-->
Docker CE をインストールするには、保守対象の CentOS 7 が必要です。
古いバージョンはサポートもテストも行っていません。

<!--
The `centos-extras` repository must be enabled. This repository is enabled by
default, but if you have disabled it, you need to
[re-enable it](https://wiki.centos.org/AdditionalResources/Repositories){: target="_blank" class="_" }.
-->
`centos-extras` リポジトリを有効にすることが必要です。
このリポジトリはデフォルトでは有効になっていますが、もし無効にしている場合は、[もう一度有効に](https://wiki.centos.org/AdditionalResources/Repositories)する必要があります。

<!--
The `overlay2` storage driver is recommended.
-->
`overlay2` ストレージドライバーの利用が推奨されます。

<!--
### Uninstall old versions
-->
### 古いバージョンのアンインストール
{: #uninstall-old-versions }

<!--
Older versions of Docker were called `docker` or `docker-engine`. If these are
installed, uninstall them, along with associated dependencies.
-->
Docker のかつてのバージョンは、`docker` あるいは `docker-engine` と呼ばれていました。
これがインストールされている場合は、関連する依存パッケージも含めアンインストールしてください。

```bash
$ sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
```

<!--
It's OK if `yum` reports that none of these packages are installed.
-->
`yum` を実行したときに、上のパッケージがインストールされていないと表示されれば OK です。

<!--
The contents of `/var/lib/docker/`, including images, containers, volumes, and
networks, are preserved. The Docker CE package is now called `docker-ce`.
-->
`/var/lib/docker/` にはイメージ、コンテナー、ボリューム、ネットワークが含まれていて、それは保持されたまま残ります。
なお Docker CE パッケージは、今は `docker-ce` と呼ばれます。

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
- Some users download the RPM package and
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

{% assign download-url-base = "https://download.docker.com/linux/centos" %}

<!--
1.  Install required packages. `yum-utils` provides the `yum-config-manager`
    utility, and `device-mapper-persistent-data` and `lvm2` are required by the
    `devicemapper` storage driver.
-->
1. 必要なパッケージをインストールします。
   `yum-utils` は `yum-config-manager` ユーティリティを提供します。
   また `device-mapper-persistent-data` と `lvm2`  は `devicemapper` ストレージドライバーを利用するために必要です。

    ```bash
    $ sudo yum install -y yum-utils \
      device-mapper-persistent-data \
      lvm2
    ```

    <!--
    //2.  Use the following command to set up the **stable** repository.
    -->
2.  以下のコマンドを使って **安定版**（stable）リポジトリをセットアップします。

    ```bash
    $ sudo yum-config-manager \
        --add-repo \
        {{ download-url-base }}/docker-ce.repo
    ```

> **任意の作業**： **最新版**（nightly）または **テスト版**（test）リポジトリの有効化
>
> このリポジトリは上記の ``docker.repo`` ファイルに含まれていますが、デフォルトで無効になっています。
> このリポジトリを **安定版**（stable）リポジトリとともに有効にします。
> 以下のコマンドは **最新版** リポジトリを有効にします。
>
> ```bash
> $ sudo yum-config-manager --enable docker-ce-nightly
> ```
>
> **テスト版** チャネルを有効にする場合は、以下のコマンドを実行します。
>
> ```bash
> $ sudo yum-config-manager --enable docker-ce-test
> ```
>
> **最新版** リポジトリ、 **テスト版** リポジトリを無効にするには `yum-config-manager` コマンドに `--disable` フラグをつけて実行します。
> 以下のコマンドは **最新版** リポジトリを無効にします。
>
> ```bash
> $ sudo yum-config-manager --disable docker-ce-nightly
> ```
>
> [**最新版** と **テスト版** チャネルについて学ぶのはこちら](/install/index.md)。

<!--
#### Install Docker CE
-->
#### Docker CE のインストール
{: #install-docker-ce }

<!--
1.  Install the _latest version_ of Docker CE and containerd, or go to the next step to install a specific version:
-->
1.  Docker CE と containerd の最新版をインストールします。あるいは次の手順に行って、特定のバージョンをインストールします。

    ```bash
    $ sudo yum install docker-ce docker-ce-cli containerd.io
    ```

    <!--
    If prompted to accept the GPG key, verify that the fingerprint matches
    `060A 61C5 1B55 8A7F 742B 77AA C52F EB6B 621E 9F35`, and if so, accept it.
    -->
    GPG 鍵を受け入れるかどうか問われたら、鍵の指紋が `060A 61C5 1B55 8A7F 742B 77AA C52F EB6B 621E 9F35` に間違いないことを確認し、鍵を受け入れてください。

    <!--
    > Got multiple Docker repositories?
    >
    > If you have multiple Docker repositories enabled, installing
    > or updating without specifying a version in the `yum install` or
    > `yum update` command always installs the highest possible version,
    > which may not be appropriate for your stability needs.
    -->
    > 複数リポジトリからの取得？
    >
    > Docker リポジトリを複数有効にしていて、バージョン指定をせずに `yum install`
    > によるインストール、または `yum update` によるアップデートを行うと、入手可能な最新版がインストールされます。
    > 安定した版が必要である場合には、適切でない場合があります。

    <!--
    Docker is installed but not started. The `docker` group is created, but no users are added to the group.
    -->
    Docker はインストールされましたが、まだ起動はしていません。
    グループ `docker` が生成されていますが、このグループにはまだユーザーが存在していない状態です。

    <!--
    2.  To install a _specific version_ of Docker CE, list the available versions
        in the repo, then select and install:
    -->
2.  特定バージョンの Docker CE をインストールする場合は、リポジトリにある利用可能なバージョンの一覧を確認し、いずれかを選んでインストールします。

    <!--
    a. List and sort the versions available in your repo. This example sorts
       results by version number, highest to lowest, and is truncated:
    -->
    a. リポジトリ内にある利用可能なバージョンを並びかえて一覧表示します。
       以下の例では出力結果をバージョン番号によりソートします。
       一覧は最新のものが上に並びます。バージョンは簡略に表示されます。

    ```bash
    $ yum list docker-ce --showduplicates | sort -r

    docker-ce.x86_64  3:18.09.1-3.el7                     docker-ce-stable
    docker-ce.x86_64  3:18.09.0-3.el7                     docker-ce-stable
    docker-ce.x86_64  18.06.1.ce-3.el7                    docker-ce-stable
    docker-ce.x86_64  18.06.0.ce-3.el7                    docker-ce-stable
    ```

    <!--
    The list returned depends on which repositories are enabled, and is specific
    to your version of CentOS (indicated by the `.el7` suffix in this example).
    -->
    この一覧内容は、どのリポジトリを有効にしているかによって変わります。
    また利用している CentOS のバージョンに応じたものになります（この例では ``.e17`` というサフィックスにより示されるバージョンです）。

    <!--
    b. Install a specific version by its fully qualified package name, which is
       the package name (`docker-ce`) plus the version string (2nd column)
       starting at the first colon (`:`), up to the first hyphen, separated by
       a hyphen (`-`). For example, `docker-ce-18.09.1`.
    -->
    b. 特定のバージョンをインストールする場合は、有効なパッケージ名を用います。
       これはパッケージ名（`docker-ce`）に加えて、バージョン文字列（２項目め）の初めのコロン（`:`）から初めのハイフン（`-`）までを、ハイフンでつなげます。
       たとえば `docker-ce-18.09.1` となります。

    ```bash
    $ sudo yum install docker-ce-<バージョン文字列> docker-ce-cli-<バージョン文字列> containerd.io
    ```

    <!--
    Docker is installed but not started. The `docker` group is created, but no users are added to the group.
    -->
    Docker はインストールされましたが、まだ起動はしていません。
    グループ ``docker`` が追加されていますが、このグループにはまだユーザーが存在していない状態です。

    <!--
    3.  Start Docker.
    -->
3. Docker を起動します。

    ```bash
    $ sudo systemctl start docker
    ```

    <!--
    //4.  Verify that Docker CE is installed correctly by running the `hello-world`
        image.
    -->
4. Docker CE が正しくインストールされているのを確認するため、`hello-world` イメージを実行します。

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
Docker CE is installed and running. You need to use `sudo` to run Docker
commands. Continue to [Linux postinstall](/install/linux/linux-postinstall.md) to allow
non-privileged users to run Docker commands and for other optional configuration
steps.
-->
Docker CE がインストールされ、実行できました。
Docker コマンドの実行には ``sudo`` が必要になります。
続いて [Linux のインストール後](/engine/installation/linux/linux-postinstall.md)に進み、非特権ユーザーでも Docker コマンドが実行できるように、またその他の追加の設定について見ていきます。

<!--
#### Upgrade Docker CE
-->
#### Docker CE のアップグレード
{: #upgrade-docker-ce }

<!--
To upgrade Docker CE, follow the [installation instructions](#install-docker-ce),
choosing the new version you want to install.
-->
Docker CE をアップグレードするには、[インストール手順](#install-docker-ce)に従って、インストールしたい新たなバージョンを選んでください。

<!--
### Install from a package
-->
### パッケージからのインストール
{: #install-from-a-package }

<!--
If you cannot use Docker's repository to install Docker, you can download the
`.rpm` file for your release and install it manually. You need to download
a new file each time you want to upgrade Docker CE.
-->
Docker リポジトリを利用した Docker インストールができない場合は、目的とするリリースの `.rpm` ファイルをダウンロードして、手動でインストールする方法があります。この場合 Docker をアップグレードするには、毎回新たな `.rpm` ファイルをダウンロードして利用することになります

<!--
1.  Go to
    [{{ download-url-base }}/7/x86_64/stable/Packages/]({{ download-url-base }}/7/x86_64/stable/Packages/)
    and download the `.rpm` file for the Docker version you want to install.
-->
1.  [{{ download-url-base }}/7/x86_64/stable/Packages/]({{ download-url-base }}/7/x86_64/stable/Packages/)
    にアクセスして、インストールしたい `.rpm` ファイルをダウンロードします。

    <!--
    > **Note**: To install a **nightly**  or **test** (pre-release) package,
    > change the word `stable` in the above URL to `nightly` or `test`.
    > [Learn about **nightly** and **test** channels](/install/index.md).
    -->
    > **メモ**:  **最新版**または**テスト版**（プレリリース版）パッケージをインストールする場合は
    > URL 内の `stable` を `nightly` または `test` に変更してください。
    > [**最新版**と**テスト版**チャンネルを学ぶにはこちら](/install/index.md)。


    <!--
    2.  Install Docker CE, changing the path below to the path where you downloaded
        the Docker package.
    -->
2.  Docker CE をインストールします。
    以下に示すパス部分は、Docker パッケージをダウンロードしたパスに書き換えます。

    ```bash
    $ sudo yum install /path/to/package.rpm
    ```

    <!--
    Docker is installed but not started. The `docker` group is created, but no
    users are added to the group.
    -->
    Docker はインストールされましたが、まだ起動はしていません。
    グループ `docker` が追加されていますが、このグループにはまだユーザーが存在していない状態です。

    <!--
    3.  Start Docker.
    -->
3. Docker を起動します。

    ```bash
    $ sudo systemctl start docker
    ```

    <!--
    //4.  Verify that Docker CE is installed correctly by running the `hello-world`
        image.
    -->
4.  Docker CE が正しくインストールされているのを確認するため `hello-world` イメージを実行します。

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
Docker CE is installed and running. You need to use `sudo` to run Docker commands.
Continue to [Post-installation steps for Linux](/install/linux/linux-postinstall.md) to allow
non-privileged users to run Docker commands and for other optional configuration
steps.
-->
Docker CE がインストールされ、実行できました。
Docker コマンドの実行には ``sudo`` が必要になります。
続いて [Linux のインストール後](/engine/installation/linux/linux-postinstall.md)に進み、非特権ユーザーでも Docker コマンドが実行できるように、またその他の追加の設定について見ていきます。

<!--
#### Upgrade Docker CE
-->
#### Docker CE のアップグレード
{: #upgrade-docker-ce }

<!--
To upgrade Docker CE, download the newer package file and repeat the
[installation procedure](#install-from-a-package), using `yum -y upgrade`
instead of `yum -y install`, and pointing to the new file.
-->
Docker CE をアップグレードする場合は、新たなパッケージファイルをダウンロードして、[インストール手順](#install-from-a-package)をもう一度行います。
その際には `yum -y install` でなく `yum -y upgrade` を実行します。
またパッケージには新しいものを指定します。

{% include install-script.md %}

<!--
## Uninstall Docker CE
-->
## Docker CE のアンインストール
{: #uninstall-docker-ce }

<!--
1.  Uninstall the Docker package:
-->
1.  Docker パッケージをアンインストールします。

    ```bash
    $ sudo yum remove docker-ce
    ```

    <!--
    2.  Images, containers, volumes, or customized configuration files on your host
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
