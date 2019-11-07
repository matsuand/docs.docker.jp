---
description: ベースイメージを生成する方法。
keywords: images, base image, examples
redirect_from:
- /engine/articles/baseimages/
- /engine/userguide/eng-image/baseimages/
title: ベースイメージの生成
---

{% comment %}
Most Dockerfiles start from a parent image. If you need to completely control
the contents of your image, you might need to create a base image instead.
Here's the difference:
{% endcomment %}
Dockerfile は普通は親イメージから作り始めます。
イメージ内容を完全にコントロールする場合は、ベースイメージを作り出すこともあります。
その違いは以下のとおりです。

{% comment %}
- A [parent image](/glossary.md#parent_image) is the image that your
  image is based on. It refers to the contents of the `FROM` directive in the
  Dockerfile. Each subsequent declaration in the Dockerfile modifies this parent
  image. Most Dockerfiles start from a parent image, rather than a base image.
  However, the terms are sometimes used interchangeably.
{% endcomment %}
- [親イメージ](/glossary.md#parent_image) は基準とするイメージのことです。
  Dockerfile 内の `FROM` ディレクティブによって指定されます。
  Dockerfile 内のこれに続く定義は、その親イメージを修正指示するものとなります。
  Dockerfile は普通は親イメージから作り始め、ベースイメージから作るのはまれです。
  ただしこの用語は混同されて用いられることもあります。

{% comment %}
- A [base image](/glossary.md#base_image) has `FROM scratch` in its Dockerfile.
{% endcomment %}
- [ベースイメージ](/glossary.md#base_image) は Dockerfile において `FROM scratch` に記述されるものです。

{% comment %}
This topic shows you several ways to create a base image. The specific process
will depend heavily on the Linux distribution you want to package. We have some
examples below, and you are encouraged to submit pull requests to contribute new
ones.
{% endcomment %}
ここではベースイメージの生成方法をいくつか示します。
パッケージ化しようとしている Linux ディストリビューションに大きく依存する処理操作もあります。
以下に例をあげていきます。
新たなイメージを提供して頂ける場合は、プルリクエストをあげることをお願いします。

{% comment %}
## Create a full image using tar
{% endcomment %}
## tar を使ったフルイメージの生成
{: #create-a-full-image-using-tar }

{% comment %}
In general, start with a working machine that is running
the distribution you'd like to package as a parent image, though that is
not required for some tools like Debian's
[Debootstrap](https://wiki.debian.org/Debootstrap), which you can also
use to build Ubuntu images.
{% endcomment %}
通常であれば、作業マシン上に稼動するディストリビューションを使い、これを親イメージとしてパッケージにしていくことになります。
しかし Debian の [Debootstrap](https://wiki.debian.org/Debootstrap) のようなツールを使えば作業マシンは不要であり、ここから Ubuntu イメージを作ることもできます。

{% comment %}
It can be as simple as this to create an Ubuntu parent image:
{% endcomment %}
Ubuntu の親イメージを作るのは、以下のように簡単にできます。

    $ sudo debootstrap xenial xenial > /dev/null
    $ sudo tar -C xenial -c . | docker import - xenial

    a29c15f1bf7a

    $ docker run xenial cat /etc/lsb-release

    DISTRIB_ID=Ubuntu
    DISTRIB_RELEASE=16.04
    DISTRIB_CODENAME=xenial
    DISTRIB_DESCRIPTION="Ubuntu 16.04 LTS"

{% comment %}
There are more example scripts for creating parent images in the Docker
GitHub Repo:
{% endcomment %}
Docker GitHub レポジトリには、親イメージを生成するスクリプトの例がいろいろとあります。

{% comment %}
 - [BusyBox](https://github.com/moby/moby/blob/master/contrib/mkimage/busybox-static)
 - CentOS / Scientific Linux CERN (SLC) [on Debian/Ubuntu](
   https://github.com/moby/moby/blob/master/contrib/mkimage/rinse) or
   [on CentOS/RHEL/SLC/etc.](
   https://github.com/moby/moby/blob/master/contrib/mkimage-yum.sh)
 - [Debian / Ubuntu](
   https://github.com/moby/moby/blob/master/contrib/mkimage/debootstrap)
{% endcomment %}
 - [BusyBox](https://github.com/moby/moby/blob/master/contrib/mkimage/busybox-static)
 - CentOS / Scientific Linux CERN (SLC)（[Debian/Ubuntu 向け](https://github.com/moby/moby/blob/master/contrib/mkimage/rinse)、または[CentOS/RHEL/SLC/など向け](https://github.com/moby/moby/blob/master/contrib/mkimage-yum.sh)）
 - [Debian / Ubuntu](
   https://github.com/moby/moby/blob/master/contrib/mkimage/debootstrap)

{% comment %}
## Create a simple parent image using scratch
{% endcomment %}
## 単純な親イメージを一から生成
{: #create-a-simple-parent-image-using-scratch }

{% comment %}
You can use Docker's reserved, minimal image, `scratch`, as a starting point for
building containers. Using the `scratch` "image" signals to the build process
that you want the next command in the `Dockerfile` to be the first filesystem
layer in your image.
{% endcomment %}
Docker が規定する最小イメージ `scratch` は、コンテナーを構築するベースイメージとして利用できます。
`scratch` を利用すると「イメージ」は、`Dockerfile`内の次に実行したいコマンドの構築プロセスに対して、最初のファイルシステムレイヤーとなるように指示を出します。

{% comment %}
While `scratch` appears in Docker's repository on the hub, you can't pull it,
run it, or tag any image with the name `scratch`. Instead, you can refer to it
in your `Dockerfile`. For example, to create a minimal container using
`scratch`:
{% endcomment %}
Docker Hub 上の Docker リポジトリとして `scratch` が登場したことにより、`scratch` という名前を使ったイメージのアップロード、実行、タグづけはできなくなりました。
そのかわり`Dockerfile` 内での参照のみが可能です。
たとえば `scratch` を利用した最小コンテナーの生成は以下のようになります。

```Dockerfile
FROM scratch
ADD hello /
CMD ["/hello"]
```

{% comment %}
Assuming you built the "hello" executable example by following the instructions
at
[https://github.com/docker-library/hello-world/](https://github.com/docker-library/hello-world/),
and you compiled it with the `-static` flag, you can build this Docker
image using this `docker build` command:
{% endcomment %}
[https://github.com/docker-library/hello-world/](https://github.com/docker-library/hello-world/)に示されている手順に従って、"hello" 実行モジュールの例を構築するとします。
実行モジュールは `-static` フラグをつけてコンパイルします。
Docker イメージは `docker build` コマンドによってビルドすることができます。

```bash
docker build --tag hello .
```

{% comment %}
Don't forget the `.` character at the end, which sets the build context to the
current directory.
{% endcomment %}
`.` の文字を最後につけるのを忘れないでください。
これはビルドコンテキストをカレントディレクトリに設定するものです。

{% comment %}
> **Note**: Because Docker Desktop for Mac and Docker Desktop for Windows use a Linux VM,
> you need a Linux binary, rather than a Mac or Windows binary.
> You can use a Docker container to build it:
>
> ```bash
> $ docker run --rm -it -v $PWD:/build ubuntu:16.04
>
> container# apt-get update && apt-get install build-essential
> container# cd /build
> container# gcc -o hello -static -nostartfiles hello.c
> ```
{% endcomment %}
> **メモ**: Docker Desktop for Mac と Docker Desktop for Windows では Linux VM を利用するため、Mac や Windows の実行バイナリではなく Linux の実行バイナリが必要になります。
> Docker コンテナーを使って以下のようにビルドします。
>
> ```bash
> $ docker run --rm -it -v $PWD:/build ubuntu:16.04
>
> container# apt-get update && apt-get install build-essential
> container# cd /build
> container# gcc -o hello -static -nostartfiles hello.c
> ```

{% comment %}
To run your new image, use the `docker run` command:
{% endcomment %}
新イメージは `docker run` コマンドを使って実行します。

```bash
docker run --rm hello
```

{% comment %}
This example creates the hello-world image used in the tutorials.
If you want to test it out, you can clone
[the image repo](https://github.com/docker-library/hello-world).
{% endcomment %}
この例は、チュートリアルにおいて用いられている hello-world イメージを生成します。
これを試してみたい場合は、[イメージリポジトリ](https://github.com/docker-library/hello-world)をクローンすることができます。


{% comment %}
## More resources
{% endcomment %}
## その他の情報
{: #more-resources }

{% comment %}
There are lots more resources available to help you write your `Dockerfile`.
{% endcomment %}
`Dockerfile`の書き方については、他にも多く触れています。

{% comment %}
* There's a [complete guide to all the instructions](/engine/reference/builder.md) available for use in a `Dockerfile` in the reference section.
* To help you write a clear, readable, maintainable `Dockerfile`, we've also
written a [`Dockerfile` best practices guide](dockerfile_best-practices.md).
* If your goal is to create a new Official Image, be sure to read up on Docker's [Official Images](/docker-hub/official_images/).
{% endcomment %}
* リファレンスには`Dockerfile`にて利用可能な[全コマンドのガイド](/engine/reference/builder.md) があります。
* `Dockerfile`をわかりやすく読みやすく、保守がしやすくするように、[`Dockerfile`ベストプラクティス](dockerfile_best-practices.md)も記述しています。
* 公式イメージを作ることが目的であれば Docker の[公式イメージ](/docker-hub/official_images/)を確認してください。
