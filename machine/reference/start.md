---
description: マシンを起動します。
keywords: machine, start, subcommand
title: docker-machine start
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
Usage: docker-machine start [arg...]

Start a machine

Description:
   Argument(s) are one or more machine names.
```
{% endcapture %}
{{ original-content | markdownify }}
</div>
<div id="japanese" class="tab-pane fade" markdown="1">
{% capture japanese-content %}
```none
利用方法: docker-machine start [arg...]

マシンを起動します。

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
$ docker-machine start dev

Starting VM...
```
