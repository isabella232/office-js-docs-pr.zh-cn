---
title: Outlook 加载项的激活和 API 使用限制
description: 请注意某些激活和 API 使用指南，并在这些限制范围内实施加载项。
ms.date: 05/08/2020
localization_priority: Normal
ms.openlocfilehash: ad7e572152e9dd9aeb3b07f1c545184343c4e5e8
ms.sourcegitcommit: 9609bd5b4982cdaa2ea7637709a78a45835ffb19
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/28/2020
ms.locfileid: "47293875"
---
# <a name="limits-for-activation-and-javascript-api-for-outlook-add-ins"></a>Outlook 加载项的激活和 JavaScript API 限制

为了向 Outlook 外接程序的用户提供令人满意的体验，您必须了解特定的激活和 API 使用准则，并执行外接程序使其不超过这些限制。 存在这些准则，以使单个外接程序在处理对 Office JavaScript API 的激活规则或调用时不需要花费很长的时间，从而影响 Outlook 和其他外接程序的总体用户体验。这些限制适用于在外接部件清单中设计激活规则，使用自定义属性、漫游设置、收件人、Exchange Web 服务 (EWS) 请求和响应以及异步调用。

> [!NOTE]
> 如果外接程序在 Outlook 富客户端中运行，还必须确认运行的外接程序是否在特定运行时资源使用状况限制内。

## <a name="limits-on-where-add-ins-activate"></a>外接程序激活位置的限制

默认情况下，加载项设计为仅在用户的主邮箱中激活。 这意味着外接程序通常不会在共享邮箱、使用代理访问、存档邮箱或公用文件夹打开的来自其他用户的邮箱中的文件夹中激活。 但是，支持代理访问的外接程序 [或共享文件夹](delegate-access.md) 应激活。

## <a name="limits-for-activation-rules"></a>激活规则的限制

为 Outlook 外接程序设计激活规则时，请遵循以下准则：

- 将清单的大小限制为 256 KB。如果超出该限制，则无法为 Exchange 邮箱安装 Outlook 外接程序。

- 可为外接程序最多指定 15 条激活规则。如果超出该限制，则无法安装外接程序。

- 如果您对所选项目的正文使用 [ItemHasKnownEntity](../reference/manifest/rule.md#itemhasknownentity-rule) 规则，预计 Outlook 富客户端将仅对正文的前 1 MB 应用规则，而不会超过此限制应用于正文的其他部分。如果正文的前 1 MB 之后存在匹配，您的外接程序将不会激活。如果您期望这成为一种可能的方案，请重新设计激活条件。

- 如果在或 ItemHasRegularExpressionMatch 规则中使用正则表达式 `ItemHasKnownEntity` ，请注意以下限制和准则，这些限制和准则通常适用于任何 Outlook 应用程序，而表1、2和3中所述的准则因应用程序而异： [ItemHasRegularExpressionMatch](../reference/manifest/rule.md#itemhasregularexpressionmatch-rule)
   - 在外接中的激活规则中最长指定五个正则表达式。 You cannot install a add-in if you exceed that limit.
   - 指定正则表达式，以便在 `getRegExMatches` 第一个50匹配项中的方法调用返回您预期的结果。
   - 可以在正则表达式中指定向前断言，但不支持向后 `(?<=text)` 和否定向后 `(?<!text)` 断言。

表1列出了这些限制，并介绍了在 Outlook 富客户端与 Outlook 网页版或移动设备之间的正则表达式支持之间的差异。 这种支持不依赖于任何特定类型的设备和项目正文。

**表 1.各种正则表达式支持的一般区别**

|Outlook 富客户端|Outlook 网页版或移动设备版|
|:-----|:-----|
|使用作为 Visual Studio 标准模板库一部分提供的 C++ 正则表达式引擎。该引擎使用 ECMAScript 5 标准编译。 |使用属于 JavaScript 一部分的正则表达式评估，由浏览器提供，且支持 ECMAScript 5 超集。|
|由于不同的 regex 引擎，预计包含基于预定义字符类的自定义字符类的正则表达式在 Outlook 富客户端中返回的结果可能比在 web 或移动设备上的 Outlook 中返回不同的结果。<br/><br/>例如，正则表达式 `[\s\S]{0,100}` 与任意数量（0 到 100）的单个空格字符或非空格字符匹配。 此正则表达式在 Outlook 富客户端中返回的结果与 web 和移动设备上的 Outlook 不同。<br/><br/>解决办法是，应将正则表达式重写为 `(\s\|\S){0,100}`。 此变通正则表达式与任意数量（0 到 100）的空格字符或非空格字符匹配。<br/><br/>应在每个 Outlook 客户端上全面测试每个 regex，如果正则表达式返回不同的结果，请重写 regex。 |应在每个 Outlook 客户端上全面测试每个 regex，如果正则表达式返回不同的结果，请重写 regex。|
|默认情况下，外接程序的所有正则表达式的计算时间限制为 1 秒。 超出此限制将导致最多重新计算 3 次。 除了重新评估限制之外，Outlook 富客户端也禁止在任何 Outlook 客户端中为同一邮箱运行外接程序。<br/><br/>管理员可以使用 `OutlookActivationAlertThreshold` 和注册表项替代这些评估限制 `OutlookActivationManagerRetryLimit` 。|不支持与 Outlook 富客户端中相同的资源监视或注册表设置。 但对于所有 Outlook 客户端上的同一邮箱，在 Outlook 富客户端上使用需要大量评估时间的正则表达式的加载项将被禁用。|

表 2 列出了这些限制并介绍了每一个 Outlook 应用了正则表达式的项正文部分的区别。如果对项正文应用了正则表达式，则其中某些限制取决于设备和项正文的类型。

**表 2.计算的项正文的大小限制**

||Outlook 富客户端|移动设备上的 Outlook|Outlook 网页版|
|:-----|:-----|:-----|:-----|
|**外形规格**|任何支持的设备|Android 智能手机、iPad 或 iPhone|Android 智能手机、iPad 和 iPhone 之外任何支持的设备|
|**纯文本项正文**|对正文数据的第一个 1 MB 而不对超出该限制的其余正文应用正则表达式。|仅当正文少于 16,000 个字符时激活加载项。|仅当正文少于 500,000 个字符时激活加载项。|
|**HTML 项正文**|对正文数据的第一个 512 KB 而不对超出该限制的其余正文应用正则表达式。（实际的字符数取决于范围可为每字符 1 到 4 字节的编码。）|对前 64,000 个字符（包括 HTML 标记字符）而不对超出该限制的其余正文应用正则表达式。|仅当正文少于 500,000 个字符时激活加载项。|

表3列出了限制，并说明了每个 Outlook 客户端在计算正则表达式后返回的匹配项之间的差异。 这种支持不依赖于任何特定设备类型，但是，如果对项正文应用了正则表达式，则该支持可能依赖于项正文的类型。

**表 3.返回的匹配项限制**

||Outlook 富客户端|Outlook 网页版或移动设备版|
|:-----|:-----|:-----|
|**返回的匹配项的顺序**|假设在 `getRegExMatches` outlook 富客户端中对同一项目应用的同一个正则表达式的匹配项返回的匹配项与在 web 或移动设备上的 outlook 中有所不同。|假定在 `getRegExMatches` outlook 富客户端中返回的顺序与 web 或移动设备上的 outlook 中的匹配顺序不同。|
|**纯文本项正文**|`getRegExMatches` 返回的任何匹配项最多为 1536 (1.5 KB) 个字符，最多为50个匹配项。<br/><br/>**注意**：不 `getRegExMatches` 会在返回的数组中返回任何特定顺序的匹配项。 通常，假设 Outlook 富客户端中的匹配顺序与在同一项目上应用的同一正则表达式不同，在 web 和移动设备上的 Outlook 中。|`getRegExMatches` 返回的任何匹配项最多为 3072 (3 KB) 个字符，最多为50个匹配项。|
|**HTML 项正文**|`getRegExMatches` 返回的任何匹配项最多为 3072 (3 KB) 个字符，最多为50个匹配项。<br/> <br/> **注意**：不 `getRegExMatches` 会在返回的数组中返回任何特定顺序的匹配项。 通常，假设 Outlook 富客户端中的匹配顺序与在同一项目上应用的同一正则表达式不同，在 web 和移动设备上的 Outlook 中。|`getRegExMatches` 返回的任何匹配项最多为 3072 (3 KB) 个字符，最多为50个匹配项。|

## <a name="limits-for-javascript-api"></a>JavaScript API 的限制

除了针对激活规则的上述准则之外，每个 Outlook 客户端都会强制实施 JavaScript 对象模型中的某些限制，如表4中所述。

**表4。使用 Office JavaScript API 获取或设置某些数据的限制**

|功能|限制|相关 API|说明|
|:-----|:-----|:-----|:-----|
|自定义属性|2500 个字符|[CustomProperties](/javascript/api/outlook/office.CustomProperties) 对象<br/> <br/>[item.loadCustomPropertiesAsync](../reference/objectmodel/preview-requirement-set/office.context.mailbox.item.md#methods) 方法|约会或邮件项目的所有自定义属性的限制。 如果外接程序的所有自定义属性的总大小超过此限制，则所有 Outlook 客户端都将返回错误。|
|漫游设置|32 KB 字符数|[RoamingSettings](/javascript/api/outlook/office.RoamingSettings) 对象<br/><br/> [context.roamingSettings](../reference/objectmodel/preview-requirement-set/office.context.md#properties) 属性|外接程序的所有漫游设置的限制。 如果设置超出此限制，则所有 Outlook 客户端都将返回错误。|
|正在提取已知实体|2000 个字符|[item.getEntities](../reference/objectmodel/preview-requirement-set/office.context.mailbox.item.md#methods) 方法<br/> <br/>[item.getEntitiesByType](../reference/objectmodel/preview-requirement-set/office.context.mailbox.item.md#methods) 方法<br/> <br/>[item.getFilteredEntitiesByName](../reference/objectmodel/preview-requirement-set/office.context.mailbox.item.md#methods) 方法|在项目正文上提取常见实体的 Exchange Server 限制。 Exchange Server 将忽略超过该限制的实体。 请注意，此限制与外接程序是否使用 `ItemHasKnownEntity` 规则无关。|
|Exchange Web 服务|1 MB 字符数|[mailbox.makeEwsRequestAsync](../reference/objectmodel/preview-requirement-set/office.context.mailbox.md#methods) 方法|对呼叫的请求或响应的限制 `Mailbox.makeEwsRequestAsync` 。|
|收件人|100 位收件人|[item.requiredAttendees](../reference/objectmodel/preview-requirement-set/office.context.mailbox.item.md#properties) 属性<br/> <br/>[item.optionalAttendees](../reference/objectmodel/preview-requirement-set/office.context.mailbox.item.md#properties) 属性<br/> <br/>[item.to](../reference/objectmodel/preview-requirement-set/office.context.mailbox.item.md#properties) 属性<br/> <br/>[item.cc](../reference/objectmodel/preview-requirement-set/office.context.mailbox.item.md#properties) 属性<br/> <br/>[Recipients.addAsync](/javascript/api/outlook/office.Recipients#addasync-recipients--options--callback-) 方法<br/> <br/>[Recipient.getAsync](/javascript/api/outlook/office.Recipients#getasync-options--callback-) 方法<br/> <br/>[Recipient.setAsync](/javascript/api/outlook/office.Recipients#setasync-recipients--options--callback-) 方法|在每个属性中指定的对收件人的限制。|
|显示名称|255 个字符|[EmailAddressDetails.displayName](/javascript/api/outlook/office.emailaddressdetails#displayname) 属性<br/><br/> [Recipients](/javascript/api/outlook/office.Recipients) 对象<br/><br/> `item.requiredAttendees` 财产<br/><br/> `item.optionalAttendees` 财产 <br/><br/>`item.to` 财产 <br/><br/>`item.cc` 财产|约会或邮件中显示名称的长度限制。|
|设置主题|255 个字符|[Mailbox.displayNewAppointmentForm](../reference/objectmodel/preview-requirement-set/office.context.mailbox.md#methods) 方法<br/><br/> [Subject.setAsync](/javascript/api/outlook/office.Subject#setasync-subject--options--callback-) 方法|新的约会窗体中的主题限制，或设置约会或邮件主题的限制。|
|设置地点|255 个字符|[Location.setAsync](/javascript/api/outlook/office.Location#setasync-location--options--callback-) 方法|设置约会或会议请求地点的限制。|
|新的约会窗体的正文|32 KB 字符数|`Mailbox.displayNewAppointmentForm` 种|新的约会窗体中正文的限制。|
|显示现有项目的正文|32 KB 字符数|[mailbox.displayAppointmentForm](../reference/objectmodel/preview-requirement-set/office.context.mailbox.md#methods) 方法<br/><br/> [mailbox.displayMessageForm](../reference/objectmodel/preview-requirement-set/office.context.mailbox.md#methods) 方法|对于 web 和移动设备上的 Outlook：限制现有约会或邮件窗体中的正文。|
|设置正文|1 MB 字符数|[Body.prependAsync](/javascript/api/outlook/office.Body#prependasync-data--options--callback-) 方法<br/> <br/>[Body.setAsync](/javascript/api/outlook/office.Body#setasync-data--options--callback-)<br/><br/>[Body.setSelectedDataAsync](/javascript/api/outlook/office.Body#setselecteddataasync-data--options--callback-) 方法|设置约会或邮件项目正文的限制。|
|附件数|499 web 和移动设备上的 Outlook 文件|[item.addFileAttachmentAsync](../reference/objectmodel/preview-requirement-set/office.context.mailbox.item.md#methods) 方法|限制可附加到发送项目的文件数量。 Web 和移动设备上的 Outlook 通常会通过用户界面和访问最多499个文件的附件 `addFileAttachmentAsync` 。 Outlook 富客户端不具体限制文件附件的数量。 但是，所有 Outlook 客户端都会看到用户的 Exchange 服务器已配置的附件大小的限制。 请查看下一行获取“附件大小”信息。|
|附件大小|取决于 Exchange Server|`item.addFileAttachmentAsync` 种|对项目所有附件的大小有限制，管理员可以在用户邮箱的 Exchange Server 上配置此限制。对于 Outlook 富客户端，这限制了项目的附件数量。 对于 web 和移动设备上的 Outlook，这两个限制中的较小者（附件数和所有附件的大小）限制项目的实际附件。|
|附件的文件名|255 个字符|`item.addFileAttachmentAsync` 种|要添加到项目的附件的文件名长度限制。|
|附件的 URI|2048 个字符|`item.addFileAttachmentAsync` 种|要添加为项目附件的文件名 URI 的限制。|
|附件 ID|100 个字符|[item.addItemAttachmentAsync](../reference/objectmodel/preview-requirement-set/office.context.mailbox.item.md#methods) 方法<br/><br/> [item.removeAttachmentAsync](../reference/objectmodel/preview-requirement-set/office.context.mailbox.item.md#methods) 方法|要添加或从项目中删除的附件 ID 的长度限制。|
|异步调用|3 次调用|`item.addFileAttachmentAsync` 种<br/><br/>`item.addItemAttachmentAsync` 种<br/><br/><br/>`item.removeAttachmentAsync` 种<br/><br/> [Body.getTypeAsync](/javascript/api/outlook/office.Body#gettypeasync-options--callback-) 方法<br/><br/>`Body.prependAsync` 种<br/><br/>`Body.setSelectedDataAsync` 种<br/><br/> [CustomProperties.saveAsync](/javascript/api/outlook/office.CustomProperties#saveasync-callback--asynccontext-) 方法<br/><br/><br/> [item.LoadCustomPropertiesAysnc](../reference/objectmodel/preview-requirement-set/office.context.mailbox.item.md#methods) 方法<br/><br/><br/> [Location.getAsync](/javascript/api/outlook/office.Location#getasync-options--callback-) 方法<br/><br/>`Location.setAsync` 种<br/><br/> [mailbox.getCallbackTokenAsync](../reference/objectmodel/preview-requirement-set/office.context.mailbox.md#methods) 方法<br/><br/> [mailbox.getUserIdentityTokenAsync](../reference/objectmodel/preview-requirement-set/office.context.mailbox.md#methods) 方法<br/><br/> [mailbox.makeEwsRequestAsync](../reference/objectmodel/preview-requirement-set/office.context.mailbox.md#methods) 方法<br/><br/>`Recipients.addAsync` 种<br/><br/> [Recipients.getAsync](/javascript/api/outlook/office.Recipients#getasync-options--callback-) 方法<br/><br/>`Recipients.setAsync` 种<br/><br/> [RoamingSettings.saveAsync](/javascript/api/outlook/office.RoamingSettings#saveasync-callback-) 方法<br/><br/> [Subject.getAsync](/javascript/api/outlook/office.Subject#getasync-options--callback-) 方法<br/><br/>`Subject.setAsync` 种<br/><br/> [Time.getAsync](/javascript/api/outlook/office.Time#getasync-options--callback-) 方法<br/><br/> [Time.setAsync](/javascript/api/outlook/office.Time#setasync-datetime--options--callback-) 方法|对于 web 或移动设备上的 Outlook：在任意时刻对并发异步呼叫数的限制，因为浏览器只允许对服务器进行有限数量的异步调用。 |

## <a name="see-also"></a>另请参阅

- [部署和安装 Outlook 加载项以进行测试](testing-and-tips.md)
- [Outlook 外接程序的隐私、权限和安全性](../concepts/privacy-and-security.md)
