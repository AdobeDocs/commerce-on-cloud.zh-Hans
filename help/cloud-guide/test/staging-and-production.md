---
title: 暂存和生产测试
description: 了解如何在暂存和生产环境中测试。
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '1329'
ht-degree: 0%

---

# 暂存和生产测试

将代码、文件和数据成功迁移到暂存或生产环境后，请使用环境URL来测试网站和存储。 下面提供了有关验证日志、测试Fastly配置、用户验收测试(UAT)等的信息。

{{second-staging}}

## 日志文件

如果在部署中遇到错误或在测试时遇到其他问题，请检查日志文件。 日志文件位于`var/log`目录下。

部署日志位于`/var/log/platform/<prodject-ID>/deploy.log`中。 `<project-ID>`的值取决于项目ID以及环境是“暂存”还是“生产”。 例如，项目ID为`yw1unoukjcawe`时，暂存用户为`yw1unoukjcawe_stg`，生产用户为`yw1unoukjcawe`。

在生产或暂存环境中访问日志时，请使用SSH登录到三个节点中的每一个来查找日志。 或者，您可以使用[New Relic日志管理](../monitor/log-management.md)查看和查询所有节点的聚合日志数据。 查看[查看日志](log-locations.md#application-logs)。

## 检查代码库

验证您的代码库是否正确部署到暂存和生产环境。 环境应具有相同的代码库。

## 验证配置设置

通过管理面板检查配置设置，包括基本URL、基本管理URL、多站点设置等。 如果必须进行任何其他更改，请在本地Git分支中完成编辑，然后推送到“集成”、“暂存”和“生产”中的`master`分支。

## 检查Fastly缓存

[配置Fastly](../cdn/fastly-configuration.md)需要注意详细信息：使用正确的Fastly服务ID和Fastly API令牌凭据、上传Fastly VCL代码、更新DNS配置并将SSL/TLS证书应用于您的环境。 完成这些设置任务后，您可以在暂存环境和生产环境中验证Fastly缓存。

**验证Fastly服务配置**：

1. 使用带有`/admin`的URL或[更新的管理员URL](../environment/variables-admin.md#admin-url)登录到暂存和生产管理员。

1. 导航到&#x200B;**商店** > **设置** > **配置** > **高级** > **系统**。 滚动并单击&#x200B;**全页缓存**。

1. 确保&#x200B;**缓存应用程序**&#x200B;值设置为&#x200B;_Fastly CDN_。

1. 测试Fastly凭证。

   - 单击&#x200B;**快速配置**。

   - 验证Fastly服务ID和Fastly API令牌凭据的值。 查看[获取Fastly凭据](/help/cloud-guide/cdn/fastly-configuration.md#get-fastly-credentials)。

   - 单击&#x200B;**测试凭据**。

   >[!WARNING]
   >
   >确保在暂存环境和生产环境中输入正确的Fastly服务ID和API令牌。 Fastly凭据按服务环境创建和映射。 如果在生产环境中输入暂存凭据，则无法上传VCL代码片段，缓存无法正常工作，且缓存配置指向错误的服务器和存储。

**检查Fastly缓存行为**：

1. 使用`dig`命令行实用程序检查标头，以获取有关站点配置的信息。

   您可以通过`dig`命令使用任何URL。 以下示例使用Pro URL：

   - 暂存： `dig https://mcstaging.<your-domain>.com`
   - 生产： `dig https://mcprod.<your-domain>.com`

   有关其他`dig`测试，请在更改DNS](https://docs.fastly.com/en/guides/working-with-domains)之前参阅Fastly的[测试。

1. 使用`cURL`验证响应标头信息。

   ```bash
   curl https://mcstaging.<your-domain>.com -H "host: mcstaging.<your-domain.com>" -k -vo /dev/null -H Fastly-Debug:1
   ```

   有关验证标头的详细信息，请参阅[检查响应标头](../cdn/fastly-troubleshooting.md#check-cache-hit-and-miss-response-headers)。

1. 在您上线后，使用`cURL`查看您的上线网站。

   ```bash
   curl https://<your-domain> -k -vo /dev/null -H Fastly-Debug:1
   ```

## 完成UAT测试

在暂存和生产环境中完成用户验收测试(UAT)。 以下测试是作为商家和客户进行测试的可能任务和区域的快速列表。 您的列表可能更长，并且包含针对自定义模块、扩展和第三方集成的其他测试。 测试时，请使用台式机、笔记本电脑和移动设备。

如果您遇到问题，请保存您的重现步骤、错误消息、奇怪的屏幕捕获和链接。 使用此信息调查和修复集成环境代码和配置或环境设置中的问题。

<table>
<tr>
<td style="width:150px">用户管理</td>
<td>
<ul>
<li>创建和编辑客户帐户，验证电子邮件</li>
<li>为商家创建管理员角色</li>
<li>创建具有特定角色的商家帐户</li>
<li>按角色测试商家帐户访问权限</li>
</ul>
</td>
</tr>
<tr>
<td>目录和产品</td>
<td>
<ul>
<li>创建包含关联产品的目录</li>
<li>为您的店面创建产品，包括所有产品类型：简单、可配置、捆绑</li>
<li>添加产品图像、色板、视频和其他媒体选项</li>
<li>配置价格、折扣、定价规则 </li>
<li>配置高级功能，包括价格范围、特色产品、发布日期</li>
<li>修改库存，并验证每次增加和完成采购时显示的值是否正确和更改</li>
</ul>
</td>
</tr>
<tr>
<td>购物车和结帐</td>
<td>
<ul>
<li>搜索产品并选择筛选选项</li>
<li>将产品从搜索结果、类别页面、产品页面添加到购物车</li>
<li>测试所有产品类型</li>
<li>查看购物车并通过删除或更改数量来修改内容 </li>
<li>通过结帐，根据购物车和产品信息验证订单金额</li>
<li>验证是否已正确计算购物车的税额</li>
<li>使用不同的选项完成购买：添加优惠券、选择送货、输入送货和帐单信息以及付款信息</li>
<li>在结账过程中验证付款网关和选项</li>
<li>检查屏幕通知、客户帐户中列出的订单以及电子邮件通知</li>
<li>测试来宾和客户结帐</li>
</ul>
</td>
</tr>
<tr>
<td>Order Management</td>
<td>
<ul>
<li>为客户创建订单</li>
<li>搜索和查看订单</li>
<li>通过添加和删除产品、更改金额、修改发运和开单信息来修改订单</li>
<li>处理退款</li>
<li>取消订单</li>
<li>应用优惠券代码和折扣</li>
</ul>
</td>
</tr>
<tr>
<td>网站内容</td>
<td>
<ul>
<li>检查所有主题和资产是否正确加载</li>
<li>验证CSS是否正确显示，包括响应式媒体大小</li>
<li>查看条款与条件、退款政策以及其他政策信息</li>
<li>查看联系信息、链接以及有关贵公司的更多信息</li>
<li>搜索产品和内容，检查筛选结果</li>
<li>验证页脚块和顶部导航块</li>
<li>测试404页和维护页</li>
</ul>
</td>
</tr>
<tr>
<td>扩展</td>
<td>
<ul>
<li>验证所有扩展设置，特别是对于任何税务、运输和支付模块（例如：订单发送到仓库和财务管理系统）</li>
<li>测试所有自定义模块和已安装的扩展交互</li>
<li>检查应完成的任何交互（付款、订单、电子邮件通知）的数据</li>
<li>按环境检查扩展的配置</li>
<li>验证模块和扩展之间的依赖关系</li>
<li>以商家和客户身份检查所有操作</li>
</ul>
</td>
</tr>
<tr>
<td>第三方集成</td>
<td>
<ul>
<li>验证数据是否正确地保存在Adobe Commerce中，以及第三方服务是否可以导出、推送或访问这些数据（例如：订单显示在第三方订单管理系统中）</li>
<li>验证每个集成的任何配置和交互</li>
<li>执行源自Adobe Commerce和您的第三方服务的往返测试</li>
<li>验证验证是否已完成</li>
<li>检查任何记录的问题，以更新控制面板中的代码集成或错误消息</li>
</ul>
</td>
</tr>
<tr>
<td>后端测试</td>
<td>
<ul>
<li>测试和清除缓存 </li>
<li>执行重新索引并验证结果</li>
<li>检查cron作业，检查是否存在任何cron_schedule错误</li>
<li>验证并检查是否存在任何shell脚本问题</li>
<li>检查任何记录的问题：应用程序日志、PHP日志、MySQL日志、电子邮件日志</li>
</ul>
</td>
</tr>
</table>

## 载荷和应力测试

在启动之前，最好在暂存环境和生产环境中执行广泛的流量和性能测试。 考虑对前端和后端流程进行性能测试。

在开始测试之前，请输入一个票证，其中包含为您正在测试的环境、您正在使用的工具以及时间范围提供建议的支持人员。 使用结果和信息更新票证以跟踪性能。 完成测试后，添加更新后的结果，并在票证测试中添加注释，其中显示日期和时间戳。

在启动前准备过程中查看[性能工具包](https://github.com/magento/magento2/tree/2.4/setup/performance-toolkit)选项。

要获得最佳结果，请使用以下工具：

- [应用程序性能测试](../environment/variables-post-deploy.md#ttfb_tested_pages) — 通过配置`TTFB_TESTED_PAGES`环境变量测试站点响应时间，以测试应用程序性能。
- [围攻](https://www.joedog.org/siege-home/) — 流量整形和测试软件将您的存储推向极限。 使用可配置的模拟客户端数量点击您的网站。 围困支持基本身份验证、Cookie、HTTP、HTTPS和FTP协议。
- [Jmeter](https://jmeter.apache.org) — 卓越的负载测试，有助于评估尖峰流量的性能，如闪存销售。 创建针对您的网站运行的自定义测试。
- [New Relic](../monitor/new-relic-service.md)（已提供） — 帮助查找导致网站性能变慢的进程和区域，跟踪每个操作（如传输数据、查询、Redis等）的逗留时间。
- [WebPageTest](https://www.webpagetest.org)和[Pingdom](https://www.pingdom.com) — 实时分析不同来源位置的网站页面加载时间。 Pingdom可能需要付费。 WebPageTest是免费工具。

## 功能测试

您可以使用Magento功能测试框架(MFTF)从Cloud Docker环境中完成Adobe Commerce的功能测试。 请参阅&#x200B;_Cloud Docker for Commerce指南_&#x200B;中的[应用程序测试](https://developer.adobe.com/commerce/cloud-tools/docker/test/application-testing/)。

## 设置安全扫描工具

您的站点有免费的安全扫描工具。 若要添加站点并运行工具，请参阅[安全扫描工具](../launch/overview.md#set-up-the-security-scan-tool)。
