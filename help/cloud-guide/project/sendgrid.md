---
title: SendGrid电子邮件服务
description: 了解云基础架构上适用于Adobe Commerce的SendGrid电子邮件服务，以及如何测试您的DNS配置。
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '1317'
ht-degree: 0%

---

# SendGrid电子邮件服务

SendGrid简单邮件传输协议(SMTP)代理服务提供出站电子邮件身份验证和信誉监视服务，包括支持：

* 所有出站事务性电子邮件
* 专用IP地址
* 域注册、用于电子邮件域验证的域密钥识别邮件(DKIM)签名（仅适用于专业测试环境和生产环境）
* 自定义域注册（仅适用于Pro）
* 简易和专业集成环境的自动化集成。 专业生产和暂存环境需要在基础架构即服务(IaaS)硬件配置过程中进行手动配置和配置

SendGrid SMTP代理不能用作接收传入电子邮件的通用电子邮件服务器，也不能用于电子邮件营销活动。

>[!TIP]
>
>确保您已转到商店>配置>常规，在管理员中配置相应的商店电子邮件地址，以避免投放能力和域验证出现问题。 必须取消选中&#x200B;**[!UICONTROL Use Default]**，并将默认值替换为您拥有的域。 通过Sendgrid发送电子邮件时，不应将公共/共享域电子邮件服务(如gmail.com和outlook.com)配置为发件人电子邮件地址。

## 启用或禁用电子邮件

您可以从Cloud Console或命令行为每个环境启用或禁用传出电子邮件。

默认情况下，在Pro生产和暂存环境中启用传出电子邮件。 但是，在您通过[命令行](outgoing-emails.md#enable-emails-in-the-cli)或[Cloud Console](outgoing-emails.md#enable-emails-in-the-cloud-console)设置`enable_smtp`属性之前，[!UICONTROL Outgoing emails]可能在环境设置中显示为禁用。 您可以为集成和暂存环境启用传出电子邮件，以发送双重身份验证或重置云项目用户的密码电子邮件。 请参阅[配置电子邮件以进行测试](outgoing-emails.md)。

如果必须在专业生产或暂存环境中禁用或重新启用传出电子邮件，则可以提交[Adobe Commerce支持票证](https://experienceleague.adobe.com/zh-hans/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide)。

>[!TIP]
>
>通过[命令行](outgoing-emails.md#enable-emails-in-the-cli)更新[!UICONTROL enable_smtp]属性值也会在[云控制台](outgoing-emails.md#enable-emails-in-the-cloud-console)上更改此环境的[!UICONTROL Enable outgoing emails]设置值。

## SendGrid仪表板

所有云项目都在中央帐户下管理，因此仅支持人员有权访问SendGrid功能板。 SendGrid不提供子帐户限制功能。

要查看活动日志中的投放状态或已退回、被拒绝或阻止电子邮件地址的列表，请[提交Adobe Commerce支持票证](https://experienceleague.adobe.com/zh-hans/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide#submit-ticket)。 支持团队&#x200B;**无法**&#x200B;检索超过30天的活动日志。

如有可能，请在请求中包含以下信息：

* 受影响的电子邮件地址
* 相关时间范围（仅限过去30天内）
* 电子邮件的主题

## 域密钥识别邮件(DKIM)

DKIM是一种电子邮件身份验证技术，它使Internet服务提供商(ISP)能够识别合法和虚假发件人地址，这种技术通常用于网络钓鱼和电子邮件诈骗。 DKIM依赖于管理DNS记录的域所有者。 使用DKIM时，发件人服务器使用私钥对邮件进行签名。 此外，域所有者还将一个DKIM记录（已修改的`TXT`记录）添加到发件人域的DNS记录中。 此`TXT`记录包含收件人邮件服务器用于验证邮件签名的公钥。 通过DKIM公钥加密过程，收件人可以验证发件人的真实性。 请参阅[DKIM记录解释](https://docs.sendgrid.com/ui/account-and-settings/dkim-records)。

>[!WARNING]
>
>SendGrid DKIM签名和域身份验证支持仅适用于专业项目的生产和暂存环境，不适用于所有入门环境。 因此，出站事务型电子邮件可能会被垃圾邮件过滤器标记。 使用DKIM可提高作为经过身份验证的电子邮件发件人的投放率。 为了提高邮件投放率，您可以从Starter升级到Pro，或者使用您自己的SMTP服务器或电子邮件投放服务提供商。 请参阅&#x200B;_管理员系统指南_&#x200B;中的[配置电子邮件连接](https://experienceleague.adobe.com/zh-hans/docs/commerce-admin/systems/communications/email-communications)。

### 发件人和域身份验证

要让SendGrid代表您从专业生产或暂存环境发送事务性电子邮件，您必须将DNS设置配置为包含三个SendGrid子域DNS条目。 每个SendGrid帐户都分配有一个唯一的`TXT`记录，用于验证出站电子邮件。

>[!TIP]
>
>请确保在&#x200B;**[!UICONTROL Stores > Configuration > General > Store Email Addresses]**&#x200B;中使用&#x200B;**[!UICONTROL S的适当域配置&lbrace;store电子邮件地址]**。 对发件人的电子邮件地址执行域身份验证。 如果配置了默认设置(`example.com`)，来自`example.com`的电子邮件将被Sendgrid阻止。

**要启用域身份验证**：

1. 提交[支持票证](https://experienceleague.adobe.com/zh-hans/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide#submit-ticket)以请求为特定域启用DKIM（仅限&#x200B;**专业暂存和生产环境**）。
1. 使用在支持票证中提供给您的`TXT`和`CNAME`记录更新您的DNS配置。

**帐户ID为**&#x200B;的示例`TXT`记录：

```text
v=spf1 include:u17504801.wl.sendgrid.net -all
```

**示例`CNAME`记录**：

| 域 | 指向 | 记录类型 |
| ---------- | ---------- | ------------- |
| em.emaildomain.com | uxxxxxx.wl.sendgrid.net | CNAME |
| s1._domainkey.emaildomain.com | s1.domainkey.uxxxxxx.wl.sendgrid.net | CNAME |
| s2。_domainkey.emaildomain.com | s2.domainkey.uxxxxxx.wl.sendgrid.net | CNAME |

### DKIM签名和自动安全

设置经过身份验证的域时，您可以在自动和手动安全之间进行选择。 如果您选择自动安全，SendGrid会自动管理您的DKIM和SPF记录。 当您将新的专用发送IP地址添加到帐户时，SendGrid会立即更新您的DNS设置和DKIM签名。 如果关闭自动安全功能，则您有责任在更改发送域时更新DKIM签名。

**已启用自动安全示例**：

```text
subdomain.mydomain.com. | CNAME | uxxxxxx.wl.sendgrid.net
s1._domainkey.mydomain.com. | CNAME | s1.domainkey.uxxxxxx.wl.sendgrid.net
s2._domainkey.mydomain.com. | CNAME | s2.domainkey.uxxxxxx.wl.sendgrid.net
```

**禁用自动安全示例**：

```text
me12345.mydomain.com | MX | mx.sendgrid.net
me12345.mydomain.com | TXT | v=spf1 include:sendgrid.net ~all
m1._mydomain.com | TXT | k=rsa; t=s; p=<public-key>
```

设置域身份验证后，SendGrid会自动为您处理Security Policy Framework (SPF)和DKIM记录。 在SendGrid提供要添加到DNS记录的`CNAME`记录后，您可以添加专用IP地址并进行其他帐户更新，而无需手动管理您的SPF记录。 查看[自动安全性和您的DKIM签名](https://docs.sendgrid.com/ui/account-and-settings/dkim-records#automated-security-and-your-dkim-signature)。

要测试DNS配置，请执行以下操作：

```
dig CNAME em.domain_name
dig CNAME s1._domainkey.domain_name
dig CNAME s2._domainkey.domain_name
```

## 事务性电子邮件阈值

事务性电子邮件阈值是指在特定时间段内可从Pro环境发送的事务性电子邮件消息数，例如每月从非生产环境发送12,000封电子邮件。 阈值旨在防止发送垃圾邮件并防止可能对您的电子邮件信誉造成损害。

只要发件人信誉得分超过95%，就可以在“生产”环境中发送的电子邮件数量就没有任何硬性限制。 信誉受退回或拒绝的电子邮件数量以及基于DNS的垃圾邮件注册是否已将您的域标记为潜在垃圾邮件来源的影响。 在&#x200B;_Commerce支持知识库_&#x200B;中，查看Adobe Commerce[&#128279;](https://experienceleague.adobe.com/zh-hans/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/emails-not-being-sent-sendgrid-credits-exceeded)上超过SendGrid积分时未发送的电子邮件。

**若要检查是否超过最大积分**：

1. 在本地工作站上，转到您的项目目录。

1. 使用SSH登录到远程环境。

   ```bash
   magento-cloud ssh
   ```

1. 在`/var/log/mail.log`中检查`authentication failed : Maxium credits exceeded`条目。

   如果您看到任何`authentication failed`个日志条目，并且&#x200B;**电子邮件发送信誉**&#x200B;至少为95，您可以[提交Adobe Commerce支持票证](https://experienceleague.adobe.com/zh-hans/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide#submit-ticket)以请求增加信用额度。

### 电子邮件发送信誉

电子邮件发送信誉是由Internet服务提供商(ISP)为发送电子邮件的公司分配的分数。 分数越高，ISP将消息发送到收件人收件箱的可能性就越大。 如果分数低于某个级别，ISP可能会将邮件路由到收件人的垃圾邮件文件夹，甚至完全拒绝邮件。 信誉得分由多个因素决定，例如IP地址相对于其他IP地址的30天平均排名以及垃圾邮件投诉率。 查看[8种检查电子邮件发送信誉的方法](https://sendgrid.com/en-us/blog/5-ways-check-sending-reputation)。

### 电子邮件禁止显示列表

电子邮件禁止列表是一组收件人列表，如果发送电子邮件会损害您的发送信誉和投放率，则不应向其发送电子邮件。 CAN-SPAM Act要求确保电子邮件发件人能够选择退出已取消订阅或将电子邮件标记为垃圾邮件的收件人。 禁止列表还会收集退回、被阻止或无效的电子邮件。

若要首先防止将电子邮件发送到垃圾邮件文件夹，请遵循Sendgrid的最佳实践文章： [为什么我的电子邮件会进入垃圾邮件？](https://sendgrid.com/en-us/blog/10-tips-to-keep-email-out-of-the-spam-folder)。

如果某些收件人没有收到您的电子邮件，您可以[提交Adobe Commerce支持票证](https://experienceleague.adobe.com/zh-hans/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide#submit-ticket)以请求查看禁止显示列表并在必要时删除收件人。

有关更多详细信息，请参阅[什么是禁止显示列表？](https://sendgrid.com/en-us/blog/what-is-a-suppression-list)
