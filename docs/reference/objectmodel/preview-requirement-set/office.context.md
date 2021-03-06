---
title: Office. context-预览要求集
description: 使用邮箱 API preview 要求集的 Outlook 外接程序可用的 Context 对象成员。
ms.date: 10/14/2020
localization_priority: Normal
ms.openlocfilehash: 8286434d2cbfc11cf0d16f8bd014b4760f0337ff
ms.sourcegitcommit: 4e7c74ad67ea8bf6b47d65b2fde54a967090f65b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/20/2020
ms.locfileid: "48626405"
---
# <a name="context-mailbox-preview-requirement-set"></a> (邮箱预览要求集的上下文) 

### <a name="officecontext"></a>[Office](office.md).context

在所有 Office 应用中，上下文提供外接程序使用的共享接口。 此列表仅记录 Outlook 外接程序使用的那些接口。有关 "context" 命名空间的完整列表，请参阅 [通用 API 中的 "office. context reference](/javascript/api/office/office.context?view=outlook-js-preview&preserve-view=true)"。

##### <a name="requirements"></a>要求

|要求| 值|
|---|---|
|[最低版本的邮箱要求集](../../requirement-sets/outlook-api-requirement-sets.md)| 1.1|
|[适用的 Outlook 模式](../../../outlook/outlook-add-ins-overview.md#extension-points)| 撰写或阅读|

##### <a name="properties"></a>属性

| 属性 | 型号 | 返回类型 | 最小值<br>要求集 |
|---|---|---|:---:|
| [认证](#auth-auth) | 撰写<br>读取 | [Auth](/javascript/api/office/office.auth?view=outlook-js-preview&preserve-view=true) | [IdentityAPI 1。3](../../requirement-sets/identity-api-requirement-sets.md) |
| [contentLanguage](#contentlanguage-string) | 撰写<br>读取 | String | [1.1](../requirement-set-1.1/outlook-requirement-set-1.1.md) |
| [过程](#diagnostics-contextinformation) | 撰写<br>读取 | [ContextInformation](/javascript/api/office/office.contextinformation?view=outlook-js-preview&preserve-view=true) | [1.1](../requirement-set-1.1/outlook-requirement-set-1.1.md) |
| [displayLanguage](#displaylanguage-string) | 撰写<br>读取 | String | [1.1](../requirement-set-1.1/outlook-requirement-set-1.1.md) |
| [host](#host-hosttype) | 撰写<br>读取 | [HostType](/javascript/api/office/office.hosttype?view=outlook-js-preview&preserve-view=true) | [1.1](../requirement-set-1.1/outlook-requirement-set-1.1.md) |
| [mailbox](office.context.mailbox.md) | 撰写<br>读取 | [邮箱](/javascript/api/outlook/office.mailbox?view=outlook-js-preview&preserve-view=true) | [1.1](../requirement-set-1.1/outlook-requirement-set-1.1.md) |
| [officeTheme](#officetheme-officetheme) | 撰写<br>读取 | [OfficeTheme](/javascript/api/office/office.officetheme?view=outlook-js-preview&preserve-view=true) | [预览](../preview-requirement-set/outlook-requirement-set-preview.md) |
| [平台](#platform-platformtype) | 撰写<br>读取 | [PlatformType](/javascript/api/office/office.platformtype?view=outlook-js-preview&preserve-view=true) | [1.1](../requirement-set-1.1/outlook-requirement-set-1.1.md) |
| [满足](#requirements-requirementsetsupport) | 撰写<br>读取 | [RequirementSetSupport](/javascript/api/office/office.requirementsetsupport?view=outlook-js-preview&preserve-view=true) | [1.1](../requirement-set-1.1/outlook-requirement-set-1.1.md) |
| [roamingSettings](#roamingsettings-roamingsettings) | 撰写<br>读取 | [RoamingSettings](/javascript/api/outlook/office.roamingsettings?view=outlook-js-preview&preserve-view=true) | [1.1](../requirement-set-1.1/outlook-requirement-set-1.1.md) |
| [ui](#ui-ui) | 撰写<br>读取 | [UI](/javascript/api/office/office.ui?view=outlook-js-preview&preserve-view=true) | [1.1](../requirement-set-1.1/outlook-requirement-set-1.1.md) |

## <a name="property-details"></a>属性详细信息

#### <a name="auth-auth"></a>auth： [auth](/javascript/api/office/office.auth)

通过提供允许 Office 应用程序获取对加载项 web 应用程序的访问令牌的方法，支持 [单一登录 (SSO) ](../../../outlook/authenticate-a-user-with-an-sso-token.md) 。 这也使加载项能够间接访问已登录用户的 Microsoft Graph 数据，而不需要用户第二次登录。

##### <a name="type"></a>类型

*   [Auth](/javascript/api/office/office.auth)

##### <a name="requirements"></a>要求

|要求| 值|
|---|---|
|[最低版本的邮箱要求集](../../requirement-sets/outlook-api-requirement-sets.md)| 预览|
|[适用的 Outlook 模式](../../../outlook/outlook-add-ins-overview.md#extension-points)| 撰写或阅读|

##### <a name="example"></a>示例

```js
Office.context.auth.getAccessTokenAsync(function(result) {
    if (result.status === "succeeded") {
        var token = result.value;
        // ...
    } else {
        console.log("Error obtaining token", result.error);
    }
});
```

<br>

---
---

#### <a name="contentlanguage-string"></a>contentLanguage： String

获取用户指定的用于编辑项目的区域设置 (语言) 。

此 `contentLanguage` 值反映了使用 Office 客户端应用程序中的 "**文件 > 选项" > 语言**指定的当前**编辑语言**设置。

##### <a name="type"></a>类型

*   String

##### <a name="requirements"></a>要求

|要求| 值|
|---|---|
|[最低版本的邮箱要求集](../../requirement-sets/outlook-api-requirement-sets.md)| 1.1|
|[适用的 Outlook 模式](../../../outlook/outlook-add-ins-overview.md#extension-points)| 撰写或阅读|

##### <a name="example"></a>示例

```js
function sayHelloWithContentLanguage() {
  var myContentLanguage = Office.context.contentLanguage;
  switch (myContentLanguage) {
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

#### <a name="diagnostics-contextinformation"></a>诊断： [ContextInformation](/javascript/api/office/office.contextinformation)

获取有关加载项在其中运行的环境的信息。

##### <a name="type"></a>类型

*   [ContextInformation](/javascript/api/office/office.contextinformation)

##### <a name="requirements"></a>要求

|要求| 值|
|---|---|
|[最低版本的邮箱要求集](../../requirement-sets/outlook-api-requirement-sets.md)| 1.1|
|[适用的 Outlook 模式](../../../outlook/outlook-add-ins-overview.md#extension-points)| 撰写或阅读|

##### <a name="example"></a>示例

```js
console.log(JSON.stringify(Office.context.diagnostics));
```

<br>

---
---

#### <a name="displaylanguage-string"></a>displayLanguage： String

获取用户为 Office 客户端应用程序的 UI 指定的 RFC 1766 语言标记格式中 (语言) 的区域设置。

此 `displayLanguage` 值反映了使用 Office 客户端应用程序中的**File > Options > 语言**指定的当前**显示语言**设置。

##### <a name="type"></a>类型

*   String

##### <a name="requirements"></a>要求

|要求| 值|
|---|---|
|[最低版本的邮箱要求集](../../requirement-sets/outlook-api-requirement-sets.md)| 1.1|
|[适用的 Outlook 模式](../../../outlook/outlook-add-ins-overview.md#extension-points)| 撰写或阅读|

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

#### <a name="host-hosttype"></a>主机： [HostType](/javascript/api/office/office.hosttype)

获取承载外接程序的 Office 应用程序。

##### <a name="type"></a>类型

*   [HostType](/javascript/api/office/office.hosttype)

##### <a name="requirements"></a>要求

|要求| 值|
|---|---|
|[最低版本的邮箱要求集](../../requirement-sets/outlook-api-requirement-sets.md)| 1.1|
|[适用的 Outlook 模式](../../../outlook/outlook-add-ins-overview.md#extension-points)| 撰写或阅读|

##### <a name="example"></a>示例

```js
console.log(JSON.stringify(Office.context.host));
```

<br>

---
---

#### <a name="officetheme-officetheme"></a>officeTheme： [officeTheme](/javascript/api/office/office.officetheme)

提供了访问 Office 主题颜色的属性。

> [!NOTE]
> 此成员仅在 Windows 中的 Outlook 中受支持。

使用 Office 主题颜色，您可以将加载项的配色方案与用户选择的当前 Office 主题进行协调，以供用户使用 **office > Office 帐户 > Office 主题 UI**，该用户在所有 Office 客户端应用程序中应用。 使用 Office 主题颜色适用于邮件和任务窗格外接程序。

##### <a name="type"></a>类型

*   [OfficeTheme](/javascript/api/office/office.officetheme)

##### <a name="properties"></a>属性：

|名称| 类型| 说明|
|---|---|---|
|`bodyBackgroundColor`| String|获取十六进制三原色形式的 Office 主题正文背景色。|
|`bodyForegroundColor`| String|获取十六进制三原色形式的 Office 主题正文前景色。|
|`controlBackgroundColor`| 字符串|获取十六进制三原色形式的 Office 主题控制背景色。|
|`controlForegroundColor`| 字符串|获取十六进制三原色形式的 Office 主题正文控制颜色。|

##### <a name="requirements"></a>要求

|要求| 值|
|---|---|
|[最低版本的邮箱要求集](../../requirement-sets/outlook-api-requirement-sets.md)| 预览|
|[适用的 Outlook 模式](../../../outlook/outlook-add-ins-overview.md#extension-points)| 撰写或阅读|

##### <a name="example"></a>示例

```js
function applyOfficeTheme(){
  // Get office theme colors.
  var bodyBackgroundColor = Office.context.officeTheme.bodyBackgroundColor;
  var bodyForegroundColor = Office.context.officeTheme.bodyForegroundColor;
  var controlBackgroundColor = Office.context.officeTheme.controlBackgroundColor
  var controlForegroundColor = Office.context.officeTheme.controlForegroundColor;

  // Apply body background color to a CSS class.
  $('.body').css('background-color', bodyBackgroundColor);
}
```

<br>

---
---

#### <a name="platform-platformtype"></a>platform： [PlatformType](/javascript/api/office/office.platformtype)

提供在其上运行外接的平台。

##### <a name="type"></a>类型

*   [PlatformType](/javascript/api/office/office.platformtype)

##### <a name="requirements"></a>要求

|要求| 值|
|---|---|
|[最低版本的邮箱要求集](../../requirement-sets/outlook-api-requirement-sets.md)| 1.1|
|[适用的 Outlook 模式](../../../outlook/outlook-add-ins-overview.md#extension-points)| 撰写或阅读|

##### <a name="example"></a>示例

```js
console.log(JSON.stringify(Office.context.platform));
```

<br>

---
---

#### <a name="requirements-requirementsetsupport"></a>要求： [RequirementSetSupport](/javascript/api/office/office.requirementsetsupport)

提供用于确定当前应用程序和平台支持哪些要求集的方法。

##### <a name="type"></a>类型

*   [RequirementSetSupport](/javascript/api/office/office.requirementsetsupport)

##### <a name="requirements"></a>要求

|要求| 值|
|---|---|
|[最低版本的邮箱要求集](../../requirement-sets/outlook-api-requirement-sets.md)| 1.1|
|[适用的 Outlook 模式](../../../outlook/outlook-add-ins-overview.md#extension-points)| 撰写或阅读|

##### <a name="example"></a>示例

```js
console.log(JSON.stringify(Office.context.requirements.isSetSupported("mailbox", "1.1")));
```

<br>

---
---

#### <a name="roamingsettings-roamingsettings"></a>roamingSettings： [roamingSettings](/javascript/api/outlook/office.roamingsettings)

获取一个对象，它表示保存到用户邮箱的邮件外接程序的自定义设置或状态。

该 `RoamingSettings` 对象使您可以存储和访问存储在用户邮箱中的邮件外接程序的数据，以便该外接程序从用于访问该邮箱的任何 Outlook 客户端运行时都可使用该加载项。

##### <a name="type"></a>类型

*   [RoamingSettings](/javascript/api/outlook/office.RoamingSettings)

##### <a name="requirements"></a>要求

|要求| 值|
|---|---|
|[最低版本的邮箱要求集](../../requirement-sets/outlook-api-requirement-sets.md)| 1.1|
|[最低权限级别](../../../outlook/understanding-outlook-add-in-permissions.md)| 受限|
|[适用的 Outlook 模式](../../../outlook/outlook-add-ins-overview.md#extension-points)| 撰写或阅读|

<br>

---
---

#### <a name="ui-ui"></a>ui： [ui](/javascript/api/office/office.ui)

提供可用于在 Office 外接程序中创建和操作 UI 组件（如对话框）的对象和方法。

##### <a name="type"></a>类型

*   [UI](/javascript/api/office/office.ui)

##### <a name="requirements"></a>要求

|要求| 值|
|---|---|
|[最低版本的邮箱要求集](../../requirement-sets/outlook-api-requirement-sets.md)| 1.1|
|[适用的 Outlook 模式](../../../outlook/outlook-add-ins-overview.md#extension-points)| 撰写或阅读|
