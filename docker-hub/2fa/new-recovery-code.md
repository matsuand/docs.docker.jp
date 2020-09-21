---
description: Generate a new 2fa recovery code
keywords: Docker, docker, registry, security, Docker Hub, authentication, two-factor authentication
title: 新たなリカバリーコードの生成
---

{% comment %}
If you have lost your two-factor authentication recovery code and still have
access to your Docker Hub account, you can generate a new recovery code.
{% endcomment %}
２要素認証のリカバリーコードが不明ながら、Docker Hub アカウントにアクセスはできている場合、新たなリカバリーコードを生成することができます。

{% comment %}
## Prerequisites
{% endcomment %}
{: #prerequisites }
## 前提条件
{% comment %}
Two-factor authentication is enabled on your Docker Hub account.
{% endcomment %}
Docker Hub アカウントにおいて２要素認証が有効であるとします。

{% comment %}
## Generate a new recovery code
{% endcomment %}
{: #generate-a-new-recovery-code }
## 新たなリカバリーコードの生成

{% comment %}
To disable two-factor authentication, log in to your Docker Hub account. Click
on your username and select **Account Settings**.  Go to **Security** and click
on **Click here to generate a new code**.
{% endcomment %}
まずは２要素認証を無効にするために、Docker Hub アカウントにログインします。
ユーザー名をクリックして **Account Settings** を実行します。
**Security** にアクセスして **Click here to generate a new code**（ここをクリックして新たなコードを生成）をクリックします。

{% comment %}
![New recovery code link](../images/2fa-disable-2fa.png)
{% endcomment %}
![新たなリカバリーコードへのリンク](../../images/2fa-disable-2fa.png)

{% comment %}
Enter your password.
{% endcomment %}
パスワードを入力します。

{% comment %}
![Enter your password](../images/2fa-pw-new-code.png){:width="250px"}
{% endcomment %}
![パスワードの入力](../../images/2fa-pw-new-code.png){:width="250px"}

{% comment %}
Your new recovery code will be displayed. Remember to save your recovery code
and store it somewhere safe.
{% endcomment %}
新たなリカバリーコードが表示されます。
このリカバリーコードを書きとめておき、どこか安全な場所に保存してください。
