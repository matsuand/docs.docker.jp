---
title: オフラインでの UCP インストール
description: Learn how to install Docker Universal Control Plane. on a machine with
  no internet access.
keywords: UCP, install, offline, Docker EE
---

{% comment %}
The procedure to install Docker Universal Control Plane on a host is the same,
whether the host has access to the internet or not.
{% endcomment %}
Docker Universal Control Plane のインストールは、ホストがインターネットに接続していても接続していなくても同じような手順になります。

{% comment %}
The only difference when installing on an offline host is that instead of
pulling the UCP images from Docker Hub, you use a computer that's connected
to the internet to download a single package with all the images. Then you
copy this package to the host where you install UCP. The offline installation
process works only if one of the following is true:
{% endcomment %}
オフラインのホスト上にてインストールを行う場合には、ただ 1 つ違う方法として Docker Hub から UCP イメージをプルするのではなく、インターネットに接続できているコンピューターを利用して、必要なイメージをすべて含んだパッケージを 1 つだけダウンロードします。
そしてこのパッケージを、UCP をインストールしようとしているホストにコピーします。
オフラインによるインストール作業は、以下のいずれかの場合に可能となります。

{% comment %}
-  All of the cluster nodes, managers and workers alike, have internet access
   to Docker Hub, and
-  None of the nodes, managers and workers alike, have internet access to
   Docker Hub.
{% endcomment %}
-  マネージャーノード、ワーカーノードなど、すべてのクラスターノードがインターネット接続により Docker Hub へのアクセスができること。
-  マネージャーノード、ワーカーノードなど、すべてのクラスターノードがインターネット接続により Docker Hub へのアクセスができないこと。

{% comment %}
If the managers have access to Docker Hub while the workers don't,
installation will fail.
{% endcomment %}
Docker Hub へのアクセスが、マネージャーのみできてワーカーができない場合、インストールは失敗します。

{% comment %}
## Versions available
{% endcomment %}
{: #versions-available }
## 利用可能なバージョン

{% comment %}
Use a computer with internet access to download the UCP package from the
following links.
{% endcomment %}
インターネットに接続可能なコンピューターを使って、以下のリンクから UCP パッケージをダウンロードします。

{% include components/ddc_url_list_2.html product="ucp" version="3.2" %}

{% comment %}
## Download the offline package
{% endcomment %}
{: #download-the-offline-package }
## オフラインパッケージのダウンロード

{% comment %}
You can also use these links to get the UCP package from the command
line:
{% endcomment %}
上のリンクによる UCP パッケージは、コマンドラインからダウンロードすることもできます。

```bash
$ wget <ucp-package-url> -O ucp.tar.gz
```

{% comment %}
Now that you have the package in your local machine, you can transfer it to
the machines where you want to install UCP.
{% endcomment %}
こうしてローカルマシン内にパッケージをダウンロードできたので、UCP をインストールする対象のマシンにそのパッケージをコピーします。

{% comment %}
For each machine that you want to manage with UCP:
{% endcomment %}
UCP による管理を行いたいマシンすべてに対して以下を行います。

{% comment %}
1.  Copy the UCP package to the machine.
{% endcomment %}
1.  UCP パッケージをマシンにコピーします。

    ```bash
    $ scp ucp.tar.gz <user>@<host>
    ```

{% comment %}
2.  Use ssh to log in to the hosts where you transferred the package.
{% endcomment %}
2.  パッケージをコピーしたホストに対して ssh によりログインします。

{% comment %}
3.  Load the UCP images.
{% endcomment %}
3.  UCP イメージをロードします。

    {% comment %}
    Once the package is transferred to the hosts, you can use the
    `docker load` command, to load the Docker images from the tar archive:
    {% endcomment %}
    パッケージをホストにコピーしていれば `docker load` コマンドが利用できます。
    このコマンドにより tar アーカイブから Docker イメージをロードすることができます。

    ```bash
    $ docker load -i ucp.tar.gz
    ```

{% comment %}
Follow the same steps for the DTR binaries.
{% endcomment %}
同じ手順を DTR バイナリに対して実行します。

{% comment %}
## Install UCP
{% endcomment %}
{: #install-ucp }
## UCP のインストール

{% comment %}
Now that the offline hosts have all the images needed to install UCP,
you can [install Docker UCP on one of the manager nodes](index.md).
{% endcomment %}
上の手順によりオフラインホストには、UCP のインストールに必要なイメージがすべて設定できたことになるので、[マネージャーノードから Docker UCP をインストールする](index.md) に進んでください。

{% comment %}
## Where to go next
{% endcomment %}
{: #where-to-go-next }
## 次に読むものは

{% comment %}
- [System requirements](system-requirements.md)
- [Install UCP](index.md)
{% endcomment %}
- [システム要件](system-requirements.md)
- [UCP のインストール](index.md)
