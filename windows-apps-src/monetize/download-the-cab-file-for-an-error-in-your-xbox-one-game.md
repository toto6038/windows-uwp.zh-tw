---
description: 若要下載您的 Xbox One 遊戲中發生錯誤的 CAB 檔案，在 Microsoft Store analytics API 中使用這個方法。
title: 針對 Xbox One 遊戲中的錯誤下載 CAB 檔案
ms.date: 11/06/2018
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 分析 API, 下載 CAB
ms.localizationpriority: medium
ms.openlocfilehash: 736219533a254e6380c10600e97f707f15e37de6
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57604323"
---
# <a name="download-the-cab-file-for-an-error-in-your-xbox-one-game"></a>針對 Xbox One 遊戲中的錯誤下載 CAB 檔案

Microsoft Store analytics API 中使用這個方法，以下載封包檔，而且在 Xbox One 遊戲都已擷取透過 Xbox 開發人員入口網站 (XDP) 中的特定錯誤相關聯 XDP Analytics 夥伴中心儀表板中可用。 這個方法只可以下載過去的 30 天內發生的錯誤封包檔。

您可以使用這個方法之前，您必須先使用[取得詳細資料發生錯誤，在您的 Xbox One 遊戲](get-details-for-an-error-in-your-xbox-one-game.md)方法來擷取您想要下載的封包檔的識別碼。

## <a name="prerequisites"></a>必要條件


若要使用這個方法，您必須先進行下列動作：

* 如果您尚未這樣做，請先完成 Microsoft Store 分析 API 的所有[先決條件](access-analytics-data-using-windows-store-services.md#prerequisites)。
* [取得 Azure AD 存取權杖](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token)以便用於這個方法的要求標頭。 在您取得存取權杖之後，您在權杖到期之前有 60 分鐘的時間可以使用權杖。 權杖到期之後，您可以取得新的權杖。
* 取得要下載之 CAB 檔案的識別碼。 若要取得這個識別碼，請使用[取得詳細資料發生錯誤，在您的 Xbox One 遊戲](get-details-for-an-error-in-your-xbox-one-game.md)方法來擷取應用程式中的特定錯誤的詳細資料，並使用**cabId**該方法的回應主體中的值。

## <a name="request"></a>要求


### <a name="request-syntax"></a>要求的語法

| 方法 | 要求 URI                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/xbox/cabdownload``` |


### <a name="request-header"></a>要求的標頭

| 標頭        | 類型   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | 字串 | 必要。 在表單中的 Azure AD 存取權杖**持有人** &lt;*語彙基元*&gt;。 |


### <a name="request-parameters"></a>要求參數

| 參數        | 類型   |  描述      |  必要  |
|---------------|--------|---------------|------|
| applicationId | 字串 | Xbox One 遊戲，您要下載的封包檔的產品識別碼。 要取得遊戲的產品識別碼，請在 Xbox 開發人員入口網站 (XDP) 中巡覽遊戲並從網址中擷取產品識別碼。 或者，如果您下載健康狀態資料從 Windows 合作夥伴中心分析報表時，產品識別碼納入.tsv 檔案。 |  是  |
| cabId | 字串 | 要下載之 CAB 檔案的唯一識別碼。 若要取得這個識別碼，請使用[取得詳細資料發生錯誤，在您的 Xbox One 遊戲](get-details-for-an-error-in-your-xbox-one-game.md)方法來擷取應用程式中的特定錯誤的詳細資料，並使用**cabId**該方法的回應主體中的值。 |  是  |

 
### <a name="request-example"></a>要求範例

下列範例示範如何使用此方法下載 CAB 檔案。 將 *applicationId* 和 *cabId* 參數以應用程式的適當值取代。

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/xbox/cabdownload?applicationId=BRRT4NJ9B3D1&cabId=1336373323853 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>回應

這個方法傳回 302（重新導向）回應碼，而且回應中的 **Location** 標頭會指派給 CAB 檔案的共用存取簽章 (SAS) URI。 呼叫端會重新導向至此 URI，以自動下載 CAB 檔案。

## <a name="related-topics"></a>相關主題

* [使用 Microsoft Store 服務的存取分析資料](access-analytics-data-using-windows-store-services.md)
* [取得錯誤報告資料，為您的 Xbox One 遊戲](get-error-reporting-data-for-your-xbox-one-game.md)
* [取得您的 Xbox One 發生錯誤的詳細資料遊戲](get-details-for-an-error-in-your-xbox-one-game.md)
* [取得堆疊追蹤錯誤，在您的 Xbox One 遊戲](get-the-stack-trace-for-an-error-in-your-xbox-one-game.md)
