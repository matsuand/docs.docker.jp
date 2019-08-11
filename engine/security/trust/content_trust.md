---
description: Enabling content trust in Docker
keywords: content, trust, security, docker, documentation
title: Docker Content Trust
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
ネットワークシステム内においてデータの受け渡しを行う際には、信頼（**trust**）というものが最も大切になります。
特にインターネットのような信頼性に欠ける媒体との間で通信を行うとき、システムが取り扱うことになるデータはすべて完全性を保ち、発信者が誰であるかを常に確実なものとすることが極めて重要です。
Docker Engine を利用する際には、イメージ（データ）を公開レジストリあるいはプライベートレジストリとの間でプッシュしたりプルしたりします。
Content Trust とは、あやゆるチャネルにわたってレジストリから取得するデータの完全性と公開者情報を検証する機能を提供するものです。

{% comment %}
## About Docker Content Trust (DCT)
{% endcomment %}
{: #about-docker-content-trust-dct }
## Docker Content Trust について

{% comment %}
Docker Content Trust (DCT) provides the ability to use digital signatures for
data sent to and received from remote Docker registries. These signatures allow
client-side or runtime verification of the integrity and publisher of specific
image tags.
{% endcomment %}
Docker Content Trust (DCT) は、リモートにある Docker レジストリとの間で送受信するデータに対して、デジタル署名を利用する機能を提供します。
この署名によって、特定のタグづけされたイメージに対する完全性と公開者情報を、クライアント側においてあるいは実行時に検証することが可能となります。

{% comment %}
Through DCT, image publishers can sign their images and image consumers can
ensure that the images they use are signed. Publishers could be individuals
or organizations manually signing their content or automated software supply
chains signing content as part of their release process.
{% endcomment %}
DCT を利用することで、イメージ公開者はイメージに署名をつけることができ、一方その利用者は、そのイメージが署名されていることを確認することができます。
公開者とは、個人や組織である場合もあります。
また公開イメージに手動で署名を行っているかもしれませんし、ソフトウェアサプライチェーンのリリース過程において署名を自動化して行っているかもしれません。

{% comment %}
### Image tags and DCT
{% endcomment %}
{: #image-tags-and-dct }
### イメージタグと DCT

{% comment %}
An individual image record has the following identifier:
{% endcomment %}
各イメージのレコードには、以下の識別子があります。

```
[REGISTRY_HOST[:REGISTRY_PORT]/]REPOSITORY[:TAG]
```

{% comment %}
A particular image `REPOSITORY` can have multiple tags. For example, `latest` and
 `3.1.2` are both tags on the `mongo` image. An image publisher can build an image
 and tag combination many times changing the image with each build.
{% endcomment %}
イメージ `REPOSITORY`にはタグを複数つけることができます。
たとえば `mongo` イメージにおいて `latest` と `3.1.2` という 2 つのタグづけが可能です。
イメージ公開者としては、イメージビルドとタグづけの組み合わせを何回も行って、イメージを作り変えていくことができます。

{% comment %}
DCT is associated with the `TAG` portion of an image. Each image repository has
a set of keys that image publishers use to sign an image tag. Image publishers
have discretion on which tags they sign.
{% endcomment %}
DCT はイメージの `TAG` 部分に関連づけられます。
イメージリポジトリでは一連のキーが管理されていて、そこにイメージ公開者がイメージタグに署名した情報が保持されます。
どのイメージに署名するのかは、イメージ公開者の判断に任されています。

{% comment %}
An image repository can contain an image with one tag that is signed and another
tag that is not. For example, consider [the Mongo image
repository](https://hub.docker.com/r/library/mongo/tags/). The `latest`
tag could be unsigned while the `3.1.6` tag could be signed. It is the
responsibility of the image publisher to decide if an image tag is signed or
not. In this representation, some image tags are signed, others are not:
{% endcomment %}
イメージリポジトリには、署名を行ったタグを持つイメージと、署名を行っていないものを同時に含めることができます。
たとえば [Mongo イメージリポジトリ](https://hub.docker.com/r/library/mongo/tags/) を確認してみてください。
`latest` タグには署名をつけず、`3.1.6` タグには署名するといったことが可能です。
イメージタグに署名するかどうかは、そのイメージ公開者の責任のもとで行うことになります。
以下の図においては、署名されているタグと署名されていないタグが示されています。

{% comment %}
![Signed tags](images/tag_signing.png)
{% endcomment %}
![署名されたタグ](images/tag_signing.png)

{% comment %}
Publishers can choose to sign a specific tag or not. As a result, the content of
an unsigned tag and that of a signed tag with the same name may not match. For
example, a publisher can push a tagged image `someimage:latest` and sign it.
Later, the same publisher can push an unsigned `someimage:latest` image. This second
push replaces the last unsigned tag `latest` but does not affect the signed `latest` version.
The ability to choose which tags they can sign, allows publishers to iterate over
the unsigned version of an image before officially signing it.
{% endcomment %}
イメージ公開者としては、特定のタグに対して署名するかしないかを選択します。
結果として同一のイメージ名であっても、署名されていないタグと署名されたタグとでは内容が異なることになります。
たとえばイメージ公開者が、タグづけしたイメージ `someimage:latest` をプッシュして、これに署名をしたとします。
この後にその人が、署名を行っていない `someimage:latest` イメージをプッシュしたとします。
2 つめにプッシュした内容は、署名を行っていない `latest` タグによって置き換えられることになりますが、ただし署名を行っていた `latest` バージョンには何ら影響を与えません。
署名する対象のタグは自由に選択できることから、公開者にとっては署名を行わないイメージ作りを何度も繰り返し行って、最後に公式イメージに署名を行うということもできます。

{% comment %}
Image consumers can enable DCT to ensure that images they use were signed. If a
consumer enables DCT, they can only pull, run, or build with trusted images.
Enabling DCT is a bit like applying a "filter" to your registry. Consumers "see"
only signed image tags and the less desirable, unsigned image tags are
"invisible" to them.
{% endcomment %}
イメージの利用者は DCT を使って、イメージが署名されているかどうかを確認することができます。
DCT を有効にしている場合、イメージの利用者は信頼できる署名済イメージのみをプル、実行、ビルドを行うことができます。
DCT を有効にするというのは、レジストリに対していわば「フィルタリング」を設定するようなものです。
利用者には署名されたイメージタグのみが「見える」のであって、これに比べて署名されていないイメージタグは好ましくないものとして「見えなくなる」というものです。

{% comment %}
{% endcomment %}
![信頼する画面](images/trust_view.png)

{% comment %}
{% endcomment %}
To the consumer who has not enabled DCT, nothing about how they work with Docker
images changes. Every image is visible regardless of whether it is signed or
not.

{% comment %}
{% endcomment %}
### Docker Content Trust Keys

{% comment %}
{% endcomment %}
Trust for an image tag is managed through the use of signing keys. A key set is
created when an operation using DCT is first invoked. A key set consists
of the following classes of keys:

{% comment %}
{% endcomment %}
- an offline key that is the root of DCT for an image tag
- repository or tagging keys that sign tags
- server-managed keys such as the timestamp key, which provides freshness
	security guarantees for your repository

{% comment %}
{% endcomment %}
The following image depicts the various signing keys and their relationships:

{% comment %}
{% endcomment %}
![Content Trust components](images/trust_components.png)

{% comment %}
{% endcomment %}
>**WARNING**:
> Loss of the root key is **very difficult** to recover from.
>Correcting this loss requires intervention from [Docker
>Support](https://support.docker.com) to reset the repository state. This loss
>also requires **manual intervention** from every consumer that used a signed
>tag from this repository prior to the loss.
{:.warning}

{% comment %}
{% endcomment %}
You should back up the root key somewhere safe. Given that it is only required
to create new repositories, it is a good idea to store it offline in hardware.
For details on securing, and backing up your keys, make sure you
read how to [manage keys for DCT](trust_key_mng.md).

{% comment %}
{% endcomment %}
## Signing Images with Docker Content Trust

{% comment %}
{% endcomment %}
> Note this applies to Docker Community Engine 17.12 and newer, and Docker
> Enterprise Engine 18.03 and newer.

{% comment %}
{% endcomment %}
Within the Docker CLI we can sign and push a container image with the
`$ docker trust` command syntax. This is built on top of the Notary feature
set, more information on Notary can be found [here](/notary/getting_started/).

{% comment %}
{% endcomment %}
A prerequisite for signing an image is a Docker Registry with a Notary server
attached (Such as the Docker Hub or Docker Trusted Registry). Instructions for
standing up a self-hosted environment can be found [here](/engine/security/trust/deploying_notary/).

{% comment %}
{% endcomment %}
To sign a Docker Image you will need a delegation key pair. These keys
can be generated locally using `$ docker trust key generate`, generated
by a certificate authority, or if you are using Docker Enterprise's
Universal Control Plane (UCP), a user's Client Bundle provides adequate keys for a
delegation. Find more information on Delegation Keys
[here](trust_delegation/#creating-delegation-keys).

{% comment %}
{% endcomment %}
First we will add the delegation private key to the local Docker trust
repository. (By default this is stored in `~/.docker/trust/`). If you are
generating delegation keys with `$ docker trust key generate`, the private key
is automatically added to the local trust store. If you are importing a separate
key, such as one from a UCP Client Bundle you will need to use the
`$ docker trust key load` command.

```
$ docker trust key generate jeff
Generating key for jeff...
Enter passphrase for new jeff key with ID 9deed25:
Repeat passphrase for new jeff key with ID 9deed25:
Successfully generated and loaded private key. Corresponding public key available: /home/ubuntu/Documents/mytrustdir/jeff.pub
```

{% comment %}
{% endcomment %}
Or if you have an existing key:

```
$ docker trust key load key.pem --name jeff
Loading key from "key.pem"...
Enter passphrase for new jeff key with ID 8ae710e:
Repeat passphrase for new jeff key with ID 8ae710e:
Successfully imported key from key.pem
```

{% comment %}
{% endcomment %}
Next we will need to add the delegation public key to the Notary server;
this is specific to a particular image repository in Notary known as a Global
Unique Name (GUN). If this is the first time you are adding a delegation to that
repository, this command will also initiate the repository, using a local Notary
canonical root key. To understand more about initiating a repository, and the
role of delegations, head to
[delegations for content trust](trust_delegation/#managing-delegations-in-a-notary-server).

```
$ docker trust signer add --key cert.pem jeff dtr.example.com/admin/demo
Adding signer "jeff" to dtr.example.com/admin/demo...
Enter passphrase for new repository key with ID 10b5e94:
```

{% comment %}
{% endcomment %}
Finally, we will use the delegation private key to sign a particular tag and
push it up to the registry.

```
$ docker trust sign dtr.example.com/admin/demo:1
Signing and pushing trust data for local image dtr.example.com/admin/demo:1, may overwrite remote trust data
The push refers to repository [dtr.example.com/admin/demo]
7bff100f35cb: Pushed
1: digest: sha256:3d2e482b82608d153a374df3357c0291589a61cc194ec4a9ca2381073a17f58e size: 528
Signing and pushing trust metadata
Enter passphrase for signer key with ID 8ae710e:
Successfully signed dtr.example.com/admin/demo:1
```

{% comment %}
{% endcomment %}
Alternatively, once the keys have been imported an image can be pushed with the
`$ docker push` command, by exporting the DCT environmental variable.

```
$ export DOCKER_CONTENT_TRUST=1

$ docker push dtr.example.com/admin/demo:1
The push refers to repository [dtr.example.com/admin/demo:1]
7bff100f35cb: Pushed
1: digest: sha256:3d2e482b82608d153a374df3357c0291589a61cc194ec4a9ca2381073a17f58e size: 528
Signing and pushing trust metadata
Enter passphrase for signer key with ID 8ae710e:
Successfully signed dtr.example.com/admin/demo:1
```

{% comment %}
{% endcomment %}
Remote trust data for a tag or a repository can be viewed by the
`$ docker trust inspect` command:

```
$ docker trust inspect --pretty dtr.example.com/admin/demo:1

{% comment %}
{% endcomment %}
Signatures for dtr.example.com/admin/demo:1

SIGNED TAG          DIGEST                                                             SIGNERS
1                   3d2e482b82608d153a374df3357c0291589a61cc194ec4a9ca2381073a17f58e   jeff

{% comment %}
{% endcomment %}
List of signers and their keys for dtr.example.com/admin/demo:1

SIGNER              KEYS
jeff                8ae710e3ba82

{% comment %}
{% endcomment %}
Administrative keys for dtr.example.com/admin/demo:1

  Repository Key:	10b5e94c916a0977471cc08fa56c1a5679819b2005ba6a257aa78ce76d3a1e27
  Root Key:	84ca6e4416416d78c4597e754f38517bea95ab427e5f95871f90d460573071fc
```

{% comment %}
{% endcomment %}
Remote Trust data for a tag can be removed by the `$ docker trust revoke` command:

```
$ docker trust revoke dtr.example.com/admin/demo:1
Enter passphrase for signer key with ID 8ae710e:
Successfully deleted signature for dtr.example.com/admin/demo:1
```

{% comment %}
{% endcomment %}
## Runtime Enforcement with Docker Content Trust

{% comment %}
{% endcomment %}
> Note this only applies to Docker Enterprise Engine 18.09 or newer. This
> implementation is also separate from the `only run signed images` feature of
> [Universal Control Plane](/ee/ucp/admin/configure/run-only-the-images-you-trust/)

{% comment %}
{% endcomment %}
Docker Content Trust within the Docker Enterprise Engine prevents a user from
using a container image from an unknown source, it will also prevent a user from
building a container image from a base layer from an unknown source. Trusted
sources could include Official Docker Images, found on the [Docker
Hub](https://hub.docker.com/search?image_filter=official&type=image), or User
trusted sources, with repositories and tags signed with the commands [above](#signing-images-with-docker-content-trust).

{% comment %}
{% endcomment %}
Engine Signature Verification prevents the following:
* `$ docker container run` of an unsigned or altered image.
* `$ docker pull` of an unsigned or altered image.
* `$ docker build` where the `FROM` image is not signed or is not scratch.

{% comment %}
{% endcomment %}
> **Note**: The implicit pulls and runs performed by worker
> nodes for a [Swarm service](/engine/swarm/services.md) on `$ docker service create` and
> `$ docker service update` are also verified. Tag resolution of services
> requires that all nodes in the Swarm including managers have content trust
> enabled and similarly configured.

{% comment %}
{% endcomment %}
DCT does not verify that a running container’s filesystem has not been altered
from what was in the image. For example, it does not prevent a container from
writing to the filesystem, once the container is running, nor does it prevent
the container’s filesystem from being altered on disk. DCT will also not prevent
unsigned images from being imported, loaded, or created.

{% comment %}
{% endcomment %}
### Enabling DCT within the Docker Enterprise Engine

{% comment %}
{% endcomment %}
DCT is controlled by the Docker Engine's configuration file. By default this is
found at `/etc/docker/daemon.json`. More details on this file can be found
[here](/engine/reference/commandline/dockerd/#daemon-configuration-file).

{% comment %}
{% endcomment %}
The `content-trust` flag is based around a `mode` variable instructing
the engine whether to enforce signed images, and a `trust-pinning` variable
instructing the engine which sources to trust.

{% comment %}
{% endcomment %}
`Mode` can take three variables:

{% comment %}
{% endcomment %}
* `Disabled` - Verification is not active and the remainder of the content-trust
related metadata will be ignored. This is the default value if `mode` is not
specified.
* `Permissive` - Verification will be performed, but only failures will be
logged and remain unenforced. This configuration is intended for testing of
changes related to content-trust. The results of the signature verification
is displayed in the Docker Engine's daemon logs.
* `Enforced` - Content trust will be enforced and an image that cannot be
verified successfully will not be pulled or run.

```
{
    "content-trust": {
        "mode": "enforced"
    }
}
```

{% comment %}
{% endcomment %}
### Official Docker images

{% comment %}
{% endcomment %}
All official Docker library images found on the Docker Hub (docker.io/library/*)
are signed by the same Notary root key. This root key's ID has been embedded
inside of the Docker Enterprise Engine. Therefore, to enforce that, only official
Docker images can be used. Specify:

```
{
  "content-trust": {
    "trust-pinning": {
      "official-library-images": true
    },
    "mode": "enforced"
  }
}
```

{% comment %}
{% endcomment %}
### User-Signed images

{% comment %}
{% endcomment %}
There are two options for trust pinning user-signed images:

{% comment %}
{% endcomment %}
* Notary Canonical Root Key ID (DCT Root Key) is an ID that describes *just* the
root key used to sign a repository (or rather its respective keys). This is the
root key on the host that originally signed the repository (i.e. your workstation).
This can be retrieved from the workstation that signed the repository through
`$ grep -r "root" ~/.docker/trust/private/` (Assuming your trust data is
at `~/.docker/trust/*`). It is expected that this canonical ID has initiated
multiple image repositories (`mydtr/user1/image1` and `mydtr/user1/image2`).

```
# Retrieving Root ID
$ grep -r "root" ~/.docker/trust/private
/home/ubuntu/.docker/trust/private/0b6101527b2ac766702e4b40aa2391805b70e5031c04714c748f914e89014403.key:role: root

# Using a Canonical ID that has signed 2 repos (mydtr/user1/repo1 and mydtr/user1/repo2). Note you can use a Wildcard.

{
  "content-trust": {
    "trust-pinning": {
      "root-keys": {
         "mydtr/user1/*": [
           "0b6101527b2ac766702e4b40aa2391805b70e5031c04714c748f914e89014403"
         ]
      }
    },
    "mode": "enforced"
  }
}
```

{% comment %}
{% endcomment %}
* Notary Root key ID (DCT Certificate ID) is an ID that describes the same, but
the ID is unique per repository. For example, `mydtr/user1/image1` and `mydtr/usr1/image2`
will have unique certificate IDs. A certificate ID can be retrieved through a
`$ docker trust inspect` command and is labelled as a root-key (referring back
to the Notary key name). This is designed for when different users are signing
their own repositories, for example, when there is no central signing server. As a cert-id
is more granular, it would take priority if a conflict occurs over a root ID.

```
# Retrieving Cert ID
$ docker trust inspect mydtr/user1/repo1 | jq -r '.[].AdministrativeKeys[] | select(.Name=="Root") | .Keys[].ID'
9430d6e31e3b3e240957a1b62bbc2d436aafa33726d0fcb50addbf7e2dfa2168

# Using Cert Ids, by specifying 2 repositories by their DCT root ID. Example for using this may be different DTRs or maybe because the repository was initiated on different hosts, therefore having different canonical IDs.

{
  "content-trust": {
    "trust-pinning": {
      "cert-ids": {
         "mydtr/user1/repo1": [
           "9430d6e31e3b3e240957a1b62bbc2d436aafa33726d0fcb50addbf7e2dfa2168"
         ],
         "mydtr/user2/repo1": [
           "544cf09f294860f9d5bc953ad80b386063357fd206b37b541bb2c54166f38d08"
         ]
      }
    },
    "mode": "enforced"
  }
}
```

{% comment %}
{% endcomment %}
### Using DCT in an offline environment

{% comment %}
{% endcomment %}
If your engine is unable to communicate to the registry, we can enable DCT to
trust cached signature data. This is done through the
`allow-expired-cached-trust-data` variable.

```
{
  "content-trust": {
    "trust-pinning": {
      "official-library-images": true,
      "root-keys": {
         "mydtr/user1/*": [
           "0b6101527b2ac766702e4b40aa2391805b70e5031c04714c748f914e89014403"
         ]
      },
      "cert-ids": {
         "mydtr/user2/repo1": [
           "9430d6e31e3b3e240957a1b62bbc2d436aafa33726d0fcb50addbf7e2dfa2168"
         ],
      }
    },
    "mode": "enforced",
    "allow-expired-cached-trust-data": true
  }
}
```

{% comment %}
{% endcomment %}
## Client Enforcement with Docker Content Trust

{% comment %}
{% endcomment %}
> Note this is supported on Docker Community and Enterprise Engines newer than
> 17.03.

{% comment %}
{% endcomment %}
Currently, content trust is disabled by default in the Docker Client. To enable
it, set the `DOCKER_CONTENT_TRUST` environment variable to `1`. This prevents
users from working with tagged images unless they contain a signature.

{% comment %}
{% endcomment %}
When DCT is enabled in the Docker client, `docker` CLI commands that operate on
tagged images must either have content signatures or explicit content hashes.
The commands that operate with DCT are:

* `push`
* `build`
* `create`
* `pull`
* `run`

{% comment %}
{% endcomment %}
For example, with DCT enabled a `docker pull someimage:latest` only
succeeds if `someimage:latest` is signed. However, an operation with an explicit
content hash always succeeds as long as the hash exists:

```
$ docker pull dtr.example.com/user/image:1
Error: remote trust data does not exist for dtr.example.com/user/image: dtr.example.com does not have trust data for dtr.example.com/user/image

$ docker pull dtr.example.com/user/image@sha256:d149ab53f8718e987c3a3024bb8aa0e2caadf6c0328f1d9d850b2a2a67f2819a
sha256:ee7491c9c31db1ffb7673d91e9fac5d6354a89d0e97408567e09df069a1687c1: Pulling from user/image
ff3a5c916c92: Pull complete
a59a168caba3: Pull complete
Digest: sha256:ee7491c9c31db1ffb7673d91e9fac5d6354a89d0e97408567e09df069a1687c1
Status: Downloaded newer image for dtr.example.com/user/image@sha256:ee7491c9c31db1ffb7673d91e9fac5d6354a89d0e97408567e09df069a1687c1
```

{% comment %}
## Related information
{% endcomment %}
{: #related-information }
## 関連情報

{% comment %}
{% endcomment %}
* [Delegations for content trust](trust_delegation.md)
* [Automation with content trust](trust_automation.md)
* [Manage keys for content trust](trust_key_mng.md)
* [Play in a content trust sandbox](trust_sandbox.md)
