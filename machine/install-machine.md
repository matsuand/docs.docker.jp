---
description: Docker Machine のインストール方法
keywords: machine, orchestration, install, installation, docker, documentation, uninstall Docker Machine, uninstall
title: Docker Machine のインストール
hide_from_sitemap: true
---

{% comment %}
Install Docker Machine binaries by following the instructions in the following section. You can find the latest
versions of the binaries on the [docker/machine release
page](https://github.com/docker/machine/releases/){: target="_blank" class="_" }
on GitHub.
{% endcomment %}
Docker Machine のバイナリをインストールしたい場合は、次の節で示す手順に従います。
GitHub 上の [docker/machine リリースページ](https://github.com/docker/machine/releases/){: target="_blank" class="_" }に、最新のバイナリバージョンがあります。

{% comment %}
## Install Docker Machine
{% endcomment %}
## Docker Machine のインストール
{: #install-docker-machine }

{% comment %}
1.  Install [Docker](/engine/installation/index.md){: target="_blank" class="_" }.
{% endcomment %}
1.  [Docker](/engine/installation/index.md){: target="_blank" class="_" } をインストールします。

{% comment %}
2.  Download the Docker Machine binary and extract it to your PATH.
{% endcomment %}
2.  Docker Machine のバイナリをダウンロードして実行パスに展開します。

    {% comment %}
    If you are running **macOS**:
    {% endcomment %}
    **macOS** を利用している場合:

    ```console
    $ base=https://github.com/docker/machine/releases/download/v{{site.machine_version}} &&
      curl -L $base/docker-machine-$(uname -s)-$(uname -m) >/usr/local/bin/docker-machine &&
      chmod +x /usr/local/bin/docker-machine
    ```

    {% comment %}
    If you are running **Linux**:
    {% endcomment %}
    **Linux** を利用している場合:

    ```console
    $ base=https://github.com/docker/machine/releases/download/v{{site.machine_version}} &&
      curl -L $base/docker-machine-$(uname -s)-$(uname -m) >/tmp/docker-machine &&
      sudo mv /tmp/docker-machine /usr/local/bin/docker-machine &&
      chmod +x /usr/local/bin/docker-machine
    ```

    {% comment %}
    If you are running **Windows** with [Git BASH](https://git-for-windows.github.io/){: target="_blank" class="_"}:
    {% endcomment %}
    **Windows** 上において [Git BASH](https://git-for-windows.github.io/){: target="_blank" class="_"} を利用している場合:

    ```console
    $ base=https://github.com/docker/machine/releases/download/v{{site.machine_version}} &&
      mkdir -p "$HOME/bin" &&
      curl -L $base/docker-machine-Windows-x86_64.exe > "$HOME/bin/docker-machine.exe" &&
      chmod +x "$HOME/bin/docker-machine.exe"
    ```

    {% comment %}
    > The above command works on Windows only if you use a
    terminal emulator such as [Git BASH](https://git-for-windows.github.io/){: target="_blank" class="_"}, which supports Linux commands like `chmod`.
    {: .important}
    {% endcomment %}
    > 上のコマンドは Windows 上において実行していますが、これができるのは [Git BASH](https://git-for-windows.github.io/){: target="_blank" class="_"} などを利用して、`chmod` といった Linux コマンドをサポートしている端末エミュレーターを使っているからです。
    {: .important}

    {% comment %}
    Otherwise, download one of the releases from the [docker/machine release
    page](https://github.com/docker/machine/releases/){: target="_blank" class="_" } directly.
    {% endcomment %}
    上記以外は、[docker/machine リリースページ](https://github.com/docker/machine/releases/){: target="_blank" class="_" } からバイナリリリースを直接ダウンロードしてください。

{% comment %}
3.  Check the installation by displaying the Machine version:
{% endcomment %}
3.  インストール後の確認として Machine のバージョンを表示してみます。

        $ docker-machine version
        docker-machine version {{site.machine_version}}, build 9371605

{% comment %}
## Install bash completion scripts
{% endcomment %}
## bash 補完スクリプトのインストール
{: #install-bash-completion-scripts }

{% comment %}
The Machine repository supplies several `bash` scripts that add features such
as:
{% endcomment %}
Machine リポジトリには便利な`bash`スクリプトがあり、以下のような機能を利用できます。

{% comment %}
-   command completion
-   a function that displays the active machine in your shell prompt
-   a function wrapper that adds a `docker-machine use` subcommand to switch the
    active machine
{% endcomment %}
-   コマンド補完
-   シェルプロンプト内にアクティブなマシンを表示する機能
-   アクティブマシンを切り替えるサブコマンド`docker-machine use`を実現するラッパー

{% comment %}
Confirm the version and save scripts to `/etc/bash_completion.d` or
`/usr/local/etc/bash_completion.d`:
{% endcomment %}
スクリプトのバージョンを確認し保存します。
保存先は`/etc/bash_completion.d`または`/usr/local/etc/bash_completion.d`とします。

```bash
base=https://raw.githubusercontent.com/docker/machine/v{{site.machine_version}}
for i in docker-machine-prompt.bash docker-machine-wrapper.bash docker-machine.bash
do
  sudo wget "$base/contrib/completion/bash/${i}" -P /etc/bash_completion.d
done
```

{% comment %}
Then you need to run `source
/etc/bash_completion.d/docker-machine-prompt.bash` in your bash
terminal to tell your setup where it can find the file
`docker-machine-prompt.bash` that you previously downloaded.
{% endcomment %}
bash 端末内で`source /etc/bash_completion.d/docker-machine-prompt.bash`を実行します。
これにより、ダウンロードした`docker-machine-prompt.bash`がどこにあるかを設定します。

{% comment %}
To enable the `docker-machine` shell prompt, add
`$(__docker_machine_ps1)` to your `PS1` setting in `~/.bashrc`.
{% endcomment %}
`docker-machine`のシェルプロンプトを有効にするために、`~/.bashrc`内の`PS1`を`$(__docker_machine_ps1)`とします。

```
PS1='[\u@\h \W$(__docker_machine_ps1)]\$ '
```

{% comment %}
You can find additional documentation in the comments at the [top of
each
script](https://github.com/docker/machine/tree/master/contrib/completion/bash){:
target="_blank" class="_"}.
{% endcomment %}
詳細な情報は[各スクリプトの上段](https://github.com/docker/machine/tree/master/contrib/completion/bash){:
target="_blank" class="_"}にコメントとして記述されています。

{% comment %}
## How to uninstall Docker Machine
{% endcomment %}
## Docker Machine のアンインストール
{: #how-to-uninstall-docker-machine }

{% comment %}
To uninstall Docker Machine:
{% endcomment %}
Docker Machine のアンインストールは以下のようにします。

{% comment %}
*  Optionally, remove the machines you created.
{% endcomment %}
*  必要に応じて生成済のマシンを削除します。

   {% comment %}
   To remove each machine individually: `docker-machine rm <machine-name>`
   {% endcomment %}
   各マシンを削除するには、それぞれに対して以下を実行します。`docker-machine rm <マシン名>`

   {% comment %}
   To remove all machines: `docker-machine rm -f $(docker-machine ls
   -q)` (you might need to use `-force` on Windows).
   {% endcomment %}
   マシンをすべて削除するには`docker-machine rm -f $(docker-machine ls -q)`を実行します。
   （Windows では`-force`オプションが必要になるかもしれません。）

   {% comment %}
   Removing machines is an optional step because there are cases where
   you might want to save and migrate existing machines to a
   [Docker for Mac](/docker-for-mac/index.md) or
   [Docker Desktop for Windows](/docker-for-windows/index.md) environment,
   for example.
   {% endcomment %}
   マシンを削除するのは任意の作業です。
   それはたとえば [Docker for Mac](/docker-for-mac/index.md) または [Docker Desktop for Windows](/docker-for-windows/index.md) 環境向けに、既存マシンを移行して利用したい場合もあるからです。

{% comment %}
*  Remove the executable: `rm $(which docker-machine)`
{% endcomment %}
*  実行モジュールを削除します。
   `rm $(which docker-machine)`


{% comment %}
>**Note**: As a point of information, the `config.json`, certificates,
and other data related to each virtual machine created by `docker-machine`
is stored in `~/.docker/machine/machines/` on Mac and Linux and in
`~\.docker\machine\machines\` on Windows. We recommend that you do not edit or
remove those files directly as this only affects information for the Docker
CLI, not the actual VMs, regardless of whether they are local or on remote
servers.
{% endcomment %}
>**メモ**: `config.json`、認証情報といった `docker-machine` により生成された各仮想環境に関連するデータは、Mac や Linux の場合 `~/.docker/machine/machines/`、Windows の場合 `~\.docker\machine\machines\` に保存されています。このようなデータは直接編集したり削除したりしないことをお勧めします。
これは Docker CLI に関する情報にのみ影響します。
VM に対しては、それがローカルでもリモートでも影響しません。

{% comment %}
## Where to go next
{% endcomment %}
## 次はどこへ
{: #where-to-go-next }

{% comment %}
-  [Docker Machine overview](overview.md)
-  Create and run a Docker host on your [local system using virtualization](get-started.md)
-  Provision multiple Docker hosts [on your cloud provider](get-started-cloud.md)
-  [Docker Machine driver reference](/machine/drivers/index.md)
-  [Docker Machine subcommand reference](/machine/reference/index.md)
{% endcomment %}
-  [Docker Machine 概要](overview.md)
-  [仮想環境を用いてローカルシステム上](get-started.md)に Docker ホストを生成して実行
-  [クラウドプロバイダー](get-started-cloud.md)上に複数の Docker ホストを実現
-  [Docker Machine ドライバーリファレンス](/machine/drivers/index.md)
-  [Docker Machine サブコマンドリファレンス](/machine/reference/index.md)
