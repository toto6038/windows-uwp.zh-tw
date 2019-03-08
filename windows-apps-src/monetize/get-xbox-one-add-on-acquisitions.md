---
description: 在 Microsoft Store analytics API 中使用這個方法，以取得彙總的附加元件取得資料。
title: 取得 Xbox One 附加元件下載數
ms.date: 10/18/2018
ms.topic: article
keywords: windows 10、 uwp、 存放區服務、 Microsoft Store 分析 API，Xbox One 的附加元件併購
ms.localizationpriority: medium
ms.openlocfilehash: f102d2d692a2307c25dcb95e66d612fc561dec70
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57633293"
---
# <a name="get-xbox-one-add-on-acquisitions"></a>取得 Xbox One 附加元件下載數

在 Microsoft Store analytics API 以取得彙總的附加元件取得資料以 JSON 格式的 Xbox One 遊戲，已擷取透過 Xbox 開發人員入口網站 (XDP)，而且可在 XDP Analytics 夥伴中心儀表板中使用這個方法。

## <a name="prerequisites"></a>必要條件

若要使用這個方法，您必須先進行下列動作：

* 如果您尚未這樣做，請先完成 Microsoft Store 分析 API 的所有[先決條件](access-analytics-data-using-windows-store-services.md#prerequisites)。
* [取得 Azure AD 存取權杖](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token)以便用於這個方法的要求標頭。 在您取得存取權杖之後，您在權杖到期之前有 60 分鐘的時間可以使用權杖。 權杖到期之後，您可以取得新的權杖。

## <a name="request"></a>要求


### <a name="request-syntax"></a>要求的語法

| 方法 | 要求 URI                                                                |
|--------|----------------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/xbox/addonacquisitions``` |


### <a name="request-header"></a>要求的標頭

| 標頭        | 類型   | 描述          |
|---------------|--------|--------------|
| Authorization | 字串 | 必要。 在表單中的 Azure AD 存取權杖**持有人** &lt;*語彙基元*&gt;。 |


### <a name="request-parameters"></a>要求參數

*ApplicationId*或是*addonProductId*是必要參數。 若要擷取所有註冊至 App 之附加元件的下載數資料，請指定 *applicationId* 參數。 若要擷取單一的附加元件的 取得資料，請指定*addonProductId*參數。 如果您同時指定上面兩個參數，*applicationId* 參數將會被忽略。

| 參數        | 類型   |  描述      |  必要  |
|---------------|--------|---------------|------|
| applicationId | 字串 | *ProductId*的 Xbox One 遊戲，您要為其擷取取得資料。 若要取得*productId*的遊戲，瀏覽至您的遊戲 XDP 分析程式，並擷取*productId*從 URL。 或者，如果您從合作夥伴中心分析報表中，下載您取得資料*productId*納入.tsv 檔案。 |  是  |
| addonProductId | 字串 | *ProductId*附加元件，您想要擷取取得資料。  | 是  |
| startDate | date | 要擷取附加元件下載數資料之日期範圍的開始日期。 預設為目前的日期。 |  否  |
| endDate | date | 要擷取附加元件下載數資料之日期範圍的結束日期。 預設為目前的日期。 |  否  |
| top | 整數 | 要在要求中傳回的資料列數目。 最大值及未指定的預設值為 10000。 如果查詢中有更多資料列，回應主體將會包含您可以用來要求下一頁資料的下一頁連結。 |  否  |
| skip | 整數 | 在查詢中要略過的資料列數目。 使用此參數來循頁瀏覽大型資料集。 例如，top=10000 且 skip=0 將擷取前 10000 個資料列的資料，top=10000 且 skip=10000 將擷取下 10000 個資料列的資料，以此類推。 |  否  |
| filter |字串  | <p>一或多個篩選回應中資料列的陳述式。 每個陳述式包含欄位名稱從回應主體和 eq 或 ne 運算子相關聯的值，而且陳述式可以使用結合和或。 必須以篩選參數中的單引號括住的字串值。 例如，篩選 = 市場 eq 是 「 美國 」 和性別 eq'。</p> <p>您可以在回應本文中指定下列欄位：</p> <ul><li><strong>acquisitionType</strong></li><li><strong>age</strong></li><li><strong>storeClient</strong></li><li><strong>gender</strong></li><li><strong>market</strong></li><li><strong>osVersion</strong></li><li><strong>deviceType</strong></li><li><strong>sandboxId</strong></li></ul>| 否   |
| aggregationLevel | 字串 | 指定要擷取彙總資料的時間範圍。 可以是下列其中一個字串：<strong>day</strong>、<strong>week</strong> 或 <strong>month</strong>。 如果沒有指定，則預設為 <strong>day</strong>。 | 否 |
| orderby | 字串 | 對每個附加元件下載數的結果資料值做出排序的陳述式。 語法是<em>orderby = field [order]，欄位 [order]，...</em><em>field</em> 參數可以是下列其中一個字串：<ul><li><strong>date</strong></li><li><strong>acquisitionType</strong></li><li><strong>age</strong></li><li><strong>storeClient</strong></li><li><strong>gender</strong></li><li><strong>market</strong></li><li><strong>osVersion</strong></li><li><strong>deviceType</strong></li><li><strong>orderName</strong></li></ul><p><em>order</em> 參數為選擇性，並可以是 <strong>asc</strong> 或 <strong>desc</strong>，以指定每個欄位的遞增或遞減順序。 預設為 <strong>asc</strong>。</p><p>下列為 <em>orderby</em> 字串的範例：<em>orderby=date,market</em></p> |  否  |
| groupby | 字串 | 將資料彙總僅套用至指定欄位的陳述式。 您可以指定下列欄位：<ul><li><strong>date</strong></li><li><strong>applicationName</strong></li><li><strong>addonProductName</strong></li><li><strong>acquisitionType</strong></li><li><strong>age</strong></li><li><strong>storeClient</strong></li><li><strong>gender</strong></li><li><strong>market</strong></li><li><strong>osVersion</strong></li><li><strong>deviceType</strong></li><li><strong>paymentInstrumentType</strong></li><li><strong>sandboxId</strong></li><li><strong>xboxTitleIdHex</strong></li></ul><p>傳回的資料列將包含 <em>groupby</em> 參數中指定的欄位，以及下列項目：</p><ul><li><strong>date</strong></li><li><strong>applicationId</strong></li><li><strong>addonProductId</strong></li><li><strong>acquisitionQuantity</strong></li></ul><p><em>groupby</em> 參數可以搭配 <em>aggregationLevel</em> 參數使用。 例如：  <em>&amp;groupby = 時代，市場&amp;aggregationLevel = 週</em></p> |  否  |


### <a name="request-example"></a>要求範例

下列範例示範數個取得附加元件下載數資料的要求。 取代*addonProductId*並*applicationId*附加元件或應用程式適當的存放區識別碼的值。

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/xbox/addonacquisitions?addonProductId=BRRT4NJ9B3D2&startDate=1/1/2015&endDate=2/1/2015&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/xbox/addonacquisitions?applicationId=BRRT4NJ9B3D1&startDate=1/1/2015&endDate=2/1/2015&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/xbox/addonacquisitions?addonProductId=BRRT4NJ9B3D2&startDate=1/1/2015&endDate=7/3/2015&top=100&skip=0&filter=market ne 'US' and gender ne 'Unknown' and gender ne 'm' and market ne 'NO' and age ne '>55' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>回應


### <a name="response-body"></a>回應主體

| 值      | 類型   | 描述         |
|------------|--------|------------------|
| 值      | 陣列  | 內含彙總附加元件下載數資料的物件陣列。 如需有關每個物件中資料的詳細資訊，請參閱下方的[＜附加元件下載數數值＞](#add-on-acquisition-values)一節。                                                                                                              |
| @nextLink  | 字串 | 如果還有其他資料頁面，此字串會包含可以用來要求下一頁資料的 URI。 例如，如果要求的 **top** 參數被設定為 10000，但是查詢卻有超過 10000 個資料列的附加元件下載數資料，就會傳回此值。 |
| TotalCount | 整數    | 查詢之資料結果的資料列總數。    |


<span id="add-on-acquisition-values" />

### <a name="add-on-acquisition-values"></a>附加元件下載數數值

*Value* 陣列中的元素包含下列值。

| 值               | 類型    | 描述        |
|---------------------|---------|---------------------|
| date                | 字串  | 下載數資料之日期範圍中的第一個日期。 如果要求指定單一天數，此值便會是該日期。 如果要求指定一週、一個月或其他日期範圍，此值便會是該日期範圍的第一個日期。 |
| addonProductId      | 字串  | *ProductId*附加元件，您要為其擷取取得資料。                                                                                                                                                                 |
| addonProductName    | 字串  | 附加元件的顯示名稱。 此值才會出現在回應資料*aggregationLevel*參數設為**天**，除非您指定**addonProductName**欄位*groupby*參數。                                                                                                                                                                                                            |
| applicationId       | 字串  | *ProductId*您要擷取附加元件取得資料的應用程式。                                                                                                                                                           |
| applicationName     | 字串  | 遊戲的顯示名稱。                                                                                                                                                                                                             |
| deviceType          | 字串  | <p>以下字串之一指定完成收購的裝置類型：</p> <ul><li>"PC"</li><li>「 電話 」</li><li>「 主控台 」</li><li>「 IoT"</li><li>「 伺服器 」</li><li>"Tablet"</li><li>「 全像攝影版 」</li><li>「 未知 」</li></ul>                                                                                                  |
| storeClient         | 字串  | <p>下列其中一個表示收購發生的 Microsoft Store 版本之字串：</p> <ul><li>「 Windows Phone 市集 （用戶端） 」</li><li>「 Microsoft Store （用戶端） 」 (或 「 Windows 市集 （用戶端） 」 如果 2018 年 3 月 23 日之前查詢資料)</li><li>「 Microsoft Store (web) 」 (或 「 Windows 市集 (web) 」 如果 2018 年 3 月 23 日之前查詢資料)</li><li>[大量購買的組織]</li><li>「 其他 」</li></ul>                                                                                            |
| osVersion           | 字串  | 發生下載的 OS 版本。 針對這個方法，這個值一律是"Windows 10"。                                                                                                   |
| market              | 字串  | 發生下載之市場的 ISO 3166 國家/地區碼。                                                                                                                                                                  |
| gender              | 字串  | <p>下列其中一個字串，這些字串詳列進行收購的使用者的性別：</p> <ul><li>"m"</li><li>"f"</li><li>「 未知 」</li></ul>                                                                                                    |
| 年齡            | 字串  | <p>下列其中一個字串，這些字串表示進行下載的使用者的年齡層：</p> <ul><li>「 小於 13 」</li><li>"13-17"</li><li>"18-24"</li><li>"25-34"</li><li>"35-44"</li><li>"44-55"</li><li>「 大於 55 」</li><li>「 未知 」</li></ul>                                                                                                 |
| acquisitionType     | 字串  | <p>其中一個下列字串，可指出收購類型：</p> <ul><li>「 免費 」</li><li>「 試用 」</li><li>"Paid"</li><li>「 促銷的程式碼 」</li><li>「 Iap"</li><li>「 訂用帳戶 Iap"</li><li>「 私用對象 」</li><li>"Pre Order"</li><li>「 Xbox Game Pass"（或者 「 遊戲傳遞 」 如果 2018 年 3 月 23 日之前查詢資料）</li><li>「 磁碟 」</li><li>「 預付的程式碼 」</li><li>「 計費前 Order"</li><li>「 取消前順序 」</li><li>「 失敗前順序 」</li></ul>                                                                                                    |
| acquisitionQuantity | 整數 | 發生的下載數目。                        |


### <a name="response-example"></a>回應範例

下列範例示範這個要求的一個範例 JSON 回應主體。

```json
{
  "Value": [
    {
      "date": "2018-10-18",
      "addonProductId": " BRRT4NJ9B3D2",
      "addonProductName": "Contoso add-on 7",
      "applicationId": "BRRT4NJ9B3D1",
      "applicationName": "Contoso Demo",
      "deviceType": "Console",
      "storeClient": "Windows Phone Store (client)",
      "osVersion": "Windows 10",
      "market": "GB",
      "gender": "m",
      "age": "50orover",
      "acquisitionType": "iap",
      "acquisitionQuantity": 1
    }
  ],
  "@nextLink": "addonacquisitions?applicationId=BRRT4NJ9B3D1&addonProductId=&aggregationLevel=day&startDate=2015/01/01&endDate=2016/02/01&top=1&skip=1",
  "TotalCount": 33677
}
```
