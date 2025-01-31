---
source-git-commit: 0df07e865c3c4fc4ac14483972643eafa8814726
workflow-type: tm+mt
source-wordcount: '13341'
ht-degree: 0%

---
# magento-cloud(云基础架构上的Adobe Commerce)

<!-- The template to render with above values -->

**版本**： 1.46.1

此引用包含119个通过`magento-cloud`命令行工具可用的命令。
在云基础架构上的Adobe Commerce中使用`magento-cloud list`命令自动生成初始列表。

## 常规

此引用从应用程序代码库生成。 要更改内容，您可以在[代码库](https://github.com/magento/magento-cloud-cli)存储库中更新相应命令实现的源代码，并提交更改以供审阅。 另一种方法是&#x200B;_向我们提供反馈_（在右上角查找链接）。 有关贡献准则，请参阅[代码贡献](https://developer.adobe.com/commerce/contributor/guides/code-contributions/)。

### 全局选项

#### `--help`，`-h`

显示此帮助消息

- 默认： `false`
- 不接受值

#### `--verbose`，`-v|-vv|-vvv`

增加消息的详细程度

- 默认： `false`
- 不接受值

#### `--version`，`-V`

显示此应用程序版本

- 默认： `false`
- 不接受值

#### `--yes`，`-y`

对确认问题回答“是”；接受其他问题的默认值；禁用交互

- 默认： `false`
- 不接受值

#### `--no-interaction`

请勿询问任何交互式问题；接受默认值。 等效于使用环境变量：MAGENTO_CLOUD_CLI_NO_INTERACTION=1

- 默认： `false`
- 不接受值


## `clear-cache`

```bash
magento-cloud magento-cloud cc
```

清除CLI缓存

### 选项

有关全局选项，请参阅[全局选项](#global-options)。


## `decode`

```bash
magento-cloud magento-cloud decode [-P|--property PROPERTY] [--] <value>
```

对编码字符串(如MAGENTO_CLOUD_VARIABLES)进行解码

### 参数

#### `value`

要解码的变量值

- 必填

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--property`，`-P`

要在变量中查看的属性

- 需要一个值


## `docs`

```bash
magento-cloud magento-cloud docs [--browser BROWSER] [--pipe] [--] [<search>]...
```

打开联机文档

### 参数

#### `search`

搜索词

- 默认： `[]`
- 数组

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--browser`

用于打开URL的浏览器。 将0设置为“无”。

- 需要一个值

#### `--pipe`

将URL输出到stdout。

- 默认： `false`
- 不接受值


## `help`

```bash
magento-cloud magento-cloud help [--format FORMAT] [--raw] [--] [<command_name>]
```

显示命令的帮助

```
The help command displays help for a given command:

  magento-cloud help list

You can also output the help in other formats by using the --format option:

  magento-cloud help --format=json list

To display the list of available commands, please use the list command.
```

### 参数

#### `command_name`

命令名称

- 默认： `help`

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--format`

输出格式（txt、json或md）

- 默认： `txt`
- 需要一个值

#### `--raw`

输出原始命令帮助

- 默认： `false`
- 不接受值


## `list`

```bash
magento-cloud magento-cloud list [--raw] [--format FORMAT] [--all] [--] [<namespace>]
```

列出命令

```
The list command lists all commands:

  magento-cloud list

You can also display the commands for a specific namespace:

  magento-cloud list project

You can also output the information in other formats by using the --format option:

  magento-cloud list --format=xml

It's also possible to get raw list of commands (useful for embedding command runner):

  magento-cloud list --raw
```

### 参数

#### `command`

要执行的命令

- 必填


#### `namespace`

命名空间名称

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--raw`

输出原始命令列表

- 默认： `false`
- 不接受值

#### `--format`

输出格式（txt、xml、json或md）

- 默认： `txt`
- 需要一个值

#### `--all`

显示所有命令，包括隐藏的命令

- 默认： `false`
- 不接受值


## `multi`

```bash
magento-cloud magento-cloud multi [-p|--projects PROJECTS] [--continue] [--sort SORT] [--reverse] [--] <cmd> (<cmd>)...
```

在多个项目上执行命令

### 参数

#### `cmd`

要执行的命令

- 默认： `[]`
- 必填

- 数组

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--projects`，`-p`

项目ID列表，用逗号和/或空格分隔

- 需要一个值

#### `--continue`

即使遇到异常，仍继续运行命令

- 默认： `false`
- 不接受值

#### `--sort`

用于对项目选项列表进行排序的属性

- 默认： `title`
- 需要一个值

#### `--reverse`

反转项目选项的顺序

- 默认： `false`
- 不接受值


## `web`

```bash
magento-cloud magento-cloud web [--browser BROWSER] [--pipe] [-p|--project PROJECT] [-e|--environment ENVIRONMENT]
```

在Web UI中打开项目

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--browser`

用于打开URL的浏览器。 将0设置为“无”。

- 需要一个值

#### `--pipe`

将URL输出到stdout。

- 默认： `false`
- 不接受值

#### `--project`，`-p`

项目ID或URL

- 需要一个值

#### `--environment`，`-e`

环境ID 使用“。” 以选择项目的默认环境。

- 需要一个值


## `activity:cancel`

```bash
magento-cloud magento-cloud activity:cancel [-t|--type TYPE] [-x|--exclude-type EXCLUDE-TYPE] [-a|--all] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--] [<id>]
```

取消活动

### 参数

#### `id`

活动ID 默认为最近的可取消活动。

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--type`，`-t`

按类型筛选（在选择默认活动时）。 值可以用逗号（例如“a，b，c”）和/或空格拆分。 %或*字符可用作该类型的通配符，例如“%var%”以选择与变量相关的活动。

- 默认： `[]`
- 需要一个值

#### `--exclude-type`，`-x`

按类型排除（在选择默认活动时）。 值可以用逗号（例如“a，b，c”）和/或空格拆分。 %或*字符可用作排除类型的通配符。

- 默认： `[]`
- 需要一个值

#### `--all`，`-a`

检查所有环境上的近期活动（在选择默认活动时）

- 默认： `false`
- 不接受值

#### `--project`，`-p`

项目ID或URL

- 需要一个值

#### `--environment`，`-e`

环境ID 使用“。” 以选择项目的默认环境。

- 需要一个值


## `activity:get`

```bash
magento-cloud magento-cloud activity:get [-P|--property PROPERTY] [-t|--type TYPE] [-x|--exclude-type EXCLUDE-TYPE] [--state STATE] [--result RESULT] [-i|--incomplete] [-a|--all] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [--date-fmt DATE-FMT] [--] [<id>]
```

查看有关单个活动的详细信息

### 参数

#### `id`

活动ID 默认为最近的活动。

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--property`，`-P`

要查看的属性

- 需要一个值

#### `--type`，`-t`

按类型筛选（在选择默认活动时）。 值可以用逗号（例如“a，b，c”）和/或空格拆分。 %或*字符可用作该类型的通配符，例如“%var%”以选择与变量相关的活动。

- 默认： `[]`
- 需要一个值

#### `--exclude-type`，`-x`

按类型排除（在选择默认活动时）。 值可以用逗号（例如“a，b，c”）和/或空格拆分。 %或*字符可用作排除类型的通配符。

- 默认： `[]`
- 需要一个值

#### `--state`

按状态筛选（在选择默认活动时）： in_progress、pending、complete或canceled。 值可以用逗号（例如“a，b，c”）和/或空格拆分。

- 默认： `[]`
- 需要一个值

#### `--result`

按结果筛选（在选择默认活动时）：成功或失败

- 需要一个值

#### `--incomplete`，`-i`

仅包括未完成的活动（在选择默认活动时）。 这是 — state=in_progress，pending的简写

- 默认： `false`
- 不接受值

#### `--all`，`-a`

检查所有环境上的近期活动（在选择默认活动时）

- 默认： `false`
- 不接受值

#### `--project`，`-p`

项目ID或URL

- 需要一个值

#### `--environment`，`-e`

环境ID 使用“。” 以选择项目的默认环境。

- 需要一个值

#### `--format`

输出格式：table、csv、tsv或plain

- 默认： `table`
- 需要一个值

#### `--columns`，`-c`

要显示的列。 %或*字符可用作通配符。 值可以用逗号（例如“a，b，c”）和/或空格拆分。

- 默认： `[]`
- 需要一个值

#### `--no-header`

不输出表头

- 默认： `false`
- 不接受值

#### `--date-fmt`

日期格式（作为PHP日期格式字符串）

- 默认： `c`
- 需要一个值


## `activity:list`

```bash
magento-cloud magento-cloud activities [-t|--type TYPE] [-x|--exclude-type EXCLUDE-TYPE] [--limit LIMIT] [--start START] [--state STATE] [--result RESULT] [-i|--incomplete] [-a|--all] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [--date-fmt DATE-FMT] [-p|--project PROJECT] [-e|--environment ENVIRONMENT]
```

获取环境或项目的活动列表

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--type`，`-t`

按类型过滤活动值可能会被逗号（例如“a，b，c”）和/或空格拆分。 可以忽略活动名称的第一部分，例如，“cron”可以选择“environment.cron”活动。 %或*字符可用作通配符，例如“%var%”以选择与变量相关的活动。

- 默认： `[]`
- 需要一个值

#### `--exclude-type`，`-x`

按类型排除活动。 值可以用逗号（例如“a，b，c”）和/或空格拆分。 可以忽略活动名称的第一部分，例如，“cron”可以排除“environment.cron”活动。 %或*字符可用作排除类型的通配符。

- 默认： `[]`
- 需要一个值

#### `--limit`

限制显示的结果数

- 默认： `10`
- 需要一个值

#### `--start`

将仅列出在此日期之前创建的活动

- 需要一个值

#### `--state`

按状态筛选活动： in_progress、pending、complete或canceled。 值可以用逗号（例如“a，b，c”）和/或空格拆分。

- 默认： `[]`
- 需要一个值

#### `--result`

按结果筛选活动：成功或失败

- 需要一个值

#### `--incomplete`，`-i`

仅列出未完成的活动

- 默认： `false`
- 不接受值

#### `--all`，`-a`

列出所有环境上的活动

- 默认： `false`
- 不接受值

#### `--format`

输出格式：table、csv、tsv或plain

- 默认： `table`
- 需要一个值

#### `--columns`，`-c`

要显示的列。 可用列： id*、created*、description*、progress*、state*、result*、completed、environments、type （* =默认列）。 字符“+”可用作默认列的占位符。 %或*字符可用作通配符。 值可以用逗号（例如“a，b，c”）和/或空格拆分。

- 默认： `[]`
- 需要一个值

#### `--no-header`

不输出表头

- 默认： `false`
- 不接受值

#### `--date-fmt`

日期格式（作为PHP日期格式字符串）

- 默认： `c`
- 需要一个值

#### `--project`，`-p`

项目ID或URL

- 需要一个值

#### `--environment`，`-e`

环境ID 使用“。” 以选择项目的默认环境。

- 需要一个值


## `activity:log`

```bash
magento-cloud magento-cloud activity:log [--refresh REFRESH] [-t|--timestamps] [--type TYPE] [-x|--exclude-type EXCLUDE-TYPE] [--state STATE] [--result RESULT] [-i|--incomplete] [-a|--all] [--date-fmt DATE-FMT] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--] [<id>]
```

显示活动的日志

### 参数

#### `id`

活动ID 默认为最近的活动。

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--refresh`

活动刷新间隔（秒）。 设置为0可禁用刷新。

- 默认： `3`
- 需要一个值

#### `--timestamps`，`-t`

在每个消息旁边显示时间戳

- 默认： `false`
- 不接受值

#### `--type`

按类型筛选（在选择默认活动时）。 值可以用逗号（例如“a，b，c”）和/或空格拆分。 %或*字符可用作该类型的通配符，例如“%var%”以选择与变量相关的活动。

- 默认： `[]`
- 需要一个值

#### `--exclude-type`，`-x`

按类型排除（在选择默认活动时）。 值可以用逗号（例如“a，b，c”）和/或空格拆分。 %或*字符可用作排除类型的通配符。

- 默认： `[]`
- 需要一个值

#### `--state`

按状态筛选（在选择默认活动时）： in_progress、pending、complete或canceled。 值可以用逗号（例如“a，b，c”）和/或空格拆分。

- 默认： `[]`
- 需要一个值

#### `--result`

按结果筛选（在选择默认活动时）：成功或失败

- 需要一个值

#### `--incomplete`，`-i`

仅包括未完成的活动（在选择默认活动时）。 这是 — state=in_progress，pending的简写

- 默认： `false`
- 不接受值

#### `--all`，`-a`

检查所有环境上的近期活动（在选择默认活动时）

- 默认： `false`
- 不接受值

#### `--date-fmt`

日期格式（作为PHP日期格式字符串）

- 默认： `c`
- 需要一个值

#### `--project`，`-p`

项目ID或URL

- 需要一个值

#### `--environment`，`-e`

环境ID 使用“。” 以选择项目的默认环境。

- 需要一个值


## `app:config-get`

```bash
magento-cloud magento-cloud app:config-get [-P|--property PROPERTY] [--refresh] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [-i|--identity-file IDENTITY-FILE]
```

查看应用程序的配置

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--property`，`-P`

要查看的配置属性

- 需要一个值

#### `--refresh`

是否刷新缓存

- 默认： `false`
- 不接受值

#### `--project`，`-p`

项目ID或URL

- 需要一个值

#### `--environment`，`-e`

环境ID 使用“。” 以选择项目的默认环境。

- 需要一个值

#### `--app`，`-A`

远程应用程序名称

- 需要一个值

#### `--identity-file`，`-i`

[已弃用选项，不再使用]

- 需要一个值


## `app:list`

```bash
magento-cloud magento-cloud apps [--refresh] [--pipe] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header]
```

列出项目中的应用程序

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--refresh`

是否刷新缓存

- 默认： `false`
- 不接受值

#### `--pipe`

仅输出应用程序名称列表

- 默认： `false`
- 不接受值

#### `--project`，`-p`

项目ID或URL

- 需要一个值

#### `--environment`，`-e`

环境ID 使用“。” 以选择项目的默认环境。

- 需要一个值

#### `--format`

输出格式：table、csv、tsv或plain

- 默认： `table`
- 需要一个值

#### `--columns`，`-c`

要显示的列。 可用列：名称*、类型*、磁盘、路径、大小（* =缺省列）。 字符“+”可用作默认列的占位符。 %或*字符可用作通配符。 值可以用逗号（例如“a，b，c”）和/或空格拆分。

- 默认： `[]`
- 需要一个值

#### `--no-header`

不输出表头

- 默认： `false`
- 不接受值


## `auth:api-token-login`

```bash
magento-cloud magento-cloud auth:api-token-login
```

使用API令牌登录Magento云

```
Use this command to log in to your Magento Cloud account using an API token.

You can create an account at:
    https://business.adobe.com/products/magento/magento-commerce.html

If you have an account, but you do not already have an API token, you can create one here:
    https://accounts.magento.cloud/user/api-tokens

Alternatively, to log in to the CLI with a browser, run:
    magento-cloud auth:browser-login
```

### 选项

有关全局选项，请参阅[全局选项](#global-options)。


## `auth:browser-login`

```bash
magento-cloud magento-cloud login [-f|--force] [--browser BROWSER] [--pipe]
```

通过浏览器登录Magento云

```
Use this command to log in to the Magento Cloud CLI using a web browser.

It launches a temporary local website which redirects you to log in if
necessary, and then captures the resulting authorization code.

Your system's default browser will be used. You can override this using the
--browser option.

Alternatively, to log in using an API token (without a browser), run:
magento-cloud auth:api-token-login

To authenticate non-interactively, configure an API token using the
MAGENTO_CLOUD_CLI_TOKEN environment variable.
```

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--force`，`-f`

再次登录，即使已登录

- 默认： `false`
- 不接受值

#### `--browser`

用于打开URL的浏览器。 将0设置为“无”。

- 需要一个值

#### `--pipe`

将URL输出到stdout。

- 默认： `false`
- 不接受值


## `auth:info`

```bash
magento-cloud magento-cloud auth:info [--no-auto-login] [-P|--property PROPERTY] [--refresh] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [--] [<property>]
```

显示您的帐户信息

### 参数

#### `property`

要查看的帐户属性

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--no-auto-login`

跳过自动登录。 如果未登录，则不会输出任何内容，并且退出代码将为0（假定没有其他错误）。

- 默认： `false`
- 不接受值

#### `--property`，`-P`

要查看的帐户属性（替代语法）

- 需要一个值

#### `--refresh`

是否刷新缓存

- 默认： `false`
- 不接受值

#### `--format`

输出格式：table、csv、tsv或plain

- 默认： `table`
- 需要一个值

#### `--columns`，`-c`

要显示的列。 %或*字符可用作通配符。 值可以用逗号（例如“a，b，c”）和/或空格拆分。

- 默认： `[]`
- 需要一个值

#### `--no-header`

不输出表头

- 默认： `false`
- 不接受值


## `auth:logout`

```bash
magento-cloud magento-cloud logout [-a|--all] [--other]
```

注销Magento Cloud

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--all`，`-a`

从所有本地会话注销

- 默认： `false`
- 不接受值

#### `--other`

从其他本地会话注销

- 默认： `false`
- 不接受值


## `blackfire:setup`

```bash
magento-cloud magento-cloud blackfire:setup [--server_id SERVER_ID] [--server_token SERVER_TOKEN] [-p|--project PROJECT] [-W|--no-wait] [--wait]
```

为项目设置Blackfire.io集成

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--server_id`

服务器ID

- 需要一个值

#### `--server_token`

服务器令牌

- 需要一个值

#### `--project`，`-p`

项目ID或URL

- 需要一个值

#### `--no-wait`，`-W`

不要等待操作完成

- 默认： `false`
- 不接受值

#### `--wait`

等待操作完成（默认）

- 默认： `false`
- 不接受值


## `certificate:add`

```bash
magento-cloud magento-cloud certificate:add [--cert CERT] [--key KEY] [--chain CHAIN] [-p|--project PROJECT] [-W|--no-wait] [--wait]
```

将SSL证书添加到项目

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--cert`

证书文件的路径

- 需要一个值

#### `--key`

证书私钥文件的路径

- 需要一个值

#### `--chain`

证书链文件的路径

- 默认： `[]`
- 需要一个值

#### `--project`，`-p`

项目ID或URL

- 需要一个值

#### `--no-wait`，`-W`

不要等待操作完成

- 默认： `false`
- 不接受值

#### `--wait`

等待操作完成（默认）

- 默认： `false`
- 不接受值


## `certificate:delete`

```bash
magento-cloud magento-cloud certificate:delete [-p|--project PROJECT] [-W|--no-wait] [--wait] [--] <id>
```

从项目中删除证书

### 参数

#### `id`

证书ID（或其开头）

- 必填

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--project`，`-p`

项目ID或URL

- 需要一个值

#### `--no-wait`，`-W`

不要等待操作完成

- 默认： `false`
- 不接受值

#### `--wait`

等待操作完成（默认）

- 默认： `false`
- 不接受值


## `certificate:get`

```bash
magento-cloud magento-cloud certificate:get [-P|--property PROPERTY] [--date-fmt DATE-FMT] [-p|--project PROJECT] [--] <id>
```

查看证书

### 参数

#### `id`

证书ID（或其开头）

- 必填

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--property`，`-P`

要查看的证书属性

- 需要一个值

#### `--date-fmt`

日期格式（作为PHP日期格式字符串）

- 默认： `c`
- 需要一个值

#### `--project`，`-p`

项目ID或URL

- 需要一个值


## `certificate:list`

```bash
magento-cloud magento-cloud certificates [--domain DOMAIN] [--exclude-domain EXCLUDE-DOMAIN] [--issuer ISSUER] [--only-auto] [--no-auto] [--ignore-expiry] [--only-expired] [--no-expired] [--pipe-domains] [--date-fmt DATE-FMT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-p|--project PROJECT]
```

列出项目证书

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--domain`

按域名筛选（不区分大小写的搜索）

- 需要一个值

#### `--exclude-domain`

排除证书，按域名匹配（搜索不区分大小写）

- 需要一个值

#### `--issuer`

按颁发者筛选

- 需要一个值

#### `--only-auto`

仅显示自动配置的证书

- 默认： `false`
- 不接受值

#### `--no-auto`

仅显示手动添加的证书

- 默认： `false`
- 不接受值

#### `--ignore-expiry`

显示过期和未过期的证书

- 默认： `false`
- 不接受值

#### `--only-expired`

仅显示过期的证书

- 默认： `false`
- 不接受值

#### `--no-expired`

仅显示未过期的证书（默认）

- 默认： `false`
- 不接受值

#### `--pipe-domains`

仅返回证书涵盖的域名列表

- 默认： `false`
- 不接受值

#### `--date-fmt`

日期格式（作为PHP日期格式字符串）

- 默认： `c`
- 需要一个值

#### `--format`

输出格式：table、csv、tsv或plain

- 默认： `table`
- 需要一个值

#### `--columns`，`-c`

要显示的列。 可用列：已创建、域、过期、ID、颁发者。 %或*字符可用作通配符。 值可以用逗号（例如“a，b，c”）和/或空格拆分。

- 默认： `[]`
- 需要一个值

#### `--no-header`

不输出表头

- 默认： `false`
- 不接受值

#### `--project`，`-p`

项目ID或URL

- 需要一个值


## `commit:get`

```bash
magento-cloud magento-cloud commit:get [-P|--property PROPERTY] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--date-fmt DATE-FMT] [--] [<commit>]
```

显示提交详细信息

### 参数

#### `commit`

承诺SHA。 这还可以接受父项提交的“HEAD”、插入符号(^)或波状符号(~)后缀。

- 默认： `HEAD`

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--property`，`-P`

要显示的提交属性。

- 需要一个值

#### `--project`，`-p`

项目ID或URL

- 需要一个值

#### `--environment`，`-e`

环境ID 使用“。” 以选择项目的默认环境。

- 需要一个值

#### `--date-fmt`

日期格式（作为PHP日期格式字符串）

- 默认： `c`
- 需要一个值


## `commit:list`

```bash
magento-cloud magento-cloud commits [--limit LIMIT] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [--date-fmt DATE-FMT] [--] [<commit>]
```

列表提交

### 参数

#### `commit`

起始Git提交SHA。 这还可以接受父项提交的“HEAD”、插入符号(^)或波状符号(~)后缀。

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--limit`

要显示的承诺数。

- 默认： `10`
- 需要一个值

#### `--project`，`-p`

项目ID或URL

- 需要一个值

#### `--environment`，`-e`

环境ID 使用“。” 以选择项目的默认环境。

- 需要一个值

#### `--format`

输出格式：table、csv、tsv或plain

- 默认： `table`
- 需要一个值

#### `--columns`，`-c`

要显示的列。 可用列：作者、日期、sha、概要。 %或*字符可用作通配符。 值可以用逗号（例如“a，b，c”）和/或空格拆分。

- 默认： `[]`
- 需要一个值

#### `--no-header`

不输出表头

- 默认： `false`
- 不接受值

#### `--date-fmt`

日期格式（作为PHP日期格式字符串）

- 默认： `c`
- 需要一个值


## `db:dump`

```bash
magento-cloud magento-cloud db:dump [--schema SCHEMA] [-f|--file FILE] [-d|--directory DIRECTORY] [-z|--gzip] [-t|--timestamp] [-o|--stdout] [--table TABLE] [--exclude-table EXCLUDE-TABLE] [--schema-only] [--charset CHARSET] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [-r|--relationship RELATIONSHIP] [-i|--identity-file IDENTITY-FILE]
```

创建远程数据库的本地转储

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--schema`

要转储的架构。 忽略以使用默认架构（通常为“main”）。

- 需要一个值

#### `--file`，`-f`

转储的自定义文件名

- 需要一个值

#### `--directory`，`-d`

转储的自定义目录

- 需要一个值

#### `--gzip`，`-z`

使用gzip压缩转储

- 默认： `false`
- 不接受值

#### `--timestamp`，`-t`

向转储文件名添加时间戳

- 默认： `false`
- 不接受值

#### `--stdout`，`-o`

输出到STDOUT而不是文件

- 默认： `false`
- 不接受值

#### `--table`

要包含的表

- 默认： `[]`
- 需要一个值

#### `--exclude-table`

要排除的表

- 默认： `[]`
- 需要一个值

#### `--schema-only`

仅转储架构，无数据

- 默认： `false`
- 不接受值

#### `--charset`

转储的字符集编码

- 需要一个值

#### `--project`，`-p`

项目ID或URL

- 需要一个值

#### `--environment`，`-e`

环境ID 使用“。” 以选择项目的默认环境。

- 需要一个值

#### `--app`，`-A`

远程应用程序名称

- 需要一个值

#### `--relationship`，`-r`

要使用的服务关系

- 需要一个值

#### `--identity-file`，`-i`

要使用的SSH身份（私钥）

- 需要一个值


## `db:size`

```bash
magento-cloud magento-cloud db:size [-B|--bytes] [-C|--cleanup] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [-r|--relationship RELATIONSHIP] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-i|--identity-file IDENTITY-FILE]
```

估计数据库的磁盘使用情况

```
This is an estimate of the database disk usage. The real size on disk is usually higher because of overhead.

To see more accurate disk usage, run: magento-cloud disk
```

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--bytes`，`-B`

以字节为单位显示大小。

- 默认： `false`
- 不接受值

#### `--cleanup`，`-C`

检查是否可以清除表并显示建议（仅限InnoDb）。

- 默认： `false`
- 不接受值

#### `--project`，`-p`

项目ID或URL

- 需要一个值

#### `--environment`，`-e`

环境ID 使用“。” 以选择项目的默认环境。

- 需要一个值

#### `--app`，`-A`

远程应用程序名称

- 需要一个值

#### `--relationship`，`-r`

要使用的服务关系

- 需要一个值

#### `--format`

输出格式：table、csv、tsv或plain

- 默认： `table`
- 需要一个值

#### `--columns`，`-c`

要显示的列。 可用列：max、percent_used、used。 %或*字符可用作通配符。 值可以用逗号（例如“a，b，c”）和/或空格拆分。

- 默认： `[]`
- 需要一个值

#### `--no-header`

不输出表头

- 默认： `false`
- 不接受值

#### `--identity-file`，`-i`

要使用的SSH身份（私钥）

- 需要一个值


## `db:sql`

```bash
magento-cloud magento-cloud sql [--raw] [--schema SCHEMA] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [-r|--relationship RELATIONSHIP] [-i|--identity-file IDENTITY-FILE] [--] [<query>]
```

在远程数据库上运行SQL

### 参数

#### `query`

要执行的SQL语句

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--raw`

生产原始、非表格式输出

- 默认： `false`
- 不接受值

#### `--schema`

要使用的架构。 忽略以使用默认架构（通常为“main”）。 传递空字符串以不使用任何架构。

- 需要一个值

#### `--project`，`-p`

项目ID或URL

- 需要一个值

#### `--environment`，`-e`

环境ID 使用“。” 以选择项目的默认环境。

- 需要一个值

#### `--app`，`-A`

远程应用程序名称

- 需要一个值

#### `--relationship`，`-r`

要使用的服务关系

- 需要一个值

#### `--identity-file`，`-i`

要使用的SSH身份（私钥）

- 需要一个值


## `domain:add`

```bash
magento-cloud magento-cloud domain:add [--cert CERT] [--key KEY] [--chain CHAIN] [--attach ATTACH] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] <name>
```

向项目添加新域

### 参数

#### `name`

域名

- 必填

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--cert`

此域的证书文件的路径

- 需要一个值

#### `--key`

所提供证书的私钥文件的路径。

- 需要一个值

#### `--chain`

提供的证书指向一个或多个证书链文件的路径

- 默认： `[]`
- 需要一个值

#### `--attach`

在环境的路由中替换的生产域。 对于非生产环境域是必需的。

- 需要一个值

#### `--project`，`-p`

项目ID或URL

- 需要一个值

#### `--environment`，`-e`

环境ID 使用“。” 以选择项目的默认环境。

- 需要一个值

#### `--no-wait`，`-W`

不要等待操作完成

- 默认： `false`
- 不接受值

#### `--wait`

等待操作完成（默认）

- 默认： `false`
- 不接受值


## `domain:delete`

```bash
magento-cloud magento-cloud domain:delete [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] <name>
```

从项目中删除域

### 参数

#### `name`

域名

- 必填

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--project`，`-p`

项目ID或URL

- 需要一个值

#### `--environment`，`-e`

环境ID 使用“。” 以选择项目的默认环境。

- 需要一个值

#### `--no-wait`，`-W`

不要等待操作完成

- 默认： `false`
- 不接受值

#### `--wait`

等待操作完成（默认）

- 默认： `false`
- 不接受值


## `domain:get`

```bash
magento-cloud magento-cloud domain:get [-P|--property PROPERTY] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [--date-fmt DATE-FMT] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--] [<name>]
```

显示域的详细信息

### 参数

#### `name`

域名

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--property`，`-P`

要查看的域属性

- 需要一个值

#### `--format`

输出格式：table、csv、tsv或plain

- 默认： `table`
- 需要一个值

#### `--columns`，`-c`

要显示的列。 %或*字符可用作通配符。 值可以用逗号（例如“a，b，c”）和/或空格拆分。

- 默认： `[]`
- 需要一个值

#### `--no-header`

不输出表头

- 默认： `false`
- 不接受值

#### `--date-fmt`

日期格式（作为PHP日期格式字符串）

- 默认： `c`
- 需要一个值

#### `--project`，`-p`

项目ID或URL

- 需要一个值

#### `--environment`，`-e`

环境ID 使用“。” 以选择项目的默认环境。

- 需要一个值


## `domain:list`

```bash
magento-cloud magento-cloud domains [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-p|--project PROJECT] [-e|--environment ENVIRONMENT]
```

获取所有域的列表

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--format`

输出格式：table、csv、tsv或plain

- 默认： `table`
- 需要一个值

#### `--columns`，`-c`

要显示的列。 可用列：name*、ssl*、created_at*、registered_name、replacement_for、type、updated_at （* =缺省列）。 字符“+”可用作默认列的占位符。 %或*字符可用作通配符。 值可以用逗号（例如“a，b，c”）和/或空格拆分。

- 默认： `[]`
- 需要一个值

#### `--no-header`

不输出表头

- 默认： `false`
- 不接受值

#### `--project`，`-p`

项目ID或URL

- 需要一个值

#### `--environment`，`-e`

环境ID 使用“。” 以选择项目的默认环境。

- 需要一个值


## `domain:update`

```bash
magento-cloud magento-cloud domain:update [--cert CERT] [--key KEY] [--chain CHAIN] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] <name>
```

更新域

### 参数

#### `name`

域名

- 必填

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--cert`

此域的证书文件的路径

- 需要一个值

#### `--key`

所提供证书的私钥文件的路径。

- 需要一个值

#### `--chain`

提供的证书指向一个或多个证书链文件的路径

- 默认： `[]`
- 需要一个值

#### `--project`，`-p`

项目ID或URL

- 需要一个值

#### `--environment`，`-e`

环境ID 使用“。” 以选择项目的默认环境。

- 需要一个值

#### `--no-wait`，`-W`

不要等待操作完成

- 默认： `false`
- 不接受值

#### `--wait`

等待操作完成（默认）

- 默认： `false`
- 不接受值


## `environment:activate`

```bash
magento-cloud magento-cloud environment:activate [--parent PARENT] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] [<environment>]...
```

激活环境

### 参数

#### `environment`

要激活的环境

- 默认： `[]`
- 数组

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--parent`

在激活之前设置新的环境父级

- 需要一个值

#### `--project`，`-p`

项目ID或URL

- 需要一个值

#### `--environment`，`-e`

环境ID 使用“。” 以选择项目的默认环境。

- 需要一个值

#### `--no-wait`，`-W`

不要等待操作完成

- 默认： `false`
- 不接受值

#### `--wait`

等待操作完成（默认）

- 默认： `false`
- 不接受值


## `environment:branch`

```bash
magento-cloud magento-cloud branch [--title TITLE] [--type TYPE] [--no-clone-parent] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] [<id>] [<parent>]
```

分支环境

### 参数

#### `id`

新环境的ID（分支名称）


#### `parent`

新环境的父级

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--title`

新环境的标题

- 需要一个值

#### `--type`

新环境的类型

- 需要一个值

#### `--no-clone-parent`

不克隆父环境的数据

- 默认： `false`
- 不接受值

#### `--project`，`-p`

项目ID或URL

- 需要一个值

#### `--environment`，`-e`

环境ID 使用“。” 以选择项目的默认环境。

- 需要一个值

#### `--no-wait`，`-W`

不要等待操作完成

- 默认： `false`
- 不接受值

#### `--wait`

等待操作完成（默认）

- 默认： `false`
- 不接受值


## `environment:checkout`

```bash
magento-cloud magento-cloud checkout [-i|--identity-file IDENTITY-FILE] [--] [<id>]
```

查看环境

### 参数

#### `id`

要签出的环境的ID。 例如：“sprint2”

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--identity-file`，`-i`

要使用的SSH身份（私钥）

- 需要一个值


## `environment:delete`

```bash
magento-cloud magento-cloud environment:delete [--delete-branch] [--no-delete-branch] [--type TYPE] [-t|--only-type ONLY-TYPE] [--exclude EXCLUDE] [--exclude-type EXCLUDE-TYPE] [--inactive] [--merged] [--allow-delete-parent] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] [<environment>]...
```

删除一个或多个环境

```
When a Magento Cloud environment is deleted, it will become "inactive": it will
exist only as a Git branch, containing code but no services, databases nor
files.

This command allows you to delete environments as well as their Git branches.
```

### 参数

#### `environment`

要删除的环境。 %或*字符可用作通配符。 值可以用逗号（例如“a，b，c”）和/或空格拆分。

- 默认： `[]`
- 数组

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--delete-branch`

删除非活动环境的Git分支，无需确认

- 默认： `false`
- 不接受值

#### `--no-delete-branch`

请勿删除任何Git分支（非活动环境）

- 默认： `false`
- 不接受值

#### `--type`

删除某个类型的所有环境（添加到任何其他选定环境）值可能会被逗号（例如“a，b，c”）和/或空格拆分。

- 默认： `[]`
- 需要一个值

#### `--only-type`，`-t`

只有特定类型的删除环境值才可以通过逗号（例如“a，b，c”）和/或空格进行拆分。

- 默认： `[]`
- 需要一个值

#### `--exclude`

不删除的环境。 %或*字符可用作通配符。 值可以用逗号（例如“a，b，c”）和/或空格拆分。

- 默认： `[]`
- 需要一个值

#### `--exclude-type`

不得删除的环境类型值可能会被逗号（例如“a，b，c”）和/或空格拆分。

- 默认： `[]`
- 需要一个值

#### `--inactive`

删除所有非活动环境（添加到任何其他选定环境）

- 默认： `false`
- 不接受值

#### `--merged`

删除所有合并的环境（添加到任何其他选定的环境）

- 默认： `false`
- 不接受值

#### `--allow-delete-parent`

允许删除具有子项的环境

- 默认： `false`
- 不接受值

#### `--project`，`-p`

项目ID或URL

- 需要一个值

#### `--environment`，`-e`

环境ID 使用“。” 以选择项目的默认环境。

- 需要一个值

#### `--no-wait`，`-W`

不要等待操作完成

- 默认： `false`
- 不接受值

#### `--wait`

等待操作完成（默认）

- 默认： `false`
- 不接受值


## `environment:http-access`

```bash
magento-cloud magento-cloud httpaccess [--access ACCESS] [--auth AUTH] [--enabled ENABLED] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait]
```

更新环境的HTTP访问设置

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--access`

以“permission：address”格式显示的访问限制。 使用0清除所有地址。

- 默认： `[]`
- 需要一个值

#### `--auth`

HTTP基本身份验证凭据，格式为“username：password”。 使用0可清除所有凭据。

- 默认： `[]`
- 需要一个值

#### `--enabled`

是否应启用访问控制：1表示启用，0表示禁用

- 需要一个值

#### `--project`，`-p`

项目ID或URL

- 需要一个值

#### `--environment`，`-e`

环境ID 使用“。” 以选择项目的默认环境。

- 需要一个值

#### `--no-wait`，`-W`

不要等待操作完成

- 默认： `false`
- 不接受值

#### `--wait`

等待操作完成（默认）

- 默认： `false`
- 不接受值


## `environment:info`

```bash
magento-cloud magento-cloud environment:info [--refresh] [--date-fmt DATE-FMT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] [<property>] [<value>]
```

读取或设置环境的属性

### 参数

#### `property`

属性的名称


#### `value`

为属性设置新值

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--refresh`

是否刷新缓存

- 默认： `false`
- 不接受值

#### `--date-fmt`

日期格式（作为PHP日期格式字符串）

- 默认： `c`
- 需要一个值

#### `--format`

输出格式：table、csv、tsv或plain

- 默认： `table`
- 需要一个值

#### `--columns`，`-c`

要显示的列。 %或*字符可用作通配符。 值可以用逗号（例如“a，b，c”）和/或空格拆分。

- 默认： `[]`
- 需要一个值

#### `--no-header`

不输出表头

- 默认： `false`
- 不接受值

#### `--project`，`-p`

项目ID或URL

- 需要一个值

#### `--environment`，`-e`

环境ID 使用“。” 以选择项目的默认环境。

- 需要一个值

#### `--no-wait`，`-W`

不要等待操作完成

- 默认： `false`
- 不接受值

#### `--wait`

等待操作完成（默认）

- 默认： `false`
- 不接受值


## `environment:init`

```bash
magento-cloud magento-cloud environment:init [--profile PROFILE] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] <url>
```

从公共Git存储库初始化环境

### 参数

#### `url`

Git存储库的URL

- 必填

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--profile`

配置文件的名称

- 需要一个值

#### `--project`，`-p`

项目ID或URL

- 需要一个值

#### `--environment`，`-e`

环境ID 使用“。” 以选择项目的默认环境。

- 需要一个值

#### `--no-wait`，`-W`

不要等待操作完成

- 默认： `false`
- 不接受值

#### `--wait`

等待操作完成（默认）

- 默认： `false`
- 不接受值


## `environment:list`

```bash
magento-cloud magento-cloud environments [-I|--no-inactive] [--pipe] [--refresh REFRESH] [--sort SORT] [--reverse] [--type TYPE] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-p|--project PROJECT]
```

获取环境列表

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--no-inactive`，`-I`

不显示非活动环境

- 默认： `false`
- 不接受值

#### `--pipe`

输出环境ID的简单列表。

- 默认： `false`
- 不接受值

#### `--refresh`

是否刷新列表。

- 默认： `1`
- 需要一个值

#### `--sort`

排序依据的属性

- 默认： `title`
- 需要一个值

#### `--reverse`

按反向（降序）排序

- 默认： `false`
- 不接受值

#### `--type`

按环境类型筛选列表。 值可以用逗号（例如“a，b，c”）和/或空格拆分。

- 默认： `[]`
- 需要一个值

#### `--format`

输出格式：table、csv、tsv或plain

- 默认： `table`
- 需要一个值

#### `--columns`，`-c`

要显示的列。 可用列： id*、title*、status*、type*、created、machine_name、updated （* =默认列）。 字符“+”可用作默认列的占位符。 %或*字符可用作通配符。 值可以用逗号（例如“a，b，c”）和/或空格拆分。

- 默认： `[]`
- 需要一个值

#### `--no-header`

不输出表头

- 默认： `false`
- 不接受值

#### `--project`，`-p`

项目ID或URL

- 需要一个值


## `environment:logs`

```bash
magento-cloud magento-cloud log [--lines LINES] [--tail] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [--worker WORKER] [-I|--instance INSTANCE] [--] [<type>]
```

读取环境的日志

### 参数

#### `type`

日志类型，如“访问”或“错误”

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--lines`

要显示的行数

- 默认： `100`
- 需要一个值

#### `--tail`

持续跟踪日志

- 默认： `false`
- 不接受值

#### `--project`，`-p`

项目ID或URL

- 需要一个值

#### `--environment`，`-e`

环境ID 使用“。” 以选择项目的默认环境。

- 需要一个值

#### `--app`，`-A`

远程应用程序名称

- 需要一个值

#### `--worker`

工作人员姓名

- 需要一个值

#### `--instance`，`-I`

实例ID

- 需要一个值


## `environment:merge`

```bash
magento-cloud magento-cloud merge [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] [<environment>]
```

合并环境

```
This command will initiate a Git merge of the specified environment into its parent environment.
```

### 参数

#### `environment`

要合并的环境

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--project`，`-p`

项目ID或URL

- 需要一个值

#### `--environment`，`-e`

环境ID 使用“。” 以选择项目的默认环境。

- 需要一个值

#### `--no-wait`，`-W`

不要等待操作完成

- 默认： `false`
- 不接受值

#### `--wait`

等待操作完成（默认）

- 默认： `false`
- 不接受值


## `environment:pause`

```bash
magento-cloud magento-cloud environment:pause [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait]
```

暂停环境

```
Pausing an environment helps to reduce resource consumption and carbon emissions.

The environment will be unavailable until it is resumed. No data will be lost.
```

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--project`，`-p`

项目ID或URL

- 需要一个值

#### `--environment`，`-e`

环境ID 使用“。” 以选择项目的默认环境。

- 需要一个值

#### `--no-wait`，`-W`

不要等待操作完成

- 默认： `false`
- 不接受值

#### `--wait`

等待操作完成（默认）

- 默认： `false`
- 不接受值


## `environment:push`

```bash
magento-cloud magento-cloud push [--target TARGET] [-f|--force] [--force-with-lease] [-u|--set-upstream] [--activate] [--parent PARENT] [--type TYPE] [--no-clone-parent] [-W|--no-wait] [--wait] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-i|--identity-file IDENTITY-FILE] [--] [<source>]
```

将代码推送到环境

### 参数

#### `source`

源引用：分支名称或提交哈希

- 默认： `HEAD`

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--target`

目标分支名称。 默认为当前分支。

- 需要一个值

#### `--force`，`-f`

允许非快速转发更新

- 默认： `false`
- 不接受值

#### `--force-with-lease`

如果远程跟踪分支是最新的，则允许非快速转发更新

- 默认： `false`
- 不接受值

#### `--set-upstream`，`-u`

将目标环境设置为源分支的上游。 这还会将目标项目设置为本地存储库的远程存储库。

- 默认： `false`
- 不接受值

#### `--activate`

在推送之前激活环境

- 默认： `false`
- 不接受值

#### `--parent`

设置新环境父级（仅与 — activate一起使用）

- 需要一个值

#### `--type`

设置环境类型（仅与 — activate一起使用）

- 需要一个值

#### `--no-clone-parent`

不克隆父分支的数据（仅与 — activate一起使用）

- 默认： `false`
- 不接受值

#### `--no-wait`，`-W`

不要等待操作完成

- 默认： `false`
- 不接受值

#### `--wait`

等待操作完成（默认）

- 默认： `false`
- 不接受值

#### `--project`，`-p`

项目ID或URL

- 需要一个值

#### `--environment`，`-e`

环境ID 使用“。” 以选择项目的默认环境。

- 需要一个值

#### `--identity-file`，`-i`

要使用的SSH身份（私钥）

- 需要一个值


## `environment:redeploy`

```bash
magento-cloud magento-cloud redeploy [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait]
```

重新部署环境

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--project`，`-p`

项目ID或URL

- 需要一个值

#### `--environment`，`-e`

环境ID 使用“。” 以选择项目的默认环境。

- 需要一个值

#### `--no-wait`，`-W`

不要等待操作完成

- 默认： `false`
- 不接受值

#### `--wait`

等待操作完成（默认）

- 默认： `false`
- 不接受值


## `environment:relationships`

```bash
magento-cloud magento-cloud relationships [-P|--property PROPERTY] [--refresh] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [-i|--identity-file IDENTITY-FILE] [--] [<environment>]
```

显示环境关系

### 参数

#### `environment`

环境

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--property`，`-P`

要查看的关系属性

- 需要一个值

#### `--refresh`

是否刷新关系

- 默认： `false`
- 不接受值

#### `--project`，`-p`

项目ID或URL

- 需要一个值

#### `--environment`，`-e`

环境ID 使用“。” 以选择项目的默认环境。

- 需要一个值

#### `--app`，`-A`

远程应用程序名称

- 需要一个值

#### `--identity-file`，`-i`

要使用的SSH身份（私钥）

- 需要一个值


## `environment:resume`

```bash
magento-cloud magento-cloud environment:resume [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait]
```

恢复暂停的环境

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--project`，`-p`

项目ID或URL

- 需要一个值

#### `--environment`，`-e`

环境ID 使用“。” 以选择项目的默认环境。

- 需要一个值

#### `--no-wait`，`-W`

不要等待操作完成

- 默认： `false`
- 不接受值

#### `--wait`

等待操作完成（默认）

- 默认： `false`
- 不接受值


## `environment:scp`

```bash
magento-cloud magento-cloud scp [-r|--recursive] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [--worker WORKER] [-I|--instance INSTANCE] [-i|--identity-file IDENTITY-FILE] [--] [<files>]...
```

使用scp将文件复制到环境中或从环境中复制文件

### 参数

#### `files`

要复制的文件。 使用remote：前缀定义远程位置。

- 默认： `[]`
- 数组

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--recursive`，`-r`

递归复制整个目录

- 默认： `false`
- 不接受值

#### `--project`，`-p`

项目ID或URL

- 需要一个值

#### `--environment`，`-e`

环境ID 使用“。” 以选择项目的默认环境。

- 需要一个值

#### `--app`，`-A`

远程应用程序名称

- 需要一个值

#### `--worker`

工作人员姓名

- 需要一个值

#### `--instance`，`-I`

实例ID

- 需要一个值

#### `--identity-file`，`-i`

要使用的SSH身份（私钥）

- 需要一个值


## `environment:ssh`

```bash
magento-cloud magento-cloud ssh [--pipe] [--all] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [--worker WORKER] [-I|--instance INSTANCE] [-i|--identity-file IDENTITY-FILE] [--] [<cmd>]...
```

SSH到当前环境

### 参数

#### `cmd`

要在环境中运行的命令。

- 默认： `[]`
- 数组

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--pipe`

仅输出SSH URL。

- 默认： `false`
- 不接受值

#### `--all`

输出所有SSH URL（对于每个应用程序）。

- 默认： `false`
- 不接受值

#### `--project`，`-p`

项目ID或URL

- 需要一个值

#### `--environment`，`-e`

环境ID 使用“。” 以选择项目的默认环境。

- 需要一个值

#### `--app`，`-A`

远程应用程序名称

- 需要一个值

#### `--worker`

工作人员姓名

- 需要一个值

#### `--instance`，`-I`

实例ID

- 需要一个值

#### `--identity-file`，`-i`

要使用的SSH身份（私钥）

- 需要一个值


## `environment:synchronize`

```bash
magento-cloud magento-cloud sync [--rebase] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] [<synchronize>]...
```

同步环境的代码和/或来自其父级的数据

```
This command synchronizes to a child environment from its parent environment.

Synchronizing "code" means there will be a Git merge from the parent to the
child. Synchronizing "data" means that all files in all services (including
static files, databases, logs, search indices, etc.) will be copied from the
parent to the child.
```

### 参数

#### `synchronize`

要同步的内容：“代码”、“数据”或两者

- 默认： `[]`
- 数组

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--rebase`

通过重新基准而不是合并来同步代码

- 默认： `false`
- 不接受值

#### `--project`，`-p`

项目ID或URL

- 需要一个值

#### `--environment`，`-e`

环境ID 使用“。” 以选择项目的默认环境。

- 需要一个值

#### `--no-wait`，`-W`

不要等待操作完成

- 默认： `false`
- 不接受值

#### `--wait`

等待操作完成（默认）

- 默认： `false`
- 不接受值


## `environment:url`

```bash
magento-cloud magento-cloud url [-1|--primary] [--browser BROWSER] [--pipe] [-p|--project PROJECT] [-e|--environment ENVIRONMENT]
```

获取环境的公共URL

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--primary`，`-1`

仅返回主路由的URL

- 默认： `false`
- 不接受值

#### `--browser`

用于打开URL的浏览器。 将0设置为“无”。

- 需要一个值

#### `--pipe`

将URL输出到stdout。

- 默认： `false`
- 不接受值

#### `--project`，`-p`

项目ID或URL

- 需要一个值

#### `--environment`，`-e`

环境ID 使用“。” 以选择项目的默认环境。

- 需要一个值


## `environment:xdebug`

```bash
magento-cloud magento-cloud xdebug [--port PORT] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [--worker WORKER] [-I|--instance INSTANCE] [-i|--identity-file IDENTITY-FILE]
```

打开通往Xdebug环境的隧道

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--port`

本地端口

- 默认： `9000`
- 需要一个值

#### `--project`，`-p`

项目ID或URL

- 需要一个值

#### `--environment`，`-e`

环境ID 使用“。” 以选择项目的默认环境。

- 需要一个值

#### `--app`，`-A`

远程应用程序名称

- 需要一个值

#### `--worker`

工作人员姓名

- 需要一个值

#### `--instance`，`-I`

实例ID

- 需要一个值

#### `--identity-file`，`-i`

要使用的SSH身份（私钥）

- 需要一个值


## `integration:activity:get`

```bash
magento-cloud magento-cloud integration:activity:get [-P|--property PROPERTY] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [--date-fmt DATE-FMT] [--] [<integration>] [<activity>]
```

查看有关单个集成活动的详细信息

### 参数

#### `integration`

集成ID。 留空可从列表中选择。


#### `activity`

活动ID 默认为最新的集成活动。

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--property`，`-P`

要查看的属性

- 需要一个值

#### `--project`，`-p`

项目ID或URL

- 需要一个值

#### `--environment`，`-e`

[已弃用选项，未使用]

- 需要一个值

#### `--format`

输出格式：table、csv、tsv或plain

- 默认： `table`
- 需要一个值

#### `--columns`，`-c`

要显示的列。 %或*字符可用作通配符。 值可以用逗号（例如“a，b，c”）和/或空格拆分。

- 默认： `[]`
- 需要一个值

#### `--no-header`

不输出表头

- 默认： `false`
- 不接受值

#### `--date-fmt`

日期格式（作为PHP日期格式字符串）

- 默认： `c`
- 需要一个值


## `integration:activity:list`

```bash
magento-cloud magento-cloud int:act [--type TYPE] [-x|--exclude-type EXCLUDE-TYPE] [--limit LIMIT] [--start START] [--state STATE] [--result RESULT] [-i|--incomplete] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [--date-fmt DATE-FMT] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--] [<id>]
```

获取集成的活动列表

### 参数

#### `id`

集成ID。 留空可从列表中选择。

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--type`

按类型筛选活动。 值可以用逗号（例如“a，b，c”）和/或空格拆分。

- 默认： `[]`
- 需要一个值

#### `--exclude-type`，`-x`

按类型排除活动。 值可以用逗号（例如“a，b，c”）和/或空格拆分。 %或*字符可用作排除类型的通配符。

- 默认： `[]`
- 需要一个值

#### `--limit`

限制显示的结果数

- 默认： `10`
- 需要一个值

#### `--start`

将仅列出在此日期之前创建的活动

- 需要一个值

#### `--state`

按状态筛选活动。 值可以用逗号（例如“a，b，c”）和/或空格拆分。

- 默认： `[]`
- 需要一个值

#### `--result`

按结果筛选活动

- 需要一个值

#### `--incomplete`，`-i`

仅列出未完成的活动

- 默认： `false`
- 不接受值

#### `--format`

输出格式：table、csv、tsv或plain

- 默认： `table`
- 需要一个值

#### `--columns`，`-c`

要显示的列。 可用列： id*、created*、description*、type*、state*、result*、completed （* =默认列）。 字符“+”可用作默认列的占位符。 %或*字符可用作通配符。 值可以用逗号（例如“a，b，c”）和/或空格拆分。

- 默认： `[]`
- 需要一个值

#### `--no-header`

不输出表头

- 默认： `false`
- 不接受值

#### `--date-fmt`

日期格式（作为PHP日期格式字符串）

- 默认： `c`
- 需要一个值

#### `--project`，`-p`

项目ID或URL

- 需要一个值

#### `--environment`，`-e`

[已弃用选项，未使用]

- 需要一个值


## `integration:activity:log`

```bash
magento-cloud magento-cloud integration:activity:log [-t|--timestamps] [--date-fmt DATE-FMT] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--] [<integration>] [<activity>]
```

显示集成活动的日志

### 参数

#### `integration`

集成ID。 留空可从列表中选择。


#### `activity`

活动ID 默认为最新的集成活动。

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--timestamps`，`-t`

在每个消息旁边显示时间戳

- 默认： `false`
- 不接受值

#### `--date-fmt`

日期格式（作为PHP日期格式字符串）

- 默认： `c`
- 需要一个值

#### `--project`，`-p`

项目ID或URL

- 需要一个值

#### `--environment`，`-e`

[已弃用选项，未使用]

- 需要一个值


## `integration:add`

```bash
magento-cloud magento-cloud integration:add [--type TYPE] [--base-url BASE-URL] [--bitbucket-url BITBUCKET-URL] [--username USERNAME] [--token TOKEN] [--key KEY] [--secret SECRET] [--license-key LICENSE-KEY] [--server-project SERVER-PROJECT] [--repository REPOSITORY] [--build-merge-requests BUILD-MERGE-REQUESTS] [--build-pull-requests BUILD-PULL-REQUESTS] [--build-draft-pull-requests BUILD-DRAFT-PULL-REQUESTS] [--build-pull-requests-post-merge BUILD-PULL-REQUESTS-POST-MERGE] [--build-wip-merge-requests BUILD-WIP-MERGE-REQUESTS] [--merge-requests-clone-parent-data MERGE-REQUESTS-CLONE-PARENT-DATA] [--pull-requests-clone-parent-data PULL-REQUESTS-CLONE-PARENT-DATA] [--resync-pull-requests RESYNC-PULL-REQUESTS] [--fetch-branches FETCH-BRANCHES] [--prune-branches PRUNE-BRANCHES] [--resources-init RESOURCES-INIT] [--url URL] [--shared-key SHARED-KEY] [--file FILE] [--events EVENTS] [--states STATES] [--environments ENVIRONMENTS] [--excluded-environments EXCLUDED-ENVIRONMENTS] [--from-address FROM-ADDRESS] [--recipients RECIPIENTS] [--channel CHANNEL] [--routing-key ROUTING-KEY] [--category CATEGORY] [--index INDEX] [--sourcetype SOURCETYPE] [--protocol PROTOCOL] [--syslog-host SYSLOG-HOST] [--syslog-port SYSLOG-PORT] [--facility FACILITY] [--message-format MESSAGE-FORMAT] [--auth-mode AUTH-MODE] [--auth-token AUTH-TOKEN] [--verify-tls VERIFY-TLS] [--header HEADER] [-p|--project PROJECT] [-W|--no-wait] [--wait]
```

将集成添加到项目

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--type`

集成类型(“bitbucket”、“bitbucket_server”、“github”、“gitlab”、“webhook”、“health.email”、“health.pagerduty”、“health.slack”、“health.webhook”、“httplog”、“script”、“newrelic”、“splunk”、“sumologic”、“syslog”)

- 需要一个值

#### `--base-url`

服务器安装的基本URL

- 需要一个值

#### `--bitbucket-url`

Bitbucket Server安装的基本URL

- 需要一个值

#### `--username`

Bitbucket服务器用户名

- 需要一个值

#### `--token`

集成的身份验证或访问令牌

- 需要一个值

#### `--key`

Bitbucket OAuth使用者密钥

- 需要一个值

#### `--secret`

Bitbucket OAuth使用者密码

- 需要一个值

#### `--license-key`

New Relic日志许可证密钥

- 需要一个值

#### `--server-project`

项目（例如“namespace/repo”）

- 需要一个值

#### `--repository`

要跟踪的存储库（例如，“所有者/存储库”）

- 需要一个值

#### `--build-merge-requests`

GitLab：将合并请求作为环境生成

- 默认： `true`
- 需要一个值

#### `--build-pull-requests`

将每个拉取请求构建为环境

- 默认： `true`
- 需要一个值

#### `--build-draft-pull-requests`

构建草稿拉取请求

- 默认： `true`
- 需要一个值

#### `--build-pull-requests-post-merge`

根据请求合并后的状态构建拉取请求

- 默认： `false`
- 需要一个值

#### `--build-wip-merge-requests`

GitLab：构建WIP合并请求

- 默认： `true`
- 需要一个值

#### `--merge-requests-clone-parent-data`

GitLab：克隆合并请求的数据

- 默认： `true`
- 需要一个值

#### `--pull-requests-clone-parent-data`

为拉取请求克隆父环境的数据

- 默认： `true`
- 需要一个值

#### `--resync-pull-requests`

重新同步每个内部版本中的拉取请求环境数据

- 默认： `false`
- 需要一个值

#### `--fetch-branches`

从远程获取所有分支（作为非活动环境）

- 默认： `true`
- 需要一个值

#### `--prune-branches`

删除远程数据库上不存在的分支

- 默认： `true`
- 需要一个值

#### `--resources-init`

初始化新服务时要使用的资源（“最小值”、“默认值”、“手动”、“父项”）

- 需要一个值

#### `--url`

集成的URL或API端点

- 需要一个值

#### `--shared-key`

Webhook： JWS共享密钥

- 需要一个值

#### `--file`

包含要上载的脚本的本地文件的名称

- 需要一个值

#### `--events`

要执行操作的事件列表，例如environment.push

- 默认： `*`
- 需要一个值

#### `--states`

要采取行动的状态列表，例如pending、in_progress、complete

- 默认： `complete`
- 需要一个值

#### `--environments`

要包含的环境ID

- 默认： `*`
- 需要一个值

#### `--excluded-environments`

要排除的环境ID

- 默认： `[]`
- 需要一个值

#### `--from-address`

警报电子邮件的[可选]自定义发件人地址

- 需要一个值

#### `--recipients`

收件人电子邮件地址

- 默认： `[]`
- 需要一个值

#### `--channel`

Slack渠道

- 需要一个值

#### `--routing-key`

PagerDuty路由密钥

- 需要一个值

#### `--category`

用于过滤的Sumo逻辑类别

- 需要一个值

#### `--index`

Splunk索引

- 需要一个值

#### `--sourcetype`

Splunk事件源类型

- 需要一个值

#### `--protocol`

Syslog传输协议(&#39;tcp&#39;、&#39;udp&#39;、&#39;tls&#39;)

- 默认： `tls`
- 需要一个值

#### `--syslog-host`

Syslog中继/收集器主机

- 需要一个值

#### `--syslog-port`

Syslog中继/收集器端口

- 需要一个值

#### `--facility`

Syslog工具

- 默认： `1`
- 需要一个值

#### `--message-format`

Syslog消息格式（“rfc3164”或“rfc5424”）

- 默认： `rfc5424`
- 需要一个值

#### `--auth-mode`

身份验证模式（“prefix”或“structured_data”）

- 默认： `prefix`
- 需要一个值

#### `--auth-token`

身份验证令牌

- 需要一个值

#### `--verify-tls`

是否应启用HTTPS证书验证（推荐）

- 默认： `true`
- 需要一个值

#### `--header`

要在POST请求中使用的HTTP标头。 用冒号(：)分隔名称和值。

- 默认： `[]`
- 需要一个值

#### `--project`，`-p`

项目ID或URL

- 需要一个值

#### `--no-wait`，`-W`

不要等待操作完成

- 默认： `false`
- 不接受值

#### `--wait`

等待操作完成（默认）

- 默认： `false`
- 不接受值


## `integration:delete`

```bash
magento-cloud magento-cloud integration:delete [-p|--project PROJECT] [-W|--no-wait] [--wait] [--] [<id>]
```

从项目中删除集成

### 参数

#### `id`

集成ID。 留空可从列表中选择。

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--project`，`-p`

项目ID或URL

- 需要一个值

#### `--no-wait`，`-W`

不要等待操作完成

- 默认： `false`
- 不接受值

#### `--wait`

等待操作完成（默认）

- 默认： `false`
- 不接受值


## `integration:get`

```bash
magento-cloud magento-cloud integration:get [-P|--property [PROPERTY]] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-p|--project PROJECT] [--] [<id>]
```

查看集成的详细信息

### 参数

#### `id`

集成ID。 留空可从列表中选择。

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--property`，`-P`

要查看的集成属性

- 接受值

#### `--format`

输出格式：table、csv、tsv或plain

- 默认： `table`
- 需要一个值

#### `--columns`，`-c`

要显示的列。 %或*字符可用作通配符。 值可以用逗号（例如“a，b，c”）和/或空格拆分。

- 默认： `[]`
- 需要一个值

#### `--no-header`

不输出表头

- 默认： `false`
- 不接受值

#### `--project`，`-p`

项目ID或URL

- 需要一个值


## `integration:list`

```bash
magento-cloud magento-cloud integrations [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-p|--project PROJECT]
```

查看项目集成的列表

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--format`

输出格式：table、csv、tsv或plain

- 默认： `table`
- 需要一个值

#### `--columns`，`-c`

要显示的列。 可用列：id、summary、type。 %或*字符可用作通配符。 值可以用逗号（例如“a，b，c”）和/或空格拆分。

- 默认： `[]`
- 需要一个值

#### `--no-header`

不输出表头

- 默认： `false`
- 不接受值

#### `--project`，`-p`

项目ID或URL

- 需要一个值


## `integration:update`

```bash
magento-cloud magento-cloud integration:update [--type TYPE] [--base-url BASE-URL] [--bitbucket-url BITBUCKET-URL] [--username USERNAME] [--token TOKEN] [--key KEY] [--secret SECRET] [--license-key LICENSE-KEY] [--server-project SERVER-PROJECT] [--repository REPOSITORY] [--build-merge-requests BUILD-MERGE-REQUESTS] [--build-pull-requests BUILD-PULL-REQUESTS] [--build-draft-pull-requests BUILD-DRAFT-PULL-REQUESTS] [--build-pull-requests-post-merge BUILD-PULL-REQUESTS-POST-MERGE] [--build-wip-merge-requests BUILD-WIP-MERGE-REQUESTS] [--merge-requests-clone-parent-data MERGE-REQUESTS-CLONE-PARENT-DATA] [--pull-requests-clone-parent-data PULL-REQUESTS-CLONE-PARENT-DATA] [--resync-pull-requests RESYNC-PULL-REQUESTS] [--fetch-branches FETCH-BRANCHES] [--prune-branches PRUNE-BRANCHES] [--resources-init RESOURCES-INIT] [--url URL] [--shared-key SHARED-KEY] [--file FILE] [--events EVENTS] [--states STATES] [--environments ENVIRONMENTS] [--excluded-environments EXCLUDED-ENVIRONMENTS] [--from-address FROM-ADDRESS] [--recipients RECIPIENTS] [--channel CHANNEL] [--routing-key ROUTING-KEY] [--category CATEGORY] [--index INDEX] [--sourcetype SOURCETYPE] [--protocol PROTOCOL] [--syslog-host SYSLOG-HOST] [--syslog-port SYSLOG-PORT] [--facility FACILITY] [--message-format MESSAGE-FORMAT] [--auth-mode AUTH-MODE] [--auth-token AUTH-TOKEN] [--verify-tls VERIFY-TLS] [--header HEADER] [-p|--project PROJECT] [-W|--no-wait] [--wait] [--] [<id>]
```

更新集成

### 参数

#### `id`

要更新的集成的ID

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--type`

集成类型(“bitbucket”、“bitbucket_server”、“github”、“gitlab”、“webhook”、“health.email”、“health.pagerduty”、“health.slack”、“health.webhook”、“httplog”、“script”、“newrelic”、“splunk”、“sumologic”、“syslog”)

- 需要一个值

#### `--base-url`

服务器安装的基本URL

- 需要一个值

#### `--bitbucket-url`

Bitbucket Server安装的基本URL

- 需要一个值

#### `--username`

Bitbucket服务器用户名

- 需要一个值

#### `--token`

集成的身份验证或访问令牌

- 需要一个值

#### `--key`

Bitbucket OAuth使用者密钥

- 需要一个值

#### `--secret`

Bitbucket OAuth使用者密码

- 需要一个值

#### `--license-key`

New Relic日志许可证密钥

- 需要一个值

#### `--server-project`

项目（例如“namespace/repo”）

- 需要一个值

#### `--repository`

要跟踪的存储库（例如，“所有者/存储库”）

- 需要一个值

#### `--build-merge-requests`

GitLab：将合并请求作为环境生成

- 默认： `true`
- 需要一个值

#### `--build-pull-requests`

将每个拉取请求构建为环境

- 默认： `true`
- 需要一个值

#### `--build-draft-pull-requests`

构建草稿拉取请求

- 默认： `true`
- 需要一个值

#### `--build-pull-requests-post-merge`

根据请求合并后的状态构建拉取请求

- 默认： `false`
- 需要一个值

#### `--build-wip-merge-requests`

GitLab：构建WIP合并请求

- 默认： `true`
- 需要一个值

#### `--merge-requests-clone-parent-data`

GitLab：克隆合并请求的数据

- 默认： `true`
- 需要一个值

#### `--pull-requests-clone-parent-data`

为拉取请求克隆父环境的数据

- 默认： `true`
- 需要一个值

#### `--resync-pull-requests`

重新同步每个内部版本中的拉取请求环境数据

- 默认： `false`
- 需要一个值

#### `--fetch-branches`

从远程获取所有分支（作为非活动环境）

- 默认： `true`
- 需要一个值

#### `--prune-branches`

删除远程数据库上不存在的分支

- 默认： `true`
- 需要一个值

#### `--resources-init`

初始化新服务时要使用的资源（“最小值”、“默认值”、“手动”、“父项”）

- 需要一个值

#### `--url`

集成的URL或API端点

- 需要一个值

#### `--shared-key`

Webhook： JWS共享密钥

- 需要一个值

#### `--file`

包含要上载的脚本的本地文件的名称

- 需要一个值

#### `--events`

要执行操作的事件列表，例如environment.push

- 默认： `*`
- 需要一个值

#### `--states`

要采取行动的状态列表，例如pending、in_progress、complete

- 默认： `complete`
- 需要一个值

#### `--environments`

要包含的环境ID

- 默认： `*`
- 需要一个值

#### `--excluded-environments`

要排除的环境ID

- 默认： `[]`
- 需要一个值

#### `--from-address`

警报电子邮件的[可选]自定义发件人地址

- 需要一个值

#### `--recipients`

收件人电子邮件地址

- 默认： `[]`
- 需要一个值

#### `--channel`

Slack渠道

- 需要一个值

#### `--routing-key`

PagerDuty路由密钥

- 需要一个值

#### `--category`

用于过滤的Sumo逻辑类别

- 需要一个值

#### `--index`

Splunk索引

- 需要一个值

#### `--sourcetype`

Splunk事件源类型

- 需要一个值

#### `--protocol`

Syslog传输协议(&#39;tcp&#39;、&#39;udp&#39;、&#39;tls&#39;)

- 默认： `tls`
- 需要一个值

#### `--syslog-host`

Syslog中继/收集器主机

- 需要一个值

#### `--syslog-port`

Syslog中继/收集器端口

- 需要一个值

#### `--facility`

Syslog工具

- 默认： `1`
- 需要一个值

#### `--message-format`

Syslog消息格式（“rfc3164”或“rfc5424”）

- 默认： `rfc5424`
- 需要一个值

#### `--auth-mode`

身份验证模式（“prefix”或“structured_data”）

- 默认： `prefix`
- 需要一个值

#### `--auth-token`

身份验证令牌

- 需要一个值

#### `--verify-tls`

是否应启用HTTPS证书验证（推荐）

- 默认： `true`
- 需要一个值

#### `--header`

要在POST请求中使用的HTTP标头。 用冒号(：)分隔名称和值。

- 默认： `[]`
- 需要一个值

#### `--project`，`-p`

项目ID或URL

- 需要一个值

#### `--no-wait`，`-W`

不要等待操作完成

- 默认： `false`
- 不接受值

#### `--wait`

等待操作完成（默认）

- 默认： `false`
- 不接受值


## `integration:validate`

```bash
magento-cloud magento-cloud integration:validate [-p|--project PROJECT] [--] [<id>]
```

验证现有集成

```
This command allows you to check whether an integration is valid.

An exit code of 0 means the integration is valid, while 4 means it is invalid.
Any other exit code indicates an unexpected error.

Integrations are validated automatically on creation and on update. However,
because they involve external resources, it is possible for a valid integration
to become invalid. For example, an access token may be revoked, or an external
repository may be deleted.
```

### 参数

#### `id`

集成ID。 留空可从列表中选择。

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--project`，`-p`

项目ID或URL

- 需要一个值


## `local:build`

```bash
magento-cloud magento-cloud build [-a|--abslinks] [-s|--source SOURCE] [-d|--destination DESTINATION] [-c|--copy] [--clone] [--run-deploy-hooks] [--no-clean] [--no-archive] [--no-backup] [--no-cache] [--no-build-hooks] [--no-deps] [--working-copy] [--concurrency CONCURRENCY] [--lock] [--] [<app>]...
```

在本地构建当前项目

### 参数

#### `app`

指定要生成的应用程序

- 默认： `[]`
- 数组

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--abslinks`，`-a`

使用绝对链接

- 默认： `false`
- 不接受值

#### `--source`，`-s`

源目录。 默认为当前项目的根目录。

- 需要一个值

#### `--destination`，`-d`

每个应用程序的Web根目录将符号链接到的目标。 默认： _www

- 需要一个值

#### `--copy`，`-c`

复制到生成目录，而不是从源进行符号链接

- 默认： `false`
- 不接受值

#### `--clone`

使用Git将当前HEAD克隆到构建目录

- 默认： `false`
- 不接受值

#### `--run-deploy-hooks`

运行部署和/或post_deploy挂钩

- 默认： `false`
- 不接受值

#### `--no-clean`

不删除旧的内部版本

- 默认： `false`
- 不接受值

#### `--no-archive`

不创建或使用生成存档

- 默认： `false`
- 不接受值

#### `--no-backup`

不备份以前的版本

- 默认： `false`
- 不接受值

#### `--no-cache`

禁用缓存

- 默认： `false`
- 不接受值

#### `--no-build-hooks`

不要运行生成后挂钩

- 默认： `false`
- 不接受值

#### `--no-deps`

不要在本地安装生成依赖关系

- 默认： `false`
- 不接受值

#### `--working-copy`

Drush：使用git克隆每个Drupal模块的存储库，而不是简单地下载版本

- 默认： `false`
- 不接受值

#### `--concurrency`

Drush：设置将同时处理的并发项目数

- 默认： `4`
- 需要一个值

#### `--lock`

Drush：创建或更新锁定文件（仅适用于Drush版本7+）

- 默认： `false`
- 不接受值


## `local:dir`

```bash
magento-cloud magento-cloud dir [<subdir>]
```

查找本地项目根

### 参数

#### `subdir`

要查找的子目录（“local”、“web”或“shared”）

### 选项

有关全局选项，请参阅[全局选项](#global-options)。


## `metrics:all`

```bash
magento-cloud magento-cloud metrics [-B|--bytes] [-r|--range RANGE] [-i|--interval INTERVAL] [--to TO] [-1|--latest] [-s|--service SERVICE] [--type TYPE] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [--date-fmt DATE-FMT]
```

Beta显示环境的CPU、磁盘和内存量度

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--bytes`，`-B`

以字节为单位显示大小

- 默认： `false`
- 不接受值

#### `--range`，`-r`

时间范围。 量度将在此持续时间内加载，直到结束时间（ — 到）。 您可以指定单位：小时(h)、分钟(m)或秒(s)。 最小5分钟，最大8小时或更多（取决于项目），默认为10分钟。

- 需要一个值

#### `--interval`，`-i`

时间间隔。 默认为范围的除数。 您可以指定单位：小时(h)、分钟(m)或秒(s)。 至少100万。

- 需要一个值

#### `--to`

结束时间。 默认为现在。

- 需要一个值

#### `--latest`，`-1`

仅显示最新的单个数据点

- 默认： `false`
- 不接受值

#### `--service`，`-s`

按服务或应用程序名称过滤%或*字符可用作通配符。

- 默认： `[]`
- 需要一个值

#### `--type`

按服务类型筛选（如果未提供 — 服务）。 版本不是必需的。 %或*字符可用作通配符。

- 默认： `[]`
- 需要一个值

#### `--project`，`-p`

项目ID或URL

- 需要一个值

#### `--environment`，`-e`

环境ID 使用“。” 以选择项目的默认环境。

- 需要一个值

#### `--format`

输出格式：table、csv、tsv或plain

- 默认： `table`
- 需要一个值

#### `--columns`，`-c`

要显示的列。 可用列：timestamp*、service*、cpu_percent*、mem_percent*、disk_percent*、tmp_disk_percent*、cpu_limit、cpu_used、disk_limit、disk_used、inodes_limit、inodes_percent、inodes_used、mem_limit、mem_used、tmp_disk_used、tmp_inodes_limit、tmp_inodes_percent、tmp_inodes_used、类型（* =默认列）。 字符“+”可用作默认列的占位符。 %或*字符可用作通配符。 值可以用逗号（例如“a，b，c”）和/或空格拆分。

- 默认： `[]`
- 需要一个值

#### `--no-header`

不输出表头

- 默认： `false`
- 不接受值

#### `--date-fmt`

日期格式（作为PHP日期格式字符串）

- 默认： `c`
- 需要一个值


## `metrics:cpu`

```bash
magento-cloud magento-cloud cpu [-r|--range RANGE] [-i|--interval INTERVAL] [--to TO] [-1|--latest] [-s|--service SERVICE] [--type TYPE] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [--date-fmt DATE-FMT]
```

Beta显示环境的CPU使用情况

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--range`，`-r`

时间范围。 量度将在此持续时间内加载，直到结束时间（ — 到）。 您可以指定单位：小时(h)、分钟(m)或秒(s)。 最小5分钟，最大8小时或更多（取决于项目），默认为10分钟。

- 需要一个值

#### `--interval`，`-i`

时间间隔。 默认为范围的除数。 您可以指定单位：小时(h)、分钟(m)或秒(s)。 至少100万。

- 需要一个值

#### `--to`

结束时间。 默认为现在。

- 需要一个值

#### `--latest`，`-1`

仅显示最新的单个数据点

- 默认： `false`
- 不接受值

#### `--service`，`-s`

按服务或应用程序名称过滤%或*字符可用作通配符。

- 默认： `[]`
- 需要一个值

#### `--type`

按服务类型筛选（如果未提供 — 服务）。 版本不是必需的。 %或*字符可用作通配符。

- 默认： `[]`
- 需要一个值

#### `--project`，`-p`

项目ID或URL

- 需要一个值

#### `--environment`，`-e`

环境ID 使用“。” 以选择项目的默认环境。

- 需要一个值

#### `--format`

输出格式：table、csv、tsv或plain

- 默认： `table`
- 需要一个值

#### `--columns`，`-c`

要显示的列。 可用列：timestamp*、service*、used*、limit*、%*、type （* =默认列）。 字符“+”可用作默认列的占位符。 %或*字符可用作通配符。 值可以用逗号（例如“a，b，c”）和/或空格拆分。

- 默认： `[]`
- 需要一个值

#### `--no-header`

不输出表头

- 默认： `false`
- 不接受值

#### `--date-fmt`

日期格式（作为PHP日期格式字符串）

- 默认： `c`
- 需要一个值


## `metrics:disk-usage`

```bash
magento-cloud magento-cloud disk [-B|--bytes] [-r|--range RANGE] [-i|--interval INTERVAL] [--to TO] [-1|--latest] [-s|--service SERVICE] [--type TYPE] [--tmp] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [--date-fmt DATE-FMT]
```

显示环境的磁盘使用情况

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--bytes`，`-B`

以字节为单位显示大小

- 默认： `false`
- 不接受值

#### `--range`，`-r`

时间范围。 量度将在此持续时间内加载，直到结束时间（ — 到）。 您可以指定单位：小时(h)、分钟(m)或秒(s)。 最小5分钟，最大8小时或更多（取决于项目），默认为10分钟。

- 需要一个值

#### `--interval`，`-i`

时间间隔。 默认为范围的除数。 您可以指定单位：小时(h)、分钟(m)或秒(s)。 至少100万。

- 需要一个值

#### `--to`

结束时间。 默认为现在。

- 需要一个值

#### `--latest`，`-1`

仅显示最新的单个数据点

- 默认： `false`
- 不接受值

#### `--service`，`-s`

按服务或应用程序名称过滤%或*字符可用作通配符。

- 默认： `[]`
- 需要一个值

#### `--type`

按服务类型筛选（如果未提供 — 服务）。 版本不是必需的。 %或*字符可用作通配符。

- 默认： `[]`
- 需要一个值

#### `--tmp`

报告临时磁盘使用情况（显示列：timestamp、service、tmp_used、tmp_limit、tmp_percent、tmp_ipercent）

- 默认： `false`
- 不接受值

#### `--project`，`-p`

项目ID或URL

- 需要一个值

#### `--environment`，`-e`

环境ID 使用“。” 以选择项目的默认环境。

- 需要一个值

#### `--format`

输出格式：table、csv、tsv或plain

- 默认： `table`
- 需要一个值

#### `--columns`，`-c`

要显示的列。 可用列：timestamp*、service*、used*、limit*、%*、ipercent*、tmp_percent*、ilimit、iused、tmp_ilimit、tmp_ipercent、tmp_iused、tmp_limit、tmp_used、type （* =默认列）。 字符“+”可用作默认列的占位符。 %或*字符可用作通配符。 值可以用逗号（例如“a，b，c”）和/或空格拆分。

- 默认： `[]`
- 需要一个值

#### `--no-header`

不输出表头

- 默认： `false`
- 不接受值

#### `--date-fmt`

日期格式（作为PHP日期格式字符串）

- 默认： `c`
- 需要一个值


## `metrics:memory`

```bash
magento-cloud magento-cloud mem [-B|--bytes] [-r|--range RANGE] [-i|--interval INTERVAL] [--to TO] [-1|--latest] [-s|--service SERVICE] [--type TYPE] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [--date-fmt DATE-FMT]
```

Beta显示环境的内存使用情况

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--bytes`，`-B`

以字节为单位显示大小

- 默认： `false`
- 不接受值

#### `--range`，`-r`

时间范围。 量度将在此持续时间内加载，直到结束时间（ — 到）。 您可以指定单位：小时(h)、分钟(m)或秒(s)。 最小5分钟，最大8小时或更多（取决于项目），默认为10分钟。

- 需要一个值

#### `--interval`，`-i`

时间间隔。 默认为范围的除数。 您可以指定单位：小时(h)、分钟(m)或秒(s)。 至少100万。

- 需要一个值

#### `--to`

结束时间。 默认为现在。

- 需要一个值

#### `--latest`，`-1`

仅显示最新的单个数据点

- 默认： `false`
- 不接受值

#### `--service`，`-s`

按服务或应用程序名称过滤%或*字符可用作通配符。

- 默认： `[]`
- 需要一个值

#### `--type`

按服务类型筛选（如果未提供 — 服务）。 版本不是必需的。 %或*字符可用作通配符。

- 默认： `[]`
- 需要一个值

#### `--project`，`-p`

项目ID或URL

- 需要一个值

#### `--environment`，`-e`

环境ID 使用“。” 以选择项目的默认环境。

- 需要一个值

#### `--format`

输出格式：table、csv、tsv或plain

- 默认： `table`
- 需要一个值

#### `--columns`，`-c`

要显示的列。 可用列：timestamp*、service*、used*、limit*、%*、type （* =默认列）。 字符“+”可用作默认列的占位符。 %或*字符可用作通配符。 值可以用逗号（例如“a，b，c”）和/或空格拆分。

- 默认： `[]`
- 需要一个值

#### `--no-header`

不输出表头

- 默认： `false`
- 不接受值

#### `--date-fmt`

日期格式（作为PHP日期格式字符串）

- 默认： `c`
- 需要一个值


## `mount:download`

```bash
magento-cloud magento-cloud mount:download [-a|--all] [-m|--mount MOUNT] [--target TARGET] [--source-path] [--delete] [--exclude EXCLUDE] [--include INCLUDE] [--refresh] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [--worker WORKER] [-I|--instance INSTANCE] [-i|--identity-file IDENTITY-FILE]
```

使用rsync从装载下载文件

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--all`，`-a`

从所有装载下载

- 默认： `false`
- 不接受值

#### `--mount`，`-m`

装载（作为应用程序相对路径）

- 需要一个值

#### `--target`

文件将下载到的目录。 如果 — all被使用，则装载路径将被附加

- 需要一个值

#### `--source-path`

在使用 — all时，使用装载的源路径（而不是装载路径）作为目标的子目录

- 默认： `false`
- 不接受值

#### `--delete`

是否删除目标目录中的无关文件

- 默认： `false`
- 不接受值

#### `--exclude`

要从下载中排除的文件（模式）

- 默认： `[]`
- 需要一个值

#### `--include`

不排除的文件（模式）

- 默认： `[]`
- 需要一个值

#### `--refresh`

是否刷新缓存

- 默认： `false`
- 不接受值

#### `--project`，`-p`

项目ID或URL

- 需要一个值

#### `--environment`，`-e`

环境ID 使用“。” 以选择项目的默认环境。

- 需要一个值

#### `--app`，`-A`

远程应用程序名称

- 需要一个值

#### `--worker`

工作人员姓名

- 需要一个值

#### `--instance`，`-I`

实例ID

- 需要一个值

#### `--identity-file`，`-i`

要使用的SSH身份（私钥）

- 需要一个值


## `mount:list`

```bash
magento-cloud magento-cloud mounts [--paths] [--refresh] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [--worker WORKER] [-I|--instance INSTANCE]
```

获取装载列表

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--paths`

仅输出安装路径（每行一个）

- 默认： `false`
- 不接受值

#### `--refresh`

是否刷新缓存

- 默认： `false`
- 不接受值

#### `--format`

输出格式：table、csv、tsv或plain

- 默认： `table`
- 需要一个值

#### `--columns`，`-c`

要显示的列。 可用列：定义、路径。 %或*字符可用作通配符。 值可以用逗号（例如“a，b，c”）和/或空格拆分。

- 默认： `[]`
- 需要一个值

#### `--no-header`

不输出表头

- 默认： `false`
- 不接受值

#### `--project`，`-p`

项目ID或URL

- 需要一个值

#### `--environment`，`-e`

环境ID 使用“。” 以选择项目的默认环境。

- 需要一个值

#### `--app`，`-A`

远程应用程序名称

- 需要一个值

#### `--worker`

工作人员姓名

- 需要一个值

#### `--instance`，`-I`

实例ID

- 需要一个值


## `mount:size`

```bash
magento-cloud magento-cloud mount:size [-B|--bytes] [--refresh] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-i|--identity-file IDENTITY-FILE] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [--worker WORKER] [-I|--instance INSTANCE]
```

检查装载的磁盘使用情况

```
Use this command to check the disk size and usage for an application's mounts.

Mounts are directories mounted into the application from a persistent, writable
filesystem. They are configured in the mounts key in the application configuration.

The filesystem's total size is determined by the disk key in the same file.
```

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--bytes`，`-B`

以字节为单位显示大小

- 默认： `false`
- 不接受值

#### `--refresh`

刷新缓存

- 默认： `false`
- 不接受值

#### `--format`

输出格式：table、csv、tsv或plain

- 默认： `table`
- 需要一个值

#### `--columns`，`-c`

要显示的列。 可用列： available、max、mounts、percent_used、sizes、used。 %或*字符可用作通配符。 值可以用逗号（例如“a，b，c”）和/或空格拆分。

- 默认： `[]`
- 需要一个值

#### `--no-header`

不输出表头

- 默认： `false`
- 不接受值

#### `--identity-file`，`-i`

要使用的SSH身份（私钥）

- 需要一个值

#### `--project`，`-p`

项目ID或URL

- 需要一个值

#### `--environment`，`-e`

环境ID 使用“。” 以选择项目的默认环境。

- 需要一个值

#### `--app`，`-A`

远程应用程序名称

- 需要一个值

#### `--worker`

工作人员姓名

- 需要一个值

#### `--instance`，`-I`

实例ID

- 需要一个值


## `mount:upload`

```bash
magento-cloud magento-cloud mount:upload [--source SOURCE] [-m|--mount MOUNT] [--delete] [--exclude EXCLUDE] [--include INCLUDE] [--refresh] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [--worker WORKER] [-I|--instance INSTANCE] [-i|--identity-file IDENTITY-FILE]
```

使用rsync将文件上载到装载

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--source`

包含要上载的文件的目录

- 需要一个值

#### `--mount`，`-m`

装载（作为应用程序相对路径）

- 需要一个值

#### `--delete`

是否删除装载中的无关文件

- 默认： `false`
- 不接受值

#### `--exclude`

要从上传中排除的文件（模式）

- 默认： `[]`
- 需要一个值

#### `--include`

不排除的文件（模式）

- 默认： `[]`
- 需要一个值

#### `--refresh`

是否刷新缓存

- 默认： `false`
- 不接受值

#### `--project`，`-p`

项目ID或URL

- 需要一个值

#### `--environment`，`-e`

环境ID 使用“。” 以选择项目的默认环境。

- 需要一个值

#### `--app`，`-A`

远程应用程序名称

- 需要一个值

#### `--worker`

工作人员姓名

- 需要一个值

#### `--instance`，`-I`

实例ID

- 需要一个值

#### `--identity-file`，`-i`

要使用的SSH身份（私钥）

- 需要一个值


## `operation:list`

```bash
magento-cloud magento-cloud ops [--full] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [--worker WORKER] [--format FORMAT] [-c|--columns COLUMNS] [--no-header]
```

Beta List对环境的运行时操作

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--full`

不要限制要显示的命令长度。 默认限制为24行。

- 默认： `false`
- 不接受值

#### `--project`，`-p`

项目ID或URL

- 需要一个值

#### `--environment`，`-e`

环境ID 使用“。” 以选择项目的默认环境。

- 需要一个值

#### `--app`，`-A`

远程应用程序名称

- 需要一个值

#### `--worker`

工作人员姓名

- 需要一个值

#### `--format`

输出格式：table、csv、tsv或plain

- 默认： `table`
- 需要一个值

#### `--columns`，`-c`

要显示的列。 可用列：service*、name*、start*、role、stop （* =缺省列）。 字符“+”可用作默认列的占位符。 %或*字符可用作通配符。 值可以用逗号（例如“a，b，c”）和/或空格拆分。

- 默认： `[]`
- 需要一个值

#### `--no-header`

不输出表头

- 默认： `false`
- 不接受值


## `operation:run`

```bash
magento-cloud magento-cloud operation:run [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [--worker WORKER] [-W|--no-wait] [--wait] [--] [<operation>]
```

Beta对环境运行操作

### 参数

#### `operation`

操作名称

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--project`，`-p`

项目ID或URL

- 需要一个值

#### `--environment`，`-e`

环境ID 使用“。” 以选择项目的默认环境。

- 需要一个值

#### `--app`，`-A`

远程应用程序名称

- 需要一个值

#### `--worker`

工作人员姓名

- 需要一个值

#### `--no-wait`，`-W`

不要等待操作完成

- 默认： `false`
- 不接受值

#### `--wait`

等待操作完成（默认）

- 默认： `false`
- 不接受值


## `project:clear-build-cache`

```bash
magento-cloud magento-cloud project:clear-build-cache [-p|--project PROJECT]
```

清除项目的生成缓存

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--project`，`-p`

项目ID或URL

- 需要一个值


## `project:get`

```bash
magento-cloud magento-cloud get [-e|--environment ENVIRONMENT] [--depth DEPTH] [--build] [-p|--project PROJECT] [-i|--identity-file IDENTITY-FILE] [--] [<project>] [<directory>]
```

在本地克隆项目

### 参数

#### `project`

项目Id


#### `directory`

要克隆到的目录。 默认为项目标题

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--environment`，`-e`

要克隆的环境ID。 默认为项目默认值，或第一个可用的环境

- 需要一个值

#### `--depth`

创建浅层克隆：限制历史记录中的提交次数

- 需要一个值

#### `--build`

克隆后生成项目

- 默认： `false`
- 不接受值

#### `--project`，`-p`

项目ID或URL

- 需要一个值

#### `--identity-file`，`-i`

要使用的SSH身份（私钥）

- 需要一个值


## `project:info`

```bash
magento-cloud magento-cloud project:info [--refresh] [--date-fmt DATE-FMT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-p|--project PROJECT] [-W|--no-wait] [--wait] [--] [<property>] [<value>]
```

读取或设置项目的属性

### 参数

#### `property`

属性的名称


#### `value`

为属性设置新值

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--refresh`

是否刷新缓存

- 默认： `false`
- 不接受值

#### `--date-fmt`

日期格式（作为PHP日期格式字符串）

- 默认： `c`
- 需要一个值

#### `--format`

输出格式：table、csv、tsv或plain

- 默认： `table`
- 需要一个值

#### `--columns`，`-c`

要显示的列。 %或*字符可用作通配符。 值可以用逗号（例如“a，b，c”）和/或空格拆分。

- 默认： `[]`
- 需要一个值

#### `--no-header`

不输出表头

- 默认： `false`
- 不接受值

#### `--project`，`-p`

项目ID或URL

- 需要一个值

#### `--no-wait`，`-W`

不要等待操作完成

- 默认： `false`
- 不接受值

#### `--wait`

等待操作完成（默认）

- 默认： `false`
- 不接受值


## `project:list`

```bash
magento-cloud magento-cloud projects [--pipe] [--region REGION] [--title TITLE] [--my] [--refresh REFRESH] [--sort SORT] [--reverse] [--page PAGE] [-c|--count COUNT] [--format FORMAT] [--columns COLUMNS] [--no-header] [--date-fmt DATE-FMT]
```

获取所有活动项目的列表

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--pipe`

输出项目ID的简单列表。 禁用分页。

- 默认： `false`
- 不接受值

#### `--region`

按区域筛选（完全匹配）

- 需要一个值

#### `--title`

按标题筛选（不区分大小写的搜索）

- 需要一个值

#### `--my`

仅显示您拥有的项目

- 默认： `false`
- 不接受值

#### `--refresh`

是否刷新列表

- 默认： `1`
- 需要一个值

#### `--sort`

排序依据的属性

- 默认： `title`
- 需要一个值

#### `--reverse`

按反向（降序）排序

- 默认： `false`
- 不接受值

#### `--page`

页码。 这支持分页，无论配置还是 — count。 如果指定了 — pipe，则忽略。

- 需要一个值

#### `--count`，`-c`

每页显示的项目数。 使用0可禁用分页。 如果指定了 — page，则忽略。

- 需要一个值

#### `--format`

输出格式：table、csv、tsv或plain

- 默认： `table`
- 需要一个值

#### `--columns`

要显示的列。 可用列：id*、title*、region*、created_at、organization_id、organization_label、organization_name、status （* =默认列）。 字符“+”可用作默认列的占位符。 %或*字符可用作通配符。 值可以用逗号（例如“a，b，c”）和/或空格拆分。

- 默认： `[]`
- 需要一个值

#### `--no-header`

不输出表头

- 默认： `false`
- 不接受值

#### `--date-fmt`

日期格式（作为PHP日期格式字符串）

- 默认： `c`
- 需要一个值


## `project:set-remote`

```bash
magento-cloud magento-cloud set-remote [<project>]
```

为当前Git存储库设置远程项目

### 参数

#### `project`

项目Id

### 选项

有关全局选项，请参阅[全局选项](#global-options)。


## `repo:cat`

```bash
magento-cloud magento-cloud repo:cat [-c|--commit COMMIT] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--] <path>
```

读取项目存储库中的文件

### 参数

#### `path`

文件的路径

- 必填

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--commit`，`-c`

承诺SHA。 这还可以接受父项提交的“HEAD”、插入符号(^)或波状符号(~)后缀。

- 需要一个值

#### `--project`，`-p`

项目ID或URL

- 需要一个值

#### `--environment`，`-e`

环境ID 使用“。” 以选择项目的默认环境。

- 需要一个值


## `repo:ls`

```bash
magento-cloud magento-cloud repo:ls [-d|--directories] [-f|--files] [--git-style] [-c|--commit COMMIT] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--] [<path>]
```

列出项目存储库中的文件

### 参数

#### `path`

子目录的路径

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--directories`，`-d`

仅显示目录

- 默认： `false`
- 不接受值

#### `--files`，`-f`

仅显示文件

- 默认： `false`
- 不接受值

#### `--git-style`

样式输出类似于“git ls-tree”

- 默认： `false`
- 不接受值

#### `--commit`，`-c`

承诺SHA。 这还可以接受父项提交的“HEAD”、插入符号(^)或波状符号(~)后缀。

- 需要一个值

#### `--project`，`-p`

项目ID或URL

- 需要一个值

#### `--environment`，`-e`

环境ID 使用“。” 以选择项目的默认环境。

- 需要一个值


## `repo:read`

```bash
magento-cloud magento-cloud read [-c|--commit COMMIT] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--] [<path>]
```

读取项目存储库中的目录或文件

### 参数

#### `path`

目录或文件的路径

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--commit`，`-c`

承诺SHA。 这还可以接受父项提交的“HEAD”、插入符号(^)或波状符号(~)后缀。

- 需要一个值

#### `--project`，`-p`

项目ID或URL

- 需要一个值

#### `--environment`，`-e`

环境ID 使用“。” 以选择项目的默认环境。

- 需要一个值


## `route:get`

```bash
magento-cloud magento-cloud route:get [--id ID] [-1|--primary] [-P|--property PROPERTY] [--refresh] [--date-fmt DATE-FMT] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [-i|--identity-file IDENTITY-FILE] [--] [<route>]
```

查看有关路由的详细信息

### 参数

#### `route`

路由的原始URL

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--id`

要选择的路由ID

- 需要一个值

#### `--primary`，`-1`

选择主要工艺路线

- 默认： `false`
- 不接受值

#### `--property`，`-P`

要显示的属性

- 需要一个值

#### `--refresh`

绕过路由缓存

- 默认： `false`
- 不接受值

#### `--date-fmt`

日期格式（作为PHP日期格式字符串）

- 默认： `c`
- 需要一个值

#### `--project`，`-p`

项目ID或URL

- 需要一个值

#### `--environment`，`-e`

环境ID 使用“。” 以选择项目的默认环境。

- 需要一个值

#### `--app`，`-A`

[已弃用选项，不再使用]

- 需要一个值

#### `--identity-file`，`-i`

[已弃用选项，不再使用]

- 需要一个值


## `route:list`

```bash
magento-cloud magento-cloud routes [--refresh] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--] [<environment>]
```

列出环境的所有路由

### 参数

#### `environment`

环境Id

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--refresh`

绕过路由缓存

- 默认： `false`
- 不接受值

#### `--format`

输出格式：table、csv、tsv或plain

- 默认： `table`
- 需要一个值

#### `--columns`，`-c`

要显示的列。 可用列： route*、type*、to*、url （* =默认列）。 字符“+”可用作默认列的占位符。 %或*字符可用作通配符。 值可以用逗号（例如“a，b，c”）和/或空格拆分。

- 默认： `[]`
- 需要一个值

#### `--no-header`

不输出表头

- 默认： `false`
- 不接受值

#### `--project`，`-p`

项目ID或URL

- 需要一个值

#### `--environment`，`-e`

环境ID 使用“。” 以选择项目的默认环境。

- 需要一个值


## `self:install`

```bash
magento-cloud magento-cloud self:install [--shell-type SHELL-TYPE]
```

安装或更新命令行界面配置文件

```
This command automatically installs shell configuration for the Magento Cloud CLI,
adding autocompletion support and handy aliases. Bash and ZSH are supported.
```

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--shell-type`

用于自动完成的外壳类型（bash或zsh）

- 需要一个值


## `self:update`

```bash
magento-cloud magento-cloud update [--no-major] [--unstable] [--manifest MANIFEST] [--current-version CURRENT-VERSION] [--timeout TIMEOUT]
```

将CLI更新至最新版本

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--no-major`

仅在次版本或修补程序版本之间更新

- 默认： `false`
- 不接受值

#### `--unstable`

更新到新的不稳定版本（如果可用）

- 默认： `false`
- 不接受值

#### `--manifest`

覆盖清单文件位置

- 需要一个值

#### `--current-version`

覆盖当前版本

- 需要一个值

#### `--timeout`

版本检查超时

- 默认： `30`
- 需要一个值


## `service:list`

```bash
magento-cloud magento-cloud services [--refresh] [--pipe] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header]
```

列出项目中的服务

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--refresh`

是否刷新缓存

- 默认： `false`
- 不接受值

#### `--pipe`

仅输出服务名称列表

- 默认： `false`
- 不接受值

#### `--project`，`-p`

项目ID或URL

- 需要一个值

#### `--environment`，`-e`

环境ID 使用“。” 以选择项目的默认环境。

- 需要一个值

#### `--format`

输出格式：table、csv、tsv或plain

- 默认： `table`
- 需要一个值

#### `--columns`，`-c`

要显示的列。 可用列：磁盘、名称、大小、类型。 %或*字符可用作通配符。 值可以用逗号（例如“a，b，c”）和/或空格拆分。

- 默认： `[]`
- 需要一个值

#### `--no-header`

不输出表头

- 默认： `false`
- 不接受值


## `service:mongo:dump`

```bash
magento-cloud magento-cloud mongodump [-c|--collection COLLECTION] [-z|--gzip] [-o|--stdout] [-r|--relationship RELATIONSHIP] [-i|--identity-file IDENTITY-FILE] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP]
```

从MongoDB创建数据的二进制档案转储

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--collection`，`-c`

要转储的集合

- 需要一个值

#### `--gzip`，`-z`

使用gzip压缩转储

- 默认： `false`
- 不接受值

#### `--stdout`，`-o`

输出到STDOUT而不是文件

- 默认： `false`
- 不接受值

#### `--relationship`，`-r`

要使用的服务关系

- 需要一个值

#### `--identity-file`，`-i`

要使用的SSH身份（私钥）

- 需要一个值

#### `--project`，`-p`

项目ID或URL

- 需要一个值

#### `--environment`，`-e`

环境ID 使用“。” 以选择项目的默认环境。

- 需要一个值

#### `--app`，`-A`

远程应用程序名称

- 需要一个值


## `service:mongo:export`

```bash
magento-cloud magento-cloud mongoexport [-c|--collection COLLECTION] [--jsonArray] [--type TYPE] [-f|--fields FIELDS] [-r|--relationship RELATIONSHIP] [-i|--identity-file IDENTITY-FILE] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP]
```

从MongoDB导出数据

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--collection`，`-c`

要导出的收藏集

- 需要一个值

#### `--jsonArray`

将数据导出为单个JSON数组

- 默认： `false`
- 不接受值

#### `--type`

导出类型，如“csv”

- 需要一个值

#### `--fields`，`-f`

要导出的字段

- 默认： `[]`
- 需要一个值

#### `--relationship`，`-r`

要使用的服务关系

- 需要一个值

#### `--identity-file`，`-i`

要使用的SSH身份（私钥）

- 需要一个值

#### `--project`，`-p`

项目ID或URL

- 需要一个值

#### `--environment`，`-e`

环境ID 使用“。” 以选择项目的默认环境。

- 需要一个值

#### `--app`，`-A`

远程应用程序名称

- 需要一个值


## `service:mongo:restore`

```bash
magento-cloud magento-cloud mongorestore [-c|--collection COLLECTION] [-r|--relationship RELATIONSHIP] [-i|--identity-file IDENTITY-FILE] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP]
```

将数据转储还原到MongoDB中

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--collection`，`-c`

要还原的收藏集

- 需要一个值

#### `--relationship`，`-r`

要使用的服务关系

- 需要一个值

#### `--identity-file`，`-i`

要使用的SSH身份（私钥）

- 需要一个值

#### `--project`，`-p`

项目ID或URL

- 需要一个值

#### `--environment`，`-e`

环境ID 使用“。” 以选择项目的默认环境。

- 需要一个值

#### `--app`，`-A`

远程应用程序名称

- 需要一个值


## `service:mongo:shell`

```bash
magento-cloud magento-cloud mongo [--eval EVAL] [-r|--relationship RELATIONSHIP] [-i|--identity-file IDENTITY-FILE] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP]
```

使用MongoDB shell

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--eval`

将JavaScript片段传递到外壳

- 需要一个值

#### `--relationship`，`-r`

要使用的服务关系

- 需要一个值

#### `--identity-file`，`-i`

要使用的SSH身份（私钥）

- 需要一个值

#### `--project`，`-p`

项目ID或URL

- 需要一个值

#### `--environment`，`-e`

环境ID 使用“。” 以选择项目的默认环境。

- 需要一个值

#### `--app`，`-A`

远程应用程序名称

- 需要一个值


## `service:redis-cli`

```bash
magento-cloud magento-cloud redis [-r|--relationship RELATIONSHIP] [-i|--identity-file IDENTITY-FILE] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [--] [<args>]
```

访问Redis CLI

### 参数

#### `args`

要添加到Redis命令的参数

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--relationship`，`-r`

要使用的服务关系

- 需要一个值

#### `--identity-file`，`-i`

要使用的SSH身份（私钥）

- 需要一个值

#### `--project`，`-p`

项目ID或URL

- 需要一个值

#### `--environment`，`-e`

环境ID 使用“。” 以选择项目的默认环境。

- 需要一个值

#### `--app`，`-A`

远程应用程序名称

- 需要一个值


## `snapshot:create`

```bash
magento-cloud magento-cloud backup [--live] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] [<environment>]
```

制作环境的快照

### 参数

#### `environment`

环境

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--live`

实时备份：不要停止环境。 如果设置，这将使环境保持运行状态并在备份期间打开连接。 这减少了停机时间，但存在以不一致状态备份数据的风险。

- 默认： `false`
- 不接受值

#### `--project`，`-p`

项目ID或URL

- 需要一个值

#### `--environment`，`-e`

环境ID 使用“。” 以选择项目的默认环境。

- 需要一个值

#### `--no-wait`，`-W`

不要等待操作完成

- 默认： `false`
- 不接受值

#### `--wait`

等待操作完成（默认）

- 默认： `false`
- 不接受值


## `snapshot:delete`

```bash
magento-cloud magento-cloud snapshot:delete [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] [<id>]
```

删除环境快照

### 参数

#### `id`

快照的ID。 在非交互模式下必需。

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--project`，`-p`

项目ID或URL

- 需要一个值

#### `--environment`，`-e`

环境ID 使用“。” 以选择项目的默认环境。

- 需要一个值

#### `--no-wait`，`-W`

不要等待操作完成

- 默认： `false`
- 不接受值

#### `--wait`

等待操作完成（默认）

- 默认： `false`
- 不接受值


## `snapshot:get`

```bash
magento-cloud magento-cloud snapshot:get [-P|--property PROPERTY] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--date-fmt DATE-FMT] [--] [<id>]
```

查看环境快照

### 参数

#### `id`

快照的ID。 默认为最近的一个。

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--property`，`-P`

要显示的属性。

- 需要一个值

#### `--project`，`-p`

项目ID或URL

- 需要一个值

#### `--environment`，`-e`

环境ID 使用“。” 以选择项目的默认环境。

- 需要一个值

#### `--date-fmt`

日期格式（作为PHP日期格式字符串）

- 默认： `c`
- 需要一个值


## `snapshot:list`

```bash
magento-cloud magento-cloud snapshots [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [--date-fmt DATE-FMT] [-p|--project PROJECT] [-e|--environment ENVIRONMENT]
```

列出环境的可用快照

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--format`

输出格式：table、csv、tsv或plain

- 默认： `table`
- 需要一个值

#### `--columns`，`-c`

要显示的列。 %或*字符可用作通配符。 值可以用逗号（例如“a，b，c”）和/或空格拆分。

- 默认： `[]`
- 需要一个值

#### `--no-header`

不输出表头

- 默认： `false`
- 不接受值

#### `--date-fmt`

日期格式（作为PHP日期格式字符串）

- 默认： `c`
- 需要一个值

#### `--project`，`-p`

项目ID或URL

- 需要一个值

#### `--environment`，`-e`

环境ID 使用“。” 以选择项目的默认环境。

- 需要一个值


## `snapshot:restore`

```bash
magento-cloud magento-cloud snapshot:restore [--target TARGET] [--branch-from BRANCH-FROM] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] [<snapshot>]
```

恢复环境快照

### 参数

#### `snapshot`

快照的名称。 默认为最近的

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--target`

要还原到的环境。 默认为快照的当前环境

- 需要一个值

#### `--branch-from`

如果 — target尚不存在，这会指定新环境的父项

- 需要一个值

#### `--project`，`-p`

项目ID或URL

- 需要一个值

#### `--environment`，`-e`

环境ID 使用“。” 以选择项目的默认环境。

- 需要一个值

#### `--no-wait`，`-W`

不要等待操作完成

- 默认： `false`
- 不接受值

#### `--wait`

等待操作完成（默认）

- 默认： `false`
- 不接受值


## `source-operation:list`

```bash
magento-cloud magento-cloud source-ops [--full] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header]
```

列出环境上的源操作

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--full`

不要限制要显示的命令长度。 默认限制为24行。

- 默认： `false`
- 不接受值

#### `--project`，`-p`

项目ID或URL

- 需要一个值

#### `--environment`，`-e`

环境ID 使用“。” 以选择项目的默认环境。

- 需要一个值

#### `--format`

输出格式：table、csv、tsv或plain

- 默认： `table`
- 需要一个值

#### `--columns`，`-c`

要显示的列。 可用列：应用程序、命令、操作。 %或*字符可用作通配符。 值可以用逗号（例如“a，b，c”）和/或空格拆分。

- 默认： `[]`
- 需要一个值

#### `--no-header`

不输出表头

- 默认： `false`
- 不接受值


## `source-operation:run`

```bash
magento-cloud magento-cloud source-operation:run [--variable VARIABLE] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] [<operation>]
```

运行源操作

### 参数

#### `operation`

操作名称

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--variable`

在操作期间要设置的变量，格式为type：name=value

- 默认： `[]`
- 需要一个值

#### `--project`，`-p`

项目ID或URL

- 需要一个值

#### `--environment`，`-e`

环境ID 使用“。” 以选择项目的默认环境。

- 需要一个值

#### `--no-wait`，`-W`

不要等待操作完成

- 默认： `false`
- 不接受值

#### `--wait`

等待操作完成（默认）

- 默认： `false`
- 不接受值


## `ssh-cert:load`

```bash
magento-cloud magento-cloud ssh-cert:load [--refresh-only] [--new] [--new-key]
```

生成SSH证书

```
This command checks if a valid SSH certificate is present, and generates a
new one if necessary.

Certificates allow you to make SSH connections without having previously
uploaded a public key. They are more secure than keys and they allow for
other features.

Normally the certificate is loaded automatically during login, or when
making an SSH connection. So this command is seldom needed.

If you want to set up certificates without login and without an SSH-related
command, for example if you are writing a script that uses an API token via
an environment variable, then you would probably want to run this command
explicitly. For unattended scripts, remember to turn off interaction via
--yes or the MAGENTO_CLOUD_CLI_NO_INTERACTION environment variable.
```

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--refresh-only`

仅在必要时刷新证书（不写入SSH配置）

- 默认： `false`
- 不接受值

#### `--new`

强制刷新证书

- 默认： `false`
- 不接受值

#### `--new-key`

[已弃用]请改用 — new

- 默认： `false`
- 不接受值


## `ssh-key:add`

```bash
magento-cloud magento-cloud ssh-key:add [--name NAME] [--] [<path>]
```

添加新的SSH密钥

```
This command lets you add an SSH key to your account. It can generate a key using OpenSSH.

Notice:
SSH keys are no longer needed by default, as SSH certificates are supported.
Certificates offer more security than keys.

To load or check your SSH certificate, run: magento-cloud ssh-cert:load
```

### 参数

#### `path`

现有SSH公钥的路径

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--name`

用于标识密钥的名称

- 需要一个值


## `ssh-key:delete`

```bash
magento-cloud magento-cloud ssh-key:delete [<id>]
```

删除SSH密钥

```
This command lets you delete SSH keys from your account.

Notice:
SSH keys are no longer needed by default, as SSH certificates are supported.
Certificates offer more security than keys.

To load or check your SSH certificate, run: magento-cloud ssh-cert:load
```

### 参数

#### `id`

要删除的SSH密钥的ID

### 选项

有关全局选项，请参阅[全局选项](#global-options)。


## `ssh-key:list`

```bash
magento-cloud magento-cloud ssh-keys [--format FORMAT] [-c|--columns COLUMNS] [--no-header]
```

获取帐户中的SSH密钥列表

```
This command lets you list SSH keys in your account.

Notice:
SSH keys are no longer needed by default, as SSH certificates are supported.
Certificates offer more security than keys.

To load or check your SSH certificate, run: magento-cloud ssh-cert:load
```

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--format`

输出格式：table、csv、tsv或plain

- 默认： `table`
- 需要一个值

#### `--columns`，`-c`

要显示的列。 可用列：id*、title*、path*、指纹（* =默认列）。 字符“+”可用作默认列的占位符。 %或*字符可用作通配符。 值可以用逗号（例如“a，b，c”）和/或空格拆分。

- 默认： `[]`
- 需要一个值

#### `--no-header`

不输出表头

- 默认： `false`
- 不接受值


## `subscription:info`

```bash
magento-cloud magento-cloud subscription:info [-s|--id ID] [--date-fmt DATE-FMT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-p|--project PROJECT] [--] [<property>] [<value>]
```

读取或修改订阅属性

### 参数

#### `property`

属性的名称


#### `value`

为属性设置新值

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--id`，`-s`

订阅ID

- 需要一个值

#### `--date-fmt`

日期格式（作为PHP日期格式字符串）

- 默认： `c`
- 需要一个值

#### `--format`

输出格式：table、csv、tsv或plain

- 默认： `table`
- 需要一个值

#### `--columns`，`-c`

要显示的列。 %或*字符可用作通配符。 值可以用逗号（例如“a，b，c”）和/或空格拆分。

- 默认： `[]`
- 需要一个值

#### `--no-header`

不输出表头

- 默认： `false`
- 不接受值

#### `--project`，`-p`

项目ID或URL

- 需要一个值


## `tunnel:close`

```bash
magento-cloud magento-cloud tunnel:close [-a|--all] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP]
```

关闭SSH隧道

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--all`，`-a`

关闭所有隧道

- 默认： `false`
- 不接受值

#### `--project`，`-p`

项目ID或URL

- 需要一个值

#### `--environment`，`-e`

环境ID 使用“。” 以选择项目的默认环境。

- 需要一个值

#### `--app`，`-A`

远程应用程序名称

- 需要一个值


## `tunnel:info`

```bash
magento-cloud magento-cloud tunnel:info [-P|--property PROPERTY] [-c|--encode] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP]
```

查看SSH隧道的关系信息

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--property`，`-P`

要查看的关系属性

- 需要一个值

#### `--encode`，`-c`

以base64编码的JSON输出

- 默认： `false`
- 不接受值

#### `--project`，`-p`

项目ID或URL

- 需要一个值

#### `--environment`，`-e`

环境ID 使用“。” 以选择项目的默认环境。

- 需要一个值

#### `--app`，`-A`

远程应用程序名称

- 需要一个值


## `tunnel:list`

```bash
magento-cloud magento-cloud tunnels [-a|--all] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [--format FORMAT] [-c|--columns COLUMNS] [--no-header]
```

列出SSH隧道

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--all`，`-a`

查看所有隧道

- 默认： `false`
- 不接受值

#### `--project`，`-p`

项目ID或URL

- 需要一个值

#### `--environment`，`-e`

环境ID 使用“。” 以选择项目的默认环境。

- 需要一个值

#### `--app`，`-A`

远程应用程序名称

- 需要一个值

#### `--format`

输出格式：table、csv、tsv或plain

- 默认： `table`
- 需要一个值

#### `--columns`，`-c`

要显示的列。 可用列：port*、project*、environment*、app*、relationship*、url （* =默认列）。 字符“+”可用作默认列的占位符。 %或*字符可用作通配符。 值可以用逗号（例如“a，b，c”）和/或空格拆分。

- 默认： `[]`
- 需要一个值

#### `--no-header`

不输出表头

- 默认： `false`
- 不接受值


## `tunnel:open`

```bash
magento-cloud magento-cloud tunnel:open [-g|--gateway-ports] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [-i|--identity-file IDENTITY-FILE]
```

打开SSH隧道以连接到应用程序的关系

```
This command opens SSH tunnels to all of the relationships of an application.

Connections can then be made to the application's services as if they were
local, for example a local MySQL client can be used, or the Solr web
administration endpoint can be accessed through a local browser.

This command requires the posix and pcntl PHP extensions (as multiple
background CLI processes are created to keep the SSH tunnels open). The
tunnel:single command can be used on systems without these
extensions.
```

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--gateway-ports`，`-g`

允许远程主机连接到本地转发端口

- 默认： `false`
- 不接受值

#### `--project`，`-p`

项目ID或URL

- 需要一个值

#### `--environment`，`-e`

环境ID 使用“。” 以选择项目的默认环境。

- 需要一个值

#### `--app`，`-A`

远程应用程序名称

- 需要一个值

#### `--identity-file`，`-i`

要使用的SSH身份（私钥）

- 需要一个值


## `tunnel:single`

```bash
magento-cloud magento-cloud tunnel:single [--port PORT] [-g|--gateway-ports] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [-r|--relationship RELATIONSHIP] [-i|--identity-file IDENTITY-FILE]
```

打开到应用程序关系的单个SSH通道

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--port`

本地端口

- 需要一个值

#### `--gateway-ports`，`-g`

允许远程主机连接到本地转发端口

- 默认： `false`
- 不接受值

#### `--project`，`-p`

项目ID或URL

- 需要一个值

#### `--environment`，`-e`

环境ID 使用“。” 以选择项目的默认环境。

- 需要一个值

#### `--app`，`-A`

远程应用程序名称

- 需要一个值

#### `--relationship`，`-r`

要使用的服务关系

- 需要一个值

#### `--identity-file`，`-i`

要使用的SSH身份（私钥）

- 需要一个值


## `user:add`

```bash
magento-cloud magento-cloud user:add [-r|--role ROLE] [--force-invite] [-p|--project PROJECT] [-W|--no-wait] [--wait] [--] [<email>]
```

在项目中添加用户

### 参数

#### `email`

用户的电子邮件地址

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--role`，`-r`

用户的项目角色（“管理员”或“查看器”）或环境类型角色（例如“staging：contributor”或“production：viewer”）。 要从环境类型中删除用户，请将角色设置为“无”。 %或*字符可用作环境类型的通配符，例如“%：viewer”，以在所有类型上为用户赋予“viewer”角色。 角色可缩写，如“production：v”。

- 默认： `[]`
- 需要一个值

#### `--force-invite`

发送邀请，即使已发送邀请

- 默认： `false`
- 不接受值

#### `--project`，`-p`

项目ID或URL

- 需要一个值

#### `--no-wait`，`-W`

不要等待操作完成

- 默认： `false`
- 不接受值

#### `--wait`

等待操作完成（默认）

- 默认： `false`
- 不接受值


## `user:delete`

```bash
magento-cloud magento-cloud user:delete [-p|--project PROJECT] [-W|--no-wait] [--wait] [--] <email>
```

从项目中删除用户

### 参数

#### `email`

用户的电子邮件地址

- 必填

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--project`，`-p`

项目ID或URL

- 需要一个值

#### `--no-wait`，`-W`

不要等待操作完成

- 默认： `false`
- 不接受值

#### `--wait`

等待操作完成（默认）

- 默认： `false`
- 不接受值


## `user:get`

```bash
magento-cloud magento-cloud user:get [-l|--level LEVEL] [--pipe] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [-r|--role ROLE] [--] [<email>]
```

查看用户的角色

### 参数

#### `email`

用户的电子邮件地址

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--level`，`-l`

角色级别（“项目”或“环境”）

- 需要一个值

#### `--pipe`

将角色输出到stdout（进行任何更改后）

- 默认： `false`
- 不接受值

#### `--project`，`-p`

项目ID或URL

- 需要一个值

#### `--environment`，`-e`

环境ID 使用“。” 以选择项目的默认环境。

- 需要一个值

#### `--no-wait`，`-W`

不要等待操作完成

- 默认： `false`
- 不接受值

#### `--wait`

等待操作完成（默认）

- 默认： `false`
- 不接受值

#### `--role`，`-r`

[已弃用：使用user：update更改用户的角色]

- 需要一个值


## `user:list`

```bash
magento-cloud magento-cloud users [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-p|--project PROJECT]
```

列出项目用户

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--format`

输出格式：table、csv、tsv或plain

- 默认： `table`
- 需要一个值

#### `--columns`，`-c`

要显示的列。 可用列：email*、name*、role*、id*、granted_at、updated_at （* =默认列）。 字符“+”可用作默认列的占位符。 %或*字符可用作通配符。 值可以用逗号（例如“a，b，c”）和/或空格拆分。

- 默认： `[]`
- 需要一个值

#### `--no-header`

不输出表头

- 默认： `false`
- 不接受值

#### `--project`，`-p`

项目ID或URL

- 需要一个值


## `user:update`

```bash
magento-cloud magento-cloud user:update [-r|--role ROLE] [-p|--project PROJECT] [-W|--no-wait] [--wait] [--] [<email>]
```

更新项目中的用户角色

### 参数

#### `email`

用户的电子邮件地址

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--role`，`-r`

用户的项目角色（“管理员”或“查看器”）或环境类型角色（例如“staging：contributor”或“production：viewer”）。 要从环境类型中删除用户，请将角色设置为“无”。 %或*字符可用作环境类型的通配符，例如“%：viewer”，以在所有类型上为用户赋予“viewer”角色。 角色可缩写，如“production：v”。

- 默认： `[]`
- 需要一个值

#### `--project`，`-p`

项目ID或URL

- 需要一个值

#### `--no-wait`，`-W`

不要等待操作完成

- 默认： `false`
- 不接受值

#### `--wait`

等待操作完成（默认）

- 默认： `false`
- 不接受值


## `variable:create`

```bash
magento-cloud magento-cloud variable:create [-u|--update] [-l|--level LEVEL] [--name NAME] [--value VALUE] [--json JSON] [--sensitive SENSITIVE] [--prefix PREFIX] [--enabled ENABLED] [--inheritable INHERITABLE] [--visible-build VISIBLE-BUILD] [--visible-runtime VISIBLE-RUNTIME] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] [<name>]
```

创建变量

### 参数

#### `name`

变量名称

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--update`，`-u`

更新变量（如果该变量已存在）

- 默认： `false`
- 不接受值

#### `--level`，`-l`

设置变量的级别（“项目”或“环境”）

- 需要一个值

#### `--name`

变量名称

- 需要一个值

#### `--value`

变量的值

- 需要一个值

#### `--json`

变量值是否为JSON格式

- 默认： `false`
- 需要一个值

#### `--sensitive`

变量值是否敏感

- 默认： `false`
- 需要一个值

#### `--prefix`

可确定其类型的变量名称的前缀，如“env”。 仅当名称尚未包含前缀时适用。 （例如“none”或“env：”）

- 默认： `none`
- 需要一个值

#### `--enabled`

是否应在环境中启用变量

- 默认： `true`
- 需要一个值

#### `--inheritable`

变量是否可由子环境继承

- 默认： `true`
- 需要一个值

#### `--visible-build`

变量在构建时是否应可见

- 需要一个值

#### `--visible-runtime`

变量是否应在运行时可见

- 默认： `true`
- 需要一个值

#### `--project`，`-p`

项目ID或URL

- 需要一个值

#### `--environment`，`-e`

环境ID 使用“。” 以选择项目的默认环境。

- 需要一个值

#### `--no-wait`，`-W`

不要等待操作完成

- 默认： `false`
- 不接受值

#### `--wait`

等待操作完成（默认）

- 默认： `false`
- 不接受值


## `variable:delete`

```bash
magento-cloud magento-cloud variable:delete [-l|--level LEVEL] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] <name>
```

删除变量

### 参数

#### `name`

变量名称

- 必填

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--level`，`-l`

变量级别（“项目”、“环境”、“p”或“e”）

- 需要一个值

#### `--project`，`-p`

项目ID或URL

- 需要一个值

#### `--environment`，`-e`

环境ID 使用“。” 以选择项目的默认环境。

- 需要一个值

#### `--no-wait`，`-W`

不要等待操作完成

- 默认： `false`
- 不接受值

#### `--wait`

等待操作完成（默认）

- 默认： `false`
- 不接受值


## `variable:get`

```bash
magento-cloud magento-cloud vget [-P|--property PROPERTY] [-l|--level LEVEL] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--pipe] [--] [<name>]
```

查看变量

### 参数

#### `name`

变量的名称

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--property`，`-P`

查看单个变量属性

- 需要一个值

#### `--level`，`-l`

变量级别（“项目”、“环境”、“p”或“e”）

- 需要一个值

#### `--format`

输出格式：table、csv、tsv或plain

- 默认： `table`
- 需要一个值

#### `--columns`，`-c`

要显示的列。 %或*字符可用作通配符。 值可以用逗号（例如“a，b，c”）和/或空格拆分。

- 默认： `[]`
- 需要一个值

#### `--no-header`

不输出表头

- 默认： `false`
- 不接受值

#### `--project`，`-p`

项目ID或URL

- 需要一个值

#### `--environment`，`-e`

环境ID 使用“。” 以选择项目的默认环境。

- 需要一个值

#### `--pipe`

[已弃用的选项]仅输出变量值

- 默认： `false`
- 不接受值


## `variable:list`

```bash
magento-cloud magento-cloud variables [-l|--level LEVEL] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-p|--project PROJECT] [-e|--environment ENVIRONMENT]
```

列表变量

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--level`，`-l`

变量级别（“项目”、“环境”、“p”或“e”）

- 需要一个值

#### `--format`

输出格式：table、csv、tsv或plain

- 默认： `table`
- 需要一个值

#### `--columns`，`-c`

要显示的列。 可用列：is_enabled、level、name、value。 %或*字符可用作通配符。 值可以用逗号（例如“a，b，c”）和/或空格拆分。

- 默认： `[]`
- 需要一个值

#### `--no-header`

不输出表头

- 默认： `false`
- 不接受值

#### `--project`，`-p`

项目ID或URL

- 需要一个值

#### `--environment`，`-e`

环境ID 使用“。” 以选择项目的默认环境。

- 需要一个值


## `variable:update`

```bash
magento-cloud magento-cloud variable:update [--allow-no-change] [-l|--level LEVEL] [--value VALUE] [--json JSON] [--sensitive SENSITIVE] [--enabled ENABLED] [--inheritable INHERITABLE] [--visible-build VISIBLE-BUILD] [--visible-runtime VISIBLE-RUNTIME] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] <name>
```

更新变量

### 参数

#### `name`

变量名称

- 必填

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--allow-no-change`

如果未提供任何更改，则返回成功（零退出代码）

- 默认： `false`
- 不接受值

#### `--level`，`-l`

变量级别（“项目”、“环境”、“p”或“e”）

- 需要一个值

#### `--value`

变量的值

- 需要一个值

#### `--json`

变量值是否为JSON格式

- 默认： `false`
- 需要一个值

#### `--sensitive`

变量值是否敏感

- 默认： `false`
- 需要一个值

#### `--enabled`

是否应在环境中启用变量

- 默认： `true`
- 需要一个值

#### `--inheritable`

变量是否可由子环境继承

- 默认： `true`
- 需要一个值

#### `--visible-build`

变量在构建时是否应可见

- 需要一个值

#### `--visible-runtime`

变量是否应在运行时可见

- 默认： `true`
- 需要一个值

#### `--project`，`-p`

项目ID或URL

- 需要一个值

#### `--environment`，`-e`

环境ID 使用“。” 以选择项目的默认环境。

- 需要一个值

#### `--no-wait`，`-W`

不要等待操作完成

- 默认： `false`
- 不接受值

#### `--wait`

等待操作完成（默认）

- 默认： `false`
- 不接受值


## `worker:list`

```bash
magento-cloud magento-cloud workers [--refresh] [--pipe] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header]
```

获取所有已部署工作人员的列表

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--refresh`

是否刷新缓存

- 默认： `false`
- 不接受值

#### `--pipe`

仅输出辅助进程名称列表

- 默认： `false`
- 不接受值

#### `--project`，`-p`

项目ID或URL

- 需要一个值

#### `--environment`，`-e`

环境ID 使用“。” 以选择项目的默认环境。

- 需要一个值

#### `--format`

输出格式：table、csv、tsv或plain

- 默认： `table`
- 需要一个值

#### `--columns`，`-c`

要显示的列。 可用列：命令、名称、类型。 %或*字符可用作通配符。 值可以用逗号（例如“a，b，c”）和/或空格拆分。

- 默认： `[]`
- 需要一个值

#### `--no-header`

不输出表头

- 默认： `false`
- 不接受值
