---
title: 对文档或电子表格中的活动选择执行数据读取和写入操作
description: 了解如何在 Word 文档或 Excel 电子表格的活动选定内容中读取和写入数据。
ms.date: 06/20/2019
localization_priority: Normal
ms.openlocfilehash: 9eaf0aac406731a9c0033e69bd8946464a4d1a4f
ms.sourcegitcommit: 9609bd5b4982cdaa2ea7637709a78a45835ffb19
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/28/2020
ms.locfileid: "47292741"
---
# <a name="read-and-write-data-to-the-active-selection-in-a-document-or-spreadsheet"></a>对文档或电子表格中的活动选择执行数据读取和写入操作

通过 [Document](/javascript/api/office/office.document) 对象公开的方法，你可以读取文档或电子表格中用户的当前选区或向其中写入内容。 若要执行此操作， `Document` 对象将提供 `getSelectedDataAsync` 和 `setSelectedDataAsync` 方法。 本主题还介绍了如何读取、写入和创建事件处理程序，以检测对用户选定内容所做的更改。

该 `getSelectedDataAsync` 方法仅适用于用户的当前选择。 如果需要在文档中保留选区，以便使用相同的选区在运行加载项的各个会话中读取和写入，必须使用 [Bindings.addFromSelectionAsync](/javascript/api/office/office.bindings#addfromselectionasync-bindingtype--options--callback-) 方法添加绑定（或创建一个与 [Bindings](/javascript/api/office/office.bindings) 对象其他“addFrom”方法的绑定）。 有关创建对文档区域的绑定，然后读取和写入绑定的信息，请参阅[绑定到文档或电子表格中的区域](bind-to-regions-in-a-document-or-spreadsheet.md)。


## <a name="read-selected-data"></a>读取选择的数据


以下示例演示如何使用 [getSelectedDataAsync](/javascript/api/office/office.document#getselecteddataasync-coerciontype--options--callback-) 方法从文档的选定内容中获取数据。


```js
Office.context.document.getSelectedDataAsync(Office.CoercionType.Text, function (asyncResult) {
    if (asyncResult.status == Office.AsyncResultStatus.Failed) {
        write('Action failed. Error: ' + asyncResult.error.message);
    }
    else {
        write('Selected data: ' + asyncResult.value);
    }
});

// Function that writes to a div with id='message' on the page.
function write(message){
    document.getElementById('message').innerText += message; 
}
```

在此示例中，将第一个  _coercionType_ 参数指定为 `Office.CoercionType.Text` (您还可以使用文字字符串) 指定此参数 `"text"` 。 这意味着在回调函数的 [asyncResult](/javascript/api/office/office.asyncresult#status) 参数中提供的 [AsyncResult](/javascript/api/office/office.asyncresult) 对象的 _value_ 属性将返回一个包含文档中选定文本的 **string**。 指定不同强制类型将产生不同的值。 [Office.CoercionType](/javascript/api/office/office.coerciontype) 是可用的强制类型值的枚举。 `Office.CoercionType.Text` 计算结果为字符串 "text"。


> [!TIP]
> **何时应使用矩阵与表格 coercionType 数据访问？** 如果需要在添加行和列时动态增大选定的表格数据，并且必须使用表格标题，则应通过将方法的 _coercionType_ 参数指定 `getSelectedDataAsync` 为 `"table"` 或) 来使用 table 数据类型 (`Office.CoercionType.Table` 。 表格数据和矩阵数据中都支持在数据结构内添加行和列，但仅支持对表格数据追加行和列。 如果您不打算添加行和列，并且数据不需要标头功能，则应通过指定方法的  _coercionType_ 参数 as 或) 来使用矩阵数据类型 (`getSelectedDataAsync` `"matrix"` `Office.CoercionType.Matrix` ，这将提供更简单的数据交互模型。

在操作完成时，将执行作为第二个  _回调_ 参数传入函数的匿名函数 `getSelectedDataAsync` 。 调用该函数时使用单个参数 _asyncResult_，后者包含调用的结果和状态。 如果调用失败，对象的 [error](/javascript/api/office/office.asyncresult#asynccontext) 属性将 `AsyncResult` 提供对 [error](/javascript/api/office/office.error) 对象的访问权限。 您可以检查 [Error.name](/javascript/api/office/office.error#name) 和 [Error.message](/javascript/api/office/office.error#message) 属性的值，以确定设置操作失败的原因。 否则，会显示文档中选定的文本。

[AsyncResult.status](/javascript/api/office/office.asyncresult#error) 属性在 **if** 语句中用于测试调用是否成功。 [AsyncResultStatus](/javascript/api/office/office.asyncresult#status) 是可用 `AsyncResult.status` 属性值的枚举。 `Office.AsyncResultStatus.Failed` 计算结果为字符串 "failed" (，同样，也可以指定为该文本字符串) 。


## <a name="write-data-to-the-selection"></a>向选定内容中写入数据


以下示例演示如何将选定内容设置为显示"Hello World!"。


```js
Office.context.document.setSelectedDataAsync("Hello World!", function (asyncResult) {
    if (asyncResult.status == Office.AsyncResultStatus.Failed) {
        write(asyncResult.error.message);
    }
});

// Function that writes to a div with id='message' on the page.
function write(message){
    document.getElementById('message').innerText += message;
}
```

为  _data_ 参数传入不同对象类型会得到不同结果。 结果取决于文档中当前选定的内容、Office 客户端应用程序托管加载项的内容，以及传入的数据是否可以强制转换为当前所选内容。

作为  [callback](/javascript/api/office/office.document#setselecteddataasync-data--options--callback-) 参数传入 _setSelectedDataAsync_ 方法的匿名函数在异步调用完成时执行。 使用方法将数据写入选定内容 `setSelectedDataAsync` 中时，回调的 _asyncResult_ 参数仅提供对调用状态的访问权限，并在调用失败时向 [Error](/javascript/api/office/office.error) 对象提供访问权限。

> [!NOTE]
> 自 Excel 2013 SP1 发行版及相应的 Excel 网页版起，现在可以[在将表格写入当前选择时设置格式](../excel/excel-add-ins-tables.md)。


## <a name="detect-changes-in-the-selection"></a>检测选择中的更改


以下示例演示如何通过使用 [Document.addHandlerAsync](/javascript/api/office/office.document#addhandlerasync-eventtype--handler--options--callback-) 方法为文档中的 [SelectionChanged](/javascript/api/office/office.documentselectionchangedeventargs) 事件添加事件处理程序来检测选定内容中的更改。


```js
Office.context.document.addHandlerAsync("documentSelectionChanged", myHandler, function(result){}
);

// Event handler function.
function myHandler(eventArgs){
write('Document Selection Changed');
}

// Function that writes to a div with id='message' on the page.
function write(message){
    document.getElementById('message').innerText += message;
}
```

第一个  _eventType_ 参数指定要订阅的事件的名称。 传递 `"documentSelectionChanged"` 此参数的字符串等效于传递 `Office.EventType.DocumentSelectionChanged` [Office.](/javascript/api/office/office.eventtype) 事件类型枚举的事件类型。

作为第二个 _handler_ 参数传入函数的 `myHander()` 函数是在文档中的选定内容更改时执行的事件处理程序。调用该函数时使用单个参数 _eventArgs_，后者在异步操作完成时将包含对 [DocumentSelectionChangedEventArgs](/javascript/api/office/office.documentselectionchangedeventargs) 对象的引用。可以使用 [DocumentSelectionChangedEventArgs.document](/javascript/api/office/office.documentselectionchangedeventargs#document) 属性访问引发事件的文档。


> [!NOTE]
> 您可以通过 `addHandlerAsync` 再次调用方法并为 _处理程序_ 参数传递其他事件处理程序函数，为给定事件添加多个事件处理程序。 只要每个事件处理程序函数的名称保持唯一，此方法就有用。


## <a name="stop-detecting-changes-in-the-selection"></a>停止检测选择中的更改


以下示例演示如何通过调用 [document.removeHandlerAsync](/javascript/api/office/office.documentselectionchangedeventargs) 方法停止侦听 [Document.SelectionChanged](/javascript/api/office/office.document#removehandlerasync-eventtype--options--callback-) 事件。


```js
Office.context.document.removeHandlerAsync("documentSelectionChanged", {handler:myHandler}, function(result){});
```

`myHandler`作为第二个_handler_参数传递的函数名称指定将从事件中移除的事件处理程序 `SelectionChanged` 。


> [!IMPORTANT]
> 如果调用方法时省略可选的_handler_参数 `removeHandlerAsync` ，则将删除指定的事件名称的所有_eventType_事件处理程序。
