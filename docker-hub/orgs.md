---
description: Docker Hub Teams & Organizations
keywords: Docker, docker, registry, teams, organizations, plans, Dockerfile, Docker Hub, docs, documentation
title: チームと組織
redirect_from:
- /docker-cloud/orgs/
---

{% comment %}
Docker Hub Organizations let you create teams so you can give your team access to shared image repositories.
{% endcomment %}
Docker Hub の組織（organizations）は、チームの生成を行い、チームメンバーがイメージリポジトリを共有アクセスできるようにするものです。

{% comment %}
### How Organizations & Teams Work
{% endcomment %}
{: #how-organizations--teams-work }
### 組織やチームがどのように動作するか

{% comment %}
- **Organizations** are a collection of teams and repositories that can be managed together.
- **Teams** are groups of Docker Hub users that belong to your organization.
{% endcomment %}
- **組織**（organizations）とは、チームやリポジトリが集まったものであり、一括して管理できるものを指します。
- **チーム**（teams）とは、組織に属している Docker Hub ユーザーのグループのことです。

{% comment %}
> **Note**: in Docker Hub, users cannot be associated directly to an organization. They belong only to teams within an organization.
{% endcomment %}
> **メモ**: Docker Hub におけるユーザーは、組織に直接関連づけられるわけではありません。
> ユーザーは、あくまで組織内にあるチームに属するものです。

{% comment %}
### Creating an organization
{% endcomment %}
{: #creating-an-organization }
### 組織の生成

{% comment %}
1. Start by clicking on [Organizations](https://cloud.docker.com/orgs) in Docker Hub
2. Click on "Create Organization"
3. Provide information about your organization:
{% endcomment %}
1. Docker Hub の [Organizations](https://cloud.docker.com/orgs) をクリックするところから始めます。
2. 「Create Organization」をクリックします。
3. 組織に関する情報を入力します。

{% comment %}
![Create Organization](images/orgs-create.png)
{% endcomment %}
![組織の生成](images/orgs-create.png)

{% comment %}
You've created an organization. You'll see you have a team, the **owners** team with a single member (you!)
{% endcomment %}
こうして組織が生成できました。
1 つのチームができているはずです。
これは **owners** チームであり、ただ一人のメンバー、つまりあなた自身が含まれています。

{% comment %}
### The owners team
{% endcomment %}
{: #the-owners-team }
### owners チーム

{% comment %}
The **owners** team is a special team that has full access to all repositories in the Organization.
{% endcomment %}
**owners** チームは特別なチームです。
組織内にあるリポジトリすべてに対して、フルアクセスを行うことができます。

{% comment %}
Members of this team can:
- Manage Organization settings and billing
- Create a team and modify the membership of any team
- Access and modify any repository belonging to the Organization
{% endcomment %}
このチームのメンバーは以下のことができます。
- 組織の設定や支払い方法を管理できます。
- チームを生成し、チームメンバーの管理を行うことができます。
- 組織に属しているリポジトリすべてにアクセスし更新することができます。

{% comment %}
### Creating a team
{% endcomment %}
{: #creating-a-team }
### チームの生成

{% comment %}
To create a team:
{% endcomment %}
チームの生成は以下のようにします。

{% comment %}
1. Go to your organization by clicking on **Organizations** in Docker Hub, and select your organization.
2. Click **Create Team** ![Create Team](images/orgs-team-create.png)
3. Fill out your team's information and click **Create** ![Create Modal](images/orgs-team-create-submit.png)
{% endcomment %}
1. Docker Hub の **Organizations** をクリックして組織の画面へアクセスします。
   そして目的の組織を選びます。
2. **Create Team** をクリックします。
   ![チームの生成](images/orgs-team-create.png)
3. チーム情報を入力し、**Create** をクリックします。
   ![チーム生成のモーダル画面](images/orgs-team-create-submit.png)

{% comment %}
### Adding a member to a team
{% endcomment %}
{: #adding-a-member-to-a-team }
### チームへのメンバー追加

{% comment %}
1. Visit your team's page in Docker Hub. Click on **Organizations** > **_Your Organization_** > **_Your Team Name_**
2. Click on **Add User**
3. Provide the user's Docker ID username _or_ email to add them to the team ![Add User to Team](images/orgs-team-add-user.png)
{% endcomment %}
1. Docker Hub のチームのページにアクセスします。
   そして **Organizations** > **_目的の組織_** > **_目的のチーム_** をクリックします。
2. **Add User** をクリックします。
3. 追加するユーザーの Docker ID ユーザー名かメールアドレスを入力して、このユーザーをチームに追加します。
   ![チームへのユーザー追加](images/orgs-team-add-user.png)

{% comment %}
> **Note**: you are not automatically added to teams created by your organization.
{% endcomment %}
> **メモ**: you are not automatically added to teams created by your organization.

{% comment %}
### Removing team members
{% endcomment %}
{: #removing-team-members }
### チームメンバーの削除

{% comment %}
To remove a member from a team, click the **x** next to their name:
{% endcomment %}
チームからメンバーを削除するには、メンバー名の横にある **x** をクリックします。

{% comment %}
![Add User to Team](images/orgs-team-remove-user.png)
{% endcomment %}
![チームメンバーの削除](images/orgs-team-remove-user.png)

{% comment %}
### Giving a team access to a repository
{% endcomment %}
{: #giving-a-team-access-to-a-repository }
### Giving a team access to a repository

{% comment %}
To provide a team access to a repository:
{% endcomment %}
To provide a team access to a repository:

{% comment %}
1. Visit the repository list on Docker Hub by clicking on **Repositories**
2. Select your organization in the namespace dropdown list
3. Click the repository you'd like to edit ![Org Repos](images/orgs-list-repos.png)
4. Click the **Permissions** tab
5. Select the team, permissions level (more on this below) and click **+**
6. Click the **+** button to add ![Add Repo Permissions for Team](images/orgs-add-team-permissions.png)
{% endcomment %}
1. Visit the repository list on Docker Hub by clicking on **Repositories**
2. Select your organization in the namespace dropdown list
3. Click the repository you'd like to edit ![Org Repos](images/orgs-list-repos.png)
4. Click the **Permissions** tab
5. Select the team, permissions level (more on this below) and click **+**
6. Click the **+** button to add ![Add Repo Permissions for Team](images/orgs-add-team-permissions.png)

{% comment %}
### Viewing a team's permissions for all repositories
{% endcomment %}
{: #viewing-a-teams-permissions-for-all-repositories }
### Viewing a team's permissions for all repositories

{% comment %}
To view a team's permissions over all repos:
1. Click on **Organizations**, then select your organization and team.
2. Click on the **Permissions** tab where you can view which repositories this team has access to ![Team Audit Permissions](images/orgs-audit-permissions.png)
{% endcomment %}
To view a team's permissions over all repos:
1. Click on **Organizations**, then select your organization and team.
2. Click on the **Permissions** tab where you can view which repositories this team has access to ![Team Audit Permissions](images/orgs-audit-permissions.png)


{% comment %}
### Permissions Reference
{% endcomment %}
{: #permissions-reference }
### Permissions Reference

{% comment %}
Permissions are cumulative. For example, if you have Write permissions, you
automatically have Read permissions:
{% endcomment %}
Permissions are cumulative. For example, if you have Write permissions, you
automatically have Read permissions:

{% comment %}
- `Read` access allows users to view, search, and pull a private repository in the same way as they can a public repository.
- `Write` access allows users to push to repositories on Docker Hub.
- `Admin` access allows users to modify the repositories "Description", "Collaborators" rights, "Public/Private" visibility and "Delete".
{% endcomment %}
- `Read` access allows users to view, search, and pull a private repository in the same way as they can a public repository.
- `Write` access allows users to push to repositories on Docker Hub.
- `Admin` access allows users to modify the repositories "Description", "Collaborators" rights, "Public/Private" visibility and "Delete".

{% comment %}
> **Note**: A User who has not yet verified their email address only has
> `Read` access to the repository, regardless of the rights their team
> membership has given them.
{% endcomment %}
> **メモ**: A User who has not yet verified their email address only has
> `Read` access to the repository, regardless of the rights their team
> membership has given them.
