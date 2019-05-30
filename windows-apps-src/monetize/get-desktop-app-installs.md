---
description: 您可以使用此 REST URI，在指定的日期範圍和其他選用的篩選器是桌面應用程式取得彙總的安裝資料。
title: 取得傳統型應用程式安裝
ms.date: 03/01/2018
ms.topic: article
keywords: windows 10 傳統型應用程式安裝時，Windows 桌面應用程式
localizationpriority: medium
ms.openlocfilehash: 27ee2f0b6977c551d50fce9dec26c864e3858413
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/29/2019
ms.locfileid: "66372129"
---
# <a name="get-desktop-application-installs"></a>取得傳統型應用程式安裝


若要取得 JSON 格式的彙總的安裝資料，您已新增至桌面應用程式使用此 REST URI [Windows 桌面應用程式](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program)。 此 URI 可讓您取得在指定的日期範圍和其他選用的篩選器的安裝資料。 這項資訊也會提供[會安裝報表](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program)在合作夥伴中心內的桌面應用程式。

## <a name="prerequisites"></a>必要條件


若要使用這個方法，您必須先進行下列動作：

* 如果您尚未這樣做，請先完成 Microsoft Store 分析 API 的所有[先決條件](access-analytics-data-using-windows-store-services.md#prerequisites)。
* [取得 Azure AD 存取權杖](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token)以便用於這個方法的要求標頭。 在您取得存取權杖之後，您在權杖到期之前有 60 分鐘的時間可以使用權杖。 權杖到期之後，您可以取得新的權杖。

## <a name="request"></a>要求


### <a name="request-syntax"></a>要求的語法

| 方法 | 要求 URI       |
|--------|----------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/installbasedaily```|


### <a name="request-header"></a>要求的標頭

| 標頭        | 類型   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | 字串 | 必要。 在表單中的 Azure AD 存取權杖**持有人** &lt;*語彙基元*&gt;。 |


### <a name="request-parameters"></a>要求參數

| 參數        | 類型   |  描述      |  必要項  
|---------------|--------|---------------|------|
| applicationId | 字串 | 您想要擷取的桌面應用程式的產品識別碼安裝資料。 若要取得桌面應用程式的產品識別碼，請開啟任何[桌面應用程式以在合作夥伴中心中的分析報告](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program)(例如**會安裝報表**)，並從 URL 擷取產品識別碼。 |  是  |
| startDate | date | 要擷取安裝資料之日期範圍的開始日期。 預設為目前日期前的 90 天。 |  否  |
| endDate | date | 要擷取安裝資料之日期範圍的結束日期。 預設為目前的日期。 |  否  |
| top | ssNoversion | 要在要求中傳回的資料列數目。 最大值及未指定的預設值為 10000。 如果查詢中有更多資料列，回應主體將會包含您可以用來要求下一頁資料的下一頁連結。 |  否  |
| skip | ssNoversion | 在查詢中要略過的資料列數目。 使用此參數來循頁瀏覽大型資料集。 例如，top=10000 且 skip=0 將擷取前 10000 個資料列的資料，top=10000 且 skip=10000 將擷取下 10000 個資料列的資料，以此類推。 |  否  |
| filter | 字串  | 一或多個篩選回應中資料列的陳述式。 每個陳述式包含一個與 **eq** 或 **ne** 運算子關聯的欄位名稱 (來自回應主體) 和值，而陳述式可以使用 **and** 或 **or** 結合。 *filter* 參數中的字串值必須由單引號括住。 您可以在回應本文中指定下列欄位：<p/><ul><li><strong>applicationVersion</strong></li><li><strong>deviceType</strong></li><li><strong>market</strong></li><li><strong>osVersion</strong></li><li><strong>osRelease</strong></li></ul> | 否   |
| orderby | 字串 | 對每個安裝的結果資料值做出排序的陳述式。 語法為 <em>orderby=field [order],field [order],...</em>。<em>field</em> 參數可以是來自回應主題的下列其中一個欄位：<p/><ul><li><strong>productName</strong></li><li><strong>date</strong><li><strong>applicationVersion</strong></li><li><strong>deviceType</strong></li><li><strong>market</strong></li><li><strong>osVersion</strong></li><li><strong>osRelease</strong></li><li><strong>installBase</strong></li></ul><p><em>order</em> 參數為選擇性，並可以是 <strong>asc</strong> 或 <strong>desc</strong>，以指定每個欄位的遞增或遞減順序。 預設為 <strong>asc</strong>。</p><p>下列為 <em>orderby</em> 字串的範例：<em>orderby=date,market</em></p> |  否  |
| groupby | 字串 | 將資料彙總僅套用至指定欄位的陳述式。 您可以在回應本文中指定下列欄位：<p/><ul><li><strong>applicationVersion</strong></li><li><strong>deviceType</strong></li><li><strong>market</strong></li><li><strong>osVersion</strong></li><li><strong>osRelease</strong></li></ul><p>傳回的資料列將包含 <em>groupby</em> 參數中指定的欄位，以及下列項目：</p><ul><li><strong>applicationId</strong></li><li><strong>date</strong></li><li><strong>productName</strong></li><li><strong>installBase</strong></li></ul></p> |  否  |


### <a name="request-example"></a>要求範例

下列範例會示範幾個要求來取得桌面應用程式安裝資料。 取代*applicationId*桌面應用程式的產品識別碼的值。

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/installbasedaily?applicationId=1234567890&startDate=2018-01-01&endDate=2018-02-01&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/installbasedaily?applicationId=1234567890&startDate=2018-01-01&endDate=2018-02-01&filter=market eq 'US' and deviceType eq 'PC' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>回應


### <a name="response-body"></a>回應主體

| 值      | 類型   | 描述                  |
|------------|--------|-------------------------------------------------------|
| 值      | 陣列  | 內含彙總安裝資料的物件陣列。 如需有關每個物件中資料的詳細資訊，請參閱下表。 |
| @nextLink  | 字串 | 如果還有其他資料頁面，此字串會包含可以用來要求下一頁資料的 URI。 例如，如果要求的 **top** 參數被設定為 10000，但是查詢卻有超過 10000 個資料列的安裝資料，就會傳回此值。 |
| TotalCount | ssNoversion    | 查詢之資料結果的資料列總數。 |


*Value* 陣列中的元素包含下列值。

| 值               | 類型   | 描述                           |
|---------------------|--------|-------------------------------------------|
| date                | 字串 | 安裝基底值與關聯的日期。 |
| applicationId       | 字串 | 要為其擷取桌面應用程式的產品識別碼安裝資料。 |
| productName         | 字串 | 從相關聯的可執行檔中繼資料衍生的傳統型應用程式的顯示名稱。 |
| applicationVersion  | 字串 | 已安裝的應用程式可執行檔版本。 |
| deviceType          | 字串 | 其中一個指定的桌面應用程式安裝所在的裝置類型的下列字串：<p/><ul><li><strong>PC</strong></li><li><strong>Server</strong></li><li><strong>平板電腦</strong></li><li><strong>未知</strong></li></ul> |
| market              | 字串 | 桌面應用程式安裝所在的市場 ISO 3166 國家/地區代碼。 |
| osVersion           | 字串 | 下列其中一個字串，指定傳統型應用程式安裝所在的作業系統版本：<p/><ul><li><strong>Windows 7</strong></li><li><strong>Windows 8.1</strong></li><li><strong>Windows 10</strong></li><li><strong>Windows Server 2016</strong></li><li><strong>Windows Server 1709</strong></li><li><strong>未知</strong></li></ul>   |
| osRelease           | 字串 | 下列其中一個字串指定的 OS 版本或正式發行前小眾測試頻道 (為 OS 版本內的次群族) 上已安裝傳統型應用程式。<p/><p>適用於 Windows 10：</p><ul><li><strong>版本 1507</strong></li><li><strong>1511 版</strong></li><li><strong>版本 1607</strong></li><li><strong>版本 1703</strong></li><li><strong>版本 1709</strong></li><li><strong>發行預覽</strong></li><li><strong>測試人員快速</strong></li><li><strong>緩慢的測試人員</strong></li></ul><p/><p>若是 Windows Server 1709：</p><ul><li><strong>RTM</strong></li></ul><p>若是 Windows Server 2016：</p><ul><li><strong>版本 1607</strong></li></ul><p>適用於 Windows 8.1：</p><ul><li><strong>Update 1</strong></li></ul><p>適用於 Windows 7：</p><ul><li><strong>Service Pack 1</strong></li></ul><p>如果 OS 版本或正式發行前小眾測試通道不明，此欄位會有<strong>不明</strong>值。</p> |
| installBase         | 數字 | 不同的裝置已安裝在指定的彙總層級的產品數目。 |


### <a name="response-example"></a>回應範例

下列範例示範這個要求的一個範例 JSON 回應主體。

```json
{
  "Value": [
    {
      "date": "2018-01-24",
      "applicationId": "123456789",
      "productName": "Contoso Demo",
      "applicationVersion": "1.0.0.0",
      "deviceType": "PC",
      "market": "All",
      "osVersion": "Windows 10",
      "osRelease": "Version 1709",
      "installBase": 348218.0
    }
  ],
  "@nextLink": "desktop/installbasedaily?applicationId=123456789&startDate=2018-01-01&endDate=2018-02-01&top=10000&skip=10000&groupby=applicationVersion,deviceType,osVersion,osRelease",
  "TotalCount": 23012
}
```

## <a name="related-topics"></a>相關主題

* [Windows 桌面應用程式](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program)
