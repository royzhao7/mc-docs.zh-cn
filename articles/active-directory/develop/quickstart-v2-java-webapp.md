---
title: Microsoft 标识平台 Java Web 应用快速入门 | Azure
description: 了解如何使用 OpenID Connect 在 Java Web 应用中实现 Microsoft 登录
services: active-directory
author: sangonzal
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: quickstart
ms.workload: identity
ms.date: 08/18/2020
ms.author: v-junlch
ms.custom: aaddev, scenarios:getting-started, languages:Java, devx-track-java
ms.openlocfilehash: 889e6d44c58486297b0b096d80f28a24c09dc4f3
ms.sourcegitcommit: 7646936d018c4392e1c138d7e541681c4dfd9041
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/20/2020
ms.locfileid: "88647609"
---
# <a name="quickstart-add-sign-in-with-microsoft-to-a-java-web-app"></a>快速入门：向 Java Web 应用添加 Microsoft 登录功能

本快速入门介绍如何将 Java Web 应用与 Microsoft 标识平台集成。 应用会将用户登录，获取用于调用 Microsoft Graph API 的访问令牌，并针对 Microsoft Graph API 发出请求。

完成本快速入门后，该应用程序将接受任何公司或组织中使用 Azure Active Directory 的工作或学校帐户进行登录。 （有关说明，请参阅[示例工作原理](#how-the-sample-works)。）

## <a name="prerequisites"></a>先决条件

若要运行此示例，需要：

- [Java 开发工具包 (JDK)](https://openjdk.java.net/) 8 或更高版本以及 [Maven](https://maven.apache.org/)。

> [!div renderon="docs"]
> ## <a name="register-and-download-your-quickstart-app"></a>注册并下载快速入门应用
> 可以使用两个选项来启动快速入门应用程序：“快速”（选项 1）和“手动”（选项 2）
>
> ### <a name="option-1-register-and-auto-configure-your-app-and-then-download-your-code-sample"></a>选项 1：注册并自动配置应用，然后下载代码示例
>
> 1. 转到 [Azure 门户 - 应用注册](https://portal.azure.cn/#blade/Microsoft_AAD_RegisteredApps/applicationsListBlade/quickStartType/JavaQuickstartPage/sourceType/docs)快速入门体验。
> 1. 输入应用程序的名称并选择“注册”。
> 1. 按照门户快速入门体验中的说明下载自动配置的应用程序代码。
>
> ### <a name="option-2-register-and-manually-configure-your-application-and-code-sample"></a>选项 2：注册并手动配置应用程序和代码示例
>
> #### <a name="step-1-register-your-application"></a>步骤 1：注册应用程序
>
> 若要注册应用程序并将应用的注册信息手动添加到应用程序，请执行以下步骤：
>
> 1. 使用工作或学校帐户登录到 [Azure 门户](https://portal.azure.cn)。
> 1. 如果你的帐户有权访问多个租户，请在右上角选择该帐户，并将门户会话设置为所需的 Azure AD 租户。
>
> 1. 导航到面向开发人员的 Microsoft 标识平台的[应用注册](https://portal.azure.cn/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredAppsPreview)页。
> 1. 选择“新注册”。
> 1. “注册应用程序”页出现后，请输入应用程序的注册信息：
>    - 在“名称”部分输入一个会显示给应用用户的有意义的应用程序名称，例如 `java-webapp`。
>    - 选择“注册”。
> 1. 在“概述”页上，找到应用程序的“应用程序(客户端) ID”和“目录(租户) ID”值。 复制这些值，供以后使用。
> 1. 从菜单选择“身份验证”，然后添加以下信息：
>    - 添加“Web”平台配置。  将这些 `https://localhost:8443/msal4jsample/secure/aad` 和 `https://localhost:8443/msal4jsample/graph/me` 添加为“重定向 URI”。
>    - 选择“保存”。
> 1. 在菜单中选择“证书和密码”，然后在“客户端密码”部分中单击“新建客户端密码”：  
>
>    - 键入（实例应用密码的）密钥说明。
>    - 选择密钥持续时间“1 年”。
>    - 选择“添加”时，将显示密钥值。
>    - 复制密钥的值供以后使用。 该密钥值将不会再次显示，也无法通过其他任何方式检索，因此，该值在 Azure 门户中显示时，请立即记下来。
>
> [!div class="sxs-lookup" renderon="portal"]
> #### <a name="step-1-configure-your-application-in-the-azure-portal"></a>步骤 1：在 Azure 门户中配置应用程序
>
> 若要正常运行本快速入门中的代码示例，需要：
>
> 1. 添加 `https://localhost:8443/msal4jsample/secure/aad` 和 `https://localhost:8443/msal4jsample/graph/me` 作为回复 URL
> 1. 创建客户端机密。
> > [!div renderon="portal" id="makechanges" class="nextstepaction"]
> > [为我进行这些更改]()
>
> > [!div id="appconfigured" class="alert alert-info"]
> > ![已配置](./media/quickstart-v2-aspnet-webapp/green-check.png) 应用程序已使用这些属性进行配置。

#### <a name="step-2-download-the-code-sample"></a>步骤 2：下载代码示例
> [!div renderon="docs"]
> [下载代码示例](https://github.com/Azure-Samples/ms-identity-java-webapp/archive/master.zip)

> [!div class="sxs-lookup" renderon="portal"]
> 下载项目并将 zip 文件解压缩到更靠近根文件夹的本地文件夹（例如，**C:\Azure-Samples**）
>
> 若要将 https 与 localhost 一起使用，请填写 server.ssl.key 属性。 若要生成自签名证书，请使用 keytool 实用工具（包含在 JRE 中）。
>
>  ```
>   Example:
>   keytool -genkeypair -alias testCert -keyalg RSA -storetype PKCS12 -keystore keystore.p12 -storepass password
>
>   server.ssl.key-store-type=PKCS12
>   server.ssl.key-store=classpath:keystore.p12
>   server.ssl.key-store-password=password
>   server.ssl.key-alias=testCert
>   ```
>   将生成的 keystore 文件放在“resources”文件夹中。

> [!div renderon="portal" id="autoupdate" class="nextstepaction"]
> [下载代码示例](https://github.com/Azure-Samples/ms-identity-java-webapp/archive/master.zip)

> [!div class="sxs-lookup" renderon="portal"]
> > [!NOTE]
> > `Enter_the_Supported_Account_Info_Here`

> [!div renderon="docs"]
> #### <a name="step-3-configure-the-code-sample"></a>步骤 3：配置代码示例
> 1. 将 zip 文件解压缩到某个本地文件夹。
> 1. 如果使用集成开发环境，请在偏好的 IDE 中打开示例（可选）。
> 1. 打开可在 src/main/resources/ 文件夹中找到的 application.properties 文件，将字段“aad.clientId”、“aad.authority”和“aad.secretKey”的值替换为相应的“应用程序 ID”、“租户 ID”和“客户端密码”值，如下所示    ：
>
>    ```file
>    aad.clientId=Enter_the_Application_Id_here
>    aad.authority=https://login.partner.microsoftonline.cn/Enter_the_Tenant_Info_Here/
>    aad.secretKey=Enter_the_Client_Secret_Here
>    aad.redirectUriSignin=https://localhost:8443/msal4jsample/secure/aad
>    aad.redirectUriGraph=https://localhost:8443/msal4jsample/graph/me
>    aad.msGraphEndpointHost="https://microsoftgraph.chinacloudapi.cn/"
>    ```
> 其中：
>
> - `Enter_the_Application_Id_here` - 是已注册应用程序的应用程序 ID。
> - `Enter_the_Client_Secret_Here` - 是你在“证书和机密”中为注册的应用程序创建的**客户端密码**。
> - `Enter_the_Tenant_Info_Here` - 是注册的应用程序的目录（租户）ID 值。
> 1. 若要将 https 与 localhost 一起使用，请填写 server.ssl.key 属性。 若要生成自签名证书，请使用 keytool 实用工具（包含在 JRE 中）。
>
>  ```
>   Example:
>   keytool -genkeypair -alias testCert -keyalg RSA -storetype PKCS12 -keystore keystore.p12 -storepass password
>
>   server.ssl.key-store-type=PKCS12
>   server.ssl.key-store=classpath:keystore.p12
>   server.ssl.key-store-password=password
>   server.ssl.key-alias=testCert
>   ```
>   将生成的 keystore 文件放在“resources”文件夹中。


> [!div class="sxs-lookup" renderon="portal"]
> #### <a name="step-3-run-the-code-sample"></a>步骤 3：运行代码示例
> [!div renderon="docs"]
> #### <a name="step-4-run-the-code-sample"></a>步骤 4：运行代码示例

若要运行该项目，可以：

使用嵌入的 spring boot 服务器直接从 IDE 中运行它，或使用 [maven](https://maven.apache.org/plugins/maven-war-plugin/usage.html) 将其打包到 WAR 文件，然后将其部署到 J2EE 容器解决方案，如 [Apache Tomcat](http://tomcat.apache.org/)。

##### <a name="running-from-ide"></a>从 IDE 运行

如果从 IDE 运行 Web 应用程序，请单击“运行”，然后导航到项目的主页。 对于本示例，标准主页 URL 为 https://localhost:8443

1. 在前面的页面上，选择“登录”按钮以重定向到 Azure Active Directory 并提示用户输入其凭据。

1. 用户通过身份验证后，会重定向到 https://localhost:8443/msal4jsample/secure/aad。 他们现在已登录，页面将显示有关已登录帐户的信息。 示例 UI 包含以下按钮：
    - *注销*：从应用程序中注销当前用户，并将其重定向到主页。
    - *显示用户信息*：获取 Microsoft Graph 的令牌，并使用包含该令牌的请求调用 Microsoft Graph，这将返回有关已登录用户的基本信息。

##### <a name="running-from-tomcat"></a>从 Tomcat 运行

若要将 Web 示例部署到 Tomcat，需要对源代码进行一些更改。

1. 打开 ms-identity-java-webapp/pom.xml
    - 在 `<name>msal-web-sample</name>` 下添加 `<packaging>war</packaging>`

2. 打开 ms-identity-java-webapp/src/main/java/com.microsoft.azure.msalwebsample/MsalWebSampleApplication

    - 删除所有源代码，将其替换为以下内容：

   ```Java
    package com.microsoft.azure.msalwebsample;

    import org.springframework.boot.SpringApplication;
    import org.springframework.boot.autoconfigure.SpringBootApplication;
    import org.springframework.boot.builder.SpringApplicationBuilder;
    import org.springframework.boot.web.servlet.support.SpringBootServletInitializer;

    @SpringBootApplication
    public class MsalWebSampleApplication extends SpringBootServletInitializer {

     public static void main(String[] args) {
      SpringApplication.run(MsalWebSampleApplication.class, args);
     }

     @Override
     protected SpringApplicationBuilder configure(SpringApplicationBuilder builder) {
      return builder.sources(MsalWebSampleApplication.class);
     }
    }
   ```

3.   Tomcat 的默认 HTTP 端口为 8080，但需通过端口 8443 进行 HTTPS 连接。 对此进行配置：
        - 转到 tomcat/conf/server.xml
        - 搜索 `<connector>` 标记，并将现有连接器替换为：
        ```
        <Connector
                   protocol="org.apache.coyote.http11.Http11NioProtocol"
                   port="8443" maxThreads="200"
                   scheme="https" secure="true" SSLEnabled="true"
                   keystoreFile="C:/Path/To/Keystore/File/keystore.p12" keystorePass="KeystorePassword"
                   clientAuth="false" sslProtocol="TLS"/>
        ``` 
       
4. 打开命令提示符，转到此示例的根文件夹（pom.xml 位于其中），然后运行 `mvn package` 以生成项目
    - 这会在 /targets 目录中生成一个 `msal-web-sample-0.1.0.war` 文件。
    - 将此文件重命名为 `msal4jsample.war`
    - 使用 Tomcat 或任何其他 J2EE 容器解决方案部署此 war 文件。
        - 若要部署，请将 msal4jsample.war 文件复制到 Tomcat 安装中的 `/webapps/` 目录，然后启动 Tomcat 服务器。

5. 部署后，在浏览器中转到 https://localhost:8443/msal4jsample


> [!IMPORTANT]
> 本快速入门应用程序使用客户端机密将自己标识为机密客户端。 由于客户端机密是以纯文本形式添加到项目文件的，因此为了安全起见，建议在考虑将应用程序用作生产应用程序之前，使用证书来代替客户端机密。 有关如何使用证书的详细信息，请参阅[用于应用程序身份验证的证书凭据](./active-directory-certificate-credentials.md)。

## <a name="more-information"></a>详细信息

### <a name="how-the-sample-works"></a>示例工作原理
![显示本快速入门生成的示例应用的工作原理](./media/quickstart-v2-java-webapp/java-quickstart.svg)

### <a name="getting-msal"></a>获取 MSAL

MSAL for Java (MSAL4J) 是一个 Java 库，用于用户登录和请求令牌，此类令牌用于访问受 Microsoft 标识平台保护的 API。

可以使用 Maven 或 Gradle 将 MSAL4J 添加到应用程序，以通过对应用程序的 pom.xml (Maven) 或 build.gradle (Gradle) 文件进行以下更改来管理依赖项。

在 pom.xml 中：

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>msal4j</artifactId>
    <version>1.0.0</version>
</dependency>
```

在 build.gradle 中：

```$xslt
compile group: 'com.microsoft.azure', name: 'msal4j', version: '1.0.0'
```

### <a name="msal-initialization"></a>MSAL 初始化

通过将以下代码添加到要在其中使用 MSAL4J 的文件的顶部，来添加对 MSAL for Java 的引用：

```Java
import com.microsoft.aad.msal4j.*;
```

## <a name="next-steps"></a>后续步骤

了解有关权限和许可的详细信息：

> [!div class="nextstepaction"]
> [权限和许可](./v2-permissions-and-consent.md)

若要详细了解此方案的授权流，请查看 Oauth 2.0 授权代码流：

> [!div class="nextstepaction"]
> [授权代码 Oauth 流](./v2-oauth2-auth-code-flow.md)

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]

