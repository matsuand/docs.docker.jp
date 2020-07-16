---
description: コンテントトラストにおいて鍵を管理します。
keywords: trust, security, root,  keys, repository
title: コンテントトラストにおける鍵の管理
---

{% comment %}
Trust for an image tag is managed through the use of keys. Docker's content
trust makes use of five different types of keys:
{% endcomment %}
イメージタグに対する信頼（trust）は、鍵を利用することによって管理します。
Docker のコンテントトラストは 5 種類の鍵を利用します。

{% comment %}
| Key        | Description |
|:-----------|:----------- |
| root key   | Root of content trust for an image tag. When content trust is enabled, you create the root key once. Also known as the offline key, because it should be kept offline. |
| targets    | This key allows you to sign image tags, to manage delegations including delegated keys or permitted delegation paths. Also known as the repository key, since this key determines what tags can be signed into an image repository. |
| snapshot   | This key signs the current collection of image tags, preventing mix and match attacks. |
| timestamp  | This key allows Docker image repositories to have freshness security guarantees without requiring periodic content refreshes on the client's side. |
| delegation | Delegation keys are optional tagging keys and allow you to delegate signing image tags to other publishers without having to share your targets key. |
{% endcomment %}
| 鍵               | 内容説明    |
|:-----------------|:----------- |
| ルート           | Root of content trust for an image tag. When content trust is enabled, you create the root key once. Also known as the offline key, because it should be kept offline. |
| ターゲット       | This key allows you to sign image tags, to manage delegations including delegated keys or permitted delegation paths. Also known as the repository key, since this key determines what tags can be signed into an image repository. |
| スナップショット | This key signs the current collection of image tags, preventing mix and match attacks. |
| タイムスタンプ   | This key allows Docker image repositories to have freshness security guarantees without requiring periodic content refreshes on the client's side. |
| 委任(delegation) | Delegation keys are optional tagging keys and allow you to delegate signing image tags to other publishers without having to share your targets key. |

{% comment %}
When doing a `docker push` with Content Trust enabled for the first time, the
root, targets, snapshot, and timestamp keys are generated automatically for
the image repository:
{% endcomment %}
コンテントトラストを有効にした状態で、初めて `docker push` を実行すると、そのイメージリポジトリに対して、ルート、ターゲット、スナップショット、タイムスタンプの各鍵が自動生成されます。

{% comment %}
- The root and targets key are generated and stored locally client-side.
{% endcomment %}
- ルート鍵とターゲット鍵が生成されると、クライアントサイドにローカルに保存されます。

{% comment %}
- The timestamp and snapshot keys are safely generated and stored in a signing server
	that is deployed alongside the Docker registry. These keys are generated in a backend
	service that isn't directly exposed to the internet and are encrypted at rest.
{% endcomment %}
- タイムスタンプ鍵とスナップショット鍵は Docker レジストリと並んでデプロイされる認証サーバー内に、安全に生成され保存されます。
  これらの鍵はバックエンドサービスにより生成されるものであり、インターネット上に直接公開されることはなく、安全に暗号化されます。

{% comment %}
Delegation keys are optional, and not generated as part of the normal `docker`
workflow.  They need to be
[manually generated and added to the repository](trust_delegation.md#creating-delegation-keys).
{% endcomment %}
委任鍵はオプションであって、`docker` の通常処理の中では生成されません。
これには [手動生成とリポジトリへの追加](trust_delegation.md#creating-delegation-keys) が必要になります。

{% comment %}
**Note**: Prior to Docker Engine 1.11, the snapshot key was also generated and stored
locally client-side.
Use the Notary CLI to [manage your snapshot key locally again](../../../notary/advanced_usage.md#rotate-keys)
for repositories created with newer versions of Docker.
{% endcomment %}
**メモ** Docker Engine 1.11 より前のバージョンでは、スナップショット鍵もクライアントサイドにローカルに保存されていました。
Use the Notary CLI to [manage your snapshot key locally again](../../../notary/advanced_usage.md#rotate-keys)
for repositories created with newer versions of Docker.

{% comment %}
## Choose a passphrase
{% endcomment %}
{: #choose-a-passphrase }
## パスフレーズの決定

{% comment %}
The passphrases you chose for both the root key and your repository key should
be randomly generated and stored in a password manager. Having the repository key
allows users to sign image tags on a repository. Passphrases are used to encrypt
your keys at rest and ensure that a lost laptop or an unintended backup doesn't
put the private key material at risk.
{% endcomment %}
The passphrases you chose for both the root key and your repository key should
be randomly generated and stored in a password manager. Having the repository key
allows users to sign image tags on a repository. Passphrases are used to encrypt
your keys at rest and ensure that a lost laptop or an unintended backup doesn't
put the private key material at risk.

{% comment %}
## Back up your keys
{% endcomment %}
{: #back-up-your-keys }
## 鍵のバックアップ

{% comment %}
{% endcomment %}
All the Docker trust keys are stored encrypted using the passphrase you provide
on creation. Even so, you should still take care of the location where you back them up.
Good practice is to create two encrypted USB keys.

{% comment %}
{% endcomment %}
It is very important that you back up your keys to a safe, secure location. Loss
of the repository key is recoverable; loss of the root key is not.

{% comment %}
{% endcomment %}
The Docker client stores the keys in the `~/.docker/trust/private` directory.
Before backing them up, you should `tar` them into an archive:

```bash
$ umask 077; tar -zcvf private_keys_backup.tar.gz ~/.docker/trust/private; umask 022
```

{% comment %}
## Hardware storage and signing
{% endcomment %}
## Hardware storage and signing

{% comment %}
{% endcomment %}
Docker Content Trust can store and sign with root keys from a Yubikey 4. The
Yubikey is prioritized over keys stored in the filesystem. When you initialize a
new repository with content trust, Docker Engine looks for a root key locally. If a
key is not found and the Yubikey 4 exists, Docker Engine creates a root key in the
Yubikey 4. Consult the [Notary documentation](../../../notary/advanced_usage.md#use-a-yubikey)
for more details.

{% comment %}
Prior to Docker Engine 1.11, this feature was only in the experimental branch.
{% endcomment %}
Docker Engine 1.11 より前のバージョンにおいてこの機能は、試験用ブランチからのみ提供されています。

{% comment %}
## Lost keys
{% endcomment %}
{: #lost-keys }
## 鍵の紛失

{% comment %}
If a publisher loses keys it means losing the ability to sign trusted content for
your repositories.  If you lose a key, send an email to [Docker Hub
Support](mailto:hub-support@docker.com) to reset the repository
state.
{% endcomment %}
イメージの公開者が鍵を失うということは、リポジトリ内のコンテントデータにサインできなくなるということです。
鍵を紛失したときは [Docker Hub サポート](mailto:hub-support@docker.com) にメールで連絡してください。
リポジトリのリセット操作を行います。
state.

{% comment %}
This loss also requires **manual intervention** from every consumer that pulled
the tagged image prior to the loss. Image consumers would get an error for
content that they already downloaded:
{% endcomment %}
鍵の紛失してしまうと、紛失以前のタグイメージをプルしていたユーザーは、全員が **手動で復旧操作を行う** 必要があります。
イメージ利用者がすでにイメージをダウンロードしてしまっているときには、エラーが発生します。

```
Warning: potential malicious behavior - trust data has insufficient signatures for remote repository docker.io/my/image: valid signatures did not meet threshold
```

{% comment %}
To correct this, they need to download a new image tag that is signed with
the new key.
{% endcomment %}
エラー復旧するには、新たな鍵によって正しくサインされた、新しいイメージをダウンロードすることが必要です。

{% comment %}
## Related information
{% endcomment %}
{: #related-information }
## 関連情報

{% comment %}
* [Content trust in Docker](content_trust.md)
* [Automation with content trust](trust_automation.md)
* [Delegations for content trust](trust_delegation.md)
* [Play in a content trust sandbox](trust_sandbox.md)
{% endcomment %}
* [Docker のコンテントトラスト](content_trust.md)
* [コンテントトラストの自動化](trust_automation.md)
* [コンテントトラストの委任鍵ペア](trust_delegation.md)
* [コンテントトラストのサンドボックスで遊ぶ](trust_sandbox.md)
