---
source-git-commit: 0df07e865c3c4fc4ac14483972643eafa8814726
workflow-type: tm+mt
source-wordcount: '93'
ht-degree: 0%

---
# 用于删除Fastly的自定义VCL的Include文件

## 删除自定义VCL代码片段

1. [登录](/help/get-started/onboarding.md#access-your-admin-panel)管理员。

1. 单击&#x200B;**存储** > **设置** > **配置** > **高级** > **系统**。

1. 展开&#x200B;**全页缓存** > **Fastly配置** > **自定义VCL代码片段**。

   ![管理自定义VCL代码片段](/help/assets/cdn/fastly-manage-snippets.png)

1. 在&#x200B;_操作_&#x200B;列中，单击要删除的代码片段旁边的垃圾桶图标。

1. 在下一个模式窗口中，单击&#x200B;**DELETE**&#x200B;并激活新版本。

>[!WARNING]
>
>_自定义VCL代码片段_ UI选项仅显示通过Adobe Commerce管理员添加的代码片段。 如果您使用Fastly API添加代码片段，请使用该API [管理它们](/help/cloud-guide/cdn/fastly-vcl-custom-snippets.md#manage-vcl-using-the-api)。
