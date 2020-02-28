---
title: 在 Outlook 的撰写窗体中获取和设置项目数据
description: 在撰写应用场景中获取或设置 Outlook 加载项中项的不同属性，包括收件人、主题、正文和约会地点和时间。
ms.date: 12/10/2019
localization_priority: Normal
ms.openlocfilehash: 3b82f418ffa2820e5f8cf04805a62b0d85691420
ms.sourcegitcommit: 5d29801180f6939ec10efb778d2311be67d8b9f1
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/27/2020
ms.locfileid: "42325445"
---
# <a name="get-and-set-item-data-in-a-compose-form-in-outlook"></a><span data-ttu-id="1ac10-103">在 Outlook 的撰写窗体中获取和设置项目数据</span><span class="sxs-lookup"><span data-stu-id="1ac10-103">Get and set item data in a compose form in Outlook</span></span>

<span data-ttu-id="1ac10-104">了解如何在撰写方案中获取或设置 Outlook 外接程序中项目的不同属性，包括收件人、主题、正文和约会地点和时间。</span><span class="sxs-lookup"><span data-stu-id="1ac10-104">Learn how to get or set various properties of an item in an Outlook add-in in a compose scenario, including its recipients, subject, body, and appointment location and time.</span></span>

## <a name="getting-and-setting-item-properties-for-a-compose-add-in"></a><span data-ttu-id="1ac10-105">获取和设置撰写加载项的项目属性</span><span class="sxs-lookup"><span data-stu-id="1ac10-105">Getting and setting item properties for a compose add-in</span></span>

<span data-ttu-id="1ac10-106">在撰写窗体中，您可以如同在阅读窗体中一样，获取在同一类型的项目上公开的大部分属性（如参与者、收件人、主题和正文），还可以获取仅与撰写窗体（而非阅读窗体）相关的一些其他属性（正文、密件抄送）。</span><span class="sxs-lookup"><span data-stu-id="1ac10-106">In a compose form, you can get most of the properties that are exposed on the same kind of item as in a read form (such as attendees, recipients, subject, and body), and you can get a few extra properties that are relevant in only a compose form but not a read form (body, bcc).</span></span>

<span data-ttu-id="1ac10-p101">对于大多数属性，由于 Outlook 外接程序和用户可能会同时修改用户界面中的同一个属性，获取和设置属性的方法将为异步。表 1 列出了项目级别属性以及用于在撰写窗体中获取和设置属性的相应异步方法。[item.itemType](../reference/objectmodel/preview-requirement-set/office.context.mailbox.item.md#properties) 和 [item.conversationId](../reference/objectmodel/preview-requirement-set/office.context.mailbox.item.md#properties) 属性是例外，因为用户无法修改。您可以使用与在阅读窗体中相同的编程方式，在撰写窗体中直接从父对象获取这些属性。</span><span class="sxs-lookup"><span data-stu-id="1ac10-p101">For most of these properties, because it's possible that an Outlook add-in and the user can be modifying the same property in the user interface at the same time, the methods to get and set them are asynchronous. Table 1 lists the item-level properties and corresponding asynchronous methods to get and set them in a compose form. The  [item.itemType](../reference/objectmodel/preview-requirement-set/office.context.mailbox.item.md#properties) and [item.conversationId](../reference/objectmodel/preview-requirement-set/office.context.mailbox.item.md#properties) properties are exceptions because users cannot modify them. You can programmatically get them the same way in a compose form as in a read form, directly from the parent object.</span></span>

<span data-ttu-id="1ac10-111">除了在 Office JavaScript API 中访问项目属性之外，还可以使用 Exchange Web 服务（EWS）访问项目级属性。</span><span class="sxs-lookup"><span data-stu-id="1ac10-111">Other than accessing item properties in the Office JavaScript API, you can access item-level properties using Exchange Web Services (EWS).</span></span> <span data-ttu-id="1ac10-112">通过 **ReadWriteMailbox** 权限，可以使用 [mailbox.makeEwsRequestAsync](../reference/objectmodel/preview-requirement-set/office.context.mailbox.md#methods) 方法访问 EWS 操作 [GetItem](/exchange/client-developer/web-service-reference/getitem-operation) 和 [UpdateItem](/exchange/client-developer/web-service-reference/updateitem-operation)，以获取和设置用户邮箱中的一个或多个项目的更多属性。</span><span class="sxs-lookup"><span data-stu-id="1ac10-112">With the **ReadWriteMailbox** permission, you can use the [mailbox.makeEwsRequestAsync](../reference/objectmodel/preview-requirement-set/office.context.mailbox.md#methods) method to access EWS operations, [GetItem](/exchange/client-developer/web-service-reference/getitem-operation) and [UpdateItem](/exchange/client-developer/web-service-reference/updateitem-operation), to get and set more properties of an item or items in the user's mailbox.</span></span>

<span data-ttu-id="1ac10-113">`makeEwsRequestAsync` 函数在撰写窗体和阅读窗体中均可用。</span><span class="sxs-lookup"><span data-stu-id="1ac10-113">The `makeEwsRequestAsync` function is available in both compose and read forms.</span></span> <span data-ttu-id="1ac10-114">有关 **ReadWriteMailbox** 权限以及通过 Office 加载项平台访问 EWS 的详细信息，请参阅[了解 Outlook 加载项权限](understanding-outlook-add-in-permissions.md)和[从 Outlook 加载项中调用 Web 服务](web-services.md)。</span><span class="sxs-lookup"><span data-stu-id="1ac10-114">For more information about the **ReadWriteMailbox** permission, and accessing EWS through the Office Add-ins platform, see [Understanding Outlook add-in permissions](understanding-outlook-add-in-permissions.md) and [Call web services from an Outlook add-in](web-services.md).</span></span>

<span data-ttu-id="1ac10-115">**表 1. 在撰写窗体中获取或设置项目属性的异步方法**</span><span class="sxs-lookup"><span data-stu-id="1ac10-115">**Table 1. Asynchronous methods to get or set item properties in a compose form**</span></span>

<br/>

| <span data-ttu-id="1ac10-116">属性</span><span class="sxs-lookup"><span data-stu-id="1ac10-116">Property</span></span> | <span data-ttu-id="1ac10-117">属性类型</span><span class="sxs-lookup"><span data-stu-id="1ac10-117">Property type</span></span> | <span data-ttu-id="1ac10-118">获取的异步方法</span><span class="sxs-lookup"><span data-stu-id="1ac10-118">Asynchronous method to get</span></span> | <span data-ttu-id="1ac10-119">设置的异步方法</span><span class="sxs-lookup"><span data-stu-id="1ac10-119">Asynchronous method(s) to set</span></span> |
|:-----|:-----|:-----|:-----|
|[<span data-ttu-id="1ac10-120">bcc</span><span class="sxs-lookup"><span data-stu-id="1ac10-120">bcc</span></span>](../reference/objectmodel/preview-requirement-set/office.context.mailbox.item.md#properties)|[<span data-ttu-id="1ac10-121">收件人</span><span class="sxs-lookup"><span data-stu-id="1ac10-121">Recipients</span></span>](/javascript/api/outlook/office.Recipients)|[<span data-ttu-id="1ac10-122">Recipients.getAsync</span><span class="sxs-lookup"><span data-stu-id="1ac10-122">Recipients.getAsync</span></span>](/javascript/api/outlook/office.Recipients#getasync-options--callback-)|<span data-ttu-id="1ac10-123">[Recipients.addAsync](/javascript/api/outlook/office.Recipients#addasync-recipients--options--callback-), [Recipients.setAsync](/javascript/api/outlook/office.Recipients#setasync-recipients--options--callback-)</span><span class="sxs-lookup"><span data-stu-id="1ac10-123">[Recipients.addAsync](/javascript/api/outlook/office.Recipients#addasync-recipients--options--callback-), [Recipients.setAsync](/javascript/api/outlook/office.Recipients#setasync-recipients--options--callback-)</span></span>|
|[<span data-ttu-id="1ac10-124">body</span><span class="sxs-lookup"><span data-stu-id="1ac10-124">body</span></span>](../reference/objectmodel/preview-requirement-set/office.context.mailbox.item.md#properties)|[<span data-ttu-id="1ac10-125">Body</span><span class="sxs-lookup"><span data-stu-id="1ac10-125">Body</span></span>](/javascript/api/outlook/office.Body)|[<span data-ttu-id="1ac10-126">Body.getAsync</span><span class="sxs-lookup"><span data-stu-id="1ac10-126">Body.getAsync</span></span>](/javascript/api/outlook/office.Body#getasync-coerciontype--options--callback-)|<span data-ttu-id="1ac10-127">[Body.prependAsync](/javascript/api/outlook/office.Body#prependasync-data--options--callback-), [Body.setAsync](/javascript/api/outlook/office.Body#setasync-data--options--callback-), [Body.setSelectedDataAsync](/javascript/api/outlook/office.Body#setselecteddataasync-data--options--callback-)</span><span class="sxs-lookup"><span data-stu-id="1ac10-127">[Body.prependAsync](/javascript/api/outlook/office.Body#prependasync-data--options--callback-), [Body.setAsync](/javascript/api/outlook/office.Body#setasync-data--options--callback-), [Body.setSelectedDataAsync](/javascript/api/outlook/office.Body#setselecteddataasync-data--options--callback-)</span></span>|
|[<span data-ttu-id="1ac10-128">cc</span><span class="sxs-lookup"><span data-stu-id="1ac10-128">cc</span></span>](../reference/objectmodel/preview-requirement-set/office.context.mailbox.item.md#properties)|<span data-ttu-id="1ac10-129">收件人</span><span class="sxs-lookup"><span data-stu-id="1ac10-129">Recipients</span></span>|<span data-ttu-id="1ac10-130">Recipients.getAsync</span><span class="sxs-lookup"><span data-stu-id="1ac10-130">Recipients.getAsync</span></span>|<span data-ttu-id="1ac10-131">Recipients.addAsync Recipients.setAsync</span><span class="sxs-lookup"><span data-stu-id="1ac10-131">Recipients.addAsync Recipients.setAsync</span></span>|
|[<span data-ttu-id="1ac10-132">end</span><span class="sxs-lookup"><span data-stu-id="1ac10-132">end</span></span>](../reference/objectmodel/preview-requirement-set/office.context.mailbox.item.md#properties)|[<span data-ttu-id="1ac10-133">时间</span><span class="sxs-lookup"><span data-stu-id="1ac10-133">Time</span></span>](/javascript/api/outlook/office.Time)|[<span data-ttu-id="1ac10-134">Time.getAsync</span><span class="sxs-lookup"><span data-stu-id="1ac10-134">Time.getAsync</span></span>](/javascript/api/outlook/office.Time#getasync-options--callback-)|[<span data-ttu-id="1ac10-135">Time.setAsync</span><span class="sxs-lookup"><span data-stu-id="1ac10-135">Time.setAsync</span></span>](/javascript/api/outlook/office.Time#setasync-datetime--options--callback-)|
|[<span data-ttu-id="1ac10-136">location</span><span class="sxs-lookup"><span data-stu-id="1ac10-136">location</span></span>](../reference/objectmodel/preview-requirement-set/office.context.mailbox.item.md#properties)|[<span data-ttu-id="1ac10-137">位置</span><span class="sxs-lookup"><span data-stu-id="1ac10-137">Location</span></span>](/javascript/api/outlook/office.Location)|[<span data-ttu-id="1ac10-138">Location.getAsync</span><span class="sxs-lookup"><span data-stu-id="1ac10-138">Location.getAsync</span></span>](/javascript/api/outlook/office.Location#getasync-options--callback-)|[<span data-ttu-id="1ac10-139">Location.setAsync</span><span class="sxs-lookup"><span data-stu-id="1ac10-139">Location.setAsync</span></span>](/javascript/api/outlook/office.Location#setasync-location--options--callback-)|
|[<span data-ttu-id="1ac10-140">optionalAttendees</span><span class="sxs-lookup"><span data-stu-id="1ac10-140">optionalAttendees</span></span>](../reference/objectmodel/preview-requirement-set/office.context.mailbox.item.md#properties)|<span data-ttu-id="1ac10-141">收件人</span><span class="sxs-lookup"><span data-stu-id="1ac10-141">Recipients</span></span>|<span data-ttu-id="1ac10-142">Recipients.getAsync</span><span class="sxs-lookup"><span data-stu-id="1ac10-142">Recipients.getAsync</span></span>|<span data-ttu-id="1ac10-143">Recipients.addAsync Recipients.setAsync</span><span class="sxs-lookup"><span data-stu-id="1ac10-143">Recipients.addAsync Recipients.setAsync</span></span>|
|[<span data-ttu-id="1ac10-144">requiredAttendees</span><span class="sxs-lookup"><span data-stu-id="1ac10-144">requiredAttendees</span></span>](../reference/objectmodel/preview-requirement-set/office.context.mailbox.item.md#properties)|<span data-ttu-id="1ac10-145">收件人</span><span class="sxs-lookup"><span data-stu-id="1ac10-145">Recipients</span></span>|<span data-ttu-id="1ac10-146">Recipients.getAsync</span><span class="sxs-lookup"><span data-stu-id="1ac10-146">Recipients.getAsync</span></span>|<span data-ttu-id="1ac10-147">Recipients.addAsync Recipients.setAsync</span><span class="sxs-lookup"><span data-stu-id="1ac10-147">Recipients.addAsync Recipients.setAsync</span></span>|
|[<span data-ttu-id="1ac10-148">start</span><span class="sxs-lookup"><span data-stu-id="1ac10-148">start</span></span>](../reference/objectmodel/preview-requirement-set/office.context.mailbox.item.md#properties)|<span data-ttu-id="1ac10-149">时间</span><span class="sxs-lookup"><span data-stu-id="1ac10-149">Time</span></span>|<span data-ttu-id="1ac10-150">Time.getAsync</span><span class="sxs-lookup"><span data-stu-id="1ac10-150">Time.getAsync</span></span>|<span data-ttu-id="1ac10-151">Time.setAsync</span><span class="sxs-lookup"><span data-stu-id="1ac10-151">Time.setAsync</span></span>|
|[<span data-ttu-id="1ac10-152">subject</span><span class="sxs-lookup"><span data-stu-id="1ac10-152">subject</span></span>](../reference/objectmodel/preview-requirement-set/office.context.mailbox.item.md#properties)|[<span data-ttu-id="1ac10-153">Subject</span><span class="sxs-lookup"><span data-stu-id="1ac10-153">Subject</span></span>](/javascript/api/outlook/office.Subject)|[<span data-ttu-id="1ac10-154">Subject.getAsync</span><span class="sxs-lookup"><span data-stu-id="1ac10-154">Subject.getAsync</span></span>](/javascript/api/outlook/office.Subject#getasync-options--callback-)|[<span data-ttu-id="1ac10-155">Subject.setAsync</span><span class="sxs-lookup"><span data-stu-id="1ac10-155">Subject.setAsync</span></span>](/javascript/api/outlook/office.Subject#setasync-subject--options--callback-)|
|[<span data-ttu-id="1ac10-156">to</span><span class="sxs-lookup"><span data-stu-id="1ac10-156">to</span></span>](../reference/objectmodel/preview-requirement-set/office.context.mailbox.item.md#properties)|<span data-ttu-id="1ac10-157">收件人</span><span class="sxs-lookup"><span data-stu-id="1ac10-157">Recipients</span></span>|<span data-ttu-id="1ac10-158">Recipients.getAsync</span><span class="sxs-lookup"><span data-stu-id="1ac10-158">Recipients.getAsync</span></span>|<span data-ttu-id="1ac10-159">Recipients.addAsync Recipients.setAsync</span><span class="sxs-lookup"><span data-stu-id="1ac10-159">Recipients.addAsync Recipients.setAsync</span></span>|

## <a name="see-also"></a><span data-ttu-id="1ac10-160">另请参阅</span><span class="sxs-lookup"><span data-stu-id="1ac10-160">See also</span></span>

- [<span data-ttu-id="1ac10-161">创建适用于撰写窗体的 Outlook 加载项</span><span class="sxs-lookup"><span data-stu-id="1ac10-161">Create Outlook add-ins for compose forms</span></span>](compose-scenario.md)
- [<span data-ttu-id="1ac10-162">了解 Outlook 外接程序权限</span><span class="sxs-lookup"><span data-stu-id="1ac10-162">Understanding Outlook add-in permissions</span></span>](understanding-outlook-add-in-permissions.md)
- [<span data-ttu-id="1ac10-163">从 Outlook 外接程序调用 Web 服务</span><span class="sxs-lookup"><span data-stu-id="1ac10-163">Call web services from an Outlook add-in</span></span>](web-services.md)
- [<span data-ttu-id="1ac10-164">在阅读或撰写窗体中获取并设置 Outlook 项目数据</span><span class="sxs-lookup"><span data-stu-id="1ac10-164">Get and set Outlook item data in read or compose forms</span></span>](item-data.md)