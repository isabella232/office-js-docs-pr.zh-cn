---
title: 从自定义函数调用 Microsoft Excel Api
description: 了解可以从自定义函数调用的 Microsoft Excel Api。
ms.date: 02/06/2020
localization_priority: Normal
ms.openlocfilehash: 2f24f8fc27db65466cb586307d7f4bc8f8eefe20
ms.sourcegitcommit: dd6d00202f6466c27418247dad7bd136555a6036
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/26/2020
ms.locfileid: "42284118"
---
# <a name="call-microsoft-excel-apis-from-a-custom-function"></a><span data-ttu-id="d8ff4-103">从自定义函数调用 Microsoft Excel Api</span><span class="sxs-lookup"><span data-stu-id="d8ff4-103">Call Microsoft Excel APIs from a custom function</span></span>

[!include[Running custom functions in a shared runtime note](../includes/excel-shared-runtime-preview-note.md)]

<span data-ttu-id="d8ff4-104">从自定义函数中调用 node.js Excel Api，以获取范围数据并获取更多用于计算的上下文。</span><span class="sxs-lookup"><span data-stu-id="d8ff4-104">Call Office.js Excel APIs from your custom functions to get range data and obtain more context for your calculations.</span></span>

<span data-ttu-id="d8ff4-105">在以下情况中，通过自定义函数调用 node.js Api 可能很有用：</span><span class="sxs-lookup"><span data-stu-id="d8ff4-105">Calling Office.js APIs through a custom function can be helpful when:</span></span>

- <span data-ttu-id="d8ff4-106">自定义函数需要在计算之前从 Excel 中获取信息。</span><span class="sxs-lookup"><span data-stu-id="d8ff4-106">A custom function needs to get information from Excel before calculation.</span></span> <span data-ttu-id="d8ff4-107">此信息可能包括文档属性、范围格式、自定义 XML 部件、工作簿名称或其他特定于 Excel 的信息。</span><span class="sxs-lookup"><span data-stu-id="d8ff4-107">This information might include document properties, range formats, custom XML parts, a workbook name, or other Excel-specific information.</span></span>
- <span data-ttu-id="d8ff4-108">自定义函数将在计算后设置单元格的返回值的数字格式。</span><span class="sxs-lookup"><span data-stu-id="d8ff4-108">A custom function will set the cell's number format for the return values after calculation.</span></span>

[!include[Excel shared runtime note](../includes/note-requires-shared-runtime.md)]

## <a name="code-sample"></a><span data-ttu-id="d8ff4-109">代码示例</span><span class="sxs-lookup"><span data-stu-id="d8ff4-109">Code sample</span></span>

<span data-ttu-id="d8ff4-110">若要调入到 node.js Api，首先需要一个上下文。</span><span class="sxs-lookup"><span data-stu-id="d8ff4-110">To call into the Office.js APIs you first need a context.</span></span> <span data-ttu-id="d8ff4-111">使用`Excel.RequestContext`对象获取上下文。</span><span class="sxs-lookup"><span data-stu-id="d8ff4-111">Use the `Excel.RequestContext` object to get a context.</span></span> <span data-ttu-id="d8ff4-112">然后，使用上下文调用工作簿中所需的 Api。</span><span class="sxs-lookup"><span data-stu-id="d8ff4-112">Then use the context to call the APIs you need in the workbook.</span></span>

<span data-ttu-id="d8ff4-113">下面的代码示例演示如何从工作簿中获取值的范围。</span><span class="sxs-lookup"><span data-stu-id="d8ff4-113">The following code sample shows how to get a range of values from the workbook.</span></span>

```JavaScript
/**
 * @customfunction
 * @param address range's address
 **/
async function getRangeValue (address) {
 var context = new Excel.RequestContext();
 var range = context.workbook.worksheets.getActiveWorksheet().getRange(address);
 range.load();
 await context.sync();
 return range.values[0][0];
}
```

## <a name="limitations-of-calling-officejs-through-a-custom-function"></a><span data-ttu-id="d8ff4-114">通过自定义函数调用 node.js 的限制</span><span class="sxs-lookup"><span data-stu-id="d8ff4-114">Limitations of calling Office.js through a custom function</span></span>

<span data-ttu-id="d8ff4-115">请勿从更改 Excel 环境的自定义函数中调用 node.js Api。</span><span class="sxs-lookup"><span data-stu-id="d8ff4-115">Don't call Office.js APIs from a custom function that change the environment of Excel.</span></span> <span data-ttu-id="d8ff4-116">这意味着您的自定义函数不应执行以下任一操作：</span><span class="sxs-lookup"><span data-stu-id="d8ff4-116">This means your custom functions should not do any of the following:</span></span>

- <span data-ttu-id="d8ff4-117">插入、删除或格式化电子表格中的单元格。</span><span class="sxs-lookup"><span data-stu-id="d8ff4-117">Insert, delete, or format cells on the spreadsheet.</span></span>
- <span data-ttu-id="d8ff4-118">更改其他单元格的值。</span><span class="sxs-lookup"><span data-stu-id="d8ff4-118">Change another cell's value.</span></span>
- <span data-ttu-id="d8ff4-119">将工作表移动、重命名、删除或添加到工作簿中。</span><span class="sxs-lookup"><span data-stu-id="d8ff4-119">Move, rename, delete, or add sheets to a workbook.</span></span>
- <span data-ttu-id="d8ff4-120">更改任何环境选项，如计算模式或屏幕视图。</span><span class="sxs-lookup"><span data-stu-id="d8ff4-120">Change any of the environment options, such as calculation mode or screen views.</span></span>
- <span data-ttu-id="d8ff4-121">将名称添加到工作簿中。</span><span class="sxs-lookup"><span data-stu-id="d8ff4-121">Add names to a workbook.</span></span>
- <span data-ttu-id="d8ff4-122">设置属性或执行大多数方法。</span><span class="sxs-lookup"><span data-stu-id="d8ff4-122">Set properties or execute most methods.</span></span>

<span data-ttu-id="d8ff4-123">更改 Excel 可能导致性能下降、超时和无限循环。</span><span class="sxs-lookup"><span data-stu-id="d8ff4-123">Changing Excel can result in poor performance, time outs, and infinite loops.</span></span> <span data-ttu-id="d8ff4-124">在 Excel 重新计算发生时，不应运行自定义函数计算，因为这会导致不可预测的结果。</span><span class="sxs-lookup"><span data-stu-id="d8ff4-124">Custom function calculations shouldn't run while an Excel recalculation is taking place as it will result in unpredictable results.</span></span>

<span data-ttu-id="d8ff4-125">而是在功能区按钮或任务窗格的上下文中对 Excel 进行更改。</span><span class="sxs-lookup"><span data-stu-id="d8ff4-125">Instead, make changes to Excel from the context of a ribbon button, or task pane.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d8ff4-126">后续步骤</span><span class="sxs-lookup"><span data-stu-id="d8ff4-126">Next steps</span></span>

- [<span data-ttu-id="d8ff4-127">Excel JavaScript API 基本编程概念</span><span class="sxs-lookup"><span data-stu-id="d8ff4-127">Fundamental programming concepts with the Excel JavaScript API</span></span>](../reference/overview/excel-add-ins-reference-overview.md)

## <a name="see-also"></a><span data-ttu-id="d8ff4-128">另请参阅</span><span class="sxs-lookup"><span data-stu-id="d8ff4-128">See also</span></span>

- [<span data-ttu-id="d8ff4-129">在 Excel 自定义函数和任务窗格教程之间共享数据和事件教程</span><span class="sxs-lookup"><span data-stu-id="d8ff4-129">Share data and events between Excel custom functions and task pane tutorial</span></span>](../tutorials/share-data-and-events-between-custom-functions-and-the-task-pane-tutorial.md)