---
title: "Node イメージのビルド"
keywords: containers, images, node.js, node, dockerfiles, node, coding, build, push, run
description: Learn how to build your first Docker image by writing a Dockerfile
---

{% include_relative nav.html selected="1" %}

{% comment %}
## Prerequisites
{% endcomment %}
{: #prerequisites }
## 前提条件

{% comment %}
Work through the orientation and setup in Quickstart [Part 1](../index.md).
{% endcomment %}
クイックスタート [1 部](../index.md) の概要と設定をひととおり行っていること。

{% comment %}
## Overview
{% endcomment %}
{: #overview }
## 概要

{% comment %}
Now that we have a good overview of containers and the Docker platform, let’s take a look at building our first image. An image includes everything you need to run an application - the code or binary, runtime, dependencies, and any other file system objects required.
{% endcomment %}
ここまでにコンテナーと Docker プラットフォームについて概要をよく見てきましたので、イメージをはじめてビルドする方法を見ていきます。
イメージというものには、アプリケーションの実行に必要なものすべてが詰まっています。
それはコード、実行バイナリ、ランタイム、依存パッケージ、これ以外にも必要となるファイルシステムオブジェクトとさまざまです。

{% comment %}
To complete this tutorial, you need the following:
{% endcomment %}
このチュートリアルを終わりまで進めるには、以下が必要です。

{% comment %}
- Node.js version 12.18 or later. [Download Node.js](https://nodejs.org/en/){: target="_blank" class="_"}
- Docker running locally: Follow the instructions to [download and install Docker](https://docs.docker.com/desktop/).
- An IDE or a text editor to edit files. We recommend using Visual Studio Code.
{% endcomment %}
- Node.js バージョン 12.18 またはそれ以降。
  [Node.js をダウンロード](https://nodejs.org/en/){: target="_blank" class="_"} してください。
- ローカルに Docker が実行していること。
  [Docker のダウンロードとインストール](https://docs.docker.com/desktop/) の手順に従ってください。
- ファイル編集のための IDE またはテキストエディター。
  ここでは Visual Studio Code の利用をお勧めします。

{% comment %}
## Sample application
{% endcomment %}
{: #sample-application }
## サンプルアプリケーション

{% comment %}
Let’s create a simple Node.js application that we can use as our example. Create a directory on your local machine named `node-docker` and follow the steps below to create a simple REST API.
{% endcomment %}
利用例として用いる、簡単な Node.js アプリケーションを生成することにします。
ローカルマシン上に`node-docker`というディレクトリを生成し以下の手順を進めることで、単純な REST API を作ります。

{% comment %}
```shell
$ cd [path to your node-docker directory]
$ npm init -y
$ npm install ronin-server ronin-mocks
$ touch server.js
```
{% endcomment %}
```shell
$ cd [node-docker ディレクトリのパス]
$ npm init -y
$ npm install ronin-server ronin-mocks
$ touch server.js
```

{% comment %}
Now, let’s add some code to handle our REST requests. We’ll use a mock server so we can focus on Dockerizing the application.
{% endcomment %}
REST 要求を扱うコードを書いていきます。
サーバーは簡易なもので済ませて、アプリケーションの Docker 化に集中することにします。

{% comment %}
Open this working directory in your IDE and add the following code into the `server.js` file.
{% endcomment %}
IDE 上において上のワーキングディレクトリを開き、`server.js`ファイルに以下のコードを加えます。

```node
const ronin     = require( 'ronin-server' )
const mocks     = require( 'ronin-mocks' )

const server = ronin.server()

server.use( '/', mocks.server( server.Router(), false, true ) )
server.start()
```

{% comment %}
The mocking server is called `Ronin.js` and will listen on port 8000 by default. You can make POST requests to the root (/) endpoint and any JSON structure you send to the server will be saved in memory. You can also send GET requests to the same endpoint and receive an array of JSON objects that you have previously POSTed.
{% endcomment %}
この簡易サーバーを`Ronin.js`と呼ぶことにします。
これはデフォルトでポート 8000 を待ち受けます。
ルート（/）エンドポイントへの POST リクエストを送信するものとし、サーバーへ送信する JSON データはメモリ上に保存されるものとします。
ルートエンドポイントに GET リクエストも送信して、それ以前に POST した結果を JSON オブジェクト配列として受信します。

{% comment %}
## Test application
{% endcomment %}
{: #test-application }
## テストアプリケーション

{% comment %}
Let’s start our application and make sure it’s running properly. Open your terminal and navigate to your working directory you created.
{% endcomment %}
アプリケーションを実行して正しく動作することを確認します。
ターミナルを開いて、上で生成したワーキングディレクトリに移動します。

```shell
$ node server.js
```

{% comment %}
To test that the application is working properly, we’ll first POST some JSON to the API and then make a GET request to see that the data has been saved. Open a new terminal and run the following curl commands:
{% endcomment %}
アプリケーションの正常動作を確認するために、まずは API に対して JSON データを POST してみます。
そして GET リクエストを行って、データが保存されていることを確認します。
新たにターミナル画面を開いて、以下の curl コマンドを実行します。

```shell
$ curl --request POST \
  --url http://localhost:8000/test \
  --header 'content-type: application/json' \
  --data '{
	"msg": "testing"
}'
{"code":"success","payload":[{"msg":"testing","id":"31f23305-f5d0-4b4f-a16f-6f4c8ec93cf1","createDate":"2020-08-28T21:53:07.157Z"}]}

$ curl http://localhost:8000/test
{"code":"success","meta":{"total":1,"count":1},"payload":[{"msg":"testing","id":"31f23305-f5d0-4b4f-a16f-6f4c8ec93cf1","createDate":"2020-08-28T21:53:07.157Z"}]}
```

{% comment %}
Switch back to the terminal where our server is running. You should now see the following requests in the server logs.
{% endcomment %}
サーバーが実行されているターミナルに戻ります。
サーバーログとして以下のリクエストが表示されるはずです。

```node
2020-XX-31T16:35:08:4260  INFO: POST /test
2020-XX-31T16:35:21:3560  INFO: GET /test
```

{% comment %}
## Create a Dockerfile for Node.js
{% endcomment %}
{: #create-a-dockerfile-for-nodejs }
## Node.js 用の Dockerfile 生成

{% comment %}
A Dockerfile is a text document that contains all the commands a user could call on the command line to assemble an image. When we tell Docker to build our image by executing the `docker build` command, Docker reads these instructions and executes them one by one and creates a Docker image as a result.
{% endcomment %}
Dockerfile はテキストファイルであり、この中にはコマンドライン上で実行するようなコマンドがさまざまに記述され、それがイメージ生成に結びつきます。
Docker に対してイメージ生成の指示を行うために`docker build`コマンドを実行すると、Docker は Dockerfile 内の命令を読み取って、1 つずつ実行していき、結果として Docker イメージを作り出します。

{% comment %}
Let’s walk through the process of creating a Dockerfile for our application. In the root of your working directory, create a file named `Dockerfile` and open this file in your text editor.
{% endcomment %}
アプリケーションに対しての Dockerfile を作成する手順を見ていきます。
ワーキングディレクトリのルートにおいて、`Dockerfile`という名前のファイルを生成して、テキストエディター内でこれを開きます。

{% comment %}
> **Note**
>
> The name of the Dockerfile is not important but the default filename for many commands is simply `Dockerfile`. So, we’ll use that as our filename throughout this series.
{% endcomment %}
> **メモ**
>
> Dockerfile という名前は重要ではありません。
> ただしデフォルトのファイル名を`Dockerfile`とするコマンドが数多くあります。
> したがってここでの説明では、このファイル名を用いていくことにします。

{% comment %}
The first thing we need to do is to add a line in our Dockerfile that tells Docker what base image we would like to use for our application.
{% endcomment %}
まずはじめに行うのは、Docker に対してアプリケーションのベースイメージとして何を使うのか、これを Docker に対して指示する行を Dockerfile に記述します。

```dockerfile
FROM node:12.18.1
```

{% comment %}
Docker images can be inherited from other images. Therefore, instead of creating our own base image, we’ll use the official Node.js image that already has all the tools and packages that we need to run a Node.js application. You can think of this in the same way you would think about class inheritance in object oriented programming. For example, if we were able to create Docker images in JavaScript, we might write something like the following.
{% endcomment %}
Docker イメージは、別のイメージを継承して生成することができます。
したがってここでは独自のベースイメージを作るのでなく、公式の Node.js イメージを用いることにします。
このイメージの中には Node.js アプリケーションの実行に必要となるツールやパッケージがすべて含まれて居ます。
この様子は、オブジェクト指向プログラミングにおけるクラス継承と同じように考えることができます。
もし仮に Docker イメージが JavaScript でプログラミングできるとしたら、以下のような書き方を行うことになります。

`class MyImage extends NodeBaseImage {}`

{% comment %}
This would create a class called `MyImage` that inherited functionality from the base class `NodeBaseImage`.
{% endcomment %}
ここでは`MyImage`というクラスが生成されて、基底クラス`NodeBaseImage`の機能を継承することになりますｓ。

{% comment %}
In the same way, when we use the `FROM` command, we tell Docker to include in our image all the functionality from the `node:12.18.1` image.
{% endcomment %}
同じような話として、上に示した`FROM`コマンドの利用の際には、`node:12.18.1`イメージからすべての機能性を受け継ぐように Docker に指示を出していることになります。

{% comment %}
> **Note**
>
> If you want to learn more about creating your own base images, see [Creating base images](https://docs.docker.com/develop/develop-images/baseimages/).
{% endcomment %}
> **メモ**
>
> 独自のベースイメージを生成する方法に関しては、[ベースイメージの生成]({{ site.baseurl }}/develop/develop-images/baseimages/) を参照してください。

{% comment %}
The `NODE_ENV` environment variable specifies the environment in which an application is running (usually, development or production). One of the simplest things you can do to improve performance is to set `NODE_ENV` to `production`.
{% endcomment %}
環境変数`NODE_ENV`は、アプリケーションを実行する環境（通常は開発環境(development)か本番環境(production)）を指定します。
性能の改善を図りたいなら、最も単純な話としてこの`NODE_ENV`を`production`に設定すればよいことになります。

```dockerfile
ENV NODE_ENV=production
```

{% comment %}
To make things easier when running the rest of our commands, let’s create a working directory. This instructs Docker to use this path as the default location for all subsequent commands. This way we do not have to type out full file paths but can use relative paths based on the working directory.
{% endcomment %}
これ以降のコマンド実行にあたって、作業をしやすくするため、ワーキングディレクトリを生成することにします。
つまりこの後に続くコマンドのデフォルトディレクトリを、特定のパスとすることを Docker に指示します。
こうすればフルパスを記述する必要がなくなり、すべてはワーキングディレクトリからの相対パスとすることができます。

```dockerfile
WORKDIR /app
```

{% comment %}
Usually the very first thing you do once you’ve downloaded a project written in Node.js is to install npm packages. This will ensure that your application has all its dependencies installed into the `node_modules` directory where the Node runtime will be able to find them.
{% endcomment %}
通常は、Node.js で開発されているプロジェクトをダウンロードした際には、直後に npm パッケージをインストールすることが必要です。
これを行えば、アプリケーションの依存パッケージはすべて`node_modules`ディレクトリにインストールされることになり、Node ランタイムがこれを見つけ出せることになります。

{% comment %}
Before we can run `npm install`, we need to get our `package.json` and `package-lock.json` files into our images. We use the `COPY` command to do this. The  `COPY` command takes two parameters. The first parameter tells Docker what file(s) you would like to copy into the image. The second parameter tells Docker where you want that file(s) to be copied to. We’ll copy the `package.json` and `package-lock.json` file into our working directory `/app`.
{% endcomment %}
`npm install`を実行する前に、イメージ内に`package.json`ファイルと`package-lock.json`ファイルを生成しておくことが必要です。
`COPY`を使ってこれを行います。
`COPY`コマンドは 2 つのパラメーターをとります。
1 つめは、イメージ内にコピーしたいファイルを指定します。
2 つめは、コピー先としたいファイルを指定します。
ここでは`package.json`と`package-lock.json`をワーキングディレクトリ`/app`にコピーします。

```dockerfile
COPY ["package.json", "package-lock.json*", "./"]
```

{% comment %}
Once we have our `package.json` files inside the image, we can use the `RUN` command to execute the command npm install. This works exactly the same as if we were running npm install locally on our machine, but this time these Node modules will be installed into the `node_modules` directory inside our image.
{% endcomment %}
`package.json`ファイルをイメージ内に置いたら、`RUN`コマンドを使って npm install を実行します。
これはローカルマシン上において npm install を実行することと全く同じです。
ただし今の場合、Node モジュールはイメージ内部にある`node_modules`ディレクトリにインストールされます。

```dockerfile
RUN npm install --production
```

{% comment %}
At this point, we have an image that is based on node version 12.18.1 and we have installed our dependencies. The next thing we need to do is to add our source code into the image. We’ll use the COPY command just like we did with our `package.json` files above.
{% endcomment %}
利用しているイメージは node バージョン 12.18.1 に基づくものであり、この時点において対応する依存パッケージをインストールしました。
次に行うのは、ソースコードをイメージ内に置くことです。
COPY コマンドを使って、上の`package.json`ファイルのコピーと同様の処理を行います。

```dockerfile
COPY . .
```

{% comment %}
The COPY command takes all the files located in the current directory and copies them into the image. Now, all we have to do is to tell Docker what command we want to run when our image is run inside of a container. We do this with the `CMD` command.
{% endcomment %}
ここでの COPY コマンドは、カレントディレクトリにあるファイルをすべて、イメージ内にコピーします。
そこで最後になすべきことは、コンテナー内部においてイメージが実行されたときに、どのようなコマンドを実行するかを Docker に指示することです。

```dockerfile
CMD [ "node", "server.js" ]
```

{% comment %}
Here's the complete Dockerfile.
{% endcomment %}
こうしてできあがった Dockerfile が以下です。

```dockerfile
FROM node:12.18.1
ENV NODE_ENV=production

WORKDIR /app

COPY ["package.json", "package-lock.json*", "./"]

RUN npm install

COPY . .

CMD [ "node", "server.js" ]
```

{% comment %}
## Build image
{% endcomment %}
{: #build-image }
## イメージのビルド

{% comment %}
Now that we’ve created our Dockerfile, let’s build our image. To do this, we use the `docker build` command. The `docker build` command builds Docker images from a Dockerfile and a “context”. A build’s context is the set of files located in the specified PATH or URL. The Docker build process can access any of the files located in the context.
{% endcomment %}
Dockerfile が生成できたので、イメージをビルドします。
イメージビルドには`docker build`コマンドを使います。
`docker build`コマンドは Dockerfile から Docker イメージをビルドし、さらに「コンテキスト」をビルドします。
ビルドコンテキストとは、指定されるパスや URL 内にある一連のファイルのことです。
Docker のビルド処理においてコンテキスト内にあるファイルは、すべてアクセスすることができます。

{% comment %}
The build command optionally takes a `--tag` flag. The tag is used to set the name of the image and an optional tag in the format `‘name:tag’`. We’ll leave off the optional “tag” for now to help simplify things. If you do not pass a tag, Docker will use “latest” as its default tag. You’ll see this in the last line of the build output.
{% endcomment %}
ビルドコマンドでは`--tag`フラグを指定することがあります。
このタグは、イメージ名に対して任意につけるタグというものを、`‘name:tag’`という書式により指定します。
今のところは話を単純にするために、この任意の「タグ」は用いないことにします。
タグを指定しない場合、Docker はデフォルトのタグとして「latest」というものを用います。
このことはビルド処理の出力結果における最終行に示されます。

{% comment %}
Let’s build our first Docker image.
{% endcomment %}
でははじめての Docker イメージビルドを行います。

```shell
$ docker build --tag node-docker .
```

```shell
Sending build context to Docker daemon  82.94kB
Step 1/7 : FROM node:12.18.1
---> f5be1883c8e0
Step 2/7 : WORKDIR /code
...
Successfully built e03018e56163
Successfully tagged node-docker:latest
Viewing Local Images
To see a list of images we have on our local machine, we have two options. One is to use the CLI and the other is to use Docker Desktop. Since we are currently working in the terminal let’s take a look at listing images with the CLI.
```

{% comment %}
To list images, simply run the `images` command.
{% endcomment %}
イメージの一覧を確認するには、単純に`images`コマンドを実行します。

```shell
$ docker images
REPOSITORY          TAG                 IMAGE ID            CREATED              SIZE
node-docker         latest              3809733582bc        About a minute ago   945MB
node                12.18.1             f5be1883c8e0        2 months ago         918MB
```

{% comment %}
You should see at least two images listed. One for the base image `node:12.18.1` and the other for our image we just build `node-docker:latest`.
{% endcomment %}
一覧には少なくとも 2 つのイメージがあるはずです。
1 つはベースイメージ`node:12.18.1`に対するものであり、もう 1 つはまさにビルドした`node-docker:latest`イメージに対するものです。

{% comment %}
## Tag images
{% endcomment %}
{: #tag-images }
## イメージのタグづけ

{% comment %}
An image name is made up of slash-separated name components. Name components may contain lowercase letters, digits and separators. A separator is defined as a period, one or two underscores, or one or more dashes. A name component may not start or end with a separator.
{% endcomment %}
イメージ名は、スラッシュで区切られた要素により構成されます。
要素に含めることができるのは、英小文字、数字と、区切り文字です。
区切り文字とは、ピリオド、アンダースコア 1 つまたは 2 つ、1つまたは複数のダッシュです。
各要素の先頭をセパレーターで始めることはできません。

{% comment %}
An image is made up of a manifest and a list of layers. In simple terms, a “tag” points to a combination of these artifacts. You can have multiple tags for an image. Let’s create a second tag for the image we built and take a look at its layers.
{% endcomment %}
イメージは、マニフェストとレイヤーのリストによって構成されます。
単純に表現すると「タグ」というのは、これらの成果物を組み合わせたものです。
イメージに対しては複数のタグを定めることができます。
そこでイメージに対して 2 つめのタグを作り出して、そのときのレイヤーを見てみることにします。

{% comment %}
To create a new tag for the image we built above, run the following command.
{% endcomment %}
これまでにビルドしたイメージに対して新たなタグを生成するには、以下のコマンドを実行します。

```shell
$ docker tag node-docker:latest node-docker:v1.0.0
```

{% comment %}
The Docker tag command creates a new tag for an image. It does not create a new image. The tag points to the same image and is just another way to reference the image.
{% endcomment %}
Docker のタグコマンドは、イメージに対して新たなタグを生成します。
これは新たなイメージを生成するものではありません。
タグは同一のイメージを参照するものであり、イメージを別の方法によって参照するものです。

{% comment %}
Now run the `docker images` command to see a list of our local images.
{% endcomment %}
ここで`docker images`コマンドを実行して、ローカルイメージの一覧を確認します。

```
$ docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
node-docker         latest              3809733582bc        24 minutes ago      945MB
node-docker         v1.0.0              3809733582bc        24 minutes ago      945MB
node                12.18.1             f5be1883c8e0        2 months ago        918MB
```

{% comment %}
You can see that we have two images that start with `node-docker`. We know they are the same image because if you look at the IMAGE ID column, you can see that the values are the same for the two images.
{% endcomment %}
`node-docker`で始まるイメージが 2 つあります。
これらは同一イメージであることがすぐにわかります。
IMAGE ID カラムを見てみれば、この 2 つのイメージに対する値は同一になっています。

{% comment %}
Let’s remove the tag that we just created. To do this, we’ll use the rmi command. The rmi command stands for “remove image”.
{% endcomment %}
生成したばかりのタグを削除してみます。
これを行うには rmi コマンドを使います。
rmi は「remove image」（イメージの削除）を意味しています。

```shell
$ docker rmi node-docker:v1.0.0
Untagged: node-docker:v1.0.0
```

{% comment %}
Notice that the response from Docker tells us that the image has not been removed but only “untagged”. Verify this by running the images command.
{% endcomment %}
コマンド実行結果からわかることですが、イメージは削除されたわけではなく「untagged」（タグづけが解除）になったということです。
このことを images コマンドを実行して確認します。

```shell
$ docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
node-docker         latest              3809733582bc        32 minutes ago      945MB
node                12.18.1             f5be1883c8e0        2 months ago        918MB
```

{% comment %}
Our image that was tagged with `:v1.0.0` has been removed but we still have the `node-docker:latest` tag available on our machine.
{% endcomment %}
`:v1.0.0`としてタグづけしたイメージは一覧からなくなりました。
しかし`node-docker:latest`タグはまだ残っていて、マシンから利用可能な状態になっています。

{% comment %}
## Conclusion
{% endcomment %}
{: #conclusion }
## まとめ

{% comment %}
In this module, we took a look at setting up our example Node application that we will use for the rest of the tutorial. We also created a Dockerfile that we used to build our Docker image. Then, we took a look at tagging our images and removing images.
{% endcomment %}
ここで Node アプリケーションの例を設定する方法を見てきました。
これはこの後に続くチュートリアルでも利用していきます。
Docker イメージをビルドするために Dockerfile を生成し、イメージへのタグづけとその解除を行ってみました。

{% comment %}
In the next module, we’ll take a look at [running containers](run-containers.md).
{% endcomment %}
次のチュートリアルでは [コンテナーの実行](run-containers.md) について見ていきます。
