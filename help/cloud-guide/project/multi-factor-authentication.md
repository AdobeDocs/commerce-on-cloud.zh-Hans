---
title: 为SSH访问启用多重身份验证
description: 了解如何在云基础架构环境中管理对Adobe Commerce的SSH访问的身份验证要求。
feature: Cloud, Security
topic: Security
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '1051'
ht-degree: 0%

---

# 为SSH访问启用多重身份验证

为了提高安全性，云基础架构上的Adobe Commerce提供了多重身份验证(MFA)实施，以管理对云环境的SSH访问的身份验证要求。

在项目上启用MFA后，所有具有SSH访问权限的用户帐户都需要双重身份验证(TFA)代码或API令牌和SSH证书才能访问环境。

>[!NOTE]
>
>默认情况下，云项目上未启用MFA。 云基础架构项目Adobe Commerce的帐户所有者必须[提交Adobe Commerce支持票证](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html?lang=zh-Hans#submit-ticket)才能启用它。 启用MFA后，所有用户必须在其Adobe Commerce上的云基础架构帐户中启用双重身份验证(TFA)，才能通过SSH访问项目环境。

## 用于SSH访问的证书

MFA允许用户使用由Adobe云认证器API生成的短期SSH证书交换OAUTH访问令牌。 如果用户具有管理员或参与者角色、有效的SSH密钥和有效的TFA代码或API令牌，则云基础架构上的Adobe Commerce将使用这些凭据生成临时SSH证书。 证书过期时间被设置为一小时，但在当前会话期间会自动刷新。

使用MFA登录项目后，用户必须使用`magento-cloud` CLI来生成SSH证书：

```bash
magento-cloud ssh-cert:load
```

`ssh-cert:load`命令会生成SSH证书并将其安装到本地用户的SSH代理中。

### 登录时自动生成证书

您可以配置本地环境，以便在向`magento-cloud` CLI进行身份验证时自动生成SSH证书。

**要将SSH证书自动生成添加到`magento-cloud` CLI配置**：

1. 在本地工作站上，在主目录的`.magento-cloud`文件夹中创建一个名为`config.yaml`的文件（如果该文件不存在）。

   ```bash
   touch ~/.magento-cloud/config.yaml
   ```

1. 将以下配置添加到`config.yaml`文件。

   ```yaml
   api:
      auto_load_ssh_cert: true
   ```

1. 使用`magento-cloud` CLI再次进行身份验证：

   >注销：

   ```bash
   magento-cloud logout
   ```

   >登录：

   ```bash
   magento-cloud login
   ```

   >按照响应操作：

   ```
   Please open the following URL in a browser and log in:
   http://127.0.0.1:5000
   
   Help:
     Leave this command running during login.
     If you need to quit, use Ctrl+C.
   
     To log in using an API token, run: magento-cloud auth:api-token-login
   
   Login information received. Verifying...
   You are logged in.
   
   Generating SSH certificate...
   A new SSH certificate has been generated.
   It will be automatically refreshed when necessary.
   The certificate is included in your SSH configuration: /Users/<user-name>/.ssh/config
   ```

## 使用带有TFA的SSH连接到环境

在项目中启用MFA后，您必须先在帐户中启用TFA，然后才能使用SSH连接到远程环境。 请参阅[启用TFA](user-access.md#enable-tfa-for-cloud-accounts)。

>[!BEGINSHADEBOX]

**先决条件：**

对于通过MFA强制启用的项目，SSH访问需要以下权限和帐户设置：

- [管理员或参与者对环境的访问权限](user-access.md)
- [帐户上配置的SSH访问密钥](../development/secure-connections.md#add-an-ssh-public-key-to-your-account)
- [TFA分期付款启用](user-access.md#enable-tfa-for-cloud-accounts)

>[!ENDSHADEBOX]

**要使用SSH与TFA用户帐户凭据连接**：

1. 登录到[您的帐户](https://console.adobecommerce.com)。

1. 在本地工作站上，使用`magento-cloud` CLI生成SSH证书。

   ```bash
   magento-cloud ssh-cert:load
   ```

   > 示例响应：

   ```
   Generating SSH certificate...
     Expires at: 2020-07-13T15:28:13-04:00
     Multi-factor authentication: verified
     Mode: interactive
   The certificate will be automatically refreshed when necessary.
   Checking SSH configuration file: /Users/<user-name>/.ssh/config
   Do you want to update the file automatically? [Y/n] Y
   Configuration file updated successfully: /Users/<user-name>/.ssh/config
   ```

1. 使用SSH连接到远程环境。

   ```bash
   ssh abcdef7uyxabce-master-7rqtwti--mymagento@ssh.us-5.magento.cloud
   ```

   ```
    __  __                   _          ___ _             _
   |  \/  |__ _ __ _ ___ _ _| |_ ___   / __| |___ _  _ __| |
   | |\/| / _` / _` / -_) ' \  _/ _ \ | (__| / _ \ || / _` |
   |_|  |_\__,_\__, \___|_||_\__\___/  \___|_\___/\_,_\__,_|
               |___/
   
    Welcome to Magento Cloud.
   
    This is environment master-7rqtwti
    of project abcdef7uyxabce.
   
   web@mymagento.0:~$
   ```

## 使用带有TFA的SSH管理源代码

在云基础架构项目中管理Adobe Commerce的源代码时，您可以使用SSH对项目的Git存储库进行身份验证。 如果您的项目启用了MFA实施，则必须先生成SSH证书，然后才能使用Git存储库执行命令行操作。

**要使用SSH与TFA用户帐户凭据连接**：

1. 登录到[您的帐户](https://console.adobecommerce.com)并使用TFA进行身份验证。

   >[!NOTE]
   >
   >如果您的帐户未启用TFA，则必须启用它。 请参阅[在云帐户上启用TFA](user-access.md#enable-tfa-for-cloud-accounts)。

1. 在本地工作站上，使用`magento-cloud` CLI生成SSH证书。

   ```bash
   magento-cloud ssh-cert:load
   ```

   > 示例响应：

   ```
   Generating SSH certificate...
     Expires at: 2020-07-13T15:28:13-04:00
     Multi-factor authentication: verified
     Mode: interactive
   The certificate will be automatically refreshed when necessary.
   Checking SSH configuration file: /Users/<user-name>/.ssh/config
   Do you want to update the file automatically? [Y/n] Y
   Configuration file updated successfully: /Users/<user-name>/.ssh/config
   ```

1. 克隆项目环境的Git存储库：

   ```bash
   git clone --branch integration abcdef7uyxabce@git.us-3.magento.cloud:abcdef7uyxabce.git myproject
   ```

   > 示例响应：

   ```
   Cloning into 'myproject'...
   Connection to git.us-3.magento.cloud port 22 [tcp/ssh] succeeded!
   remote: counting objects: 22, done.
   Receiving objects: 100% (22/22), 82.42 KiB | 16.48 MiB/s, done.
   ```

## 使用带有API令牌的SSH连接到环境

在项目上启用MFA后，需要对云环境进行SSH访问的自动化流程需要API令牌。 您可以通过具有项目管理员或参与者访问权限的云基础架构帐户上的Adobe Commerce来生成令牌。

使用API令牌进行身份验证仍需要生成SSH证书。 自动化流程还必须自动生成SSH证书。

>[!BEGINSHADEBOX]

**先决条件：**

- [管理员或参与者对云基础架构环境上Adobe Commerce的访问权限](user-access.md)
- [帐户中可用的有效API令牌](user-access.md#create-an-api-token)

>[!ENDSHADEBOX]

**要使用SSH与API令牌凭据连接**：

1. 使用API密钥身份验证登录到云项目。

   ```bash
   magento-cloud auth:api-token
   ```

1. 在提示符下，输入有效API令牌的值。

   ```
   Please enter an API token:
   >
   
   The API token is valid.
   You are logged in.
   ```

### 示例：自动SSH脚本

存储API令牌有两种选项。

>[!NOTE]
>
>如果存储了API令牌，则`magento-cloud` CLI会自动进行身份验证，而无需执行`magento-cloud login`命令。

**选项1**：创建环境变量以存储API令牌

将令牌写入bash_profile

```bash
echo "export MAGENTO_CLOUD_CLI_TOKEN=<your api token>" >> ~/.bash_profile
```

**选项2**：将令牌添加到`config.yaml`文件

1. 在本地工作站上，在主目录的`.magento-cloud`文件夹中创建一个名为`config.yaml`的文件（如果该文件不存在）。

   ```bash
   touch ~/.magento-cloud/config.yaml
   ```

1. 将以下配置添加到`config.yaml`文件。

   ```yaml
   api:
      token: <your api token>
   ```

>示例bash脚本

```shell
#!/bin/bash
magento-cloud ssh-cert:load
ssh abcdef7uyxabce-master-7rqtabc--mymagento@ssh.us-3.magento.cloud "tail -n 10 ~/var/log/cloud.log"
```

## 故障排除

使用以下信息解决由于身份验证错误（如`access requires MFA`或`permission denied`）导致的SSH连接请求失败。

### 您的请求未提供有效证书

如果您的请求未提供有效的证书，则会显示一条与以下内容类似的消息：

```
to Hello user-test (UUID: abaacca12-5cd1-4b123-9096-411add578998), you successfully
authenticated, but could not connect to service abcdef7uyxabce-master-7rqtabc--mymagento@ssh.us-3.magento.cloud:>
(reason: access requires MFA)
```

尝试以下故障排除过程来解决连接问题：

- 验证帐户TFA配置
- 再次进行身份验证，然后重新加载证书

**验证TFA配置和身份验证**：

1. 登录到[您的帐户](https://console.adobecommerce.com)。

1. 在右上角帐户菜单中，单击&#x200B;**[!UICONTROL My Profile]**。

1. 在&#x200B;_我的个人资料_&#x200B;页面上，单击&#x200B;**[!UICONTROL Security]**&#x200B;选项卡。

   如果启用了TFA，则“安全性”部分将提供用于管理TFA配置的选项。

1. 如果未设置TFA，请单击&#x200B;**[!UICONTROL Set up application]**&#x200B;并按照说明启用它。 请参阅[启用TFA](user-access.md#enable-tfa-for-cloud-accounts)。

1. 如果配置了TFA，请尝试再次进行身份验证。

**验证并重新加载SSH证书**：

1. 使用`magento-cloud` CLI再次进行身份验证：

   ```bash
   magento-cloud logout
   ```

   ```bash
   magento-cloud login
   ```

1. 重新加载SSH证书：

   ```bash
   magento-cloud ssh-cert:load
   ```

### 权限被拒绝

如果SSH密钥缺失或无效，则SSH连接请求返回`Permission denied (publickey)`错误。

```
Hello user-test (UUID: abaacca12-5cd1-4b123-9096-411add578998), you successfully authenticated, but could not connect to service oh2wi6klp5ytk-mc-35985-integration-nnulm4a--mymagento (reason: service doesn't exist or you do not have access to it)
oh2wi6klp5ytk-mc-35985-integration-nnulm4a--mymagento@ssh.eu-3.magento.cloud: Permission denied (publickey).
```

要解决此问题，请将SSH密钥添加到当前会话，或更新SSH配置文件以自动加载SSH密钥。 请参阅[添加公共SSH密钥](../development/secure-connections.md#add-an-ssh-public-key-to-your-account)。

### 无法在没有MFA的情况下访问项目

如果您对启用了多重身份验证(MFA)的项目进行身份验证，则在连接到其他不需要MFA的项目时，可能会收到以下错误：

```bash
ssh abcdef7uyxabce-master-7rqtabc--mymagento@ssh.us-3.magento.cloud
```

示例响应：

```
abcdef7uyxabce-master-7rqtabc--mymagento@ssh.us-3.magento.cloud: Permission denied (publickey).
```

在SSH证书生成过程中，`magento-cloud` CLI会向本地环境添加一个额外的SSH密钥。 如果您的本地SSH配置不包括用于项目访问的SSH密钥，则默认使用该密钥。

**要将SSH密钥添加到本地配置**：

1. 创建`config`文件（如果该文件不存在）。

   ```bash
   touch ~/.ssh/config
   ```

1. 添加`IdentityFile`配置。

   ```yaml
   Host *
     IdentityFile ~/.ssh/id_rsa
   ```

>[!NOTE]
>
>您可以通过向配置中添加多个`IdentityFile`条目来指定多个SSH密钥。
