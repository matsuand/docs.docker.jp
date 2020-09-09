---
description: Docker Compose のインストール方法。
keywords: compose, orchestration, install, installation, docker, documentation
title: Docker Compose のインストール
toc_max: 2
---

{% comment %}
You can run Compose on macOS, Windows, and 64-bit Linux.
{% endcomment %}
Docker Compose は macOS、Windows、64 ビット Linux で動作します。

{% comment %}
## Prerequisites
{% endcomment %}
## 前提条件
{: #prerequisites }

{% comment %}
Docker Compose relies on Docker Engine for any meaningful work, so make sure you
have Docker Engine installed either locally or remote, depending on your setup.
{% endcomment %}
Docker Compose は重要な処理を Docker Engine に委ねています。
したがって Docker Engine がインストールされていることを確認します。
それはローカル、リモートいずれでも、設定次第で可能です。

{% comment %}
- On desktop systems like Docker Desktop for Mac and Windows, Docker Compose is
included as part of those desktop installs.
{% endcomment %}
- Docker Desktop for Mac、Docker Desktop for Windows のようなデスクトップシステムにおいては、インストールの一部として Docker Compose が含まれています。

{% comment %}
- On Linux systems, first install the
[Docker Engine](../engine/install/index.md#server){: target="_blank" class="_"}
for your OS as described on the Get Docker page, then come back here for
instructions on installing Compose on
Linux systems.
{% endcomment %}
- Linux システムではまず、 Docker 入手のページに説明されている各 OS 向けの [Docker](../engine/install/index.md#server){: target="_blank" class="_"} をインストールします。
そしてこのページに戻り、Linux システム上での Compose のインストール手順に従います。

{% comment %}
- To run Compose as a non-root user, see [Manage Docker as a non-root user](../engine/install/linux-postinstall.md).
{% endcomment %}
Compose をルートユーザー以外で起動するには、[ルートユーザー以外による Docker の管理](../engine/install/linux-postinstall.md) を参照してください。

{% comment %}
## Install Compose
{% endcomment %}
## Compose のインストール
{: #install-compose }

{% comment %}
Follow the instructions below to install Compose on Mac, Windows, Windows Server
2016, or Linux systems, or find out about alternatives like using the `pip`
Python package manager or installing Compose as a container.
{% endcomment %}
以下に示す手順を通して Mac、Windows、Windows Server 2016、Linux に Compose をインストールします。
またその他の方法として Python パッケージマネージャー `pip` を使う方法や、コンテナーとして Compose をインストール方法もあります。

{% comment %}
> Install a different version
>
> The instructions below outline installation of the current stable release
> (**v{{site.compose_version}}**) of Compose. To install a different version of
> Compose, replace the given release number with the one that you want. Compose
> releases are also listed and available for direct download on the
> [Compose repository release page on GitHub](https://github.com/docker/compose/releases){:target="_blank" class="_"}.
> To install a **pre-release** of Compose, refer to the [install pre-release builds](#install-pre-release-builds)
> section.
{% endcomment %}
> 別バージョンのインストール
>
> 以降に示す手順は Compose の現時点での最新安定版（**v{{site.compose_version}}**）に基づいたものです。
> これ以外のバージョンをインストールする場合は、記述されているリリース番号を、目的とするものに置き換えてください。
> Compose のリリースは [GitHub 上の Compose リポジトリのリリースページ](https://github.com/docker/compose/releases){:target="_blank" class="_"} においても一覧が示され利用可能です。
> Compose の**プレリリース版**をインストールする場合は、[プレリリース版のインストール](#install-pre-release-builds)の説明を参照してください。

<ul class="nav nav-tabs">
<li class="active"><a data-toggle="tab" data-target="#macOS">Mac</a></li>
<li><a data-toggle="tab" data-target="#windows">Windows</a></li>
<li><a data-toggle="tab" data-target="#windows-server">Windows Server</a></li>
<li><a data-toggle="tab" data-target="#linux">Linux</a></li>
<li><a data-toggle="tab" data-target="#alternatives">その他のインストール</a></li>
</ul>
<div class="tab-content">
<div id="macOS" class="tab-pane fade in active" markdown="1">

{% comment %}
### Install Compose on macOS
{% endcomment %}
### macOS への Compose インストール
{: #install-compose-on-macos }

{% comment %}
**Docker Desktop for Mac** and **Docker Toolbox** already include Compose along
with other Docker apps, so Mac users do not need to install Compose separately.
Docker install instructions for these are here:
{% endcomment %}
**Docker Desktop for Mac** と **Docker Toolbox** には、他の Docker アプリとともに Compose が含まれています。したがって Mac ユーザーは Compose を個別にインストールする必要はありません。
上記ソフトウェアのインストール手順は以下にあります。

{% comment %}
  * [Get Docker Desktop for Mac](../docker-for-mac/install.md)
  * [Get Docker Toolbox](../toolbox/overview.md) (for older systems)
{% endcomment %}
  * [Docker Desktop for Mac の入手](../docker-for-mac/install.md)。
  * [Docker Toolbox の入手](../toolbox/overview.md)（古いシステムの場合）。

</div>
<div id="windows" class="tab-pane fade" markdown="1">

{% comment %}
### Install Compose on Windows desktop systems
{% endcomment %}
### Windows デスクトップへの Compose インストール
{: #install-compose-on-windows-desktop-systems }

{% comment %}
**Docker Desktop for Windows** and **Docker Toolbox** already include Compose
along with other Docker apps, so most Windows users do not need to
install Compose separately. Docker install instructions for these are here:
{% endcomment %}
**Docker Desktop for Windows** と **Docker Toolbox** には、他の Docker アプリとともに Compose が含まれています。したがって Windows ユーザーは Compose を個別にインストールする必要はありません。
上記ソフトウェアのインストール手順は以下にあります。

{% comment %}
* [Get Docker Desktop for Windows](../docker-for-windows/install.md)
* [Get Docker Toolbox](../toolbox/overview.md) (for older systems)
{% endcomment %}
* [Docker Desktop for Windows の入手](../docker-for-windows/install.md)。
* [Docker Toolbox の入手](../toolbox/overview.md)（古いシステムの場合）。

{% comment %}
If you are running the Docker daemon and client directly on Microsoft
Windows Server, follow the instructions in the Windows Server tab.
{% endcomment %}
Docker デーモンや Docker クライアントを Windows Server 上で直接起動している場合は、Windows Server のタブ内にある手順に従ってください。

</div>
<div id="windows-server" class="tab-pane fade in active" markdown="1">

{% comment %}
### Install Compose on Windows Server
{% endcomment %}
### Windows Server への Compose インストール
{: #install-compose-on-windows-server }

{% comment %}
Follow these instructions if you are running the Docker daemon and client directly
on Microsoft Windows Server and want to install Docker Compose.
{% endcomment %}
Docker デーモンや Docker クライアントを Microsoft Windows Server 上で直接起動している場合、および Docker Compose をインストールしたい場合は、以下の手順に従ってください。


{% comment %}
1.  Start an "elevated" PowerShell (run it as administrator).
    Search for PowerShell, right-click, and choose
    **Run as administrator**. When asked if you want to allow this app
    to make changes to your device, click **Yes**.
{% endcomment %}
1.  PowerShell を管理者権限で実行。
    PowerShell アイコンを見つけ、右クリックメニューから**管理者として実行**を選びます。
    このアプリによるデバイスへの変更の許可を問われたら**はい**をクリックします。

{% comment %}
2.  In PowerShell, since GitHub now requires TLS1.2, run the following:
{% endcomment %}
2.  GitHub では TLS1.2 が必要となるので、PowerShell において以下を実行します。

    ```powershell
    [Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12
    ```

    {% comment %}
    Then run the following command to download the current stable release of
    Compose (v{{site.compose_version}}):
    {% endcomment %}
    そして以下のコマンドを実行して Compose の現時点での最新版（v{{site.compose_version}}）をダウンロードします。

    ```powershell
    Invoke-WebRequest "https://github.com/docker/compose/releases/download/{{site.compose_version}}/docker-compose-Windows-x86_64.exe" -UseBasicParsing -OutFile $Env:ProgramFiles\Docker\docker-compose.exe
    ```

    {% comment %}
    **Note**: On Windows Server 2019, you can add the Compose executable to `$Env:ProgramFiles\Docker`. Because this directory is  registered in the system `PATH`, you can run the `docker-compose --version` command on the subsequent step with no additional configuration.
    {% endcomment %}
    **メモ**: Windows Server 2019 の場合は、Compose の実行パスとして `$Env:ProgramFiles\Docker` を加えます。
     このディレクトリはシステムの `PATH` に登録されるので、この後の手順における `docker-compose --version` コマンドが、他になにも設定せずに実行できるようになります。

    {% comment %}
    > To install a different version of Compose, substitute `{{site.compose_version}}`
    > with the version of Compose you want to use.
    {% endcomment %}
    > 別バージョンの Compose をインストールする場合は、`{{site.compose_version}}` の部分を、目的とするバージョンに置き換えてください。

{% comment %}
3.  Test the installation.
{% endcomment %}
3.  インストール結果をテストします。

    ```powershell
    docker-compose --version

    docker-compose version {{site.compose_version}}, build 01110ad01
    ```

</div>
<div id="linux" class="tab-pane fade" markdown="1">

{% comment %}
### Install Compose on Linux systems
{% endcomment %}
### Linux システムへの Compose インストール
{: #install-compose-on-linux-systems }

{% comment %}
On Linux, you can download the Docker Compose binary from the
[Compose repository release page on GitHub](https://github.com/docker/compose/releases){:target="_blank" class="_"}.
Follow the instructions from the link, which involve running the `curl` command
in your terminal to download the binaries. These step-by-step instructions are
also included below.
{% endcomment %}
Linux の場合は、[GitHub 上の Compose リポジトリのリリースページ](https://github.com/docker/compose/releases){:target="_blank" class="_"} から Docker Compose の実行バイナリをダウンロードします。
リンク先の手順では、端末画面上から `curl` コマンドを使ってダウンロードを行います。
その手順は以下にも示します。

{% comment %}
> For `alpine`, the following dependency packages are needed:
> `py-pip`, `python-dev`, `libffi-dev`, `openssl-dev`, `gcc`, `libc-dev`, and `make`.
{: .important}
{% endcomment %}
> `alpine` では、以下に示す依存パッケージが必要です。
> `py-pip`, `python-dev`, `libffi-dev`, `openssl-dev`, `gcc`, `libc-dev`,  `make`
{: .important}

{% comment %}
1.  Run this command to download the current stable release of Docker Compose:
{% endcomment %}
1.  以下のコマンドを実行して Docker Compose の現時点での最新安定版をダウンロードします。

    ```bash
    sudo curl -L "https://github.com/docker/compose/releases/download/{{site.compose_version}}/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
    ```

    {% comment %}
    > To install a different version of Compose, substitute `{{site.compose_version}}`
    > with the version of Compose you want to use.
    {% endcomment %}
    > 別バージョンの Compose をインストールする場合は、`{{site.compose_version}}` の部分を、目的とするバージョンに置き換えてください。

    {% comment %}
    If you have problems installing with `curl`, see
    [Alternative Install Options](install.md#alternative-install-options) tab above.
    {% endcomment %}
    `curl` を使ったインストールに問題がある場合は、上のタブにある [その他のインストール](install.md#alternative-install-options) を確認してください。

{% comment %}
2.  Apply executable permissions to the binary:
{% endcomment %}
2.  実行バイナリに対して実行権限を与えます。

    ```bash
    sudo chmod +x /usr/local/bin/docker-compose
    ```

    {% comment %}
    > **Note**: If the command `docker-compose` fails after installation, check your path.
    > You can also create a symbolic link to `/usr/bin` or any other directory in your path.
    {% endcomment %}
    > **メモ**: インストールした後に `docker-compose` の実行に失敗する場合は、パスを確認してください。
    > `/usr/bin`、あるいはパス設定されているディレクトリへのシンボリックリンクを作る方法もあります。

    {% comment %}
    For example:
    {% endcomment %}
    たとえば以下のとおりです。

    ```bash
    sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
    ```

    {% comment %}
    3.  Optionally, install [command completion](completion.md) for the
        `bash` and `zsh` shell.
    {% endcomment %}
3.  必要に応じて `bash` や `zsh` シェルに対する[コマンド補完](completion.md)をインストールします。

{% comment %}
4.  Test the installation.
{% endcomment %}
4.  インストール結果をテストします。

    ```bash
    $ docker-compose --version
    docker-compose version {{site.compose_version}}, build 1110ad01
    ```
</div>
<div id="alternatives" class="tab-pane fade" markdown="1">

{% comment %}
### Alternative install options
{% endcomment %}
### その他のインストール
{: #alternative-install-options }

{% comment %}
- [Install using pip](#install-using-pip)
- [Install as a container](#install-as-a-container)
{% endcomment %}
- [pip を用いたインストール](#install-using-pip)
- [コンテナーとしてインストール](#install-as-a-container)

{% comment %}
#### Install using pip
{% endcomment %}
#### pip を用いたインストール
{: #install-using-pip }

{% comment %}
> For `alpine`, the following dependency packages are needed:
> `py-pip`, `python-dev`, `libffi-dev`, `openssl-dev`, `gcc`, `libc-dev`, and `make`.
{: .important}
{% endcomment %}
> `alpine` では、以下に示す依存パッケージが必要です。
> `py-pip`, `python-dev`, `libffi-dev`, `openssl-dev`, `gcc`, `libc-dev`,  `make`
{: .important}

{% comment %}
Compose can be installed from
[pypi](https://pypi.python.org/pypi/docker-compose) using `pip`. If you install
using `pip`, we recommend that you use a
[virtualenv](https://virtualenv.pypa.io/en/latest/) because many operating
systems have python system packages that conflict with docker-compose
dependencies. See the [virtualenv
tutorial](http://docs.python-guide.org/en/latest/dev/virtualenvs/) to get
started.
{% endcomment %}
Compose は `pip` を使って [pypi](https://pypi.python.org/pypi/docker-compose) からインストールすることができます。
`pip` を使ったインストールでは [virtualenv](https://virtualenv.pypa.io/en/latest/) を用いることをお勧めします。
これは多くのオペレーティングシステムにおいて、python パッケージが docker-compose の依存パッケージと不整合を起こすことがあるからです。
[virtualenv チュートリアル](http://docs.python-guide.org/en/latest/dev/virtualenvs/) から始めてみてください。

```bash
pip install docker-compose
```
{% comment %}
If you are not using virtualenv,
{% endcomment %}
virtualenv を使わない場合は以下のようにします。

```bash
sudo pip install docker-compose
```

{% comment %}
> pip version 6.0 or greater is required.
{% endcomment %}
> pip のバージョンは 6.0 またはそれ以上が必要です。

{% comment %}
#### Install as a container
{% endcomment %}
#### コンテナーとしてインストール
{: #install-as-a-container }

{% comment %}
Compose can also be run inside a container, from a small bash script wrapper. To
install compose as a container run this command:
{% endcomment %}
Compose は 簡易な bash のラッパースクリプトを用いて、コンテナー内部にて実行することもできます。
コンテナーとして Compose をインストールするには、以下のコマンドを実行します。

```bash
sudo curl -L --fail https://github.com/docker/compose/releases/download/{{site.compose_version}}/run.sh -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
```

</div>
</div>

----

{% comment %}
## Install pre-release builds
{% endcomment %}
## プレリリース版のインストール
{: #install-pre-release-builds }

{% comment %}
If you're interested in trying out a pre-release build, you can download release
candidates from the [Compose repository release page on GitHub](https://github.com/docker/compose/releases){: target="_blank" class="_"}.
Follow the instructions from the link, which involves running the `curl` command
in your terminal to download the binaries.
{% endcomment %}
プレリリース版を試してみたい方は [GitHub 上の Compose リポジトリのリリースページ](https://github.com/docker/compose/releases){: target="_blank" class="_"} からリリース候補（release candidate）をダウンロードしてください。
リンク先の手順では、端末画面上から `curl` コマンドを使って実行バイナリをダウンロードします。

{% comment %}
Pre-releases built from the "master" branch are also available for download at
[https://dl.bintray.com/docker-compose/master/](https://dl.bintray.com/docker-compose/master/){: target="_blank" class="_"}.
{% endcomment %}
マスターブランチからビルドされたプレリリース版もあります。
[https://dl.bintray.com/docker-compose/master/](https://dl.bintray.com/docker-compose/master/){: target="_blank" class="_"} からダウンロードしてください。

{% comment %}
> Pre-release builds allow you to try out new features before they are released,
> but may be less stable.
{: .important}
{% endcomment %}
> プレリリース版では、正式リリース前に新機能を試すことができます。
> ただし安定しているかどうかはわかりません。
{: .important}


{% comment %}
## Upgrading
{% endcomment %}
## アップグレード
{: #upgrading }

{% comment %}
If you're upgrading from Compose 1.2 or earlier, remove or
migrate your existing containers after upgrading Compose. This is because, as of
version 1.3, Compose uses Docker labels to keep track of containers, and your
containers need to be recreated to add the labels.
{% endcomment %}
Compose のバージョン 1.2 またはそれ以前からアップグレードを行う場合、アップグレードした後に既存のコンテナーは削除するか移行する必要があります。
Compose 1.3 からは Docker ラベルが導入されたためです。
このラベルとはコンテナーの追跡を行うものであり、古いコンテナーはこのラベルをつけて再生成する必要があります。

{% comment %}
If Compose detects containers that were created without labels, it refuses
to run, so that you don't end up with two sets of them. If you want to keep using
your existing containers (for example, because they have data volumes you want
to preserve), you can use Compose 1.5.x to migrate them with the following
command:
{% endcomment %}
Docker ラベルを持っていないコンテナーであることが検出されると、Compose はそのようなコンテナーの実行を拒否するため利用することができません。
それまで使っていたコンテナーを引き続き利用したい場合（たとえばデータボリュームを用いてデータ保存をしている場合）、Compose 1.5.x を使って、以下のようなコマンドによりデータ移行を行うことができます。

```bash
docker-compose migrate-to-labels
```

{% comment %}
Alternatively, if you're not worried about keeping them, you can remove them.
Compose just creates new ones.
{% endcomment %}
コンテナーを維持しておく必要がないのであれば、削除するだけで構いません。
Compose は新しくコンテナーを生成してくれます。

```bash
docker container rm -f -v myapp_web_1 myapp_db_1 ...
```

{% comment %}
## Uninstallation
{% endcomment %}
## アンインストール
{: #uninstallation }

{% comment %}
To uninstall Docker Compose if you installed using `curl`:
{% endcomment %}
`curl` を使って Docker Compose をインストールしていた場合は、次のようにしてアンインストールします。

```bash
sudo rm /usr/local/bin/docker-compose
```

{% comment %}
To uninstall Docker Compose if you installed using `pip`:
{% endcomment %}
`pip` を使って Docker Compose をインストールしていた場合は、次のようにしてアンインストールします。

```bash
pip uninstall docker-compose
```

{% comment %}
> Got a "Permission denied" error?
>
> If you get a "Permission denied" error using either of the above
> methods, you probably do not have the proper permissions to remove
> `docker-compose`. To force the removal, prepend `sudo` to either of the above
> commands and run again.
{% endcomment %}
> "Permission denied" エラーが出たときは
>
> 上のコマンドのいずれかを実行したときに "Permission denied" エラーが発生したら、それは `docker-compose` を削除するための適切な権限がないことが考えられます。
> どうしても削除したいときは、上のコマンドの先頭に `sudo` をつけて、もう一度コマンドを実行してください。


{% comment %}
## Where to go next
{% endcomment %}
## 次に読むものは
{: #where-to-go-next }

{% comment %}
- [User guide](index.md)
- [Installing Compose](install.md)
- [Getting Started](gettingstarted.md)
- [Command line reference](reference/index.md)
- [Compose file reference](compose-file/index.md)
- [Sample apps with Compose](samples-for-compose.md)
{% endcomment %}
- [ユーザーガイド](index.md)
- [Compose のインストール](install.md)
- [Compose をはじめよう](gettingstarted.md)
- [コマンドラインリファレンス](reference/index.md)
- [Compose ファイルリファレンス](compose-file/index.md)
- [Compose を使ったサンプルアプリ](samples-for-compose.md)
