---
description: docker-compose のヘルプ
keywords: fig, composition, compose, docker, orchestration, cli, help
title: docker-compose help
notoc: true
---

<ul class="nav nav-tabs">
  <li class="active"><a data-toggle="tab" href="#origin">英語表記</a></li>
  <li><a data-toggle="tab" href="#japanese">日本語訳</a></li>
</ul>
<div class="tab-content">
  <div id="origin" class="tab-pane fade in active">
{% capture original-content %}
```
Usage: help COMMAND
```
{% endcapture %}
{{ original-content | markdownify }}
</div>
<div id="japanese" class="tab-pane fade" markdown="1">
{% capture japanese-content %}
```
利用方法: help コマンド
```
{% endcapture %}
{{ japanese-content | markdownify }}
</div>
<hr>
</div>

{% comment %}
Displays help and usage instructions for a command.
{% endcomment %}
コマンドに対するヘルプと利用方法を表示します。
