---
title: 监视和排查 Azure IoT 中心的连接断开问题
description: 了解如何监视和排查 Azure IoT 中心设备连接的常见错误
author: jlian
manager: briz
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
origin.date: 01/30/2020
ms.date: 03/09/2020
ms.author: v-yiso
ms.openlocfilehash: 25a809a59cc484d5f5117727905709e56ad8aec9
ms.sourcegitcommit: 0130a709d934d89db5cccb3b4997b9237b357803
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/29/2020
ms.locfileid: "84186883"
---
# <a name="monitor-diagnose-and-troubleshoot-disconnects-with-azure-iot-hub"></a>监视、诊断和排查 Azure IoT 中心的连接断开问题

由于存在许多可能的故障点，IoT 设备的连接问题有时很难排查。 应用程序逻辑、物理网络、协议、硬件、IoT 中心和其他云服务都可能会导致问题。 检测和查明问题根源的能力至关重要。 但是，大规模的 IoT 解决方案可能有数千台设备，因此，手动检查各台设备是不切实际的。 为了帮助你大规模地检测、诊断和排查这些问题，请使用 IoT 中心通过 Azure Monitor 提供的监视功能。 这些功能仅限于 IoT 中心可以观察到的内容，因此我们还建议你遵循适用于设备和其他 Azure 服务的监视最佳做法。

## <a name="get-alerts-and-error-logs"></a>获取警报和错误日志

使用 Azure Monitor 可在设备断开连接时获取警报并写入日志。

### <a name="turn-on-diagnostic-logs"></a>启用诊断日志 

若要记录设备连接事件和错误，请为 IoT 中心启用诊断。 建议尽早启用这些日志，因为如果未启用诊断日志，则当设备断开连接时，你将不会有任何信息可用来排查问题。

1. 登录 [Azure 门户](https://portal.azure.cn)。

2. 浏览到 IoT 中心。

3. 选择“诊断设置”  。

4. 选择“启用诊断”  。

5. 启用要收集的“连接”日志  。

6. 为便于分析，请启用“发送到 Log Analytics”  。 请参阅[解决连接错误](#resolve-connectivity-errors)下的示例。

   ![建议的设置](./media/iot-hub-troubleshoot-connectivity/diagnostic-settings-recommendation.png)

有关详细信息，请参阅[监视 Azure IoT 中心的运行状况并快速诊断问题](iot-hub-monitor-resource-health.md)。

### <a name="set-up-alerts-for-device-disconnect-at-scale"></a>针对设备断开连接大规模地设置警报

若要在设备断开连接时获取警报，请针对“联网设备(预览版)”指标配置警报  。

1. 登录 [Azure 门户](https://portal.azure.cn)。

2. 浏览到 IoT 中心。

3. 选择“**警报**”。

4. 选择“新建警报规则”。 

5. 选择“添加条件”，然后选择“联网设备(预览版)”。 

6. 按照提示设置阈值和警报。

若要了解详细信息，请参阅[什么是 Microsoft Azure 中的警报？](../azure-monitor/platform/alerts-overview.md)。

#### <a name="detecting-individual-device-disconnects"></a>检测各个设备断开连接

若要检测“每设备”  断开连接，例如当需要知道某个工厂刚刚离线时，请[使用事件网格配置设备断开连接事件](iot-hub-event-grid.md)。

## <a name="resolve-connectivity-errors"></a>解决连接错误

为联网设备启用诊断日志和警报后，如果出错，则会收到警报。 本部分介绍了如何在收到警报时查找常见问题。 以下步骤假设已经在 Azure Monitor 日志中设置了诊断日志。

1. 登录 [Azure 门户](https://portal.azure.cn)。

1. 浏览到 IoT 中心。

1. 选择“日志”。 

1. 若要隔离 IoT 中心的连接错误日志，请输入以下查询，然后选择“运行”  ：

    ```kusto
    AzureDiagnostics
    | where ( ResourceType == "IOTHUBS" and Category == "Connections" and Level == "Error")
    ```

1. 如果返回了结果，请查看 `OperationName`、`ResultType`（错误代码）和 `ResultDescription`（错误消息），以获取有关错误的更多详细信息。

   ![错误日志示例](./media/iot-hub-troubleshoot-connectivity/diag-logs.png)

1. 对于最常见的错误，请遵循以下问题解决指南：

    - **[404104 DeviceConnectionClosedRemotely](iot-hub-troubleshoot-error-404104-deviceconnectionclosedremotely.md)**
    - **[401003 IoTHubUnauthorized](iot-hub-troubleshoot-error-401003-iothubunauthorized.md)**
    - **[409002 LinkCreationConflict](iot-hub-troubleshoot-error-409002-linkcreationconflict.md)**
    - **[500001 ServerError](iot-hub-troubleshoot-error-500xxx-internal-errors.md)**
    - **[500008 GenericTimeout](iot-hub-troubleshoot-error-500xxx-internal-errors.md)**

## <a name="i-tried-the-steps-but-they-didnt-work"></a>我尝试了这些步骤，但它们不起作用

如果前面的步骤没有帮助，请尝试以下操作：

* 如果你有权以物理方式或远程访问（例如通过 SSH）有问题的设备，请遵循[设备端故障排除指南](https://github.com/Azure/azure-iot-sdk-node/wiki/Troubleshooting-Guide-Devices)继续进行故障排除。
* 在 Azure 门户 > IoT 中心 > IoT 设备中验证你的设备是否**已启用**。
* 如果设备使用 MQTT 协议，请确认端口 8883 已打开。 有关详细信息，请参阅[连接到 IoT 中心 (MQTT)](iot-hub-mqtt-support.md#connecting-to-iot-hub)。
* 从 [Azure IoT 中心论坛](https://social.msdn.microsoft.com/Forums/azure/home?forum=azureiothub)、[Stack Overflow](https://stackoverflow.com/questions/tagged/azure-iot-hub) 或 [Azure 支持](https://azure.microsoft.com/support/options/)获得帮助。

如果本指南未能提供所需的帮助，请在下面的反馈部分中留言，以帮助我们改进文档。

## <a name="next-steps"></a>后续步骤

* 要了解有关解决暂时性问题的详细信息，请参阅[暂时性故障处理](https://docs.microsoft.com/en-us/azure/architecture/best-practices/transient-faults)。
* 要了解有关 Azure IoT SDK 和管理重试的详细信息，请参阅[如何使用 Azure IoT Hub 设备 SDK 管理连接和可靠消息传递](iot-hub-reliability-features-in-sdks.md#connection-and-retry)。
