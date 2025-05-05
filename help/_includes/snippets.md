---
source-git-commit: 0d9d3d64cd0ad4792824992af354653f61e4388d
workflow-type: tm+mt
source-wordcount: '907'
ht-degree: 0%

---
# 云片段

## Elasticsearch警告 {#elasticsearch-support}

>[!WARNING]
>
>云基础架构上的Adobe Commerce不支持Elasticsearch7.11及更高版本。 Adobe Commerce版本2.3.7-p3、2.4.3-p2、2.4.4及更高版本支持OpenSearch服务。 内部部署安装继续支持Elasticsearch。

## 增强的集成 {#enhanced-integration-envs}

>[!NOTE]
>
>在2020年6月5日之前配置的项目具有多个较小的集成环境。 如果您需要更大的集成环境来进行测试和开发，请请求升级到增强集成环境。 有关详细信息，请参阅&#x200B;_Adobe Commerce帮助中心_&#x200B;中的[集成环境请求](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/announcements/commerce-announcements/integration-environment-enhancement-request-pro-and-starter.html?lang=zh-Hans)文章。

## 合并选项 {#merge-options}

默认情况下，部署进程将覆盖`env.php`文件中的所有设置；但是，您可以选择为服务配置合并一个或多个值而不覆盖所有值。

将`_merge`选项设置为以下选项之一：

- `true`—**将配置的服务值与环境变量值合并**。
- `false`—**使用环境变量值覆盖配置的服务值**。

## 专用存储库 {#private-repository}

>[!NOTE]
>
>Adobe强烈建议为Adobe Commerce on cloud infrastructure项目使用专用存储库来保护任何专有信息或开发工作，例如扩展和敏感配置。

## 专业自助警告 {#pro-self-service-warning}

>[!WARNING]
>
>某些&#x200B;**Pro项目**&#x200B;需要支持票证来更新`routes.yaml`文件中的路由配置和`.magento.app.yaml`文件中的cron配置。 Adobe建议在集成环境中更新和测试YAML配置文件，然后将更改部署到暂存环境。 如果重新部署后更改未应用于暂存站点，并且日志中没有相关错误消息，则您&#x200B;**必须** [提交描述尝试的配置更改的Adobe Commerce支持票证](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html?lang=zh-Hans#submit-ticket)。 在票证中包含任何更新的YAML配置文件。

## 专业服务支持 {#pro-update-service}

>[!TIP]
>
>对于Pro项目，您必须[提交Adobe Commerce支持票证](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html?lang=zh-Hans#submit-ticket)才能仅在`Staging`和`Production`环境中安装或更新[服务](https://experienceleague.adobe.com/docs/commerce-on-cloud/user-guide/configure/service/services-yaml.html?lang=zh-Hans)。
>
>指示所需的服务更改，包括更新的`.magento.app.yaml`和`services.yaml`文件，并在票证中声明PHP版本。 有关对PHP版本、扩展或环境设置的自助更改，请参阅&#x200B;_应用程序配置_&#x200B;中的[PHP设置](https://experienceleague.adobe.com/docs/commerce-on-cloud/user-guide/configure/app/php-settings.html?lang=zh-Hans)。
>
>对实时生产环境的更改（仅限&#x200B;**Pro**），至少需要48小时的通知。 这使云基础架构团队有充足的时间来调配资源并进行安全升级。 当基础架构团队确认请求并计划升级（不包括周末）时，通知期即开始。 例如，要在星期一完成服务升级，必须在星期三之前收到计划升级的确认。 在需求高峰期，处理您的请求可能需要更多时间。

## 专业备份 {#pro-backups}

>[!TIP]
>
>在Pro暂存和生产环境中，您必须[提交Adobe Commerce支持票证](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html?lang=zh-Hans#submit-ticket)以检索票证中注明日期、时间和时区的特定备份。
>
>Adobe **不**&#x200B;从自动备份还原任何环境。 请参阅[从暂存或生产还原数据库快照](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/how-to/restore-a-db-snapshot-from-staging-or-production.html?lang=zh-Hans)，以帮助选择还原暂存或生产快照的方法。

## 重新部署警告 {#redeploy-warning}

>[!WARNING]
>
>当您执行合并、推送或同步环境时，或者当您触发手动重新部署（期间的[!DNL Commerce]应用程序处于维护模式）时，部署过程将开始。 对于生产环境，Adobe建议在非高峰时间完成此工作，以避免服务中断。

## 工艺路线占位符 {#route-placeholder}

>[!NOTE]
>
>以下路由配置示例使用带有占位符的路由模板。 `{default}`占位符表示为您的站点配置的默认域。 如果您的项目有多个域，请使用`{all}`占位符为默认域和所有别名配置路由。 请参阅[配置路由](/help/cloud-guide/routes/routes-yaml.md)。

## SCD计时 {#scd-timing-warning}

>[!WARNING]
>
>如果在部署后应用程序中遇到静态内容文件问题（例如缺少自定义主题文件），请将最大预期执行时间增加到900秒或更高。

## 基于方案的部署 {#scenarios}

>[!NOTE]
>
>使用[!DNL ECE-Tools] 2002.1.0及更高版本，您可以使用基于方案的部署功能在云基础架构项目上自定义Adobe Commerce的生成、部署和部署后流程。 请参阅[基于方案的部署](/help/cloud-guide/deploy/scenario-based.md)。

## 第二次暂存 {#second-staging}

>[!NOTE]
>
>有些项目需要更复杂的开发工作流。 为了满足此需求，Adobe提供了[额外的暂存环境](/help/cloud-guide/test/second-staging.md)作为云基础架构的附加选项。

## 服务指令 {#service-instruction}

请按照以下说明在专业集成环境和入门环境（包括`master`分支）上进行服务设置。

>[!NOTE]
>
>[提交Adobe Commerce支持票证](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html?lang=zh-Hans#submit-ticket)以更改Pro生产和暂存环境中的服务配置。

## 服务更改 {#service-change-tip}

>[!TIP]
>
>在初始服务设置之后，您可以通过更新`services.yaml`和`.magento.app.yaml`配置文件来更改已安装服务的软件版本。 有关升级或降级服务的指导，请参阅[更改服务版本](/help/cloud-guide/services/services-yaml.md#change-service-version)。

## 停滞的部署提示 {#stuck-deployment-tip}

>[!TIP]
>
>要获得停滞部署的帮助，请使用&#x200B;_Adobe Commerce帮助中心_&#x200B;中的[Commerce部署疑难解答程序](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/deployment/magento-deployment-troubleshooter.html?lang=zh-Hans)。

## ECE工具的更新 {#ece-tools-package}

>[!NOTE]
>
>如果您在不包含`ece-tools`包的云基础架构上使用Adobe Commerce版本，则必须对云项目执行[一次性升级](/help/cloud-guide/dev-tools/install-package.md)以删除已弃用的包。 如果您当前使用的是`ece-tools`程序包，需要对其进行更新，请参阅[更新ECE-Tools程序包](/help/cloud-guide/dev-tools/update-package.md)。

## 升级提示 {#upgrade-tip}

>[!TIP]
>
>在开始升级或修补过程之前，请从集成环境创建一个活动分支，并将新分支签出到您的本地工作站。 将分支专用于升级或修补过程有助于避免干扰正在进行的工作。

<!-- Fastly-related snippets begin -->

## 管理员登录 {#admin-login-step}

1. [登录](/help/get-started/onboarding.md#access-your-admin-panel)管理员。

## 自动部署自定义VCL代码片段 {#automate-vcl-snippet-deployment}

>[!NOTE]
>
>您可以向环境中的`$MAGENTO_CLOUD_APP_DIR/var/vcl_snippets_custom`目录添加代码片段，而不是手动上传自定义VCL代码片段。 当您在Commerce Admin中单击&#x200B;_将VCL上传到Fastly_&#x200B;时，此目录中的代码片段会自动上传。 有关Magento2文档，请参阅Fastly CDN模块中的[自动自定义VCL代码片段部署](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/CUSTOM-VCL-SNIPPETS.md#automated-custom-vcl-snippets-deployment)。

<!-- Fastly-related snippets end -->
