---
title: 清单文件中的 AllFormFactors 元素
description: 指定加载项的所有外观设置。
ms.date: 10/09/2018
localization_priority: Normal
ms.openlocfilehash: 9dac322312c1dfd60f6deb4296413e12b55a6a49
ms.sourcegitcommit: be23b68eb661015508797333915b44381dd29bdb
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/08/2020
ms.locfileid: "44608794"
---
# <a name="allformfactors-element"></a>AllFormFactors 元素

指定加载项的所有外观设置。 目前，使用 **AllFormFactors** 的唯一功能是自定义函数。 使用自定义函数时，**AllFormFactors** 是必备元素。

## <a name="child-elements"></a>子元素

|  元素 |  必需  |  Description  |
|:-----|:-----|:-----|
|  [ExtensionPoint](extensionpoint.md) |  是 |  定义加载项用于公开功能的位置。 |

## <a name="allformfactors-example"></a>AllFormFactors 示例

```xml
<Hosts>
    <Host xsi:type="Workbook">
        <AllFormFactors>
            <ExtensionPoint xsi:type="CustomFunctions">
                    <!-- Information on this extension point -->
            </ExtensionPoint>
        </AllFormFactors>
    </Host>
</Hosts>
```
