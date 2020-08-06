---
title: ホストネットワークのチュートリアル
description: Tutorials for networking using the host network, disabling network isolation
keywords: networking, host, standalone
---

{% comment %}
This series of tutorials deals with networking standalone containers which bind
directly to the Docker host's network, with no network isolation. For other
networking topics, see the [overview](index.md).
{% endcomment %}
このチュートリアルでは、スタンドアロンコンテナーのネットワークを取り扱います。
Docker ホストのネットワークに直接割り当てられ、ネットワークの分離は行われません。
これ以外のネットワークに関することは [ネットワーク概要](index.md) を参照してください。

{% comment %}
## Goal
{% endcomment %}
{: #goal }
## 目指すこと

{% comment %}
The goal of this tutorial is to start a `nginx` container which binds directly
to port 80 on the Docker host. From a networking point of view, this is the
same level of isolation as if the `nginx` process were running directly on the
Docker host and not in a container. However, in all other ways, such as storage,
process namespace, and user namespace, the `nginx` process is isolated from the
host.
{% endcomment %}
本チュートリアルの目標は、`nginx` コンテナーを起動させ、Docker ホスト上のポート 80 に直接割り当てることです。
ネットワークの観点からは `nginx` のプロセスが、コンテナー内でなく Docker ホスト上で直接稼動している場合と同じレベルの分離が実現されていると言えます。
ただしこれ以外の点、つまりストレージ、プロセス名前空間、ユーザー名前空間などから見ると、`nginx` のプロセスはホストから分離されています。

{% comment %}
## Prerequisites
{% endcomment %}
{: #prerequisites }
## 前提条件

{% comment %}
- This procedure requires port 80 to be available on the Docker host. To make
  Nginx listen on a different port, see the
  [documentation for the `nginx` image](https://hub.docker.com/_/nginx/)
{% endcomment %}
- 以下の手順においては、Docker ホスト上のポート 80 が利用できることが必要です。
  Nginx が別のポートを利用できるようにするには [`nginx` イメージのドキュメント](https://hub.docker.com/_/nginx/) を参照してください。

{% comment %}
- The `host` networking driver only works on Linux hosts, and is not supported
  on Docker Desktop for Mac, Docker Desktop for Windows, or Docker EE for Windows Server.
{% endcomment %}
- `host` ネットワークドライバーは Linux ホスト上においてのみ動作します。
  一方、Docker Desktop for Mac、Docker Desktop for Windows、Docker EE for Windows Server ではサポートされていません。

{% comment %}
## Procedure
{% endcomment %}
{: #procedure }
## 手順

{% comment %}
1.  Create and start the container as a detached process. The `--rm` option means to remove the container once it exits/stops. The `-d` flag means to start the container detached (in the background).
{% endcomment %}
1.  コンテナーを生成し、デタッチモードで起動します。
    `--rm` オプションは、コンテナーが終了または停止したときにコンテナーを削除するものです。
    `-d` フラグは、コンテナーをデタッチモード（バックグラウンド）で起動します。

    ```bash
    docker run --rm -d --network host --name my_nginx nginx
    ```

{% comment %}
2.  Access Nginx by browsing to
    [http://localhost:80/](http://localhost:80/).
{% endcomment %}
2.  ブラウザーから [http://localhost:80/](http://localhost:80/) を入力して Nginx にアクセスします。

{% comment %}
3.  Examine your network stack using the following commands:
{% endcomment %}
3.  以下のコマンドを実行して、ネットワークの状況を確認します。

    {% comment %}
    - Examine all network interfaces and verify that a new one was not created.
    {% endcomment %}
    - ネットワークインターフェースをすべて確認します。
      そして新しいインターフェースは生成されていないことを確認します。

      ```bash
      ip addr show
      ```

    {% comment %}
    - Verify which process is bound to port 80, using the `netstat` command. You
      need to use `sudo` because the process is owned by the Docker daemon user
      and you otherwise won't be able to see its name or PID.
    {% endcomment %}
    - `netstat` コマンドを使って、どのプロセスがポート 80 に割り当てられているかを確認します。
      実行にあたっては `sudo` が必要です。
      プロセスが Docker デーモンユーザーの所有になっているからであり、これを用いないとプロセス名やプロセス ID を確認することができません。

      ```bash
      sudo netstat -tulpn | grep :80
      ```

{% comment %}
4.  Stop the container. It will be removed automatically as it was started using the `--rm` option.
{% endcomment %}
4.  コンテナーを停止します。
    `--rm` オプションを指定して起動しているので、コンテナーは自動的に削除されます。

    ```basn
    docker container stop my_nginx
    ```

{% comment %}
## Other networking tutorials
{% endcomment %}
{: #other-networking-tutorials }
## その他のネットワークチュートリアル

{% comment %}
Now that you have completed the networking tutorials for standalone containers,
you might want to run through these other networking tutorials:
{% endcomment %}
ホストネットワークのチュートリアルを終えたので、以下に示すような別のネットワークチュートリアルも見てください。

{% comment %}
- [Standalone networking tutorial](network-tutorial-standalone.md)
- [Overlay networking tutorial](network-tutorial-overlay.md)
- [Macvlan networking tutorial](network-tutorial-macvlan.md)
{% endcomment %}
- [スタンドアロンネットワークのチュートリアル](network-tutorial-standalone.md)
- [オーバーレイネットワークのチュートリアル](network-tutorial-overlay.md)
- [Macvlan ネットワークのチュートリアル](network-tutorial-macvlan.md)

