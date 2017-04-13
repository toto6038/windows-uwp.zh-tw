---
author: mcleanbyron
ms.assetid: 2FBA0B73-17C6-4F25-A79D-63F2F262491A
description: "在 Windows 市集分析 API 中使用此方法，以取得 Windows 7 和 Windows 8.x 驅動程式的詳細資料。 這個方法僅供 IHV 使用。"
title: "取得 Windows 7 或 Windows 8.x 驅動程式錯誤的詳細資料"
ms.author: mcleans
ms.date: 03/17/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Windows 10, UWP, 市集服務, Windows 市集分析 API, 錯誤, 詳細資料"
ms.openlocfilehash: fbf1ee456080ff228abde061dbfef859baf79465
ms.sourcegitcommit: 64cfb79fd27b09d49df99e8c9c46792c884593a7
translationtype: HT
---
# <a name="get-details-for-a-windows-7-or-windows-8x-driver-error"></a>取得 Windows 7 或 Windows 8.x 驅動程式錯誤的詳細資料

在 Windows 市集分析 API 中使用此方法，以取得特定 Windows 7/Windows 8.x 驅動程式錯誤的詳細資料 (JSON 格式)。 使用這個方法之前，您必須先使用[取得 Windows 7 和 Windows 8.x 驅動程式的錯誤報告資料](get-error-reporting-data-for-windows-7-and-windows-8.x-drivers.md)方法，來擷取您要取得詳細資訊之錯誤的識別碼。

> [!NOTE]
> 此方法僅供隸屬於 [Windows 硬體開發人員中心計畫](https://msdn.microsoft.com/windows/hardware/drivers/dashboard/get-started-with-the-hardware-dashboard)的開發人員帳戶使用。

## <a name="prerequisites"></a>必要條件


若要使用這個方法，您必須先進行下列動作：

* 如果您尚未這樣做，請先完成 Windows 市集分析 API 的所有[先決條件](access-analytics-data-using-windows-store-services.md#prerequisites)。
* [取得 Azure AD 存取權杖](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token)以便用於這個方法的要求標頭。 在您取得存取權杖之後，您在權杖到期之前有 60 分鐘的時間可以使用權杖。 權杖到期之後，您可以取得新的權杖。
* 取得您想要取得詳細資訊之錯誤的識別碼。 若要取得此識別碼，請使用[取得 Windows 7 和 Windows 8.x 驅動程式的錯誤報告資料](get-error-reporting-data-for-windows-7-and-windows-8.x-drivers.md)方法，並使用該方法回應主體中的 **failureHash** 值。

## <a name="request"></a>要求


### <a name="request-syntax"></a>要求的語法

| 方法 | 要求 URI                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/ihvdriver/failuredetails``` |

<span/> 

### <a name="request-header"></a>要求的標頭

| 標頭        | 類型   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | 字串 | 必要。 Azure AD 存取權杖，形式為 **Bearer** &lt;*token*&gt;。 |

<span/> 

### <a name="request-parameters"></a>要求參數

| 參數        | 類型   |  描述      |  必要  
|---------------|--------|---------------|------|
| failureHash | 字串 | 您想要取得詳細資訊之錯誤的唯一識別碼。 若要取得您有興趣之錯誤的此值，請使用[取得 OEM 硬體錯誤報告資料](get-oem-hardware-error-reporting-data.md)方法，並並使用該方法回應主體中的 **failureHash** 值。 |  是  |
| startDate | 日期 | 要擷取詳細錯誤資料之日期範圍的開始日期。 預設為目前日期的前 30 天。 |  否  |
| endDate | 日期 | 要擷取詳細錯誤資料之日期範圍的結束日期。 預設為目前的日期。 |  否  |
| top | 整數 | 在要求中傳回的資料列數目。 最大值及未指定的預設值為 10000。 如果查詢中有更多資料列，回應主體將會包含您可以用來要求下一頁資料的下一頁連結。 |  否  |
| skip | 整數 | 在查詢中要略過的資料列數目。 使用此參數來瀏覽大型資料集。 例如，top=10 且 skip=0 將擷取前 10 個資料列的資料，top=10 且 skip=10 將擷取下 10 個資料列的資料，以此類推。 |  否  |
| filter | 字串  | 一或多個篩選回應中資料列的陳述式。 每個陳述式包含一個與 **eq** 或 **ne** 運算子關聯的欄位名稱 (來自回應主體) 和值，而陳述式可以使用 **and** 或 **or** 結合。 *filter* 參數中的字串值必須由單引號括住。 您可以指定來自回應主體的下列欄位：<p/><ul><li><strong>date</strong></li><li><strong>failureName</strong></li><li><strong>failureHash</strong></li><li><strong>symbol</strong></li><li><strong>osVersion</strong></li><li><strong>osName</strong></li><li><strong>eventType</strong></li><li><strong>market</strong></li><li><strong>deviceType</strong></li><li><strong>driverName</strong></li><li><strong>driverVersion</strong></li><li><strong>oemName</strong></li><li><strong>oemModel</strong></li><li><strong>flightRing</strong></li><li><strong>architecture</strong></li><li><strong>cabType</strong></li><li><strong>cabExpirationTime</strong></li></ul> | 否   |
| orderby | 字串 | 將結果資料值排序的陳述式。 語法為 <em>orderby=field [order],field [order],...</em>。 您可以指定來自回應主體的下列欄位：<p/><ul><li><strong>failureName</strong></li><li><strong>failureHash</strong></li><li><strong>cabType</strong></li><li><strong>cabExpirationTime</strong></li><li><strong>symbol</strong></li><li><strong>osVersion</strong></li><li><strong>osName</strong></li><li><strong>eventType</strong></li><li><strong>market</strong></li><li><strong>deviceType</strong></li><li><strong>driverName</strong></li><li><strong>driverVersion</strong></li><li><strong>oemName</strong></li><li><strong>oemModel</strong></li><li><strong>flightRing</strong></li><li><strong>architecture</strong></li></ul><p><em>order</em> 參數為選擇性，並可以是 <strong>asc</strong> 或 <strong>desc</strong>，用來指定每個欄位的遞增或遞減順序。 預設為 <strong>asc</strong>。</p><p>下列為 <em>orderby</em> 字串的範例：<em>orderby=date,market</em></p> |  否  |

<span/> 

### <a name="request-example"></a>要求範例

下列範例示範取得詳細錯誤資料的數個要求。

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/ihvdriver/failuredetails?failureHash=012e33e3-dbc9-b12f-c124-9d9810f05d8b&startDate=2016-11-05&endDate=2016-11-06&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/ihvdriver/failuredetails?failureHash=012e33e3-dbc9-b12f-c124-9d9810f05d8b&startDate=2016-11-05&endDate=2016-11-06&top=10&skip=0&filter=market eq 'US' and deviceType eq 'PC' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>回應


### <a name="response-body"></a>回應主體

| 值      | 類型    | 描述    |
|------------|---------|------------|
| 值      | 陣列   | 包含詳細錯誤資料的物件陣列。 如需有關每個物件中資料的詳細資訊，請參閱下表。          |
| @nextLink  | 字串  | 如果還有其他資料頁面，此字串包含可以用來要求下一頁資料的 URI。 例如，如果要求的 **top** 參數被設定為 10，但是查詢卻有超過 10 個資料列的錯誤，就會傳回此值。 |
| TotalCount | inumber | 查詢之資料結果的資料列總數。        |

<span/>

*Value* 陣列中的元素包含下列值。

| 值           | 類型    | 描述     |
|-----------------|---------|----------------------------|
| 日期            | 字串  | 錯誤資料之日期範圍中的第一個日期。 如果要求指定單一天數，此值便會是該日期。 如果要求指定一週、一個月或其他日期範圍，此值便會是該日期範圍的第一個日期。 |
| sellerId   | 字串  | 與開發人員帳戶相關聯的賣方識別碼值 (符合開發人員中心帳戶設定中的**賣方識別碼**值)。 |
| failureName     | 字串  | 錯誤的名稱。 此名稱與「Windows 開發人員中心」儀表板中[健康情況報告](../publish/health-report.md)的 **\[失敗\]** 區段中顯示的名稱相同。            |
| failureHash     | 字串  | 錯誤的唯一識別碼。     |
| symbol     | 字串  | 指派給此錯誤的符號。     |
| osVersion       | 字串  | 發生錯誤之 OS 的四個部分組建版本。    |
| osName       | 字串  | 發生錯誤之 OS 的名稱。  |
| eventType       | 字串  | 發生錯誤的類型。      |
| market          | 字串  | 裝置市場的 ISO 3166 國家/地區碼。     |
| deviceType      | 字串  | 發生錯誤的裝置類型。     |
| driverName     | 字串  | 與此錯誤關聯之驅動程式的唯一名稱。      |
| driverVersion  | 字串  | 與此錯誤關聯之驅動程式的版本。   |
| architecture | 字串 |  發生錯誤之裝置的架構。  |
| oemName | 字串 | 發生錯誤之裝置的 OEM 名稱。 |
| oemModel | 字串 | 發生錯誤之裝置的型號名稱。 |
| flightRing | 字串 | 發生錯誤之作業系統正式發行前小眾測試版的名稱。 |
| clientDeviceId | 字串 | 發生錯誤的裝置識別碼。 |
| cabIdHash         | 字串  | 與此錯誤關聯之 CAB 檔案的唯一識別碼。   |
| cabType         | 字串  | CAB 檔案的類型。   |
| cabExpirationTime  | 字串  | CAB 檔案到期且無法再下載的日期和時間 (ISO 8601 格式)。   |

<span/> 

### <a name="response-example"></a>回應範例

下列範例針對此要求示範範例 JSON 回應主體。

```json
{
  "Value": [
    {
      "sellerId": "14313740",
      "architecture": "x64",
      "cabExpirationTime": "2017-06-12 23:51:11",
      "cabIdHash": "cc1797f9-b783-44d4-b0e5-46ae7ca668bc",
      "cabType": "MINI",
      "clientDeviceId": null,
      "date": "2017-03-14 23:51:11",
      "deviceType": "PC",
      "driverName": "fastboot.sys",
      "driverVersion": "2.1.1.0",
      "failureHash": "f060c0b6-fbe6-463f-a9f1-b7ebc1cc722f",
      "failureName": "0x109_18_2_LoadImageNotify",
      "market": "US",
      "oemModel": "C-122",
      "oemName": "Contoso",
      "osName": "Windows 8.1",
      "osVersion": "6.3.9600.9600"
    }]
}
```

## <a name="related-topics"></a>相關主題

* [取得 Windows 7 和 Windows 8.x 驅動程式的錯誤報告資料](get-error-reporting-data-for-windows-7-and-windows-8.x-drivers.md)
* [下載適用於 Windows 7 或 Windows 8.x 驅動程式錯誤的 CAB 檔案](download-the-cab-file-for-a-windows-7-or-windows-8.x-driver-error.md)
