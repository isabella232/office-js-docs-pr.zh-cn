---
title: Office。上下文要求集1。8
description: ''
ms.date: 10/31/2019
localization_priority: Normal
ms.openlocfilehash: 791c88b3b6bc69834ca5635fe1021b9d11029629
ms.sourcegitcommit: e989096f3d19761bf8477c585cde20b3f8e0b90d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/31/2019
ms.locfileid: "37902138"
---
# <a name="context"></a>context

### <a name="officeofficemdcontext"></a>[Office](Office.md).context

Office.context 命名空间提供所有 Office 应用中的加载项所使用的共享接口。此列表仅记录 Outlook 加载项所使用的接口。有关 Office.context 命名空间的完整列表，请参阅[通用 API 中的 Office.context 引用](/javascript/api/office/office.context)。

##### <a name="requirements"></a>Requirements

|要求| 值|
|---|---|
|[最低版本的邮箱要求集](/office/dev/add-ins/reference/requirement-sets/outlook-api-requirement-sets)| 1.0|
|[适用的 Outlook 模式](/outlook/add-ins/#extension-points)| 撰写或阅读|

##### <a name="members-and-methods"></a>成员和方法

| 成员 | 类型 |
|--------|------|
| [displayLanguage](#displaylanguage-string) | Member |
| [roamingSettings](#roamingsettings-roamingsettings) | 成员 |

### <a name="namespaces"></a>命名空间

[邮箱](office.context.mailbox.md)：提供对 Microsoft Outlook 的 outlook 外接程序对象模型的访问权限。

### <a name="members"></a>Members

#### <a name="displaylanguage-string"></a>displayLanguage： String

获取用户针对 Office 主机应用程序的 UI 指定的 RFC 1766 语言标记格式的区域设置（语言）。

`displayLanguage` 值反映在 Office 主机应用程序中通过“**文件 > 选项 > 语言**”指定的当前“**显示语言**”设置。

##### <a name="type"></a>类型

*   String

##### <a name="requirements"></a>要求

|要求| 值|
|---|---|
|[最低版本的邮箱要求集](/office/dev/add-ins/reference/requirement-sets/outlook-api-requirement-sets)| 1.0|
|[适用的 Outlook 模式](/outlook/add-ins/#extension-points)| 撰写或阅读|

##### <a name="example"></a>示例

```js
function sayHelloWithDisplayLanguage() {
  var myDisplayLanguage = Office.context.displayLanguage;
  switch (myDisplayLanguage) {
    case 'en-US':
      write('Hello!');
      break;
    case 'en-NZ':
      write('G\'day mate!');
      break;
  }
}

// Function that writes to a div with id='message' on the page.
function write(message){
  document.getElementById('message').innerText += message;
}
```

<br>

---
---

#### <a name="roamingsettings-roamingsettingsjavascriptapioutlookofficeroamingsettingsviewoutlook-js-18"></a>roamingSettings： [roamingSettings](/javascript/api/outlook/office.RoamingSettings?view=outlook-js-1.8)

获取一个对象，它表示保存到用户邮箱的邮件外接程序的自定义设置或状态。

`RoamingSettings` 对象允许您存储和访问用户邮箱中存储的邮件外接程序的数据，以便从用于访问该邮箱的任何主机客户端应用程序中运行该外接程序时，该外接程序可以使用该数据。

##### <a name="type"></a>类型

*   [RoamingSettings](/javascript/api/outlook/office.RoamingSettings?view=outlook-js-1.8)

##### <a name="requirements"></a>Requirements

|要求| 值|
|---|---|
|[最低版本的邮箱要求集](/office/dev/add-ins/reference/requirement-sets/outlook-api-requirement-sets)| 1.0|
|[最低权限级别](/outlook/add-ins/understanding-outlook-add-in-permissions)| 受限|
|[适用的 Outlook 模式](/outlook/add-ins/#extension-points)| 撰写或阅读|