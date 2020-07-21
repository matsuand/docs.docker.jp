---
description: docker-compose down
keywords: fig, composition, compose, docker, orchestration, cli, down
title: docker-compose down
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
Usage: down [options]

Options:
    --rmi type              Remove images. Type must be one of:
                              'all': Remove all images used by any service.
                              'local': Remove only images that don't have a
                              custom tag set by the `image` field.
    -v, --volumes           Remove named volumes declared in the `volumes`
                            section of the Compose file and anonymous volumes
                            attached to containers.
    --remove-orphans        Remove containers for services not defined in the
                            Compose file
    -t, --timeout TIMEOUT   Specify a shutdown timeout in seconds.
                            (default: 10)
```
{% endcapture %}
{{ original-content | markdownify }}
</div>
<div id="japanese" class="tab-pane fade" markdown="1">
{% capture japanese-content %}
```
利用方法: down [オプション]

オプション:
    --rmi type              イメージを削除します。type は以下のいずれか。
                               'all': 全サービスに用いられているイメージすべてを削除。
                               'local': `image` フィールドにカスタムタグが設定されて
                               いないイメージのみ削除。
    -v, --volumes           Composeファイルの`volumes`セクションにて宣言された名前
                            つきボリュームや、コンテナーに結びついている匿名ボリューム
                            を削除します。
    --remove-orphans        Compose ファイルにて定義されていないサービスのコンテナー
                            を削除します。
    -t, --timeout TIMEOUT   シャットダウンするまでの時間を秒単位で設定します。
                            （デフォルト: 10）
```
{% endcapture %}
{{ japanese-content | markdownify }}
</div>
<hr>
</div>

{% comment %}
Stops containers and removes containers, networks, volumes, and images
created by `up`.
{% endcomment %}
コンテナーを停止します。
そして `up` コマンドによって生成されたコンテナー、ネットワーク、ボリューム、イメージを削除します。

{% comment %}
By default, the only things removed are:
{% endcomment %}
デフォルトで、削除されるものは以下に限ります。

{% comment %}
- Containers for services defined in the Compose file
- Networks defined in the `networks` section of the Compose file
- The default network, if one is used
{% endcomment %}
- Compose ファイルにおいて定義されているサービスのコンテナー。
- Compose ファイルの `networks` セクションに定義されているネットワーク。
- デフォルトネットワーク。ただし利用されている場合に限る。

{% comment %}
Networks and volumes defined as `external` are never removed.
{% endcomment %}
`external` として定義されているネットワークやボリュームは削除されません。
