---
title: 存储配置的最佳实践
description: 了解在云基础架构上在Adobe Commerce上配置存储的最佳实践。
feature: Cloud, Best Practices
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '1087'
ht-degree: 0%

---

# 存储配置的最佳实践

有关配置您的商店、站点和网站的详细信息，您可能需要查看[Adobe Commerce用户指南](https://experienceleague.adobe.com/docs/commerce-admin/user-guides/home.html?lang=zh-Hans)。 此页面提供了用于配置商店、网站等的最佳实践、有用信息和指南，以及随着时间推移和在不同版本之间发布的其他内容。

## 营销活动和促销

此信息对于云基础架构2.1.X和2.2.X上的Adobe Commerce很有帮助。

要创建营销活动和促销活动，请在[内容暂存](https://experienceleague.adobe.com/docs/commerce-admin/content-design/staging/content-staging.html?lang=zh-Hans)中创建选项和设置。 利用此功能，您可以先创建和预览营销活动，然后再将其公开发布给客户销售。 以下信息提供了有用的信息。 有关确切说明，请参阅链接的《Adobe Commerce用户指南》内容。

_促销活动_&#x200B;是季节性销售、新产品系列等的营销活动。 每个促销活动均可包括自定义主题、内容块、用于控制和显示内容的构件以及带价格规则的相关促销活动。 由于活动的广泛性质，您可以通过内容暂存创建具有开始和结束日期的营销活动。

_促销活动_&#x200B;提供折扣、一次性优惠、优惠券、首次购买奖励等等。 您将这些促销活动创建为&#x200B;_价格规则_，该规则设置了促销条款、折扣和选项以鼓励客户购买。 您可以在[购物车](https://experienceleague.adobe.com/docs/commerce-admin/marketing/promotions/cart-rules/price-rules-cart.html?lang=zh-Hans)或[目录](https://experienceleague.adobe.com/docs/commerce-admin/marketing/promotions/catalog-rules/price-rules-catalog.html?lang=zh-Hans)上创建价格规则，并添加横幅、奖励积分等更多选项。 您可以计划促销活动，并为新产品线或季节性销售等主要活动应用价格规则。

以下提示可帮助创建、更新和管理促销和活动：

* 促销可以是营销策划的一部分。 促销活动不能包含在促销活动中。 您可以将促销列表作为价格规则使用，以使用多次且具有多个促销活动。
* 创建促销活动时，它始终会创建一个不活动的初始促销活动。 它有开始日期但没有结束日期。 您可以忽略此初始营销活动。 您可以使用正确的活动计划安排新的更新，并使其处于活动状态。
* 营销策划具有开始和结束日期，而不是促销活动。 创建促销时显示的计划程序不会配置促销的开始日期和结束日期。 通过它，您可以在促销活动的配置页面上为此促销活动安排促销活动。
* 不能直接在暂存内容中进行编辑。 如果必须在营销活动中编辑设置和选项，请编辑原始或复制副本并在暂存内容中推送以覆盖。 例如，如果您没有促销活动的结束日期，则必须编辑原始日期并推送以更新。

## 高级定价和暂存内容

此信息对于云基础架构2.1.X和2.2.X上的Adobe Commerce很有帮助。

通常，您可以通过Admin的&#x200B;**Products** > **Catalogs**&#x200B;区域为产品设置[高级定价](https://experienceleague.adobe.com/docs/commerce-admin/catalog/products/pricing/pricing-advanced.html?lang=zh-Hans)。 利用暂存内容，完成一些额外的步骤以将定价添加到促销和促销活动。

要编辑高级定价并更新内容暂存，请执行以下操作：

1. 登录到管理员。
1. 导航到&#x200B;**产品** > **目录**&#x200B;并选择产品并进行编辑。
1. 在“定价”选项卡中，选择&#x200B;**高级定价**。 编辑价格并保存更改。
1. 在页面顶部，单击&#x200B;**计划新更新**。
1. 为产品创建促销活动。
1. 填写促销信息。 对于调度程序，输入开始和结束日期和时间。
1. 保存促销活动。 将创建一个不活动的初始营销活动。
1. 您可以预览以复查促销活动的特殊价格、促销名称、正常价格和计划日期范围。

有关其他步骤，您可以继续按照[计划更改目录价格规则](https://experienceleague.adobe.com/docs/commerce-admin/marketing/promotions/catalog-rules/price-rule-catalog-scheduled-changes.html?lang=zh-Hans)的说明进行操作。 单击&#x200B;**下一步**&#x200B;逐步完成这些步骤。

## 价格规则

价格规则可以包括逻辑和条件，就像营销想象力一样无限。 一些受欢迎的示例包括“买一送一”、“买一送一”50%折扣、超过100美元的订单有25美元折扣等等。

要创建价格规则，请参阅[Adobe Commerce用户指南](https://experienceleague.adobe.com/docs/commerce-admin/marketing/promotions/catalog-rules/price-rules-catalog-create.html?lang=zh-Hans)。

下面提供了一个为仅限于第一张订单的折扣创建价格规则的示例。 对于此折扣，您需要：

* 创建一个价格规则，该规则包含[客户区段](https://experienceleague.adobe.com/zh-hans/docs/commerce-admin/customers/segments/customer-segment-price-rule)，其条件为：订单总数小于1
* 将此客户区段作为条件添加到购物车规则
* 可选 — 添加条件和规则以将折扣应用于特定的SKU或产品类别以进行重点购买

这可确保未购买的新客户或现有客户仅在第一笔订单上获得折扣。 您可以创建横幅并为首次购买折扣发送电子邮件促销活动。

## 商店视图

您可以在云基础架构上通过一次实施Adobe Commerce来设置和运行多个商店。 请参阅[设置多个网站或商店](multiple-sites.md)。

对于不相互交互的商店，您可以创建多个&#x200B;_网站_。 每个网站都有特定的文章、客户数据、结账和购物车，没有与Adobe Commerce中的其他网站共享。

每个网站可以包括一个或多个&#x200B;_商店_，其中包含不同的类别和文章、共享的客户数据、结帐和购物车。 对于这些商店，客户只需一次注册，即可通过一次结账在不同的产品目录中购物。

此外，您还可以为不同的语言、布局和设计创建&#x200B;_存储视图_。 在共享文章、客户数据、结帐和购物车时，每个视图可以具有单独的域、品牌和语言。

以下是一些可以更好地解释的示例：

* 单个网站，带有一个商店和两个视图，用于英语和西班牙语区域设置。 共享所有文章数据、客户、结帐和购物车。

  ![存储示例1](../../assets/example-store1.png)

* 开设女装商店的单一网站包括两种视图：一种显示英语，另一种显示西班牙语。 这家童装店只有一家英文店。 共享所有文章数据、客户、结帐和购物车。 所述商店可以具有不同的域和主题。

  ![存储示例2](../../assets/example-store2.png)

* 两个网站，一个用于服装，另一个用于拥有不同目录和单独文章、客户数据和购物车的家庭装饰。 每个网站都可以有多个商店和视图，以便仅在该网站中共享文章、客户数据、结帐和购物车。

  ![存储示例3](../../assets/example-store3.png)

>[!WARNING]
>
>目录数据会随着您增加网站和商店的数量而扩展。 根据您的项目架构，额外的存储可能会导致非缓存目录页面的索引过程更长，响应速度更慢。 Adobe建议您密切监控站点性能。
