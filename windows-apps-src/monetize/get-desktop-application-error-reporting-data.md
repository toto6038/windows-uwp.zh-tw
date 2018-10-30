---
author: Xansky
description: 在 Microsoft Store 分析 API 中使用此方法，以針對特定日期範圍與其他選擇性篩選器，取得傳統型應用程式的彙總錯誤報告資料。
title: 取得傳統型應用程式的錯誤報告資料
ms.author: mhopkins
ms.date: 09/04/2018
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store 服務, Microsoft Store 分析 API, 錯誤, 傳統型應用程式
ms.localizationpriority: medium
ms.openlocfilehash: fb48efb2a1792d1c6691dfd38d0a5e36faac6e0b
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2018
ms.locfileid: "5762959"
---
# <a name="get-error-reporting-data-for-your-desktop-application"></a>取得傳統型應用程式的錯誤報告資料

在 Microsoft Store 分析 API 使用此方法，取得傳統型應用程式之彙總錯誤報告資料，而您已將其加入到 [Windows 傳統型應用程式](https://msdn.microsoft.com/library/windows/desktop/mt826504)。 這個方法只能擷取過去 30 天內發生的錯誤。 「Windows 開發人員中心」儀表板中傳統型應用程式的[健康情況報告](https://msdn.microsoft.com/library/windows/desktop/mt826504)也提供此資訊。

## <a name="prerequisites"></a>必要條件

若要使用這個方法，您必須先進行下列動作：

* 如果您尚未這樣做，請先完成 Microsoft Store 分析 API 的所有[先決條件](access-analytics-data-using-windows-store-services.md#prerequisites)。
* [取得 Azure AD 存取權杖](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token)以便用於這個方法的要求標頭。 在您取得存取權杖之後，您在權杖到期之前有 60 分鐘的時間可以使用權杖。 權杖到期之後，您可以取得新的權杖。

## <a name="request"></a>要求


### <a name="request-syntax"></a>要求的語法

| 方法 | 要求 URI                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/failurehits``` |


### <a name="request-header"></a>要求的標頭

| 標頭        | 類型   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 授權 | 字串 | 必要。 Azure AD 存取權杖，形式為 **Bearer** &lt;*token*&gt;。 |


### <a name="request-parameters"></a>要求參數

| 參數        | 類型   |  描述      |  必要  
|---------------|--------|---------------|------|
| applicationId | 字串 | 您想要擷取其錯誤報告資料的傳統型應用程式的產品識別碼。 若要取得傳統型應用程式的產品識別碼，請開啟任何[傳統型應用程式的開發人員中心分析報告](https://msdn.microsoft.com/library/windows/desktop/mt826504) (例如**健康報告**)，並從 URL 擷取產品識別碼。 |  是  |
| startDate | 日期 | 要擷取錯誤報告資料之日期範圍的開始日期，格式為 ```mm/dd/yyyy```。 預設為目前的日期。<p/><p/>**注意：**&nbsp;&nbsp;這個方法只能擷取過去 30 天內發生的錯誤。  |  否  |
| endDate | 日期 | 要擷取錯誤報告資料之日期範圍的結束日期，格式為 ```mm/dd/yyyy```。 預設為目前的日期。   |  否  |
| top | 整數 | 在要求中傳回的資料列數目。 最大值及未指定的預設值為 10000。 如果查詢中有更多資料列，回應主體將會包含您可以用來要求下一頁資料的下一頁連結。 |  否  |
| skip | 整數 | 在查詢中要略過的資料列數目。 使用此參數來瀏覽大型資料集。 例如，top=10000 且 skip=0 將擷取前 10000 個資料列的資料，top=10000 且 skip=10000 將擷取下 10000 個資料列的資料，以此類推。 |  否  |
| filter |字串  | 在回應中篩選資料列的一或多個陳述式。 每個陳述式包含一個與 **eq** 或 **ne** 運算子關聯的欄位名稱 (來自回應主體) 和值，而陳述式可以使用 **and** 或 **or** 結合。 *filter* 參數中的字串值必須由單引號括住。 您可以指定來自回應主體的下列欄位：<p/><ul><li><strong>fileName</strong></li><li><strong>applicationVersion</strong></li><li><strong>failureName</strong></li><li><strong>failureHash</strong></li><li><strong>symbol</strong></li><li><strong>osVersion</strong></li><li><strong>osBuild</strong></li><li><strong>osRelease</strong></li><li><strong>eventType</strong></li><li><strong>market</strong></li><li><strong>deviceType</strong></li><li><strong>productName</strong></li><li><strong>date</strong></li></ul> | 不可以   |
| aggregationLevel | 字串 | 指定要擷取彙總資料的時間範圍。 可以是下列其中一個字串：**day**、**week** 或 **month**。 如果沒有指定，則預設為 **day**。 如果您指定 **week** 或 **month**，*failureName* 和 *failureHash* 值將會被限制在 1000 個值區。<p/>  | 否 |
| orderby | 字串 | 將結果資料值排序的陳述式。 語法為 *orderby=field [order],field [order],...*，其中 *field* 參數可以是下列其中一個字串︰<ul><li><strong>fileName</strong></li><li><strong>applicationVersion</strong></li><li><strong>failureName</strong></li><li><strong>failureHash</strong></li><li><strong>symbol</strong></li><li><strong>osVersion</strong></li><li><strong>osBuild</strong></li><li><strong>osRelease</strong></li><li><strong>eventType</strong></li><li><strong>market</strong></li><li><strong>deviceType</strong></li><li><strong>productName</strong></li><li><strong>date</strong></li></ul>*order* 參數為選擇性，並可以是 **asc** 或 **desc**，以指定每個欄位的遞增或遞減順序。 預設為 **asc**。</p><p>下列為 *orderby* 字串的範例：*orderby=date,market*</p> |  否  |
| groupby | 字串 | 將資料彙總僅套用至指定欄位的陳述式。 您可以指定下列欄位：<ul><li>**failureName**</li><li>**failureHash**</li><li>**symbol**</li><li>**osVersion**</li><li>**eventType**</li><li>**market**</li><li>**deviceType**</li></ul><p>傳回的資料列將包含 *groupby* 參數中指定的欄位，以及下列項目：</p><ul><li>**date**</li><li>**applicationId**</li><li>**applicationName**</li><li>**eventCount**</li></ul><p>*groupby* 參數可以搭配 *aggregationLevel* 參數使用。 例如：*&amp;groupby=failureName,market&amp;aggregationLevel=week*</p></p> |  否  |


### <a name="request-example"></a>要求範例

下列範例示範取得錯誤報告資料的數個要求。 將 *applicationId* 值取代為您傳統型應用程式的產品識別碼。

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/failurehits?applicationId=10238467886765136388&startDate=1/1/2018&endDate=2/1/2018&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/failurehits?applicationId=10238467886765136388&startDate=8/1/2017&endDate=8/31/2017&skip=0&filter=market eq 'US' and deviceType eq 'PC' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>回應


### <a name="response-body"></a>回應主體

| 值      | 類型    | 描述     |
|------------|---------|--------------|
| 值      | array   | 內含彙總錯誤報告資料的物件陣列。 如需有關每個物件中資料的詳細資訊，請參閱下方的[錯誤數值](#error-values)一節。     |
| @nextLink  | 字串  | 如果還有其他資料頁面，此字串包含可以用來要求下一頁資料的 URI。 例如，如果要求的 **top** 參數被設定為 10000，但是查詢卻有超過 10000 個資料列的錯誤，就會傳回此值。 |
| TotalCount | 整數 | 查詢之資料結果的資料列總數。     |


### <a name="error-values"></a>錯誤數值

*Value* 陣列中的元素包含下列值。

| 值           | 類型    | 描述        |
|-----------------|---------|---------------------|
| 日期            | 字串  | 錯誤資料之日期範圍中的第一個日期，格式為 ```yyyy-mm-dd```。 如果要求指定單一天數，此值便會是該日期。 如果要求指定一個較長的日期範圍，此值便會是該日期範圍的第一個日期。 對於指定 **hour** 的 *aggregationLevel* 值，這個值也包括 ```hh:mm:ss``` 格式的時間。  |
| applicationId   | 字串  | 您已擷取錯誤資料的傳統型應用程式的產品識別碼值。    |
| productName | 字串  | 從相關聯的可執行檔中繼資料衍生的傳統型應用程式的顯示名稱。   |
| appName | 字串  |  TBD  |
| fileName | 字串  | 傳統型應用程式的可執行檔名稱。   |
| failureName     | 字串  | 失敗的名稱，由四個部分組成：一個或多個問題類別、例外狀況/錯誤檢查碼、失敗發生位置映像名稱，以及相關函式名稱。  |
| failureHash     | 字串  | 錯誤的唯一識別碼。   |
| symbol          | 字串  | 指派給此錯誤的符號。 |
| osBuild       | 字串  | 發生錯誤之 OS 的四個部分組建號碼。  |
| osVersion       | 字串  | 下列其中一個字串，指定傳統型應用程式安裝所在的作業系統版本：<p/><ul><li><strong>Windows 7</strong></li><li><strong>Windows 8.1</strong></li><li><strong>Windows10</strong></li><li><strong>Windows Server 2016</strong></li><li><strong>Windows Server 1709</strong></li><li><strong>不明</strong></li></ul>   |   
| osRelease | 字串  | 下列其中一個字串指定的 OS 版本或正式發行前小眾測試頻道 (為 OS 版本內的次群族) 上發生的錯誤。<p/><p>適用於 Windows10：</p><ul><li><strong>版本 1507</strong></li><li><strong>版本 1511</strong></li><li><strong>版本 1607</strong></li><li><strong>版本 1703</strong></li><li><strong>版本 1709</strong></li><li><strong>版本 1803</strong></li><li><strong>發行預覽</strong></li><li><strong>測試人員 - 快</strong></li><li><strong>測試人員 - 慢</strong></li></ul><p/><p>若是 Windows Server 1709：</p><ul><li><strong>RTM</strong></li></ul><p>若是 Windows Server 2016：</p><ul><li><strong>版本 1607</strong></li></ul><p>適用於 Windows8.1：</p><ul><li><strong>Update 1</strong></li></ul><p>適用於 Windows7：</p><ul><li><strong>Service Pack 1</strong></li></ul><p>如果 OS 版本或正式發行前小眾測試通道不明，此欄位會有<strong>不明</strong>值。</p> |
| eventType       | 字串  | 其中一個下列字串，可指出錯誤事件的類型：<ul><li>**crash**</li><li>**hang**</li><li>**memory**</li><li>**jse**</li></ul>       |
| market          | 字串  | 裝置市場的 ISO 3166 國家/地區碼。   |
| deviceType      | 字串  | 下列其中一個字串，指定發生錯誤的裝置類型：<p/><ul><li><strong>電腦</strong></li><li><strong>伺服器</strong></li><li><strong>平板電腦</strong></li><li><strong>不明</strong></li></ul>    |
| applicationVersion     | 字串  |   發生錯誤之應用程式可執行的版本。    |
| eventCount      | 數目 | 針對指定的彙總層級被歸類到此錯誤的事件數目。      |


### <a name="response-example"></a>回應範例

下列範例針對此要求示範範例 JSON 回應主體。

```json
{
  "Value": [
    {
      "date": "2018-02-01",
      "applicationId": "10238467886765136388",
      "productName": "Contoso Demo",
      "appName": "Contoso Demo",
      "fileName": "contosodemo.exe",
      "failureName": "SVCHOSTGROUP_localservice_IN_PAGE_ERROR_c0000006_hardware_disk!Unknown",
      "failureHash": "11242ef3-ebd8-d525-838d-b5497b225695",
      "symbol": "hardware_disk!Unknown",
      "osBuild": "10.0.15063.850",
      "osVersion": "Windows 10",
      "osRelease": "Version 1703",
      "eventType": "crash",
      "market": "US",
      "deviceType": "PC",
      "applicationVersion": "2.2.2.0",
      "eventCount": 0.0012422360248447205
    }
  ],
  "@nextLink": "desktop/failurehits?applicationId=10238467886765136388&aggregationLevel=week&startDate=2018/02/01&endDate2018/02/08&top=1&skip=1",
  "TotalCount": 21
}

```

## <a name="related-topics"></a>相關主題

* [健康情況報告](../publish/health-report.md)
* [使用 Microsoft Store 服務存取分析資料](access-analytics-data-using-windows-store-services.md)
* [取得傳統型應用程式中錯誤的詳細資料](get-details-for-an-error-in-your-desktop-application.md)
* [取得傳統型應用程式中錯誤的堆疊追蹤](get-the-stack-trace-for-an-error-in-your-desktop-application.md)
* [下載傳統型應用程式中錯誤的 CAB 檔案](download-the-cab-file-for-an-error-in-your-desktop-application.md)
