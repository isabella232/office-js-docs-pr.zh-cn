---
title: Office 命名空间-要求集1。9
description: 使用邮箱 API 要求集1.9 的 Outlook 外接程序可用的 Office 命名空间成员。
ms.date: 10/14/2020
localization_priority: Normal
ms.openlocfilehash: e6a932c528dea692ff5fd7ea8d3e1454bb9a7e03
ms.sourcegitcommit: 4e7c74ad67ea8bf6b47d65b2fde54a967090f65b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/20/2020
ms.locfileid: "48628040"
---
# <a name="office-mailbox-requirement-set-19"></a><span data-ttu-id="d4e7d-103">Office (邮箱要求集 1.9) </span><span class="sxs-lookup"><span data-stu-id="d4e7d-103">Office (Mailbox requirement set 1.9)</span></span>

<span data-ttu-id="d4e7d-p101">该 Office 命名空间提供所有 Office 应用中的加载项所使用的共享接口。此列表仅记录 Outlook 加载项所使用的接口。有关 Office 命名空间的完整列表，请参阅[公用 API](/javascript/api/office)。</span><span class="sxs-lookup"><span data-stu-id="d4e7d-p101">The Office namespace provides shared interfaces that are used by add-ins in all of the Office apps. This listing documents only those interfaces that are used by Outlook add-ins. For a full listing of the Office namespace, see the [Common API](/javascript/api/office).</span></span>

##### <a name="requirements"></a><span data-ttu-id="d4e7d-106">要求</span><span class="sxs-lookup"><span data-stu-id="d4e7d-106">Requirements</span></span>

|<span data-ttu-id="d4e7d-107">要求</span><span class="sxs-lookup"><span data-stu-id="d4e7d-107">Requirement</span></span>| <span data-ttu-id="d4e7d-108">值</span><span class="sxs-lookup"><span data-stu-id="d4e7d-108">Value</span></span>|
|---|---|
|[<span data-ttu-id="d4e7d-109">最低版本的邮箱要求集</span><span class="sxs-lookup"><span data-stu-id="d4e7d-109">Minimum mailbox requirement set version</span></span>](../../requirement-sets/outlook-api-requirement-sets.md)| <span data-ttu-id="d4e7d-110">1.1</span><span class="sxs-lookup"><span data-stu-id="d4e7d-110">1.1</span></span>|
|[<span data-ttu-id="d4e7d-111">适用的 Outlook 模式</span><span class="sxs-lookup"><span data-stu-id="d4e7d-111">Applicable Outlook mode</span></span>](../../../outlook/outlook-add-ins-overview.md#extension-points)| <span data-ttu-id="d4e7d-112">撰写或阅读</span><span class="sxs-lookup"><span data-stu-id="d4e7d-112">Compose or Read</span></span>|

##### <a name="properties"></a><span data-ttu-id="d4e7d-113">属性</span><span class="sxs-lookup"><span data-stu-id="d4e7d-113">Properties</span></span>

| <span data-ttu-id="d4e7d-114">属性</span><span class="sxs-lookup"><span data-stu-id="d4e7d-114">Property</span></span> | <span data-ttu-id="d4e7d-115">型号</span><span class="sxs-lookup"><span data-stu-id="d4e7d-115">Modes</span></span> | <span data-ttu-id="d4e7d-116">返回类型</span><span class="sxs-lookup"><span data-stu-id="d4e7d-116">Return type</span></span> | <span data-ttu-id="d4e7d-117">最小值</span><span class="sxs-lookup"><span data-stu-id="d4e7d-117">Minimum</span></span><br><span data-ttu-id="d4e7d-118">要求集</span><span class="sxs-lookup"><span data-stu-id="d4e7d-118">requirement set</span></span> |
|---|---|---|:---:|
| [<span data-ttu-id="d4e7d-119">context</span><span class="sxs-lookup"><span data-stu-id="d4e7d-119">context</span></span>](office.context.md) | <span data-ttu-id="d4e7d-120">撰写</span><span class="sxs-lookup"><span data-stu-id="d4e7d-120">Compose</span></span><br><span data-ttu-id="d4e7d-121">读取</span><span class="sxs-lookup"><span data-stu-id="d4e7d-121">Read</span></span> | [<span data-ttu-id="d4e7d-122">Context</span><span class="sxs-lookup"><span data-stu-id="d4e7d-122">Context</span></span>](/javascript/api/office/office.context?view=outlook-js-1.9&preserve-view=true) | [<span data-ttu-id="d4e7d-123">1.1</span><span class="sxs-lookup"><span data-stu-id="d4e7d-123">1.1</span></span>](../requirement-set-1.1/outlook-requirement-set-1.1.md) |

##### <a name="enumerations"></a><span data-ttu-id="d4e7d-124">枚举</span><span class="sxs-lookup"><span data-stu-id="d4e7d-124">Enumerations</span></span>

| <span data-ttu-id="d4e7d-125">枚举</span><span class="sxs-lookup"><span data-stu-id="d4e7d-125">Enumeration</span></span> | <span data-ttu-id="d4e7d-126">型号</span><span class="sxs-lookup"><span data-stu-id="d4e7d-126">Modes</span></span> | <span data-ttu-id="d4e7d-127">返回类型</span><span class="sxs-lookup"><span data-stu-id="d4e7d-127">Return type</span></span> | <span data-ttu-id="d4e7d-128">最小值</span><span class="sxs-lookup"><span data-stu-id="d4e7d-128">Minimum</span></span><br><span data-ttu-id="d4e7d-129">要求集</span><span class="sxs-lookup"><span data-stu-id="d4e7d-129">requirement set</span></span> |
|---|---|---|:---:|
| [<span data-ttu-id="d4e7d-130">AsyncResultStatus</span><span class="sxs-lookup"><span data-stu-id="d4e7d-130">AsyncResultStatus</span></span>](#asyncresultstatus-string) | <span data-ttu-id="d4e7d-131">撰写</span><span class="sxs-lookup"><span data-stu-id="d4e7d-131">Compose</span></span><br><span data-ttu-id="d4e7d-132">读取</span><span class="sxs-lookup"><span data-stu-id="d4e7d-132">Read</span></span> | <span data-ttu-id="d4e7d-133">String</span><span class="sxs-lookup"><span data-stu-id="d4e7d-133">String</span></span> | [<span data-ttu-id="d4e7d-134">1.1</span><span class="sxs-lookup"><span data-stu-id="d4e7d-134">1.1</span></span>](../requirement-set-1.1/outlook-requirement-set-1.1.md) |
| [<span data-ttu-id="d4e7d-135">CoercionType</span><span class="sxs-lookup"><span data-stu-id="d4e7d-135">CoercionType</span></span>](#coerciontype-string) | <span data-ttu-id="d4e7d-136">撰写</span><span class="sxs-lookup"><span data-stu-id="d4e7d-136">Compose</span></span><br><span data-ttu-id="d4e7d-137">读取</span><span class="sxs-lookup"><span data-stu-id="d4e7d-137">Read</span></span> | <span data-ttu-id="d4e7d-138">String</span><span class="sxs-lookup"><span data-stu-id="d4e7d-138">String</span></span> | [<span data-ttu-id="d4e7d-139">1.1</span><span class="sxs-lookup"><span data-stu-id="d4e7d-139">1.1</span></span>](../requirement-set-1.1/outlook-requirement-set-1.1.md) |
| [<span data-ttu-id="d4e7d-140">EventType</span><span class="sxs-lookup"><span data-stu-id="d4e7d-140">EventType</span></span>](#eventtype-string) | <span data-ttu-id="d4e7d-141">撰写</span><span class="sxs-lookup"><span data-stu-id="d4e7d-141">Compose</span></span><br><span data-ttu-id="d4e7d-142">读取</span><span class="sxs-lookup"><span data-stu-id="d4e7d-142">Read</span></span> | <span data-ttu-id="d4e7d-143">String</span><span class="sxs-lookup"><span data-stu-id="d4e7d-143">String</span></span> | [<span data-ttu-id="d4e7d-144">1.5</span><span class="sxs-lookup"><span data-stu-id="d4e7d-144">1.5</span></span>](../requirement-set-1.5/outlook-requirement-set-1.5.md) |
| [<span data-ttu-id="d4e7d-145">SourceProperty</span><span class="sxs-lookup"><span data-stu-id="d4e7d-145">SourceProperty</span></span>](#sourceproperty-string) | <span data-ttu-id="d4e7d-146">撰写</span><span class="sxs-lookup"><span data-stu-id="d4e7d-146">Compose</span></span><br><span data-ttu-id="d4e7d-147">读取</span><span class="sxs-lookup"><span data-stu-id="d4e7d-147">Read</span></span> | <span data-ttu-id="d4e7d-148">String</span><span class="sxs-lookup"><span data-stu-id="d4e7d-148">String</span></span> | [<span data-ttu-id="d4e7d-149">1.1</span><span class="sxs-lookup"><span data-stu-id="d4e7d-149">1.1</span></span>](../requirement-set-1.1/outlook-requirement-set-1.1.md) |

### <a name="namespaces"></a><span data-ttu-id="d4e7d-150">命名空间</span><span class="sxs-lookup"><span data-stu-id="d4e7d-150">Namespaces</span></span>

<span data-ttu-id="d4e7d-151">[MailboxEnums](/javascript/api/outlook/office.mailboxenums.attachmentcontentformat?view=outlook-js-1.9&preserve-view=true)：包含许多特定于 Outlook 的枚举，例如、、、、、 `ItemType` `EntityType` `AttachmentType` `RecipientType` `ResponseType` 和 `ItemNotificationMessageType` 。</span><span class="sxs-lookup"><span data-stu-id="d4e7d-151">[MailboxEnums](/javascript/api/outlook/office.mailboxenums.attachmentcontentformat?view=outlook-js-1.9&preserve-view=true): Includes a number of Outlook-specific enumerations, for example, `ItemType`, `EntityType`, `AttachmentType`, `RecipientType`, `ResponseType`, and `ItemNotificationMessageType`.</span></span>

## <a name="enumeration-details"></a><span data-ttu-id="d4e7d-152">枚举详细信息</span><span class="sxs-lookup"><span data-stu-id="d4e7d-152">Enumeration details</span></span>

#### <a name="asyncresultstatus-string"></a><span data-ttu-id="d4e7d-153">AsyncResultStatus： String</span><span class="sxs-lookup"><span data-stu-id="d4e7d-153">AsyncResultStatus: String</span></span>

<span data-ttu-id="d4e7d-154">指定异步调用的结果。</span><span class="sxs-lookup"><span data-stu-id="d4e7d-154">Specifies the result of an asynchronous call.</span></span>

##### <a name="type"></a><span data-ttu-id="d4e7d-155">类型</span><span class="sxs-lookup"><span data-stu-id="d4e7d-155">Type</span></span>

*   <span data-ttu-id="d4e7d-156">String</span><span class="sxs-lookup"><span data-stu-id="d4e7d-156">String</span></span>

##### <a name="properties"></a><span data-ttu-id="d4e7d-157">属性：</span><span class="sxs-lookup"><span data-stu-id="d4e7d-157">Properties:</span></span>

|<span data-ttu-id="d4e7d-158">名称</span><span class="sxs-lookup"><span data-stu-id="d4e7d-158">Name</span></span>| <span data-ttu-id="d4e7d-159">类型</span><span class="sxs-lookup"><span data-stu-id="d4e7d-159">Type</span></span>| <span data-ttu-id="d4e7d-160">说明</span><span class="sxs-lookup"><span data-stu-id="d4e7d-160">Description</span></span>|
|---|---|---|
|`Succeeded`| <span data-ttu-id="d4e7d-161">String</span><span class="sxs-lookup"><span data-stu-id="d4e7d-161">String</span></span>|<span data-ttu-id="d4e7d-162">调用成功。</span><span class="sxs-lookup"><span data-stu-id="d4e7d-162">The call succeeded.</span></span>|
|`Failed`| <span data-ttu-id="d4e7d-163">字符串</span><span class="sxs-lookup"><span data-stu-id="d4e7d-163">String</span></span>|<span data-ttu-id="d4e7d-164">调用失败。</span><span class="sxs-lookup"><span data-stu-id="d4e7d-164">The call failed.</span></span>|

##### <a name="requirements"></a><span data-ttu-id="d4e7d-165">要求</span><span class="sxs-lookup"><span data-stu-id="d4e7d-165">Requirements</span></span>

|<span data-ttu-id="d4e7d-166">要求</span><span class="sxs-lookup"><span data-stu-id="d4e7d-166">Requirement</span></span>| <span data-ttu-id="d4e7d-167">值</span><span class="sxs-lookup"><span data-stu-id="d4e7d-167">Value</span></span>|
|---|---|
|[<span data-ttu-id="d4e7d-168">最低版本的邮箱要求集</span><span class="sxs-lookup"><span data-stu-id="d4e7d-168">Minimum mailbox requirement set version</span></span>](../../requirement-sets/outlook-api-requirement-sets.md)| <span data-ttu-id="d4e7d-169">1.1</span><span class="sxs-lookup"><span data-stu-id="d4e7d-169">1.1</span></span>|
|[<span data-ttu-id="d4e7d-170">适用的 Outlook 模式</span><span class="sxs-lookup"><span data-stu-id="d4e7d-170">Applicable Outlook mode</span></span>](../../../outlook/outlook-add-ins-overview.md#extension-points)| <span data-ttu-id="d4e7d-171">撰写或阅读</span><span class="sxs-lookup"><span data-stu-id="d4e7d-171">Compose or Read</span></span>|

<br>

---
---

#### <a name="coerciontype-string"></a><span data-ttu-id="d4e7d-172">CoercionType： String</span><span class="sxs-lookup"><span data-stu-id="d4e7d-172">CoercionType: String</span></span>

<span data-ttu-id="d4e7d-173">指定如何强制由调用方法返回或设置的数据。</span><span class="sxs-lookup"><span data-stu-id="d4e7d-173">Specifies how to coerce data returned or set by the invoked method.</span></span>

##### <a name="type"></a><span data-ttu-id="d4e7d-174">类型</span><span class="sxs-lookup"><span data-stu-id="d4e7d-174">Type</span></span>

*   <span data-ttu-id="d4e7d-175">String</span><span class="sxs-lookup"><span data-stu-id="d4e7d-175">String</span></span>

##### <a name="properties"></a><span data-ttu-id="d4e7d-176">属性：</span><span class="sxs-lookup"><span data-stu-id="d4e7d-176">Properties:</span></span>

|<span data-ttu-id="d4e7d-177">名称</span><span class="sxs-lookup"><span data-stu-id="d4e7d-177">Name</span></span>| <span data-ttu-id="d4e7d-178">类型</span><span class="sxs-lookup"><span data-stu-id="d4e7d-178">Type</span></span>| <span data-ttu-id="d4e7d-179">说明</span><span class="sxs-lookup"><span data-stu-id="d4e7d-179">Description</span></span>|
|---|---|---|
|`Html`| <span data-ttu-id="d4e7d-180">String</span><span class="sxs-lookup"><span data-stu-id="d4e7d-180">String</span></span>|<span data-ttu-id="d4e7d-181">请求以 HTML 格式返回的数据。</span><span class="sxs-lookup"><span data-stu-id="d4e7d-181">Requests the data be returned in HTML format.</span></span>|
|`Text`| <span data-ttu-id="d4e7d-182">字符串</span><span class="sxs-lookup"><span data-stu-id="d4e7d-182">String</span></span>|<span data-ttu-id="d4e7d-183">请求以文本格式返回的数据。</span><span class="sxs-lookup"><span data-stu-id="d4e7d-183">Requests the data be returned in text format.</span></span>|

##### <a name="requirements"></a><span data-ttu-id="d4e7d-184">要求</span><span class="sxs-lookup"><span data-stu-id="d4e7d-184">Requirements</span></span>

|<span data-ttu-id="d4e7d-185">要求</span><span class="sxs-lookup"><span data-stu-id="d4e7d-185">Requirement</span></span>| <span data-ttu-id="d4e7d-186">值</span><span class="sxs-lookup"><span data-stu-id="d4e7d-186">Value</span></span>|
|---|---|
|[<span data-ttu-id="d4e7d-187">最低版本的邮箱要求集</span><span class="sxs-lookup"><span data-stu-id="d4e7d-187">Minimum mailbox requirement set version</span></span>](../../requirement-sets/outlook-api-requirement-sets.md)| <span data-ttu-id="d4e7d-188">1.1</span><span class="sxs-lookup"><span data-stu-id="d4e7d-188">1.1</span></span>|
|[<span data-ttu-id="d4e7d-189">适用的 Outlook 模式</span><span class="sxs-lookup"><span data-stu-id="d4e7d-189">Applicable Outlook mode</span></span>](../../../outlook/outlook-add-ins-overview.md#extension-points)| <span data-ttu-id="d4e7d-190">撰写或阅读</span><span class="sxs-lookup"><span data-stu-id="d4e7d-190">Compose or Read</span></span>|

<br>

---
---

#### <a name="eventtype-string"></a><span data-ttu-id="d4e7d-191">事件类型： String</span><span class="sxs-lookup"><span data-stu-id="d4e7d-191">EventType: String</span></span>

<span data-ttu-id="d4e7d-192">指定与事件处理程序相关联的事件。</span><span class="sxs-lookup"><span data-stu-id="d4e7d-192">Specifies the event associated with an event handler.</span></span>

##### <a name="type"></a><span data-ttu-id="d4e7d-193">类型</span><span class="sxs-lookup"><span data-stu-id="d4e7d-193">Type</span></span>

*   <span data-ttu-id="d4e7d-194">String</span><span class="sxs-lookup"><span data-stu-id="d4e7d-194">String</span></span>

##### <a name="properties"></a><span data-ttu-id="d4e7d-195">属性：</span><span class="sxs-lookup"><span data-stu-id="d4e7d-195">Properties:</span></span>

| <span data-ttu-id="d4e7d-196">名称</span><span class="sxs-lookup"><span data-stu-id="d4e7d-196">Name</span></span> | <span data-ttu-id="d4e7d-197">类型</span><span class="sxs-lookup"><span data-stu-id="d4e7d-197">Type</span></span> | <span data-ttu-id="d4e7d-198">说明</span><span class="sxs-lookup"><span data-stu-id="d4e7d-198">Description</span></span> | <span data-ttu-id="d4e7d-199">最低要求集</span><span class="sxs-lookup"><span data-stu-id="d4e7d-199">Minimum requirement set</span></span> |
|---|---|---|:---:|
|`AppointmentTimeChanged`| <span data-ttu-id="d4e7d-200">字符串</span><span class="sxs-lookup"><span data-stu-id="d4e7d-200">String</span></span> | <span data-ttu-id="d4e7d-201">所选的约会或系列的日期或时间已更改。</span><span class="sxs-lookup"><span data-stu-id="d4e7d-201">The date or time of the selected appointment or series has changed.</span></span> | <span data-ttu-id="d4e7d-202">1.7</span><span class="sxs-lookup"><span data-stu-id="d4e7d-202">1.7</span></span> |
|`AttachmentsChanged`| <span data-ttu-id="d4e7d-203">字符串</span><span class="sxs-lookup"><span data-stu-id="d4e7d-203">String</span></span> | <span data-ttu-id="d4e7d-204">已将附件添加到项目或已从项目删除附件。</span><span class="sxs-lookup"><span data-stu-id="d4e7d-204">An attachment has been added to or removed from the item.</span></span> | <span data-ttu-id="d4e7d-205">1.8</span><span class="sxs-lookup"><span data-stu-id="d4e7d-205">1.8</span></span> |
|`EnhancedLocationsChanged`| <span data-ttu-id="d4e7d-206">String</span><span class="sxs-lookup"><span data-stu-id="d4e7d-206">String</span></span> | <span data-ttu-id="d4e7d-207">所选约会的位置已更改。</span><span class="sxs-lookup"><span data-stu-id="d4e7d-207">The location of the selected appointment has changed.</span></span> | <span data-ttu-id="d4e7d-208">1.8</span><span class="sxs-lookup"><span data-stu-id="d4e7d-208">1.8</span></span> |
|`ItemChanged`| <span data-ttu-id="d4e7d-209">字符串</span><span class="sxs-lookup"><span data-stu-id="d4e7d-209">String</span></span> | <span data-ttu-id="d4e7d-210">在任务窗格固定时，将选择不同的 Outlook 项进行查看。</span><span class="sxs-lookup"><span data-stu-id="d4e7d-210">A different Outlook item is selected for viewing while the task pane is pinned.</span></span> | <span data-ttu-id="d4e7d-211">1.5</span><span class="sxs-lookup"><span data-stu-id="d4e7d-211">1.5</span></span> |
|`RecipientsChanged`| <span data-ttu-id="d4e7d-212">字符串</span><span class="sxs-lookup"><span data-stu-id="d4e7d-212">String</span></span> | <span data-ttu-id="d4e7d-213">选定项目或约会位置的收件人列表已更改。</span><span class="sxs-lookup"><span data-stu-id="d4e7d-213">The recipient list of the selected item or appointment location has changed.</span></span> | <span data-ttu-id="d4e7d-214">1.7</span><span class="sxs-lookup"><span data-stu-id="d4e7d-214">1.7</span></span> |
|`RecurrenceChanged`| <span data-ttu-id="d4e7d-215">字符串</span><span class="sxs-lookup"><span data-stu-id="d4e7d-215">String</span></span> | <span data-ttu-id="d4e7d-216">选定系列的定期模式已更改。</span><span class="sxs-lookup"><span data-stu-id="d4e7d-216">The recurrence pattern of the selected series has changed.</span></span> | <span data-ttu-id="d4e7d-217">1.7</span><span class="sxs-lookup"><span data-stu-id="d4e7d-217">1.7</span></span> |

##### <a name="requirements"></a><span data-ttu-id="d4e7d-218">要求</span><span class="sxs-lookup"><span data-stu-id="d4e7d-218">Requirements</span></span>

|<span data-ttu-id="d4e7d-219">要求</span><span class="sxs-lookup"><span data-stu-id="d4e7d-219">Requirement</span></span>| <span data-ttu-id="d4e7d-220">值</span><span class="sxs-lookup"><span data-stu-id="d4e7d-220">Value</span></span>|
|---|---|
|[<span data-ttu-id="d4e7d-221">最低版本的邮箱要求集</span><span class="sxs-lookup"><span data-stu-id="d4e7d-221">Minimum mailbox requirement set version</span></span>](../../requirement-sets/outlook-api-requirement-sets.md)| <span data-ttu-id="d4e7d-222">1.5</span><span class="sxs-lookup"><span data-stu-id="d4e7d-222">1.5</span></span> |
|[<span data-ttu-id="d4e7d-223">适用的 Outlook 模式</span><span class="sxs-lookup"><span data-stu-id="d4e7d-223">Applicable Outlook mode</span></span>](../../../outlook/outlook-add-ins-overview.md#extension-points)| <span data-ttu-id="d4e7d-224">撰写或阅读</span><span class="sxs-lookup"><span data-stu-id="d4e7d-224">Compose or Read</span></span>|

<br>

---
---

#### <a name="sourceproperty-string"></a><span data-ttu-id="d4e7d-225">SourceProperty： String</span><span class="sxs-lookup"><span data-stu-id="d4e7d-225">SourceProperty: String</span></span>

<span data-ttu-id="d4e7d-226">指定由调用方法返回的数据源。</span><span class="sxs-lookup"><span data-stu-id="d4e7d-226">Specifies the source of the data returned by the invoked method.</span></span>

##### <a name="type"></a><span data-ttu-id="d4e7d-227">类型</span><span class="sxs-lookup"><span data-stu-id="d4e7d-227">Type</span></span>

*   <span data-ttu-id="d4e7d-228">String</span><span class="sxs-lookup"><span data-stu-id="d4e7d-228">String</span></span>

##### <a name="properties"></a><span data-ttu-id="d4e7d-229">属性：</span><span class="sxs-lookup"><span data-stu-id="d4e7d-229">Properties:</span></span>

|<span data-ttu-id="d4e7d-230">名称</span><span class="sxs-lookup"><span data-stu-id="d4e7d-230">Name</span></span>| <span data-ttu-id="d4e7d-231">类型</span><span class="sxs-lookup"><span data-stu-id="d4e7d-231">Type</span></span>| <span data-ttu-id="d4e7d-232">说明</span><span class="sxs-lookup"><span data-stu-id="d4e7d-232">Description</span></span>|
|---|---|---|
|`Body`| <span data-ttu-id="d4e7d-233">String</span><span class="sxs-lookup"><span data-stu-id="d4e7d-233">String</span></span>|<span data-ttu-id="d4e7d-234">数据源来自邮件的正文。</span><span class="sxs-lookup"><span data-stu-id="d4e7d-234">The source of the data is from the body of a message.</span></span>|
|`Subject`| <span data-ttu-id="d4e7d-235">String</span><span class="sxs-lookup"><span data-stu-id="d4e7d-235">String</span></span>|<span data-ttu-id="d4e7d-236">数据源来自邮件的主题。</span><span class="sxs-lookup"><span data-stu-id="d4e7d-236">The source of the data is from the subject of a message.</span></span>|

##### <a name="requirements"></a><span data-ttu-id="d4e7d-237">要求</span><span class="sxs-lookup"><span data-stu-id="d4e7d-237">Requirements</span></span>

|<span data-ttu-id="d4e7d-238">要求</span><span class="sxs-lookup"><span data-stu-id="d4e7d-238">Requirement</span></span>| <span data-ttu-id="d4e7d-239">值</span><span class="sxs-lookup"><span data-stu-id="d4e7d-239">Value</span></span>|
|---|---|
|[<span data-ttu-id="d4e7d-240">最低版本的邮箱要求集</span><span class="sxs-lookup"><span data-stu-id="d4e7d-240">Minimum mailbox requirement set version</span></span>](../../requirement-sets/outlook-api-requirement-sets.md)| <span data-ttu-id="d4e7d-241">1.1</span><span class="sxs-lookup"><span data-stu-id="d4e7d-241">1.1</span></span>|
|[<span data-ttu-id="d4e7d-242">适用的 Outlook 模式</span><span class="sxs-lookup"><span data-stu-id="d4e7d-242">Applicable Outlook mode</span></span>](../../../outlook/outlook-add-ins-overview.md#extension-points)| <span data-ttu-id="d4e7d-243">撰写或阅读</span><span class="sxs-lookup"><span data-stu-id="d4e7d-243">Compose or Read</span></span>|