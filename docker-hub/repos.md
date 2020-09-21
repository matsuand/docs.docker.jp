---
description: Docker Hub 上のリポジトリを利用する。
keywords: Docker, docker, trusted, registry, accounts, plans, Dockerfile, Docker Hub, webhooks, docs, documentation
title: リポジトリ
---

{% comment %}
Docker Hub repositories allow you share container images with your team,
customers, or the Docker community at large.
{% endcomment %}
Docker Hub リポジトリは、開発チーム、顧客、Docker コミュニティ全般に対してコンテナーの共有を可能にします。

{% comment %}
Docker images are pushed to Docker Hub through the [`docker push`](https://docs.docker.com/engine/reference/commandline/push/)
command. A single Docker Hub repository can hold many Docker images (stored as
**tags**).
{% endcomment %}
Docker イメージは Docker Hub に対して [`docker push`](https://docs.docker.com/engine/reference/commandline/push/) コマンドを使ってプッシュします。
1 つの Docker Hub リポジトリ内には複数の Docker イメージを含めることができます（**タグ**を使って保存します）。

{% comment %}
## Creating repositories
{% endcomment %}
{: #creating-repositories }
## リポジトリの生成

{% comment %}
To create a repository, sign into Docker Hub, click on **Repositories** then
**Create Repository**:
{% endcomment %}
リポジトリを生成するには Docker Hub にサインインして、**Repositories** をクリックし、さらに **Create Repository** をクリックします。

{% comment %}
![Create repo](images/repos-create.png)
{% endcomment %}
![リポジトリの生成](images/repos-create.png)

{% comment %}
When creating a new repository:
{% endcomment %}
リポジトリの新規生成時には以下を行います。

{% comment %}
 * You can choose to put it in your Docker ID
namespace, or in any [organization](orgs.md) where you are an
[_owner_](orgs.md#the-owners-team).
{% endcomment %}
 * リポジトリを Docker ID 名前空間に含めるか、自身が [**所有者**](orgs.md#the-owners-team) となっている [組織](orgs.md) に含めるかを選びます。

{% comment %}
* The repository name needs to be unique in that namespace, can be two
to 255 characters, and can only contain lowercase letters, numbers or `-` and
`_`.
{% endcomment %}
* リポジトリ名は、これが属する名前空間内においてユニークである必要があります。
  文字数は 2 文字以上 255 文字までです。
  利用できる文字は、英小文字、数字、`-`、`_` です。

{% comment %}
* The description can be up to 100 characters and is used in the search
result.
{% endcomment %}
* 内容説明を 100 文字以内で記述できます。
  これは検索を行った際に、結果として表示されます。

{% comment %}
* You can link a GitHub or Bitbucket account now, or choose to do it
later in the repository settings.
{% endcomment %}
* この場ですぐに GitHub や Bitbucket のアカウントにリンクさせることができます。
  またはリポジトリ設定において、後で行うこともできます。

![リポジトリ生成の設定ページ](images/repo-create-details.png)

{% comment %}
After you hit the **Create** button, you can start using `docker push` to push
images to this repository.
{% endcomment %}
**Create** ボタンをクリックした後は、`docker push` を利用してこのリポジトリに対してイメージをプッシュしていくことができます。

{% comment %}
## Pushing a Docker container image to Docker Hub
{% endcomment %}
{: #pushing-a-docker-container-image-to-docker-hub }
## Docker コンテナーイメージの Docker Hub へのプッシュ

{% comment %}
To push an image to Docker Hub, you must first name your local image using your
Docker Hub username and the repository name that you created through Docker Hub
on the web.
{% endcomment %}
Docker Hub にイメージをプッシュするには、Docker Hub のユーザー名と、ウェブ画面を通じて Docker Hub 上に生成したリポジトリ名を使って、まずはローカルイメージに名前をつける必要があります。


{% comment %}
You can add multiple images to a repository by adding a specific `:<tag>` to
them (for example `docs/base:testing`). If it's not specified, the tag defaults
to `latest`.
{% endcomment %}
リポジトリへは複数のイメージを追加することができます。
これにはイメージに対して、特定の`:<tag>`というものを加えます。
これが指定されていない場合、タグのデフォルト名は`latest`になります。

{% comment %}
Name your local images using one of these methods:
* When you build them, using
`docker build -t <hub-user>/<repo-name>[:<tag>]`
{% endcomment %}
ローカルイメージに名前をつけるには、以下の 3 つの方法の中から選びます。
* イメージをビルドする際に`docker build -t <hub-user>/<repo-name>[:<tag>]`を実行します。

{% comment %}
* By re-tagging an existing local image `docker tag <existing-image> <hub-user>/<repo-name>[:<tag>]`
{% endcomment %}
* ローカルイメージに改めてタグをつける`docker tag <existing-image> <hub-user>/<repo-name>[:<tag>]`を実行します。

{% comment %}
* By using `docker commit <existing-container> <hub-user>/<repo-name>[:<tag>]`
to commit changes
{% endcomment %}
* `docker commit <existing-container> <hub-user>/<repo-name>[:<tag>]`を実行して変更をコミットします。

{% comment %}
Now you can push this repository to the registry designated by its name or tag.
{% endcomment %}
そこでこのリポジトリを、名前およびタグによって指定されるレジストリへプッシュします。

    $ docker push <hub-user>/<repo-name>:<tag>

{% comment %}
The image is then uploaded and available for use by your teammates and/or
the community.
{% endcomment %}
イメージがアップロードされて、チームメンバーやコミュニティが利用できるようになりました。

{% comment %}
## Private repositories
{% endcomment %}
{: #private-repositories }
## プライベートリポジトリ

{% comment %}
Private repositories let you keep container images private, either to your
own account or within an organization or team.
{% endcomment %}
プライベートリポジトリは、コンテナーイメージをプライベートなものにします。
これは自身のアカウントだけでなく、組織やチーム内においてもプライベートなものになります。

{% comment %}
To create a private repository, select **Private** when creating a repository:
{% endcomment %}
プライベートリポジトリを生成するには、リポジトリ生成の操作時に **Private** を選びます。

{% comment %}
![Create Private Repo](images/repo-create-private.png)
{% endcomment %}
![プライベートリポジトリの生成](images/repo-create-private.png)

{% comment %}
You can also make an existing repository private by going to its **Settings** tab:
{% endcomment %}
**Settings** タブにおいては、既存のリポジトリをプライベートに変更することもできます。

{% comment %}
![Convert Repo to Private](images/repo-make-private.png)
{% endcomment %}
![リポジトリをプライベートに変更](images/repo-make-private.png)

{% comment %}
You get one private repository for free with your Docker Hub user account (not
usable for organizations you're a member of). If you need more private
repositories for your user account, upgrade your Docker Hub plan from your
[Billing Information](https://hub.docker.com/billing/plan) page.
{% endcomment %}
Docker Hub ユーザーアカウントに対して、プライベートリポジトリは無償で 1 つだけ生成することができます。
（メンバーとして所属している組織からは利用できません。）
ユーザーアカウントにおいて、これ以上のプライベートリポジトリを必要とする場合は、[有料プランの情報](https://hub.docker.com/billing/plan) ページから Docker Hub プランをアップグレードすることが必要になります。

{% comment %}
Once the private repository is created, you can `push` and `pull` images to and
from it using Docker.
{% endcomment %}
プライベートリポジトリが生成できたら、Docker を使ってイメージの`push`や`pull`を行うことができます。

{% comment %}
> **Note**: You need to be signed in and have access to work with a
> private repository.
{% endcomment %}
> **メモ**: プライベートリポジトリにアクセスして操作を行っていくには、サインインしておくことが必要です。

{% comment %}
> **Note**: Private repositories are not currently available to search through
> the top-level search or `docker search`.
{% endcomment %}
> **メモ**: プライベートリポジトリは今のところ、トップレベルの検索、つまり`docker search`を利用した検索は利用できません。

{% comment %}
You can designate collaborators and manage their access to a private
repository from that repository's **Settings** page. You can also toggle the
repository's status between public and private, if you have an available
repository slot open. Otherwise, you can upgrade your
[Docker Hub](https://hub.docker.com/account/billing-plans/) plan.
{% endcomment %}
プライベートリポジトリでは、協力者（collaborator）を設定して、リポジトリへのアクセスを管理することができます。
これはリポジトリの **Settings** ページから行います。
またリポジトリの状態は、利用可能なスロット数があれば、パブリックとプライベートの間で切り替えることもできます。
スロット数が足りない場合、[Docker Hub](https://hub.docker.com/account/billing-plans/) プランをアップグレードすることができます。

{% comment %}
## Collaborators and their role
{% endcomment %}
{: #collaborators-and-their-role }
## 協力者とその役割

{% comment %}
A collaborator is someone you want to give access to a private repository. Once
designated, they can `push` and `pull` to your repositories. They are not
allowed to perform any administrative tasks such as deleting the repository or
changing its status from private to public.
{% endcomment %}
協力者（collaborator）とは、プライベートリポジトリへのアクセスを許可する他ユーザーのことです。
これを設定すると、協力者もリポジトリに対して`push`と`pull`ができるようになります。
ただし協力者になったからといって、リポジトリを削除したり、プライベートリポジトリをパブリックに変更したり、といった管理操作を行うことはできません。

{% comment %}
> **Note**:
> A collaborator cannot add other collaborators. Only the owner of
> the repository has administrative access.
{% endcomment %}
> **メモ**:
> 協力者が他の協力者を追加することはできません。
> 管理操作が可能なのは、あくまでそのリポジトリの所有者のみです。

{% comment %}
You can also assign more granular collaborator rights ("Read", "Write", or
"Admin") on Docker Hub by using organizations and teams. For more information
see the [organizations documentation](orgs.md).
{% endcomment %}
組織やチームを利用すれば、Docker Hub での協力者の権限は、さらに細かく割り当てることもできます（「Read」、「Write」、「Admin」）。
詳しくは [組織のドキュメント](orgs.md) を参照してください。


{% comment %}
## Viewing repository tags
{% endcomment %}
{: #viewing-repository-tags }
## リポジトリのタグ参照

{% comment %}
Docker Hub's individual repositories view shows you the available tags and the
size of the associated image. Go to the **Repositories** view and click on a
repository to see its tags.
{% endcomment %}
Docker Hub の個々のリポジトリ画面では、利用可能なタグと、関連イメージのサイズが表示されます。
**Repositories** 画面にアクセスして、1 つのリポジトリをクリックし、タグを確認してください。

{% comment %}
![Repository View](images/repos-create.png)
{% endcomment %}
![リポジトリ画面](images/repos-create.png)

{% comment %}
![View Repo Tags](images/repo-overview.png)
{% endcomment %}
![リポジトリタグの参照](images/repo-overview.png)

{% comment %}
Image sizes are the cumulative space taken up by the image and all its parent
images. This is also the disk space used by the contents of the `.tar` file
created when you `docker save` an image.
{% endcomment %}
イメージサイズは、そのイメージと親イメージによって占有される合計サイズです。
これはまた、`docker save`によりイメージを生成した際の`.tar`ファイルのディスク容量でもあります。

{% comment %}
To view individual tags, click on the **Tags** tab.
{% endcomment %}
個々のタグを確認するには **Tags** タブを開きます。

{% comment %}
![Manage Repo Tags](images/repo-tags-list.png)
{% endcomment %}
![リポジトリタグの管理](images/repo-tags-list.png)

{% comment %}
Select a tag's digest to view details.
{% endcomment %}
タグのダイジェスト値をクリックして、タグの詳細を確認します。

{% comment %}
![View Tag](images/repo-image-layers.png)
{% endcomment %}
![タグの確認](images/repo-image-layers.png)

{% comment %}
## Searching for Repositories
{% endcomment %}
{: #searching-for-repositories }
## リポジトリの検索

{% comment %}
You can search the [Docker Hub](https://hub.docker.com) registry through its
search interface or by using the command line interface. Searching can find
images by image name, username, or description:
{% endcomment %}
[Docker Hub](https://hub.docker.com) レジストリに対しては、検索画面やコマンドラインインターフェースを利用して検索することができます。
検索では、イメージ名、ユーザー名、内容説明を使ってイメージを検索します。

```
$ docker search centos
NAME                                 DESCRIPTION                                     STARS     OFFICIAL   AUTOMATED
centos                               The official build of CentOS.                   1034      [OK]
ansible/centos7-ansible              Ansible on Centos7                              43                   [OK]
tutum/centos                         Centos image with SSH access. For the root...   13                   [OK]
...
```

{% comment %}
There you can see two example results: `centos` and `ansible/centos7-ansible`.
The second result shows that it comes from the public repository of a user,
named `ansible/`, while the first result, `centos`, doesn't explicitly list a
repository which means that it comes from the top-level namespace for
[official images](official_images.md). The `/` character separates
a user's repository from the image name.
{% endcomment %}
上の検索によると、検索結果として`centos`と`ansible/centos7-ansible`という 2 つがあります。
2 つめの結果は、パブリックリポジトリにある、ユーザー`ansible/`によるイメージを表わしています。
一方 1 つめである`centos`はリポジトリが表示されていません。
これは [公式イメージ](official_images.md) によるトップレベルの名前空間からきていることを表わします。
なお`/`は、ユーザーのリポジトリ名とイメージ名を区切るものです。

{% comment %}
Once you've found the image you want, you can download it with `docker pull <imagename>`:
{% endcomment %}
検索したいイメージが見つかったら、`docker pull <イメージ名>`を実行してダウンロードします。

```
$ docker pull centos
latest: Pulling from centos
6941bfcbbfca: Pull complete
41459f052977: Pull complete
fd44297e2ddb: Already exists
centos:latest: The image you are pulling has been verified. Important: image verification is a tech preview feature and should not be relied on to provide security.
Digest: sha256:d601d3b928eb2954653c59e65862aabb31edefa868bd5148a41fa45004c12288
Status: Downloaded newer image for centos:latest
```

{% comment %}
You now have an image from which you can run containers.
{% endcomment %}
イメージが取得できたので、ここからコンテナーを実行することができます。

{% comment %}
## Starring Repositories
{% endcomment %}
{: #starring-repositories }
## リポジトリへの星マークづけ

{% comment %}
Your repositories can be starred and you can star repositories in return. Stars
are a way to show that you like a repository. They are also an easy way of
bookmarking your favorites.
{% endcomment %}
リポジトリは他の方から星マークをつけてもらうことがあり、逆に他のリポジトリへ星マークをつけることができます。
星マークをつけるのは、好きなリポジトリが何かを公開する方法として使えます。
またお気に入りのリポジトリをブックマークしておくということもできます。

{% comment %}
## Service accounts
{% endcomment %}
{: #service-accounts }
## サービスアカウント

 {% comment %}
 A service account is a Docker ID used by a bot for automating the build pipeline for containerized applications. Service accounts are typically used in an automated workflow and do not share Docker IDs with the members in the Team plan.
 {% endcomment %}
 サービスアカウントとは、コンテナー化アプリケーションの自動ビルドを行うためのボット（bot）が用いる Docker ID です。
 通常は自動化処理において用いられるものであって、チームプランにおけるメンバーの Docker ID として利用するものではありません。

 {% comment %}
 To create a new service account:
 {% endcomment %}
 新たなサービスアカウントを生成するには、以下のようにします。

 {% comment %}
 1. Create a new Docker ID.
 2. Create a [team](orgs.md#create-a-team) in your organization and grant it read-only access to your private repositories.
 3. Add the new Docker ID to your [organization](orgs.md#working-with-organizations).
 4. Add the new Docker ID  to the [team](orgs.md#add-a-member-to-a-team) you created earlier.
 5. Create a new [personal access token (PAT)](/access-tokens.md) from the user account and use it for CI.
 {% endcomment %}
 1. 新たな Docker ID を生成します。
 2. 組織内に [チーム](orgs.md#create-a-team) を生成し、プライベートリポジトリに対して読み取り専用の権限を与えます。
 3. その Docker ID を [組織](orgs.md#working-with-organizations) に加えます。
 4. その Docker ID を、先ほど生成した [チーム](orgs.md#add-a-member-to-a-team) に加えます。
 5. ユーザーアカウントから新たに [個人用アクセストークン（personal access token; PAT）](/access-tokens.md) を生成して、CI において利用します。

 {% comment %}
 > **Note**
 >
 > If you want a read-only PAT just for your open source repos, or to access official images and other public images, you do not have to grant any access permissions to the new Docker ID.
 {% endcomment %}
 > **メモ**
 >
 > 読み取り専用の PAT を生成して、オープンソースリポジトリ向けに利用したり、公式イメージや他の公開イメージにアクセスするだけの利用としたい場合には、その Docker ID に何も権限を与える必要はありません。
