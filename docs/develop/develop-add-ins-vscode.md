---
title: 使用 Visual Studio Code 开发 Office 加载项
description: 如何使用 Visual Studio Code 开发 Office 加载项。
ms.date: 10/14/2020
localization_priority: Priority
ms.openlocfilehash: 7bd12f6d6e4aff6e8a80f9e9c2e5042b726eca0c
ms.sourcegitcommit: 42e6cfe51d99d4f3f05a3245829d764b28c46bbb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/23/2020
ms.locfileid: "48741097"
---
# <a name="develop-office-add-ins-with-visual-studio-code"></a>使用 Visual Studio Code 开发 Office 加载项

本文介绍如何使用 [Visual Studio Code (VS Code)](https://code.visualstudio.com) 开发 Office 加载项。

> [!NOTE]
> 要了解如何使用 Visual Studio 创建 Office 加载项，请参阅[使用 Visual Studio 开发 Office 加载项](develop-add-ins-visual-studio.md)。

## <a name="prerequisites"></a>先决条件

- [Visual Studio Code](https://code.visualstudio.com/)

[!include[Yeoman generator prerequisites](../includes/quickstart-yo-prerequisites.md)]

## <a name="create-the-add-in-project-using-the-yeoman-generator"></a>使用 Yeoman 生成器创建加载项项目

如果你正在将 VS Code 用作集成开发环境 (IDE)，则应使用[适用于 Office 加载项的 Yeoman 生成器](https://github.com/OfficeDev/generator-office)来创建 Office 加载项项目。Yeoman 生成器会创建一个 Node.js 项目，它可通过 VS Code 或任何其他编辑器进行管理。 

要使用 Yeoman 生成器创建 Office 加载项，请按照 [5 分钟快速入门](/office/dev/add-ins/)中与你要创建的加载项类型相对应的说明进行操作。

## <a name="develop-the-add-in-using-vs-code"></a>使用 VS Code 开发加载项

在 Yeoman 生成器完成加载项项目的创建后，请使用 VS Code 打开项目的根文件夹。 

> [!TIP]
> 在 Windows 上，可通过命令行导航到项目的根目录，然后输入 `code .`在 VS Code 中打开该文件夹。 在 Mac 上，需要先[将 `code` 命令添加到路径中](https://code.visualstudio.com/docs/setup/mac#_launching-from-the-command-line)，然后才可使用该命令在 VS Code 中打开项目文件夹。

Yeoman 生成器会创建一个功能受限的基本加载项。 你可通过在 VS Code 中编辑[清单](add-in-manifests.md)HTML、JavaScript/TypeScript 和 CSS 文件，自定义该加载项。 要简要了解 Yeoman 生成器创建的加载项项目中的项目结构和文件，请查看 [5 分钟快速入门](/office/dev/add-ins/)中与你创建的加载项类型相对应的 Yeoman 生成器指南。

## <a name="test-and-debug-the-add-in"></a>测试和调试加载项

用于测试、调试和故障排除 Office 加载项的方法因平台而异。 有关详细信息，请参阅 [测试和调试 Office 加载项](../testing/test-debug-office-add-ins.md)。

## <a name="publish-the-add-in"></a>发布加载项

[!include[instructions for publishing an Office Add-in](../includes/publish-add-in.md)]

## <a name="see-also"></a>另请参阅

- [Office 加载项的核心概念](../overview/core-concepts-office-add-ins.md)
- [开发 Office 加载项](../develop/develop-overview.md)
- [设计 Office 加载项](../design/add-in-design.md)
- [测试和调试 Office 加载项](../testing/test-debug-office-add-ins.md)
- [发布 Office 加载项](../publish/publish.md)
