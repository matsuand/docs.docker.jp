{% capture tabChar %}	{% endcapture %}<!-- Make sure atom is using hard tabs -->
{% assign controller_data = site.data[include.datafolder][include.datafile] %}
{% assign parentPath = page.path | prepend: "/" | remove: page.name %}

{% comment %}
## Description
{% endcomment %}
{: #description }
## 説明

{{ controller_data.short | replace_relative_links: page.path }}

{% if controller_data.min_api_version %}

{% comment %}
<a href="/engine/api/v{{ controller_data.min_api_version }}/" target="_blank" class="_"><span class="badge badge-info" data-toggle="tooltip" data-placement="right" title="Open the {{ controller_data.min_api_version }} API reference (in a new window)">API {{ controller_data.min_api_version }}+</span></a>&nbsp;
The client and daemon API must both be at least
<a href="/engine/api/v{{ site.data[include.datafolder][include.datafile].min_api_version }}/" target="_blank" class="_">{{ site.data[include.datafolder][include.datafile].min_api_version }}</a>
to use this command. Use the `docker version` command on the client to check
your client and daemon API versions.
{% endcomment %}
<a href="/engine/api/v{{ site.data[include.datafolder][include.datafile].min_api_version }}/" target="_blank" class="_"><span class="badge badge-info" data-toggle="tooltip" data-placement="right" title="Open the {{ site.data[include.datafolder][include.datafile].min_api_version }} API reference (in a new window)">API {{ site.data[include.datafolder][include.datafile].min_api_version }} 以上</span></a>&nbsp;
このコマンドを利用するには、クライアントとデーモンの API はともに、最低でも
<a href="/engine/api/v{{ controller_data.min_api_version }}/" target="_blank" class="_">{{ controller_data.min_api_version }}</a>
である必要があります。
クライアント上において `docker version` コマンドを実行して、クライアントとデーモンの API バージョンを確認してください。

{% endif %}

{% if controller_data.deprecated %}

> This command is [deprecated](/engine/deprecated/){: target="_blank" class="_"}.
>
> It may be removed in a future Docker version.
{: .warning }

{% endif %}

{% if page.enterprise_only == true %}

> This command is only available on Docker Enterprise Edition.
>
> Learn more about [Docker Enterprise products](/ee/supported-platforms/){: target="_blank" class="_"}.
{: .important }

{% endif %}

{% if controller_data.experimental %}

> This command is experimental.
>
> This command is experimental on the Docker daemon. It should not be used in
> production environments.
> To enable experimental features on the Docker daemon, edit the
> [daemon.json](/engine/reference/commandline/dockerd/#daemon-configuration-file)
> and set `experimental` to `true`.
>
> {% include experimental.md %}

{% endif %}

{% if controller_data.experimentalcli %}

> This command is experimental on the Docker client.
>
> **It should not be used in production environments.**
>
> To enable experimental features in the Docker CLI, edit the
> [config.json](/engine/reference/commandline/cli/#configuration-files)
> and set `experimental` to `enabled`. You can go [here](https://docs.docker.com/engine/reference/commandline/cli/#experimental-features)
> for more information.
{: .important }

{% endif %}

{% capture command-orchestrator %}
{% if controller_data.swarm %}

<span class="badge badge-info" data-toggle="tooltip" data-placement="right" title="This command works with the Swarm orchestrator.">Swarm</span> This command works with the Swarm orchestrator.

{% endif %}
{% if controller_data.kubernetes %}

<span class="badge badge-info" data-toggle="tooltip" data-placement="right" title="This command works with the Kubernetes orchestrator.">Kubernetes</span> This command works with the Kubernetes orchestrator.

{% endif %}
{% endcapture %}{{ command-orchestrator }}


{% if controller_data.usage %}

{% comment %}
## Usage
{% endcomment %}
{: #usage }
## 利用方法

```console
{{ controller_data.usage | replace: tabChar, "" | strip }}{% if controller_data.cname %} COMMAND{% endif %}
```

{% endif %}
{% unless controller_data.long == controller_data.short %}

{% comment %}
## Extended description
{% endcomment %}
{: #extended-description }
## 追加説明

{{ controller_data.long | replace_relative_links: page.path }}

{% endunless %}

{% if controller_data.examples %}
{% comment %}
For example uses of this command, refer to the [examples section](#examples) below.
{% endcomment %}
本コマンドの利用例については、以下に示す [利用例の節](#examples) を参照してください。
{% endif %}

{% if controller_data.options %}
  {% if controller_data.inherited_options %}
    {% assign alloptions = controller_data.options | concat:controller_data.inherited_options %}
  {% else %}
    {% assign alloptions = controller_data.options %}
  {% endif %}
{% comment %}
## Options
{% endcomment %}
{: #options }
## オプション

<table>
<thead>
  <tr>
    <td>名前／省略形</td>
    <td>デフォルト</td>
    <td>説明</td>
  </tr>
</thead>
<tbody>
{% for option in alloptions %}
  {% capture deprecated-badge %}{% if option.deprecated %}<a href="/engine/deprecated/" target="_blank" class="_"><span class="badge badge-danger" data-toggle="tooltip" title="Read the deprecation reference (in a new window).">deprecated</span></a>{% endif %}{% endcapture %}
  {% capture experimental-daemon-badge %}{% if option.experimental %}<a href="/engine/reference/commandline/dockerd/#daemon-configuration-file" target="_blank" class="_"><span class="badge badge-warning" data-toggle="tooltip" title="Read about experimental daemon options (in a new window).">experimental (daemon)</span></a>{% endif %}{% endcapture %}
  {% capture experimental-cli-badge %}{% if option.experimentalcli %}<a href="/engine/reference/commandline/cli/#configuration-files" target="_blank" class="_"><span class="badge badge-warning"  data-toggle="tooltip" title="Read about experimental CLI options (in a new window).">experimental (CLI)</span></a>{% endif %}{% endcapture %}
  {% capture min-api %}{% if option.min_api_version %}<a href="/engine/api/v{{ option.min_api_version }}/" target="_blank" class="_"><span class="badge badge-info" data-toggle="tooltip" ttitle="Open the {{ controller_data.min_api_version }} API reference (in a new window)">API {{ option.min_api_version }}+</span></a>{% endif %}{%endcapture%}
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

{% if controller_data.examples %}

{% comment %}
## Examples
{% endcomment %}
{: #examples }
## 利用例

{{ controller_data.examples | replace_relative_links: page.path }}

{% endif %}

{% if controller_data.pname %}
{% unless controller_data.pname == include.datafile %}

{% comment %}
## Parent command
{% endcomment %}
{: #parent-command }
## 上位コマンド

{% capture parentfile %}{{ controller_data.plink | remove_first: ".yaml" | remove_first: "docker_" }}{% endcapture %}
{% capture parentdatafile %}{{ controller_data.plink | remove_first: ".yaml" }}{% endcapture %}
{% capture parentDesc %}{{ site.data[include.datafolder][parentdatafile].short }}{% endcapture %}

| コマンド | 説明        |
| -------- | ----------- |
| [{{ controller_data.pname }}]({{ site.baseurl }}{{parentPath}}{{ parentfile }}/) | {{ parentDesc }}|

{% endunless %}
{% endif %}

{% if controller_data.cname %}

{% comment %}
## Child commands
{% endcomment %}
{: #child-commands }
## 下位コマンド

<table>
<thead>
  <tr>
    <td>コマンド</td>
    <td>説明</td>
  </tr>
</thead>
<tbody>
{% for command in controller_data.cname %}
  {% capture dataFileName %}{{ command | strip | replace: " ", "_" }}{% endcapture %}
  <tr>
    <td markdown="span">[{{ command }}]({{ site.baseurl }}{{ parentPath }}{{ dataFileName | remove_first: "docker_" }}/)</td>
    <td markdown="span">{{ site.data[include.datafolder][dataFileName].short }}</td>
  </tr>
{% endfor %}
</tbody>
</table>
{% endif %}

{% unless controller_data.pname == "docker" or controller_data.pname == "dockerd" or include.datafile=="docker" %}

{% comment %}
## Related commands
{% endcomment %}
{: #related-commands }
## 関連コマンド

<table>
<thead>
  <tr>
    <td>コマンド</td>
    <td>説明</td>
  </tr>
</thead>
<tbody>
{% for command in site.data[include.datafolder][parentdatafile].cname %}
  {% capture dataFileName %}{{ command | strip | replace: " ", "_" }}{% endcapture %}
  <tr>
    <td markdown="span">[{{ command }}]({{ site.baseurl }}{{ parentPath }}{{ dataFileName | remove_first: "docker_" }}/)</td>
    <td markdown="span">{{ site.data[include.datafolder][dataFileName].short }}</td>
  </tr>
{% endfor %}
</tbody>
</table>

{% endunless %}
