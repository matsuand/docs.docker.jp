---
description: スタックをスケール変更します。
keywords: aws, amazon, iaas, tutorial
title: AWS 上の Docker インストールの変更
---

{% comment %}
## Scaling workers
{% endcomment %}
{: #scaling-workers }
## ワーカーのスケール変更

{% comment %}
You can scale the worker count using the AWS Auto Scaling group. Docker
automatically joins or removes new instances to the Swarm.
{% endcomment %}
ワーカー数は AWS Auto Scaling グループを使って変更することができます。
Docker は自動的にスウォームに対して、新たなインスタンスの参加あるいは削除を行います。

{% comment %}
There are currently two ways to scale your worker group. You can "update" your
stack, and change the number of workers in the CloudFormation template
parameters, or you can manually update the Auto Scaling group in the AWS console
for EC2 auto scaling groups.
{% endcomment %}
ワーカーグループのスケールを変更するには、現在 2 つの方法があります。
You can "update" your
stack, and change the number of workers in the CloudFormation template
parameters, or you can manually update the Auto Scaling group in the AWS console
for EC2 auto scaling groups.

{% comment %}
Changing manager count live is _not_ currently supported.
{% endcomment %}
Changing manager count live is _not_ currently supported.

{% comment %}
### AWS console
{% endcomment %}
{: #aws-console }
### AWS コンソール
{% comment %}
Log in to the AWS console, and go to the EC2 dashboard. On the lower left hand
side select the "Auto Scaling Groups" link.
{% endcomment %}
AWS コンソールにログインして EC2 ダッシュボードを開いてください。
左側の下段にある「Auto Scaling Groups」リンクをクリックします。

{% comment %}
Look for the Auto Scaling group with the name that looks like
$STACK_NAME-NodeASG-* Where `$STACK_NAME` is the name of the stack you created
when filling out the CloudFormation template for Docker for AWS. Once you find
it, click the checkbox, next to the name. Then Click on the "Edit" button on the
lower detail pane.
{% endcomment %}
Look for the Auto Scaling group with the name that looks like
$STACK_NAME-NodeASG-* Where `$STACK_NAME` is the name of the stack you created
when filling out the CloudFormation template for Docker for AWS. Once you find
it, click the checkbox, next to the name. Then Click on the "Edit" button on the
lower detail pane.

{% comment %}
![edit autoscale settings](img/autoscale_update.png)
{% endcomment %}
![autoscale 設定の編集](img/autoscale_update.png)

{% comment %}
Change the "Desired" field to the size of the worker pool that you would like, and hit "Save".
{% endcomment %}
「Desired」欄において、必要とするワーカーのプールサイズを入力して「Save」をクリックします。

{% comment %}
![save autoscale settings](img/autoscale_save.png)
{% endcomment %}
![autoscale 設定の保存](img/autoscale_save.png)

{% comment %}
This takes a few minutes and add the new workers to your swarm
automatically. To lower the number of workers back down, you just need to update
"Desired" again, with the lower number, and it shrinks the worker pool until
it reaches the new size.
{% endcomment %}
数分後にはスウォームに対して新たなワーカーが自動的に加えられます。
ワーカー数を減らすには、また「Desired」欄に戻って、ワーカー数を入力します。
ワーカープールサイズは、新たに入力したサイズまで縮小されていきます。

{% comment %}
### CloudFormation update
{% endcomment %}
{: #cloudformation-update }
### CloudFormation のアップデート

{% comment %}
Go to the CloudFormation management page, and click the checkbox next to the
stack you want to update. Then Click on the action button at the top, and select
"Update Stack".
{% endcomment %}
Go to the CloudFormation management page, and click the checkbox next to the
stack you want to update. Then Click on the action button at the top, and select
"Update Stack".

{% comment %}
{% endcomment %}
![update stack on CloudFormation page](img/cloudformation_update.png)

{% comment %}
{% endcomment %}
Pick "Use current template", and then click "Next". Fill out the same parameters
you have specified before, but this time, change your worker count to the new
count, click "Next". Answer the rest of the form questions. CloudFormation
shows you a preview of the changes. Review the changes and if they
look good, click "Update". CloudFormation changes the worker pool size to
the new value you specified. It takes a few minutes (longer for a larger
increase / decrease of nodes), but when complete the swarm has
the new worker pool size.
