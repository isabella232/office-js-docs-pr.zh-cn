---
title: 在 Outlook 加载项中启用代理访问方案
description: 简要介绍了代理访问权限，并讨论了如何配置加载项支持。
ms.date: 09/30/2020
localization_priority: Normal
ms.openlocfilehash: 68e9c8003f8d223a591283fd1a73f0a38bd3c8a4
ms.sourcegitcommit: 6c3a04acde57832feeaaa599148f93af7e3e36ea
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/02/2020
ms.locfileid: "48336417"
---
# <a name="enable-delegate-access-scenarios-in-an-outlook-add-in"></a>在 Outlook 加载项中启用代理访问方案

邮箱所有者可以使用代理访问功能，以 [允许其他人管理其邮件和日历](https://support.office.com/article/allow-someone-else-to-manage-your-mail-and-calendar-41c40c04-3bd1-4d22-963a-28eafec25926)。 本文指定 Office JavaScript API 支持的代理权限，并介绍如何在 Outlook 外接程序中启用代理访问方案。

> [!IMPORTANT]
> 目前在 Android 和 iOS 上的 Outlook 中不提供委派访问权限。 此外，此功能当前不适用于 web 上的 Outlook 中的 [组共享邮箱](/microsoft-365/admin/create-groups/compare-groups?view=o365-worldwide&preserve-view=true#shared-mailboxes) 。 将来可提供此功能。
>
> 对此功能的支持是在要求集1.8 中引入的。 请查看支持此要求集的[客户端和平台](../reference/requirement-sets/outlook-api-requirement-sets.md#requirement-sets-supported-by-exchange-servers-and-outlook-clients)。

## <a name="supported-permissions-for-delegate-access"></a>代理访问支持的权限

下表介绍了 Office JavaScript API 支持的代理权限。

|权限|值|说明|
|---|---:|---|
|阅读|1 (000001) |可以读取项目。|
|写入|2 (000010) |可以创建项目。|
|DeleteOwn|4 (000100) |只能删除其创建的项目。|
|DeleteAll|8 (001000) |可以删除任何项目。|
|EditOwn|16 (010000) |只能编辑其创建的项目。|
|EditAll|32 (100000) |可以编辑任何项目。|

> [!NOTE]
> 目前，API 支持获取现有的代理权限，但不支持设置委派权限。

使用位掩码来实现 [DelegatePermissions](/javascript/api/outlook/office.mailboxenums.delegatepermissions) 对象，以指示代理的权限。 位掩码中的每个位置都代表一个特定权限，如果将其设置为，则 `1` 代理具有相应的权限。 例如，如果右边的第二位是 `1` ，则代理具有 " **写入** " 权限。 您可以在本文后面的 "将 [操作作为代理执行操作](#perform-an-operation-as-delegate) " 一节中查看有关如何检查特定权限的示例。

## <a name="sync-across-mailbox-clients"></a>在邮箱客户端之间同步

代理对所有者邮箱的更新通常会在邮箱之间立即同步。

但是，如果 REST 或 Exchange Web 服务 (EWS) 操作用于设置项的扩展属性，则此类更改可能需要几个小时才能同步。我们建议您改为使用 [CustomProperties](/javascript/api/outlook/office.customproperties) 对象和相关 api 以避免此类延迟。 若要了解详细信息，请参阅 "在 Outlook 外接程序中获取和设置元数据" 一文中的 " [自定义属性" 部分](metadata-for-an-outlook-add-in.md#custom-data-per-item-in-a-mailbox-custom-properties) 。

> [!IMPORTANT]
> 在委托方案中，不能将 EWS 与 office.js API 当前提供的令牌结合使用。

## <a name="configure-the-manifest"></a>配置清单

若要在外接程序中启用代理访问方案，必须在[SupportsSharedFolders](../reference/manifest/supportssharedfolders.md) `true` 父元素下的清单中将 SupportsSharedFolders 元素设置为 `DesktopFormFactor` 。 目前，其他外观因素不受支持。

若要支持来自代理的 REST 调用，请将清单中的 " [权限](../reference/manifest/permissions.md) " 节点设置为 `ReadWriteMailbox` 。

下面的示例演示 `SupportsSharedFolders` `true` 在清单的部分中设置的元素。

```XML
...
<VersionOverrides xmlns="http://schemas.microsoft.com/office/mailappversionoverrides" xsi:type="VersionOverridesV1_0">
  <VersionOverrides xmlns="http://schemas.microsoft.com/office/mailappversionoverrides/1.1" xsi:type="VersionOverridesV1_1">
    ...
    <Hosts>
      <Host xsi:type="MailHost">
        <DesktopFormFactor>
          <SupportsSharedFolders>true</SupportsSharedFolders>
          <FunctionFile resid="residDesktopFuncUrl" />
          <ExtensionPoint xsi:type="MessageReadCommandSurface">
            <!-- configure selected extension point -->
          </ExtensionPoint>

          <!-- You can define more than one ExtensionPoint element as needed -->

        </DesktopFormFactor>
      </Host>
    </Hosts>
    ...
  </VersionOverrides>
</VersionOverrides>
...
```

## <a name="perform-an-operation-as-delegate"></a>将操作作为代理执行

可以通过调用 [getSharedPropertiesAsync](../reference/objectmodel/preview-requirement-set/office.context.mailbox.item.md#methods) 方法，在撰写或阅读模式下获取项目的共享属性。 这将返回一个 [SharedProperties](/javascript/api/outlook/office.sharedproperties) 对象，该对象当前提供代理的权限、所有者的电子邮件地址、REST API 的基 URL 和目标邮箱。

下面的示例展示了如何获取邮件或约会的共享属性、检查代理是否具有 **写入** 权限，以及如何发出 REST 调用。

```js
function performOperation() {
  Office.context.mailbox.getCallbackTokenAsync({
      isRest: true
    },
    function (asyncResult) {
      if (asyncResult.status === Office.AsyncResultStatus.Succeeded && asyncResult.value !== "") {
        Office.context.mailbox.item.getSharedPropertiesAsync({
            // Pass auth token along.
            asyncContext: asyncResult.value
          },
          function (asyncResult1) {
            let sharedProperties = asyncResult1.value;
            let delegatePermissions = sharedProperties.delegatePermissions;

            // Determine if user can do the expected operation.
            // E.g., do they have Write permission?
            if ((delegatePermissions & Office.MailboxEnums.DelegatePermissions.Write) != 0) {
              // Construct REST URL for your operation.
              // Update <version> placeholder with actual Outlook REST API version e.g. "v2.0".
              // Update <operation> placeholder with actual operation.
              let rest_url = sharedProperties.targetRestUrl + "/<version>/users/" + sharedProperties.targetMailbox + "/<operation>";
  
              $.ajax({
                  url: rest_url,
                  dataType: 'json',
                  headers:
                  {
                    "Authorization": "Bearer " + asyncResult1.asyncContext
                  }
                }
              ).done(
                function (response) {
                  console.log("success");
                }
              ).fail(
                function (error) {
                  console.log("error message");
                }
              );
            }
          }
        );
      }
    }
  );
}
```

> [!TIP]
> 作为代理，您可以使用 REST [获取附加到 outlook 项目或组文章的 outlook 邮件的内容](/graph/outlook-get-mime-message#get-mime-content-of-an-outlook-message-attached-to-an-outlook-item-or-group-post)。

## <a name="handle-calling-rest-on-shared-and-non-shared-items"></a>处理共享和非共享项上的呼叫 REST

如果要对某个项目调用 REST 操作（无论该项目是否共享），则可以使用 `getSharedPropertiesAsync` API 来确定该项目是否已共享。 之后，可以使用相应的对象构造该操作的 REST URL。

```js
if (item.getSharedPropertiesAsync) {
  // In Windows, Mac, and the web client, this indicates a shared item so use SharedProperties properties to construct the REST URL.
  // Add-ins don't activate on shared items in mobile so no need to handle.

  // Perform operation for shared item.
} else {
  // In general, this is not a shared item, so construct the REST URL using info from the Call REST APIs article:
  // https://docs.microsoft.com/office/dev/add-ins/outlook/use-rest-api

  // Perform operation for non-shared item.
}
```

## <a name="limitations"></a>限制

根据你的外接程序的方案，在处理委派情况时需要考虑以下几个限制。

### <a name="rest-and-ews"></a>REST 和 EWS

您的外接程序可以使用 REST 而不是 EWS，并且必须将外接程序的权限设置为，以 `ReadWriteMailbox` 启用对所有者邮箱的 rest 访问。

### <a name="message-compose-mode"></a>邮件撰写模式

在邮件撰写模式下，在 web 或 Windows 上的 Outlook 中不支持 [getSharedPropertiesAsync](/javascript/api/outlook/office.messagecompose#getsharedpropertiesasync-options--callback-) ，除非满足以下条件。

1. 所有者至少与委派共享一个邮箱文件夹。
1. 代理将邮件草稿共享到共享文件夹中。

    示例：

    - 代理答复或转发共享文件夹中的电子邮件。
    - 代理保存草稿邮件，然后将其从其自己的 " **草稿** " 文件夹移动到共享文件夹。 委派从共享文件夹打开草稿，然后继续撰写。

邮件发送后，通常会在代理的 " **已发送邮件** " 文件夹中找到。

## <a name="see-also"></a>另请参阅

- [允许其他人管理您的邮件和日历](https://support.office.com/article/allow-someone-else-to-manage-your-mail-and-calendar-41c40c04-3bd1-4d22-963a-28eafec25926)
- [Office 365 中的日历共享](https://support.office.com/article/calendar-sharing-in-office-365-b576ecc3-0945-4d75-85f1-5efafb8a37b4)
- [如何对清单元素进行排序](../develop/manifest-element-ordering.md)
- [掩码 (计算) ](https://en.wikipedia.org/wiki/Mask_(computing))
- [JavaScript 按位运算符](https://www.w3schools.com/js/js_bitwise.asp)