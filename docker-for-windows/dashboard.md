---
description: Docker Desktop ダッシュボード
keywords: Docker Desktop Dashboard, container view
title: Docker Desktop ダッシュボード
---

{% comment %}
The Docker Desktop Dashboard provides a simple interface that enables you to interact with containers and applications, and manage the lifecycle of your applications directly from your machine. The Dashboard UI shows all running, stopped, and started containers with their status. It provides an intuitive interface to perform common actions to inspect, interact with, and manage your Docker objects including containers and Docker Compose-based applications.
{% endcomment %}
The Docker Desktop Dashboard provides a simple interface that enables you to interact with containers and applications, and manage the lifecycle of your applications directly from your machine. The Dashboard UI shows all running, stopped, and started containers with their status. It provides an intuitive interface to perform common actions to inspect, interact with, and manage your Docker objects including containers and Docker Compose-based applications.

{% comment %}
The Docker Desktop Dashboard offers the following benefits:
{% endcomment %}
Docker Desktop ダッシュボードには以下のような利点があります。

{% comment %}
- A GUI to abstract core information from the CLI
- Access to container logs directly in the UI to search and explore container behavior
- Access to combined Compose logs from the UI to understand Compose applications
- Quick visibility into ports being used by containers
- Monitor container resource utilization
{% endcomment %}
- CLI によって得られる重要な情報を GUI 上に示します。
- UI 上からコンテナーのログに直接アクセスし、コンテナーの動作を確認することができます。
- UI から Compose の統合ログにアクセスでき、Compose アプリケーションの動作を確認することができます。
- コンテナーが利用しているポートをすぐに確認できます。
- コンテナーのリソース使用状況を監視できます。

{% comment %}
In addition, the Dashboard UI allows you to:
{% endcomment %}
さらにダッシュボード UI では以下のことが可能です。

{% comment %}
- Navigate to the [Settings](/docker-for-windows/index/#docker-settings-dialog) menu to configure Docker Desktop preferences
- Access the [Troubleshoot](troubleshoot.md) menu to debug and perform restart operations
- Sign into [Docker Hub](/docker-for-windows/index/#docker-hub) using your Docker ID
{% endcomment %}
- [Settings](/docker-for-windows/index/#docker-settings-dialog) メニューから Docker Desktop の設定を行うことができます。
- [Troubleshoot](troubleshoot.md) メニューを使ってデバッグや再起動操作を行うことができます。
- Docker ID を使って [Docker Hub](/docker-for-windows/index/#docker-hub) にサインインすることができます。

{% comment %}
To access the Docker Desktop Dashboard, from the Docker menu, select **Dashboard**. The Dashboard provides a runtime view of all your containers and applications.
{% endcomment %}
Docker Desktop ダッシュボードにアクセスするには、Docker メニューの **Dashboard** を実行します。
Dashboard からは、すべてのコンテナーやアプリケーションの最新状況を確認することができます。

{% comment %}
![Docker Desktop Dashboard](images/dashboard-settings.png)
{% endcomment %}
![Docker Desktop ダッシュボード](images/dashboard-settings.png)

{% comment %}
## Explore running containers and applications
{% endcomment %}
{: #explore-running-containers-and-applications }
## 実行中コンテナーやアプリケーションの動作確認

{% comment %}
From the Docker menu, select **Dashboard**. This lists all your running containers and applications. Note that you must have running containers and applications to see them listed on the Docker Desktop Dashboard.
{% endcomment %}
Docker メニューから **Dashboard** を実行します。
起動している全コンテナーおよびアプリケーションが一覧表示されます。
なお Docker Desktop ダッシュボード上に一覧として表示させるためには、そのコンテナーやアプリケーションは実行しておかなければなりません。

{% comment %}
The following sections guide you through the process of creating a sample Redis container and a sample application to demonstrate the core functionalities in Docker Desktop Dashboard.
{% endcomment %}
これ以降の節では、サンプルとして Redis コンテナーとサンプルアプリケーションの生成手順を説明します。
これによって、Docker Desktop ダッシュボードの重要な機能を示していきます。

{% comment %}
### Start a Redis container
{% endcomment %}
{: #start-a-redis-container }
### Redis コンテナーの起動

{% comment %}
To start a Redis container, open your preferred CLI and run the following command:
{% endcomment %}
Redis コンテナーを起動するために、好みの CLI 環境を起動して以下のコマンドを実行します。

`docker run -dt redis`

{% comment %}
This creates a new Redis container. From the Docker menu, select **Dashboard** to see the new Redis container.
{% endcomment %}
上により新規の Redis コンテナーが生成されます。
Docker メニューから **Dashboard** を実行して、新しい Redis コンテナーを確認してみます。

{% comment %}
![Redis container](images/redis-container.png){:width="700px"}
{% endcomment %}
![Redis コンテナー](images/redis-container.png){:width="700px"}

{% comment %}
### Start a sample application
{% endcomment %}
{: #start-a-sample-application }
### サンプルアプリケーションの起動

{% comment %}
Now, let us start a sample application. You can download the [Example voting app](https://github.com/dockersamples/example-voting-app) from the Docker samples page. The example voting app is a distributed application that runs across multiple Docker containers.
{% endcomment %}
次にサンプルアプリケーションを起動します。
Docker サンプルページから [サンプル投票アプリ](https://github.com/dockersamples/example-voting-app) をダウンロードします。
このサンプル投票アプリは、複数の Docker コンテナー間において実行可能な分散アプリケーションになっています。

{% comment %}
![Example voting app architecture diagram](images/example-app-architecture.png){:width="600px"}
{% endcomment %}
![サンプル投票アプリのアーキテクチャー構成図](images/example-app-architecture.png){:width="600px"}

{% comment %}
The example voting application contains:
{% endcomment %}
サンプル投票アプリは以下により構成されます。

{% comment %}
- A front-end web app in [Python](https://github.com/dockersamples/example-voting-app/blob/master/vote) or [ASP.NET Core](https://github.com/dockersamples/example-voting-app/blob/master/vote/dotnet) which lets you vote between two options
- A [Redis](https://hub.docker.com/_/redis/) or [NATS](https://hub.docker.com/_/nats/) queue which collects new votes
- A [.NET Core](https://github.com/dockersamples/example-voting-app/blob/master/worker/src/Worker), [Java](https://github.com/dockersamples/example-voting-app/blob/master/worker/src/main) or [.NET Core 2.1](https://github.com/dockersamples/example-voting-app/blob/master/worker/dotnet) worker which consumes votes and stores them
- A [Postgres](https://hub.docker.com/_/postgres/) or [TiDB](https://hub.docker.com/r/dockersamples/tidb/tags/) database backed by a Docker volume
- A [Node.js](https://github.com/dockersamples/example-voting-app/blob/master/result) or [ASP.NET Core SignalR](https://github.com/dockersamples/example-voting-app/blob/master/result/dotnet) web app which shows the results of the voting in real time
{% endcomment %}
- [Python](https://github.com/dockersamples/example-voting-app/blob/master/vote) や [ASP.NET Core](https://github.com/dockersamples/example-voting-app/blob/master/vote/dotnet) によって書かれたウェブアプリのフロントエンド。2 つの選択肢から投票を行う機能を提供します。
- [Redis](https://hub.docker.com/_/redis/) または [NATS](https://hub.docker.com/_/nats/) によるキュー。これが新しい投票データを集めます。
- [.NET Core](https://github.com/dockersamples/example-voting-app/blob/master/worker/src/Worker)、[Java](https://github.com/dockersamples/example-voting-app/blob/master/worker/src/main)、[.NET Core 2.1](https://github.com/dockersamples/example-voting-app/blob/master/worker/dotnet) によるワーカー。投票データを取り込み保存します。
- [Postgres](https://hub.docker.com/_/postgres/) または [TiDB](https://hub.docker.com/r/dockersamples/tidb/tags/) データベース。Docker ボリューム上に置かれます。
- [Node.js](https://github.com/dockersamples/example-voting-app/blob/master/result) または [ASP.NET Core SignalR](https://github.com/dockersamples/example-voting-app/blob/master/result/dotnet) によるウェブアプリ。リアルタイムに投票結果を表示します。

{% comment %}
To start the application, navigate to the directory containing the example voting application in the CLI and run `docker-compose up --build`.
{% endcomment %}
このアプリケーションを起動するには、CLI 環境においてこのアプリケーションが存在しているディレクトリへ移動して `docker-compose up --build` を実行します。

```
$ docker-compose up --build
Creating network "example-voting-app-master_front-tier" with the default driver
Creating network "example-voting-app-master_back-tier" with the default driver
Creating volume "example-voting-app-master_db-data" with default driver
Building vote
Step 1/7 : FROM python:2.7-alpine
2.7-alpine: Pulling from library/python
Digest: sha256:d2cc8451e799d4a75819661329ea6e0d3e13b3dadd56420e25fcb8601ff6ba49
Status: Downloaded newer image for python:2.7-alpine
 ---> 1bf48bb21060
Step 2/7 : WORKDIR /app
 ---> Running in 7a6a0c9d8b61
Removing intermediate container 7a6a0c9d8b61
 ---> b1242f3c6d0c
Step 3/7 : ADD requirements.txt /app/requirements.txt
 ---> 0f5d69b65243
Step 4/7 : RUN pip install -r requirements.txt
 ---> Running in 92788dc9d682

...
Successfully built 69da1319c6ce
Successfully tagged example-voting-app-master_worker:latest
Creating example-voting-app-master_vote_1   ... done
Creating example-voting-app-master_result_1 ... done
Creating db                                 ... done
Creating redis                              ... done
Creating example-voting-app-master_worker_1 ... done
Attaching to db, redis, example-voting-app-master_result_1, example-voting-app-master_vote_1, example-voting-app-master_worker_1
...
```

{% comment %}
When the application successfully starts, from the Docker menu, select **Dashboard** to see the example voting application. Expand the application to see the containers running inside the application.
{% endcomment %}
アプリケーションが正常に起動したら、Docker メニューから **Dashboard** を実行して、サンプル投票アプリを確認してみます。
アプリケーションの表示を展開して、アプリケーション内部で稼動するコンテナーを確認してみます。

{% comment %}
![App Dashboard view](images/app-dashboard-view.png){:width="700px"}
{% endcomment %}
![アプリダッシュボードビュー](images/app-dashboard-view.png){:width="700px"}

{% comment %}
Now that you can see the list of running containers and applications on the Dashboard, let us explore some of the actions you can perform:
{% endcomment %}
このようにして起動中のコンテナーやアプリケーションの一覧は、ダッシュボード上において確認できるようになりました。
ここからさらに操作できる内容を見ていきます。

{% comment %}
- Click **Port** to open the port exposed by the container in a browser.
- Click **CLI** to open a terminal and run commands on the container.
- Click **Stop**, **Start**, **Restart**, or **Delete** to perform lifecycle operations on the container.
{% endcomment %}
- **Port** をクリックすると、コンテナーが開放しているそのポートをブラウザー上にて確認することができます。
- **CLI** をクリックすると端末画面が開くので、コンテナー上でのコマンド実行が可能です。
- **Stop**、**Start**、**Restart**、**Delete** をクリックすることで、コンテナーのライフサイクルを制御する操作を実行できます。

{% comment %}
Use the **Search** option to search for a specific object. You can also sort your containers and applications using various options. Click the **Sort by** drop-down to see a list of available options.
{% endcomment %}
**Search** オプションを用いると、特定オブジェクトを検索することができます。
またコンテナーやアプリケーションの並び順は、さまざまなオプション指定により変更可能です。
ドロップダウンメニュー **Sort by** を実行すれば、利用可能な検索オプションの一覧を確認することができます。

{% comment %}
## Interact with containers and applications
{% endcomment %}
{: #interact-with-containers-and-applications }
## コンテナーやアプリケーションとのやりとり

{% comment %}
From the Docker Desktop Dashboard, select the example voting application we started earlier.
{% endcomment %}
Docker Desktop ダッシュボードから、先ほど起動したサンプル投票アプリを選びます。

{% comment %}
The **application view** lists all the containers running on the application and contains a detailed logs view. It also allows you to start, stop, or delete the application.
{% endcomment %}
**application view** は、アプリケーション上にて起動中の全コンテナーの一覧が表示され、詳細ログビューも示されます。
ここからアプリケーションの起動、停止、削除を行うこともできます。

{% comment %}
Hover over the containers to see some of the core actions you can perform. Use the **Search** option at the bottom to search the application logs for specific events, or select the **Copy** icon to copy the logs to your clipboard.
{% endcomment %}
コンテナー上にマウスを移動させると、実行可能な操作がいくつか表示されます。
下段にある **Search** オプションを使うと、特定イベントに対するアプリケーションログを検索できます。
また **Copy** アイコンを選択すれば、その出力ログをクリップボードにコピーすることができます。

{% comment %}
![Application view](images/application-view.png){:width="700px"}
{% endcomment %}
![アプリケーションビュー](images/application-view.png){:width="700px"}

{% comment %}
Click on a specific container for detailed information about the container. The **container view** displays **Logs**, **Inspect**, and **Stats** tabs and provides quick action buttons to perform various actions.
{% endcomment %}
コンテナーのどれか 1 つをクリックし、コンテナーの詳細な情報を見てみます。
**container view** には **Logs**、**Inspect**、**Stats** の各タブが表示されています。
ボタンクリックによってさまざまな操作をすばやく行うことができます。

{% comment %}
![Explore the app](images/container-view.png){:width="700px"}
{% endcomment %}
![アプリの確認](images/container-view.png){:width="700px"}

{% comment %}
- Select **Logs** to see logs from the container. You can also search the logs for specific events and copy the logs to your clipboard.
{% endcomment %}
- **Logs** をクリックすると、コンテナーからのログを確認できます。
特定イベントに対するアプリケーションログを検索したり、出力ログをクリップボードにコピーしたりすることができます。

{% comment %}
- Select **Inspect** to view low-level information about the container. You can see the local path, version number of the image, SHA-256, port mapping, and other details.
{% endcomment %}
- **Inspect** をクリックすると、コンテナーに関する低レベル情報が確認できます。
ローカルパス、イメージのバージョン番号、SHA-256、ポートマッピングなどの情報です。

{% comment %}
- Select **Stats** to view information about the container resource utilization. You can see the amount of CPU, disk I/O, memory, and network I/O used by the container.
{% endcomment %}
- **Stats** をクリックすると、コンテナーのリソース使用状況を確認することができます。
コンテナーが利用している CPU 数、ディスク I/O、メモリ、ネットワーク I/O などです。

{% comment %}
You can also use the quick action buttons on the top bar to perform common actions such as opening a CLI to run commands in a container, and perform lifecycle operations such as stop, start, restart, or delete your container.
{% endcomment %}
またトップバー上にはすばやく操作するためのボタン類があり、共通的な操作を行うことができます。
たとえば CLI を開いてコンテナー内にてコマンドを実行したり、コンテナーのライフサイクルを操作する操作としてコンテナーの停止、起動、再起動を行ったりできます。

{% comment %}
Click **Port** to open the port exposed by the container in a browser.
{% endcomment %}
**Port** をクリックすると、コンテナーが開放しているポートをブラウザー上にて確認することができます。

{% comment %}
![Spring app browser view](images/app-browser-view.png){:width="700px"}
{% endcomment %}
![Spring アプリのブラウザービュー](images/app-browser-view.png){:width="700px"}

{% comment %}
## Feedback
{% endcomment %}
{: #feedback }
## フィードバック

{% comment %}
We would like to hear from you about the new Dashboard UI. Let us know your feedback by creating an issue in the [docker/for-win](https://github.com/docker/for-win/issues) GitHub repository.
{% endcomment %}
新たなこのダッシュボード UI について、みなさんからのご意見をうかがいたいと思います。
GitHub リポジトリ [docker/for-win](https://github.com/docker/for-win/issues) に issue を生成してフィードバックを行ってください。
