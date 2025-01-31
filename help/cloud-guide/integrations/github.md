---
title: GitHub集成
description: 了解如何在云基础架构项目中将Adobe Commerce与GitHub集成。
feature: Cloud, Integration
last-substantial-update: 2023-05-25T00:00:00Z
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '949'
ht-degree: 0%

---

# GitHub集成

通过GitHub集成，您可以直接从GitHub存储库在云基础架构环境中管理Adobe Commerce。 该集成管理GitHub中已有的内容，并在云基础架构代码存储库上与Adobe Commerce同步。 本质上，代码存储库是GitHub存储库的镜像。

{{private-repository}}

通过此集成，您可以：

- 创建分支时创建环境
- 合并拉取请求时重新部署环境
- 删除分支时删除环境

您必须获取GitHub令牌和webhook才能继续此过程。

## 先决条件

- 对云基础架构项目上Adobe Commerce的管理员访问权限
- GitHub存储库
- github个人访问令牌

## 生成GitHub令牌

在GitHub开发人员设置中创建经典个人访问令牌。 您必须是具有对GitHub存储库的写入权限的组的成员，这样您就可以&#x200B;_将_&#x200B;推送到存储库。 创建令牌时包括以下范围：

- `admin:repo_hook` — 创建Web挂接
- `repo` — 与您的存储库集成
- `read:org` — 与您的组织存储库集成

请参阅[GitHub：创建](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token)。

## 准备存储库

从现有环境克隆Adobe Commerce on cloud infrastructure项目，并将项目分支迁移到空的新的GitHub存储库，并保留相同的分支名称。 **关键是**&#x200B;要保留相同的Git树，以便您不会丢失云基础架构项目上的Adobe Commerce中的任何现有环境或分支。

1. 从终端，登录到您的Adobe Commerce on cloud infrastructure项目。

   ```bash
   magento-cloud login
   ```

1. 列出您的项目并复制项目ID。

   ```bash
   magento-cloud project:list
   ```

1. 将项目克隆到本地环境。

   ```bash
   magento-cloud project:get <project-ID>
   ```

1. 将您的GitHub存储库添加为远程存储库。

   ```bash
   git remote add origin git@github.com:<user-name>/<repo-name>.git
   ```

   远程连接的默认名称可以是`origin`或`magento`。 如果`origin`存在，则可以选择其他名称，也可以重命名或删除现有引用。 请参阅[git-remote文档](https://git-scm.com/docs/git-remote)。

1. 验证是否正确添加了GitHub远程设备。

   ```bash
   git remote -v
   ```

   预期响应：

   ```
   origin git@github.com:<user-name>/<repo-name>.git (fetch)
   origin git@github.com:<user-name>/<repo-name>.git (push)
   ```

1. 将项目文件推送到新的GitHub存储库。 请记住保持所有分支名称相同。

   ```bash
   git push -u origin master
   ```

   如果您从新的GitHub存储库开始，则可能需要使用`-f`选项，因为远程存储库与您的本地副本不匹配。

1. 验证GitHub存储库是否包含所有项目文件。

## 启用GitHub集成

在开始之前，您的项目代码和环境必须位于GitHub存储库中。 启用集成后，GitHub存储库将成为代码源。 如果将代码更改推送到原始`magento`存储库，则在将代码更改推送到GitHub存储库时，该集成会覆盖该更改。

以下内容启用GitHub集成，并提供在创建webhook时使用的有效负载URL。

>[!WARNING]
>
>以下命令使用GitHub存储库中的代码覆盖Adobe Commerce on cloud infrastructure项目中的&#x200B;_所有_&#x200B;代码，该存储库包括所有分支，包括`production`分支。 此操作立即发生且无法撤消。 作为最佳实践，在添加GitHub集成之前&#x200B;**，请务必从Adobe Commerce在云基础架构项目中克隆所有分支并将其推送到GitHub存储库**。

您可以选择使用`magento-cloud integration:add`逐步完成CLI提示，也可以使用以下选项生成集成命令：

| 选项 | 必需？ | 描述 |
| ----------------------- | --------- | --------------------------------- |
| `--base-url` | 是 | 服务器安装的基本URL，可能是`https://github.com/`或自定义。 如果您的存储库使用公共Github托管，或者您的存储库不是在专用服务器上托管，请忽略此选项。 如果您的存储库URL类似于`https://github.com/{account}/{repository-name}`，请忽略此选项。 这可能会导致`Unable to connect to GitHub: repository not found`等错误。 |
| `--token` | 是 | 您为GitHub生成的个人访问令牌 |
| `--repository` | 是 | 存储库名称： `owner-or-organisation/repository` |
| `--build-pull-requests` | 可选 | 指示云基础架构上的Adobe Commerce在您合并拉取请求（默认情况下为`true`）后进行部署 |
| `--fetch-branches` | 可选 | 导致云基础架构上的Adobe Commerce跟踪分支并在您更新分支后进行部署（默认情况下为`true`） |
| `--prune-branches` | 可选 | 删除远程上不存在的分支（默认为`true`） |

还有许多选项，您可以使用帮助选项查看这些选项：

```bash
magento-cloud integration:add --help
```

**启用GitHub集成**：

1. 启用集成。

   ```bash
   magento-cloud integration:add --type=github --project=<project-ID> --token=<your-GitHub-token> {--repository=USER/REPOSITORY | --repository=ORGANIZATION/REPOSITORY} [--build-pull-requests={true|false} --fetch-branches={true|false}
   ```

   **示例1**：为个人专用存储库启用GitHub集成：

   ```bash
   magento-cloud integration:add --type=github --project=ov58dlacU2e --base-url=https://github.com --token=<token> --repository=myUserName/myrepo
   ```

   **示例2**：为组织存储库启用GitHub集成：

   ```bash
   magento-cloud integration:add --type=github --project=ov58dlacU2e --base-url=https://github.com --token=<token> --repository=Magento/teamrepo
   ```

1. 出现提示时输入所需信息。

1. 复制返回输出显示的&#x200B;**有效负载URL**。

   ```
   Created integration <integration-ID> (type: github)
   Repository: myUserName/myrepo
   Build PRs: yes
   Fetch branches: yes
   Payload URL: https://us.magento.cloud/api/projects/<project-id>/integrations/wO8a0eoamxwcg/hook
   ```

## 在GitHub中添加webhook

要通过Cloud Git服务器传达事件（例如推送），您必须为GitHub存储库创建一个webhook：

1. 在GitHub存储库中，单击&#x200B;**设置**&#x200B;选项卡。

1. 在左侧导航栏中，单击&#x200B;**Webhooks**。

1. 在&#x200B;_Webhooks_&#x200B;窗格中，单击&#x200B;**添加webhook**。

1. 在&#x200B;_Webhooks/添加webhook_&#x200B;表单中，编辑以下字段：

   - **负载URL**：输入启用GitHub集成时返回的URL。
   - **内容类型**：从列表中选择&#x200B;**application/json**。
   - **密码**：输入验证密码。
   - **您要触发此webhook的哪些事件？**：选择&#x200B;**向我发送所有内容**。
   - 选中&#x200B;**活动**&#x200B;复选框。

1. 单击&#x200B;**添加webhook**。

## 测试集成

配置GitHub集成后，您可以使用`magento-cloud` CLI验证集成是否可正常工作：

```bash
magento-cloud integration:validate
```

或者，您也可以通过将简单的更改推送到GitHub存储库来进行测试。

1. 创建测试文件。

   ```bash
   touch test.md
   ```

1. 提交更改并将其推送到GitHub存储库。

   ```bash
   git add . && git commit -m "Testing GitHub integration" && git push
   ```

1. 登录到[[!DNL Cloud Console]](../project/overview.md)并验证是否显示提交消息以及是否部署项目。

## 删除集成

您可以安全地从项目中删除GitHub集成，而不会影响代码。

**要删除GitHub集成**：

1. 从终端，登录到您的Adobe Commerce on cloud infrastructure项目。

1. 列出您的集成。 您需要GitHub集成ID才能完成下一步。

   ```bash
   magento-cloud integration:list
   ```

1. 删除集成。

   ```bash
   magento-cloud integration:delete <int-ID>
   ```

此外，您可以通过登录到GitHub帐户并删除存储库&#x200B;_设置_&#x200B;的&#x200B;_Webhooks_&#x200B;选项卡中的Web挂接来删除GitHub集成。
