---
description: Docker Billing overview
keywords: Docker, pricing, billing, Pro, Team, subscription, plans,
title: Docker の有料プラン概要
redirect_from:
- /docker-hub/billing/faq/
---

{% comment %}
On May 14, 2020, Docker announced a new, per-seat pricing model that aligns with Docker’s [product strategy](https://www.docker.com/blog/docker-strategy-helping-devs-build-and-ship-faster/){: target="_blank" class="_"} to accelerate developer workflows for cloud-native development. The previous private repository/parallel autobuild-based plans have been replaced with new **Pro** and **Team** plans that include unlimited private repositories.
{% endcomment %}
2020年5月14日に Docker は [製品戦略](https://www.docker.com/blog/docker-strategy-helping-devs-build-and-ship-faster/){: target="_blank" class="_"} に連携して、新たにユーザーごとの料金体系を発表しました。
これはクラウドネイティブな開発に向けた開発ワークフローを推進するためです。
それまでのプライベートリポジトリと自動ビルドベースのプランは、**プロ**プランと**チーム**プランとして変更され、無制限のプライベートリポジトリを含むようになりました。

{% comment %}
Existing Docker customers subscribed to a paid plan before May 14, 2020 must upgrade to the new Pro or Team plan by January 31, 2021. Starting with May 14, 2020, new customers who sign up for Docker can choose between the new Free, Pro, and Team plans.
{% endcomment %}
2020年5月14日以前に有料プランを購入していた Docker ユーザーは、2021年1月31日までに新たなプロプランまたはチームプランへアップグレードすることが必要です。
2020年5月14日以降、Docker にサインアップした新規ユーザーは、新たな無料プラン、プロプラン、チームプランを選択できるようになっています。

{% comment %}
## Pricing plans
{% endcomment %}
{: #pricing-plans }
## 料金プラン

{% comment %}
Docker offers pricing plans that are tailored for individual developers and development teams. You can also choose between an **Annual** or a **Monthly** subscription. The Pro and Team plans offered through annual subscription include a discount compared to the price of the same plan offered through monthly subscription.
{% endcomment %}
Docker では個人開発者向けや開発チーム向けの料金プランを用意しています。
さらに支払い方法として **Annual**（年額）か **Monthly**（月額）のサブスクリプションが選択できます。
プロプランとチームプランにおいて年額のサブスクリプションを選ぶと、同じプランで月額とした場合に比べて料金値引きがあります。

{% comment %}
**For individuals:**
{% endcomment %}
**個人利用者向け**

{% comment %}
The **Pro** plan includes unlimited public repositories, unlimited private repositories, unlimited [collaborators](../repos.md#collaborators-and-their-role) for public repositories, one [service account](../repos.md#service-accounts) for private repositories, and 2 parallel builds, starting at $5 per month with the annual subscription.
{% endcomment %}
**プロ**プランには以下が含まれます。
無制限のパブリックリポジトリ、無制限のプライベートリポジトリ、パブリックリポジトリにおける無制限の [協力者](../repos.md#collaborators-and-their-role)、プライベートリポジトリにおける 1 つの [サービスアカウント](../repos.md#service-accounts)、2 つの同時並行ビルド。
年額のサブスクリプションは、月額 5 ドルから用意されています。

{% comment %}
> **Note**
>
> Pro plans allow one service account for private repositories. For more information, see [service accounts](../repos.md#service-accounts).
{% endcomment %}
> **メモ**
>
> プロプランでは、プライベートリポジトリに対してサービスアカウントを 1 つだけ利用可能です。
> 詳しくは [サービスアカウント](../repos.md#service-accounts) を参照してください。

{% comment %}
The **Free** plan includes unlimited public repositories and unlimited collaborators for public repositories and zero collaborators for private repositories at no cost per month.
{% endcomment %}
**無料**プランでは、無制限のパブリックリポジトリ、パブリックリポジトリに対する無制限の協力者が利用できますが、プライベートリポジトリにおいての協力者は利用できません。
月額は無料です。

{% comment %}
**For development teams:**
{% endcomment %}
**開発チーム向け**

{% comment %}
The **Team** plan includes unlimited public and unlimited private repositories starting at $25 per month for the first 5 users and $7 per month for each user thereafter with the annual subscription. In addition, the Team plan offers 3 parallel builds, advanced collaboration and management tools, including organization and team management with role-based access controls for the whole team.
{% endcomment %}
**チーム**プランでは、無制限のパブリックリポジトリ、無制限のプライベートリポジトリが利用できます。
5 ユーザーに対して月額 25 ドルから、また年額サブスクリプションではその後、各ユーザーに対して月額 7 ドルから用意されています。
またチームプランでは 3 つの同時並行ビルド、高度な共同および管理ツール、チーム全体に向けたロールベースのアクセス制御による組織およびチーム管理が利用できます。

{% comment %}
The **Free Team** plan includes unlimited public repositories at no cost per month. This plan also offers advanced collaboration and management tools, including organization and team management with role-based access controls which are limited to 1 team and 3 team members.
{% endcomment %}
**無料チーム**プランでは、無制限のパブリックリポジトリが月額無料で利用できます。
このプランでは、高度な共同および管理ツールが提供され、ロールベースのアクセス制御による組織およびチーム管理を 1 チームおよび 3 チームメンバーに限って利用できます。

{% comment %}
For detailed information about the features available in each plan, see [Docker Pricing](https://www.docker.com/pricing){: target="_blank" class="_"}.
{% endcomment %}
各プランにおいて利用可能な機能の詳細は [Docker の料金体系](https://www.docker.com/pricing){: target="_blank" class="_"} を参照してください。

{% comment %}
For frequently asked questions about pricing, see [Docker pricing FAQ](https://www.docker.com/pricing/faq){: target="_blank" class="_"}.
{% endcomment %}
料金に関してよくたずねられる質問が [Docker の料金に関する FAQ](https://www.docker.com/pricing/faq){: target="_blank" class="_"} にあります。

{% comment %}
### Purchase a Pro plan
{% endcomment %}
{: #purchase-a-pro-plan }
### プロプランの購入

{% comment %}
The following section contains information on how to purchase a Pro plan for new customers. If you are already subscribed to a legacy plan and would like to upgrade to Pro, see [Upgrade to Pro from a legacy plan](upgrade.md#upgrade-to-a-pro-plan).
{% endcomment %}
この節では、プロプランを新たに購入する方法について説明します。
すでに従来のプランを購入していて、そこからプロプランにアップグレードしたい場合は [従来プランからプロプランへのアップグレード](upgrade.md#upgrade-to-a-pro-plan) を参照してください。

{% comment %}
To purchase a Pro plan:
{% endcomment %}
プロプランの購入は以下のようにします。

{% comment %}
1. Log into your [Docker Hub](https://hub.docker.com){: target="_blank" class="_"} account.
{% endcomment %}
1. [Docker Hub](https://hub.docker.com){: target="_blank" class="_"} アカウントでログインします。

{% comment %}
2. The Docker Hub **Pricing** page displays information about the new pricing plans.
{% endcomment %}
2. Docker Hub の **Pricing**（料金）ページにて、新たな料金プランの情報を確認します。

{% comment %}
3. Review the plan details in the **Pro** section and click **Buy Now**.
{% endcomment %}
3. **Pro** セクションにあるプランの詳細を確認して **Buy Now**（今すぐ購入）をクリックします。

{% comment %}
4. The **Payment Method** page displays the cost for the annual plan by default. If you prefer to opt for the monthly plan, choose **pay monthly**.
{% endcomment %}
4. **Payment Method**（お支払い方法）のページにおいて、デフォルトの年額プランの料金を確認します。
   月額でよければ **pay monthly**（月払い）を選択します。

{% comment %}
5. Enter the card details and click **Purchase**.
{% endcomment %}
5. カード情報を入力して **Purchase**（購入）をクリックします。

{% comment %}
6. Verify your email address.
{% endcomment %}
6. メールアドレスを確認します。

{% comment %}
7. Open the drop-down menu next to your username in the top-right corner and select **Billing**.
{% endcomment %}
7. 画面右上のユーザー名の隣りにあるドロップダウンメニューを開いて、**Billing** を選びます。

    {% comment %}
    The **Billing** page displays information about your new Pro plan and the total cost.
    {% endcomment %}
    Billing ページには、新たなプロプランとその支払い合計額が表示されています。

{% comment %}
### Purchase a Team plan
{% endcomment %}
{: #purchase-a-team-plan }
### チームプランの購入

{% comment %}
The following section contains information on how to purchase a Team plan for new organizations. If you are already subscribed to a legacy plan and would like to upgrade to the Team plan, see [Upgrade to Team from a legacy plan](upgrade.md#upgrade-to-a-team-plan).
{% endcomment %}
この節では、新たな組織に対してチームプランを購入する方法について説明します。
すでに従来のプランを購入していて、そこからチームプランにアップグレードしたい場合は [従来プランからチームプランへのアップグレード](upgrade.md#upgrade-to-a-team-plan) を参照してください。

{% comment %}
To purchase a Team plan:
{% endcomment %}
チームプランの購入は以下のようにします。

{% comment %}
1. Log into your [Docker Hub](https://hub.docker.com){: target="_blank" class="_"} account.
{% endcomment %}
1. [Docker Hub](https://hub.docker.com){: target="_blank" class="_"} アカウントにログインします。

{% comment %}
2. The Docker Hub **Pricing** page displays information about the new pricing plans.
{% endcomment %}
2. Docker Hub の **Pricing**（料金）ページにて、新たな料金プランの情報を確認します。

{% comment %}
3. Review the plan details in the **Team** section and click **Buy Now**.
{% endcomment %}
3. **Team** セクションにあるプランの詳細を確認して **Buy Now**（今すぐ購入）をクリックします。

{% comment %}
4. Enter a name for your new organization and add a company name.
{% endcomment %}
4. 新たな組織名と会社名を入力します。

{% comment %}
5. The **Organization size** page displays the cost for the annual plan by default. If you prefer to change this, select **Pay Monthly**.
{% endcomment %}
5. **Organization size**（組織のサイズ）ページにて、 デフォルトの年額プランの料金を確認します。
   これでよければ **Pay Monthly**（月払い）を実行します。

{% comment %}
6. Specify the number of users you’d like to add to your organization and click **Continue to Payment**.
{% endcomment %}
6. 組織に加えるユーザー数を指定して **Continue to Payment**（購入操作を続ける）をクリックします。

{% comment %}
6. On the **Payment Method** page, enter the card details and click **Purchase**.
{% endcomment %}
6. **Payment Method**（お支払い方法）のページにおいて、カード情報を入力して **Purchase**（購入）をクリックします。

{% comment %}
7. Verify your email address.
{% endcomment %}
7. メールアドレスを確認します。

{% comment %}
8. Navigate to Organizations from the menu at the top of the page, choose your organization, and select **Billing**. The Billing tab displays information about your new Team plan and the total cost.
{% endcomment %}
8. ページ上段にあるメニューから Organizations にアクセスし、自身の組織を選びます。
   そして **Billing** を選択します。
   Billing タブには、新たなチームプランとその支払い合計額が表示されています。
