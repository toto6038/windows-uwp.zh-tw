---
description: 您可以使用此 REST URI，取得區塊的詳細資料在指定的日期範圍和其他選用的篩選器是桌面應用程式。
title: 取得傳統型應用程式的升級區塊詳細資料
ms.date: 07/11/2018
ms.topic: article
keywords: windows 10 傳統型應用程式區塊，Windows 桌面應用程式
localizationpriority: medium
ms.openlocfilehash: 66516c2bee58b3628542b66afcb0de24d0d9702b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57600223"
---
# <a name="get-upgrade-block-details-for-your-desktop-application"></a>取得傳統型應用程式的升級區塊詳細資料

使用此 REST URI，以取得詳細資料的 Windows 10 裝置的桌面應用程式中的特定可執行檔會封鎖從執行 Windows 10 升級。 您可以使用此 URI 只對您加入的桌面應用程式[Windows 桌面應用程式](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program)。 這項資訊也會提供[應用程式會封鎖報表](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program#application-blocks-report)在合作夥伴中心內的桌面應用程式。

此 URI 會類似於[桌面應用程式會取得升級封鎖](get-desktop-block-data.md)，但它會傳回您桌面應用程式中的裝置特定的可執行檔的區塊資訊。

## <a name="prerequisites"></a>必要條件


若要使用這個方法，您必須先進行下列動作：

* 如果您尚未這樣做，請先完成 Microsoft Store 分析 API 的所有[先決條件](access-analytics-data-using-windows-store-services.md#prerequisites)。
* [取得 Azure AD 存取權杖](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token)以便用於這個方法的要求標頭。 在您取得存取權杖之後，您在權杖到期之前有 60 分鐘的時間可以使用權杖。 權杖到期之後，您可以取得新的權杖。

## <a name="request"></a>要求


### <a name="request-syntax"></a>要求的語法

| 方法 | 要求 URI       |
|--------|----------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/blockdetails```|


### <a name="request-header"></a>要求的標頭

| 標頭        | 類型   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | 字串 | 必要。 在表單中的 Azure AD 存取權杖**持有人** &lt;*語彙基元*&gt;。 |


### <a name="request-parameters"></a>要求參數

| 參數        | 類型   |  描述      |  必要  
|---------------|--------|---------------|------|
| applicationId | 字串 | 您要擷取的資料區塊的桌面應用程式產品識別碼。 若要取得桌面應用程式的產品識別碼，請開啟任何[桌面應用程式以在合作夥伴中心中的分析報告](https://msdn.microsoft.com/library/windows/desktop/mt826504)(例如**封鎖報表**)，並從 URL 擷取產品識別碼。 |  是  |
| fileName | 字串 | 已封鎖的可執行檔名稱 |
| startDate | date | 若要擷取區塊資料的日期範圍中開始日期。 預設為目前日期前的 90 天。 |  否  |
| endDate | date | 若要擷取的日期範圍內的區塊資料結束日期。 預設為目前的日期。 |  否  |
| top | 整數 | 要在要求中傳回的資料列數目。 最大值及未指定的預設值為 10000。 如果查詢中有更多資料列，回應主體將會包含您可以用來要求下一頁資料的下一頁連結。 |  否  |
| skip | 整數 | 在查詢中要略過的資料列數目。 使用此參數來循頁瀏覽大型資料集。 例如，top=10000 且 skip=0 將擷取前 10000 個資料列的資料，top=10000 且 skip=10000 將擷取下 10000 個資料列的資料，以此類推。 |  否  |
| filter | 字串  | 一或多個篩選回應中資料列的陳述式。 每個陳述式包含一個與 **eq** 或 **ne** 運算子關聯的欄位名稱 (來自回應主體) 和值，而陳述式可以使用 **and** 或 **or** 結合。 *filter* 參數中的字串值必須由單引號括住。 您可以在回應本文中指定下列欄位：<p/><ul><li><strong>applicationVersion</strong></li><li><strong>architecture</strong></li><li><strong>blockType</strong></li><li><strong>deviceType</strong></li><li><strong>market</strong></li><li><strong>osRelease</strong></li><li><strong>osVersion</strong></li><li><strong>productName</strong></li><li><strong>targetOs</strong></li></ul> | 否   |
| orderby | 字串 | 排序結果的每個區塊的資料值的陳述式。 語法為 <em>orderby=field [order],field [order],...</em>。<em>field</em> 參數可以是來自回應主題的下列其中一個欄位：<p/><ul><li><strong>applicationVersion</strong></li><li><strong>architecture</strong></li><li><strong>blockType</strong></li><li><strong>date</strong><li><strong>deviceType</strong></li><li><strong>market</strong></li><li><strong>osRelease</strong></li><li><strong>osVersion</strong></li><li><strong>productName</strong></li><li><strong>targetOs</strong></li><li><strong>deviceCount</strong></li></ul><p><em>order</em> 參數為選擇性，並可以是 <strong>asc</strong> 或 <strong>desc</strong>，以指定每個欄位的遞增或遞減順序。 預設為 <strong>asc</strong>。</p><p>下列為 <em>orderby</em> 字串的範例：<em>orderby=date,market</em></p> |  否  |
| groupby | 字串 | 將資料彙總僅套用至指定欄位的陳述式。 您可以在回應本文中指定下列欄位：<p/><ul><li><strong>applicationVersion</strong></li><li><strong>architecture</strong></li><li><strong>blockType</strong></li><li><strong>deviceType</strong></li><li><strong>market</strong></li><li><strong>osRelease</strong></li><li><strong>osVersion</strong></li><li><strong>targetOs</strong></li></ul><p>傳回的資料列將包含 <em>groupby</em> 參數中指定的欄位，以及下列項目：</p><ul><li><strong>applicationId</strong></li><li><strong>date</strong></li><li><strong>productName</strong></li><li><strong>deviceCount</strong></li></ul></p> |  否  |


### <a name="request-example"></a>要求範例

下列範例會示範幾個要求來取得桌面應用程式區塊資料。 取代*applicationId*桌面應用程式的產品識別碼的值。

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/blockdetails?applicationId=10238467886765136388&fileName=contoso.exe&startDate=2018-05-01&endDate=2018-06-07&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/blockdetails?applicationId=10238467886765136388&fileName=contoso.exe&startDate=2018-05-01&endDate=2018-06-07&filter=market eq 'US' and deviceType eq 'PC' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>回應


### <a name="response-body"></a>回應主體

| 值      | 類型   | 描述                  |
|------------|--------|-------------------------------------------------------|
| 值      | 陣列  | 包含彙總的區塊資料的物件陣列。 如需有關每個物件中資料的詳細資訊，請參閱下表。 |
| @nextLink  | 字串 | 如果還有其他資料頁面，此字串會包含可以用來要求下一頁資料的 URI。 例如，這個值會傳回如果**頂端**要求參數設定為 10000，但有超過 10000 個資料列的區塊資料查詢。 |
| TotalCount | 整數    | 查詢之資料結果的資料列總數。 |


*Value* 陣列中的元素包含下列值。

| 值               | 類型   | 描述                           |
|---------------------|--------|-------------------------------------------|
| applicationId       | 字串 | 您可以為其擷取資料區塊的桌面應用程式產品識別碼。 |
| date                | 字串 | 區塊點擊值相關聯的日期。 |
| productName         | 字串 | 從相關聯的可執行檔中繼資料衍生的傳統型應用程式的顯示名稱。 |
| fileName            | 字串 | 已封鎖可執行檔。 |
| applicationVersion  | 字串 | 已封鎖的應用程式可執行檔版本。 |
| osVersion           | 字串 | 其中一個指定的桌面應用程式目前執行所在的作業系統版本的下列字串：<p/><ul><li><strong>Windows 7</strong></li><li><strong>Windows 8.1</strong></li><li><strong>Windows 10</strong></li><li><strong>Windows Server 2016</strong></li><li><strong>Windows Server 1709</strong></li><li><strong>未知</strong></li></ul>   |
| osRelease           | 字串 | 其中一個指定的作業系統版本或試驗的環形 （subpopulation 內的 OS 版本) 目前執行所在的桌面應用程式的下列字串。<p/><p>適用於 Windows 10：</p><ul><li><strong>版本 1507</strong></li><li><strong>1511 版</strong></li><li><strong>版本 1607</strong></li><li><strong>版本 1703</strong></li><li><strong>版本 1709</strong></li><li><strong>發行預覽</strong></li><li><strong>測試人員快速</strong></li><li><strong>緩慢的測試人員</strong></li></ul><p/><p>若是 Windows Server 1709：</p><ul><li><strong>RTM</strong></li></ul><p>若是 Windows Server 2016：</p><ul><li><strong>版本 1607</strong></li></ul><p>適用於 Windows 8.1：</p><ul><li><strong>Update 1</strong></li></ul><p>適用於 Windows 7：</p><ul><li><strong>Service Pack 1</strong></li></ul><p>如果 OS 版本或正式發行前小眾測試通道不明，此欄位會有<strong>不明</strong>值。</p> |
| market              | 字串 | 桌面應用程式被封鎖的市場 ISO 3166 國家/地區代碼。 |
| deviceType          | 字串 | 下列字串，指定在其的桌面應用程式被封鎖的裝置類型的其中一項：<p/><ul><li><strong>PC</strong></li><li><strong>Server</strong></li><li><strong>平板電腦</strong></li><li><strong>未知</strong></li></ul> |
| blockType            | 字串 | 其中一個下列字串，指定在裝置上找到的區塊類型：<p/><ul><li><strong>潛在 Sediment</strong></li><li><strong>暫存 Sediment</strong></li><li><strong>執行階段通知</strong></li></ul><p/><p/>如需有關這些區塊類型以及其對開發人員和使用者所代表的意義的詳細資訊，請參閱說明[應用程式會封鎖報表](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program#application-blocks-report)。 |
| architecture        | 字串 | 區塊所在的裝置的架構： <p/><ul><li><strong>ARM64</strong></li><li><strong>X86</strong></li></ul> |
| targetOs            | 字串 | 其中一個下列字串，指定在其的桌面應用程式會封鎖而無法執行的 Windows 10 作業系統版本： <p/><ul><li><strong>版本 1709</strong></li><li><strong>1803 版</strong></li></ul> |
| deviceCount         | 數字 | 不同的裝置具有指定的彙總層級的區塊數目。 |


### <a name="response-example"></a>回應範例

下列範例示範這個要求的一個範例 JSON 回應主體。

```json
{
  "Value": [
    {
     "applicationId": "10238467886765136388",
     "date": "2018-06-03",
     "productName": "Contoso Demo",
     "fileName": "contosodemo.exe",
     "applicationVersion": "2.2.2.0",
     "osVersion": "Windows 8.1",
     "osRelease": "Update 1",
     "market": "ZA",
     "deviceType": "All",
     "blockType": "Runtime Notification",
     "architecture": "X86",
     "targetOs": "RS4",
     "deviceCount": 120
    }
  ],
  "@nextLink": "desktop/blockdetails?applicationId=123456789&startDate=2018-01-01&endDate=2018-02-01&top=10000&skip=10000&groupby=applicationVersion,deviceType,osVersion,osRelease",
  "TotalCount": 23012
}
```

## <a name="related-topics"></a>相關主題

* [Windows 桌面應用程式](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program)
* [取得您的桌面應用程式升級的區塊](get-desktop-block-data.md)
