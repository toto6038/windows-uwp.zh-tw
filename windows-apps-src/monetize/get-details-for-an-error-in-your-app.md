---
author: Xansky
ms.assetid: f0c0325e-ad61-4238-a096-c37802db3d3b
description: 在 Microsoft Store 分析 API 中使用此方法，以取得 App 特定錯誤的詳細資料。
title: 取得應用程式中錯誤的詳細資料
ms.author: mhopkins
ms.date: 06/05/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP, Microsoft Store 服務, Microsoft Store 分析 API, 錯誤, 詳細資料
ms.localizationpriority: medium
ms.openlocfilehash: ef523619d5c0be297223b34d480d2582f5605ec8
ms.sourcegitcommit: c4d3115348c8b54fcc92aae8e18fdabc3deb301d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/22/2018
ms.locfileid: "5400780"
---
# <a name="get-details-for-an-error-in-your-app"></a>取得應用程式中錯誤的詳細資料

在 Microsoft Store 分析 API 中使用此方法，以取得 App 特定錯誤的 JSON 格式詳細資料。 這個方法只能擷取最近 30 天內發生的錯誤的詳細資料。 「Windows 開發人員中心」儀表板中[健康情況報告](../publish/health-report.md)的 **\[失敗\]** 區段也提供詳細的錯誤資料。

使用這個方法之前，您必須先使用[取得錯誤報告資料](get-error-reporting-data.md)方法來擷取您要取得詳細資訊之錯誤的識別碼。

## <a name="prerequisites"></a>先決條件


若要使用這個方法，您必須先進行下列動作：

* 如果您尚未這樣做，請先完成 Microsoft Store 分析 API 的所有[先決條件](access-analytics-data-using-windows-store-services.md#prerequisites)。
* [取得 Azure AD 存取權杖](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token)以便用於這個方法的要求標頭。 在您取得存取權杖之後，您在權杖到期之前有 60 分鐘的時間可以使用權杖。 權杖到期之後，您可以取得新的權杖。
* 取得您想要取得詳細資訊之錯誤的識別碼。 若要取得此識別碼，請使用[取得錯誤報告資料](get-error-reporting-data.md)方法，並使用該方法回應內文中的 **failureHash** 值。

## <a name="request"></a>要求


### <a name="request-syntax"></a>要求的語法

| 方法 | 要求 URI                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/failuredetails``` |


### <a name="request-header"></a>要求的標頭

| 標頭        | 類型   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 授權 | 字串 | 必要。 Azure AD 存取權杖，形式為 **Bearer** &lt;*token*&gt;。 |


### <a name="request-parameters"></a>要求參數

| 參數        | 類型   |  說明      |  必要  
|---------------|--------|---------------|------|
| applicationId | 字串 | 您想要擷取詳細錯誤資料之 App 的 Store 識別碼。  Store 識別碼可在開發人員中心儀表板的 [App 身分識別頁面](../publish/view-app-identity-details.md)取得。 舉例來說，Store 識別碼可以是「9WZDNCRFJ3Q8」。 |  是  |
| failureHash | 字串 | 您想要取得詳細資訊之錯誤的唯一識別碼。 若要取得您有興趣之錯誤的此值，請使用[取得錯誤報告資料](get-error-reporting-data.md)方法，並在該方法的回應主體中使用 **failureHash** 值。 |  是  |
| startDate | 日期 | 要擷取詳細錯誤資料之日期範圍的開始日期。 預設為目前日期的前 30 天。<p/><p/>**注意：**&nbsp;&nbsp;這個方法只能擷取過去 30 天內發生的錯誤的詳細資料。 |  否  |
| endDate | 日期 | 要擷取詳細錯誤資料之日期範圍的結束日期。 預設為目前的日期。 |  否  |
| top | 整數 | 在要求中傳回的資料列數目。 最大值及未指定的預設值為 10000。 如果查詢中有更多資料列，回應主體將會包含您可以用來要求下一頁資料的下一頁連結。 |  否  |
| skip | 整數 | 在查詢中要略過的資料列數目。 使用此參數來瀏覽大型資料集。 例如，top=10 且 skip=0 將擷取前 10 個資料列的資料，top=10 且 skip=10 將擷取下 10 個資料列的資料，以此類推。 |  否  |
| filter |字串  | 在回應中篩選資料列的一或多個陳述式。 每個陳述式包含一個與 **eq** 或 **ne** 運算子關聯的欄位名稱 (來自回應主體) 和值，而陳述式可以使用 **and** 或 **or** 結合。 *filter* 參數中的字串值必須由單引號括住。 您可以指定來自回應主體的下列欄位：<p/><ul><li><strong>market</strong></li><li><strong>date</strong></li><li><strong>cabId</strong></li><li><strong>cabExpirationTime</strong></li><li><strong>deviceType</strong></li><li><strong>deviceModel</strong></li><li><strong>osVersion</strong></li><li><strong>osRelease</strong></li><li><strong>packageVersion</strong></li><li><strong>osBuild</strong></li></ul> | 不可以   |
| orderby | 字串 | 將結果資料值排序的陳述式。 語法為 <em>orderby=field [order],field [order],...</em>，其中 <em>field</em> 參數可以是下列其中一個字串︰<ul><li><strong>market</strong></li><li><strong>date</strong></li><li><strong>cabId</strong></li><li><strong>cabExpirationTime</strong></li><li><strong>deviceType</strong></li><li><strong>deviceModel</strong></li><li><strong>osVersion</strong></li><li><strong>osRelease</strong></li><li><strong>packageVersion</strong></li><li><strong>osBuild</strong></li></ul><p><em>order</em> 參數為選擇性，並可以是 <strong>asc</strong> 或 <strong>desc</strong>，以指定每個欄位的遞增或遞減順序。 預設為 <strong>asc</strong>。</p><p>下列為 <em>orderby</em> 字串的範例：<em>orderby=date,market</em></p> |  否  |


### <a name="request-example"></a>要求範例

下列範例示範取得詳細錯誤資料的數個要求。 將 *applicationId* 值取代為您 App 的 Store 識別碼。

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/failuredetails?applicationId=9NBLGGGZ5QDR&failureHash=012e33e3-dbc9-b12f-c124-9d9810f05d8b&startDate=2016-11-05&endDate=2016-11-06&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/failuredetails?applicationId=9NBLGGGZ5QDR&failureHash=012e33e3-dbc9-b12f-c124-9d9810f05d8b&startDate=2016-11-05&endDate=2016-11-06&top=10&skip=0&filter=market eq 'US' and deviceType eq 'Windows.Desktop' HTTP/1.1
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
| applicationId   | 字串  | 您擷取的詳細錯誤資料之 App 的 Store 識別碼。      |
| failureHash     | 字串  | 錯誤的唯一識別碼。     |
| failureName     | 字串  | 失敗的名稱，由四個部分組成：一個或多個問題類別、例外狀況/錯誤檢查碼、失敗發生位置映像名稱，以及相關函式名稱。           |
| 日期            | 字串  | 錯誤資料之日期範圍中的第一個日期。 如果要求指定單一天數，此值便會是該日期。 如果要求指定一週、一個月或其他日期範圍，此值便會是該日期範圍的第一個日期。 |
| cabId           | 字串  | 與此錯誤關聯之 CAB 檔案的唯一識別碼。   |
| cabExpirationTime  | 字串  | CAB 檔案到期且無法再下載的日期和時間 (ISO 8601 格式)。   |
| market          | 字串  | 裝置市場的 ISO 3166 國家/地區碼。     |
| osBuild         | 字串  | 發生錯誤之 OS 的組建編號。       |
| packageVersion  | 字串  | 與此錯誤關聯之應用程式套件的版本。    |
| deviceModel           | 字串  | 此字串指定錯誤發生時，App 正在執行的裝置機型。   |
| osVersion       | 字串  | 下列其中一個字串，指出發生錯誤所在的作業系統版本：<ul><li><strong>Windows Phone 7.5</strong></li><li><strong>Windows Phone 8</strong></li><li><strong>Windows Phone 8.1</strong></li><li><strong>Windows Phone 10</strong></li><li><strong>Windows8</strong></li><li><strong>Windows8.1</strong></li><li><strong>Windows10</strong></li><li><strong>不明</strong></li></ul>    |
| osRelease       | 字串  |  下列其中一個字串指定的 OS 版本或正式發行前小眾測試頻道 (為 OS 版本內的次群族) 上發生的錯誤。<p/><p>適用於 Windows10：</p><ul><li><strong>版本 1507</strong></li><li><strong>版本 1511</strong></li><li><strong>版本 1607</strong></li><li><strong>版本 1703</strong></li><li><strong>版本 1709</strong></li><li><strong>版本 1803</strong></li><li><strong>發行預覽</strong></li><li><strong>測試人員 - 快</strong></li><li><strong>測試人員 - 慢</strong></li></ul><p/><p>若是 Windows Server 1709：</p><ul><li><strong>RTM</strong></li></ul><p>若是 Windows Server 2016：</p><ul><li><strong>版本 1607</strong></li></ul><p>適用於 Windows8.1：</p><ul><li><strong>Update 1</strong></li></ul><p>適用於 Windows7：</p><ul><li><strong>Service Pack 1</strong></li></ul><p>如果 OS 版本或正式發行前小眾測試通道不明，此欄位會有<strong>不明</strong>值。</p>    |
| deviceType      | 字串  | 下列其中一個字串指定錯誤發生時，App 正在執行的裝置類型︰<ul><li><strong>PC</strong></li><li><strong>Phone</strong></li><li><strong>Console</strong></li><li><strong>IoT</strong></li><li><strong>Holographic</strong></li><li><strong>不明</strong></li></ul>     |
| cabDownloadable           | 布林值  | 指示 CAB 檔案是否可供這個使用者下載。   |


### <a name="response-example"></a>回應範例

下列範例針對此要求示範範例 JSON 回應主體。

```json
{
  "Value": [
    {
      "applicationId": "9NBLGGGZ5QDR ",
      "failureHash": "012345-5dbc9-b12f-c124-9d9810f05d8b",
      "failureName": "STOWED_EXCEPTION_System.UriFormatException_exe!ContosoGame.GroupedItems+_ItemView_ItemClick_d__9.MoveNext",
      "date": "2018-02-05 09:11:25",
      "cabId": "133637331323",
      "cabExpirationTime": "2016-12-05 09:11:25",
      "market": "US",
      "osBuild": "10.0.10240",
      "packageVersion": "1.0.2.6",
      "deviceModel": "Contoso Computer",
      "osVersion": "Windows 10",
      "osRelease": "Version 1507",
      "deviceType": "PC",
      "cabDownloadable": false
    }
  ],
  "@nextLink": null,
  "TotalCount": 1
}
```

## <a name="related-topics"></a>相關主題

* [健康情況報告](../publish/health-report.md)
* [使用 Microsoft Store 服務存取分析資料](access-analytics-data-using-windows-store-services.md)
* [取得錯誤報告資料](get-error-reporting-data.md)
* [取得 App 中錯誤的堆疊追蹤](get-the-stack-trace-for-an-error-in-your-app.md)
* [下載應用程式中錯誤的 CAB 檔案](download-the-cab-file-for-an-error-in-your-app.md)
