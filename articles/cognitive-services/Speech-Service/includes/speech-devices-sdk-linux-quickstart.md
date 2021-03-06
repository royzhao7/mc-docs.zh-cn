---
author: trevorbye
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
origin.date: 02/20/2020
ms.date: 09/02/2020
ms.author: v-tawe
ms.openlocfilehash: 0e6c489682fb5717b787264f6d6b22b75004fec5
ms.sourcegitcommit: 4db9853370c9d4c7e5d54f1e1cfadf40efcc12a6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/02/2020
ms.locfileid: "89317606"
---
本快速入门介绍如何使用适用于 Linux 的语音设备 SDK 来生成支持语音的产品。

该应用程序是使用语音 SDK 程序包和 Eclipse Java IDE (v4) 在 64 位 Linux（Ubuntu 16.04、Ubuntu 18.04、Debian 9、RHEL 7/8、CentOS 7/8）上构建的。 它在 64 位 Java 8 运行时环境 (JRE) 中运行。

本指南需要一个包含语音服务资源的 [Azure 认知服务](../get-started.md)帐户。 

[示例应用程序](https://aka.ms/sdsdk-download-JRE)的源代码随附在语音设备 SDK 中， 也可在 [GitHub 上获取](https://github.com/Azure-Samples/Cognitive-Services-Speech-Devices-SDK)。

## <a name="prerequisites"></a>先决条件

本快速入门需要：

* 操作系统：64 位 Linux（Ubuntu 16.04、Ubuntu 18.04、Debian 9、RHEL 7/8、CentOS 7/8）
* [Azure Kinect DK](https://azure.microsoft.com/services/kinect-dk/)
* [Eclipse Java IDE](https://www.eclipse.org/downloads/)
* 仅限 [Java 8](https://www.oracle.com/technetwork/java/javase/downloads/jre8-downloads-2133155.html) 或 [JDK 8](https://www.oracle.com/technetwork/java/javase/downloads/index.html)。
* 语音服务的 Azure 订阅密钥。 [获取一个试用版](../get-started.md)。
* 下载用于 Java 的[语音设备 SDK](https://aka.ms/sdsdk-download-JRE) 的最新版本，并将 .zip 提取到工作目录。
   > [!NOTE]
   > 本快速入门假设应用已解压缩到 /home/wcaltest/JRE-Sample-Release

确保在启动 Eclipse 之前安装这些依赖项。

* 在 Ubuntu 上：

  ```sh
  sudo apt-get update
  sudo apt-get install libssl1.0.0 libasound2
  ```

* 在 Debian 9 上：

  ```sh
  sudo apt-get update
  sudo apt-get install libssl1.0.2 libasound2
  ```

* 在 RHEL/CentOS 上：
  
  ```sh
  sudo yum update
  sudo yum install alsa-lib openssl
  ```

  > [!NOTE]
  > - 在 RHEL/CentOS 7 上，按照[如何为语音 SDK 配置 RHEL/CentOS 7](~/articles/cognitive-services/speech-service/how-to-configure-rhel-centos-7.md) 上的说明进行操作。
> - 在 RHEL/CentOS 8 上，按照[如何配置 OpenSSL for Linux](~/articles/cognitive-services/speech-service/how-to-configure-openssl-linux.md) 上的说明进行操作。

<!-- Conversation Transcription is currently only available for "en-US" and "zh-CN", in the "chinaeast2" regions. You must have a speech key in one of those regions to use Conversation Transcription. -->

如果计划使用意向，则将需要[语音理解服务 (LUIS)](https://docs.azure.cn/cognitive-services/luis/azureibizasubscription) 订阅。 若要了解有关 LUIS 和意向识别的详细信息，请参阅[使用 LUIS、C# 识别语音意向](https://docs.azure.cn/cognitive-services/speech-service/how-to-recognize-intents-from-speech-csharp)。 [示例 LUIS 模型](https://aka.ms/sdsdk-luis)适用于此应用。

## <a name="create-and-configure-the-project"></a>创建和配置项目

1. 启动 Eclipse。

1. 在 Eclipse IDE Launcher 的“工作区”字段中，输入新工作区目录的名称 。 然后选择“启动”。

   ![Eclipse Launcher 的屏幕截图](../media/speech-devices-sdk/eclipse-launcher-linux.png)

1. 片刻之后，Eclipse IDE 的主窗口将会显示。 如果出现了欢迎屏幕，请将其关闭。

1. 从 Eclipse 菜单栏上，依次选择“文件” > “新建” > “Java 项目”以新建一个项目。 如果不可用，请依次选择“项目”和“Java 项目”。

1. 此时将启动“新建 Java 项目”向导。 **浏览**示例项目所在的位置。 选择“完成”。

   ![“新建 Java 项目”向导的屏幕截图](../media/speech-devices-sdk/eclipse-new-java-project-linux.png)

1. 在“包资源管理器”中，右键单击你的项目。 从上下文菜单中选择“配置” > “转换为 Maven 项目”。 选择“完成”。

   ![包资源管理器的屏幕截图](../media/speech-devices-sdk/eclipse-convert-to-maven.png)

1. 打开 pom.xml 文件并对其进行编辑。

    在文件末尾，在结束标记 `</project>` 之前，创建 `repositories` 和 `dependencies` 元素（如此处所示），并确保 `version` 与当前版本匹配：
    ```xml    
    <repositories>
         <repository>
             <id>maven-cognitiveservices-speech</id>
             <name>Microsoft Cognitive Services Speech Maven Repository</name>
             <url>https://csspeechstorage.blob.core.windows.net/maven/</url>
         </repository>
    </repositories>
 
    <dependencies>
        <dependency>
             <groupId>com.microsoft.cognitiveservices.speech</groupId>
             <artifactId>client-sdk</artifactId>
             <version>1.13.0</version>
        </dependency>
    </dependencies>
   ```

1. 在“包资源管理器”中，右键单击你的项目。 选择“属性”，然后依次选择“运行/调试设置” > “新建...” > **Java 应用程序**。 

1. 此时将显示“编辑配置”窗口。 在“名称”字段中，输入 **Main**，并对“Main 类”使用“搜索” ，以查找和选择 **com.microsoft.cognitiveservices.speech.samples.FunctionsList**。

   ![“编辑启动配置”的屏幕截图](../media/speech-devices-sdk/eclipse-edit-launch-configuration-linux.png)

1. 从 **Linux-arm** 或 **Linux-x64** 中将目标体系结构的音频二进制文件复制到 Java 项目位置，例如 /home/wcaltest/JRE-Sample-Release

1. 同样从“编辑配置”窗口中，选择“环境”页和“新建”。 此时将显示“新环境变量”窗口。 在“名称”字段中输入 **LD_LIBRARY_PATH**，并在“值”字段中输入包含 *.so 文件的文件夹，例如 **/home/wcaltest/JRE-Sample-Release**

1. 将 `kws.table` 和 `participants.properties` 复制到项目文件夹 **target/classes**


## <a name="configure-the-sample-application"></a>配置示例应用程序

1. 将语音订阅密钥添加到源代码。 如果想要尝试意向识别，还需要添加[语言理解服务](https://azure.microsoft.com/services/cognitive-services/language-understanding-intelligent-service/)订阅密钥和应用程序 ID。

   对于语音和 LUIS，你的信息将进入 `FunctionsList.java`：

   ```java
    // Subscription
    private static String SpeechSubscriptionKey = "<enter your subscription info here>";
    private static String SpeechRegion = "chinaeast2"; // You can change this if your speech region is different.
    private static String LuisSubscriptionKey = "<enter your subscription info here>";
    private static String LuisRegion = "chinaeast2"; // you can change this, if you want to test the intent, and your LUIS region is different.
    private static String LuisAppId = "<enter your LUIS AppId>";
   ```

    <!-- If you are using conversation transcription, your speech key and region information are also needed in `Cts.java`:

   ```java
    private static final String CTSKey = "<Conversation Transcription Service Key>";
    private static final String CTSRegion="<Conversation Transcription Service Region>";// Region may be "chinaeast2"
    ``` -->

1. 默认关键字为“Computer”。 还可以尝试所提供的其他关键字之一，例如“Machine”或“Assistant”。 这些备用关键字的资源文件位于语音设备 SDK 的 keyword 文件夹中。 例如，`/home/wcaltest/JRE-Sample-Release/keyword/Computer` 包含用于关键字“Computer”的文件。

   <!-- > [!TIP]
   > You can also [create a custom keyword](../speech-devices-sdk-create-kws.md). -->

    要使用新的关键字，请更新 `FunctionsList.java` 中的下面一行，并将关键字复制到应用。 例如，要使用关键字包 `machine.zip` 中的关键字“Machine”，请执行以下操作：

   * 将 `kws.table` 文件从 zip 包复制到项目文件夹“target/classes”中。

   * 使用关键字名称更新 `FunctionsList.java`：

     ```java
     private static final String Keyword = "Machine";
     ```

## <a name="run-the-sample-application-from-eclipse"></a>从 Eclipse 运行示例应用程序

1. 从 Eclipse 菜单栏中，依次选择“运行” > “运行” 

1. 语音设备 SDK 示例应用程序将会启动，并显示以下选项：

   ![示例语音设备 SDK 的示例应用程序和选项](../media/speech-devices-sdk/java-sample-app-linux.png)

   <!-- 1. Try the new **Conversation Transcription** demo. Start transcribing with **Session** > **Start**. By default everyone is a guest. However, if you have participant's voice signatures they can be put into `participants.properties` in the project folder **target/classes**. To generate the voice signature, look at [Transcribe conversations (SDK)](../how-to-use-conversation-transcription-service.md).

   ![Demo Conversation Transcription application](../media/speech-devices-sdk/cts-sample-app-linux.png) -->

## <a name="create-and-run-standalone-the-application"></a>创建并运行独立应用程序

1. 在“包资源管理器”中，右键单击你的项目。 选择“导出”。 
1. 此时将显示“导出”窗口。 展开“Java”，选择“可运行的 JAR 文件”，然后选择“下一步”。

   ![“导出”窗口的屏幕截图](../media/speech-devices-sdk/eclipse-export-linux.png) 

1. 此时将显示“可运行的 JAR 文件导出”窗口。 为应用程序选择“导出目标”，然后选择“完成”。
 
   ![“可运行的 JAR 文件导出”的屏幕截图](../media/speech-devices-sdk/eclipse-export-jar-linux.png)

1. 请将 `kws.table` 和 `participants.properties` 放入上面选择的目标文件夹中，因为该应用程序需要这些文件。

1. 将 LD_LIBRARY_LIB 设置为包含 *.so 文件的文件夹

     ```bash
     export LD_LIBRARY_PATH=/home/wcaltest/JRE-Sample-Release
     ```

1. 运行独立的应用程序

     ```bash
     java -jar SpeechDemo.jar
     ```
