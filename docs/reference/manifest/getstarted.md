---
title: 清单文件中的 GetStarted 元素
description: 提供在 Word、Excel、PowerPoint 和 OneNote 中安装外接程序时显示的标注所使用的信息。
ms.date: 10/09/2018
localization_priority: Normal
ms.openlocfilehash: 01b10b8316c87b046cf816d6f86551bf1a349267
ms.sourcegitcommit: 9609bd5b4982cdaa2ea7637709a78a45835ffb19
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/28/2020
ms.locfileid: "47292291"
---
# <a name="getstarted-element"></a>GetStarted 元素

提供在 Word、Excel、PowerPoint 和 OneNote 中安装外接程序时显示的标注所使用的信息。 **GetStarted**元素是[DesktopFormFactor](desktopformfactor.md)的子元素。

## <a name="child-elements"></a>子元素

| 元素                       | 必需 | 说明                                        |
|:------------------------------|:--------:|:---------------------------------------------------|
| [标题](#title)               | 是      | 定义外接程序公开功能的位置。     |
| [说明](#description)   | 是      | 包含 JavaScript 函数的文件的 URL。|
| [LearnMoreUrl](#learnmoreurl) | 是       | 指向详细说明外接程序的页面的 URL。   |

### <a name="title"></a>标题 

必需。 用于标注顶部的标题。 **resid** 属性引用 **Resources** 分区的 [ShortStrings](resources.md) 元素中的有效 ID。

### <a name="description"></a>说明

必需。 标注的说明/正文内容。 **resid** 属性引用 **Resources** 分区的 [LongStrings](resources.md) 元素中的有效 ID。

### <a name="learnmoreurl"></a>LearnMoreUrl

必需。指向用户可以了解你的外接程序详细信息的页面 URL。**resid** 属性引用 [Resources](resources.md) 分区的 **Urls** 元素中的有效 ID。

> [!NOTE]
> **LearnMoreUrl** 当前无法在 Word、Excel 或 PowerPoint 客户端中呈现。 我们建议为所有客户端添加此 URL，以便 URL 在可用时呈现。 

## <a name="see-also"></a>另请参阅

下面的代码示例使用 **GetStarted** 元素：

* [用于控制表和图表格式化的 Excel Web 外接程序](https://github.com/OfficeDev/Excel-Add-in-JavaScript-SalesTracker)
* [Word 外接程序 JavaScript SpecKit](https://github.com/OfficeDev/Word-Add-in-JS-SpecKit)
* [在 PowerPoint 加载项中使用 Microsoft Graph 插入 Excel 图表](https://github.com/OfficeDev/PowerPoint-Add-in-Microsoft-Graph-ASPNET-InsertChart)
