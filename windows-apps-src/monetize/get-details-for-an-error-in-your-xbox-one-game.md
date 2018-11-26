---
description: 在 Microsoft Store 分析 API 中使用這個方法，以取得詳細的資料的特定錯誤的 Xbox One 遊戲。
title: 取得您的 Xbox One 中錯誤的詳細資料遊戲
ms.date: 11/06/2018
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store 服務, Microsoft Store 分析 API, 錯誤, 詳細資料
ms.localizationpriority: medium
ms.openlocfilehash: 6b713e3c6c2f7b82e5779e4785cc6b2e320b24f0
ms.sourcegitcommit: 681c70f964210ab49ac5d06357ae96505bb78741
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/26/2018
ms.locfileid: "7712602"
---
# <a name="get-details-for-an-error-in-your-xbox-one-game"></a>取得您的 Xbox One 中錯誤的詳細資料遊戲

在 Microsoft Store 分析 API 以取得詳細的資料的特定錯誤的 Xbox One 遊戲是透過 Xbox 開發人員入口網站 (xdp)，並可用於 XDP 分析合作夥伴中心儀表板中使用此方法。 這個方法只能擷取最近 30 天內發生的錯誤的詳細資料。

您可以使用此方法之前，您必須先使用[取得錯誤報告資料為您的 Xbox One 遊戲](get-error-reporting-data-for-your-xbox-one-game.md)的方法來擷取您要取得詳細的資訊之錯誤的識別碼。

## <a name="prerequisites"></a>必要條件


若要使用這個方法，您必須先進行下列動作：

* 如果您尚未這樣做，請先完成 Microsoft Store 分析 API 的所有[先決條件](access-analytics-data-using-windows-store-services.md#prerequisites)。
* [取得 Azure AD 存取權杖](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token)以便用於這個方法的要求標頭。 在您取得存取權杖之後，您在權杖到期之前有 60 分鐘的時間可以使用權杖。 權杖到期之後，您可以取得新的權杖。
* 取得您想要取得詳細資訊之錯誤的識別碼。 若要取得此識別碼，使用[取得錯誤報告資料，Xbox one 遊戲](get-error-reporting-data-for-your-xbox-one-game.md)的方法，並使用該方法回應主體中的**failureHash**值。

## <a name="request"></a>要求


### <a name="request-syntax"></a>要求的語法

| 方法 | 要求 URI                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/xbox/failuredetails``` |


### <a name="request-header"></a>要求的標頭

| 標頭        | 類型   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 授權 | 字串 | 必要。 Azure AD 存取權杖，形式為 **Bearer** &lt;*token*&gt;。 |


### <a name="request-parameters"></a>要求參數

| 參數        | 類型   |  描述      |  必要  
|---------------|--------|---------------|------|
| applicationId | string | 您正在擷取錯誤詳細資料 Xbox One 遊戲的產品識別碼。 要取得遊戲的產品識別碼，請在 Xbox 開發人員入口網站 (XDP) 中巡覽遊戲並從網址中擷取產品識別碼。 或者，如果您從 Windows 合作夥伴中心分析報告下載您的健康情況資料，產品識別碼包含在.tsv 檔案中。 |  是  |
| failureHash | 字串 | 您想要取得詳細資訊之錯誤的唯一識別碼。 若要取得您感興趣之錯誤的此值，使用[取得錯誤報告資料，Xbox one 遊戲](get-error-reporting-data-for-your-xbox-one-game.md)的方法，並使用該方法回應主體中的**failureHash**值。 |  是  |
| startDate | 日期 | 要擷取詳細錯誤資料之日期範圍的開始日期。 預設為目前日期的前 30 天。 |  否  |
| endDate | 日期 | 要擷取詳細錯誤資料之日期範圍的結束日期。 預設為目前的日期。 |  否  |
| top | 整數 | 在要求中傳回的資料列數目。 最大值及未指定的預設值為 10000。 如果查詢中有更多資料列，回應主體將會包含您可以用來要求下一頁資料的下一頁連結。 |  否  |
| skip | 整數 | 在查詢中要略過的資料列數目。 使用此參數來瀏覽大型資料集。 例如，top=10 且 skip=0 將擷取前 10 個資料列的資料，top=10 且 skip=10 將擷取下 10 個資料列的資料，以此類推。 |  否  |
| filter |字串  | 在回應中篩選資料列的一或多個陳述式。 每個陳述式包含一個與 **eq** 或 **ne** 運算子關聯的欄位名稱 (來自回應主體) 和值，而陳述式可以使用 **and** 或 **or** 結合。 *filter* 參數中的字串值必須由單引號括住。 您可以指定來自回應主體的下列欄位：<p/><ul><li><strong>market</strong></li><li><strong>date</strong></li><li><strong>cabId</strong></li><li><strong>cabExpirationTime</strong></li><li><strong>deviceType</strong></li><li><strong>deviceModel</strong></li><li><strong>osVersion</strong></li><li><strong>osRelease</strong></li><li><strong>packageVersion</strong></li><li><strong>osBuild</strong></li></ul> | 不可以   |
| orderby | 字串 | 將結果資料值排序的陳述式。 語法為 <em>orderby=field [order],field [order],...</em>，其中 <em>field</em> 參數可以是下列其中一個字串︰<ul><li><strong>market</strong></li><li><strong>date</strong></li><li><strong>cabId</strong></li><li><strong>cabExpirationTime</strong></li><li><strong>deviceType</strong></li><li><strong>deviceModel</strong></li><li><strong>osVersion</strong></li><li><strong>osRelease</strong></li><li><strong>packageVersion</strong></li><li><strong>osBuild</strong></li></ul><p><em>order</em> 參數為選擇性，並可以是 <strong>asc</strong> 或 <strong>desc</strong>，以指定每個欄位的遞增或遞減順序。 預設為 <strong>asc</strong>。</p><p>下列為 <em>orderby</em> 字串的範例：<em>orderby=date,market</em></p> |  否  |


### <a name="request-example"></a>要求範例

下列範例示範取得詳細的錯誤資料的 Xbox One 遊戲的數個要求。 *ApplicationId*值取代為您的遊戲的產品識別碼。

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/xbox/failuredetails?applicationId=BRRT4NJ9B3D1&failureHash=012e33e3-dbc9-b12f-c124-9d9810f05d8b&startDate=2016-11-05&endDate=2016-11-06&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/xbox/failuredetails?applicationId=BRRT4NJ9B3D1&failureHash=012e33e3-dbc9-b12f-c124-9d9810f05d8b&startDate=2016-11-05&endDate=2016-11-06&top=10&skip=0&filter=market eq 'US' and deviceType eq 'Windows.Desktop' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>回應


### <a name="response-body"></a>回應主體

| 值      | 類型    | 描述    |
|------------|---------|------------|
| 值      | 陣列   | 包含詳細錯誤資料的物件陣列。 如需有關每個物件中資料的詳細資訊，請參閱下方的[錯誤詳細資料值](#error-detail-values)一節。          |
| @nextLink  | 字串  | 如果還有其他資料頁面，此字串包含可以用來要求下一頁資料的 URI。 例如，如果要求的 **top** 參數被設定為 10，但是查詢卻有超過 10 個資料列的錯誤，就會傳回此值。 |
| TotalCount | 整數 | 查詢之資料結果的資料列總數。        |


<span id="error-detail-values"/>

### <a name="error-detail-values"></a>錯誤詳細資料值

*Value* 陣列中的元素包含下列值。

| 值           | 類型    | 描述     |
|-----------------|---------|----------------------------|
| applicationId   | string  | 您已擷取詳細的錯誤資料之 Xbox One 遊戲的產品識別碼。      |
| failureHash     | 字串  | 錯誤的唯一識別碼。     |
| failureName     | 字串  | 失敗的名稱，由四個部分組成：一個或多個問題類別、例外狀況/錯誤檢查碼、失敗發生位置映像名稱，以及相關函式名稱。           |
| 日期            | 字串  | 錯誤資料之日期範圍中的第一個日期。 如果要求指定單一天數，此值便會是該日期。 如果要求指定一週、一個月或其他日期範圍，此值便會是該日期範圍的第一個日期。 |
| cabId           | 字串  | 與此錯誤關聯之 CAB 檔案的唯一識別碼。   |
| cabExpirationTime  | 字串  | CAB 檔案到期且無法再下載的日期和時間 (ISO 8601 格式)。   |
| market          | 字串  | 裝置市場的 ISO 3166 國家/地區碼。     |
| osBuild         | 字串  | 發生錯誤之 OS 的組建編號。       |
| packageVersion  | 字串  | 遊戲與此錯誤關聯之套件的版本。    |
| deviceModel           | 字串  | 其中一個下列字串指定錯誤發生時，遊戲已執行的 Xbox One 主機。<p/><ul><li><strong>Microsoft Xbox 其中一個</strong></li><li><strong>Microsoft Xbox One S</strong></li><li><strong>Microsoft Xbox One X</strong></li></ul>  |
| osVersion       | 字串  | 發生錯誤的 OS 版本。 這一律是**Windows 10**的值。    |
| osRelease       | 字串  |  其中一個下列字串，指定的 Windows 10 OS 版本或正式發行前小眾 （為 OS 版本內的次群族） 上發生的錯誤。<p/><ul><li><strong>版本 1507</strong></li><li><strong>版本 1511</strong></li><li><strong>版本 1607</strong></li><li><strong>版本 1703</strong></li><li><strong>版本 1709</strong></li><li><strong>版本 1803</strong></li><li><strong>發行預覽</strong></li><li><strong>測試人員 - 快</strong></li><li><strong>測試人員 - 慢</strong></li></ul><p>如果 OS 版本或正式發行前小眾測試通道不明，此欄位會有<strong>不明</strong>值。</p>    |
| deviceType      | 字串  | 發生錯誤的裝置類型。 這一律是**主控台**的值。     |
| cabDownloadable           | 布林值  | 指示 CAB 檔案是否可供這個使用者下載。   |


### <a name="response-example"></a>回應範例

下列範例針對此要求示範範例 JSON 回應主體。

```json
{
  "Value": [
    {
      "applicationId": "BRRT4NJ9B3D1",
      "failureHash": "012345-5dbc9-b12f-c124-9d9810f05d8b",
      "failureName": "STOWED_EXCEPTION_System.UriFormatException_exe!ContosoSports.GroupedItems+_ItemView_ItemClick_d__9.MoveNext",
      "date": "2018-02-05 09:11:25",
      "cabId": "133637331323",
      "cabExpirationTime": "2016-12-05 09:11:25",
      "market": "US",
      "osBuild": "10.0.17134",
      "packageVersion": "1.0.2.6",
      "deviceModel": "Microsoft-Xbox One",
      "osVersion": "Windows 10",
      "osRelease": "Version 1803",
      "deviceType": "Console",
      "cabDownloadable": false
    }
  ],
  "@nextLink": null,
  "TotalCount": 1
}
```

## <a name="related-topics"></a>相關主題

* [使用 Microsoft Store 服務存取分析資料](access-analytics-data-using-windows-store-services.md)
* [取得錯誤報告資料 Xbox One 遊戲](get-error-reporting-data-for-your-xbox-one-game.md)
* [取得您的 Xbox One 中錯誤的堆疊追蹤遊戲](get-the-stack-trace-for-an-error-in-your-xbox-one-game.md)
* [下載您的 Xbox One 遊戲中錯誤的 CAB 檔案](download-the-cab-file-for-an-error-in-your-xbox-one-game.md)
