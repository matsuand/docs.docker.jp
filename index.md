---
description: Docker ドキュメントホームページ
keywords: Docker, documentation, manual, guide, reference, api, samples
landing: true
title: Docker ドキュメント
lang: ja_JP
notoc: true
notags: true
---
{% assign page.title = site.name %}

<div class="row">
<div markdown="1" class="col-xs-12 col-sm-12 col-md-12 col-lg-6 block">

## Docker をはじめよう

<!--
Try our new multi-part walkthrough that covers writing your first app,
data storage, networking, and swarms, and ends with your app running on
production servers in the cloud. Total reading time is less than an hour.
-->
さまざまなウォークスルーを進めていくために、まずはアプリを作るところから始めて、データストレージ、ネットワーク、スウォームと進みます。最終的にはクラウド上にサーバーアプリケーションを稼動させるところまで行きましょう。すべてを読むのに１時間もかかりません。

[Docker をはじめよう](/get-started/){: class="button outline-btn"}

</div>
<div markdown="1" class="col-xs-12 col-sm-12 col-md-12 col-lg-6 block">

## Docker エンタープライズエディションのすすめ

<!--
Run your solution in production with Docker Enterprise Edition to get a
management dashboard, security scanning, LDAP integration, content signing,
multi-cloud support, and more. Click below to test-drive a running instance of
Docker EE without installing anything.
-->
Docker エンタープライズエディションをソリューションにおいて利用すれば、管理ダッシュボード、セキュリティスキャニング、LDAP 統合、コンテンツ認証、マルチクラウド対応などをさまざまに行うことができます。
以下をクリックして、Docker EE インスタンスを試してみてください。インストールは一切不要です。

[Docker エンタープライズエディションのすすめ](https://trial.docker.com){: class="button outline-btn" onclick="ga('send', 'event', 'EE Trial Referral', 'Front Page', 'Click');"}

</div>
</div>

## Docker のエディション

<div class="row">
<div markdown="1" class="col-xs-12 col-sm-12 col-md-12 col-lg-6 block">

### Docker コミュニティエディション

<!--
Get started with Docker and experimenting with container-based apps. Docker CE
is available on many platforms, from desktop to cloud to server. Build and share
containers and automate the development pipeline from a single environment.
Choose the Edge channel to get access to the latest features, or the Stable
channel for more predictability.
-->
Docker を使い、コンテナーをベースとしたアプリケーションを体験しましょう。
Docker CE はデスクトップからクラウドのサーバーに至るまで、多くのプラットフォームで利用可能です。
単一の環境から自動デプロイパイプラインを通し、コンテナーを構築、共有します。
最新機能をいち早く試したい場合はエッジ（edge）チャンネルを選んでください。
また理解できているものを利用したい場合は安定（stable）チャンネルを選んでください。

<!--
[Learn more about Docker CE](/install/){: class="button outline-btn"}
-->
[Docker CE について学ぶ](/install/){: class="button outline-btn"}

</div>
<div markdown="1" class="col-xs-12 col-sm-12 col-md-12 col-lg-6 block">

### Docker エンタープライズエディション

<!--
Designed for enterprise development and IT teams who build, ship, and run
business critical applications in production at scale. Integrated, certified,
and supported to provide enterprises with the most secure container platform in
the industry to modernize all applications. Docker EE Advanced comes with enterprise
[add-ons](#docker-ee-add-ons) like UCP and DTR.
-->
エンタープライズ開発や IT チーム向けに設計されたものであり、最重要のビジネスアプリケーションが、稼働中にその規模を拡大していっても、アプリケーションの構築、導入、実行を容易に実現できます。エンタープライズ開発の統合、認証、サポートを行い、先進的なアプリケーションを提供する業界において、もっとも安全なコンテナ・プラットフォームを提供します。

<!--
[Learn more about Docker EE](/ee/supported-platforms/){: class="button outline-btn"}
-->
[Docker EE について学ぶ](/ee/supported-platforms/){: class="button outline-btn"}

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
