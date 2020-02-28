---
title: 获取 Outlook 加载项中的附件
description: 加载项可使用附件 API 将与附件相关的信息发送至远程服务。
ms.date: 01/13/2020
localization_priority: Normal
ms.openlocfilehash: 7188359193e675f53d0e8358c75f03669b34a170
ms.sourcegitcommit: a3ddfdb8a95477850148c4177e20e56a8673517c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/20/2020
ms.locfileid: "42166011"
---
# <a name="get-attachments-of-an-outlook-item-from-the-server"></a><span data-ttu-id="811ac-103">从服务器获取 Outlook 项的附件</span><span class="sxs-lookup"><span data-stu-id="811ac-103">Get attachments of an Outlook item from the server</span></span>

<span data-ttu-id="811ac-p101">Outlook 外接程序无法将选定项目上的附件直接传递给在您服务器上运行的远程服务。但是，外接程序可以使用附件 API 将关于附件的信息发送到远程服务。然后，该服务可以直接联系 Exchange 服务器来检索附件。</span><span class="sxs-lookup"><span data-stu-id="811ac-p101">An Outlook add-in cannot pass the attachments of a selected item directly to the remote service that runs on your server. Instead, the add-in can use the attachments API to send information about the attachments to the remote service. The service can then contact the Exchange server directly to retrieve the attachments.</span></span>

<span data-ttu-id="811ac-107">要向远程服务发送附件信息，您可以使用以下属性和函数：</span><span class="sxs-lookup"><span data-stu-id="811ac-107">To send attachment information to the remote service, you use the following properties and function:</span></span>

- <span data-ttu-id="811ac-p102">[Office.context.mailbox.ewsUrl](/javascript/api/outlook/office.entities) 属性 &ndash; 提供托管邮箱的 Exchange 服务器上 Exchange Web 服务 (EWS) 的 URL。服务使用此 URL 调用 [ExchangeService.GetAttachments](/exchange/client-developer/exchange-web-services/how-to-get-attachments-by-using-ews-in-exchange) 方法或 [GetAttachment](/exchange/client-developer/web-service-reference/getattachment-operation) EWS 操作。</span><span class="sxs-lookup"><span data-stu-id="811ac-p102">[Office.context.mailbox.ewsUrl](/javascript/api/outlook/office.entities) property &ndash; Provides the URL of Exchange Web Services (EWS) on the Exchange server that hosts the mailbox. Your service uses this URL to call the [ExchangeService.GetAttachments](/exchange/client-developer/exchange-web-services/how-to-get-attachments-by-using-ews-in-exchange) method, or the [GetAttachment](/exchange/client-developer/web-service-reference/getattachment-operation) EWS operation.</span></span>

- <span data-ttu-id="811ac-110">[Office.context.mailbox.item.attachments](../reference/objectmodel/preview-requirement-set/office.context.mailbox.item.md#properties) 属性 &ndash; 获取一组 [AttachmentDetails](/javascript/api/outlook/office.attachmentdetails) 对象，每个对象对应于一个项附件。</span><span class="sxs-lookup"><span data-stu-id="811ac-110">[Office.context.mailbox.item.attachments](../reference/objectmodel/preview-requirement-set/office.context.mailbox.item.md#properties) property &ndash; Gets an array of [AttachmentDetails](/javascript/api/outlook/office.attachmentdetails) objects, one for each attachment to the item.</span></span>

- <span data-ttu-id="811ac-111">[Office.context.mailbox.getCallbackTokenAsync](../reference/objectmodel/preview-requirement-set/office.context.mailbox.md#methods) 函数 &ndash; 对托管邮箱的 Exchange 服务执行异步调用，以获取服务器发回给 Exchange 服务器的回调令牌，从而验证附件请求。</span><span class="sxs-lookup"><span data-stu-id="811ac-111">[Office.context.mailbox.getCallbackTokenAsync](../reference/objectmodel/preview-requirement-set/office.context.mailbox.md#methods) function &ndash; Makes an asynchronous call to the Exchange server that hosts the mailbox to get a callback token that the server sends back to the Exchange server to authenticate a request for an attachment.</span></span>

## <a name="using-the-attachments-api"></a><span data-ttu-id="811ac-112">使用附件 API</span><span class="sxs-lookup"><span data-stu-id="811ac-112">Using the attachments API</span></span>

<span data-ttu-id="811ac-113">若要使用附件 API 获取 Exchange 邮箱中的附件，请执行以下步骤：</span><span class="sxs-lookup"><span data-stu-id="811ac-113">To use the attachments API to get attachments from an Exchange mailbox, perform the following steps:</span></span>

1. <span data-ttu-id="811ac-114">当用户查看包含附件的邮件或约会时显示外接程序。</span><span class="sxs-lookup"><span data-stu-id="811ac-114">Show the add-in when the user is viewing a message or appointment that contains an attachment.</span></span>

1. <span data-ttu-id="811ac-115">从 Exchange 服务器获取回调令牌。</span><span class="sxs-lookup"><span data-stu-id="811ac-115">Get the callback token from the Exchange server.</span></span>

1. <span data-ttu-id="811ac-116">将回调令牌和附件信息发给远程服务。</span><span class="sxs-lookup"><span data-stu-id="811ac-116">Send the callback token and attachment information to the remote service.</span></span>

1. <span data-ttu-id="811ac-117">使用 `ExchangeService.GetAttachments` 方法或 `GetAttachment` 操作获取来自 Exchange 服务器的附件。</span><span class="sxs-lookup"><span data-stu-id="811ac-117">Get the attachments from the Exchange server by using the `ExchangeService.GetAttachments` method or the `GetAttachment` operation.</span></span>

<span data-ttu-id="811ac-118">以下各节使用 [Outlook-Add-in-JavaScript-GetAttachments](https://github.com/OfficeDev/Outlook-Add-in-JavaScript-GetAttachments) 示例中的代码详细介绍了每个步骤。</span><span class="sxs-lookup"><span data-stu-id="811ac-118">Each of these steps is covered in detail in the following sections using code from the [Outlook-Add-in-JavaScript-GetAttachments](https://github.com/OfficeDev/Outlook-Add-in-JavaScript-GetAttachments) sample.</span></span>

> [!NOTE]
> <span data-ttu-id="811ac-p103">为强调附件信息，这些示例中的代码已被缩短。该示例包含用于向远程服务器进行外接程序身份验证和管理请求状态的额外代码。</span><span class="sxs-lookup"><span data-stu-id="811ac-p103">The code in these examples has been shortened to emphasize the attachment information. The sample contains additional code for authenticating the add-in with the remote server and managing the state of the request.</span></span>

## <a name="get-a-callback-token"></a><span data-ttu-id="811ac-121">获取回调令牌</span><span class="sxs-lookup"><span data-stu-id="811ac-121">Get a callback token</span></span>

<span data-ttu-id="811ac-122">[Office.context.mailbox](../reference/objectmodel/preview-requirement-set/office.context.mailbox.md) 对象提供 `getCallbackTokenAsync` 函数来获取远程服务器可以用来对 Exchange Server 进行身份验证的令牌。</span><span class="sxs-lookup"><span data-stu-id="811ac-122">The [Office.context.mailbox](../reference/objectmodel/preview-requirement-set/office.context.mailbox.md) object provides the `getCallbackTokenAsync` function to get a token that the remote server can use to authenticate with the Exchange server.</span></span> <span data-ttu-id="811ac-123">以下代码显示了加载项中的一个函数，它启动异步请求来获取回调令牌以及用于获取响应的回调函数。</span><span class="sxs-lookup"><span data-stu-id="811ac-123">The following code shows a function in an add-in that starts the asynchronous request to get the callback token, and the callback function that gets the response.</span></span> <span data-ttu-id="811ac-124">回调令牌存储在在下一部分定义的服务请求对象中。</span><span class="sxs-lookup"><span data-stu-id="811ac-124">The callback token is stored in the service request object that is defined in the next section.</span></span>

```js
function getAttachmentToken() {
    if (serviceRequest.attachmentToken == "") {
        Office.context.mailbox.getCallbackTokenAsync(attachmentTokenCallback);
    }
}

function attachmentTokenCallback(asyncResult, userContext) {
    if (asyncResult.status === "succeeded") {
        // Cache the result from the server.
        serviceRequest.attachmentToken = asyncResult.value;
        serviceRequest.state = 3;
        testAttachments();
    } else {
        showToast("Error", "Could not get callback token: " + asyncResult.error.message);
    }
}
```

## <a name="send-attachment-information-to-the-remote-service"></a><span data-ttu-id="811ac-125">向远程服务发送附件信息</span><span class="sxs-lookup"><span data-stu-id="811ac-125">Send attachment information to the remote service</span></span>

<span data-ttu-id="811ac-p105">外接程序调用的远程服务定义了应该如何向该服务发送附件信息的详情。在本示例中，远程服务是通过使用 Visual Studio 2013 创建的一个 Web API 应用程序。远程服务需要 JSON 对象中的附件信息。以下代码初始化一个包含附件信息的对象。</span><span class="sxs-lookup"><span data-stu-id="811ac-p105">The remote service that your add-in calls defines the specifics of how you should send the attachment information to the service. In this example, the remote service is a Web API application created by using Visual Studio 2013. The remote service expects the attachment information in a JSON object. The following code initializes an object that contains the attachment information.</span></span>

```js
// Initialize a context object for the add-in.
//   Set the fields that are used on the request
//   object to default values.
 var serviceRequest = {
    attachmentToken: '',
    ewsUrl         : Office.context.mailbox.ewsUrl,
    attachments    : []
 };
```

<br/>

<span data-ttu-id="811ac-130">`Office.context.mailbox.item.attachments` 属性包含 `AttachmentDetails` 对象的集合，每个对象对应于项的每个附件。</span><span class="sxs-lookup"><span data-stu-id="811ac-130">The `Office.context.mailbox.item.attachments` property contains a collection of `AttachmentDetails` objects, one for each attachment to the item.</span></span> <span data-ttu-id="811ac-131">在大多数情况下，加载项可以只将 `AttachmentDetails` 对象的附件 ID 属性传递到远程服务。</span><span class="sxs-lookup"><span data-stu-id="811ac-131">In most cases, the add-in can pass just the attachment ID property of an `AttachmentDetails` object to the remote service.</span></span> <span data-ttu-id="811ac-132">如果远程服务需要有关附件的更多详细信息，可以传递全部或部分 `AttachmentDetails` 对象。</span><span class="sxs-lookup"><span data-stu-id="811ac-132">If the remote service needs more details about the attachment, you can pass all or part of the `AttachmentDetails` object.</span></span> <span data-ttu-id="811ac-133">以下代码定义了将整个 `AttachmentDetails` 数组置于 `serviceRequest` 对象中并向远程服务发送请求的方法。</span><span class="sxs-lookup"><span data-stu-id="811ac-133">The following code defines a method that puts the entire `AttachmentDetails` array in the `serviceRequest` object and sends a request to the remote service.</span></span>

```js
function makeServiceRequest() {
  // Format the attachment details for sending.
  for (var i = 0; i < mailbox.item.attachments.length; i++) {
    serviceRequest.attachments[i] = JSON.parse(JSON.stringify(mailbox.item.attachments[i]));
  }

  $.ajax({
    url: '../../api/Default',
    type: 'POST',
    data: JSON.stringify(serviceRequest),
    contentType: 'application/json;charset=utf-8'
  }).done(function (response) {
    if (!response.isError) {
      var names = "<h2>Attachments processed using " +
                    serviceRequest.service +
                    ": " +
                    response.attachmentsProcessed +
                    "</h2>";
      for (i = 0; i < response.attachmentNames.length; i++) {
        names += response.attachmentNames[i] + "<br />";
      }
      document.getElementById("names").innerHTML = names;
    } else {
      app.showNotification("Runtime error", response.message);
    }
  }).fail(function (status) {

  }).always(function () {
    $('.disable-while-sending').prop('disabled', false);
  })
}
```

## <a name="get-the-attachments-from-the-exchange-server"></a><span data-ttu-id="811ac-134">从 Exchange 服务器获取附件</span><span class="sxs-lookup"><span data-stu-id="811ac-134">Get the attachments from the Exchange server</span></span>

<span data-ttu-id="811ac-p107">您的远程服务可以使用 [GetAttachments](/exchange/client-developer/exchange-web-services/how-to-get-attachments-by-using-ews-in-exchange) EWS 托管 API 方法或 [GetAttachment](/exchange/client-developer/web-service-reference/getattachment-operation) EWS 操作从服务器检索附件。服务器应用程序需要两个对象将 JSON 字符串反序列化为可在服务器上使用的 .NET Framework 对象。以下代码显示了反序列化对象的定义。</span><span class="sxs-lookup"><span data-stu-id="811ac-p107">Your remote service can use either the [GetAttachments](/exchange/client-developer/exchange-web-services/how-to-get-attachments-by-using-ews-in-exchange) EWS Managed API method or the [GetAttachment](/exchange/client-developer/web-service-reference/getattachment-operation) EWS operation to retrieve attachments from the server. The service application needs two objects to deserialize the JSON string into .NET Framework objects that can be used on the server. The following code shows the definitions of the deserialization objects.</span></span>

```cs
namespace AttachmentsSample
{
  public class AttachmentSampleServiceRequest
  {
    public string attachmentToken { get; set; }
    public string ewsUrl { get; set; }
    public string service { get; set; }
    public AttachmentDetails [] attachments { get; set; }
  }

  public class AttachmentDetails
  {
    public string attachmentType { get; set; }
    public string contentType { get; set; }
    public string id { get; set; }
    public bool isInline { get; set; }
    public string name { get; set; }
    public int size { get; set; }
  }
}
```

### <a name="use-the-ews-managed-api-to-get-the-attachments"></a><span data-ttu-id="811ac-138">使用 EWS 托管 API 来获取附件</span><span class="sxs-lookup"><span data-stu-id="811ac-138">Use the EWS Managed API to get the attachments</span></span>

<span data-ttu-id="811ac-p108">如果使用远程服务中的 [EWS 托管 API](https://go.microsoft.com/fwlink/?LinkID=255472)，则可以使用 [GetAttachments](/exchange/client-developer/exchange-web-services/how-to-get-attachments-by-using-ews-in-exchange) 方法构建、发送和接收 EWS SOAP 请求以获取附件。我们建议您使用 EWS 托管 API，因为它所需的代码行数较少，并可提供用于调用 EWS 的更为直观的界面。以下代码发出一个检索所有附件的请求，并返回已处理附件的数目和名称。</span><span class="sxs-lookup"><span data-stu-id="811ac-p108">If you use the [EWS Managed API](https://go.microsoft.com/fwlink/?LinkID=255472) in your remote service, you can use the [GetAttachments](/exchange/client-developer/exchange-web-services/how-to-get-attachments-by-using-ews-in-exchange) method, which will construct, send, and receive an EWS SOAP request to get the attachments. We recommend that you use the EWS Managed API because it requires fewer lines of code and provides a more intuitive interface for making calls to EWS. The following code makes one request to retrieve all the attachments, and returns the count and names of the attachments processed.</span></span>

```cs
private AttachmentSampleServiceResponse GetAtttachmentsFromExchangeServerUsingEWSManagedApi(AttachmentSampleServiceRequest request)
{
  var attachmentsProcessedCount = 0;
  var attachmentNames = new List<string>();

  // Create an ExchangeService object, set the credentials and the EWS URL.
  ExchangeService service = new ExchangeService();
  service.Credentials = new OAuthCredentials(request.attachmentToken);
  service.Url = new Uri(request.ewsUrl);

  var attachmentIds = new List<string>();

  foreach (AttachmentDetails attachment in request.attachments)
  {
    attachmentIds.Add(attachment.id);
  }

  // Call the GetAttachments method to retrieve the attachments on the message.
  // This method results in a GetAttachments EWS SOAP request and response
  // from the Exchange server.
  var getAttachmentsResponse =
    service.GetAttachments(attachmentIds.ToArray(),
                            null,
                            new PropertySet(BasePropertySet.FirstClassProperties,
                                            ItemSchema.MimeContent));

  if (getAttachmentsResponse.OverallResult == ServiceResult.Success)
  {
    foreach (var attachmentResponse in getAttachmentsResponse)
    {
      attachmentNames.Add(attachmentResponse.Attachment.Name);

      // Write the content of each attachment to a stream.
      if (attachmentResponse.Attachment is FileAttachment)
      {
        FileAttachment fileAttachment = attachmentResponse.Attachment as FileAttachment;
        Stream s = new MemoryStream(fileAttachment.Content);
        // Process the contents of the attachment here.
      }

      if (attachmentResponse.Attachment is ItemAttachment)
      {
        ItemAttachment itemAttachment = attachmentResponse.Attachment as ItemAttachment;
        Stream s = new MemoryStream(itemAttachment.Item.MimeContent.Content);
        // Process the contents of the attachment here.
      }

      attachmentsProcessedCount++;
    }
  }

  // Return the names and number of attachments processed for display
  // in the add-in UI.
  var response = new AttachmentSampleServiceResponse();
  response.attachmentNames = attachmentNames.ToArray();
  response.attachmentsProcessed = attachmentsProcessedCount;

  return response;
}
```

### <a name="use-ews-to-get-the-attachments"></a><span data-ttu-id="811ac-142">使用 EWS 获取附件</span><span class="sxs-lookup"><span data-stu-id="811ac-142">Use EWS to get the attachments</span></span>

<span data-ttu-id="811ac-143">如果在远程服务中使用 EWS，则需要构造 [GetAttachment](/exchange/client-developer/web-service-reference/getattachment-operation) SOAP 请求，从 Exchange Server 获取附件。</span><span class="sxs-lookup"><span data-stu-id="811ac-143">If you use EWS in your remote service, you need to construct a [GetAttachment](/exchange/client-developer/web-service-reference/getattachment-operation) SOAP request to get the attachments from the Exchange server.</span></span> <span data-ttu-id="811ac-144">下面的代码返回了提供 SOAP 请求的字符串。</span><span class="sxs-lookup"><span data-stu-id="811ac-144">The following code returns a string that provides the SOAP request.</span></span> <span data-ttu-id="811ac-145">远程服务使用 `String.Format` 方法将附件的附件 ID 插入字符串。</span><span class="sxs-lookup"><span data-stu-id="811ac-145">The remote service uses the `String.Format` method to insert the attachment ID for an attachment into the string.</span></span>


```cs
private const string GetAttachmentSoapRequest =
@"<?xml version=""1.0"" encoding=""utf-8""?>
<soap:Envelope xmlns:xsi=""https://www.w3.org/2001/XMLSchema-instance""
xmlns:xsd=""https://www.w3.org/2001/XMLSchema""
xmlns:soap=""http://schemas.xmlsoap.org/soap/envelope/""
xmlns:t=""http://schemas.microsoft.com/exchange/services/2006/types"">
<soap:Header>
<t:RequestServerVersion Version=""Exchange2013"" />
</soap:Header>
  <soap:Body>
    <GetAttachment xmlns=""http://schemas.microsoft.com/exchange/services/2006/messages""
    xmlns:t=""http://schemas.microsoft.com/exchange/services/2006/types"">
      <AttachmentShape/>
      <AttachmentIds>
        <t:AttachmentId Id=""{0}""/>
      </AttachmentIds>
    </GetAttachment>
  </soap:Body>
</soap:Envelope>";
```

<br/>

<span data-ttu-id="811ac-146">最后，下列方法使用 EWS `GetAttachment` 请求从 Exchange Server 获取附件。</span><span class="sxs-lookup"><span data-stu-id="811ac-146">Finally, the following method does the work of using an EWS `GetAttachment` request to get the attachments from the Exchange server.</span></span> <span data-ttu-id="811ac-147">此实现分别对各个附件发起请求并返回已处理附件的计数。</span><span class="sxs-lookup"><span data-stu-id="811ac-147">This implementation makes an individual request for each attachment, and returns the count of attachments processed.</span></span> <span data-ttu-id="811ac-148">在单独的 `ProcessXmlResponse` 方法中处理每个响应，然后进行定义。</span><span class="sxs-lookup"><span data-stu-id="811ac-148">Each response is processed in a separate `ProcessXmlResponse` method, defined next.</span></span>

```cs
private AttachmentSampleServiceResponse GetAttachmentsFromExchangeServerUsingEWS(AttachmentSampleServiceRequest request)
{
  var attachmentsProcessedCount = 0;
  var attachmentNames = new List<string>();

  foreach (var attachment in request.attachments)
  {
    // Prepare a web request object.
    HttpWebRequest webRequest = WebRequest.CreateHttp(request.ewsUrl);
    webRequest.Headers.Add("Authorization",
      string.Format("Bearer {0}", request.attachmentToken));
    webRequest.PreAuthenticate = true;
    webRequest.AllowAutoRedirect = false;
    webRequest.Method = "POST";
    webRequest.ContentType = "text/xml; charset=utf-8";

    // Construct the SOAP message for the GetAttachment operation.
    byte[] bodyBytes = Encoding.UTF8.GetBytes(
      string.Format(GetAttachmentSoapRequest, attachment.id));
    webRequest.ContentLength = bodyBytes.Length;

    Stream requestStream = webRequest.GetRequestStream();
    requestStream.Write(bodyBytes, 0, bodyBytes.Length);
    requestStream.Close();

    // Make the request to the Exchange server and get the response.
    HttpWebResponse webResponse = (HttpWebResponse)webRequest.GetResponse();

    // If the response is okay, create an XML document from the response
    // and process the request.
    if (webResponse.StatusCode == HttpStatusCode.OK)
    {
      var responseStream = webResponse.GetResponseStream();

      var responseEnvelope = XElement.Load(responseStream);

      // After creating a memory stream containing the contents of the
      // attachment, this method writes the XML document to the trace output.
      // Your service would perform it's processing here.
      if (responseEnvelope != null)
      {
        var processResult = ProcessXmlResponse(responseEnvelope);
        attachmentNames.Add(string.Format("{0} {1}", attachment.name, processResult));

      }

      // Close the response stream.
      responseStream.Close();
      webResponse.Close();

    }
    // If the response is not OK, return an error message for the
    // attachment.
    else
    {
      var errorString = string.Format("Attachment \"{0}\" could not be processed. " +
        "Error message: {1}.", attachment.name, webResponse.StatusDescription);
      attachmentNames.Add(errorString);
    }
    attachmentsProcessedCount++;
  }

  // Return the names and number of attachments processed for display
  // in the add-in UI.
  var response = new AttachmentSampleServiceResponse();
  response.attachmentNames = attachmentNames.ToArray();
  response.attachmentsProcessed = attachmentsProcessedCount;

  return response;
}
```

<br/>

<span data-ttu-id="811ac-149">将 `GetAttachment` 操作的每个响应发送给 `ProcessXmlResponse` 方法。</span><span class="sxs-lookup"><span data-stu-id="811ac-149">Each response from the `GetAttachment` operation is sent to the `ProcessXmlResponse` method.</span></span> <span data-ttu-id="811ac-150">此方法检查响应是否出错。</span><span class="sxs-lookup"><span data-stu-id="811ac-150">This method checks the response for errors.</span></span> <span data-ttu-id="811ac-151">如果它未找到任何错误，则会处理文件附件和项附件。</span><span class="sxs-lookup"><span data-stu-id="811ac-151">If it doesn't find any errors, it processes file attachments and item attachments.</span></span> <span data-ttu-id="811ac-152">`ProcessXmlResponse` 方法执行大量工作来处理附件。</span><span class="sxs-lookup"><span data-stu-id="811ac-152">The `ProcessXmlResponse` method performs the bulk of the work to process the attachment.</span></span>

```cs
// This method processes the response from the Exchange server.
// In your application the bulk of the processing occurs here.
private string ProcessXmlResponse(XElement responseEnvelope)
{
  // First, check the response for web service errors.
  var errorCodes = from errorCode in responseEnvelope.Descendants
                    ("{http://schemas.microsoft.com/exchange/services/2006/messages}ResponseCode")
                    select errorCode;
  // Return the first error code found.
  foreach (var errorCode in errorCodes)
  {
    if (errorCode.Value != "NoError")
    {
      return string.Format("Could not process result. Error: {0}", errorCode.Value);
    }
  }

  // No errors found, proceed with processing the content.
  // First, get and process file attachments.
  var fileAttachments = from fileAttachment in responseEnvelope.Descendants
                    ("{http://schemas.microsoft.com/exchange/services/2006/types}FileAttachment")
                        select fileAttachment;
  foreach(var fileAttachment in fileAttachments)
  {
    var fileContent = fileAttachment.Element("{http://schemas.microsoft.com/exchange/services/2006/types}Content");
    var fileData = System.Convert.FromBase64String(fileContent.Value);
    var s = new MemoryStream(fileData);
    // Process the file attachment here.
  }

  // Second, get and process item attachments.
  var itemAttachments = from itemAttachment in responseEnvelope.Descendants
                        ("{http://schemas.microsoft.com/exchange/services/2006/types}ItemAttachment")
                        select itemAttachment;
  foreach(var itemAttachment in itemAttachments)
  {
    var message = itemAttachment.Element("{http://schemas.microsoft.com/exchange/services/2006/types}Message");
    if (message != null)
    {
      // Process a message here.
      break;
    }
    var calendarItem = itemAttachment.Element("{http://schemas.microsoft.com/exchange/services/2006/types}CalendarItem");
    if (calendarItem != null)
    {
      // Process calendar item here.
      break;
    }
    var contact = itemAttachment.Element("{http://schemas.microsoft.com/exchange/services/2006/types}Contact");
    if (contact != null)
    {
      // Process contact here.
      break;
    }
    var task = itemAttachment.Element("{http://schemas.microsoft.com/exchange/services/2006/types}Tontact");
    if (task != null)
    {
      // Process task here.
      break;
    }
    var meetingMessage = itemAttachment.Element("{http://schemas.microsoft.com/exchange/services/2006/types}MeetingMessage");
    if (meetingMessage != null)
    {
      // Process meeting message here.
      break;
    }
    var meetingRequest = itemAttachment.Element("{http://schemas.microsoft.com/exchange/services/2006/types}MeetingRequest");
    if (meetingRequest != null)
    {
      // Process meeting request here.
      break;
    }
    var meetingResponse = itemAttachment.Element("{http://schemas.microsoft.com/exchange/services/2006/types}MeetingResponse");
    if (meetingResponse != null)
    {
      // Process meeting response here.
      break;
    }
    var meetingCancellation = itemAttachment.Element("{http://schemas.microsoft.com/exchange/services/2006/types}MeetingCancellation");
    if (meetingCancellation != null)
    {
      // Process meeting cancellation here.
      break;
    }
  }

  return string.Empty;
}
```

## <a name="see-also"></a><span data-ttu-id="811ac-153">另请参阅</span><span class="sxs-lookup"><span data-stu-id="811ac-153">See also</span></span>

- [<span data-ttu-id="811ac-154">创建适用于阅读窗体的 Outlook 加载项</span><span class="sxs-lookup"><span data-stu-id="811ac-154">Create Outlook add-ins for read forms</span></span>](read-scenario.md)
- [<span data-ttu-id="811ac-155">在 Exchange 中浏览 EWS 托管 API、EWS 和 Web 服务</span><span class="sxs-lookup"><span data-stu-id="811ac-155">Explore the EWS Managed API, EWS, and web services in Exchange</span></span>](/exchange/client-developer/exchange-web-services/explore-the-ews-managed-api-ews-and-web-services-in-exchange)
- [<span data-ttu-id="811ac-156">EWS 托管 API 客户端应用程序入门</span><span class="sxs-lookup"><span data-stu-id="811ac-156">Get started with EWS Managed API client applications</span></span>](/exchange/client-developer/exchange-web-services/get-started-with-ews-managed-api-client-applications)
- [<span data-ttu-id="811ac-157">AttachmentsDemo 示例 Outlook 加载项</span><span class="sxs-lookup"><span data-stu-id="811ac-157">AttachmentsDemo Sample Outlook add-in</span></span>](https://github.com/OfficeDev/outlook-add-in-attachments-demo)