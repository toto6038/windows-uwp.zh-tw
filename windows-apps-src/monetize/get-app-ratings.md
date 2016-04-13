---
ms.assetid: DD4F6BC4-67CD-4AEF-9444-F184353B0072
description: 在 Windows 市集分析 API 中使用此方法，以針對特定日期範圍與其他選擇性篩選器，取得彙總評分資料。
title: 取得應用程式評分
---

# 取得應用程式評分


\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

在 Windows 市集分析 API 中使用此方法，以針對特定日期範圍與其他選擇性篩選器，取得彙總評分資料。 這個方法會傳回 JSON 格式的資料。

## 先決條件


若要使用這個方法，您需要進行下列動作：

-   將您會用來呼叫此方法的 Azure AD 應用程式與您的開發人員中心帳戶產生關聯。

-   取得您應用程式的 Azure AD 存取權杖。

如需詳細資訊，請參閱[使用 Windows 市集服務存取分析資料](access-analytics-data-using-windows-store-services.md)。

## 要求


### 要求的語法

| 方法 | 要求 URI                                                      |
|--------|------------------------------------------------------------------|
| GET    | https://manage.devcenter.microsoft.com/v1.0/my/analytics/ratings |

 

### 要求的標頭

| 標頭        | 類型   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | 字串 | 必要。 Azure AD 存取權杖，形式為 **Bearer** &lt;*token*&gt;。 |

 

### 要求主體

<table>
<colgroup>
<col width="25%" />
<col width="25%" />
<col width="25%" />
<col width="25%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">參數</th>
<th align="left">類型</th>
<th align="left">描述</th>
<th align="left">必要</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">applicationId</td>
<td align="left">字串</td>
<td align="left">您想要擷取評分資料的 app 產品識別碼。 產品識別碼內嵌於 app 的刊登連結，該連結可於開發人員中心儀表板的 [App identity page](https://msdn.microsoft.com/library/windows/apps/mt148561) 上取得。 舉例來說，產品識別碼可以是 9WZDNCRFJ3Q8。</td>
<td align="left">是</td>
</tr>
<tr class="even">
<td align="left">startDate</td>
<td align="left">日期</td>
<td align="left">要擷取評分資料之日期範圍的開始日期。 預設為目前的日期。</td>
<td align="left">否</td>
</tr>
<tr class="odd">
<td align="left">endDate</td>
<td align="left">日期</td>
<td align="left">要擷取評分資料之日期範圍的結束日期。 預設為目前的日期。</td>
<td align="left">否</td>
</tr>
<tr class="even">
<td align="left">top</td>
<td align="left">整數</td>
<td align="left">在要求中傳回的資料列數目。 最大值及未指定的預設值為 10000。 如果查詢中有更多資料列，回應主體將會包含您可以用來要求下一頁資料的下一頁連結。</td>
<td align="left">否</td>
</tr>
<tr class="odd">
<td align="left">skip</td>
<td align="left">整數</td>
<td align="left">在查詢中要略過的資料列數目。 使用此參數來瀏覽大型資料集。 例如，top=10000 且 skip=0 將擷取前 10000 個資料列的資料，top=10000 且 skip=10000 將擷取下 10000 個資料列的資料，以此類推。</td>
<td align="left">否</td>
</tr>
<tr class="even">
<td align="left">filter</td>
<td align="left">字串</td>
<td align="left">在回應中篩選資料列的一或多個陳述式。 如需更多資訊，請參閱下方的＜[filter fields](#filter-fields)＞一節。</td>
<td align="left">否</td>
</tr>
<tr class="odd">
<td align="left">aggregationLevel</td>
<td align="left">字串</td>
<td align="left">指定要擷取彙總資料的時間範圍。 可以是下列其中一個字串：<strong>day</strong>、<strong>week</strong> 或 <strong>month</strong>。 如果沒有指定，則預設為 <strong>day</strong>。</td>
<td align="left">否</td>
</tr>
<tr class="even">
<td align="left">orderby</td>
<td align="left">字串</td>
<td align="left">對每個評分的結果資料值做出排序的陳述式。 語法為 <em>orderby=field [order],field [order],...</em>。 <em>field</em> 參數可以是下列其中一個字串：
<ul>
<li><strong>日期</strong></li>
<li><strong>osVersion</strong></li>
<li><strong>market</strong></li>
<li><strong>deviceType</strong></li>
<li><strong>isRevised</strong></li>
</ul>
<p><em>order</em> 參數為選擇性，並可以是 <strong>asc</strong> 或 <strong>desc</strong>，以指定每個欄位的遞增或遞減順序。 預設為 <strong>asc</strong>。</p>
<p>下列為 <em>orderby</em> 字串的範例：<em>orderby=date,market</em></p></td>
<td align="left">否</td>
</tr>
</tbody>
</table>

 
### 篩選欄位

要求本體的 *filter* 參數包含在回應中篩選資料列的一或多個陳述式。 每個陳述式包含一個與 **eq** 或 **ne** 運算子關聯的欄位和值，而陳述式可以使用 **and** 或 **or** 結合。

以下為 *filter* 字串的範例：*filter=market eq 'US' and deviceType eq 'phone' and isRevised eq true*

如需支援欄位的清單，請參閱下列表格。 *filter* 參數中的字串值必須由單引號括住。

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">欄位</th>
<th align="left">描述</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">market</td>
<td align="left">內含裝置市場的 ISO 3166 國家/地區碼的字串。</td>
</tr>
<tr class="even">
<td align="left">osVersion</td>
<td align="left">下列其中一個字串：
<ul>
<li><strong>Windows Phone 7.5</strong></li>
<li><strong>Windows Phone 8</strong></li>
<li><strong>Windows Phone 8.1</strong></li>
<li><strong>Windows Phone 10</strong></li>
<li><strong>Windows 8</strong></li>
<li><strong>Windows 8.1</strong></li>
<li><strong>Windows 10</strong></li>
<li><strong>Unknown</strong></li>
</ul></td>
</tr>
<tr class="odd">
<td align="left">deviceType</td>
<td align="left">下列其中一個字串：
<ul>
<li><strong>PC</strong></li>
<li><strong>Tablet</strong></li>
<li><strong>Phone</strong></li>
<li><strong>IoT</strong></li>
<li><strong>Wearable</strong></li>
<li><strong>Server</strong></li>
<li><strong>Collaborative</strong></li>
<li><strong>Other</strong></li>
</ul></td>
</tr>
<tr class="even">
<td align="left">isRevised</td>
<td align="left">指定 <strong>true</strong> 以篩選已修訂的評分，否則請指定 <strong>false</strong>。</td>
</tr>
</tbody>
</table>

 

### 要求範例

下列範例示範取得評分資料的數個要求。 將 *applicationId* 值以您 app 的產品識別碼取代。

```
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/ratings?applicationId=9NBLGGGZ5QDR&startDate=1/1/2015&endDate=2/1/2015&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/ratings?applicationId=9NBLGGGZ5QDR&startDate=8/1/2015&endDate=8/31/2015&skip=0&filter=market eq 'US' and deviceType eq 'phone' HTTP/1.1
Authorization: Bearer <your access token>
```

## 回應


### 回應主體

| 值      | 類型   | 描述                                                                                                                                                                                                                                                                            |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 值      | array  | 內含彙總評分資料的物件陣列。 如需有關每個物件中資料的詳細資訊，請參閱下方的＜[評分數值](#rating-values)＞一節。                                                                                                                           |
| @nextLink  | 字串 | 如果還有其他資料頁面，此字串包含可以用來要求下一頁資料的 URI。 例如，如果要求的 **top** 參數被設定為 10000，但是查詢卻有超過 10000 個資料列的下載數資料，就會傳回此值。 |
| TotalCount | 整數    | 查詢之資料結果的資料列總數。                                                                                                                                                                                                                             |

 
### 評分數值

*Value* 陣列中的元素包含下列的值。

| 值           | 類型    | 描述                                                                                                                                                                                                                          |
|-----------------|---------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 日期            | 字串  | 評分資料之日期範圍中的第一個日期。 如果要求指定單一天數，此值便會是該日期。 如果要求指定一週、一個月或其他日期範圍，此值便會是該日期範圍的第一個日期。 |
| applicationId   | 字串  | 您正在擷取評分資料的 app 產品識別碼。                                                                                                                                                                 |
| applicationName | 字串  | App 的顯示名稱。                                                                                                                                                                                                         |
| market          | 字串  | 提交評分之市場的 ISO 3166 國家/地區碼。                                                                                                                                                              |
| osVersion       | 字串  | 提交評分的 OS 版本。 如需支援的字串清單，請參閱上方的＜[篩選欄位](#filter-fields)＞一節。                                                                                               |
| deviceType      | 字串  | 提交評分的裝置類型。 如需支援的字串清單，請參閱上方的＜[篩選欄位](#filter-fields)＞一節。                                                                                           |
| isRevised       | 布林值 | **true** 值表示評分已修訂，否則為 **false**。                                                                                                                                                       |
| oneStar         | 數字  | 一顆星評分的數目。                                                                                                                                                                                                      |
| twoStars        | 數字  | 兩顆星評分的數目。                                                                                                                                                                                                      |
| threeStars      | 數字  | 三顆星評分的數目。                                                                                                                                                                                                    |
| fourStars       | 數字  | 四顆星評分的數目。                                                                                                                                                                                                     |
| fiveStars       | 數字  | 五顆星評分的數目。                                                                                                                                                                                                     |

 

### 回應範例

下列範例針對此要求示範範例 JSON 回應主體。

```json
{
  "Value": [
    {
      "date": "2015-02-13",
      "applicationId": "9NBLGGGZ5QDR",
      "applicationName": "Contoso demo",
      "market": "CN",
      "osVersion": "8.0.10517.0",
      "deviceType": "Phone",
      "isRevised": false,
      "oneStar": 0,
      "twoStars": 0,
      "threeStars": 0,
      "fourStars": 0,
      "fiveStars": 2
    }
  ],
  "@nextLink": "ratings?applicationId=9NBLGGGZ5QDR&aggregationLevel=day&startDate=2015/01/01&endDate=2016/02/01&top=1&skip=1",
  "TotalCount": 15242
} 

```

## 相關主題

* [使用 Windows 市集服務存取分析資料](access-analytics-data-using-windows-store-services.md)
* [取得應用程式下載數](get-app-acquisitions.md)
* [取得 IAP 下載數](get-in-app-acquisitions.md)
* [取得錯誤報告資料](get-error-reporting-data.md)
* [取得應用程式評論](get-app-reviews.md)




<!--HONumber=Mar16_HO2-->


