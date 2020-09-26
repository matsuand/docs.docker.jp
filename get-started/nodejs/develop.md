---
title: "開発向けのコンテナー利用"
keywords: get started, NodeJS, local, development
description: Learn how to develop your application locally.
---

{% include_relative nav.html selected="3" %}

{% comment %}
## Prerequisites
{% endcomment %}
{: #prerequisites }
## 前提条件

{% comment %}
Work through the steps to build an image and run it as a containerized application in [Run your image as a container](run-containers.md).
{% endcomment %}
[イメージのコンテナーとしての実行](run-containers.md) を通じて、コンテナー化したアプリケーションであるイメージのビルドと実行を行っていること。

{% comment %}
## Introduction
{% endcomment %}
{: #introduction }
## はじめに

{% comment %}
In this module, we’ll walk through setting up a local development environment for the application we built in the previous modules. We’ll use Docker to build our images and Docker Compose to make everything a whole lot easier.
{% endcomment %}
このチュートリアルでは、その前で作ったアプリケーションの開発環境をローカルに構築する手順を進めていきます。
Docker を使ってイメージをビルドするとともに Docker Compose を使って、あらゆるものを簡単に取り扱っていきます。

{% comment %}
## Use Compose to develop locally
{% endcomment %}
{: #use-compose-to-develop-locally }
## Compose を使ったローカル開発

{% comment %}
The notes-service project uses MongoDB as its data store. If you remember from Part I of this series, we had to start the Mongo container manually and connect it to the same network that our notes-service is running on. We also had to create a couple of volumes so we could persist our data across restarts of our application and MongoDB.
{% endcomment %}
notes-service プロジェクトは、データストアとして MongoDB を利用します。
本チュートリアルの 1 部からわかるように、Mongo コンテナーは手動で起動して、notes-service が起動している同一のネットワークに接続しなければなりません。
またボリュームをいくつか生成してデータ保存を行います。
こうすることでアプリケーションや MongoDB の再起動を行っても、データが失われないようにします。

{% comment %}
In this section, we’ll create a Compose file to start our node-docker and the MongoDB with one command. We’ll also set up the Compose file to start the node-docker in debug mode so that we can connect a debugger to the running node process.
{% endcomment %}
本節では Compose ファイルを生成して node-docker と MongoDB を 1 つのコマンドで起動していきます。
さらに Compose ファイルにおいては node-docker をデバッグモードで起動するように設定して、実行中のノードプロセスにデバッガーから接続します。

{% comment %}
Open the notes-service in your IDE or text editor and create a new file named `docker-compose.dev.yml`. Copy and paste the below commands into the file.
{% endcomment %}
IDE またはテキストエディターにより notes-service を開いて、`docker-compose.dev.yml`という新規ファイルを生成します。
そしてそのファイルに以下のコードをコピーペーストしてください。

```yaml
version: '3.8'

services:
 notes:
   build:
     context: .
   ports:
     - 8080:8080
     - 9229:9229
   environment:
     - SERVER_PORT=8080
     - DATABASE_CONNECTIONSTRING=mongodb://mongo:27017/notes
   volumes:
     - ./:/code
   command: npm run debug

 mongo:
   image: mongo:4.2.8
   ports:
     - 27017:27017
   volumes:
     - mongodb:/data/db
     - mongodb_config:/data/configdb
 volumes:
   mongodb:
   mongodb_config:
```

{% comment %}
This Compose file is super convenient as we do not have to type all the parameters to pass to the `docker run` command. We can declaratively do that in the Compose file.
{% endcomment %}
この Compose ファイルはとても優れていて、`docker run`コマンドにパラメーターを入力する面倒をなくしてくれます。
Compose ファイル内では、すべてを宣言的に記述していきます。

{% comment %}
We are exposing port 9229 so that we can attach a debugger. We are also mapping our local source code into the running container so that we can make changes in our text editor and have those changes picked up in the container.
{% endcomment %}
デバッガーをアタッチできるように、ポート 9229 を公開します。
さらにローカルにあるソースコードを実行中のコンテナーにマッピングして、テキストエディター上でのソース変更を可能とし、さらにその変更をコンテナーが察知できるようにします。

{% comment %}
One other really cool feature of using a Compose file, is that we have service resolution set up to use the service names. So we are now able to use `“mongo”` in our connection string. The reason we use mongo is because that is what we have named our mongo service in the Compose file as.
{% endcomment %}
Compose ファイルを利用する際の優れた機能として、サービスの名前を使っただけでサービス指定ができる点が挙げられます。
そこで指定文字として「mongo」を利用します。
mongo という文字を使うのは、Compose ファイルにおいて mongo サービスに対して、そのように命名したからです。

{% comment %}
Let’s start our application and confirm that it is running properly.
{% endcomment %}
アプリケーションを起動して、正常に動作することを確認します。

```shell
$ docker-compose -f docker-compose.dev.yml up --build
```

{% comment %}
We pass the `--build` flag so Docker will compile our image and then starts it.
{% endcomment %}
ここで`--build`フラグを与えています。
これは Docker がイメージをコンパイルした上で起動するようにします。

{% comment %}
If all goes will you should see something similar:
{% endcomment %}
うまく動作すれば、以下と同様に表示されます。

  ![node-compile](images/node-compile.png)

{% comment %}
Now let’s test our API endpoint. Run the following curl command:
{% endcomment %}
API エンドポイントをテストしてみます。
以下の curl コマンドを実行してください。

```shell
$ curl --request GET --url http://localhost:8080/services/m/notes
```

{% comment %}
You should receive the following response:
{% endcomment %}
以下のようなレスポンスが返ってくるはずです。

```json
{"code":"success","meta":{"total":0,"count":0},"payload":[]}
```

{% comment %}
## Connect a debugger
{% endcomment %}
{: #connect-a-debugger }
## デバッガーへの接続

{% comment %}
We’ll use the debugger that comes with the Chrome browser. Open Chrome on your machine and then type the following into the address bar.
{% endcomment %}
ここで Chrome ブラウザーに付属しているデバッガーを利用することにします。
マシン上において Chrome ブラウザーを開き、アドレスバーに以下を入力します。

`about:inspect`

{% comment %}
It opens the following screen.
{% endcomment %}
以下の画面が開きます。

  ![Chrome-inspect](images/chrome-inspect.png)

{% comment %}
Click the **Open dedicated DevTools for Node** link. This opens the DevTools that are connected to the running Node.js process inside our container.
{% endcomment %}
**Open dedicated DevTools for Node**（Node 専用の DevTools を開く）リンクをクリックします。
これにより DevTools が開きます。
このツールは、コンテナー内部で稼動している Node.js プロセスに接続されています。

{% comment %}
Let’s change the source code and then set a breakpoint.
{% endcomment %}
ソースコードを変更し、ブレークポイントを設定してみます。

{% comment %}
Add the following code to the server.js file on line 19 and save the file.
{% endcomment %}
server.js ファイルの 19 行めに以下のコードを追加して、ファイルを保存します。

```node
 server.use( '/foo', (req, res) => {
   return res.json({ "foo": "bar" })
 })
```

{% comment %}
If you take a look at the terminal where our Compose application is running, you’ll see that nodemon noticed the changes and reloaded our application.
{% endcomment %}
Compose アプリケーションを実行しているターミナルを確認してみると、nodemon が変更を検出して、アプリケーションをリロードしているのがわかります。

 ![nodemon](images/nodemon.png)

{% comment %}
Navigate back to the Chrome DevTools and set a breakpoint on line 20 and then run the following curl command to trigger the breakpoint.
{% endcomment %}
Chrome のDevTools 画面に戻って、20 行目にブレークポイントを設定します。
そして以下の curl コマンドを実行してブレークポイントまで進めます。

```shell
$ curl --request GET --url http://localhost:8080/foo
```

{% comment %}
You should have seen the code break on line 20 and now you are able to use the debugger just like you would normally. You can inspect and watch variables, set conditional breakpoints, view stack traces, etc.
{% endcomment %}
コードの 20 行目でブレークされたはずです。
ここから普段使っている方法でデバッガーを操作できるようになります。
変数を詳細に確認しウォッチすることや条件つきブレークポイントの設定、スタックトレースの確認などを行うことができます。

{% comment %}
## Conclusion
{% endcomment %}
{: #conclusion }
## まとめ

{% comment %}
In this article, we took a look at creating a general development image that we can use pretty much like our normal command line. We also set up our Compose file to map our source code into the running container and exposed the debugging port.
{% endcomment %}
本稿を通じて、汎用的な開発イメージの生成方法を見てきました。
それは普通のコマンドラインから実行することと全く同じことです。
Compose ファイルを生成して、ソースコードを実行コンテナー内にマッピングし、デバッグ用のポート公開も行いました。
