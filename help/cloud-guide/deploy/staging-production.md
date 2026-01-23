---
title: 部署到暂存和生产环境
description: 了解如何在云基础架构上将Adobe Commerce代码部署到暂存环境和生产环境以进行进一步测试。
feature: Cloud, Console, Deploy, SCD, Storage
exl-id: 1cfeb472-c6ec-44ff-9b32-516ffa1b30d2
source-git-commit: fe634412c6de8325faa36c07e9769cde0eb76c48
workflow-type: tm+mt
source-wordcount: '1311'
ht-degree: 0%

---

# 部署到暂存和生产环境

部署和上线的过程从开发开始，一直到暂存，最后是在生产环境中上线。 Adobe提供了一个端到端环境解决方案，以确保配置的一致性。 每个环境都支持对店面的直接URL访问，以及CLI命令的管理员和SSH访问。

准备好部署存储时，必须先在暂存环境中完成部署和测试，然后才能部署到生产环境。 本节提供了有关构建和部署过程、迁移数据和内容以及测试的深入说明和信息。

>[!TIP]
>
>Adobe建议先创建环境的[备份](../storage/snapshots.md)，然后再部署。

此外，您还可以启用[使用New Relic跟踪部署](../monitor/track-deployments.md)以监视部署事件并帮助您分析部署之间的性能。

## 入门部署流程

Adobe建议从`staging`分支创建一个`master`分支，以最好地支持您的入门计划开发和部署。 然后，您的四个活动环境中有两个已准备就绪：`master`用于生产，`staging`用于暂存。

有关进程的详细信息，请参阅[入门开发和部署工作流](../architecture/starter-develop-deploy-workflow.md)。

## 专业部署流程

Pro随附带两个活动分支的大型集成环境：全局`master`分支、暂存分支和生产分支。 在创建项目时，代码可以随时分支、开发和推送以构建和部署站点。 尽管集成环境可以具有多个分支，但暂存和生产环境对于每个环境只能有一个分支。

有关进程的详细信息，请参阅[专业开发和部署工作流](../architecture/pro-develop-deploy-workflow.md)。

## 将代码部署到暂存环境

暂存环境提供了一个准生产环境，其中包括数据库、Web服务器以及包括Fastly和New Relic在内的所有服务。 您可以通过终端应用程序通过[[!DNL Cloud Console]](../project/overview.md)或[云CLI命令](../dev-tools/cloud-cli-overview.md)完全推送、合并和部署。

### 使用[!DNL Cloud Console]部署代码

[!DNL Cloud Console]提供用于在集成、暂存和生产环境中为入门和专业计划创建、管理和部署代码的功能。

**对于Pro项目，将集成分支部署到暂存环境**：

1. [登录](https://accounts.magento.cloud)您的项目。
1. 选择`integration`分支。
1. 选择要部署到暂存环境的&#x200B;**合并**&#x200B;选项。

   ![合并](../../assets/button-merge.png){width="150"}

1. 在暂存环境中完成所有[测试](../test/staging-and-production.md)。
1. 当暂存就绪时，选择&#x200B;**合并**&#x200B;选项以部署到生产环境。

**首先，将开发分支部署到暂存环境**：

1. [登录](https://accounts.magento.cloud)您的项目。
1. 选择准备的代码分支。
1. 选择要部署到暂存环境的&#x200B;**合并**&#x200B;选项。

   ![合并](../../assets/button-merge.png){width="150"}

1. 在暂存环境中完成所有[测试](../test/staging-and-production.md)。
1. 当暂存就绪时，选择&#x200B;**合并**&#x200B;选项以部署到生产环境(`master`)。

### 使用命令行部署代码

Cloud CLI提供用于部署代码的命令。 您需要SSH和Git权限才能访问项目。

#### 步骤1：部署和测试集成环境

1. 登录项目后，请查看集成环境：

   ```bash
   magento-cloud environment:checkout <environment-ID>
   ```

1. 将本地集成环境与远程环境同步：

   ```bash
   magento-cloud environment:synchronize <environment-ID>
   ```

1. 创建环境的快照作为备份：

   ```bash
   magento-cloud snapshot: create -e <environment-ID>
   ```

1. 根据需要更新本地分支中的代码。

1. 添加、提交更改并将更改推送到环境。

   ```bash
   git add -A && git commit -m "Commit message" && git push origin <environment-ID>
   ```

1. 完成站点测试。

#### 步骤2：将更改合并到暂存和部署

1. 查看暂存环境：

   ```bash
   magento-cloud environment:checkout <environment-ID>
   ```

1. 将本地暂存环境与远程环境同步：

   ```bash
   magento-cloud environment:synchronize <environment-ID>
   ```

1. 创建环境的快照作为备份：

   ```bash
   magento-cloud snapshot: create -e <environment-ID>
   ```

1. 将集成环境合并到暂存环境以进行部署：

   ```bash
   magento-cloud environment:merge <integration-ID>
   ```

1. 完成站点测试。

#### 步骤3：部署到生产

1. 签出、同步和创建本地生产环境的快照。

1. 将暂存环境合并到要部署的生产环境：

   ```bash
   magento-cloud environment:merge <staging-ID>
   ```

1. 完成站点测试。

## 迁移静态文件

[静态文件](https://experienceleague.adobe.com/zh-hans/docs/commerce-operations/implementation-playbook/glossary)存储在`mounts`中。 将文件从源装载位置（如本地环境）迁移到目标装载位置的方法有两种。 这两种方法都使用`rsync`实用程序，但Adobe建议使用`magento-cloud` CLI在本地和远程环境之间移动文件。 在将文件从远程源移动到其他远程位置时，Adobe建议使用`rsync`方法。

### 使用CLI迁移文件

您可以使用`mount:upload`和`mount:download` CLI命令在本地和远程环境之间迁移文件。 这两个命令都使用`rsync`实用程序，但CLI命令提供了针对Adobe Commerce on cloud infrastructure环境定制的选项和提示。 例如，如果使用不带选项的简单命令，CLI将提示您选择要上载或下载的装载或装载。

```bash
magento-cloud mount:download
```

示例响应：

```
Enter a number to choose a mount to download from:
  [0] app/etc
  [1] pub/static
  [2] var
  [3] pub/media
  [4] All mounts
 > 3

Target directory: ~/pub/media/

Downloading files from the remote mount pub/media to pub/media

Are you sure you want to continue? [Y/n] Y
```

**要将文件从本地`pub/media/`文件夹上载到当前环境的远程`pub/media/`文件夹**：

```bash
magento-cloud mount:upload --source /path/to/project/pub/media/ --mount pub/media/
```

示例响应：

```
Uploading files from pub/media to the remote mount pub/media

Are you sure you want to continue? [Y/n] Y

  building file list ...   done
  ./
  sample-file.jpeg

  sent 8.43K bytes  received 48 bytes  3.39K bytes/sec
  total size is 154.57K  speedup is 18.23
```

对`--help`和`mount:upload`命令使用`mount:download`选项查看更多选项。 例如，有一个`--delete`选项可在迁移期间删除无关文件。

### 使用rsync迁移文件

或者，您可以使用`rsync`实用程序迁移文件。

```bash
rsync -azvP <source> <destination>
```

此命令使用以下选项：

- `a` — 存档
- 迁移期间压缩`z`个文件
- `v` — 详细
- `P` — 部分进度

请参阅[rsync](https://linux.die.net/man/1/rsync)帮助。

>[!NOTE]
>
>若要将媒体从远程环境直接传输到远程环境，必须启用SSH代理转发，请参阅[GitHub指南](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/using-ssh-agent-forwarding)。

**若要将静态文件直接从远程环境迁移到远程环境（快速方法）**：

1. 使用SSH登录到源环境。 请勿使用`magento-cloud` CLI。 使用`-A`选项很重要，因为它允许转发身份验证代理连接。

   >[!TIP]
   >
   >要在您的&#x200B;**中找到** SSH访问[!DNL Cloud Console]链接，请选择环境并单击&#x200B;**访问站点**。

   ```bash
   ssh -A <environment_ssh_link@ssh.region.magento.cloud>
   ```

1. 使用`rsync`命令将`pub/media`目录从源环境复制到其他远程环境。

   ```bash
   rsync -azvP pub/media/ <destination_environment_ssh_link@ssh.region.magento.cloud>:pub/media/
   ```

1. 登录到另一个远程环境，验证文件是否已成功迁移。

## 迁移数据库

>[!WARNING]
>
>数据库导入和导出操作可能需要较长时间，这可能会影响站点性能和可用性。 在非高峰时间安排导入和导出操作，以防止生产站点的性能变慢或中断。

>[!BEGINSHADEBOX]

**先决条件：**&#x200B;数据库转储（请参阅步骤3）应包含数据库触发器。 若要转储它们，请确认您具有[TRIGGER权限](https://dev.mysql.com/doc/refman/5.7/en/privileges-provided.html#priv_trigger)。

>[!IMPORTANT]
>
>集成环境数据库严格用于开发测试，可以包含您不希望迁移到暂存和生产环境的数据。

对于持续集成部署，Adobe **不建议**&#x200B;将数据从集成迁移到暂存和生产环境。 您可以传递测试数据或覆盖重要数据。 在生成和部署期间使用[配置文件](../store/store-settings.md)和`setup:upgrade`命令传递任何重要配置。

>[!ENDSHADEBOX]

Adobe **建议**&#x200B;将数据从生产环境迁移到暂存环境，以完全测试您的网站并在接近生产环境的环境中使用所有服务和设置进行存储。

>[!NOTE]
>
>若要将媒体从远程环境直接传输到远程环境，必须启用ssh代理转发，请参阅[GitHub指南](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/using-ssh-agent-forwarding)。

### 备份数据库

最佳做法是创建数据库的备份。 以下过程使用[备份数据库](../storage/database-dump.md)的指导。

**要转储数据库**：

1. [使用SSH登录到包含要复制数据库的远程环境](../development/secure-connections.md#use-an-ssh-command)。

1. 列出环境关系并记下数据库登录信息。

   ```bash
   php -r 'print_r(json_decode(base64_decode($_ENV["MAGENTO_CLOUD_RELATIONSHIPS"]))->database);'
   ```

   对于Pro暂存和生产环境，数据库的名称在`MAGENTO_CLOUD_RELATIONSHIPS`变量中（通常与应用程序名称和用户名相同）。

1. 创建数据库的备份。 要为数据库转储选择目标目录，请使用`--dump-directory`选项。

   对于入门环境和Pro集成环境，请使用`main`作为数据库的名称：

   ```bash
   php vendor/bin/ece-tools db-dump main
   ```

   转储选项：
   - `--dump-directory=<dir>` — 选择数据库转储的目标目录
   - `--remove-definers` — 从数据库转储中删除DEFINER语句

1. 虽然ECE-Tools方法是首选，但另一种方法是使用GZIP格式的本机MySQL创建数据库转储文件。

   ```bash
   mysqldump -h <database-host> --user=<database-username> --password=<password> --single-transaction --triggers <database-name> | gzip - > /tmp/database.sql.gz
   ```

   如果在目标环境中配置了双重身份验证，最好排除相关的2FA表，以避免在数据库迁移后重新配置它：

   ```bash
   mysqldump -h <database-host> --user=<database-username> --password=<password> --single-transaction --triggers --ignore-table=<database-name>.tfa_user_config --ignore-table=<database-name>.tfa_country_codes <database-name> | gzip - > /tmp/database.sql.gz
   ```

1. 键入`logout`以终止SSH连接。

### 删除并重新创建数据库

导入数据时，必须删除并创建数据库。

**要删除并重新创建数据库**：

1. 建立到远程环境的[SSH通道](../development/secure-connections.md#ssh-tunneling)。

1. 连接到数据库服务。

   ```bash
   mysql --host=127.0.0.1 --user='<database-username>' --pass='<user-password>' --database='<name>' --port='<port>'
   ```

1. 在`MariaDB [main]>`提示符下，删除数据库。

   对于Starter and Pro集成：

   ```shell
   drop database main;
   ```

   对于生产和暂存环境：

   ```shell
   drop database <database_name>;
   ```

1. 重新创建数据库。

   对于Starter and Pro集成：

   ```shell
   create database main;
   ```

1. 导入数据库。

   为生产导入：

   ```shell
   zcat <cluster-ID>.sql.gz | sed -e 's/DEFINER[ ]*=[ ]*[^*]*\*/\*/' | mysql -h 127.0.0.1 -p -u <database-username> <database-name>;
   ```

   导入以进行暂存：

   ```shell
   zcat <cluster-ID_stg>.sql.gz | sed -e 's/DEFINER[ ]*=[ ]*[^*]*\*/\*/' | mysql -h 127.0.0.1 -p -u <database-username> <database-name>;
   ```

   这些命令解压缩数据库转储文件，删除`DEFINER`语句，并使用指定的凭据导入数据库。
