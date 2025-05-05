---
title: PrivateLink服务
description: 了解如何使用PrivateLink服务在同一地区的专用云和Adobe Commerce云平台之间建立安全连接。
feature: Cloud, Iaas, Security
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '1609'
ht-degree: 0%

---

# PrivateLink服务

云基础架构上的Adobe Commerce支持与[AWS PrivateLink](https://aws.amazon.com/privatelink/)或[Azure Private Link](https://learn.microsoft.com/en-us/azure/private-link/)服务的集成。 您可以使用PrivateLink在云基础架构环境上的Adobe Commerce与托管在外部系统上的服务和应用程序之间建立安全的私有通信。 必须通过在同一云区域中同一云平台(AWS或Azure)上配置的Virtual Private Cloud (VPC)端点，才能访问Adobe Commerce应用程序和外部系统。

>[!TIP]
>
>PrivateLink最适合用于保护非HTTP集成（如数据库或文件传输）的连接。 如果您计划将应用程序与Adobe Commerce API集成，请参阅如何在&#x200B;_适用于Adobe Developer App Builder的API Mesh_&#x200B;中创建[AdobeAPI Mesh](https://developer.adobe.com/graphql-mesh-gateway/gateway/create-mesh/)。

## 功能和支持

云基础架构项目上适用于Adobe Commerce的PrivateLink服务集成包括以下功能和支持：

- 同一云区域内，同一云平台(AWS或Azure)上的客户虚拟专用云(VPC)和AdobeVPC之间的安全连接。
- 支持在Adobe和客户VPC上提供的端点服务之间进行单向或双向通信。
- 服务启用：

   - 在云基础架构环境上的Adobe Commerce中打开所需的端口
   - 建立客户和AdobeVPC之间的初始连接
   - 启用期间的连接问题疑难解答

## 限制

- 仅在专业生产和暂存环境中支持PrivateLink。 它不适用于本地或集成环境，也不适用于入门项目。
- 无法使用PrivateLink建立SSH连接。 请参阅[启用SSH密钥](secure-connections.md)。
- Adobe Commerce支持不涵盖对AWS PrivateLink初始启用以外的问题进行故障诊断。
- 客户负责与管理自己的VPC相关的成本。
- 由于[Fastly源遮蔽](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/faq/fastly-origin-cloaking-enablement-faq.html?lang=zh-Hans)，无法使用HTTPS协议（端口443）通过Azure专用链接连接到云基础架构上的Adobe Commerce。 此限制不适用于AWS PrivateLink。
- PrivateDNS不可用。

## PrivateLink连接类型

在以下网络图中显示有两种可用的PrivateLink连接类型，用于在您的商店与云环境之外托管的外部系统之间建立安全通信。

![PrivateLink网络图](../../assets/privatelink-architecture-diagram.png)

在云基础架构环境中，选择最适合您的Adobe Commerce的PrivateLink连接类型之一：

- **单向PrivateLink** — 选择此配置以安全地从Adobe Commerce上的云基础架构存储检索数据。
- **双向PrivateLink** — 选择此配置以在云基础架构环境上建立与Adobe Commerce外部系统的安全连接。 双向选项需要两个连接：

   - 客户VPC和AdobeVPC之间的连接
   - AdobeVPC和客户VPC之间的连接

>[!TIP]
>
>请与网络管理员或Cloud平台提供商合作，以获得有关选择PrivateLink连接类型的帮助，或有关VPC设置和管理方面的帮助。 请参阅Cloud Platform PrivateLink文档：[AWS PrivateLink](https://aws.amazon.com/privatelink/)或[Azure Private Link](https://learn.microsoft.com/en-us/azure/private-link/)。

## 请求启用PrivateLink

>[!WARNING]
>
>启用PrivateLink最多需要&#x200B;_5_&#x200B;个工作日。 提供不完整或不准确的信息可能会延迟该过程。

### 先决条件

![检查](../../assets/fix.svg)与Cloud Infrastructure实例上的Adobe Commerce位于同一区域的云帐户(AWS或Azure)。

![检查](../../assets/fix.svg)客户环境中的VPC，它托管要通过PrivateLink连接的服务。 请参阅AWS或Azure文档，以获取有关VPC设置的帮助或联系网络管理员。

![检查](../../assets/fix.svg)对于双向PrivateLink连接，必须先为应用程序或服务创建端点服务配置，并在VPC环境中创建端点，然后才能请求PrivateLink启用。 请参阅[设置双向PrivateLink连接](#set-up-for-bidirectional-privatelink-connections)。

收集启用PrivateLink所需的以下数据：

- **Customer Cloud帐号**(AWS或Azure) — 必须与Adobe Commerce on cloud infrastructure实例位于同一区域
- **云区域** — 提供托管帐户的云区域以进行验证
- **服务和通信端口** -Adobe必须打开端口才能启用VPC之间的服务通信，例如SQL端口3306、SFTP端口2222
- **项目ID** — 提供Adobe Commerce on cloud infrastructure Pro项目ID。 您可以使用以下[Cloud CLI](../dev-tools/cloud-cli-overview.md)命令获取项目ID和其他项目信息： `magento-cloud project:info`
- **连接类型** — 为连接类型指定单向或双向
- **终结点服务** — 对于双向PrivateLink连接，提供Adobe必须连接到的VPC终结点服务的DNS URL，例如： `com.amazonaws.vpce.<cloud-region>.vpce-svc-<service-id>`
- 已授予&#x200B;**终结点服务访问权限** — 若要连接到外部服务，请允许终结点服务访问以下AWS帐户主体： `arn:aws:iam::402592597372:root`

  >[!WARNING]
  >
  >如果未提供对端点服务的访问，则未添加到VPC中该服务的双向PrivateLink连接&#x200B;**未添加**，这会延迟安装。

#### 特定于Azure专用链接启用的其他先决条件

- 提供群集ID；使用SSH登录到远程并使用命令： `cat /etc/platform_cluster`
- 对于要连接到Adobe Commerce Pro群集的外部服务，您需要：

   - 要向新的外部专用端点公开的Pro群集上的端口列表
   - 专用终结点连接的Azure订阅ID列表

- 要将Adobe Commerce Pro群集连接到外部服务，您需要：

   - 目标服务的资源ID列表。 外部专用链接服务ID类似于以下内容：

  ```text
  /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/privateLinkServices/{svcNameID}
  ```

### 启用工作流

以下工作流程概述了PrivateLink与Adobe Commerce在云基础架构上集成的实施过程。

1. **客户**&#x200B;提交请求主题行`PrivateLink support for <company>`启用PrivateLink的支持票证。 在票证中包含启用[&#128279;](#prerequisites)所需的数据。 Adobe使用支持工单在启用过程中协调通信。

1. **Adobe**&#x200B;允许客户帐户访问AdobeVPC中的终结点服务。

   - 更新Adobe端点服务配置以接受从客户AWS或Azure帐户启动的请求。
   - 更新支持票证以提供要连接的AdobeVPC端点的服务名称，例如`com.amazonaws.vpce.<cloud-region>.vpce-svc-<service-id>`。

1. **客户**&#x200B;将Adobe终结点服务添加到其云帐户(AWS或Azure)，这会触发连接请求以Adobe。 有关说明，请参阅云平台文档：

   - 对于AWS，请参阅[接受和拒绝接口端点连接请求]。
   - 对于Azure，请参阅[管理连接请求]。

1. **Adobe**&#x200B;批准连接请求。

1. 连接请求批准后，**客户** [验证其VPC与AdobeVPC之间的连接](#test-vpc-endpoint-service-connection)。

1. 启用双向连接的其他步骤：

   - **Adobe**&#x200B;提供Adobe帐户主体(AWS或Azure帐户的根用户)并请求访问客户VPC端点服务。
   - **客户**&#x200B;允许Adobe访问客户VPC中的端点服务。 这假定Adobe帐户主体具有对`arn:aws:iam::402592597372:root`的访问权限，如先前在授予&#x200B;**必备项的**&#x200B;终结点服务访问权限中所述。

      - 更新客户端点服务配置以接受从Adobe帐户发起的请求。 有关说明，请参阅云平台文档：

         - 对于AWS，请参阅[添加和删除端点服务的权限]。
         - 对于Azure，请参阅[管理专用终结点连接]

      - 向Adobe提供客户VPC的端点服务名称。

   - **Adobe**&#x200B;将客户端点服务添加到Adobe平台帐户(AWS或Azure)，这会触发到客户VPC的连接请求。
   - **客户**&#x200B;批准Adobe的连接请求以完成设置。
   - **客户** [验证来自AdobeVPC的连接](#test-vpc-endpoint-service-connection)。

## 测试VPC端点服务连接

您可以使用Telnet应用程序来测试与VPC端点服务的连接。

**要测试与VPC终结点服务的连接**：

1. 从项目根目录中，**签出**&#x200B;配置为访问PrivateLink终结点服务的暂存或生产环境。

   ```bash
   magento-cloud environment:checkout <environment-id>
   ```

1. 运行以下CURL命令：

   ```bash
   curl -v telnet://<endpoint-service-dns-url>:<port>/
   ```

   示例：

   ```
   $ curl -v telnet://vpce-007ffnb9qkcnjgult-yfhmywqh.vpce-svc-083cqvm2ta3rxqat5v.us-east-1.vpce.amazonaws.com:80 -vvv
   ```

   成功响应示例：

   ```
   * Rebuilt URL to: telnet://vpce-007ffnb9qkcnjgult-yfhmywqh.vpce-svc-083cqvm2ta3rxqat5v.us-east-1.vpce.amazonaws.com:80
   * Connected to vpce-0088d56482571241d-yfhmywqh.vpce-svc-083cqvm2ta3rxqat5v.us-east-1.vpce. amazonaws.com (191.210.82.246) port 80 (#0)
   ```

   失败的响应示例：

   ```
   Failed to connect to vpce-007ffnb9qkcnjgult-yfhmywqh.vpce-svc-083cqvm2ta3rxqat5v.ap-southeast-1.vpce.amazonaws.com port 80: Connection timed out
   * Closing connection 0
   ```

1. 验证该服务是否正在监听VM。

   ```bash
   netstat -na | grep <port>
   ```

1. 检查包流。

   ```bash
   tcpdump -i <ethernet-interface> -tt -nn port <destination-port> and host <source-host>
   ```

   检查以下内部设置以确保配置有效：

   - 端点和端点服务设置
   - 网络负载平衡器(NLB)设置
   - NLB中的目标组，并验证其是否正常
   - 来自每个VM的netcat/curl端点URL（如上所列）

   有关解决连接问题的帮助，请参阅以下文章：

   - [AWS：终结点服务连接疑难解答]
   - [Amazon：正在排查Azure专用链接连接问题]

   如果无法解决错误，请更新Adobe Commerce支持票证以请求建立连接的帮助。

## 更改PrivateLink配置

[提交Adobe Commerce支持票证](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html?lang=zh-Hans#submit-ticket)以更改现有的PrivateLink配置。 例如，您可以请求进行如下更改：

- 在云基础架构Pro生产或暂存环境中从Adobe Commerce中删除PrivateLink连接。
- 更改用于访问Adobe端点服务的客户云平台帐号。
- 在AdobeVPC中添加或删除PrivateLink连接，这些连接可连接到客户VPC环境中可用的其他端点服务。

## 设置双向PrivateLink连接

客户VPC必须具有以下资源来支持双向PrivateLink连接：

- 网络负载平衡器(NLB)
- 允许从客户VPC访问应用程序或服务的端点服务配置
- 允许Adobe连接到VPC中托管的[接口终结点] (AWS)或[私有终结点] (Azure)

如果这些资源在客户VPC中不可用，您必须登录您的Cloud Platform帐户来添加配置。

- Amazon VPC控制台 — `https://console.aws.amazon.com/vpc/`
- Azure门户 — `https://portal.azure.com`

有关PrivateLink设置说明，请参阅云平台文档：

- **AWS PrivateLink文档**
   - [创建网络负载平衡器]
   - [创建终结点服务配置]
   - [创建接口终结点]
   - [接口终结点生命周期]

- **Azure PrivateLink文档**
   - [创建负载平衡器]
   - [Azure专用链接工作流]

<!--Link definitions-->

[接受和拒绝接口端点连接请求]: https://docs.aws.amazon.com/vpc/latest/userguide/accept-reject-endpoint-requests.html
[添加和删除端点服务的权限]: https://docs.aws.amazon.com/vpc/latest/userguide/add-endpoint-service-permissions.html
[Amazon：Azure专用链接连接问题疑难解答]: https://docs.microsoft.com/en-us/azure/private-link/troubleshoot-private-link-connectivity
[AWS：端点服务连接疑难解答]: https://aws.amazon.com/premiumsupport/knowledge-center/connect-endpoint-service-vpc/
[Azure专用链接工作流]: https://docs.microsoft.com/en-us/azure/private-link/private-link-service-overview#workflow
[创建负载平衡器]: https://docs.microsoft.com/en-us/azure/load-balancer/quickstart-load-balancer-standard-public-portal
[创建网络负载平衡器]: https://docs.aws.amazon.com/elasticloadbalancing/latest/network/create-network-load-balancer.html
[创建端点服务配置]: https://docs.aws.amazon.com/vpc/latest/userguide/create-endpoint-service.html
[创建接口端点]: https://docs.aws.amazon.com/vpc/latest/userguide/vpce-interface.html#create-interface-endpoint
[interface endpoint lifecycle]: https://docs.aws.amazon.com/vpc/latest/userguide/vpce-interface.html#vpce-interface-lifecycle
[接口端点]: https://docs.aws.amazon.com/vpc/latest/userguide/vpce-interface.html
[管理专用端点连接]: https://docs.microsoft.com/en-us/azure/private-link/manage-private-endpoint
[管理连接请求]: https://docs.microsoft.com/en-us/azure/private-link/private-link-service-overview#manage-your-connection-requests
[专用端点]: https://docs.microsoft.com/en-us/azure/private-link/private-endpoint-overview
