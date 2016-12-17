---
author: mcleanbyron
ms.assetid: 37F2C162-4910-4336-BEED-8536C88DCA65
description: "在 Windows 市集提交 API 中使用這些方法，來為登錄到您 Windows 開發人員中心帳戶的 App 管理套件正式發行前小眾測試版。"
title: "使用 Windows 市集提交 API 管理套件正式發行前小眾測試版"
translationtype: Human Translation
ms.sourcegitcommit: 5f975d0a99539292e1ce91ca09dbd5fac11c4a49
ms.openlocfilehash: de1c23d721cee67d813520e3a23eb553cd90b7e9

---

# 使用 Windows 市集提交 API 管理套件正式發行前小眾測試版




在 Windows 市集提交 API 中使用下列方法，來為登錄到您 Windows 開發人員中心帳戶的 App 管理套件正式發行前小眾測試版。 如需 Windows 市集提交 API 的簡介，包括使用此 API 的先決條件，請參閱[使用 Windows 市集服務建立和管理提交](create-and-manage-submissions-using-windows-store-services.md)。

>**注意**  這些方法僅供已獲授權使用 Windows 市集提交 API 的 Windows 開發人員中心帳戶使用。 並非所有的帳戶都已啟用此權限。 這些方法只可用來取得、建立或刪除套件正式發行前小眾測試版。 若要為套件正式發行前小眾測試版建立提交，請參閱[管理套件正式發行前小眾測試版提交](manage-flight-submissions.md)中的方法。

| 方法        | URI    | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| GET | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}``` | 為登錄到您 Windows 開發人員中心帳戶的 App 取得套件正式發行前小眾測試版的資料。 如需詳細資訊，請參閱[取得套件正式發行前小眾測試版](get-a-flight.md)。 |
| POST | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights``` | 建立新的套件正式發行前小眾測試版。 如需詳細資訊，請參閱[建立套件正式發行前小眾測試版](create-a-flight.md)。|
| DELETE | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}``` | 刪除套件正式發行前小眾測試版。 如需詳細資訊，請參閱[刪除套件正式發行前小眾測試版](delete-a-flight.md)。 |


## 先決條件

如果您尚未完成，請先完成 Windows 市集提交 API 的所有[先決條件](create-and-manage-submissions-using-windows-store-services.md#prerequisites)，然後再嘗試使用這其中的任何方法。

## 相關主題

* [使用 Windows 市集服務建立和管理提交](create-and-manage-submissions-using-windows-store-services.md)
* [管理套件正式發行前小眾測試版提交](manage-flight-submissions.md)



<!--HONumber=Aug16_HO5-->


