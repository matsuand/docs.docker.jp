---
description: Docker Machine にコマンドライン補完をインストールします。
keywords: machine, docker, orchestration, cli, reference
title: コマンドライン補完
---

{% comment %}
Docker Machine comes with [command completion](http://en.wikipedia.org/wiki/Command-line_completion)
for the bash and zsh shell.
{% endcomment %}
Docker Machine には bash と zsh 向けの [コマンド補完](http://en.wikipedia.org/wiki/Command-line_completion) が用意されています。

{% comment %}
## Installing Command Completion
{% endcomment %}
{: #installing-command-completion }
## コマンド補完のインストール

### Bash

{% comment %}
Make sure bash completion is installed. If you are using a current version of
Linux in a non-minimal installation, bash completion should be available.
{% endcomment %}
bash 補完がインストールされているかどうかを確認します。
最新の Linux をインストールしていて、それが最小版でない限りは、bash 補完が利用可能なはずです。

{% comment %}
On a Mac, install with `brew install bash-completion`.
{% endcomment %}
Mac 上では `brew install bash-completion` によりインストールします。

{% comment %}
Place the completion script in `/etc/bash_completion.d/` as follows:
{% endcomment %}
補完スクリプトを、以下のようにして `/etc/bash_completion.d/` を移動させます。

{% comment %}
*   On a Mac:
{% endcomment %}
*   Mac 上では以下のようにします。

    ```shell
    sudo curl -L https://raw.githubusercontent.com/docker/machine/v{{site.machine_version}}/contrib/completion/bash/docker-machine.bash -o `brew --prefix`/etc/bash_completion.d/docker-machine
    ```

{% comment %}
*   On a standard Linux installation:
{% endcomment %}
*   標準的な Linux ディストリビューションでは以下のようにします。

    ```shell
    sudo curl -L https://raw.githubusercontent.com/docker/machine/v{{site.machine_version}}/contrib/completion/bash/docker-machine.bash -o /etc/bash_completion.d/docker-machine
    ```

{% comment %}
Completion is available upon next login.
{% endcomment %}
コマンド補完は、次のログイン以降に有効となります。


### Zsh

{% comment %}
Place the completion script in your a `completion` file within the ZSH
configuration directory, such as `~/.zsh/completion/`.
{% endcomment %}
ZSH 設定ディレクトリ内の `completion` ファイルに補完スクリプトを置きます。
たとえば `~/.zsh/completion/` などです。

```shell
mkdir -p ~/.zsh/completion
curl -L https://raw.githubusercontent.com/docker/machine/v{{site.machine_version}}/contrib/completion/zsh/_docker-machine > ~/.zsh/completion/_docker-machine
```

{% comment %}
Include the directory in your `$fpath`, by adding a like the following to the
`~/.zshrc` configuration file.
{% endcomment %}
`$fpath` にそのディレクトリを含めます。
たとえば以下の記述を `~/.zshrc` 設定ファイルに追加します。

```shell
fpath=(~/.zsh/completion $fpath)
```

{% comment %}
Make sure `compinit` is loaded or do it by adding in `~/.zshrc`:
{% endcomment %}
`compinit` をロードしているか、あるいは `~/.zshrc` 内にて実行していることを確認します。

```shell
autoload -Uz compinit && compinit -i
```

{% comment %}
Then reload your shell:
{% endcomment %}
そしてシェルをリロードします。

```shell
exec $SHELL -l
```

{% comment %}
## Available completions
{% endcomment %}
{: #available-completions }
## 補完入力でできること

{% comment %}
Depending on what you typed on the command line so far, it completes:
{% endcomment %}
コマンドラインにどこまで入力したかによって、以下のような補完入力が可能です。

{% comment %}
- commands and their options
- container IDs and names
- image repositories and image tags
- file paths
{% endcomment %}
- コマンドとそのオプション。
- コンテナー ID とコンテナー名。
- イメージリポジトリとイメージタグ。
- ファイルパス。

{% comment %}
## Where to go next
{% endcomment %}
{: #where-to-go-next }
## 次に読むものは

{% comment %}
* [Get started with a local VM](get-started.md)
* [Machine command-line reference](reference/index.md)
* [Machine drivers](drivers/index.md)
* [Machine concepts and help](concepts.md)
{% endcomment %}
* [Docker Machine をローカル VM ではじめよう](get-started.md)
* [Machine コマンドラインリファレンス](reference/index.md)
* [Machine ドライバー](drivers/index.md)
* [Machine の考え方とヘルプ](concepts.md)
