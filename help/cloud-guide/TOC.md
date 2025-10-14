---
user-guide-title: 云上 Commerce 指南
user-guide-description: 了解如何在云基础架构上管理 Adobe Commerce 应用程序。
product: magento
feature: Cloud
source-git-commit: 5fc2082ca2aae8a1466821075c01ce756ba382cc
workflow-type: tm+mt
source-wordcount: '360'
ht-degree: 8%

---


# 云基础架构上的Commerce {#user-guide}

+ [Commerce](overview.md)
+ 架构 {#architecture}
   + [云基础架构](architecture/cloud-architecture.md)
   + [安全性](architecture/security.md)
   + [技术栈栈](architecture/tech-stack.md)
   + [入门级架构](architecture/starter-architecture.md)
   + [入门工作流](architecture/starter-develop-deploy-workflow.md)
   + [专业体系结构](architecture/pro-architecture.md)
   + [专业工作流程](architecture/pro-develop-deploy-workflow.md)
   + [可扩展的体系结构](architecture/scaled-architecture.md)
   + [自动缩放](architecture/autoscaling.md)
+ [开始](https://experienceleague.adobe.com/docs/commerce-on-cloud/start/overview.html?lang=zh-Hans)
+ 发行说明 {#release-notes}
   + [Cloud tools suite](release-notes/cloud-tools-suite.md)
   + [ECE-Tools包](release-notes/ece-tools-package.md)
   + [云修补程序](release-notes/cloud-patches.md)
   + [Cloud Docker包](release-notes/cloud-docker.md)
   + [云组件](release-notes/cloud-components.md)
   + [云包](release-notes/cloud-packages.md)
   + [向后不兼容的更改](release-notes/backward-incompatible-changes.md)
   + [发行说明存档](release-notes/cloud-release-archive.md)
+ 云项目 {#project}
   + [项目概述](project/overview.md)
   + [项目结构](project/file-structure.md)
   + [用户访问权限](project/user-access.md)
   + [多重身份验证](project/multi-factor-authentication.md)
   + [活动流](project/activity-stream.md)
   + [传出电子邮件](project/outgoing-emails.md)
   + [SendGrid电子邮件服务](project/sendgrid.md)
   + [控制台分支管理](project/console-branches.md)
   + [区域IP地址](project/regional-ip-addresses.md)
+ 开发人员工具 {#dev-tools}
   + [概述](dev-tools/overview.md)
   + Cloud CLI {#cloud-cli}
      + [CLI概述](dev-tools/cloud-cli-overview.md)
      + [CLI参考](dev-tools/cloud-cli-reference.md)
   + [Cloud Docker](dev-tools/cloud-docker.md)
   + ECE-Tools {#ece-tools}
      + [包概述](dev-tools/package-overview.md)
      + [使用ECE-Tools的一次性升级](dev-tools/install-package.md)
      + [更新ECE工具包](dev-tools/update-package.md)
      + [CLI参考](dev-tools/ece-tools-cli-reference.md)
      + [错误引用](dev-tools/error-reference.md)
   + 集成 {#integrations}
      + [概述](integrations/overview.md)
      + [比特桶](integrations/bitbucket.md)
      + [GitHub](integrations/github.md)
      + [GitLab](integrations/gitlab.md)
      + [运行状况通知](integrations/health-notifications.md)
+ 开发 {#develop}
   + [概述](development/overview.md)
   + [身份验证密钥](development/authentication-keys.md)
   + [CLI分支管理](development/cli-branches.md)
   + [安全连接](development/secure-connections.md)
   + 部署 {#deploy}
      + [部署过程](deploy/process.md)
      + [优化](deploy/optimization.md)
      + [最佳实践](deploy/best-practices.md)
      + [基于方案的部署](deploy/scenario-based.md)
      + [零停机部署](deploy/reduce-downtime.md)
      + [静态内容部署](deploy/static-content.md)
      + [智能向导](deploy/smart-wizards.md)
      + [部署到暂存和生产环境](deploy/staging-production.md)
      + [从组件故障中恢复](deploy/recover-failed-deployment.md)
   + 测试 {#test}
      + [测试指南](test/guidance.md)
      + [日志](test/log-locations.md)
      + [Xdebug](test/debug.md)
      + [示例数据](test/sample-data.md)
      + [暂存和生产](test/staging-and-production.md)
      + [第二个暂存环境](test/second-staging.md)
   + [PrivateLink服务](development/privatelink-service.md)
   + [保护块](development/protective-block.md)
   + [恢复环境](development/restore-environment.md)
   + 存储 {#storage}
      + [管理磁盘空间](storage/manage-disk-space.md)
      + [配置文件数据库查询](storage/profile-database-queries.md)
      + [备份数据库](storage/database-dump.md)
      + [备份管理](storage/snapshots.md)
   + 升级和修补程序 {#upgrade}
      + [最佳实践](development/best-practices.md)
      + [升级Commerce版本](development/commerce-version.md)
      + [应用修补程序](development/apply-patches.md)
+ 配置 {#configure}
   + [概述](environment/overview.md)
   + 应用程序 {#app}
      + [配置应用程序部署](application/configure-app-yaml.md)
      + [PHP设置](application/php-settings.md)
      + 属性 {#properties}
         + [应用程序属性](application/properties.md)
         + [克朗](application/crons-property.md)
         + [防火墙（仅限起始者）](application/firewall-property.md)
         + [挂钩](application/hooks-property.md)
         + [变量](application/variables-property.md)
         + [Web](application/web-property.md)
         + [工作人员](application/workers-property.md)
      + [设置静态文件的缓存](application/set-cache.md)
   + 环境 {#env}
      + [配置环境部署](environment/configure-env-yaml.md)
      + [变量级别和选项](environment/variable-levels.md)
      + 覆盖变量 {#stage}
         + [环境变量](environment/variables-intro.md)
         + [管理员](environment/variables-admin.md)
         + [云变量](environment/variables-cloud.md)
         + [全局](environment/variables-global.md)
         + [生成](environment/variables-build.md)
         + [部署](environment/variables-deploy.md)
         + [部署后](environment/variables-post-deploy.md)
      + 配置通知 {#log}
         + [通知](environment/set-up-notifications.md)
         + [日志处理程序](environment/log-handlers.md)
   + 路由 {#routes}
      + [配置路由](routes/routes-yaml.md)
      + [缓存](routes/caching.md)
      + [重定向](routes/redirects.md)
      + [服务器端包括](routes/server-side-includes.md)
   + 服务 {#service}
      + [配置服务](services/services-yaml.md)
      + [ActiveMQ](services/activemq.md)
      + [Elasticsearch](services/elasticsearch.md)
      + [MySQL](services/mysql.md)
      + [OpenSearch](services/opensearch.md)
      + [RabbitMQ](services/rabbitmq.md)
      + [Redis](services/redis.md)
      + [Valkey](services/valkey.md)
+ Fastly服务 {#cdn}
   + [概述](cdn/fastly.md)
   + Fastly设置 {#setup-fastly}
      + [配置Fastly服务](cdn/fastly-configuration.md)
      + [自定义缓存配置](cdn/fastly-custom-cache-configuration.md)
      + [自定义错误和维护页面](cdn/fastly-custom-response.md)
   + [Web应用程序防火墙](cdn/fastly-waf-service.md)
   + [图像优化](cdn/fastly-image-optimization.md)
   + 用VCL定制 {#custom-vcl-snippets}
      + [快速入门](cdn/fastly-vcl-custom-snippets.md)
      + [将请求重新路由到CMS后端](cdn/fastly-vcl-wordpress.md)
      + [阻止反向链接垃圾邮件](cdn/fastly-vcl-badreferer.md)
      + [IP允许列表](cdn/fastly-vcl-allowlist.md)
      + [IP阻止列表](cdn/fastly-vcl-blocking.md)
      + [绕过Fastly缓存](cdn/fastly-vcl-bypass-to-origin.md)
   + [Fastly故障诊断](cdn/fastly-troubleshooting.md)
+ 商店设置 {#configure-store}
   + [概述](store/overview.md)
   + [最佳实践](store/best-practices.md)
   + [自定义主题](store/custom-theme.md)
   + [扩展](store/extensions.md)
   + [B2B模块](store/b2b-module.md)
   + [多个站点](store/multiple-sites.md)
   + [站点地图和搜索引擎机器人](store/robots-sitemap.md)
   + [PayPal支付方式](store/paypal.md)
   + [配置管理](store/store-settings.md)
+ 启动站点 {#launch}
   + [概述](launch/overview.md)
   + [启动项核对清单](launch/checklist.md)
   + [启动步骤](launch/steps.md)
+ 监控站点 {#monitor}
   + [性能](monitor/performance.md)
   + [操作遥测](monitor/operational-telemetry.md)
   + New Relic服务 {#new-relic}
      + [New Relic概述](monitor/new-relic-service.md)
      + [帐户和用户管理](monitor/account-management.md)
      + 调查性能 {#investigate}
         + [策略、警报和工作流](monitor/investigate-performance.md)
         + [数据摄取](monitor/ingest-data.md)
         + [跟踪部署](monitor/track-deployments.md)
      + [日志管理](monitor/log-management.md)
