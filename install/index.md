---
title: Docker CE について
description: インストール方法を一覧列記。
keywords: docker, installation, install, docker ce, docker ee, docker editions, stable, edge
redirect_from:
- /installation/
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
---

{% comment %}
Docker Community Edition (CE) is ideal for developers and small
teams looking to get started with Docker and experimenting with container-based
apps. Docker CE has three types of update channels, **stable**, **test**, and **nightly**:
{% endcomment %}
Docker Community エディション（Community Edition; CE）は個人の開発者や小さな開発チームに向けたものであり、Docker をはじめようとしたり、コンテナーベースのアプリケーションを試そうとしたりする方に適しています。
Docker CE には更新チャネルとして **安定版**（stable）、**テスト版**（test）、**最新版**（nightly）の 3 つがあります。

{% comment %}
* **Stable** gives you latest releases for general availability.
* **Test** gives pre-releases that are ready for testing before general availability.
* **Nightly** gives you latest builds of work in progress for the next major release.
{% endcomment %}
* **安定版**（stable）は、正規安定版（general availability; GA）の最新リリースです。
* **テスト版**（test）は、正規安定版に向けてテスト向けとなっているプレリリース版です。
* **最新版**（nightly）は、次のメジャーリリースに向けての開発途上の最新ビルド版です。

{% comment %}
For more information about Docker CE, see
[Docker Community Edition](https://www.docker.com/community-edition/){: target="_blank" class="_" }.
{% endcomment %}
Docker CE についての詳細は [Docker Community エディション](https://www.docker.com/community-edition/){: target="_blank" class="_" } を参照してください。

{% comment %}
## Releases
{% endcomment %}
## リリース
{: #releases }

{% comment %}
For the Docker CE engine, the open
repositories [Docker Engine](https://github.com/docker/engine) and
[Docker Client](https://github.com/docker/cli) apply.
{% endcomment %}
Docker CE エンジンには、[Docker Engine](https://github.com/docker/engine) と
[Docker Client](https://github.com/docker/cli) を提供するオープンリポジトリがあります。

{% comment %}
Releases of Docker Engine and Docker Client for general availability
are versioned using dotted triples. The components of this triple
are `YY.mm.<patch>` where the `YY.mm` component is referred to as the
year-month release. The version numbering format is chosen to illustrate
cadence and does not guarantee SemVer, but the desired date for general
availability. The version number may have additional information, such as
beta and release candidate qualifications. Such releases are considered
"pre-releases".
{% endcomment %}
Docker Engine と Docker Client の正規安定版リリースは、3 つの数字をドットで区切ったバージョン番号により管理されています。
この 3 つの数字は `YY.mm.<patch>` といった形式で、このうち `YY.mm` の部分はリリースされた年月を表わします。
バージョン番号のフォーマットは順にあがっていくように番号づけされますが、番号に意味づけを行っているわけではなく、安定版リリース予定の年月を示すにすぎません。
バージョン番号には付加情報がつくことがあります。
それはベータ版であるとか、リリース候補であるといった識別情報です。
そのようなリリースは "プレリリース" として扱われます。

{% comment %}
The cadence of the year-month releases is every 6 months starting with
the `18.09` release. The patch releases for a year-month release take
place as needed to address bug fixes during its support cycle.
{% endcomment %}
年月によるリリースは `18.09` のリリース以降 6 ヶ月ごとに行われます。
その年月リリースに対するパッチのリリースは必要に応じて行われ、年月リリースのサイクルの合間のバグフィックスとして提供されます。

{% comment %}
Docker CE binaries for a release are available on [download.docker.com](https://download.docker.com/)
as packages for the supported operating systems. Docker EE binaries are
available on the [Docker Hub](https://hub.docker.com/) for the supported operating systems. The
release channels are available for each of the year-month releases and
allow users to "pin" on a year-month release of choice. The release
channel also receives patch releases when they become available.
{% endcomment %}
Docker CE のバイナリリリースは [download.docker.com](https://download.docker.com/) から、サポートするオペレーティングシステム向けのパッケージとして提供されます。
Docker EE のバイナリリリースは [Docker Hub](https://hub.docker.com/) から、サポートするオペレーティングシステム向けに提供されます。
リリースチャネルは、個々の年月リリースを提供するものなので、年月リリースを選びやすくしています。
リリースチャネルはまた、パッチリリースが提供された際に、そのパッチリリースを得ることもできます。

{% comment %}
### Nightly builds
{% endcomment %}
### 最新版
{: #nightly-builds }

{% comment %}
Nightly builds are created once per day from the master branch. The version
number for nightly builds take the format:
{% endcomment %}
最新版（nightly）はマスターブランチから 1 日 1 回生成されます。
最新版のバージョン番号は以下のような書式です。

    0.0.0-YYYYmmddHHMMSS-abcdefabcdef

{% comment %}
where the time is the commit time in UTC and the final suffix is the prefix
of the commit hash, for example `0.0.0-20180720214833-f61e0f7`.
{% endcomment %}
日付時刻の部分は UTC でのコミット時刻です。
また最後の文字はコミットハッシュの先頭文字です。
具体的には `0.0.0-20180720214833-f61e0f7` のようになります。

{% comment %}
These builds allow for testing from the latest code on the master branch. No
qualifications or guarantees are made for the nightly builds.
{% endcomment %}
このビルドは、マスターブランチにある最新コードを使ったテストのためのものです。
最新版ビルドの品質、動作は保証されません。

{% comment %}
The release channel for these builds is called `nightly`.
{% endcomment %}
このビルドに対するリリースチャネルは `nightly` と呼ばれます。

{% comment %}
### Pre-releases
{% endcomment %}
### プレリリース
{: #pre-releases }

{% comment %}
In preparation for a new year-month release, a branch is created from
the master branch with format `YY.mm` when the milestones desired by
Docker for the release have achieved feature-complete. Pre-releases
such as betas and release candidates are conducted from their respective release
branches. Patch releases and the corresponding pre-releases are performed
from within the corresponding release branch.
{% endcomment %}
次の年月リリースに向けては、マスターブランチから新たなブランチが `YY.mm` の形式で生成されます。
これは Docker のリリースに向けて設定されたマイルストーンにおいて、機能実現を達成したときに生成されます。
ベータ版やリリース候補版などのプレリリース版は、対応するリリースブランチに基づいて作業が行われます。
パッチリリースとそれに対応するプレリリース版は、対応するリリースブランチに基づいて作業が行われます。

{% comment %}
While pre-releases are done to assist in the stabilization process, no
guarantees are provided.
{% endcomment %}
プレリリース版の作業は安定性を保って行われますが、保証されるものではありません。

{% comment %}
Binaries built for pre-releases are available in the test channel for
the targeted year-month release using the naming format `test-YY.mm`,
for example `test-18.09`.
{% endcomment %}
プレリリース版に対応するバイナリリリースを入手することができます。
これはテストチャネル内の対象となる年月リリースに対応して `test-YY.mm` といった形式、例えば `test-18.09` といった名前で提供されます。

{% comment %}
### General availability
{% endcomment %}
### 正規安定版（general availability; GA）
{: #general-availability }

{% comment %}
Year-month releases are made from a release branch diverged from the master
branch. The branch is created with format `<year>.<month>`, for example
`18.09`. The year-month name indicates the earliest possible calendar
month to expect the release to be generally available. All further patch
releases are performed from that branch. For example, once `v18.09.0` is
released, all subsequent patch releases are built from the `18.09` branch.
{% endcomment %}
年月によるリリースは、マスターブランチから分岐したリリースブランチとして生成されます。
このブランチの書式は `<年>.<月>`、たとえば `18.09` となります。
年月による名前は、正規安定版としてリリースが予定される、最も早い年月が定められます。
これに対してのパッチリリースも、同じブランチから作り出されます。
たとえば `v18.09.0` がリリースされた場合に、これに続くパッチリリースは `18.09` ブランチから作られます。

{% comment %}
Binaries built from this releases are available in the stable channel
`stable-YY.mm`, for example `stable-18.09`, as well as the corresponding
test channel.
{% endcomment %}
このリリースからビルドされるバイナリは、安定版チャネル `stable-YY.mm` から入手できます。
たとえば `stable-18.09` となります。
テストチャネルについても同様です。

{% comment %}
### Relationship between CE and EE code
{% endcomment %}
### Docker CE と EE のコード関係
{: #relationship-between-ce-and-ee-code }

{% comment %}
For a given year-month release, Docker releases both CE and EE
variants concurrently. EE is a superset of the code delivered in
CE. Docker maintains publicly visible repositories for the CE code
as well as private repositories for the EE code. Automation (a bot)
is used to keep the branches between CE and EE in sync so as features
and fixes are merged on the various branches in the CE repositories
(upstream), the corresponding EE repositories and branches are kept
in sync (downstream). While Docker and its partners make every effort
to minimize merge conflicts between CE and EE, occasionally they will
happen, and Docker will work hard to resolve them in a timely fashion.
{% endcomment %}
各年月のリリース時には、Docker CE、Docker EE の双方が同時にリリースされます。
EE は CE の提供コードの上位セットです。
Docker では、CE のコードをどなたもが見ることができる公開リポジトリ上で管理していますが、EE コードに対してはプライベートリポジトリを管理しています。CE と EE 間にあるブランチは、自動的に（bot を用いて）同期が取られています。
いろいろなブランチ上における機能や修正が CE リポジトリ上でマージされ（アップストリーム）、対応する EE リポジトリやブランチが同期されます（ダウンストリーム）。
Docker や開発パートナーは、CE と EE 間のマージ時の衝突をできるだけ少なくするように努めています。
ただし衝突は起きることがあるため、Docker では適宜、解決を図る努力を行っていきます。

{% comment %}
## Next release
{% endcomment %}
## 次期リリース
{: #next-release }

{% comment %}
The activity for upcoming year-month releases is tracked in the milestones
of the repository.
{% endcomment %}
直近の年月リリースに向けた活動は、リポジトリ内のマイルストーンによって管理されています。

{% comment %}
## Support
{% endcomment %}
## サポート
{: #support }

{% comment %}
Docker CE releases of a year-month branch are supported with patches
as needed for 7 months after the first year-month general availability
release. Docker EE releases are supported for 24 months after the first
year-month general availability release.
{% endcomment %}
年月で定めたブランチに基づいた Docker CE のリリースは、関連するパッチも含めて、正規安定版としてリリースした日から、必要に応じて 7ヶ月後までサポートされます。
Docker EE のリリースは、正規安定版のリリース日から 24 ヶ月後までサポートされます。

{% comment %}
This means bug reports and backports to release branches are assessed
until the end-of-life date.
{% endcomment %}
これはつまり、リリース後のブランチに対するバグ報告やバックポートが行われるのは、そのサポート期日までということです。

{% comment %}
After the year-month branch has reached end-of-life, the branch may be
deleted from the repository.
{% endcomment %}
年月で定められたブランチがサポート期日に達した場合、そのブランチはリポジトリから削除されます。

{% comment %}
### Reporting security issues
{% endcomment %}
### セキュリティに関する問題の報告
{: #reporting-security-issues }

{% comment %}
The Docker maintainers take security seriously. If you discover a security
issue, please bring it to their attention right away!
{% endcomment %}
Docker の開発者はセキュリティを重視しています。
セキュリティに関する問題を発見した場合は、開発者へ適切にお伝えください。

{% comment %}
Please DO NOT file a public issue; instead send your report privately
to security@docker.com.
{% endcomment %}
報告は公開で行わないでください。
そのかわりに security@docker.com に個別にあげてください。

{% comment %}
Security reports are greatly appreciated, and Docker will publicly thank you
for it. Docker also likes to send gifts — if you're into swag, make sure to
let us know. Docker currently does not offer a paid security bounty program
but are not ruling it out in the future.
{% endcomment %}
セキュリティ報告は大いに歓迎します。
Docker からは公開で感謝を示します。
また何かプレゼントでもお送りしたいと思いますが、不要ならお知らせください。
なお現時点では、セキュリティ報告に対する報償制度は提供していませんが、将来もそのとおりかどうかはわかりません。

{% comment %}
### Supported platforms
{% endcomment %}
### 対応プラットフォーム
{: #supported-platforms }

{% comment %}
Docker CE is available on multiple platforms. Use the following tables
to choose the best installation path for you.
{% endcomment %}
Docker CE は各種のプラットフォームにて利用可能です。
以下の表を参考にして、適切なインストールを選んでください。

{% comment %}
#### Desktop
{% endcomment %}
#### デスクトップ
{: #desktop }

{% assign green-check = '![yes](/install/images/green-check.svg){: style="height: 14px; margin: 0 auto"}' %}

| プラットフォーム                                                            |      x86_64       |
|:----------------------------------------------------------------------------|:-----------------:|
| [Docker Desktop for Mac (macOS)](/docker-for-mac/install/)                        | {{ green-check }} |
| [Docker Desktop for Windows (Microsoft Windows 10)](/docker-for-windows/install/) | {{ green-check }} |

{% comment %}
#### Server
{% endcomment %}
#### サーバー
{: #server }

{% assign green-check = '![yes](/install/images/green-check.svg){: style="height: 14px; margin: 0 auto"}' %}
{% assign install-prefix-ce = '/install/linux/docker-ce' %}

| プラットフォーム                            | x86_64 / amd64                                         | ARM                                                    | ARM64 / AARCH64                                        | IBM Power (ppc64le)                                    | IBM Z (s390x)                                          |
|:--------------------------------------------|:-------------------------------------------------------|:-------------------------------------------------------|:-------------------------------------------------------|:-------------------------------------------------------|:-------------------------------------------------------|
| [CentOS]({{ install-prefix-ce }}/centos/) | [{{ green-check }}]({{ install-prefix-ce }}/centos/) |                                                        | [{{ green-check }}]({{ install-prefix-ce }}/centos/) |                                                        |                                                        |
| [Debian]({{ install-prefix-ce }}/debian/) | [{{ green-check }}]({{ install-prefix-ce }}/debian/) | [{{ green-check }}]({{ install-prefix-ce }}/debian/) | [{{ green-check }}]({{ install-prefix-ce }}/debian/) |                                                        |                                                        |
| [Fedora]({{ install-prefix-ce }}/fedora/) | [{{ green-check }}]({{ install-prefix-ce }}/fedora/) |                                                        | [{{ green-check }}]({{ install-prefix-ce }}/fedora/) |                                                        |                                                        |
| [Ubuntu]({{ install-prefix-ce }}/ubuntu/) | [{{ green-check }}]({{ install-prefix-ce }}/ubuntu/) | [{{ green-check }}]({{ install-prefix-ce }}/ubuntu/) | [{{ green-check }}]({{ install-prefix-ce }}/ubuntu/) | [{{ green-check }}]({{ install-prefix-ce }}/ubuntu/) | [{{ green-check }}]({{ install-prefix-ce }}/ubuntu/) |

{% comment %}
### Backporting
{% endcomment %}
### バックポート
{: #backporting }

{% comment %}
Backports to the Docker products are prioritized by the Docker company. A
Docker employee or repository maintainer will endeavour to ensure sensible
bugfixes make it into _active_ releases.
{% endcomment %}
Docker 製品へのバックポートは Docker 社によるものが優先されます。
Docker 社の関係者およびリポジトリ管理者は、バグフィックスを慎重にアクティブなリリースに適用するよう努力していきます。

{% comment %}
If there are important fixes that ought to be considered for backport to
active release branches, be sure to highlight this in the PR description
or by adding a comment to the PR.
{% endcomment %}
重大なバグフィックスがあって、アクティブなリリースに向けてのバックポートが必要と考えられる場合は、プルリクエストにおいて目立つように説明してください。
あるいはプルリクエストにそのようなコメントをつけてください。

{% comment %}
### Upgrade path
{% endcomment %}
### アップグレードの方針
{: #upgrade-path }

{% comment %}
Patch releases are always backward compatible with its year-month version.
{% endcomment %}
パッチリリースは常に、年月に基づくリリースと後方互換性を保ちます。

{% comment %}
## Not covered
{% endcomment %}
## 対象外
{: #not-covered }

{% comment %}
As a general rule, anything not mentioned in this document may change in any release.
{% endcomment %}
一般的な取り決めとして、本ドキュメントに示していない内容は、リリースによらず変更する可能性があります。

{% comment %}
## Exceptions
{% endcomment %}
## 例外的事項
{: #exceptions }

{% comment %}
Exceptions are made in the interest of __security patches__. If a break
in release procedure or product functionality is required, it will
be communicated clearly, and the solution will be considered against
total impact.
{% endcomment %}
セキュリティパッチを優先し、これを例外的に扱うことがあります。
リリース手順や製品機能を取りやめにするような事情が発生した場合は、十分な議論を行ない、全体的な影響を考慮した上で結論を導きます。

{% comment %}
## Get started
{% endcomment %}
## はじめよう
{: #get-started }

{% comment %}
After setting up Docker, you can learn the basics with
[Getting started with Docker](/get-started/).
{% endcomment %}
Docker をセットアップしたら [Docker をはじめよう](/get-started/) を読んで基礎を学んでください。
