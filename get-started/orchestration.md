---
title: "オーケストレーション"
keywords: orchestration, deploy, kubernetes, swarm,
description: Get oriented on some basics of Docker and install Docker Desktop.
---

{% comment %}
The portability and reproducibility of a containerized process mean we have an opportunity to move and scale our containerized applications across clouds and datacenters. Containers effectively guarantee that those applications run the same way anywhere, allowing us to quickly and easily take advantage of all these environments. Furthermore, as we scale our applications up, we'll want some tooling to help automate the maintenance of those applications, able to replace failed containers automatically, and manage the rollout of updates and reconfigurations of those containers during their lifecycle.
{% endcomment %}
The portability and reproducibility of a containerized process mean we have an opportunity to move and scale our containerized applications across clouds and datacenters. Containers effectively guarantee that those applications run the same way anywhere, allowing us to quickly and easily take advantage of all these environments. Furthermore, as we scale our applications up, we'll want some tooling to help automate the maintenance of those applications, able to replace failed containers automatically, and manage the rollout of updates and reconfigurations of those containers during their lifecycle.

{% comment %}
Tools to manage, scale, and maintain containerized applications are called _orchestrators_, and the most common examples of these are _Kubernetes_ and _Docker Swarm_. Development environment deployments of both of these
orchestrators are provided by Docker Desktop, which we'll use throughout
this guide to create our first orchestrated, containerized application.
{% endcomment %}
Tools to manage, scale, and maintain containerized applications are called _orchestrators_, and the most common examples of these are _Kubernetes_ and _Docker Swarm_. Development environment deployments of both of these
orchestrators are provided by Docker Desktop, which we'll use throughout
this guide to create our first orchestrated, containerized application.

{% comment %}
The advanced modules teach you how to:
{% endcomment %}
The advanced modules teach you how to:

{% comment %}
1. [Set up and use a Kubernetes environment on your development machine](kube-deploy.md)
2. [Set up and use a Swarm environment on your development machine](swarm-deploy.md)
{% endcomment %}
1. [Set up and use a Kubernetes environment on your development machine](kube-deploy.md)
2. [Set up and use a Swarm environment on your development machine](swarm-deploy.md)

{% comment %}
## Enable Kubernetes
{% endcomment %}
{: #enable-kubernetes }
## Kubernetes の有効化

{% comment %}
Docker Desktop will set up Kubernetes for you quickly and easily. Follow the setup and validation instructions appropriate for your operating system:
{% endcomment %}
Docker Desktop では Kubernetes をすばやく簡単に設定することができます。
利用しているオペレーティングシステムに合わせて、以下の設定および確認手順を進めてください。

<ul class="nav nav-tabs">
  <li class="active"><a data-toggle="tab" href="#kubeosx">Mac</a></li>
  <li><a data-toggle="tab" href="#kubewin">Windows</a></li>
</ul>
<div class="tab-content">
  <div id="kubeosx" class="tab-pane fade in active">
{% capture local-content %}

### Mac

{% comment %}
1.  After installing Docker Desktop, you should see a Docker icon in your menu bar. Click on it, and navigate to **Preferences** > **Kubernetes**.
{% endcomment %}
1.  After installing Docker Desktop, you should see a Docker icon in your menu bar. Click on it, and navigate to **Preferences** > **Kubernetes**.

{% comment %}
{% endcomment %}
2.  Check the checkbox labeled **Enable Kubernetes**, and click **Apply & Restart**. Docker Desktop will automatically set up Kubernetes for you. You'll know that Kubernetes has been successfully enabled when you see a green light beside 'Kubernetes _running_' in the Preferences menu.

{% comment %}
{% endcomment %}
3.  In order to confirm that Kubernetes is up and running, create a text file called `pod.yaml` with the following content:

    ```yaml
    apiVersion: v1
    kind: Pod
    metadata:
      name: demo
    spec:
      containers:
      - name: testpod
        image: alpine:3.5
        command: ["ping", "8.8.8.8"]
    ```

    {% comment %}
    {% endcomment %}
    This describes a pod with a single container, isolating a simple ping to 8.8.8.8.

{% comment %}
{% endcomment %}
4.  In a terminal, navigate to where you created `pod.yaml` and create your pod:

    ```shell
    kubectl apply -f pod.yaml
    ```

{% comment %}
{% endcomment %}
5.  Check that your pod is up and running:

    ```shell
    kubectl get pods
    ```

    {% comment %}
    {% endcomment %}
    You should see something like:

    ```shell
    NAME      READY     STATUS    RESTARTS   AGE
    demo      1/1       Running   0          4s
    ```

{% comment %}
{% endcomment %}
6.  Check that you get the logs you'd expect for a ping process:

    ```shell
    kubectl logs demo
    ```

    {% comment %}
    {% endcomment %}
    You should see the output of a healthy ping process:

    ```shell
    PING 8.8.8.8 (8.8.8.8): 56 data bytes
    64 bytes from 8.8.8.8: seq=0 ttl=37 time=21.393 ms
    64 bytes from 8.8.8.8: seq=1 ttl=37 time=15.320 ms
    64 bytes from 8.8.8.8: seq=2 ttl=37 time=11.111 ms
    ...
    ```

{% comment %}
{% endcomment %}
7.  Finally, tear down your test pod:

    ```shell
    kubectl delete -f pod.yaml
    ```

{% endcapture %}
{{ local-content | markdownify }}

</div>
<div id="kubewin" class="tab-pane fade" markdown="1">
{% capture localwin-content %}

### Windows

{% comment %}
{% endcomment %}
1.  After installing Docker Desktop, you should see a Docker icon in your system tray. Right-click on it, and navigate **Settings** > **Kubernetes**.

{% comment %}
{% endcomment %}
2.  Check the checkbox labeled **Enable Kubernetes**, and click **Apply & Restart**. Docker Desktop will automatically set up Kubernetes for you. You'll know that Kubernetes has been successfully enabled when you see a green light beside 'Kubernetes _running_' in the **Settings** menu.

{% comment %}
{% endcomment %}
3.  In order to confirm that Kubernetes is up and running, create a text file called `pod.yaml` with the following content:

    ```yaml
    apiVersion: v1
    kind: Pod
    metadata:
      name: demo
    spec:
      containers:
      - name: testpod
        image: alpine:3.5
        command: ["ping", "8.8.8.8"]
    ```

    {% comment %}
    {% endcomment %}
    This describes a pod with a single container, isolating a simple ping to 8.8.8.8.

{% comment %}
{% endcomment %}
4.  In PowerShell, navigate to where you created `pod.yaml` and create your pod:

    ```shell
    kubectl apply -f pod.yaml
    ```

{% comment %}
{% endcomment %}
5.  Check that your pod is up and running:

    ```shell
    kubectl get pods
    ```

    {% comment %}
    {% endcomment %}
    You should see something like:

    ```shell
    NAME      READY     STATUS    RESTARTS   AGE
    demo      1/1       Running   0          4s
    ```

{% comment %}
{% endcomment %}
6.  Check that you get the logs you'd expect for a ping process:

    ```shell
    kubectl logs demo
    ```

    {% comment %}
    {% endcomment %}
    You should see the output of a healthy ping process:

    ```shell
    PING 8.8.8.8 (8.8.8.8): 56 data bytes
    64 bytes from 8.8.8.8: seq=0 ttl=37 time=21.393 ms
    64 bytes from 8.8.8.8: seq=1 ttl=37 time=15.320 ms
    64 bytes from 8.8.8.8: seq=2 ttl=37 time=11.111 ms
    ...
    ```

{% comment %}
{% endcomment %}
7.  Finally, tear down your test pod:

    ```shell
    kubectl delete -f pod.yaml
    ```

{% endcapture %}
{{ localwin-content | markdownify }}
</div>
<hr>
</div>

{% comment %}
## Enable Docker Swarm
{% endcomment %}
{: #enable-docker-swarm }
## Docker Swarm の有効化

{% comment %}
Docker Desktop runs primarily on Docker Engine, which has everything you need to run a Swarm built in. Follow the setup and validation instructions appropriate for your operating system:
{% endcomment %}
Docker Desktop runs primarily on Docker Engine, which has everything you need to run a Swarm built in. Follow the setup and validation instructions appropriate for your operating system:

<ul class="nav nav-tabs">
  <li class="active"><a data-toggle="tab" href="#swarmosx">Mac</a></li>
  <li><a data-toggle="tab" href="#swarmwin">Windows</a></li>
</ul>
<div class="tab-content">
  <div id="swarmosx" class="tab-pane fade in active">
{% capture local-content %}

### Mac

{% comment %}
1.  Open a terminal, and initialize Docker Swarm mode:
{% endcomment %}
1.  端末を開いて Docker Swarm モードを初期化します。

    ```shell
    docker swarm init
    ```

    {% comment %}
    If all goes well, you should see a message similar to the following:
    {% endcomment %}
    正常に処理されると、以下のようなメッセージが出力されます。

    ```shell
    Swarm initialized: current node (tjjggogqpnpj2phbfbz8jd5oq) is now a manager.

    To add a worker to this swarm, run the following command:

        docker swarm join --token SWMTKN-1-3e0hh0jd5t4yjg209f4g5qpowbsczfahv2dea9a1ay2l8787cf-2h4ly330d0j917ocvzw30j5x9 192.168.65.3:2377

    To add a manager to this swarm, run 'docker swarm join-token manager' and follow the instructions.
    ```

{% comment %}
2.  Run a simple Docker service that uses an alpine-based filesystem, and isolates a ping to 8.8.8.8:
{% endcomment %}
2.  単純な Docker サービスを実行します。
    これは alpine ベースのファイルシステムを利用するものであり、8.8.8.8 への ping プロセスを分離します。

    ```shell
    docker service create --name demo alpine:3.5 ping 8.8.8.8
    ```

{% comment %}
3.  Check that your service created one running container:
{% endcomment %}
3.  サービスから 1 つのコンテナーが生成されていることを確認します。

    ```shell
    docker service ps demo
    ```

    {% comment %}
    You should see something like:
    {% endcomment %}
    以下のような出力が得られます。

    ```shell
    ID                  NAME                IMAGE               NODE                DESIRED STATE       CURRENT STATE           ERROR               PORTS
    463j2s3y4b5o        demo.1              alpine:3.5          docker-desktop      Running             Running 8 seconds ago
    ```

{% comment %}
4.  Check that you get the logs you'd expect for a ping process:
{% endcomment %}
4.  ログを参照して、期待どおりに ping プロセスが得られていることを確認します。

    ```shell
    docker service logs demo
    ```

    {% comment %}
    You should see the output of a healthy ping process:
    {% endcomment %}
    ping 処理が正常に処理されて以下のように出力されているはずです。

    ```shell
    demo.1.463j2s3y4b5o@docker-desktop    | PING 8.8.8.8 (8.8.8.8): 56 data bytes
    demo.1.463j2s3y4b5o@docker-desktop    | 64 bytes from 8.8.8.8: seq=0 ttl=37 time=13.005 ms
    demo.1.463j2s3y4b5o@docker-desktop    | 64 bytes from 8.8.8.8: seq=1 ttl=37 time=13.847 ms
    demo.1.463j2s3y4b5o@docker-desktop    | 64 bytes from 8.8.8.8: seq=2 ttl=37 time=41.296 ms
    ...
    ```

{% comment %}
5.  Finally, tear down your test service:
{% endcomment %}
5.  最後はテストしたサービスを終了させます。

    ```shell
    docker service rm demo
    ```

{% endcapture %}
{{ local-content | markdownify }}

</div>
<div id="swarmwin" class="tab-pane fade" markdown="1">
{% capture localwin-content %}

### Windows

{% comment %}
1.  Open a powershell, and initialize Docker Swarm mode:
{% endcomment %}
1.  Open a powershell, and initialize Docker Swarm mode:

    ```shell
    docker swarm init
    ```

    {% comment %}
    {% endcomment %}
    If all goes well, you should see a message similar to the following:

    ```shell
    Swarm initialized: current node (tjjggogqpnpj2phbfbz8jd5oq) is now a manager.

    To add a worker to this swarm, run the following command:

        docker swarm join --token SWMTKN-1-3e0hh0jd5t4yjg209f4g5qpowbsczfahv2dea9a1ay2l8787cf-2h4ly330d0j917ocvzw30j5x9 192.168.65.3:2377

    To add a manager to this swarm, run 'docker swarm join-token manager' and follow the instructions.
    ```

{% comment %}
{% endcomment %}
2.  Run a simple Docker service that uses an alpine-based filesystem, and isolates a ping to 8.8.8.8:

    ```shell
    docker service create --name demo alpine:3.5 ping 8.8.8.8
    ```

{% comment %}
{% endcomment %}
3.  Check that your service created one running container:

    ```shell
    docker service ps demo
    ```

    {% comment %}
    {% endcomment %}
    You should see something like:

    ```shell
    ID                  NAME                IMAGE               NODE                DESIRED STATE       CURRENT STATE           ERROR               PORTS
    463j2s3y4b5o        demo.1              alpine:3.5          docker-desktop      Running             Running 8 seconds ago
    ```

{% comment %}
{% endcomment %}
4.  Check that you get the logs you'd expect for a ping process:

    ```shell
    docker service logs demo
    ```

    {% comment %}
    {% endcomment %}
    You should see the output of a healthy ping process:

    ```shell
    demo.1.463j2s3y4b5o@docker-desktop    | PING 8.8.8.8 (8.8.8.8): 56 data bytes
    demo.1.463j2s3y4b5o@docker-desktop    | 64 bytes from 8.8.8.8: seq=0 ttl=37 time=13.005 ms
    demo.1.463j2s3y4b5o@docker-desktop    | 64 bytes from 8.8.8.8: seq=1 ttl=37 time=13.847 ms
    demo.1.463j2s3y4b5o@docker-desktop    | 64 bytes from 8.8.8.8: seq=2 ttl=37 time=41.296 ms
    ...
    ```

{% comment %}
{% endcomment %}
5.  Finally, tear down your test service:

    ```shell
    docker service rm demo
    ```

{% endcapture %}
{{ localwin-content | markdownify }}
</div>
<hr>
</div>

{% comment %}
## Conclusion
{% endcomment %}
{: #conclusion }
## まとめ

{% comment %}
{% endcomment %}
At this point, you've confirmed that you can run simple containerized workloads in Kubernetes and Swarm. The next step will be to write the Kubernetes yaml that describes how to run and manage these containers on Kubernetes.

{% comment %}
{% endcomment %}
[On to deploying to Kubernetes >>](kube-deploy.md){: class="button outline-btn" style="margin-bottom: 30px; margin-right: 200%"}

{% comment %}
{% endcomment %}
To learn how to write the stack file to help you run and manage containers on Swarm, see [Deploying to Swarm](swarm-deploy.md).

{% comment %}
{% endcomment %}
## CLI references

{% comment %}
{% endcomment %}
Further documentation for all CLI commands used in this article are available here:

{% comment %}
{% endcomment %}
- [`kubectl apply`](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply)
- [`kubectl get`](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get)
- [`kubectl logs`](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#logs)
- [`kubectl delete`](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#delete)
- [`docker swarm init`](https://docs.docker.com/engine/reference/commandline/swarm_init/)
- [`docker service *`](https://docs.docker.com/engine/reference/commandline/service/)
