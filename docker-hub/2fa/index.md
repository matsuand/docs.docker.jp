---
description: Enabling two-factor authentication on Docker Hub
keywords: Docker, docker, registry, security, Docker Hub, authentication, two-factor authentication
title: Docker Hub における 2 要素認証の有効化
---

{% comment %}
## About two-factor authentication
{% endcomment %}
{: #about-two-factor-authentication }
## 2 要素認証について
{% comment %}
{% endcomment %}
Two-factor authentication adds an extra layer of security to your Docker Hub
account by requiring a unique security code when you log into your account. The
security code will be required in addition to your password.

{% comment %}
{% endcomment %}
When you enable two-factor authentication, you will also be provided a recovery
code. Each recovery code is unique and specific to your account. You can use
this code to recover your account in case you lose access to your authenticator
app. See [Recover your Docker Hub account](recover-hub-account/).


{% comment %}
{% endcomment %}
## Prerequisites
{% comment %}
{% endcomment %}
You need a mobile phone with a time-based one-time password authenticator
application installed. Common examples include Google Authenticator or Yubico
Authenticator with a registered YubiKey.

{% comment %}
{% endcomment %}
> **Note:**
> Two-factor authentication is currently in beta. Feel free to provide feedback
> at the [Docker Hub feedback repo](https://github.com/docker/hub-feedback/issues).
{: .important}

{% comment %}
{% endcomment %}
## Enable two-factor authentication
{% comment %}
{% endcomment %}
To enable two-factor authentication, log in to your Docker Hub account. Click
on your username and select **Account Settings**. Go to Security and click
**Enable Two-Factor Authentication**.

{% comment %}
{% endcomment %}
![Two-factor home](../images/2fa-security-home.png)

{% comment %}
{% endcomment %}
The next page will remind you to download an authenticator app. Click **Set up**
**using an app**. You will receive your unique recovery code.

{% comment %}
{% endcomment %}
> **Save your recovery code and store it somewhere safe.**
>
> Your recovery code can be used to recover your account in the event you lose
> access to your authenticator app.
{: .important }

{% comment %}
{% endcomment %}
![Recovery code example](../images/2fa-recovery-code.png)

{% comment %}
{% endcomment %}
After you have saved your code, click **Next**.

{% comment %}
{% endcomment %}
Open your authenticator app. You can choose between scanning the QR code or
entering a text code into your authenticator app. Once you have linked your
authenticator app, it will give you a six-digit code to enter in text field.
Click **Next**.

{% comment %}
{% endcomment %}
![Enter special code](../images/2fa-enter-code.png)

{% comment %}
{% endcomment %}
You have successfully enabled two-factor authentication. The next time you log
in to your Docker Hub account, you will be asked for a security code.

{% comment %}
{% endcomment %}
> **Note:**
> Now that you have two-factor authentication enabled on your account, you must
> create at least one personal access token. Otherwise, you will be unable to
> log in to your account from the Docker CLI. See [Managing access tokens](../access-tokens)
> for more information.
{: .important }
