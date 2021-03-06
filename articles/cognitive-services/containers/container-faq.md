---
title: 认知服务容器常见问题解答 (FAQ)
titleSuffix: Azure Cognitive Services
description: 常见问题及其解答。
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: conceptual
ms.date: 08/07/2020
ms.author: v-johya
ms.openlocfilehash: e0d7ab5d1d6bdaa90fbc37c4b4939016fd6415da
ms.sourcegitcommit: caa18677adb51b5321ad32ae62afcf92ac00b40b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/08/2020
ms.locfileid: "88023714"
---
# <a name="azure-cognitive-services-containers-frequently-asked-questions-faq"></a>Azure 认知服务容器常见问题解答 (FAQ)

## <a name="general-questions"></a>一般问题

**问：认知服务容器提供哪些功能？**

**答:** Azure 认知服务容器可让开发人员使用 Azure 中提供的相同智能 API，但除此之外，还提供容器化的[优势](../cognitive-services-container-support.md#features-and-benefits)。 某些容器以封闭预览版的形式提供，可能需要通过应用程序进行访问。 其他容器公开以非封闭预览版或正式版的形式发布。 可在 [Azure 认知服务中的容器支持](../cognitive-services-container-support.md#container-availability-in-azure-cognitive-services)一文中找到容器及其可用性的完整列表。 

**问：认知服务云与容器之间是否有任何差异？**

**答:** 认知服务容器是认知服务云的替代方案。 容器提供的功能与相应的云服务相同。 客户可在本地或 Azure 中部署容器。 容器与相应云服务的核心 AI 技术、定价层、API 密钥和 API 签名是相同的。 容器的[这些功能和优势](../cognitive-services-container-support.md#features-and-benefits)使得它们比其对应的云服务更受欢迎。

**问：容器是否适用于所有认知服务，接下来期待会推出哪一套容器？**

**答:** 我们希望以容器产品/服务的形式推出更多的认知服务。 请与当地的 Microsoft 客户经理联系，以获取有关新容器版本和其他认知服务公告的更新。

**问：认知服务容器的服务级别协议 (SLA) 是什么？**

**答:** 认知服务容器不附带 SLA。

认知服务容器的资源配置由客户控制，因此 Microsoft 不会为正式版 (GA) 提供 SLA。 客户可以在本地自由部署容器，因此宿主环境由他们定义。

> [!IMPORTANT]
> 若要详细了解认知服务服务级别协议，请[访问我们的 SLA 页](https://www.azure.cn/support/sla/cognitive-services/index.html)。

### <a name="versioning"></a>版本控制

**问：如何将容器更新到最新版本？**

**答:** 客户可以选择何时更新已部署的容器。 容器将标有标准的 [Docker 标记](https://docs.docker.com/engine/reference/commandline/tag/)（例如 `latest`），表示使用的是最新版本。 我们建议客户下载发布的最新版本的容器。有关在映像更新后如何接收通知的详细信息，请查看 [Azure 容器注册表 Webhook](../../container-registry/container-registry-webhook.md)。
 
**问：支持哪些版本？**

**答:** 支持容器的当前版本和最新主要版本。 但是，我们建议客户保持使用最新技术。
 
**问：如何对更新进行版本控制？**

**答:** 主要版本变更表示 API 签名发生了中断性变更。 我们预计，这种变更往往与相应认知服务云产品/服务的主要版本变更相一致。 次要版本变更表示发生了 bug 修复、模型更新或推出了新的功能，但 API 签名未发生中断性变更。

## <a name="technical-questions"></a>技术问题

**问：如何在 IoT 设备上运行认知服务容器？**

**答:** 你没有可靠的 Internet 连接，或者想要节省带宽成本。 或者，如果具有低延迟要求，或者正在处理需要在现场进行分析的敏感数据，[Azure IoT Edge 与认知服务容器](https://azure.microsoft.com/blog/running-cognitive-services-on-iot-edge/)能提供与云的一致性。

**问：这些容器是否与 OpenShift 兼容？** 

我们不会通过 OpenShift 测试容器，但通常情况下，认知服务容器应可在支持 Docker 映像的任何平台上运行。 如果你使用 OpenShift，建议以 `root-user` 身份运行容器。

**问：如何提供产品反馈和功能建议？**

**答:** 我们鼓励客户公开[表达其关注的问题](https://cognitive.uservoice.com/)，并在可能有问题重叠时投票赞同其他人提出的问题。 用户语音工具可用于产品反馈和功能建议。

**问：如何联系支持人员？**

**答:** 客户支持渠道与认知服务云产品/服务相同。 所有认知服务容器包括日志记录功能，可帮助我们和社区为客户提供支持。 如需其他支持，请查看以下选项。

### <a name="customer-support-plan"></a>客户支持计划

客户应参考其 [Azure 支持计划](https://www.azure.cn/support/plans/)来确定要向谁请求支持。

### <a name="azure-knowledge-center"></a>Azure 知识中心

客户可以任意浏览 [Azure 知识中心](https://azure.microsoft.com/resources/knowledge-center/)以解答问题和帮助解决问题。

### <a name="stack-overflow"></a>Stack Overflow

> [Stack Overflow](https://en.wikipedia.org/wiki/Stack_Overflow) 是面向专业和发烧级程序员的问答站点。

如果遇到潜在问题并想要获得满意的解答，请浏览以下标记。

* [Azure 认知服务](https://stackoverflow.com/questions/tagged/azure-cognitive-services)
* [Microsoft 认知](https://stackoverflow.com/questions/tagged/microsoft-cognitive)

**问：如何计费？**

**答:** 类似于认知服务云，客户按消耗量付费。 容器需配置为向 Azure 发送计量数据，因此会相应地收取事务费。 跨托管服务和本地服务使用的资源将按分层定价计入到单个配额，并且会同时计入到这两个服务的用量。 有关更多详细信息，请参阅相应产品/服务的定价页。

* [异常检测器][AD - 容器 - 计费]
* [计算机视觉][cv-containers-billing]
* [人脸][fa-containers-billing]
* [表单识别器][FR - 容器 - 计费]
* [语言理解 (LUIS)][lu-containers-billing]
* [语音服务 API][SP - 容器 - 计费]
* [文本分析][ta-containers-billing]

> [!IMPORTANT]
> 如果未连接到 Azure 进行计量，则无法授权并运行认知服务容器。 客户需要始终让容器向计量服务传送账单信息。 认知服务容器不会将客户数据发送给 Microsoft。
 
**问：容器的当前支持担保是什么？**

**答:** 预览版不提供任何担证。 在正式宣布推出容器的正式版 (GA) 后，将实施 Microsoft 面向企业软件的标准担保。
 
**问：断开 Internet 连接时，认知服务容器会发生什么情况？**

**答:** 如果未连接到 Azure 进行计量，则无法授权并运行认知服务容器。 客户需使容器始终能够与计量服务通信。

**问：在未连接到 Azure 的情况下，容器可以运行多长时间？**

**答:** 如果未连接到 Azure 进行计量，则无法授权并运行认知服务容器。 客户需使容器始终能够与计量服务通信。
 
**问：当前需要哪些硬件才能运行这些容器？**

**答:** 认知服务容器是基于 x64 的容器，可运行任何兼容的 Linux 节点、VM，以及支持 x64 Linux Docker 容器的边缘设备。 这些设备都需要 CPU 处理器。 下面提供了每个容器产品/服务的最低和推荐配置：

* [异常检测器][AD - 容器 - 建议]
* [计算机视觉][cv-containers-recommendations]
* [人脸][fa-containers-recommendations]
* [表单识别器][FR - 容器 - 建议]
* [语言理解 (LUIS)][lu-containers-recommendations]
* [语音服务 API][SP - 容器 - 建议]
* [文本分析][ta-containers-recommendations]
 
**问：Windows 当前是否支持这些容器？**

**答:** 认知服务容器属于 Linux 容器，但 Windows 对 Linux 容器提供一定的支持。 有关 Windows 上的 Linux 容器的详细信息，请参阅 [Docker 文档](https://blog.docker.com/2017/09/preview-linux-containers-on-windows/)。
 
**问：如何发现容器？**

**答:** 认知服务容器在各个位置均可用，例如 Azure 门户、Docker Hub 和 Azure 容器注册表。 有关最新的容器位置，请参阅[容器存储库和映像](../cognitive-services-container-support.md#container-repositories-and-images)。

**问：认知服务容器与 AWS 和 Google 产品/服务有何不同？**

**答:** Microsoft 是第一家将预先训练的 AI 模型移入容器并采用简单的按事务计费的云提供商，在体验中客户如同使用云服务一样。 Microsoft 相信混合云能够为客户提供更多的选择。

**问：容器已获得哪些合规认证？**

**答:** 认知服务容器未获得任何合规认证

**问：可在哪些区域使用认知服务容器？**

**答:** 可在任何区域的任何位置运行容器，但是，容器需要一个密钥，并需要接回 Azure 以进行计量。 云服务支持的所有区域也受访问计量的容器的支持。

[!INCLUDE [Containers next steps](includes/containers-next-steps.md)]

[cv-containers]: ../computer-vision/computer-vision-how-to-install-containers.md
[fa-containers]: ../face/face-how-to-install-containers.md
[lu-containers]: ../luis/luis-container-howto.md
[ta-containers]: ../text-analytics/how-tos/text-analytics-how-to-install-containers.md

[cv-containers-billing]: ../computer-vision/computer-vision-how-to-install-containers.md#billing
[fa-containers-billing]: ../face/face-how-to-install-containers.md#billing
[lu-containers-billing]: ../luis/luis-container-howto.md#billing
[ta-containers-billing]: ../text-analytics/how-tos/text-analytics-how-to-install-containers.md#billing

[cv-containers-recommendations]: ../computer-vision/computer-vision-how-to-install-containers.md#container-requirements-and-recommendations
[fa-containers-recommendations]: ../face/face-how-to-install-containers.md#container-requirements-and-recommendations
[lu-containers-recommendations]: ../luis/luis-container-howto.md#container-requirements-and-recommendations
[ta-containers-recommendations]: ../text-analytics/how-tos/text-analytics-how-to-install-containers.md#container-requirements-and-recommendations

