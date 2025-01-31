---
title: 日志处理程序
description: 了解如何在云基础架构上为Adobe Commerce配置日志处理程序。
feature: Cloud, Logs, Configuration
role: Developer
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '232'
ht-degree: 0%

---

# 日志处理程序

您可以配置日志处理程序以将消息发送到远程日志服务器。 日志处理程序将生成和部署日志推送到其他系统，类似于将日志推送到Slack和电子邮件。 您可以启用&#x200B;_syslog_&#x200B;处理程序，该处理程序适用于记录与硬件相关的消息，或者启用Graylog扩展日志格式(GELF)处理程序，该处理程序适用于记录来自软件应用程序的消息。

以下示例通过将配置添加到`.magento.env.yaml`文件来配置这两个处理程序。 有关最低日志记录级别(`min_level`)值，请参阅[日志级别](#log-levels)。

```yaml
log:
  syslog:
    ident: "<syslog-ident>"
    facility: 8 # https://php.net/manual/en/network.constants.php
    min_level: "info"
    logopts: <syslog-logopts>

  syslog_udp:
    host: "<syslog-host>"
    port: <syslog-port>
    facility: 8  # https://php.net/manual/en/network.constants.php
    ident: "<syslog-ident>"
    min_level: "info"

  gelf:
    min_level: "info"
    use_default_formatter: true
    additional: # Some additional information for each log message
      project: "<some-project-id>"
      app_id: "<some-app-id>"
    transport:
      http:
        host: "<http-host>"
        port: <http-port>
        path: "<http-path>"
        connection_timeout: 60
      tcp:
        host: "<tcp-host>"
        port: <tcp-port>
        connection_timeout: 60
      udp:
        host: "<udp-host>"
        port: <udp-port>
        chunk_size: 1024
```

## 日志级别

日志级别确定通知消息中的详细信息级别。 以下日志级别类别包括它下的每个日志级别。 例如，`debug`级别包括来自每个级别的日志记录，而`alert`级别仅显示警报和紧急情况。

- **debug** — 详细的调试信息
- **信息** — 感兴趣的事件，如用户登录或SQL日志
- **通知** — 正常但重要的事件
- **警告** — 不是错误的特殊发生次数，例如使用已弃用的API或很少使用API
- **错误** — 不需要立即操作的运行时错误
- **critical** — 严重情况，如不可用的应用程序组件或意外异常
- **警报** — 需要立即执行触发短信警报的操作，例如网站已关闭或数据库不可用
- **紧急** — 系统不可用
