---
title: 清单文件中的 Hosts 元素
description: 指定将在其中激活 Office 外接程序的 Office 客户端应用程序。
ms.date: 10/09/2018
localization_priority: Normal
ms.openlocfilehash: 037ac2b5fedbfb1b59b7523382574942fe59a00a
ms.sourcegitcommit: be23b68eb661015508797333915b44381dd29bdb
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/08/2020
ms.locfileid: "44611804"
---
# <a name="hosts-element"></a>Hosts 元素

指定将在其中激活 Office 外接程序的 Office 客户端应用程序。包含 **Host** 元素及其设置的集合。 

当该元素被包括在 [VersionOverrides](versionoverrides.md)(#versionoverrides) 节点中时，它将替代清单中父级部分中的 **Hosts** 元素。 

## <a name="child-elements"></a>子元素

|  元素 |  必需  |  Description  |
|:-----|:-----|:-----|
|  [Host](host.md)    |  是   |  说明主机及其设置。 |
