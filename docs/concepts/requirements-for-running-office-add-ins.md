---
title: ?? Office ??????
description: ''
ms.date: 02/09/2018
ms.openlocfilehash: a4859af73d8e9cf041990a3533894b24f1cbde6f
ms.sourcegitcommit: c72c35e8389c47a795afbac1b2bcf98c8e216d82
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/23/2018
---
# <a name="requirements-for-running-office-add-ins"></a>?? Office ??????

??????? Office ????????????

> [!NOTE]
> ????????[??](../publish/publish.md)? AppSource ???? Office ???????? [AppSource ????](https://docs.microsoft.com/en-us/office/dev/store/validation-policies)??????????????????????????????????????????[? 4.12 ??](https://docs.microsoft.com/en-us/office/dev/store/validation-policies#4-apps-and-add-ins-behave-predictably)?? [Office ?????????](../overview/office-add-in-availability.md)???? 

???? Office ??????????????? [Office ???????????](../overview/office-add-in-availability.md)?

## <a name="server-requirements"></a>?????

??????????? Office ??????????? UI ??????????????????????????????

???????????????Outlook ????????????????????????????????????? Web ???? Web ?????? [Microsoft Azure](../publish/host-an-office-add-in-on-microsoft-azure.md)?

[!include[HTTPS guidance](../includes/https-guidance.md)]

> [!TIP]
> ? Visual Studio ???????????Visual Studio ?? IIS Express ??????????????????????? Web ???? 

??????????????????? Office ???????Access Web App?Word?Excel?PowerPoint ? Project??????? SharePoint ???? [??????](../publish/publish-task-pane-and-content-add-ins-to-an-add-in-catalog.md)????????? XML ?????

?????? Outlook ???????? Outlook ?????????? Exchange 2013 ?????????? Office 365?Exchange Online ????????????????????????? Outlook ??????????

> [!NOTE]
> Outlook ?? POP ? IMAP ????????? Office ????

## <a name="client-requirements-windows-desktop-and-tablet"></a>??????Windows ????????

??? Windows ???????????????????????? Office ?????? Web ????? Office ????????????


- ?? Windows x86 ? x64 ?????????? Surface Pro??
    - ? Windows 7 ????????? 32 ?? 64 ??? Office 2013?
    - Excel 2013?Outlook 2013?PowerPoint 2013?Project Professional 2013?Project 2013 SP1?Word 2013 ?????? Office ??????????????? Office ?????????? Office ??????Office ??????????????????????????????
    
  ?? Office 365 ?????????? Office 2013??????? CDN ?????????       
    - [Office 2013 ??? (.exe)](https://c2rsetup.officeapps.live.com/c2r/download.aspx?productReleaseID=O365BusinessRetail&platform=X86&language=en-us&version=O15GA&source=O15OLSO365) 
    - [Office 2013 ??? (.exe)](https://c2rsetup.officeapps.live.com/c2r/download.aspx?productReleaseID=O365HomePremRetail&platform=X86&language=en-us&version=O15GA&source=O15OLSO365) 

- ????? Internet Explorer 11 ?????????????????? Office ?????????? Office ????????????? Internet Explorer 11 ??????????
- ????????????????Internet Explorer 11 ??????? Microsoft Edge?Chrome?Firefox ? Safari (Mac OS) ??????
- HTML ? JavaScript ??????????[Visual Studio ? Microsoft ??????](https://www.visualstudio.com/features/office-tools-vs) ???? Web ?????

## <a name="client-requirements-os-x-desktop"></a>??????OS X ??

?? Office 365 ??????? ??? Mac ? Outlook ?? Outlook ?????? ??? Mac ? Outlook ??? Outlook ????? ??? Mac ? Outlook ????????????????? OS X v10.10"Yosemite"??? ??? Mac ? Outlook ?? WebKit ????????????????????????????

????? Office ????? Office for Mac ?????????

- Word for Mac ?? 15.18 (160109) 
- Excel for Mac ?? 15.19 (160206) 
- PowerPoint for Mac ?? 15.24 (160614)

## <a name="client-requirements-browser-support-for-office-online-web-clients-and-sharepoint"></a>???????? Office Online Web ???? SharePoint ??????

?? ECMAScript 5.1?HTML5 ? CSS3 ???????? Internet Explorer 11 ??????? Microsoft Edge?Chrome?Firefox ? Safari (Mac OS) ??????


## <a name="client-requirements-non-windows-smartphone-and-tablet"></a>??????? Windows ?????????

???????????? Windows ??????????????? ?????? OWA ? Outlook Web App?????? Outlook ???????????


| ?????? | ?? | ???? | Exchange ?? | ????? |
|:-----|:-----|:-----|:-----|:-----|
|OWA for Android|Android ??????????? [Android OS](https://developer.android.com/guide/practices/screens_support.html) ???????"??"?"??"?|Android 4.4 KitKat ?????|?? Office 365 ??? ? Exchange Online ?????|?? Android ?????????????|
|OWA for iPad|iPad 2 ?????|iOS 6 ?????|?? Office 365 ??? ? Exchange Online ?????|?? iOS ?????????????|
|OWA for iPhone|iPhone 4S ?????|iOS 6 ?????|?? Office 365 ??? ? Exchange Online ?????|?? iOS ?????????????|
|Outlook Web App|iPhone 4 ??????iPad 2 ??????iPod Touch 4 ?????|iOS 5 ?????|?? Office 365?Exchange Online ???? Exchange Server 2013 ?????|Safari|


## <a name="see-also"></a>????

- [Office ???????](../overview/office-add-ins.md)
- [Office ???????????](../overview/office-add-in-availability.md)