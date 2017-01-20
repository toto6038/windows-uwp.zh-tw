---
author: mcleanbyron
ms.assetid: 37F2C162-4910-4336-BEED-8536C88DCA65
description: "在 Windows 市集提交 API 中使用這些方法，來為登錄到您 Windows 開發人員中心帳戶的應用程式管理套件正式發行前小眾測試版。"
title: "使用 Windows 市集提交 API 管理套件正式發行前小眾測試版"
translationtype: Human Translation
ms.sourcegitcommit: 020c8b3f4d9785842bbe127dd391d92af0962117
ms.openlocfilehash: 626a59ba848c9ae1d97b85b67ef2c76eed49065a

---

# <a name="manage-package-flights-using-the-windows-store-submission-api"></a>使用 Windows 市集提交 API 管理套件正式發行前小眾測試版

使用 Windows 市集提交 API 中的下列方法，管理應用程式的套件正式發行前小眾測試版。 如需 Windows 市集提交 API 的簡介，包括使用此 API 的必要條件，請參閱[使用 Windows 市集服務建立和管理提交](create-and-manage-submissions-using-windows-store-services.md)。

>**注意**&nbsp;&nbsp;這些方法僅供已獲授權使用 Windows 市集提交 API 的 Windows 開發人員中心帳戶使用。 並非所有的帳戶都已啟用此權限。 這些方法只可用來取得、建立或刪除套件正式發行前小眾測試版。 若要為套件正式發行前小眾測試版建立提交，請參閱[管理套件正式發行前小眾測試版提交](manage-flight-submissions.md)中的方法。

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



<!--HONumber=Dec16_HO3-->


