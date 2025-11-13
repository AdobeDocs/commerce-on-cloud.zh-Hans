---
title: Cloud Docker包
description: 请参阅Cloud Docker包的最新改进列表。
feature: Cloud, Docker, Release Notes
recommendations: noDisplay, catalog
last-substantial-update: 2025-08-07T00:00:00Z
exl-id: 95cf4f30-6bce-4bac-8e11-cfe53cac2c70
source-git-commit: 16d5577da8841c2f65f9b5298beaa7fb84a1ab47
workflow-type: tm+mt
source-wordcount: '3806'
ht-degree: 0%

---

# Cloud Docker包

[`magento/magento-cloud-docker`](https://github.com/magento/magento-cloud-docker)包提供了将Adobe Commerce部署到本地云环境的功能和Docker映像。 这些发行说明介绍了此包的最新改进，此包是[Cloud Tools Suite for Commerce](cloud-tools-suite.md)的组件。

`magento/magento-cloud-docker`包使用以下版本序列： `<major>.<minor>.<patch>`

发行说明包括：

- ![新图标](../../assets/new.svg)新功能
- ![修复图标](../../assets/fix.svg)修复和改进

<!--Add release notes below-->

## v1.4.6 {#latest}

发行日期： 2025年11月13日

- ![修复图标](../../assets/fix.svg) **Symfony包** — 已添加对最新Symfony YAML包的支持。<!-- MCLOUD-14020 -->

## v1.4.5

发行日期： 2025年10月8日

- ![新图标](../../assets/new.svg) **ActiveMQ** — 已添加功能测试的cloud-docker中的ActiveMQ支持。<!-- MCLOUD-13771 -->

## v1.4.4

发行日期： 2025年8月7日

- ![修复图标](../../assets/fix.svg) **PHP 8.4** — 已添加PHP 8.4测试。<!-- MCLOUD-13311 -->
- ![修复图标](../../assets/fix.svg) **FTP扩展** — 已添加FTP扩展的修复。<!-- MCLOUD-13843 -->
- ![新图标](../../assets/new.svg) **Opensearch3图像** — 添加了对Opensearch3的支持。<!-- MCLOUD-13766 -->
- ![新图标](../../assets/new.svg) **Opensearch3测试** — 已添加Opensearch3的PHP 8.4测试。<!-- MCLOUD-13768 -->
- ![新图标](../../assets/new.svg) **Valkey** — 已添加对Valkey.<!-- MCLOUD-13558 -->的支持

## v1.4.3

发布日期： 2025年6月3日

- ![修复图标](../../assets/fix.svg) **改进与2.4.8的兼容性** — 更新了第三方库以更好地与2.4.8<!-- MCLOUD-13707	 - -->兼容

## v1.4.2

发行日期： 2025年4月7日

- ![新图标](../../assets/new.svg) **PHP 8.4** — 已添加`php-cli` 8.4和`php-fpm` 8.4图像。


## v1.4.1

发行日期： 2025年2月6日

- ![新图标](../../assets/new.svg) **PHP 8.4** — 添加了对PHP 8.4的支持。


## v1.4.0

发行日期： 2024年10月7日

- ![修复图标](../../assets/fix.svg) **重构的代码** — 删除了对旧PHP版本(7.4、7.3、7.2)以及相关库和图像的支持。

## v1.3.7

发行日期： 2024年4月8日

- ![新图标](../../assets/new.svg) **PHP** — 添加了对PHP 8.3和PHP 8.3映像的支持。
- ![新图标](../../assets/new.svg) **Nginx** — 已添加图像nginx v. 1.24。
- ![新图标](../../assets/new.svg) **Opensearch** — 已添加OpenSearch v. 2.12、1.3。
- ![新图标](../../assets/new.svg) **Composer** — 已将Composer版本更新为2.2.23。

## v1.3.6

发行日期： 2023年7月31日

- ![新图标](../../assets/new.svg) **已添加新的服务版本**—OpenSearch 2.5。
- ![新图标](../../assets/new.svg) **启用编辑器缓存** — 现在，您可以扩展Docker配置以在启动Docker容器时启用编辑器清除缓存。 请参阅[Cloud Docker for Commerce](https://developer.adobe.com/commerce/cloud-tools/docker/configure/extend-docker-configuration/)指南中的&#x200B;_扩展Docker配置_。

## v1.3.5

发行日期： 2023年3月10日

- ![新图标](../../assets/new.svg) **ionCube** — 已为PHP 8.1映像添加ionCube扩展。
- ![新图标](../../assets/new.svg) **已添加新的服务版本**—OpenSearch 2.3和2.4、PHP 8.2、Varnish 7.1.1。
- ![新图标](../../assets/new.svg) **对PHP 8.2**&#x200B;的增强支持 — 修复了某些PHP 8.2.x版本存在的兼容性问题，以支持Commerce 2.4.6。
- ![修复图标](../../assets/fix.svg) **编辑器问题** — 修复了在Docker容器中更新Composer版本后出现的问题。

## v1.3.4

发行日期： 2022年10月27日

- ![新图标](../../assets/new.svg) **已添加新的清漆图像** — 已添加清漆6.5、7.0和7.1的图像。<!-- MCLOUD-7879 -->

## v1.3.3

发行日期： 2022年9月13日

- ![新图标](../../assets/new.svg) **Apple M1 (ARM64)支持** — 已添加对Docker映像的更改，以支持Apple M1 (ARM64)体系结构。<!-- MCLOUD-7989-2 MCLOUD-7989 -->
- ![修复图标](../../assets/fix.svg) **Mailhog** — 修复了在开发人员模式下邮件服务未捕获电子邮件的问题。<!-- MCLOUD-8643 -->
- ![修复图标](../../assets/fix.svg) **init-docker.sh** — 修复了`init-docker.sh`脚本中的服务版本验证器。<!-- MCLOUD-8765 -->

## v1.3.2

发行日期： 2022年3月31日

- ![新图标](../../assets/new.svg) **已添加Elasticsearch 7.10图像**<!-- MCLOUD-8548 -->

## v1.3.1

发行日期： 2022年3月10日

- ![新图标](../../assets/new.svg) **支持PHP 8.1** — 添加了对PHP 8.1的支持。
- ![新图标](../../assets/new.svg) **OpenSearch** — 已添加OpenSearch版本1.1和1.2的图像。
- ![新图标](../../assets/new.svg) **Composer 2.1** — 在PHP 8.x映像中默认设置composer 2.1.x。
- ![新图标](../../assets/new.svg) **PHP映像改进**—

   - 添加了PHP 8.1图像
   - 已升级xDebug版本3.1.2
   - 已升级xmlrpc 1.0.0RC3

- ![修复图标](../../assets/fix.svg) **Elasticsearch和OpenSearch改进** — 改进了Elasticsearch和OpenSearch Dockerfiles；删除了Elasticsearch 5.2图像。
- ![修复图标](../../assets/fix.svg) **Na扩展** — 默认情况下在所有PHP映像中启用了`sodium`扩展。
- ![修复图标](../../assets/fix.svg) **Composer缓存卷** — 修复了Composer缓存卷具有缓存的Composer包的路径。
- ![修复图标](../../assets/fix.svg) **nginx中的内存限制** — 修复了NGINX映像中的内存限制。

## v1.3.0

发行日期： 2021年10月25日

- ![修复图标](../../assets/fix.svg) **改进开发人员模式工作流** — 以前，您需要在生成和部署步骤中指定模式。 现在，`--mode`步骤中的`build`选项决定了稍后`deploy`步骤中的模式。 不再需要设置部署后的模式。 查看[开发人员模式](https://developer.adobe.com/commerce/cloud-tools/docker/deploy/developer-mode/).<!-- ACMP-1086 -->
- ![修复图标](../../assets/fix.svg) **只读文件系统的改进**—<!-- ACMP-1106 -->
   - 修复了启动邮件配置的PHP容器时出现的问题。
   - 可以在INI文件中使用环境变量。
   - 确保PHP入口点不需要写入权限。
- ![修复图标](../../assets/fix.svg) **更新节点** — 更新捆绑的节点版本；在PHP-CLI映像中安装节点时，它现在使用当前的LTS版本。<!-- ACMP-1539 -->
- ![修复图标](../../assets/fix.svg) **更新Symfony** — 已更新Symfony配置依赖项以便与Adobe Commerce 2.4.4兼容。<!-- ACMP-1533 -->

## v1.2.4

发行日期： 2021年7月29日

- ![新图标](../../assets/new.svg) **新`Zookeeper`容器** — 添加了[Zookeeper容器](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service/#zookeeper-container)，用于管理未部署到Cloud Infrastructure上的Adobe Commerce的项目的锁定提供程序配置。<!--MCLOUD-8000-->

- ![新图标](../../assets/new.svg) **已添加对Composer 2.0的支持。** — 已将Composer 2.0版本添加到Composer配置文件以支持从Composer 1.0进行升级，该版本即将终止。<!--MCLOUD-8003-->

## v1.2.3

发布日期： 2021年6月14日

- ![新图标](../../assets/new.svg) **添加了PHP 8.0** — 已将PHP更新为版本8.0，允许您利用PHP 8.0包含的所有新功能和优化。<!--MCLOUD-7941-->
- ![新图标](../../assets/new.svg) **已更新为Varnish 6.6和Elasticsearch 7.11.2** — 以下链接提供有关[Varnish缓存6.6](https://varnish-cache.org/releases/rel6.6.0.html#rel6-6-0)和Elasticsearch 7.11.2.<!--MCLOUD-7921-->的发行信息
- ![新图标](../../assets/new.svg) **为PHP 7.4映像`ioncube`添加了**&#x200B;扩展 — 在最初从PHP 7.3升级到PHP 7.4后，`ioncube`扩展已重新添加到PHP 7.4映像。 *[由mattskr](https://github.com/magento/magento-cloud-docker/pull/314)提交。*<!--PR #314-->
- ![新图标](../../assets/new.svg) **添加了一个文件同步选项：`manual-native`** — `manual-native`文件同步选项提供了对同步的手动控制，为macOS和Windows环境提供了最佳性能。 阅读有关在`manual-native`开发人员模式[中使用](https://developer.adobe.com/commerce/cloud-tools/docker/deploy/developer-mode/)选项以及在Docker开发人员环境中同步数据[的信息。](https://developer.adobe.com/commerce/cloud-tools/docker/setup/synchronize-data/#file-synchronization-options)<!--MCLOUD-7977-->
- ![新图标](../../assets/new.svg) **已从`up`和`down`命令中删除卷删除** — 已从`--volume`和`bin/magento-docker up`命令中删除`bin/magento-docker down`选项，替换为带有数据丢失警告的新`bin/magento-docker init`命令。 此更改有助于防止意外数据丢失。 *[由joeshelton-wagento提交](https://github.com/magento/magento-cloud-docker/pull/319)。*<!--PR #319-->
- ![修复图标](../../assets/fix.svg) **已更新生成的证书的`CN`值** — 已从Dockerfile中删除硬编码的`CN`值。 此值创建了一个证书错误(`NET::ERR_CERT_INVALID`)，导致忽略了`--host`命令的`ece-docker build:compose`选项。<!--MCLOUD-7934-->

## v1.2.2

发行日期： 2021年4月20日

- ![新图标](../../assets/new.svg) **已更新`host.docker.internal`以独立于平台** — 您现在可以为Ubuntu、Windows和macOS创建相同的Docker撰写脚本。 在Ubuntu上使用Xdebug不再需要单独的环境变量。 由Igor Vitol[提交的](https://github.com/magento/magento-cloud-docker/pull/299)修复。<!--Issue #298-->
- ![新图标](../../assets/new.svg) **已更新init-docker.sh** — 已将`mounts`对象添加到`MAGENTO_CLOUD_APPLICATION`环境变量。 由Chiranjevi提交的[修复](https://github.com/magento/magento-cloud-docker/pull/299)。<!--Issue #299-->
- ![新图标](../../assets/new.svg) **已更新init-docker.sh** — 已使用PHP 7.4和Cloud Docker 1.2.1版本更新`init-docker.sh`脚本。 由Adarsh Manickam提交的[修复](https://github.com/magento/magento-cloud-docker/pull/300)。<!--Issue #300-->
- ![新图标](../../assets/new.svg) **默认情况下启用** — 默认情况下在PHP Docker映像中启用`sodium` PHP扩展。<!--MCLOUD-7548-->
- ![新图标](../../assets/new.svg) **`custom-registry`选项** — 已将`--custom-registry`选项添加到`php ./vendor/bin/ece-docker build:compose`命令以使用您自己的图像注册表。<!--MCLOUD-7476-->

  ```bash
  ./vendor/bin/ece-docker build:compose --custom-registry=my-registry.example.com
  ```

- ![新图标](../../assets/new.svg) **已删除旧的Elasticsearch版本** — 已从Elasticsearch映像中移除Elasticsearch版本1.7和2.4。<!--MCLOUD-7504-->
- ![新图标](../../assets/new.svg) **自动生成NGINX证书** — 已从NGINX映像中删除现有证书。 现在，每个新部署都会自动生成NGINX证书，以提高安全性。<!--MCLOUD-7396-->
- ![修复图标](../../assets/fix.svg) **已启用`opcache.validate_timestamps`** — 在开发人员模式下默认启用`opcache.validate_timestamps` PHP设置。 启用此设置修复了在Docker中无法识别文件系统更改的问题。<!--MCLOUD-7466-->
- ![修复图标](../../assets/fix.svg) **修复`build:custom:compose`** — 修复了`build:custom:compose`命令，以便在生成过程中无法覆盖文件时引发错误。 引发错误可防止`docker-compose up`使用错误文件的情况。<!--MCLOUD-7457-->
- ![修复图标](../../assets/fix.svg) **修复`--sync_engine="native"`选项** — 修复了在生产模式(`--mode="production"`)中，`--sync_engine="native"`选项不会在`docker.composer.yml`文件中为本地文件夹创建任何条目的问题。<!--MCLOUD-7254-->
- ![修复图标](../../assets/fix.svg) **修复的服务版本验证错误** — 已将[!DNL RabbitMQ]、Elasticsearch和其他服务的服务版本添加到`type`变量中的`MAGENTO_CLOUD_RELATIONSHIP`属性。 将这些版本添加到`relationships`变量修复了在部署阶段发生的验证错误。<!--MCLOUD-7572-->

## v1.2.1

发行日期： 2020年12月21日

- ![新图标](../../assets/new.svg) **NGINX命令选项** — 已添加生成命令选项以更改TLS和Web服务的NGINX `worker_processes`和NGINX `worker_connections`的数量。 `worker_process`参数保留将值设置为`auto`的功能。 示例： <!--MCLOUD-7259-->

  ```bash
  ./vendor/bin/ece-docker build:compose --nginx-worker-processes=2
  ./vendor/bin/ece-docker build:compose --nginx-worker-connections=2048
  ```

- ![新图标](../../assets/new.svg) **TLS命令选项** — 已添加生成命令选项，以创建不带TLS服务的配置。 示例： <!--MCLOUD-7259-->

  ```bash
  ./vendor/bin/ece-docker build:compose --no-tls
  ```

- ![新图标](../../assets/new.svg) **NGINX内存消耗** — 已减少NGINX进程为TLS和Web服务所消耗的内存。<!--MCLOUD-7259-->

- ![新图标](../../assets/new.svg) **Blackfire** — 在Cloud Docker映像中默认禁用了Blackfire PHP扩展。

- ![修复图标](../../assets/fix.svg) **PHP-FPM容器** — 通过将`WEB_PORT`从`80`更改为`8080`修复了PHP-FPM容器运行状况检查。<!--MCLOUD-7232-->

- ![修复图标](../../assets/fix.svg) **无效的卷命名** — 修复了在开发人员模式下无效的卷命名错误。<!--MCLOUD-7442-->

- ![修复图标](../../assets/fix.svg) **NGINX上游端口** — 已更新Docker NGINX 1.19映像以使用端口8080以避免无限循环。 由Adarsh Manickam提交的[修复](https://github.com/magento/magento-cloud-docker/pull/296)。<!--Issue 295-->

## v1.2.0

发行日期： 2020年11月9日

- ![新图标](../../assets/new.svg) **容器更新 —**

   - ![新图标](../../assets/new.svg) **PHP-FPM容器** — 添加了对gnupg PHP扩展的支持。 G Arvind从Zilker Technology提交的[修复](https://github.com/magento/magento-cloud-docker/pull/210).<!--MCLOUD-5981-->

   - ![修复图标](../../assets/fix.svg) **数据库容器** — 通过将所需的数据库密码添加到运行状况检查命令来修复数据库容器运行状况检查。<!--MCLOUD-7122-->

   - ![新图标](../../assets/new.svg) **Elasticsearch容器**

      - 添加了对Elasticsearch 7.9的支持，以便与即将发布的Adobe Commerce版本兼容。<!--MCLOUD-7190-->

      - **Elasticsearch插件配置** — 添加了对使用`services.yaml`文件中的Elasticsearch插件配置信息来为Commerce环境的Cloud Docker生成`docker-compose.yaml`文件的支持。 查看[Elasticsearch插件](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service/#elasticsearch-plugins).<!--MCLOUD-2789-->

      - **Elasticsearch插件支持** — 已添加对以下Elasticsearch插件的支持： `analysis-icu`、`analysis-phonetic`、`analysis-stempel`和`analysis-nori`。 默认情况下，`analysis-icu`和`analysis-phonetic`插件已安装。 您可以根据需要添加或删除`analysis-stempel`和`analysis-nori`插件。<!--MCLOUD-2789-->

   - ![新图标](../../assets/new.svg) **CLI容器**

      - **在Docker PHP容器中运行命令** — 现在，您可以使用Cloud Docker CLI在Docker环境中的PHP容器中运行命令，而无需在主机上安装PHP。 例如，以下命令构建配置： `./bin/magento-docker php 7.3 vendor/bin/ece-docker build:compose`。 请参阅[Cloud Docker CLI](https://developer.adobe.com/commerce/cloud-tools/docker/quick-reference/#cloud-docker-cli)。 G Arvind从Zilker Technology提交的[修复](https://github.com/magento/magento-cloud-docker/pull/209).<!--MCLOUD-5982-->

      - 将OpenSSH-client添加到PHP CLI容器。 现在，如果`composer.json`文件包含需要ssh客户端使用编辑器命令的私有Git存储库，则可以使用Composer的ssh代理转发。<!--MCLOUD-6008-->

   - ![修复图标](../../assets/fix.svg) **TLS容器** — 现在，[TLS容器](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service/#tls-container)基于`https://hub.docker.com/r/magento/magento-cloud-docker-nginx` Docker映像，而不是CentOS映像。 此更改修复了在Cloud Docker环境中的容器之间发送HTTPS请求时导致错误的问题。<!--MCLOUD-6469-->

   - ![新图标](../../assets/new.svg) **测试容器** — 添加了用于应用程序测试的测试容器，并向Docker `--with-test`命令添加了`build:compose`选项，以便仅在Docker环境中测试时创建容器。 查看[应用程序测试](https://developer.adobe.com/commerce/cloud-tools/docker/test/application-testing/).<!--MCLOUD-6394-->

   - ![新图标](../../assets/new.svg) **FPM-XDEBUG容器**

      - ![新图标](../../assets/new.svg) **在Linux上配置Xdebug** — 已将`--set-docker-host`选项添加到`ece-docker build:compose`命令以在Xdebug容器中配置`host.docker.internal`值。 在Linux系统上使用Xdebug时需要此选项。 请参阅[为Docker配置Xdebug](https://developer.adobe.com/commerce/cloud-tools/docker/test/configure-xdebug/)。<!--MCLOUD-6430-->

      - ![修复图标](../../assets/fix.svg)修复了Docker ENTRYPOINT的Xdebug变量配置以解决日志中的`uninitialized "with_xdebug" variable`错误。 由Florent Olivaud提交的[修复](https://github.com/magento/magento-cloud-docker/pull/218)<!--MCLOUD-6043-->

- ![新图标](../../assets/new.svg) **Docker配置更改**

   - **MailHog配置** — 现在您可以使用以下`ece-docker build:compose`命令选项禁用MailHog并指定端口： `--no-mailhog`、`--mailhog-http-port`和`--mailhog-smtp-port`。 查看[设置电子邮件](https://developer.adobe.com/commerce/cloud-tools/docker/configure/#set-up-email)。<!--MCLOUD-6898, MCLOUD-6660-->

   - 对于Cloud Docker for Commerce 1.2.0及更高版本，Adobe现在为每个修补程序版本提供Docker图像，并且Docker配置生成器使用指定的修补程序版本创建Docker配置，而不是使用最新的修补程序版本。 以前，Docker配置生成器使用最新的修补程序版本构建配置，该版本可能会破坏使用早期版本构建的Commerce环境的Cloud Docker。<!--MCLOUD-7093-->

   - **在自定义Cloud Docker配置中指定自定义图像和版本** — 在生成自定义Docker编写配置文件(`build:custom:compose`)时更新了包含用于指定自定义图像和版本的选项的`docker-compose.yaml`命令。 请参阅[生成自定义Docker撰写配置](https://developer.adobe.com/commerce/cloud-tools/docker/configure/custom-docker-compose/)。<!--MCLOUD-7089-->

   - 更新了Docker主机配置以公开端口443，从而允许从所有CLI容器访问Adobe Commerce (`https://magento2.docker`)。 在生成Docker配置文件时，可通过添加`--tls-port`选项更改默认端口。<!--MCLOUD-6806-->

- ![修复图标](../../assets/fix.svg)修复了在`app/etc/env.php`文件存在时导致Commerce的Cloud Docker内部版本失败的问题。<!--MCLOUD-6732-->

- ![修复图标](../../assets/fix.svg)更新了生成配置以将命名卷替换为常规卷，以防止在Linux上部署Cloud Docker for Commerce或在Linux上部署Windows子系统(WSL2)时出现问题。<!--MCLOUD-6732-->

- ![修复图标](../../assets/fix.svg)已更新Cloud Docker for Commerce功能测试以支持编辑器2.0。<!--MCLOUD-7183-->

## v1.1.2

发行日期： 2020年9月9日

- ![新图标](../../assets/new.svg)已添加对Elasticsearch 7.7<!--MCLOUD-6219-->的支持

## v1.1.1

发行日期： 2020年8月5日

- ![修复图标](../../assets/fix.svg) **已更新电子邮件配置** — 已更新Commerce的默认Cloud Docker配置以支持MailHog服务，而不是使用SendMail。 查看[设置电子邮件](https://developer.adobe.com/commerce/cloud-tools/docker/configure/#set-up-email)。<!--MCLOUD-5624-->

- ![修复图标](../../assets/fix.svg)已将PS库还原到Cloud Docker环境配置以修复`ps:  command not found`错误。<!--MCLOUD-6621-->

- ![修复图标](../../assets/fix.svg)更新了默认Cloud Docker for Commerce配置以删除自动装入数据库入口点和MariaDB卷，从而修复在启动Cloud Docker环境时可能发生的`Cannot create container for service db`错误。

  现在，您可以通过向`ece-docker build:compose`命令添加以下选项来配置Cloud Docker环境以装载数据库目录： `--with-entry-point`和`with-mariadb-conf`。 查看[服务配置选项](https://developer.adobe.com/commerce/cloud-tools/docker/containers/#service-configuration-options).<!--MCLOUD-6424-->

- ![新图标](../../assets/new.svg) **CLI命令更新**

| 操作 | 命令 |
| ------------------------------------------------------------------------------- | -------------------------------------------------------------- |
| 向数据库容器添加入口点，以从备份还原数据库 | `./vendor/bin/ece-docker build:compose --db --with-entrypoint` |
| 添加MariaDB配置卷 | `./vendor/bin/ece-docker build:compose --db --mariadb-conf` |

## v1.1.0

发布日期： 2020年6月25日

- ![新图标](../../assets/new.svg) **添加了对拆分数据库性能解决方案的支持** — 现在可以在Cloud Docker环境中使用拆分数据库性能解决方案配置和部署存储。<!--MCLOUD-3740-->

- ![新图标](../../assets/new.svg) **对Adobe Commerce和Magento Open Source部署的支持** — 现在您可以使用适用于Commerce的Cloud Docker为云基础架构上未在Adobe Commerce上托管的项目部署本地开发环境。<!--MCLOUD-5667-->

- ![新图标](../../assets/new.svg) **Blackfire.io支持** — 添加了对使用[Blackfire.io扩展](https://developer.adobe.com/commerce/cloud-tools/docker/test/blackfire/)进行自动性能测试的支持。 由Adarsh Manickam从Zilker Technology提交的[修复](https://github.com/magento/magento-cloud-docker/pull/202)<!--MCLOUD-5857-->

- ![新图标](../../assets/new.svg) **容器更新**

   - **Varnish** — 现在，当您使用支持的云应用程序模板版本在Cloud Docker环境中部署Adobe Commerce时，Varnish是默认缓存。 查看[清漆容器](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service/#varnish-container).<!--MCLOUD-2634-->

   - 添加了在生成Cloud Docker配置文件时跳过Varnish服务安装的`--no-varnish`选项。<!--MCLOUD-2634-->

   - ![新图标](../../assets/new.svg) **数据库**

      - 添加了对MySQL数据库的支持。 现在，您可以使用MariaDB或MySQL配置Cloud Docker环境。 查看[服务配置选项](https://developer.adobe.com/commerce/cloud-tools/docker/containers/#service-configuration-options).<!--MCLOUD-5691-->

      - 添加了生成Docker组合文件时为数据库复制设置增量设置和偏移设置的功能。 查看[服务容器](https://developer.adobe.com/commerce/cloud-tools/docker/containers/#service-containers).<!--MCLOUD-5735-->

   - ![新图标](../../assets/new.svg) **PHP-FPM**

      - 添加了对PHP 7.4的支持。[Mohanela Murugan从Zilker Technology提交的修复](https://github.com/magento/magento-cloud-docker/pull/198)<!--MCLOUD-198-->

      - 添加了将根项目目录中的`php.ini`文件复制到Cloud Docker环境并将自定义PHP设置应用到PHP-FPM和CLI容器的功能。 请参阅[自定义PHP设置](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service/#customize-php-settings)。 Mathew Beane从Zilker Technology提交的[修复](https://github.com/magento/magento-cloud-docker/pull/130).<!--MCLOUD-6012-->

      - 添加了容器运行状况检查。 Visanth Sampath从Zilker Technology提交的[修复](https://github.com/magento/magento-cloud-docker/pull/188)。<!--MCLOUD-5752-->

   - ![修复图标](../../assets/fix.svg) **Node.js** — 已将默认Node.js版本从版本8更新到版本10，以提高安全性。 Node.js版本8已弃用，不会再更新为错误修复或安全修补程序。 Mohan Elamurugan从Zilker Technology提交的[修复](https://github.com/magento/magento-cloud-docker/pull/183)。<!--MCLOUD-5586-->

   - ![新图标](../../assets/new.svg) **Elasticsearch**

      - 添加了对Elasticsearch 6.8、7.2、7.5和7.6的支持。<!--MCLOUD-4050, MCLOUD-5855,MCLOUD-5860-->

      - 添加了生成Docker组合配置文件时自定义[Elasticsearch容器配置](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service/#elasticsearch-container)的功能。<!--MCLOUD-3059-->

      - 向用于生成Docker编写配置文件的服务配置选项添加了`--no-es`选项。 使用此选项可跳过Elasticsearch容器安装，并改为使用MySQL搜索。 仅Adobe Commerce版本2.3.5及更早版本支持此选项。<!--MCLOUD-3766-->

   - ![新图标](../../assets/new.svg) **FPM-XDEBUG容器** — 添加了一个服务配置选项，用于在Cloud Docker环境中安装和配置Xdebug以调试PHP。 请参阅[配置Xdebug](https://developer.adobe.com/commerce/cloud-tools/docker/test/configure-xdebug/)。<!--MCLOUD-4098-->

- ![新图标](../../assets/new.svg) **Docker配置更改**

   - 为PHP-FPM、Redis、Elasticsearch和MySQL Docker服务容器添加了运行状况检查。<!--MCLOUD-3335 and MCLOUD-5856-->

   - 在开发人员模式下将默认文件同步模式更改为`native`。<!--MCLOUD-3890 -->

   - 在生成`docker-compose.yml`文件时向通用Docker服务容器图像添加了版本信息。<!--MCLOUD-3878-->

   - 通过增加Nginx服务器的`fastcgi_buffers`值，改进了处理来自上游PHP-FPM容器的大型响应的能力。<!--MCLOUD-5980-->

   - 通过添加第二个同步会话来同步`vendor`目录中的文件，提高了突变文件同步性能。 此更改可防止突变在文件同步过程中卡住。 Mathew Beane从Zilker Technology提交的[修复](https://github.com/magento/magento-cloud-docker/pull/127).<!--MCLOUD-6010-->

   - ![新图标](../../assets/new.svg) **CLI命令更新**

| 操作 | 命令 |
| -------- | --------------- |
| 清除Redis缓存 | `bin/magento-docker flush-redis` |
| 清除清漆缓存 | `bin/magento-docker flush-varnish` |
| 跳过默认清漆安装 | `.vendor/bin/ece-docker build:compose --no-varnish`<!--MCLOUD-2634--> |
| [自定义Elasticsearch选项](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service/#elasticsearch-container) | `.vendor/bin/ece-docker build:compose --es-env-var`<!--MCLOUD-3059--> |
| [删除Elasticsearch配置](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service/#elasticsearch-container) | `.vendor/bin/ece-docker build:compose --no-es`<!--MCLOUD-3766--> |
| 使用MySQL版本5.6或5.7配置数据库容器 | `./vendor/bin/ece-docker build:compose --db <mysql-version-number> --db-image mysql`<!--MCLOUD-5691--> |
| 指定自定义基本URL | `./vendor/bin/ece-docker build:compose --host=<hostname> --port=<port-number>`<!--MCLOUD-3063--> |
| [添加Xdebug配置的容器](https://developer.adobe.com/commerce/cloud-tools/docker/test/configure-xdebug/) | `.vendor/bin/ece-docker build:compose --mode developer --sync-engine native --with-xdebug`<!--MCLOUD-4098--> |

- ![修复图标](../../assets/fix.svg)修复了mutagen文件同步的配置，以防止创建mutagen过时会话。 Mathew Beane从Zilker Technology提交的[修复](https://github.com/magento/magento-cloud-docker/pull/127).<!--MCLOUD-6010-->

- ![修复图标](../../assets/fix.svg)修复了在启动PHP-FPM容器时导致Docker撰写日志中出现语法错误的配置问题。 Mathew Beane从Zilker Technology提交的[修复](https://github.com/magento/magento-cloud-docker/pull/129)<!--MCLOUD-3958-->

- ![修复图标](../../assets/fix.svg)修复了在使用多个Docker环境时有时发生的卷冲突错误。 G Arvind从Zilker Technology[提交的](https://github.com/magento/magento-cloud-docker/pull/168)修复。

- ![修复图标](../../assets/fix.svg)修复了在配置包含Blackfire.io时导致`ece-docker build:compose`命令失败的问题。 G Arvind从Zilker Technology[提交的](https://github.com/magento/magento-cloud-docker/pull/199)修复。<!--MCLOUD-5797-->

- ![修复图标](../../assets/fix.svg)更新了PHP CLI映像配置，以防止在使用Cloud Docker for Commerce安装多个包时发生内存不足错误。 Mohan Elamurugan从Zilker Technology提交的[修复](https://github.com/magento/magento-cloud-docker/pull/197)。*<!--MCLOUD-5818-->

- ![修复图标](../../assets/fix.svg)在Cloud Docker环境中添加了对多个MySQL用户的支持。 在早期版本中，如果`build:compose`文件指定了多个数据库用户，则`magento.app.yaml`操作失败。 G Arvind从Zilker Technology提交的[修复](https://github.com/magento/magento-cloud-docker/pull/181).<!--MCLOUD-5670-->

- ![修复图标](../../assets/fix.svg)已从Commerce PHP容器的Cloud Docker中删除`rsyslog`以解决在部署期间导致警告通知的兼容性问题。 Cloud Docker不使用rsyslog实用工具。<!--MCLOUD-6173-->

## v1.0.0

发行日期：2020年2月5日

- ![新图标](../../assets/new.svg) **已创建单独的包以交付`Cloud Docker for Commerce`** — 已将用于交付Cloud Docker for Commerce的源代码从`ece-tools`存储库移动到[新`magento-cloud-docker`存储库](https://github.com/magento/magento-cloud-docker)，以保持代码质量并提供独立的版本。 新软件包依赖于ECE-Tools v2002.1.0及更高版本。

  当您更新ece-tools时，您还将`magento/magento-cloud-docker`包更新到1.0.0版本。如果您使用具有早期`ece-tools`版本(2002.0.x)的Cloud Docker for Commerce，请查看[向后不兼容](backward-incompatible-changes.md)，并根据需要以脚本、命令和进程的形式更新您的项目。

- ![新图标](../../assets/new.svg) **已向Docker映像添加版本控制** — 您现在必须更新`magento/magento-cloud-docker`包才能获取更新的映像。<!--MAGECLOUD-4737-->

- ![新图标](../../assets/new.svg) **容器更新**—

   - ![新图标](../../assets/new.svg) **PHP-FPM容器**—

      - ![新图标](../../assets/new.svg) **添加了Node.js支持** — 更新了PHP-FPM映像以支持PHP容器中的节点、npm和grunt-cli功能。<!--MAGECLOUD-3953-->

      - ![新图标](../../assets/new.svg) **添加了对[ionCube](https://www.ioncube.com/)**&#x200B;的支持 — 更新了默认Docker配置以支持本地Docker开发环境中的ionCube。<!--MAGECLOUD-4354-->

   - ![新图标](../../assets/new.svg) **Web容器**—

      - ![新图标](../../assets/new.svg) **自定义NGINX配置** — 添加了将自定义`nginx.conf`文件挂载到Cloud Docker for Commerce环境的功能。 查看[Web容器](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service/#web-container).<!--MAGECLOUD-4204-->

      - ![新图标](../../assets/new.svg) **自动生成的NGINX证书**—Docker配置文件现在包含为Web容器自动生成NGINX证书的配置。<!--MAGECLOUD-4258-->

   - ![新图标](../../assets/new.svg) **新Selenium容器** — 添加了[Selenium容器](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service/#selenium-container)以支持使用Magento功能测试框架(MFTF)进行Adobe Commerce应用程序测试。<!--MAGECLOUD-4040-->

   - ![新图标](../../assets/new.svg) **[!DNL RabbitMQ]版本支持** — 已更新[!DNL RabbitMQ]容器配置以支持[!DNL RabbitMQ]版本3.8。<!--MAGECLOUD-4674-->

   - ![修复图标](../../assets/fix.svg) **持久性数据库容器** — 在您停止并删除Docker配置并在重新启动Docker配置时恢复后，`magento-db: /var/lib/mysql`数据库卷现在会持续存在。 现在，您必须手动删除数据库卷。 查看[数据库容器].<!--MAGECLOUD-3978-->

   - ![新图标](../../assets/new.svg) **TLS容器**—

      - ![新图标](../../assets/new.svg) **更新了容器基本图像以使用官方图像**— [云TLS容器](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service/#tls-container)图像现在基于官方`debian:jessie` Docker图像。—<!--MAGECLOUD-4163-->

      - ![新图标](../../assets/new.svg) **已添加对[英镑TLS终止代理]**&#x200B;的支持 — [英镑配置文件](https://github.com/magento/magento-cloud-docker/blob/1.0/images/tls/)添加了以下ENV变量以自定义TLS容器的Docker配置：

         - **`TimeOut`** — 设置首字节时间(TTFB)超时值。 默认值为300秒。

         - **`RewriteLocation`** — 确定英镑代理是否默认将位置重写到请求URL。 默认值为`0`，以防止重写中断对外部网站（如外部SSO网站）的重定向。 由Sorin Sugar提交的[修复](https://github.com/magento/magento-cloud-docker/pull/37)<!--MAGECLOUD-4061-->

      - ![新图标](../../assets/new.svg)已将TLS容器配置中的超时值从15秒增加到300秒。 Mathew Beane从Zilker Technology提交的[修复](https://github.com/magento/magento-cloud-docker/pull/78)<!--MAGECLOUD-4460-->

   - ![新图标](../../assets/new.svg) **清漆容器**—

      - ![新图标](../../assets/new.svg) **已更新容器基础图像以使用正式图像**— [云上光容器](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service/#varnish-container)现在基于正式的`centos` Docker图像。<!--MAGECLOUD-4163-->

      - ![新图标](../../assets/new.svg) **已改进默认超时配置** — 已将`.first_byte_timeout`和`.between_bytes_timeout`配置添加到Varnish容器。 这两个超时值都默认为`300s`（5分钟）。 Mathew Beane从Zilker Technology提交的[修复](https://github.com/magento/magento-cloud-docker/pull/78)<!--MAGECLOUD-4460-->

      - ![修复图标](../../assets/fix.svg) **在Xdebug会话期间跳过涂漆** — 更新了涂漆容器配置以在启用Xdebug时收到请求时返回`pass`。 在以前的版本中，如果Docker环境包含Varnish，则无法使用Xdebug。 Mathew Beane从Zilker Technology提交的[修复](https://github.com/magento/magento-cloud-docker/pull/111).<!--MAGECLOUD-4873-->

- ![新图标](../../assets/new.svg) **Docker配置更改**—

   - ![新图标](../../assets/new.svg) **管理项目的挂载和卷** — 添加了为本地开发启动Docker环境时管理挂载和卷的功能。 查看[共享项目数据]。<!--MAGECLOUD-3248-->

   - ![新图标](../../assets/new.svg) **对网桥模式的支持** — 添加了对网桥模式的支持，以便通过本地网络启用Docker容器之间的连接。<!--MAGECLOUD-4165-->

   - ![新图标](../../assets/new.svg) **默认情况下禁用的Cron容器** — 为了提高性能，在构建Docker环境时，默认情况下不再配置Cron容器。 您可以使用Docker构建命令上的`--with-cron`选项将Cron容器添加到环境中。 查看[管理cron作业](https://developer.adobe.com/commerce/cloud-tools/docker/configure/manage-cron-jobs/)。<!--MAGECLOUD-5181-->

   - ![新图标](../../assets/new.svg) **停止同步大型备份文件** — 已将数据库转储和存档文件（ZIP、SQL、GZ和BZ2）添加到`dist/docker-sync.yml`和`dist/mutagen.sh`文件的排除列表中。 同步大型文件(>1 GB)可能会导致一段时间不活动，并且备份文件通常不需要同步，因为您可以重新生成它们。<!--MAGECLOUD-3979-->

- ![新图标](../../assets/new.svg) **命令更改**—

   - ![修复图标](../../assets/fix.svg)已将`./bin/docker`文件重命名为`./bin/magento-docker`以修复由于`./bin/docker`文件覆盖现有Docker二进制文件而导致某些Docker环境中断的问题。 这是[向后不兼容的更改](backward-incompatible-changes.md)，需要更新脚本和命令。<!-- MAGECLOUD-4038 -->

   - ![新图标](../../assets/new.svg) **添加了一个服务配置选项以将数据库端口公开给主机** — 在构建`--expose-db-port= [Fix submitted by Adarsh Manickam from Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/101).<PORT>`文件时使用`docker-compose.yml`选项将数据库端口公开给主机： `bin/ece-docker build:compose --expose-db-port=<PORT>`<!--MAGECLOUD-4454-->

   - ![新图标](../../assets/new.svg) **新部署后命令** — 以前，在使用`.magento.app.yaml`命令将Adobe Commerce部署到Cloud Docker容器后，`cloud-deploy`文件中定义的部署后挂接会自动运行。 现在，您必须发出单独的`cloud-post-deploy`命令以在部署后运行部署后挂接。 查看[开发人员](https://developer.adobe.com/commerce/cloud-tools/docker/deploy/developer-mode/)和[生产](https://developer.adobe.com/commerce/cloud-tools/docker/deploy/production-mode/)模式的更新启动说明。<!--MAGECLOUD-3996-->

   - ![新图标](../../assets/new.svg)已将`--rm`选项添加到生成和部署容器的`./bin/magento-docker`命令。 任务完成后，这将删除容器。<!--MAGECLOUD-4205-->

   - ![新图标](../../assets/new.svg) **对`build:compose`命令的更新**—

      - ![新图标](../../assets/new.svg)在`--sync-engine="native"`命令中添加了`docker-build`选项，以在开发人员模式下生成Docker撰写配置文件时禁用文件同步。 在Linux系统上开发时，使用此选项，这些系统不需要文件同步以进行本地Docker开发。 请参阅[在Docker环境中同步数据](https://developer.adobe.com/commerce/cloud-tools/docker/setup/synchronize-data/).<!--MCLOUD-3231, MCLOUD-3890-->

   - ![新图标](../../assets/new.svg)已将默认文件同步设置从`docker-sync`更改为`native`。 Mathew Beane从Zilker Technology提交的[修复](https://github.com/magento/magento-cloud-docker/pull/124).<!--MAGECLOUD-5066-->

- ![新图标](../../assets/new.svg) **验证改进**—

   - ![新图标](../../assets/new.svg)为本地Docker开发环境的部署过程添加了验证，以验证云环境配置是否包含解密数据库所需的加密密钥。 现在，如果环境配置未指定加密密钥的值，则日志中会显示错误消息。<!--MAGECLOUD-4423-->

   - ![新图标](../../assets/new.svg)已向Elasticsearch服务添加容器运行状况检查，以确保该服务在继续生成和部署处理之前已准备就绪。 如果运行状况检查返回错误，容器将自动重新启动。<!--MAGECLOUD-4456-->
