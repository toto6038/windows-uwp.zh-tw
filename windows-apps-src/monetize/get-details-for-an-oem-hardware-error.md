---
author: mcleanbyron
ms.assetid: 8425F704-8A03-493F-A3D2-8442E85FD835
description: 在 Microsoft Store 分析 API 中使用此方法，以取得特定硬體錯誤的詳細資料。 這個方法僅供 OEM 使用。
title: 取得 OEM 硬體錯誤的詳細資料
ms.author: mcleans
ms.date: 06/04/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP, Microsoft Store 服務, Microsoft Store 分析 API, 錯誤, 詳細資料
ms.localizationpriority: medium
ms.openlocfilehash: c4ac647559b71b7c8cf2724940e857fd99c557f5
ms.sourcegitcommit: cd91724c9b81c836af4773df8cd78e9f808a0bb4
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/07/2018
ms.locfileid: "1989552"
---
# <a name="get-details-for-an-oem-hardware-error"></a>取得 OEM 硬體錯誤的詳細資料

在 Microsoft Store 分析 API 中使用此方法，以取得特定 OEM 硬體錯誤的詳細資料 (JSON 格式)。 使用這個方法之前，您必須先使用[取得 OEM 硬體錯誤報告資料](get-oem-hardware-error-reporting-data.md)方法來擷取您要取得詳細資訊之錯誤的識別碼。

> [!NOTE]
> 此方法僅供隸屬於 [Windows 硬體開發人員中心計畫](https://msdn.microsoft.com/windows/hardware/drivers/dashboard/get-started-with-the-hardware-dashboard)的開發人員帳戶使用。

## <a name="prerequisites"></a>必要條件


若要使用這個方法，您必須先進行下列動作：

* 如果您尚未這樣做，請先完成 Microsoft Store 分析 API 的所有[先決條件](access-analytics-data-using-windows-store-services.md#prerequisites)。
* [取得 Azure AD 存取權杖](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token)以便用於這個方法的要求標頭。 在您取得存取權杖之後，您在權杖到期之前有 60 分鐘的時間可以使用權杖。 權杖到期之後，您可以取得新的權杖。
* 取得您想要取得詳細資訊之錯誤的識別碼。 若要取得此識別碼，請使用[取得 OEM 硬體錯誤報告資料](get-oem-hardware-error-reporting-data.md)方法，並使用該方法回應主體中的 **failureHash** 值。

## <a name="request"></a>要求


### <a name="request-syntax"></a>要求的語法

| 方法 | 要求 URI                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/hardware/failuredetails``` |


### <a name="request-header"></a>要求的標頭

| 標頭        | 類型   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 授權 | 字串 | 必要。 Azure AD 存取權杖，形式為 **Bearer** &lt;*token*&gt;。 |


### <a name="request-parameters"></a>要求參數

| 參數        | 類型   |  說明      |  必要  
|---------------|--------|---------------|------|
| failureHash | 字串 | 您想要取得詳細資訊之錯誤的唯一識別碼。 若要取得您有興趣之錯誤的此值，請使用[取得 OEM 硬體錯誤報告資料](get-oem-hardware-error-reporting-data.md)方法，並並使用該方法回應主體中的 **failureHash** 值。 |  是  |
| startDate | 日期 | 要擷取詳細錯誤資料之日期範圍的開始日期。 預設為目前日期的前 30 天。 |  否  |
| endDate | 日期 | 要擷取詳細錯誤資料之日期範圍的結束日期。 預設為目前的日期。 |  否  |
| top | 整數 | 在要求中傳回的資料列數目。 最大值及未指定的預設值為 10000。 如果查詢中有更多資料列，回應主體將會包含您可以用來要求下一頁資料的下一頁連結。 |  否  |
| skip | 整數 | 在查詢中要略過的資料列數目。 使用此參數來瀏覽大型資料集。 例如，top=10 且 skip=0 將擷取前 10 個資料列的資料，top=10 且 skip=10 將擷取下 10 個資料列的資料，以此類推。 |  否  |
| filter |字串  | 一或多個篩選回應中資料列的陳述式。 每個陳述式包含一個與 **eq** 或 **ne** 運算子關聯的欄位名稱 (來自回應主體) 和值，而陳述式可以使用 **and** 或 **or** 結合。 *filter* 參數中的字串值必須由單引號括住。 您可以指定下列欄位：<p/><ul><li><strong>failureName</strong></li><li><strong>failureHash</strong></li><li><strong>symbol</strong></li><li><strong>osVersion</strong></li><li><strong>market</strong></li><li><strong>deviceType</strong></li><li><strong>moduleName</strong></li><li><strong>moduleVersion</strong></li><li><strong>mode</strong></li><li><strong>architecture</strong></li><li><strong>model</strong></li><li><strong>baseboard</strong></li><li><strong>modelFamily</strong></li><li><strong>flightRing</strong></li><li><strong>cabIdHash</strong></li><li><strong>cabType</strong></li><li><strong>cabExpirationTime</strong></li></ul> | 否   |
| orderby | 字串 | 將結果資料值排序的陳述式。 語法為 <em>orderby=field [order],field [order],...</em>，您可以從回應本文指定下列欄位︰<p/><ul><li><strong>date</strong></li><li><strong>market</strong></li><li><strong>cabIdHash</strong></li><li><strong>cabExpirationTime</strong></li><li><strong>cabType</strong></li><li><strong>deviceType</strong></li><li><strong>moduleName</strong></li><li><strong>moduleVersion</strong></li><li><strong>osVersion</strong></li><li><strong>packageVersion</strong></li><li><strong>osBuild</strong></li><li><strong>mode</strong></li><li><strong>architecture</strong></li><li><strong>model</strong></li><li><strong>baseboard</strong></li><li><strong>modelFamily</strong></li></ul><p><em>order</em> 參數為選擇性，並可以是 <strong>asc</strong> 或 <strong>desc</strong>，用來指定每個欄位的遞增或遞減順序。 預設為 <strong>asc</strong>。</p><p>下列為 <em>orderby</em> 字串的範例：<em>orderby=date,market</em></p> |  否  |


### <a name="request-example"></a>要求範例

下列範例示範取得詳細錯誤資料的數個要求。

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/hardware/failuredetails?failureHash=012e33e3-dbc9-b12f-c124-9d9810f05d8b&startDate=2016-11-05&endDate=2016-11-06&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/hardware/failuredetails?failureHash=012e33e3-dbc9-b12f-c124-9d9810f05d8b&startDate=2016-11-05&endDate=2016-11-06&top=10&skip=0&filter=market eq 'US' and deviceType eq 'PC' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>回應


### <a name="response-body"></a>回應主體

| 值      | 類型    | 說明    |
|------------|---------|------------|
| 值      | 陣列   | 包含詳細錯誤資料的物件陣列。 如需有關每個物件中資料的詳細資訊，請參閱下表。          |
| @nextLink  | 字串  | 如果還有其他資料頁面，此字串包含可以用來要求下一頁資料的 URI。 例如，如果要求的 **top** 參數被設定為 10，但是查詢卻有超過 10 個資料列的錯誤，就會傳回此值。 |
| TotalCount | 整數 | 查詢之資料結果的資料列總數。        |


*Value* 陣列中的元素包含下列值。

| 值           | 類型    | 描述     |
|-----------------|---------|----------------------------|
| 日期            | 字串  | 錯誤資料之日期範圍中的第一個日期。 如果要求指定單一天數，此值便會是該日期。 如果要求指定一週、一個月或其他日期範圍，此值便會是該日期範圍的第一個日期。 |
| sellerId   | 字串  | 與開發人員帳戶相關聯的賣方識別碼值 (符合開發人員中心帳戶設定中的**賣方識別碼**值)。 |
| failureName     | 字串  | 失敗的名稱，由四個部分組成：一個或多個問題類別、例外狀況/錯誤檢查碼、失敗發生位置映像/驅動程式名稱，以及相關函式名稱。             |
| failureHash     | 字串  | 錯誤的唯一識別碼。     |
| osVersion       | 字串  | 發生錯誤之 OS 的四個部分組建版本。    |
| market          | 字串  | 裝置市場的 ISO 3166 國家/地區碼。     |
| deviceType      | 字串  | 下列其中一個字串，指定發生錯誤的裝置類型：<p/><ul><li><strong>電腦</strong></li><li><strong>Phone</strong></li><li><strong>Console</strong></li><li><strong>IoT</strong></li><li><strong>Holographic</strong></li><li><strong>Unknown</strong></li></ul>     |
| moduleName     | 字串  | 與此錯誤關聯之模組的唯一名稱。      |
| moduleVersion  | 字串  | 與此錯誤關聯之模組的版本。   |
| architecture | 字串 |  發生錯誤之裝置的架構。  |
| 模型 | 字串 | 發生錯誤之裝置的型號名稱。 |
| baseboard | 字串 | 發生錯誤之裝置基板的名稱。 |
| modelFamily | 字串 | 發生錯誤之裝置型號系列的名稱。 |
| mode | 字串 | 這個值永遠都是 *kernel*。 |
| cabIdHash         | 字串  | 與此錯誤關聯之 CAB 檔案的唯一識別碼。   |
| cabType         | 字串  | CAB 檔案的類型。   |
| cabExpirationTime  | 字串  | CAB 檔案到期且無法再下載的日期和時間 (ISO 8601 格式)。   |


### <a name="response-example"></a>回應範例

下列範例針對此要求示範範例 JSON 回應主體。

```json
{
  "Value": [
    {
      "date": "2017-03-14 23:51:11",
      "sellerId": "14313740",
      "mode": "Kernel",
      "failureName": "0x109_18_2_LoadImageNotify",
      "failureHash": "f060c0b6-fbe6-463f-a9f1-b7ebc1cc722f",
      "cabIdHash": "cc1797f9-b783-44d4-b0e5-46ae7ca668bc",
      "cabType": "MINI",
      "cabExpirationTime": "2017-06-12 23:51:11",
      "osVersion": "10.0.14393.10",
      "market": "US",
      "deviceType": "PC",
      "moduleName": "Unknown_Image",
      "moduleVersion": "Unknown",
      "architecture": "x64",
      "model": "101",
      "baseboard": "750-ABX",
      "modelFamily": "ContosoPad",
      "flightRing": "Windows 10 Retail"
    }]
}
```

## <a name="related-topics"></a>相關主題

* [取得 OEM 硬體錯誤報告資料](get-oem-hardware-error-reporting-data.md)
* [下載適用於 OEM 硬體錯誤的 CAB 檔案](download-the-cab-file-for-an-oem-hardware-error.md)
