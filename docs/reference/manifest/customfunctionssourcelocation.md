---
title: 清单文件中的自定义函数的 SourceLocation 元素
description: 定义 Excel 中自定义函数所使用的 Script 或 Page 元素所需的资源的位置。
ms.date: 08/07/2020
localization_priority: Normal
ms.openlocfilehash: 1c509987b0ce7948a63fa8ad51f7cf9c84144c5f
ms.sourcegitcommit: cc6886b47c84ac37a3c957ff85dd0ed526ca5e43
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/12/2020
ms.locfileid: "46641380"
---
# <a name="sourcelocation-element-custom-functions"></a>SourceLocation 元素 (自定义函数) 

定义 Excel 中自定义函数所使用的 Script 或 Page 元素所需的资源的位置。

## <a name="attributes"></a>属性

| 属性 | 必需 | 说明                                                                          |
|-----------|----------|--------------------------------------------------------------------------------------|
| resid     | 是      | 清单的 &lt;Resources&gt; 部分中所定义的 URL 资源的名称。 |

## <a name="child-elements"></a>子元素

无

## <a name="example"></a>示例

```xml
<SourceLocation resid="pageURL"/>
```
