---
title: 初始化 Office 加载项
description: 了解如何初始化 Office 外接程序。
ms.date: 02/27/2020
localization_priority: Normal
ms.openlocfilehash: 5dc9d0143ac9eaab18625e280891bd601fa9f899
ms.sourcegitcommit: 9609bd5b4982cdaa2ea7637709a78a45835ffb19
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/28/2020
ms.locfileid: "47293322"
---
# <a name="initialize-your-office-add-in"></a>初始化 Office 加载项

Office 加载项通常使用启动逻辑执行以下操作：

- 检查用户的 Office 版本是否支持您的代码调用的所有 Office Api。

- 确保存在某些项目，如具有特定名称的工作表。

- 提示用户选择 Excel 中的某些单元格，然后插入用这些选定值初始化的图表。

- 建立绑定。

- 使用 Office 对话框 API 提示用户输入默认的外接程序设置值。

但是，在加载库之前，Office 外接程序无法成功调用任何 Office JavaScript Api。 本文介绍了您的代码可确保库已加载的两种方法：

- 使用初始化 `Office.onReady()` 。
- 使用初始化 `Office.initialize` 。

> [!TIP]
> 建议使用 `Office.onReady()` 取代 `Office.initialize`。 尽管 `Office.initialize` 仍受支持，但 `Office.onReady()` 提供了更大的灵活性。 只能将一个处理程序分配给 `Office.initialize` ，而只是通过 Office 基础结构调用一次。 您可以 `Office.onReady()` 在代码中的不同位置调用，并使用不同的回调。
> 
> 有关这两种方法之间的差别信息，请参阅 [Office.initialize 和 Office.onReady() 之间的主要差别](#major-differences-between-officeinitialize-and-officeonready)。

有关初始化加载项时的事件顺序的更多详细信息，请参阅 [加载 DOM 和运行时环境](loading-the-dom-and-runtime-environment.md)。

## <a name="initialize-with-officeonready"></a>使用 Office.onReady() 进行初始化

`Office.onReady()` 是一种异步方法，它在检查是否已加载 Office.js 库时返回一个 [承诺](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise) 对象。 加载库时，它会将承诺解析为一个对象，该对象指定具有 enum 值的 Office 客户端应用程序 `Office.HostType` (`Excel` 、、 `Word` 等 ) 以及具有 `Office.PlatformType` enum 值的平台 (`PC` 、 `Mac` 、、 `OfficeOnline` 等 ) 。 如果在调用 `Office.onReady()` 时已加载库，则 Promise 将立即解析。

调用 `Office.onReady()` 的一种方法是向其传递一个回调方法。 下面是一个示例：

```js
Office.onReady(function(info) {
    if (info.host === Office.HostType.Excel) {
        // Do Excel-specific initialization (for example, make add-in task pane's
        // appearance compatible with Excel "green").
    }
    if (info.platform === Office.PlatformType.PC) {
        // Make minor layout changes in the task pane.
    }
    console.log(`Office.js is now ready in ${info.host} on ${info.platform}`);
});
```

或者，可以将 `then()` 方法链接到 `Office.onReady()` 的调用，而不是传递回调。 例如，以下代码检查用户的 Excel 版本是否支持加载项可能调用的所有 API。

```js
Office.onReady()
    .then(function() {
        if (!Office.context.requirements.isSetSupported('ExcelApi', '1.7')) {
            console.log("Sorry, this add-in only works with newer versions of Excel.");
        }
    });
```

以下是在 TypeScript 中使用 `async` 和 `await` 关键字的同一示例：

```typescript
(async () => {
    await Office.onReady();
    if (!Office.context.requirements.isSetSupported('ExcelApi', '1.7')) {
        console.log("Sorry, this add-in only works with newer versions of Excel.");
    }
})();
```

如果使用的是其他 JavaScript 框架，其中包括它们自己的初始化处理程序或测试，那么它们*通常*应放置在 `Office.onReady()` 的响应内。 例如，会对 [JQuery 的](https://jquery.com) `$(document).ready()` 函数进行以下引用：

```js
Office.onReady(function() {
    // Office is ready
    $(document).ready(function () {
        // The document is ready
    });
});
```

但是，此做法存在例外情况。 例如，假设您想要在浏览器中打开加载项 (而不是将其旁加载在 Office 应用程序中) ，以便使用浏览器工具调试您的 UI。 由于 Office.js 将不会在浏览器中加载，所以，`onReady` 将不会运行，且如果在 Office `$(document).ready` 内调用它，则 `onReady` 将不会运行。 

如果您希望在加载加载项时，任务窗格中显示进度指示器，另一个例外。 在这种情况下，代码应调用 jQuery `ready` 并使用其回调来呈现进度指示器。 然后，Office `onReady` 的回调可将进度指示器替换为最终 UI。 

## <a name="initialize-with-officeinitialize"></a>使用 Office.initialize 进行初始化

当 Office.js 库加载并准备好用于用户交互时将触发初始化事件。 可将处理程序分配到实现初始化逻辑的 `Office.initialize`。 以下是检查用户的 Excel 版本是否支持加载项可能调用的所有 API 的示例。

```js
Office.initialize = function () {
    if (!Office.context.requirements.isSetSupported('ExcelApi', '1.7')) {
        console.log("Sorry, this add-in only works with newer versions of Excel.");
    }
};
```

如果使用的是包含其自己的初始化处理程序或测试的其他 JavaScript 框架，则 *通常* 应将这些框架放在 `Office.initialize` 事件 (在 OnReady 中的 **Initialize ( # B2 ** 部分中所述的异常也会在此情况下应用) 。 例如，会对 [JQuery 的](https://jquery.com) `$(document).ready()` 函数进行以下引用：

```js
Office.initialize = function () {
    // Office is ready
    $(document).ready(function () {
        // The document is ready
    });
  };
```

对于任务窗格和内容加载项，`Office.initialize` 提供了其他 _reason_ 参数。 此参数指定如何将加载项添加到当前文档。 可以使用此参数针对首次插入加载项时和加载项已存在于文档中时实施不同的逻辑。

```js
Office.initialize = function (reason) {
    $(document).ready(function () {
        switch (reason) {
            case 'inserted': console.log('The add-in was just inserted.');
            case 'documentOpened': console.log('The add-in is already part of the document.');
        }
    });
 };
```

有关详细信息，请参阅 [Office.initialize 事件](/javascript/api/office)和 [InitializationReason 枚举](/javascript/api/office/office.initializationreason)。

## <a name="major-differences-between-officeinitialize-and-officeonready"></a>Office.initialize 和 Office.onReady 之间的主要差别

- 可以仅将一个处理程序分配到 `Office.initialize` 并仅由 Office 基础结构调用一次，但可以在代码中的不同位置调用 `Office.onReady()` 并使用不同的回调。 例如，只要自定义脚本使用运行初始化逻辑的回调进行加载，代码就可以调用 `Office.onReady()`。代码还可以在任务窗格中设置一个按钮，其脚本会使用不同的回调调用 `Office.onReady()`。 如果是这样，则会在单击该按钮后运行第二个回调。

- `Office.initialize` 事件将在 Office.js 初始化其本身的内部过程的末尾处触发。 并且它会在内部过程结束后*立即*触发。 如果将处理程序分配到事件所使用的代码在事件触发后执行的时间过长，则处理程序将不会运行。 例如，如果使用的是 WebPack 任务管理器，则在加载 Office.js 后但在加载自定义 JavaScript 前，它会配置加载项的主页以加载填充代码文件。 在脚本加载和分配处理程序时，初始化事件已经发生。 但调用 `Office.onReady()` 永远不会“太迟”。 如果初始化事件已经发生，则回调将立即运行。

> [!NOTE]
> 即使没有启动逻辑，也应在加载项 JavaScript 加载时调用 `Office.onReady()` 或将空函数分配到 `Office.initialize`。 某些 Office 应用程序和平台组合不会加载任务窗格，除非发生其中一种情况。 以下示例显示了这两种方法。
>
>```js    
>Office.onReady();
>```
>
>
>```js
>Office.initialize = function () {};
>```

## <a name="see-also"></a>另请参阅

- [了解 Office JavaScript API](understanding-the-javascript-api-for-office.md)
- [加载 DOM 和运行时环境](loading-the-dom-and-runtime-environment.md)