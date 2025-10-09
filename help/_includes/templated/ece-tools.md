---
source-git-commit: 11d0e86ad4633d9c4232474b9921ed491f285969
workflow-type: tm+mt
source-wordcount: '934'
ht-degree: 3%

---
# ece-tools

**版本**： 2002.2.8

此引用包含通过`ece-tools`命令行工具提供的34个命令。
在云基础架构上的Adobe Commerce中使用`ece-tools list`命令自动生成初始列表。

## 常规

此引用从应用程序代码库生成。 要更改内容，请&#x200B;_向我们提供反馈_ （在右上角查找链接）。 有关贡献准则，请参阅[代码贡献](https://developer.adobe.com/commerce/contributor/guides/code-contributions/)。

### 全局选项

#### `--help`，`-h`

显示给定命令的帮助。 未给出任何命令时，显示list命令的帮助

- 默认： `false`
- 不接受值

#### `--silent`

不输出任何消息

- 默认： `false`
- 不接受值

#### `--quiet`，`-q`

仅显示错误。 所有其他输出都将被抑制

- 默认： `false`
- 不接受值

#### `--verbose`，`-v|-vv|-vvv`

增加消息的详细程度：1表示正常输出，2表示更多详细输出，3表示调试

- 默认： `false`
- 不接受值

#### `--version`，`-V`

显示此应用程序版本

- 默认： `false`
- 不接受值

#### `--ansi`

强制（或禁用 — no-ansi） ANSI输出

- 不接受值

#### `--no-ansi`

否定“ — ansi”选项

- 不接受值

#### `--no-interaction`，`-n`

不要问任何交互式问题

- 默认： `false`
- 不接受值


## `_complete`

```bash
ece-tools _complete [-s|--shell SHELL] [-i|--input INPUT] [-c|--current CURRENT] [-a|--api-version API-VERSION] [-S|--symfony SYMFONY]
```

用于提供外壳完成建议的内部命令

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--shell`，`-s`

壳类型(“bash”、“fish”、“zsh”)

- 需要一个值

#### `--input`，`-i`

输入令牌的数组（例如COMP_WORDS或argv）

- 默认： `[]`
- 需要一个值

#### `--current`，`-c`

光标所在的“输入”数组的索引（例如COMP_CWORD）

- 需要一个值

#### `--api-version`，`-a`

完成脚本的API版本

- 需要一个值

#### `--symfony`，`-S`

已弃用

- 需要一个值


## `build`

```bash
ece-tools build
```

生成应用程序。

### 选项

有关全局选项，请参阅[全局选项](#global-options)。


## `completion`

```bash
ece-tools completion [--debug] [--] [<shell>]
```

转储shell完成脚本

```
The completion command dumps the shell completion script required
to use shell autocompletion (currently, bash, fish, zsh completion are supported).

Static installation
-------------------

Dump the script to a global completion file and restart your shell:

    bin/ece-tools completion  | sudo tee /etc/bash_completion.d/ece-tools

Or dump the script to a local file and source it:

    bin/ece-tools completion  > completion.sh

    # source the file whenever you use the project
    source completion.sh

    # or add this line at the end of your "~/.bashrc" file:
    source /path/to/completion.sh

Dynamic installation
--------------------

Add this to the end of your shell configuration file (e.g. "~/.bashrc"):

    eval "$(/var/jenkins/workspace/gendocs-ece-tools-cli/bin/ece-tools completion )"
```

### 参数

#### `shell`

如果未提供shell类型（例如“bash”），则将使用“$SHELL”环境变量的值

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--debug`

跟踪完成调试日志

- 默认： `false`
- 不接受值


## `db-dump`

```bash
ece-tools db-dump [-d|--remove-definers] [-a|--dump-directory DUMP-DIRECTORY] [--] [<databases>...]
```

创建数据库备份。

### 参数

#### `databases`

用于备份的数据库。 可用值： [主报价销售]。 如果未指定参数值，则将使用存储在`MAGENTO_CLOUD_RELATIONSHIP`环境变量或/和.magento.env.yaml配置文件的`stage.deploy.DATABASE_CONFIGURATION`属性中的凭据创建数据库备份。

- 默认： `[]`
- 数组

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--remove-definers`，`-d`

从数据库转储中删除定义符

- 默认： `false`
- 不接受值

#### `--dump-directory`，`-a`

使用替代目录保存转储

- 需要一个值


## `deploy`

```bash
ece-tools deploy
```

部署应用程序。

### 选项

有关全局选项，请参阅[全局选项](#global-options)。


## `help`

```bash
ece-tools help [--format FORMAT] [--raw] [--] [<command_name>]
```

显示命令的帮助

```
The help command displays help for a given command:

  bin/ece-tools help list

You can also output the help in other formats by using the --format option:

  bin/ece-tools help --format=xml list

To display the list of available commands, please use the list command.
```

### 参数

#### `command_name`

命令名称

- 默认： `help`

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--format`

输出格式（txt、xml、json或md）

- 默认： `txt`
- 需要一个值

#### `--raw`

输出原始命令帮助

- 默认： `false`
- 不接受值


## `list`

```bash
ece-tools list [--raw] [--format FORMAT] [--short] [--] [<namespace>]
```

列出命令

```
The list command lists all commands:

  bin/ece-tools list

You can also display the commands for a specific namespace:

  bin/ece-tools list test

You can also output the information in other formats by using the --format option:

  bin/ece-tools list --format=xml

It's also possible to get raw list of commands (useful for embedding command runner):

  bin/ece-tools list --raw
```

### 参数

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

#### `--short`

跳过描述命令的参数

- 默认： `false`
- 不接受值


## `patch`

```bash
ece-tools patch
```

应用自定义修补程序。

### 选项

有关全局选项，请参阅[全局选项](#global-options)。


## `post-deploy`

```bash
ece-tools post-deploy
```

在部署操作后执行。

### 选项

有关全局选项，请参阅[全局选项](#global-options)。


## `run`

```bash
ece-tools run <scenario>...
```

执行方案。

### 参数

#### `scenario`

方案

- 默认： `[]`
- 必填

- 数组

### 选项

有关全局选项，请参阅[全局选项](#global-options)。


## `backup:list`

```bash
ece-tools backup:list
```

显示备份文件的列表。

### 选项

有关全局选项，请参阅[全局选项](#global-options)。


## `backup:restore`

```bash
ece-tools backup:restore [-f|--force] [--file [FILE]]
```

还原重要的配置文件。 运行备份:list以显示备份文件列表。

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--force`，`-f`

在还原备份期间覆盖现有文件

- 默认： `false`
- 不接受值

#### `--file`

特定文件恢复路径

- 接受值


## `build:generate`

```bash
ece-tools build:generate
```

为构建阶段生成所有必需的文件。

### 选项

有关全局选项，请参阅[全局选项](#global-options)。


## `build:transfer`

```bash
ece-tools build:transfer
```

将生成的文件传输到init目录中。

### 选项

有关全局选项，请参阅[全局选项](#global-options)。


## `cloud:config:create`

```bash
ece-tools cloud:config:create <configuration>
```

使用指定的生成、部署和部署后变量配置创建`.magento.env.yaml`文件。 覆盖任何现有的`.magento.env.yaml`文件。

### 参数

#### `configuration`

JSON格式的配置

- 必填

### 选项

有关全局选项，请参阅[全局选项](#global-options)。


## `cloud:config:update`

```bash
ece-tools cloud:config:update <configuration>
```

使用指定的配置更新现有`.magento.env.yaml`文件。 创建`.magento.env.yaml`文件（如果该文件不存在）。

### 参数

#### `configuration`

JSON格式的配置

- 必填

### 选项

有关全局选项，请参阅[全局选项](#global-options)。


## `cloud:config:validate`

```bash
ece-tools cloud:config:validate
```

验证`.magento.env.yaml`配置文件

### 选项

有关全局选项，请参阅[全局选项](#global-options)。


## `config:dump`

```bash
ece-tools config:dumpdump
```

用于静态内容部署的转储配置。

### 选项

有关全局选项，请参阅[全局选项](#global-options)。


## `cron:disable`

```bash
ece-tools cron:disable
```

禁用所有Magento cron进程并终止所有正在运行的进程。

### 选项

有关全局选项，请参阅[全局选项](#global-options)。


## `cron:enable`

```bash
ece-tools cron:enable
```

启用Magento cron流程。

### 选项

有关全局选项，请参阅[全局选项](#global-options)。


## `cron:kill`

```bash
ece-tools cron:kill
```

终止所有Magento cron流程。

### 选项

有关全局选项，请参阅[全局选项](#global-options)。


## `cron:unlock`

```bash
ece-tools cron:unlock [--job-code [JOB-CODE]]
```

解锁卡在“正在运行”状态的cron作业。

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--job-code`

要解锁的Cron作业代码。

- 默认： `[]`
- 接受多个值


## `dev:generate:schema-error`

```bash
ece-tools dev:generate:schema-error
```

从schema.error.yaml文件生成dist/error-codes.md文件。

### 选项

有关全局选项，请参阅[全局选项](#global-options)。


## `dev:git:update-composer`

```bash
ece-tools dev:git:update-composer
```

从Git更新部署的编辑器。

### 选项

有关全局选项，请参阅[全局选项](#global-options)。


## `env:config:show`

```bash
ece-tools env:config:show [<variable>...]
```

显示编码的云配置环境变量。

### 参数

#### `variable`

要显示的环境变量，可能的选项：服务、路由、变量

- 默认： `[]`
- 数组

### 选项

有关全局选项，请参阅[全局选项](#global-options)。


## `error:show`

```bash
ece-tools error:show [-j|--json] [--] [<error-code>]
```

按错误ID显示有关错误的信息或显示有关上次部署以来所有错误的信息。

### 参数

#### `error-code`

错误代码（如果未传递）命令显示有关上次部署的所有错误的信息

### 选项

有关全局选项，请参阅[全局选项](#global-options)。

#### `--json`，`-j`

用于以JSON格式获取结果

- 默认： `false`
- 不接受值


## `module:refresh`

```bash
ece-tools module:refresh
```

刷新配置以启用新添加的模块。

### 选项

有关全局选项，请参阅[全局选项](#global-options)。


## `schema:generate`

```bash
ece-tools schema:generate
```

生成架构*.dist文件。

### 选项

有关全局选项，请参阅[全局选项](#global-options)。


## `wizard:ideal-state`

```bash
ece-tools wizard:ideal-state
```

验证配置的理想状态。

### 选项

有关全局选项，请参阅[全局选项](#global-options)。


## `wizard:master-slave`

```bash
ece-tools wizard:master-slave
```

验证主从配置。

### 选项

有关全局选项，请参阅[全局选项](#global-options)。


## `wizard:scd-on-build`

```bash
ece-tools wizard:scd-on-build
```

验证生成配置的SCD。

### 选项

有关全局选项，请参阅[全局选项](#global-options)。


## `wizard:scd-on-demand`

```bash
ece-tools wizard:scd-on-demand
```

验证SCD on demand配置。

### 选项

有关全局选项，请参阅[全局选项](#global-options)。


## `wizard:scd-on-deploy`

```bash
ece-tools wizard:scd-on-deploy
```

在部署配置时验证SCD。

### 选项

有关全局选项，请参阅[全局选项](#global-options)。


## `wizard:split-db-state`

```bash
ece-tools wizard:split-db-state
```

验证剥离数据库的能力，以及数据库是否已剥离。

### 选项

有关全局选项，请参阅[全局选项](#global-options)。
