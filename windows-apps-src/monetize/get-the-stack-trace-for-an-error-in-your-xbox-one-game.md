---
description: 若要取得的堆疊追蹤錯誤，在您的 Xbox One 遊戲，Microsoft Store analytics API 中使用這個方法。
title: 針對 Xbox One 遊戲中的錯誤取得堆疊追蹤
ms.date: 11/06/2018
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store 服務, Microsoft Store 分析 API, 堆疊追蹤, 錯誤
ms.localizationpriority: medium
ms.openlocfilehash: fd43305c54245c3281a0e840d3df4c5c87ff7ad8
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57658683"
---
# <a name="get-the-stack-trace-for-an-error-in-your-xbox-one-game"></a>針對 Xbox One 遊戲中的錯誤取得堆疊追蹤

在 Microsoft Store analytics API 取得堆疊追蹤錯誤，在您的 Xbox One 遊戲，已擷取透過 Xbox 開發人員入口網站 (XDP)，而且可在 XDP Analytics 夥伴中心儀表板中使用這個方法。 這個方法只可以下載最近 30 天發生之錯誤的堆疊追蹤。

您可以使用這個方法之前，您必須先使用[取得詳細資料發生錯誤，在您的 Xbox One 遊戲](get-details-for-an-error-in-your-xbox-one-game.md)方法來擷取與您要擷取的堆疊追蹤錯誤相關聯的 CAB 檔案的識別碼。

## <a name="prerequisites"></a>必要條件


若要使用這個方法，您必須先進行下列動作：

* 如果您尚未這樣做，請先完成 Microsoft Store 分析 API 的所有[先決條件](access-analytics-data-using-windows-store-services.md#prerequisites)。
* [取得 Azure AD 存取權杖](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token)以便用於這個方法的要求標頭。 在您取得存取權杖之後，您在權杖到期之前有 60 分鐘的時間可以使用權杖。 權杖到期之後，您可以取得新的權杖。
* 取得與您想要擷取堆疊追蹤的錯誤相關聯之 CAB 檔案的識別碼。 若要取得這個識別碼，請使用[取得詳細資料發生錯誤，在您的 Xbox One 遊戲](get-details-for-an-error-in-your-xbox-one-game.md)方法來擷取應用程式中的特定錯誤的詳細資料，並使用**cabId**該方法的回應主體中的值。

## <a name="request"></a>要求


### <a name="request-syntax"></a>要求的語法

| 方法 | 要求 URI                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/xbox/stacktrace``` |


### <a name="request-header"></a>要求的標頭

| 標頭        | 類型   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | 字串 | 必要。 在表單中的 Azure AD 存取權杖**持有人** &lt;*語彙基元*&gt;。 |


### <a name="request-parameters"></a>要求參數

| 參數        | 類型   |  描述      |  必要  |
|---------------|--------|---------------|------|
| applicationId | 字串 | Xbox One 遊戲，您要擷取的堆疊追蹤的產品識別碼。 要取得遊戲的產品識別碼，請在 Xbox 開發人員入口網站 (XDP) 中巡覽遊戲並從網址中擷取產品識別碼。 或者，如果您下載健康狀態資料從 Windows 合作夥伴中心分析報表時，產品識別碼納入.tsv 檔案。 |  是  |
| cabId | 字串 | 與您想要擷取堆疊追蹤的錯誤相關聯之 CAB 檔案的唯一識別碼。 若要取得這個識別碼，請使用[取得詳細資料發生錯誤，在您的 Xbox One 遊戲](get-details-for-an-error-in-your-xbox-one-game.md)方法來擷取應用程式中的特定錯誤的詳細資料，並使用**cabId**該方法的回應主體中的值。 |  是  |

 
### <a name="request-example"></a>要求範例

下列範例示範如何取得堆疊追蹤 Xbox One 遊戲使用此方法。 取代*applicationId*與您的遊戲的產品識別碼的值。

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/xbox/stacktrace?applicationId=BRRT4NJ9B3D1&cabId=1336373323853 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>回應


### <a name="response-body"></a>回應主體

| 值      | 類型    | 描述                  |
|------------|---------|--------------------------------|
| 值      | 陣列   | 物件的陣列，每個物件都包含一個堆疊追蹤資料框架。 如需有關每個物件中資料的詳細資訊，請參閱下方的[堆疊追蹤值](#stack-trace-values)一節。 |
| @nextLink  | 字串  | 如果還有其他資料頁面，此字串會包含可以用來要求下一頁資料的 URI。 例如，如果要求的 **top** 參數被設定為 10，但是查詢卻有超過 10 個資料列的錯誤，就會傳回此值。 |
| TotalCount | 整數 | 查詢之資料結果的資料列總數。          |


### <a name="stack-trace-values"></a>堆疊追蹤值

*Value* 陣列中的元素包含下列值。

| 值           | 類型    | 描述      |
|-----------------|---------|----------------|
| level            | 字串  |  此元素在呼叫堆疊中所代表的框架數目。  |
| image   | 字串  |   可執行檔名稱或程式庫映像，包含在此堆疊框架中呼叫的函式。           |
| function | 字串  |  在此堆疊框架中呼叫的函式名稱。 這是您的遊戲包含可執行檔或程式庫的符號時，才提供使用。              |
| offset     | 字串  |  相對於函式開始之目前指示的位元組位移。      |


### <a name="response-example"></a>回應範例

下列範例示範這個要求的一個範例 JSON 回應主體。

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

* [使用 Microsoft Store 服務的存取分析資料](access-analytics-data-using-windows-store-services.md)
* [取得錯誤報告資料，為您的 Xbox One 遊戲](get-error-reporting-data-for-your-xbox-one-game.md)
* [取得您的 Xbox One 發生錯誤的詳細資料遊戲](get-details-for-an-error-in-your-xbox-one-game.md)
* [在您的 Xbox One 遊戲中的錯誤封包檔下載](download-the-cab-file-for-an-error-in-your-xbox-one-game.md)
