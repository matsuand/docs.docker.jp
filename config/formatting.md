---
description: CLI and log output formatting reference
keywords: format, formatting, output, templates, log
title: コマンド出力やログ出力のフォーマット変更
redirect_from:
- /engine/admin/formatting/
---

{% comment %}
Docker uses [Go templates](https://golang.org/pkg/text/template/) which you can
use to manipulate the output format of certain commands and log drivers.
{% endcomment %}
Docker は [Go 言語の templates パッケージ](https://golang.org/pkg/text/template/) を利用して、所定のコマンドやログドライバーの出力フォーマットを操作できるようにしています。

{% comment %}
Docker provides a set of basic functions to manipulate template elements.
All of these examples use the `docker inspect` command, but many other CLI
commands have a `--format` flag, and many of the CLI command references
include examples of customizing the output format.
{% endcomment %}
Docker では、template 項目を操作する基本機能を提供します。
以下では、すべて `docker inspect` コマンドを使った例を示します。
ただしこれ以外の CLI コマンドにも `--format` フラグは用意されています。
また多くの CLI コマンドリファレンスにて、出力フォーマットをカスタマイズする例を示しています。

## join

{% comment %}
`join` concatenates a list of strings to create a single string.
It puts a separator between each element in the list.
{% endcomment %}
`join` は文字列のリストを連結して、１つの文字列を生成します。
リスト内では、各項目はセパレーターで区切ります。

{% raw %}
```
docker inspect --format '{{join .Args " , "}}' container
```
{% endraw %}

## table

{% comment %}
`table` specifies which fields you want to see its output.
{% endcomment %}
`table` は出力したい項目を指定します。

{% raw %}
```
docker image list --format "table {{.ID}}\t{{.Repository}}\t{{.Tag}}\t{{.Size}}"
```
{% endraw %}

## json

{% comment %}
`json` encodes an element as a json string.
{% endcomment %}
`json` は項目を json 文字列としてエンコードします。


{% raw %}
```
docker inspect --format '{{json .Mounts}}' container
```
{% endraw %}

## lower

{% comment %}
`lower` transforms a string into its lowercase representation.
{% endcomment %}
`lower` は文字列を小文字化します。

{% raw %}
```
docker inspect --format "{{lower .Name}}" container
```
{% endraw %}

## split

{% comment %}
`split` slices a string into a list of strings separated by a separator.
{% endcomment %}
`split` は、文字列を分割して、セパレーターによって区切られた文字列リストにします。

{% raw %}
```
docker inspect --format '{{split .Image ":"}}'
```
{% endraw %}

## title

{% comment %}
`title` capitalizes the first character of a string.
{% endcomment %}
`title` は文字列の先頭文字を大文字にします。

{% raw %}
```
docker inspect --format "{{title .Name}}" container
```
{% endraw %}

## upper

{% comment %}
`upper` transforms a string into its uppercase representation.
{% endcomment %}
`upper` は文字列を大文字化します。

{% raw %}
```
docker inspect --format "{{upper .Name}}" container
```
{% endraw %}


## println

{% comment %}
`println` prints each value on a new line.
{% endcomment %}
`println` は各項目を一行ずつ出力します。

{% raw %}
```
docker inspect --format='{{range .NetworkSettings.Networks}}{{println .IPAddress}}{{end}}' container
```
{% endraw %}

{% comment %}
# Hint
{% endcomment %}
{: #hint }
# ヒント

{% comment %}
To find out what data can be printed, show all content as json:
{% endcomment %}
どの項目を出力できるかを知るには、以下を実行すれば全項目を json 形式で表示できます。

{% raw %}
```
docker container ls --format='{{json .}}'
```
{% endraw %}
