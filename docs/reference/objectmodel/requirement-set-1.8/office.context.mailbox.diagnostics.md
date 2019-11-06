---
title: "\"Context.subname\"： \"邮箱\"。诊断-要求集1。8"
description: ''
ms.date: 10/30/2019
localization_priority: Normal
ms.openlocfilehash: 8b2d67fbc5eb8462af67a0dc73ce65a433ad5795
ms.sourcegitcommit: e989096f3d19761bf8477c585cde20b3f8e0b90d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/31/2019
ms.locfileid: "37902139"
---
# <a name="diagnostics"></a>diagnostics

### <a name="officeofficemdcontextofficecontextmdmailboxofficecontextmailboxmddiagnostics"></a>[Office](Office.md)[.context](Office.context.md)[.mailbox](Office.context.mailbox.md).diagnostics

将诊断信息提供给 Outlook 外接程序。

##### <a name="requirements"></a>Requirements

|要求| 值|
|---|---|
|[最低版本的邮箱要求集](/office/dev/add-ins/reference/requirement-sets/outlook-api-requirement-sets)| 1.0|
|[最低权限级别](/outlook/add-ins/understanding-outlook-add-in-permissions)| ReadItem|
|[适用的 Outlook 模式](/outlook/add-ins/#extension-points)| 撰写或阅读|

##### <a name="members-and-methods"></a>成员和方法

| 成员 | 类型 |
|--------|------|
| [主机名](#hostname-string) | Member |
| [Diagnostics.hostversion](#hostversion-string) | Member |
| [OWAView](#owaview-string) | Member |

### <a name="members"></a>Members

#### <a name="hostname-string"></a>hostName： String

获取表示主机应用程序的名称的字符串。

可以是下列值之一的字符串：`Outlook`、`OutlookWebApp`、`OutlookIOS` 或 `OutlookAndroid`。

> [!NOTE]
> 对`Outlook`桌面客户端（即 Windows 和 Mac）上的 Outlook 返回值。

##### <a name="type"></a>类型

*   String

##### <a name="requirements"></a>要求

|要求| 值|
|---|---|
|[最低版本的邮箱要求集](/office/dev/add-ins/reference/requirement-sets/outlook-api-requirement-sets)| 1.0|
|[最低权限级别](/outlook/add-ins/understanding-outlook-add-in-permissions)| ReadItem|
|[适用的 Outlook 模式](/outlook/add-ins/#extension-points)| 撰写或阅读|

<br>

---
---

#### <a name="hostversion-string"></a>Diagnostics.hostversion： String

获取表示主机应用程序或 Exchange 服务器的版本的字符串（例如，"15.0.468.0"）。

如果邮件外接程序在 Outlook 桌面客户端或 iOS 上运行，则该`hostVersion`属性返回主机应用程序（Outlook）的版本。 在 Outlook 网页版中，该属性返回的是 Exchange 服务器的版本。

##### <a name="type"></a>类型

*   String

##### <a name="requirements"></a>要求

|要求| 值|
|---|---|
|[最低版本的邮箱要求集](/office/dev/add-ins/reference/requirement-sets/outlook-api-requirement-sets)| 1.0|
|[最低权限级别](/outlook/add-ins/understanding-outlook-add-in-permissions)| ReadItem|
|[适用的 Outlook 模式](/outlook/add-ins/#extension-points)| 撰写或阅读|

<br>

---
---

#### <a name="owaview-string"></a>OWAView： String

获取表示 web 上的 Outlook 的当前视图的字符串。

返回的字符串可以是下列值之一：`OneColumn`、`TwoColumns` 或 `ThreeColumns`。

如果主机应用程序不是 web 上的 Outlook，则访问此属性将导致`undefined`。

Web 上的 Outlook 具有三个视图，分别对应于屏幕的宽度和窗口，以及可以显示的列数：

*   `OneColumn` 在屏幕较窄时显示。 Web 上的 Outlook 在智能手机的整个屏幕上使用此单列布局。
*   `TwoColumns` 在屏幕较宽时显示。 Outlook 网页版在大多数平板电脑上使用此视图。
*   `ThreeColumns` 在屏幕为宽屏时显示。 例如，web 上的 Outlook 在桌面计算机上的全屏窗口中使用此视图。

##### <a name="type"></a>类型

*   String

##### <a name="requirements"></a>要求

|要求| 值|
|---|---|
|[最低版本的邮箱要求集](/office/dev/add-ins/reference/requirement-sets/outlook-api-requirement-sets)| 1.0|
|[最低权限级别](/outlook/add-ins/understanding-outlook-add-in-permissions)| ReadItem|
|[适用的 Outlook 模式](/outlook/add-ins/#extension-points)| 撰写或阅读|