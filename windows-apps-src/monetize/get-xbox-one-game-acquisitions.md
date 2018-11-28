---
ms.assetid: C1E42E8B-B97D-4B09-9326-25E968680A0F
description: 在 Microsoft Store 分析 API 中使用此方法，以針對特定日期範圍與其他選擇性篩選器，取得 Xbox One 遊戲的彙總下載數資料。
title: 取得 Xbox One 遊戲下載數
ms.date: 10/18/2018
ms.topic: article
keywords: Windows 10, uwp, Microsoft Store 服務, Microsoft Store 分析 API, Xbox One 遊戲下載數
ms.localizationpriority: medium
ms.openlocfilehash: 348430f7ceee66a9c4e82f258a70e57d8f344943
ms.sourcegitcommit: b11f305dbf7649c4b68550b666487c77ea30d98f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/27/2018
ms.locfileid: "7837583"
---
# <a name="get-xbox-one-game-acquisitions"></a>取得 Xbox One 遊戲下載數

在 Microsoft Store 分析 API，取得彙總下載數資料的 JSON 格式的 Xbox One 遊戲是透過 Xbox 開發人員入口網站 (xdp)，並可用於 XDP 分析儀表板中使用此方法。

## <a name="prerequisites"></a>必要條件

若要使用這個方法，您必須先進行下列動作：

* 如果您尚未這樣做，請先完成 Microsoft Store 分析 API 的所有[先決條件](access-analytics-data-using-windows-store-services.md#prerequisites)。
* [取得 Azure AD 存取權杖](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token)以便用於這個方法的要求標頭。 在您取得存取權杖之後，您在權杖到期之前有 60 分鐘的時間可以使用權杖。 權杖到期之後，您可以取得新的權杖。

## <a name="request"></a>要求


### <a name="request-syntax"></a>要求的語法

| 方法 | 要求 URI       |
|--------|----------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/xbox/acquisitions``` |


### <a name="request-header"></a>要求的標頭

| 標頭        | 類型   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 授權 | 字串 | 必要。 Azure AD 存取權杖，形式為 **Bearer** &lt;*token*&gt;。 |


### <a name="request-parameters"></a>要求參數

| 參數        | 類型   |  描述      |  必要  
|---------------|--------|---------------|------|
| applicationId | 字串 | 您正在擷取下載數資料之 Xbox One 遊戲的產品識別碼。 若要取得您的遊戲的產品識別碼，瀏覽至您的遊戲在 XDP 分析程式，並從 URL 擷取產品識別碼。 或者，如果您從合作夥伴中心分析報告下載您的下載數資料，產品識別碼包含在.tsv 檔案中。  |  是  |
| startDate | 日期 | 要擷取下載數資料之日期範圍的開始日期。 預設為目前的日期。 |  否  |
| endDate | 日期 | 要擷取下載數資料之日期範圍的結束日期。 預設為目前的日期。 |  否  |
| top | 整數 | 要傳回的資料列數目。 如果未指定，最大值和預設值為 10000。 如果查詢中有更多資料列，回應主體將會包含您可以用來要求下一頁資料的下一頁連結。 |  否  |
| skip | 整數 | 在查詢中要略過的資料列數目。 使用此參數來瀏覽大型資料集。 例如，top=10000 且 skip=0 將擷取前 10000 個資料列的資料，top=10000 且 skip=10000 將擷取下 10000 個資料列的資料，以此類推。 |  否  |
| filter | 字串  | 在回應中篩選資料列的一或多個陳述式。 每個陳述式包含一個與 **eq** 或 **ne** 運算子關聯的欄位名稱 (來自回應主體) 和值，而陳述式可以使用 **and** 或 **or** 結合。 *filter* 參數中的字串值必須由單引號括住。 例如，*filter=market eq 'US' and gender eq 'm'*。 <p/><p/>您可以指定來自回應主體的下列欄位：<p/><ul><li><strong>acquisitionType</strong></li><li><strong>年齡</strong></li><li><strong>storeClient</strong></li><li><strong>gender</strong></li><li><strong>market</strong></li><li><strong>osVersion</strong></li><li><strong>deviceType</strong></li><li><strong>sandboxId</strong></li></ul> | 否   |
| aggregationLevel | 字串 | 指定要擷取彙總資料的時間範圍。 可以是下列其中一個字串：<strong>day</strong>、<strong>week</strong> 或 <strong>month</strong>。 如果沒有指定，則預設為 <strong>day</strong>。 | 否 |
| orderby | 字串 | 對每個下載數的結果資料值做出排序的陳述式。 語法為 <em>orderby=field [order],field [order],...</em>，其中 <em>field</em> 參數可以是下列其中一個字串︰<ul><li><strong>日期</strong></li><li><strong>acquisitionType</strong></li><li><strong>年齡</strong></li><li><strong>storeClient</strong></li><li><strong>gender</strong></li><li><strong>market</strong></li><li><strong>osVersion</strong></li><li><strong>deviceType</strong></li><li><strong>paymentInstrumentType</strong></li><li><strong>sandboxId</strong></li><li><strong>xboxTitleIdHex</strong></li></ul><p><em>order</em> 參數為選擇性，並可以是 <strong>asc</strong> 或 <strong>desc</strong>，用來指定每個欄位的遞增或遞減順序。 預設為 <strong>asc</strong>。</p><p>下列為 <em>orderby</em> 字串的範例：<em>orderby=date,market</em></p> |  否  |
| groupby | 字串 | 將資料彙總僅套用至指定欄位的陳述式。 您可以指定下列欄位：<ul><li><strong>date</strong></li><li><strong>applicationName</strong></li><li><strong>acquisitionType</strong></li><li><strong>ageGroup</strong></li><li><strong>storeClient</strong></li><li><strong>gender</strong></li><li><strong>market</strong></li><li><strong>osVersion</strong></li><li><strong>deviceType</strong></li><li><strong>paymentInstrumentType</strong></li><li><strong>sandboxId</strong></li><li><strong>xboxTitleIdHex</strong></li></ul><p>傳回的資料列將包含 <em>groupby</em> 參數中指定的欄位，以及下列項目：</p><ul><li><strong>date</strong></li><li><strong>applicationId</strong></li><li><strong>acquisitionQuantity</strong></li></ul><p><em>groupby</em> 參數可以搭配 <em>aggregationLevel</em> 參數使用。 例如：<em>&amp;groupby=ageGroup,market&amp;aggregationLevel=week</em></p> |  否  |


### <a name="request-example"></a>要求範例

下列範例示範數個取得 Xbox One 遊戲範下載數資料的要求。 *ApplicationId*值取代為您的遊戲的產品識別碼。

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/xbox/acquisitions?applicationId=BRRT4NJ9B3D1&startDate=1/1/2017&endDate=2/1/2017&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/xbox/acquisitions?applicationId=BRRT4NJ9B3D1&startDate=8/1/2017&endDate=8/31/2017&skip=0&filter=market eq 'US' and gender eq 'm' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>回應


### <a name="response-body"></a>回應主體

| 值      | 類型   | 描述                  |
|------------|--------|-------------------------------------------------------|
| 值      | 陣列  | 內含遊戲的彙總下載數資料的物件陣列。 如需有關每個物件中資料的詳細資訊，請參閱下方的＜[下載數數值](#acquisition-values)＞一節。                                                                                                                      |
| @nextLink  | 字串 | 如果還有其他資料頁面，此字串包含可以用來要求下一頁資料的 URI。 例如，如果要求的 **top** 參數被設定為 10000，但是查詢卻有超過 10000 個資料列的下載數資料，就會傳回此值。 |
| TotalCount | 整數    | 查詢之資料結果的資料列總數。              |


### <a name="acquisition-values"></a>下載數數值

*Value* 陣列中的元素包含下列值。

| 值               | 類型   | 描述                           |
|---------------------|--------|-------------------------------------------|
| 日期                | 字串 | 下載數資料之日期範圍中的第一個日期。 如果要求指定單一天數，此值便會是該日期。 如果要求指定一週、一個月或其他日期範圍，此值便會是該日期範圍的第一個日期。 |
| applicationId       | 字串 | 您正在擷取下載數資料之 Xbox One 遊戲的產品識別碼。 |
| applicationName     | 字串 | 遊戲的顯示名稱。       |
| acquisitionType     | 字串 | 其中一個下列字串，可指出收購類型：<ul><li><strong>免費</strong></li><li><strong>試用版</strong></li><li><strong>付費</strong></li><li><strong>促銷代碼</strong></li><li><strong>Iap</strong></li><li><strong>訂閱 Iap</strong></li><li><strong>私人對象</strong></li><li><strong>Pre 順序</strong></li><li><strong>Xbox Game Pass</strong> (或<strong>Game Pass</strong>如果在 2018 年 3 月 23 之前查詢資料)</li><li><strong>磁碟</strong></li><li><strong>預付碼</strong></li><li><strong>充電的 Pre 順序</strong></li><li><strong>取消的前順序</strong></li><li><strong>失敗的 Pre 順序</strong></li></ul>    |
| 年齡                 | 字串 | 下列其中一個字串，這些字串表示進行下載的使用者的年齡層：<ul><li><strong>less than 13</strong></li><li><strong>13-17</strong></li><li><strong>18-24</strong></li><li><strong>25-34</strong></li><li><strong>35-44</strong></li><li><strong>44-55</strong></li><li><strong>greater than 55</strong></li><li><strong>Unknown</strong></li></ul>     |
| deviceType          | 字串 | 以下字串之一指定完成收購的裝置類型：<ul><li><strong>電腦</strong></li><li><strong>Phone</strong></li><li><strong>Console</strong></li><li><strong>IoT</strong></li><li><strong>伺服器</strong></li><li><strong>平板電腦</strong></li><li><strong>全息影像</strong></li><li><strong>不明</strong></li></ul>  |
| gender              | 字串 | 下列其中一個字串，這些字串詳列進行收購的使用者的性別：<ul><li><strong>m</strong></li><li><strong>f</strong></li><li><strong>Unknown</strong></li></ul>     |
| market              | 字串 | 發生下載之市場的 ISO 3166 國家/地區碼。  |
| osVersion           | 字串 | 發生下載的 OS 版本。 適用於這種方法，這個值總是 **Windows 10**。</li></ul>    |
| paymentInstrumentType           | 字串 | 下列其中一個字串，這些字串表示用於採集的付款指示：<ul><li><strong>信用卡</strong></li><li><strong>直接轉帳卡</strong></li><li><strong>推斷購買</strong></li><li><strong>MS 餘額</strong></li><li><strong>電信業者</strong></li><li><strong>線上銀行轉帳</strong></li><li><strong>PayPal</strong></li><li><strong>分段交易</strong></li><li><strong>預付碼兌換</strong></li><li><strong>已支付零金額</strong></li><li><strong>eWallet</strong></li><li><strong>不明</strong></li></ul>    |
| sandboxId              | 字串 | 為遊戲建立的沙箱 ID。 這可以是 **RETAIL** 值或專用沙箱的 ID。  |
| storeClient         | 字串 | 下列其中一個表示收購發生的 Microsoft Store 版本之字串：<ul><li>**Windows Phone Store (client)**</li><li>**Microsoft Store (用戶端)** (或**Microsoft Store (用戶端)** 如果在 2018 年 3 月 23 之前查詢資料)</li><li>**Microsoft Store (網頁)** (或**Microsoft Store (網頁)** 如果在 2018 年 3 月 23 之前查詢資料)</li><li>**Volume purchase by organizations**</li><li>**其他**</li></ul>                             |
| xboxTitleIdHex              | 字串 | 由 Xbox 開發人員入口網站 (XDP) 所指派用於已啟用 Xbox Live 遊戲的Xbox Live 標題 ID (以十六進位值表示)。  |
| acquisitionQuantity | 數字 | 指定彙總層級期間發生的下載數目。     |
| purchasePriceUSDAmount | 數目 | 由客戶為該收購的付費金額係按當月匯率換算為美元。    |
| taxUSDAmount     | 數目 | 適用於收購的稅額轉換為美元。 |


### <a name="response-example"></a>回應範例

下列範例針對此要求示範範例 JSON 回應主體。

```json
{
  "Value": [
    {
      "date": "2017-02-01",
      "applicationId": "BRRT4NJ9B3D1 ",
      "applicationName": "Contoso Game",
      "acquisitionType": "Paid",
      "age": "35-49",
      "deviceType": "Console",
      "gender": "m",
      "market": "US",
      "osVersion": "Windows 10",
      "PaymentInstrumentType": "Credit Card ",
      "sandboxId": "RETAIL",
      "storeClient": "Windows Store (web)",
      "xboxTitleIdHex": "",
      "acquisitionQuantity": 1,
      "purchasePriceUSDAmount": 29.99,
      "taxUSDAmount": 2.99
    }
  ],
  "@nextLink": "xbox/acquisitions?applicationId=BRRT4NJ9B3D1&aggregationLevel=day&startDate=2017/02/01&endDate=2017/03/01&top=1&skip=1&orderby=date desc",
  "TotalCount": 39812
}
```

## <a name="related-topics"></a>相關主題

* [使用 Microsoft Store 服務存取分析資料](access-analytics-data-using-windows-store-services.md)
