---
title: 管理磁盘空间
description: 了解如何使用命令行界面管理磁盘空间。
feature: Cloud, Storage
exl-id: 1d13dc4e-56eb-4153-a8b1-48d2263ebc4c
source-git-commit: b8cabaad4b7805858563cecbe5ffc2fdb9aeac58
workflow-type: tm+mt
source-wordcount: '716'
ht-degree: 0%

---

# 管理磁盘空间

您可以在云基础架构合同上的Adobe Commerce中和[帐户页面](https://accounts.magento.cloud/user)上找到您的云项目的总存储容量。 您帐户中的每个项目卡都显示了&#x200B;_环境_&#x200B;的数量、_存储_&#x200B;容量（以GB为单位）以及&#x200B;_用户_&#x200B;的数量。 或者，您可以使用以下Cloud命令：

```bash
magento-cloud subscription:info | grep storage
```

示例响应：

```
| storage              | 51200
```

当Pro生产或暂存环境达到或超过存储容量的95%时，云基础架构监控工具会触发支持警报，通知您存储容量的自动增加。

通知示例：

>[!BEGINSHADEBOX]

_“我们的监视功能检测到您的群集(project-id-environment)上的文件存储已接近满。 磁盘使用率当前处于关键使用率级别，剩余使用率不到1 GiB。 共享存储卷当前正从60 GiB升级到70 GiB，以使您的服务保持正常运行。 请检查生产和暂存文件的使用情况，以查看是否可以清除一些空间。&quot;_

>[!ENDSHADEBOX]

>[!TIP]
>
>Adobe建议您定期监控存储容量，并将其维持在90%以下，以避免此类自动增加。 分配后，Pro暂存和生产的存储增加是永久性的，无法恢复。

## 检查集成环境

您可以使用`magento-cloud` CLI检查集成环境的磁盘空间使用情况。

**要检查大约的磁盘空间使用率**：

```bash
magento-cloud db:size
```

示例响应：

```
Checking database service mysql...

+----------------+-----------------+--------+
| Allocated disk | Estimated usage | % used |
+----------------+-----------------+--------+
| 2.0 GiB        | 193.3 MiB       | ~ 9%   |
+----------------+-----------------+--------+
```

所有挂载都共享一个磁盘。 您可以使用`magento-cloud` CLI检查装载的磁盘空间使用情况。

**要检查挂载的大致磁盘空间使用率**：

```bash
magento-cloud mount:size
```

示例响应：

```
Checking disk usage for all mounts on <project>-<environment>-mymagento@ssh.us.magento.cloud...

+------------+-----------+---------+-----------+-----------+--------+
| Mount(s)   | Size(s)   | Disk    | Used      | Available | % Used |
+------------+-----------+---------+-----------+-----------+--------+
| app/etc    | 184 KiB   | 1.9 GiB | 481.3 MiB | 1.4 GiB   | 24.7%  |
| pub/media  | 128 KiB   |         |           |           |        |
| pub/static | 158.2 MiB |         |           |           |        |
| var        | 316.7 MiB |         |           |           |        |
+------------+-----------+---------+-----------+-----------+--------+
```

## 检查专用群集

对于Pro暂存环境和生产环境，您可以使用`disk free`命令检查每个环境中的磁盘空间使用情况，该命令报告文件系统使用的磁盘空间量。 必须使用SSH登录到远程环境。

```bash
df -h
```

`-h`选项使用人类可读的格式（KB、MB或GB）显示报告。

在以下示例响应中，`/data/exports`装载显示介质的磁盘空间，`/data/mysql/`装载显示数据库的磁盘空间：

```
Filesystem                                    Size  Used Avail Use% Mounted on
udev                                           16G     0   16G   0% /dev
tmpfs                                         3.2G  9.1M  3.2G   1% /run
/dev/xvda1                                     59G  8.9G   48G  16% /
tmpfs                                          16G   36K   16G   1% /dev/shm
tmpfs                                         5.0M     0  5.0M   0% /run/lock
tmpfs                                          16G     0   16G   0% /sys/fs/cgroup
/dev/xvdj                                     9.8G  2.3G  7.6G  23% /data/mysql
/dev/xvdi                                     9.8G  491M  9.3G   5% /data/exports
192.168.5.5:/shared                           9.8G  591M  9.3G   6% /mnt/shared
/dev/loop0                                     91M   91M     0 100% /app/project
192.168.5.5:/shared/project/var         9.8G  591M  9.3G   6% /app/project/var
192.168.5.5:/shared/project/app/etc     9.8G  591M  9.3G   6% /app/project/app/etc
192.168.5.5:/shared/project/pub/media   9.8G  591M  9.3G   6% /app/project/pub/media
192.168.5.5:/shared/project/pub/static  9.8G  591M  9.3G   6% /app/project/pub/static
```

可以通过指定目录来限制响应。 例如：

```bash
df -h var/
```

示例响应：

```
Filesystem                                    Size  Used Avail Use% Mounted on
192.168.5.5:/shared/project/var         9.8G  591M  9.3G   6% /app/project/var
```

## 分配磁盘空间

两个[配置文件](../environment/overview.md)控制云环境中的磁盘空间分配： `.magento.app.yaml`文件和`.magento/services.yaml`文件。 每个文件都包含`disk`属性，该属性定义相应配置的磁盘大小值（以MB为单位）。 您只能在Pro集成和Starter环境中更改磁盘空间分配。

>[!IMPORTANT]
>
>对于Pro生产和暂存环境，您必须[提交Adobe Commerce支持票证](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket)以更改磁盘空间分配。 Pro生产和暂存环境的大小只能按特定的时间间隔增加，因此，根据您当前的磁盘空间使用情况，支持人员可能建议将磁盘空间分配至少增加10 GB。 分配后，无法恢复Pro暂存和生产中的存储增长。 无法在资源之间重新分配或重新分配存储。 要增加更多文件存储空间，请减少分配给MySQL的磁盘空间。

### 应用程序磁盘空间

`.magento.app.yaml`文件控制应用程序可用的[永久磁盘空间](../application/properties.md#disk)。

**增加应用程序的磁盘空间**：

1. 在本地开发环境中，打开`.magento.app.yaml`配置文件。

1. 为`disk`属性设置一个新值（以MB为单位）。

   ```yaml
   disk: <value-mb>
   ```

1. 在文件中保存更改。

1. 添加、提交和推送代码更改。

   ```bash
   git add .magento.app.yaml && git commit -m "Increase disk space for application" && git push origin <branch-name>
   ```

   将更新的YAML文件推送到远程环境后，更改将生效。

### 服务磁盘空间

`.magento/services.yaml`文件控制每个服务（如MySQL和Redis）的可用磁盘空间。

**增加服务的磁盘空间**：

1. 在本地开发环境中，打开`.magento/services.yaml`配置文件。

1. 在文件中添加或查找服务。 有关配置服务的详细信息，请参阅[&#128279;](../services/services-yaml.md)。

1. 为磁盘属性设置一个新值（以MB为单位）。

   ```yaml
   <name>:
       type: <service-name>:<service-version>
       disk: <value-mb>
   ```

1. 在文件中保存更改。

1. 添加、提交和推送代码更改。

   ```bash
   git add .magento/services.yaml && git commit -m "Increase disk space for service" && git push origin <branch-name>
   ```

   将更新的YAML文件推送到远程环境后，更改将生效。

## 监视磁盘空间

在Pro Production环境中，您可以使用New Relic的“Adobe Commerce的托管警报”警报策略监控磁盘空间和其他性能指标。 有关详细信息，请参阅[使用托管警报监视性能](../monitor/investigate-performance.md#monitor-performance-with-managed-alerts)。 有关进一步指导，请参阅[解决数据库性能问题的最佳实践](https://experienceleague.adobe.com/docs/commerce-operations/implementation-playbook/best-practices/maintenance/resolve-database-performance-issues.html)。

## 无剩余空间

构建缓存会随着时间的推移而增长。 如果收到状态为`No space left on device`的警告，请尝试清除生成缓存并重新部署：

```bash
magento-cloud project:clear-build-cache
```
