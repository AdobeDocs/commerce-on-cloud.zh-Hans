---
title: 跟踪部署
description: 了解如何配置New Relic以跟踪Adobe Commerce中云基础架构项目的部署并分析性能变化。
feature: Cloud, Deploy, Observability
topic: Performance
last-substantial-update: 2023-10-12T00:00:00Z
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '283'
ht-degree: 0%

---

# 跟踪部署

您可以启用New Relic _跟踪更改_&#x200B;功能以监视云基础架构项目上Commerce上的部署事件。

部署数据收集有助于分析部署更改对整体性能的影响，例如CPU、内存、响应时间等。 请参阅&#x200B;_New Relic文档_&#x200B;中的[使用NerdGraph](https://docs.newrelic.com/docs/change-tracking/change-tracking-graphql/)跟踪更改。

>[!PREREQUISITES]
>
>- `NR_API_URL`： New Relic API端点，在此例中为NerdGraph API URL `https://api.newrelic.com/graphql`
>- `NR_API_KEY`：创建用户密钥，请参阅&#x200B;_New Relic_&#x200B;文档中的[New Relic API密钥](https://docs.newrelic.com/docs/apis/intro-apis/new-relic-api-keys)。
>- `NR_APP_GUID`：向New Relic报告数据的实体具有唯一ID (GUID)。 例如，要在暂存环境中启用，请使用New Relic中的&#x200B;_暂存实体GUID_&#x200B;调整暂存环境`NR_APP_GUID`云变量。 请参阅&#x200B;_New Relic_&#x200B;文档中的[了解New Relic实体](https://docs.newrelic.com/docs/new-relic-solutions/new-relic-one/core-concepts/what-entity-new-relic/)和[NerdGraph教程：查看实体数据](https://docs.newrelic.com/docs/apis/nerdgraph/examples/nerdgraph-entities-api-tutorial/)。

## 启用跟踪部署

通过创建&#x200B;_脚本_&#x200B;集成，跟踪New Relic中的Commerce项目部署事件。

**要启用跟踪部署**：

1. 在本地工作站上，转到您的项目目录。
1. 创建`action-integration.js`文件。 复制以下代码并将其粘贴到`action-integration.js`文件中并保存：

   ```javascript
   function trackDeployments() {
     const envName = activity.payload.environment.name;
     let variables;
     activity.payload.deployment.variables.forEach(function(variable) {
       if (variable.name === "env:NR_CONFIG") {
         variables = variable.value;
       }
     });
     const config = JSON.parse(variables.replace(/'/g, '"'));
     const commitSha = activity.payload.commits ? activity.payload.commits[0].sha : activity.payload.environment.head_commit;
     const deploymentType = activity.type;
   
     if (!(envName in config)) {
       throw new Error('There is no configuration for ' + envName);
     }
   
     const configEnv = config[envName];
   
     if (!configEnv.NR_APP_GUID || !configEnv.NR_API_KEY || !configEnv.NR_API_URL) {
       throw new Error('You must define the next configuation in the env variable NR_CONFIG: NR_APP_GUID, NR_API_KEY and NR_API_URL');
     }
   
     const query = `mutation {
       changeTrackingCreateDeployment(
       deployment: {
           version: "${commitSha}",
           entityGuid: "${configEnv.NR_APP_GUID}",
           commit: "${commitSha}",
           changelog: "${deploymentType}"
       }
       ) {
         deploymentId
         entityGuid
       }
     }`;
   
     var resp = fetch(configEnv.NR_API_URL, {
       method: 'POST',
       headers: {
           'Content-Type': 'application/json',
           'API-Key': configEnv.NR_API_KEY
       },
       body: JSON.stringify({
           query
       })
     });
   
     if (!resp.ok) {
       console.log('Sending new relic change tracking failed: ' + resp.text());
     } else {
       console.log(resp.text());
     }
   }
   
   trackDeployments();
   ```

1. 使用`magento-cloud` CLI命令创建&#x200B;_脚本_&#x200B;集成并引用`action-integration.js`文件。

   ```bash
   magento-cloud integration:add --type script --events='environment.restore, environment.push, environment.branch, environment.activate, environment.synchronize, environment.initialize, environment.merge, environment.redeploy, environment.variable.create, environment.variable.delete, environment.variable.update' --file ./action-integration.js --project=<YOUR_PROJECT_ID> --environments=<YOUR_ENVIRONMENT_ID>
   ```

   示例响应：

   ```
   Created integration 767u4hathojjw (type: script)
   +-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------+
   | Property              | Value                                                                                                                                   |
   +-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------+
   | id                    | 767u4hathojjw                                                                                                                           |
   | type                  | script                                                                                                                                  |
   | role                  |                                                                                                                                         |
   | events                | - environment.restore                                                                                                                   |
   |                       | - environment.push                                                                                                                      |
   |                       | - environment.branch                                                                                                                    |
   |                       | - environment.activate                                                                                                                  |
   |                       | - environment.synchronize                                                                                                               |
   |                       | - environment.initialize                                                                                                                |
   |                       | - environment.merge                                                                                                                     |
   |                       | - environment.redeploy                                                                                                                  |
   |                       | - environment.variable.create                                                                                                           |
   |                       | - environment.variable.delete                                                                                                           |
   |                       | - environment.variable.update                                                                                                           |
   | environments          | - staging                                                                                                                               |
   |                       | - production                                                                                                                            |
   | excluded_environments | {  }                                                                                                                                    |
   | states                | - complete                                                                                                                              |
   | result                | *                                                                                                                                       |
   | script                | function variables() {                                                                                                                  |
   |                       |     var vars = {};                                                                                                                      |
   |                       |     activity.payload.deployment.variables.forEach(function(variable) {                                                                  |
   |                       |         vars[variable.name] = variable.value;                                                                                           |
   |                       |     });                                                                                                                                 |
   |                       |     return vars;                                                                                                                        |
   |                       | }                                                                                                                                       |
   |                       |                                                                                                                                         |
   |                       | function trackDeployments() {                                                                                                           |
   |                       |     const envName = activity.payload.environment.name;                                                                                  |
   |                       |                                                                                                                                         |
   |                       |     const config = JSON.parse(variables()['env:NR_CONFIG'].replace(/'/g, '"'));                                                         |
   |                       |     const commitSha = activity.payload.commits ? activity.payload.commits[0].sha : activity.payload.environment.head_commit;            |
   |                       |     const deploymentType = activity.type;                                                                                               |
   |                       |                                                                                                                                         |
   |                       |     if (!(envName in config)) {                                                                                                         |
   |                       |         throw new Error('There is no configuration for ' + envName);                                                                    |
   |                       |     }                                                                                                                                   |
   |                       |                                                                                                                                         |
   |                       |     const configEnv = config[envName];                                                                                                  |
   |                       |                                                                                                                                         |
   |                       |     if (!configEnv.NR_APP_GUID || !configEnv.NR_API_KEY || !configEnv.NR_API_URL) {                                                     |
   |                       |         throw new Error('You must define the next configuation in the env variable NR_CONFIG: NR_APP_GUID, NR_API_KEY and NR_API_URL'); |
   |                       |     }                                                                                                                                   |
   |                       |                                                                                                                                         |
   |                       |     const query = `mutation {                                                                                                           |
   |                       |         changeTrackingCreateDeployment(                                                                                                 |
   |                       |           deployment: {                                                                                                                 |
   |                       |             version: "${commitSha}",                                                                                                    |
   |                       |             entityGuid: "${configEnv.NR_APP_GUID}",                                                                                     |
   |                       |             commit: "${commitSha}",                                                                                                     |
   |                       |             changelog: "${deploymentType}"                                                                                              |
   |                       |           }                                                                                                                             |
   |                       |         ) {                                                                                                                             |
   |                       |           deploymentId                                                                                                                  |
   |                       |           entityGuid                                                                                                                    |
   |                       |         }                                                                                                                               |
   |                       |     }`;                                                                                                                                 |
   |                       |                                                                                                                                         |
   |                       |     var resp = fetch(configEnv.NR_API_URL, {                                                                                            |
   |                       |         method: 'POST',                                                                                                                 |
   |                       |         headers: {                                                                                                                      |
   |                       |             'Content-Type': 'application/json',                                                                                         |
   |                       |             'API-Key': configEnv.NR_API_KEY                                                                                             |
   |                       |         },                                                                                                                              |
   |                       |         body: JSON.stringify({                                                                                                          |
   |                       |             query                                                                                                                       |
   |                       |         })                                                                                                                              |
   |                       |     });                                                                                                                                 |
   |                       |                                                                                                                                         |
   |                       |     if (!resp.ok) {                                                                                                                     |
   |                       |         console.log('Sending new relic change tracking failed: ' + resp.text());                                                        |
   |                       |     } else {                                                                                                                            |
   |                       |         console.log(resp.text());                                                                                                       |
   |                       |     }                                                                                                                                   |
   |                       | }                                                                                                                                       |
   |                       |                                                                                                                                         |
   |                       | trackDeployments();                                                                                                                     |
   |                       |                                                                                                                                         |
   +-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------+
   ```

1. 记下集成ID以供将来使用。 在此示例中，ID是：

   ```
   Created integration 767u4hathojjw (type: script)
   ```

   或者，您可以使用以下代码验证集成并记下集成ID： `magento-cloud integration:list`

1. 使用先决条件创建环境变量。

   ```bash
   magento-cloud variable:create --level project --name=env:NR_CONFIG --value='{"<YOUR_ENVIRONMENT_ID>":{"NR_API_KEY": "<YOUR_API_KEY>", "NR_API_URL": "https://api.newrelic.com/graphql", "NR_APP_GUID":"<YOUR_APP_GUID>"}}'  -p <YOUR_PROJECT_ID>
   ```

1. 查看上一个活动日志。

   ```bash
   magento-cloud integration:activity:log <INTEGRATION_ID> -p <YOUR_PROJECT_ID> -e <YOUR_ENVIRONMENT_ID>
   ```

   响应：

   ```
   Integration ID: 767u4hathojjw
   Activity ID: poxqidsfajkmg
   Type: integration.script
   Description: Running activity script
   Created: 2023-08-28T20:32:02+00:00
   State: complete
   Log:
   HTTP request
   HTTP response
   {"data":{"changeTrackingCreateDeployment":{"deploymentId":"some-deployment-id","entityGuid":"SomeGUIDhere"}}}
   ```

1. 登录到您的[New Relic帐户](https://login.newrelic.com/login)。

1. 在Explorer导航菜单中，单击&#x200B;**[!UICONTROL APM & Services]**。 选择您的环境[!UICONTROL Name]和[!UICONTROL Account]。

1. 在&#x200B;_事件_&#x200B;下，单击&#x200B;**[!UICONTROL Change tracking]**。

   ![部署](../../assets/new-relic/deployments.png)
