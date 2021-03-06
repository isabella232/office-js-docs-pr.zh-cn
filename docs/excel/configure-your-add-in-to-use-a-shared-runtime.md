---
ms.date: 08/25/2020
title: 将 Excel 加载项配置为共享浏览器运行时
ms.prod: excel
description: 将 Excel 加载项配置为共享浏览器运行时并在同一运行时中运行功能区、任务窗格和自定义函数代码。
localization_priority: Priority
ms.openlocfilehash: be4e79ae54376a9574ffb0669681c2fba7cd158c
ms.sourcegitcommit: ca66ff7462bfdf4ed7ae04f43d1388c24de63bf9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/11/2020
ms.locfileid: "48996275"
---
# <a name="configure-your-excel-add-in-to-use-a-shared-javascript-runtime"></a>将 Excel 加载项配置为使用共享 JavaScript 运行时

[!include[Excel custom functions note](../includes/excel-custom-functions-note.md)]

运行 Windows 版 Excel 或 Mac 版 Excel 时，加载项将在单独的 JavaScript 运行时环境中运行功能区按钮、自定义函数和任务窗格的代码。 这会产生一些局限性，例如无法轻松共享全局数据，也不能从自定义函数访问所有 CORS 功能。

但是，你可以将 Excel 加载项配置为在共享 JavaScript 运行时中共享代码。 这可在加载项中实现更好的协调，并且可从加载项的所有部分访问 DOM 和 CORS。 它还允许在文档打开时运行代码，或在关闭任务窗格后继续运行代码。 若要将加载项配置为使用共享运行时，请按照本文中的说明进行操作。

## <a name="create-the-add-in-project"></a>创建加载项项目

如果要启动新项目，请按照以下步骤使用 Yeoman 生成器创建 Excel 加载项项目。 运行下面的命令，使用下面的答案回答提示问题：

```command line
yo office
```

- 选择项目类型： **Excel 自定义函数加载项项目**
- 选择脚本类型： **JavaScript**
- 你想要如何命名加载项？ **我的 Office 加载项**

![回答 Office 中的提示问题以创建加载项项目的屏幕截图。](../images/yo-office-excel-project.png)

完成此向导后，生成器会创建项目，并安装支持的 Node 组件。

## <a name="configure-the-manifest"></a>配置清单

对于新项目或现有项目，请按照以下步骤将其配置为使用共享运行时。

1. 启动 Visual Studio Code 并打开“ **我的 Office 加载项** ”项目。
2. 打开 **manifest.xml** 文件。
3. 找到 `<VersionOverrides>` 部分并添加以下 `<Runtimes>` 部分。 生存期需要 **较长** ，以便在关闭任务窗格时自定义函数仍可正常工作。 resid 是 `ContosoAddin.Url`，它在后面的资源部分中引用字符串。 可使用所需的任何 resid 值，但它应匹配加载项元素中其他元素的 resid。

   ```xml
   <VersionOverrides xmlns="http://schemas.microsoft.com/office/taskpaneappversionoverrides" xsi:type="VersionOverridesV1_0">
     <Hosts>
       <Host xsi:type="Workbook">
       <Runtimes>
         <Runtime resid="ContosoAddin.Url" lifetime="long" />
       </Runtimes>
       <AllFormFactors>
   ```

4. 在 `<Page>` 元素中，将源位置从 **Functions.Page.Url** 更改为 **ContosoAddin.Url** 。 此 resid 匹配 `<Runtime>` resid 元素。 请注意，如果你没有自定义函数，则不会有 **页面** 条目，可跳过此步骤。

   ```xml
   <AllFormFactors>
   ...
   <Page>
   <SourceLocation resid="ContosoAddin.Url"/>
   </Page>
   ...
   ```

5. 在 `<DesktopFormFactor>` 部分中，将 **FunctionFile** 从 **Commands.Url** 更改为使用 **ContosoAddin.Url** 。 请注意，如果你没有操作命令，则不会有 **FunctionFile** 条目，可跳过此步骤。

   ```xml
   <DesktopFormFactor>
   <GetStarted>
   ...
   </GetStarted>
   <FunctionFile resid="ContosoAddin.Url"/>
   ```

6. 在 `<Action>` 部分中，将源位置从 **Taskpane.Url** 更改为 **ContosoAddin.Url** 。 请注意，如果你没有任务窗格，则不会有 **ShowTaskpane** 操作，可跳过此步骤。

   ```xml
   <Action xsi:type="ShowTaskpane">
   <TaskpaneId>ButtonId1</TaskpaneId>
   <SourceLocation resid="ContosoAddin.Url"/>
   </Action>
   ```

7. 为 **ContosoAddin.Url** 添加新的 **Url id** ，它指向 **taskpane.html** 。

   ```xml
   <bt:Urls>
   <bt:Url id="Functions.Script.Url" DefaultValue="https://localhost:3000/dist/functions.js"/>
   ...
   <bt:Url id="ContosoAddin.Url" DefaultValue="https://localhost:3000/dist/taskpane.html"/>
   ...
   ```

8. 确保 taskpane.html 有一个参考 dist/functions.js file 文件的 `<script>` 标记。 示例如下。

   ```html
   <script type="text/javascript" src="/dist/functions.js" ></script>
   ```

   > [!NOTE]
   > 如果加载项使用 Webpack 和 HtmlWebpackPlugin 插入脚本标记，与 Yeoman 生成器创建的加载项一样（请参阅上面的[创建加载项项目](#create-the-add-in-project)），则必须确保 functions.js 模块包含在 `chunks` 数组中，如下例所示。
   >
   > ```javascript
   > new HtmlWebpackPlugin({
   >     filename: "taskpane.html",
   >     template: "./src/taskpane/taskpane.html",
   >     chunks: ["polyfill", "taskpane", "functions"]
   > }),
   >```

9. 保存更改并重新生成项目。

   ```command line
   npm run build
   ```

## <a name="runtime-lifetime"></a>运行时生存期

添加 `Runtime` 元素时，还需要指定值为 `long` 或 `short` 的生存期。 将此值设置为 `long` 以利用相关功能，例如在文档打开时启动加载项，在关闭任务窗格后继续运行代码，或从自定义函数中使用 CORS 和 DOM。

>[!NOTE]
> 默认生存期值为`short`，但我们建议在 Excel 加载项中使用`long`。如果在此例中将运行时设置为`short`，则当按下某个功能区按钮时，Excel 加载项将启动，但在功能区处理程序运行完毕后，它可能会关闭。 同样，打开任务窗格时，加载项将启动，但在任务窗格关闭时可能会关闭。

```xml
<Runtimes>
  <Runtime resid="ContosoAddin.Url" lifetime="long" />
</Runtimes>
```

>[!NOTE]
> 如果加载启动项包括清单中的 `Runtimes` 元素（共享运行时所需），它将使用 Internet Explorer 11，而不考虑 Windows 或 Microsoft 365 版本。 有关详细信息，请参阅[运行时](../reference/manifest/runtimes.md)。

## <a name="multiple-task-panes"></a>多个任务窗格

如果计划使用共享运行时，请勿将你的加载项设计为使用多个任务窗格。 共享运行时仅支持使用一个任务窗格。 请注意，不含 `<TaskpaneID>` 的任何任务窗格都被视为不同的任务窗格。

## <a name="next-steps"></a>后续步骤

- 有关在共享运行时中使用 Excel JavaScript API 和自定义 Excel 函数的详细信息，请参阅文章[从自定义函数中调用 Excel API](call-excel-apis-from-custom-function.md)。
- 探索模式和实践示例[管理功能区和任务窗格 UI，并在文档打开时运行代码](https://github.com/OfficeDev/PnP-OfficeAddins/tree/master/Samples/excel-shared-runtime-scenario)，以查看活动中的共享 JavaScript 运行时的更大示例。
- 有关向项目添加自定义键盘快捷方式的信息，请阅读 [Office 加载项中的自定义键盘快捷方式](../design/keyboard-shortcuts.md)。

## <a name="see-also"></a>另请参阅

- [概述：在共享 JavaScript 运行时中运行加载项代码](custom-functions-shared-overview.md)
