{% assign section = include.section %}

{% comment %}

Include a chunk of this file, using variables already set in the file
where you want to reuse the chunk.

Usage: {% include ee-linux-install-reuse.md section="ee-install-intro" %}

{% endcomment %}


{% if section == "ee-install-intro" %}

{% comment %}
There are two ways to install and upgrade [Docker Enterprise](https://www.docker.com/enterprise-edition/){: target="_blank" class="_" }
on {{ linux-dist-long }}:
{% endcomment %}
[Docker Enterprise](https://www.docker.com/enterprise-edition/){: target="_blank" class="_" } を {{ linux-dist-long }} にインストールしアップグレードする方法には二種類あります。

{% comment %}
- [YUM repository](#repo-install-and-upgrade): Set up a Docker repository and install Docker Engine - Enterprise from it. This is the recommended approach because installation and upgrades are managed with YUM and easier to do.
{% endcomment %}
- [YUM リポジトリ](#repo-install-and-upgrade)利用: Docker リポジトリを設定して、そこから Docker Engine - Enterprise をインストールします。
YUM を使ってインストールしアップグレードも行うため、簡単に操作できます。
この方法を推奨します。

{% comment %}
- [RPM package](#package-install-and-upgrade): Download the {{ package-format }} package, install it manually, and manage upgrades manually. This is useful when installing Docker Engine - Enterprise on air-gapped systems with no access to the internet.
{% endcomment %}
- [RPM パッケージ](#package-install-and-upgrade)利用: {{ package-format }} パッケージをダウンロードして、手動でインストールやアップグレードの管理を行います。この方法は、インターネットへのアクセスができない環境において Docker Engine - Enterprise をインストールすることができます。

{% if linux-dist == "rhel" or linux-dist == "oraclelinux" %}
{% comment %}
Docker Engine - Community is _not_ supported on {{ linux-dist-long }}.
{% endcomment %}
Docker Engine - Community は {{ linux-dist-long }} に対してサポート_されていません_。
{% endif %}
{% if linux-dist == "centos" %}
{% comment %}
For Docker Community Edition on {{ linux-dist-cap }}, see [Get Docker Engine - Community for CentOS](/engine/install/centos.md).
{% endcomment %}
{{ linux-dist-cap }} 向けの Docker Community エディションについては、[Docker Engine - Community の入手（CentOS 向け）](/engine/install/centos.md) を参照してください。
{% endif %}

{% elsif section == "find-ee-repo-url" %}

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


{% elsif section == "using-yum-repo" %}

{% comment %}
The advantage of using a repository from which to install Docker Engine - Enterprise (or any software) is that it provides a certain level of automation. RPM-based distributions such as {{ linux-dist-long }}, use a tool called YUM that work with your repositories to manage dependencies and provide automatic updates.
{% endcomment %}
リポジトリを使って Docker Engine - Enterprise を（あるいはどんなソフトウェアでも）インストールする利点は、ある程度、自動的に操作を進めることができるところです。
{{ linux-dist-long }} のような RPM ベースのディストリビューションでは、YUM というツールを使ってリポジトリを操作し、依存パッケージの管理や自動アップデート機能を提供しています。


{% elsif section == "set-up-yum-repo" %}
{% comment %}
You only need to set up the repository once, after which you can install Docker Engine - Enterprise _from_ the repo and repeatedly upgrade as necessary.
{% endcomment %}
リポジトリをセットアップする必要があるのは一回だけです。
その後は Docker Engine - Enterprise のインストールを、必要であれば何度でも行うことができます。

{% if linux-dist == "rhel" %}

<ul class="nav nav-tabs">
  <li class="active"><a data-toggle="tab" data-target="#RHEL_7" data-group="7">RHEL 7</a></li>
  <li><a data-toggle="tab" data-target="#RHEL_8" data-group="8">RHEL 8</a></li>
</ul>
<div class="tab-content" id="myFirstTab">
<div id="RHEL_7" class="tab-pane fade in active" markdown="1">
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
    Also, store your OS version string in `/etc/yum/vars/dockerosversion`. Most users should use `7` or `8`, but you can also use the more specific minor version, starting from `7.2`.
    {% endcomment %}
    同様に OS バージョン文字列を `/etc/yum/vars/dockerosversion` に保存します。
    たいていのユーザーにとっては `7` や `8` で十分ですが、`7.2` で始まるマイナーバージョンまで指定することもできます。

    ```bash
    $ sudo sh -c 'echo "7" > /etc/yum/vars/dockerosversion'
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

{% comment %}
5.  Enable the `extras` RHEL repository. This ensures access to the `container-selinux` package required by `docker-ee`.
{% endcomment %}
5.  RHEL の `extras` リポジトリを有効にします。
    これにより `docker-ee` が必要としている `container-selinux` パッケージを入手できるようにします。

    {% comment %}
    The repository can differ per your architecture and cloud provider, so review the options in this step before running:
    {% endcomment %}
    リポジトリは利用するアーキテクチャーやクラウドプロバイダーにより異なります。
    したがってここに示す内容を確認してから設定を実行してください。

    {% comment %}
    **For all architectures _except_ IBM Power:**
    {% endcomment %}
    **IBM Power を除くアーキテクチャーすべての場合:**

    ```bash
    $ sudo yum-config-manager --enable rhel-7-server-extras-rpms
    ```

    {% comment %}
    **For IBM Power only (little endian):**
    {% endcomment %}
    **IBM Power の場合（リトルエンディアン）:**

    ```bash
    $ sudo yum-config-manager --enable extras
    $ sudo subscription-manager repos --enable=rhel-7-for-power-le-extras-rpms
    $ sudo yum makecache fast
    $ sudo yum -y install container-selinux
    ```

    {% comment %}
    Depending on cloud provider, you may also need to enable another repository:
    {% endcomment %}
    クラウドプロバイダーによっては、別のリポジトリを有効にする必要があるかもしれません。

    {% comment %}
    **For AWS** (where `REGION` is a literal, and does _not_ represent the region your machine is running in):
    {% endcomment %}
    **AWS の場合** （以下において `REGION` は固定の文字列です。マシンを利用する地域を示すわけではありません。）

    ```bash
    $ sudo yum-config-manager --enable rhui-REGION-rhel-server-extras
    ```

    {% comment %}
    **For Azure:**
    {% endcomment %}
    **Azure の場合**

    ```bash
    $ sudo yum-config-manager --enable rhui-rhel-7-server-rhui-extras-rpms
    ```

6.  Add the Docker Engine - Enterprise **stable** repository:

    ```bash
    $ sudo -E yum-config-manager \
        --add-repo \
        "$DOCKERURL/{{ linux-dist-url-slug }}/docker-ee.repo"
    ```

</div>
<div id="RHEL_8" class="tab-pane fade" markdown="1">
1.  Remove existing Docker repositories from `/etc/yum.repos.d/`:

    ```bash
    $ sudo rm /etc/yum.repos.d/docker*.repo
    ```

2.  Temporarily store the URL (that you [copied above](#find-your-docker-ee-repo-url)) in an environment variable. Replace `<DOCKER-EE-URL>` with your URL in the following command. This variable assignment does not persist when the session ends:

    ```bash
    $ export DOCKERURL="<DOCKER-EE-URL>"
    ```

3.  Store the value of the variable, `DOCKERURL` (from the previous step), in a `yum` variable in `/etc/yum/vars/`:

    ```bash
    $ sudo -E sh -c 'echo "$DOCKERURL/{{ linux-dist-url-slug }}" > /etc/yum/vars/dockerurl'
    ```

    Also, store your OS version string in `/etc/yum/vars/dockerosversion`. Most users should use `8`, but you can also use the more specific minor version.

    ```bash
    $ sudo sh -c 'echo "8" > /etc/yum/vars/dockerosversion'
    ```


4.  Install required packages: `yum-utils` provides the _yum-config-manager_ utility, and `device-mapper-persistent-data` and `lvm2` are required by the _devicemapper_ storage driver:

    ```bash
    $ sudo yum install -y yum-utils \
      device-mapper-persistent-data \
      lvm2
    ```

5.  Add the Docker Engine - Enterprise **stable** repository:

    ```bash
    $ sudo -E yum-config-manager \
        --add-repo \
        "$DOCKERURL/{{ linux-dist-url-slug }}/docker-ee.repo"
    ```

</div>
</div>
{% endif %}

{% if linux-dist != "rhel" %}

1.  Remove existing Docker repositories from `/etc/yum.repos.d/`:

    ```bash
    $ sudo rm /etc/yum.repos.d/docker*.repo
    ```

2.  Temporarily store the URL (that you [copied above](#find-your-docker-ee-repo-url)) in an environment variable. Replace `<DOCKER-EE-URL>` with your URL in the following command. This variable assignment does not persist when the session ends:

    ```bash
    $ export DOCKERURL="<DOCKER-EE-URL>"
    ```

3.  Store the value of the variable, `DOCKERURL` (from the previous step), in a `yum` variable in `/etc/yum/vars/`:

    ```bash
    $ sudo -E sh -c 'echo "$DOCKERURL/{{ linux-dist-url-slug }}" > /etc/yum/vars/dockerurl'
    ```

4.  Install required packages: `yum-utils` provides the _yum-config-manager_ utility, and `device-mapper-persistent-data` and `lvm2` are required by the _devicemapper_ storage driver:

    ```bash
    $ sudo yum install -y yum-utils \
      device-mapper-persistent-data \
      lvm2
    ```

{% if linux-dist == "oraclelinux" %}

{% comment %}
5.  Enable the `ol7_addons` Oracle repository. This ensures access to the `container-selinux` package required by `docker-ee`.
{% endcomment %}
5.  Oracle のリポジトリ `ol7_addons` を有効にします。
    これによって `docker-ee` が必要としている `container-selinux` パッケージへアクセスできるようになります。

    ```bash
    $ sudo yum-config-manager --enable ol7_addons
    ```

{% endif %}

{% comment %}
6.  Add the Docker Engine - Enterprise **stable** repository:
{% endcomment %}
6.  Docker Engine - Enterprise の**安定版**（stable）リポジトリを追加します。

    ```bash
    $ sudo -E yum-config-manager \
        --add-repo \
        "$DOCKERURL/{{ linux-dist-url-slug }}/docker-ee.repo"
    ```
{% endif %}

{% elsif section == "install-using-yum-repo" %}

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

    {% comment %}
    docker-ee.x86_64      {{ site.docker_ee_version }}.ee.2-1.el7.{{ linux-dist }}      docker-ee-stable-18.09
    {% endcomment %}
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
    [Linux postinstall](/engine/install/linux-postinstall.md){: target="_blank" class="_" } to allow
    non-privileged users to run Docker commands.
    {% endcomment %}
    Docker Engine - Enterprise がインストールされ、実行できました。
    Docker コマンドの実行には `sudo` が必要になります。
    続いて [Linux のインストール後](/engine/install/linux-postinstall.md)に進み、非特権ユーザーでも Docker コマンドが実行できる設定方法を参照してください。


{% elsif section == "upgrade-using-yum-repo" %}

{% comment %}
1.  [Add the new repository](#set-up-the-repository).
{% endcomment %}
1.  [新たなリポジトリを追加します](#set-up-the-repository)。

{% comment %}
2.  Follow the [installation instructions](#install-from-the-repository) and install a new version.
{% endcomment %}
2.  [インストール手順](#install-from-the-repository)に従って、新たなバージョンをインストールします。


{% elsif section == "package-installation" %}

{% comment %}
To manually install Docker Enterprise, download the `.{{ package-format | downcase }}` file for your release. You need to download a new file each time you want to upgrade Docker Enterprise.
{% endcomment %}
Docker Enterprise を手動でインストールするには、ディストリビューションに応じた `.{{ package-format | downcase }}` ファイルをダウンロードします。
Docker Enterprise をアップグレードする際には、常に新しいファイルをダウンロードすることが必要です。

{% elsif section == "install-using-yum-package" %}

{% if linux-dist == "rhel" %}
<ul class="nav nav-tabs">
  <li class="active"><a data-toggle="tab" data-target="#RHEL-7" data-group="7">RHEL 7</a></li>
  <li><a data-toggle="tab" data-target="#RHEL-8" data-group="8">RHEL 8</a></li>
</ul>

<div class="tab-content" id="mySecondTab">

<div id="RHEL-7" class="tab-pane fade in active" markdown="1">

{% comment %}
1.  Enable the `extras` RHEL repository. This ensures access to the `container-selinux` package which is required by `docker-ee`:
{% endcomment %}
1.  RHEL の `extras` リポジトリを有効にします。
    これにより `docker-ee` が必要としている `container-selinux` パッケージを入手できるようにします。

    ```bash
    $ sudo yum-config-manager --enable rhel-7-server-extras-rpms
    ```

    {% comment %}
    Alternately, obtain that package manually from Red Hat. There is no way to publicly browse this repository.
    {% endcomment %}
    別の方法として Red Hat から直接パッケージを入手することもできます。
    そのリポジトリは公開されていないので、ブラウザーではアクセスできません。

2.  Go to the Docker Engine - Enterprise repository URL associated with your
    trial or subscription in your browser. Go to
    `{{ linux-dist-url-slug }}/`. Choose your {{ linux-dist-long }} version,
    architecture, and Docker version. Download the
    `.{{ package-format | downcase }}` file from the `Packages` directory.

    > If you have trouble with `selinux` using the packages under the `7` directory,
    > try choosing the version-specific directory instead, such as `7.3`.

3.  Install Docker Enterprise, changing the path below to the path where you downloaded
    the Docker package.

    ```bash
    $ sudo yum install /path/to/package.rpm
    ```

    Docker is installed but not started. The `docker` group is created, but no
    users are added to the group.

4.  Start Docker:

    > If using `devicemapper`, ensure it is properly configured before starting Docker, per the [storage guide](/storage/storagedriver/device-mapper-driver/){: target="_blank" class="_" }.

    ```bash
    $ sudo systemctl start docker
    ```

5.  Verify that Docker Engine - Enterprise is installed correctly by running the `hello-world`
    image. This command downloads a test image, runs it in a container, prints
    an informational message, and exits:

    ```bash
    $ sudo docker run hello-world
    ```

    Docker Engine - Enterprise is installed and running. Use `sudo` to run Docker commands. See
    [Linux postinstall](/engine/install/linux-postinstall.md){: target="_blank" class="_" } to allow
    non-privileged users to run Docker commands.

</div>

<div id="RHEL-8" class="tab-pane fade" markdown="1">

1.  Go to the Docker Engine - Enterprise repository URL associated with your
    trial or subscription in your browser. Go to
    `{{ linux-dist-url-slug }}/`. Choose your {{ linux-dist-long }} version,
    architecture, and Docker version. Download the
    `.{{ package-format | downcase }}` file from the `Packages` directory.

    > If you have trouble with `selinux` using the packages under the `8` directory,
    > try choosing the version-specific directory instead.

2.  Install Docker Enterprise, changing the path below to the path where you downloaded
    the Docker package.

    ```bash
    $ sudo yum install /path/to/package.rpm
    ```

    Docker is installed but not started. The `docker` group is created, but no
    users are added to the group.

3.  Start Docker:

    > If using `devicemapper`, ensure it is properly configured before starting Docker, per the [storage guide](/storage/storagedriver/device-mapper-driver/){: target="_blank" class="_" }.

    ```bash
    $ sudo systemctl start docker
    ```

4.  Verify that Docker Engine - Enterprise is installed correctly by running the `hello-world`
    image. This command downloads a test image, runs it in a container, prints
    an informational message, and exits:

    ```bash
    $ sudo docker run hello-world
    ```

    Docker Engine - Enterprise is installed and running. Use `sudo` to run Docker commands. See
    [Linux postinstall](/engine/install/linux-postinstall.md){: target="_blank" class="_" } to allow
    non-privileged users to run Docker commands.

</div>
</div>

{% endif %}
{% if linux-dist != "rhel" %}
{% if linux-dist == "centos" %}
{% comment %}
1.  Go to the Docker Engine - Enterprise repository URL associated with your trial or subscription
    in your browser. Go to `{{ linux-dist-url-slug }}/7/x86_64/stable-<VERSION>/Packages`
    and download the `.{{ package-format | downcase }}` file for the Docker version you want to install.
{% endcomment %}
1.  ブラウザーから Docker Engine - Enterprise リポジトリ上のお試し用（trial） URL、あるいは購入者用（subscription）URL にアクセスします。
    そして `{{ linux-dist-url-slug }}/7/x86_64/stable-<VERSION>/Packages` を開いて、目的とするバージョンの `.{{ package-format | downcase }}` ファイルをダウンロードします。
{% endif %}

{% if linux-dist == "oraclelinux" %}
{% comment %}
1.  Go to the Docker Engine - Enterprise repository URL associated with your
    trial or subscription in your browser. Go to
    `{{ linux-dist-url-slug }}/`. Choose your {{ linux-dist-long }} version,
    architecture, and Docker version. Download the
    `.{{ package-format | downcase }}` file from the `Packages` directory.
{% endcomment %}
1.  ブラウザーから Docker Engine - Enterprise リポジトリ上のお試し用（trial） URL、あるいは購入者用（subscription）URL にアクセスします。
    そして `{{ linux-dist-url-slug }}/` を開いて、目的とするアーキテクチャーとバージョンの {{ linux-dist-long }} を選びます。
    `Package` ディレクトリから `.{{ package-format | downcase }}` ファイルをダウンロードします。

{% endif %}

{% comment %}
2.  Install Docker Enterprise, changing the path below to the path where you downloaded
    the Docker package.
{% endcomment %}
2.  Docker Enterprise をインストールします。
    以下に示すパス部分は、Docker パッケージをダウンロードしたパスに書き換えます。

    ```bash
    $ sudo yum install /path/to/package.rpm
    ```

    {% comment %}
    Docker is installed but not started. The `docker` group is created, but no
    users are added to the group.
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
    [Linux postinstall](/engine/install/linux-postinstall.md){: target="_blank" class="_" } to allow
    non-privileged users to run Docker commands.
    {% endcomment %}
    Docker Engine - Enterprise がインストールされ、実行できました。
    Docker コマンドの実行には `sudo` が必要になります。
    続いて [Linux のインストール後](/engine/install/linux-postinstall.md){: target="_blank" class="_" } に進み、非特権ユーザーでも Docker コマンドが実行できる設定方法を参照してください。
{% endif %}

{% elsif section == "upgrade-using-yum-package" %}

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


{% elsif section == "yum-uninstall" %}

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


{% elsif section == "linux-install-nextsteps" %}

{% comment %}
- Continue to [Post-installation steps for Linux](/engine/install/linux-postinstall.md){: target="_blank" class="_" }
{% endcomment %}
- [Linux のインストール後](/engine/install/linux-postinstall.md){: target="_blank" class="_" }へ進む

{% comment %}
- Continue with user guides on [Universal Control Plane (UCP)](/ee/ucp/){: target="_blank" class="_" } and [Docker Trusted Registry (DTR)](/ee/dtr/){: target="_blank" class="_" }
{% endcomment %}
- [Universal Control Plane (UCP)](/ee/ucp/){: target="_blank" class="_" } や [Docker Trusted Registry (DTR)](/ee/dtr/){: target="_blank" class="_" } のユーザーガイドへ進む

{% endif %}
