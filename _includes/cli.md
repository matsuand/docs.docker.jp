{% capture tabChar %}	{% endcapture %}<!-- Make sure atom is using hard tabs -->
{% capture dockerBaseDesc %}Docker CLI のベースとなるコマンド。{% endcapture %}
{% if include.datafolder and include.datafile %}

{% comment %}
## Description
{% endcomment %}
{: #description }
## 説明

{% if include.datafile=="docker" %}<!-- docker.yaml is textless, so override -->
{{ dockerBaseDesc }}
{% else %}
{{ site.data[include.datafolder][include.datafile].short }}
{% endif %}

{% if site.data[include.datafolder][include.datafile].min_api_version %}

{% comment %}
<a href="/engine/api/v{{ site.data[include.datafolder][include.datafile].min_api_version }}/" target="_blank" class="_"><span class="badge badge-info" data-toggle="tooltip" data-placement="right" title="Open the {{ site.data[include.datafolder][include.datafile].min_api_version }} API reference (in a new window)">API {{ site.data[include.datafolder][include.datafile].min_api_version }}+</span></a>&nbsp;
The client and daemon API must both be at least
<a href="/engine/api/v{{ site.data[include.datafolder][include.datafile].min_api_version }}/" target="_blank" class="_">{{ site.data[include.datafolder][include.datafile].min_api_version }}</a>
to use this command. Use the `docker version` command on the client to check
your client and daemon API versions.
{% endcomment %}
<a href="/engine/api/v{{ site.data[include.datafolder][include.datafile].min_api_version }}/" target="_blank" class="_"><span class="badge badge-info" data-toggle="tooltip" data-placement="right" title="Open the {{ site.data[include.datafolder][include.datafile].min_api_version }} API reference (in a new window)">API {{ site.data[include.datafolder][include.datafile].min_api_version }} 以上</span></a>&nbsp;
このコマンドを利用するには、クライアントとデーモンの API はともに、最低でも
<a href="/engine/api/v{{ site.data[include.datafolder][include.datafile].min_api_version }}/" target="_blank" class="_">{{ site.data[include.datafolder][include.datafile].min_api_version }}</a>
である必要があります。
クライアント上において `docker version` コマンドを実行して、クライアントとデーモンの API バージョンを確認してください。

{% endif %}

{% if site.data[include.datafolder][include.datafile].deprecated %}

> This command is [deprecated](/engine/deprecated.md){: target="_blank" class="_"}.
>
> It may be removed in a future Docker version.
{: .warning }

{% endif %}

{% if site.data[include.datafolder][include.datafile].experimental %}

> This command is experimental.
>
> This command is experimental on the Docker daemon. It should not be used in
> production environments.
> To enable experimental features on the Docker daemon, edit the
> [daemon.json](/engine/reference/commandline/dockerd.md#daemon-configuration-file)
> and set `experimental` to `true`.
>
> {% include experimental.md %}

{% endif %}

{% if site.data[include.datafolder][include.datafile].experimentalcli %}

> This command is experimental on the Docker client.
>
> **It should not be used in production environments.**
>
> To enable experimental features in the Docker CLI, edit the
> [config.json](/engine/reference/commandline/cli.md#configuration-files)
> and set `experimental` to `enabled`. You can go [here](https://github.com/docker/docker.github.io/blob/master/_includes/experimental.md)
> for more information.
{: .important }

{% endif %}

{% capture command-orchestrator %}
{% if site.data[include.datafolder][include.datafile].swarm %}

<span class="badge badge-info" data-toggle="tooltip" data-placement="right" title="This command works with the Swarm orchestrator.">Swarm</span> This command works with the Swarm orchestrator.

{% endif %}
{% if site.data[include.datafolder][include.datafile].kubernetes %}

<span class="badge badge-info" data-toggle="tooltip" data-placement="right" title="This command works with the Kubernetes orchestrator.">Kubernetes</span> This command works with the Kubernetes orchestrator.

{% endif %}
{% endcapture %}{{ command-orchestrator }}


{% if site.data[include.datafolder][include.datafile].usage %}

{% comment %}
## Usage
{% endcomment %}
{: #usage }
## 利用方法

```none
{{ site.data[include.datafolder][include.datafile].usage | replace: tabChar,"" | strip }}{% if site.data[include.datafolder][include.datafile].cname %} COMMAND{% endif %}
```

{% endif %}
{% if site.data[include.datafolder][include.datafile].options %}
  {% if site.data[include.datafolder][include.datafile].inherited_options %}
    {% assign alloptions = site.data[include.datafolder][include.datafile].options | concat:site.data[include.datafolder][include.datafile].inherited_options %}
  {% else %}
    {% assign alloptions = site.data[include.datafolder][include.datafile].options %}
  {% endif %}
{% comment %}
## Options
{% endcomment %}
{: #options }
## オプション

<table>
<thead>
  <tr>
    <!--
    <td>Name, shorthand</td>
    <td>Default</td>
    <td>Description</td>
    -->
    <td>名前／省略形</td>
    <td>デフォルト</td>
    <td>説明</td>
  </tr>
</thead>
<tbody>
{% for option in alloptions %}

  {% capture deprecated-badge %}{% if option.deprecated %}<a href="/engine/deprecated.md" target="_blank" class="_"><span class="badge badge-danger" data-toggle="tooltip" title="Read the deprecation reference (in a new window).">deprecated</span></a>{% endif %}{% endcapture %}
  {% capture experimental-daemon-badge %}{% if option.experimental %}<a href="/engine/reference/commandline/dockerd.md#daemon-configuration-file" target="_blank" class="_"><span class="badge badge-warning" data-toggle="tooltip" title="Read about experimental daemon options (in a new window).">experimental (daemon)</span></a>{% endif %}{% endcapture %}
  {% capture experimental-cli-badge %}{% if option.experimentalcli %}<a href="/engine/reference/commandline/cli.md#configuration-files" target="_blank" class="_"><span class="badge badge-warning"  data-toggle="tooltip" title="Read about experimental CLI options (in a new window).">experimental (CLI)</span></a>{% endif %}{% endcapture %}
  {% capture min-api %}{% if option.min_api_version %}<a href="/engine/api/v{{ option.min_api_version }}/" target="_blank" class="_"><span class="badge badge-info" data-toggle="tooltip" ttitle="Open the {{ site.data[include.datafolder][include.datafile].min_api_version }} API reference (in a new window)">API {{ option.min_api_version }} 以上</span></a>{% endif %}{%endcapture%}
  {% capture flag-orchestrator %}{% if option.swarm %}<span class="badge badge-info" data-toggle="tooltip" title="This option works for the Swarm orchestrator.">Swarm</span>{% endif %}{% if option.kubernetes %}<span class="badge badge-info" data-toggle="tooltip" title="This option works for the Kubernetes orchestrator.">Kubernetes</span>{% endif %}{% endcapture %}

  {% capture all-badges %}{{ deprecated-badge }}{{ experimental-daemon-badge }}{{ experimental-cli-badge }}{{ min-api }}{{ flag-orchestrator }}{% endcapture %}

  {% assign defaults-to-skip = "[],map[],false,0,0s,default,'',\"\"" | split: ',' %}
  {% capture option-default %}{% if option.default_value %}{% unless defaults-to-skip contains option.default_value or defaults-to-skip == blank %}`{{ option.default_value }}`{% endunless %}{% endif %}{% endcapture %}
  <tr>
    <td markdown="span">`--{{ option.option }}{% if option.shorthand %} , -{{ option.shorthand }}{% endif %}`</td>
    <td markdown="span">{{ option-default }}</td>
    <td markdown="span">{% if all-badges != '' %}{{ all-badges | strip }}<br />{% endif %}{{ option.description | strip }}</td>
  </tr>

{% endfor %} <!-- end for option -->
</tbody>
</table>

{% endif %} <!-- end if options -->

{% if site.data[include.datafolder][include.datafile].cname %}

{% comment %}
## Child commands
{% endcomment %}
{: #child-commands }
## 下位コマンド

<table>
<thead>
  <tr>
    <!--
    <td>Command</td>
    <td>Description</td>
    -->
    <td>コマンド</td>
    <td>説明</td>
  </tr>
</thead>
<tbody>
{% for command in site.data[include.datafolder][include.datafile].cname %}
  {% capture dataFileName %}{{ command | strip | replace: " ","_" }}{% endcapture %}
  <tr>
    <td markdown="span">[{{ command }}]({{ dataFileName | replace: "docker_","" }}/)</td>
    <td markdown="span">{{ site.data[include.datafolder][dataFileName].short }}</td>
  </tr>
{% endfor %}
</tbody>
</table>
{% endif %}

{% if site.data[include.datafolder][include.datafile].pname %}
{% unless site.data[include.datafolder][include.datafile].pname == include.datafile %}

{% comment %}
## Parent command
{% endcomment %}
{: #parent-command }
## 上位コマンド

{% capture parentfile %}{{ site.data[include.datafolder][include.datafile].plink | replace: ".yaml", "" | replace: "docker_","" }}{% endcapture %}
{% capture parentdatafile %}{{ site.data[include.datafolder][include.datafile].plink | replace: ".yaml", "" }}{% endcapture %}

{% if site.data[include.datafolder][include.datafile].pname == "docker" %}
  {% capture parentDesc %}{{ dockerBaseDesc }}{% endcapture %}
{% else %}
  {% capture parentDesc %}{{ site.data[include.datafolder][parentdatafile].short }}{% endcapture %}
{% endif %}

{% comment %}
| Command | Description |
| ------- | ----------- |
| [{{ site.data[include.datafolder][include.datafile].pname }}]({{ parentfile }}) | {{ parentDesc }}|
{% endcomment %}
| コマンド | 説明        |
| -------- | ----------- |
| [{{ site.data[include.datafolder][include.datafile].pname }}]({{ parentfile }}) | {{ parentDesc }}|

{% endunless %}
{% endif %}

{% unless site.data[include.datafolder][include.datafile].pname == "docker" or site.data[include.datafolder][include.datafile].pname == "dockerd" or include.datafile=="docker" %}

{% comment %}
## Related commands
{% endcomment %}
{: #related-commands }
## 関連コマンド

<table>
<thead>
  <tr>
    <!--
    <td>Command</td>
    <td>Description</td>
    -->
    <td>コマンド</td>
    <td>説明</td>
  </tr>
</thead>
<tbody>
{% for command in site.data[include.datafolder][parentdatafile].cname %}
  {% capture dataFileName %}{{ command | strip | replace: " ","_" }}{% endcapture %}
  <tr>
    <td markdown="span">[{{ command }}]({{ dataFileName | replace: "docker_","" }}/)</td>
    <td markdown="span">{{ site.data[include.datafolder][dataFileName].short }}</td>
  </tr>
{% endfor %}
</tbody>
</table>

{% endunless %}

{% unless site.data[include.datafolder][include.datafile].long == site.data[include.datafolder][include.datafile].short %}

{% comment %}
## Extended description
{% endcomment %}
{: #extended-description }
## 追加説明

{{ site.data[include.datafolder][include.datafile].long }}

{% endunless %}

{% if site.data[include.datafolder][include.datafile].examples %}

{% comment %}
## Examples
{% endcomment %}
{: #examples }
## 利用例

{{ site.data[include.datafolder][include.datafile].examples }}

{% endif %}
{% else %}

The include.datafolder or include.datafile was not set.

{% endif %}
