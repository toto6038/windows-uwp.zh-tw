---
ms.assetid: ''
description: 在 Microsoft Store 分析 API 中使用此方法，以下載適應用程式中錯誤的 CAB 檔案。
title: 下載應用程式中錯誤的 CAB 檔案
ms.date: 06/16/2017
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 分析 API, 下載 CAB
ms.localizationpriority: medium
ms.openlocfilehash: a4643f94236e62c46c12fd656ab5ddba5e1e0632
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57594533"
---
# <a name="download-the-cab-file-for-an-error-in-your-app"></a>下載應用程式中錯誤的 CAB 檔案

若要下載特定錯誤已經呈報給合作夥伴中心的應用程式中相關聯的 CAB 檔案，Microsoft Store analytics API 中使用這個方法。 這個方法只可以下載最近 30 天發生之應用程式錯誤的 CAB 檔案。 封包檔下載中也會有**失敗**一節[健康情況報告](../publish/health-report.md)在合作夥伴中心。

使用此方法之前，您必須先使用[取得應用程式中錯誤的詳細資料](get-details-for-an-error-in-your-app.md)方法，擷取要下載之 CAB 檔案的識別碼。

## <a name="prerequisites"></a>必要條件


若要使用這個方法，您必須先進行下列動作：

* 如果您尚未這樣做，請先完成 Microsoft Store 分析 API 的所有[先決條件](access-analytics-data-using-windows-store-services.md#prerequisites)。
* [取得 Azure AD 存取權杖](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token)以便用於這個方法的要求標頭。 在您取得存取權杖之後，您在權杖到期之前有 60 分鐘的時間可以使用權杖。 權杖到期之後，您可以取得新的權杖。
* 取得要下載之 CAB 檔案的識別碼。 若要取得此識別碼，請使用[取得 App 中錯誤的詳細資料](get-details-for-an-error-in-your-app.md)方法以擷取您的 App 中特定錯誤的詳細資料，並在該方法的回應主體中使用 **cabId** 值。

## <a name="request"></a>要求


### <a name="request-syntax"></a>要求的語法

| 方法 | 要求 URI                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/cabdownload``` |


### <a name="request-header"></a>要求的標頭

| 標頭        | 類型   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | 字串 | 必要。 在表單中的 Azure AD 存取權杖**持有人** &lt;*語彙基元*&gt;。 |


### <a name="request-parameters"></a>要求參數

| 參數        | 類型   |  描述      |  必要  |
|---------------|--------|---------------|------|
| applicationId | 字串 | 您想要下載 CAB 檔案之應用程式的 Store 識別碼。 存放區識別碼位於[應用程式識別碼 頁面上](../publish/view-app-identity-details.md)的合作夥伴中心。 舉例來說，市集識別碼可以是「9WZDNCRFJ3Q8」。 |  是  |
| cabId | 字串 | 要下載之 CAB 檔案的唯一識別碼。 若要取得此識別碼，請使用[取得 App 中錯誤的詳細資料](get-details-for-an-error-in-your-app.md)方法以擷取您的 App 中特定錯誤的詳細資料，並在該方法的回應主體中使用 **cabId** 值。 |  是  |

 
### <a name="request-example"></a>要求範例

下列範例示範如何使用此方法下載 CAB 檔案。 將 *applicationId* 和 *cabId* 參數以應用程式的適當值取代。

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/cabdownload?applicationId=9NBLGGGZ5QDR&cabId=1336373323853 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>回應

這個方法傳回 302（重新導向）回應碼，而且回應中的 **Location** 標頭會指派給 CAB 檔案的共用存取簽章 (SAS) URI。 呼叫端會重新導向至此 URI，以自動下載 CAB 檔案。

## <a name="related-topics"></a>相關主題

* [健康情況報告](../publish/health-report.md)
* [使用 Microsoft Store 服務的存取分析資料](access-analytics-data-using-windows-store-services.md)
* [取得錯誤報告資料](get-error-reporting-data.md)
* [取得您的應用程式中的錯誤詳細資料](get-details-for-an-error-in-your-app.md)
* [取得您的應用程式中發生錯誤的堆疊追蹤](get-the-stack-trace-for-an-error-in-your-app.md)
