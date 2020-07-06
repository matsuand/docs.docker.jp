---
description: Docker Hub Teams & Organizations
keywords: Docker, docker, registry, teams, organizations, plans, Dockerfile, Docker Hub, docs, documentation
title: チームと組織
redirect_from:
- /docker-cloud/orgs/
---

{% comment %}
Docker Hub organizations let you create teams so you can give your team access
to shared image repositories.
{% endcomment %}
Docker Hub の組織（organizations）は、チームの生成を行い、チームメンバーがイメージリポジトリを共有アクセスできるようにするものです。

{% comment %}
- **Organizations** are collections of teams and repositories that can be managed together.
- **Teams** are groups of Docker Hub users that belong to an organization.
{% endcomment %}
- **組織**（organizations）とは、チームやリポジトリが集まったものであり、一括して管理できるものを指します。
- **チーム**（teams）とは、組織に属している Docker Hub ユーザーのグループのことです。

{% comment %}
> **Note:** in Docker Hub, users cannot belong directly to an organization.
They belong only to teams within an organization.
{% endcomment %}
> **メモ**  Docker Hub におけるユーザーは、組織に直接属するものではありません。
> ユーザーは、あくまで組織内にあるチームに属するものです。

{% comment %}
<iframe width="560" height="315" src="https://www.youtube-nocookie.com/embed/G7lvSnAqed8" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
{% endcomment %}
<iframe width="560" height="315" src="https://www.youtube-nocookie.com/embed/G7lvSnAqed8" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

{% comment %}
## Working with organizations
{% endcomment %}
{: #working-with-organizations }
## 組織の利用

{% comment %}
### Create an organization
{% endcomment %}
{: #create-an-organization }
### 組織の生成

<iframe width="560" height="315" src="https://www.youtube-nocookie.com/embed/b0TKcIqa9Po" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

{% comment %}
1. Start by clicking on **[Organizations](https://hub.docker.com/orgs)** in
Docker Hub.
{% endcomment %}
1. Docker Hub の [Organizations](https://cloud.docker.com/orgs) をクリックするところから始めます。

{% comment %}
2. Click on **Create Organization**.
{% endcomment %}
2. **Create Organization** をクリックします。

{% comment %}
3. Provide information about your organization.
{% endcomment %}
3. 組織に関する情報を入力します。

{% comment %}
You've created an organization. You'll see you have a team, the **owners** team
with a single member (you!).
{% endcomment %}
こうして組織が生成できました。
1 つのチームができているはずです。
これは **owners** チームであり、ただ一人のメンバー、つまりあなた自身が含まれています。

{% comment %}
#### The owners team
{% endcomment %}
{: #the-owners-team }
#### owners チーム

{% comment %}
The **owners** team is a special team that has full access to all repositories
in the organization.
{% endcomment %}
**owners** チームは特別なチームです。
組織内にあるリポジトリすべてに対して、フルアクセスを行うことができます。

{% comment %}
Members of this team can:
- Manage organization settings and billing
- Create a team and modify the membership of any team
- Access and modify any repository belonging to the organization
{% endcomment %}
このチームのメンバーは以下のことができます。
- 組織の設定や支払い方法を管理できます。
- チームを生成し、チームメンバーの管理を行うことができます。
- 組織に属しているリポジトリすべてにアクセスし更新することができます。

{% comment %}
### Access an organization
{% endcomment %}
{: #access-an-organization }
### 組織へのアクセス

{% comment %}
You can't _directly_ log into an organization. This is especially important to note if you create an organization by converting a user account, as conversion means you lose the ability to log into that "account", since it no longer exists.
{% endcomment %}
組織には**直接**ログインすることはできません。
特に気をつけておくべきことですが、ユーザーアカウントを変更し組織として作り直した場合です。
この際の変更を通じて、その「アカウント」ではログインできなくなります。
もうその「アカウント」が存在しなくなるからです。

{% comment %}
To access an organization:
{% endcomment %}
組織にアクセスするには以下のようにします。

{% comment %}
1. Log into Docker Hub with a user account that is a member of any team in the organization.
{% endcomment %}
1. ユーザーアカウントを使って Docker Hub にログインします。
   このユーザーアカウントはその組織内のメンバーであれば、どのチームに属していてもかまいません。

    {% comment %}
    > If you want access to organization settings, this account has to be part of the **owners** team.
    {% endcomment %}
    > 組織の設定を行いたい場合、そのアカウントは **owners** チームに属している必要があります。

{% comment %}
2. Click **Organizations** in the top navigation bar, then choose your organization from the list.
{% endcomment %}
2. 最上段のナビゲーションバーにある **Organizations** をクリックします。
   そして一覧の中から目的の組織を選びます。

{% comment %}
If you don't see the organization, then you are neither a member or an owner of it. An organization administrator will need to add you as a member of the organization team.
{% endcomment %}
目的の組織が見つからない場合は、つまりログインしたユーザーアカウントはその組織のメンバーや所有者ではないということです。
組織の管理者から、組織のメンバーに加えてもらってください。

{% comment %}
## Working with teams and members
{% endcomment %}
{: #working-with-teams-and-members }
## チームとメンバーの利用

<iframe width="560" height="315" src="https://www.youtube-nocookie.com/embed/MROKmtmWCVI" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

{% comment %}
### Create a team
{% endcomment %}
{: #create-a-team }
### チームの生成

<iframe width="560" height="315" src="https://www.youtube-nocookie.com/embed/78wbbBoasIc" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

{% comment %}
1. Go to **Organizations** in Docker Hub, and select your organization.
{% endcomment %}
1. Docker Hub の **Organizations** へアクセスし、目的の組織を選びます。

{% comment %}
2. Open the **Teams** tab and click **Create Team**.
{% endcomment %}
2. **Teams** タブを開いて **Create Team** をクリックします。

{% comment %}
3. Fill out your team's information and click **Create**.
{% endcomment %}
3. チーム情報を入力し、**Create** をクリックします。

{% comment %}
### Add a member to a team
{% endcomment %}
{: #add-a-member-to-a-team }
### チームへのメンバー追加

{% comment %}
You can add a member to a team in one of two ways.
{% endcomment %}
チームに対してメンバーを追加するには以下の 2 つの方法があります。

{% comment %}
If the user isn't in your organization:
{% endcomment %}
ユーザーが組織に含まれていない場合、

{% comment %}
1. Go to **Organizations** in Docker Hub, and select your organization.
{% endcomment %}
1. Docker Hub の **Organizations** へアクセスし、目的の組織を選びます。

{% comment %}
2. Click **Add Member**.
{% endcomment %}
2. **Add Member** をクリックします。

{% comment %}
3. Enter the user's Docker ID or email, and select a team from the drop-down list.
{% endcomment %}
3. 追加するユーザーの Docker ID かメールアドレスを入力して、ドロップダウンリストからチームを選びます。

{% comment %}
4. Click **Add** to confirm.
{% endcomment %}
4. **Add** をクリックして確定します。

{% comment %}
If the user already belongs to another team in the organization:
{% endcomment %}
目的のユーザーが、組織内の別のチームにすでに属している場合、

{% comment %}
1. Open the team's page in Docker Hub: **Organizations** > **_Your Organization_** > **Teams** > **_Your Team Name_**
{% endcomment %}
1. Docker Hub のチームページを開きます。
   **Organizations** > **＜目的の組織＞** > **Teams** > **＜目的のチーム＞**

{% comment %}
2. Click **Add user**.
{% endcomment %}
2. **Add user** をクリックします。

{% comment %}
3. Enter the user's Docker ID or email to add them to the team.
{% endcomment %}
3. 追加するユーザーの Docker ID メールアドレスを入力して、チームへ追加します。

      {% comment %}
      > **Note**: You are not automatically added to teams created by your organization.
      {% endcomment %}
      > **メモ**: 組織から生成したチームへは、操作したユーザーが自動的に追加されるわけではありません。

{% comment %}
### Remove team members
{% endcomment %}
{: #remove-team-members }
### チームメンバーの削除

{% comment %}
To remove a member from all teams in an organization:
{% endcomment %}
組織内のすべてのチームから、メンバー 1 名を削除するには、

{% comment %}
1. Go to **Organizations** in Docker Hub, and select your organization. The Organizations page lists all team members.
{% endcomment %}
1. Docker Hub の **Organizations** にアクセスして、目的の組織を選択します。
   組織のページにはチームメンバーがすべて一覧表示されています。

{% comment %}
2. Click the **x** next to a member’s name to remove them from all the teams in the organization.
{% endcomment %}
2. メンバー名の横にある **x** をクリックします。
   これによって組織内にあるチームのすべてからメンバーが削除されます。

{% comment %}
3. When prompted, click **Remove** to confirm the removal.
{% endcomment %}
3. 確認画面が出たら、**Remove** をクリックして削除を確定しす。

{% comment %}
To remove a member from a specific team:
{% endcomment %}
1 つのチームからメンバー 1 名を削除するには、

{% comment %}
1. Go to **Organizations** in Docker Hub, and select your organization.
{% endcomment %}
1. Docker Hub の **Organizations** にアクセスして、目的の組織を選択します。

{% comment %}
2. Click on the **Teams** tab and select the team from the list.
{% endcomment %}
2. **Teams** タブをクリックし、一覧から目的のチームを選択します。

{% comment %}
3. Click the **x** next to the user's name to remove them from the team.
{% endcomment %}
3. ユーザー名の横にある **x** をクリックします。
   これによってチーム内からユーザーが削除されます。

{% comment %}
4. When prompted, click **Remove** to confirm the removal.
to confirm the removal.
{% endcomment %}
4. 確認画面が出たら、**Remove** をクリックして削除を確定しす。

{% comment %}
### Give a team access to a repository
{% endcomment %}
{: #give-a-team-access-to-a-repository }
### リポジトリに対するチームアクセスの追加

{% comment %}
1. Visit the repository list on Docker Hub by clicking on **Repositories**.
{% endcomment %}
1. Docker Hub 上において **Repositories** をクリックしてリポジトリ一覧を表示します。

{% comment %}
2. Select your organization in the namespace dropdown list.
{% endcomment %}
2. 名前空間ドロップダウンリストから、目的の組織を選択します。

{% comment %}
3. Click the repository you'd like to edit.
{% endcomment %}
3. 編集したいリポジトリをクリックします。

      {% comment %}
      ![Org Repos](images/repos-list2019.png)
      {% endcomment %}
      ![組織のリポジトリ](images/repos-list2019.png)

{% comment %}
4. Click the **Permissions** tab.
{% endcomment %}
4. **Permissions** タブをクリックします。

{% comment %}
5. Select the team, the [permissions level](#permissions-reference), and click **+** to save.
{% endcomment %}
5. チームと [パーミッションレベル](#permissions-reference) を選択して **+** をクリックし保存します。

      {% comment %}
      ![Add Repo Permissions for Team](images/orgs-repo-perms2019.png)
      {% endcomment %}
      ![チームに対するリポジトリパーミッションの追加](images/orgs-repo-perms2019.png)

{% comment %}
### View a team's permissions for all repositories
{% endcomment %}
{: #view-a-teams-permissions-for-all-repositories }
### リポジトリすべてに対するチームパーミッションの確認

{% comment %}
To view a team's permissions over all repos:
{% endcomment %}
リポジトリすべてにわたってのチームのパーミッションを確認するには、以下を行います。

{% comment %}
1. Open **Organizations** > **_Your Organization_** > **Teams** > **_Team Name_**.
{% endcomment %}
1. **Organizations** > **＜目的の組織＞** > **Teams** > **＜目的のチーム名＞** を開きます。

{% comment %}
2. Click on the **Permissions** tab, where you can view the repositories this team can access.
{% endcomment %}
2. **Permissions** タブをクリックして、チームがアクセス可能なリポジトリはどれであるかを確認します。

      {% comment %}
      ![Team Audit Permissions](images/orgs-teams-perms2019.png)
      {% endcomment %}
      ![チームのパーミッション](images/orgs-teams-perms2019.png)

You can also edit repository permissions from this tab.


{% comment %}
### Permissions reference
{% endcomment %}
{: #permissions-reference }
### パーミッションに関するリファレンス

{% comment %}
Permissions are cumulative. For example, if you have Write permissions, you
automatically have Read permissions:
{% endcomment %}
パーミッションは積み上げられるような性質を持っています。
たとえば書き込みパーミッションがあったとすると、それは自動的に読み込みパーミッションも有していることになります。

{% comment %}
- `Read` access allows users to view, search, and pull a private repository in the same way as they can a public repository.
- `Write` access allows users to push to repositories on Docker Hub.
- `Admin` access allows users to modify the repositories "Description", "Collaborators" rights, "Public/Private" visibility, and "Delete".
{% endcomment %}
- `Read`（読み込み）権限は、公開リポジトリに対する操作と同じように、 プライベートリポジトリの参照、検索、プルを行うことができます。
- `Write`（書き込み）権限は、Docker Hub 上のリポジトリにプッシュすることができます。
- `Admin`（管理）権限は、リポジトリに対して "Description"、"Collaborators" の権限、"Public/Private" の別を編集したり、"Delete" を行ったりすることができます。

{% comment %}
> **Note**: A User who has not yet verified their email address only has
> `Read` access to the repository, regardless of the rights their team
> membership has given them.
{% endcomment %}
> **メモ**: メールアドレスの検証が済んでいないユーザーは、たとえチームメンバーとしての権限が与えられていても、リポジトリに対しては `Read` 権限しか与えられません。
