---
title: "はじめよう 1部: 概要とセットアップ"
keywords: get started, setup, orientation, quickstart, intro, concepts, containers, docker desktop
description: Get oriented on some basics of Docker and install Docker Desktop.
redirect_from:
- /getstarted/
- /get-started/part1/
- /get-started/part6/
- /engine/getstarted/
- /learn/
- /engine/getstarted/step_one/
- /engine/getstarted/step_two/
- /engine/getstarted/step_three/
- /engine/getstarted/step_four/
- /engine/getstarted/step_five/
- /engine/getstarted/step_six/
- /engine/getstarted/last_page/
- /engine/getstarted-voting-app/
- /engine/getstarted-voting-app/node-setup/
- /engine/getstarted-voting-app/create-swarm/
- /engine/getstarted-voting-app/deploy-app/
- /engine/getstarted-voting-app/test-drive/
- /engine/getstarted-voting-app/customize-app/
- /engine/getstarted-voting-app/cleanup/
- /engine/userguide/intro/
- /mac/started/
- /windows/started/
- /linux/started/
- /getting-started/
- /mac/step_one/
- /windows/step_one/
- /linux/step_one/
- /engine/tutorials/dockerizing/
- /mac/step_two/
- /windows/step_two/
- /linux/step_two/
- /mac/step_three/
- /windows/step_three/
- /linux/step_three/
- /engine/tutorials/usingdocker/
- /mac/step_four/
- /windows/step_four/
- /linux/step_four/
- /engine/tutorials/dockerimages/
- /userguide/dockerimages/
- /engine/userguide/dockerimages/
- /mac/last_page/
- /windows/last_page/
- /linux/last_page/
- /mac/step_six/
- /windows/step_six/
- /linux/step_six/
- /engine/tutorials/dockerrepos/
- /userguide/dockerrepos/
- /engine/userguide/containers/dockerimages/
---

{% include_relative nav.html selected="1" %}

{% comment %}
Welcome! We are excited that you want to learn Docker. The _Docker Get Started Tutorial_
teaches you how to:
{% endcomment %}
ようこそ！  皆さんが Docker の使い方を学ぼうとしており、私たちは嬉しく思います。
**Docker をはじめよう** では以下のことを学んでいきます。

{% comment %}
1. Set up your Docker environment (on this page)
2. [Build an image and run it as one container](part2.md)
3. [Set up and use a Kubernetes environment on your development machine](part3.md)
4. [Set up and use a Swarm environment on your development machine](part4.md)
5. [Share your containerized applications on Docker Hub](part5.md)
{% endcomment %}
1. Docker 環境のセットアップ（このページ）
2. [イメージの構築とコンテナーを一つ実行](part2.md)
3. [開発マシン上に Kubernetes 環境を構築して利用](part3.md)
4. [開発マシン上に Swarm 環境を構築して利用](part4.md)
5. [Docker Hub にてコンテナー化アプリを共有](part5.md)

{% comment %}
## Docker concepts
{% endcomment %}
{: #docker-concepts }
## Docker の考え方

{% comment %}
Docker is a platform for developers and sysadmins to **build, share, and run**
applications with containers. The use of containers to deploy applications
{% endcomment %}
Docker は開発者やシステム管理者が、コンテナーを使ってアプリケーションを**ビルド、共有、実行**するためのプラットフォームです。
コンテナーを利用してアプリケーションをデプロイすることを「コンテナー化」（_containerization_）と呼びます。
コンテナーは新たな技術というものではありませんが、これを使えばアプリケーションを簡単にデプロイできます。

{% comment %}
Containerization is increasingly popular because containers are:
{% endcomment %}
コンテナー化は、よりポピュラーなものになっています。
その理由は以下です。

{% comment %}
- Flexible: Even the most complex applications can be containerized.
- Lightweight: Containers leverage and share the host kernel,
  making them much more efficient in terms of system resources than virtual machines.
- Portable: You can build locally, deploy to the cloud, and run anywhere.
- Loosely coupled: Containers are highly self sufficient and encapsulated,
  allowing you to replace or upgrade one without disrupting others.
- Scalable: You can increase and automatically distribute container replicas across a datacenter.
- Secure: Containers apply aggressive constraints and isolations to processes without
  any configuration required on the part of the user.
{% endcomment %}
- 柔軟性: より複雑なアプリケーションであるほど、コンテナー化ができます。
- 軽量: コンテナーはホストシステムのカーネルを活用し共有します。
  システムリソースを活用する観点からは、仮想環境よりもさらに効率性が増します。
- 可搬性: ローカルでビルド、クラウドにデプロイ、どこでも動きます。
- Loosely coupled: Containers are highly self sufficient and encapsulated,
  allowing you to replace or upgrade one without disrupting others.
- スケーラブル: データセンター内において、コンテナーのレプリカを増やしたり自動的に提供することができます。
- セキュア: Containers apply aggressive constraints and isolations to processes without
  any configuration required on the part of the user.

{% comment %}
![Containers are portable](images/laurel-docker-containers2019.png){:width="100%"}
{% endcomment %}
![コンテナーは可搬性に優れる](images/laurel-docker-containers2019.png){:width="100%"}

{% comment %}
### Images and containers
{% endcomment %}
{: #images-and-containers }
### イメージとコンテナー

{% comment %}
Fundamentally, a container is nothing but a running process,
with some added encapsulation features applied to it in order to keep it isolated from the host
and from other containers.
One of the most important aspects of container isolation is that each container interacts
with its own, private filesystem; this filesystem is provided by a Docker **image**.
An image includes everything needed to run an application -- the code or binary,
runtimes, dependencies, and any other filesystem objects required.
{% endcomment %}
基本的にコンテナーとは、単にプロセスが起動しているものにすぎません。
ただしホストや別のコンテナーとは切り離すために、カプセル化された機能が加えられています。
コンテナーを独立させる際に重要となる点として、各コンテナーが独自のプライベートなファイルシステムにアクセスすることがあげられます。
このファイルシステムは Docker **イメージ** によって提供されるものです。
イメージには、アプリケーションの実行に必要となるすべて、つまりコード、バイナリモジュール、ランタイム、依存パッケージなど、必要となるファイルシステムオブジェクトが含まれます。

{% comment %}
### Containers and virtual machines
{% endcomment %}
{: #containers-and-virtual-machines }
### コンテナーと仮想マシン

{% comment %}
A container runs _natively_ on Linux and shares the kernel of the host
machine with other containers. It runs a discrete process, taking no more memory
than any other executable, making it lightweight.
{% endcomment %}
コンテナーは Linux 上においてネイティブに実行され、他のコンテナーも含めてホストマシン上のカーネルを共有します。
そのプロセスは分離されていて、通常の実行モジュールよりも少ないメモリで動作します。
つまり軽量化されています。

{% comment %}
By contrast, a **virtual machine** (VM) runs a full-blown "guest" operating
system with _virtual_ access to host resources through a hypervisor. In general,
VMs incur a lot of overhead beyond what is being consumed by your application logic.
{% endcomment %}
これとは対照的に**仮想マシン（virtual machine; VM）**は、本格的な"ゲスト"オペレーティングシステムを稼動させ、仮想的なアクセスはハイパーバイザーを通じてホストリソースを操作します。
VMs incur a lot of overhead beyond what is being consumed by your application logic.
一般に仮想マシンが提供する環境は、通常のアプリケーションに比べて多くのリソースを必要とします。

{% comment %}
![Container stack example](/images/Container%402x.png){:width="300px"} | ![Virtual machine stack example](/images/VM%402x.png){:width="300px"}
{% endcomment %}
![コンテナースタックの例](/images/Container%402x.png){:width="300px"} | ![仮想マシンスタックの例](/images/VM%402x.png){:width="300px"}

{% comment %}
## Install Docker Desktop
{% endcomment %}
{: #install-docker-desktop }
## Docker Desktop のインストール

{% comment %}
The best way to get started developing containerized applications is with Docker Desktop, for OSX or Windows. Docker Desktop will allow you to easily set up Kubernetes or Swarm on your local development machine, so you can use all the features of the orchestrator you're developing applications for right away, no cluster required. Follow the installation instructions appropriate for your operating system:
{% endcomment %}
コンテナー化したアプリケーションのデプロイを始めるには、OSX 向けあるいは Windows 向けの Docker Desktop を利用します。
Docker Desktop を使えば、ローカルの開発マシン上に Kubernetes や Swarm を容易に実現でき、開発するアプリケーションに対するオーケストレーターの全機能を即座に利用することができます。
そこにはクラスターは必要としません。
利用するオペレーティングシステムに応じて、以下のインストール手順に従ってください。

 {% comment %}
 - [OSX](/docker-for-mac/install/){: target="_blank" class="_"}
 - [Windows](/docker-for-windows/install/){: target="_blank" class="_"}
 {% endcomment %}
 - [OSX](/docker-for-mac/install/){: target="_blank" class="_"}
 - [Windows](/docker-for-windows/install/){: target="_blank" class="_"}

{% comment %}
## Enable Kubernetes
{% endcomment %}
{: #enable-kubernetes }
## Kubernetes 有効化

{% comment %}
Docker Desktop will set up Kubernetes for you quickly and easily. Follow the setup and validation instructions appropriate for your operating system:
{% endcomment %}
Docker Desktop では Kubernetes の設定を素早く簡単に行っていくことができます。
目的とするオペレーティングシステムに対応する手順により、設定と確認を行ってください。

<ul class="nav nav-tabs">
  <li class="active"><a data-toggle="tab" href="#kubeosx">OSX</a></li>
  <li><a data-toggle="tab" href="#kubewin">Windows</a></li>
</ul>
<div class="tab-content">
  <div id="kubeosx" class="tab-pane fade in active">
{% capture local-content %}

#### OSX

{% comment %}
1. After installing Docker Desktop, you should see a Docker icon in your menu bar. Click on it, and navigate **Preferences... -> Kubernetes**.
{% endcomment %}
1. Docker Desktop をインストールすると、メニューバーに Docker アイコンが表示されます。
   これをクリックして **Preferences... -> Kubernetes** を選びます。

{% comment %}
2. Check the checkbox labeled *Enable Kubernetes*, and click **Apply**. Docker Desktop will automatically set up Kubernetes for you. You'll know everything has completed successfully once you can click on the Docker icon in the menu bar, and see a green light beside 'Kubernetes is Running'.
{% endcomment %}
2. *Enable Kubernetes* と書かれたチェックボックスにチェックを入れて **Apply** をクリックします。
   Docker Desktop では Kubernetes に対する設定が自動的に行われます。
   メニューバー上の Docker アイコンをクリックしてみれば、すべての作業がうまくできていることがわかります。
   'Kubernetes is Running' という表記の横に緑色が点灯しているはずです。

{% comment %}
3. In order to confirm that Kubernetes is up and running, create a text file called `pod.yaml` with the following content:
{% endcomment %}
3. Kubernetes の設定と起動が正しく行われていることを確認するために、`pod.yaml` というテキストファイルを生成し、以下の内容とします。

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
    This describes a pod with a single container, isolating a simple ping to 8.8.8.8.
    {% endcomment %}
    このファイルは、単一のコンテナーからなる Pod を表わします。

This describes a pod with a single container, isolating a simple ping to 8.8.8.8.

{% comment %}
4. In a terminal, navigate to where you created `pod.yaml` and create your pod:
{% endcomment %}
4. 端末画面から `pod.yaml` を生成したディレクトリに移動し、この pod を生成します。

    ```shell
    kubectl apply -f pod.yaml
    ```

{% comment %}
5. Check that your pod is up and running:
{% endcomment %}
5. pod が起動していることを確認します。

    ```shell
    kubectl get pods
    ```

    {% comment %}
    You should see something like:
    {% endcomment %}
    以下のような表示が行われるはずです。

    ```shell
    NAME      READY     STATUS    RESTARTS   AGE
    demo      1/1       Running   0          4s
    ```

{% comment %}
6. Check that you get the logs you'd expect for a ping process:
{% endcomment %}
6. ログ出力内にて行われる ping 処理が正しく行われていることを確認します。

    ```shell
    kubectl logs demo
    ```

    {% comment %}
    You should see the output of a healthy ping process:
    {% endcomment %}
    ping 処理の出力が正しく行われていることがわかります。

    ```shell
    PING 8.8.8.8 (8.8.8.8): 56 data bytes
    64 bytes from 8.8.8.8: seq=0 ttl=37 time=21.393 ms
    64 bytes from 8.8.8.8: seq=1 ttl=37 time=15.320 ms
    64 bytes from 8.8.8.8: seq=2 ttl=37 time=11.111 ms
    ...
    ```

{% comment %}
7. Finally, tear down your test pod:
{% endcomment %}
7. 最後に、テストとして生成した pod を削除します。

    ```shell
    kubectl delete -f pod.yaml
    ```

{% endcapture %}
{{ local-content | markdownify }}

</div>
<div id="kubewin" class="tab-pane fade" markdown="1">
{% capture localwin-content %}

#### Windows

{% comment %}
1. After installing Docker Desktop, you should see a Docker icon in your system tray. Right-click on it, and navigate **Settings -> Kubernetes**.
{% endcomment %}
1. Docker Desktop をインストールすると、システムトレイに Docker アイコンが表示されます。
   このアイコン上で右クリックして、**Settings -> Kubernetes** を選びます。

{% comment %}
2. Check the checkbox labeled *Enable Kubernetes*, and click **Apply**. Docker Desktop will automatically set up Kubernetes for you. Note this can take a significant amount of time (20 minutes). You'll know everything has completed successfully once you can right-click on the Docker icon in the menu bar, click **Settings**, and see a green light beside 'Kubernetes is running'.
{% endcomment %}
2. **Enable Kubernetes** と書かれたチェックボックスにチェックを入れて **Apply** をクリックします。
   Docker Desktop では Kubernetes に対する設定が自動的に行われます。
   ただしこれにはかなりの時間（20 分）がかかります。
   システムトレイ上の Docker アイコンを右クリックして **Settings** をクリックしてみれば、すべての作業がうまくできていることがわかります。
   'Kubernetes is Running' という表記の横に緑色が点灯しているはずです。

{% comment %}
3. In order to confirm that Kubernetes is up and running, create a text file called `pod.yaml` with the following content:
{% endcomment %}
3. Kubernetes の設定と起動が正しく行われていることを確認するために、`pod.yaml` というテキストファイルを生成し、以下の内容とします。

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
    This describes a pod with a single container, isolating a simple ping to 8.8.8.8.
    {% endcomment %}
    This describes a pod with a single container, isolating a simple ping to 8.8.8.8.

{% comment %}
4. In powershell, navigate to where you created `pod.yaml` and create your pod:
{% endcomment %}
4. In powershell, navigate to where you created `pod.yaml` and create your pod:

    ```shell
    kubectl apply -f pod.yaml
    ```

{% comment %}
5. Check that your pod is up and running:
{% endcomment %}
5. Check that your pod is up and running:

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
6. Check that you get the logs you'd expect for a ping process:

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
7. Finally, tear down your test pod:

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
{% endcomment %}
Docker Desktop runs primarily on Docker Engine, which has everything you need to run a Swarm built in. Follow the setup and validation instructions appropriate for your operating system:

<ul class="nav nav-tabs">
  <li class="active"><a data-toggle="tab" href="#swarmosx">OSX</a></li>
  <li><a data-toggle="tab" href="#swarmwin">Windows</a></li>
</ul>
<div class="tab-content">
  <div id="swarmosx" class="tab-pane fade in active">
{% capture local-content %}

#### OSX

{% comment %}
{% endcomment %}
1. Open a terminal, and initialize Docker Swarm mode:

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
2. Run a simple Docker service that uses an alpine-based filesystem, and isolates a ping to 8.8.8.8:

    ```shell
    docker service create --name demo alpine:3.5 ping 8.8.8.8
    ```

{% comment %}
{% endcomment %}
3. Check that your service created one running container:

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
4. Check that you get the logs you'd expect for a ping process:

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
5. Finally, tear down your test service:

    ```shell
    docker service rm demo
    ```

{% endcapture %}
{{ local-content | markdownify }}

</div>
<div id="swarmwin" class="tab-pane fade" markdown="1">
{% capture localwin-content %}

#### Windows

{% comment %}
{% endcomment %}
1. Open a powershell, and initialize Docker Swarm mode:

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
2. Run a simple Docker service that uses an alpine-based filesystem, and isolates a ping to 8.8.8.8:

    ```shell
    docker service create --name demo alpine:3.5 ping 8.8.8.8
    ```

{% comment %}
{% endcomment %}
3. Check that your service created one running container:

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
4. Check that you get the logs you'd expect for a ping process:

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
5. Finally, tear down your test service:
{% endcomment %}
5. Finally, tear down your test service:

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
At this point, you've installed Docker Desktop on your development machine, and confirmed that you can run simple containerized workloads in Kuberentes and Swarm. In the next section, we'll start developing our first containerized application.
{% endcomment %}
At this point, you've installed Docker Desktop on your development machine, and confirmed that you can run simple containerized workloads in Kuberentes and Swarm. In the next section, we'll start developing our first containerized application.

{% comment %}
[On to Part 2 >>](part2.md){: class="button outline-btn" style="margin-bottom: 30px; margin-right: 100%"}
{% endcomment %}
[2 部へ >>](part2.md){: class="button outline-btn" style="margin-bottom: 30px; margin-right: 100%"}

{% comment %}
## CLI References
{% endcomment %}
{: #cli-references }
## CLI リファレンス

{% comment %}
Further documentation for all CLI commands used in this article are available here:
{% endcomment %}
Further documentation for all CLI commands used in this article are available here:

- [`kubectl apply`](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply)
- [`kubectl get`](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get)
- [`kubectl logs`](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#logs)
- [`kubectl delete`](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#delete)
- [`docker swarm init`](https://docs.docker.com/engine/reference/commandline/swarm_init/)
- [`docker service *`](https://docs.docker.com/engine/reference/commandline/service/)
