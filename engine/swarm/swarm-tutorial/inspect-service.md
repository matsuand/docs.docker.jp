---
description: アプリケーションを確認します。
keywords: tutorial, cluster management, swarm mode
title: Swarm 上のサービスの確認
notoc: true
---

{% comment %}
When you have [deployed a service](deploy-service.md) to your swarm, you can use
the Docker CLI to see details about the service running in the swarm.
{% endcomment %}
Swarm に対して [サービスのデプロイ](deploy-service.md) を行ったので、Docker CLI を利用して Swarm 内で起動しているサービスの詳細を確認していきます。

{% comment %}
1.  If you haven't already, open a terminal and ssh into the machine where you
    run your manager node. For example, the tutorial uses a machine named
    `manager1`.
{% endcomment %}
1.  マシンへの接続ができていなければ、端末画面を開いて SSH により接続します。
    接続先はマネージャーノードを起動したマシンです。
    たとえばこのチュートリアルでは `manager1` というマシンを利用します。

{% comment %}
2.  Run `docker service inspect --pretty <SERVICE-ID>` to display the details
    about a service in an easily readable format.
{% endcomment %}
2.  `docker service inspect --pretty <SERVICE-ID>` を実行します。
    サービスの詳細が非常にわかりやすく表示されます。

    {% comment %}
    To see the details on the `helloworld` service:
    {% endcomment %}
    `helloworld` サービスの詳細を見るには、以下のようにします。

    ```bash
    [manager1]$ docker service inspect --pretty helloworld

    ID:		9uk4639qpg7npwf3fn2aasksr
    Name:		helloworld
    Service Mode:	REPLICATED
     Replicas:		1
    Placement:
    UpdateConfig:
     Parallelism:	1
    ContainerSpec:
     Image:		alpine
     Args:	ping docker.com
    Resources:
    Endpoint Mode:  vip
    ```

    {% comment %}
    >**Tip**: To return the service details in json format, run the same command
    without the `--pretty` flag.
    {% endcomment %}
    >**ヒント**: サービスの詳細表示を JSON 形式に戻すには、`--pretty` フラグを除いて同じコマンドを実行します。

    ```bash
    [manager1]$ docker service inspect helloworld
    [
    {
        "ID": "9uk4639qpg7npwf3fn2aasksr",
        "Version": {
            "Index": 418
        },
        "CreatedAt": "2016-06-16T21:57:11.622222327Z",
        "UpdatedAt": "2016-06-16T21:57:11.622222327Z",
        "Spec": {
            "Name": "helloworld",
            "TaskTemplate": {
                "ContainerSpec": {
                    "Image": "alpine",
                    "Args": [
                        "ping",
                        "docker.com"
                    ]
                },
                "Resources": {
                    "Limits": {},
                    "Reservations": {}
                },
                "RestartPolicy": {
                    "Condition": "any",
                    "MaxAttempts": 0
                },
                "Placement": {}
            },
            "Mode": {
                "Replicated": {
                    "Replicas": 1
                }
            },
            "UpdateConfig": {
                "Parallelism": 1
            },
            "EndpointSpec": {
                "Mode": "vip"
            }
        },
        "Endpoint": {
            "Spec": {}
        }
    }
    ]
    ```

{% comment %}
4.  Run `docker service ps <SERVICE-ID>` to see which nodes are running the
    service:
{% endcomment %}
4.  `docker service ps <SERVICE-ID>` を実行します。
    サービスがどのノード上で動いているかがわかります。

    ```bash
    [manager1]$ docker service ps helloworld

    NAME                                    IMAGE   NODE     DESIRED STATE  CURRENT STATE           ERROR               PORTS
    helloworld.1.8p1vev3fq5zm0mi8g0as41w35  alpine  worker2  Running        Running 3 minutes
    ```

    {% comment %}
    In this case, the one instance of the `helloworld` service is running on the
    `worker2` node. You may see the service running on your manager node. By
    default, manager nodes in a swarm can execute tasks just like worker nodes.
    {% endcomment %}
    この例では `helloworld` サービスの 1 インスタンスが `worker2` ノード上で動いているのがわかります。
    マネージャーノード上においても、サービスが動いているかもしれません。
    デフォルトにおいて Swarm のマネージャーノードは、ワーカーノードと同じようにタスクを実行することができます。

    {% comment %}
    Swarm also shows you the `DESIRED STATE` and `CURRENT STATE` of the service
    task so you can see if tasks are running according to the service
    definition.
    {% endcomment %}
    Swarm においては、サービスタスクに対して `DESIRED STATE`（期待される状態）と `CURRENT STATE`（現在の状態）が示されます。
    サービス定義に従ってタスクが実行されているかどうかが、これによってわかります。

{% comment %}
4.  Run `docker ps` on the node where the task is running to see details about
    the container for the task.
{% endcomment %}
4.  タスクが動いているノード上において `docker ps` を実行します。
    これにより、そのタスクに応じたコンテナーの詳細を確認することができます。

    {% comment %}
    >**Tip**: If `helloworld` is running on a node other than your manager node,
    you must ssh to that node.
    {% endcomment %}
    >**ヒント**: `helloworld` がマネージャーノードではない、別のノード上で動いている場合は、そのノードに SSH でアクセスしておくことが必要です。

    ```bash
    [worker2]$ docker ps

    CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
    e609dde94e47        alpine:latest       "ping docker.com"   3 minutes ago       Up 3 minutes                            helloworld.1.8p1vev3fq5zm0mi8g0as41w35
    ```

{% comment %}
## What's next?
{% endcomment %}
{: #whats-next }
## 次にすることは

{% comment %}
Next, you can [change the scale](scale-service.md) for the service running in
the swarm.
{% endcomment %}
次は、Swarm 内で起動しているサービスに対して [スケールの変更](scale-service.md) を行います。
