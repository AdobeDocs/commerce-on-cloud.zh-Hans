---
title: 配置 [!DNL Xdebug]
description: 了解如何配置Xdebug扩展以便在云基础架构项目开发中调试Adobe Commerce。
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '1920'
ht-degree: 0%

---

# 配置Xdebug

[!DNL Xdebug]是用于调试PHP的扩展。 尽管您可以使用您选择的IDE，但以下内容将介绍如何将[!DNL Xdebug]和[!DNL PhpStorm]配置为在本地环境中调试。

>[!NOTE]
>
>您可以将[!DNL Xdebug]配置为在Cloud Docker环境中运行以进行本地调试，而无需更改云基础架构项目配置上的Adobe Commerce。 请参阅[为Docker配置Xdebug](https://developer.adobe.com/commerce/cloud-tools/docker/test/configure-xdebug/)。

要启用[!DNL Xdebug]，必须在Git存储库中配置文件、配置IDE并设置端口转发。 您可以在`magento.app.yaml`文件中配置某些设置。 编辑后，跨所有入门环境和Pro集成环境推送Git更改以启用[!DNL Xdebug]。 [!DNL Xdebug]在专业暂存和生产环境中已经可用。

配置完毕后，即可调试CLI命令、Web请求和代码。 请记住，所有云基础架构环境都是只读的。 将代码克隆到本地开发环境以执行调试。 对于Pro暂存环境和生产环境，请参阅有关[!DNL Xdebug]的[其他说明](#debug-for-pro-staging-and-production)。

## 要求

要运行和使用[!DNL Xdebug]，您需要环境的SSH URL。 您可以通过[[!DNL Cloud Console]](../project/overview.md)或您的[!DNL Cloud Onboarding UI]查找信息。

## 配置Xdebug

要配置[!DNL Xdebug]，请执行以下步骤：

- [在分支中工作以推送文件更新](#get-started-with-a-branch)
- [为环境启用 [!DNL Xdebug] &#x200B;](#enable-xdebug-in-your-environment)
- [配置PHPStorm服务器](#configure-phpstorm-server)
- [设置端口转发](#set-up-port-forwarding)

### 分支入门

要添加[!DNL Xdebug]，Adobe建议在[开发分支](../dev-tools/cloud-cli-overview.md#create-an-environment-branch)中工作。

### 在您的环境中启用Xdebug

您可以直接为所有入门环境和Pro集成环境启用[!DNL Xdebug]。 专业生产和暂存环境不需要此配置步骤。 请参阅[Debug for Pro Staging and Production](#debug-for-pro-staging-and-production)。

>[!VIDEO](https://video.tv.adobe.com/v/3437407?learn=on)

要为您的项目启用[!DNL Xdebug]，请将`xdebug`添加到`.magento.app.yaml`文件的`runtime:extensions`部分。

**启用Xdebug**：

1. 在本地终端中，在文本编辑器中打开`.magento.app.yaml`文件。

1. 在`runtime`部分的`extensions`下，添加`xdebug`。 例如：

   ```yaml
   runtime:
       extensions:
           - redis
           - xsl
           - newrelic
           - sodium
           - xdebug
   ```

1. 保存对`.magento.app.yaml`文件所做的更改并退出文本编辑器。

1. 添加、提交和推送更改以重新部署环境。

   ```bash
   git add .magento.app.yaml
   ```

   ```bash
   git commit -m "add xdebug"
   ```

   ```bash
   git push origin <environment-ID>
   ```

当部署到入门环境和Pro集成环境时，[!DNL Xdebug]现在可用。 继续配置IDE。 有关PhpStorm，请参阅[配置PhpStorm](#configure-phpstorm)。

### 配置PhpStorm服务器

>[!VIDEO](https://video.tv.adobe.com/v/3437409?learn=on)

必须将[PhpStorm](https://www.jetbrains.com/phpstorm/) IDE配置为正确使用[!DNL Xdebug]。

**要将PhpStorm配置为使用Xdebug**：

1. 在PhpStorm项目中，打开&#x200B;**设置**&#x200B;面板。

   - _macOS_ — 选择&#x200B;**PhpStorm** > **设置**。
   - _Windows/Linux_ — 选择&#x200B;**文件** > **设置**。

1. 在&#x200B;_设置_&#x200B;面板中，展开&#x200B;**PHP**&#x200B;部分并单击&#x200B;**服务器**。

1. 单击&#x200B;**+**&#x200B;以添加服务器配置。 项目名称在顶部为灰色。

1. [可选]为新服务器配置以下设置。 请参阅&#x200B;_PHPStorm_&#x200B;文档中的[未配置调试服务器](https://www.jetbrains.com/help/phpstorm/troubleshooting-php-debugging.html#no-debug-server-is-configured)。

   - **名称** — 输入与主机名相同的名称。 此值必须与[Debug CLI命令](#debug-cli-commands)中`PHP_IDE_CONFIG`变量的值匹配，才能使用CLI进行调试。
   - **主机** — 输入主机名。
   - **端口** — 输入`443`。
   - **调试器** — 选择`Xdebug`。

1. 选择&#x200B;**使用路径映射**。 在&#x200B;_文件/目录_&#x200B;窗格中，显示`serverName`的项目的根目录。

1. 在服务器&#x200B;**上的**&#x200B;绝对路径列中，单击&#x200B;**编辑**&#x200B;图标并根据环境添加设置。

   - 对于所有Starter环境和Pro集成环境，远程路径为`/app`。
   - 对于Pro暂存和生产环境：

      - 生产： `/app/<project_code>/`
      - 暂存： `/app/<project_code>_stg/`

1. 将[!DNL Xdebug]端口更改为`9000,9003`，或者可以在&#x200B;**PHP** > **调试** > **Xdebug** > **调试端口**&#x200B;面板中将其限制为仅`9000`。

1. 单击&#x200B;**应用**。

### 创建PHPStorm运行/调试配置

这使应用程序能够拥有正确的调试设置以处理来自Adobe Commerce应用程序的请求。

>[!VIDEO](https://video.tv.adobe.com/v/3437426?learn=on)

1. 打开PHPStorm应用程序，然后单击屏幕右上角的&#x200B;**[!UICONTROL Add Configuration]**。

1. 单击&#x200B;**[!UICONTROL Add new run configuration]**。

1. 选择&#x200B;**[!UICONTROL PHP Remote Debug]**&#x200B;选项。

   - 输入唯一但可识别的名称。
   - 选中[!UICONTROL Filter debug connection by IDE key]**复选框。
   - 选择您在[上一节](#configure-phpstorm-server)中创建的服务器。 如果尚未创建，可以立即创建一个，但请参阅设置指南的这一部分。
   - 在&#x200B;**[!UICONTROL IDE key(session id)]**&#x200B;文本字段中，输入大写字母中的`PHPSTORM`。 我们将在设置的其他部分使用此功能，因此保持不变非常重要。 如果选择其他字符串，则必须记得在设置和配置过程的其他位置使用它。

1. 单击&#x200B;**[!UICONTROL Apply]** > **[!UICONTROL OK]**。

### 设置端口转发

>[!VIDEO](https://video.tv.adobe.com/v/3437410?learn=on)

将`XDEBUG`连接从服务器映射到本地系统。 要执行任何类型的调试，必须将端口9000从云基础架构服务器上的Adobe Commerce转发到本地计算机。 请参阅以下部分之一：

- [Mac或UNIX上的端口转发](#port-forwarding-on-mac-or-unix)
- [Windows上的端口转发](#port-forwarding-on-windows)

#### Mac或UNIX上的端口转发®

**要在Mac或UNIX®环境中设置端口转发**，请执行以下操作：

1. 打开终端。

1. 使用SSH建立连接。

   ```bash
   ssh -R 9000:localhost:9000 <ssh url>
   ```

   使用`-v` (verbose)选项，以便每当套接字连接到要转发的端口时，它都会显示在终端中。

   如果显示“无法连接”或“无法侦听远程端口”错误，则可能是另一个活动的SSH会话持续存在于占用端口9000的服务器上。 如果未使用该连接，则可以终止该连接。

**要对连接进行故障排除**：

1. 使用SSH登录到远程集成、暂存或生产环境。

1. 查看SSH会话列表：`who`

1. 按用户查看现有SSH会话。 注意不要影响除您自己以外的用户！

   - 集成：用户名与`dd2q5ct7mhgus`类似
   - 暂存：用户名类似于`dd2q5ct7mhgus_stg`
   - 生产：用户名类似于`dd2q5ct7mhgus`

1. 对于早于您的用户会话，请查找伪终端(PTS)值，如`pts/0`。

1. 终止与PTS值对应的进程ID (PID)。

   ```bash
   ps aux | grep ssh
   kill <PID>
   ```

   示例响应：

   ```
   dd2q5ct7mhgus        5504  0.0  0.0  82612  3664 ?      S    18:45   0:00 sshd: dd2q5ct7mhgus@pts/0
   ```

   要终止连接，请输入带有进程ID (PID)的kill命令。

   ```bash
   kill 3664
   ```

#### Windows上的端口转发

要在Windows上设置端口转发（SSH隧道），必须配置Windows终端应用程序。 此示例逐步说明如何使用[Putty](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html)创建SSH通道。 您可以使用Cygwin等其他应用程序。 有关其他应用程序的更多信息，请参阅随这些应用程序提供的供应商文档。

**要使用Putty在Windows上设置SSH通道**：

1. 如果您尚未这样做，请下载[Putty](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html)。

1. 启动Putty。

1. 在类别窗格中，单击&#x200B;**会话**。

1. 输入以下信息：

   - **主机名（或IP地址）**&#x200B;字段：输入云服务器的[SSH URL](../development/secure-connections.md#connect-to-a-remote-environment)
   - **端口**&#x200B;字段：输入`22`

   ![设置Putty](../../assets/xdebug/putty-session.png)

1. 在&#x200B;_类别_&#x200B;窗格中，单击&#x200B;**连接** > **SSH** > **隧道**。

1. 输入以下信息：

   - **Source端口**&#x200B;字段：输入`9000`
   - **目标**&#x200B;字段：输入`127.0.0.1:9000`
   - 单击&#x200B;**远程**

1. 单击&#x200B;**添加**。

   ![在Putty中创建SSH通道](../../assets/xdebug/putty-tunnels.png)

1. 在&#x200B;_类别_&#x200B;窗格中，单击&#x200B;**会话**。

1. 在&#x200B;**保存的会话**&#x200B;字段中，输入此SSH通道的名称。

1. 单击&#x200B;**保存**。

   ![保存SSH通道](../../assets/xdebug/putty-session-save.png)

1. 要测试SSH通道，请单击&#x200B;**加载**，然后单击&#x200B;**打开**。

   如果显示“无法连接”错误，请验证以下各项：

   - 所有Putty设置均正确
   - 您在云基础架构上的专用Adobe Commerce SSH密钥所在的计算机上运行Putty

## 通过SSH访问Xdebug环境

要启动调试、执行设置等，您需要SSH命令来访问环境。 您可以通过[[!DNL Cloud Console]](../development/secure-connections.md#use-an-ssh-command)和项目电子表格获取此信息。

对于入门级环境和Pro集成环境，您可以使用以下`magento-cloud` CLI命令以通过SSH连接到这些环境：

```bash
magento-cloud environment:ssh --pipe -e <environment-ID>
```

要使用[!DNL Xdebug]，请按如下方式通过SSH连接到环境：

```bash
ssh -R <xdebug listen port>:<host>:<xdebug listen port> <SSH-URL>
```

例如，

```bash
ssh -R 9000:localhost:9000 pwga8A0bhuk7o-mybranch@ssh.us.magentosite.cloud
```

## 调试Pro暂存和生产环境

>[!NOTE]
>
>在Pro暂存和生产环境中，[!DNL Xdebug]始终可用，因为这些环境对[!DNL Xdebug]进行了特殊设置。 所有正常Web请求都被路由到没有[!DNL Xdebug]的专用PHP进程。 因此，这些请求会正常处理，并且不会在[!DNL Xdebug]加载时发生性能降级。 当发送具有[!DNL Xdebug]键的Web请求时，该请求被路由到加载了[!DNL Xdebug]的单独的PHP进程。

要专门在Pro计划暂存和生产环境中使用[!DNL Xdebug]，请创建单独的SSH通道和Web会话，只有您才有权访问。 此用法不同于典型访问，它仅向您提供访问权限，并不向所有用户提供访问权限。

您需要以下各项：

- SSH命令用于访问环境。 您可以通过[[!DNL Cloud Console]](../project/overview.md)或您的[!DNL Cloud Onboarding UI]获取此信息。
- 配置暂存和Pro环境时设置的`xdebug_key`值。

  通过使用SSH登录到主节点并执行：`xdebug_key`

  ```bash
  cat /etc/platform/*/nginx.conf | grep xdebug.sock | head -n1
  ```

**要设置到暂存或生产环境的SSH通道**：

1. 打开终端。

1. 清理集群中每个Web节点的所有SSH会话。

   ```bash
   ssh USERNAME@CLUSTER.ent.magento.cloud 'rm /run/platform/USERNAME/xdebug.sock'
   ```

1. 为群集的每个Web节点设置Xdebug的SSH通道。

   ```bash
   ssh -R /run/platform/USERNAME/xdebug.sock:localhost:9000 -N USERNAME@CLUSTER.ent.magento.cloud
   ```

>[!NOTE]
>
>要获得`USERNAME@CLUSTER.ent.magento.cloud`的正确值，请执行以下操作：
>- 方法1： magento-cloud CLI： `magento-cloud ssh --all`
>- 方法2： Commerce Console： https://CONSOLE-URL/ENVIRONMENT，单击`SSH v`下拉列表

**要使用环境URL**&#x200B;开始调试：

演示了用于启动远程GET会话的配置，以及调试参数。

>[!VIDEO](https://video.tv.adobe.com/v/3437417?learn=on)

1. 启用远程调试；访问浏览器中的站点，并将以下内容附加到URL中，`KEY`为`xdebug_key`的值。

   ```http
   ?XDEBUG_SESSION_START=KEY
   ```

   此步骤设置发送浏览器请求以触发[!DNL Xdebug]的Cookie。

1. 使用[!DNL Xdebug]完成调试。

1. 准备好结束会话后，使用以下命令删除Cookie并结束通过浏览器进行的调试，其中`KEY`为`xdebug_key`的值。

   ```http
   ?XDEBUG_SESSION_STOP=KEY
   ```

   >[!NOTE]
   >
   >不支持`POST`请求传递的`XDEBUG_SESSION_START`。

## 调试CLI命令

本节介绍如何调试CLI命令。

要调试CLI命令：

1. 通过SSH连接到要使用CLI命令调试的服务器。

1. 创建以下环境变量：

   ```bash
   export XDEBUG_CONFIG='PHPSTORM'
   ```

   ```bash
   export PHP_IDE_CONFIG="serverName=<name of the server that is configured in PHPSTORM>"
   ```

   这些变量将在SSH会话结束时删除。

1. 开始调试

   在入门环境和Pro集成环境中，运行CLI命令进行调试。
您可以添加运行时选项，例如：

   ```bash
   php -d xdebug.profiler_enable=On -d xdebug.max_nesting_level=9999 bin/magento cache:clean
   ```

   在Pro暂存和生产环境中，在调试CLI命令时，必须指定到[!DNL Xdebug] PHP配置文件的路径，例如：

   ```bash
   php -c /etc/platform/USERNAME/php.xdebug.ini bin/magento cache:clean
   ```

## 调试Web请求

以下步骤可帮助您调试Web请求。

1. 在&#x200B;_扩展_&#x200B;菜单中，单击&#x200B;**调试**&#x200B;以启用。

1. 右键单击，选择选项菜单，然后将IDE键设置为&#x200B;**PHPSTORM**。

1. 在浏览器上安装[!DNL Xdebug]客户端。 配置并启用它。

### 示例：Chrome设置

本节讨论如何使用[!DNL Xdebug]帮助程序扩展在Chrome中使用[!DNL Xdebug]。 有关适用于其他浏览器的[!DNL Xdebug]工具的信息，请参阅浏览器文档。

**要将Xdebug帮助程序与Chrome**&#x200B;一起使用：

1. 创建到云服务器的[SSH通道](#ssh-access-to-xdebug-environments)。

1. 从Chrome应用商店安装[Xdebug Helper扩展](https://chromewebstore.google.com/detail/eadndfjplgieldjbigjakmdgkmoaaaoc)。

1. 在Chrome中启用该扩展，如下图所示。

   ![在Chrome中启用Xdebug扩展](../../assets/xdebug/enable-chrome-ext.png)

1. 在Chrome中，右键单击Chrome工具栏中的绿色帮助程序图标。

1. 从弹出菜单中，单击&#x200B;**选项**。

1. 从&#x200B;_IDE密钥_&#x200B;列表中，单击&#x200B;**PhpStorm**。

1. 单击&#x200B;**保存**。

   ![Xdebug帮助程序选项](../../assets/xdebug/helper-options.png)

1. 打开PhpStorm项目。

1. 在顶部导航栏中，单击&#x200B;**开始收听**&#x200B;图标。

   如果未显示导航栏，请单击&#x200B;**查看** > **导航栏**。

1. 在PhpStorm导航窗格中，双击PHP文件进行测试。

## 调试本地代码

由于环境是只读的，因此您必须从环境或特定Git分支将代码拉到本地工作站才能执行调试。

您选择的方法由您决定。 您可以选择以下选项：

- 从Git中签出代码并运行`composer install`

  除非`composer.json`引用您无权访问的专用存储库中的包，否则此方法有效。 此方法可获取整个Adobe Commerce代码库。

- 复制`vendor`、`app`、`pub`、`lib`和`setup`目录

  此方法可使您拥有所有可以测试的代码。 根据您拥有的静态资产数量，这可能会导致传输时间较长，并包含大量文件。

- 仅复制`vendor`目录

  由于大多数代码位于`vendor`目录中，此方法可能会产生良好的测试，但不会测试整个代码库。

**要压缩文件并将它们复制到本地计算机**：

1. 使用SSH登录到远程环境。

1. 压缩文件。

   ```bash
   tar -czf /tmp/<file-name>.tgz <directory list>
   ```

   例如，要仅压缩`vendor`目录：

   ```bash
   tar -czf /tmp/vendor.tgz vendor
   ```

1. 在本地环境中，使用PhpStorm压缩文件。

   ```bash
   cd <phpstorm project root dir>
   ```

   ```bash
   rsync <SSH-URL>:/tmp/<file-name>.tgz .
   ```

   ```bash
   tar xzf <file-name>.tgz
   ```
