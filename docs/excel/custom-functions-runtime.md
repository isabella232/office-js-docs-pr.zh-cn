---
ms.date: 09/25/2020
description: 了解不使用任务窗格及其特定 JavaScript 运行时的 Excel 自定义函数。
title: 不带 UI 的 Excel 自定义函数的运行时
localization_priority: Normal
ms.openlocfilehash: 94254dfb5a0d03b7c9fec392b2377aff91b58af4
ms.sourcegitcommit: b47318a24a50443b0579e05e178b3bb5433c372f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/25/2020
ms.locfileid: "48279511"
---
# <a name="runtime-for-ui-less-excel-custom-functions"></a>不带 UI 的 Excel 自定义函数的运行时

不使用任务窗格的自定义函数 (UI 更少的自定义函数) 使用旨在优化计算性能的 JavaScript 运行时。

[!include[Excel custom functions note](../includes/excel-custom-functions-note.md)]

[!include[Shared runtime note](../includes/shared-runtime-note.md)]

此 JavaScript 运行时提供对命名空间中的 Api 的访问 `OfficeRuntime` ，这些 api 可由无 UI 的自定义函数和任务窗格用于存储数据。

## <a name="requesting-external-data"></a>请求外部数据

在无 UI 的自定义函数中，您可以通过使用 API （如 [Fetch](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) ）或使用 [XmlHttpRequest (XHR) ](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest)（一个标准 web API，它会发出与服务器交互的 HTTP 请求）请求外部数据。

请注意，在进行 XmlHttpRequests 时，无 UI 的函数必须使用其他安全措施，这需要 [相同的源策略](https://developer.mozilla.org/en-US/docs/Web/Security/Same-origin_policy) 和简单的 [CORS](https://www.w3.org/TR/cors/)。

简单的 CORS 实现不能使用 cookie，并且仅支持 (GET、HEAD、POST) 的简单方法。 简单的 CORS 接受字段名称为 `Accept`、`Accept-Language`、`Content-Language` 的简单标题。 您还可以使用 `Content-Type` 简单 CORS 中的标头，只要内容类型为 `application/x-www-form-urlencoded` 、 `text/plain` 或 `multipart/form-data` 。

## <a name="storing-and-accessing-data"></a>存储和访问数据

在不带 UI 的自定义函数中，您可以使用对象存储和访问数据 `OfficeRuntime.storage` 。 `Storage` 是一个永久性的未加密的键值存储系统，可提供 [localStorage](https://developer.mozilla.org/en-US/docs/Web/API/Window/localStorage)的替代方法，不能由无 UI 的自定义函数使用。 `Storage` 每个域提供 10 MB 的数据。 域可由多个加载项共享。

`Storage` 旨在作为共享存储解决方案，这意味着外接程序的多个部分将能访问相同数据。 例如，可以将用于用户身份验证的令牌存储在中， `storage` 因为无 UI 的自定义函数和外接程序 ui 元素（如任务窗格）可以访问它。 同样，如果两个加载项共享同一个域 (例如， `www.contoso.com/addin1` `www.contoso.com/addin2`) ，也允许它们前后共享信息 `storage` 。 请注意，具有不同子域的外接程序将具有不同的 `storage` (实例，例如 `subdomain.contoso.com/addin1` ， `differentsubdomain.contoso.com/addin2`) 。

由于 `storage` 可能是共享的位置，因此一定要认识到，可能会存在替代键值对的情况。

`storage` 对象支持以下方法：

 - `getItem`
 - `getItems`
 - `setItem`
 - `setItems`
 - `removeItem`
 - `removeItems`
 - `getKeys`

> [!NOTE]
> 没有用于清除所有信息 (如) 的方法 `clear` 。 相反，需要使用 `removeItems` 来一次性删除多个条目。

### <a name="officeruntimestorage-example"></a>OfficeRuntime 示例

下面的代码示例调用 `OfficeRuntime.storage.setItem` 函数，以将键和值设置为 `storage` 。

```js
function StoreValue(key, value) {

  return OfficeRuntime.storage.setItem(key, value).then(function (result) {
      return "Success: Item with key '" + key + "' saved to storage.";
  }, function (error) {
      return "Error: Unable to save item with key '" + key + "' to storage. " + error;
  });
}
```

## <a name="additional-considerations"></a>其他注意事项

如果外接程序仅使用无 UI 的自定义函数，请注意，不能使用不带 UI 的自定义函数 (DOM) 访问文档对象模型，也不能使用依赖于 DOM 的 jQuery 这样的库。

## <a name="next-steps"></a>后续步骤
了解如何 [调试不带 UI 的自定义函数](custom-functions-debugging.md)。

## <a name="see-also"></a>另请参阅

* [对 UI 进行身份验证-更少的自定义函数](custom-functions-authentication.md)
* [在 Excel 中创建自定义函数](custom-functions-overview.md)
* [自定义函数教程](../tutorials/excel-tutorial-create-custom-functions.md)
