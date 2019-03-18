---
ms.date: 02/08/2019
description: 处理 Excel 自定义函数中的错误。
title: Excel 中自定义函数的错误处理（预览）
localization_priority: Priority
ms.openlocfilehash: 170da03331663d6779bed7bf0bf5a9b75b908b3f
ms.sourcegitcommit: 8fb60c3a31faedaea8b51b46238eb80c590a2491
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/14/2019
ms.locfileid: "30632693"
---
# <a name="error-handling-within-custom-functions"></a>自定义函数中的错误处理

在生成定义自定义函数的加载项时，请务必加入错误处理逻辑，以便解决运行时错误。 自定义函数的错误处理与 [Excel JavaScript API 的错误处理](excel-add-ins-error-handling.md)大致相同。

[!include[Excel custom functions note](../includes/excel-custom-functions-note.md)]

在以下代码示例中，`.catch` 将处理之前发生在代码中的任何错误。

```js
function getComment(commentID) {
  let url = "https://www.contoso.com/comments/" + x;

  return fetch(url)
    .then(function (data) {
      return data.json();
    })
    .then((json) => {
      return json.body;
    })
    .catch(function (error) {
      throw error;
    })
}
```

## <a name="see-also"></a>另请参阅

* [Excel 自定义函数教程](../tutorials/excel-tutorial-create-custom-functions.md)
* [自定义函数元数据](custom-functions-json.md)
* [Excel 自定义函数的运行时](custom-functions-runtime.md)
* [自定义函数最佳实践](custom-functions-best-practices.md)
* [自定义函数更改日志](custom-functions-changelog.md)