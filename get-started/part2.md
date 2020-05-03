---
title: "イメージの構築と実行"
keywords: containers, images, dockerfiles, node, code, coding, build, push, run
description: Learn how to create a Docker image by writing a Dockerfile, and use it to run a simple container.
---

{% include_relative nav.html selected="2" %}

{% comment %}
## Prerequisites
{% endcomment %}
{: #prerequisites }
## 前提条件

{% comment %}
Work through the orientation and setup in [Part 1](index.md).
{% endcomment %}
[1 部](index.md) でのオリエンテーション、設定をひととおり終えていること。

{% comment %}
## Introduction
{% endcomment %}
{: #introduction }
## はじめに

{% comment %}
Now that you've set up your development environment, thanks to Docker Desktop,
you can begin to develop containerized applications. In general, the development workflow looks like this:
{% endcomment %}
Docker Desktop というもののおかげで、開発環境を整えることができました。
ここからはコンテナー化されたアプリケーションを作り出していくことにします。
構築の手順は一般に以下のようになります。

{% comment %}
1. Create and test individual containers for each component of your application by first creating Docker images.
2. Assemble your containers and supporting infrastructure into a complete application.
3. Test, share, and deploy your complete containerized application.
{% endcomment %}
1. 初めての Docker イメージを生成した上で、アプリケーションの各コンポーネントに対応するコンテナーを生成しテストします。
2. コンテナーとこれを支えるインフラストラクチャーを整えて、一体となったアプリケーションとします。
3. 完全にコンテナー化されたアプリケーションをテスト、共有しデプロイを行います。

{% comment %}
In this stage of the tutorial, let's focus on step 1 of this workflow: creating the images that your containers will be based on. Remember, a Docker image captures the private filesystem that your containerized processes will run in; you need to create an image that contains just what your application needs to run.
{% endcomment %}
In this stage of the tutorial, let's focus on step 1 of this workflow: creating the images that your containers will be based on. Remember, a Docker image captures the private filesystem that your containerized processes will run in; you need to create an image that contains just what your application needs to run.

{% comment %}
> **Containerized development environments** are easier to set up than traditional development environments, once you learn how to build images as we'll discuss below. This is because a containerized development environment will isolate all the dependencies your app needs inside your Docker image; there's no need to install anything other than Docker on your development machine. In this way, you can easily develop applications for different stacks without changing anything on your development machine.
{% endcomment %}
> **Containerized development environments** are easier to set up than traditional development environments, once you learn how to build images as we'll discuss below. This is because a containerized development environment will isolate all the dependencies your app needs inside your Docker image; there's no need to install anything other than Docker on your development machine. In this way, you can easily develop applications for different stacks without changing anything on your development machine.

{% comment %}
## Set up
{% endcomment %}
{: #set-up }
## 設定

{% comment %}
Let us download an example project from the [Docker Samples](https://github.com/dockersamples/node-bulletin-board) page.
{% endcomment %}
[Docker サンプル](https://github.com/dockersamples/node-bulletin-board) のページから、サンプルプロジェクトをダウンロードすることにします。

<ul class="nav nav-tabs">
  <li class="active"><a data-toggle="tab" href="#clonegit">Git</a></li>
  <li><a data-toggle="tab" href="#clonewin">Windows (Git がない場合)</a></li>
  <li><a data-toggle="tab" href="#clonemac">Mac または Linux (Git がない場合)</a></li>
</ul>
<div class="tab-content">
  <div id="clonegit" class="tab-pane fade in active">
{% capture git-clone-content %}

### Git

{% comment %}
If you are using Git, you can clone the example project from GitHub:
{% endcomment %}
Git を利用している場合は GitHub からサンプルプロジェクトをクローンします。

```shell
git clone https://github.com/dockersamples/node-bulletin-board
cd node-bulletin-board/bulletin-board-app
```

{% endcapture %}
{{ git-clone-content | markdownify }}

</div>
<div id="clonewin" class="tab-pane fade" markdown="1">
{% capture win-clone-content %}

### Windows (Git がない場合)

{% comment %}
If you are using a Windows machine and prefer to download the example project without installing Git, run the following commands in PowerShell:
{% endcomment %}
Windows ユーザーであって Git をインストールせずにサンプルプロジェクトをダウンロードしたい場合は、PowerShell から以下のコマンドを実行します。

```shell
curl.exe -LO https://github.com/dockersamples/node-bulletin-board/archive/master.zip
tar.exe xf master.zip
cd node-bulletin-board-master\bulletin-board-app
```

{% endcapture %}
{{ win-clone-content | markdownify }}
</div>

<div id="clonemac" class="tab-pane fade" markdown="1">
{% capture mac-clone-content %}

### Mac または Linux (Git がない場合)

{% comment %}
If you are using a Mac or a Linux machine and prefer to download the example project without installing Git, run the following commands in a terminal:
{% endcomment %}
Mac ユーザーあるいは Linux ユーザーであって Git をインストールせずにサンプルプロジェクトをダウンロードしたい場合は、端末画面から以下のコマンドを実行します。

```shell
curl -LO https://github.com/dockersamples/node-bulletin-board/archive/master.zip
unzip master.zip
cd node-bulletin-board-master/bulletin-board-app
```

{% endcapture %}
{{ mac-clone-content | markdownify }}
</div>
<hr>
</div>

{% comment %}
The `node-bulletin-board` project is a simple bulletin board application, written in Node.js. In this example, let's imagine you wrote this app, and are now trying to containerize it.
{% endcomment %}
`node-bulletin-board` プロジェクトは簡易な掲示板（bulletin board）アプリケーションであり、Node.js を用いて開発されているものです。
この例では、すでに作り上げているアプリケーションを、これからコンテナー化していく状況にあると考えてください。

{% comment %}
## Define a container with Dockerfile
{% endcomment %}
## Dockerfile におけるコンテナーの定義

{% comment %}
Take a look at the file called `Dockerfile` in the bulletin board application. Dockerfiles describe how to assemble a private filesystem for a container, and can also contain some metadata describing how to run a container based on this image. The bulletin board app Dockerfile looks like this:
{% endcomment %}
掲示板アプリケーションに対応する `Dockerfile` というファイルを見てみます。
Dockerfile というものには、コンテナーにはどのようなプライベートファイルシステムを用いるのかが記述されます。
あるいはそのイメージに基づくコンテナーをどのように起動させるかを表わすメタデータを含ませることもあります。
掲示板アプリケーションの Dockerfile はたとえば以下のようになります。

```dockerfile
# 親イメージとして公式イメージを利用
FROM node:current-slim

# ワーキングディレクトリの設定
WORKDIR /usr/src/app

# ホストシステム内からカレントディレクトリにファイルをコピー
COPY package.json .

# イメージのファイルシステム内からコマンドを実行
RUN npm install

# コンテナーが実行中に利用するポート番号を指定
EXPOSE 8080

# コンテナー内から所定のコマンドを実行
CMD [ "npm", "start" ]

# アプリソースの残りすべてを、ホストからイメージ内ファイルシステムへコピー
COPY . .
```

{% comment %}
Writing a Dockerfile is the first step to containerizing an application. You can think of these Dockerfile commands as a step-by-step recipe on how to build up your image. This one takes the following steps:
{% endcomment %}
Dockerfile を記述することが、アプリケーションコンテナー化の第一歩です。
この Dockerfile に記述するコマンドは、いってみればイメージの構築方法を順に示すレシピのようなものです。
このファイルでは以下の手順を行います。

{% comment %}
- Start `FROM` the pre-existing `node:current-slim` image. This is an *official image*, built by the node.js vendors and validated by Docker to be a high-quality image containing the Node.js Long Term Support (LTS) interpreter and basic dependencies.
- Use `WORKDIR` to specify that all subsequent actions should be taken from the directory `/usr/src/app` *in your image filesystem* (never the host's filesystem).
- `COPY` the file `package.json` from your host to the present location (`.`) in your image (so in this case, to `/usr/src/app/package.json`)
- `RUN` the command `npm install` inside your image filesystem (which will read `package.json` to determine your app's node dependencies, and install them)
- `COPY` in the rest of your app's source code from your host to your image filesystem.
{% endcomment %}
- `FROM` により既存の `node:current-slim` というイメージからはじめます。
  これは **公式イメージ** の  1 つであり node.js ベンダーによって構築されています。
  さらに Node.js 長期サポート（LTS）インタープリターや基本的な依存パッケージを含んだ高品質イメージとして、Docker により検証されているものです。
- この後に続けて行っていく処理を、**イメージのファイルシステム** 上にて実行する `/usr/src/app` ディレクトリを `WORKDIR` により指定します（ホストのファイルシステムではありません）。
- `COPY` によってホスト内のファイル `package.json` を、イメージ内の現在ディレクトリ（`.`）にコピーします（この例では `/usr/src/app/package.json` となります）。
- `RUN` によってイメージ内のファイルシステム上で、コマンド `npm install` を実行します（このコマンドは `package.json` を読み込み、node アプリケーションに必要となる依存パッケージを検出してインストールします）。
- 上記以外のアプリケーションソースコードを、`COPY` によってホストからイメージ内のファイルシステムにコピーします。

{% comment %}
You can see that these are much the same steps you might have taken to set up and install your app on your host. However, capturing these as a Dockerfile allows you to do the same thing inside a portable, isolated Docker image.
{% endcomment %}
お気づきのように上記の手順は、ホスト上においてアプリケーションをインストールし設定することと、ほとんど変わりません。
ただし Dockerfile 内にこういった手順を書き記すことにより、携帯可能で独立した Docker イメージに対して、同様の処理内容を実現できることを意味します。

{% comment %}
The steps above built up the filesystem of our image, but there are other lines in your Dockerfile.
{% endcomment %}
上の手順ではイメージ内にファイルシステムを構築していますが、Dockerfile 内にはそれ以外のことも行っています。

{% comment %}
The `CMD` directive is the first example of specifying some metadata in your image that describes how to run a container based on this image. In this case, it's saying that the containerized process that this image is meant to support is `npm start`.
{% endcomment %}
`CMD` ディレクティブは、イメージ内にてメタデータを指定する最初の例です。
これはイメージ内において、どのようにしてコンテナーを起動するかを記述しています。
今の場合、このイメージがサポートするコンテナー化プロセスが `npm start` である、ということになります。

{% comment %}
The `EXPOSE 8080` informs Docker that the container is listening on port 8080 at runtime.
{% endcomment %}
`EXPOSE 8080` は Docker に対して、コンテナー実行時にはポート 8080 を用いることを指示します。

{% comment %}
What you see above is a good way to organize a simple Dockerfile; always start with a `FROM` command, follow it with the steps to build up your private filesystem, and conclude with any metadata specifications. There are many more Dockerfile directives than just the few you see above. For a complete list, see the [Dockerfile reference](https://docs.docker.com/engine/reference/builder/).
{% endcomment %}
上で見てきた内容は、単純な Dockerfile を構成するものとしては的確なものです。
必ず `FROM` コマンドから始めます。
続けてプライベートファイルシステムを構築していきます。
そしてメタデータの仕様を定めます。
上に示した Dockerfile ディレクティブはほんのわずかであって、まだまだたくさんのものがあります。
ディレクティブの一覧は [Dockerfile リファレンス](https://docs.docker.com/engine/reference/builder/) を参照してください。

{% comment %}
## Build and test your image
{% endcomment %}
{: #build-and-test-your-image }
## イメージの構築とテスト

{% comment %}
Now that you have some source code and a Dockerfile, it's time to build your first image, and make sure the containers launched from it work as expected.
{% endcomment %}
ここにソースコードと Dockerfile の準備が整いました。
そこで初めてのイメージビルドを行います。
そしてそこから起動されるコンテナーが、期待どおりに動作することを確認します。

{% comment %}
> **Windows users**: this example uses Linux containers. Make sure your environment is running Linux containers by right-clicking on the Docker logo in your system tray, and clicking **Switch to Linux containers** if the option appears. Don't worry - all the commands in this tutorial work the exact same way for Windows containers.
{% endcomment %}
> **Windows ユーザーの場合**: 本例では Linux コンテナーを用います。
> Make sure your environment is running Linux containers by right-clicking on the Docker logo in your system tray, and clicking **Switch to Linux containers** if the option appears. Don't worry - all the commands in this tutorial work the exact same way for Windows containers.

{% comment %}
Make sure you're in the directory `node-bulletin-board/bulletin-board-app` in a terminal or PowerShell using the `cd` command. Let's build your bulletin board image:
{% endcomment %}
Make sure you're in the directory `node-bulletin-board/bulletin-board-app` in a terminal or PowerShell using the `cd` command. Let's build your bulletin board image:

```script
docker build --tag bulletinboard:1.0 .
```

{% comment %}
You'll see Docker step through each instruction in your Dockerfile, building up your image as it goes. If successful, the build process should end with a message `Successfully tagged bulletinboard:1.0`.
{% endcomment %}
You'll see Docker step through each instruction in your Dockerfile, building up your image as it goes. If successful, the build process should end with a message `Successfully tagged bulletinboard:1.0`.

{% comment %}
> **Windows users:** you may receive a message titled 'SECURITY WARNING' at this step, noting the read, write, and execute permissions being set for files added to your image. We aren't handling any sensitive information in this example, so feel free to disregard the warning in this example.
{% endcomment %}
> **Windows users:** you may receive a message titled 'SECURITY WARNING' at this step, noting the read, write, and execute permissions being set for files added to your image. We aren't handling any sensitive information in this example, so feel free to disregard the warning in this example.

{% comment %}
## Run your image as a container
{% endcomment %}
{: #run-your-image-as-a-container }
## イメージをコンテナーとして実行

{% comment %}
1.  Start a container based on your new image:
{% endcomment %}
1.  新しいイメージに基づいたコンテナーを起動します。

    ```script
    docker run --publish 8000:8080 --detach --name bb bulletinboard:1.0
    ```

    {% comment %}
    There are a couple of common flags here:
    {% endcomment %}
    There are a couple of common flags here:

    {% comment %}
    - `--publish` asks Docker to forward traffic incoming on the host's port 8000, to the container's port 8080. Containers have their own private set of ports, so if you want to reach one from the network, you have to forward traffic to it in this way. Otherwise, firewall rules will prevent all network traffic from reaching your container, as a default security posture.
    - `--detach` asks Docker to run this container in the background.
    - `--name` specifies a name with which you can refer to your container in subsequent commands, in this case `bb`.
    {% endcomment %}
    - `--publish` asks Docker to forward traffic incoming on the host's port 8000, to the container's port 8080. Containers have their own private set of ports, so if you want to reach one from the network, you have to forward traffic to it in this way. Otherwise, firewall rules will prevent all network traffic from reaching your container, as a default security posture.
    - `--detach` asks Docker to run this container in the background.
    - `--name` specifies a name with which you can refer to your container in subsequent commands, in this case `bb`.

    {% comment %}
    Also notice, you didn't specify what process you wanted your container to run. You didn't have to, as you've used the `CMD` directive when building your Dockerfile; thanks to this, Docker knows to automatically run the process `npm start` inside the container when it starts up.
    {% endcomment %}
    Also notice, you didn't specify what process you wanted your container to run. You didn't have to, as you've used the `CMD` directive when building your Dockerfile; thanks to this, Docker knows to automatically run the process `npm start` inside the container when it starts up.

{% comment %}
2.  Visit your application in a browser at `localhost:8000`. You should see your bulletin board application up and running. At this step, you would normally do everything you could to ensure your container works the way you expected; now would be the time to run unit tests, for example.
{% endcomment %}
2.  Visit your application in a browser at `localhost:8000`. You should see your bulletin board application up and running. At this step, you would normally do everything you could to ensure your container works the way you expected; now would be the time to run unit tests, for example.

{% comment %}
3.  Once you're satisfied that your bulletin board container works correctly, you can delete it:
{% endcomment %}
3.  Once you're satisfied that your bulletin board container works correctly, you can delete it:

    ```script
    docker rm --force bb
    ```

    The `--force` option removes the running container. If you stop the container running with `docker stop bb` you do not need to use `--force`.

{% comment %}
## Conclusion
{% endcomment %}
{: #conclusion }
## まとめ

{% comment %}
At this point, you've successfully built an image, performed a simple containerization of an application, and confirmed that your app runs successfully in its container. The next step will be to share your images on [Docker Hub](https://hub.docker.com/), so they can be easily downloaded and run on any destination machine.
{% endcomment %}
At this point, you've successfully built an image, performed a simple containerization of an application, and confirmed that your app runs successfully in its container. The next step will be to share your images on [Docker Hub](https://hub.docker.com/), so they can be easily downloaded and run on any destination machine.

{% comment %}
[On to Part 3 >>](part3.md){: class="button outline-btn" style="margin-bottom: 30px; margin-right: 100%"}
{% endcomment %}
[3 部へ >>](part3.md){: class="button outline-btn" style="margin-bottom: 30px; margin-right: 100%"}

{% comment %}
## CLI references
{% endcomment %}
{: #cli-references }
## CLI リファレンス

{% comment %}
Further documentation for all CLI commands used in this article are available here:
{% endcomment %}
Further documentation for all CLI commands used in this article are available here:

{% comment %}
- [docker image](https://docs.docker.com/engine/reference/commandline/image/)
- [docker container](https://docs.docker.com/engine/reference/commandline/container/)
- [Dockerfile reference](https://docs.docker.com/engine/reference/builder/)
{% endcomment %}
- [docker image]({{ site.baseurl }}/engine/reference/commandline/image/)
- [docker container]({{ site.baseurl }}/engine/reference/commandline/container/)
- [Dockerfile リファレンス]({{ site.baseurl }}/engine/reference/builder/)
