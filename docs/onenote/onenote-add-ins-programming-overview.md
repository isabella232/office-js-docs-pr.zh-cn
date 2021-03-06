---
title: OneNote JavaScript API 编程概述
description: 了解有关适用于 OneNote 网页版加载项的 OneNote JavaScript API。
ms.date: 10/14/2020
ms.topic: conceptual
ms.custom: scenarios:getting-started
localization_priority: Priority
ms.openlocfilehash: e71535dce7892889a13e4546d8dd388f568ab5c4
ms.sourcegitcommit: 42e6cfe51d99d4f3f05a3245829d764b28c46bbb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/23/2020
ms.locfileid: "48741118"
---
# <a name="onenote-javascript-api-programming-overview"></a>OneNote JavaScript API 编程概述

OneNote 引入了适用于 OneNote 网页版加载项的 JavaScript API。 可以创建任务窗格加载项、内容加载项，以及与 OneNote 对象交互并连接到 Web 服务或其他基于 Web 的资源的加载项命令。

[!INCLUDE [publish policies note](../includes/note-publish-policies.md)]

## <a name="components-of-an-office-add-in"></a>Office 加载项的组件

加载项由两个基本部分组成：

- 包含网页和所有相应 JavaScript、CSS 或其他文件的 **Web 应用程序**。 这些文件托管在 Web 服务器或 Web 托管服务上，例如 Microsoft Azure。 在 OneNote 网页版中，Web 应用程序在浏览器控件或 iframe 中显示。

- **XML 清单**指定外接程序网页的 URL 和适用于外接程序的任何访问要求、设置和功能。此文件存储在客户端上。OneNote 外接程序使用与其他 Office 外接程序相同的 [清单](../develop/add-in-manifests.md)格式。

### <a name="office-add-in--manifest--webpage"></a>Office 加载项 = 清单 + 网页

![Office 加载项包含清单和网页](../images/onenote-add-in.png)

## <a name="using-the-javascript-api"></a>使用 JavaScript API

加载项使用 Office 应用程序的运行时上下文以访问 JavaScript API。API 有两层：

- 用于执行 OneNote 专属操作的**应用程序特定 API**，可通过 `Application` 对象访问。
- 跨 Office 应用程序分享的**通用 API**，通过 `Document` 对象访问。

### <a name="accessing-the-application-specific-api-through-the-application-object"></a>通过 *Application* 对象访问应用程序特定 API。

使用 `Application` 对象访问 OneNote 对象，如 **Notebook**、**Section** 和 **Page**。 通过应用程序特定 API，可在代理对象上运行批处理操作。 基本流程类似如下：

1. 从上下文中获取应用程序实例。

2. 创建您想要使用的表示 OneNote 对象的代理。通过读取和写入代理对象的属性和调用其方法，您可以与其同步交互。

3. 调用代理上的 `load` 以使用在参数中指定的属性值填充它。此调用将添加至命令队列中。

   > [!NOTE]
   > API 方法调用（如 `context.application.getActiveSection().pages;`）也会添加到队列中。

4. 调用 `context.sync` 以按它们已排队的顺序运行所有排队的命令。这将同步您正在运行的脚本和真实对象之间的状态，并通过检索已加载的用于您的脚本的 OneNote 对象的属性实现。您可以使用返回的 promise 对象以链接其他操作。

例如：

```js
function getPagesInSection() {
    OneNote.run(function (context) {

        // Get the pages in the current section.
        var pages = context.application.getActiveSection().pages;

        // Queue a command to load the id and title for each page.
        pages.load('id,title');

        // Run the queued commands, and return a promise to indicate task completion.
        return context.sync()
            .then(function () {

                // Read the id and title of each page.
                $.each(pages.items, function(index, page) {
                    var pageId = page.id;
                    var pageTitle = page.title;
                    console.log(pageTitle + ': ' + pageId);
                });
            })
            .catch(function (error) {
                app.showNotification("Error: " + error);
                console.log("Error: " + error);
                if (error instanceof OfficeExtension.Error) {
                    console.log("Debug info: " + JSON.stringify(error.debugInfo));
                }
            });
    });
}
```

有关详细信息，请参阅[使用特定于应用程序的 API 模型](../develop/application-specific-api-model.md)，了解 OneNote JavaScript API 中的 `load`/`sync` 模式以及其他常见做法。

可以在 [API 参考](../reference/overview/onenote-add-ins-javascript-reference.md) 中找到受支持的 OneNote 对象和操作。

#### <a name="onenote-javascript-api-requirement-sets"></a>OneNote JavaScript API 要求集

要求集是指各组已命名的 API 成员。 Office 加载项使用清单中指定的要求集或执行运行时检查，以确定 Office 应用程序是否支持加载项所需的 API。 有关 OneNote JavaScript API 要求集的详细信息，请参阅 [OneNote JavaScript API 要求集](../reference/requirement-sets/onenote-api-requirement-sets.md)。

### <a name="accessing-the-common-api-through-the-document-object"></a>通过 *Document* 对象访问通用 API

使用 `Document` 对象以访问通用 API，例如 [getSelectedDataAsync](/javascript/api/office/office.document#getselecteddataasync-coerciontype--options--callback-) 和 [setSelectedDataAsync](/javascript/api/office/office.document#setselecteddataasync-data--options--callback-) 方法。

例如：  

```js
function getSelectionFromPage() {
    Office.context.document.getSelectedDataAsync(
        Office.CoercionType.Text,
        { valueFormat: "unformatted" },
        function (asyncResult) {
            var error = asyncResult.error;
            if (asyncResult.status === Office.AsyncResultStatus.Failed) {
                console.log(error.message);
            }
            else $('#input').val(asyncResult.value);
        });
}
```

OneNote 加载项仅支持以下通用 API：

| API | 注释 |
|:------|:------|
| [Office.context.document.getSelectedDataAsync](/javascript/api/office/office.document#getselecteddataasync-coerciontype--options--callback-) | 仅 `Office.CoercionType.Text` 和 `Office.CoercionType.Matrix` |
| [Office.context.document.setSelectedDataAsync](/javascript/api/office/office.document#setselecteddataasync-data--options--callback-) | 仅 `Office.CoercionType.Text`、`Office.CoercionType.Image` 和 `Office.CoercionType.Html` | 
| [var mySetting = Office.context.document.settings.get(name);](/javascript/api/office/office.settings#get-name-) | 设置仅受内容外接程序支持 | 
| [Office.context.document.settings.set(name, value);](/javascript/api/office/office.settings#set-name--value-) | 设置仅受内容外接程序支持 | 
| [Office.EventType.DocumentSelectionChanged](/javascript/api/office/office.documentselectionchangedeventargs) ||

一般情况下，需要使用通用 API 执行应用程序特定 API 不支持的操作。 要详细了解如何使用通用 API，请参阅[常见 JavaScript API 对象模型](../develop/office-javascript-api-object-model.md)。

<a name="om-diagram"></a>
## <a name="onenote-object-model-diagram"></a>OneNote 对象模型图 
下图表示了 OneNote JavaScript API 中当前可用的内容。

  ![OneNote 对象模型图](../images/onenote-om.png)

## <a name="see-also"></a>另请参阅

- [开发 Office 加载项](../develop/develop-overview.md)
- [了解 Microsoft 365 开发人员计划](https://developer.microsoft.com/microsoft-365/dev-program)
- [生成首个 OneNote 加载项](../quickstarts/onenote-quickstart.md)
- [OneNote JavaScript API 参考](../reference/overview/onenote-add-ins-javascript-reference.md)
- [Rubric Grader 示例](https://github.com/OfficeDev/OneNote-Add-in-Rubric-Grader)
- [Office 加载项平台概述](../overview/office-add-ins.md)
