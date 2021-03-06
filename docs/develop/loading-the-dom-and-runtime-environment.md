---
title: 加载 DOM 和运行时环境
description: 加载 DOM 和 Office 外接程序运行时环境
ms.date: 04/22/2020
localization_priority: Normal
ms.openlocfilehash: 02f950ca23d52b333f704c7d8aed431cb426a6f0
ms.sourcegitcommit: 9609bd5b4982cdaa2ea7637709a78a45835ffb19
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/28/2020
ms.locfileid: "47293273"
---
# <a name="loading-the-dom-and-runtime-environment"></a>加载 DOM 和运行时环境

外接程序在运行自己的自定义逻辑前必须确保 DOM 和 Office 外接程序运行时环境都已加载。

## <a name="startup-of-a-content-or-task-pane-add-in"></a>启动内容或任务窗格加载项

下图显示了在 Excel、PowerPoint、Project 或 Word 中启动内容或任务窗格加载项所涉及的事件流。

![启动内容/任务窗格外接程序时的事件流](../images/office15-app-sdk-loading-dom-agave-runtime.png)

启动内容/任务窗格外接程序时，将发生以下事件：

1. 用户打开已包含加载项的文档，或在文档中插入加载项。

2. Office 客户端应用程序从 AppSource、SharePoint 上的应用程序目录或其源于的共享文件夹目录中读取加载项的 XML 清单。

3. Office 客户端应用程序在浏览器控件中打开加载项的 HTML 页面。

    后面的两个步骤第 4 步和第 5 步以异步方式并行发生。因此，您的加载项代码必须在继续之前确保 DOM 和加载项运行时环境已加载完。

4. 浏览器控件加载 DOM 和 HTML 正文，并调用事件的事件处理程序 `window.onload` 。

5. Office 客户端应用程序加载运行时环境，该环境将从内容分发网络 (CDN) 服务器下载并缓存 Office JavaScript API 库文件，然后在向其分配处理程序的情况下调用[office](/javascript/api/office)对象的[initialize](/javascript/api/office#office-initialize-reason-)事件的加载项事件处理程序。 此时它还会检查是否有任何回调（或链接 `then()` 函数）已传递（或链接）到 `Office.onReady` 处理程序。 有关和的区别的详细信息 `Office.initialize` `Office.onReady` ，请参阅 [初始化外接程序](initialize-add-in.md)。

6. 当 DOM 和 HTML 正文加载完毕并且加载项完成初始化后，加载项的主函数就可以继续进行。


## <a name="startup-of-an-outlook-add-in"></a>启动 Outlook 外接程序

下图显示了启动在台式机、平板电脑或智能手机上运行的 Outlook 外接程序所涉及的事件流。

![启动 Outlook 外接程序时的事件流](../images/outlook15-loading-dom-agave-runtime.png)

启动 Outlook 外接程序时，将发生以下事件：

1. 当 Outlook 启动时，Outlook 读取已为用户的电子邮件帐户安装的 Outlook 外接程序的 XML 清单。

2. 用户选择 Outlook 中的一个项目。

3. 如果所选项目满足某个 Outlook 外接程序的激活条件，则 Outlook 将激活该外接程序，并使其按钮在 UI 中可见。

4. 如果用户单击该按钮以启动 Outlook 外接程序，Outlook 将在浏览器控件中打开 HTML 页面。下面两个步骤（步骤 5 和 6）并行发生。

5. 浏览器控件加载 DOM 和 HTML 正文，并调用事件的事件处理程序 `onload` 。

6. Outlook 加载运行时环境，这将从内容分发网络 (CDN) 服务器中为 JavaScript 库文件下载并缓存 JavaScript API，然后为 [Office](/javascript/api/office) 加载项对象的 [initialize](/javascript/api/office#office-initialize-reason-) 事件调用事件处理程序（如果已为其分配处理程序）。 此时它还会检查是否有任何回调（或链接 `then()` 函数）已传递（或链接）到 `Office.onReady` 处理程序。 有关和的区别的详细信息 `Office.initialize` `Office.onReady` ，请参阅 [初始化外接程序](initialize-add-in.md)。

7. 当 DOM 和 HTML 正文加载完毕并且加载项完成初始化后，加载项的主函数就可以继续进行。


## <a name="checking-the-load-status"></a>检查加载状态

检查 DOM 和运行时环境是否已完成加载的一种方法是使用 jQuery [.ready()](https://api.jquery.com/ready/) 函数：`$(document).ready()`。 例如，以下 `onReady` 事件处理程序确保在初始化外接程序的特定代码运行之前先加载 DOM。 随后， `onReady` 处理程序将继续使用 [邮箱. item](/javascript/api/outlook/office.mailbox#item) 属性获取 Outlook 中当前选定的项，并调用外接程序的主函数 `initDialer` 。

```js
Office.onReady()
    .then(
        // Checks for the DOM to load.
        $(document).ready(function () {
            // After the DOM is loaded, add-in-specific code can run.
            var mailbox = Office.context.mailbox;
            _Item = mailbox.item;
            initDialer();
        });
);
```

或者，您可以在事件处理程序中使用相同的代码， `initialize` 如下面的示例所示。

```js
Office.initialize = function () {
    // Checks for the DOM to load.
    $(document).ready(function () {
        // After the DOM is loaded, add-in-specific code can run.
        var mailbox = Office.context.mailbox;
        _Item = mailbox.item;
        initDialer();
    });
}
```

此方法可在 `onReady` 任何 Office 外接程序的或 `initialize` 处理程序中使用。

电话拨号器示例 Outlook 加载项展示了略为不同的方法，此方法仅使用 JavaScript 检查这些相同条件。

> [!IMPORTANT]
> 即使外接程序没有要执行的初始化任务，也必须至少包含调用 `Office.onReady` 或分配最少 `Office.initialize` 事件处理程序函数，如以下示例中所示。
>
>```js
>Office.onReady();
>```
>
>```js
>Office.initialize = function () {};
>```
>
> 如果您不调用 `Office.onReady` 或分配 `Office.initialize` 事件处理程序，则加载项会在启动时引发错误。 而且，如果某个用户尝试通过 Office Web 客户端（例如 Excel、PowerPoint 或 Outlook）使用你的加载项，则加载项会无法运行。
>
> 如果您的外接程序包含多个页面，则每当它加载一个新页面时，该页都必须调用 `Office.onReady` 或分配一个 `Office.initialize` 事件处理程序。

## <a name="see-also"></a>另请参阅

- [了解 Office JavaScript API](understanding-the-javascript-api-for-office.md)
- [初始化 Office 加载项](initialize-add-in.md)
