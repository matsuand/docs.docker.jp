---
description: マシン状態を取得します。
keywords: machine, status, subcommand
title: docker-machine status
hide_from_sitemap: true
---

<ul class="nav nav-tabs">
  <li class="active"><a data-toggle="tab" href="#origin">英語表記</a></li>
  <li><a data-toggle="tab" href="#japanese">日本語訳</a></li>
</ul>
<div class="tab-content">
  <div id="origin" class="tab-pane fade in active">
{% capture original-content %}
```none
Usage: docker-machine status [arg...]

Get the status of a machine

Description:
   Argument is a machine name.
```
{% endcapture %}
{{ original-content | markdownify }}
</div>
<div id="japanese" class="tab-pane fade" markdown="1">
{% capture japanese-content %}
```none
利用方法: docker-machine status [arg...]

マシン状態を取得します。

内容説明:
   引数にはマシン名を必要な数だけ指定します。
```
{% endcapture %}
{{ japanese-content | markdownify }}
</div>
</div>

{% comment %}
For example:
{% endcomment %}
たとえば以下のとおりです。

```bash
$ docker-machine status dev

Running
```
