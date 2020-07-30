---
description: コンテナーを自動起動する方法。
keywords: containers, restart, policies, automation, administration
redirect_from:
- /engine/articles/host_integration/
- /engine/admin/host_integration/
- /engine/admin/start-containers-automatically/
title: コンテナーの自動起動
---

{% comment %}
Docker provides [restart policies](../../engine/reference/run.md#restart-policies---restart)
to control whether your containers start automatically when they exit, or when
Docker restarts. Restart policies ensure that linked containers are started in
the correct order. Docker recommends that you use restart policies, and avoid
using process managers to start containers.
{% endcomment %}
Docker では [再起動ポリシー](../../engine/reference/run.md#restart-policies---restart) (restart policy) が提供されています。
これは Docker の停止や再起動を指示した場合に、コンテナーを自動的に起動するかどうかを制御するものです。
再起動ポリシーを使えば、リンクしているコンテナーは確実に正しい順番で起動させることができます。
Docker ではこの再起動ポリシーを利用することが推奨されています。
プロセスマネージャーを利用してコンテナーを起動することはやめてください。

{% comment %}
Restart policies are different from the `--live-restore` flag of the `dockerd`
command. Using `--live-restore` allows you to keep your containers running
during a Docker upgrade, though networking and user input are interrupted.
{% endcomment %}
再起動ポリシーは `dockerd` コマンドの `--live-restore` フラグとは異なります。
`--live-restore` は Docker のアップグレード中にコンテナーを起動し続けるものですが、ネットワーク接続やユーザー入力は遮断されます。

{% comment %}
## Use a restart policy
{% endcomment %}
{: #use-a-restart-policy }
## 再起動ポリシーの利用

{% comment %}
To configure the restart policy for a container, use the `--restart` flag
when using the `docker run` command. The value of the `--restart` flag can be
any of the following:
{% endcomment %}
コンテナーに対して再起動ポリシーを設定するには、`docker run` コマンド実行時の `--restart` フラグを用います。
`--restart` フラグには以下に示す値を指定することができます。

{% comment %}
| Flag             | Description                                                                                     |
|:-----------------|:------------------------------------------------------------------------------------------------|
| `no`             | Do not automatically restart the container. (the default)                                       |
| `on-failure`     | Restart the container if it exits due to an error, which manifests as a non-zero exit code.     |
| `always`         | Always restart the container if it stops. If it is manually stopped, it is restarted only when Docker daemon restarts or the container itself is manually restarted. (See the second bullet listed in [restart policy details](#restart-policy-details)) |
| `unless-stopped` | Similar to `always`, except that when the container is stopped (manually or otherwise), it is not restarted even after Docker daemon restarts. |
{% endcomment %}
| フラグ           | 内容説明                                                                                        |
|:-----------------|:------------------------------------------------------------------------------------------------|
| `no`             | コンテナーを自動では再起動しません。（デフォルト）                                              |
| `on-failure`     | エラー発生により停止したコンテナーを再起動します。非ゼロの終了コードが返ります。                |
| `always`         | コンテナー停止時に常に再起動します。手動で停止させた場合は、Docker デーモン再起動時、あるいはコンテナーそのものが手動で再起動された場合のみ再起動します。 ([再起動ポリシーの詳細](#restart-policy-details) における 2 項めを参照してください。) |
| `unless-stopped` | `always` と同様。ただしコンテナーが（手動またはその他によって）停止した場合は除きます。Docker デーモンが再起動した場合には再起動しません。|

{% comment %}
The following example starts a Redis container and configures it to always
restart unless it is explicitly stopped or Docker is restarted.
{% endcomment %}
以下の例は Redis コンテナーを起動する際に、常に (always) 再起動するように設定していますが、ただし明示的に停止した場合や Docker が再起動された場合には再起動しないという設定です。

```bash
$ docker run -d --restart unless-stopped redis
```

{% comment %}
This command changes the restart policy for an already running container named `redis`.
{% endcomment %}
以下のコマンドは、`redis` というすでに起動されているコンテナーの再起動ポリシーを変更します。

```bash
$ docker update --restart unless-stopped redis
```

{% comment %}
And this command will ensure all currently running containers will be restarted unless stopped.
{% endcomment %}
また以下のコマンドは、現在あるコンテナーの中で、停止されていてないものはすべて再起動します。

```bash
$ docker update --restart unless-stopped $(docker ps -q)
```

{% comment %}
### Restart policy details
{% endcomment %}
{: #restart-policy-details }
### 再起動ポリシーの詳細

{% comment %}
Keep the following in mind when using restart policies:
{% endcomment %}
再起動ポリシーの利用にあたっては以下の点に注意してください。

{% comment %}
- A restart policy only takes effect after a container starts successfully. In
  this case, starting successfully means that the container is up for at least
  10 seconds and Docker has started monitoring it. This prevents a container
  which does not start at all from going into a restart loop.
{% endcomment %}
- 再起動ポリシーが処理適用されるのは、コンテナーが正常に起動した後です。
  この正常に起動というのは、コンテナーの起動から 10 秒以上が経過し Docker がこれを監視し始めたときです。
  このようになっているのは、コンテナーが全く起動していないまま再起動ループに陥ることがないようにするためです。

{% comment %}
- If you manually stop a container, its restart policy is ignored until the
  Docker daemon restarts or the container is manually restarted. This is another
  attempt to prevent a restart loop.
{% endcomment %}
- コンテナーを手動で停止した場合は、Docker デーモンの再起動、あるいはコンテナーの手動再起動を行わない限り、再起動ポリシーは無視されます。
  これも再起動ループを避けるための動作です。

{% comment %}
- Restart policies only apply to _containers_. Restart policies for swarm
  services are configured differently. See the
  [flags related to service restart](../../engine/reference/commandline/service_create.md).
{% endcomment %}
- 再起動ポリシーが適用されるのは **コンテナー** に対してのみです。
  Swarm サービスに対する再起動ポリシーは、別の方法により設定します。
  [service restart に対するフラグ](../../engine/reference/commandline/service_create.md) を参照してください。


{% comment %}
## Use a process manager
{% endcomment %}
{: #use-a-process-manager }
## プロセスマネージャーの利用

{% comment %}
If restart policies don't suit your needs, such as when processes outside
Docker depend on Docker containers, you can use a process manager such as
[upstart](http://upstart.ubuntu.com/),
[systemd](http://freedesktop.org/wiki/Software/systemd/), or
[supervisor](http://supervisord.org/) instead.
{% endcomment %}
Docker 外部にあるプロセスが Docker コンテナーに依存するような場合、再起動ポリシーの利用は適当ではありません。
この場合は [upstart](http://upstart.ubuntu.com/)、[systemd](http://freedesktop.org/wiki/Software/systemd/)、[supervisor](http://supervisord.org/) といったプロセスマネージャーを利用します。

{% comment %}
> **Warning**
>
> Do not try to combine Docker restart policies with host-level process managers,
> because this creates conflicts.
{:.warning}
{% endcomment %}
> **警告**
>
> Docker の再起動ポリシーとホストのプロセスマネージャーを組み合わせた利用は避けてください。
> これを行うと衝突が発生します。
{:.warning}

{% comment %}
To use a process manager, configure it to start your container or service using
the same `docker start` or `docker service` command you would normally use to
start the container manually. Consult the documentation for the specific
process manager for more details.
{% endcomment %}
プロセスマネージャーを利用する場合は、普段コンテナー起動を手動で行っているときと同じコマンド
`docker start` または `docker service` を使って、コンテナーを起動するように設定してください。
詳しくは個々のプロセスマネージャーのドキュメントを参照してください。

{% comment %}
### Using a process manager inside containers
{% endcomment %}
{: #using-a-process-manager-inside-containers }
### コンテナー内部のプロセスマネージャーの利用

{% comment %}
Process managers can also run within the container to check whether a process is
running and starts/restart it if not.
{% endcomment %}
プロセスマネージャーはコンテナー内部において起動させることができます。
このプロセスマネージャーから、プロセスの実行状態を確認したり、起動、再起動を行うこともできます。

{% comment %}
> **Warning**
>
> These are not Docker-aware and just monitor operating system processes within
> the container. Docker does not recommend this approach, because it is
> platform-dependent and even differs within different versions of a given Linux
> distribution.
{:.warning}
{% endcomment %}
> **警告**
>
> この場合は Docker が察知できる状況ではなくなり、コンテナー内のオペレーティングシステムのプロセスをただ監視するだけになります。
> Docker ではこの方法を推奨しません。
> これはプラットフォームに依存する話であり、利用する Linux ディストリビューションの各バージョンによって異なるからです。
{:.warning}
