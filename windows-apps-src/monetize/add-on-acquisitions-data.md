---
description: 在 Microsoft Store 分析 API 中使用此方法，以取得適用于 UWP 應用程式的 JSON 格式的匯總附加元件，以及透過 Xbox Developer 入口網站（XDP）內嵌的 Xbox One 遊戲，並可在 XDP Analytics 合作夥伴中心儀表板中找到。
title: 取得遊戲和應用程式的其他銷售資料
ms.date: 03/06/2019
ms.topic: article
keywords: Windows 10, UWP, 廣告網路, 應用程式中繼資料
ms.localizationpriority: medium
ms.openlocfilehash: 3e1349582515ce66232ea8266efc588610faae98
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/20/2020
ms.locfileid: "86493563"
---
# <a name="get-add-on-acquisitions-data-for-your-games-and-apps"></a>取得遊戲和應用程式的其他銷售資料 
在 Microsoft Store 分析 API 中使用此方法，以取得適用于 UWP 應用程式的 JSON 格式的匯總附加元件，以及透過 Xbox Developer 入口網站（XDP）內嵌的 Xbox One 遊戲，並可在 XDP Analytics 合作夥伴中心儀表板中找到。 

## <a name="prerequisites"></a>必要條件
若要使用這個方法，您必須先進行下列動作： 

* 如果您尚未這樣做，請先完成 Microsoft Store 分析 API 的所有[先決條件](access-analytics-data-using-windows-store-services.md)。 
* [取得 Azure AD 存取權杖](access-analytics-data-using-windows-store-services.md)以便用於這個方法的要求標頭。 在您取得存取權杖之後，您有 60 分鐘的使用時間，之後其便會到期。 權杖到期之後，您可以取得新的權杖。 

> [!NOTE]
> 此 API 不會在10月 2016 1 日之前提供每日匯總資料。 

## <a name="request"></a>要求

### <a name="request-syntax"></a>要求的語法
| 方法 | 要求 URI |
| --- | --- | 
| GET | `https://manage.devcenter.microsoft.com/v1.0/my/analytics/addonacquisitions` |

### <a name="request-header"></a>要求標頭 
| 頁首 | 類型 | 描述 | 
| --- | --- | --- |
| 授權 | 字串 | 必要。 Azure AD 存取權杖，格式為**持有**人 `<token>` 。 |

### <a name="request-parameters"></a>要求參數
*ApplicationId*或*addonProductId*參數是必要的。 若要擷取所有註冊至 App 之附加元件的下載數資料，請指定 *applicationId* 參數。 若要取出單一附加元件的取得資料，請指定*addonProductId*參數。 如果您同時指定上面兩個參數，*applicationId* 參數將會被忽略。 

| 參數 | 類型 | 說明 | 必要 | 
| --- | --- | --- | --- |
| applicationId | 字串 | 您要抓取取得資料的 Xbox One 遊戲的*productId* 。 若要取得您的遊戲的*productId* ，請在 XDP 分析計畫中流覽至您的遊戲，並從 URL 取出*productId* 。 或者，如果您從合作夥伴中心分析報表下載您的收購資料，則*productId*會包含在 tsv 檔案中。 | 是 |
| addonProductId | 字串 | 您想要抓取取得資料之附加元件的*productId* 。 | 是 |
| startDate | date | 要擷取附加元件下載數資料之日期範圍的開始日期。 預設值是目前的日期。 | 否 |
| endDate | date | 要擷取附加元件下載數資料之日期範圍的結束日期。 預設值是目前的日期。 | 否 |
| top | int | 在要求中傳回的資料列數目。 如果未指定，最大值和預設值為 10000。 如果查詢中有更多資料列，回應主體將會包含您可以用來要求下一頁資料的下一頁連結。 | 否 |
| skip | int | 在查詢中要略過的資料列數目。 使用此參數來瀏覽大型資料集。 例如，top=10000 且 skip=0 將擷取前 10000 個資料列的資料，top=10000 且 skip=10000 將擷取下 10000 個資料列的資料，以此類推。 | 否 |
| filter | 字串 | 一或多個篩選回應中資料列的陳述式。 每個陳述式包含一個與 eq 或 ne 運算子關聯的欄位名稱 (來自回應主體) 和值，而陳述式可以使用 and 或 or 結合。 篩選 參數中的字串值必須由單引號括住。 例如，filter=market eq 'US' and gender eq 'm'。 <br/> 您可以在回應本文中指定下列欄位： <ul><li>**acquisitionType**</li><li>**存在**</li><li>**storeClient**</li><li>**gender**</li><li>**細分**</li><li>**osVersion**</li><li>**deviceType**</li><li>**sandboxId**</li></ul> | 否 |
| aggregationLevel | 字串 | 指定要擷取彙總資料的時間範圍。 可以是下列其中一個字串：**day**、**week** 或 **month**。 如果沒有指定，則預設為 **天**。 | 否 |
| orderby | 字串 | 對每個附加元件下載數的結果資料值做出排序的陳述式。 語法為*orderby = field [order]，field [order],...**Field*參數可以是下列其中一個字串： <ul><li>**date**</li><li>**acquisitionType**</li><li>**存在**</li><li>**storeClient**</li><li>**gender**</li><li>**細分**</li><li>**osVersion**</li><li>**deviceType**</li><li>**orderName**</li></ul> order 參數為選擇性，並可以是 **asc** 或 **desc**，以指定每個欄位的遞增或遞減順序。 預設值是**asc**。 <br/> 以下是範例*orderby*字串： *orderby = date，市* | 否 |
| groupby | 字串 | 將資料彙總僅套用至指定欄位的陳述式。 您可以指定下列欄位： <ul><li>**date**</li><li>**applicationName**</li><li>**addonProductName**</li> <li>**acquisitionType**</li><li>**存在**</li> <li>**storeClient**</li><li>**gender**</li> <li>**細分**</li> <li>**osVersion**</li><li>**deviceType**</li><li>**paymentInstrumentType**</li><li>**sandboxId**</li><li>**xboxTitleIdHex**</li></ul> 傳回的資料列將包含 *groupby* 參數中指定的欄位，以及下列項目： <ul><li>**date**</li><li>**applicationId**</li><li>**addonProductId**</li><li>**acquisitionQuantity**</li></ul> groupby 參數可以搭配 *aggregationLevel* 參數使用。 例如： *&groupby = age，市&aggregationLevel = week* | 否 |

### <a name="request-example"></a>要求範例
下列範例示範數個取得附加元件下載數資料的要求。 以適用于您附加元件或應用程式的適當存放區識別碼取代*addonProductId*和*applicationId*值。 

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/addonacquisitions?applicationId=9WZDNCRFJ314&startDate=1/1/2015&endDate=2/1/2015&skip=0 HTTP/1.1 

Authorization: Bearer <your access token> 

 

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/addonacquisitions?applicationId=9WZDNCRFJ314&startDate=1/1/2015&endDate=2/1/2015&skip=0&filter=market eq 'GB' and gender eq 'm' HTTP/1.1 

Authorization: Bearer <your access token>
```

## <a name="response"></a>回應

### <a name="response-body"></a>回應本文

| 值 | 類型 | 描述 |
| --- | --- | --- |
| 值 | array | 內含彙總附加元件下載數資料的物件陣列。 如需有關每個物件中資料的詳細資訊，請參閱下方的[附加元件下載數數值](#add-on-acquisition-values)一節。 |
| @nextLink | 字串 | 如果還有其他資料頁面，此字串包含可以用來要求下一頁資料的 URI。 例如，如果要求的 **top** 參數被設定為 10000，但是查詢卻有超過 10000 個資料列的附加元件下載數資料，就會傳回此值。 |
| TotalCount | int | 查詢之資料結果的資料列總數。 |

### <a name="add-on-acquisition-values"></a>附加元件下載數數值
Value 陣列中的元素包含下列值。

| 值 | 類型 | 描述 | 
| --- | --- | --- |
| date | 字串 | 下載數資料之日期範圍中的第一個日期。 如果要求指定單一天數，此值便會是該日期。 如果要求指定一週、一個月或其他日期範圍，此值便會是該日期範圍的第一個日期。 |
| addonProductId | 字串 | 您要抓取取得資料之附加元件的*productId* 。 |
| addonProductName | 字串 | 附加元件的顯示名稱。 只有在 [ *aggregationLevel* ] 參數設定為 [**日**] 時，此值才會出現在回應資料中，除非您在*groupby*參數中指定**addonProductName**欄位。 |
| applicationId | 字串 | 您想要抓取附加元件取得資料之應用程式的*productId* 。 |
| applicationName | 字串 | 遊戲的顯示名稱。 |
| deviceType | 字串 | 以下字串之一指定完成收購的裝置類型： <ul><li>PC</li><li>來電</li><li>「主控台-Xbox One」</li><li>「主控台-Xbox 系列 X」</li><li>IoT</li><li>伺服器</li><li>適合</li><li>全息</li><li>未知</li></ul> |
| storeClient | 字串 | 下列其中一個表示收購發生的 Microsoft Store 版本之字串： <ul><li>"Windows Phone Store （用戶端）"</li><li>「Microsoft Store （用戶端）」（或「Windows Store （用戶端）」，如果是在2018年3月23日之前查詢資料）</li><li>"Microsoft Store （web）" （或 "Windows Store （web）"）（如果在2018年3月23日之前查詢資料）</li><li>「依組織的大量採購」</li><li>另一方面</li></ul> |
| osVersion | 字串 | 發生下載的 OS 版本。 對於此方法，此值一律為 "Windows 10"。 |
| market | 字串 | 發生下載之市場的 ISO 3166 國家/地區碼。 |
| gender | 字串 | 下列其中一個字串，這些字串詳列進行收購的使用者的性別： <ul><li>"m"</li><li>"f"</li><li>未知</li></ul> |
| age | 字串 | 下列其中一個字串，這些字串表示進行下載的使用者的年齡層： <ul><li>「小於13」</li><li>"13-17"</li><li>"18-24"</li><li>"25-34"</li><li>"35-44"</li><li>"44-55"</li><li>「大於55」</li><li>未知</li></ul> |
| acquisitionType | 字串 | 其中一個下列字串，可指出收購類型： <ul><li>受 </li><li>試用 </li><li>付費</li><li>「促銷代碼」 </li><li>Iap</li><li>「訂用帳戶 Iap」</li><li>「私用物件」</li><li>「前置訂單」</li><li>「Xbox 遊戲通過」（或「遊戲通過」，如果是在2018年3月23日前查詢資料）</li><li>硬碟</li><li>「預付碼」</li><li>「按訂單計費」</li><li>「取消的前置順序」</li><li>「失敗的前置順序」</li></ul> |
| acquisitionQuantity | integer | 發生的下載數目。 |
| inAppProductId | 字串 | 使用此附加元件之產品的產品識別碼。  |
| inAppProductName | 字串 | 使用此附加元件之產品的產品名稱。  |
| paymentInstrumentType | 字串 | 用於取得的付款檢測類型。  |
| sandboxId | 字串 | 針對遊戲所建立的沙箱識別碼。 這可以是**零售**或私用沙箱識別碼的值。  |
| xboxTitleId | 字串 | 來自 XDP 之產品的 Xbox 標題識別碼（如果適用）。  |
| localCurrencyCode | 字串 | 以合作夥伴中心帳戶的國家（地區）為基礎的當地貨幣代碼。  |
| xboxProductId | 字串 | 來自 XDP 之產品的 Xbox 產品識別碼（如果適用）。  |
| availabilityId | 字串 | 來自 XDP 之產品的可用性識別碼（如果適用）。  |
| skuId | 字串 | 來自 XDP 之產品的 SKU 識別碼（如果適用）。  |
| skuDisplayName | 字串 | XDP 中產品的 SKU 顯示名稱（如果適用）。  |
| xboxParentProductId | 字串 | 來自 XDP 之產品的 Xbox 父系產品識別碼（如果適用）。  |
| parentProductName | 字串 | 來自 XDP 之產品的父系產品名稱（如果適用）。  |
| productTypeName | 字串 | 來自 XDP 之產品的產品類型名稱（如果適用）。  |
| purchaseTaxType | 字串 | 從 XDP 購買產品的稅務類型（如果適用）。  |
| purchasePriceUSDAmount | number | 客戶針對附加元件所支付的金額，轉換為美元。  |
| purchasePriceLocalAmount | number | 套用至附加元件的稅額。  |
| purchaseTaxUSDAmount | number | 套用至附加元件的稅額，轉換為美元。  |
| purchaseTaxLocalAmount | number | 從 XDP 購買產品的當地金額（如果適用的話）。  |

### <a name="response-example"></a>回應範例
下列範例針對此要求示範範例 JSON 回應主體。 

```JSON
{ 
  "Value": [ 
    { 
            "inAppProductId": "9NBLGGH1864K", 
            "inAppProductName": "866879", 
            "addonProductId": "9NBLGGH1864K", 
            "addonProductName": "866879", 
            "date": "2017-11-05", 
            "applicationId": "9WZDNCRFJ314", 
            "applicationName": "Tetris Blitz", 
            "acquisitionType": "Iap", 
            "age": "35-49", 
            "deviceType": "Phone", 
            "gender": "m", 
            "market": "US", 
            "osVersion": "Windows Phone 8.1", 
            "paymentInstrumentType": "Credit Card", 
            "sandboxId": "RETAIL", 
            "storeClient": "Windows Phone Store (client)", 
            "xboxTitleId": "", 
            "localCurrencyCode": "USD", 
            "xboxProductId": "00000000-0000-0000-0000-000000000000", 
            "availabilityId": "", 
            "skuId": "", 
            "skuDisplayName": "Full", 
            "xboxParentProductId": "", 
            "parentProductName": "Tetris Blitz", 
            "productTypeName": "Add-On", 
            "purchaseTaxType": "", 
            "acquisitionQuantity": 1, 
            "purchasePriceUSDAmount": 1.08, 
            "purchasePriceLocalAmount": 0.09, 
            "purchaseTaxUSDAmount": 1.08, 
            "purchaseTaxLocalAmount": 0.09 
        } 
    ], 

    "@nextLink": null, 
    
    "TotalCount": 7601 
} 
```