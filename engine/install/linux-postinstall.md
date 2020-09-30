---
description: Linux インストール後の任意の作業。
keywords: Docker, Docker documentation, requirements, apt, installation, ubuntu, install, uninstall, upgrade, update
title: Linux インストール後の作業
redirect_from:
- /engine/installation/linux/docker-ee/linux-postinstall/
- /engine/installation/linux/linux-postinstall/
- /install/linux/linux-postinstall/
---

{% comment %}
This section contains optional procedures for configuring Linux hosts to work
better with Docker.
{% endcomment %}
ここに示すのは任意の作業ですが、Linux ホストにおいて Docker をよりよく動作させるための設定を行います。

{% comment %}
## Manage Docker as a non-root user
{% endcomment %}
## root ユーザー以外で Docker を管理する
{: #manage-docker-as-a-non-root-user }

{% comment %}
The Docker daemon binds to a Unix socket instead of a TCP port. By default
that Unix socket is owned by the user `root` and other users can only access it
using `sudo`. The Docker daemon always runs as the `root` user.
{% endcomment %}
Docker デーモンは TCP ポートのかわりに Unix ソケットにバインドされます。
デフォルトで Unix ソケットの所有者は `root` ユーザーであるため、他のユーザーは `sudo` を使ってアクセスすることになります。
Docker デーモンは常に `root` ユーザーが起動しています。

{% comment %}
If you don't want to preface the `docker` command with `sudo`, create a Unix
group called `docker` and add users to it. When the Docker daemon starts, it
creates a Unix socket accessible by members of the `docker` group.
{% endcomment %}
`docker` コマンドの実行にあたって `sudo` を用いたくない場合は、`docker` という Unix グループを生成してユーザーをそのグループに加えます。
Docker デーモンが起動したときに生成される Unix ソケットは、`docker` グループに所属するユーザーであればアクセスすることが可能です。

{% comment %}
> Warning
>
> The `docker` group grants privileges equivalent to the `root`
> user. For details on how this impacts security in your system, see
> [*Docker Daemon Attack Surface*](../security/index.md#docker-daemon-attack-surface).
{: .warning}
{% endcomment %}
> 注意
>
> `docker` グループは `root` ユーザーと同等の権限を持ちます。
> このことがシステムセキュリティ上でどのような意味を持つのか、[*Docker Daemon Attack Surface*](../security/index.md#docker-daemon-attack-surface) を参照してください。
{: .warning}

{% comment %}
> **Note**:
>
> To run Docker without root privileges, see
> [Run the Docker daemon as a non-root user (Rootless mode)](../security/rootless.md).
>
> Rootless mode is currently available as an experimental feature.
{% endcomment %}
> **メモ**:
>
> ルート権限なしに Docker をインストールする場合は [非ルートユーザーとして Docker デーモンを起動する (rootless モード)](../security/rootless.md) を参照してください。
>
> rootless モードは現時点では、試験的機能として利用できます。

{% comment %}
To create the `docker` group and add your user:
{% endcomment %}
`docker` グループを生成してユーザーを追加します。

{% comment %}
1.  Create the `docker` group.
{% endcomment %}
1.  `docker` グループを生成します。

    ```bash
    $ sudo groupadd docker
    ```

{% comment %}
2.  Add your user to the `docker` group.
{% endcomment %}
2.  ユーザーを `docker` グループに追加します。

    ```bash
    $ sudo usermod -aG docker $USER
    ```

{% comment %}
3.  Log out and log back in so that your group membership is re-evaluated.
{% endcomment %}
3.  いったんログアウトしてからログインし直してください。
    グループに属することが認識されるようにするためです。

    {% comment %}
    If testing on a virtual machine, it may be necessary to restart the virtual machine for changes to take effect.
    {% endcomment %}
    仮想マシン上でテストを行っている場合は、仮想マシンを再起動して設定を有効にする必要が出てくるかもしれません。

    {% comment %}
    On a desktop Linux environment such as X Windows, log out of your session completely and then log back in.
    {% endcomment %}
    X ウィンドウのような Linux デスクトップ環境においては、いったんセッションから完全にログアウトして、再ログインを行ってください。

    {% comment %}
    On Linux, you can also run the following command to activate the changes to groups:
    {% endcomment %}
    Linux ではまた、以下のコマンドによってグループ変更を行うこともできます。

    ```bash
    $ newgrp docker
    ```

{% comment %}
4.  Verify that you can run `docker` commands without `sudo`.
{% endcomment %}
4.  `sudo` がなくても `docker` コマンドが実行できることを確認します。

    ```bash
    $ docker run hello-world
    ```

    {% comment %}
    This command downloads a test image and runs it in a container. When the
    container runs, it prints an informational message and exits.
    {% endcomment %}
    このコマンドはテストイメージをダウンロードして、コンテナー内で実行します。
    コンテナーが起動すると、メッセージを表示して終了します。

    {% comment %}
    If you initially ran Docker CLI commands using `sudo` before adding
    your user to the `docker` group, you may see the following error,
    which indicates that your `~/.docker/` directory was created with
    incorrect permissions due to the `sudo` commands.
    {% endcomment %}
    `docker` グループへのユーザー追加を行わずに `sudo` を使って Docker CLI コマンドを実行していたときは、以下のエラーが出ていたかも知れません。
    これは `sudo` コマンドを使っているために、`~/.docker/` ディレクトリが不適切なパーミッションにより生成されたことを示しています。

    ```none
    WARNING: Error loading config file: /home/user/.docker/config.json -
    stat /home/user/.docker/config.json: permission denied
    ```

    {% comment %}
    To fix this problem, either remove the `~/.docker/` directory
    (it is recreated automatically, but any custom settings
    are lost), or change its ownership and permissions using the
    following commands:
    {% endcomment %}
    この問題を解消するには、1 つには `~/.docker/` ディレクトリをいったん削除することです。
    （このディレクトリは自動的に再作成されます。ただし追加設定している内容は失われます。）
    あるいは以下のコマンドのようにして、所有者とパーミッションを変更することです。

    ```bash
    $ sudo chown "$USER":"$USER" /home/"$USER"/.docker -R
    $ sudo chmod g+rwx "$HOME/.docker" -R
    ```

{% comment %}
## Configure Docker to start on boot
{% endcomment %}
## システムブート時の Docker 起動設定
{: #configure-docker-to-start-on-boot }

{% comment %}
Most current Linux distributions (RHEL, CentOS, Fedora, Ubuntu 16.04 and higher)
use [`systemd`](#systemd) to manage which services start when the system boots.
Ubuntu 14.10 and below use [`upstart`](#upstart).
{% endcomment %}
最近の Linux ディストリビューション（RHEL、CentOS、Fedora、Ubuntu 16.04 およびそれ以上）の多くでは、[`systemd`](#systemd) を使ってシステムブート時のサービス起動の管理を行っています。
Ubuntu 14.10 およびそれ以下では [`upstart`](#upstart) が用いられています。

### `systemd`

```bash
$ sudo systemctl enable docker
```

{% comment %}
To disable this behavior, use `disable` instead.
{% endcomment %}
無効にするには、かわりに `disable` を用います。

```bash
$ sudo systemctl disable docker
```

{% comment %}
If you need to add an HTTP Proxy, set a different directory or partition for the
Docker runtime files, or make other customizations, see
[customize your systemd Docker daemon options](../../config/daemon/systemd.md).
{% endcomment %}
HTTP プロキシーを加える必要がある場合、Docker の起動に関連するファイルは別のディレクトリ、あるいは別のパーティションに置きます。
あるいは別の設定を行うこともあります。
詳しくは [systemd での Docker デーモンオプションの設定](../../config/daemon/systemd.md) を参照してください。

### `upstart`

{% comment %}
Docker is automatically configured to start on boot using
`upstart`. To disable this behavior, use the following command:
{% endcomment %}
`upstart` を利用している場合、Docker はシステムブート時に自動的に起動するように設定されています。
無効とするには以下のコマンドを実行します。

```bash
$ echo manual | sudo tee /etc/init/docker.override
```

### `chkconfig`

```bash
$ sudo chkconfig docker on
```

{% comment %}
## Use a different storage engine
{% endcomment %}
## 異なるストレージドライバーの利用
{: #use-a-different-storage-engine }

{% comment %}
For information about the different storage engines, see
[Storage drivers](../../storage/storagedriver/index.md).
The default storage engine and the list of supported storage engines depend on
your host's Linux distribution and available kernel drivers.
{% endcomment %}
さまざまなストレージエンジンに関する情報は、[ストレージドライバー](../../storage/storagedriver/index.md) を参照してください。
デフォルトのストレージエンジン、あるいはサポートされるストレージエンジンは何かということは、利用する Linux ディストリビューションおよび利用可能なカーネルドライバーによります。

{% comment %}
## Configure default logging driver
{% endcomment %}
## デフォルトのロギングドライバーの設定
{: #configure-default-logging-driver }

{% comment %}
Docker provides the [capability](../../config/containers/logging/index.md) to collect and view log data from all containers running on a host via a series of logging drivers. The default logging driver, `json-file`, writes log data to JSON-formatted files on the host filesystem. Over time, these log files expand in size, leading to potential exhaustion of disk resources. To alleviate such issues, either configure an alternative logging driver such as Splunk or Syslog, or [set up log rotation](/config/containers/logging/configure/#configure-the-default-logging-driver) for the default driver. If you configure an alternative logging driver, see [Use `docker logs` to read container logs for remote logging drivers](/config/containers/logging/dual-logging/).
{% endcomment %}
Docker では [ロギングドライバーの機能](../../config/containers/logging/index.md) を通じて、ホスト上に稼動するすべてのコンテナーからログを収集して参照する機能が提供されています。
デフォルトのロギングドライバーは `json-file` であり、ホスト上のファイルシステム内に JSON 形式のファイルとしてログデータを保存します。
時間の経過とともにログファイルはサイズが増大するため、気づかないうちにディスクリソースを浪費することにつながります。
この問題を解消するため、Splunk や Syslog といった別のロギングドライバーを設定するか、あるいはデフォルトドライバーに対して [ログローテーション](/config/containers/logging/configure/#configure-the-default-logging-driver) を設定するようにしてください。
別のロギングドライバーを設定する場合は [リモートロギングドライバーにより `docker logs` を使ってコンテナーログを読む](/config/containers/logging/dual-logging/) を参照してください。

{% comment %}
## Configure where the Docker daemon listens for connections
{% endcomment %}
## Docker デーモンがどこからの接続待ちをするかの設定
{: #configure-where-the-docker-daemon-listens-for-connections }

{% comment %}
By default, the Docker daemon listens for connections on a UNIX socket to accept requests from local clients. It is possible to allow Docker to accept requests from remote hosts by configuring it to listen on an IP address and port as well as the UNIX socket. For more detailed information on this configuration option take a look at "Bind Docker to another host/port or a unix socket" section of the [Docker CLI Reference](https://docs.docker.com/engine/reference/commandline/dockerd/) article.
{% endcomment %}
デフォルトにおいて Docker デーモンは、UNIX ソケットにより接続を待ち（リッスンし）、ローカルクライアントからの要求を受け付けます。
リモートホストから要求は UNIX ソケットからだけでなく、設定により IP アドレスおよびポートから受け付けるようにすることも可能です。
そのような設定オプションについての詳細は、[Docker CLI リファレンス](https://docs.docker.com/engine/reference/commandline/dockerd/) のセクション "Bind Docker to another host/port or a unix socket" を参照してください。

{% comment %}
> Secure your connection
>
> Before configuring Docker to accept connections from remote hosts it is critically important that you
> understand the security implications of opening docker to the network. If steps are not taken to secure the connection,
> it is possible for remote non-root users to gain root access on the host. For more information on how to use TLS
> certificates to secure this connection, check this article on
> [how to protect the Docker daemon socket](https://docs.docker.com/engine/security/https/).
{: .warning}
{% endcomment %}
> セキュアな接続
>
> Docker においてリモートホストからの接続を許可する設定を行うには、十分に理解しておくべきことがあります。
> ネットワークに向けて Docker をオープンなものにするということは、セキュリティに関する問題を大いに含んでいるということです。
> 設定を誤ってセキュアな接続が確保されていなかったら、それはつまり、リモートの root ユーザー以外がホスト上の root 権限を奪取してしまいます。
> TLS 証明書を使ってセキュアな接続を行う方法については、[Docker デーモンソケットを守る方法](https://docs.docker.com/engine/security/https/) の説明を参照してください。
{: .warning}

{% comment %}
Configuring Docker to accept remote connections can be done with the `docker.service` systemd unit file for Linux distributions using systemd, such as recent versions of RedHat, CentOS, Ubuntu and SLES, or with the `daemon.json` file which is recommended for Linux distributions that do not use systemd.
{% endcomment %}
リモート接続を許可する設定は systemd ユニットファイル `docker.service` を使って行うことができます。
これは systemd を利用する Linux ディストリビューション、たとえば RedHat、CentOS、Ubuntu、SLES の最新版において設定できます。
あるいは `daemon.json` ファイルを使う方法もあります。
systemd を利用していない Linux ディストリビューションでは、この方法が推奨されます。

{% comment %}
> systemd vs daemon.json
>
> Configuring Docker to listen for connections using both the `systemd` unit file and the `daemon.json`
> file causes a conflict that prevents Docker from starting.
{% endcomment %}
> systemd 対 daemon.json
>
> Docker が接続待ちを行う設定として `systemd` ユニットファイルと `daemon.json` の双方を利用してしまうと、両者が衝突し Docker が起動しなくなりますから注意してください。

{% comment %}
### Configuring remote access with `systemd` unit file
{% endcomment %}
### `systemd` ユニットファイルによるリモートアクセスの設定
{: #configuring-remote-access-with-systemd-unit-file }

{% comment %}
1.  Use the command `sudo systemctl edit docker.service` to open an override file for `docker.service` in a text editor.
{% endcomment %}
1.  `sudo systemctl edit docker.service` とコマンド入力し、`docker.service` に対してオーバーライドするファイルをテキストエディターで開きます。

{% comment %}
2.  Add or modify the following lines, substituting your own values.
{% endcomment %}
2.  以下の行を追加、修正します。
    各値はそれぞれの環境に合わせたものに置き換えます。

    ```none
    [Service]
    ExecStart=
    ExecStart=/usr/bin/dockerd -H fd:// -H tcp://127.0.0.1:2375
    ```

{% comment %}
3. Save the file.
{% endcomment %}
3. ファイルを保存します。

{% comment %}
4. Reload the `systemctl` configuration.
{% endcomment %}
4. `systemctl` の設定を再読み込みします。

   ```bash
    $ sudo systemctl daemon-reload
    ```

{% comment %}
5.  Restart Docker.
{% endcomment %}
5.  Docker を再起動します。

    ```bash
    $ sudo systemctl restart docker.service
    ```

{% comment %}
6.  Check to see whether the change was honored by reviewing the output of `netstat` to confirm `dockerd` is listening on the configured port.
{% endcomment %}
6.  設定の変更が適用されたことを確認するため `netstat` の出力を見ます。
    `dockerd` が指定したポートで接続待ちしていることを確認します。

    ```bash
    $ sudo netstat -lntp | grep dockerd
    tcp        0      0 127.0.0.1:2375          0.0.0.0:*               LISTEN      3758/dockerd
    ```

{% comment %}
### Configuring remote access with `daemon.json`
{% endcomment %}
### `daemon.json` によるリモートアクセスの設定
{: #configuring-remote-access-with-daemonjson }

{% comment %}
1.  Set the `hosts` array in the `/etc/docker/daemon.json` to connect to the UNIX socket and an IP address, as follows:
{% endcomment %}
1.  `/etc/docker/daemon.json` 内の `hosts` に配列として、UNIX ソケットと IP アドレスを以下のように設定します。

    ```json
    {
    "hosts": ["unix:///var/run/docker.sock", "tcp://127.0.0.1:2375"]
    }
    ```

{% comment %}
2.  Restart Docker.
{% endcomment %}
2.  Docker を再起動します。

{% comment %}
3.  Check to see whether the change was honored by reviewing the output of `netstat` to confirm `dockerd` is listening on the configured port.
{% endcomment %}
3.  設定の変更が適用されたことを確認するため `netstat` の出力を見ます。
    `dockerd` が指定したポートで接続待ちしていることを確認します。

    ```bash
    $ sudo netstat -lntp | grep dockerd
    tcp        0      0 127.0.0.1:2375          0.0.0.0:*               LISTEN      3758/dockerd
    ```

{% comment %}
## Enable IPv6 on the Docker daemon
{% endcomment %}
## Docker デーモンの IPv6 利用
{: #enable-ipv6-on-the-docker-daemon }

{% comment %}
To enable IPv6 on the Docker daemon, see
[Enable IPv6 support](../../config/daemon/ipv6.md).
{% endcomment %}
Docker デーモンに対して IPv6 を有効にするには、[IPv6 サポートの有効化](../../config/daemon/ipv6.md) を参照してください。

{% comment %}
## Troubleshooting
{% endcomment %}
## トラブルシューティング
{: #troubleshooting }

{% comment %}
### Kernel compatibility
{% endcomment %}
### カーネルの条件
{: #kernel-compatibility }

{% comment %}
Docker cannot run correctly if your kernel is older than version 3.10 or if it
is missing some modules. To check kernel compatibility, you can download and
run the [`check-config.sh`](https://raw.githubusercontent.com/docker/docker/master/contrib/check-config.sh)
script.
{% endcomment %}
Docker は Linux カーネルが 3.10 より古い場合、あるいは所定のモジュールがない場合には正常に起動しません。
カーネルが適合していることをチェックするには、[`check-config.sh`](https://raw.githubusercontent.com/docker/docker/master/contrib/check-config.sh) スクリプトをダウンロードして実行する方法があります。

```bash
$ curl https://raw.githubusercontent.com/docker/docker/master/contrib/check-config.sh > check-config.sh

$ bash ./check-config.sh
```

{% comment %}
The script only works on Linux, not macOS.
{% endcomment %}
このスクリプトは Linux 上でのみ動作します。
macOS では動作しません。

{% comment %}
### `Cannot connect to the Docker daemon`
{% endcomment %}
### `Cannot connect to the Docker daemon`
{: #cannot-connect-to-the-docker-daemon }

{% comment %}
If you see an error such as the following, your Docker client may be configured
to connect to a Docker daemon on a different host, and that host may not be
reachable.
{% endcomment %}
以下に示すようなエラーが発生した場合は、Docker クライアントから Docker デーモンへ接続する設定が、間違ったホストになっている可能性があります。そしてそのホストへはネットワークが届いていません。

```none
Cannot connect to the Docker daemon. Is 'docker daemon' running on this host?
```

{% comment %}
To see which host your client is configured to connect to, check the value of
the `DOCKER_HOST` variable in your environment.
{% endcomment %}
クライアントがどのホストに接続しにいくように設定されているのかは、環境内にて `DOCKER_HOST` 変数を確認することで分かります。

```bash
$ env | grep DOCKER_HOST
```

{% comment %}
If this command returns a value, the Docker client is set to connect to a
Docker daemon running on that host. If it is unset, the Docker client is set to
connect to the Docker daemon running on the local host. If it is set in error,
use the following command to unset it:
{% endcomment %}
Docker デーモンに対して Docker クライアントが接続するホストは、上のコマンドの戻り値であるホストとなります。
変数が未設定の場合、Docker クライアントはローカルホストで稼動する Docker デーモンに接続しにいきます。
設定に誤りがある場合は、以下のコマンドにより変数を未設定とします。

```bash
$ unset DOCKER_HOST
```

{% comment %}
You may need to edit your environment in files such as `~/.bashrc` or
`~/.profile` to prevent the `DOCKER_HOST` variable from being set
erroneously.
{% endcomment %}
`DOCKER_HOST` を適切に設定するためには、`~/.bashrc` や `~/.profile` といったファイルを用いて環境設定をしておくことが必要かもしません。

{% comment %}
If `DOCKER_HOST` is set as intended, verify that the Docker daemon is running
on the remote host and that a firewall or network outage is not preventing you
from connecting.
{% endcomment %}
`DOCKER_HOST` を正しく設定したら、Docker デーモンがリモートホスト上に稼動していて、ファイアウォールやネットワークには問題がなく、接続ができることを確認します。

{% comment %}
### IP forwarding problems
{% endcomment %}
### IP フォワーディングに関する問題
{: #ip-forwarding-problems }

{% comment %}
If you manually configure your network using `systemd-network` with `systemd`
version 219 or higher, Docker containers may not be able to access your network.
Beginning with `systemd` version 220, the forwarding setting for a given network
(`net.ipv4.conf.<interface>.forwarding`) defaults to *off*. This setting
prevents IP forwarding. It also conflicts with Docker's behavior of enabling
the `net.ipv4.conf.all.forwarding` setting within containers.
{% endcomment %}
`systemd` バージョン 219 またはそれ以上を用いている場合で、`systemd-network` を使ってネットワークを手動で設定している場合、Docker コンテナーがネットワーク上でアクセスできないかもしれません。
`systemd` バージョン 220 以降、指定されたネットワークのフォワーディング設定（`net.ipv4.conf.<interface>.forwarding`）がデフォルトで*オフ*となりました。
この設定では IP フォワーディングが行われません。
またコンテナー内にて `net.ipv4.conf.all.forwarding` を有効にすることは、Docker の動作と衝突を起こします。

{% comment %}
To work around this on RHEL, CentOS, or Fedora, edit the `<interface>.network`
file in `/usr/lib/systemd/network/` on your Docker host
(ex: `/usr/lib/systemd/network/80-container-host0.network`) and add the
following block within the `[Network]` section.
{% endcomment %}
RHEL、CentOS、Fedora においてこれを回避するには、Docker ホスト内の `/usr/lib/systemd/network/` にある `<interface>.network` ファイル（たとえば `/usr/lib/systemd/network/80-container-host0.network`）を編集します。
`[Network]` セクション内に以下の記述を加えます。

```
[Network]
...
IPForward=kernel
# または
IPForward=true
...
```

{% comment %}
This configuration allows IP forwarding from the container as expected.
{% endcomment %}
この設定によって、コンテナー内からの IP フォワーディングが正常に行われるようになります。


{% comment %}
### `DNS resolver found in resolv.conf and containers can't use it`
{% endcomment %}
### `DNS resolver found in resolv.conf and containers can't use it`
{: #dns-resolver-found-in-resolvconf-and-containers-cant-use-it }

{% comment %}
Linux systems which use a GUI often have a network manager running, which uses a
`dnsmasq` instance running on a loopback address such as `127.0.0.1` or
`127.0.1.1` to cache DNS requests, and adds this entry to
`/etc/resolv.conf`. The `dnsmasq` service speeds up
DNS look-ups and also provides DHCP services. This configuration does not work
within a Docker container which has its own network namespace, because
the Docker container resolves loopback addresses such as `127.0.0.1` to
**itself**, and it is very unlikely to be running a DNS server on its own
loopback address.
{% endcomment %}
GUI を利用している Linux システムでは、ネットワークマネージャーを起動していることがよくあります。
そのときには `dnsmasq` インスタンスをループバックアドレス、たとえば `127.0.0.1` や `127.0.1.1` などにおいて稼動させて DNS リクエストをキャッシュしています。
この情報は `/etc/resolv.conf` に項目追加されています。
`dnsmasq` サービスは DNS の問い合わせ処理を高速にし、また DHCP サービスも提供するものです。
Docker コンテナーは独自のネットワーク名前空間を持つため、その設定はコンテナー内では動作しません。
というのも、Docker コンテナーは `127.0.0.1` などのループバックアドレスを**自分そのもの**と解釈するためです。
したがって独自のループバックアドレスのもとで DNS サーバーを稼動させることが、まずできません。

{% comment %}
If Docker detects that no DNS server referenced in `/etc/resolv.conf` is a fully
functional DNS server, the following warning occurs and Docker uses the public
DNS servers provided by Google at `8.8.8.8` and `8.8.4.4` for DNS resolution.
{% endcomment %}
`/etc/resolv.conf` 内に設定されている DNS サーバーが十分に機能していないことが Docker によって検出されると、以下の警告メッセージが出力され、Docker は Google によって提供されている公開の DNS サーバー、つまり `8.8.8.8` と `8.8.4.4` を使って DNS 名前解決を行います。

```none
WARNING: Local (127.0.0.1) DNS resolver found in resolv.conf and containers
can't use it. Using default external servers : [8.8.8.8 8.8.4.4]
```

{% comment %}
If you see this warning, first check to see if you use `dnsmasq`:
{% endcomment %}
この警告メッセージが出力された場合は、まず最初に `dnsmasq` が利用されているかどうかを確認します。

```bash
$ ps aux |grep dnsmasq
```

{% comment %}
If your container needs to resolve hosts which are internal to your network, the
public nameservers are not adequate. You have two choices:
{% endcomment %}
ネットワーク内部にあるホストの名前解決をコンテナーがしなければならないのであれば、公開のネームサーバーを用いるのは不適当です。以下の 2 つの選択を行ってください。

{% comment %}
- You can specify a DNS server for Docker to use, **or**
- You can disable `dnsmasq` in NetworkManager. If you do this, NetworkManager
  adds your true DNS nameserver to `/etc/resolv.conf`, but you lose the
  possible benefits of `dnsmasq`.
{% endcomment %}
- Docker が利用する DNS サーバーを指定してください。**あるいは**
- ネットワークマネージャーによる `dnsmasq` の利用を停止してください。
  これを行った場合、ネットワークマネージャーは `/etc/resolv.conf` に本当の DNS ネームサーバーを追加します。ただし `dnsmasq` による機能性は失われます。

{% comment %}
**You only need to use one of these methods.**
{% endcomment %}
**上の 2 つのうち 1 つだけを選んで実行してください。**

{% comment %}
### Specify DNS servers for Docker
{% endcomment %}
### Docker が利用する DNS サーバーの指定
{: #specify-dns-servers-for-docker }

{% comment %}
The default location of the configuration file is `/etc/docker/daemon.json`. You
can change the location of the configuration file using the `--config-file`
daemon flag. The documentation below assumes the configuration file is located
at `/etc/docker/daemon.json`.
{% endcomment %}
設定ファイルがあるのはデフォルトでは `/etc/docker/daemon.json` です。
この設定ファイルの場所を変更するには、デーモンフラグ `--config-file` を使います。
以降において、設定ファイルは `/etc/docker/daemon.json` にあるものとして説明していきます。

{% comment %}
1.  Create or edit the Docker daemon configuration file, which defaults to
    `/etc/docker/daemon.json` file, which controls the Docker daemon
    configuration.
{% endcomment %}
1.  デフォルトで `/etc/docker/daemon.json` となっている Docker デーモンの設定ファイルを生成編集します。
    このファイルが Docker デーモンの設定を制御します。

    ```bash
    $ sudo nano /etc/docker/daemon.json
    ```

{% comment %}
2.  Add a `dns` key with one or more IP addresses as values. If the file has
    existing contents, you only need to add or edit the `dns` line.
{% endcomment %}
2.  `dns` キーを追加して、その値に 1 つあるいは複数の IP アドレスを設定します。
    このファイルにすでに内容が書き込まれている場合は、`dns` の行だけを追加します。

    ```json
    {
    	"dns": ["8.8.8.8", "8.8.4.4"]
    }
    ```

    {% comment %}
    If your internal DNS server cannot resolve public IP addresses, include at
    least one DNS server which can, so that you can connect to Docker Hub and so
    that your containers can resolve internet domain names.
    {% endcomment %}
    ネットワーク内の DNS サーバーが、パブリック IP アドレスの名前解決ができなかったとしても、少なくともそれができる DNS サーバーをここに加えることになります。
    こうすれば Docker Hub にもアクセスでき、コンテナーがインターネットドメイン名を解決できることになります。

    {% comment %}
    Save and close the file.
    {% endcomment %}
    ファイルを保存して閉じます。

{% comment %}
3.  Restart the Docker daemon.
{% endcomment %}
3.  Docker デーモンを再起動します。

    ```bash
    $ sudo service docker restart
    ```

{% comment %}
4.  Verify that Docker can resolve external IP addresses by trying to pull an
    image:
{% endcomment %}
4.  Docker が外部の IP アドレスを解決できるかどうかを確認するために、イメージを取得してみます。

    ```bash
    $ docker pull hello-world
    ```

{% comment %}
5.  If necessary, verify that Docker containers can resolve an internal hostname
    by pinging it.
{% endcomment %}
5.  必要なら Docker コンテナーが、内部ホスト名も解決できることを確認するため ping を行ってみます。

    ```bash
    $ docker run --rm -it alpine ping -c4 <my_internal_host>

    PING google.com (192.168.1.2): 56 data bytes
    64 bytes from 192.168.1.2: seq=0 ttl=41 time=7.597 ms
    64 bytes from 192.168.1.2: seq=1 ttl=41 time=7.635 ms
    64 bytes from 192.168.1.2: seq=2 ttl=41 time=7.660 ms
    64 bytes from 192.168.1.2: seq=3 ttl=41 time=7.677 ms
    ```

{% comment %}
#### Disable `dnsmasq`
{% endcomment %}
#### `dnsmasq` の無効化
{: #disable-dnsmasq }

##### Ubuntu

{% comment %}
If you prefer not to change the Docker daemon's configuration to use a specific
IP address, follow these instructions to disable `dnsmasq` in NetworkManager.
{% endcomment %}
Docker デーモンが特定の IP アドレスを利用するような設定を行いたくない場合は、以下の手順に従って、ネットワークマネージャーの `dnsmasq` を無効にします。

{% comment %}
1.  Edit the `/etc/NetworkManager/NetworkManager.conf` file.
{% endcomment %}
1.  `/etc/NetworkManager/NetworkManager.conf` ファイルを編集します。

{% comment %}
2.  Comment out the `dns=dnsmasq` line by adding a `#` character to the beginning
    of the line.
{% endcomment %}
2.  `dns=dnsmasq` の行をコメントアウトするため、行の先頭に `#` を書き加えます。

    ```none
    # dns=dnsmasq
    ```

    {% comment %}
    Save and close the file.
    {% endcomment %}
    ファイルを保存して閉じます。

{% comment %}
4.  Restart both NetworkManager and Docker. As an alternative, you can reboot
    your system.
{% endcomment %}
4.  ネットワークマネージャーと Docker をともに再起動します。
    あるいはシステムをリブートします。

    ```bash
    $ sudo restart network-manager
    $ sudo restart docker
    ```

{% comment %}
##### RHEL, CentOS, or Fedora
{% endcomment %}
##### RHEL、CentOS、Fedora
{: #rhel-centos-or-fedora }

{% comment %}
To disable `dnsmasq` on RHEL, CentOS, or Fedora:
{% endcomment %}
RHEL、CentOS、Fedora において `dnsmasq` を無効にするには以下のようにします。

{% comment %}
1.  Disable the `dnsmasq` service:
{% endcomment %}
1.  `dnsmasq` サービスを無効にします。

    ```bash
    $ sudo service dnsmasq stop

    $ sudo systemctl disable dnsmasq
    ```

{% comment %}
2.  Configure the DNS servers manually using the
    [Red Hat documentation](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/6/html/Deployment_Guide/s1-networkscripts-interfaces.html){: target="_blank" class="_"}.
{% endcomment %}
2.  [Red Hat ドキュメント](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/6/html/Deployment_Guide/s1-networkscripts-interfaces.html){: target="_blank" class="_"}を参考にして、DNS サーバーを手動で設定します。

{% comment %}
### Allow access to the remote API through a firewall
{% endcomment %}
### ファイアーウォール越しにリモート API へのアクセスを許可する
{: #allow-access-to-the-remote-api-through-a-firewall }

{% comment %}
If you run a firewall on the same host as you run Docker and you want to access
the Docker Remote API from another host and remote access is enabled, you need
to configure your firewall to allow incoming connections on the Docker port,
which defaults to `2376` if TLS encrypted transport is enabled or `2375`
otherwise.
{% endcomment %}
Docker を起動させているホストにおいてファイアーウォールを設定している場合に、他のホストからの Docker リモート API へアクセスしたり、リモートホストへのアクセスを可能にしたりしたいときは、ファイアウォールの設定が必要であり、Docker ポートを通る内部への接続を有効にします。
TLS 暗号化転送が有効な場合は、デフォルトの`2376`、そうでない場合は`2375`です。

{% comment %}
Two common firewall daemons are
[UFW (Uncomplicated Firewall)](https://help.ubuntu.com/community/UFW) (often
used for Ubuntu systems) and [firewalld](http://www.firewalld.org/) (often used
for RPM-based systems). Consult the documentation for your OS and firewall, but
the following information might help you get started. These options are fairly
permissive and you may want to use a different configuration that locks your
system down more.
{% endcomment %}
代表的なファイアーウォールデーモンといえば、[UFW (Uncomplicated Firewall)](https://help.ubuntu.com/community/UFW)（Ubuntu において利用されることが多い）と [firewalld](http://www.firewalld.org/)（RPMベースのシステムで利用されることが多い）の 2 つがあります。
OS やファイアーウォールに関するドキュメントをよく確認することになりますが、以下の情報も手始めとして参考になるかもしれません。
ただしこの方法ではやや寛容すぎるというので、システムの設定を違った方法で強固にしたいと思うかもしれません。

{% comment %}
- **UFW**: Set `DEFAULT_FORWARD_POLICY="ACCEPT"` in your configuration.
{% endcomment %}
- **UFW**: 設定ファイルにて `DEFAULT_FORWARD_POLICY="ACCEPT"` を設定します。

{% comment %}
- **firewalld**: Add rules similar to the following to your policy (one for
  incoming requests and one for outgoing requests). Be sure the interface names
  and chain names are correct.
{% endcomment %}
- **firewalld**: ポリシーに以下のようなルールを追加します。
  （1 つは内部接続要求用、もう 1 つは外部接続要求用です。）
  インターフェース名とチェイン名が正しいことを確認してください。

  ```xml
  <direct>
    [ <rule ipv="ipv6" table="filter" chain="FORWARD_direct" priority="0"> -i zt0 -j ACCEPT </rule> ]
    [ <rule ipv="ipv6" table="filter" chain="FORWARD_direct" priority="0"> -o zt0 -j ACCEPT </rule> ]
  </direct>
  ```

### `Your kernel does not support cgroup swap limit capabilities`

{% comment %}
On Ubuntu or Debian hosts, You may see messages similar to the following when
working with an image.
{% endcomment %}
Ubuntu や Debian の場合、イメージの動作中に以下のようなメッセージが表示されることがあります。

```none
WARNING: Your kernel does not support swap limit capabilities. Limitation discarded.
```

{% comment %}
This warning does not occur on RPM-based systems, which enable these
capabilities by default.
{% endcomment %}
RPM ベースのシステムではスワップ領域の制限機能（swap limit capability）があるため、このメッセージは発生しません。

{% comment %}
If you don't need these capabilities, you can ignore the warning. You can enable
these capabilities on Ubuntu or Debian by following these instructions. Memory
and swap accounting incur an overhead of about 1% of the total available memory
and a 10% overall performance degradation, even if Docker is not running.
{% endcomment %}
この機能が不要であれば、警告メッセージを無視して構いません。
Ubuntu や Debian においてこの機能を有効にするには以下の手順に従います。
メモリとスワップの計算には、メモリ使用総量の 1% のオーバーヘッドが発生します。
また全体として 10% の処理性能ダウンが発生します。
これは Docker を稼動しなくても発生するものです。

{% comment %}
1.  Log into the Ubuntu or Debian host as a user with `sudo` privileges.
{% endcomment %}
1.  `sudo` 権限を持つユーザーを使って Ubuntu あるいは Debian にログインします。

{% comment %}
2.  Edit the `/etc/default/grub` file. Add or edit the `GRUB_CMDLINE_LINUX` line
    to add the following two key-value pairs:
{% endcomment %}
2.  `/etc/default/grub` ファイルを編集します。
    `GRUB_CMDLINE_LINUX` の行を新規追加あるいは編集して、以下の 2 つのキーバリューのペアを追加します。

    ```none
    GRUB_CMDLINE_LINUX="cgroup_enable=memory swapaccount=1"
    ```

    {% comment %}
    Save and close the file.
    {% endcomment %}
    ファイルを保存して閉じます。

{% comment %}
3.  Update GRUB.
{% endcomment %}
3.  GRUB をアップデートします。

    ```bash
    $ sudo update-grub
    ```

    {% comment %}
    If your GRUB configuration file has incorrect syntax, an error occurs.
    In this case, repeat steps 2 and 3.
    {% endcomment %}
    GRUB の設定ファイルに文法誤りがあれば、エラーが発生します。
    その場合は手順 2 と 3 を再度行います。

    {% comment %}
    The changes take effect when the system is rebooted.
    {% endcomment %}
    変更を有効にするために、システムを再起動します。

{% comment %}
## Next steps
{% endcomment %}
## 次のステップ

{% comment %}
- Take a look at the [Get started](../../get-started/index.md) training modules to learn  how to build an image and run it as a containerized application.
- Review the topics in [Develop with Docker](../../develop/index.md) to learn how to build new applications using Docker.
{% endcomment %}
- [Docker をはじめよう](../../get-started/index.md) に示すトレーニングを見てください。
  イメージのビルド方法や、イメージをコンテナー化アプリケーションとして起動する方法を学んでいきます。
- [Docker を用いた開発](../../develop/index.md) における各項目を参照してください。
  Docker を使ったアプリケーションの構築方法を学びます。
