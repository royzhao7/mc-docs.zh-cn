---
title: 什么是 Azure 机器学习计算实例？
titleSuffix: Azure Machine Learning
description: 了解 Azure 机器学习计算实例 - 完全托管式基于云的工作站。
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: v-yiso
author: sdgilley
ms.date: 07/27/2020
ms.openlocfilehash: 0f8059f6f5c7375f4fe294e4684cfdb2c39ad19e
ms.sourcegitcommit: 78c71698daffee3a6b316e794f5bdcf6d160f326
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/11/2020
ms.locfileid: "90021375"
---
# <a name="what-is-an-azure-machine-learning-compute-instance"></a>什么是 Azure 机器学习计算实例？

Azure 机器学习计算实例是面向数据科学家的基于云的托管式工作站。

计算实例可让客户轻松地开始进行 Azure 机器学习开发，并为 IT 管理员提供管理和企业就绪功能。  

可以使用计算实例作为在云中进行机器学习的完全配置和托管的开发环境。 还可以在开发和测试中将它们用作训练和推理的计算目标。  

对于生产级模型训练，请使用具有多节点缩放功能的 [Azure 机器学习计算群集](how-to-create-attach-compute-sdk.md#amlcompute)。 对于生产级模型部署，请使用 [Azure Kubernetes 服务群集](how-to-deploy-azure-kubernetes-service.md)。

## <a name="why-use-a-compute-instance"></a>为何使用计算实例？

计算实例是完全托管式基于云的工作站，已针对机器学习开发环境进行优化。 它提供以下优势：

|主要优点|描述|
|----|----|
|工作效率|可以在 Azure 机器学习工作室中使用集成的笔记本及以下工具来构建和部署模型：<br/>-  Jupyter<br/>-  JupyterLab<br/>-  RStudio（预览版）<br/>计算实例与 Azure 机器学习工作区和工作室完全集成。 你可以与工作区中的其他数据科学家共享笔记本和数据。 你还可以使用 [SSH](how-to-set-up-vs-code-remote.md) 设置 VS Code 远程开发 |
|无需自行管理且安全|减少安全保护工作，增强企业的安全要求合规性。 计算实例提供可靠的管理策略和安全网络配置，例如：<br/><br/>- 通过资源管理器模板或 Azure 机器学习 SDK 自动预配<br/>- [Azure 基于角色的访问控制 (Azure RBAC)](/azure/role-based-access-control/overview)<br/>- [虚拟网络支持](how-to-enable-virtual-network.md#compute-instance)<br/>- 用于启用/禁用 SSH 访问的 SSH 策略<br/>已启用 TLS 1.2 |
|已针对 ML 进行了预配置|使用预配置的最新 ML 包、深度学习框架和 GPU 驱动程序完成设置任务，可节省时间。|
|完全可自定义|支持多种 Azure VM 类型，包括 GPU 和持久性低级自定义，例如，安装相应的包和驱动程序可以轻而易举地实现高级方案。 |

## <a name="tools-and-environments"></a><a name="contents"></a>工具和环境

> [!IMPORTANT]
> 下面标记了“（预览版）”的工具目前为公共预览版。
> 该预览版在提供时没有附带服务级别协议，建议不要将其用于生产工作负载。 某些功能可能不受支持或者受限。 

使用 Azure 机器学习计算实例可以在工作区中的完全集成式笔记本体验中创作、训练和部署模型。


以下工具和环境安装在计算实例上： 

|常规工具和环境|详细信息|
|----|:----:|
|驱动程序|`CUDA`</br>`cuDNN`</br>`NVIDIA`</br>`Blob FUSE` |
|Intel MPI 库||
|Azure CLI ||
|Azure 机器学习示例 ||
|Docker||
|Nginx||
|NCCL 2.0 ||
|Protobuf|| 

|**R** 工具和环境|详细信息|
|----|:----:|
|RStudio Server 开源版（预览版）||
|R 内核||
|适用于 R 的 Azure 机器学习 SDK|[azuremlsdk](https://azure.github.io/azureml-sdk-for-r/reference/index.html)</br>SDK 示例|

|**PYTHON** 工具和环境|详细信息|
|----|----|
|Anaconda Python||
|Jupyter 和扩展||
|Jupyterlab 和扩展||
[适用于 Python 的 Azure 机器学习 SDK](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py)</br>（来自 PyPI）|包括大多数 azureml 额外包。  若要查看完整列表，请[打开计算实例上的终端窗口](how-to-run-jupyter-notebooks.md#terminal)并运行 <br/> `conda list -n azureml_py36 azureml*` |
|其他 PyPI 包|`jupytext`</br>`tensorboard`</br>`nbconvert`</br>`notebook`</br>`Pillow`|
|Conda 包|`cython`</br>`numpy`</br>`ipykernel`</br>`scikit-learn`</br>`matplotlib`</br>`tqdm`</br>`joblib`</br>`nodejs`</br>`nb_conda_kernels`|
|深度学习包|`PyTorch`</br>`TensorFlow`</br>`Keras`</br>`Horovod`</br>`MLFlow`</br>`pandas-ml`</br>`scrapbook`|
|ONNX 包|`keras2onnx`</br>`onnx`</br>`onnxconverter-common`</br>`skl2onnx`</br>`onnxmltools`|                           
|Azure 机器学习 Python 和 R SDK 示例||

Python 包都安装在 **Python 3.6 - AzureML** 环境中。  

### <a name="installing-packages"></a>安装包

可以直接在 Jupyter 笔记本或 Rstudio 中安装包：

* RStudio 使用右下的“包”选项卡或左上的“控制台”选项卡。  
* Python:添加安装代码并在 Jupyter 笔记本单元中执行它。

也可通过以下任一方式访问终端窗口：

* RStudio：选择左上的“终端”选项卡。
* Jupyter 实验室：选择“启动器”选项卡中“其他”标题下的“终端”磁贴。
* Jupyter：在“文件”选项卡的右上方选择“新建>“终端”。
* 通过 SSH 连接到计算机。  然后，将 Python 包安装到 **Python 3.6 - AzureML** 环境中。  将 R 包安装到 **R** 环境中。

## <a name="accessing-files"></a>访问文件

笔记本和 R 脚本存储在 Azure 文件共享中工作区的默认存储帐户内。  这些文件位于“用户文件”目录下。 通过此存储可以轻松地在计算实例之间共享笔记本。 停止或删除计算实例时，存储帐户还会安全保存笔记本。

工作区的 Azure 文件共享帐户作为驱动器装载到计算实例上。 此驱动器是 Jupyter、Jupyter Labs 和 RStudio 的默认工作目录。 这意味着，在 Jupyter、JupyterLab 或 RStudio 中创建的笔记本和其他文件会自动存储在文件共享上，并可在其他计算实例中使用。

可以从同一工作区中的所有计算实例访问文件共享中的文件。 对计算实例上的这些文件所做的任何更改将可靠地保存回到文件共享。

还可以将最新 Azure 机器学习示例克隆到工作区文件共享中“用户文件”目录下的文件夹内。

与写入到计算实例本地磁盘本身相比，在网络驱动器上写入小文件可能速度更慢。  若要写入许多小文件，请尝试直接在计算实例上使用某个目录，例如 `/tmp` 目录。 请注意，无法从其他计算实例访问这些文件。 

你可以使用计算实例上的 `/tmp` 目录来保存临时数据。  但是，不要在计算实例的 OS 磁盘上写入大型数据文件。  请改用[数据存储](concept-azure-machine-learning-architecture.md#datasets-and-datastores)。 如果已安装 JupyterLab git 扩展，它也会导致计算实例性能下降。

## <a name="managing-a-compute-instance"></a>管理计算实例

在 Azure 机器学习工作室中的工作区内选择“计算”，然后在顶部选择“计算实例”。 

![管理计算实例](./media/concept-compute-instance/manage-compute-instance.png)

可执行以下操作：

* [创建计算实例](#create)。 
* 刷新“计算实例”选项卡。
* 启动、停止和重启计算实例。  只要实例在运行，你就需要为其付费。 不使用计算实例时，请将其停止，以便降低成本。 停止计算实例会将其解除分配。 然后在需要时重启。 
* 删除计算实例。
* 将计算实例的列表筛选为你创建的实例。  这些是你可以访问的计算实例。

对于工作区中你有权访问的每个计算实例，你可以：

* 访问计算实例上的 Jupyter、JupyterLab、RStudio
* 通过 SSH 连接到计算实例。 默认已禁用 SSH 访问，但可以在创建计算实例时启用。 SSH 访问是通过公钥/私钥机制实现的。 选项卡中将提供 IP 地址、用户名和端口号等 SSH 连接详细信息。
* 获取有关特定计算实例的详细信息，例如 IP 地址和区域。

使用 [RBAC](/role-based-access-control/overview) 可以控制工作区中的哪些用户可以创建、删除、启动、停止和重启计算实例。 充当工作区参与者和所有者角色的所有用户可以在整个工作区中创建、删除、启动、停止和重启计算实例。 但是，只有特定计算实例的创建者可在该计算实例上访问 Jupyter、JupyterLab 和 RStudio。 计算实例的创建者拥有专用的计算实例，具有根访问权限，且可从终端通过 Jupyter/JupyterLab/RStudio 进入。 计算实例具有创建者用户的单用户登录名，所有操作将使用该用户的标识进行试验运行的 RBAC 控制和权限划分。 SSH 访问是通过公钥/私钥机制控制的。

可以通过 RBAC 来控制这些操作：
* *Microsoft.MachineLearningServices/workspaces/computes/read*
* *Microsoft.MachineLearningServices/workspaces/computes/write*
* *Microsoft.MachineLearningServices/workspaces/computes/delete*
* *Microsoft.MachineLearningServices/workspaces/computes/start/action*
* *Microsoft.MachineLearningServices/workspaces/computes/stop/action*
* *Microsoft.MachineLearningServices/workspaces/computes/restart/action*

### <a name="create-a-compute-instance"></a><a name="create"></a>创建计算实例

在 Azure 机器学习工作室的工作区中，当你准备好运行某个笔记本时，请从“计算”部分或“笔记本”部分[创建新的计算实例](how-to-create-attach-compute-studio.md#compute-instance) 。 

也可以通过以下方式创建实例
* 直接从[集成式笔记本体验](tutorial-1st-experiment-sdk-setup.md#azure)
* 在 Azure 门户中配置
* 通过 Azure 资源管理器模板。 有关示例模板，请参阅[创建 Azure 机器学习计算实例模板](https://github.com/Azure/azure-quickstart-templates/tree/master/101-machine-learning-compute-create-computeinstance)。
* 使用 Azure 机器学习 SDK
* 从 [Azure 机器学习的 CLI 扩展](reference-azure-machine-learning-cli.md#computeinstance)

应用于计算实例创建过程的每区域每 VM 系列专用核心数配额和区域总配额 与 Azure 机器学习训练计算群集配额统一并共享。 停止计算实例不会释放配额，因此无法确保你能够重启计算实例。

## <a name="compute-target"></a>计算目标

计算实例可用作类似于 Azure 机器学习计算群集的[训练计算目标](concept-compute-target.md#train)。 

计算实例：
* 具有作业队列。
* 在虚拟网络环境中安全地运行作业，无需企业打开 SSH 端口。 作业在容器化环境中执行，并将模型依赖项打包到 Docker 容器中。
* 可以并行运行多个小型作业（预览版）。  每个核心可以并行运行两个作业，而剩余的作业将排队。
* 支持单节点多 GPU 分布式训练作业

可以使用计算实例作为测试/调试方案的本地推理部署目标。

## <a name="what-happened-to-notebook-vm"></a><a name="notebookvm"></a>Notebook VM 发生了什么情况？

计算实例即将取代 Notebook VM。  

任何存储在工作区文件共享中的笔记本文件和工作区数据存储中的数据都可以从计算实例访问。 但是，以前安装在 Notebook VM 上的任何自定义包都需要在计算实例上重新安装。 创建计算群集时适用的配额限制在创建计算实例时同样适用。 

不能创建新的 Notebook VM。 但你仍然可以访问和使用已创建的 Notebook VM 及其完整功能。 可以在现有 Notebook VM 所在的同一工作区中创建计算实例。 


## <a name="next-steps"></a>后续步骤

 * [教程：训练自己的首个 ML 模型](tutorial-1st-experiment-sdk-train.md)演示如何将计算实例与集成的笔记本配合使用。
