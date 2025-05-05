---
title: 设置PayPal支付方式
description: 在云基础架构上为Adobe Commerce设置PayPal支付方法。
feature: Cloud, Checkout, Payments
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '669'
ht-degree: 0%

---

# 设置PayPal支付方式

云基础架构上的Adobe Commerce提供了一个载入工具，用于直接通过管理员配置PayPal Express签出帐户。 此工具可用于ECE 2.1.8及更高版本。 为了更好地支持上线和测试PayPal支付方法，您可以为沙盒或生产帐户启用和配置您的PayPal Express结账帐户。

您可以在每个环境中配置沙盒或生产帐户：

* 对于集成和暂存环境，请设置沙盒凭据。
* 对于生产环境，请设置沙盒凭据以进行初始测试，然后为已启动的存储区将凭据替换为实时生产凭据。

## PayPal帐户

虽然最好使用已准备并配置的PayPal商家帐户，但您可以通过“管理员”创建帐户或升级个人帐户。

[!DNL PayPal onboarding]支持与以下帐户连接：

* PayPal企业帐户
* PayPal个人帐户，转换为企业帐户。 如果您现有个人PayPal帐户，则可以使用这些凭据登录，并在完成同步时将此帐户升级到企业帐户。

如果您没有现有的PayPal帐户，请创建一个。 输入新帐户的电子邮件地址。 如果找不到匹配的PayPal帐户，请按照提示创建PayPal Business帐户。 或者，您也可以直接通过[PayPal](https://www.paypal.com/us/webapps/mpp/account-selection)创建帐户。

### PayPal限制

PayPal支持在全球范围内为国家/地区连接PayPal Express Checkout，但以下限制除外：

* 印度和日本（未来的PayPal更新可能支持这些帐户）
* 以色列

对于巴西，您必须拥有现有的PayPal企业帐户才能进行连接。 在此过程中，您无法转换巴西的现有个人PayPal帐户。 如果您需要帐户，请[创建商业PayPal帐户](https://www.paypal.com/us/webapps/mpp/account-selection)。

## 配置PayPal Express签出

要配置PayPal Express签出，请执行以下操作：

1. 访问环境的管理员。
1. 在左侧导航中，选择&#x200B;**商店** > **配置**，然后选择&#x200B;**销售** > **付款方式**。
1. 对于PayPal，选择&#x200B;**配置**。 配置字段显示在“快速结帐”、“通告PayPal点数”以及“基本”和“高级”设置的可展开部分中。
1. 连接您的PayPal帐户。 在连接帐户之前，将禁用要启用的选项。 有关可用和支持的帐户连接和限制的详细信息，请参阅[PayPal帐户](#paypal-account)。

   * 要连接您的PayPal实时帐户，请单击“与PayPal连接”，然后按照提示操作。 任何客户使用实时PayPal购买均可在实时商店中完成并主动向客户收费。
   * 要连接沙盒帐户以进行测试，请单击沙盒凭据并按照提示操作。 任何客户使用Sandbox PayPal购买均可在不主动向客户收费的情况下完成。

1. 配置“快速签出”设置以验证并使用PayPal API：

   * **与PayPal商家帐户关联的电子邮件**（可选）输入与您的PayPal商家帐户关联的电子邮件地址。 此电子邮件区分大小写。
   * **API身份验证方法**&#x200B;作为API签名或API证书。
   * 从您的PayPal帐户捕获的API用户名、密码和签名。
   * **沙盒模式**&#x200B;选择“是”或“否”以指示您输入的凭据是否用于沙盒。 如果您输入了生产凭据，请选择“否”。
   * **API使用代理**&#x200B;如果系统使用代理服务器在Adobe Commerce和PayPal支付系统之间建立连接，请选择“是”或“否”进行设置。 如果为Yes，则输入代理主机和端口。

1. 有关配置帐户的详细信息和步骤，请参阅[PayPal Express签出](https://experienceleague.adobe.com/zh-hans/docs/commerce-admin/stores-sales/payments/paypal/paypal-express-checkout)，从步骤2开始，完成所需的设置。

通过配置和身份验证帐户，您可以在“必需的PayPal设置”下启用和禁用PayPal付款选项：

* **启用此解决方案**&#x200B;通过网站向客户显示PayPal付款方式。
* **启用In-Context签出体验**
* **启用PayPal点数**&#x200B;允许客户在不增加额外费用的情况下进行PayPal点数融资。 PayPal预先支付订单，直接与客户处理所有信用退款。

## PayPal变量

在云基础架构上将PayPal载入工具与Adobe Commerce结合使用时，请将以下变量添加到`magento.app.yaml`文件的`variables:env`部分。

```yaml
# Environment variables
variables:
  env:
    CONFIG__DEFAULT__PAYPAL_ONBOARDING__MIDDLEMAN_DOMAIN: 'payment-broker.magento.com'
```

如果您从2.1.8或更高版本升级到2.2，则仍需要添加此变量。
