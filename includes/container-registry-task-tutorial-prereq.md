---
author: rockboyfor
ms.service: container-registry
ms.topic: include
origin.date: 05/02/2019
ms.date: 08/26/2019
ms.author: v-yeche
ms.openlocfilehash: 57fb5024cfa07061ce9908d90547f15507365a45
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "70134508"
---
## <a name="prerequisites"></a>先决条件

### <a name="get-sample-code"></a>获取示例代码

本教程假定你已对示例存储库创建分支和克隆。

<!--Not Available on [previous tutorial](../articles/container-registry/container-registry-tutorial-quick-task.md)-->
<!--Not Available on [Prerequisites](../articles/container-registry/container-registry-tutorial-quick-task.md#prerequisites)-->
<!--Not Available on  If you haven't already done so, complete the steps in the Prerequisites section of the previous tutorial before proceeding.-->

### <a name="container-registry"></a>容器注册表

Azure 订阅中必须具有 Azure 容器注册表才能完成此教程。 如果需要注册表，请参阅[快速入门：使用 Azure CLI 创建容器注册表](../articles/container-registry/container-registry-get-started-azure-cli.md)。

<!--Not Available on [previous tutorial](../articles/container-registry/container-registry-tutorial-quick-task.md)-->

## <a name="create-a-github-personal-access-token"></a>创建 GitHub 个人访问令牌

若要在向 Git 存储库提交内容时触发任务，ACR 任务需要用于访问存储库的个人访问令牌 (PAT)。 如果还没有 PAT，请按照以下步骤在 GitHub 中生成一个：

1. 导航到 GitHub 上的 PAT 创建页面 https://github.com/settings/tokens/new
1. 输入令牌的简短**说明**，例如“ACR 任务演示”
1. 选择 ACR 的作用域以访问存储库。 要像本教程一样访问公共存储库，请在“存储库”下方，启用“存储库:状态”和“public_repo”   

    ![GitHub 中个人访问令牌生成页面的屏幕截图][build-task-01-new-token]

    > [!NOTE]
    > 若要生成 PAT 以访问专用  存储库，请选择完全**存储库**控制的作用域。

1. 选择“生成令牌”按钮（可能会要求你确认密码） 
1. 将生成的令牌复制并保存到**安全位置**（在后续部分定义任务时会使用此令牌）

    ![GitHub 中已生成的个人访问令牌的屏幕截图][build-task-02-generated-token]

<!-- Images -->

[build-task-01-new-token]: ./media/container-registry-task-tutorial-prereq/build-task-01-new-token.png
[build-task-02-generated-token]: ./media/container-registry-task-tutorial-prereq/build-task-02-generated-token.png
