---
title: Azure Functions SendGrid 绑定
description: Azure Functions SendGrid 绑定参考。
author: craigshoemaker
ms.topic: reference
ms.date: 03/18/2020
ms.author: v-junlch
ms.openlocfilehash: cfb2de3f9d753f7edbf3f984ec3bcd74fdf4cd32
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "79546886"
---
# <a name="azure-functions-sendgrid-bindings"></a>Azure Functions SendGrid 绑定

本文介绍如何使用 Azure Functions 中的 [SendGrid](https://sendgrid.com/docs/User_Guide/index.html) 绑定发送电子邮件。 Azure Functions 支持 SendGrid 的输出绑定。

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="packages---functions-1x"></a>包 - Functions 1.x

[Microsoft.Azure.WebJobs.Extensions.SendGrid](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.SendGrid) NuGet 包 2.x 版本中提供了 SendGrid 绑定。 [azure-webjobs-sdk-extensions](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/v2.x/src/WebJobs.Extensions.SendGrid/) GitHub 存储库中提供了此包的源代码。

[!INCLUDE [functions-package](../../includes/functions-package.md)]

## <a name="packages---functions-2x-and-higher"></a>包 - Functions 2.x 及更高版本

[Microsoft.Azure.WebJobs.Extensions.SendGrid](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.SendGrid) NuGet 包 3.x 版本中提供了 SendGrid 绑定。 [azure-webjobs-sdk-extensions](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.SendGrid/) GitHub 存储库中提供了此包的源代码。

[!INCLUDE [functions-package-v2](../../includes/functions-package-v2.md)]

## <a name="example"></a>示例

# <a name="c"></a>[C#](#tab/csharp)

以下示例演示使用服务总线队列触发器和 SendGrid 输出绑定的 [C# 函数](functions-dotnet-class-library.md)。

### <a name="synchronous"></a>同步

```cs
using SendGrid.Helpers.Mail;

...

[FunctionName("SendEmail")]
public static void Run(
    [ServiceBusTrigger("myqueue", Connection = "ServiceBusConnection")] Message email,
    [SendGrid(ApiKey = "CustomSendGridKeyAppSettingName")] out SendGridMessage message)
{
var emailObject = JsonConvert.DeserializeObject<OutgoingEmail>(Encoding.UTF8.GetString(email.Body));

message = new SendGridMessage();
message.AddTo(emailObject.To);
message.AddContent("text/html", emailObject.Body);
message.SetFrom(new EmailAddress(emailObject.From));
message.SetSubject(emailObject.Subject);
}

public class OutgoingEmail
{
    public string To { get; set; }
    public string From { get; set; }
    public string Subject { get; set; }
    public string Body { get; set; }
}
```

### <a name="asynchronous"></a>异步

```cs
using SendGrid.Helpers.Mail;

...

[FunctionName("SendEmail")]
public static async void Run(
 [ServiceBusTrigger("myqueue", Connection = "ServiceBusConnection")] Message email,
 [SendGrid(ApiKey = "CustomSendGridKeyAppSettingName")] IAsyncCollector<SendGridMessage> messageCollector)
{
 var emailObject = JsonConvert.DeserializeObject<OutgoingEmail>(Encoding.UTF8.GetString(email.Body));

 var message = new SendGridMessage();
 message.AddTo(emailObject.To);
 message.AddContent("text/html", emailObject.Body);
 message.SetFrom(new EmailAddress(emailObject.From));
 message.SetSubject(emailObject.Subject);
 
 await messageCollector.AddAsync(message);
}

public class OutgoingEmail
{
 public string To { get; set; }
 public string From { get; set; }
 public string Subject { get; set; }
 public string Body { get; set; }
}
```

如果在应用设置中指定了名为“AzureWebJobsSendGridApiKey”的 API 密钥，则可以不设置特性的 `ApiKey` 属性。

# <a name="c-script"></a>[C# 脚本](#tab/csharp-script)

以下示例演示 *function.json* 文件中的一个 SendGrid 输出绑定以及使用该绑定的 [C# 脚本函数](functions-reference-csharp.md)。

下面是 function.json  文件中的绑定数据：

```json 
{
    "bindings": [
        {
          "type": "queueTrigger",
          "name": "mymsg",
          "queueName": "myqueue",
          "connection": "AzureWebJobsStorage",
          "direction": "in"
        },
        {
          "type": "sendGrid",
          "name": "$return",
          "direction": "out",
          "apiKey": "SendGridAPIKeyAsAppSetting",
          "from": "{FromEmail}",
          "to": "{ToEmail}"
        }
    ]
}
```

[配置](#configuration)部分解释了这些属性。

C# 脚本代码如下所示：

```csharp
#r "SendGrid"

using System;
using SendGrid.Helpers.Mail;
using Microsoft.Azure.WebJobs.Host;

public static SendGridMessage Run(Message mymsg, ILogger log)
{
    SendGridMessage message = new SendGridMessage()
    {
        Subject = $"{mymsg.Subject}"
    };
    
    message.AddContent("text/plain", $"{mymsg.Content}");

    return message;
}
public class Message
{
    public string ToEmail { get; set; }
    public string FromEmail { get; set; }
    public string Subject { get; set; }
    public string Content { get; set; }
}
```

# <a name="javascript"></a>[JavaScript](#tab/javascript)

以下示例演示 *function.json* 文件中的一个 SendGrid 输出绑定以及使用该绑定的 [JavaScript 函数](functions-reference-node.md)。

下面是 function.json  文件中的绑定数据：

```json 
{
    "bindings": [
        {
            "name": "$return",
            "type": "sendGrid",
            "direction": "out",
            "apiKey" : "MySendGridKey",
            "to": "{ToEmail}",
            "from": "{FromEmail}",
            "subject": "SendGrid output bindings"
        }
    ]
}
```

[配置](#configuration)部分解释了这些属性。

JavaScript 代码如下所示：

```javascript
module.exports = function (context, input) {
    var message = {
         "personalizations": [ { "to": [ { "email": "sample@sample.com" } ] } ],
        from: { email: "sender@contoso.com" },
        subject: "Azure news",
        content: [{
            type: 'text/plain',
            value: input
        }]
    };

    context.done(null, message);
};
```

# <a name="java"></a>[Java](#tab/java)

以下示例使用 [Java 函数运行时库](https://docs.microsoft.com/java/api/overview/azure/functions/runtime)中的 `@SendGridOutput` 注释来发送使用 SendGrid 输出绑定的电子邮件。

```java
package com.function;

import java.util.*;
import com.microsoft.azure.functions.annotation.*;
import com.microsoft.azure.functions.*;

public class HttpTriggerSendGrid {

    @FunctionName("HttpTriggerSendGrid")
    public HttpResponseMessage run(

        @HttpTrigger(
            name = "req",
            methods = { HttpMethod.GET, HttpMethod.POST },
            authLevel = AuthorizationLevel.FUNCTION)
                HttpRequestMessage<Optional<String>> request,

        @SendGridOutput(
            name = "message",
            dataType = "String",
            apiKey = "SendGrid_API_Key",
            to = "user@contoso.com",
            from = "sender@contoso.com",
            subject = "Azure Functions email with SendGrid",
            text = "Sent from Azure Functions")
                OutputBinding<String> message,

        final ExecutionContext context) {

        final String toAddress = "user@contoso.com";
        final String value = "Sent from Azure Functions";

        StringBuilder builder = new StringBuilder()
            .append("{")
            .append("\"personalizations\": [{ \"to\": [{ \"email\": \"%s\"}]}],")
            .append("\"content\": [{\"type\": \"text/plain\", \"value\": \"%s\"}]")
            .append("}");

        final String body = String.format(builder.toString(), toAddress, value);

        message.setValue(body);

        return request.createResponseBuilder(HttpStatus.OK).body("Sent").build();
    }
}
```

---

## <a name="attributes-and-annotations"></a>特性和注释

# <a name="c"></a>[C#](#tab/csharp)

在 [C# 类库](functions-dotnet-class-library.md)中，使用 [SendGrid](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.SendGrid/SendGridAttribute.cs) 特性。

有关可以配置的特性属性的信息，请参阅[配置](#configuration)。 下面是某个方法签名中的 `SendGrid` 特性示例：

```csharp
[FunctionName("SendEmail")]
public static void Run(
    [ServiceBusTrigger("myqueue", Connection = "ServiceBusConnection")] OutgoingEmail email,
    [SendGrid(ApiKey = "CustomSendGridKeyAppSettingName")] out SendGridMessage message)
{
    ...
}
```

有关完整示例，请参阅 [C# 示例](#example)。

# <a name="c-script"></a>[C# 脚本](#tab/csharp-script)

C# 脚本不支持特性。

# <a name="javascript"></a>[JavaScript](#tab/javascript)

JavaScript 不支持特性。

# <a name="java"></a>[Java](#tab/java)

使用 [SendGridOutput](https://github.com/Azure/azure-functions-java-library/blob/master/src/main/java/com/microsoft/azure/functions/annotation/SendGridOutput.java) 注释，可以通过提供配置值以声明方式配置 SendGrid 绑定。 有关更多详细信息，请参阅[示例](#example)和[配置](#configuration)部分。

---

## <a name="configuration"></a>配置

下表列出了在 function.json 文件和 `SendGrid` 特性/注释中可用的绑定配置属性。

| function.json 属性  | 特性/注释属性 | 说明 | 可选 |
|--------------------------|-------------------------------|-------------|----------|
| type |不适用| 必须设置为 `sendGrid`。| 否 |
| direction |不适用| 必须设置为 `out`。| 否 |
| name |不适用| 在请求或请求正文的函数代码中使用的变量名称。 只有一个返回值时，此值为 `$return`。 | 否 |
| apiKey | ApiKey | 包含 API 密钥的应用设置的名称。 如果未设置，默认应用设置名称为“AzureWebJobsSendGridApiKey”  。| 否 |
| to| 目标 | 收件人的电子邮件地址。 | 是 |
| 从| 源 | 发件人的电子邮件地址。 |  是 |
| subject| 主题 | 电子邮件主题。 | 是 |
| text| 文本 | 电子邮件内容。 | 是 |

在绑定中可能会定义可选属性的默认值，并以编程方式添加或重写这些值。

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

<a name="host-json"></a>  

## <a name="hostjson-settings"></a>host.json 设置

本部分介绍版本 2.x 及更高版本中可用于此绑定的全局配置设置。 下面的示例 host.json 文件仅包含此绑定的 2.x 版及更高版本设置。 若要详细了解 2.x 版及更高版本中的全局配置设置，请参阅 [Azure Functions 的 host.json 参考](functions-host-json.md)。

> [!NOTE]
> 有关 Functions 1.x 中 host.json 的参考，请参阅 [Azure Functions 1.x 的 host.json 参考](functions-host-json-v1.md)。

```json
{
    "version": "2.0",
    "extensions": {
        "sendGrid": {
            "from": "Azure Functions <samples@functions.com>"
        }
    }
}
```  

|properties  |默认 | 说明 |
|---------|---------|---------| 
|从|不适用|所有函数的发件人电子邮件地址。| 


## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [详细了解 Azure Functions 触发器和绑定](functions-triggers-bindings.md)

<!-- Update_Description: wording update -->