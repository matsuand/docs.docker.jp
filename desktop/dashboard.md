---
description: Docker ダッシュボード
keywords: Docker Dashboard, manage, containers, images
title: Docker ダッシュボード
redirect_from:
- /docker-for-mac/dashboard/
- /docker-for-windows/dashboard/
---

{% comment %}
The Docker Dashboard provides a simple interface that enables you to manage your containers, applications, and images directly from your machine without having to use the CLI to perform core actions.
{% endcomment %}
Docker Desktop ダッシュボードは、コンテナー、アプリケーション、イメージに食説にアクセスできるようなシンプルなインターフェースを提供します。
CLI を使って難しい操作を行う必要がなくなります。

{% comment %}
The **Containers/Apps** view provides a runtime view of all your containers and applications. It allows you to interact with containers and applications, and manage the lifecycle of your applications directly from your machine. This view also provides an intuitive interface to perform common actions to inspect, interact with, and manage your Docker objects including containers and Docker Compose-based applications.
{% endcomment %}
**Containers/Apps** 画面では、コンテナーやアプリケーションの実行状態を表示します。
この画面においてはコンテナーやアプリケーションとのやりとりを行うことが可能であり、手元のマシンからアプリケーションのライフサイクルを直接制御することができます。
この画面では、コンテナーや Docker Compose ベースのアプリケーションに含まれる Docker オブジェクトに対して、主要な操作、つまり詳細確認、対話指示、管理を直感的なインターフェースにより提供します。

{% comment %}
The **Images** view displays a list of your Docker images, and allows you to run an image as a container, pull the latest version of an image from Docker Hub, and inspect images. It also contains clean up options to remove unwanted images from the disk to reclaim space. If you are logged in, you can also see the images you and your organization have shared on Docker Hub.
{% endcomment %}
**Images** 画面には Docker イメージが一覧表示されます。
ここからイメージをコンテナーとして実行したり、Docker Hub からの最新版イメージのプル、イメージの詳細確認を行ったりすることができます。
またイメージ削除の機能もあり、不要となったイメージをディスク上から削除して容量を確保することができます。
Docker Hub にログインできていれば、Docker Hub 上において自分や組織が共有しているイメージを参照することもできます。

{% comment %}
In addition, the Docker Dashboard allows you to:
{% endcomment %}
さらに Docker ダッシュボード では以下のことが可能です。

{% comment %}
- Easily navigate to the **Preferences** (**Settings** in Windows) menu to configure Docker Desktop preferences
- Access the **Troubleshoot** menu to debug and perform restart operations
- Sign into [Docker Hub](https://hub.docker.com/) using your Docker ID
{% endcomment %}
- **Preferences** から（Windows では **Settings**）メニューから簡単に Docker Desktop の設定を行うことができます。
- **Troubleshoot** メニューを使ってデバッグや再起動操作を行うことができます。
- Docker ID を使って [Docker Hub](https://hub.docker.com/) にサインインすることができます。

{% comment %}
To access the Docker Dashboard, from the Docker menu, select **Dashboard**. On Windows, click the Docker icon to open the Dashboard.
{% endcomment %}
Docker ダッシュボードにアクセスするには、Docker メニューの **Dashboard** を実行します。
Windows においては Docker アイコンから Dashboard を開くことができます。

{% comment %}
## Explore running containers and applications
{% endcomment %}
{: #explore-running-containers-and-applications }
## 実行中コンテナーやアプリケーションの動作確認

{% comment %}
From the Docker menu, select **Dashboard**. This lists all your running containers and applications. You must have running or stopped containers and applications to see them listed on the Docker Dashboard.
{% endcomment %}
Docker メニューから **Dashboard** を実行します。
起動している全コンテナーおよびアプリケーションが一覧表示されます。
なお Docker ダッシュボード上に一覧として表示させるためには、そのコンテナーやアプリケーションは実行しているか停止中でなければなりません。

![Docker Desktop Dashboard](images/mac-dashboard.png)

{% comment %}
The following sections guide you through the process of creating a sample Redis container and a sample application to demonstrate the core functionalities in Docker Dashboard.
{% endcomment %}
これ以降の節では、サンプルとして Redis コンテナーとサンプルアプリケーションの生成手順を説明します。
これによって、Docker ダッシュボードの重要な機能を示していきます。

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
### Start a sample application
{% endcomment %}
{: #start-a-sample-application }
### サンプルアプリケーションの起動

{% comment %}
Let's start a sample application. Download the [Example voting app](https://github.com/dockersamples/example-voting-app) from the Docker samples page. The example voting app is a distributed application that runs across multiple Docker containers. The app contains:
{% endcomment %}
サンプルアプリケーションを作ります。
Docker のサンプルページから [サンプル投票アプリ](https://github.com/dockersamples/example-voting-app) をダウンロードしてください。
このサンプル投票アプリは、さまざまな Docker コンテナー上において起動する分散アプリケーションです。
このアプリは以下により構成されます。

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

```shell
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
When the application starts successfully, from the Docker menu, select **Dashboard** to see the Example voting application. Expand the application to see the containers running inside the application.
{% endcomment %}
アプリケーションが正常に起動したら、Docker メニューから **Dashboard** を実行して、サンプル投票アプリを確認してみます。
アプリケーションの表示を展開して、アプリケーション内部で稼動するコンテナーを確認してみます。

{% comment %}
![Spring Boot application view](images/app-dashboard-view.png){:width="700px"}
{% endcomment %}
![Spring Boot アプリケーションビュー](images/app-dashboard-view.png){:width="700px"}

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
From the Docker Dashboard, select the example voting application we started earlier.
{% endcomment %}
Docker ダッシュボードから、先ほど起動したサンプル投票アプリを選びます。

{% comment %}
The **Containers/Apps** view lists all the containers running on the application and contains a detailed logs view. It also allows you to start, stop, or delete the application. Use the **Search** option at the bottom of the logs view to search application logs for specific events, or select the **Copy** icon to copy the logs to your clipboard.
{% endcomment %}
**Containers/Apps** 画面には、アプリケーション上に起動する全コンテナーの一覧が表示され、詳細ログ画面も示されます。
ここからアプリケーションの起動、停止、削除を行うこともできます。
ログ画面の下段にある **Search** オプションを利用すると、特定イベントのアプリケーションログを検索することができます。
また **Copy** アイコンを選択すれば、その出力ログをクリップボードにコピーすることができます。

{% comment %}
Click **Open in Visual Studio Code** to open the application to open the application in VS Code. Hover over the list of containers to see some of the core actions you can perform.
{% endcomment %}
**Open in Visual Studio Code** をクリックすると、VS Code 上にアプリケーションを開くことができます。
コンテナー一覧の上にマウスを移動させると、実行可能な主要操作が示されます。

{% comment %}
![Application view](images/mac-application-view.png){:width="700px"}
{% endcomment %}
![アプリケーションビュー](images/mac-application-view.png){:width="700px"}

{% comment %}
## Container view
{% endcomment %}
{: #container-view }
## コンテナー画面

{% comment %}
Click on a specific container for detailed information about the container. The **container view** displays **Logs**, **Inspect**, and **Stats** tabs and provides quick action buttons to perform various actions.
{% endcomment %}
コンテナーのどれか 1 つをクリックし、コンテナーの詳細な情報を見てみます。
**container view** には **Logs**、**Inspect**、**Stats** の各タブが表示されています。
ボタンクリックによってさまざまな操作をすばやく行うことができます。

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
## Explore your images
{% endcomment %}
{: #explore-your-images }
## イメージの確認

{% comment %}
The **Images**  view is a simple interface that lets you manage Docker images without having to use the CLI. By default, it displays a list of all Docker images on your local disk. To view images in remote repositories, click **Sign in** and connect to Docker Hub. This allows you to collaborate with your team and manage your images directly through Docker Desktop.
{% endcomment %}
**Images** 画面の操作インターフェースはとても簡単です。
CLI を利用してくても Docker イメージを管理することができます。
Docker イメージの一覧は、デフォルトではローカルディスクにあるものが表示されます。
リモートリポジトリにあるイメージを表示するには **Sign in** をクリックして Docker Hub に接続します。
これを行うと開発チーム内での共同作業が可能となり、イメージを直接 Docker Desktop から管理することができます。

{% comment %}
The Images view allows you to perform core operations such as running an image as a container, pulling the latest version of an image from Docker Hub, pushing the image to Docker Hub, and inspecting images.
{% endcomment %}
イメージ画面では主要な操作、たとえばイメージのコンテナーとしての実行、Docker Hub からのイメージ最新版のプル、Docker Hub へのイメージのプッシュ、イメージの詳細確認などを行うことができます。

{% comment %}
In addition, the Images view displays metadata about the image such as the tag, image ID, date when the image was created, and the size of the image. It also displays **In Use** tags next to images used by running and stopped containers. This allows you to review the list of images and use the **Clean up images** option to remove any unwanted images from the disk to reclaim space.
{% endcomment %}
さらにイメージ画面には、イメージに関するメタデータ情報が表示されます。
たとえばタグ、イメージ ID、イメージ生成日付、イメージサイズなどです。
またイメージの横に **In Use** タグがあって、コンテナーの起動や停止に利用できます。
これを使ってイメージ一覧を抽出して **Clean up images** オプションを利用すれば、必要のないイメージを削除して容量を確保することができます。

{% comment %}
The Images view also allows you to search images on your local disk and sort them using various options.
{% endcomment %}
イメージ画面では、ローカルディスク内のイメージを検索することもできます。
そしてさまざまなオプションを使って並べ替えができます。

{% comment %}
Let's explore the various options in the **Images** view.
{% endcomment %}
**Images** 画面にてさまざまなオプションを使って試してみます。

{% comment %}
If you don’t have any images on your disk, run the command `docker pull redis` in a terminal to pull the latest Redis image. This command pulls the latest Redis image from Docker Hub.
{% endcomment %}
ディスク上にイメージがまったくない場合は、ターミナルからコマンド`docker pull redis`を実行して、最新の Redis イメージを取得してください。
このコマンドは最新の Redis イメージを Docker Hub からプルします。

{% comment %}
Select **Dashboard** > **Images** to see the Redis image.
{% endcomment %}
**Dashboard** > **Images** を実行して Redis イメージを確認します。

{% comment %}
![Redis image](images/redis-image.png){:width="700px"}
{% endcomment %}
![Redis イメージ](images/redis-image.png){:width="700px"}

{% comment %}
### Run an image as a container
{% endcomment %}
{: #run-an-image-as-a-container }
### コンテナーとしてのイメージ実行

{% comment %}
Now that you have a Redis image on your disk, let’s run this image as a container:
{% endcomment %}
ディスク上に Redis イメージを取得したので、このイメージをコンテナーとして実行します。

{% comment %}
1. From the Docker menu, select **Dashboard** > **Images**. This displays a list of images on your local disk.
2. Select the Redis image from the list and click **Run**.
3. When prompted, click the **Optional settings** drop-down to specify a name, port, volumes, and click **Run**.
{% endcomment %}
1. Docker メニューから **Dashboard** > **Images** を実行します。
   これによりローカルディスク上のイメージ一覧が表示されます。
2. 一覧から Redis イメージを選んで **Run** をクリックします。
3. 確認画面が表示されたら **Optional settings** ドロップダウンメニューをクリックして、名前、ポート、ボリュームを指定した上で **Run** をクリックします。

    {% comment %}
    To use the defaults, click **Run** without specifying any optional settings. This creates a new container from the Redis image and opens it on the **Container/Apps** view.
    {% endcomment %}
    デフォルト設定を利用する場合は、オプション設定を何も指定せずに **Run** をクリックします。
    こうすると Redis イメージから新たなコンテナーが生成されて、**Container/Apps** 画面内に表示されます。

{% comment %}
### Pull the latest image from Docker Hub
{% endcomment %}
{: #pull-the-latest-image-from-docker-hub }
### Docker Hub からの最新イメージのプル

{% comment %}
To pull the latest image from Docker Hub:
{% endcomment %}
Docker Hub から最新イメージをプルするには以下を行います。

{% comment %}
1. From the Docker menu, select **Dashboard** > **Images**. This displays a list of images on your local disk.
2. Select the image from the list and click the more options button.
3. Click **Pull**. This pulls the latest version of the image from Docker Hub.
{% endcomment %}
1. Docker メニューから **Dashboard** > **Images** を実行します。
   これによりローカルディスク上のイメージ一覧が表示されます。
2. 一覧から目的のイメージを選んで、more options ボタンをクリックします。
3. **Pull** をクリックします。
   Docker Hub から最新版のイメージがプルされます。

{% comment %}
> **Note**
>
> The repository must exist on Docker Hub in order to pull the latest version of an image. You must be logged in to pull private images.
{% endcomment %}
> **メモ**
>
> イメージの最新版をプルするためには、利用するリポジトリが Docker Hub 上に存在していなければなりません。
> プライベートイメージをプルする場合には、ログインしておくことが必要です。

{% comment %}
### Push an image to Docker Hub
{% endcomment %}
{: #push-an-image-to-docker-hub }
### Docker Hub へのイメージのプッシュ

{% comment %}
To push an image to Docker Hub:
{% endcomment %}
Docker Hub へイメージをプッシュするには以下を行います。

{% comment %}
1. From the Docker menu, select **Dashboard** > **Images**. This displays a list of images on your local disk.
2. Select the image from the list and click the more options button.
3. Click **Push to Hub.**
{% endcomment %}
1. Docker メニューから **Dashboard** > **Images** を実行します。
   これによりローカルディスク上のイメージ一覧が表示されます。
2. 一覧から目的のイメージを選んで、more options ボタンをクリックします。
3. **Push to Hub** をクリックします。

{% comment %}
> **Note**
>
> You can only push an image to Docker Hub if the image belongs to your Docker ID or your organization. That is, the image must contain the correct username/organization in its tag to be able to push it to Docker Hub.
{% endcomment %}
> **メモ**
>
> Docker Hub にイメージをプッシュできるのは、そのイメージが自身の Docker ID や組織に属している場合に限ります。
> つまりそのイメージのタグ内に適切なユーザー名/組織が含まれている場合に限って、Docker Hub へのプッシュができるようになります。

{% comment %}
### Inspect an image
{% endcomment %}
{: #inspect-an-image }
### イメージの確認

{% comment %}
Inspecting an image displays detailed information about the image such as the image history, image ID, the date the image was created, size of the image, etc. To inspect an image:
{% endcomment %}
イメージの詳細確認を行うと、イメージに関する詳細情報が表示されます。
たとえばイメージ履歴、イメージ ID、イメージの生成日付、イメージサイズなどです。
イメージの詳細確認は以下のようにして行います。

{% comment %}
1. From the Docker menu, select **Dashboard** > **Images**. This displays a list of images on your local disk.
2. Select the image from the list and click the more options button.
3. Click **Inspect**.
4. The image inspect view also provides options to pull the latest image, push image to Hub, remove the image, or run the image as a container.
{% endcomment %}
1. Docker メニューから **Dashboard** > **Images** を実行します。
   これによりローカルディスク上のイメージ一覧が表示されます。
2. 一覧から目的のイメージを選んで、more options ボタンをクリックします。
3. **Inspect** をクリックします。
4. イメージ詳細画面では、最新イメージのプル、Hub へのイメージプッシュ、イメージ削除、コンテナーとしてのイメージ実行も行うことができます。

{% comment %}
### Remove an image
{% endcomment %}
{: #remove-an-image }
### イメージの削除

{% comment %}
The **Images** view allows you to remove unwanted images from the disk. The Images on disk status bar displays the number of images and the total disk space used by the images.
{% endcomment %}
**Images** 画面からは、不要なイメージをディスクから削除することができます。
ディスクステータスバー上のイメージには、イメージ数、イメージによるディスク総容量が表示されます。

{% comment %}
You can remove individual images or use the **Clean up** option to delete unused and dangling images.
{% endcomment %}
個別にイメージを削除することができます。
あるいは **Clean up** オプションを使って、未使用の dangling なイメージを削除することができます。

{% comment %}
To remove individual images:
{% endcomment %}
イメージを個別に削除するには以下を行います。

{% comment %}
1. From the Docker menu, select **Dashboard** > **Images**. This displays a list of images on your local disk.
2. Select the image from the list and click the more options button.
3. Click **Remove**. This removes the image from your disk.
{% endcomment %}
1. Docker メニューから **Dashboard** > **Images** を実行します。
   これによりローカルディスク上のイメージ一覧が表示されます。
2. 一覧から目的のイメージを選んで、more options ボタンをクリックします。
3. **Remove** をクリックします。
   これによりディスクからイメージが削除されます。

{% comment %}
> **Note**
>
> To remove an image used by a running or a stopped container, you must first remove the associated container.
{% endcomment %}
> **メモ**
>
> 実行中、停止中のコンテナーによって利用されているイメージを削除するには、その前に関連するコンテナーを削除しなければなりません。

{% comment %}
**To remove unused and dangling images:**
{% endcomment %}
**未使用の dangling なイメージの削除**

{% comment %}
An **unused** image is an image which is not used by any running or stopped containers. An image becomes **dangling** when you build a new version of the image with the same tag.
{% endcomment %}
**未使用の** イメージとは、実行中、停止中のコンテナーのいずれからも利用されていないイメージのことです。
同一のタグを用いて、新たなバージョンのそのイメージをビルドしたときに、そのイメージが **dangling**（宙ぶらり）になります。

{% comment %}
**To remove an unused or a dangling image:**
{% endcomment %}
**未使用の dangling なイメージの削除方法**

{% comment %}
1. From the Docker menu, select **Dashboard** > **Images**. This displays a list of images on your disk.
2. Select the **Clean up** option from the **Images on disk** status bar.
3. Use the **Unused** and **Dangling** check boxes to select the type of images you would like to remove.
{% endcomment %}
1. Docker メニューから **Dashboard** > **Images** を実行します。
   これによりローカルディスク上のイメージ一覧が表示されます。
2. ステータスバー上の **Images on disk** から **Clean up** を選びます。
3. 削除jしたいイメージのタイプに合わせて **Unused** と **Dangling** のチェックボックスを選びます。

    {% comment %}
    The **Clean up** images status bar displays the total space you can reclaim by removing the selected images.
    {% endcomment %}
    ステータスバーの **Clean up** イメージには、選択したイメージの削除によって増加するディスク総量が示されます。
{% comment %}
4. Click **Remove** to confirm.
{% endcomment %}
4. 確認のために **Remove** をクリックします。

{% comment %}
### Interact with remote repositories
{% endcomment %}
{: #interact-with-remote-repositories }
### リモートリポジトリとのやりとり

{% comment %}
The Images view also allows you to manage and interact with images in remote repositories and lets you switch between organizations.
{% endcomment %}
Images 画面では、リモートリポジトリ内のイメージを管理したりやりとりしたりすることができます。
そして組織間を切り替えることもできます。

{% comment %}
The **Pull** option allows you to pull the latest version of the image from Docker Hub. The **View in Hub** option opens the Docker Hub page and displays detailed information about the image, such as the OS architecture, size of the image, the date when the image was pushed, and a list of the image layers.
{% endcomment %}
**Pull** オプションを使うと、Docker Hub から最新版イメージをプルすることができます。
**View in Hub** オプションを使うと Docker Hub ページが開き、イメージに関する詳細情報が表示されます。
たとえば OS アーキテクチャー、イメージサイズ、イメージのプッシュ日付、イメージレイヤー一覧などです。

{% comment %}
![View image in Hub](images/image-details.png){:width="700px"}
{% endcomment %}
![Hub 上のイメージ一覧](images/image-details.png){:width="700px"}

{% comment %}
To interact with remote repositories:
{% endcomment %}
リモートリポジトリをやりとりするには以下を行います。

{% comment %}
1. Click the **Remote repositories** tab.
2. Select an organization from the drop-down list. This displays a list of repositories in your organization.
3. Click on an image from the list and then select **Pull** to pull the latest image from the remote repository.
4. To view a detailed information about the image in Docker Hub, select the image and then click **View in Hub**.
{% endcomment %}
1. **Remote repositories** タブをクリックします。
2. ドロップダウンリストから組織を選択します。
   これにより組織内のリポジトリ一覧が表示されます。
3. 一覧の中からイメージをクリックしてから **Pull** を選び、リモートリポジトリから最新イメージをプルします。
4. Docker Hub 内のイメージに関する詳細情報を確認するには、イメージを選択して **View in Hub** をクリックします。
