---
description: Docker Hub 上のリポジトリの利用
keywords: Docker, docker, trusted, registry, accounts, plans, Dockerfile, Docker Hub, webhooks, docs, documentation
title: リポジトリ
---

{% comment %}
Docker Hub repositories allow you share container images with your team,
customers, or the Docker community at large.
{% endcomment %}
Docker Hub リポジトリは、開発チーム、顧客、Docker コミュニティ全般に対してコンテナーの共有を可能にします。

{% comment %}
Docker images are pushed to Docker Hub through the [`docker push`](https://docs.docker.com/engine/reference/commandline/push/) command. A single Docker Hub repository can hold many Docker images (stored as **tags**).
{% endcomment %}
Docker イメージは Docker Hub に対して [`docker push`](https://docs.docker.com/engine/reference/commandline/push/) コマンドを使ってプッシュします。
1 つの Docker Hub リポジトリ内には複数の Docker イメージを含めることができます（**タグ**を使って保存します）。

{% comment %}
## Creating Repositories
{% endcomment %}
## リポジトリの生成
{: #creating-repositories }

{% comment %}
To create a repository, sign into Docker Hub, click on **Repositories** then **Create Repo**:
{% endcomment %}
リポジトリを生成するには Docker Hub にサインインして、**Repositories** をクリックし、さらに **Create Repo** をクリックします。

{% comment %}
![Create repo](images/repos-create.png)
{% endcomment %}
![リポジトリの生成](images/repos-create.png)

{% comment %}
When creating a new repository, you can choose to put it in your Docker ID
namespace, or that of any [Organization](/docker-hub/orgs.md) that you are in the "Owners"
team. The Repository Name needs to be unique in that namespace, can be two
to 255 characters, and can only contain lowercase letters, numbers or `-` and
`_`.
{% endcomment %}
リポジトリを新規に生成する際には、そのリポジトリをどこに置くかを選択することができます。
1 つは自身の Docker ID 名前空間内です。
もう 1 つは [組織](/docker-hub/orgs.md) の名前空間であって、「所有者」（Owners）チームに属している場合です。
リポジトリ名は、その名前空間内においてユニークである必要があります。
そして 2 文字以上 255 文字までで構成され、英小文字、数字、`-`、`_` を用いることができます。

{% comment %}
The "Short Description" of 100 characters is used in the search results,
while the "Full Description" can be used as the Readme for the repository, and
can use Markdown to add simple formatting.
{% endcomment %}
「Short Description」（簡易説明）の 100 文字は検索結果に表示されます。
「Full Description」（詳細説明）は、リポジトリの Readme として利用できます。
またマークダウンを使って簡単な書式を加えることができます。

{% comment %}
After you hit the "Create" button, you then need to `docker push` images to that
Hub based repository.
{% endcomment %}
「Create」ボタンをクリックした後は、`docker push` を実行してこの Hub ベースのリポジトリに対してイメージをプッシュすることが必要です。

{% comment %}
## Pushing a Docker container image to Docker Hub
{% endcomment %}
## Docker コンテナーイメージの Docker Hub へのプッシュ
{: #pushing-a-docker-container-image-to-docker-hub }

{% comment %}
To push a repository to the Docker Hub, you must
name your local image using your Docker Hub username, and the
repository name that you created through Docker Hub on the web.
{% endcomment %}
リポジトリを Docker Hub へプッシュするには、利用している Docker Hub ユーザー名を使って、ローカルイメージに名前をつける必要があります。
さらにウェブ上の Docker Hub 画面を通じて生成したリポジトリ名を使う必要があります。

{% comment %}
You can add multiple images to a repository, by adding a specific `:<tag>` to
it (for example `docs/base:testing`). If it's not specified, the tag defaults to
`latest`.
{% endcomment %}
リポジトリへは複数のイメージを追加することができます。
その際にはイメージを特定するタグを `:<tag>` のようにつけます（たとえば `docs/base:testing`）。
このタグ指定を行わなかった場合、タグにはデフォルトとして `latest` がつきます。

{% comment %}
You can name your local images either when you build it, using
`docker build -t <hub-user>/<repo-name>[:<tag>]`,
by re-tagging an existing local image `docker tag <existing-image> <hub-user>/<repo-name>[:<tag>]`,
or by using `docker commit <existing-container> <hub-user>/<repo-name>[:<tag>]` to commit
changes.
{% endcomment %}
ローカルイメージに名前をつけるのは、ビルド時には `docker build -t <hub-user>/<repo-name>[:<tag>]` のようにして行いますが、すでにあるローカルイメージに対しては `docker tag <existing-image> <hub-user>/<repo-name>[:<tag>]`、またコミットの際には `docker commit <existing-container> <hub-user>/<repo-name>[:<tag>]` としてタグ名を再設定することができます。

{% comment %}
Now you can push this repository to the registry designated by its name or tag.
{% endcomment %}
このリポジトリを、名前またはタグにより指定したレジストリにプッシュします。

    $ docker push <hub-user>/<repo-name>:<tag>

{% comment %}
The image is then uploaded and available for use by your teammates and/or
the community.
{% endcomment %}
そのイメージがアップロードされます。
こうして開発チームメンバーやコミュニティが利用できるようになります。

{% comment %}
## Private Repositories
{% endcomment %}
## プライベートリポジトリ
{: #private-repositories }

{% comment %}
Private repositories allow you keep container images private, either to your own account or within an organization or
team.
{% endcomment %}
プライベートリポジトリは、コンテナーイメージをプライベートに管理します。
自身のアカウント用でも、また組織や開発チーム用でも管理可能です。

{% comment %}
To create a private repo select **Private** when creating a private repo:
{% endcomment %}
プライベートリポジトリを生成するには、リポジトリ生成時に **Private** を選択します。

{% comment %}
![Create Private Repo](images/repo-create-private.png)
{% endcomment %}
![プライベートリポジトリの生成](images/repo-create-private.png)

{% comment %}
You can also make an existing repository private by going to the repo's **Settings** tab:
{% endcomment %}
既存のリポジトリをプライベートに変更することもできます。
これはリポジトリの **Settings** タブにて行います。

{% comment %}
![Convert Repo to Private](images/repo-make-private.png)
{% endcomment %}
![リポジトリをプライベートに変更](images/repo-make-private.png)

{% comment %}
You get one private repository for free with your Docker Hub user account (not usable for
organizations you're a member of). If you need more private repositories for your user account, upgrade
your Docker Hub plan from your [Billing Information](https://hub.docker.com/billing/plan) page.
{% endcomment %}
Docker Hub の 1 ユーザーアカウントにおいては、無料で 1 つのプライベートリポジトリを生成することができます（これはメンバーとなっている組織に対しては利用できません）。
ユーザーアカウントにおいて、それ以上のプライベートリポジトリを必要とする場合は、[課金情報](https://hub.docker.com/billing/plan) のページから Docker Hub プランをアップグレードしてください。

{% comment %}
Once the private repository is created, you can `push` and `pull` images to and
from it using Docker.
{% endcomment %}
プライベートリポジトリを生成したら Docker を利用して、イメージの `push` や `pull` ができるようになります。

{% comment %}
> **Note**: You need to be signed in and have access to work with a
> private repository.
{% endcomment %}
> **メモ**: プライベートリポジトリにアクセスして作業を進めるには、サインインが必要です。

{% comment %}
> **Note**: Private repositories are not currently available to search through the
top-level search or `docker search`
{% endcomment %}
> **メモ**: プライベートリポジトリは現在のところ、トップレベルの検索や `docker search` を行うことはできません。

{% comment %}
You can designate collaborators and manage their access to a private
repository from that repository's *Settings* page. You can also toggle the
repository's status between public and private, if you have an available
repository slot open. Otherwise, you can upgrade your
[Docker Hub](https://hub.docker.com/account/billing-plans/) plan.
{% endcomment %}
プライベートリポジトリに対してはコラボレーター（collaborator）を指定し、リポジトリへのアクセスを管理することができます。
これはリポジトリの *Settings* ページから行います。
またリポジトリスロットに空きがある場合は、リポジトリの状態を公開とするかプライベートとするかを切り替えることもできます。
空きスロットがない場合は、[Docker Hub](https://hub.docker.com/account/billing-plans/) のプランをアップグレードすることになります。

{% comment %}
## Collaborators and their role
{% endcomment %}
## コラボレーターとそのロール
{: #collaborators-and-their-role }

{% comment %}
A collaborator is someone you want to give access to a private repository. Once
designated, they can `push` and `pull` to your repositories. They are not
allowed to perform any administrative tasks such as deleting the repository or
changing its status from private to public.
{% endcomment %}
コラボレーター（collaborator）とは、プライベートリポジトリに対してアクセスする許可を与えた人のことです。
この許可を与えたら、コラボレーターはプライベートリポジトリに対して `push` や `pull` を行えるようになります。
ただしコラボレーターは、管理操作を実行する権限までは与えられません。
したがってリポジトリを削除したり、リポジトリをプライベートから公開に変更したりするようなことはできません。

{% comment %}
> **Note**:
> A collaborator cannot add other collaborators. Only the owner of
> the repository has administrative access.
{% endcomment %}
> **メモ**:
> コラボレーターが他のコラボレーターを追加することはできません。
> リポジトリに対する管理アクセスが可能なのは、その所有者のみです。

{% comment %}
You can also assign more granular collaborator rights ("Read", "Write", or
"Admin") on Docker Hub by using organizations and teams. For more information
see the [organizations documentation](/docker-hub/orgs.md).
{% endcomment %}
Docker Hub 上において組織とチームに関する機能を利用すれば、コラボレーターのより詳細な権限（「読み込み」、「書き込み」、「管理」）を割り当てることが可能になります。
詳細は [組織に関するドキュメント](/docker-hub/orgs.md) を参照してください。


{% comment %}
## Viewing repository tags
{% endcomment %}
## リポジトリタグの参照
{: #viewing-repository-tags }

{% comment %}
Docker Hub's individual repositories view shows you the available tags and the
size of the associated image. Go to the "Repositories" view and click on a
repository to see its tags.
{% endcomment %}
それぞれの Docker Hub リポジトリでは、利用可能なタグと関連イメージのサイズが確認できます。
「Repositories」画面を表示し、リポジトリの 1 つをクリックしてそのタグを確認します。

{% comment %}
![Repository View](images/repo-view-2019.png)
{% endcomment %}
![リポジトリ画面](images/repo-view-2019.png)

{% comment %}
![View Repo Tags](images/repos-tags-view-2019.png)
{% endcomment %}
![リポジトリタグの参照](images/repos-tags-view-2019.png)

{% comment %}
Image sizes are the cumulative space taken up by the image and all its parent
images. This is also the disk space used by the contents of the .tar file
created when you `docker save` an image.
{% endcomment %}
イメージサイズは、そのイメージと親イメージを含んだすべての容量です。
これはイメージを保存する際に `docker save` によって生成される .tar ファイルの容量でもあります。

{% comment %}
To view tags, click on "Tags" tab and then select a tag to view.
{% endcomment %}
タグの参照は「Tags」タブをクリックして、参照したいタグを選びます。

{% comment %}
![Manage Repo Tags](images/repos-tags-manage-2019.png)
{% endcomment %}
![リポジトリタグの管理](images/repos-tags-manage-2019.png)

{% comment %}
![View Tag](images/repo-single-tag-view-2019.png)
{% endcomment %}
![タグの参照](images/repo-single-tag-view-2019.png)

{% comment %}
## Searching for Repositories
{% endcomment %}
## リポジトリの検索
{: #searching-for-repositories }

{% comment %}
You can search the [Docker Hub](https://hub.docker.com) registry through its search
interface or by using the command line interface. Searching can find images by
image name, user name, or description:
{% endcomment %}
[Docker Hub](https://hub.docker.com) のレジストリは、検索インターフェース、あるいはコマンドラインインターフェースを使って検索することができます。
検索の際には、イメージ名、ユーザー名、説明を用いて検索することができます。

    $ docker search centos
    NAME                                 DESCRIPTION                                     STARS     OFFICIAL   AUTOMATED
    centos                               The official build of CentOS.                   1034      [OK]
    ansible/centos7-ansible              Ansible on Centos7                              43                   [OK]
    tutum/centos                         Centos image with SSH access. For the root...   13                   [OK]
    ...

{% comment %}
There you can see two example results: `centos` and `ansible/centos7-ansible`.
The second result shows that it comes from the public repository of a user,
named `ansible/`, while the first result, `centos`, doesn't explicitly list a
repository which means that it comes from the top-level namespace for [Official
Images](/docker-hub/official_images.md). The `/` character separates a user's
repository from the image name.
{% endcomment %}
上では `centos` と `ansible/centos7-ansible` という 2 つの結果が示されました。
2 つめの結果は、`ansible/` というユーザーが所有する公開リポジトリであることがわかります。
その一方、1 つめの結果 である `centos` にはリポジトリに関する情報が明確には示されていません。
というのも 1 つめは [公式イメージ](/docker-hub/official_images.md) における最上位の名前空間であることを意味しています。
文字 `/` は、ユーザーのリポジトリ名とイメージ名を分けるためのものです。

{% comment %}
Once you've found the image you want, you can download it with `docker pull <imagename>`:
{% endcomment %}
目的のイメージが見つかったら `docker pull <imagename>` によってダウンロードします。

    $ docker pull centos
    latest: Pulling from centos
    6941bfcbbfca: Pull complete
    41459f052977: Pull complete
    fd44297e2ddb: Already exists
    centos:latest: The image you are pulling has been verified. Important: image verification is a tech preview feature and should not be relied on to provide security.
    Digest: sha256:d601d3b928eb2954653c59e65862aabb31edefa868bd5148a41fa45004c12288
    Status: Downloaded newer image for centos:latest

{% comment %}
You now have an image from which you can run containers.
{% endcomment %}
イメージを入手したので、ここからコンテナーを実行することができます。


{% comment %}
## Starring Repositories
{% endcomment %}
## リポジトリへの星マークづけ
{: #starring-repositories }

{% comment %}
Your repositories can be starred and you can star repositories in return. Stars
are a way to show that you like a repository. They are also an easy way of
bookmarking your favorites.
{% endcomment %}
リポジトリは他の方から星マークをつけてもらうことがあり、逆に他のリポジトリへ星マークをつけることができます。
星マークをつけるのは、好きなリポジトリが何かを公開する方法として使えます。
またお気に入りのリポジトリをブックマークしておくということもできます。
