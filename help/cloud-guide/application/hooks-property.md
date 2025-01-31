---
title: 挂接属性
description: 请参阅有关如何在 [!DNL Commerce] 应用程序配置文件中配置挂接属性的示例。
feature: Cloud, Configuration, Build, Deploy
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '316'
ht-degree: 0%

---

# 挂接属性

使用`hooks`部分在生成、部署和部署后阶段运行外壳命令：

- **`build`** — 在&#x200B;_打包应用程序之前执行命令_。 诸如数据库或Redis之类的服务不可用，因为应用程序尚未部署。 在&#x200B;_之前添加自定义命令_&#x200B;和默认的`php ./vendor/bin/ece-tools`命令，以便自定义生成的内容继续进入部署阶段。

- **`deploy`** — 在&#x200B;_打包和部署应用程序后执行命令_。 此时您可以访问其他服务。 由于默认的`php ./vendor/bin/ece-tools`命令将`app/etc`目录复制到正确的位置，因此您必须在&#x200B;_部署命令之后添加自定义命令_&#x200B;以防止自定义命令失败。

- **`post_deploy`** — 在部署应用程序后&#x200B;_执行命令_，在容器开始接受连接&#x200B;_后执行命令_。 `post_deploy`挂接清除缓存并预加载（加温）缓存。 您可以使用[部署后阶段](../environment/variables-post-deploy.md)中的`WARM_UP_PAGES`变量自定义页面列表。 尽管不是必需的，但此变量可与`SCD_ON_DEMAND`环境变量配合使用。

以下示例显示了`.magento.app.yaml`文件中的默认配置。 在`ece-tools`命令&#x200B;_之前的`build`、`deploy`或`post_deploy`节_&#x200B;下添加CLI命令：

```yaml
hooks:
    # We run build hooks before your application has been packaged.
    build: |
        set -e
        composer install
        php ./vendor/bin/ece-tools run scenario/build/generate.xml
        php ./vendor/bin/ece-tools run scenario/build/transfer.xml
    # We run deploy hook after your application has been deployed and started.
    deploy: |
        php ./vendor/bin/ece-tools run scenario/deploy.xml
    # We run post deploy hook to clean and warm the cache. Available with ECE-Tools 2002.0.10.
    post_deploy: |
        php ./vendor/bin/ece-tools run scenario/post-deploy.xml
```

此外，您还可以使用`generate`和`transfer`命令进一步自定义构建阶段，以在专门构建代码或移动文件时执行其他操作。

```yaml
hooks:
    # We run build hooks before your application has been packaged.
    build: |
        set -e
        php ./vendor/bin/ece-tools build:generate
        # php /path/to/your/script
        php ./vendor/bin/ece-tools build:transfer
```

- `set -e` — 导致挂接在第一个失败的命令上失败，而不是在最后一个失败的命令上失败。
- `build:generate` — 如果为生成阶段启用了SCD，则应用修补程序、验证配置、生成DI并生成静态内容。
- `build:transfer` — 将生成的代码和静态内容传输到最终目标。

命令从应用程序(`/app`)目录运行。 您可以使用`cd`命令更改目录。 如果挂接中的最终命令失败，则挂接将失败。 若要导致它们在第一个失败的命令上失败，请将`set -e`添加到挂接的开头。

**使用grunt**&#x200B;编译Sass文件：

```yaml
dependencies:
    ruby:
        sass: "3.4.7"
    nodejs:
        grunt-cli: "~0.1.13"

hooks:
    build: |
        cd public/profiles/project_name/themes/custom/theme_name
        npm install
        grunt
        cd
        php ./vendor/bin/ece-tools build
```

在静态内容部署之前使用`grunt`编译Sass文件，这会在生成期间发生。 将`grunt`命令放在`build`命令之前。

{{scenarios}}
