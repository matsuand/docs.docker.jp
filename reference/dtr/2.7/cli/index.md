---
title: docker/dtr 概要
description: docker/dtr イメージにおいて利用可能なコマンドについて学ぶ。
keywords: dtr, install, uninstall, configure
---

>{% include enterprise_label_shortform.md %}

{% comment %}
This tool has commands to install, configure, and backup Docker
Trusted Registry (DTR). It also allows uninstalling DTR.
By default the tool runs in interactive mode. It prompts you for
the values needed.
{% endcomment %}
このツールは Docker Trusted Registry（DTR）をインストール、設定、バックアップするためのコマンドを提供します。
DTR をアンインストールすることもできます。
デフォルトでこのツールは、対話モードで実行されます。
必要な値はプロンプト入力が求められます。

{% comment %}
Additional help is available for each command with the '--help' option.
{% endcomment %}
各コマンドのより詳細な情報は'--help'オプションをつけて確認することができます。


{% comment %}
## Usage
{% endcomment %}
## 利用方法
{: #usage }

```bash
docker run -it --rm docker/dtr \
    command [command options]
```


{% comment %}
If not specified, `docker/dtr` uses the `latest` tag by default. To work with a different version, specify it in the command. For example, `docker run -it --rm docker/dtr:2.6.0`.
{% endcomment %}
特に指定がなければ docker/dtr はデフォルトとして最新のタグを用います。
別バージョンを対象とする場合には、コマンドにて指定します。
たとえば `docker run -it --rm docker/dtr:2.6.0` のようにします。


{% comment %}
## Commands
{% endcomment %}
## コマンド
{: #commands }

| オプション                           | 説明                                                        |
|:-------------------------------------|:------------------------------------------------------------|
| [install](install/)                   | Docker Trusted Registry をインストールします。              |
| [join](join/)                         | 既存 DTR クラスターに新たなレプリカを追加します。           |
| [reconfigure](reconfigure/)           | DTR 設定を変更します。                                      |
| [remove](remove/)                     | クラスターから DTR レプリカを削除します。                   |
| [destroy](destroy/)                   | DTR レプリカのデータを削除します。                          |
| [restore](restore/)                   | バックアップから DTR をインストール復元します。             |
| [backup](backup/)                     | DTR のバックアップを生成します。                            |
| [upgrade](upgrade/)                   | DTR 2.4.x のクラスターを本バージョンにアップグレードします。|
| [images](images/)                     | DTR のインストールに必要なイメージを一覧表示します。        |
| [emergency-repair](emergency-repair/) | DTR の quorum 不足を回復します。                            |
