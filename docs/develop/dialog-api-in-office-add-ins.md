---
title: 在 Office 加载项中使用 Office 对话框 API
description: 了解在 Office 外接程序中创建对话框的基础知识。
ms.date: 10/21/2020
localization_priority: Normal
ms.openlocfilehash: 1aa7a306402885f37d1cf07010eb43958407bf0f
ms.sourcegitcommit: 42e6cfe51d99d4f3f05a3245829d764b28c46bbb
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/23/2020
ms.locfileid: "48741083"
---
# <a name="use-the-office-dialog-api-in-office-add-ins"></a>在 Office 加载项中使用 Office 对话框 API

可以在 Office 加载项中使用 [Office 对话框 API](/javascript/api/office/office.ui) 打开对话框。 本文提供了有关如何在 Office 加载项中使用对话框 API 的指南。

> [!NOTE]
> 若要了解对话框 API 目前的受支持情况，请参阅[对话框 API 要求集](../reference/requirement-sets/dialog-api-requirement-sets.md)。 Excel、PowerPoint 和 Word 目前支持对话框 API。 有关各种邮箱要求集的 Outlook 支持包括 &mdash; ：有关更多详细信息，请参阅 API 参考。

对话框 API 的主要应用场景是为 Google、Facebook 或 Microsoft Graph 等资源启用身份验证。 有关详细信息，请在熟悉本文*之后*，参阅[使用 Office 对话框 API 进行身份验证](auth-with-office-dialog-api.md)。

不妨通过任务窗格/内容加载项/[加载项命令](../design/add-in-commands.md)打开对话框，以便执行下列操作：

- 显示无法直接在任务窗格中打开的登录页。
- 为加载项中的某些任务提供更多屏幕空间，或甚至整个屏幕。
- 托管在任务窗格中显得太小的视频。

> [!NOTE]
> 由于不赞成重叠 UI 元素，因此除非应用场景需要，否则请勿从任务窗格打开对话框。 考虑如何使用任务窗格区域时，请注意任务窗格中可以有选项卡。 有关示例，请参阅 [Excel 加载项 JavaScript SalesTracker](https://github.com/OfficeDev/Excel-Add-in-JavaScript-SalesTracker) 样本。

下图展示了对话框示例。

![外接程序命令](../images/auth-o-dialog-open.png)

请注意，对话框总是在屏幕的中心打开。 用户可以移动并重设对话框的大小。 窗口为 *是非*--用户可以继续与 Office 应用程序中的文档和任务窗格中的页面进行交互（如果有的话）。

## <a name="open-a-dialog-box-from-a-host-page"></a>从主机页面打开对话框

Office JavaScript API 在 [Office.context.ui 命名空间](/javascript/api/office/office.ui)中包含一个 [Dialog](/javascript/api/office/office.dialog) 对象和两个函数。

为了打开对话框，代码（通常是任务窗格中的一页）调用 [displayDialogAsync](/javascript/api/office/office.ui) 方法，并将要打开的资源 URL 传递到此方法。 调用方法的页面称为“主机页”。 例如在任务窗格中的 index.html 页面上使用脚本调用此方法，随后 index.html 是打开此方法对话框的主机页。

对话框中打开的资源通常是页面，，但也可以是 MVC 应用中的控制器方法、路由、Web 服务方法或其他任何资源。 在本文中，“页面”或“网站”是指对话框中的资源。 下面的代码就是一个非常简单的示例：

```js
Office.context.ui.displayDialogAsync('https://myAddinDomain/myDialog.html');
```

> [!NOTE]
> - URL 使用 HTTP**S** 协议。 对话框中加载的所有页面都必须要遵循此要求，而不仅仅是加载的第一个页面。
> - 对话框域与宿主页的域相同，宿主页可以是任务窗格中的页面，也可以是加载项命令的[函数文件](../reference/manifest/functionfile.md)。 这要求：传递到 `displayDialogAsync` 方法的页面、控制器方法或其他资源必须与主机页位于相同的域。

> [!IMPORTANT]
> 对话框中打开的主机页面和资源必须具有相同的完整域。 如果尝试传递 `displayDialogAsync` 加载项域的子域，则不会起作用。 完整域（包括任何子域）必须匹配。

加载第一个页面（或其它资源）后，用户可使用链接或其它用户界面来导航至任何使用 HTTPS 的网站（或其他资源）。 还可以将第一个页面设计为直接重定向到另一个站点。

默认情况下，对话框的高度和宽度占设备屏幕的 80%。不过，也可以设置不同的百分比，只需将配置对象传递给方法即可，如以下示例所示：

```js
Office.context.ui.displayDialogAsync('https://myDomain/myDialog.html', {height: 30, width: 20});
```

有关实现这一点的样本加载项，请参阅 [Office 加载项 Dialog API 示例](https://github.com/OfficeDev/Office-Add-in-Dialog-API-Simple-Example)。

将两个值均设置为 100% 可有效提供全屏体验。（有效最大值为 99.5%，窗口仍可移动和调整大小。）

> [!NOTE]
> 只能从主机窗口打开一个对话框。如果尝试再打开一个对话框，就会生成错误。比方说，如果用户从任务窗格打开一个对话框，她就无法再从任务窗格中的其他页面打开第二个对话框。不过，如果对话框是通过[加载项命令](../design/add-in-commands.md)打开，那么只要选择此命令，就会打开新 HTML 文件（但不可见）。这会新建（不可见的）主机窗口，所以每个这样的窗口都可以启动自己的对话框。有关详细信息，请参阅 [displayDialogAsync 返回的错误](dialog-handle-errors-events.md#errors-from-displaydialogasync)。

### <a name="take-advantage-of-a-performance-option-in-office-on-the-web"></a>利用 Office 网页版中的性能选项

`displayInIframe` 属性是配置对象中另一个可以传递到 `displayDialogAsync` 的属性。 如果将此属性设置为 `true`，且加载项在 Office 网页版打开的文档中运行，对话框就会以浮动 iframe（而不是独立窗口）的形式打开，从而加快对话框的打开速度。 示例如下：

```js
Office.context.ui.displayDialogAsync('https://myDomain/myDialog.html', {height: 30, width: 20, displayInIframe: true});
```

默认值为 `false`，与完全省略此属性时相同。 如果加载项没有在 Office 网页版中运行，`displayInIframe` 将被忽略。

> [!NOTE]
> 如果对话框始终重定向到无法在 iframe 中打开的页面，**不**得使用 `displayInIframe: true`。 例如，不能在 iframe 中打开许多常用 web 服务（如 Google 和 Microsoft 帐户）的 "登录" 页面。

## <a name="send-information-from-the-dialog-box-to-the-host-page"></a>将信息从对话框发送到主机页

对话框无法与任务窗格中的主机页进行通信，除非：

- 对话框中的当前页面与主机页在同一个域中。
- 在页面中加载 Office JavaScript API 库。  (与使用 Office JavaScript API 库的任何页面一样，页面的脚本必须为属性分配方法 `Office.initialize` ，尽管它可以是空方法。 有关详细信息，请参阅 [初始化 Office 外接程序](initialize-add-in.md)。 ) 

对话框中的代码使用 [messageParent](/javascript/api/office/office.ui#messageparent-message-) 函数，向主机页发送布尔值或字符串消息。 字符串可以是单词、句子、XML blob、字符串化 JSON 或其他任何能够序列化成字符串的内容。 示例如下：

```js
if (loginSuccess) {
    Office.context.ui.messageParent(true);
}
```

> [!IMPORTANT]
> - `messageParent` 函数只能在与主机页位于同一域（包括协议和端口）的页面上调用。
> - `messageParent`函数*只*是可在对话框中调用的两个 Office JS api 之一。 
> - 可以在对话框中调用的其他 JS API 为 `Office.context.requirements.isSetSupported` 。 有关它的信息，请参阅 [指定 Office 应用程序和 API 要求](specify-office-hosts-and-api-requirements.md)。 但是，在该对话框中，Outlook 2016 1-time purchase (中不支持此 API，即 MSI 版本) 。


在下一个示例中，`googleProfile` 是用户 Google 配置文件的字符串化版本。

```js
if (loginSuccess) {
    Office.context.ui.messageParent(googleProfile);
}
```

必须将主机页配置为接收消息。为此，可以向 `displayDialogAsync` 的原始调用添加回调参数。回调向 `DialogMessageReceived` 事件分配处理程序。示例如下：

```js
var dialog;
Office.context.ui.displayDialogAsync('https://myDomain/myDialog.html', {height: 30, width: 20},
    function (asyncResult) {
        dialog = asyncResult.value;
        dialog.addEventHandler(Office.EventType.DialogMessageReceived, processMessage);
    }
);
```

> [!NOTE]
> - Office 将 [AsyncResult ](/javascript/api/office/office.asyncresult) 对象传递给回叫。 表示尝试打开对话框的结果， 不表示对话框中任何事件的结果。 若要详细了解此区别，请参阅[处理错误和事件](dialog-handle-errors-events.md)。
> - `asyncResult` 的 `value` 属性设置为 [Dialog](/javascript/api/office/office.dialog) 对象，此对象位于主机页（而不是对话框的执行上下文）中。
> - `processMessage` 是用于处理事件的函数。可以根据需要任意命名。
> - `dialog` 变量的声明范围比回调更广，因为 `processMessage` 中也会引用此变量。

下面展示了 `DialogMessageReceived` 事件处理程序的简单示例：

```js
function processMessage(arg) {
    var messageFromDialog = JSON.parse(arg.message);
    showUserName(messageFromDialog.name);
}
```

> [!NOTE]
> - Office 将 `arg` 对象传递给处理程序。 它的 `message` 属性是对话框中的 `messageParent` 调用发送的布尔值或字符串。 在此示例中，它是用户配置文件从 Microsoft 帐户或 Google 等服务的字符串化表示形式，因此它将被反序列化为使用的对象 `JSON.parse` 。
> - 未显示 `showUserName` 实现。它可能在任务窗格上显示定制的欢迎消息。

在用户完成与对话框的交互后，消息处理程序应关闭对话框，如下面的示例所示。

```js
function processMessage(arg) {
    dialog.close();
    // message processing code goes here;
}
```

> [!NOTE]
> - `dialog` 对象必须是 `displayDialogAsync` 调用返回的对象。
> - `dialog.close` 调用指示 Office 立即关闭对话框。

有关使用这些技术的示例加载项，请参阅 [Office 加载项对话框 API 示例](https://github.com/OfficeDev/Office-Add-in-Dialog-API-Simple-Example)。

如果加载项在收到消息后需要打开任务窗格的其他页面，可以使用 `window.location.replace` 方法（或 `window.location.href`）作为处理程序的最后一行。示例如下：

```js
function processMessage(arg) {
    // message processing code goes here;
    window.location.replace("/newPage.html");
    // Alternatively ...
    // window.location.href = "/newPage.html";
}
```

有关具有此用途的加载项示例，请参阅[Insert Excel charts using Microsoft Graph in a PowerPoint add-in](https://github.com/OfficeDev/PowerPoint-Add-in-Microsoft-Graph-ASPNET-InsertChart)（在 PowerPoint 加载项中使用 Microsoft Graph 插入 Excel 图表）示例。

### <a name="conditional-messaging"></a>条件消息

由于可以从对话框发送多个 `messageParent` 调用，但在主机页中只有一个 `DialogMessageReceived` 事件处理程序，因此处理程序必须使用条件逻辑来区分不同的消息。 例如，如果对话框提示用户登录到标识提供程序（如 Microsoft 帐户或 Google），则它会将用户的配置文件作为邮件发送。 如果身份验证失败，对话框会将错误消息发送到主机页，如下面的示例所示：

```js
if (loginSuccess) {
    var userProfile = getProfile();
    var messageObject = {messageType: "signinSuccess", profile: userProfile};
    var jsonMessage = JSON.stringify(messageObject);
    Office.context.ui.messageParent(jsonMessage);
} else {
    var errorDetails = getError();
    var messageObject = {messageType: "signinFailure", error: errorDetails};
    var jsonMessage = JSON.stringify(messageObject);
    Office.context.ui.messageParent(jsonMessage);
}
```

> [!NOTE]
> - `loginSuccess` 变量通过读取标识提供程序返回的 HTTP 响应进行初始化。
> - 未显示 `getProfile` 和 `getError` 函数的实现。这两个函数均从查询参数或 HTTP 响应的正文获取数据。
> - 根据登录是否成功，发送不同类型的匿名对象。两者都有 `messageType` 属性。不同之处在于，一个有 `profile` 属性，另一个有 `error` 属性。

主机页中的处理程序代码使用 `messageType` 属性的值设置分支，如下面的示例所示。请注意，`showUserName` 函数的用法与之前的示例相同，`showNotification` 函数在主机页的 UI 中显示错误。

```js
function processMessage(arg) {
    var messageFromDialog = JSON.parse(arg.message);
    if (messageFromDialog.messageType === "signinSuccess") {
        dialog.close();
        showUserName(messageFromDialog.profile.name);
        window.location.replace("/newPage.html");
    } else {
        dialog.close();
        showNotification("Unable to authenticate user: " + messageFromDialog.error);
    }
}
```

> [!NOTE]
> `showNotification` 实施未在本文提供的示例代码中显示。 有关如何在外接程序中实施此函数的示例，请参阅 [Office 外接程序对话框 API 示例](https://github.com/OfficeDev/Office-Add-in-Dialog-API-Simple-Example)。

## <a name="pass-information-to-the-dialog-box"></a>向对话框传递信息

您的外接程序可以使用[messageChild](/javascript/api/office/office.dialog#messagechild-message-)将邮件从[主机页面](dialog-api-in-office-add-ins.md#open-a-dialog-box-from-a-host-page)发送到对话框。

### <a name="use-messagechild-from-the-host-page"></a>`messageChild()`从主机页使用

调用 Office 对话框 API 打开对话框时，将返回 [dialog](/javascript/api/office/office.dialog) 对象。 应将其分配给具有大于 [displayDialogAsync](/javascript/api/office/office.ui#displaydialogasync-startaddress--callback-) 方法的作用域的变量，因为该对象将被其他方法引用。 示例如下：

```javascript
var dialog;
Office.context.ui.displayDialogAsync('https://myDomain/myDialog.html',
    function (asyncResult) {
        dialog = asyncResult.value;
        dialog.addEventHandler(Office.EventType.DialogMessageReceived, processMessage);
    }
);

function processMessage(arg) {
    dialog.close();

  // message processing code goes here;

}
```

此 `Dialog` 对象具有一个 [messageChild](/javascript/api/office/office.dialog#messagechild-message-) 方法，该方法将任何字符串（包括字符串化数据）发送到对话框。 这 `DialogParentMessageReceived` 将在对话框中引发事件。 您的代码应处理此事件，如下一节中所示。

假设对话框的 UI 与当前活动的工作表相关，并且该工作表相对于其他工作表的位置。 在下面的示例中， `sheetPropertiesChanged` 将 Excel 工作表属性发送到对话框。 在这种情况下，当前工作表名为 "我的工作表"，并且它是工作簿中的第二个工作表。 数据封装在对象和字符串化中，以便可以将其传递给 `messageChild` 。

```javascript
function sheetPropertiesChanged() {
    var messageToDialog = JSON.stringify({
                               name: "My Sheet",
                               position: 2
                           });

    dialog.messageChild(messageToDialog);
}
```

### <a name="handle-dialogparentmessagereceived-in-the-dialog-box"></a>在对话框中处理 DialogParentMessageReceived

在对话框的 JavaScript 中， `DialogParentMessageReceived` 使用 [addHandlerAsync](/javascript/api/office/office.ui#addhandlerasync-eventtype--handler--options--callback-) 方法为事件注册处理程序。 通常在 [onReady 或 Office.initialize 方法](initialize-add-in.md)中执行此操作，如下所示。  (更强健的示例如下所示。 ) 

```javascript
Office.onReady()
    .then(function() {
        Office.context.ui.addHandlerAsync(
            Office.EventType.DialogParentMessageReceived,
            onMessageFromParent);
    });
```

然后，定义该 `onMessageFromParent` 处理程序。 下面的代码将继续上一节中的示例。 请注意，Office 会将参数传递给处理程序，并确保 `message` argument 对象的属性包含主机页中的字符串。 在此示例中，邮件被 reconverted 到对象，jQuery 用于将对话框的顶部标题设置为与新工作表名称相匹配。

```javascript
function onMessageFromParent(event) {
    var messageFromParent = JSON.parse(event.message);
    $('h1').text(messageFromParent.name);
}
```

最佳做法是验证是否正确注册了处理程序。 为此，可以将回调传递给 `addHandlerAsync` 方法。 注册处理程序的尝试完成时，将运行此过程。 如果未成功注册处理程序，请使用该处理程序记录或显示错误。 示例如下。 请注意，这 `reportError` 是未在此处定义的函数，它会记录或显示错误。

```javascript
Office.onReady()
    .then(function() {
        Office.context.ui.addHandlerAsync(
            Office.EventType.DialogParentMessageReceived,
            onMessageFromParent,
            onRegisterMessageComplete);
    });

function onRegisterMessageComplete(asyncResult) {
    if (asyncResult.status !== Office.AsyncResultStatus.Succeeded) {
        reportError(asyncResult.error.message);
    }
}
```

### <a name="conditional-messaging-from-parent-page-to-dialog-box"></a>"将父页的条件消息传递到" 对话框

由于可以 `messageChild` 从主机页进行多次调用，但在该事件的对话框中只有一个处理程序 `DialogParentMessageReceived` ，因此处理程序必须使用条件逻辑来区分不同的消息。 您可以按照与 [条件消息](#conditional-messaging)中所述的方式将消息发送到主机页时，精确地与构造条件消息传递的方式完全并行。

> [!NOTE]
> 在某些情况下， `messageChild` 可能不支持作为 [DialogApi 1.2 要求集](../reference/requirement-sets/dialog-api-requirement-sets.md)的一部分的 API。 以 [其他方式将邮件从其主机页传递到对话框，以其他方式](parent-to-dialog.md)对 "父对话" 对话消息进行描述。

> [!IMPORTANT]
> 不能在外接程序清单的部分中指定 [DialogApi 1.2 要求集](../reference/requirement-sets/dialog-api-requirement-sets.md) `<Requirements>` 。 您必须在运行时使用 [isSetSupported](specify-office-hosts-and-api-requirements.md#use-runtime-checks-in-your-javascript-code) 方法检查是否支持 DialogApi 1.2。 对清单要求的支持正在开发中。

## <a name="closing-the-dialog-box"></a>关闭对话框

可以在对话框中实现对话框关闭按钮。为此，关闭按钮的单击事件处理程序应使用 `messageParent` 通知主机页，关闭按钮已获单击。示例如下：

```js
function closeButtonClick() {
    var messageObject = {messageType: "dialogClosed"};
    var jsonMessage = JSON.stringify(messageObject);
    Office.context.ui.messageParent(jsonMessage);
}
```

`DialogMessageReceived` 的主机页处理程序将调用 `dialog.close`，如以下示例所示。 （请参阅前面的示例，其中展示了 `dialog` 对象的初始化方式。）

```js
function processMessage(arg) {
    var messageFromDialog = JSON.parse(arg.message);
    if (messageFromDialog.messageType === "dialogClosed") {
       dialog.close();
    }
}
```

即使你没有自己的关闭对话框 UI，最终用户也可以通过选择右上角的 **X** 关闭对话框。 此操作将触发 `DialogEventReceived` 事件。 如果主机窗格需要知道此事件何时发生，应为此事件声明一个处理程序。 有关详细信息，请参阅[对话框中的错误和事件](dialog-handle-errors-events.md#errors-and-events-in-the-dialog-box)部分。

## <a name="advanced-topics-and-special-scenarios"></a>高级主题和特殊情景

### <a name="use-the-dialog-api-to-show-a-video"></a>使用对话框 API 显示视频

参见“[使用 Office 对话框显示视频](dialog-video.md)”。

### <a name="use-the-dialog-apis-in-an-authentication-flow"></a>在身份验证流中使用对话框 API

请参阅[使用 Office 对话框 API 进行身份验证](auth-with-office-dialog-api.md)。

### <a name="using-the-office-dialog-api-with-single-page-applications-and-client-side-routing"></a>将 Office 对话框 API 与单页应用程序和客户端路由结合使用

使用 Office 对话框 API 时，需要小心处理 SPA 和客户端路由。 请参阅“[在 SPA 中使用 Office 对话框 API 的最佳做法](dialog-best-practices.md#best-practices-for-using-the-office-dialog-api-in-an-spa)”。

### <a name="error-and-event-handling"></a>错误和事件处理

参见“[处理 Office 对话框中的错误和事件](dialog-handle-errors-events.md)。

## <a name="next-steps"></a>后续步骤

在“[Office 对话框 API 最佳做法和规则](dialog-best-practices.md)”中了解 Office 对话框 API 的陷阱和最佳做法。
