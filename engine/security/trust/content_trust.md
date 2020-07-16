---
description: Docker においてコンテントトラストを有効にします。
keywords: content, trust, security, docker, documentation
title: Docker のコンテントトラスト
---

{% comment %}
When transferring data among networked systems, *trust* is a central concern. In
particular, when communicating over an untrusted medium such as the internet, it
is critical to ensure the integrity and the publisher of all the data a system
operates on. You use the Docker Engine to push and pull images (data) to a
public or private registry. Content trust gives you the ability to verify both
the integrity and the publisher of all the data received from a registry over
any channel.
{% endcomment %}
ネットワークシステム内でデータ転送を行う際には、**信頼**（trust）できるものであるかどうかが一番大切なことです。
特にインターネットのような信頼に欠けるシステム上で通信を行う場合、システムが扱うデータの整合性とその発信者情報を確実にすることが極めて重要です。
Docker Engine では、公開リポジトリあるいはプライベートリポジトリに対して、イメージ（データ）をプッシュしプルすることができます。
コンテントトラスト（content trust）はあらゆるチャネルを通じて、レジストリから取得するデータの整合性と公開者情報を、ともに検証できる仕組みを提供します。

{% comment %}
## About Docker Content Trust (DCT)
{% endcomment %}
{: #about-docker-content-trust-dct }
## Docker コンテントトラストについて

{% comment %}
Docker Content Trust (DCT) provides the ability to use digital signatures for
data sent to and received from remote Docker registries. These signatures allow
client-side or runtime verification of the integrity and publisher of specific
image tags.
{% endcomment %}
Docker コンテントトラスト (Docker Content Trust; DCT) は、リモート Docker レジストリとの間で送受信されるデータに対して、デジタル証明書を利用する機能を提供します。
この証明書があることによって、所定のイメージタグに対して、クライアントサイドつまり実行時での整合性および公開者情報の検証を可能としています。

{% comment %}
Through DCT, image publishers can sign their images and image consumers can
ensure that the images they pull are signed. Publishers could be individuals
or organizations manually signing their content or automated software supply
chains signing content as part of their release process.
{% endcomment %}
DCT を通じてイメージ公開者は、そのイメージを証明することができます。
そしてイメージ利用者は、プルを行うイメージが証明されていることがわかります。
公開者は個人の場合もあり組織の場合もあります。
リリース工程の一部としてこの証明作業を、手動で行っていたり、自動化されたソフトウェアサプライチェーンによって行っているかもしれません。

{% comment %}
### Image tags and DCT
{% endcomment %}
{: #image-tags-and-dct }
### イメージタグと DCT

{% comment %}
An individual image record has the following identifier:
{% endcomment %}
1 つのイメージには、以下の識別子が記録されています。

```
[REGISTRY_HOST[:REGISTRY_PORT]/]REPOSITORY[:TAG]
```

{% comment %}
A particular image `REPOSITORY` can have multiple tags. For example, `latest` and
 `3.1.2` are both tags on the `mongo` image. An image publisher can build an image
 and tag combination many times changing the image with each build.
{% endcomment %}
イメージ `REPOSITORY` は複数のタグを持つことができます。
たとえば `mongo` イメージにある `latest` と `3.1.2` というのは、どちらもタグです。
イメージ公開者は、イメージをビルドするたびに、何度もイメージとタグの組み合わせを作り出します。

{% comment %}
DCT is associated with the `TAG` portion of an image. Each image repository has
a set of keys that image publishers use to sign an image tag. Image publishers
have discretion on which tags they sign.
{% endcomment %}
DCT はイメージの `TAG` 部分に関連づけられます。
各イメージリポジトリには、公開者がイメージタグにサインするための鍵が複数あります。
どのタグに対してサインを行うかを、イメージ公開者は自由に取り決めています。

{% comment %}
An image repository can contain an image with one tag that is signed and another
tag that is not. For example, consider [the Mongo image
repository](https://hub.docker.com/r/library/mongo/tags/). The `latest`
tag could be unsigned while the `3.1.6` tag could be signed. It is the
responsibility of the image publisher to decide if an image tag is signed or
not. In this representation, some image tags are signed, others are not:
{% endcomment %}
イメージリポジトリには 1 つのイメージに対して、サインされたタグを持つものが 1 つだけあり、それ以外のタグはサインされていません。
[Mongo イメージリポジトリ](https://hub.docker.com/r/library/mongo/tags/) を例にして説明します。
`3.1.6` がサインされているタグであったとすると、`latest` はサインされていません。
どのイメージタグにサインし、サインしないかは、イメージ公開者が取り決める責任があります。
以下の図においては、サインされているイメージがあり、それ以外はサインされていません。

{% comment %}
{% endcomment %}
![サインされたタグ](images/tag_signing.png)

{% comment %}
Publishers can choose to sign a specific tag or not. As a result, the content of
an unsigned tag and that of a signed tag with the same name may not match. For
example, a publisher can push a tagged image `someimage:latest` and sign it.
Later, the same publisher can push an unsigned `someimage:latest` image. This second
push replaces the last unsigned tag `latest` but does not affect the signed `latest` version.
The ability to choose which tags they can sign, allows publishers to iterate over
the unsigned version of an image before officially signing it.
{% endcomment %}
公開者としては、どのタグにサインするかを取り決めることができます。
サインされているものとサインされていないものが、結果的に同じ名称になったとしても、同一の内容としては扱われません。
たとえば公開者がタグづけしたイメージ `someimage:latest` をプッシュして、これにサインしたとします。
後にその公開者は、サインをしていない `someimage:latest` というイメージをプッシュすることができます。
2 度めに行ったプッシュによって、サインをしていなかった直前の `latest` がプッシュしたものに置き換えられますが、サインしている `latest` には何ら影響しません。
公開者がサインするタグを選べるということは、イメージをサインせずに何回も繰り返しプッシュした上で、最後に公式イメージにサインするというやり方が可能になります。

{% comment %}
Image consumers can enable DCT to ensure that images they use were signed. If a
consumer enables DCT, they can only pull, run, or build with trusted images.
Enabling DCT is a bit like applying a "filter" to your registry. Consumers "see"
only signed image tags and the less desirable, unsigned image tags are
"invisible" to them.
{% endcomment %}
イメージ利用者は DCT を通じて、利用するイメージがサインされているかどうかがわかります。
利用者が DCT を有効にしている場合、イメージをプルしビルドし実行するのは、信頼できる（trusted）イメージのみとなります。
DCT を有効にすることは、レジストリに「フィルター」をかけるようなものです。
利用者にとって、サインされたイメージタグだけが「見える」ものになります。
あまり利用することがない、未サインのイメージタグは「見えない」わけです。

{% comment %}
![Trust view](images/trust_view.png)
{% endcomment %}
![信頼ビュー](images/trust_view.png)

{% comment %}
To the consumer who has not enabled DCT, nothing about how they work with Docker
images changes. Every image is visible regardless of whether it is signed or
not.
{% endcomment %}
利用者が DCT を有効にしていなければ、Docker イメージの操作方法は何も変わりません。
イメージがサインされていてもいなくても、すべてのイメージを見ることができます。

{% comment %}
### Docker Content Trust Keys
{% endcomment %}
{: #docker-content-trust-keys }
### Docker コンテントトラストの鍵

{% comment %}
Trust for an image tag is managed through the use of signing keys. A key set is
created when an operation using DCT is first invoked. A key set consists
of the following classes of keys:
{% endcomment %}
イメージタグへのトラストつまり信頼ができるかどうかは、証明書鍵を利用して管理されます。
DCT を利用する操作を行った初回において、鍵一式が生成されます。
鍵一式には、以下のような種類の鍵があります。

{% comment %}
- an offline key that is the root of DCT for an image tag
- repository or tagging keys that sign tags
- server-managed keys such as the timestamp key, which provides freshness
	security guarantees for your repository
{% endcomment %}
- オフライン鍵（offline key）。イメージタグ用の DCT ルート鍵です。
- タグにサインをするリポジトリ鍵（repository key）あるいはタグ用鍵（tagging key）。
- サーバーによって管理される、たとえばタイムスタンプ鍵（timestamp key）。
  リポジトリに対して、最新のセキュリティ保証を行います。

{% comment %}
The following image depicts the various signing keys and their relationships:
{% endcomment %}
以下の図では、いろいろな種類の鍵とその関係を表わしています。

{% comment %}
![Content Trust components](images/trust_components.png)
{% endcomment %}
![コンテントトラストの構成](images/trust_components.png)

{% comment %}
> **WARNING**:
> Loss of the root key is **very difficult** to recover from.
>Correcting this loss requires intervention from [Docker
>Support](https://support.docker.com) to reset the repository state. This loss
>also requires **manual intervention** from every consumer that used a signed
>tag from this repository prior to the loss.
{:.warning}
{% endcomment %}
> **警告**:
> ルート鍵を失ってしまうと復旧が **極めて困難** になります。
> これを復旧するためには [Docker サポート](https://support.docker.com) が調整を行う必要があり、リポジトリ状態をリセットしなければなりません。
> またこのリポジトリを通じてサイン済タグを利用していたすべてのユーザーに対して、ルート鍵を失なう以前のイメージは **手動で復旧操作を行う** 必要があります。
{:.warning}

{% comment %}
You should back up the root key somewhere safe. Given that it is only required
to create new repositories, it is a good idea to store it offline in hardware.
For details on securing, and backing up your keys, make sure you
read how to [manage keys for DCT](trust_key_mng.md).
{% endcomment %}
ルート鍵はどこか安全なところにバックアップをとっておいてください。
このルート鍵は新たなリポジトリを生成する重要なものなので、オフラインのデータとして保存しておくことをお勧めします。
セキュアなリポジトリの維持、鍵のバックアップに関しては [DCT における鍵の管理](trust_key_mng.md) を参照してください。

{% comment %}
## Signing Images with Docker Content Trust
{% endcomment %}
{: #signing-images-with-docker-content-trust }
## DCT によるイメージへのサイン

{% comment %}
> **Note:**
> This applies to Docker Community Engine 17.12 and newer.
{% endcomment %}
> **メモ**
> これは Docker Community Engine 17.12 またはそれ以降に適用されます。

{% comment %}
Within the Docker CLI we can sign and push a container image with the
`$ docker trust` command syntax. This is built on top of the Notary feature
set, more information on Notary can be found [here](/notary/getting_started/).
{% endcomment %}
Docker CLI においては `docker trust` コマンドを使って、コンテナーイメージにサインしてプッシュすることができます。
これは Notary 機能の上に実現されています。
Notary の詳細は [こちら](/notary/getting_started/) を参照してください。

{% comment %}
A prerequisite for signing an image is a Docker Registry with a Notary server
attached (Such as the Docker Hub ). Instructions for
standing up a self-hosted environment can be found [here](/engine/security/trust/deploying_notary/).
{% endcomment %}
イメージへのサインを行うためには、（Docker Hub のような）Notary サーバーが連携した Docker Registry が必要です。
独自のサーバー環境を構築するのであれば、その手順が [こちら](/engine/security/trust/deploying_notary/) にあります。

{% comment %}
To sign a Docker Image you will need a delegation key pair. These keys
can be generated locally using `$ docker trust key generate` or generated
by a certificate authority.
{% endcomment %}
Docker イメージにサインするには、委任鍵ペア（delegation key pair）が必要です。
この鍵はローカルで `docker trust key generate` の実行によって生成するか、認証局によって生成されます。

{% comment %}
First we will add the delegation private key to the local Docker trust
repository. (By default this is stored in `~/.docker/trust/`). If you are
generating delegation keys with `$ docker trust key generate`, the private key
is automatically added to the local trust store. If you are importing a separate
key, you will need to use the
`$ docker trust key load` command.
{% endcomment %}
はじめにローカルの Docker トラストレジストリに、委任鍵ペアを追加します。
（デフォルトでこの鍵は `~/.docker/trust/` に保存します。)
`docker trust key generate` によって委任鍵ペアを生成している場合、秘密鍵（private key）も自動的にローカルの保存場所に生成されています。
個別に鍵をインポートしている場合は、`docker trust key load` コマンドを実行することが必要です。

```
$ docker trust key generate jeff
Generating key for jeff...
Enter passphrase for new jeff key with ID 9deed25:
Repeat passphrase for new jeff key with ID 9deed25:
Successfully generated and loaded private key. Corresponding public key available: /home/ubuntu/Documents/mytrustdir/jeff.pub
```

{% comment %}
Or if you have an existing key:
{% endcomment %}
すでに鍵がある場合は以下のようにします。

```
$ docker trust key load key.pem --name jeff
Loading key from "key.pem"...
Enter passphrase for new jeff key with ID 8ae710e:
Repeat passphrase for new jeff key with ID 8ae710e:
Successfully imported key from key.pem
```

{% comment %}
Next we will need to add the delegation public key to the Notary server;
this is specific to a particular image repository in Notary known as a Global
Unique Name (GUN). If this is the first time you are adding a delegation to that
repository, this command will also initiate the repository, using a local Notary
canonical root key. To understand more about initiating a repository, and the
role of delegations, head to
[delegations for content trust](trust_delegation/#managing-delegations-in-a-notary-server).
{% endcomment %}
次に委任鍵ペアの公開鍵を Notary サーバーへ追加します。
これは Notary の Global Unique Name (GUN) と呼ばれるイメージリポジトリとして特化したものです。
委任鍵ペアをリポジトリに追加する初回は、このコマンド実行において、ローカル Notary サーバーの正規のルート鍵を使って、リポジトリが初期化されます。
リポジトリの初期化と委任鍵の役割に関しては、[コンテントトラストの委任鍵](trust_delegation/#managing-delegations-in-a-notary-server) を参照してください。

```
$ docker trust signer add --key cert.pem jeff registry.example.com/admin/demo
Adding signer "jeff" to registry.example.com/admin/demo...
Enter passphrase for new repository key with ID 10b5e94:
```

{% comment %}
Finally, we will use the delegation private key to sign a particular tag and
push it up to the registry.
{% endcomment %}
最後に委任鍵ペアの秘密鍵を使って、指定するタグに対してサインを行い、これをレジストリにプッシュします。

```
$ docker trust sign registry.example.com/admin/demo:1
Signing and pushing trust data for local image registry.example.com/admin/demo:1, may overwrite remote trust data
The push refers to repository [registry.example.com/admin/demo]
7bff100f35cb: Pushed
1: digest: sha256:3d2e482b82608d153a374df3357c0291589a61cc194ec4a9ca2381073a17f58e size: 528
Signing and pushing trust metadata
Enter passphrase for signer key with ID 8ae710e:
Successfully signed registry.example.com/admin/demo:1
```

{% comment %}
Alternatively, once the keys have been imported an image can be pushed with the
`$ docker push` command, by exporting the DCT environmental variable.
{% endcomment %}
あるいは鍵ペアがインポート済であれば、DCT 環境変数を設定しておくことで、`docker push` コマンドによりイメージをプッシュすることもできます。

```
$ export DOCKER_CONTENT_TRUST=1

$ docker push registry.example.com/admin/demo:1
The push refers to repository [registry.example.com/admin/demo:1]
7bff100f35cb: Pushed
1: digest: sha256:3d2e482b82608d153a374df3357c0291589a61cc194ec4a9ca2381073a17f58e size: 528
Signing and pushing trust metadata
Enter passphrase for signer key with ID 8ae710e:
Successfully signed registry.example.com/admin/demo:1
```

{% comment %}
Remote trust data for a tag or a repository can be viewed by the
`$ docker trust inspect` command:
{% endcomment %}
リモートにあるトラストデータやリポジトリを参照するには `docker trust inspect` コマンドを実行します。

```
$ docker trust inspect --pretty registry.example.com/admin/demo:1

Signatures for registry.example.com/admin/demo:1

SIGNED TAG          DIGEST                                                             SIGNERS
1                   3d2e482b82608d153a374df3357c0291589a61cc194ec4a9ca2381073a17f58e   jeff

List of signers and their keys for registry.example.com/admin/demo:1

SIGNER              KEYS
jeff                8ae710e3ba82

Administrative keys for registry.example.com/admin/demo:1

  Repository Key:	10b5e94c916a0977471cc08fa56c1a5679819b2005ba6a257aa78ce76d3a1e27
  Root Key:	84ca6e4416416d78c4597e754f38517bea95ab427e5f95871f90d460573071fc
```

{% comment %}
Remote Trust data for a tag can be removed by the `$ docker trust revoke` command:
{% endcomment %}
リモートにあるトラストデータは `docker trust revoke` コマンドにより削除することができます。

```
$ docker trust revoke registry.example.com/admin/demo:1
Enter passphrase for signer key with ID 8ae710e:
Successfully deleted signature for registry.example.com/admin/demo:1
```

{% comment %}
## Client Enforcement with Docker Content Trust
{% endcomment %}
{: #client-enforcement-with-docker-content-trust }
## Docker コンテントトラストのクライントでの利用

{% comment %}
Content trust is disabled by default in the Docker Client. To enable
it, set the `DOCKER_CONTENT_TRUST` environment variable to `1`. This prevents
users from working with tagged images unless they contain a signature.
{% endcomment %}
Docker クライアントにおいて、コンテントトラストはデフォルトで無効になっています。
これを有効にするには、環境変数 `DOCKER_CONTENT_TRUST` を `1` に設定します。
これを行っておけば、サインされていないタグつきイメージは、操作対象になりません。

{% comment %}
When DCT is enabled in the Docker client, `docker` CLI commands that operate on
tagged images must either have content signatures or explicit content hashes.
The commands that operate with DCT are:
{% endcomment %}
Docker クライアントにおいて DCT が有効である場合、タグつきのイメージを操作する `docker` コマンドには、コンテント署名あるいは明示的なコンテントハッシュがなければなりません。
DCT の操作が可能なコマンドは以下です。

* `push`
* `build`
* `create`
* `pull`
* `run`

{% comment %}
For example, with DCT enabled a `docker pull someimage:latest` only
succeeds if `someimage:latest` is signed. However, an operation with an explicit
content hash always succeeds as long as the hash exists:
{% endcomment %}
たとえば DCT を有効にしている場合、`docker pull someimage:latest` というコマンドは `someimage:latest` がサインされている場合のみ処理が成功します。
ただしコンテントハッシュが明示的に指定されている場合は、このコマンドの処理は常に成功します。

```
$ docker pull registry.example.com/user/image:1
Error: remote trust data does not exist for registry.example.com/user/image: registry.example.com does not have trust data for registry.example.com/user/image

$ docker pull registry.example.com/user/image@sha256:d149ab53f8718e987c3a3024bb8aa0e2caadf6c0328f1d9d850b2a2a67f2819a
sha256:ee7491c9c31db1ffb7673d91e9fac5d6354a89d0e97408567e09df069a1687c1: Pulling from user/image
ff3a5c916c92: Pull complete
a59a168caba3: Pull complete
Digest: sha256:ee7491c9c31db1ffb7673d91e9fac5d6354a89d0e97408567e09df069a1687c1
Status: Downloaded newer image for registry.example.com/user/image@sha256:ee7491c9c31db1ffb7673d91e9fac5d6354a89d0e97408567e09df069a1687c1
```

{% comment %}
## Related information
{% endcomment %}
{: #related-information }
## 関連情報

{% comment %}
* [Delegations for content trust](trust_delegation.md)
* [Automation with content trust](trust_automation.md)
* [Manage keys for content trust](trust_key_mng.md)
* [Play in a content trust sandbox](trust_sandbox.md)
{% endcomment %}
* [コンテントトラストの委任鍵ペア](trust_delegation.md)
* [コンテントトラストの自動化](trust_automation.md)
* [コンテントトラストにおける鍵の管理](trust_key_mng.md)
* [コンテントトラストのサンドボックスで遊ぶ](trust_sandbox.md)
