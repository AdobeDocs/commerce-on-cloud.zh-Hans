---
title: 防火墙属性
description: 请参阅有关如何在Commerce应用程序配置文件中配置firewall属性的示例。
feature: Cloud, Configuration, Security
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '844'
ht-degree: 0%

---

# 防火墙属性

>[!IMPORTANT]
>
>仅入门项目

对于入门项目，`firewall`属性向应用程序添加&#x200B;_出站_&#x200B;防火墙。 此防火墙对传入请求没有影响。 它定义哪些`tcp`出站请求可以&#x200B;_保留_ Adobe Commerce站点。 这称为出口过滤。 出站防火墙会筛选出可以导出的内容 — 退出或转义您的网站。 限制可以转义的内容会向服务器添加强大的安全工具。

## 默认限制策略

防火墙提供两种默认策略来控制出站流量： `allow`和`deny`。 默认情况下，`allow`策略&#x200B;_允许_&#x200B;所有出站流量。 默认情况下，`deny`策略&#x200B;_拒绝_&#x200B;所有出站流量。 但添加规则时，默认策略将被覆盖，并且防火墙会阻止规则不允许的&#x200B;**所有**&#x200B;出站流量。

对于入门计划，默认策略设置为`allow`。 该设置确保在添加出口过滤规则之前，所有当前出站流量保持未阻塞状态。 可根据请求将默认策略设置为`deny`。

**检查您的默认策略**：

```bash
magento-cloud p:curl --project PROJECT_ID /settings | grep -i outbound
```

除非您为策略请求`deny`，否则命令应显示您的策略设置为`allow`：

```json
"outbound_restrictions_default_policy": "allow"
```

>[!NOTE]
>
>**密钥外带**：添加出站规则时，将阻止所有出站流量，但添加到规则的域、IP地址或端口除外。 因此，在将其添加到生产站点之前，请务必定义和测试完整的出站列表。

## 防火墙选项

`.magento.app.yaml`文件中的以下示例配置显示了可用于添加出口筛选规则的所有`firewall`选项。

```yaml
firewall:
    outbound:
        - # Common accessed domains
            domains:
                - newrelic.com
                - fastly.com
                - magento.com
                - magentocommerce.com
                - google.com
            ports:
                - 80
                - 443
            protocol: tcp # Can be omitted from rules.

        - # Adobe Stock integration
            domains:
                - account.adobe.com
                - stock.adobe.com
                - console.adobe.io
            ports:
                - 80
                - 443

        - # Payment services
            domains:
                - braintreepayments.com
                - paypal.com
            ports:
                - 80
                - 443

        - # Shipping services
            domains:
                - ups.com
                - usps.com
                - fedex.com
                - dhl.com
            ports:
                - 80
                - 443

        - # Vertex Integrated Address Cleansing
            domains:
                - mgcsconnect.vertexsmb.com
            ports:
                - 80
                - 443

        - # New Relic networks
            ips:
                - 162.247.240.0/22 # US region accounts
                - 185.221.84.0/22 # EU region accounts
            ports:
                - 443

        - # New Relic endpoints
            domains:
                - collector.newrelic.com, # US region accounts
                - collector.eu01.nr-data.net # EU region accounts
            ports:
                - 443

        - # Fastly IP ranges
            ips:
                - 23.235.32.0/20
                - 43.249.72.0/22
                - 103.244.50.0/24
                - 103.245.222.0/23
                - 103.245.224.0/24
                - 104.156.80.0/20
                - 146.75.0.0/16
                - 151.101.0.0/16
                - 157.52.64.0/18
                - 167.82.0.0/17
                - 167.82.128.0/20
                - 167.82.160.0/20
                - 167.82.224.0/20
                - 172.111.64.0/18
                - 185.31.16.0/22
                - 199.27.72.0/21
                - 199.232.0.0/16
            ports:
                - 80
                - 443
```

## 出口筛选规则

出站防火墙配置由规则组成。 您可以根据需要定义任意数量的规则。 对规则的要求如下。

**每个规则：**

- 必须以连字符(`-`)开头。 在同一行上添加注释有助于文档并以可视方式将一个规则与下一个规则分开。
- 必须至少定义以下选项之一： `domains`、`ips`或`ports`。
- 必须使用`tcp`协议。 由于这是所有规则的默认协议，因此您可以从规则中忽略它。
- 可以在同一规则中定义`domains`或`ips`，但不能同时定义两者。
- 可以包含`yaml`注释(`#`)和换行符以组织允许的域、IP地址和端口。

每个规则都使用以下属性：

### `domains`

`domains`选项允许列出完全限定的域名(FQDN)。

如果规则定义`domains`而非`ports`，则防火墙允许在任何端口上发送域请求。

### `ips`

`ips`选项允许以CIDR表示法列出IP地址。 您可以指定单个IP地址或IP地址范围。

要指定单个IP地址，请将`/32` CIDR前缀添加到IP地址的末尾：

```
172.217.11.174/32  # google.com
```

要指定IP地址范围，请使用[到CIDR的IP范围](https://ipaddressguide.com/cidr)计算器。

如果规则定义`ips`而非`ports`，则防火墙允许任何端口上的IP请求。

### `ports`

`ports`选项允许从1到65535的端口列表。 对于示例中的大多数规则，端口`80`和`443`都允许HTTP和HTTPS请求。 但对于New Relic，规则仅允许访问端口`443`上的域和IP地址，如New Relic有关[网络流量](https://docs.newrelic.com/docs/new-relic-solutions/get-started/networks/#agents)的文档中所建议。

如果规则仅定义`ports`，则防火墙允许访问定义端口的所有域和IP地址。

>[!NOTE]
>
>端口`25`（发送电子邮件的SMTP端口）始终被阻止，无例外。

### `protocol`

如前所述，TCP是默认协议，并且只允许用于规则。 不允许使用UDP及其端口。 因此，您可以从所有规则中忽略`protocol`选项。 如果仍要包含它，则必须将值设置为`tcp`，如示例的第一个规则所示。

## 查找要允许的域名

为了帮助您识别要包含在出口过滤规则中的域，请使用以下命令解析服务器的`dns.log`文件，并显示您的网站已记录的所有DNS请求的列表：

```shell
awk '($5 ~/query/)' /var/log/dns.log | awk '{print $6}' | sort | uniq -c | sort -rn
```

此命令还会显示已发出但被出口筛选规则阻止的DNS请求。 输出不会显示哪些域被阻止，只会显示已发出的请求。 输出不会显示使用IP地址发出的请求。

```
Example output:

97 magento.com
93 magentocommerce.com
88 google.com
70 metadata.google.internal.0
70 metadata.google.internal
65 newrelic.com
56 fastly.com
17 mcprod-0vunku5xn24ip.ap-4.magentosite.cloud
6 advancedreporting.rjmetrics.com
```

与IP地址相比，域通常对于出口过滤更具体和安全。 例如，如果您为使用CDN的服务添加一个IP地址，则您将允许该CDN的IP地址，该地址可供成千上万的其他域使用。 使用一个IP地址，您可以允许对数千台其他服务器进行出站访问。

## 测试出口过滤规则

在收集并配置域和IP地址访问规则后，便可以开始推送和测试。

要测试出口过滤规则，请执行以下操作：

1. 创建`curl`命令的Shell脚本以访问规则中的域和IP地址。 包含用于测试应阻止的域和IP地址访问权限的命令。

1. 在`.magento.app.yaml`文件中配置`post_deploy`挂接以运行脚本。

1. 将您的`firewall`配置和测试脚本推送到`integration`分支。

1. 检查`curl`命令中的`post_deploy`输出。

1. 优化`firewall`规则，更新`curl`脚本，提交、推送和重复。

### `curl`脚本示例

```shell
# curl-tests-for-egress-filtering.sh

# Use the -v option to display connection details

# Check domain access
curl -v newrelic.com
curl -v fastly.com
curl -v magento.com
curl -v magentocommerce.com
curl -v google.com
curl -v account.adobe.com
curl -v stock.adobe.com
curl -v console.adobe.io
curl -v braintreepayments.com
curl -v paypal.com
curl -v ups.com
curl -v usps.com
curl -v fedex.com
curl -v dhl.com
curl -v devdocs.magento.com

# Check domain denials
curl -v amazon.com
curl -v facebook.com
curl -v twitter.com

# IP address access
...

# IP address denials
...
```

### `post_deploy`示例

```yaml
hooks:
    build: |
        set -e
        php ./vendor/bin/ece-tools run scenario/build/generate.xml
        php ./vendor/bin/ece-tools run scenario/build/transfer.xml
    deploy: "php ./vendor/bin/ece-tools run scenario/deploy.xml\n"
    post_deploy: |
        set -e
        php ./vendor/bin/ece-tools run scenario/post-deploy.xml
        echo "[$(date)] post-deploy hook end"
        ./curl-tests-for-egress-filtering.sh
        echo "[$(date)] curl finished"
```
