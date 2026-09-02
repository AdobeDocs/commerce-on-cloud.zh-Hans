---
source-git-commit: 79ac13115bd3f275651a5477f2939c8f00a5a985
workflow-type: tm+mt
source-wordcount: '704'
ht-degree: 0%

---
# 专业服务支持和客户可用性

## 专业服务支持

要在暂存或生产环境中请求并完成Pro服务升级，请执行以下步骤：

1. **若要仅在`Staging`和`Production`环境中安装或更新[服务](https://experienceleague.adobe.com/zh-hans/docs/commerce-on-cloud/user-guide/configure/service/services-yaml)**，请提交[Adobe Commerce支持票证](https://experienceleague.adobe.com/zh-hans/docs/support-resources/adobe-support-tools-guide/adobe-commerce-support/adobe-commerce-help-center-user-guide#submit-ticket)。

   在票证中，指定所需的服务更改，包括更新的`.magento.app.yaml`和`.magento/services.yaml`文件，并记下目标PHP版本。

   PHP版本、编辑器更新、扩展和环境设置都是自助更改。 为兼容PHP版本，Adobe可能需要更新New Relic代理。 查看&#x200B;_应用程序配置_&#x200B;中的[PHP设置](https://experienceleague.adobe.com/zh-hans/docs/commerce-on-cloud/user-guide/configure/app/php-settings)。

   >[!IMPORTANT]
   >
   >在票证表单中选择&#x200B;**[!UICONTROL Environment]**&#x200B;字段时，请使用Adobe的环境命名。 例如，即使您在内部调用该环境&#x200B;**Dev**，请选择“暂存”。 您可以在描述中提及您的内部名称，但[!UICONTROL Environment]字段必须使用Adobe的命名法。

1. **通过Adobe的两部分流程确认升级计划**：首先确认请求的日期和时间，然后支持部门将其提交给基础架构团队进行最终确认。

   生产变更（仅限Pro）需要提前至少两个工作日发出通知，不包括周末。 例如，Cloud Infrastructure团队必须在前一个星期三确认星期一的升级。 预计需求高峰期会有额外的提前期。 为避免延迟，请在窗口之前至少48小时响应初始请求。 在收到最终确认之前，不会将升级视为已计划。

   >[!NOTE]
   >
   >提供UTC格式的维护时段。 暂存升级不会提前计划，通常与请求在同一天完成。
   >
   >RabbitMQ升级后，重新部署环境以重新初始化消息队列。

1. **先在暂存或集成环境中验证升级**，然后再在生产环境中计划升级。

   在服务升级后的重新部署期间，由第三方模块、自定义代码或依赖关系兼容性导致的问题通常会浮出水面。 要一次验证多个服务升级，合理的顺序依次为Valkey或Redis、RabbitMQ、OpenSearch和MariaDB。 这不是必需的序列。 数据库升级具有最高的操作影响，应引起最大的警觉。

   Adobe不保证提前生产维护窗口的确切持续时间，因为时间取决于环境和涉及的服务。 在规划“生产”窗口时，将暂存升级所花费的时间作为实际估计值。

1. 在Adobe完成服务升级后&#x200B;**重新部署环境**，以使更改生效，即使Adobe Commerce应用程序版本未更改也是如此。

   如果升级包括OpenSearch，则还应计划完全重新索引。 Adobe无法保证服务升级的零停机时间，因此请规划一个维护时段，以便有时间重新部署、根据需要重新编制索引，并在重新打开网站之前验证店面和管理员。

## 客户在升级期间的可用性

**在计划的生产升级时段内，您的团队或实施合作伙伴的代表必须在线可用。** 在低流量期间进行计划不会使升级自动进行。 Adobe管理云基础架构升级，但无法验证您的应用程序行为、集成、自定义代码或业务工作流。

可用的代表必须能够：

- **监控**&#x200B;升级期间和升级后的店面交易和关键业务交易。
- **回复** Adobe支持或Cloud Infrastructure团队提出的问题。
- **确认**&#x200B;集成、扩展、自定义、cron作业、队列和其他特定于客户的功能均按预期工作。
- **验证**&#x200B;业务关键型工作流，如签出、目录视图、搜索、登录和订单处理。
- 当升级上下文和日志仍然可用时，立即&#x200B;**报告**&#x200B;意外行为。

>[!TIP]
>
>对于Pro项目，生产中的服务升级还需要提前计划和包含Adobe支持的两部分确认流程。 请参阅[专业服务支持](#pro-services-support)。

### 维护模式

**维护模式不能代替客户可用性。** 维护模式会阻止店面访问，但不会验证应用程序服务、集成、队列、cron作业、结账或其他特定于客户的功能。

如果计划内的工作需要维护模式，请协调与Adobe支持人员一起使用它，然后按照该升级说明操作。 之后，在考虑工作完成之前，请确认店面工作流和关键工作流运行正常。
