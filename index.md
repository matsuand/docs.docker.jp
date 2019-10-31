---
description: Docker ドキュメントホームページ
keywords: Docker, documentation, manual, guide, reference, api, samples
landing: true
title: Docker ドキュメント
lang: ja_JP
notoc: true
notags: true
skip_read_time: true
---
{% assign page.title = site.name %}

<div class="row">
<div markdown="1" class="col-xs-12 col-sm-12 col-md-12 col-lg-6 block">

## Docker をはじめよう

{% comment %}
Try our multi-part walkthrough that covers writing your first app,
data storage, networking, and swarms, and ends with your app running on
production servers in the cloud. Total reading time is less than an hour.
{% endcomment %}
さまざまなウォークスルーを進めていくために、まずはアプリを作るところから始めて、データストレージ、ネットワーク、スウォームと進みます。最終的にはクラウド上にサーバーアプリケーションを稼動させるところまで行きましょう。すべてを読むのに１時間もかかりません。

[Docker をはじめよう](/get-started/){: class="button outline-btn"}

</div>
<div markdown="1" class="col-xs-12 col-sm-12 col-md-12 col-lg-6 block">

## Docker Enterprise のすすめ

{% comment %}
Run your solution in production with Docker Enterprise to get a
management dashboard, security scanning, LDAP integration, content signing,
multi-cloud support, and more. Click below to test-drive a running instance of
Docker Enterprise without installing anything.
{% endcomment %}
Docker Enterprise をソリューションにおいて利用すれば、管理ダッシュボード、セキュリティスキャニング、LDAP 統合、コンテンツ認証、マルチクラウド対応などをさまざまに行うことができます。
以下をクリックして、Docker エンタープライズインスタンスを試してみてください。インストールは一切不要です。

[Docker Enterprise のすすめ](https://trial.docker.com){: class="button outline-btn" onclick="ga('send', 'event', 'EE Trial Referral', 'Front Page', 'Click');"}

</div>
</div>

{% comment %}
## Docker products
{% endcomment %}
{: #docker-products }
## Docker 製品

<div class="row">
<div markdown="1" class="col-xs-12 col-sm-12 col-md-12 col-lg-6 block">

{% comment %}
### Docker Engine - Community
{% endcomment %}
{: #docker-engine-community }
### Docker Engine - Community

{% comment %}
Get started with Docker and experimenting with container-based apps. Docker Engine - Community
is available on many platforms, from desktop to cloud to server. Build and share
containers and automate the development pipeline from a single environment.
Choose the Edge channel to get access to the latest features, or the Stable
channel for more predictability.
{% endcomment %}
Docker を使い、コンテナーをベースとしたアプリケーションを体験しましょう。
Docker Engine - Community はデスクトップからクラウドのサーバーに至るまで、多くのプラットフォームで利用可能です。
単一の環境から自動デプロイパイプラインを通し、コンテナーを構築、共有します。
最新機能をいち早く試したい場合はエッジ（edge）チャンネルを選んでください。
また理解できているものを利用したい場合は安定（stable）チャンネルを選んでください。

{% comment %}
[Learn more about Docker Engine - Community](/install/){: class="button outline-btn"}
{% endcomment %}
[Docker Engine - Community について学ぶ](/install/){: class="button outline-btn"}

</div>
<div markdown="1" class="col-xs-12 col-sm-12 col-md-12 col-lg-6 block">

### Docker Enterprise

{% comment %}
Designed for enterprise development and IT teams who build, ship, and run
business critical applications in production at scale. Integrated, certified,
and supported to provide enterprises with the most secure container platform in
the industry to modernize all applications. Docker Enterprise comes with enterprise
[add-ons](#docker-ee-add-ons) like Universal Control Plane (UCP) for managing and
orchestrating the container runtime, and Docker Trusted Registry (DTR) for storing and
securing images in an enterprise grade registry.
{% endcomment %}
エンタープライズ開発や IT チーム向けに設計されたものであり、最重要のビジネスアプリケーションが、稼働中にその規模を拡大していっても、アプリケーションの構築、導入、実行を容易に実現できます。
エンタープライズ開発の統合、認証、サポートを行い、先進的なアプリケーションを提供する業界において、もっとも安全なコンテナープラットフォームを提供します。
Docker Enterprise には Universal Control Plane (UCP) や Docker Trusted Registry (DTR) といった[アドオン](#docker-ee-add-ons) が含まれます。
UCP はコンテナー実行時の管理とオーケストレーションを行います。
DTR はエンタープライズレベルのレジストリにおいてイメージの保存と保護を行います。

{% comment %}
[Learn more about Docker Enterprise products](/ee/supported-platforms/){: class="button outline-btn"}
{% endcomment %}
[Docker Enterprise 製品について学ぶ](/ee/supported-platforms/){: class="button outline-btn"}

</div>
</div><!-- end row -->

## どこでも Docker 環境

<div class="component-container">
    <!--start row-->
    <div class="row">
        <div class="col-sm-12 col-md-12 col-lg-4 block">
            <div class="component">
                <div class="component-icon">
                    <a href="docker-for-mac/"> <img src="../images/apple_48.svg" alt="Docker Desktop for Mac"> </a>
                </div>
                <h3 id="docker-for-mac"><a href="docker-for-mac/">Docker Desktop for Mac</a></h3>
                <!--
                <p>A native application using the macOS sandbox security model which delivers all Docker tools to your Mac.</p>
                -->
                <p>Mac 上ですべての Docker ツールを実行するために macOS サンドボックスセキュリティモデルを使うネイティブなアプリケーションです。</p>
            </div>
        </div>
        <div class="col-sm-12 col-md-12 col-lg-4 block">
            <div class="component">
                <div class="component-icon">
                    <a href="docker-for-windows/"> <img src="../images/windows_48.svg" alt="Docker Desktop for Windows"> </a>
                </div>
                <h3 id="docker-for-windows"><a href="docker-for-windows/">Docker Desktop for Windows</a></h3>
                <!--
                <p>A native Windows application which delivers all Docker tools to your Windows computer.</p>
                -->
                <p>Windows コンピューター上ですべての Docker ツールを実行するためのネイティブ Windows アプリケーションです。</p>
            </div>
        </div>
        <div class="col-sm-12 col-md-12 col-lg-4 block">
            <div class="component">
                <div class="component-icon">
                    <a href="install/linux/ubuntu/"> <img src="../images/linux_48.svg" alt="Docker for Linux"> </a>
                </div>
                <h3 id="docker-for-linux"><a href="install/linux/ubuntu/">Docker for Linux</a></h3>
                <!--
                <p>Install Docker on a computer which already has a Linux distribution installed.</p>
                -->
                <p>インストール済みの Linux ディストリビューション上に Docker をインストールします。</p>
            </div>
        </div>
    </div>
</div>
