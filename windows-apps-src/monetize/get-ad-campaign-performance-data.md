---
author: mcleanbyron
ms.assetid: A26A287C-B4B0-49E9-BB28-6F02472AE1BA
description: "使用「Windows 市集分析 API」中的這個方法，以針對特定日期範圍及其他選擇性篩選，取得所指定應用程式的彙總廣告行銷活動績效資料。"
title: "取得廣告活動績效資料"
translationtype: Human Translation
ms.sourcegitcommit: 67845c76448ed13fd458cb3ee9eb2b75430faade
ms.openlocfilehash: 8859658675d31deccc3403e4862c47a54ca25b1a

---

# 取得廣告活動績效資料


使用「Windows 市集分析 API」中的這個方法，以針對特定日期範圍及其他選擇性篩選，取得您應用程式廣告行銷活動績效資料的彙總摘要。 這個方法會傳回 JSON 格式的資料。

這個方法所傳回的資料與「Windows 開發人員中心」儀表板上的 [App 安裝廣告報告](../publish/app-install-ads-reports.md)所提供的資料相同。 如需有關廣告行銷活動的詳細資訊，請參閱[為您的 App 建立廣告行銷活動](https://msdn.microsoft.com/windows/uwp/publish/create-an-ad-campaign-for-your-app)。

若要擷取已為您「開發人員中心」帳戶建立的所有廣告行銷活動的完整詳細資料，您可以使用本文稍後所述的[取得所有廣告行銷活動的詳細資料方法](#get-details-for-all-ad-campaigns)。

## 先決條件


若要使用這個方法，您必須先進行下列動作：

* 如果您尚未這樣做，請先完成 Windows 市集分析 API 的所有[先決條件](access-analytics-data-using-windows-store-services.md#prerequisites)。
* [取得 Azure AD 存取權杖](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token)以便用於這個方法的要求標頭。 在您取得存取權杖之後，您在權杖到期之前有 60 分鐘的時間可以使用權杖。 權杖到期之後，您可以取得新的權杖。

## 要求


### 要求的語法

| 方法 | 要求 URI                                                              |
|--------|--------------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/promotion``` |

<span />

### 要求的標頭

| 標頭        | 類型   | 描述                |
|---------------|--------|---------------|
| Authorization | 字串 | 必要。 Azure AD 存取權杖，形式為 **Bearer** &lt;*token*&gt;。 |

<span />

### 要求參數

若要擷取特定 App 的廣告行銷活動績效資料，請使用 *applicationId* 參數。 若要擷取與您開發人員帳戶關聯之所有 App 的廣告績效資料，請省略 *applicationId* 參數。

| 參數     | 類型   | 描述     | 必要 |
|---------------|--------|-----------------|----------|
| applicationId   | 字串    | 您想要擷取廣告行銷活動績效資料之 App 的「市集識別碼」。 從「開發人員中心」儀表板的 [App 身分識別](../publish/view-app-identity-details.md)頁面即可取得「市集識別碼」。 範例「市集識別碼」如 9NBLGGH4R315。 |    否      |
|  startDate  |  日期   |  要擷取的廣告行銷活動績效資料之日期範圍的開始日期，格式為 YYYY/MM/DD。 預設為目前的日期減去 30 天。   |   否    |
| endDate   |  日期   |  要擷取的廣告行銷活動績效資料之日期範圍的結束日期，格式為 YYYY/MM/DD。 預設為目前的日期減去一天。   |   否    |
| top   |  整數   |  在要求中傳回的資料列數目。 最大值及未指定的預設值為 10000。 如果查詢中有更多資料列，回應主體將會包含您可以用來要求下一頁資料的下一頁連結。   |   否    |
| skip   | 整數    |  在查詢中要略過的資料列數目。 使用此參數來瀏覽大型資料集。 例如，top=10000 且 skip=0 將擷取前 10000 個資料列的資料，top=10000 且 skip=10000 將擷取下 10000 個資料列的資料，以此類推。   |   否    |
| filter   |  字串   |  一或多個篩選回應中資料列的陳述式。 目前唯一支援的篩選是 **campaignId**。 每個陳述式都可以使用 **eq** 或 **ne** 運算子，而陳述式之間可以使用 **and** 或 **or** 來結合。  以下是一個範例 *filter* 參數：```filter=campaignId eq '100023'```。   |   否    |
|  aggregationLevel  |  字串   | 指定要擷取彙總資料的時間範圍。 可以是下列其中一個字串：<strong>day</strong>、<strong>week</strong> 或 <strong>month</strong>。 如果沒有指定，則預設為 <strong>day</strong>。    |   否    |
| orderby   |  字串   |  <p>將廣告行銷活動績效資料的結果資料值排序的陳述式。 語法為 <em>orderby=field [order],field [order],...</em>。 <em>field</em> 參數可以是下列其中一個字串：</p><ul><li><strong>date</strong></li><li><strong>campaignId</strong></li></ul><p><em>order</em> 參數為選擇性，並可以是 <strong>asc</strong> 或 <strong>desc</strong>，用來指定每個欄位的遞增或遞減順序。 預設為 <strong>asc</strong>。</p><p>以下是一個範例 <em>orderby</em> 字串：<em>orderby=date,campaignId</em></p>   |   否    |
|  groupby  |  字串   |  <p>將資料彙總僅套用至指定欄位的陳述式。 您可以指定下列欄位：</p><ul><li><strong>campaignId</strong></li><li><strong>applicationId</strong></li><li><strong>date</strong></li><li><strong>currencyCode</strong></li></ul><p><em>groupby</em> 參數可以搭配 <em>aggregationLevel</em> 參數使用。 例如：<em>&amp;groupby=applicationId&amp;aggregationLevel=week</em></p>   |   否    |


<span />
 

### 要求範例

下列範例示範數個取得廣告行銷活動績效資料的要求。

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/promotion?aggregationLevel=week&groupby=applicationId,campaignId,date  HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/promotion?applicationId=9NBLGGH0XK8Z&startDate=2015/1/20&endDate=2016/8/31&skip=0&filter=campaignId eq '31007388' HTTP/1.1
Authorization: Bearer <your access token>
```

## 回應


### 回應主體

| 值      | 類型   | 描述                                                                                                                                                                                                                                                                            |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 值      | 陣列  | 內含彙總廣告行銷活動績效資料的物件陣列。 如需有關每個物件中資料的詳細資訊，請參閱下方的[行銷活動績效物件](#campaign-performance-object)一節。                                                                                                                      |
| @nextLink  | 字串 | 如果還有其他資料頁面，此字串會包含可以用來要求下一頁資料的 URI。 例如，如果要求的 **top** 參數被設定為 5，但是查詢有超過 5 個資料項目，就會傳回此值。 |
| TotalCount | 整數    | 查詢之資料結果的資料列總數。                                                                                                                                                                                                                             |

<span id="campaign-performance-object" />
### 行銷活動績效物件

*Value* 陣列中的元素包含下列值。

| 值               | 類型   | 描述            |
|---------------------|--------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 日期                | 字串 | 廣告行銷活動績效資料之日期範圍中的第一個日期。 如果要求指定單一天數，此值便會是該日期。 如果要求指定一週、一個月或其他日期範圍，此值便會是該日期範圍的第一個日期。 |
| applicationId       | 字串 | 您要擷取廣告行銷活動績效資料之 App 的「市集識別碼」。                     |
| campaignId     | 字串 | 廣告行銷活動的識別碼。           |
| currencyCode              | 字串 | 行銷活動預算的貨幣代碼。              |
| spend          | 字串 |  已花費的廣告行銷活動預算總金額。     |
| impressions           | 長整數 | 行銷活動的廣告曝光次數。        |
| installs              | 長整數 | 與行銷活動相關的 App 安裝次數。   |
| clicks            | 長整數 | 行銷活動的廣告點按次數。      |

<span />

### 回應範例

下列範例示範這個要求的一個範例 JSON 回應主體。

```json
{
  "Value": [
    {
      "date": "2015-04-12",
      "applicationId": "9WZDNCRFJ31Q",
      "campaignId": "4568",
      "currencyCode": "USD",
      "spend": 700.6,
      "impressions": 200,
      "installs": 30,
      "clicks": 8
    },
    {
      "date": "2015-05-12",
      "applicationId": "9WZDNCRFJ31Q",
      "campaignId": "1234",
      "currencyCode": "USD",
      "spend": 325.3,
      "impressions": 20,
      "installs": 2,
      "clicks": 5
    }
  ],
  "@nextLink": "promotion?applicationId=9NBLGGGZ5QDR&aggregationLevel=day& startDate=2015/1/20&endDate=2016/8/31&top=2&skip=2",
  "TotalCount": 1917
}
```

<span id="get-details-for-all-ad-campaigns" />
## 取得所有廣告行銷活動的詳細資料


使用這個方法來針對已向您「Windows 開發人員中心」帳戶登錄的 App，取得為其建立之所有廣告行銷活動的詳細資料。 這個方法會傳回 JSON 格式的資料。 如需有關廣告行銷活動的詳細資訊，請參閱[為您的 App 建立廣告行銷活動](https://msdn.microsoft.com/windows/uwp/publish/create-an-ad-campaign-for-your-app)。

### 先決條件

若要使用這個方法，您必須先進行下列動作：

* 如果您尚未這樣做，請先完成 Windows 市集分析 API 的所有[先決條件](access-analytics-data-using-windows-store-services.md#prerequisites)。
* [取得 Azure AD 存取權杖](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token)以便用於這個方法的要求標頭。 在您取得存取權杖之後，您在權杖到期之前有 60 分鐘的時間可以使用權杖。 權杖到期之後，您可以取得新的權杖。

### 要求


#### 要求的語法

| 方法 | 要求 URI                                                              |
|--------|--------------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/adscampaign``` |

<span />

#### 要求的標頭

| 標頭        | 類型   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | 字串 | 必要。 Azure AD 存取權杖，形式為 **Bearer** &lt;*token*&gt;。 |

<span />

#### 要求參數

| 參數     | 類型   | 描述     | 必要 |
|---------------|--------|-----------------|----------|
| top           | 整數    | 要在要求中傳回的資料列數目。 最大值及預設值 (如果未指定) 為 1000。 如果查詢中有更多資料列，回應主體將會包含您可以用來要求下一頁資料的下一頁連結。 |    否      |
| skip           | 整數    | 在查詢中要略過的資料列數目。 使用此參數來循頁瀏覽大型資料集。 例如，top=100 且 skip=0 時，會擷取前 100 列資料，top=100 且 skip=100 時，會擷取下 100 列資料，以此類推。 |    否      |


<span />
 

#### 要求範例

下列範例示範數個取得您所有廣告行銷活動之詳細資料的要求。

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/promotion/adscampaign?top=10&skip=0  HTTP/1.1
Authorization: Bearer <your access token>
```

### 回應


#### 回應主體

| 值      | 類型   | 描述  |
|------------|--------|--------------|
| 值      | 陣列  | 內含您廣告行銷活動詳細資料的物件陣列。 如需有關每個物件中資料的詳細資訊，請參閱下方的[行銷活動物件](#campaign-object)一節。                        |
| @nextLink  | 字串 | 如果還有其他資料頁面，此字串會包含可以用來要求下一頁資料的 URI。 例如，如果要求的 **top** 參數被設定為 5，但是查詢有超過 5 個資料項目，就會傳回此值。 |                                   |

<span id="campaign-object" />
#### 行銷活動物件

*Value* 陣列中的元素包含下列值。

| 值               | 類型   | 描述          |
|---------------------|--------|------------------|
| id     | 字串 | 廣告行銷活動的識別碼。 |
| name     | 字串 | 廣告行銷活動的名稱。 |
| createDate     | 字串 | 廣告行銷活動的建立日期，採用 UTC 時區。 |
| startDate     | 字串 | 廣告行銷活動的開始日期，採用 UTC 時區。 |
| endDate     | 字串 | 廣告行銷活動的結束日期，採用 UTC 時區。 |
| applicationId       | 字串 | 廣告行銷活動所關聯之 App 的「市集識別碼」。 從「開發人員中心」儀表板的 [App 身分識別](../publish/view-app-identity-details.md)頁面即可取得「市集識別碼」。 範例「市集識別碼」如 9NBLGGH4R315。                    |
| budget     | 數字 | 廣告行銷活動的預算。           |
| budgetType    | 字串 | 廣告行銷活動的預算類型。 這可以是下列其中一個字串：**Monthly** 或 **Total**。           |
| currencyCode              | 字串 | 行銷活動預算的貨幣代碼。              |
| type              | 字串 | 廣告行銷活動類型。 這可以是下列其中一個字串：**Paid**、**House** 或 **Community**。             |
| status              | 字串 | 廣告行銷活動的狀態。 這可以是下列其中一個字串：**Active**、**UserPaused**、**Pending**、**Ended** 或 **SystemPaused**。             |
| targetingType              | 字串 | 廣告行銷活動的目標鎖定類型。 這可以是下列其中一個字串：**Auto**、**Manual** 或 **Segment**。             |
| target              | 字典 | 內含行銷活動目標鎖定資訊之機碼和值組的字典。 如需有關此物件的詳細資訊，請參閱下方的[目標物件](#target-object)一節。             |

<span />

<span id="target-object" />
#### 目標物件

此資源是下列機碼和值組的字典。

| 機碼               | 類型   | 值          |
|---------------------|--------|------------------|
| Country     | 陣列 |   一或多個內含廣告行銷活動目標國家或地區之 ISO 3166 Alpha-2 代碼的字串。 |
| OsVersion     | 陣列 | 下列一或多個指定廣告行銷活動目標 OS 版本的字串：**10** 或 **8.x**。  |
| Age     |  陣列 |  下列一或多個指定目標年齡範圍的字串：**Age13To17**、**Age18To24**、**Age25To34**、**Age35To49** 或 **Age50AndAbove**。 |
| Gender     |  陣列 |  下列一或多個指定目標性別的字串：**Female** 或 **Male**。 |
| DeviceType       |  陣列  |  下列一或多個指定目標裝置類型的字串：**PC/Tablet** 或 **Phone**。                  |
| Segment     |  陣列 |    一或多個內含目標客戶區隔識別碼的字串。       |


<span />

#### 回應範例

下列範例針對此要求示範範例 JSON 回應主體。

```json
{
   "Value": [
      {
         "id":31010026,
         "name": "Contoso App 1 Campaign",
         "createDate":"2016-07-14T10:01:21Z",
         "startDate":"2016-07-21T11:23:36Z",
         "applicationId":"9NBLGGH0XK8Z",
         "budget": 100,
         "budgetType":"Monthly",
         "currencyCode": "USD",
         "type": "Paid",
         "status": "Active",
         "targetingType": "Auto",
         "target": null
      },
      {
         "id":31010025,
         "name":"Contoso App 2 Campaign",
         "createDate":"2016-07-14T10:02:21Z",
         "startDate":"2016-07-21T11:18:44Z",
         "endDate":"2016-08-21T11:18:44Z",
         "applicationId":"9WZDNCRDX48S",
         "budget": 50,
         "budgetType":"Total",
         "currencyCode": "USD",
         "type": "Paid",
         "status": "Ended",
         "targetingType": "Manual",
         "target": {
            "Country": [
               "US",
               "BR",
               "FR",
               "IN",
               "IT"
            ],
            "OsVersion": [
               "8.X"
            ],
            "Age": [
               "Age18To24",
               "Age25To34",
               "Age35To49",
               "Age50AndAbove"
            ],
            "Gender": [
               "Male"
            ],
            "DeviceType": [
               "PC/Tablet",
               "Phone"
            ]
         }
      }
   ]
}
```

## 相關主題

* [App 安裝廣告報告](../publish/app-install-ads-reports.md)
* [為您的 App 建立廣告行銷活動](https://msdn.microsoft.com/windows/uwp/publish/create-an-ad-campaign-for-your-app)
* [使用 Windows 市集服務存取分析資料](access-analytics-data-using-windows-store-services.md)



<!--HONumber=Nov16_HO1-->


