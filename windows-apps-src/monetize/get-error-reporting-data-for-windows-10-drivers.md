---
author: mcleanbyron
ms.assetid: BAC79A64-445D-4702-8E96-7727FF180245
description: 在 Microsoft Store 分析 API 中使用此方法，以針對特定日期範圍與其他選擇性篩選器，取得 Windows 10 驅動程式的彙總錯誤報告資料。 這個方法僅供 IHV 使用。
title: 取得 Windows 10 驅動程式的錯誤報告資料
ms.author: mcleans
ms.date: 06/04/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP, Microsoft Store 服務, Microsoft Store 分析 API, 錯誤
ms.localizationpriority: medium
ms.openlocfilehash: e87a836c08f29e1e7279c19566ead8a8d7e36453
ms.sourcegitcommit: cd91724c9b81c836af4773df8cd78e9f808a0bb4
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/07/2018
ms.locfileid: "1989402"
---
# <a name="get-error-reporting-data-for-windows-10-drivers"></a>取得 Windows 10 驅動程式的錯誤報告資料

在 Microsoft Store 分析 API 中使用此方法，以針對特定日期範圍與其他選擇性篩選器，取得 Windows 10 驅動程式錯誤的彙總報告資料。 您可以使用[取得 Windows 10 驅動程式錯誤的詳細資料](get-details-for-a-windows-10-driver-error.md)方法，擷取其他錯誤資訊。

> [!NOTE]
> 此方法僅供隸屬於 [Windows 硬體開發人員中心計畫](https://msdn.microsoft.com/windows/hardware/drivers/dashboard/get-started-with-the-hardware-dashboard)的開發人員帳戶使用。

## <a name="prerequisites"></a>必要條件

若要使用這個方法，您必須先進行下列動作：

* 如果您尚未這樣做，請先完成 Microsoft Store 分析 API 的所有[先決條件](access-analytics-data-using-windows-store-services.md#prerequisites)。
* [取得 Azure AD 存取權杖](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token)以便用於這個方法的要求標頭。 在您取得存取權杖之後，您在權杖到期之前有 60 分鐘的時間可以使用權杖。 權杖到期之後，您可以取得新的權杖。

## <a name="request"></a>要求


### <a name="request-syntax"></a>要求的語法

| 方法 | 要求 URI                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/driver/failurehits``` |


### <a name="request-header"></a>要求的標頭

| 標頭        | 類型   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 授權 | 字串 | 必要。 Azure AD 存取權杖，形式為 **Bearer** &lt;*token*&gt;。 |


### <a name="request-parameters"></a>要求參數

| 參數        | 類型   |  說明      |  必要  
|---------------|--------|---------------|------|
| applicationId   | 字串  | 您想要擷取錯誤資料的驅動程式的產品識別碼值。 | 是  |
| startDate | 日期 | 要擷取錯誤報告資料之日期範圍的開始日期。 預設為目前的日期。 |  否  |
| endDate | 日期 | 要擷取錯誤報告資料之日期範圍的結束日期。 預設為目前的日期。 |  否  |
| top | 整數 | 在要求中傳回的資料列數目。 最大值及未指定的預設值為 10000。 如果查詢中有更多資料列，回應主體將會包含您可以用來要求下一頁資料的下一頁連結。 |  否  |
| skip | 整數 | 在查詢中要略過的資料列數目。 使用此參數來瀏覽大型資料集。 例如，top=10000 且 skip=0 將擷取前 10000 個資料列的資料，top=10000 且 skip=10000 將擷取下 10000 個資料列的資料，以此類推。 |  否  |
| filter |字串  | 一或多個篩選回應中資料列的陳述式。 每個陳述式包含一個與 **eq** 或 **ne** 運算子關聯的欄位名稱 (來自回應主體) 和值，而陳述式可以使用 **and** 或 **or** 結合。 *filter* 參數中的字串值必須由單引號括住。 您可以指定來自回應主體的下列欄位：<p/><ul><li><strong>日期</strong></li><li><strong>submissionId</strong></li><li><strong>failureName</strong></li><li><strong>failureHash</strong></li><li><strong>symbol</strong></li><li><strong>osVersion</strong></li><li><strong>osName</strong></li><li><strong>eventType</strong></li><li><strong>market</strong></li><li><strong>deviceType</strong></li><li><strong>driverName</strong></li><li><strong>driverVersion</strong></li><li><strong>oemName</strong></li><li><strong>oemModel</strong></li><li><strong>flightRing</strong></li><li><strong>architecture</strong></li></ul> | 否   |
| aggregationLevel | 字串 | 指定要擷取彙總資料的時間範圍。 可以是下列其中一個字串：<strong>day</strong>、<strong>week</strong> 或 <strong>month</strong>。 如果沒有指定，則預設為 <strong>day</strong>。 如果您指定 <strong>week</strong> 或 <strong>month</strong>，<em>failureName</em> 和 <em>failureHash</em> 值將會被限制在 1000 個值區。 | 否 |
| orderby | 字串 | 將結果資料值排序的陳述式。 語法為 <em>orderby=field [order],field [order],...</em>，您可以從回應本文指定下列欄位︰<p/><ul><li><strong>日期</strong></li><li><strong>submissionId</strong></li><li><strong>failureName</strong></li><li><strong>failureHash</strong></li><li><strong>symbol</strong></li><li><strong>osVersion</strong></li><li><strong>osName</strong></li><li><strong>eventType</strong></li><li><strong>market</strong></li><li><strong>deviceType</strong></li><li><strong>driverName</strong></li><li><strong>driverVersion</strong></li><li><strong>oemName</strong></li><li><strong>oemModel</strong></li><li><strong>flightRing</strong></li><li><strong>architecture</strong></li></ul><p><em>order</em> 參數為選擇性，並可以是 <strong>asc</strong> 或 <strong>desc</strong>，用來指定每個欄位的遞增或遞減順序。 預設為 <strong>asc</strong>。</p><p>下列為 <em>orderby</em> 字串的範例：<em>orderby=date,market</em></p> |  否  |
| groupby | 字串 | 將資料彙總僅套用至指定欄位的陳述式。 您可以指定來自回應主體的下列欄位：<p/><ul><li><strong>submissionId</strong></li><li><strong>failureName</strong></li><li><strong>failureHash</strong></li><li><strong>symbol</strong></li><li><strong>osVersion</strong></li><li><strong>eventType</strong></li><li><strong>market</strong></li><li><strong>deviceType</strong></li><li><strong>driverName</strong></li><li><strong>driverVersion</strong></li><li><strong>oemName</strong></li><li><strong>oemModel</strong></li><li><strong>flightRing</strong></li><li><strong>architecture</strong></li></ul><p>傳回的資料列將包含 <em>groupby</em> 參數中指定的欄位，以及下列項目：</p><ul><li><strong>日期</strong></li><li><strong>applicationId</strong></li><li><strong>eventCount</strong></li></ul><p><em>groupby</em> 參數可以搭配 <em>aggregationLevel</em> 參數使用。 例如：<em>&amp;groupby=failureName,market&amp;aggregationLevel=week</em></p></p> |  否  |


### <a name="request-example"></a>要求範例

下列範例示範取得 OEM 硬體錯誤報告資料的數個要求。

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/driver/failurehits?applicationId=18430592857500421&startDate=1/1/2015&endDate=2/1/2015&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/driver/failurehits?applicationId=18430592857500421&startDate=8/1/2015&endDate=8/31/2015&skip=0&filter=market eq 'US' and deviceType eq 'PC' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>回應


### <a name="response-body"></a>回應主體

| 值      | 類型    | 說明     |
|------------|---------|--------------|
| 值      | array   | 內含彙總錯誤報告資料的物件陣列。 如需有關每個物件中資料的詳細資訊，請參閱下表。     |
| @nextLink  | 字串  | 如果還有其他資料頁面，此字串包含可以用來要求下一頁資料的 URI。 例如，如果要求的 **top** 參數被設定為 10000，但是查詢卻有超過 10000 個資料列的錯誤，就會傳回此值。 |
| TotalCount | 整數 | 查詢之資料結果的資料列總數。     |


*Value* 陣列中的元素包含下列值。

| 值           | 類型    | 描述        |
|-----------------|---------|---------------------|
| 日期            | 字串  | 錯誤資料之日期範圍中的第一個日期。 如果要求指定單一天數，此值便會是該日期。 如果要求指定一週、一個月或其他日期範圍，此值便會是該日期範圍的第一個日期。 |
| applicationId   | 字串  | 您擷取錯誤資料的驅動程式的產品識別碼值。 |
| submissionId   | 字串  | 與驅動程式相關聯的提交識別碼。 |
| failureName     | 字串  | 失敗的名稱，由四個部分組成：一個或多個問題類別、例外狀況/錯誤檢查碼、失敗發生位置驅動程式名稱，以及相關函式名稱。  |
| failureHash     | 字串  | 錯誤的唯一識別碼。   |
| symbol          | 字串  | 指派給此錯誤的符號。 |
| osVersion       | 字串  | 發生錯誤之 OS 的四個部分組建版本。  |
| osName       | 字串  | 發生錯誤之 OS 的名稱。  |
| eventType       | 字串  | 發生錯誤的類型。      |
| market          | 字串  | 裝置市場的 ISO 3166 國家/地區碼。   |
| deviceType      | 字串  | 下列其中一個字串，指定發生錯誤的裝置類型：<p/><ul><li><strong>電腦</strong></li><li><strong>Phone</strong></li><li><strong>Console</strong></li><li><strong>IoT</strong></li><li><strong>Holographic</strong></li><li><strong>Unknown</strong></li></ul>    |
| driverName     | 字串  | 與此錯誤關聯之驅動程式的唯一名稱。      |
| driverVersion  | 字串  | 與此錯誤關聯之驅動程式的版本。   |
| architecture | 字串 |  發生錯誤之裝置的架構。  |
| oemName | 字串 | 發生錯誤之裝置的 OEM 名稱。 |
| oemModel | 字串 | 發生錯誤之裝置的型號名稱。 |
| flightRing | 字串 | 發生錯誤之作業系統正式發行前小眾測試版的名稱。 |
| eventCount      | 整數 | 針對指定的彙總層級被歸類到此錯誤的事件數目。      |


### <a name="response-example"></a>回應範例

下列範例針對此要求示範範例 JSON 回應主體。

```json
{
  "Value": [
    {
      "date": "2017-02-26",
      "submissionId":"29989500_13736592797500435_1152921504626321174",
      "applicationId":"18430592857500421",
      "driverName": "fastboot.sys",
      "osVersion": "10.0.14393.693",
      "osName": "Windows 10",
      "flightRing": "Windows 10 Retail",
      "oemName": "Contoso",
      "oemModel": "C-122",
      "market": "US",
      "symbol": "fastboot",
      "failureName": "AV_fastboot!unknown_function",
      "failureHash": "f060c0b6-fbe6-463f-a9f1-b7ebc1cc722f",
      "architecture": "x64",
      "driverVersion": "2.1.1.0",
      "deviceType": "Unknown",
      "eventType": "System crash",
      "deviceCount": 1.0,
      "eventCount": 1.0
    }]
}
```

## <a name="related-topics"></a>相關主題

* [取得 Windows 10 驅動程式錯誤的詳細資料](get-details-for-a-windows-10-driver-error.md)
* [下載適用於 Windows 10 驅動程式錯誤的 CAB 檔案](download-the-cab-file-for-a-windows-10-driver-error.md)
