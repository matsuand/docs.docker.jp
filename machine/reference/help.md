---
description: Show command help
keywords: machine, help, subcommand
title: docker-machine help
hide_from_sitemap: true
---

<ul class="nav nav-tabs">
  <li class="active"><a data-toggle="tab" href="#origin1">英語表記</a></li>
  <li><a data-toggle="tab" href="#japanese1">日本語訳</a></li>
</ul>
<div class="tab-content">
  <div id="origin1" class="tab-pane fade in active">
{% capture original-content %}
```none
Usage: docker-machine help [arg...]

Shows a list of commands or help for one command
```
{% endcapture %}
{{ original-content | markdownify }}
</div>
<div id="japanese1" class="tab-pane fade" markdown="1">
{% capture japanese-content %}
```none
利用方法: docker-machine help [arg...]

コマンド一覧、または指定されたコマンドのヘルプを表示します。
```
{% endcapture %}
{{ japanese-content | markdownify }}
</div>
</div>

{% comment %}
Usage: docker-machine help _subcommand_
{% endcomment %}

{% comment %}
For example:
{% endcomment %}
たとえば以下のとおりです。

<ul class="nav nav-tabs">
  <li class="active"><a data-toggle="tab" href="#origin2">英語表記</a></li>
  <li><a data-toggle="tab" href="#japanese2">日本語訳</a></li>
</ul>
<div class="tab-content">
  <div id="origin2" class="tab-pane fade in active">
{% capture original-content %}
```bash
$ docker-machine help config

Usage: docker-machine config [OPTIONS] [arg...]

Print the connection config for machine

Description:
   Argument is a machine name.

Options:

   --swarm      Display the Swarm config instead of the Docker daemon
```
{% endcapture %}
{{ original-content | markdownify }}
</div>
<div id="japanese2" class="tab-pane fade" markdown="1">
{% capture japanese-content %}
```bash
$ docker-machine help config

利用方法: docker-machine config [オプション] [arg...]

マシンの接続設定を表示します。

内容説明:
   引数にはマシン名を指定します。

オプション:

   --swarm      Docker デーモンでなく Swarm の設定を表示します。
```
{% endcapture %}
{{ japanese-content | markdownify }}
</div>
</div>
