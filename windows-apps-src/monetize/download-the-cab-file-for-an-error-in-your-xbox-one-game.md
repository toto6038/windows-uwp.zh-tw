---
description: 在 Microsoft Store 分析 API 中使用這個方法，以下載您的 Xbox One 遊戲中錯誤的 CAB 檔案。
title: 下載您的 Xbox One 遊戲中錯誤的 CAB 檔案
ms.date: 11/06/2018
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 分析 API, 下載 CAB
ms.localizationpriority: medium
ms.openlocfilehash: 736219533a254e6380c10600e97f707f15e37de6
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/03/2018
ms.locfileid: "8457871"
---
# <a name="download-the-cab-file-for-an-error-in-your-xbox-one-game"></a>下載您的 Xbox One 遊戲中錯誤的 CAB 檔案

在 Microsoft Store 分析 API 中使用這個方法，下載 CAB 檔案的已內嵌透過 Xbox 開發人員入口網站 (XDP) 在 Xbox One 遊戲中特定錯誤相關聯，並可用於 XDP 分析合作夥伴中心儀表板。 這個方法只可以下載最近 30 天發生錯誤的 CAB 檔案。

您可以使用此方法之前，您必須先使用[取得您的 Xbox One 遊戲中錯誤的詳細資料](get-details-for-an-error-in-your-xbox-one-game.md)的方法，擷取要下載之 CAB 檔案的識別碼。

## <a name="prerequisites"></a>必要條件


若要使用這個方法，您必須先進行下列動作：

* 如果您尚未這樣做，請先完成 Microsoft Store 分析 API 的所有[先決條件](access-analytics-data-using-windows-store-services.md#prerequisites)。
* [取得 Azure AD 存取權杖](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token)以便用於這個方法的要求標頭。 在您取得存取權杖之後，您在權杖到期之前有 60 分鐘的時間可以使用權杖。 權杖到期之後，您可以取得新的權杖。
* 取得要下載之 CAB 檔案的識別碼。 若要取得此識別碼，使用[取得遊戲的 Xbox One 中錯誤的詳細資料](get-details-for-an-error-in-your-xbox-one-game.md)的方法來擷取您的應用程式中特定錯誤的詳細資料並使用該方法回應主體中的**cabId**值。

## <a name="request"></a>要求


### <a name="request-syntax"></a>要求的語法

| 方法 | 要求 URI                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/xbox/cabdownload``` |


### <a name="request-header"></a>要求的標頭

| 標頭        | 類型   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 授權 | 字串 | 必要。 Azure AD 存取權杖，形式為 **Bearer** &lt;*token*&gt;。 |


### <a name="request-parameters"></a>要求參數

| 參數        | 類型   |  描述      |  必要  |
|---------------|--------|---------------|------|
| applicationId | string | 您下載之 CAB 檔案的 Xbox One 遊戲的產品識別碼。 要取得遊戲的產品識別碼，請在 Xbox 開發人員入口網站 (XDP) 中巡覽遊戲並從網址中擷取產品識別碼。 或者，如果您從 Windows 合作夥伴中心分析報告下載您的健康情況資料，產品識別碼包含在.tsv 檔案中。 |  是  |
| cabId | 字串 | 要下載之 CAB 檔案的唯一識別碼。 若要取得此識別碼，使用[取得遊戲的 Xbox One 中錯誤的詳細資料](get-details-for-an-error-in-your-xbox-one-game.md)的方法來擷取您的應用程式中特定錯誤的詳細資料並使用該方法回應主體中的**cabId**值。 |  是  |

 
### <a name="request-example"></a>要求範例

下列範例示範如何使用此方法下載 CAB 檔案。 將 *applicationId* 和 *cabId* 參數以應用程式的適當值取代。

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/xbox/cabdownload?applicationId=BRRT4NJ9B3D1&cabId=1336373323853 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>回應

這個方法傳回 302（重新導向）回應碼，而且回應中的 **Location** 標頭會指派給 CAB 檔案的共用存取簽章 (SAS) URI。 呼叫端會重新導向至此 URI，以自動下載 CAB 檔案。

## <a name="related-topics"></a>相關主題

* [使用 Microsoft Store 服務存取分析資料](access-analytics-data-using-windows-store-services.md)
* [取得錯誤報告資料 Xbox One 遊戲](get-error-reporting-data-for-your-xbox-one-game.md)
* [取得您的 Xbox One 中錯誤的詳細資料遊戲](get-details-for-an-error-in-your-xbox-one-game.md)
* [取得您的 Xbox One 中錯誤的堆疊追蹤遊戲](get-the-stack-trace-for-an-error-in-your-xbox-one-game.md)
