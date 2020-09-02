---
title: Docker ローカルイメージに対するぜい弱性スキャン
description: Dockerローカルイメージに対するぜい弱性スキャン。
keywords: Docker, scan, Snyk, images, local, CVE, vulnerability, security
toc_min: 1
toc_max: 2
---

{% comment %}
## Overview
{% endcomment %}
{: #overview }
## 概要

{% comment %}
Vulnerability scanning for Docker local images runs on Snyk engine, providing users with visibility into the security posture of their local Dockerfiles and local images. Users trigger vulnerability scans through the CLI, and use the CLI to view the scan results.  The scanning function creates the list of Common Vulnerabilities and Exposures (CVEs), and provides recommendations for CVE remediations.
{% endcomment %}
Docker のローカルイメージに対するぜい弱性スキャンは、Snyk エンジンを使って動作します。
ローカルの Dockerfile やローカルイメージに対して、セキュリティ状況を可視化して示します。
ぜい弱性スキャンは CLI から起動し、CLI を使ってスキャン結果を確認します。
スキャン機能は CVE 形式の一覧を生成し、また CVE 修復に対する推奨書式を提供します。

{% comment %}
>**Note**
>
> Docker vulnerability scanning for local images  is currently a beta release. The commands and flags are subject to change in subsequent releases.
{:.important}
{% endcomment %}
>**メモ**
>
> ローカルイメージに対するぜい弱性スキャンは、今のところベータ版です。
> 各コマンドやフラグは、今後のリリースにおいて変更される場合があります。
{:.important}

{% comment %}
## Prerequisites
{% endcomment %}
{: #prerequisites }
## 前提条件

{% comment %}
To run vulnerability scanning on your Docker images, you must meet the following requirements:
{% endcomment %}
Docker イメージに対してぜい弱性スキャンを実行するには、以下を満たしていることが必要です。

{% comment %}
1. Download and install Docker Desktop Edge version 2.3.6.0 or later.
{% endcomment %}
1. Docker Desktop 最新版（Edge）バージョン 2.3.6.0 またはそれ以降をダウンロードしインストールしていること。

    {% comment %}
    - [Download for Mac](https://desktop.docker.com/mac/edge/Docker.dmg){: target="_blank" class="_"}
    - [Download for Windows](https://desktop.docker.com/win/edge/Docker%20Desktop%20Installer.exe){: target="_blank" class="_"}
    {% endcomment %}
    - [Download for Mac](https://desktop.docker.com/mac/edge/Docker.dmg){: target="_blank" class="_"}
    - [Download for Windows](https://desktop.docker.com/win/edge/Docker%20Desktop%20Installer.exe){: target="_blank" class="_"}

{% comment %}
2. Sign into [Docker Hub](https://hub.docker.com){: target="_blank" class="_"}.
{% endcomment %}
2. [Docker Hub](https://hub.docker.com){: target="_blank" class="_"} にサインインしていること。

{% comment %}
3. From the Docker Desktop menu, select **Sign in/ Create Docker ID**. Alternatively, open a terminal and run the command `docker login`.
{% endcomment %}
3. Docker Desktop メニューにおいて **Sign in/ Create Docker ID** を選択していること。
   あるいはターミナル画面を開いて `docker login` を実行していること。

{% comment %}
4. (Optional) You can create a [Snyk account](https://dockr.ly/3ePqVcp){: target="_blank" class="_"} for scans, or use the additional monthly free scans provided by Snyk with your Docker Hub account.
{% endcomment %}
4. （任意作業）スキャンを行うために [Snyk アカウント](https://dockr.ly/3ePqVcp){: target="_blank" class="_"} を生成していること。
   または Docker Hub アカウントとともに利用できるものとして Snyk 社が提供する、月単位の追加無償スキャンを利用していること。

{% comment %}
Check your installation by running `docker scan --version`, it should print the current version of docker scan and the Snyk engine version. For example:
{% endcomment %}
インストールができているかどうかを `docker scan --version` を実行して確認してください。
コマンド実行によって、docker scan の現行バージョン、および Snyk エンジンのバージョンを表示されます。
たとえば以下のとおりです。

```shell
$ docker scan --version
Version:    v0.3.0
Git commit: b8ee514
Provider:   Snyk (1.383.1)
```

{% comment %}
## Supported options
{% endcomment %}
{: #supported-options }
## サポートされているオプション

{% comment %}
The high-level `docker scan` command scans local images using the image name or the image ID. It supports the following options:
{% endcomment %}
高度なコマンド `docker scan` では、イメージの名前または ID を使ってローカルイメージをスキャンします。
このコマンドは以下のオプションをサポートしています。

{% comment %}
| Option                                                       | Description                                   |
|:------------------------------------------------------------------ :------------------------------------------------|
| `--acept license` | Accept the license agreement of the third-party scanning provider    |
| `--dependency-tree` | Display the dependency tree of the image along with scan results |
| `-exclude-base` | Exclude the base image during scanning. This option requires the --file option to be set |
| `-f`, `--file string` | Specify the location of the Dockerfile associated with the image. This option displays a detailed scan result |
| `--json` | Display the result of the scan in JSON format|
| `--login` | Log into Snyk using an optional token (using the flag --token), or by using a web-based token |
| `--reject-license` | Reject the license agreement of the third-party scanning provider |
| `--token string`  | Use the authentication token to log into the third-party scanning provider |
| `--version` | Display the Docker Scan plugin version |
{% endcomment %}
| オプション                                                   | 内容説明                                             |
|:------------------------------------------------------------ :------------------------------------------------|
| `--acept license` | サードパーティー製プロバイダーのライセンスを承認します。|
| `--dependency-tree` | スキャン結果において依存パッケージのツリーを表示します。|
| `-exclude-base` | スキャンにあたってベースイメージは取り除きます。このオプションには --file オプションの設定が必要です。|
| `-f`, `--file string` | イメージに対応する Dockerfile を指定します。このオプションはスキャンの詳細結果を表示します。|
| `--json` | スキャン結果を JSON 書式により表示します。|
| `--login` | 任意に指定されたトークン（--token フラグ利用）を使って Snyk にログインします。あるいはウェブベースのトークンを利用します。|
| `--reject-license` | サードパーティー製プロバイダーのライセンスを否認します。|
| `--token string`  | 認証トークンを使ってサードパーティー製スキャンプロバイダーにログインします。|
| `--version` | Docker Scan プラグインのバージョンを表示します。|

{% comment %}
## How to Scan images
{% endcomment %}
{: #how-to-scan-images }
## イメージのスキャン方法

{% comment %}
The `docker scan` command allows you to scan existing Docker images using the image name or ID.  For example, run the following command to scan the hello-world image:
{% endcomment %}
`docker scan` コマンドは、イメージ名またはイメージ ID を指定して、既存の Docker イメージをスキャンします。
たとえば以下のコマンドは hello-world イメージをスキャンします。

```shell
$  docker scan hello-world

  Testing hello-world...

  Organization:      docker-desktop-test
  Package manager:   linux
  Project name:      docker-image|hello-world
  Docker image:      hello-world
  Licenses:          enabled

  ✓ Tested 0 dependencies for known issues, no vulnerable paths found.

  Note that we do not currently have vulnerability data for your image.
```

{% comment %}
### Getting a detailed scan report
{% endcomment %}
{: #gettingva-detailed-scan-report }
### スキャン結果詳細

{% comment %}
You can get a detailed scan report about a Docker image by providing the Dockerfile used to create the image. The syntax is `docker scan --file PATH_TO_DOCKERFILE DOCKER_IMAGE`.
{% endcomment %}
イメージを作り出すために用いられた Dockerfile を指定することで、Docker イメージに関する詳細なスキャン結果を得ることができます。
コマンド文法は `docker scan --file <Dockerfileへのパス> <Dockerイメージ>` というものです。

{% comment %}
For example, if you apply the option to the `docker-scan` test image, it displays the following result:
{% endcomment %}
たとえばテストイメージに対してそのオプションを指定すると、以下のような結果が表示されます。

```shell
$ docker scan --file Dockerfile docker-scan:e2e
Testing docker-scan:e2e
...
✗ High severity vulnerability found in perl
  Description: Integer Overflow or Wraparound
  Info: https://snyk.io/vuln/SNYK-DEBIAN10-PERL-570802
  Introduced through: git@1:2.20.1-2+deb10u3, meta-common-packages@meta
  From: git@1:2.20.1-2+deb10u3 > perl@5.28.1-6
  From: git@1:2.20.1-2+deb10u3 > liberror-perl@0.17027-2 > perl@5.28.1-6
  From: git@1:2.20.1-2+deb10u3 > perl@5.28.1-6 > perl/perl-modules-5.28@5.28.1-6
  and 3 more...
  Introduced by your base image (golang:1.14.6)

Organization:      docker-desktop-test
Package manager:   deb
Target file:       Dockerfile
Project name:      docker-image|99138c65ebc7
Docker image:      99138c65ebc7
Base image:        golang:1.14.6
Licenses:          enabled

Tested 200 dependencies for known issues, found 157 issues.

According to our scan, you are currently using the most secure version of the selected base image
```

{% comment %}
### Excluding the base image
{% endcomment %}
{: #excluding-the-base-image }
### ベースイメージの除外

{% comment %}
When using docker scan with the `--file` flag, you can also add the `--exclude-base` tag. This excludes the base image (specified in the Dockerfile using the `FROM` directive) vulnerabilities from your report. For example:
{% endcomment %}
`--file` フラグを使って docker scan を行う場合に `--exclude-base` フラグを同時に指定することができます。
このフラグは、ぜい弱性スキャン結果からベースイメージ（Dockerfile において `FROM` ディレクティブにより指定されたもの）を除外します。
たとえば以下のとおりです。

```shell
$ docker scan --file Dockerfile --exclude-base docker-scan:e2e
Testing docker-scan:e2e
...
✗ Medium severity vulnerability found in libidn2/libidn2-0
  Description: Improper Input Validation
  Info: https://snyk.io/vuln/SNYK-DEBIAN10-LIBIDN2-474100
  Introduced through: iputils/iputils-ping@3:20180629-2+deb10u1, wget@1.20.1-1.1, curl@7.64.0-4+deb10u1, git@1:2.20.1-2+deb10u3
  From: iputils/iputils-ping@3:20180629-2+deb10u1 > libidn2/libidn2-0@2.0.5-1+deb10u1
  From: wget@1.20.1-1.1 > libidn2/libidn2-0@2.0.5-1+deb10u1
  From: curl@7.64.0-4+deb10u1 > curl/libcurl4@7.64.0-4+deb10u1 > libidn2/libidn2-0@2.0.5-1+deb10u1
  and 3 more...
  Introduced in your Dockerfile by 'RUN apk add -U --no-cache wget tar'



Organization:      docker-desktop-test
Package manager:   deb
Target file:       Dockerfile
Project name:      docker-image|99138c65ebc7
Docker image:      99138c65ebc7
Base image:        golang:1.14.6
Licenses:          enabled

Tested 200 dependencies for known issues, found 16 issues.
```

{% comment %}
### Viewing the JSON output
{% endcomment %}
{: #viewing-the-json-output }
### JSON 書式による出力

{% comment %}
You can also display the scan result as a JSON output by adding the `--json` flag to the command. For example:
{% endcomment %}
コマンドに対して `--json` フラグを指定することで、スキャン結果を JSON 書式により表示することができます。
たとえば以下のとおりです。

```shell
$ docker scan --json hello-world
{
  "vulnerabilities": [],
  "ok": true,
  "dependencyCount": 0,
  "org": "docker-desktop-test",
  "policy": "# Snyk (https://snyk.io) policy file, patches or ignores known vulnerabilities.\nversion: v1.19.0\nignore: {}\npatch: {}\n",
  "isPrivate": true,
  "licensesPolicy": {
    "severities": {},
    "orgLicenseRules": {
      "AGPL-1.0": {
        "licenseType": "AGPL-1.0",
        "severity": "high",
        "instructions": ""
      },
      ...
      "SimPL-2.0": {
        "licenseType": "SimPL-2.0",
        "severity": "high",
        "instructions": ""
      }
    }
  },
  "packageManager": "linux",
  "ignoreSettings": null,
  "docker": {
    "baseImageRemediation": {
      "code": "SCRATCH_BASE_IMAGE",
      "advice": [
        {
          "message": "Note that we do not currently have vulnerability data for your image.",
          "bold": true,
          "color": "yellow"
        }
      ]
    },
    "binariesVulns": {
      "issuesData": {},
      "affectedPkgs": {}
    }
  },
  "summary": "No known vulnerabilities",
  "filesystemPolicy": false,
  "uniqueCount": 0,
  "projectName": "docker-image|hello-world",
  "path": "hello-world"
}
```

{% comment %}
### Checking the dependency tree
{% endcomment %}
{: #checking-the-dependency-tree }
### 依存パッケージツリーの確認

{% comment %}
To view the dependency tree of your image, use the --dependency-tree flag. This displays all the dependencies before the scan result. For example:
{% endcomment %}
イメージの依存パッケージを確認するには --dependency-tree フラグを利用します。
これはスキャン前に、依存パッケージをすべて表示するものです。
たとえば以下です。

```shell
$ docker scan --dependency-tree debian:buster

$ docker-image|99138c65ebc7 @ latest
     ├─ ca-certificates @ 20200601~deb10u1
     │  └─ openssl @ 1.1.1d-0+deb10u3
     │     └─ openssl/libssl1.1 @ 1.1.1d-0+deb10u3
     ├─ curl @ 7.64.0-4+deb10u1
     │  └─ curl/libcurl4 @ 7.64.0-4+deb10u1
     │     ├─ e2fsprogs/libcom-err2 @ 1.44.5-1+deb10u3
     │     ├─ krb5/libgssapi-krb5-2 @ 1.17-3
     │     │  ├─ e2fsprogs/libcom-err2 @ 1.44.5-1+deb10u3
     │     │  ├─ krb5/libk5crypto3 @ 1.17-3
     │     │  │  └─ krb5/libkrb5support0 @ 1.17-3
     │     │  ├─ krb5/libkrb5-3 @ 1.17-3
     │     │  │  ├─ e2fsprogs/libcom-err2 @ 1.44.5-1+deb10u3
     │     │  │  ├─ krb5/libk5crypto3 @ 1.17-3
     │     │  │  ├─ krb5/libkrb5support0 @ 1.17-3
     │     │  │  └─ openssl/libssl1.1 @ 1.1.1d-0+deb10u3
     │     │  └─ krb5/libkrb5support0 @ 1.17-3
     │     ├─ libidn2/libidn2-0 @ 2.0.5-1+deb10u1
     │     │  └─ libunistring/libunistring2 @ 0.9.10-1
     │     ├─ krb5/libk5crypto3 @ 1.17-3
     │     ├─ krb5/libkrb5-3 @ 1.17-3
     │     ├─ openldap/libldap-2.4-2 @ 2.4.47+dfsg-3+deb10u2
     │     │  ├─ gnutls28/libgnutls30 @ 3.6.7-4+deb10u4
     │     │  │  ├─ nettle/libhogweed4 @ 3.4.1-1
     │     │  │  │  └─ nettle/libnettle6 @ 3.4.1-1
     │     │  │  ├─ libidn2/libidn2-0 @ 2.0.5-1+deb10u1
     │     │  │  ├─ nettle/libnettle6 @ 3.4.1-1
     │     │  │  ├─ p11-kit/libp11-kit0 @ 0.23.15-2
     │     │  │  │  └─ libffi/libffi6 @ 3.2.1-9
     │     │  │  ├─ libtasn1-6 @ 4.13-3
     │     │  │  └─ libunistring/libunistring2 @ 0.9.10-1
     │     │  ├─ cyrus-sasl2/libsasl2-2 @ 2.1.27+dfsg-1+deb10u1
     │     │  │  └─ cyrus-sasl2/libsasl2-modules-db @ 2.1.27+dfsg-1+deb10u1
     │     │  │     └─ db5.3/libdb5.3 @ 5.3.28+dfsg1-0.5
     │     │  └─ openldap/libldap-common @ 2.4.47+dfsg-3+deb10u2
     │     ├─ nghttp2/libnghttp2-14 @ 1.36.0-2+deb10u1
     │     ├─ libpsl/libpsl5 @ 0.20.2-2
     │     │  ├─ libidn2/libidn2-0 @ 2.0.5-1+deb10u1
     │     │  └─ libunistring/libunistring2 @ 0.9.10-1
     │     ├─ rtmpdump/librtmp1 @ 2.4+20151223.gitfa8646d.1-2
     │     │  ├─ gnutls28/libgnutls30 @ 3.6.7-4+deb10u4
     │     │  ├─ nettle/libhogweed4 @ 3.4.1-1
     │     │  └─ nettle/libnettle6 @ 3.4.1-1
     │     ├─ libssh2/libssh2-1 @ 1.8.0-2.1
     │     │  └─ libgcrypt20 @ 1.8.4-5
     │     └─ openssl/libssl1.1 @ 1.1.1d-0+deb10u3
     ├─ gnupg2/dirmngr @ 2.2.12-1+deb10u1
    ...

Organization:      docker-desktop-test
Package manager:   deb
Project name:      docker-image|99138c65ebc7
Docker image:      99138c65ebc7
Licenses:          enabled

Tested 200 dependencies for known issues, found 157 issues.

For more free scans that keep your images secure, sign up to Snyk at https://dockr.ly/3ePqVcp.
```

{% comment %}
## Provider authentication
{% endcomment %}
{: #provider-authentication }
## プロバイダー認証

{% comment %}
If you have an existing Snyk account, you can directly use your Snyk [API token](https://app.snyk.io/account){: target="_blank" class="_"}:
{% endcomment %}
すでに Snyk アカウントを持っている場合は、Snyk の [API トークン](https://app.snyk.io/account){: target="_blank" class="_"} を直接利用することができます。

```shell
$ docker scan --login --token <Synk認証トークン>

Your account has been authenticated. Snyk is now ready to be used.
```

{% comment %}
If you use the `--login` flag without any token, you will be redirected to the Snyk website to login.
{% endcomment %}
トークンを指定せずに `--login` フラグを用いた場合、Snyk のウェブサイトにリダイレクトされてログインを行います。

{% comment %}
## Known issues
{% endcomment %}
{: #known-issues }
## 既知の問題

{% comment %}
**WSL 2**
{% endcomment %}
**WSL 2 の場合**

{% comment %}
- The Vulnerability scanning feature doesn’t work with Alpine distributions.
- If you are using Debian and OpenSUSE distributions, the login process only works with the `--token` flag, you won’t be redirected to the Snyk website for authentication.
{% endcomment %}
- ぜい弱性スキャン機能は Alpine ディストリビューションにおいては動作しません。
- ディストリビューション Debian または OpenSUSE を利用している場合、ログイン処理は `--token` フラグを利用した場合にのみ動作します。その場合、Synk ウェブサイトへはリダイレクトされません。

{% comment %}
## Feedback
{% endcomment %}
{: #feedback }
## フィードバック

{% comment %}
Thank you for trying out the beta release of vulnerability scanning for Docker local images. Your feedback is very important to us. Let us know your feedback by creating an issue in the Let us know your feedback by creating an issue in the [scan-cli-plugin](https://github.com/docker/scan-cli-plugin/issues/new){: target="_blank" class="_"} GitHub repository.
{% endcomment %}
Docker ローカルイメージに対するぜい弱性スキャン機能のベータ版をご利用いただき、ありがとうございます。
みなさんからのフィードバックを頂くことが重要です。
フィードバックは GitHub リポジトリ内の [scan-cli-plugin](https://github.com/docker/scan-cli-plugin/issues/new){: target="_blank" class="_"} において issue を生成してお知らせください。
