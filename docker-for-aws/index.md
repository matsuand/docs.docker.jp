---
description: セットアップと前提条件
keywords: aws, amazon, iaas, tutorial
title: Docker for AWS のセットアップと前提条件
redirect_from:
- /engine/installation/cloud/cloud-ex-aws/
- /engine/installation/amazon/
- /datacenter/install/aws/
---

{% include d4a_buttons.md %}

{% comment %}
## Docker Enterprise Edition (EE) for AWS
{% endcomment %}
{: #docker-enterprise-edition-ee-for-aws }
## Docker Enterprise Edition (EE) for AWS

{% comment %}
This deployment is fully baked and tested, and comes with the latest Docker
Enterprise Edition for AWS.
{% endcomment %}
This deployment is fully baked and tested, and comes with the latest Docker
Enterprise Edition for AWS.

{% comment %}
This release is maintained and receives **security and critical bug fixes for
one year**.
{% endcomment %}
This release is maintained and receives **security and critical bug fixes for
one year**.

{% comment %}
[Deploy Docker Enterprise Edition (EE) for AWS](https://hub.docker.com/editions/enterprise/docker-ee-aws?tab=description){: target="_blank" class="button outline-btn blank_"}
{% endcomment %}
[Docker Enterprise Edition (EE) for AWS のデプロイ](https://hub.docker.com/editions/enterprise/docker-ee-aws?tab=description){: target="_blank" class="button outline-btn blank_"}


{% comment %}
## Docker Community Edition (CE) for AWS
{% endcomment %}
{: #docker-community-edition-ce-for-aws }
## Docker Community Edition (CE) for AWS

{% comment %}
### Quickstart
{% endcomment %}
{: #quickstart }
### クィックスタート

{% comment %}
If your account [has the proper
permissions](/docker-for-aws/iam-permissions.md), you can
use the blue button to bootstrap Docker for AWS
using CloudFormation.
{% endcomment %}
If your account [has the proper
permissions](/docker-for-aws/iam-permissions.md), you can
use the blue button to bootstrap Docker for AWS
using CloudFormation.

{{aws_blue_latest}}
{{aws_blue_vpc_latest}}

{% comment %}
### Deployment options
{% endcomment %}
{: #deployment-options }
### デプロイ方法の選択

{% comment %}
There are two ways to deploy Docker for AWS:
{% endcomment %}
Docker for AWS をデプロイするには以下の 2 通りがあります。

{% comment %}
- With a pre-existing VPC
- With a new VPC created by Docker
{% endcomment %}
- すでにある VPC を利用する方法
- Docker によって生成した新規 VPC を利用する方法

{% comment %}
We recommend allowing Docker for AWS to create the VPC since it allows Docker to optimize the environment. Installing in an existing VPC requires more work.
{% endcomment %}
We recommend allowing Docker for AWS to create the VPC since it allows Docker to optimize the environment. Installing in an existing VPC requires more work.

{% comment %}
#### Create a new VPC
{% endcomment %}
{: #create-a-new-vpc }
#### VPC の新規生成
{% comment %}
This approach creates a new VPC, subnets, gateways, and everything else needed to run Docker for AWS. It is the easiest way to get started, and requires the least amount of work.
{% endcomment %}
この方法では新たな VPC、サブネット、ゲートウェイといった、Docker for AWS の実行に必要となるものを新規に作り出します。
もっとも簡単に始められる方法であり、必要となる作業も一番少なくて済みます。

{% comment %}
All you need to do is run the CloudFormation template, answer some questions, and you are good to go.
{% endcomment %}
必要になることと言えば、CloudFormation テンプレートを実行し、質問に答えるだけです。
これだけですぐに始められます。

{% comment %}
#### Install with an Existing VPC
{% endcomment %}
{: #install-with-an-existing-vpc }
#### 既存 VPC を用いたインストール
{% comment %}
If you need to install Docker for AWS with an existing VPC, you need to do a few preliminary steps. See [recommended VPC and Subnet setup](faqs.md#recommended-vpc-and-subnet-setup) for more details.
{% endcomment %}
If you need to install Docker for AWS with an existing VPC, you need to do a few preliminary steps. See [recommended VPC and Subnet setup](faqs.md#recommended-vpc-and-subnet-setup) for more details.

{% comment %}
1. Pick a VPC in a region you want to use.
{% endcomment %}
1. Pick a VPC in a region you want to use.

{% comment %}
2. Make sure the selected VPC is setup with an Internet Gateway, Subnets, and Route Tables.
{% endcomment %}
2. Make sure the selected VPC is setup with an Internet Gateway, Subnets, and Route Tables.

{% comment %}
3. You need to have three different subnets, ideally each in their own availability zone. If you are running in a region with only two Availability Zones, you need to add more than one subnet into one of the availability zones. For production deployments we recommend only deploying to regions that have three or more Availability Zones.
{% endcomment %}
3. You need to have three different subnets, ideally each in their own availability zone. If you are running in a region with only two Availability Zones, you need to add more than one subnet into one of the availability zones. For production deployments we recommend only deploying to regions that have three or more Availability Zones.

{% comment %}
4. When you launch the docker for AWS CloudFormation stack, make sure you use the one for existing VPCs. This template prompts you for the VPC and subnets that you want to use for Docker for AWS.
{% endcomment %}
4. When you launch the docker for AWS CloudFormation stack, make sure you use the one for existing VPCs. This template prompts you for the VPC and subnets that you want to use for Docker for AWS.

{% comment %}
### Prerequisites
{% endcomment %}
{: #prerequisites }
### 前提条件

{% comment %}
- Access to an AWS account with permissions to use CloudFormation and creating the following objects. [Full set of required permissions](iam-permissions.md).
    - EC2 instances + Auto Scaling groups
    - IAM profiles
    - DynamoDB Tables
    - SQS Queue
    - VPC + subnets and security groups
    - ELB
    - CloudWatch Log Group
- SSH key in AWS in the region where you want to deploy (required to access the completed Docker install)
- AWS account that support EC2-VPC (See the [FAQ for details about EC2-Classic](faqs.md))
{% endcomment %}
- Access to an AWS account with permissions to use CloudFormation and creating the following objects. [Full set of required permissions](iam-permissions.md).
    - EC2 instances + Auto Scaling groups
    - IAM profiles
    - DynamoDB Tables
    - SQS Queue
    - VPC + subnets and security groups
    - ELB
    - CloudWatch Log Group
- SSH key in AWS in the region where you want to deploy (required to access the completed Docker install)
- AWS account that support EC2-VPC (See the [FAQ for details about EC2-Classic](faqs.md))

{% comment %}
For more information about adding an SSH key pair to your account, refer
to the [Amazon EC2 Key Pairs
docs](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html).
{% endcomment %}
For more information about adding an SSH key pair to your account, refer
to the [Amazon EC2 Key Pairs
docs](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html).

{% comment %}
The China and US Gov Cloud AWS partitions are not currently supported.
{% endcomment %}
The China and US Gov Cloud AWS partitions are not currently supported.

{% comment %}
### Configuration
{% endcomment %}
{: #configuration }
### 設定

{% comment %}
Docker for AWS is installed with a CloudFormation template that configures
Docker in swarm mode, running on instances backed by custom AMIs. There are two
ways you can deploy Docker for AWS. You can use the AWS Management Console
(browser based), or use the AWS CLI. Both have the following configuration
options.
{% endcomment %}
Docker for AWS is installed with a CloudFormation template that configures
Docker in swarm mode, running on instances backed by custom AMIs. There are two
ways you can deploy Docker for AWS. You can use the AWS Management Console
(browser based), or use the AWS CLI. Both have the following configuration
options.

{% comment %}
#### Configuration options
{% endcomment %}
{: #configuration-options }
#### 設定オプション

##### KeyName

{% comment %}
Pick the SSH key to be used when you SSH into the manager nodes.
{% endcomment %}
Pick the SSH key to be used when you SSH into the manager nodes.

##### InstanceType

{% comment %}
The EC2 instance type for your worker nodes.
{% endcomment %}
The EC2 instance type for your worker nodes.

##### ManagerInstanceType

{% comment %}
The EC2 instance type for your manager nodes. The larger your swarm, the larger
the instance size you should use.
{% endcomment %}
The EC2 instance type for your manager nodes. The larger your swarm, the larger
the instance size you should use.

##### ClusterSize

{% comment %}
The number of workers you want in your swarm (0-1000).
{% endcomment %}
スウォーム内にて必要とするワーカー数。
0 から 1000 まで。

##### ManagerSize

{% comment %}
The number of managers in your swarm. On Docker Engine - Community, you can select either 1,
3 or 5 managers. We only recommend 1 manager for testing and dev setups. There
are no failover guarantees with 1 manager — if the single manager fails the
swarm goes down as well. Additionally, upgrading single-manager swarms is not
currently guaranteed to succeed.
{% endcomment %}
The number of managers in your swarm. On Docker Engine - Community, you can select either 1,
3 or 5 managers. We only recommend 1 manager for testing and dev setups. There
are no failover guarantees with 1 manager — if the single manager fails the
swarm goes down as well. Additionally, upgrading single-manager swarms is not
currently guaranteed to succeed.

{% comment %}
On Docker EE, you can choose to run with 3 or 5 managers.
{% endcomment %}
On Docker EE, you can choose to run with 3 or 5 managers.

{% comment %}
We recommend at least 3 managers, and if you have a lot of workers, you should
use 5 managers.
{% endcomment %}
We recommend at least 3 managers, and if you have a lot of workers, you should
use 5 managers.

##### EnableSystemPrune

{% comment %}
Enable if you want Docker for AWS to automatically cleanup unused space on your
swarm nodes.
{% endcomment %}
Enable if you want Docker for AWS to automatically cleanup unused space on your
swarm nodes.

{% comment %}
When enabled, `docker system prune` runs staggered every day, starting at
1:42AM UTC on both workers and managers. The prune times are staggered slightly
so that not all nodes are pruned at the same time. This limits resource
spikes on the swarm.
{% endcomment %}
When enabled, `docker system prune` runs staggered every day, starting at
1:42AM UTC on both workers and managers. The prune times are staggered slightly
so that not all nodes are pruned at the same time. This limits resource
spikes on the swarm.

{% comment %}
{% endcomment %}
Pruning removes the following:
- All stopped containers
- All volumes not used by at least one container
- All dangling images
- All unused networks

##### EnableCloudWatchLogs

{% comment %}
{% endcomment %}
Enable if you want Docker to send your container logs to CloudWatch. ("yes",
"no") Defaults to yes.

##### WorkerDiskSize

{% comment %}
{% endcomment %}
Size of Workers' ephemeral storage volume in GiB (20 - 1024).

##### WorkerDiskType

{% comment %}
{% endcomment %}
Worker ephemeral storage volume type ("standard", "gp2").

##### ManagerDiskSize

{% comment %}
{% endcomment %}
Size of Manager's ephemeral storage volume in GiB (20 - 1024)

##### ManagerDiskType

{% comment %}
{% endcomment %}
Manager ephemeral storage volume type ("standard", "gp2")

{% comment %}
{% endcomment %}
#### Installing with the AWS Management Console

{% comment %}
{% endcomment %}
The simplest way to use the template is to use one of the
[Quickstart](#docker-community-edition-ce-for-aws) options with the
CloudFormation section of the AWS Management Console.

{% comment %}
{% endcomment %}
![create a stack](img/aws-select-template.png)

{% comment %}
{% endcomment %}
#### Installing with the CLI

{% comment %}
{% endcomment %}
You can also invoke the Docker for AWS CloudFormation template from the AWS CLI:

{% comment %}
{% endcomment %}
Here is an example of how to use the CLI. Make sure you populate all of the
parameters and their values from the above list:

```bash
$ aws cloudformation create-stack --stack-name teststack --template-url <templateurl> --parameters ParameterKey=<keyname>,ParameterValue=<keyvalue> ParameterKey=InstanceType,ParameterValue=t2.micro ParameterKey=ManagerInstanceType,ParameterValue=t2.micro ParameterKey=ClusterSize,ParameterValue=1 .... --capabilities CAPABILITY_IAM
```

{% comment %}
{% endcomment %}
To fully automate installs, you can use the [AWS Cloudformation API](http://docs.aws.amazon.com/AWSCloudFormation/latest/APIReference/Welcome.html).

{% comment %}
{% endcomment %}
### How it works

{% comment %}
{% endcomment %}
Docker for AWS starts with a CloudFormation template that creates everything
that you need from scratch. There are only a few prerequisites that are listed
above.

{% comment %}
{% endcomment %}
The CloudFormation template first creates a new VPC along with subnets and
security groups. After the networking set-up completes, two Auto Scaling Groups
are created, one for the managers and one for the workers, and the configured
capacity setting is applied. Managers start first and create a quorum using
Raft, then the workers start and join the swarm one at a time. At this point,
the swarm is comprised of X number of managers and Y number of workers, and you
can deploy your applications. See the [deployment](deploy.md) docs for your next
steps.

{% comment %}
{% endcomment %}
> To [log into your nodes using SSH](/docker-for-aws/deploy.md#connecting-via-ssh),
> use the `docker` user rather than `root` or `ec2-user`.

{% comment %}
{% endcomment %}
If you increase the number of instances running in your worker Auto Scaling
Group (via the AWS console, or updating the CloudFormation configuration), the
new nodes that start up automatically join the swarm.

{% comment %}
{% endcomment %}
Elastic Load Balancers (ELBs) are set up to help with routing traffic to your
swarm.

{% comment %}
{% endcomment %}
### Logging

{% comment %}
{% endcomment %}
Docker for AWS automatically configures logging to Cloudwatch for containers you
run on Docker for AWS. A Log Group is created for each Docker for AWS install,
and a log stream for each container.

{% comment %}
{% endcomment %}
The `docker logs` and `docker service logs` commands are not supported on Docker
for AWS when using Cloudwatch for logs. Instead, check container logs in
CloudWatch.

{% comment %}
{% endcomment %}
### System containers

{% comment %}
{% endcomment %}
Each node has a few system containers running on it to help run your
swarm cluster. For everything to run smoothly, keep those
containers running, and don't make any changes. If you make any changes, Docker
for AWS does not work correctly.

{% comment %}
{% endcomment %}
### Uninstalling or removing a stack

{% comment %}
{% endcomment %}
To uninstall Docker for AWS, log on to the [AWS
Console](https://aws.amazon.com/){: target="_blank" class="_"}, navigate to
**Management Tools -> CloudFormation -> Actions -> Delete Stack**, and select
the Docker stack you want to remove.

{% comment %}
{% endcomment %}
![uninstall](img/aws-delete-stack.png)

{% comment %}
{% endcomment %}
Stack removal does not remove EBS and EFS volumes created by the cloudstor
volume plugin or the S3 bucket associated with DTR. Those resources must be
removed manually. See the [cloudstor](/docker-for-aws/persistent-data-volumes/#list-or-remove-volumes-created-by-cloudstor)
docs for instructions on removing volumes.
