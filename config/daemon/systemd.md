---
description: Controlling and configuring Docker using systemd
keywords: docker, daemon, systemd, configuration
redirect_from:
- /engine/articles/systemd/
- /articles/systemd/
- /engine/admin/systemd/
title: systemd を用いた Docker の管理
---

{% comment %}
Many Linux distributions use systemd to start the Docker daemon. This document
shows a few examples of how to customize Docker's settings.
{% endcomment %}
Linux ディストリビューションでは、Docker デーモンの起動に systemd を用いるものが多くあります。
このドキュメントでは Docker の設定例をいくつか示します。

{% comment %}
## Start the Docker daemon
{% endcomment %}
{: #start-the-docker-daemon }
## Docker デーモンの起動

{% comment %}
### Start manually
{% endcomment %}
{: #start-manually }
### 手動で起動する場合

{% comment %}
Once Docker is installed, you need to start the Docker daemon.
Most Linux distributions use `systemctl` to start services. If you
do not have `systemctl`, use the `service` command.
{% endcomment %}
Docker をインストールしたら Docker デーモンを起動する必要があります。
たいていの Linux ディストリビューションでは `systemctl` を使ってサービスを起動します。
`systemctl` がない場合は `service` コマンドを使ってください。

{% comment %}
- **`systemctl`**:
{% endcomment %}
- **`systemctl` の場合**

  ```bash
  $ sudo systemctl start docker
  ```

{% comment %}
- **`service`**:
{% endcomment %}
- **`service` の場合**

  ```bash
  $ sudo service docker start
  ```

{% comment %}
### Start automatically at system boot
{% endcomment %}
{: #start-automatically-at-system-boot }
### システムブート時に自動起動する場合

{% comment %}
If you want Docker to start at boot, see
[Configure Docker to start on boot](../../engine/install/linux-postinstall.md#configure-docker-to-start-on-boot).
{% endcomment %}
Docker をシステムブート時に起動したい場合は [システムブート時の Docker 起動設定](../../engine/install/linux-postinstall.md#configure-docker-to-start-on-boot) を参照してください。

{% comment %}
## Custom Docker daemon options
{% endcomment %}
{: #custom-docker-daemon-options }
## Docker デーモンオプションのカスタマイズ

{% comment %}
There are a number of ways to configure the daemon flags and environment variables
for your Docker daemon. The recommended way is to use the platform-independent
`daemon.json` file, which is located in `/etc/docker/` on Linux by default. See
[Daemon configuration file](../../engine/reference/commandline/dockerd.md#daemon-configuration-file).
{% endcomment %}
Docker デーモンに対してのデーモンフラグや環境変数を設定する方法はいろいろあります。
推奨されるのは、プラットフォームに依存しない `daemon.json` ファイルを用いる方法です。
この `daemon.json` ファイルは Linux においてはデフォルトで `/etc/docker/` に置かれます。
詳しくは [デーモン設定ファイル](../../engine/reference/commandline/dockerd.md#daemon-configuration-file) を参照してください。

{% comment %}
You can configure nearly all daemon configuration options using `daemon.json`. The following
example configures two options. One thing you cannot configure using `daemon.json` mechanism is
a [HTTP proxy](#http-proxy).
{% endcomment %}
`daemon.json` を使うと、デーモンオプションはほぼすべて設定することができます。
以下の例では 2 つのオプションを設定しています。
`daemon.json` による仕組みで設定できないものに [HTTP プロキシー](#http-proxy) があります。

{% comment %}
### Runtime directory and storage driver
{% endcomment %}
{: #runtime-directory-and-storage-driver }
### 実行時の利用ディレクトリとストレージドライバー

{% comment %}
You may want to control the disk space used for Docker images, containers,
and volumes by moving it to a separate partition.
{% endcomment %}
Docker のイメージ、コンテナー、ボリュームは、別のパーティションを使ってディスク管理を行いたいと考えるかもしれません。

{% comment %}
To accomplish this, set the following flags in the `daemon.json` file:
{% endcomment %}
これを行うには `daemon.json` ファイルにおいて、以下のようなフラグ設定を行います。

```none
{
    "data-root": "/mnt/docker-data",
    "storage-driver": "overlay2"
}
```

{% comment %}
### HTTP/HTTPS proxy
{% endcomment %}
{: #httphttps-proxy }
### HTTP/HTTPS プロキシー

{% comment %}
The Docker daemon uses the `HTTP_PROXY`, `HTTPS_PROXY`, and `NO_PROXY` environmental variables in
its start-up environment to configure HTTP or HTTPS proxy behavior. You cannot configure
these environment variables using the `daemon.json` file.
{% endcomment %}
Docker デーモンではその起動環境において `HTTP_PROXY`, `HTTPS_PROXY`, `NO_PROXY` という環境変数を利用して、HTTP または HTTPS プロキシーの動作を定めています。
この環境変数による設定は `daemon.json` ファイルを用いて行うことはできません。

{% comment %}
This example overrides the default `docker.service` file.
{% endcomment %}
以下は、デフォルトの `docker.service` ファイルを上書き設定する例です。

{% comment %}
If you are behind an HTTP or HTTPS proxy server, for example in corporate settings,
you need to add this configuration in the Docker systemd service file.
{% endcomment %}
企業内で設定されるような HTTP あるいは HTTPS プロキシーサーバーを利用している場合は、Docker systemd サービスファイルに、これらの設定を加える必要があります。

{% comment %}
> **Note for rootless mode**
{% endcomment %}
> **rootless モードに関するメモ**
>
{% comment %}
> The location of systemd configuration files are different when running Docker
> in [rootless mode](../../engine/security/rootless.md). When running in rootless
> mode, Docker is started as a user-mode systemd service, and uses files stored
> in each users' home directory in `~/.config/systemd/user/docker.service.d/`.
> In addition, `systemctl` must be executed without `sudo` and with the `--user`
> flag. Select the _"rootless mode"_ tab below if you are running Docker in rootless mode.
{% endcomment %}
> The location of systemd configuration files are different when running Docker
> in [rootless mode](../../engine/security/rootless.md). When running in rootless
> mode, Docker is started as a user-mode systemd service, and uses files stored
> in each users' home directory in `~/.config/systemd/user/docker.service.d/`.
> In addition, `systemctl` must be executed without `sudo` and with the `--user`
> flag. Select the _"rootless mode"_ tab below if you are running Docker in rootless mode.


<ul class="nav nav-tabs">
  <li class="active"><a data-toggle="tab" data-target="#rootful">通常のインストール</a></li>
  <li><a data-toggle="tab" data-target="#rootless">rootless モード</a></li>
</ul>
<div class="tab-content">
<div id="rootful" class="tab-pane fade in active" markdown="1">

{% comment %}
1.  Create a systemd drop-in directory for the docker service:
{% endcomment %}
1.  Docker サービスに対応した systemd のドロップインディレクトリを生成します。

    ```bash
    sudo mkdir -p /etc/systemd/system/docker.service.d
    ```

{% comment %}
2.  Create a file named `/etc/systemd/system/docker.service.d/http-proxy.conf`
    that adds the `HTTP_PROXY` environment variable:
{% endcomment %}
2.  `/etc/systemd/system/docker.service.d/http-proxy.conf` というファイルを生成して、そこに環境変数 `HTTP_PROXY` の設定を書きます。

    ```conf
    [Service]
    Environment="HTTP_PROXY=http://proxy.example.com:80"
    ```

    {% comment %}
    If you are behind an HTTPS proxy server, set the `HTTPS_PROXY` environment
    variable:
    {% endcomment %}
    HTTPS プロキシーサーバーを利用している場合には、環境変数 `HTTPS_PROXY` の設定を書きます。

    ```conf
    [Service]
    Environment="HTTPS_PROXY=https://proxy.example.com:443"
    ```

    {% comment %}
    Multiple environment variables can be set; to set both a non-HTTPS and
    a HTTPs proxy;
    {% endcomment %}
    環境変数を同時に複数設定することもできます。
    以下は HTTP および HTTPs プロキシーを設定します。

    ```conf
    [Service]
    Environment="HTTP_PROXY=http://proxy.example.com:80"
    Environment="HTTPS_PROXY=https://proxy.example.com:443"
    ```

{% comment %}
3.  If you have internal Docker registries that you need to contact without
    proxying you can specify them via the `NO_PROXY` environment variable.
{% endcomment %}
3.  内部に Docker レジストリがあって、プロキシーを介さずに接続する必要がある場合は、環境変数 `NO_PROXY` を通じて設定することができます。

    {% comment %}
    The `NO_PROXY` variable specifies a string that contains comma-separated
    values for hosts that should be excluded from proxying. These are the
    options you can specify to exclude hosts:
    * IP address prefix (`1.2.3.4`)
    * Domain name, or a special DNS label (`*`)
    * A domain name matches that name and all subdomains. A domain name with
      a leading "." matches subdomains only. For example, given the domains
      `foo.example.com` and `example.com`:
      * `example.com` matches `example.com` and `foo.example.com`, and
      * `.example.com` matches only `foo.example.com`
    * A single asterisk (`*`) indicates that no proxying should be done
    * Literal port numbers are accepted by IP address prefixes (`1.2.3.4:80`)
      and domain names (`foo.example.com:80`)
    {% endcomment %}
    The `NO_PROXY` variable specifies a string that contains comma-separated
    values for hosts that should be excluded from proxying. These are the
    options you can specify to exclude hosts:
    * IP address prefix (`1.2.3.4`)
    * Domain name, or a special DNS label (`*`)
    * A domain name matches that name and all subdomains. A domain name with
      a leading "." matches subdomains only. For example, given the domains
      `foo.example.com` and `example.com`:
      * `example.com` matches `example.com` and `foo.example.com`, and
      * `.example.com` matches only `foo.example.com`
    * A single asterisk (`*`) indicates that no proxying should be done
    * Literal port numbers are accepted by IP address prefixes (`1.2.3.4:80`)
      and domain names (`foo.example.com:80`)

    {% comment %}
    Config example:
    {% endcomment %}
    設定例は以下です。

    ```conf
    [Service]
    Environment="HTTP_PROXY=http://proxy.example.com:80"
    Environment="HTTPS_PROXY=https://proxy.example.com:443"
    Environment="NO_PROXY=localhost,127.0.0.1,docker-registry.example.com,.corp"
    ```

{% comment %}
4.  Flush changes and restart Docker
{% endcomment %}
4.  設定を反映して Docker を再起動します。

    ```bash
    sudo systemctl daemon-reload
    sudo systemctl restart docker
    ```

{% comment %}
5.  Verify that the configuration has been loaded and matches the changes you
    made, for example:
{% endcomment %}
5.  設定が適切にロードされていること、そして変更した内容が反映されていることを確認します。
    たとえば以下のようにします。

    ```bash
    sudo systemctl show --property=Environment docker

    Environment=HTTP_PROXY=http://proxy.example.com:80 HTTPS_PROXY=https://proxy.example.com:443 NO_PROXY=localhost,127.0.0.1,docker-registry.example.com,.corp
    ```

</div>
<div id="rootless" class="tab-pane fade in" markdown="1">

{% comment %}
1.  Create a systemd drop-in directory for the docker service:
{% endcomment %}
1.  Create a systemd drop-in directory for the docker service:

    ```bash
    mkdir -p ~/.config/systemd/user/docker.service.d
    ```

{% comment %}
2.  Create a file named `~/.config/systemd/user/docker.service.d/http-proxy.conf`
    that adds the `HTTP_PROXY` environment variable:
{% endcomment %}
2.  Create a file named `~/.config/systemd/user/docker.service.d/http-proxy.conf`
    that adds the `HTTP_PROXY` environment variable:

    ```conf
    [Service]
    Environment="HTTP_PROXY=http://proxy.example.com:80"
    ```

    {% comment %}
    If you are behind an HTTPS proxy server, set the `HTTPS_PROXY` environment
    variable:
    {% endcomment %}
    If you are behind an HTTPS proxy server, set the `HTTPS_PROXY` environment
    variable:

    ```conf
    [Service]
    Environment="HTTPS_PROXY=https://proxy.example.com:443"
    ```

    {% comment %}
    Multiple environment variables can be set; to set both a non-HTTPS and
    a HTTPs proxy;
    {% endcomment %}
    Multiple environment variables can be set; to set both a non-HTTPS and
    a HTTPs proxy;

    ```conf
    [Service]
    Environment="HTTP_PROXY=http://proxy.example.com:80"
    Environment="HTTPS_PROXY=https://proxy.example.com:443"
    ```

{% comment %}
3.  If you have internal Docker registries that you need to contact without
    proxying, you can specify them via the `NO_PROXY` environment variable.
{% endcomment %}
3.  If you have internal Docker registries that you need to contact without
    proxying, you can specify them via the `NO_PROXY` environment variable.

    {% comment %}
    The `NO_PROXY` variable specifies a string that contains comma-separated
    values for hosts that should be excluded from proxying. These are the
    options you can specify to exclude hosts:
    * IP address prefix (`1.2.3.4`)
    * Domain name, or a special DNS label (`*`)
    * A domain name matches that name and all subdomains. A domain name with
      a leading "." matches subdomains only. For example, given the domains
      `foo.example.com` and `example.com`:
      * `example.com` matches `example.com` and `foo.example.com`, and
      * `.example.com` matches only `foo.example.com`
    * A single asterisk (`*`) indicates that no proxying should be done
    * Literal port numbers are accepted by IP address prefixes (`1.2.3.4:80`)
      and domain names (`foo.example.com:80`)
    {% endcomment %}
    The `NO_PROXY` variable specifies a string that contains comma-separated
    values for hosts that should be excluded from proxying. These are the
    options you can specify to exclude hosts:
    * IP address prefix (`1.2.3.4`)
    * Domain name, or a special DNS label (`*`)
    * A domain name matches that name and all subdomains. A domain name with
      a leading "." matches subdomains only. For example, given the domains
      `foo.example.com` and `example.com`:
      * `example.com` matches `example.com` and `foo.example.com`, and
      * `.example.com` matches only `foo.example.com`
    * A single asterisk (`*`) indicates that no proxying should be done
    * Literal port numbers are accepted by IP address prefixes (`1.2.3.4:80`)
      and domain names (`foo.example.com:80`)

    {% comment %}
    Config example:
    {% endcomment %}
    Config example:

    ```conf
    [Service]
    Environment="HTTP_PROXY=http://proxy.example.com:80"
    Environment="HTTPS_PROXY=https://proxy.example.com:443"
    Environment="NO_PROXY=localhost,127.0.0.1,docker-registry.example.com,.corp"
    ```

{% comment %}
4.  Flush changes and restart Docker
{% endcomment %}
4.  Flush changes and restart Docker

    ```bash
    systemctl --user daemon-reload
    systemctl --user restart docker
    ```

{% comment %}
5.  Verify that the configuration has been loaded and matches the changes you
    made, for example:
{% endcomment %}
5.  Verify that the configuration has been loaded and matches the changes you
    made, for example:

    ```bash
    systemctl --user show --property=Environment docker

    Environment=HTTP_PROXY=http://proxy.example.com:80 HTTPS_PROXY=https://proxy.example.com:443 NO_PROXY=localhost,127.0.0.1,docker-registry.example.com,.corp
    ```

</div>
</div> <!-- tab-content -->


{% comment %}
## Configure where the Docker daemon listens for connections
{% endcomment %}
{: #configure-where-the-docker-daemon-listens-for-connections }
## Configure where the Docker daemon listens for connections

{% comment %}
[Configure where the Docker daemon listens for connections](../../engine/install/linux-postinstall.md#control-where-the-docker-daemon-listens-for-connections).
{% endcomment %}
See
[Configure where the Docker daemon listens for connections](../../engine/install/linux-postinstall.md#control-where-the-docker-daemon-listens-for-connections).

{% comment %}
## Manually create the systemd unit files
{% endcomment %}
{: #manually-create-the-systemd-unit-files }
## systemd ユニットファイルの手動生成

{% comment %}
When installing the binary without a package, you may want
to integrate Docker with systemd. For this, install the two unit files
(`service` and `socket`) from [the github repository](https://github.com/moby/moby/tree/master/contrib/init/systemd)
to `/etc/systemd/system`.
{% endcomment %}
パッケージを利用せずにインストールを行った場合は、systemd を用いた Docker の設定が必要になるはずです。
これを行うには 2 つのユニットファイル（`service` と `socket`）を [Github リポジトリ](https://github.com/moby/moby/tree/master/contrib/init/systemd) から入手して `/etc/systemd/system` に置いてください。
