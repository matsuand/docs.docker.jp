---
title: Docker Engine のインストール
description: Lists the installation methods
keywords: docker, installation, install, Docker Engine, Docker Engine, docker editions, stable, edge
redirect_from:
- /engine/installation/linux/
- /engine/installation/linux/frugalware/
- /engine/installation/frugalware/
- /engine/installation/linux/other/
- /engine/installation/linux/archlinux/
- /engine/installation/linux/cruxlinux/
- /engine/installation/linux/gentoolinux/
- /engine/installation/linux/docker-ce/
- /engine/installation/linux/docker-ee/
- /engine/installation/
- /en/latest/installation/
toc_max: 2
---


{% comment %}
## Supported platforms
{% endcomment %}
{: #supported-platforms }
## 対応プラットフォーム

{% comment %}
Docker Engine is available on a variety of [Linux platforms](#server),
[macOS](../../docker-for-mac/install.md) and [Windows 10](../../docker-for-windows/install.md)
through Docker Desktop, and as a [static binary installation](binaries.md). Find
your preferred operating system below.
{% endcomment %}
Docker Engine は Docker Desktop を通じて、各種の [Linux プラットフォーム](#server) や [macOS](../../docker-for-mac/install.md)、[Windows 10](../../docker-for-windows/install.md) において利用可能です。
また [バイナリによるインストール](binaries.md) もできます。
以下の中から好みに合ったオペレーティングシステムを選んでください。

{% comment %}
#### Desktop
{% endcomment %}
{: #desktop }
#### デスクトップ

{% assign yes = '![yes](../../images/green-check.svg){: style="height: 14px; margin: 0 auto"}' %}

{% comment %}
| Platform                                                          | x86_64 / amd64                                   |
|:------------------------------------------------------------------|:------------------------------------------------:|
| [Docker Desktop for Mac (macOS)](../../docker-for-mac/install.md) | [{{ yes }}](../../docker-for-mac/install.md)     |
| [Docker Desktop for Windows](../../docker-for-windows/install.md) | [{{ yes }}](../../docker-for-windows/install.md) |
{% endcomment %}
| プラットフォーム                                                  | x86_64 / amd64                                   |
|:------------------------------------------------------------------|:------------------------------------------------:|
| [Docker Desktop for Mac (macOS)](../../docker-for-mac/install.md) | [{{ yes }}](../../docker-for-mac/install.md)     |
| [Docker Desktop for Windows](../../docker-for-windows/install.md) | [{{ yes }}](../../docker-for-windows/install.md) |

{% comment %}
#### Server
{% endcomment %}
{: #server }
#### サーバー

{% comment %}
Docker provides `.deb` and `.rpm` packages from the following Linux distributions
and architectures:
{% endcomment %}
Docker は `.deb` や `.rpm` といったパッケージの形式により、以下の Linux ディストリビューションやアークテクチャー向けに提供されています。

{% comment %}
| Platform              | x86_64 / amd64         | ARM                      | ARM64 / AARCH64        |
|:----------------------|:-----------------------|:-------------------------|:-----------------------|
| [CentOS](centos.md)   | [{{ yes }}](centos.md) |                          | [{{ yes }}](centos.md) |
| [Debian](debian.md)   | [{{ yes }}](debian.md) | [{{ yes }}](debian.md)   | [{{ yes }}](debian.md) |
| [Fedora](fedora.md)   | [{{ yes }}](fedora.md) |                          | [{{ yes }}](fedora.md) |
| [Raspbian](debian.md) |                        | [{{ yes }}](debian.md)   | [{{ yes }}](debian.md) |
| [Ubuntu](ubuntu.md)   | [{{ yes }}](ubuntu.md) | [{{ yes }}](ubuntu.md)   | [{{ yes }}](ubuntu.md) |
{% endcomment %}
| プラットフォーム      | x86_64 / amd64         | ARM                      | ARM64 / AARCH64        |
|:----------------------|:-----------------------|:-------------------------|:-----------------------|
| [CentOS](centos.md)   | [{{ yes }}](centos.md) |                          | [{{ yes }}](centos.md) |
| [Debian](debian.md)   | [{{ yes }}](debian.md) | [{{ yes }}](debian.md)   | [{{ yes }}](debian.md) |
| [Fedora](fedora.md)   | [{{ yes }}](fedora.md) |                          | [{{ yes }}](fedora.md) |
| [Raspbian](debian.md) |                        | [{{ yes }}](debian.md)   | [{{ yes }}](debian.md) |
| [Ubuntu](ubuntu.md)   | [{{ yes }}](ubuntu.md) | [{{ yes }}](ubuntu.md)   | [{{ yes }}](ubuntu.md) |

{% comment %}
##### Other Linux distributions
{% endcomment %}
{: #other-linux-distributions }
##### 上記以外の Linux ディストリビューション

{% comment %}
> **Note**
>
> While the instructions below may work, Docker does not test or verify
> installation on derivatives.
{% endcomment %}
> **メモ**
>
> 以降に示す手順は他の派生ディストリビューションで動作するかもしれませんが、Docker ではそういったディストリビューションへのインストールの確認検証は行っていません。

{% comment %}
- Users of Debian derivatives such as "BunsenLabs Linux", "Kali Linux" or
  "LMDE" (Debian-based Mint) should follow the installation instructions for
  [Debian](debian.md), substituting the version of their distro for the
  corresponding Debian release. Refer to the documentation of your distro to find
  which Debian release corresponds with your derivative version.
{% endcomment %}
- Debian 派生ディストリビューション、たとえば "BunsenLabs Linux"、"Kali Linux"、"LMDE" (Debian ベースの Mint) などを利用するユーザーは、[Debian](debian.md) に示すインストール手順に従ってください。
  その際には、Debian のリリースに対応づいた、各ディストリビューションのバージョンを利用してください。
  利用しているディストリビューションがどの Debian リリースに対応しているかは、ディストリビューションのドキュメントを参照してください。
{% comment %}
- Likewise, users of Ubuntu derivatives such as "Kubuntu", "Lubuntu" or "Xubuntu"
  should follow the installation instructions for [Ubuntu](ubuntu.md),
  substituting the version of their distro for the corresponding Ubuntu release.
  Refer to the documentation of your distro to find which Ubuntu release
  corresponds with your derivative version.
{% endcomment %}
- 同様に Ubuntu 派生ディストリビューション、たとえば "Kubuntu"、"Lubuntu"、"Xubuntu" などを利用するユーザーは、[Ubuntu](ubuntu.md) に示すインストール手順に従ってください。
  その際には、Ubuntu のリリースに対応づいた、各ディストリビューションのバージョンを利用してください。
  利用しているディストリビューションがどの Ubuntu リリースに対応しているかは、ディストリビューションのドキュメントを参照してください。
{% comment %}
- Some Linux distributions are providing a package of Docker Engine through their
  package repositories. These packages are built and maintained by the Linux
  distribution's package maintainers and may have differences in configuration
  or built from modified source code. Docker is not involved in releasing these
  packages and bugs or issues involving these packages should be reported in
  your Linux distribution's issue tracker.
{% endcomment %}
- Linux ディストリビューションの中には、パッケージリポジトリから Docker Engine パッケージを提供しているものがあります。
  このようなパッケージは、そのパッケージ配布の担当者がビルドし保守を行っています。
  そこでは異なる設定を行ったり、ソースコードを修正してビルドしたりしているかもしれません。
  Docker はそういったパッケージのリリースには関わっていません。
  そのパッケージに対するバグや問題報告は、各 Linux ディストリビューションの issue トラッカーなどに報告してください。

{% comment %}
Docker provides [binaries](binaries.md) for manual installation of Docker Engine.
These binaries are statically linked and can be used on any Linux distribution.
{% endcomment %}
Docker Engine を手動でインストールするための [実行バイナリ](binaries.md) も提供されています。
このバイナリはスタティックライブラリをリンクしているため、どのような Linux ディストリビューションでも利用可能です。

{% comment %}
## Release channels
{% endcomment %}
{: #release-channels }
## リリースチャネル

{% comment %}
Docker Engine has three types of update channels, **stable**, **test**,
and **nightly**:
{% endcomment %}
Docker Engine には **安定版**（stable）、**テスト版**（test）、**最新版**（nightly）という 3 つの更新チャネルがあります。

{% comment %}
* The **Stable** channel gives you latest releases for general availability.
* The **Test** channel gives pre-releases that are ready for testing before
  general availability (GA).
* The **Nightly** channel gives you latest builds of work in progress for the
  next major release.
{% endcomment %}
* **安定版**（stable）チャネルは、正規安定版（general availability; GA）の最新リリースです。
* **テスト版**（test）チャネルは、正規安定版に向けてテスト準備に入っているプレリリース版です。
* **最新版**（nightly）チャネルは、次のメジャーリリースに向けての開発中ソースによる最新ビルド版です。

{% comment %}
### Stable
{% endcomment %}
{: #stable }
### 安定版

{% comment %}
Year-month releases are made from a release branch diverged from the master
branch. The branch is created with format `<year>.<month>`, for example
`18.09`. The year-month name indicates the earliest possible calendar
month to expect the release to be generally available. All further patch
releases are performed from that branch. For example, once `v18.09.0` is
released, all subsequent patch releases are built from the `18.09` branch.
{% endcomment %}
年月ごとのリリースは、マスターブランチから分岐したリリースブランチより生成されます。
このブランチ名は `<年>.<月>` という書式、具体例としては `18.09` などです。
年月による名前は、正規安定版としてリリース予定の、最も早い年月を意味しています。
これ以降のパッチリリースは、そのブランチから生成されます。
たとえば `v18.09.0` がリリースされると、この後に続くパッチリリースは `18.09` ブランチからビルドされます。

{% comment %}
### Test
{% endcomment %}
{: #test }
### テスト版

{% comment %}
In preparation for a new year-month release, a branch is created from
the master branch with format `YY.mm` when the milestones desired by
Docker for the release have achieved feature-complete. Pre-releases
such as betas and release candidates are conducted from their respective release
branches. Patch releases and the corresponding pre-releases are performed
from within the corresponding release branch.
{% endcomment %}
次の年月によるリリースに向けては、マスターブランチから新たなブランチが `YY.mm` の形式で生成されます。
これは Docker のリリースに向けて設定されたマイルストーンにおいて、機能実現を達成したときに生成されます。
ベータ版やリリース候補版などのプレリリース版は、対応するリリースブランチに基づいて作業が行われます。
パッチリリースとそれに対応するプレリリース版も、対応するリリースブランチに基づいて作業が行われます。

{% comment %}
### Nightly
{% endcomment %}
{: #nightly }
### 最新版

{% comment %}
Nightly builds give you the latest builds of work in progress for the next major
release. They are created once per day from the master branch with the version
format:
{% endcomment %}
最新版は、次のメジャーリリースに向けての開発中ソースによる最新ビルド版です。
これは毎日、マスターブランチから生成され、その名前は以下の書式です。

    0.0.0-YYYYmmddHHMMSS-abcdefabcdef

{% comment %}
where the time is the commit time in UTC and the final suffix is the prefix
of the commit hash, for example `0.0.0-20180720214833-f61e0f7`.
{% endcomment %}
ここにおいて、時刻は UTC によるコミット時刻を表わし、最後のサフィックスはコミットハッシュ値の先頭文字列です。
たとえば `0.0.0-20180720214833-f61e0f7` のようになります。

{% comment %}
These builds allow for testing from the latest code on the master branch.
{% endcomment %}
このビルドは、マスターブランチから得られる最新コードを使ったもので、テストが可能なものです。

{% comment %}
> **Note:**
> No qualifications or guarantees are made for the nightly builds.
{% endcomment %}
> **メモ:**
> 最新版のビルドでは、その品質や動作の保証はありません。

{% comment %}
## Support
{% endcomment %}
{: #support }
## サポート

{% comment %}
Docker Engine releases of a year-month branch are supported with patches as
needed for one month after the next year-month general availability release.
{% endcomment %}
年月で表わされたブランチからのリリースがサポートされるのは、次の年月による正規安定版がリリースされた後の 1 ヶ月後までとなります。
ここには必要に応じてパッチも含まれます。

{% comment %}
This means bug reports and backports to release branches are assessed
until the end-of-life date.
{% endcomment %}
これはつまりその最終日付までの間は、そのリリースブランチに対するバグリポートやバックポートが適用されるということです。

{% comment %}
After the year-month branch has reached end-of-life, the branch may be
deleted from the repository.
{% endcomment %}
年月によるブランチが最終日付に達したとき、そのブランチはリポジトリから削除されることがあります。

{% comment %}
### Backporting
{% endcomment %}
{: #backporting }
### バックポート

{% comment %}
Backports to the Docker products are prioritized by the Docker company. A
Docker employee or repository maintainer will endeavour to ensure sensible
bugfixes make it into _active_ releases.
{% endcomment %}
Docker 製品へのバックポートは、Docker 社が優先的に行います。
Docker 社の従業員またはリポジトリの保守担当者は、バグフィックスを的確に行い、**有効な**現行リリースになるよう努めます。

{% comment %}
If there are important fixes that ought to be considered for backport to
active release branches, be sure to highlight this in the PR description
or by adding a comment to the PR.
{% endcomment %}
現行のリリースブランチに対するバックポートとして重要なフィックスと考えられるものは、プルリクエストの説明において、あるいはプルリクエストに加えるコメントにおいて、強調して示します。

{% comment %}
### Upgrade path
{% endcomment %}
{: #upgrade-path }
### アップグレードの方法

{% comment %}
Patch releases are always backward compatible with its year-month version.
{% endcomment %}
パッチリリースは、年月によるバージョンと常に下位互換性があります。

{% comment %}
### Licensing
{% endcomment %}
{: #licensing }
### ライセンス

{% comment %}
Docker is licensed under the Apache License, Version 2.0. See
[LICENSE](https://github.com/moby/moby/blob/master/LICENSE) for the full
license text.
{% endcomment %}
Docker が採用するライセンスは Apache License, Version 2.0 です。
その全文は [LICENSE](https://github.com/moby/moby/blob/master/LICENSE) を参照してください。

{% comment %}
## Reporting security issues
{% endcomment %}
{: #reporting-security-issues }
## セキュリティに関する問題報告

{% comment %}
The Docker maintainers take security seriously. If you discover a security
issue, please bring it to their attention right away!
{% endcomment %}
Docker の保守担当者は、セキュリティを重要なものとして捉えています。
セキュリティに関する問題を発見した方は、すぐにお知らせください。

{% comment %}
Please DO NOT file a public issue; instead send your report privately
to security@docker.com.
{% endcomment %}
その際には公開の issue とはしないでください。
security@docker.com 宛てに、プライベートな報告として連絡してください。

{% comment %}
Security reports are greatly appreciated, and Docker will publicly thank you
for it.
{% endcomment %}
セキュリティに関する報告を大いに歓迎します。
Docker では公開で感謝の意を表わすものとします。

{% comment %}
## Get started
{% endcomment %}
{: #get-started }
## はじめよう

{% comment %}
After setting up Docker, you can learn the basics with
[Getting started with Docker](../../get-started/index.md).
{% endcomment %}
Docker を設定し終えたら、[Docker をはじめよう](../../get-started/index.md) を通じてその基本を学んでください。
