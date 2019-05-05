---
description: Compose CLI リファレンス
keywords: fig, composition, compose, docker, orchestration, cli, reference
title: コマンドライン補完
---

<!--
Compose comes with [command completion](http://en.wikipedia.org/wiki/Command-line_completion)
for the bash and zsh shell.
-->
Compose には bash 用あるいは zsh 用の[コマンド補完](http://en.wikipedia.org/wiki/Command-line_completion)（command completion）が用意されています。

<!--
## Install command completion
-->
## コマンド補完のインストール
{: #install-command-completion }

### Bash

<!--
Make sure bash completion is installed.
-->
bash 補完がインストールされていることを確認します。

#### Linux

<!--
1. On a current Linux OS (in a non-minimal installation), bash completion should be
available.
-->
1. 利用している Linux OSであれば（最小インストールではない）、bash 補完が利用できるはずです。

    <!--
    //2. Place the completion script in `/etc/bash_completion.d/`.
    -->
2.  補完スクリプトを `/etc/bash_completion.d/` に置きます。

    ```shell
    sudo curl -L https://raw.githubusercontent.com/docker/compose/{{site.compose_version}}/contrib/completion/bash/docker-compose -o /etc/bash_completion.d/docker-compose
    ```

### Mac

<!--
##### Install via Homebrew
-->
##### Homebrew によるインストール
{: #install-via-homebrew }

<!--
1. Install with `brew install bash-completion`.
2. After the installation, Brew displays the installation path. Make sure to place the completion script in the path.
-->
1. `brew install bash-completion` によりインストールします。
2. インストール後に Brew がインストール先のパスを表示します。
   補完スクリプトをそのパスに置きます。

    <!--
    For example, when running this command on Mac 10.13.2, place the completion script in `/usr/local/etc/bash_completion.d/`.
    -->
    たとえば Mac 10.13.2 を利用している場合、補完スクリプトを `/usr/local/etc/bash_completion.d/` に置きます。

    ```shell
    sudo curl -L https://raw.githubusercontent.com/docker/compose/{{site.compose_version}}/contrib/completion/bash/docker-compose -o /usr/local/etc/bash_completion.d/docker-compose
    ```

    <!--
    //3. Add the following to your `~/.bash_profile`:
    -->
3. 以下を `~/.bash_profile` に追加します。

    ```shell
    if [ -f $(brew --prefix)/etc/bash_completion ]; then
    . $(brew --prefix)/etc/bash_completion
    fi
    ```

    <!--
    //4. You can source your `~/.bash_profile` or launch a new terminal to utilize
    completion.
    -->
4. 補完を利用するため `~/.bash_profile` を source で取り込むか、あるいは新たな端末画面を起動します。

<!--
##### Install via MacPorts
-->
##### MacPorts によるインストール
{: #install-via-macports }

<!--
1. Run `sudo port install bash-completion` to install bash completion.
-->
1. `sudo port install bash-completion` を実行して bash 補完をインストールします。

    <!--
    //2. Add the following lines to `~/.bash_profile`:
    -->
2. 以下を `~/.bash_profile` に追加します。

    ```shell
    if [ -f /opt/local/etc/profile.d/bash_completion.sh ]; then
    . /opt/local/etc/profile.d/bash_completion.sh
    fi
    ```

    <!--
    //3. You can source your `~/.bash_profile` or launch a new terminal to utilize
    completion.
    -->
3. 補完を利用するため `~/.bash_profile` を source で取り込むか、あるいは新たな端末画面を起動します。

### Zsh

<!--
Make sure you have [installed `oh-my-zsh`](https://ohmyz.sh/) on your computer.
-->
[`oh-my-zsh`](https://ohmyz.sh/) がインストールされているかどうかを確認します。

<!--
#### With oh-my-zsh shell
-->
#### oh-my-zsh シェルがある場合
{: #with-oh-my-zsh-shell }

<!--
Add `docker` and `docker-compose` to the plugins list in `~/.zshrc` to run autocompletion within the oh-my-zsh shell. In the following example, `...` represent other Zsh plugins you may have installed.
-->
`~/.zshrc` 内のプラグインリストに `docker` と `docker-compose` を加えます。
これにより oh-my-zsh シェル内での自動補完機能を有効にします。
以下の例において `...` の部分は、以前からインストールされている Zsh プラグインを表わします。

```shell
plugins=(... docker docker-compose
)
 ```

<!--
#### Without oh-my-zsh shell
-->
#### oh-my-zsh シェルがない場合
{: #without-oh-my-zsh-shell }

<!--
1. Place the completion script in your `/path/to/zsh/completion` (typically `~/.zsh/completion/`):
-->
1.  `/path/to/zsh/completion`（通常は `~/.zsh/completion/`）に補完スクリプトを置きます。

    ```shell
    $ mkdir -p ~/.zsh/completion
    $ curl -L https://raw.githubusercontent.com/docker/compose/{{site.compose_version}}/contrib/completion/zsh/_docker-compose > ~/.zsh/completion/_docker-compose
    ```

    <!--
    //2. Include the directory in your `$fpath` by adding in `~/.zshrc`:
    -->
2. `~/.zshrc` 内にて `$fpath` にディレクトリを追加します。

    ```shell
    fpath=(~/.zsh/completion $fpath)
    ```

    <!--
    //3. Make sure `compinit` is loaded or do it by adding in `~/.zshrc`:
    -->
3. `compinit` がロードされているか、あるいは `~/.zshrc` 内への追加によりそれが行われるかを確認します。

    ```shell
    autoload -Uz compinit && compinit -i
    ```

    <!--
    //4. Then reload your shell:
    -->
4. その後にシェルをリロードします。

    ```shell
    exec $SHELL -l
    ```

<!--
## Available completions
-->
## 利用できる補完機能
{: #available-completions }

<!--
Depending on what you typed on the command line so far, it completes:
-->
コマンドラインにどこまで入力しているかによって、補完される内容はさまざまです。

<!--
 - available docker-compose commands
 - options that are available for a particular command
 - service names that make sense in a given context, such as services with running or stopped instances or services based on images vs. services based on Dockerfiles. For `docker-compose scale`, completed service names automatically have "=" appended.
 - arguments for selected options. For example, `docker-compose kill -s` completes some signals like SIGHUP and SIGUSR1.
-->
 - 利用可能な docker-compose コマンド。
 - 特定のコマンドにて利用可能なオプション。
 - 指定されているコンテキストにおいて用いられるサービス名。
   たとえば実行中サービス、停止中のインスタンス、イメージに基づくサービス、Dockerfile に基づくサービスなどです。
   `docker-compose scale` では、サービス名が補完されると自動的に "=" が追加されます。
 - 指定されたオプションへの引数。
   たとえば `docker-compose kill -s` に対して、シグナル名、たとえば SIGHUP や SIGUSR1 が補完されます。

<!--
Enjoy working with Compose faster and with fewer typos!
-->
Compose での作業を、入力は速くミスは少なく！

<!--
## Compose documentation
-->
## Compose ドキュメント
{: #compose-documentation }

<!--
- [User guide](index.md)
- [Installing Compose](install.md)
- [Get started with Django](django.md)
- [Get started with Rails](rails.md)
- [Get started with WordPress](wordpress.md)
- [Command line reference](./reference/index.md)
- [Compose file reference](compose-file.md)
-->
- [ユーザーガイド](index.md)
- [Compose のインストール](install.md)
- [Django とともにはじめよう](django.md)
- [Rails とともにはじめよう](rails.md)
- [WordPress とともにはじめよう](wordpress.md)
- [コマンドラインリファレンス](./reference/index.md)
- [Compose ファイルリファレンス](compose-file.md)
