---
description: create コマンドはサービスのコンテナーを生成します。
keywords: fig, composition, compose, docker, orchestration, cli, create
title: docker-compose create
notoc: true
---

サービスコンテナーを生成します。
このコマンドは廃止予定（deprecated）です。
このかわりに `up` コマンドに `--no-start` オプションをつけて実行してください。

{% comment %}
Usage: create [options] [SERVICE...]

Options:
    --force-recreate       Recreate containers even if their configuration and
                           image haven't changed. Incompatible with --no-recreate.
    --no-recreate          If containers already exist, don't recreate them.
                           Incompatible with --force-recreate.
    --no-build             Don't build an image, even if it's missing.
    --build                Build images before creating containers.
{% endcomment %}
```
利用方法: create [オプション] [サービス名...]

オプション:
    --force-recreate       設定やイメージに変更がなくてもコンテナーを再生成します。
                           --no-recreate と同時には使えません。
    --no-recreate          コンテナーが存在する場合は再生成しません。
                           --force-recreate と同時は使えません。
    --no-build             イメージが見つからなくてもイメージをビルドしません。
    --build                コンテナーの生成前にイメージをビルドします。
```
