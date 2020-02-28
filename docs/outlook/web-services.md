---
title: 从 Outlook 加载项使用 Exchange Web 服务 (EWS)
description: 提供的示例显示 Outlook 加载项如何通过 Exchange Web 服务请求信息。
ms.date: 10/31/2019
localization_priority: Normal
ms.openlocfilehash: 1f497452519111a82968affdf418f750945c53db
ms.sourcegitcommit: 5d29801180f6939ec10efb778d2311be67d8b9f1
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/27/2020
ms.locfileid: "42325317"
---
# <a name="call-web-services-from-an-outlook-add-in"></a><span data-ttu-id="e451b-103">从 Outlook 加载项调用 Web 服务</span><span class="sxs-lookup"><span data-stu-id="e451b-103">Call web services from an Outlook add-in</span></span>

<span data-ttu-id="e451b-p101">您的外接程序可使用运行 Exchange Server 2013 的计算机中的 Exchange Web 服务 (EWS)，该 Web 服务可在为外接程序的 UI 提供源位置的服务器上获得，也可在 Internet 上获得。本文提供展示 Outlook 外接程序如何从 EWS 请求信息的示例。</span><span class="sxs-lookup"><span data-stu-id="e451b-p101">Your add-in can use Exchange Web Services (EWS) from a computer that is running Exchange Server 2013, a web service that is available on the server that provides the source location for the add-in's UI, or a web service that is available on the Internet. This article provides an example that shows how an Outlook add-in can request information from EWS.</span></span>

<span data-ttu-id="e451b-p102">您用来调用 Web 服务的方法随 Web 服务所在的位置的不同而不同。表 1 列出了可以基于位置调用 Web 服务的不同方法。</span><span class="sxs-lookup"><span data-stu-id="e451b-p102">The way that you call a web service varies based on where the web service is located. Table 1 lists the different ways that you can call a web service based on location.</span></span>


<span data-ttu-id="e451b-108">**表 1.从 Outlook 外接程序调用 Web 服务的方式**</span><span class="sxs-lookup"><span data-stu-id="e451b-108">**Table 1. Ways to call web services from an Outlook add-in**</span></span>

<br/>

|<span data-ttu-id="e451b-109">**Web 服务位置**</span><span class="sxs-lookup"><span data-stu-id="e451b-109">**Web service location**</span></span>|<span data-ttu-id="e451b-110">**调用 Web 服务的方法**</span><span class="sxs-lookup"><span data-stu-id="e451b-110">**Way to call the web service**</span></span>|
|:-----|:-----|
|<span data-ttu-id="e451b-111">托管客户端邮箱的 Exchange 服务器</span><span class="sxs-lookup"><span data-stu-id="e451b-111">The Exchange server that hosts the client mailbox</span></span>|<span data-ttu-id="e451b-p103">使用 [mailbox.makeEwsRequestAsync](../reference/objectmodel/preview-requirement-set/office.context.mailbox.md#methods) 方法可调用外接程序支持的 EWS 操作。承载邮箱的 Exchange 服务器还会公开 EWS。</span><span class="sxs-lookup"><span data-stu-id="e451b-p103">Use the [mailbox.makeEwsRequestAsync](../reference/objectmodel/preview-requirement-set/office.context.mailbox.md#methods) method to call EWS operations that add-ins support. The Exchange server that hosts the mailbox also exposes EWS.</span></span>|
|<span data-ttu-id="e451b-114">为加载项 UI 提供源位置的 Web 服务器</span><span class="sxs-lookup"><span data-stu-id="e451b-114">The web server that provides the source location for the add-in UI</span></span>|<span data-ttu-id="e451b-p104">使用标准 JavaScript 技术调用 Web 服务。UI 框架中的 JavaScript 代码将在提供 UI 的 Web 服务器的上下文中运行。因此，此代码可以调用该服务器上的 Web 服务，而不会导致出现跨网站脚本错误。</span><span class="sxs-lookup"><span data-stu-id="e451b-p104">Call the web service by using standard JavaScript techniques. The JavaScript code in the UI frame runs in the context of the web server that provides the UI. Therefore, it can call web services on that server without causing a cross-site scripting error.</span></span>|
|<span data-ttu-id="e451b-118">所有其他位置</span><span class="sxs-lookup"><span data-stu-id="e451b-118">All other locations</span></span>|<span data-ttu-id="e451b-p105">为提供 UI 源位置的 Web 服务器上的 Web 服务创建代理。如果您不提供代理，跨网站脚本错误将阻止外接程序运行。提供代理的一种方式是使用 JSON/P。有关详细信息，请参阅 [Office 外接程序的隐私和安全性](../develop/privacy-and-security.md)。</span><span class="sxs-lookup"><span data-stu-id="e451b-p105">Create a proxy for the web service on the web server that provides the source location for the UI. If you do not provide a proxy, cross-site scripting errors will prevent your add-in from running. One way to provide a proxy is by using JSON/P. For more information, see [Privacy and security for Office Add-ins](../develop/privacy-and-security.md).</span></span>|

## <a name="using-the-makeewsrequestasync-method-to-access-ews-operations"></a><span data-ttu-id="e451b-123">使用 makeEwsRequestAsync 方法访问 EWS 操作</span><span class="sxs-lookup"><span data-stu-id="e451b-123">Using the makeEwsRequestAsync method to access EWS operations</span></span>

<span data-ttu-id="e451b-124">可以使用 [mailbox.makeEwsRequestAsync](../reference/objectmodel/preview-requirement-set/office.context.mailbox.md#methods) 方法向托管用户邮箱的 Exchange 服务器发出 EWS 请求。</span><span class="sxs-lookup"><span data-stu-id="e451b-124">You can use the [mailbox.makeEwsRequestAsync](../reference/objectmodel/preview-requirement-set/office.context.mailbox.md#methods) method to make an EWS request to the Exchange server that hosts the user's mailbox.</span></span>

<span data-ttu-id="e451b-p106">EWS 服务支持 Exchange 服务器中的不同操作；例如复制、查找、更新或发送项目的项目级操作，以及创建、获取或更新文件夹的文件夹级操作。若要执行 EWS 操作，请创建一个执行该操作的 XML SOAP 请求。当操作完成时，你将获得包含该操作相关数据的 XML SOAP 响应。EWS SOAP 请求和响应遵循 Messages.xsd 文件中定义的架构。正如其他 EWS 架构文件一样，Message.xsd 文件位于托管 EWS 的 IIS 虚拟目录中。</span><span class="sxs-lookup"><span data-stu-id="e451b-p106">EWS supports different operations on an Exchange server; for example, item-level operations to copy, find, update, or send an item, and folder-level operations to create, get, or update a folder. To perform an EWS operation, create an XML SOAP request for that operation. When the operation finishes, you get an XML SOAP response that contains data that is relevant to the operation. EWS SOAP requests and responses follow the schema defined in the Messages.xsd file. Like other EWS schema files, the Message.xsd file is located in the IIS virtual directory that hosts EWS.</span></span>

<span data-ttu-id="e451b-130">若要使用`makeEwsRequestAsync`方法启动 EWS 操作，请提供以下内容：</span><span class="sxs-lookup"><span data-stu-id="e451b-130">To use the `makeEwsRequestAsync` method to initiate an EWS operation, provide the following:</span></span>

- <span data-ttu-id="e451b-131">针对该 EWS 操作的 SOAP 请求的 XML，作为  _data_ 形参的实参</span><span class="sxs-lookup"><span data-stu-id="e451b-131">The XML for the SOAP request for that EWS operation, as an argument to the  _data_ parameter</span></span>

- <span data-ttu-id="e451b-132">回调方法（作为  _callback_ 实参）</span><span class="sxs-lookup"><span data-stu-id="e451b-132">A callback method (as the  _callback_ argument)</span></span>

- <span data-ttu-id="e451b-133">该回调方法的任何可选输入数据（作为  _userContext_ 实参）</span><span class="sxs-lookup"><span data-stu-id="e451b-133">Any optional input data for that callback method (as the  _userContext_ argument)</span></span>

<span data-ttu-id="e451b-p107">当 EWS SOAP 请求完成时，Outlook 将使用一个参数（即[AsyncResult](/javascript/api/office/office.asyncresult)对象）调用回调方法。回调方法可以`AsyncResult`访问对象的两个属性：该`value`属性包含 EWS 操作的 XML SOAP 响应，也可以是`asyncContext`包含作为`userContext`参数传递的任何数据的属性（可选）。通常情况下，回调方法会将 SOAP 响应中的 XML 解析为获取任何相关信息，并相应地处理这些信息。</span><span class="sxs-lookup"><span data-stu-id="e451b-p107">When the EWS SOAP request is complete, Outlook calls the callback method with one argument, which is an [AsyncResult](/javascript/api/office/office.asyncresult) object. The callback method can access two properties of the `AsyncResult` object: the `value` property, which contains the XML SOAP response of the EWS operation, and optionally, the `asyncContext` property, which contains any data passed as the `userContext` parameter. Typically, the callback method then parses the XML in the SOAP response to get any relevant information, and processes that information accordingly.</span></span>


## <a name="tips-for-parsing-ews-responses"></a><span data-ttu-id="e451b-137">解析 EWS 响应的提示</span><span class="sxs-lookup"><span data-stu-id="e451b-137">Tips for parsing EWS responses</span></span>

<span data-ttu-id="e451b-138">分析 EWS 操作的 SOAP 响应时，请注意下列与浏览器相关的问题：</span><span class="sxs-lookup"><span data-stu-id="e451b-138">When parsing a SOAP response from an EWS operation, note the following browser-dependent issues:</span></span>


- <span data-ttu-id="e451b-139">使用 DOM 方法`getElementsByTagName`时，请指定标记名称的前缀，以包含对 Internet Explorer 的支持。</span><span class="sxs-lookup"><span data-stu-id="e451b-139">Specify the prefix for a tag name when using the DOM method `getElementsByTagName`, to include support for Internet Explorer.</span></span>

  <span data-ttu-id="e451b-p108">`getElementsByTagName`的行为有所不同，具体取决于浏览器类型。例如，EWS 响应可包含以下 XML （格式设置并为显示目的而缩写）：</span><span class="sxs-lookup"><span data-stu-id="e451b-p108">`getElementsByTagName` behaves differently depending on browser type. For example, an EWS response can contain the following XML (formatted and abbreviated for display purposes):</span></span>

   ```XML
        <t:ExtendedProperty><t:ExtendedFieldURI PropertySetId="00000000-0000-0000-0000-000000000000" 
        PropertyName="MyProperty" 
        PropertyType="String"/>
        <t:Value>{
        ...
        }</t:Value></t:ExtendedProperty>
   ```

   <span data-ttu-id="e451b-142">如下所示的代码将在类似于浏览器的浏览器上运行，以获取由`ExtendedProperty`标记括起来的 XML：</span><span class="sxs-lookup"><span data-stu-id="e451b-142">Code, as in the following, would work on a browser like Chrome to get the XML enclosed by the `ExtendedProperty` tags:</span></span>

   ```js
        var mailbox = Office.context.mailbox;
        mailbox.makeEwsRequestAsync(mailbox.item.itemId, function(result) {
            var response = $.parseXML(result.value);
            var extendedProps = response.getElementsByTagName("ExtendedProperty")
            });
   ```

   <span data-ttu-id="e451b-143">在 Internet Explorer 上，必须包含标记名称的 `t:` 前缀，如下所示：</span><span class="sxs-lookup"><span data-stu-id="e451b-143">On Internet Explorer, you must include the `t:` prefix of the tag name, as shown below:</span></span>

   ```js
        var mailbox = Office.context.mailbox;
        mailbox.makeEwsRequestAsync(mailbox.item.itemId, function(result) {
            var response = $.parseXML(result.value);
            var extendedProps = response.getElementsByTagName("t:ExtendedProperty")
            });
   ```

- <span data-ttu-id="e451b-144">使用 DOM 属性`textContent`获取 EWS 响应中标记的内容，如下所示：</span><span class="sxs-lookup"><span data-stu-id="e451b-144">Use the DOM property `textContent` to get the contents of a tag in an EWS response, as shown below:</span></span>
    
   ```js
      content = $.parseJSON(value.textContent);
   ```

   <span data-ttu-id="e451b-145">对于 EWS 响应中`innerHTML`的某些标记，其他属性（例如，可能无法在 Internet Explorer 上工作）。</span><span class="sxs-lookup"><span data-stu-id="e451b-145">Other properties such as `innerHTML` may not work on Internet Explorer for some tags in an EWS response.</span></span>
    

## <a name="example"></a><span data-ttu-id="e451b-146">示例</span><span class="sxs-lookup"><span data-stu-id="e451b-146">Example</span></span>

<span data-ttu-id="e451b-p109">下面的示例调用`makeEwsRequestAsync`以使用[GetItem](/exchange/client-developer/web-service-reference/getitem-operation)操作来获取项目的主题。此示例包括以下三个函数：</span><span class="sxs-lookup"><span data-stu-id="e451b-p109">The following example calls `makeEwsRequestAsync` to use the [GetItem](/exchange/client-developer/web-service-reference/getitem-operation) operation to get the subject of an item. This example includes the following three functions:</span></span>

-  <span data-ttu-id="e451b-149">`getSubjectRequest`&ndash;采用项 ID 作为输入，并返回 SOAP 请求的 XML，以调用`GetItem`指定的项。</span><span class="sxs-lookup"><span data-stu-id="e451b-149">`getSubjectRequest` &ndash; Takes an item ID as input, and returns the XML for the SOAP request to call `GetItem` for the specified item.</span></span>
    
-  <span data-ttu-id="e451b-150">`sendRequest`&ndash;调用`getSubjectRequest`以获取对选定项的 soap 请求，然后将 soap 请求和回调方法`callback` `makeEwsRequestAsync`传递给，以获取指定项的主题。</span><span class="sxs-lookup"><span data-stu-id="e451b-150">`sendRequest` &ndash; Calls  `getSubjectRequest` to get the SOAP request for the selected item, then passes the SOAP request and the callback method, `callback`, to `makeEwsRequestAsync` to get the subject of the specified item.</span></span>
    
-  <span data-ttu-id="e451b-151">`callback` &ndash; 处理包含有关指定项目的任何主题和其他信息的 SOAP 响应。</span><span class="sxs-lookup"><span data-stu-id="e451b-151">`callback` &ndash; Processes the SOAP response which includes any subject and other information about the specified item.</span></span>
    

```js
function getSubjectRequest(id) {
   // Return a GetItem operation request for the subject of the specified item. 
   var result = 
'<?xml version="1.0" encoding="utf-8"?>' +
'<soap:Envelope xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance"' +
'               xmlns:xsd="https://www.w3.org/2001/XMLSchema"' +
'               xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/"' +
'               xmlns:t="http://schemas.microsoft.com/exchange/services/2006/types">' +
'  <soap:Header>' +
'    <RequestServerVersion Version="Exchange2013" xmlns="http://schemas.microsoft.com/exchange/services/2006/types" soap:mustUnderstand="0" />' +
'  </soap:Header>' +
'  <soap:Body>' +
'    <GetItem xmlns="http://schemas.microsoft.com/exchange/services/2006/messages">' +
'      <ItemShape>' +
'        <t:BaseShape>IdOnly</t:BaseShape>' +
'        <t:AdditionalProperties>' +
'            <t:FieldURI FieldURI="item:Subject"/>' +
'        </t:AdditionalProperties>' +
'      </ItemShape>' +
'      <ItemIds><t:ItemId Id="' + id + '"/></ItemIds>' +
'    </GetItem>' +
'  </soap:Body>' +
'</soap:Envelope>';

   return result;
}

function sendRequest() {
   // Create a local variable that contains the mailbox.
   var mailbox = Office.context.mailbox;

   mailbox.makeEwsRequestAsync(getSubjectRequest(mailbox.item.itemId), callback);
}

function callback(asyncResult)  {
   var result = asyncResult.value;
   var context = asyncResult.context;

   // Process the returned response here.
}
```


## <a name="ews-operations-that-add-ins-support"></a><span data-ttu-id="e451b-152">外接程序支持的 EWS 操作</span><span class="sxs-lookup"><span data-stu-id="e451b-152">EWS operations that add-ins support</span></span>

<span data-ttu-id="e451b-p110">Outlook 外接程序可以通过`makeEwsRequestAsync`方法访问 EWS 中可用的部分操作。如果您不熟悉 EWS 操作，以及如何使用`makeEwsRequestAsync`方法访问操作，请首先从 SOAP 请求示例中自定义_数据_参数。</span><span class="sxs-lookup"><span data-stu-id="e451b-p110">Outlook add-ins can access a subset of operations that are available in EWS via the `makeEwsRequestAsync` method. If you are unfamiliar with EWS operations and how to use the `makeEwsRequestAsync` method to access an operation, start with a SOAP request example to customize your _data_ argument.</span></span> 

<span data-ttu-id="e451b-155">下面介绍了如何使用`makeEwsRequestAsync`方法：</span><span class="sxs-lookup"><span data-stu-id="e451b-155">The following describes how you can use the `makeEwsRequestAsync` method:</span></span>

1. <span data-ttu-id="e451b-156">在 XML 中，用适当值替换所有项目 ID 和相关 EWS 操作属性。</span><span class="sxs-lookup"><span data-stu-id="e451b-156">In the XML, substitute any item IDs and relevant EWS operation attributes with appropriate values.</span></span>
    
2. <span data-ttu-id="e451b-157">将 SOAP 请求作为的_data_参数的参数包括在内`makeEwsRequestAsync`。</span><span class="sxs-lookup"><span data-stu-id="e451b-157">Include the SOAP request as an argument for the  _data_ parameter of `makeEwsRequestAsync`.</span></span>
    
3. <span data-ttu-id="e451b-158">指定回调方法和调用`makeEwsRequestAsync`。</span><span class="sxs-lookup"><span data-stu-id="e451b-158">Specify a callback method and call `makeEwsRequestAsync`.</span></span>
    
4. <span data-ttu-id="e451b-159">在回调方法中，验证 SOAP 响应中操作的结果。</span><span class="sxs-lookup"><span data-stu-id="e451b-159">In the callback method, verify the results of the operation in the SOAP response.</span></span>
    
5. <span data-ttu-id="e451b-160">根据需要使用 EWS 操作的结果。</span><span class="sxs-lookup"><span data-stu-id="e451b-160">Use the results of the EWS operation according to your needs.</span></span>
    
<span data-ttu-id="e451b-p111">下表列出了外接程序支持的 EWS 操作。若要查看 SOAP 请求和响应的示例，请选择各操作对应的链接。有关 EWS 操作的详细信息，请参阅 [在交换 EWS 操作](/exchange/client-developer/web-service-reference/ews-operations-in-exchange)。</span><span class="sxs-lookup"><span data-stu-id="e451b-p111">The following table lists the EWS operations that add-ins support. To see examples of SOAP requests and responses, choose the link for each operation. For more information about EWS operations, see [EWS operations in Exchange](/exchange/client-developer/web-service-reference/ews-operations-in-exchange).</span></span>

<span data-ttu-id="e451b-164">**表 2.支持的 EWS 操作**</span><span class="sxs-lookup"><span data-stu-id="e451b-164">**Table 2. Supported EWS operations**</span></span>

<br/>

|<span data-ttu-id="e451b-165">**EWS 操作**</span><span class="sxs-lookup"><span data-stu-id="e451b-165">**EWS operation**</span></span>|<span data-ttu-id="e451b-166">**说明**</span><span class="sxs-lookup"><span data-stu-id="e451b-166">**Description**</span></span>|
|:-----|:-----|
|[<span data-ttu-id="e451b-167">CopyItem 操作</span><span class="sxs-lookup"><span data-stu-id="e451b-167">CopyItem operation</span></span>](/exchange/client-developer/web-service-reference/copyitem-operation)|<span data-ttu-id="e451b-168">在 Exchange 存储的指定文件夹中复制指定项目并在其中放入新项目。</span><span class="sxs-lookup"><span data-stu-id="e451b-168">Copies the specified items and puts the new items in a designated folder in the Exchange store.</span></span>|
|[<span data-ttu-id="e451b-169">CreateFolder 操作</span><span class="sxs-lookup"><span data-stu-id="e451b-169">CreateFolder operation</span></span>](/exchange/client-developer/web-service-reference/createfolder-operation)|<span data-ttu-id="e451b-170">在 Exchange 存储中的指定位置创建文件夹。</span><span class="sxs-lookup"><span data-stu-id="e451b-170">Creates folders in the specified location in the Exchange store.</span></span>|
|[<span data-ttu-id="e451b-171">CreateItem 操作</span><span class="sxs-lookup"><span data-stu-id="e451b-171">CreateItem operation</span></span>](/exchange/client-developer/web-service-reference/createitem-operation)|<span data-ttu-id="e451b-172">在 Exchange 存储中创建指定项目。</span><span class="sxs-lookup"><span data-stu-id="e451b-172">Creates the specified items in the Exchange store.</span></span>|
|[<span data-ttu-id="e451b-173">ExpandDL 操作</span><span class="sxs-lookup"><span data-stu-id="e451b-173">ExpandDL operation</span></span>](/exchange/client-developer/web-service-reference/expanddl-operation)|<span data-ttu-id="e451b-174">显示通讯组列表的完整成员身份。</span><span class="sxs-lookup"><span data-stu-id="e451b-174">Displays the full membership of distribution lists.</span></span>|
|[<span data-ttu-id="e451b-175">FindConversation 操作</span><span class="sxs-lookup"><span data-stu-id="e451b-175">FindConversation operation</span></span>](/exchange/client-developer/web-service-reference/findconversation-operation)|<span data-ttu-id="e451b-176">在 Exchange 存储的指定文件夹中枚举会话列表。</span><span class="sxs-lookup"><span data-stu-id="e451b-176">Enumerates a list of conversations in the specified folder in the Exchange store.</span></span>|
|[<span data-ttu-id="e451b-177">FindFolder 操作</span><span class="sxs-lookup"><span data-stu-id="e451b-177">FindFolder operation</span></span>](/exchange/client-developer/web-service-reference/findfolder-operation)|<span data-ttu-id="e451b-178">查找指定文件夹的子文件夹并返回描述这组子文件夹的一组属性。</span><span class="sxs-lookup"><span data-stu-id="e451b-178">Finds subfolders of an identified folder and returns a set of properties that describe the set of subfolders.</span></span>|
|[<span data-ttu-id="e451b-179">FindItem 操作</span><span class="sxs-lookup"><span data-stu-id="e451b-179">FindItem operation</span></span>](/exchange/client-developer/web-service-reference/finditem-operation)|<span data-ttu-id="e451b-180">标识位于 Exchange 存储的指定文件夹中的项目。</span><span class="sxs-lookup"><span data-stu-id="e451b-180">Identifies items that are located in a specified folder in the Exchange store.</span></span>|
|[<span data-ttu-id="e451b-181">GetConversationItems 操作</span><span class="sxs-lookup"><span data-stu-id="e451b-181">GetConversationItems operation</span></span>](/exchange/client-developer/web-service-reference/getconversationitems-operation)|<span data-ttu-id="e451b-182">在会话中获取排列为节点的一个或多个项集。</span><span class="sxs-lookup"><span data-stu-id="e451b-182">Gets one or more sets of items that are organized in nodes in a conversation.</span></span>|
|[<span data-ttu-id="e451b-183">GetFolder 操作</span><span class="sxs-lookup"><span data-stu-id="e451b-183">GetFolder operation</span></span>](/exchange/client-developer/web-service-reference/getfolder-operation)|<span data-ttu-id="e451b-184">从 Exchange 存储中获取文件夹的指定属性和内容。</span><span class="sxs-lookup"><span data-stu-id="e451b-184">Gets the specified properties and contents of folders from the Exchange store.</span></span>|
|[<span data-ttu-id="e451b-185">GetItem 操作</span><span class="sxs-lookup"><span data-stu-id="e451b-185">GetItem operation</span></span>](/exchange/client-developer/web-service-reference/getitem-operation)|<span data-ttu-id="e451b-186">从 Exchange 存储中获取项目的指定属性和内容。</span><span class="sxs-lookup"><span data-stu-id="e451b-186">Gets the specified properties and contents of items from the Exchange store.</span></span>|
|[<span data-ttu-id="e451b-187">GetUserAvailability 操作</span><span class="sxs-lookup"><span data-stu-id="e451b-187">GetUserAvailability operation</span></span>](/exchange/client-developer/web-service-reference/getuseravailability-operation)|<span data-ttu-id="e451b-188">提供特定时间段内有关一组用户、会议室和资源的可用性的详细信息。</span><span class="sxs-lookup"><span data-stu-id="e451b-188">Provides detailed information about the availability of a set of users, rooms, and resources within a specified time period.</span></span>|
|[<span data-ttu-id="e451b-189">MarkAsJunk 操作</span><span class="sxs-lookup"><span data-stu-id="e451b-189">MarkAsJunk operation</span></span>](/exchange/client-developer/web-service-reference/markasjunk-operation)|<span data-ttu-id="e451b-190">将电子邮件移动到"垃圾邮件"文件夹，并相应地在阻止的发件人名单中添加或删除邮件的发件人。</span><span class="sxs-lookup"><span data-stu-id="e451b-190">Moves email messages to the Junk Email folder, and adds or removes senders of the messages from the blocked senders list accordingly.</span></span>|
|[<span data-ttu-id="e451b-191">MoveItem 操作</span><span class="sxs-lookup"><span data-stu-id="e451b-191">MoveItem operation</span></span>](/exchange/client-developer/web-service-reference/moveitem-operation)|<span data-ttu-id="e451b-192">将项目移动到 Exchange 存储中的单个目标文件夹。</span><span class="sxs-lookup"><span data-stu-id="e451b-192">Moves items to a single destination folder in the Exchange store.</span></span>|
|[<span data-ttu-id="e451b-193">ResolveNames 操作</span><span class="sxs-lookup"><span data-stu-id="e451b-193">ResolveNames operation</span></span>](/exchange/client-developer/web-service-reference/resolvenames-operation)|<span data-ttu-id="e451b-194">解析不确定的电子邮件地址和显示名称。</span><span class="sxs-lookup"><span data-stu-id="e451b-194">Resolves ambiguous email addresses and display names.</span></span>|
|[<span data-ttu-id="e451b-195">SendItem 操作</span><span class="sxs-lookup"><span data-stu-id="e451b-195">SendItem operation</span></span>](/exchange/client-developer/web-service-reference/senditem-operation)|<span data-ttu-id="e451b-196">发送位于 Exchange 存储中的电子邮件。</span><span class="sxs-lookup"><span data-stu-id="e451b-196">Sends email messages that are located in the Exchange store.</span></span>|
|[<span data-ttu-id="e451b-197">UpdateFolder 操作</span><span class="sxs-lookup"><span data-stu-id="e451b-197">UpdateFolder operation</span></span>](/exchange/client-developer/web-service-reference/updatefolder-operation)|<span data-ttu-id="e451b-198">修改 Exchange 存储中现有文件夹的属性。</span><span class="sxs-lookup"><span data-stu-id="e451b-198">Modifies the properties of existing folders in the Exchange store.</span></span>|
|[<span data-ttu-id="e451b-199">UpdateItem 操作</span><span class="sxs-lookup"><span data-stu-id="e451b-199">UpdateItem operation</span></span>](/exchange/client-developer/web-service-reference/updateitem-operation)|<span data-ttu-id="e451b-200">修改 Exchange 存储中现有项的属性。</span><span class="sxs-lookup"><span data-stu-id="e451b-200">Modifies the properties of existing items in the Exchange store.</span></span>|

 > [!NOTE]
 > <span data-ttu-id="e451b-201">FAI（文件夹关联信息）项不能通过外接程序进行更新（或创建）。</span><span class="sxs-lookup"><span data-stu-id="e451b-201">FAI (Folder Associated Information) items cannot be updated (or created) from an add-in.</span></span> <span data-ttu-id="e451b-202">这些隐藏的消息存储在文件夹中，用于存储各种设置和辅助数据。</span><span class="sxs-lookup"><span data-stu-id="e451b-202">These hidden messages are stored in a folder and are used to store a variety of settings and auxiliary data.</span></span>  <span data-ttu-id="e451b-203">尝试使用 UpdateItem 操作会导致以下 ErrorAccessDenied 错误抛出：“不得使用 Office 扩展来更新此类项”。</span><span class="sxs-lookup"><span data-stu-id="e451b-203">Attempting to use the UpdateItem operation will throw an ErrorAccessDenied error: "Office extension is not allowed to update this type of item".</span></span> <span data-ttu-id="e451b-204">此外，也可以使用 [EWS 托管 API](/exchange/client-developer/exchange-web-services/get-started-with-ews-managed-api-client-applications) 通过 Windows 客户端或服务器应用更新这些项。</span><span class="sxs-lookup"><span data-stu-id="e451b-204">As an alternative, you may use the [EWS Managed API](/exchange/client-developer/exchange-web-services/get-started-with-ews-managed-api-client-applications) to update these items from a Windows client or a server application.</span></span> <span data-ttu-id="e451b-205">建议谨慎操作，因为内部服务类型数据结构可能会发生变化并破坏解决方案。</span><span class="sxs-lookup"><span data-stu-id="e451b-205">Caution is recommended as internal, service-type data structures are subject to change and could break your solution.</span></span>


## <a name="authentication-and-permission-considerations-for-makeewsrequestasync"></a><span data-ttu-id="e451b-206">makeEwsRequestAsync 的身份验证和权限注意事项</span><span class="sxs-lookup"><span data-stu-id="e451b-206">Authentication and permission considerations for makeEwsRequestAsync</span></span>

<span data-ttu-id="e451b-207">使用此`makeEwsRequestAsync`方法时，将使用当前用户的电子邮件帐户凭据对请求进行身份验证。</span><span class="sxs-lookup"><span data-stu-id="e451b-207">When you use the `makeEwsRequestAsync` method, the request is authenticated by using the email account credentials of the current user.</span></span> <span data-ttu-id="e451b-208">该`makeEwsRequestAsync`方法为您管理凭据，因此您无需向您的请求提供身份验证凭据。</span><span class="sxs-lookup"><span data-stu-id="e451b-208">The `makeEwsRequestAsync` method manages the credentials for you so that you do not have to provide authentication credentials with your request.</span></span>

> [!NOTE]
> <span data-ttu-id="e451b-209">服务器管理员必须使用[set-webservicesvirtualdirectory](/powershell/module/exchange/client-access-servers/New-WebServicesVirtualDirectory?view=exchange-ps)或[Set-webservicesvirtualdirectory](/powershell/module/exchange/client-access-servers/Set-WebServicesVirtualDirectory?view=exchange-ps) Cmldet 将客户端访问服务器 EWS 目录上的_OAuthAuthentication_参数设置为**true** ，才能使该`makeEwsRequestAsync`方法能够发出 EWS 请求。</span><span class="sxs-lookup"><span data-stu-id="e451b-209">The server administrator must use the [New-WebServicesVirtualDirectory](/powershell/module/exchange/client-access-servers/New-WebServicesVirtualDirectory?view=exchange-ps) or the [Set-WebServicesVirtualDirectory](/powershell/module/exchange/client-access-servers/Set-WebServicesVirtualDirectory?view=exchange-ps) cmldet to set the _OAuthAuthentication_ parameter to **true** on the Client Access server EWS directory in order to enable the `makeEwsRequestAsync` method to make EWS requests.</span></span>

<span data-ttu-id="e451b-210">您的外接程序必须在`ReadWriteMailbox`其外接程序清单中指定要使用`makeEwsRequestAsync`方法的权限。</span><span class="sxs-lookup"><span data-stu-id="e451b-210">Your add-in must specify the `ReadWriteMailbox` permission in its add-in manifest to use the `makeEwsRequestAsync` method.</span></span> <span data-ttu-id="e451b-211">有关使用`ReadWriteMailbox`权限的信息，请参阅[了解 Outlook 加载项权限](understanding-outlook-add-in-permissions.md)中的[ReadWriteMailbox 权限](understanding-outlook-add-in-permissions.md#readwritemailbox-permission)一节。</span><span class="sxs-lookup"><span data-stu-id="e451b-211">For information about using the `ReadWriteMailbox` permission, see the section [ReadWriteMailbox permission](understanding-outlook-add-in-permissions.md#readwritemailbox-permission) in [Understanding Outlook add-in permissions](understanding-outlook-add-in-permissions.md).</span></span>

> [!NOTE]
> <span data-ttu-id="e451b-212">服务器管理员必须使用[set-webservicesvirtualdirectory](/powershell/module/exchange/client-access-servers/New-WebServicesVirtualDirectory?view=exchange-ps)或[Set-webservicesvirtualdirectory](/powershell/module/exchange/client-access-servers/Set-WebServicesVirtualDirectory?view=exchange-ps) Cmldet 将客户端访问服务器 EWS 目录上的_OAuthAuthentication_参数设置为**true** ，才能使该`makeEwsRequestAsync`方法能够发出 EWS 请求。</span><span class="sxs-lookup"><span data-stu-id="e451b-212">The server administrator must use the [New-WebServicesVirtualDirectory](/powershell/module/exchange/client-access-servers/New-WebServicesVirtualDirectory?view=exchange-ps) or the [Set-WebServicesVirtualDirectory](/powershell/module/exchange/client-access-servers/Set-WebServicesVirtualDirectory?view=exchange-ps) cmldet to set the _OAuthAuthentication_ parameter to **true** on the Client Access server EWS directory in order to enable the `makeEwsRequestAsync` method to make EWS requests.</span></span>



## <a name="see-also"></a><span data-ttu-id="e451b-213">另请参阅</span><span class="sxs-lookup"><span data-stu-id="e451b-213">See also</span></span>

- [<span data-ttu-id="e451b-214">Office 加载项的隐私和安全性</span><span class="sxs-lookup"><span data-stu-id="e451b-214">Privacy and security for Office Add-ins</span></span>](../develop/privacy-and-security.md)   
- [<span data-ttu-id="e451b-215">解决 Office 外接程序中的同源策略限制</span><span class="sxs-lookup"><span data-stu-id="e451b-215">Addressing same-origin policy limitations in Office Add-ins</span></span>](../develop/addressing-same-origin-policy-limitations.md)
- [<span data-ttu-id="e451b-216">Exchange 的 EWS 引用</span><span class="sxs-lookup"><span data-stu-id="e451b-216">EWS reference for Exchange</span></span>](/exchange/client-developer/web-service-reference/ews-reference-for-exchange)   
- [<span data-ttu-id="e451b-217">Outlook 和 Exchange 中的 EWS 的邮件应用程序</span><span class="sxs-lookup"><span data-stu-id="e451b-217">Mail apps for Outlook and EWS in Exchange</span></span>](/exchange/client-developer/exchange-web-services/mail-apps-for-outlook-and-ews-in-exchange)
   
<span data-ttu-id="e451b-218">请参阅下文，了解如何使用 ASP.NET Web API 为外接程序创建后端服务：</span><span class="sxs-lookup"><span data-stu-id="e451b-218">See the following for creating backend services for add-ins using ASP.NET Web API:</span></span>

- [<span data-ttu-id="e451b-219">使用 ASP.NET Web API 为 Office 外接程序创建 Web 服务</span><span class="sxs-lookup"><span data-stu-id="e451b-219">Create a web service for an Office Add-in using the ASP.NET Web API</span></span>](https://blogs.msdn.microsoft.com/officeapps/2013/06/10/create-a-web-service-for-an-app-for-office-using-the-asp-net-web-api/)    
- [<span data-ttu-id="e451b-220">使用 ASP.NET Web API 构建 HTTP 服务的基础知识</span><span class="sxs-lookup"><span data-stu-id="e451b-220">The basics of building an HTTP service using ASP.NET Web API</span></span>](https://www.asp.net/web-api)
    