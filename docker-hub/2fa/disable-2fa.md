---
description: Disable two-factor authentication on Docker Hub
keywords: Docker, docker, registry, security, Docker Hub, authentication, two-factor authentication
title: Docker Hub における２要素認証の無効化
---

{% comment %}
> **Note:**
> Disabling two-factor authentication will result in decreased security for your
> Docker Hub account.
{: .warning }
{% endcomment %}
> **メモ**
> ２要素認証を無効化すると、Docker Hub アカウントのセキュリティを低下させることになります。
{: .warning }


{% comment %}
## Prerequisites
{% endcomment %}
{: #prerequisites }
## 前提条件
{% comment %}
Two-factor authentication is enabled on your Docker Hub account.
{% endcomment %}
Docker Hub アカウントに対して、２要素認証が有効であることが必要です。

{% comment %}
## Disable two-factor authentication
{% endcomment %}
{: #disable-two-factor-authentication }
## ２要素認証の無効化
{% comment %}
To disable two-factor authentication, log in to your Docker Hub account. Click
on your username and select **Account Settings**. Go to Security and click on
**Disable 2FA**.
{% endcomment %}
２要素認証を無効化するには、まず Docker Hub アカウントにログインします。
ユーザー名をクリックして **Account Settings** を実行します。
Security にアクセスして **Disable 2FA**（２要素認証の無効化）をクリックします。

{% comment %}
![Disable 2fa button](../images/2fa-disable-2fa.png)
{% endcomment %}
![２要素認証の無効化ボタン](../images/2fa-disable-2fa.png)

{% comment %}
You will be prompted to input your Docker ID password. Enter your password and
click **Disable 2FA**.
{% endcomment %}
Docker ID パスワードの入力を求められるので、パスワードを入力して **Disable 2FA**（２要素認証の無効化）をクリックします。

{% comment %}
![Enter your password](../images/2fa-enter-pw-disable-2fa.png){:width="250px"}
{% endcomment %}
![パスワード入力](../images/2fa-enter-pw-disable-2fa.png){:width="250px"}

{% comment %}
You have successfully disabled two-factor authentication.
{% endcomment %}
これにより２要素認証が無効化されます。
