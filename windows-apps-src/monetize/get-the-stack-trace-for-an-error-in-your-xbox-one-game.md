---
author: Xansky
description: 在 Microsoft Store 分析 API 中使用這個方法，以取得遊戲中您的 Xbox One 錯誤的堆疊追蹤。
title: 取得您的 Xbox One 中錯誤的堆疊追蹤遊戲
ms.author: mhopkins
ms.date: 11/06/2018
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store 服務, Microsoft Store 分析 API, 堆疊追蹤, 錯誤
ms.localizationpriority: medium
ms.openlocfilehash: 78e65ad78079762ea5aabb95ddcaf4ce508b89bc
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/16/2018
ms.locfileid: "7173541"
---
# <a name="get-the-stack-trace-for-an-error-in-your-xbox-one-game"></a>取得您的 Xbox One 中錯誤的堆疊追蹤遊戲

在 Microsoft Store 分析 API 來取得您的 Xbox One 中錯誤的堆疊追蹤遊戲是透過 Xbox 開發人員入口網站 (xdp)，並可用於 XDP 分析合作夥伴中心儀表板中使用此方法。 這個方法只可以下載最近 30 天發生之錯誤的堆疊追蹤。

您可以使用此方法之前，您必須先使用[取得您的 Xbox One 遊戲中錯誤的詳細資料](get-details-for-an-error-in-your-xbox-one-game.md)的方法來擷取與您想要擷取堆疊追蹤的錯誤相關聯之 CAB 檔案的識別碼。

## <a name="prerequisites"></a>必要條件


若要使用這個方法，您必須先進行下列動作：

* 如果您尚未這樣做，請先完成 Microsoft Store 分析 API 的所有[先決條件](access-analytics-data-using-windows-store-services.md#prerequisites)。
* [取得 Azure AD 存取權杖](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token)以便用於這個方法的要求標頭。 在您取得存取權杖之後，您在權杖到期之前有 60 分鐘的時間可以使用權杖。 權杖到期之後，您可以取得新的權杖。
* 取得與您想要擷取堆疊追蹤的錯誤相關聯之 CAB 檔案的識別碼。 若要取得此識別碼，使用[取得遊戲的 Xbox One 中錯誤的詳細資料](get-details-for-an-error-in-your-xbox-one-game.md)的方法來擷取您的應用程式中特定錯誤的詳細資料並使用該方法回應主體中的**cabId**值。

## <a name="request"></a>要求


### <a name="request-syntax"></a>要求的語法

| 方法 | 要求 URI                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/xbox/stacktrace``` |


### <a name="request-header"></a>要求的標頭

| 標頭        | 類型   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 授權 | 字串 | 必要。 Azure AD 存取權杖，形式為 **Bearer** &lt;*token*&gt;。 |


### <a name="request-parameters"></a>要求參數

| 參數        | 類型   |  描述      |  必要  |
|---------------|--------|---------------|------|
| applicationId | string | 您正在擷取堆疊追蹤 Xbox One 遊戲的產品識別碼。 要取得遊戲的產品識別碼，請在 Xbox 開發人員入口網站 (XDP) 中巡覽遊戲並從網址中擷取產品識別碼。 或者，如果您從 Windows 合作夥伴中心分析報告下載您的健康情況資料，產品識別碼包含在.tsv 檔案中。 |  是  |
| cabId | 字串 | 與您想要擷取堆疊追蹤的錯誤相關聯之 CAB 檔案的唯一識別碼。 若要取得此識別碼，使用[取得遊戲的 Xbox One 中錯誤的詳細資料](get-details-for-an-error-in-your-xbox-one-game.md)的方法來擷取您的應用程式中特定錯誤的詳細資料並使用該方法回應主體中的**cabId**值。 |  是  |

 
### <a name="request-example"></a>要求範例

下列範例示範如何針對 Xbox One 遊戲使用此方法取得堆疊追蹤。 *ApplicationId*值取代為您的遊戲的產品識別碼。

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/xbox/stacktrace?applicationId=BRRT4NJ9B3D1&cabId=1336373323853 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>回應


### <a name="response-body"></a>回應主體

| 值      | 類型    | 描述                  |
|------------|---------|--------------------------------|
| 值      | 陣列   | 物件的陣列，每個物件都包含一個堆疊追蹤資料框架。 如需有關每個物件中資料的詳細資訊，請參閱下方的[堆疊追蹤值](#stack-trace-values)一節。 |
| @nextLink  | 字串  | 如果還有其他資料頁面，此字串包含可以用來要求下一頁資料的 URI。 例如，如果要求的 **top** 參數被設定為 10，但是查詢卻有超過 10 個資料列的錯誤，就會傳回此值。 |
| TotalCount | 整數 | 查詢之資料結果的資料列總數。          |


### <a name="stack-trace-values"></a>堆疊追蹤值

*Value* 陣列中的元素包含下列值。

| 值           | 類型    | 描述      |
|-----------------|---------|----------------|
| level            | 字串  |  此元素在呼叫堆疊中所代表的框架數目。  |
| image   | 字串  |   可執行檔名稱或程式庫映像，包含在此堆疊框架中呼叫的函式。           |
| function | 字串  |  在此堆疊框架中呼叫的函式名稱。 這是只有當您的遊戲包含可執行檔或程式庫的符號時，才可使用。              |
| offset     | 字串  |  相對於函式開始之目前指示的位元組位移。      |


### <a name="response-example"></a>回應範例

下列範例針對此要求示範範例 JSON 回應主體。

```json
{
  "Value": [
    {
      "level": "0",
      "image": "Contoso.ContosoApp",
      "function": "Contoso.ContosoApp.MainPage.DoWork",
      "offset": "0x25C"
    }
    {
      "level": "1",
      "image": "Contoso.ContosoApp",
      "function": "Contoso.ContosoApp.MainPage.Initialize",
      "offset": "0x26"
    }
    {
      "level": "2",
      "image": "Contoso.ContosoApp",
      "function": "Contoso.ContosoApp.Start",
      "offset": "0x66"
    }
  ],
  "@nextLink": null,
  "TotalCount": 3
}

```

## <a name="related-topics"></a>相關主題

* [使用 Microsoft Store 服務存取分析資料](access-analytics-data-using-windows-store-services.md)
* [取得錯誤報告資料 Xbox One 遊戲](get-error-reporting-data-for-your-xbox-one-game.md)
* [取得您的 Xbox One 中錯誤的詳細資料遊戲](get-details-for-an-error-in-your-xbox-one-game.md)
* [下載您的 Xbox One 遊戲中錯誤的 CAB 檔案](download-the-cab-file-for-an-error-in-your-xbox-one-game.md)
