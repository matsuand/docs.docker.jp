---
description: Recover your Docker Hub account
keywords: Docker, docker, registry, security, Docker Hub, authentication, two-factor authentication
title: Docker Hub アカウントの復旧
---

{% comment %}
If you have lost your two-factor authentication device and need to access your
Docker Hub account, you can gain access to your account using your two-factor
authentication recovery code.
{% endcomment %}
２要素認証を行ったデバイスを失ってしまい、なお Docker Hub アカウントにアクセスが必要である場合、２要素認証のリカバリーコードを使って、アカウントアクセスを復旧させることができます。

{% comment %}
## Prerequisites
{% endcomment %}
{: #prerequisites }
## 前提条件

{% comment %}
Two-factor authentication is enabled on your Docker Hub account and you have
your two-factor authentication recovery code.
{% endcomment %}
Docker Hub アカウントに対して２要素認証が有効であり、２要素認証のリカバリーコードが手元にあるものとします。

{% comment %}
> If you lose both your 2FA authentication device and recovery code, you may
> not be able to recover your account.
{: .important }
{% endcomment %}
> ２要素認証を行ったデバイスとリカバリーコードをともに失ってしまった場合、アカウントを復旧することはできません。
{: .important }

{% comment %}
## Recover your Docker Hub account with a recovery code
{% endcomment %}
{: #recover-your-docker-hub-account-with-a-recovery-code }
## リカバリーコードによる Docker Hub アカウントの復旧

{% comment %}
Go through the login process on Docker Hub. When you're asked to enter your
two-factor authentication code, click **I've lost my authentication device**.
{% endcomment %}
Docker Hub においてログイン操作を進めます。
２要素認証コードの入力が求められたら、**I've lost my authentication device**（認証済みデバイスを失いました）をクリックします。

{% comment %}
![Lost authentication device](../images/2fa-enter-2fa-code.png){:width="250px"}
{% endcomment %}
![認証デバイスの紛失](../images/2fa-enter-2fa-code.png){:width="250px"}

{% comment %}
On the next screen, click "I have my recovery code".
{% endcomment %}
次の画面において「I have my recovery code」（リカバリーコードを利用します）をクリックします。

{% comment %}
![You have your code](../images/2fa-have-recovery-code.png){:width="250px"}
{% endcomment %}
![リカバリーコードの利用](../images/2fa-have-recovery-code.png){:width="250px"}

{% comment %}
Enter your recovery code.
{% endcomment %}
そこでリカバリーコードを入力します。

{% comment %}
![Enter recovery code](../images/2fa-enter-recover-code.png){:width="250px"}
{% endcomment %}
![リカバリーコードの入力](../images/2fa-enter-recover-code.png){:width="250px"}

{% comment %}
Once you have used your recovery code, you will have to re-enable two-factor
authentication. See [Enabling two-factor authentication on Docker Hub](/docker-hub/2fa).
{% endcomment %}
リカバリーコードを入力したら、２要素認証を再び有効にすることが必要です。
[Docker Hub における２要素認証の有効化](/docker-hub/2fa) を参照してください。

{% comment %}
## Alternative recovery methods
{% endcomment %}
{: #alternative-recovery-methods }
## 別の復旧方法

{% comment %}
If you have lost access to both your two-factor authentication application and
your recovery code, send an email to [Docker Hub Support](mailto:hub-support@docker.com) from the primary email associated with your Docker ID for recovery instructions.
{% endcomment %}
２要素認証アプリケーションとリカバリーコードをともに失ってしまった場合は、Docker ID に関連づけた主となるメールアドレスを使って [Docker Hub サポート](mailto:hub-support@docker.com) にメールしてください。
復旧の手順をお示しします。
