---
author: mcleanbyron
ms.assetid: 3D6EE7D7-7D75-499D-AA7A-55DA1C485BA6
description: "在 Windows 市集分析 API 中使用此方法，以下載適用於 Windows 10 驅動程式錯誤的 CAB 檔案。 這個方法僅供 IHV 使用。"
title: "下載適用於 Windows 10 驅動程式錯誤的 CAB 檔案"
ms.author: mcleans
ms.date: 03/17/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, Windows 市集分析 API, 下載 CAB"
ms.openlocfilehash: 9a8539a3d9d8f85fd9277165eb393eb372459b0a
ms.sourcegitcommit: 64cfb79fd27b09d49df99e8c9c46792c884593a7
translationtype: HT
---
# <a name="download-the-cab-file-for-a-windows-10-driver-error"></a>下載適用於 Windows 10 驅動程式錯誤的 CAB 檔案

在 Windows 市集分析 API 中使用此方法，以下載與特定 Windows 10 驅動程式錯誤相關聯的 CAB 檔案。 使用此方法之前，您必須先使用[取得 Windows 10 驅動程式錯誤的詳細資料](get-details-for-a-windows-10-driver-error.md)方法，擷取要下載之 CAB 檔案的識別碼。

您可以在 Windows 市集分析 API 中使用[取得 Windows 10 驅動程式的錯誤報告資料](get-error-reporting-data-for-windows-10-drivers.md)和[取得 Windows 10 驅動程式錯誤的詳細資料](get-details-for-a-windows-10-driver-error.md)方法，取得 OEM 硬體錯誤的詳細資訊。

> [!NOTE]
> 此方法僅供隸屬於 [Windows 硬體開發人員中心計畫](https://msdn.microsoft.com/windows/hardware/drivers/dashboard/get-started-with-the-hardware-dashboard)的開發人員帳戶使用。

## <a name="prerequisites"></a>必要條件

若要使用這個方法，您必須先進行下列動作：

* 如果您尚未這樣做，請先完成 Windows 市集分析 API 的所有[先決條件](access-analytics-data-using-windows-store-services.md#prerequisites)。
* [取得 Azure AD 存取權杖](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token)以便用於這個方法的要求標頭。 在您取得存取權杖之後，您在權杖到期之前有 60 分鐘的時間可以使用權杖。 權杖到期之後，您可以取得新的權杖。
* 取得要下載之 CAB 檔案的識別碼。 若要取得此識別碼，請使用[取得 Windows 10 驅動程式錯誤的詳細資料](get-details-for-a-windows-10-driver-error.md)方法，以擷取特定驅動程式錯誤的詳細資料，並並使用該方法回應主體中的 **cabIdHash** 值。

## <a name="request"></a>要求


### <a name="request-syntax"></a>要求的語法

| 方法 | 要求 URI                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/driver/cabdownload``` |

<span/> 

### <a name="request-header"></a>要求的標頭

| 標頭        | 類型   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | 字串 | 必要。 Azure AD 存取權杖，形式為 **Bearer** &lt;*token*&gt;。 |

<span/> 

### <a name="request-parameters"></a>要求參數

| 參數        | 類型   |  描述      |  必要  |
|---------------|--------|---------------|------|
| cabIdHash | 字串 | 要下載之 CAB 檔案的唯一識別碼。 若要取得此識別碼，請使用[取得 Windows 10 驅動程式錯誤的詳細資料](get-details-for-a-windows-10-driver-error.md)方法，以擷取 App 中特定錯誤的詳細資料，並並使用該方法回應主體中的 **cabIdHash** 值。 |  是  |

<span/>
 
### <a name="request-example"></a>要求範例

下列範例示範如何使用此方法下載 CAB 檔案。

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/driver/cabdownload?cabIdHash=c1a51104-d682-4230-be14-7278b18e3555 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>回應

這個方法傳回 302（重新導向）回應碼，而且回應中的 **Location** 標頭會指派給 CAB 檔案的共用存取簽章 (SAS) URI。 呼叫端會重新導向至此 URI，以自動下載 CAB 檔案。

## <a name="related-topics"></a>相關主題

* [取得 Windows 10 驅動程式的錯誤報告資料](get-error-reporting-data-for-windows-10-drivers.md)
* [取得 Windows 10 驅動程式錯誤的詳細資料](get-details-for-a-windows-10-driver-error.md)
