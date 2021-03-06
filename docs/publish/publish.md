---
title: 部署和发布 Office 加载项
description: 部署 Office 加载项以进行测试或分发给用户的方法和选项。
ms.date: 06/02/2020
localization_priority: Priority
ms.openlocfilehash: 7792b2196b686f36b81869ce81a3e3b87659f1c2
ms.sourcegitcommit: 9609bd5b4982cdaa2ea7637709a78a45835ffb19
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/28/2020
ms.locfileid: "47292391"
---
# <a name="deploy-and-publish-office-add-ins"></a>部署和发布 Office 加载项

可以使用几种方法之一来部署 Office 外接程序，以用于对用户进行测试或分发：

|**方法**|**Use...**|
|:---------|:------------|
|[旁加载](../testing/test-debug-office-add-ins.md#sideload-an-office-add-in-for-testing)|在开发过程中测试在 Windows、iPad、Mac 或浏览器中运行的加载项。 （不适用于生产版加载项。）|
|[网络共享](../testing/create-a-network-shared-folder-catalog-for-task-pane-and-content-add-ins.md)|作为开发过程的一部分，用于在将加载项发布到本地主机以外的服务器后，测试在 Windows 上运行的加载项。 （不适用于生产版加载项，也不适用于在 iPad、Mac 或 Web 上进行测试。）|
|[集中部署](centralized-deployment.md)|在云部署中，使用 Microsoft 365 管理中心将加载项分发给组织中的用户。|
|[SharePoint 目录](publish-task-pane-and-content-add-ins-to-an-add-in-catalog.md)|在本地环境中，用于向组织用户分发加载项。|
|[AppSource](/office/dev/store/submit-to-appsource-via-partner-center)|用于向用户公开分发加载项。|
|[Exchange 服务器](#outlook-add-in-deployment)|在本地或在线环境中，用于向用户分发 Outlook 加载项。|

[!INCLUDE [publish policies note](../includes/note-publish-policies.md)]

## <a name="deployment-options-by-office-application-and-add-in-type"></a>按 Office 应用程序和加载项类型划分的部署选项

可用的部署选项具体取决于你面向的 Office 应用程序以及所创建的加载项的类型。

### <a name="deployment-options-for-word-excel-and-powerpoint-add-ins"></a>Word、Excel 和 PowerPoint 加载项的部署选项

| 扩展点 | 旁加载 | 网络共享 | Microsoft 365 管理中心 |AppSource   | SharePoint 目录\* |
|:----------------|:-----------:|:-------------:|:-----------------------:|:----------:|:--------------------:|
| 内容         | X           | X             | X                       | X          | X                    |
| 任务窗格       | X           | X             | X                       | X          | X                    |
| 命令         | X           | X             | X                       | X          |                      |

&#42; SharePoint 目录不支持 Mac 版 Office。

### <a name="deployment-options-for-outlook-add-ins"></a>Outlook 加载项的部署选项

| 扩展点 | 旁加载 | Exchange 服务器 | AppSource    |
|:----------------|:-----------:|:---------------:|:------------:|
| 邮件应用        | X           | X               | X            |
| 命令         | X           | X               | X            |

## <a name="production-deployment-methods"></a>产品部署方法

以下各部分提供了有关向组织中的用户分发生产版 Office 加载项的最常用部署方法的其他信息。

有关最终用户如何获取、插入和运行加载项的信息，请参阅[开始使用 Office 加载项](https://support.office.com/article/start-using-your-office-add-in-82e665c4-6700-4b56-a3f3-ef5441996862)。

### <a name="centralized-deployment-via-the-microsoft-365-admin-center"></a>通过 Microsoft 365 管理中心进行集中部署

通过 Microsoft 365 管理中心，管理员可以为组织中的用户和组轻松部署 Office 加载项。 通过管理中心部署加载项后，用户可立即在其 Office 应用程序中使用此加载项，而无需进行客户端配置。 可以通过集中部署来部署内部加载项以及 ISV 提供的加载项。

有关详细信息，请参阅[通过 Microsoft 365 管理中心进行集中部署来发布 Office 加载项](centralized-deployment.md)。

### <a name="sharepoint-app-catalog-deployment"></a>SharePoint 应用目录部署

SharePoint 应用目录是特殊网站集，创建后可用于托管 Word、Excel 和 PowerPoint 加载项。由于 SharePoint 目录不支持在清单的 `VersionOverrides` 节点中实现的新加载项功能（包括加载项命令），因此建议尽可能通过管理中心进行集中部署。通过 SharePoint 目录部署的加载项命令默认在任务窗格中打开。

如果要在本地环境中部署外接程序，请使用 SharePoint 目录。有关详细信息，请参阅[将任务窗格和内容外接程序发布到 SharePoint 目录](publish-task-pane-and-content-add-ins-to-an-add-in-catalog.md)。

> [!NOTE]
> SharePoint 目录不支持 Mac 版 Office。 若要向 Mac 客户端部署 Office 加载项，必须将其提交到 [AppSource](/office/dev/store/submit-to-the-office-store)。

### <a name="outlook-add-in-deployment"></a>Outlook 加载项部署

对于不使用 Azure AD 标识服务的本地和联机环境，可以通过 Exchange 服务器部署 Outlook 外接程序。

Outlook 外接程序部署需要以下内容：

- Microsoft 365、Exchange Online 或 Exchange Server 2013 或更高版本
- Outlook 2013 或更高版本

若要将加载项分配给租户，请使用 Exchange 管理中心通过文件或 URL 直接上传清单，或从 AppSource 添加加载项。若要将加载项分配给单个用户，必须使用 Exchange PowerShell。有关详细信息，请参阅 TechNet 上的[为组织安装或删除 Outlook 加载项](https://technet.microsoft.com/library/jj943752(v=exchg.150).aspx)。

## <a name="see-also"></a>另请参阅

- [旁加载 Outlook 加载项以供测试](../testing/create-a-network-shared-folder-catalog-for-task-pane-and-content-add-ins.md)
- [提交到 AppSource][AppSource]
- [Office 加载项的设计准则](../design/add-in-design.md)
- [创建有效的 AppSource 一览](/office/dev/store/create-effective-office-store-listings)
- [排查 Office 加载项中的用户错误](../testing/testing-and-troubleshooting.md)

[AppSource]: /office/dev/store/submit-to-appsource-via-partner-center
