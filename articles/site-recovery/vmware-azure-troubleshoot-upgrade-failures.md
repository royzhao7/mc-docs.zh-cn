---
title: 排查 Azure Site Recovery 提供程序升级问题
description: 解决升级 Azure Site Recovery 提供程序时出现的常见问题。
author: rockboyfor
manager: digimobile
ms.service: site-recovery
ms.topic: troubleshooting
origin.date: 11/10/2019
ms.date: 08/03/2020
ms.testscope: no
ms.testdate: 02/24/2020
ms.author: v-yeche
ms.openlocfilehash: d9114dfe7642a4fff7dbc3eb5d5d21f2c0ee5594
ms.sourcegitcommit: 692b9bad6d8e4d3a8e81c73c49c8cf921e1955e7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/30/2020
ms.locfileid: "87426495"
---
# <a name="troubleshoot-azure-site-recovery-provider-upgrade-failures"></a>排查 Azure Site Recovery 提供程序升级故障

本文帮助你解决在 Azure Site Recovery 提供程序升级期间可能导致失败的问题。

## <a name="the-upgrade-fails-reporting-that-the-latest-site-recovery-provider-is-already-installed"></a>升级失败并报告已安装最新的 Site Recovery 提供程序

升级 Azure Site Recovery 提供程序 (DRA) 时，统一安装程序升级失败并发出错误消息：

由于已安装该软件的更高版本，因此不支持升级。

若要升级，请使用以下步骤：

1. 下载 Azure Site Recovery 统一安装程序：
    1. 在 [Azure Site Recovery 中的服务更新](service-updates-how-to.md#links-to-currently-supported-update-rollups)一文的“当前支持的更新汇总链接”部分，选择要升级到的提供程序。
    2. 在汇总页上找到“更新信息”部分，并下载 Azure Site Recovery 统一安装的更新汇总。 

2. 打开命令提示符并导航到下载的统一安装程序文件所在的文件夹。 使用以下命令从下载内容中提取安装程序文件：MicrosoftAzureSiteRecoveryUnifiedSetup.exe /q /x:&lt;提取的文件所在的文件夹路径&gt;。

    示例命令：

    MicrosoftAzureSiteRecoveryUnifiedSetup.exe /q /x:C:\Temp\Extracted

3. 在命令提示符下，导航到文件所提取到的文件夹，并运行以下安装命令：

    CX_THIRDPARTY_SETUP.EXE /VERYSILENT /SUPPRESSMSGBOXES /NORESTART  UCX_SERVER_SETUP.EXE /VERYSILENT /SUPPRESSMSGBOXES /NORESTART /UPGRADE

1. 返回到下载的统一安装程序所在的文件夹，并运行 MicrosoftAzureSiteRecoveryUnifiedSetup.exe 以完成升级。 

## <a name="upgrade-failure-due-to-the-3rd-party-folder-being-renamed"></a>由于重命名第三方文件夹而导致升级失败

若要成功升级，不能重命名第三方文件夹。

解决问题。

1. 启动注册表编辑器 (regedit.exe) 并打开 HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\InMage Systems\Installed Products\10 分支。
1. 检查 `Build_Version` 项值。 如果此项设置为最新版本，请减小版本号。 例如，如果最新版本为 9.22.\*，而 `Build_Version` 项设置为该值，请将它减小为 9.21.\*。
1. 下载最新的 Azure Site Recovery 统一安装程序：
    1. 在 [Azure Site Recovery 中的服务更新](service-updates-how-to.md#links-to-currently-supported-update-rollups)一文的“当前支持的更新汇总链接”部分，选择要升级到的提供程序。
    2. 在汇总页上找到“更新信息”部分，并下载 Azure Site Recovery 统一安装的更新汇总。 
1. 打开命令提示符，导航到下载的统一安装程序文件所在的文件夹，然后使用以下命令从下载内容中提取安装程序文件：MicrosoftAzureSiteRecoveryUnifiedSetup.exe /q /x:&lt;提取的文件所在的文件夹路径&gt;。

    示例命令：

    MicrosoftAzureSiteRecoveryUnifiedSetup.exe /q /x:C:\Temp\Extracted

1. 在命令提示符下，导航到文件所提取到的文件夹，并运行以下安装命令：

    CX_THIRDPARTY_SETUP.EXE /VERYSILENT /SUPPRESSMSGBOXES /NORESTART

1. 使用任务管理器监视安装进度。 当 CX_THIRDPARTY_SETUP.EXE 的进程不再显示在任务管理器中时，请转到下一步。
1. 验证 C:\thirdparty 是否存在，以及该文件夹是否包含 RRD 库。
1. 返回到下载的统一安装程序所在的文件夹，并运行 MicrosoftAzureSiteRecoveryUnifiedSetup.exe 以完成升级。

## <a name="upgrade-failure-due-to-master-target-installation-failure"></a>主目标安装失败导致升级失败

升级 Azure Site Recovery 提供程序 (DRA) 时，主目标安装失败，并出现错误“安装位置不存在且/或其上可用空间不足 1 GB 且/或它不存在于固定驱动器上。”。

这可能是由于注册表项中某个参数的值为 null 导致的。 若要解决问题，请执行以下操作 -

1. 启动注册表编辑器 (regedit.exe) 并打开 HKEY_LOCAL_MACHINE\SOFTWARE\Wow6432Node\InMage Systems\Installed Products\4 分支。
1. 检查“InstallDirectory”键值。 如果它为 null，则添加当前安装目录值。
1. 类似地，在注册表编辑器中打开 HKEY_LOCAL_MACHINE\SOFTWARE\Wow6432Node\InMage Systems\Installed Products\5 分支。
1. 检查“InstallDirectory”键值并添加当前安装目录值。
1. 再次运行“统一安装程序”安装程序。

<!-- Update_Description: update meta properties, wording update, update link -->