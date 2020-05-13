---
title: "Docker Hub におけるイメージ共有"
keywords: docker hub, push, images
description: Learn how to share images on Docker Hub.
redirect_from:
- /get-started/part5/
---

{% include_relative nav.html selected="3" %}

{% comment %}
## Prerequisites
{% endcomment %}
{: #prerequisites }
## 前提条件

{% comment %}
Work through the steps to build an image and run it as a containerized application in [Part 2](part2.md).
{% endcomment %}
[2 部](part2.md) にてイメージをビルドし、コンテナー化アプリケーションとして実行できていること。

{% comment %}
## Introduction
{% endcomment %}
{: #introduction }
## はじめに

{% comment %}
At this point, you've built a containerized application in [Part 2](part2.md) on your local development machine, thanks to Docker Desktop. The final step in developing a containerized application is to share your images on a registry like [Docker Hub](https://hub.docker.com/), so they can be easily downloaded and run on any destination machine.
{% endcomment %}
ここでは [2 部](part2.md) での作業を通じて、ローカル開発マシン上にコンテナー化されたアプリケーションが構築できているものとします。
最後の作業として、開発しているコンテナー化アプリケーションを、[Docker Hub](https://hub.docker.com/) のようなレジストリ上にてイメージを共有していきます。
こうすればイメージを簡単にダウンロードすることができ、目的とするマシンのいずれにおいても動作させることができます。


{% comment %}
## Set up your Docker Hub account
{% endcomment %}
{: #set-up-your-docker-hub-account }
## Docker Hub アカウントの設定

{% comment %}
If you don't yet have a Docker ID, follow these steps to set one up; this will allow you to share images on Docker Hub.
{% endcomment %}
まだ Docker ID を持っていない場合は、以下の手順により取得します。
これを行っておくことで Docker Hub 上でのイメージの共有ができるようになります。

{% comment %}
1.  Visit the Docker Hub sign up page, [https://hub.docker.com/signup](https://hub.docker.com/signup).
{% endcomment %}
1.  Docker Hub のサインアップページ [https://hub.docker.com/signup](https://hub.docker.com/signup) にアクセスします。

{% comment %}
2.  Fill out the form and submit to create your Docker ID.
{% endcomment %}
2.  入力欄への入力を行って submit ボタンをクリックし Docker ID を生成します。

{% comment %}
3.  Verify your email address to complete the registration process.
{% endcomment %}
3.  電子メールアドレスを確認し、登録手順を終了します。

{% comment %}
4.  Click on the Docker icon in your toolbar or system tray, and click **Sign in / Create Docker ID**.
{% endcomment %}
4.  ツールバーあるいはシステムトレイにある Docker アイコンをクリックして **Sign in / Create Docker ID** を実行します。

{% comment %}
5.  Fill in your new Docker ID and password. After you have successfully authenticated, your Docker ID appears in the Docker Desktop menu in place of the 'Sign in' option you just used.
{% endcomment %}
5.  Docker ID とパスワードを入力します。
    認証が正しく行われると、先ほど実行した Docker Desktop メニューの 'Sign in' のところに Docker ID が表示されます。

    {% comment %}
    > You can do the same thing from the command line by typing `docker login`.
    {% endcomment %}
    > 同じことはコマンドラインから `docker login` を入力して行うこともできます。

{% comment %}
## Create a Docker Hub repository and push your image
{% endcomment %}
{: #create-a-docker-hub-repository-and-push-your-image }
## Docker Hub リポジトリの生成とイメージプッシュ

{% comment %}
At this point, you've set up your Docker Hub account and have connected it to your Docker Desktop. Now let's make our first repo, and share our bulletin board app there.
{% endcomment %}
ここまでに Docker Hub アカウントを設定し、Docker Desktop に接続しました。
それでは初めてのリポジトリを生成し、掲示板アプリを共有しましょう。

{% comment %}
1.  Click on the Docker icon in your menu bar, and navigate to **Repositories > Create**. You'll be taken to a Docker Hub page to create a new repository.
{% endcomment %}
1.  メニューバー上の Docker アイコンをクリックして **Repositories > Create** を実行します。
    Docker Hub ページが開くので、新たなリポジトリを生成していきます。

{% comment %}
2.  Fill out the repository name as `bulletinboard`. Leave all the other options alone for now, and click **Create** at the bottom.
{% endcomment %}
2.  リポジトリ名に `bulletinboard` と入力します。
    これ以外の欄は、ここではとりあえずそのままとします。
    そして下段にある **Create** をクリックします。

    {% comment %}
    ![make a repo](images/newrepo.png){:width="100%"}
    {% endcomment %}
    ![リポジトリの生成](images/newrepo.png){:width="100%"}

{% comment %}
3.  Now you are ready to share your image on Docker Hub, but there's one thing you must do first: images must be *namespaced correctly* to share on Docker Hub. Specifically, you must name images like `<Docker ID>/<Repository Name>:<tag>`. You can relabel your `bulletinboard:1.0` image like this (of course, please replace `gordon` with your Docker ID):
{% endcomment %}
3.  Docker Hub 上においてイメージを共有できる状態になりました。
    ただし共有する前に、まずやっておくべきことがあります。
    Docker Hub 上にイメージを共有するには、イメージの名前空間を適切に設定しておかなければなりません。
    具体的には、イメージ名を `<Docker ID>/<リポジトリ名>:<タグ>` とする必要があります。
    つまり `bulletinboard:1.0` イメージは、以下のようにしてリネームします。
    (もちろん `gordon` の部分は、自分の Docker ID に代えてください。)

    ```shell
    docker tag bulletinboard:1.0 gordon/bulletinboard:1.0
    ```

{% comment %}
4.  Finally, push your image to Docker Hub:
{% endcomment %}
4.  最後にイメージを Docker Hub にプッシュします。

    ```shell
    docker push gordon/bulletinboard:1.0
    ```

    {% comment %}
    Visit your repository in Docker Hub, and you'll see your new image there. Remember, Docker Hub repositories are public by default.
    {% endcomment %}
    Docker Hub においてこのリポジトリにアクセスしてみると、新たなイメージがそこに表示されています。
    Docker Hub リポジトリは、デフォルトで公開されている（public となっている）ことに注意してください。

    {% comment %}
    > **Having trouble pushing?** Remember, you must be signed in to Docker Hub through Docker Desktop or the command line, and you must also name your images correctly, as per the above steps. If the push seemed to work, but you don't see it in Docker Hub, refresh your browser after a couple of minutes and check again.
    {% endcomment %}
    > **プッシュに失敗した場合** Docker Hub へは Docker Desktop から、あるいはコマンドラインからサインインしていることが必要です。
    > また上記の手順にて示したように、イメージは適切な名称としていなければなりません。
    > プッシュを行っても Docker Hub 上に表示されていない場合は、数分待ってからブラウザーを再表示して確認してください。

{% comment %}
## Conclusion
{% endcomment %}
{: #conclusion }
## まとめ

{% comment %}
Now that your image is available on Docker Hub, you'll be able to run it anywhere. If you try to use it on a new machine that doesn't have it yet, Docker will automatically try and download it from Docker Hub. By moving images around in this way, you no longer need to install any dependencies except Docker on the machines you want to run your software on. The dependencies of containerized applications are completely encapsulated and isolated within your images, which you can share using Docker Hub as described above.
{% endcomment %}
Now that your image is available on Docker Hub, you'll be able to run it anywhere. If you try to use it on a new machine that doesn't have it yet, Docker will automatically try and download it from Docker Hub. By moving images around in this way, you no longer need to install any dependencies except Docker on the machines you want to run your software on. The dependencies of containerized applications are completely encapsulated and isolated within your images, which you can share using Docker Hub as described above.

{% comment %}
Another thing to keep in mind: at the moment, you've only pushed your image to Docker Hub; what about your Dockerfile? A crucial best practice is to keep these in version control, perhaps alongside your source code for your application. You can add a link or note in your Docker Hub repository description indicating where these files can be found, preserving the record not only of how your image was built, but how it's meant to be run as a full application.
{% endcomment %}
Another thing to keep in mind: at the moment, you've only pushed your image to Docker Hub; what about your Dockerfile? A crucial best practice is to keep these in version control, perhaps alongside your source code for your application. You can add a link or note in your Docker Hub repository description indicating where these files can be found, preserving the record not only of how your image was built, but how it's meant to be run as a full application.

{% comment %}
## Where to go next
{% endcomment %}
{: #where-to-go-next }
## 次に読むものは

{% comment %}
We recommend that you take a look at the topics in [Develop with Docker](../develop/index.md) to learn how to develop your own applications using Docker.
{% endcomment %}
We recommend that you take a look at the topics in [Develop with Docker](../develop/index.md) to learn how to develop your own applications using Docker.
