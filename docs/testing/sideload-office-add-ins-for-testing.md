---
title: 在 Office 网页版中旁加载 Office 加载项进行测试
description: 通过旁加载在 Office 上测试 Office 外接程序（网址为）。
ms.date: 09/24/2020
localization_priority: Normal
ms.openlocfilehash: 91f23200a2c393eb5c79f615765df52f205ac6e1
ms.sourcegitcommit: 09e1d8ff14b3c09a3eb11c91432c224a539181a4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/25/2020
ms.locfileid: "48268563"
---
# <a name="sideload-office-add-ins-in-office-on-the-web-for-testing"></a>在 Office 网页版中旁加载 Office 加载项进行测试

可以通过使用旁加载来安装 Office 加载项以供测试，而无需先将它添加到加载项目录中。 可以在 Microsoft 365 或 web 上的 Office 中完成旁加载。 该过程使用的两个平台略有不同。

当旁加载外接程序时，外接程序清单存储在浏览器的本地存储区中，因此如果清除浏览器的缓存，或切换到另一个浏览器，就必须再次旁加载该外接程序。

> [!NOTE]
> 如本文所述，Word、Excel 和 PowerPoint 支持旁加载。若要旁加载 Outlook 外接程序，请参阅[旁加载 Outlook 外接程序进行测试](../outlook/sideload-outlook-add-ins-for-testing.md)。

下面的视频逐步展示了如何在 Office 网页版或桌面上旁加载加载项。

> [!VIDEO https://www.youtube.com/embed/XXsAw2UUiQo]

## <a name="sideload-an-office-add-in-in-office-on-the-web"></a>在 Office 网页版中旁加载 Office 加载项

1. [在 web 上打开 Office](https://office.live.com/)。

2. 在 **"立即开始使用在线应用程序**" 中，选择 " **Excel**"、" **Word**" 或 " **PowerPoint**";，然后打开一个新文档。

3. 打开功能区上的 " **插入** " 选项卡，然后在 " **外接程序** " 部分中，选择 " **Office 外接程序**"。

4. 在 " **Office 外接程序** " 对话框中，选择 " **我的外** 接程序" 选项卡，选择 " **管理我的外接**程序"，然后 **上传我的外接程序**。

    ![“Office 加载项”对话框，右上方有“管理我的加载项”下拉列表，其中有下拉选项“上传我的加载项”](../images/office-add-ins-my-account.png)

5. **转到**加载项清单文件，再选择“上传”****。

    ![带浏览、上载和取消按钮的上载外接程序对话框。](../images/upload-add-in.png)

6. 验证是否已安装外接程序。例如，如果它是一个外接程序命令，它应显示在功能区或上下文菜单上。如果它是一个任务窗格外接程序，则应显示窗格。

> [!NOTE]
> 若要使用原始 Web 视图 (EdgeHTML) 在 Microsoft Edge 中测试 Office 加载项，需要执行其他配置步骤。 在 Windows 命令提示符下，运行以下行： `npx office-addin-dev-settings appcontainer EdgeWebView --loopback --yes` 。 当 Office 使用基于 Chromium 的边缘 WebView2 时，这不是必需的。 有关详细信息，请参阅 [Office 加载项使用的浏览器](../concepts/browsers-used-by-office-web-add-ins.md)。

## <a name="sideload-an-office-add-in-in-office-365"></a>在 Office 365 上旁加载 Office 加载项

1. 登录到 Microsoft 365 帐户。

2. 打开工具栏左端的应用启动器并选择 " **Excel**"、" **Word**" 或 " **PowerPoint**"，然后创建一个新文档。

3. 步骤 3 - 6 与上一部分**在 Office 网页版中旁加载 Office 加载项**相同。

## <a name="sideload-an-add-in-when-using-visual-studio"></a>使用 Visual Studio 时旁加载加载项

如果使用 Visual Studio 来开发加载项，则旁加载的过程类似。 唯一的区别是，必须更新清单中 **SourceURL** 元素的值以包含部署加载项位置的完整 URL。

> [!NOTE]
> 虽然可以将加载项从 Visual Studio 旁加载到 Office 网页版，但无法从 Visual Studio 调试它们。 若要进行调试，需要使用浏览器调试工具。 有关详细信息，请参阅[在 Office 网页版中调试加载项](debug-add-ins-in-office-online.md)。

1. 在 Visual Studio 中，通过选择**视图** -> **属性窗口**来显示**属性**窗口。
2. 在**解决方案资源管理器**中，选择 Web 项目。 这将在**属性**窗口中显示项目的属性。
3. 在“属性”窗口中复制 **SSL URL**。
4. 在加载项项目中，打开清单 XML 文件。 请确保正在编辑源 XML。 对于某些项目类型，Visual Studio 将打开 XML 的可视视图，它不适用于下一步骤。
5. 使用刚复制的 SSL URL 来搜索和替换 **~remoteAppUrl/** 的所有实例。 将看到多个替换，具体取决于项目类型。将显示新 URL，类似于 `https://localhost:44300/Home.html`。
6. 保存 XML 文件。
7. 右键单击 Web 项目，然后选择**调试** -> **启动新实例**。 这将在不启动 Office 的情况下运行 Web 项目。
8. 从 Office 网页版，使用之前[在 Office 网页版中加载 Office 加载项](#sideload-an-office-add-in-in-office-on-the-web)中所述的步骤旁加载加载项。

## <a name="remove-a-sideloaded-add-in"></a>删除旁加载加载项

您可以通过清除浏览器的缓存来删除以前的旁加载外接程序。 此外，如果您对外接程序的清单进行了更改 (例如，更新) 的加载项命令的图标或文本的文件名，则可能需要清除缓存，然后使用更新的清单重新旁加载加载项。 执行此操作后，Office 将按照更新清单中所述的方式呈现该加载项。
