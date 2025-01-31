---
title: GitLab集成
description: 了解如何在云基础架构项目中将Adobe Commerce与GitLab集成。
feature: Cloud, Integration
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '777'
ht-degree: 0%

---

# GitLab集成

您可以配置GitLab存储库，以便在推送代码更改时自动构建和部署环境。 此集成会将您的GitLab存储库与云基础架构帐户上的Adobe Commerce同步。

{{private-repository}}

通过此集成，您可以：

- 创建分支时创建环境
- 合并拉取请求时重新部署环境
- 删除分支时删除环境

您必须获取GitLab令牌和Webhook才能继续此过程。

## 先决条件

- 对云基础架构项目上Adobe Commerce的管理员访问权限
- 本地环境中的[`magento-cloud` CLI](../dev-tools/cloud-cli-overview.md)工具
- GitLab帐户
- GitLab个人访问令牌具有对GitLab存储库的写访问权限，所选范围必须至少为： `api`和`read_repository`。

## 准备存储库

从现有环境克隆Adobe Commerce on cloud infrastructure项目，并将项目分支迁移到空的新GitLab存储库，并保留相同的分支名称。 **关键是**&#x200B;要保留相同的Git树，以便您不会丢失云基础架构项目上的Adobe Commerce中的任何现有环境或分支。

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
   magento-cloud project:get <project-id>
   ```

1. 将GitLab存储库添加为远程存储库（假设在其SaaS版本中使用GitLab）。

   ```bash
   git remote add origin git@gitlab.com:<user-name>/<repo-name>.git
   ```

   远程连接的默认名称可以是`origin`或`magento`。 如果`origin`存在，则可以选择其他名称，也可以重命名或删除现有引用。 请参阅[git-remote文档](https://git-scm.com/docs/git-remote)。

1. 验证是否正确添加了GitLab遥控器。

   ```bash
   git remote -v
   ```

   预期响应：

   ```
   origin git@gitlab.com:<user-name>/<repo-name>.git (fetch)
   origin git@gitlab.com:<user-name>/<repo-name>.git (push)
   ```

1. 将项目文件推送到新的GitLab存储库。 请记住保持所有分支名称相同。

   ```bash
   git push -u origin master
   ```

   如果您从新的GitLab存储库开始，则可能需要使用`-f`选项，因为远程存储库与您的本地副本不匹配。

1. 验证GitLab存储库是否包含所有项目文件。

## 启用GitLab集成

使用`magento-cloud integration`命令启用GitLab集成，并获取GitLab webhook的有效负荷URL，以将更新从GitLab发送到云基础架构项目上的Adobe Commerce。

```bash
magento-cloud integration:add --type=gitlab --project=<project-ID> --token=<your-GitLab-token> [--base-url=<GitLab-url> --server-project=<GitLab-project> --build-merge-requests={true|false} --merge-requests-clone-parent-data={true|false} --fetch-branches={true|false} --prune-branches={true|false}]
```

| 选项 | 描述 |
| ------ | ----------- |
| `<project-ID>` | 您的Adobe Commerce on cloud基础架构项目ID |
| `<your-GitLab-token>` | 您为GitLab生成的个人访问令牌 |
| `--base-url` | GitLab的URL （`https://gitlab.com/`，如果在其版SaaS中使用GitLab） |
| `--server-project` | GitLab中的项目名称（基本URL后面的部分） |
| `--build-merge-requests` | 一个&#x200B;_可选_&#x200B;参数，它指示Adobe Commerce在云基础架构上为每个合并请求（默认为`true`）构建一个新环境 |
| `--merge-requests-clone-parent-data` | _可选_&#x200B;参数，它指示云基础架构上的Adobe Commerce克隆合并请求的父环境数据（默认为`true`） |
| `--fetch-branches` | 一个&#x200B;_可选_&#x200B;参数，该参数导致云基础架构上的Adobe Commerce从远程获取所有分支（作为非活动环境）（默认情况下为`true`） |
| `--prune-branches` | 一个&#x200B;_可选_&#x200B;参数，它指示云基础架构上的Adobe Commerce删除远程服务器上不存在的分支（默认为`true`） |

>[!WARNING]
>
>`magento-cloud integration`命令使用GitLab存储库中的代码覆盖Adobe Commerce on cloud infrastructure项目中的&#x200B;_所有_&#x200B;代码。 这包括所有分支，包括`production`分支。 此操作立即发生且无法撤消。 作为最佳实践，在添加GitLab集成之前，请务必在云基础架构项目中克隆Adobe Commerce中的所有分支，并将其推送到GitLab存储库。

**启用GitLab集成**：

1. 从终端，将GitLab集成添加到云基础架构项目上的Adobe Commerce：

   ```bash
   magento-cloud integration:add --type gitlab --project=3txxjf32gtryos --token=qVUfeEn4ouze7A7JH --base-url=https://gitlab.com/ --server-project=my-agency/project-name --build-merge-requests=false --merge-requests-clone-parent-data=false --fetch-branches=true --prune-branches=true
   ```

1. 出现提示时，输入`y`以添加集成。

   ```
   Warning: adding a 'gitlab' integration will automatically synchronize code from the external Git repository.
   This means it can overwrite all the code in your project.
   Are you sure you want to continue? [y/N] y
   ```

1. 复制返回输出显示的&#x200B;**挂接URL**。

   ```
   Hook URL: https://eu-3.magento.cloud/api/projects/3txxjf32gtryos/integrations/eolmpfizzg9lu/hook
   Created integration eolmpfizzg9lu (type: gitlab)
   +----------------------------------+---------------------------------------------------------------------------------------+
   | Property                         | Value                                                                                 |
   +----------------------------------+---------------------------------------------------------------------------------------+
   | id                               | <integration-id>                                                                      |
   | type                             | gitlab                                                                                |
   | token                            | ******                                                                                |
   | base_url                         | https://gitlab.com/                                                                   |
   | project                          | my-agency/project-name                                                                |
   | fetch_branches                   | true                                                                                  |
   | prune_branches                   | true                                                                                  |
   | build_merge_requests             | false                                                                                 |
   | merge_requests_clone_parent_data | false                                                                                 |
   | hook_url                         | https://eu-3.magento.cloud/api/projects/<project-id>/integrations/<integration-id>/hook |
   +----------------------------------+---------------------------------------------------------------------------------------+
   ```

### 在GitLab中添加webhook

要通过Cloud Git服务器传达事件（例如推送或合并请求），您必须为GitLab存储库[创建webhook](https://docs.gitlab.com/ee/user/project/integrations/webhooks.html#overview)

1. 在GitLab存储库中，单击&#x200B;**设置**&#x200B;选项卡。

1. 在左侧导航栏中，单击&#x200B;**Webhooks**。

1. 在&#x200B;_Webhooks_&#x200B;表单中，编辑以下字段：

   - **URL**：输入启用GitLab集成时返回的`Hook URL`。
   - **密码令牌**：如果需要，请输入验证密码。
   - **触发器**：根据需要检查`Merge request events`和/或`Push events`。
   - **启用SSL验证**：必须选择此选项。

1. 单击&#x200B;**添加webhook**。

### 测试集成

配置GitLab集成后，您可以使用`magento-cloud` CLI验证集成是否可正常工作：

```bash
magento-cloud integration:validate
```

或者，您也可以通过将简单的更改推送到GitLab存储库来进行测试。

1. 创建测试文件。

   ```bash
   touch test.md
   ```

1. 提交更改并将其推送到您的GitLab存储库。

   ```bash
   git add . && git commit -m "Testing GitLab integration" && git push
   ```

1. 登录到[[!DNL Cloud Console]](../project/overview.md)并验证是否显示提交消息以及是否部署项目。

## 创建云分支

使用`magento-cloud` CLI `environment:push`命令创建和激活新环境。 请参阅[创建云分支](bitbucket.md#create-a-cloud-branch)。

## 删除集成

使用`magento-cloud` CLI `integration:delete`命令删除集成。 请参阅[删除集成](bitbucket.md#remove-the-integration)。
