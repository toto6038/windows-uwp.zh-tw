---
description: 在 Microsoft Store 分析 API 中使用此方法，以針對特定日期範圍與其他選擇性篩選器，取得彙總錯誤報告資料。
title: 取得 Xbox One 遊戲的錯誤報告資料
ms.date: 11/06/2018
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store 服務, Microsoft Store 分析 API, 錯誤
ms.localizationpriority: medium
ms.openlocfilehash: 22dff391e787e1763cb730272ba9cea029758c99
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57662073"
---
# <a name="get-error-reporting-data-for-your-xbox-one-game"></a>取得 Xbox One 遊戲的錯誤報告資料

使用 Microsoft Store 分析 API，以便針對您的 Xbox One 遊戲取得彙總的錯誤報告資料中已擷取透過 Xbox 開發人員入口網站 (XDP)，而且可在 XDP Analytics 夥伴中心儀表板的這個方法。

您可以使用，以擷取其他錯誤資訊[取得詳細資料發生錯誤，在您的 Xbox One 遊戲](get-details-for-an-error-in-your-xbox-one-game.md)，[取得的堆疊追蹤錯誤，在您的 Xbox One 遊戲](get-the-stack-trace-for-an-error-in-your-xbox-one-game.md)，和[封包檔下載在您的 Xbox One 遊戲中的錯誤](download-the-cab-file-for-an-error-in-your-xbox-one-game.md)方法。

## <a name="prerequisites"></a>必要條件


若要使用這個方法，您必須先進行下列動作：

* 如果您尚未這樣做，請先完成 Microsoft Store 分析 API 的所有[先決條件](access-analytics-data-using-windows-store-services.md#prerequisites)。
* [取得 Azure AD 存取權杖](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token)以便用於這個方法的要求標頭。 在您取得存取權杖之後，您在權杖到期之前有 60 分鐘的時間可以使用權杖。 權杖到期之後，您可以取得新的權杖。

## <a name="request"></a>要求


### <a name="request-syntax"></a>要求的語法

| 方法 | 要求 URI                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/xbox/failurehits``` |


### <a name="request-header"></a>要求的標頭

| 標頭        | 類型   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | 字串 | 必要。 在表單中的 Azure AD 存取權杖**持有人** &lt;*語彙基元*&gt;。 |


### <a name="request-parameters"></a>要求參數

| 參數        | 類型   |  描述      |  必要  
|---------------|--------|---------------|------|
| applicationId | 字串 | **存放區識別碼**Xbox One 遊戲，您要為其擷取錯誤的報告資料。 **存放區識別碼**位於在合作夥伴中心內的 [應用程式身分識別] 頁面。 舉例**存放區識別碼**是 9WZDNCRFJ3Q8。 |  是  |
| startDate | date | 要擷取錯誤報告資料之日期範圍的開始日期。 預設為目前的日期。 如果 *aggregationLevel* 是**日**、**星期**或**月份**，則此參數應該指定格式為 ```mm/dd/yyyy``` 的日期。 如果 *aggregationLevel* 是**小時**，則此參數可以指定 ```mm/dd/yyyy``` 格式的日期，或 ```yyyy-mm-dd hh:mm:ss``` 格式的日期和時間。  |  否  |
| endDate | date | 要擷取錯誤報告資料之日期範圍的結束日期。 預設為目前的日期。 如果 *aggregationLevel* 是**日**、**星期**或**月份**，則此參數應該指定格式為 ```mm/dd/yyyy``` 的日期。 如果 *aggregationLevel* 是**小時**，則此參數可以指定 ```mm/dd/yyyy``` 格式的日期，或 ```yyyy-mm-dd hh:mm:ss``` 格式的日期和時間。 |  否  |
| top | 整數 | 要在要求中傳回的資料列數目。 最大值及未指定的預設值為 10000。 如果查詢中有更多資料列，回應主體將會包含您可以用來要求下一頁資料的下一頁連結。 |  否  |
| skip | 整數 | 在查詢中要略過的資料列數目。 使用此參數來循頁瀏覽大型資料集。 例如，top=10000 且 skip=0 將擷取前 10000 個資料列的資料，top=10000 且 skip=10000 將擷取下 10000 個資料列的資料，以此類推。 |  否  |
| filter |字串  | 一或多個篩選回應中資料列的陳述式。 每個陳述式包含一個與 **eq** 或 **ne** 運算子關聯的欄位名稱 (來自回應主體) 和值，而陳述式可以使用 **and** 或 **or** 結合。 *filter* 參數中的字串值必須由單引號括住。 您可以在回應本文中指定下列欄位：<p/><ul><li>**applicationName**</li><li>**failureName**</li><li>**failureHash**</li><li>**symbol**</li><li>**osVersion**</li><li>**osRelease**</li><li>**eventType**</li><li>**market**</li><li>**deviceType**</li><li>**packageName**</li><li>**packageVersion**</li><li>**date**</li></ul> | 否   |
| aggregationLevel | 字串 | 指定要擷取彙總資料的時間範圍。 可以是下列其中一個字串：**hour**、**day**、**week** 或 **month**。 如果沒有指定，則預設為 **day**。 如果您指定 **week** 或 **month**，*failureName* 和 *failureHash* 值將會被限制在 1000 個值區。<p/><p/>**注意：**&nbsp;&nbsp;如果指定 **hour**，您只能從先前的 72 小時擷取錯誤資料。 若要擷取超過 72 小時的錯誤資料，請指定**day**或其他彙總層級的其中之一。  | 否 |
| orderby | 字串 | 將結果資料值排序的陳述式。 語法為 *orderby=field [order],field [order],...*。*field* 參數可以是下列其中一個字串：<ul><li>**applicationName**</li><li>**failureName**</li><li>**failureHash**</li><li>**symbol**</li><li>**osVersion**</li><li>**osRelease**</li><li>**eventType**</li><li>**market**</li><li>**deviceType**</li><li>**packageName**</li><li>**packageVersion**</li><li>**date**</li></ul><p>*order* 參數為選擇性，並可以是 **asc** 或 **desc**，以指定每個欄位的遞增或遞減順序。 預設為 **asc**。</p><p>下列為 *orderby* 字串的範例：*orderby=date,market*</p> |  否  |
| groupby | 字串 | 將資料彙總僅套用至指定欄位的陳述式。 您可以指定下列欄位：<ul><li>**failureName**</li><li>**failureHash**</li><li>**symbol**</li><li>**osVersion**</li><li>**eventType**</li><li>**market**</li><li>**deviceType**</li><li>**packageName**</li><li>**packageVersion**</li></ul><p>傳回的資料列將包含 *groupby* 參數中指定的欄位，以及下列項目：</p><ul><li>**date**</li><li>**applicationId**</li><li>**applicationName**</li><li>**deviceCount**</li><li>**eventCount**</li></ul><p>*groupby* 參數可以搭配 *aggregationLevel* 參數使用。 例如：*&amp;groupby=failureName,market&amp;aggregationLevel=week*</p></p> |  否  |


### <a name="request-example"></a>要求範例

下列範例會示範幾個要求來取得 Xbox One 遊戲的錯誤報告資料。 取代*applicationId*值替換**存放區識別碼**為遊戲。

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/xbox/failurehits?applicationId=BRRT4NJ9B3D1&startDate=1/1/2015&endDate=2/1/2015&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/xbox/failurehits?applicationId=BRRT4NJ9B3D1&startDate=8/1/2015&endDate=8/31/2015&skip=0&filter=market eq 'US' and deviceType eq 'Console' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>回應


### <a name="response-body"></a>回應主體

| 值      | 類型    | 描述     |
|------------|---------|--------------|
| 值      | 陣列   | 內含彙總錯誤報告資料的物件陣列。 如需有關每個物件中資料的詳細資訊，請參閱下方的＜[錯誤數值](#error-values)＞一節。     |
| @nextLink  | 字串  | 如果還有其他資料頁面，此字串會包含可以用來要求下一頁資料的 URI。 例如，如果要求的 **top** 參數被設定為 10000，但是查詢卻有超過 10000 個資料列的錯誤，就會傳回此值。 |
| TotalCount | 整數 | 查詢之資料結果的資料列總數。     |


### <a name="error-values"></a>錯誤數值

*Value* 陣列中的元素包含下列值。

| 值           | 類型    | 描述        |
|-----------------|---------|---------------------|
| date            | 字串  | 錯誤資料之日期範圍中的第一個日期，格式為 ```yyyy-mm-dd```。 如果要求指定單一天數，此值便會是該日期。 如果要求指定一個較長的日期範圍，此值便會是該日期範圍的第一個日期。 指定的要求*aggregationLevel*的值**小時**，此值也包含時間值格式```hh:mm:ss```中發生錯誤的當地時區。  |
| applicationId   | 字串  | **存放區識別碼**的 Xbox One 遊戲，您想要擷取資料時發生錯誤。   |
| applicationName | 字串  | 遊戲的顯示名稱。   |
| failureName     | 字串  | 失敗的名稱，由四個部分組成：一個或多個問題類別、例外狀況/錯誤檢查碼、失敗發生位置映像名稱，以及相關函式名稱。  |
| failureHash     | 字串  | 錯誤的唯一識別碼。   |
| symbol          | 字串  | 指派給此錯誤的符號。 |
| osVersion       | 字串  | 發生錯誤的 OS 版本。 這一律是值**Windows 10**。  |
| osRelease       | 字串  |  其中一個下列指定的 Windows 10 作業系統版本或試驗的環形 （subpopulation 內的 OS 版本) 發生錯誤的字串。<p/><ul><li><strong>版本 1507</strong></li><li><strong>1511 版</strong></li><li><strong>版本 1607</strong></li><li><strong>版本 1703</strong></li><li><strong>版本 1709</strong></li><li><strong>1803 版</strong></li><li><strong>發行預覽</strong></li><li><strong>測試人員快速</strong></li><li><strong>緩慢的測試人員</strong></li></ul><p>如果 OS 版本或正式發行前小眾測試通道不明，此欄位會有<strong>不明</strong>值。</p>    |
| eventType       | 字串  | 下列其中一個字串：<ul><li>**crash**</li><li>**hang**</li><li>**記憶體失敗**</li></ul>      |
| market          | 字串  | 裝置市場的 ISO 3166 國家/地區碼。   |
| deviceType      | 字串  | 發生錯誤的裝置類型。 這一律是值**主控台**。    |
| packageName     | 字串  | 唯一名稱遊戲的封裝與此錯誤相關聯。      |
| packageVersion  | 字串  | 遊戲與此錯誤相關聯的封裝版本。   |
| deviceCount     | 整數 | 針對指定的彙總層級對應到此錯誤的唯一裝置數目。  |
| eventCount      | 整數 | 針對指定的彙總層級被歸類到此錯誤的事件數目。      |


### <a name="response-example"></a>回應範例

下列範例示範這個要求的一個範例 JSON 回應主體。

```json
{
  "Value": [
    {
      "date": "2017-03-09",
      "applicationId": "BRRT4NJ9B3D1",
      "applicationName": "Contoso Sports",
      "failureName": "APPLICATION_FAULT_8013150a_StoreWrapper.ni.DLL!70475e55",
      "failureHash": "5a6b2170-1661-ed47-24d7-230fed0077af",
      "symbol": "storewrapper_ni!70475e55",
      "osVersion": "Windows 10",
      "osRelease": "Version 1803",
      "eventType": "crash",
      "market": "US",
      "deviceType": "Console",
      "packageName": "",
      "packageVersion": "0.0.0.0",
      "deviceCount": 0.0,
      "eventCount": 1.0
    }
  ],
  "@nextLink": "failurehits?applicationId=BRRT4NJ9B3D1&aggregationLevel=week&startDate=2017/03/01&endDate=2016/02/01&top=1&skip=1",
  "TotalCount": 21
}

```

## <a name="related-topics"></a>相關主題

* [取得您的 Xbox One 發生錯誤的詳細資料遊戲](get-details-for-an-error-in-your-xbox-one-game.md)
* [取得堆疊追蹤錯誤，在您的 Xbox One 遊戲](get-the-stack-trace-for-an-error-in-your-xbox-one-game.md)
* [在您的 Xbox One 遊戲中的錯誤封包檔下載](download-the-cab-file-for-an-error-in-your-xbox-one-game.md)
* [使用 Microsoft Store 服務的存取分析資料](access-analytics-data-using-windows-store-services.md)
