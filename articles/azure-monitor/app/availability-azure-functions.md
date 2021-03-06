---
title: 使用 Azure Functions 创建和运行自定义可用性测试
description: 本文档将介绍如何使用 TrackAvailability() 创建一个 Azure 函数，该函数会根据 TimerTrigger 函数中给定的配置定期运行。 此测试的结果将发送到 Application Insights 资源，你可以在其中查询可用性结果数据并对其发出警报。 使用自定义测试，你可以编写比使用门户 UI 更为复杂的可用性测试、监视 Azure VNET 内部的应用、更改终结点地址或创建可用性测试（如果在你所在的区域中不可用）。
ms.topic: conceptual
author: Johnnytechn
ms.author: v-johya
ms.date: 05/25/2020
ms.openlocfilehash: 92d1109eb5c5e351498cd1beb81af6942a335f45
ms.sourcegitcommit: be0a8e909fbce6b1b09699a721268f2fc7eb89de
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/29/2020
ms.locfileid: "84199860"
---
# <a name="create-and-run-custom-availability-tests-using-azure-functions"></a>使用 Azure Functions 创建和运行自定义可用性测试

本文将介绍如何使用 TrackAvailability() 创建一个 Azure 函数，该函数会根据 TimerTrigger 函数中给定的配置和你自己的业务逻辑定期运行。 此测试的结果将发送到 Application Insights 资源，你可以在其中查询可用性结果数据并对其发出警报。 这样，你就可以创建自定义测试，类似于通过门户的[可用性监视](../../azure-monitor/app/monitor-web-app-availability.md)执行的测试。 使用自定义测试，你可以编写比使用门户 UI 更为复杂的可用性测试、监视 Azure VNET 内部的应用、更改终结点地址或创建可用性测试（即使该功能在你所在的区域中不可用）。

> [!NOTE]
> 此示例只是为了向你展示TrackAvailability() API 调用在 Azure 函数中的工作机制， 而不是为了演示如何编写基础 HTTP 测试代码/业务逻辑，后者是将其转变为功能完全正常的可用性测试所需的。 默认情况下，如果你逐步完成此示例，你将创建一个始终会产生故障的可用性测试。

## <a name="create-timer-triggered-function"></a>创建计时器触发的函数

- 如果有 Application Insights 资源：
    - 默认情况下，Azure Functions 会创建 Application Insights 资源，但如果你想要使用已创建的资源之一，则需在创建过程中指定。
    - 使用以下选项，按照有关如何[创建 Azure Functions 资源和计时器触发的函数](/azure-functions/functions-create-scheduled-function)的说明进行操作（在清理前停止）。
        -  选择顶部附近的“监视”选项卡。

            ![ 使用自己的 App Insights 资源创建 Azure Functions 应用](./media/availability-azure-functions/create-function-app.png)

        - 选择“Application Insights”下拉框，然后键入或选择资源的名称。

            ![选择现有 Application Insights 资源](./media/availability-azure-functions/app-insights-resource.png)

        - 选择“查看 + 创建”
- 如果尚未为计时器触发的函数创建 Application Insights 资源：
    - 默认情况下，在创建 Azure Functions 应用程序时，它将为你创建一个 Application Insights 资源。
    - 按照有关如何[创建 Azure Functions 资源和计时器触发的函数](/azure-functions/functions-create-scheduled-function)的说明进行操作（在清理前停止）。

## <a name="sample-code"></a>代码示例

将下面的代码复制到 run.csx 文件中（这将替换预先存在的代码）。 为此，请进入 Azure Functions 应用程序，选择左侧的计时器触发器函数。

>[!div class="mx-imgBorder"]
>![Azure 门户中 Azure 函数的 run.csx](./media/availability-azure-functions/runcsx.png)

> [!NOTE]
> 对于“终结点地址”，你将使用 `EndpointAddress= https://dc.applicationinsights.azure.cn/v2/track`。 
<!-- Correct on Endpoint Address in MC:  https://dc.applicationinsights.azure.cn/v2/track -->

```C#
#load "runAvailabilityTest.csx"
 
using System;
using System.Diagnostics;
using Microsoft.ApplicationInsights;
using Microsoft.ApplicationInsights.Channel;
using Microsoft.ApplicationInsights.DataContracts;
using Microsoft.ApplicationInsights.Extensibility;
 
// The Application Insights Instrumentation Key can be changed by going to the overview page of your Function App, selecting configuration, and changing the value of the APPINSIGHTS_INSTRUMENTATIONKEY Application setting.
// DO NOT replace the code below with your instrumentation key, the key's value is pulled from the environment variable/application setting key/value pair.
private static readonly string instrumentationKey = Environment.GetEnvironmentVariable("APPINSIGHTS_INSTRUMENTATIONKEY");
 
//[CONFIGURATION_REQUIRED]
// Visit /azure-monitor/app/custom-endpoints#regions-that-require-endpoint-modification for more details.
private const string EndpointAddress = "https://dc.applicationinsights.azure.cn/v2/track";
 
private static readonly TelemetryConfiguration telemetryConfiguration = new TelemetryConfiguration(instrumentationKey, new InMemoryChannel { EndpointAddress = EndpointAddress });
private static readonly TelemetryClient telemetryClient = new TelemetryClient(telemetryConfiguration);
 
public async static Task Run(TimerInfo myTimer, ILogger log)
{
    log.LogInformation($"Entering Run at: {DateTime.Now}");
 
    if (myTimer.IsPastDue)
    {
        log.LogWarning($"[Warning]: Timer is running late! Last ran at: {myTimer.ScheduleStatus.Last}");
    }
 
    // [CONFIGURATION_REQUIRED] provide {testName} accordingly for your test function
    string testName = "AvailabilityTestFunction";
 
    // REGION_NAME is a default environment variable that comes with App Service
    string location = Environment.GetEnvironmentVariable("REGION_NAME");
 
    log.LogInformation($"Executing availability test run for {testName} at: {DateTime.Now}");
    string operationId = Guid.NewGuid().ToString("N");
 
    var availability = new AvailabilityTelemetry
    {
        Id = operationId,
        Name = testName,
        RunLocation = location,
        Success = false
    };
 
    var stopwatch = new Stopwatch();
    stopwatch.Start();
 
    try
    {
        await RunAvailbiltyTestAsync(log);
        availability.Success = true;
    }
    catch (Exception ex)
    {
        availability.Message = ex.Message;
 
        var exceptionTelemetry = new ExceptionTelemetry(ex);
        exceptionTelemetry.Context.Operation.Id = operationId;
        exceptionTelemetry.Properties.Add("TestName", testName);
        exceptionTelemetry.Properties.Add("TestLocation", location);
        telemetryClient.TrackException(exceptionTelemetry);
    }
    finally
    {
        stopwatch.Stop();
        availability.Duration = stopwatch.Elapsed;
        availability.Timestamp = DateTimeOffset.UtcNow;
 
        telemetryClient.TrackAvailability(availability);
        // call flush to ensure telemetry is sent
        telemetryClient.Flush();
    }
}

```

在“视图文件”下的右侧，选择“添加”。 将新文件命名为“function.proj”，其中包含以下配置。

```C#
<Project Sdk="Microsoft.NET.Sdk">
    <PropertyGroup>
        <TargetFramework>netstandard2.0</TargetFramework>
    </PropertyGroup>
    <ItemGroup>
        <PackageReference Include="Microsoft.ApplicationInsights.AspNetCore" Version="2.8.2" /> <!-- Ensure you’re using the latest version -->
    </ItemGroup>
</Project>

```

>[!div class="mx-imgBorder"]
>![在右侧选择“添加”。 将文件命名为“function.proj”](./media/availability-azure-functions/addfile.png)

在“视图文件”下的右侧，选择“添加”。 将新文件命名为“runAvailabilityTest.csx”，其中包含以下配置。

```C#
public async static Task RunAvailbiltyTestAsync(ILogger log)
{
    // Add your business logic here.
    throw new NotImplementedException();
}

```

## <a name="check-availability"></a>检查可用性

为了确保一切正常工作，可以查看 Application Insights 资源的“可用性”选项卡中的图。

> [!NOTE]
> 如果在 runAvailabilityTest.csx 中实现了自己的业务逻辑，那么你将看到成功结果（如下面的屏幕截图所示）；如果未实现，那么你将看到失败结果。 使用 `TrackAvailability()` 创建的测试显示时会在测试名称旁边带有“CUSTOM”字样。

>[!div class="mx-imgBorder"]
>![显示成功结果的“可用性”选项卡](./media/availability-azure-functions/availability-custom.png)

若要查看端到端事务详细信息，请在“深入钻取”下选择“成功”或“失败”，然后选择一个示例。 还可以通过选择图上的数据点来访问端到端事务详细信息。

>[!div class="mx-imgBorder"]
>![选择示例可用性测试](./media/availability-azure-functions/sample.png)

>[!div class="mx-imgBorder"]
>![端到端事务详细信息](./media/availability-azure-functions/end-to-end.png)

如果按原样运行所有代码（不添加业务逻辑），则会看到测试失败。

## <a name="query-in-logs-analytics"></a>在“日志(分析)”中查询

可以使用“日志(分析)”查看可用性结果、依赖关系等。 若要详细了解“日志”，请访问[日志查询概述](../../azure-monitor/log-query/log-query-overview.md)。

>[!div class="mx-imgBorder"]
>![可用性结果](./media/availability-azure-functions/availabilityresults.png)

>[!div class="mx-imgBorder"]
>![依赖项](./media/availability-azure-functions/dependencies.png)

## <a name="next-steps"></a>后续步骤

- [应用程序映射](../../azure-monitor/app/app-map.md)
- [事务诊断](../../azure-monitor/app/transaction-diagnostics.md)

