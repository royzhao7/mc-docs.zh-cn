---
title: include 文件
description: include 文件
services: virtual-machines
author: rockboyfor
ms.service: virtual-machines
ms.topic: include
origin.date: 04/25/2019
ms.date: 05/20/2019
ms.author: v-yeche
ms.custom: include file
ms.openlocfilehash: 1775d5d403768a839190c1ce45dd99dc01a5543f
ms.sourcegitcommit: 981a75a78f8cf74ab5a76f9e6b0dc5978387be4b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/22/2020
ms.locfileid: "83801275"
---
<!--Verified successfully-->
## <a name="update-resources"></a>更新资源

对于能够更新的内容，存在一些限制。 以下项目可以更新： 

共享映像库：
- 说明

映像定义：
- 建议的 vCPU 数
- 建议的内存
- 说明
- 生命周期终结日期

映像版本：
- 区域副本计数
- 目标区域数
- 从最新项中排除
- 生命周期终结日期

如果打算添加副本区域，请勿删除源托管映像。 源托管映像是将映像版本复制到其他区域所需的。 

若要更新库的说明，请使用 [Update-AzGallery](https://docs.microsoft.com/powershell/module/az.compute/update-azgallery)。

```powershell
Update-AzGallery `
   -Name $gallery.Name ` 
   -ResourceGroupName $resourceGroup.Name
```

此示例演示如何使用 [Update-AzGalleryImageDefinition](https://docs.microsoft.com/powershell/module/az.compute/update-azgalleryimagedefinition) 来更新映像定义的生命周期结束日期。

```powershell
Update-AzGalleryImageDefinition `
   -GalleryName $gallery.Name `
   -Name $galleryImage.Name `
   -ResourceGroupName $resourceGroup.Name `
   -EndOfLifeDate 01/01/2030
```

此示例演示如何使用 [Update-AzGalleryImageVersion](https://docs.microsoft.com/powershell/module/az.compute/update-azgalleryimageversion) 来排除此映像版本，使之不能用作最新映像。 

```powershell
Update-AzGalleryImageVersion `
   -GalleryImageDefinitionName $galleryImage.Name `
   -GalleryName $gallery.Name `
   -Name $galleryVersion.Name `
   -ResourceGroupName $resourceGroup.Name `
   -PublishingProfileExcludeFromLatest
```

## <a name="clean-up-resources"></a>清理资源

删除资源时，需从嵌套资源中的最后一项（映像版本）开始。 删除版本以后，即可删除映像定义。 必须先删除库下的所有资源，然后才能删除库。

```powershell
$resourceGroup = "myResourceGroup"
$gallery = "myGallery"
$imageDefinition = "myImageDefinition"
$imageVersion = "myImageVersion"

Remove-AzGalleryImageVersion `
   -GalleryImageDefinitionName $imageDefinition `
   -GalleryName $gallery `
   -Name $imageVersion `
   -ResourceGroupName $resourceGroup

Remove-AzGalleryImageDefinition `
   -ResourceGroupName $resourceGroup `
   -GalleryName $gallery `
   -GalleryImageDefinitionName $imageDefinition

Remove-AzGallery `
   -Name $gallery `
   -ResourceGroupName $resourceGroup

Remove-AzResourceGroup -Name $resourceGroup
```