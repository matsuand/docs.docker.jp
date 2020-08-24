---
description: 生成済みマシン上でプロビジョンを再実行します。
keywords: machine, provision, subcommand
title: docker-machine provision
hide_from_sitemap: true
---

{% comment %}
Re-run provisioning on a created machine.
{% endcomment %}
生成済みマシン上でプロビジョンを再実行します。

{% comment %}
Sometimes it may be helpful to re-run Machine's provisioning process on a
created machine.  Reasons for doing so may include a failure during the original
provisioning process, or a drift from the desired system state (including the
originally specified Swarm or Engine configuration).
{% endcomment %}
生成済みマシン上において、プロビジョニング処理を再実行することが効果を発揮する場合があります。
その理由として、もともとのプロビジョニング処理に失敗していたり、期待しているシステムの状態（もともとの Swarm や Engine の設定）からずれてしまうことがあるからです。

{% comment %}
Usage is `docker-machine provision [name]`.  Multiple names may be specified.
{% endcomment %}
実行は `docker-machine provision [マシン名]` とします。
マシン名は複数指定することができます。

```bash
$ docker-machine provision foo bar

Copying certs to the local machine directory...
Copying certs to the remote machine...
Setting Docker configuration on the remote daemon...
```

{% comment %}
The Machine provisioning process will:
{% endcomment %}
マシンのプロビジョニング処理は以下を行います。

{% comment %}
1.  Set the hostname on the instance to the name Machine addresses it by, such
    as `default`.
2.  Install Docker if it is not present already.
3.  Generate a set of certificates (usually with the default, self-signed CA) and
    configure the daemon to accept connections over TLS.
4.  Copy the generated certificates to the server and local config directory.
5.  Configure the Docker Engine according to the options specified at create
    time.
6.  Configure and activate Swarm if applicable.
{% endcomment %}
1.  インスタンス上のホスト名を、Docker Machine が処理している名前に設定します。
    たとえば `default` などです。
2.  Docker が存在していない場合はインストールします。
3.  一連の証明書（通常はデフォルトの自己署名 CA）を生成します。
    そしてデーモンが TLS を介しての接続を可能とします。
4.  生成した証明書を、サーバーとローカルディレクトリにコピーします。
5.  マシン生成時に指定されたオプションに基づいて Docker Engine を設定します。
6.  Swarm である場合、その設定とアクティブ化を行います。
