---
description: CentOS 上に Docker Engine - Community をインストールする手順を説明。
keywords: requirements, apt, installation, centos, rpm, install, uninstall, upgrade, update
redirect_from:
- /engine/installation/centos/
- /engine/installation/linux/docker-ce/centos/
- /install/linux/centos/
- /engine/installation/linux/centos/
title: Docker Engine - Community の入手（CentOS 向け）
toc_max: 4
---

{% comment %}
To get started with Docker Engine - Community on CentOS, make sure you
[meet the prerequisites](#prerequisites), then
[install Docker](#install-docker-ce).
{% endcomment %}
CentOS 向けに Docker Engine - Community を始めるには、[前提条件を満たしているか](#prerequisites)を確認してから、[Docker をインストール](#install-docker-ce)してください。

{% comment %}
## Prerequisites
{% endcomment %}
## 前提条件
{: #prerequisites }

{% comment %}
### Docker EE customers
{% endcomment %}
### Docker EE を利用する方は
{: #docker-ee-customers }

{% comment %}
To install Docker Enterprise Edition (Docker EE), go to
[Get Docker EE for CentOS](/install/linux/docker-ee/centos/)
**instead of this topic**.
{% endcomment %}
Docker Enterprise エディション（Docker Enterprise Edition; EE）をインストールする場合は、
**このページではなく** [Docer EE の入手（CentOS 向け）](/install/linux/docker-ee/centos/)に進んでください。

{% comment %}
To learn more about Docker EE, see
[Docker Enterprise Edition](https://www.docker.com/enterprise-edition/){: target="_blank" class="_" }.
{% endcomment %}
Docker EE の詳細を学ぶには、[Docker Enterprise エディション](https://www.docker.com/enterprise-edition/)をご覧ください。

{% comment %}
### OS requirements
{% endcomment %}
### OS 要件
{: #os-requirements }

{% comment %}
To install Docker Engine - Community, you need a maintained version of CentOS 7. Archived
versions aren't supported or tested.
{% endcomment %}
Docker Engine - Community をインストールするには、保守対象の CentOS 7 が必要です。
古いバージョンはサポートもテストも行っていません。

{% comment %}
The `centos-extras` repository must be enabled. This repository is enabled by
default, but if you have disabled it, you need to
[re-enable it](https://wiki.centos.org/AdditionalResources/Repositories){: target="_blank" class="_" }.
{% endcomment %}
`centos-extras` リポジトリを有効にすることが必要です。
このリポジトリはデフォルトでは有効になっていますが、もし無効にしている場合は、[もう一度有効に](https://wiki.centos.org/AdditionalResources/Repositories)する必要があります。

{% comment %}
The `overlay2` storage driver is recommended.
{% endcomment %}
`overlay2` ストレージドライバーの利用が推奨されます。

{% comment %}
### Uninstall old versions
{% endcomment %}
### 古いバージョンのアンインストール
{: #uninstall-old-versions }

{% comment %}
Older versions of Docker were called `docker` or `docker-engine`. If these are
installed, uninstall them, along with associated dependencies.
{% endcomment %}
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

{% comment %}
It's OK if `yum` reports that none of these packages are installed.
{% endcomment %}
`yum` を実行したときに、上のパッケージがインストールされていないと表示されれば OK です。

{% comment %}
The contents of `/var/lib/docker/`, including images, containers, volumes, and
networks, are preserved. The Docker Engine - Community package is now called `docker-ce`.
{% endcomment %}
`/var/lib/docker/` にはイメージ、コンテナー、ボリューム、ネットワークが含まれていて、それは保持されたまま残ります。
なお Docker Engine - Community パッケージは、今は `docker-ce` と呼ばれます。

{% comment %}
## Install Docker Engine - Community
{% endcomment %}
## Docker Engine - Community のインストール
{: #install-docker-engine-community }

{% comment %}
You can install Docker Engine - Community in different ways, depending on your needs:
{% endcomment %}
Docker Engine - Community のインストール方法はいくつかあります。
必要に応じて選んでください。

{% comment %}
- Most users
  [set up Docker's repositories](#install-using-the-repository) and install
  from them, for ease of installation and upgrade tasks. This is the
  recommended approach.
{% endcomment %}
- たいていのユーザーは [Docker のリポジトリをセットアップ](#install-using-the-repository)して、そこからインストールしています。
  インストールやアップグレードの作業が簡単だからです。
  この方法をお勧めします。

{% comment %}
- Some users download the RPM package and
  [install it manually](#install-from-a-package) and manage
  upgrades completely manually. This is useful in situations such as installing
  Docker on air-gapped systems with no access to the internet.
{% endcomment %}
- ユーザーの中には RPM パッケージをダウンロードし、[手動でインストール](#install-from-a-package)している方もいます。
  アップグレードも完全に手動となります。
  この方法は、インターネットにアクセスできない環境で Docker をインストールするような場合には有用です。

{% comment %}
- In testing and development environments, some users choose to use automated
  [convenience scripts](#install-using-the-convenience-script) to install Docker.
{% endcomment %}
- テスト環境や開発環境向けに、自動化された[便利なスクリプト](#install-using-the-convenience-script)を使って
  Docker のインストールを行うユーザーもいます。

{% comment %}
### Install using the repository
{% endcomment %}
### リポジトリを利用したインストール
{: #install-using-the-repository }

{% comment %}
Before you install Docker Engine - Community for the first time on a new host machine, you need
to set up the Docker repository. Afterward, you can install and update Docker
from the repository.
{% endcomment %}
新しいホストマシンに Docker Engine - Community を初めてインストールするときは、その前に Docker リポジトリをセットアップしておくことが必要です。
これを行った後に、リポジトリからの Docker のインストールやアップグレードができるようになります。

{% comment %}
#### Set up the repository
{% endcomment %}
#### リポジトリのセットアップ
{: #set-up-the-repository }

{% assign download-url-base = "https://download.docker.com/linux/centos" %}

{% comment %}
1.  Install required packages. `yum-utils` provides the `yum-config-manager`
    utility, and `device-mapper-persistent-data` and `lvm2` are required by the
    `devicemapper` storage driver.
{% endcomment %}
1. 必要なパッケージをインストールします。
   `yum-utils` は `yum-config-manager` ユーティリティを提供します。
   また `device-mapper-persistent-data` と `lvm2`  は `devicemapper` ストレージドライバーを利用するために必要です。

    ```bash
    $ sudo yum install -y yum-utils \
      device-mapper-persistent-data \
      lvm2
    ```

{% comment %}
2.  Use the following command to set up the **stable** repository.
{% endcomment %}
2.  以下のコマンドを使って **安定版**（stable）リポジトリをセットアップします。

    ```bash
    $ sudo yum-config-manager \
        --add-repo \
        {{ download-url-base }}/docker-ce.repo
    ```

{% comment %}
> **Optional**: Enable the **nightly** or **test** repositories.
>
> These repositories are included in the `docker.repo` file above but are disabled
> by default. You can enable them alongside the stable repository.  The following
> command enables the **nightly** repository.
>
> ```bash
> $ sudo yum-config-manager --enable docker-ce-nightly
> ```
>
> To enable the **test** channel, run the following command:
>
> ```bash
> $ sudo yum-config-manager --enable docker-ce-test
> ```
>
> You can disable the **nightly** or **test** repository by running the
> `yum-config-manager` command with the `--disable` flag. To re-enable it, use
> the `--enable` flag. The following command disables the **nightly** repository.
>
> ```bash
> $ sudo yum-config-manager --disable docker-ce-nightly
> ```
>
> [Learn about **nightly** and **test** channels](/install/index.md).
{% endcomment %}
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

{% comment %}
#### Install Docker Engine - Community
{% endcomment %}
#### Docker Engine - Community のインストール
{: #install-docker-engine-community }

{% comment %}
1.  Install the _latest version_ of Docker Engine - Community and containerd, or go to the next step to install a specific version:
{% endcomment %}
1.  Docker Engine - Community と containerd の最新版をインストールします。あるいは次の手順に行って、特定のバージョンをインストールします。

    ```bash
    $ sudo yum install docker-ce docker-ce-cli containerd.io
    ```

    {% comment %}
    If prompted to accept the GPG key, verify that the fingerprint matches
    `060A 61C5 1B55 8A7F 742B 77AA C52F EB6B 621E 9F35`, and if so, accept it.
    {% endcomment %}
    GPG 鍵を受け入れるかどうか問われたら、鍵の指紋が `060A 61C5 1B55 8A7F 742B 77AA C52F EB6B 621E 9F35` に間違いないことを確認し、鍵を受け入れてください。

    {% comment %}
    > Got multiple Docker repositories?
    >
    > If you have multiple Docker repositories enabled, installing
    > or updating without specifying a version in the `yum install` or
    > `yum update` command always installs the highest possible version,
    > which may not be appropriate for your stability needs.
    {% endcomment %}
    > 複数リポジトリからの取得？
    >
    > Docker リポジトリを複数有効にしていて、バージョン指定をせずに `yum install`
    > によるインストール、または `yum update` によるアップデートを行うと、入手可能な最新版がインストールされます。
    > 安定した版が必要である場合には、適切でない場合があります。

    {% comment %}
    Docker is installed but not started. The `docker` group is created, but no users are added to the group.
    {% endcomment %}
    Docker はインストールされましたが、まだ起動はしていません。
    グループ `docker` が生成されていますが、このグループにはまだユーザーが存在していない状態です。

{% comment %}
2.  To install a _specific version_ of Docker Engine - Community, list the available versions
    in the repo, then select and install:
{% endcomment %}
2.  特定バージョンの Docker Engine - Community をインストールする場合は、リポジトリにある利用可能なバージョンの一覧を確認し、いずれかを選んでインストールします。

    {% comment %}
    a. List and sort the versions available in your repo. This example sorts
       results by version number, highest to lowest, and is truncated:
    {% endcomment %}
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

    {% comment %}
    The list returned depends on which repositories are enabled, and is specific
    to your version of CentOS (indicated by the `.el7` suffix in this example).
    {% endcomment %}
    この一覧内容は、どのリポジトリを有効にしているかによって変わります。
    また利用している CentOS のバージョンに応じたものになります（この例では ``.e17`` というサフィックスにより示されるバージョンです）。

    {% comment %}
    b. Install a specific version by its fully qualified package name, which is
       the package name (`docker-ce`) plus the version string (2nd column)
       starting at the first colon (`:`), up to the first hyphen, separated by
       a hyphen (`-`). For example, `docker-ce-18.09.1`.
    {% endcomment %}
    b. 特定のバージョンをインストールする場合は、有効なパッケージ名を用います。
       これはパッケージ名（`docker-ce`）に加えて、バージョン文字列（２項目め）の初めのコロン（`:`）から初めのハイフン（`-`）までを、ハイフンでつなげます。
       たとえば `docker-ce-18.09.1` となります。

    ```bash
    $ sudo yum install docker-ce-<バージョン文字列> docker-ce-cli-<バージョン文字列> containerd.io
    ```

    {% comment %}
    Docker is installed but not started. The `docker` group is created, but no users are added to the group.
    {% endcomment %}
    Docker はインストールされましたが、まだ起動はしていません。
    グループ ``docker`` が追加されていますが、このグループにはまだユーザーが存在していない状態です。

{% comment %}
3.  Start Docker.
{% endcomment %}
3. Docker を起動します。

    ```bash
    $ sudo systemctl start docker
    ```

{% comment %}
4.  Verify that Docker Engine - Community is installed correctly by running the `hello-world`
    image.
{% endcomment %}
4. Docker Engine - Community が正しくインストールされているのを確認するため、`hello-world` イメージを実行します。

    ```bash
    $ sudo docker run hello-world
    ```

    {% comment %}
    This command downloads a test image and runs it in a container. When the
    container runs, it prints an informational message and exits.
    {% endcomment %}
    このコマンドはテスト用イメージをダウンロードし、コンテナー内で実行します。
    コンテナーが起動すると、メッセージを表示して終了します。

{% comment %}
Docker Engine - Community is installed and running. You need to use `sudo` to run Docker
commands. Continue to [Linux postinstall](/install/linux/linux-postinstall.md) to allow
non-privileged users to run Docker commands and for other optional configuration
steps.
{% endcomment %}
Docker Engine - Community がインストールされ、実行できました。
Docker コマンドの実行には `sudo` が必要になります。
続いて [Linux のインストール後](/engine/installation/linux/linux-postinstall.md)に進み、非特権ユーザーでも Docker コマンドが実行できるように、またその他の追加の設定について見ていきます。

{% comment %}
#### Upgrade Docker Engine - Community
{% endcomment %}
{: #upgrade-docker-engine-community }
#### Docker Engine - Community のアップグレード

{% comment %}
To upgrade Docker Engine - Community, follow the [installation instructions](#install-docker-ce),
choosing the new version you want to install.
{% endcomment %}
Docker Engine - Community をアップグレードするには、[インストール手順](#install-docker-ce)に従って、インストールしたい新たなバージョンを選んでください。

{% comment %}
### Install from a package
{% endcomment %}
### パッケージからのインストール
{: #install-from-a-package }

{% comment %}
If you cannot use Docker's repository to install Docker, you can download the
`.rpm` file for your release and install it manually. You need to download
a new file each time you want to upgrade Docker Engine - Community.
{% endcomment %}
Docker リポジトリを利用した Docker インストールができない場合は、目的とするリリースの `.rpm` ファイルをダウンロードして、手動でインストールする方法があります。この場合 Docker Engine - Community をアップグレードするには、毎回新たな `.rpm` ファイルをダウンロードして利用することになります

{% comment %}
1.  Go to
    [{{ download-url-base }}/7/x86_64/stable/Packages/]({{ download-url-base }}/7/x86_64/stable/Packages/)
    and download the `.rpm` file for the Docker version you want to install.
{% endcomment %}
1.  [{{ download-url-base }}/7/x86_64/stable/Packages/]({{ download-url-base }}/7/x86_64/stable/Packages/)
    にアクセスして、インストールしたい `.rpm` ファイルをダウンロードします。

    {% comment %}
    > **Note**: To install a **nightly**  or **test** (pre-release) package,
    > change the word `stable` in the above URL to `nightly` or `test`.
    > [Learn about **nightly** and **test** channels](/install/index.md).
    {% endcomment %}
    > **メモ**:  **最新版**または**テスト版**（プレリリース版）パッケージをインストールする場合は
    > URL 内の `stable` を `nightly` または `test` に変更してください。
    > [**最新版**と**テスト版**チャンネルを学ぶにはこちら](/install/index.md)。


{% comment %}
2.  Install Docker Engine - Community, changing the path below to the path where you downloaded
    the Docker package.
{% endcomment %}
2.  Docker Engine - Community をインストールします。
    以下に示すパス部分は、Docker パッケージをダウンロードしたパスに書き換えます。

    ```bash
    $ sudo yum install /path/to/package.rpm
    ```

    {% comment %}
    Docker is installed but not started. The `docker` group is created, but no
    users are added to the group.
    {% endcomment %}
    Docker はインストールされましたが、まだ起動はしていません。
    グループ `docker` が追加されていますが、このグループにはまだユーザーが存在していない状態です。

{% comment %}
3.  Start Docker.
{% endcomment %}
3. Docker を起動します。

    ```bash
    $ sudo systemctl start docker
    ```

{% comment %}
4.  Verify that Docker Engine - Community is installed correctly by running the `hello-world`
    image.
{% endcomment %}
4.  Docker Engine - Community が正しくインストールされているのを確認するため `hello-world` イメージを実行します。

    ```bash
    $ sudo docker run hello-world
    ```

    {% comment %}
    This command downloads a test image and runs it in a container. When the
    container runs, it prints an informational message and exits.
    {% endcomment %}
    このコマンドはテスト用イメージをダウンロードし、コンテナー内で実行します。
    コンテナーが起動すると、メッセージを表示して終了します。

{% comment %}
Docker Engine - Community is installed and running. You need to use `sudo` to run Docker commands.
Continue to [Post-installation steps for Linux](/install/linux/linux-postinstall.md) to allow
non-privileged users to run Docker commands and for other optional configuration
steps.
{% endcomment %}
Docker Engine - Community がインストールされ、実行できました。
Docker コマンドの実行には ``sudo`` が必要になります。
続いて [Linux のインストール後](/engine/installation/linux/linux-postinstall.md)に進み、非特権ユーザーでも Docker コマンドが実行できるように、またその他の追加の設定について見ていきます。

{% comment %}
#### Upgrade Docker Engine - Community
{% endcomment %}
#### Docker Engine - Community のアップグレード
{: #upgrade-docker-engine-community }

{% comment %}
To upgrade Docker Engine - Community, download the newer package file and repeat the
[installation procedure](#install-from-a-package), using `yum -y upgrade`
instead of `yum -y install`, and pointing to the new file.
{% endcomment %}
Docker Engine - Community をアップグレードする場合は、新たなパッケージファイルをダウンロードして、[インストール手順](#install-from-a-package)をもう一度行います。
その際には `yum -y install` でなく `yum -y upgrade` を実行します。
またパッケージには新しいものを指定します。

{% include install-script.md %}

{% comment %}
## Uninstall Docker Engine - Community
{% endcomment %}
## Docker Engine - Community のアンインストール
{: #uninstall-docker-engine-community }

{% comment %}
1.  Uninstall the Docker package:
{% endcomment %}
1.  Docker パッケージをアンインストールします。

    ```bash
    $ sudo yum remove docker-ce
    ```

{% comment %}
2.  Images, containers, volumes, or customized configuration files on your host
    are not automatically removed. To delete all images, containers, and
    volumes:
{% endcomment %}
2.  ホスト上のイメージ、コンテナー、ボリューム、カスタマイズした設定ファイルは自動的に削除されません。
    イメージ、コンテナー、ボリュームをすべて削除するには、以下を実行します。

    ```bash
    $ sudo rm -rf /var/lib/docker
    ```

{% comment %}
You must delete any edited configuration files manually.
{% endcomment %}
編集した設定ファイルはすべて手動で削除する必要があります。

{% comment %}
## Next steps
{% endcomment %}
## 次のステップ
{: #next-steps }

{% comment %}
- Continue to [Post-installation steps for Linux](/install/linux/linux-postinstall.md)
{% endcomment %}
- [Linux のインストール後](/install/linux/linux-postinstall.md) へ進む

{% comment %}
- Continue with the [User Guide](/get-started/index.md).
{% endcomment %}
- [ユーザーガイド](/get-started/index.md) へ進む
