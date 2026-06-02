---
title: 区域IP地址
description: 查看Adobe Commerce在用于集成环境的云基础架构上使用的AWS和Azure区域的IP地址列表。
exl-id: 1137f5cf-4879-46d7-878c-bf47de7a0e34
TQID: https://experienceleague.adobe.com/Ozs-ubM3egZwkxKYqvGnjOfjhxiSxNUFNXdWqLGP94k
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2: id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bdid: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 167
ht-degree: 0%

---

# 区域IP地址

下表列出了Adobe Commerce在云基础架构[集成环境](../architecture/pro-architecture.md#integration-environment)上使用的传入和传出IP地址。 这些IP地址稳定，但可能会更改。 Adobe会在更改任何IP地址之前通知客户。

用于解决集成环境的语法如下：

```text
<branch>-<unique-ID>-<project-ID>.<region>.magentosite.cloud
```

- **唯一ID** = 7个随机字母数字字符
- **项目ID** = 13个字符的项目ID
- **地区** = AWS或Azure地区名称

您可以使用`ping`或`dig`命令检索传入IP地址：

**Ping**

```bash
ping integration-abcd123-abcd78910.us-3.magentosite.cloud
```

示例响应：

```console
PING integration-abcd123-abcd78910.us-3.magentosite.cloud (34.210.133.187): 56 data bytes
Request timeout for icmp_seq 0
Request timeout for icmp_seq 1
Request timeout for icmp_seq 2
```

**Dig**

```bash
dig +short integration-abcd123-abcd78910.us-3.magentosite.cloud
```

示例响应

```bash
34.210.133.187
```

如果您的公司防火墙阻止传出SSH连接，则可以将入站IP地址添加到中。

## AWS地区

|     | 美国 |       |      | 欧洲 |      |      |      | 亚太 |
| --- | :------------ | :---- | :--- | :----- | :--- | :--- | :--- | :----------- |
|     | US | US-3 | US-5 | 欧盟 | 欧盟3 | 欧盟5 | 欧盟6 | AP-3 |
| 传入 | 52.200.159.23<p>52.200.159.125<p>52.200.160.5 | 34.210.133.187<p>34.214.72.239<p>34.215.10.85 | 50.112.160.58<p>54.213.195.223<p>35.163.170.185 | 52.209.44.44<p>52.209.23.96<p>52.51.117.101 | 34.240.75.192<p>34.251.110.37<p>52.19.113.35 | 35.157.81.88<p>3.122.198.131<p>52.28.102.195 | 35.181.23.47<p>35.181.24.165<p>35.180.237.48 | 52.65.39.201<p>52.65.10.202<p>52.65.30.37 |
| 传出 | 52.200.155.111<p>52.200.149.44<p>50.17.163.75 | 34.210.166.180<p>34.215.83.92<p>34.213.20.158 | 54.70.238.217<p>52.88.113.98<p>52.36.188.230 | 52.51.163.159<p>52.209.44.60<p>52.208.156.247 | 34.240.57.142<p>52.16.140.48<p>52.209.134.55 | 3.121.163.221<p>3.121.79.229<p>18.197.3.230 | 52.47.155.26<p>35.181.0.157<p>35.181.12.15 | 52.65.143.178<p>13.54.80.197<p>52.62.224.4 |

<!--US-->
<!--US-3-->
<!--US-5-->
<!--EU-->
<!--EU-3-->
<!--EU-5-->
<!--EU-6-->
<!--AP-3-->
<!--EU-->
<!--EU-3-->
<!--EU-5-->
<!--EU-6-->
<!--AP-3-->
<!--US-->
<!--US-3-->
<!--US-5-->




## Azure地区

|          | 美国 | 欧洲 |
| -------- | :-------------- | :-------------- |
|          | US-A1 | AZ-WESTEUROPE-1 |
| 传入 | 20.186.27.68<p>20.186.58.163<p>20.186.113.8 | 50.112.160.58<p>54.213.195.223<p>35.163.170.185 |
| 传出 | 20.186.58.163<p>20.186.27.68<p>20.186.113.8 | 104.45.78.98<p>51.105.168.218<p>51.105.163.143 |

<!--AZ-W-1-->
<!--US-A1-->
<!--US-A1-->
<!--AZ-W-1-->
