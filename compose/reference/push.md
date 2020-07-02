---
description: サービスのイメージをプッシュします。
keywords: fig, composition, compose, docker, orchestration, cli,  push
title: docker-compose push
notoc: true
---

{% comment %}
```
Usage: push [options] [SERVICE...]

Options:
    --ignore-push-failures  Push what it can and ignores images with push failures.
```
{% endcomment %}
```
利用方法: push [オプション] [SERVICE...]

オプション:
    --ignore-push-failures  可能なものはプッシュし、失敗するものは無視します。
```

{% comment %}
Pushes images for services to their respective `registry/repository`.
{% endcomment %}
サービスのイメージを、それぞれの `registry/repository` に対してプッシュします。

{% comment %}
The following assumptions are made:
{% endcomment %}
以下のことを前提としています。

{% comment %}
- You are pushing an image you have built locally
{% endcomment %}
- ローカルにビルド済のイメージをプッシュするものとします。

{% comment %}
- You have access to the build key
{% endcomment %}
- ビルドキーに対してアクセス権を有しているものとします。

{% comment %}
## Example
{% endcomment %}
## 例
{: #example }

{% comment %}
```yaml
version: '3'
services:
  service1:
    build: .
    image: localhost:5000/yourimage  # goes to local registry

  service2:
    build: .
    image: your-dockerid/yourimage  # goes to your repository on Docker Hub
```
{% endcomment %}
```yaml
version: '3'
services:
  service1:
    build: .
    image: localhost:5000/yourimage  # ローカルレジストリへ

  service2:
    build: .
    image: youruser/yourimage  # 自ユーザーの DockerHub レジストリへ
```
