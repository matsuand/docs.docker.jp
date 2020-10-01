---
description: Hints, tips and guidelines for writing clean, reliable Dockerfiles
keywords: parent image, images, dockerfile, best practices, hub, official image
redirect_from:
- /articles/dockerfile_best-practices/
- /engine/articles/dockerfile_best-practices/
- /docker-cloud/getting-started/intermediate/optimize-dockerfiles/
- /docker-cloud/tutorials/optimize-dockerfiles/
- /engine/userguide/eng-image/dockerfile_best-practices/
title: Dockerfile 記述のベストプラクティス
---

{% comment %}
This document covers recommended best practices and methods for building
efficient images.
{% endcomment %}
このドキュメントは、効果的なイメージを構築する方法、お勧めのベストプラクティスについて示します。

{% comment %}
Docker builds images automatically by reading the instructions from a
`Dockerfile` -- a text file that contains all commands, in order, needed to
build a given image. A `Dockerfile` adheres to a specific format and set of
instructions which you can find at [Dockerfile reference](../../engine/reference/builder.md).
{% endcomment %}
Docker は `Dockerfile` に書かれた指示を読み込んで、自動的にイメージを構築します。
このファイルはあらゆる命令を含んだテキストファイルであり、順に処理することで指定されたイメージを構築するために必要となるものです。
`Dockerfile` は所定のフォーマットや各種の命令に従います。
その内容は [Dockerfile リファレンス](../../engine/reference/builder.md) に示しています。

{% comment %}
A Docker image consists of read-only layers each of which represents a
Dockerfile  instruction. The layers are stacked and each one is a delta of the
changes from the previous layer. Consider this `Dockerfile`:
{% endcomment %}
Docker イメージは読み取り専用のレイヤーにより構成されます。
個々のレイヤーは Dockerfile の各命令を表現しています。
レイヤーは順に積み上げられ、それぞれは直前のレイヤーからの差分を表わします。
以下のような `Dockerfile` を見てみます。

```dockerfile
FROM ubuntu:18.04
COPY . /app
RUN make /app
CMD python /app/app.py
```

{% comment %}
Each instruction creates one layer:
{% endcomment %}
各コマンドからは 1 つずつレイヤーが生成されます。

{% comment %}
- `FROM` creates a layer from the `ubuntu:18.04` Docker image.
- `COPY` adds files from your Docker client's current directory.
- `RUN` builds your application with `make`.
- `CMD` specifies what command to run within the container.
{% endcomment %}
- `FROM` Docker イメージ `ubuntu:18.04` からレイヤーを 1 つ生成します。
- `COPY` Docker クライアントのカレントディレクトリからファイルをコピーします。
- `RUN` `make` を使ってアプリケーションをビルドします。
- `CMD` コンテナー内にて実行するコマンドを指定します。

{% comment %}
When you run an image and generate a container, you add a new _writable layer_
(the "container layer") on top of the underlying layers. All changes made to
the running container, such as writing new files, modifying existing files, and
deleting files, are written to this thin writable container layer.
{% endcomment %}
イメージを実行してコンテナーが生成されると、それまであったレイヤーの上に_書き込み可能なレイヤー（"コンテナーレイヤー"）が加えられます。
実行されているコンテナーへの変更、つまり新規ファイル生成や既存ファイル編集、ファイル削除などはすべて、その薄くできあがった書き込みレイヤーに書き込まれます。

{% comment %}
For more on image layers (and how Docker builds and stores images), see
[About storage drivers](../../storage/storagedriver/index.md).
{% endcomment %}
イメージレイヤー（また Docker がイメージをどう作り保存するか）については [ストレージドライバーについて](../../storage/storagedriver/index.md) を参照してください。

{% comment %}
## General guidelines and recommendations
{% endcomment %}
## 一般的なガイドラインとアドバイス

{% comment %}
### Create ephemeral containers
{% endcomment %}
### エフェメラルなコンテナーの生成

{% comment %}
The image defined by your `Dockerfile` should generate containers that are as
ephemeral as possible. By "ephemeral", we mean that the container can be stopped
and destroyed, then rebuilt and replaced with an absolute minimum set up and
configuration.
{% endcomment %}
`Dockerfile` によって定義されるイメージからコンテナーが作り出されます。
このコンテナーはできるだけエフェメラルな（ephemeral; はかない）ものとして生成されます。
「エフェメラル」という語を使うのは、コンテナーが停止、破棄されて、すぐに新たなものが作り出されるからです。
最小限の構成や設定を行うだけで、新たなものに置き換えられます。

{% comment %}
Refer to [Processes](https://12factor.net/processes) under _The Twelve-factor App_
methodology to get a feel for the motivations of running containers in such a
stateless fashion.
{% endcomment %}
_The Twelve-factor App_ 手法にある[プロセス](https://12factor.net/processes)を見てみると、コンテナーの実行の仕方をステートレス（stateless）にしている理由がつかめると思います。

{% comment %}
### Understand build context
{% endcomment %}
### ビルドコンテキストの理解

{% comment %}
When you issue a `docker build` command, the current working directory is called
the _build context_. By default, the Dockerfile is assumed to be located here,
but you can specify a different location with the file flag (`-f`). Regardless
of where the `Dockerfile` actually lives, all recursive contents of files and
directories in the current directory are sent to the Docker daemon as the build
context.
{% endcomment %}
``docker build`` コマンドを実行したときの、カレントなワーキングディレクトリのことを *ビルドコンテキスト*（build context）と呼びます。
デフォルトで Dockerfile は、カレントなワーキングディレクトリにあるものとみなされます。
ただしファイルフラグ（`-f`）を使って別のディレクトリとすることもできます。
`Dockerfile` が実際にどこにあったとしても、カレントディレクトリ配下にあるファイルやディレクトリの内容がすべて、ビルドコンテキストとして Docker デーモンに送られることになります。

{% comment %}
> Build context example
>
> Create a directory for the build context and `cd` into it. Write "hello" into
> a text file named `hello` and create a Dockerfile that runs `cat` on it. Build
> the image from within the build context (`.`):
>
> ```shell
> mkdir myproject && cd myproject
> echo "hello" > hello
> echo -e "FROM busybox\nCOPY /hello /\nRUN cat /hello" > Dockerfile
> docker build -t helloapp:v1 .
> ```
>
> Move `Dockerfile` and `hello` into separate directories and build a second
> version of the image (without relying on cache from the last build). Use `-f`
> to point to the Dockerfile and specify the directory of the build context:
>
> ```shell
> mkdir -p dockerfiles context
> mv Dockerfile dockerfiles && mv hello context
> docker build --no-cache -t helloapp:v2 -f dockerfiles/Dockerfile context
> ```
{% endcomment %}
> ビルドコンテキストの例
>
> ビルドコンテキストとするディレクトリを生成してそこに `cd` で移動します。
> テキストファイル `hello` に "hello" と書き込み、Dockerfile 上でそのファイルに対して `cat` コマンドを与えるようにします。
> ビルドコンテキスト（`.`）の中からイメージをビルドします。
>
> ```shell
> mkdir myproject && cd myproject
> echo "hello" > hello
> echo -e "FROM busybox\nCOPY /hello /\nRUN cat /hello" > Dockerfile
> docker build -t helloapp:v1 .
> ```
>
> `Dockerfile` と `hello` をそれぞれ別のディレクトリに移動させて、（上でビルドした際のキャッシュは用いずに）2 つめのイメージをビルドします。
> Dockerfile に対して `-f` を使い、ビルドコンテキストとなるディレクトリを指定します。
>
> ```shell
> mkdir -p dockerfiles context
> mv Dockerfile dockerfiles && mv hello context
> docker build --no-cache -t helloapp:v2 -f dockerfiles/Dockerfile context
> ```

{% comment %}
Inadvertently including files that are not necessary for building an image
results in a larger build context and larger image size. This can increase the
time to build the image, time to pull and push it, and the container runtime
size. To see how big your build context is, look for a message like this when
building your `Dockerfile`:
{% endcomment %}
イメージのビルドには必要のないファイルを誤って含めてしまうと、ビルドコンテキストがそれだけ大きくなり、結果としてサイズの大きなイメージが生成されることになります。
こうしてしまうと、イメージビルドの時間、このイメージをプッシュしたりプルしたりする時間が、その分だけ要することとなり、コンテナーの実行時の容量も増えてしまいます。
ビルドコンテキストのサイズがどれだけになったかは、`Dockerfile` を使ってビルド処理を行った際の以下のようなメッセージを確認すればわかります。

```none
Sending build context to Docker daemon  187.8MB
```

{% comment %}
### Pipe Dockerfile through `stdin`
{% endcomment %}
### `stdin` を通じた Dockerfile のパイプ

{% comment %}
Docker has the ability to build images by piping `Dockerfile` through `stdin`
with a _local or remote build context_. Piping a `Dockerfile` through `stdin`
can be useful to perform one-off builds without writing a Dockerfile to disk,
or in situations where the `Dockerfile` is generated, and should not persist
afterwards.
{% endcomment %}
Docker には、`Dockerfile` を `stdin` からパイプ入力してイメージをビルドできる機能があります。
その際には *ローカルの、あるいはリモートのビルドコンテキスト* を利用することができます。
`stdin` からの `Dockerfile` のパイプ入力機能は、Dockerfile をディスクに書き込む必要のない、1 度きりのイメージビルドを行う場合に利用できます。
あるいは `Dockerfile` は生成されているものの、その後は必要がなくなるような場合にも活用できます。

{% comment %}
> The examples in this section use [here documents](http://tldp.org/LDP/abs/html/here-docs.html)
> for convenience, but any method to provide the `Dockerfile` on `stdin` can be
> used.
>
> For example, the following commands are equivalent:
>
> ```bash
> echo -e 'FROM busybox\nRUN echo "hello world"' | docker build -
> ```
>
> ```bash
> docker build -<<EOF
> FROM busybox
> RUN echo "hello world"
> EOF
> ```
>
> You can substitute the examples with your preferred approach, or the approach
> that best fits your use-case.
{% endcomment %}
> この節における利用例では、扱いやすい [ヒアドキュメント](http://tldp.org/LDP/abs/html/here-docs.html) を使っていますが、`stdin` から `Dockerfile` を与える方法には、他にもいろいろとあります。
>
> たとえば以下の 2 つのコマンドは同じ処理を行います。
>
> ```bash
> echo -e 'FROM busybox\nRUN echo "hello world"' | docker build -
> ```
>
> ```bash
> docker build -<<EOF
> FROM busybox
> RUN echo "hello world"
> EOF
> ```
>
> この例に示している方法は、好みの方法あるいは作業に適した方法に置き換えてください。


{% comment %}
#### Build an image using a Dockerfile from stdin, without sending build context
{% endcomment %}
{: #build-an-image-using-a-dockerfile-from-stdin-without-sending-build-context }
#### ビルドコンテキストの送信なく `stdin` からの Dockerfile によりイメージをビルド

{% comment %}
Use this syntax to build an image using a `Dockerfile` from `stdin`, without
sending additional files as build context. The hyphen (`-`) takes the position
of the `PATH`, and instructs Docker to read the build context (which only
contains a `Dockerfile`) from `stdin` instead of a directory:
{% endcomment %}
以下に示す構文は、`stdin` から Dockerfile を指定してイメージをビルドしますが、その際にビルドコンテキストとして余計なファイルは送信しないようになります。
ハイフン（`-`）を `PATH` の指定場所に記述します。
こうすると Docker は、ディレクトリからではなく `stdin` からビルドコンテキストを読み込むことになります。
（そこには `Dockerfile` しかありません。）

```bash
docker build [OPTIONS] -
```

{% comment %}
The following example builds an image using a `Dockerfile` that is passed through
`stdin`. No files are sent as build context to the daemon.
{% endcomment %}
以下の例は `stdin` から受け渡された `Dockerfile` を使ってイメージをビルドします。
ビルドコンテキストからデーモンに送信されるファイルは 1 つもありません。

```bash
docker build -t myimage:latest -<<EOF
FROM busybox
RUN echo "hello world"
EOF
```

{% comment %}
Omitting the build context can be useful in situations where your `Dockerfile`
does not require files to be copied into the image, and improves the build-speed,
as no files are sent to the daemon.
{% endcomment %}
ビルドコンテキストを省略するのは、イメージにコピーするファイルが何もないような `Dockerfile` を用いる際に利用できます。
その場合はデーモンへのファイル送信がない分だけ、ビルド時間が短縮されます。

{% comment %}
If you want to improve the build-speed by excluding _some_ files from the build-
context, refer to [exclude with .dockerignore](#exclude-with-dockerignore).
{% endcomment %}
ビルドコンテキストからファイルを除外することによって、イメージビルドの時間を短縮しようとする場合は、[.dockerignore によるファイル除外の指定](#exclude-with-dockerignore) を参照してください。

{% comment %}
> **Note**: Attempting to build a Dockerfile that uses `COPY` or `ADD` will fail
> if this syntax is used. The following example illustrates this:
>
> ```bash
> # create a directory to work in
> mkdir example
> cd example
>
> # create an example file
> touch somefile.txt
>
> docker build -t myimage:latest -<<EOF
> FROM busybox
> COPY somefile.txt .
> RUN cat /somefile.txt
> EOF
>
> # observe that the build fails
> ...
> Step 2/3 : COPY somefile.txt .
> COPY failed: stat /var/lib/docker/tmp/docker-builder249218248/somefile.txt: no such file or directory
> ```
{% endcomment %}
> **メモ**: この構文を用いる際に、Dockerfile に `COPY` や `ADD` を含めるとビルドに失敗します。
> 以下にその例を説明します。
>
> ```bash
> # 作業ディレクトリを生成します。
> mkdir example
> cd example
>
> # サンプルのファイルを生成します。
> touch somefile.txt
>
> docker build -t myimage:latest -<<EOF
> FROM busybox
> COPY somefile.txt .
> RUN cat /somefile.txt
> EOF
>
> # ビルドが失敗する様子を確認します。
> ...
> Step 2/3 : COPY somefile.txt .
> COPY failed: stat /var/lib/docker/tmp/docker-builder249218248/somefile.txt: no such file or directory
> ```

{% comment %}
#### Build from a local build context, using a Dockerfile from stdin
{% endcomment %}
{: #build-from-a-local-build-context-using-a-dockerfile-from-stdin }
#### ローカルのビルドコンテキストを使ったビルド、`stdio` からの Dockerfile 利用

{% comment %}
Use this syntax to build an image using files on your local filesystem, but using
a `Dockerfile` from `stdin`. The syntax uses the `-f` (or `--file`) option to
specify the `Dockerfile` to use, using a hyphen (`-`) as filename to instruct
Docker to read the `Dockerfile` from `stdin`:
{% endcomment %}
以下の構文により、ローカルファイルシステム上のファイルを使ってイメージをビルドします。
ただし `Dockerfile` は `stdin` からの入力とします。
この構文では `-f`（または `--file`）オプションによって `Dockerfile` を指定します。
そしてファイル名にはハイフン（`-`）を指定することで、`Dockerfile` を `stdin` から読み込むことを指示しています。

```bash
docker build [OPTIONS] -f- PATH
```

{% comment %}
The example below uses the current directory (`.`) as the build context, and builds
an image using a `Dockerfile` that is passed through `stdin` using a [here
document](http://tldp.org/LDP/abs/html/here-docs.html).
{% endcomment %}
以下の例ではビルドコンテキストとしてカレントディレクトリ（`.`）を利用します。
そして `stdin` から受け渡される `Dockerfile` を使ってイメージをビルドします。
この際には [ヒアドキュメント](http://tldp.org/LDP/abs/html/here-docs.html) を使っています。

{% comment %}
```bash
# create a directory to work in
mkdir example
cd example

# create an example file
touch somefile.txt

# build an image using the current directory as context, and a Dockerfile passed through stdin
docker build -t myimage:latest -f- . <<EOF
FROM busybox
COPY somefile.txt .
RUN cat /somefile.txt
EOF
```
{% endcomment %}
```bash
# 作業ディレクトリを生成します。
mkdir example
cd example

# サンプルのファイルを生成します。
touch somefile.txt

# カレントディレクトリをコンテキストとして stdin から Dockerfile をビルドします。
docker build -t myimage:latest -f- . <<EOF
FROM busybox
COPY somefile.txt .
RUN cat /somefile.txt
EOF
```

{% comment %}
#### Build from a remote build context, using a Dockerfile from stdin
{% endcomment %}
{: #build-from-a-remote-build-context-using-a-dockerfile-from-stdin }
#### リモートのビルドコンテキストを使ったビルド、`stdio` からの Dockerfile 利用

{% comment %}
Use this syntax to build an image using files from a remote `git` repository,
using a `Dockerfile` from `stdin`. The syntax uses the `-f` (or `--file`) option to
specify the `Dockerfile` to use, using a hyphen (`-`) as filename to instruct
Docker to read the `Dockerfile` from `stdin`:
{% endcomment %}
以下の構文により、リモートの git リポジトリ上のファイルを使ってイメージをビルドします。
ここでも `Dockerfile` は `stdin` からの入力とします。
この構文では `-f`（または `--file`）オプションによって `Dockerfile` を指定します。
そしてファイル名にはハイフン（`-`）を指定することで、`Dockerfile` を `stdin` から読み込むことを指示しています。

```bash
docker build [OPTIONS] -f- PATH
```

{% comment %}
This syntax can be useful in situations where you want to build an image from a
repository that does not contain a `Dockerfile`, or if you want to build with a custom
`Dockerfile`, without maintaining your own fork of the repository.
{% endcomment %}
この構文は、イメージをビルドするために利用するリポジトリに `Dockerfile` が含まれていないような場合に用いることができます。
あるいは `Dockerfile` には独自のものを使ってビルドを行いたい場合です。
つまりフォークしたリポジトリを変更しなくて済みます。

{% comment %}
The example below builds an image using a `Dockerfile` from `stdin`, and adds
the `hello.c` file from the ["hello-world" Git repository on GitHub](https://github.com/docker-library/hello-world).
{% endcomment %}
以下の例は `stdin` から `Dockerfile` を指定してイメージをビルドします。
そして [GitHub 上の git リポジトリ "hello-world"](https://github.com/docker-library/hello-world) から `hello.c` ファイルを取得して加えます。

```bash
docker build -t myimage:latest -f- https://github.com/docker-library/hello-world.git <<EOF
FROM busybox
COPY hello.c .
EOF
```

{% comment %}
> **Under the hood**
>
> When building an image using a remote Git repository as build context, Docker
> performs a `git clone` of the repository on the local machine, and sends
> those files as build context to the daemon. This feature requires `git` to be
> installed on the host where you run the `docker build` command.
{% endcomment %}
> **内部の処理**
>
> ビルドコンテキストとしてリモートの git リポジトリを使ってイメージをビルドする場合、Docker はローカルマシン上において、そのリポジトリに対して `git clone` を実行します。
> そして取得されるファイルをビルドコンテキストとしてデーモンに送ります。
> つまりこの機能を実行するには、`docker build` コマンドを実行するホスト上に `git` をインストールしていることが必要です。

{% comment %}
### Exclude with .dockerignore
{% endcomment %}
{: #exclude-with-dockerignore }
### .dockerignore を使ったファイル除外の指定

{% comment %}
To exclude files not relevant to the build (without restructuring your source
repository) use a `.dockerignore` file. This file supports exclusion patterns
similar to `.gitignore` files. For information on creating one, see the
[.dockerignore file](../../engine/reference/builder.md#dockerignore-file).
{% endcomment %}
ビルドに関係のないファイルを（ソースリポジトリを変更することなく）除外するには `.dockerignore` ファイルを利用します。
このファイルは `.gitignore` ファイルと同様のファイル除外指定パターンに対応しています。
ファイルの生成に関しては [.dockerignore ファイル](../../engine/reference/builder.md#dockerignore-file) を参照してください。

{% comment %}
### Use multi-stage builds
{% endcomment %}
{: #use-multi-stage-builds }
### マルチステージビルドの利用

{% comment %}
[Multi-stage builds](multistage-build.md) allow you to drastically reduce the
size of your final image, without struggling to reduce the number of intermediate
layers and files.
{% endcomment %}
[マルチステージビルド](multistage-build.md) は、最終的なイメージサイズを激減させることができます。
中間的に生成されるレイヤーやファイルの数を減らすような苦労は必要ありません。

{% comment %}
Because an image is built during the final stage of the build process, you can
minimize image layers by [leveraging build cache](#leverage-build-cache).
{% endcomment %}
イメージというものは、ビルド処理の最終段階で生成されるものです。
したがってイメージのレイヤーは [ビルドキャッシュの活用](#leverage-build-cache) によって最小限に抑えることができます。

{% comment %}
For example, if your build contains several layers, you can order them from the
less frequently changed (to ensure the build cache is reusable) to the more
frequently changed:
{% endcomment %}
たとえばビルドするイメージにレイヤーがいくつかある場合、変更があまり行われないもの（ビルドキャッシュが確実に再利用されるもの）から、頻繁に変更されるものへと並び順を定めることができます。

{% comment %}
* Install tools you need to build your application
{% endcomment %}
* アプリケーションのビルドに必要なツールのインストール

{% comment %}
* Install or update library dependencies
{% endcomment %}
* 依存するライブラリのインストールまたはアップデート

{% comment %}
* Generate your application
{% endcomment %}
* アプリケーションのビルド

{% comment %}
A Dockerfile for a Go application could look like:
{% endcomment %}
たとえば Go 言語アプリケーションの Dockerfile は以下のようになります。

{% comment %}
```dockerfile
FROM golang:1.11-alpine AS build

# Install tools required for project
# Run `docker build --no-cache .` to update dependencies
RUN apk add --no-cache git
RUN go get github.com/golang/dep/cmd/dep

# List project dependencies with Gopkg.toml and Gopkg.lock
# These layers are only re-built when Gopkg files are updated
COPY Gopkg.lock Gopkg.toml /go/src/project/
WORKDIR /go/src/project/
# Install library dependencies
RUN dep ensure -vendor-only

# Copy the entire project and build it
# This layer is rebuilt when a file changes in the project directory
COPY . /go/src/project/
RUN go build -o /bin/project

# This results in a single layer image
FROM scratch
COPY --from=build /bin/project /bin/project
ENTRYPOINT ["/bin/project"]
CMD ["--help"]
```
{% endcomment %}
```dockerfile
FROM golang:1.11-alpine AS build

# 本プロジェクトに必要なツールをインストール。
# `docker build --no-cache .` を実行して依存パッケージのアップデート。
RUN apk add --no-cache git
RUN go get github.com/golang/dep/cmd/dep

# プロジェクトの依存関係を Gopkg.toml と Gopkg.lock に列記。
# これに対応するレイヤーは Gopkg ファイルの更新時のみ再ビルド。
COPY Gopkg.lock Gopkg.toml /go/src/project/
WORKDIR /go/src/project/
# 依存ライブラリをインストール。
RUN dep ensure -vendor-only

# プロジェクト全体をコピーしてビルド。
# このレイヤーはプロジェクトディレクトリ内のファイル変更時に再ビルド。
COPY . /go/src/project/
RUN go build -o /bin/project

# 以下によりイメージを 1 レイヤーに。
FROM scratch
COPY --from=build /bin/project /bin/project
ENTRYPOINT ["/bin/project"]
CMD ["--help"]
```

{% comment %}
### Don't install unnecessary packages
{% endcomment %}
{: #dont-install-unnecessary-packages }
### 不要なパッケージをインストールしない

{% comment %}
To reduce complexity, dependencies, file sizes, and build times, avoid
installing extra or unnecessary packages just because they might be "nice to
have." For example, you don’t need to include a text editor in a database image.
{% endcomment %}
Dockerfile をわかりやすくして、依存関係、ファイルサイズ、構築時間をいずれも減らすためには、余分で必須ではない「あった方が良いだろう」程度のパッケージはインストールしないようにします。
例えば、データベースイメージであればテキストエディターは不要のはずです。

{% comment %}
### Decouple applications
{% endcomment %}
{: #decouple-applications }
### アプリケーションの分割

{% comment %}
Each container should have only one concern. Decoupling applications into
multiple containers makes it easier to scale horizontally and reuse containers.
For instance, a web application stack might consist of three separate
containers, each with its own unique image, to manage the web application,
database, and an in-memory cache in a decoupled manner.
{% endcomment %}
1 つのコンテナーが取り扱う内容は 1 つにしぼるべきです。
アプリケーションを複数のコンテナーに分けると、スケールアウトやコンテナーの再利用がしやすくなります。
たとえばウェブアプリケーションが 3 つの独立したコンテナーにより成り立っているとします。
それらは個々のイメージを持ち、それぞれに分かれてウェブアプリケーション、データベース、メモリキャッシュを管理するようになります。

{% comment %}
Limiting each container to one process is a good rule of thumb, but it is not a
hard and fast rule. For example, not only can containers be
[spawned with an init process](../../engine/reference/run.md#specify-an-init-process),
some programs might spawn additional processes of their own accord. For
instance, [Celery](http://www.celeryproject.org/) can spawn multiple worker
processes, and [Apache](https://httpd.apache.org/) can create one process per
request.
{% endcomment %}
個々のコンテナーを 1 つのプロセスのみに限定して割り当てることは、優れた経験則となることがあります。
しかし決して厳密な規則というわけでもありません。
たとえばコンテナーは [初期プロセスにおいて起動](../../engine/reference/run.md#specify-an-init-process) することが可能であり、プログラムの中には必要に応じて追加のプロセスを起動するようなものもあります。
例をあげると、[Celery](http://www.celeryproject.org/) はワーカープロセスを複数起動し、[Apache](https://httpd.apache.org/) はリクエストごとにプロセスを生成します。

{% comment %}
Use your best judgment to keep containers as clean and modular as possible. If
containers depend on each other, you can use [Docker container networks](../../network/index.md)
to ensure that these containers can communicate.
{% endcomment %}
コンテナーはできるかぎりすっきりとモジュール分割されるように、適切に判断してください。
コンテナーが互いに依存している場合は、[Docker container ネットワーク](../../network/index.md) を用いることで、コンテナー間の通信を確実に行うことができます。

{% comment %}
### Minimize the number of layers
{% endcomment %}
{: #minimize-the-number-of-layers }
### レイヤー数は最小に

{% comment %}
In older versions of Docker, it was important that you minimized the number of
layers in your images to ensure they were performant. The following features
were added to reduce this limitation:
{% endcomment %}
Docker の古いバージョンでは、イメージに含まれるレイヤー数を最小におさえることが重要であり、これにより処理性能を確保していました。
この制約は後々、以下のような機能が加えられることで軽減されました。

{% comment %}
- Only the instructions `RUN`, `COPY`, `ADD` create layers. Other instructions
  create temporary intermediate images, and do not increase the size of the build.
{% endcomment %}
- `RUN`, `COPY`, `ADD` の命令のみレイヤーを生成します。
  他の命令は一時的な中間イメージを生成するため、ビルドイメージのサイズは増加させません。

{% comment %}
- Where possible, use [multi-stage builds](multistage-build.md), and only copy
  the artifacts you need into the final image. This allows you to include tools
  and debug information in your intermediate build stages without increasing the
  size of the final image.
{% endcomment %}
- 可能であれば [マルチステージビルド](multistage-build.md) を利用します。
  そして必要な成果物のみを最終イメージに含めるようにします。
  途中で利用するツールやデバッグ情報は、中間的なビルドステージの中で取り扱うことができ、最終イメージのサイズを増やさずに済みます。

{% comment %}
### Sort multi-line arguments
{% endcomment %}
{: #sort-multi-line-arguments }
### 複数行にわたる引数は並びを適切に

{% comment %}
Whenever possible, ease later changes by sorting multi-line arguments
alphanumerically. This helps to avoid duplication of packages and make the
list much easier to update. This also makes PRs a lot easier to read and
review. Adding a space before a backslash (`\`) helps as well.
{% endcomment %}
複数行にわたる引数は、後々の変更を容易にするために、できるならその並びはアルファベット順にします。
そうしておけば、パッケージを重複指定することはなくなり、一覧の変更も簡単になります。
プルリクエストを読んだりレビューしたりすることも、さらに楽になります。
バックスラッシュ（`\`） の前に空白を含めておくことも同様です。

{% comment %}
Here’s an example from the [`buildpack-deps` image](https://github.com/docker-library/buildpack-deps):
{% endcomment %}
以下は [`buildpack-deps` イメージ](https://github.com/docker-library/buildpack-deps) の記述例です。

```dockerfile
RUN apt-get update && apt-get install -y \
  bzr \
  cvs \
  git \
  mercurial \
  subversion \
  && rm -rf /var/lib/apt/lists/*
```

{% comment %}
### Leverage build cache
{% endcomment %}
{: #leverage-build-cache }
### ビルドキャッシュの利用

{% comment %}
When building an image, Docker steps through the instructions in your
`Dockerfile`, executing each in the order specified. As each instruction is
examined, Docker looks for an existing image in its cache that it can reuse,
rather than creating a new (duplicate) image.
{% endcomment %}
イメージの構築時に Docker は `Dockerfile` 内に示されている命令を記述順に実行していきます。
個々の命令が処理される際に Docker は、既存イメージのキャッシュが再利用できるかどうかを調べます。
そこでは新たな（同じ）イメージを作ることはしません。

{% comment %}
If you do not want to use the cache at all, you can use the `--no-cache=true`
option on the `docker build` command. However, if you do let Docker use its
cache, it is important to understand when it can, and cannot, find a matching
image. The basic rules that Docker follows are outlined below:
{% endcomment %}
キャッシュをまったく使いたくない場合は `docker build` コマンドに `--no-cache=true` オプションをつけて実行します。
一方で Docker のキャッシュを利用する場合、Docker が適切なイメージを見つけた上で、どのようなときにキャッシュを利用し、どのようなときには利用しないのかを理解しておくことが必要です。
Docker が従っている規則は以下のとおりです。

{% comment %}
- Starting with a parent image that is already in the cache, the next
  instruction is compared against all child images derived from that base
  image to see if one of them was built using the exact same instruction. If
  not, the cache is invalidated.
{% endcomment %}
- キャッシュ内にすでに存在している親イメージから処理を始めます。
  そのベースとなるイメージから派生した子イメージに対して、次の命令が合致するかどうかが比較され、子イメージのいずれかが同一の命令によって構築されているかを確認します。
  そのようなものが存在しなければ、キャッシュは無効になります。

{% comment %}
- In most cases, simply comparing the instruction in the `Dockerfile` with one
  of the child images is sufficient. However, certain instructions require more
  examination and explanation.
{% endcomment %}
- ほとんどの場合 `Dockerfile` 内の命令と子イメージのどれかを単純に比較するだけで十分です。
  しかし命令によっては、多少の検査や解釈が必要となるものもあります。

{% comment %}
- For the `ADD` and `COPY` instructions, the contents of the file(s)
  in the image are examined and a checksum is calculated for each file.
  The last-modified and last-accessed times of the file(s) are not considered in
  these checksums. During the cache lookup, the checksum is compared against the
  checksum in the existing images. If anything has changed in the file(s), such
  as the contents and metadata, then the cache is invalidated.
{% endcomment %}
- `ADD` 命令や `COPY` 命令では、イメージに含まれるファイルの内容が検査され、個々のファイルについてチェックサムが計算されます。
  この計算において、ファイルの最終更新時刻、最終アクセス時刻は考慮されません。
  キャッシュを探す際に、このチェックサムと既存イメージのチェックサムが比較されます。
  ファイル内の何かが変更になったとき、たとえばファイル内容やメタデータが変わっていれば、キャッシュは無効になります。

{% comment %}
- Aside from the `ADD` and `COPY` commands, cache checking does not look at the
  files in the container to determine a cache match. For example, when processing
  a `RUN apt-get -y update` command the files updated in the container
  are not examined to determine if a cache hit exists.  In that case just
  the command string itself is used to find a match.
{% endcomment %}
- `ADD` と `COPY` 以外のコマンドの場合、キャッシュのチェックは、コンテナー内のファイル内容を見ることはなく、それによってキャッシュと合致しているかどうかが決定されるわけでありません。
  たとえば `RUN apt-get -y update` コマンドの処理が行われる際には、コンテナー内にて更新されたファイルは、キャッシュが合致するかどうかの判断のために用いられません。
  この場合にはコマンド文字列そのものが、キャッシュの合致判断に用いられます。

{% comment %}
Once the cache is invalidated, all subsequent `Dockerfile` commands generate new
images and the cache is not used.
{% endcomment %}
キャッシュが無効化されると、以降の `Dockerfile` 命令ではキャッシュは使われず、新しいイメージを生成します。

{% comment %}
## Dockerfile instructions
{% endcomment %}
{: #dockerfile-instructions }
## Dockerfile コマンド

{% comment %}
These recommendations are designed to help you create an efficient and
maintainable `Dockerfile`.
{% endcomment %}
効率のよい保守性に優れた `Dockerfile` を生成するために、推奨する内容を以下に示します。

### FROM

{% comment %}
[Dockerfile reference for the FROM instruction](../../engine/reference/builder.md#from)
{% endcomment %}
[Dockerfile リファレンスの FROM コマンド](../../engine/reference/builder.md#from)

{% comment %}
Whenever possible, use current official images as the basis for your
images. We recommend the [Alpine image](https://hub.docker.com/_/alpine/) as it
is tightly controlled and small in size (currently under 5 MB), while still
being a full Linux distribution.
{% endcomment %}
イメージのベースは、できるだけ現時点での公式リポジトリを利用してください。
[Alpine イメージ](https://hub.docker.com/_/alpine/) がお勧めです。
このイメージはしっかりと管理されていて、充実した Linux ディストリビューションであるにもかかわらず、非常にコンパクトなものになっています（現在 5 MB 以下）。

### LABEL

{% comment %}
[Understanding object labels](../../config/labels-custom-metadata.md)
{% endcomment %}
[オブジェクトラベルを理解する](../../config/labels-custom-metadata.md)

{% comment %}
You can add labels to your image to help organize images by project, record
licensing information, to aid in automation, or for other reasons. For each
label, add a line beginning with `LABEL` and with one or more key-value pairs.
The following examples show the different acceptable formats. Explanatory comments are included inline.
{% endcomment %}
イメージにラベルを追加するのは、プロジェクト内でのイメージ管理をしやすくしたり、ライセンス情報の記録や自動化の助けとするなど、さまざまな目的があります。
ラベルを指定するには、 `LABEL` で始まる行を追加して、そこにキーバリューペアをいくつか設定します。
以下に示す例は、いずれも正しい構文です。
説明をコメントとしてつけています。

{% comment %}
> Strings with spaces must be quoted **or** the spaces must be escaped. Inner
> quote characters (`"`), must also be escaped.
{% endcomment %}
> 文字列に空白が含まれる場合は、引用符でくくるか、**あるいは**エスケープする必要があります。
> 文字列内に引用符がある場合も、同様にエスケープしてください。


{% comment %}
```dockerfile
# Set one or more individual labels
LABEL com.example.version="0.0.1-beta"
LABEL vendor1="ACME Incorporated"
LABEL vendor2=ZENITH\ Incorporated
LABEL com.example.release-date="2015-02-12"
LABEL com.example.version.is-production=""
```
{% endcomment %}
```dockerfile
# 個々にラベルを設定
LABEL com.example.version="0.0.1-beta"
LABEL vendor1="ACME Incorporated"
LABEL vendor2=ZENITH\ Incorporated
LABEL com.example.release-date="2015-02-12"
LABEL com.example.version.is-production=""
```

{% comment %}
An image can have more than one label. Prior to Docker 1.10, it was recommended
to combine all labels into a single `LABEL` instruction, to prevent extra layers
from being created. This is no longer necessary, but combining labels is still
supported.
{% endcomment %}
イメージには複数のラベルを設定することができます。
Docker 1.10 以前のバージョンでは、ラベルをすべてまとめて 1 つの `LABEL` 命令にすることが推奨されていました。
これによって余分なレイヤーが生成されることを防ぐためです。
このことは、現在は必要ではなくなっています。
ただしラベルをまとめる機能は今もサポートされています。

{% comment %}
```dockerfile
# Set multiple labels on one line
LABEL com.example.version="0.0.1-beta" com.example.release-date="2015-02-12"
```
{% endcomment %}
```dockerfile
# 1 行でラベルを設定
LABEL com.example.version="0.0.1-beta" com.example.release-date="2015-02-12"
```

{% comment %}
The above can also be written as:
{% endcomment %}
上は以下のように書くこともできます。

{% comment %}
```dockerfile
# Set multiple labels at once, using line-continuation characters to break long lines
LABEL vendor=ACME\ Incorporated \
      com.example.is-beta= \
      com.example.is-production="" \
      com.example.version="0.0.1-beta" \
      com.example.release-date="2015-02-12"
```
{% endcomment %}
```dockerfile
# 複数のラベルを一度に設定、ただし行継続の文字を使い、長い文字列を改行する
LABEL vendor=ACME\ Incorporated \
      com.example.is-beta= \
      com.example.is-production="" \
      com.example.version="0.0.1-beta" \
      com.example.release-date="2015-02-12"
```

{% comment %}
See [Understanding object labels](../../config/labels-custom-metadata.md)
for guidelines about acceptable label keys and values. For information about
querying labels, refer to the items related to filtering in
[Managing labels on objects](../../config/labels-custom-metadata.md#manage-labels-on-objects).
See also [LABEL](../../engine/reference/builder.md#label) in the Dockerfile reference.
{% endcomment %}
利用可能なラベルのキーおよび値に関するガイドラインが [オブジェクトラベルの理解](../../config/labels-custom-metadata.md) に示されています。
ラベルを検索する方法については、[オブジェクト内のラベル管理](../../config/labels-custom-metadata.md#manage-labels-on-objects) に示されているフィルタリングに関する項目を参照してください。
また Dockerfile リファレンスの [LABEL](../../engine/reference/builder.md#label) も参考になります。

### RUN

{% comment %}
[Dockerfile reference for the RUN instruction](../../engine/reference/builder.md#run)
{% endcomment %}
[Dockerfile リファレンスの RUN コマンド](../../engine/reference/builder.md#run)

{% comment %}
Split long or complex `RUN` statements on multiple lines separated with
backslashes to make your `Dockerfile` more readable, understandable, and
maintainable.
{% endcomment %}
`RUN` コマンドが複数行にわたって長く複雑になるようであれば、バックスラッシュを使って行を分けてください。
`Dockerfile` を読みやすく理解しやすく、そして保守しやすくするためです。

#### apt-get

{% comment %}
Probably the most common use-case for `RUN` is an application of `apt-get`.
Because it installs packages, the `RUN apt-get` command has several gotchas to
look out for.
{% endcomment %}
おそらく `RUN` において一番利用する使い方が `apt-get` アプリケーションの実行です。
`RUN apt-get` はパッケージをインストールするものであるため、注意点がいくつかあります。

{% comment %}
Avoid `RUN apt-get upgrade` and `dist-upgrade`, as many of the "essential"
packages from the parent images cannot upgrade inside an
[unprivileged container](../../engine/reference/run.md#security-configuration). If a package
contained in the parent image is out-of-date, contact its maintainers. If you
know there is a particular package, `foo`, that needs to be updated, use
`apt-get install -y foo` to update automatically.
{% endcomment %}
`RUN apt-get upgrade` や `dist-upgrade` の実行は避けてください。
親イメージに含まれる重要パッケージは、[権限が与えられていないコンテナー](../../engine/reference/run.md#security-configuration) 内ではほとんど更新できないからです。
親イメージ内のパッケージが古くなっていたら、開発者に連絡をとってください。
`foo` というパッケージを更新する必要があれば `apt-get install -y foo` を利用してください。
これによってパッケージは自動的に更新されます。

{% comment %}
Always combine `RUN apt-get update` with `apt-get install` in the same `RUN`
statement. For example:
{% endcomment %}
`RUN apt-get update` と ``apt-get install`` は、同一の `RUN` コマンド内にて同時実行するようにしてください。
たとえば以下のようにします。

```dockerfile
RUN apt-get update && apt-get install -y \
    package-bar \
    package-baz \
    package-foo  \
    && rm -rf /var/lib/apt/lists/*
```

{% comment %}
Using `apt-get update` alone in a `RUN` statement causes caching issues and
subsequent `apt-get install` instructions fail. For example, say you have a
Dockerfile:
{% endcomment %}
１つの `RUN` コマンド内で `apt-get update` だけを使うとキャッシュに問題が発生し、その後の `apt-get install` コマンドが失敗します。
たとえば Dockerfile を以下のように記述したとします。

```dockerfile
FROM ubuntu:18.04
RUN apt-get update
RUN apt-get install -y curl
```

{% comment %}
After building the image, all layers are in the Docker cache. Suppose you later
modify `apt-get install` by adding extra package:
{% endcomment %}
イメージが構築されると、レイヤーがすべて Docker のキャッシュに入ります。
この次に `apt-get install` を編集して別のパッケージを追加したとします。

```dockerfile
FROM ubuntu:18.04
RUN apt-get update
RUN apt-get install -y curl nginx
```

{% comment %}
Docker sees the initial and modified instructions as identical and reuses the
cache from previous steps. As a result the `apt-get update` is _not_ executed
because the build uses the cached version. Because the `apt-get update` is not
run, your build can potentially get an outdated version of the `curl` and
`nginx` packages.
{% endcomment %}
Docker は当初のコマンドと修正後のコマンドを見て、同一のコマンドであると判断するので、前回の処理において作られたキャッシュを再利用します。
キャッシュされたものを利用して処理が行われるわけですから、結果として `apt-get update` は実行されません。
`apt-get update` が実行されないということは、つまり `curl` にしても `nginx` にしても、古いバージョンのまま利用する可能性が出てくるということです。

{% comment %}
Using `RUN apt-get update && apt-get install -y` ensures your Dockerfile
installs the latest package versions with no further coding or manual
intervention. This technique is known as "cache busting". You can also achieve
cache-busting by specifying a package version. This is known as version pinning,
for example:
{% endcomment %}
`RUN apt-get update && apt-get install -y` というコマンドにすると、 Dockerfile が確実に最新バージョンをインストールしてくれるものとなり、さらにコードを書いたり手作業を加えたりする必要がなくなります。
これは「キャッシュバスティング（cache busting）」と呼ばれる技術です。
この技術は、パッケージのバージョンを指定することによっても利用することができます。
これはバージョンピニング（version pinning）というものです。
以下に例を示します。

```dockerfile
RUN apt-get update && apt-get install -y \
    package-bar \
    package-baz \
    package-foo=1.3.*
```

{% comment %}
Version pinning forces the build to retrieve a particular version regardless of
what’s in the cache. This technique can also reduce failures due to unanticipated changes
in required packages.
{% endcomment %}
バージョンピニングでは、キャッシュにどのようなイメージがあろうとも、指定されたバージョンを使ってビルドが行われます。
この手法を用いれば、そのパッケージの最新版に、思いもよらない変更が加わっていたとしても、ビルド失敗を回避できることもあります。

{% comment %}
Below is a well-formed `RUN` instruction that demonstrates all the `apt-get`
recommendations.
{% endcomment %}
以下の `RUN` コマンドはきれいに整えられていて `apt-get` の推奨する利用方法を示しています。

```dockerfile
RUN apt-get update && apt-get install -y \
    aufs-tools \
    automake \
    build-essential \
    curl \
    dpkg-sig \
    libcap-dev \
    libsqlite3-dev \
    mercurial \
    reprepro \
    ruby1.9.1 \
    ruby1.9.1-dev \
    s3cmd=1.1.* \
 && rm -rf /var/lib/apt/lists/*
```

{% comment %}
The `s3cmd` argument specifies a version `1.1.*`. If the image previously
used an older version, specifying the new one causes a cache bust of `apt-get
update` and ensures the installation of the new version. Listing packages on
each line can also prevent mistakes in package duplication.
{% endcomment %}
`s3cmd` のコマンド行は、バージョン `1.1.*` を指定しています。
以前に作られたイメージが古いバージョンを使っていたとしても、新たなバージョンの指定により `apt-get update` のキャッシュバスティングが働いて、確実に新バージョンがインストールされるようになります。
パッケージを各行に分けて記述しているのは、パッケージを重複して書くようなミスを防ぐためです。

{% comment %}
In addition, when you clean up the apt cache by removing `/var/lib/apt/lists` it
reduces the image size, since the apt cache is not stored in a layer. Since the
`RUN` statement starts with `apt-get update`, the package cache is always
refreshed prior to `apt-get install`.
{% endcomment %}
apt キャッシュをクリーンアップし `/var/lib/apt/lists` を削除するのは、イメージサイズを小さくするためです。
そもそも apt キャッシュはレイヤー内に保存されません。
`RUN` コマンドを `apt-get update` から始めているので、`apt-get install` の前に必ずパッケージのキャッシュが更新されることになります。

{% comment %}
> Official Debian and Ubuntu images [automatically run `apt-get clean`](https://github.com/moby/moby/blob/03e2923e42446dbb830c654d0eec323a0b4ef02a/contrib/mkimage/debootstrap#L82-L105),
> so explicit invocation is not required.
{% endcomment %}
> 公式の Debian と Ubuntu のイメージは[自動的に `apt-get clean` を実行する](https://github.com/moby/moby/blob/03e2923e42446dbb830c654d0eec323a0b4ef02a/contrib/mkimage/debootstrap#L82-L105)ので、明示的にこのコマンドを実行する必要はありません。

{% comment %}
#### Using pipes
{% endcomment %}
#### パイプの利用

{% comment %}
Some `RUN` commands depend on the ability to pipe the output of one command into another, using the pipe character (`|`), as in the following example:
{% endcomment %}
`RUN` コマンドの中には、その出力をパイプを使って他のコマンドへ受け渡すことを前提としているものがあります。
そのときにはパイプを行う文字（ ``|`` ）を使います。
たとえば以下のような例があります。

```dockerfile
RUN wget -O - https://some.site | wc -l > /number
```

{% comment %}
Docker executes these commands using the `/bin/sh -c` interpreter, which only
evaluates the exit code of the last operation in the pipe to determine success.
In the example above this build step succeeds and produces a new image so long
as the `wc -l` command succeeds, even if the `wget` command fails.
{% endcomment %}
Docker はこういったコマンドを `/bin/sh -c` というインタープリター実行により実現します。
正常処理されたかどうかは、パイプの最後の処理の終了コードにより評価されます。
上の例では、このビルド処理が成功して新たなイメージが生成されるかどうかは、`wc -l` コマンドの成功にかかっています。
つまり `wget` コマンドが成功するかどうかは関係がありません。

{% comment %}
If you want the command to fail due to an error at any stage in the pipe,
prepend `set -o pipefail &&` to ensure that an unexpected error prevents the
build from inadvertently succeeding. For example:
{% endcomment %}
パイプ内のどの段階でも、エラーが発生したらコマンド失敗としたい場合は、頭に `set -o pipefail &&` をつけて実行します。
こうしておくと、予期しないエラーが発生して、それに気づかずにビルドされてしまう、といったことはなくなります。
たとえば以下です。

```dockerfile
RUN set -o pipefail && wget -O - https://some.site | wc -l > /number
```
{% comment %}
> Not all shells support the `-o pipefail` option.
>
> In cases such as the `dash` shell on
> Debian-based images, consider using the _exec_ form of `RUN` to explicitly
> choose a shell that does support the `pipefail` option. For example:
>
> ```dockerfile
> RUN ["/bin/bash", "-c", "set -o pipefail && wget -O - https://some.site | wc -l > /number"]
> ```
{% endcomment %}
> すべてのシェルが ``-o pipefail`` オプションをサポートしているわけではありません。
> その場合（例えば Debian ベースのイメージにおけるデフォルトシェル ``dash`` である場合）、``RUN`` コマンドにおける **exec** 形式の利用を考えてみてください。
> これは ``pipefail`` オプションをサポートしているシェルを明示的に指示するものです。
> たとえば以下です。
>
> ```dockerfile
> RUN ["/bin/bash", "-c", "set -o pipefail && wget -O - https://some.site | wc -l > /number"]
> ```

### CMD

{% comment %}
[Dockerfile reference for the CMD instruction](../../engine/reference/builder.md#cmd)
{% endcomment %}
[Dockerfile リファレンスの CMD コマンド](../../engine/reference/builder.md#cmd)

{% comment %}
The `CMD` instruction should be used to run the software contained in your
image, along with any arguments. `CMD` should almost always be used in the form
of `CMD ["executable", "param1", "param2"…]`. Thus, if the image is for a
service, such as Apache and Rails, you would run something like `CMD
["apache2","-DFOREGROUND"]`. Indeed, this form of the instruction is recommended
for any service-based image.
{% endcomment %}
`CMD` コマンドは、イメージ内に含まれるソフトウェアを実行するために用いるもので、引数を指定して実行します。
`CMD` はほぼ、`CMD ["実行モジュール名", "引数1", "引数2"…]` の形式をとります。
Apache や Rails のようにサービスをともなうイメージに対しては、たとえば `CMD ["apache2","-DFOREGROUND"]` といったコマンド実行になります。
実際にサービスベースのイメージに対しては、この実行形式が推奨されます。

{% comment %}
In most other cases, `CMD` should be given an interactive shell, such as bash,
python and perl. For example, `CMD ["perl", "-de0"]`, `CMD ["python"]`, or `CMD
["php", "-a"]`. Using this form means that when you execute something like
`docker run -it python`, you’ll get dropped into a usable shell, ready to go.
`CMD` should rarely be used in the manner of `CMD ["param", "param"]` in
conjunction with [`ENTRYPOINT`](../../engine/reference/builder.md#entrypoint), unless
you and your expected users are already quite familiar with how `ENTRYPOINT`
works.
{% endcomment %}
上記以外では、`CMD` に対して bash、python、perl などインタラクティブシェルを与えることが行われます。
たとえば `CMD ["perl", "-de0"]`、`CMD ["python"]`、`CMD ["php", "-a"]` といった具合です。
この実行形式を利用するということは、たとえば `docker run -it python` というコマンドを実行したときに、指定したシェルの中に入り込んで、処理を進めていくことを意味します。
`CMD` と [`ENTRYPOINT`](../../engine/reference/builder.md#entrypoint) を組み合わせて用いる `CMD ["引数", "引数"]` という実行形式がありますが、これを利用するのはまれです。
開発者自身や利用者にとって `ENTRYPOINT` がどのように動作するのかが十分に分かっていないなら、用いないようにしましょう。

### EXPOSE

{% comment %}
[Dockerfile reference for the EXPOSE instruction](../../engine/reference/builder.md#expose)
{% endcomment %}
[Dockerfile リファレンスの EXPOSE コマンド](../../engine/reference/builder.md#expose)

{% comment %}
The `EXPOSE` instruction indicates the ports on which a container listens
for connections. Consequently, you should use the common, traditional port for
your application. For example, an image containing the Apache web server would
use `EXPOSE 80`, while an image containing MongoDB would use `EXPOSE 27017` and
so on.
{% endcomment %}
`EXPOSE` コマンドは、コンテナーが接続のためにリッスンするポートを指定します。
当然のことながらアプリケーションにおいては、標準的なポートを利用します。
たとえば Apache ウェブサーバーを含んでいるイメージに対しては `EXPOSE 80` を使います。
また MongoDB を含んでいれば `EXPOSE 27017` を使うことになります。

{% comment %}
For external access, your users can execute `docker run` with a flag indicating
how to map the specified port to the port of their choice.
For container linking, Docker provides environment variables for the path from
the recipient container back to the source (ie, `MYSQL_PORT_3306_TCP`).
{% endcomment %}
外部からアクセスできるようにするため、これを実行するユーザーは `docker run` にフラグをつけて実行します。
そのフラグとは、指定されているポートを、自分が取り決めるどのようなポートに割り当てるかを指示するものです。
Docker のリンク機能においては環境変数が利用できます。
受け側のコンテナーが提供元をたどることができるようにするものです（例: `MYSQL_PORT_3306_TCP` ）。

### ENV

{% comment %}
[Dockerfile reference for the ENV instruction](../../engine/reference/builder.md#env)
{% endcomment %}
[Dockerfile リファレンスの ENV コマンド](../../engine/reference/builder.md#env)

{% comment %}
To make new software easier to run, you can use `ENV` to update the
`PATH` environment variable for the software your container installs. For
example, `ENV PATH=/usr/local/nginx/bin:$PATH` ensures that `CMD ["nginx"]`
just works.
{% endcomment %}
新しいソフトウェアに対しては `ENV` を用いれば簡単にそのソフトウェアを実行できます。
コンテナーがインストールするソフトウェアに必要な環境変数 `PATH` を、この `ENV` を使って更新します。
たとえば `ENV PATH=/usr/local/nginx/bin:$PATH` を実行すれば、 `CMD ["nginx"]` が確実に動作するようになります。

{% comment %}
The `ENV` instruction is also useful for providing required environment
variables specific to services you wish to containerize, such as Postgres’s
`PGDATA`.
{% endcomment %}
`ENV` コマンドは、必要となる環境変数を設定するときにも利用します。
たとえば Postgres の `PGDATA` のように、コンテナー化したいサービスに固有の環境変数が設定できます。

{% comment %}
Lastly, `ENV` can also be used to set commonly used version numbers so that
version bumps are easier to maintain, as seen in the following example:
{% endcomment %}
また `ENV` は普段利用している各種バージョン番号を設定しておくときにも利用されます。
これによってバージョンを混同することなく、管理が容易になります。
たとえば以下がその例です。

```dockerfile
ENV PG_MAJOR=9.3
ENV PG_VERSION=9.3.4
RUN curl -SL http://example.com/postgres-$PG_VERSION.tar.xz | tar -xJC /usr/src/postgress && …
ENV PATH=/usr/local/postgres-$PG_MAJOR/bin:$PATH
```

{% comment %}
Similar to having constant variables in a program (as opposed to hard-coding
values), this approach lets you change a single `ENV` instruction to
auto-magically bump the version of the software in your container.
{% endcomment %}
プログラムにおける（ハードコーディングではない）定数定義と同じことで、この方法をとっておくのが便利です。
ただ１つの `ENV` コマンドを変更するだけで、コンテナー内のソフトウェアバージョンは、いとも簡単に変えてしまうことができるからです。

{% comment %}
Each `ENV` line creates a new intermediate layer, just like `RUN` commands. This
means that even if you unset the environment variable in a future layer, it
still persists in this layer and its value can't be dumped. You can test this by
creating a Dockerfile like the following, and then building it.
{% endcomment %}
それぞれの `ENV` 行からは新たな中間レイヤーが生成されます。
`RUN` コマンドと同じです。
ということはつまり、環境変数を先々のレイヤーにおいて無効化したとしても、その中間レイヤーに変数データは残ることになり、この値を消すことはできません。
このことを確認するには、以下のような Dockerfile を生成してビルドを行ってみればわかります。

```dockerfile
FROM alpine
ENV ADMIN_USER="mark"
RUN echo $ADMIN_USER > ./mark
RUN unset ADMIN_USER
```

```bash
$ docker run --rm test sh -c 'echo $ADMIN_USER'

mark
```

{% comment %}
To prevent this, and really unset the environment variable, use a `RUN` command
with shell commands, to set, use, and unset the variable all in a single layer.
You can separate your commands with `;` or `&&`. If you use the second method,
and one of the commands fails, the `docker build` also fails. This is usually a
good idea. Using `\` as a line continuation character for Linux Dockerfiles
improves readability. You could also put all of the commands into a shell script
and have the `RUN` command just run that shell script.
{% endcomment %}
このようにはならないように、つまり本当に環境変数を無効化したい場合には、シェルコマンドを用いた `RUN` コマンドを利用します。
そして環境変数への値設定、利用、無効化を、すべて 1 つのレイヤー内にて行うようにします。
各コマンドの区切りには `;` や `&&` を使います。
この 2 つめの方法をとった場合、コマンドが 1 つでも失敗すれば `docker build` も失敗します。
この方が適切なやり方です。
Linux における Dockerfile では行継続文字を表わす `\` を用いると、読みやすくなります。
あるいは実行する命令をすべてシェルスクリプトに書き入れて、`RUN` コマンドによってそのシェルスクリプトを実行するようなこともできます。

```dockerfile
FROM alpine
RUN export ADMIN_USER="mark" \
    && echo $ADMIN_USER > ./mark \
    && unset ADMIN_USER
CMD sh
```

```bash
$ docker run --rm test sh -c 'echo $ADMIN_USER'

```


{% comment %}
### ADD or COPY
{% endcomment %}
{: #add-or-copy }
### ADD と COPY

{% comment %}
- [Dockerfile reference for the ADD instruction](../../engine/reference/builder.md#add)
- [Dockerfile reference for the COPY instruction](../../engine/reference/builder.md#copy)
{% endcomment %}
- [Dockerfile リファレンスの ADD コマンド](../../engine/reference/builder.md#add)
- [Dockerfile リファレンスの COPY コマンド](../../engine/reference/builder.md#copy)

{% comment %}
Although `ADD` and `COPY` are functionally similar, generally speaking, `COPY`
is preferred. That’s because it’s more transparent than `ADD`. `COPY` only
supports the basic copying of local files into the container, while `ADD` has
some features (like local-only tar extraction and remote URL support) that are
not immediately obvious. Consequently, the best use for `ADD` is local tar file
auto-extraction into the image, as in `ADD rootfs.tar.xz /`.
{% endcomment %}
`ADD` と `COPY` の機能は似ていますが、一般的には `COPY` が選ばれます。
それは `ADD` よりも機能がはっきりしているからです。
`COPY` は単に、基本的なコピー機能を使ってローカルファイルをコンテナーにコピーするだけです。
一方 `ADD` には特定の機能（ローカルでの tar 展開やリモート URL サポート）があり、これはすぐにわかるものではありません。
結局 `ADD` の最も適切な利用場面は、ローカルの tar ファイルを自動的に展開してイメージに書き込むときです。
たとえば `ADD rootfs.tar.xz /` といったコマンドになります。

{% comment %}
If you have multiple `Dockerfile` steps that use different files from your
context, `COPY` them individually, rather than all at once. This ensures that
each step's build cache is only invalidated (forcing the step to be re-run) if
the specifically required files change.
{% endcomment %}
`Dockerfile` 内の複数ステップにおいて異なるファイルをコピーするときには、一度にすべてをコピーするのではなく、`COPY` を使って個別にコピーしてください。
こうしておくと、個々のステップに対するキャッシュのビルドは最低限に抑えることができます。
つまり指定されているファイルが変更になったときのみキャッシュが無効化されます（そのステップは再実行されます）。

{% comment %}
For example:
{% endcomment %}
例

```dockerfile
COPY requirements.txt /tmp/
RUN pip install --requirement /tmp/requirements.txt
COPY . /tmp/
```

{% comment %}
Results in fewer cache invalidations for the `RUN` step, than if you put the
`COPY . /tmp/` before it.
{% endcomment %}
`RUN` コマンドのステップより前に `COPY . /tmp/` を実行していたとしたら、それに比べて上の例はキャッシュ無効化の可能性が低くなっています。

{% comment %}
Because image size matters, using `ADD` to fetch packages from remote URLs is
strongly discouraged; you should use `curl` or `wget` instead. That way you can
delete the files you no longer need after they've been extracted and you don't
have to add another layer in your image. For example, you should avoid doing
things like:
{% endcomment %}
イメージサイズの問題があるので、`ADD` を用いてリモート URL からパッケージを取得することはやめてください。
かわりに `curl` や `wget` を使ってください。
こうしておくことで、ファイルを取得し展開した後や、イメージ内の他のレイヤーにファイルを加える必要がないのであれば、その後にファイルを削除することができます。
たとえば以下に示すのは、やってはいけない例です。

```dockerfile
ADD http://example.com/big.tar.xz /usr/src/things/
RUN tar -xJf /usr/src/things/big.tar.xz -C /usr/src/things
RUN make -C /usr/src/things all
```

{% comment %}
And instead, do something like:
{% endcomment %}
そのかわり、次のように記述します。

```dockerfile
RUN mkdir -p /usr/src/things \
    && curl -SL http://example.com/big.tar.xz \
    | tar -xJC /usr/src/things \
    && make -C /usr/src/things all
```

{% comment %}
For other items (files, directories) that do not require `ADD`’s tar
auto-extraction capability, you should always use `COPY`.
{% endcomment %}
`ADD` の自動展開機能を必要としないもの（ファイルやディレクトリ）に対しては、常に `COPY` を使うようにしてください。

### ENTRYPOINT

{% comment %}
[Dockerfile reference for the ENTRYPOINT instruction](../../engine/reference/builder.md#entrypoint)
{% endcomment %}
[Dockerfile リファレンスの ENTRYPOINT コマンド](../../engine/reference/builder.md#entrypoint)

{% comment %}
The best use for `ENTRYPOINT` is to set the image's main command, allowing that
image to be run as though it was that command (and then use `CMD` as the
default flags).
{% endcomment %}
`ENTRYPOINT` の最適な利用方法は、イメージに対してメインのコマンドを設定することです。
これを設定すると、イメージをそのコマンドそのものであるかのようにして実行できます（その次に `CMD` を使ってデフォルトフラグを指定します）。

{% comment %}
Let's start with an example of an image for the command line tool `s3cmd`:
{% endcomment %}
コマンドラインツール `s3cmd` のイメージ例から始めます。

```dockerfile
ENTRYPOINT ["s3cmd"]
CMD ["--help"]
```

{% comment %}
Now the image can be run like this to show the command's help:
{% endcomment %}
このイメージが実行されると、コマンドのヘルプが表示されます。

```bash
$ docker run s3cmd
```

{% comment %}
Or using the right parameters to execute a command:
{% endcomment %}
あるいは適正なパラメーターを指定してコマンドを実行します。

```bash
$ docker run s3cmd ls s3://mybucket
```

{% comment %}
This is useful because the image name can double as a reference to the binary as
shown in the command above.
{% endcomment %}
このコマンドのようにして、イメージ名がバイナリへの参照としても使えるので便利です。

{% comment %}
The `ENTRYPOINT` instruction can also be used in combination with a helper
script, allowing it to function in a similar way to the command above, even
when starting the tool may require more than one step.
{% endcomment %}
`ENTRYPOINT` コマンドはヘルパースクリプトとの組み合わせにより利用することもできます。
そのスクリプトは、上記のコマンド例と同じように機能させられます。
たとえ対象ツールの起動に複数ステップを要するような場合でも、それが可能です。

{% comment %}
For example, the [Postgres Official Image](https://hub.docker.com/_/postgres/)
uses the following script as its `ENTRYPOINT`:
{% endcomment %}
たとえば [Postgres 公式イメージ](https://hub.docker.com/_/postgres/) では `ENTRYPOINT` として以下のスクリプトを利用しています。

```bash
#!/bin/bash
set -e

if [ "$1" = 'postgres' ]; then
    chown -R postgres "$PGDATA"

    if [ -z "$(ls -A "$PGDATA")" ]; then
        gosu postgres initdb
    fi

    exec gosu postgres "$@"
fi

exec "$@"
```

{% comment %}
> Configure app as PID 1
>
> This script uses [the `exec` Bash command](http://wiki.bash-hackers.org/commands/builtin/exec)
> so that the final running application becomes the container's PID 1. This
> allows the application to receive any Unix signals sent to the container.
> For more, see the [`ENTRYPOINT` reference](../../engine/reference/builder.md#entrypoint).
{% endcomment %}
> アプリを PID 1 として実行
>
> このスクリプトでは [Bash の `exec` コマンド](http://wiki.bash-hackers.org/commands/builtin/exec) を利用しています。
> このため最後に実行されるアプリケーションが、コンテナーの PID 1 になります。
> したがってコンテナーに送信される Unix シグナルは、そのアプリケーションが受け取ることになります。
> 詳しくは [`ENTRYPOINT` リファレンス](../../engine/reference/builder.md#entrypoint) を参照してください。

{% comment %}
The helper script is copied into the container and run via `ENTRYPOINT` on
container start:
{% endcomment %}
ヘルパースクリプトはコンテナーの中にコピーされ、コンテナー開始時に `ENTRYPOINT` から実行されます。

```dockerfile
COPY ./docker-entrypoint.sh /
ENTRYPOINT ["/docker-entrypoint.sh"]
CMD ["postgres"]
```

{% comment %}
This script allows the user to interact with Postgres in several ways.
{% endcomment %}
このスクリプトを用いると、Postgres との間で、ユーザーがいろいろな方法でやり取りできるようになります。

{% comment %}
It can simply start Postgres:
{% endcomment %}
以下は単純に Postgres を起動します。

```bash
$ docker run postgres
```

{% comment %}
Or, it can be used to run Postgres and pass parameters to the server:
{% endcomment %}
あるいは PostgreSQL 実行時にサーバーに対してパラメーターを渡すことができます。

```bash
$ docker run postgres postgres --help
```

{% comment %}
Lastly, it could also be used to start a totally different tool, such as Bash:
{% endcomment %}
または Bash のような全く異なるツールを起動するために利用することもできます。

```bash
$ docker run --rm -it postgres bash
```

### VOLUME

{% comment %}
[Dockerfile reference for the VOLUME instruction](../../engine/reference/builder.md#volume)
{% endcomment %}
[Dockerfile リファレンスの VOLUME コマンド](../../engine/reference/builder.md#volume)

{% comment %}
The `VOLUME` instruction should be used to expose any database storage area,
configuration storage, or files/folders created by your docker container. You
are strongly encouraged to use `VOLUME` for any mutable and/or user-serviceable
parts of your image.
{% endcomment %}
`VOLUME` コマンドは、データベースストレージ領域、設定用ストレージ、Docker コンテナーによって作成されるファイルやフォルダの公開に使います。
イメージの可変的な部分、あるいはユーザーが設定可能な部分については VOLUME の利用が強く推奨されます。

### USER

{% comment %}
[Dockerfile reference for the USER instruction](../../engine/reference/builder.md#user)
{% endcomment %}
[Dockerfile リファレンスの USER コマンド](../../engine/reference/builder.md#user)

{% comment %}
If a service can run without privileges, use `USER` to change to a non-root
user. Start by creating the user and group in the `Dockerfile` with something
like `RUN groupadd -r postgres && useradd --no-log-init -r -g postgres postgres`.
{% endcomment %}
サービスが特権ユーザーでなくても実行できる場合は、`USER` を用いて非 root ユーザーに変更します。
ユーザーとグループを生成するところから始めてください。
`Dockerfile` 内にてたとえば `RUN groupadd -r postgres && useradd -r -g postgres postgres` のようなコマンドを実行します。

{% comment %}
> Consider an explicit UID/GID
>
> Users and groups in an image are assigned a non-deterministic UID/GID in that
> the "next" UID/GID is assigned regardless of image rebuilds. So, if it’s
> critical, you should assign an explicit UID/GID.
{% endcomment %}
> 明示的な UID、GID 指定
>
> イメージ内のユーザとグループに割り当てられる UID、GID は確定的なものではありません。
> イメージが再構築されるかどうかには関係なく、「次の」値が UID、GID に割り当てられます。
> これが問題となる場合は、UID、GID を明示的に割り当ててください。

{% comment %}
> Due to an [unresolved bug](https://github.com/golang/go/issues/13548) in the
> Go archive/tar package's handling of sparse files, attempting to create a user
> with a significantly large UID inside a Docker container can lead to disk
> exhaustion because `/var/log/faillog` in the container layer is filled with
> NULL (\0) characters. A workaround is to pass the `--no-log-init` flag to
> useradd. The Debian/Ubuntu `adduser` wrapper does not support this flag.
{% endcomment %}
> Go 言語の archive/tar パッケージが取り扱うスパースファイルにおいて
> [未解決のバグ](https://github.com/golang/go/issues/13548)があります。
> これは Docker コンテナー内にて非常に大きな値の UID を使ってユーザーを生成しようとするため、ディスク消費が異常に発生します。
> コンテナーレイヤー内の `/var/log/faillog` が NUL (\0) キャラクターにより埋められてしまいます。
> useradd に対して `--no-log-init` フラグをつけることで、とりあえずこの問題は回避できます。
> ただし Debian/Ubuntu の `adduser` ラッパーは `--no-log-init` フラグをサポートしていないため、利用することはできません。

{% comment %}
Avoid installing or using `sudo` as it has unpredictable TTY and
signal-forwarding behavior that can cause problems. If you absolutely need
functionality similar to `sudo`, such as initializing the daemon as `root` but
running it as non-`root`, consider using [“gosu”](https://github.com/tianon/gosu).
{% endcomment %}
`sudo` のインストールやその利用は避けてください。
これは予期できない TTY 動作やシグナルフォワーディングが起こってしまい問題となるためです。
たとえば デーモンは `root` で初期化するものの、実行は `root` 以外で行うといったように、`sudo` と同等の機能が本当に必要なのであれば、[“gosu”](https://github.com/tianon/gosu) の利用を検討してみてください。

{% comment %}
Lastly, to reduce layers and complexity, avoid switching `USER` back and forth
frequently.
{% endcomment %}
最後に、レイヤー数を減らしてわかりやすいものとなるように、`USER` を何度も切り替えるようなことは避けてください。

### WORKDIR

{% comment %}
[Dockerfile reference for the WORKDIR instruction](../../engine/reference/builder.md#workdir)
{% endcomment %}
[Dockerfile リファレンスの WORKDIR コマンド](../../engine/reference/builder.md#workdir)

{% comment %}
For clarity and reliability, you should always use absolute paths for your
`WORKDIR`. Also, you should use `WORKDIR` instead of  proliferating instructions
like `RUN cd … && do-something`, which are hard to read, troubleshoot, and
maintain.
{% endcomment %}
`WORKDIR` に設定するパスは、わかりやすく確実なものとするために、絶対パス指定としてください。
また `RUN cd … && do-something` といった長くなる一方のコマンドを書くくらいなら、`WORKDIR` を利用してください。
そのような書き方は読みにくいため、トラブル発生時には解決を遅らせ保守が困難になるためです。

### ONBUILD

{% comment %}
[Dockerfile reference for the ONBUILD instruction](../../engine/reference/builder.md#onbuild)
{% endcomment %}
[Dockerfile リファレンスの ONBUILD コマンド](../../engine/reference/builder.md#onbuild)

{% comment %}
An `ONBUILD` command executes after the current `Dockerfile` build completes.
`ONBUILD` executes in any child image derived `FROM` the current image.  Think
of the `ONBUILD` command as an instruction the parent `Dockerfile` gives
to the child `Dockerfile`.
{% endcomment %}
`ONBUILD` コマンドは、`Dockerfile` によるビルドが完了した後に実行されます。
`ONBUILD` は、現在のイメージから `FROM` によって派生した子イメージにおいて実行されます。
つまり `ONBUILD` とは、親の `Dockerfile` から子どもの `Dockerfile` へ与える命令であると言えます。

{% comment %}
A Docker build executes `ONBUILD` commands before any command in a child
`Dockerfile`.
{% endcomment %}
Docker によるビルドにおいては `ONBUILD` の実行が済んでから、子イメージのコマンド実行が行われます。

{% comment %}
`ONBUILD` is useful for images that are going to be built `FROM` a given
image. For example, you would use `ONBUILD` for a language stack image that
builds arbitrary user software written in that language within the
`Dockerfile`, as you can see in [Ruby’s `ONBUILD` variants](https://github.com/docker-library/ruby/blob/c43fef8a60cea31eb9e7d960a076d633cb62ba8d/2.4/jessie/onbuild/Dockerfile).
{% endcomment %}
`ONBUILD` は、所定のイメージから `FROM` を使ってイメージをビルドしようとするときに利用できます。
たとえば特定言語のスタックイメージは `ONBUILD` を利用します。
`Dockerfile` 内にて、その言語で書かれたどのようなユーザーソフトウェアであってもビルドすることができます。
その例として [Ruby's ONBUILD variants](https://github.com/docker-library/ruby/blob/c43fef8a60cea31eb9e7d960a076d633cb62ba8d/2.4/jessie/onbuild/Dockerfile) があります。

{% comment %}
Images built with `ONBUILD` should get a separate tag, for example:
`ruby:1.9-onbuild` or `ruby:2.0-onbuild`.
{% endcomment %}
`ONBUILD` によって構築するイメージは、異なったタグを指定してください。
たとえば `ruby:1.9-onbuild` と `ruby:2.0-onbuild` などです。

{% comment %}
Be careful when putting `ADD` or `COPY` in `ONBUILD`. The "onbuild" image
fails catastrophically if the new build's context is missing the resource being
added. Adding a separate tag, as recommended above, helps mitigate this by
allowing the `Dockerfile` author to make a choice.
{% endcomment %}
`ONBUILD` において `ADD` や `COPY` を用いるときは注意してください。
"onbuild" イメージが新たにビルドされる際に、追加しようとしているリソースが見つからなかったとしたら、このイメージは復旧できない状態になります。上に示したように個別にタグをつけておけば、`Dockerfile` の開発者にとっても判断ができるようになるので、不測の事態は軽減されます。

{% comment %}
## Examples for Official Images
{% endcomment %}
## 公式イメージの例

{% comment %}
These Official Images have exemplary `Dockerfile`s:
{% endcomment %}
以下に示すのは代表的な `Dockerfile` の例です。

* [Go](https://hub.docker.com/_/golang/)
* [Perl](https://hub.docker.com/_/perl/)
* [Hy](https://hub.docker.com/_/hylang/)
* [Ruby](https://hub.docker.com/_/ruby/)

{% comment %}
## Additional resources:
{% endcomment %}
## その他の情報

{% comment %}
* [Dockerfile Reference](../../engine/reference/builder.md)
* [More about Base Images](baseimages.md)
* [More about Automated Builds](../../docker-hub/builds/index.md)
* [Guidelines for Creating Official Images](../../docker-hub/official_images.md)
{% endcomment %}
* [Dockerfile リファレンス](../../engine/reference/builder.md)
* [ベースイメージの詳細](baseimages.md)
* [自動ビルドの詳細](../../docker-hub/builds/index.md)
* [公式イメージ作成のガイドライン](../../docker-hub/official_images.md)

