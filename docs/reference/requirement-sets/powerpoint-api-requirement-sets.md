---
title: PowerPoint JavaScript API 要求集
description: 了解有关 PowerPoint JavaScript API 要求集的详细信息。
ms.date: 10/26/2020
ms.prod: powerpoint
localization_priority: Priority
ms.openlocfilehash: cf9ab510e4b35a140c77ee958279cb85a2189fa2
ms.sourcegitcommit: a4e09546fd59579439025aca9cc58474b5ae7676
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2020
ms.locfileid: "48774725"
---
# <a name="powerpoint-javascript-api-requirement-sets"></a>PowerPoint JavaScript API 要求集

要求集是指各组已命名的 API 成员。Office 加载项使用清单中指定的要求集或执行运行时检查，以确定 Office 应用程序是否支持加载项所需的 API。有关详细信息，请参阅 [Office 版本和要求集](../../develop/office-versions-and-requirement-sets.md)。

下表列出了 PowerPoint 要求集、支持这些要求集的 Office 客户端应用程序，以及这些应用程序的内部版本或发布日期。

|  要求集  |  Windows 版 Office<br>（关联至 Microsoft 365 订阅）  |  iPad 版 Office<br>（关联至 Microsoft 365 订阅）  |  Mac 版 Office<br>（关联至 Microsoft 365 订阅）  | Office 网页版 |
|:-----|-----|:-----|:-----|:-----|:-----|
| [预览](powerpoint-preview-apis.md)  | 请使用最新的 Office 版本来试用预览 API（你可能需要加入 [Office 预览体验成员计划](https://insider.office.com)）。 |
| PowerPointApi 1.1 | 版本 1810（内部版本 11001.20074）或更高版本 | 2.17 或更高版本 | 16.19 或更高版本 | 2018 年 10 月 |

## <a name="office-versions-and-build-numbers"></a>Office 版本和内部版本号

有关 Office 版本和内部版本号的详细信息，请参阅：

[!INCLUDE [Links to get Office versions and how to find Office client version](../../includes/links-get-office-versions-builds.md)]

## <a name="powerpoint-javascript-api-11"></a>PowerPoint JavaScript API 1.1

PowerPoint JavaScript API 1.1 包含[用于创建新演示文稿的单一 API](/javascript/api/powerpoint#powerpoint-createpresentation-base64file-)。 有关 API 的详细信息，请参阅[创建演示文稿](../../powerpoint/powerpoint-add-ins.md#create-a-presentation)。

## <a name="how-to-use-powerpoint-requirement-sets-at-runtime-and-in-the-manifest"></a>如何在运行时和清单中使用 PowerPoint 要求集

> [!NOTE]
> 本节假定你熟悉 [Office 版本和要求集](../../develop/office-versions-and-requirement-sets.md)和[指定 Office 应用程序和 API 要求集](../../develop/specify-office-hosts-and-api-requirements.md)处的要求集概述。

要求集是指各组已命名的 API 成员。 Office 加载项可以执行运行时检查或使用清单中指定的要求集确定 Office 应用程序是否支持加载项所需的 API。

### <a name="checking-for-requirement-set-support-at-runtime"></a>在运行时检查要求集支持

以下代码示例显示如何确定运行加载项的 Office 应用程序是否支持指定的 API 要求集。

```js
if (Office.context.requirements.isSetSupported('PowerPointApi', '1.1')) {
  // Perform actions.
} else {
  // Provide alternate flow/logic.
}
```

### <a name="defining-requirement-set-support-in-the-manifest"></a>在清单中定义要求集支持

可以在加载项清单中使用[要求元素](../manifest/requirements.md)指定加载项要求激活的最小要求集和/或 API 方法。 如果 Office 应用程序或平台不支持清单的 `Requirements` 元素中指定的要求集或 API 方法，则加载项将不会在该应用程序或平台中运行，也不会出现在“ **我的加载项** ”中显示的加载项列表中。如果你的加载项需要特定要求集以实现完整功能，但是即使在不支持该要求集的平台上也可以为用户提供值，则建议在运行时按照上述方式检查要求支持，而不是在清单中定义要求集支持。

以下代码示例显示加载项清单中的 `Requirements` 元素，该元素指定应在支持 PowerPointApi 要求集版本 1.1 或更高版本的所有 Office 客户端应用程序中加载该加载项。

```xml
<Requirements>
   <Sets DefaultMinVersion="1.1">
      <Set Name="PowerPointApi" MinVersion="1.1"/>
   </Sets>
</Requirements>
```

## <a name="office-common-api-requirement-sets"></a>Office 通用 API 要求集

大多数 PowerPoint 加载项功能都来自通用 API 集。 若要了解通用 API 要求集，请参阅 [Office 通用 API 要求集](office-add-in-requirement-sets.md)。

## <a name="see-also"></a>另请参阅

- [PowerPoint JavaScript API 参考文档](/javascript/api/powerpoint)
- [Office 版本和要求集](../../develop/office-versions-and-requirement-sets.md)
- [指定 Office 应用程序和 API 要求](../../develop/specify-office-hosts-and-api-requirements.md)
- [Office 加载项 XML 清单](../../develop/add-in-manifests.md)
