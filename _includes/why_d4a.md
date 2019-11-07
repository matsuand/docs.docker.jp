{% comment %}
The [Docker for {{cloudprovider}}](/docker-for-{{cloudprovider | downcase}}/)
project was created and is being actively developed to ensure that Docker users
can enjoy a fantastic out-of-the-box experience on {{cloudprovider}}. It is now
generally available and can now be used by everyone.
{% endcomment %}
[Docker for {{cloudprovider}}](/docker-for-{{cloudprovider | downcase}}/) プロジェクトは開始されて以来、活発に活動を続けています。
そして Docker ユーザーへは、{{cloudprovider}} 上に即座に実現できる優れた環境を提供しています。
今この環境は広く入手可能となり、どなたでも利用できるものとなりました。

{% comment %}
As an informed user, you might be curious to know what this project offers
you for running your development, staging, or production workloads.
{% endcomment %}
知識を豊富に持つユーザーであれば、このプロジェクトがどのように開発環境を動作させ、ステージングを行い、本番環境における作業を行うものか知りたいところでしょう。

{% comment %}
## Native to Docker
{% endcomment %}
{: #native-to-docker }
## Docker ネイティブであること

{% comment %}
Docker for {{cloudprovider}} provides a Docker-native solution that avoids
operational complexity and adding unneeded additional APIs to the Docker stack.
{% endcomment %}
Docker for {{cloudprovider}} は Docker ネイティブなソリューションを提供するものです。
操作が複雑になるわけでなく、Docker スタックに対して不必要に API を加えたりするようなこともありません。

{% comment %}
Docker for {{cloudprovider}} allows you to interact with Docker directly
(including native Docker orchestration), instead of distracting you with the
need to navigate extra layers on top of Docker. You can focus instead on the
thing that matters most: running your workloads. This helps you and your
team to deliver more value to the business faster, to speak one common
"language", and to have fewer details to keep in your head at once.
{% endcomment %}
Docker for {{cloudprovider}} は Docker を使った直接の操作（ネイティブな Docker オーケストレーションを含む）を可能としています。
Docker の上にさらに別の手法を求めるようなものではありません。
では何に注力していくべきかと言えば、それはイメージを動作させることです。
つまり開発者やチームに向けて、より多くの価値を迅速なビジネス実現に向けて提供します。
利用するのは共通の「ことば」です。
細かなことを同時に覚えていく必要はありません。

{% comment %}
The skills that you and your team have already learned, and continue to
learn, using Docker on the desktop or elsewhere automatically carry over to
using Docker on {{cloudprovider}}. The added consistency across clouds also
helps to ensure that a migration or multi-cloud strategy is easier to accomplish
in the future if desired.
{% endcomment %}
デスクトップ版などの Docker 製品を用いて、これまでに開発者やチームでは多くのスキルを得てきたはずです。
そして今も学び続けているでしょう。
そのスキルは Docker on {{cloudprovider}} を用いる際にも自動的に受け継がれます。
クラウド間においては一貫性が加わるため、移行やマルチクラウド対応などが将来必要になったとしても、容易に実現することもできます。

{% comment %}
## Skip the boilerplate and maintenance work
{% endcomment %}
{: #skip-the-boilerplate-and-maintenance-work }
## 定型的作業や保守作業の省略

{% comment %}
Docker for {{cloudprovider}} bootstraps all of the recommended infrastructure to
start using Docker on {{cloudprovider}} automatically. You don't need to worry
about rolling your own instances, security groups, or load balancers when using
Docker for {{cloudprovider}}.
{% endcomment %}
Docker for {{cloudprovider}} では、これを利用してシステム起動するために推奨されるインフラストラクチャーをすべて自動的に初期設定します。
Docker for {{cloudprovider}} を利用するにあたって、実行インスタンスをどう動かしていくのか、セキュリティグループは、ロードバランサーは、といった心配をする必要がありません。

{% comment %}
Likewise, setting up and using Docker swarm mode functionality for container
orchestration is managed across the cluster's lifecycle when you use Docker for
{{cloudprovider}}. Docker has already coordinated the various bits of automation
you would otherwise be gluing together on your own to bootstrap Docker swarm
mode on these platforms. When the cluster is finished booting, you can jump
right in and start running `docker service` commands.
{% endcomment %}
さらに Docker for {{cloudprovider}} を用いれば、コンテナーオーケストレーション実現のための Docker スウォームモードの設定とその利用が、クラスターのライフサイクル全般にわたって管理されます。
Docker では数多くの自動化が組み入れられているため、ユーザー自らがまとめるようなことをしなくても、各種プラットフォーム上において Docker スウォームモードを稼動していくことができます。
クラスターの起動が成功すれば、`docker service` コマンドを使った作業に進んでいきます。

{% comment %}
We also provide a prescriptive upgrade path that helps users upgrade between
various versions of Docker in a smooth and automatic way. Instead of
experiencing "maintenance dread" as you ponder your future responsibilities
upgrading the software you are using, you can easily upgrade to new versions
when they are released.
{% endcomment %}
確立されたアップグレード手順が提供されているので、各種の Docker バージョンにおいてのアップグレードをスムーズに自動化により行うことができます。
利用中のソフトウェアをアップグレードする際に、自分の責任として降りかかってくる「メンテナンスの恐怖」を感じる必要はありません。
新バージョンがリリースされたらすぐにアップグレードすることができます。

{% comment %}
## Minimal, Docker-focused base
{% endcomment %}
{: #minimal-docker-focused-base }
## Docker に焦点をあてて必要最小限に

{% comment %}
The custom Linux distribution used by Docker for {{cloudprovider}} is carefully
developed and configured to run Docker well. Everything from the kernel
configuration to the networking stack is customized to make it a favorable place
to run Docker. For instance, we make sure that the kernel versions are
compatible with the latest and greatest in Docker functionality, such as the
`overlay2` storage driver.
{% endcomment %}
Docker for {{cloudprovider}} は独自の Linux ディストリビューションを利用しています。
そしてこれは Docker が適切に動作するように慎重に開発され設定されています。
カーネルの設定からネットワーク関連まで、あらゆることを Docker が快適に動作するようにカスタマイズしているものです。
たとえばカーネルのバージョンは、最新の Docker 機能が最大限稼動できるように、常に互換性を保つようにしています。
`overlay2` ストレージドライバーがその例です。

{% comment %}
Instead of facing the trade-offs of a general purpose operating system, Docker's
custom Linux distribution focuses on only one thing: providing the best _Docker_
experience for you and your team.
{% endcomment %}
汎用目的のオペレーティングシステムを利用することでのトレードオフに直面することはありません。
Docker の独自 Linux ディストリビューションは、ただ 1 点を目指しています。
それはユーザーや開発チームが、最大限 _Docker_ を利用できるようにすることです。

{% comment %}
## Self-cleaning and self-healing
{% endcomment %}
{: #self-cleaning-and-self-healing }
## 自動クリーン機能と自動修正機能

{% comment %}
Even the most conscientious admin can be caught off guard by issues such as
unexpectedly aggressive logging or the Linux kernel killing memory-hungry
processes. In Docker for {{cloudprovider}}, your cluster is resilient to a
variety of such issues by default.
{% endcomment %}
たとえ慎重な管理を行っていたとしても、不意に発生する問題というものがあります。
たとえば予想に反してログ出力が頻繁に発生したり、Linux カーネルのメモリを大量消費するプロセスが停止したり、といったことです。
これが Docker for {{cloudprovider}} であれば、そういった諸問題に対してクラスターをすばやく復旧させる能力をデフォルトで有しています。

{% comment %}
Log rotation native to the host is configured for you automatically, so chatty
logs don't use up all of your disk space. Likewise, the "system prune" option
allows you to ensure unused Docker resources such as old images are cleaned up
automatically. The lifecycle of nodes is managed using auto-scaling groups or
similar constructs, so that if a node enters an unhealthy state for unforeseen
reasons, the node is taken out of load balancer rotation and/or replaced
automatically and all of its container tasks are rescheduled.
{% endcomment %}
ログローテーションの設定は、ホストに適応したものが自動的に設定されます。
長々としたログであったとしても、ディスク容量をくいつぶすようなことにはなりません。
同じようなこととして、Docker には「system prune」オプションなるものがあります。
これにより、古いイメージなど未使用の Docker リソースは自動的に削除されるようになっています。
ノードの存続期間は、自動スケーリンググループや同様の構成により管理されるので、予期しない理由によりノードが不健康な状態におちいったとすると、そのノードはロードバランサーのローテーションから除外されたり、自動的に置き換えられたりします。
そしてコンテナー内のタスクはすべて、再スケジュールされることになります。

{% comment %}
These self-cleaning and self-healing properties are enabled by default and don't
need configuration, so you can breathe easier as the risk of downtime is
reduced.
{% endcomment %}
このような自動クリーン機能や自動修正機能は、デフォルトにおいて有効になっているため、設定は不要です。
ダウンタイムのリスクは軽減されるので安心して利用できます。

{% comment %}
## Logging native to the platforms
{% endcomment %}
{: #logging-native-to-the-platforms }
## プラットフォームに適応したログ出力

{% comment %}
Centralized logging is a critical component of many modern infrastructure
stacks. To have these logs indexed and searchable proves invaluable for
debugging application and system issues as they come up. Out of the box, Docker
for {{cloudprovider}} forwards logs from containers to a native cloud provider
abstraction ({{cloudprovider_log_dest}}).
{% endcomment %}
現代のインフラストラクチャースタックにおいて、ログ処理を集中管理することは欠かせない重要な機能です。
そのログをインデックスづけして検索可能にしておくことが、アプリケーションのデバッグやシステム障害への備えとして重要です。
そして Docker for {{cloudprovider}} は、コンテナー内のログをネイティブなクラウドプロバイダー（{{cloudprovider_log_dest}}）へ出力します。

{% comment %}
## Next-generation Docker bug reporting tools
{% endcomment %}
{: #next-generation-docker-bug-reporting-tools }
## 次世代の Docker バグレポートツール

{% comment %}
One common pain point in open source issue reporting is effectively
communicating the current state of your infrastructure and the issues you are
seeing to the upstream. In Docker for {{cloudprovider}}, you receive new tools
to communicate any issues you experience quickly and securely to Docker
employees. The Docker for {{cloudprovider}} shell includes a `docker-diagnose`
script which, at your request, transmits detailed diagnostic information to
Docker support staff to reduce the traditional
"please-post-the-output-of-this-command" back and forth frequently encountered
in bug reports.
{% endcomment %}
One common pain point in open source issue reporting is effectively
communicating the current state of your infrastructure and the issues you are
seeing to the upstream. In Docker for {{cloudprovider}}, you receive new tools
to communicate any issues you experience quickly and securely to Docker
employees. The Docker for {{cloudprovider}} shell includes a `docker-diagnose`
script which, at your request, transmits detailed diagnostic information to
Docker support staff to reduce the traditional
"please-post-the-output-of-this-command" back and forth frequently encountered
in bug reports.

{% comment %}
# Try it today
{% endcomment %}
{: #try-it-today }
# すぐにはじめましょう

{% comment %}
Ready to get started? [Try Docker for {{cloudprovider}} today](/docker-for-{{cloudprovider | downcase}}/).
Search for existing issues, or create a new one, within the
[for {{cloudprovider}}](https://github.com/docker/for-{{cloudprovider | downcase}}) repository.
{% endcomment %}
準備はいいですか？
[今すぐに Docker for {{cloudprovider}} を試してみてください](/docker-for-{{cloudprovider | downcase}}/)。
なにかの問題を検索したり、報告したりする場合は [Docker for {{cloudprovider}}](https://github.com/docker/for-{{cloudprovider | downcase}}) リポジトリを利用してください。
