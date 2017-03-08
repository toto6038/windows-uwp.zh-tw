---
author: mcleanbyron
ms.assetid: 37F2C162-4910-4336-BEED-8536C88DCA65
description: "在 Windows 市集提交 API 中使用這些方法，來為登錄到您 Windows 開發人員中心帳戶的應用程式管理套件正式發行前小眾測試版。"
title: "使用 Windows 市集提交 API 管理套件正式發行前小眾測試版"
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Windows 10, UWP, Windows 市集提交 API, 正式發行前小眾測試版"
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 51d7481d0491c85bddcae906a846cb8773f33417
ms.lasthandoff: 02/07/2017

---

# <a name="manage-package-flights-using-the-windows-store-submission-api"></a>使用 Windows 市集提交 API 管理套件正式發行前小眾測試版

使用 Windows 市集提交 API 中的下列方法，管理應用程式的套件正式發行前小眾測試版。 如需 Windows 市集提交 API 的簡介，包括使用此 API 的必要條件，請參閱[使用 Windows 市集服務建立和管理提交](create-and-manage-submissions-using-windows-store-services.md)。

>**注意**&nbsp;&nbsp;這些方法僅供已獲授權使用 Windows 市集提交 API 的 Windows 開發人員中心帳戶使用。 此權限是在各個階段中針對開發人員帳戶啟用，並非所有帳戶目前都啟用此權限。 若要要求早一點存取，請登入開發人員中心儀表板，按一下儀表板下方的 **\[意見反應\]**，選取意見反應區域的 **\[提交 API\]**，並提交您的要求。 當您的帳戶啟用此權限時，您會收到電子郵件。

這些方法只可用來取得、建立或刪除套件正式發行前小眾測試版。 若要為套件正式發行前小眾測試版建立提交，請參閱[管理套件正式發行前小眾測試版提交](manage-flight-submissions.md)中的方法。

<table>
<colgroup>
<col width="10%" />
<col width="30%" />
<col width="60%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">方法</th>
<th align="left">URI</th>
<th align="left">描述</th>
</tr>
</thead>
<tbody>
<tr>
<td align="left">GET</td>
<td align="left">```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}```</td>
<td align="left">[取得套件正式發行前小眾測試版](get-a-flight.md)</td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights```</td>
<td align="left">[建立套件正式發行前小眾測試版](create-a-flight.md)</td>
</tr>
<tr>
<td align="left">DELETE</td>
<td align="left">```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}```</td>
<td align="left">[刪除套件正式發行前小眾測試版](delete-a-flight.md)</td>
</tr>
</tbody>
</table>

## <a name="prerequisites"></a>必要條件

如果您尚未完成，請先完成 Windows 市集提交 API 的所有[必要條件](create-and-manage-submissions-using-windows-store-services.md#prerequisites)，然後再嘗試使用這其中的任何方法。

## <a name="related-topics"></a>相關主題

* [使用 Windows 市集服務建立和管理提交](create-and-manage-submissions-using-windows-store-services.md)
* [管理套件正式發行前小眾測試版提交](manage-flight-submissions.md)

