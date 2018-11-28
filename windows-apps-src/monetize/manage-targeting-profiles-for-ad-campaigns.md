---
ms.assetid: d305746a-d370-4404-8cde-c85765bf3578
description: 在 Microsoft Store 促銷 API 中使用此方法，管理適用於廣告行銷活動的目標設定檔。
title: 管理目標設定檔
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store 促銷 API, 廣告行銷活動
ms.localizationpriority: medium
ms.openlocfilehash: 0d84c6eb678bf884709e13ecefd81e64097ee738
ms.sourcegitcommit: b11f305dbf7649c4b68550b666487c77ea30d98f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/27/2018
ms.locfileid: "7839023"
---
# <a name="manage-targeting-profiles"></a>管理目標設定檔


在 Microsoft Store 促銷 API 使用這些方法，選取使用者、地區，以及您想要在廣告行銷活動中針對每個播送行設立目標的庫存廣告類型。 可以跨多個播送行建立並重複使用目標設定檔。

如需有關目標設定檔、廣告行銷活動、播送行和廣告素材之間關聯性的詳細資訊，請參閱[使用 Microsoft Store 服務執行廣告行銷活動](run-ad-campaigns-using-windows-store-services.md#call-the-windows-store-promotions-api)。

## <a name="prerequisites"></a>必要條件

若要使用這些方法，您必須先進行下列動作：

* 如果您尚未完成，請先完成 Microsoft Store 促銷交 API 的所有[先決條件](run-ad-campaigns-using-windows-store-services.md#prerequisites)。
* [取得 Azure AD 存取權杖](run-ad-campaigns-using-windows-store-services.md#obtain-an-azure-ad-access-token)以便用於這些方法的要求標頭。 在您取得存取權杖之後，您在權杖到期之前有 60 分鐘的時間可以使用權杖。 權杖到期之後，您可以取得新的權杖。

## <a name="request"></a>要求

這些方法具有下列 URI。

| 方法類型 | 要求 URI                                                      |  描述  |
|--------|------------------------------------------------------------------|---------------|
| POST   | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/targeting-profile``` |  建立新的目標設定檔。  |
| PUT    | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/targeting-profile/{targetingProfileId}``` |  編輯 *targetingProfileId* 指定的目標設定檔。  |
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/targeting-profile/{targetingProfileId}``` |  取得 *targetingProfileId* 指定的目標設定檔。  |


### <a name="header"></a>標頭

| 標頭        | 類型   | 描述         |
|---------------|--------|---------------------|
| 授權 | 字串 | 必要。 Azure AD 存取權杖，形式為 **Bearer** &lt;*token*&gt;。 |
| 追蹤識別碼   | GUID   | 選用。 追蹤呼叫流程的識別碼。                                  |


### <a name="request-body"></a>要求本文

POST 和 PUT 方法需要 JSON 要求本文及所需欄位[目標設定檔](#targeting-profile)物件，以及您想要設定或變更的任何其他欄位。


### <a name="request-examples"></a>要求範例

以下範例示範如何呼叫 POST 方法，以建立目標設定檔。

```json
POST https://manage.devcenter.microsoft.com/v1.0/my/promotion/targeting-profile HTTP/1.1
Authorization: Bearer <your access token>

{
    "name": "Contoso App Campaign - Targeting Profile 1",
    "targetingType": "Manual",
    "age": [
      651,
      652],
    "gender": [
      700
    ],
    "country": [
      11,
      12
    ],
    "osVersion": [
      504
    ],
    "deviceType": [
      710
    ],
    "supplyType": [
      11470
    ]
}
```

以下範例示範如何呼叫 GET 方法，以擷取目標設定檔。

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/promotion/targeting-profile/310023951  HTTP/1.1
Authorization: Bearer <your access token>
```

<span/>

## <a name="response"></a>回應

下列方法傳回 JSON 回應本文及[目標設定檔](#targeting-profile)物件，其包含所建立、更新或擷取的目標設定檔的相關資訊。 以下為範例示範這些方法的回應本文。

```json
{
  "Data": {
    "id": 310021746,
    "name": "Contoso App Campaign - Targeting Profile 1",
    "targetingType": "Manual",
    "age": [
      651,
      652
    ],
    "gender": [
      700
    ],
    "country": [
      6,
      13,
      29
    ],
    "osVersion": [
      504,
      505,
      506,
      507,
      508
    ],
    "deviceType": [
      710,
      711
    ],
    "supplyType": [
      11470
    ]
  }
}
```

<span id="targeting-profile"/>

## <a name="targeting-profile-object"></a>目標設定檔物件

下列方法的要求和回應本文包含下列欄位。 下表顯示的欄位為唯讀 (亦即們無法在 PUT 方法中變更)，而且僅 POST 方法之要求本文所需的欄位。

| 欄位        | 類型   |  描述      |  唯讀  | 預設值  | POST 所需 |  
|--------------|--------|---------------|------|-------------|------------|
|  id   |  整數   |  目標設定檔的識別碼。     |   是    |       |   否      |       
|  name   |  字串   |   目標設定檔的名稱。    |    否   |      |  是     |       
|  targetingType   |  字串   |  下列其中一個值： <ul><li>**自動**： 指定這個值，允許 Microsoft 來選擇根據您的應用程式，在合作夥伴中心設定的目標設定檔。</li><li>**手動**︰指定這個值以定義您自己的目標設定檔。</li></ul>     |  否     |  自動    |   是    |       
|  年齡   |  陣列   |   一或多個整數，識別目標使用者的年齡範圍。 如需整數的完整清單，請參閱本篇文章中的[年齡值](#age-values)。    |    否    |  null    |     否    |       
|  性別   |  陣列   |  一或多個整數，識別目標使用者的性別。 如需整數的完整清單，請參閱本篇文章中的[性別值](#gender-values)。       |  否    |  null    |     否    |       
|  國家/地區   |  陣列   |  一或多個整數，識別目標使用者的國碼 (地區碼)。 如需整數的完整清單，請參閱本篇文章中的[國碼 (地區碼) 值](#country-code-values)。    |  否    |  null   |      否   |       
|  osVersion   |  陣列   |   一或多個整數，識別目標使用者的 OS 版本。 如需整數的完整清單，請參閱本篇文章中的[ OS 版本值](#osversion-values)。     |   否    |  null   |     否    |       
|  deviceType   | 陣列    |  一或多個整數，識別目標使用者的裝置類型。 如需整數的完整清單，請參閱本篇文章中的[ 裝置類型值](#devicetype-values)。       |   否    |  null    |    否     |       
|  supplyType   |  陣列   |  一或多個整數，識別將會顯示行銷活動廣告的廣告庫存類型。 如需整數的完整清單，請參閱本篇文章中的[ 供應類型值](#supplytype-values)。      |   否    |  null   |     否    |   |  


<span id="age-values"/>

### <a name="age-values"></a>年齡值

[TargetingProfile](#targeting-profile) 物件中的*年齡*欄位，包含下列一或多個整數，可識別目標使用者的年齡範圍。

|  適用於*年齡*欄位的整數值  |  對應的年齡範圍  |  
|---------------------------------|---------------------------|
|     651     |            13 到 17 個             |
|     652     |           18 到 24 個             |
|     653     |            25 到 34 個             |
|     654     |            35 到 49 個             |
|     655     |            50 及以上             |

若要以程式設計方式取得*年齡*欄位中支援的值，您可以呼叫以下 GET 方法。  對於 ```Authorization``` 標頭，以 **Bearer** &lt;*token*&gt; 形式傳遞 Azure AD 存取權杖。

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/promotion/reference/age
Authorization: Bearer <your access token>
```

下列範例顯示此方法的回應本文。

```json
{
  "Data": {
    "Age": {
      "651": "Age13To17",
      "652": "Age18To24",
      "653": "Age25To34",
      "654": "Age35To49",
      "655": "Age50AndAbove"
    }
  }
}
```

<span id="gender-values"/>

### <a name="gender-values"></a>性別值

[TargetingProfile](#targeting-profile) 物件中的*性別*欄位，包含下列一或多個整數，可識別目標使用者的性別。

|  *性別*欄位的整數值  |  對應性別  |  
|---------------------------------|---------------------------|
|     700     |            男性             |
|     701     |           女性             |

若要以程式設計方式取得*性別*欄位中支援的值，您可以呼叫以下 GET 方法。  對於 ```Authorization``` 標頭，以 **Bearer** &lt;*token*&gt; 形式傳遞 Azure AD 存取權杖。

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/promotion/reference/gender
Authorization: Bearer <your access token>
```

下列範例顯示此方法的回應本文。

```json
{
  "Data": {
    "Gender": {
      "700": "Male",
      "701": "Female"
    }
  }
}
```


<span id="osversion-values"/>

### <a name="os-version-values"></a>OS 版本值

[TargetingProfile](#targeting-profile) 物件中的 *osVersion* 欄位，包含下列一或多個整數，可識別目標使用者的 OS 版本。

|  *osVersion*欄位的整數值  |  對應的 OS 版本  |  
|---------------------------------|---------------------------|
|     500     |            Windows Phone 7             |
|     501     |           Windows Phone 7.1             |
|     502     |           Windows Phone 7.5             |
|     503     |           Windows Phone 7.8             |
|     504     |           Windows Phone 8.0             |
|     505     |           Windows Phone 8.1             |
|     506     |           Windows8.0             |
|     507     |           Windows8.1             |
|     508     |           Windows10             |
|     509     |           Windows 10 行動裝置版             |

若要以程式設計方式取得 *osVersion* 欄位中支援的值，您可以呼叫以下 GET 方法。  對於 ```Authorization``` 標頭，以 **Bearer** &lt;*token*&gt; 形式傳遞 Azure AD 存取權杖。

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/promotion/reference/osversion
Authorization: Bearer <your access token>
```

下列範例顯示此方法的回應本文。

```json
{
  "Data": {
    "OsVersion": {
      "500": "WindowsPhone70",
      "501": "WindowsPhone71",
      "502": "WindowsPhone75",
      "503": "WindowsPhone78",
      "504": "WindowsPhone80",
      "505": "WindowsPhone81",
      "506": "Windows80",
      "507": "Windows81",
      "508": "Windows10",
      "509": "WindowsPhone10"
    }
  }
}
```


<span id="devicetype-values"/>

### <a name="device-type-values"></a>裝置類型值

[TargetingProfile](#targeting-profile) 物件中的 *deviceType* 欄位，包含下列一或多個整數，可識別目標使用者的裝置類型。

|  *deviceType* 欄位的整數值  |  對應的裝置類型  |  描述  |
|---------------------------------|---------------------------|---------------------------|
|     710     |  Windows   |  這表示執行 Windows 10 或 Windows 的傳統型版本的裝置 8.x。  |
|     711     |  Phone     |  這表示執行 Windows 10 行動裝置版、Windows Phone 8.x 或 Windows Phone 7.x 的裝置。

若要以程式設計方式取得 *deviceType* 欄位中支援的值，您可以呼叫以下 GET 方法。  對於 ```Authorization``` 標頭，以 **Bearer** &lt;*token*&gt; 形式傳遞 Azure AD 存取權杖。

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/promotion/reference/devicetype
Authorization: Bearer <your access token>
```

下列範例顯示此方法的回應本文。

```json
{
  "Data": {
    "DeviceType": {
      "710": "Windows",
      "711": "Phone"
    }
  }
}
```


<span id="supplytype-values"/>

### <a name="supply-type-values"></a>提供類型值

[TargetingProfile](#targeting-profile) 物件中的 *supplyType* 欄位包含下列一或多個整數，識別將會顯示行銷活動廣告的廣告庫存類型。

|  *supplyType* 欄位的整數值  |  對應的供應類型  |  描述  |
|---------------------------------|---------------------------|---------------------------|
|     11470     |  應用程式        |  這是指以應用程式顯示的廣告。  |
|     11471     |  通用        |  這是指在網路上和在其他顯示表面以應用程式顯示的廣告。  |

若要以程式設計方式取得 *supplyType* 欄位中支援的值，您可以呼叫以下 GET 方法。  對於 ```Authorization``` 標頭，以 **Bearer** &lt;*token*&gt; 形式傳遞 Azure AD 存取權杖。

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/promotion/reference/supplytype
Authorization: Bearer <your access token>
```

下列範例顯示此方法的回應本文。

```json
{
  "Data": {
    "SupplyType": {
      "11470": "App",
      "11471": "Universal"
    }
  }
}
```

<span id="country-code-values"/>

### <a name="country-code-values"></a>國碼 (地區碼) 值

[TargetingProfile](#targeting-profile)物件中的 *country* 欄位，包含下列一或多個整數，可識別目標使用者的 [ISO 3166-1 alpha 2](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2) 國碼 (地區碼)。

|  *country* 欄位的整數值  |  對應的國碼 (地區碼)  |  
|-------------------------------------|------------------------------|
|     1      |            US                  |
|     2      |            AU                  |
|     3      |            AT                  |
|     4      |            BE                  |
|     5      |            BR                  |
|     6      |            CA                  |
|     7      |            DK                  |
|     8      |            FI                  |
|     9      |            FR                  |
|     10      |            DE                  |
|     11      |            GR                  |
|     12      |            HK                  |
|     13      |            IN                  |
|     14      |            IE                  |
|     15      |            IT                  |
|     16      |            JP                  |
|     17      |            LU                  |
|     18      |            MX                  |
|     19      |            NL                  |
|     20      |            NZ                  |
|     21      |            NO                  |
|     22      |            PL                  |
|     23      |            PT                  |
|     24      |            SG                  |
|     25      |            ES                  |
|     26      |            SE                  |
|     27      |            CH                  |
|     28      |            TW                  |
|     29      |            GB                  |
|     30      |            RU                  |
|     31      |            CL                  |
|     32      |            CO                  |
|     33      |            CZ                  |
|     34      |            HU                  |
|     35      |            ZA                  |
|     36      |            KR                  |
|     37      |            CN                  |
|     38      |            RO                  |
|     39      |            TR                  |
|     40      |            SK                  |
|     41      |            IL                  |
|     42      |            ID                  |
|     43      |            AR                  |
|     44      |            MY                  |
|     45      |            PH                  |
|     46      |            PE                  |
|     47      |            UA                  |
|     48      |            AE                  |
|     49      |            TH                  |
|     50      |            IQ                  |
|     51      |            VN                  |
|     52      |            CR                  |
|     53      |            VE                  |
|     54      |            QA                  |
|     55      |            SI                  |
|     56      |            BG                  |
|     57      |            LT                  |
|     58      |            RS                  |
|     59      |            HR                  |
|     60      |            HR                  |
|     61      |            LV                  |
|     62      |            EE                  |
|     63      |            IS                  |
|     64      |            KZ                  |
|     65      |            SA                  |
|     67      |            AL                  |
|     68      |            DZ                  |
|     70      |            AO                  |
|     72      |            AM                  |
|     73      |            AZ                  |
|     74      |            BS                  |
|     75      |            BD                  |
|     76      |            BB                  |
|     77      |            BY                  |
|     81      |            BO                  |
|     82      |            BA                  |
|     83      |            BW                  |
|     87      |            KH                  |
|     88      |            CM                  |
|     94      |            CD                  |
|     95      |            CI                  |
|     96      |            CY                  |
|     99      |            DO                  |
|     100      |            EC                  |
|     101      |            EG                  |
|     102      |            SV                  |
|     107      |            FJ                  |
|     108      |            GA                  |
|     110      |            GE                  |
|     111      |            GH                  |
|     114      |            GT                  |
|     118      |            HT                  |
|     119      |            HN                  |
|     120      |            JM                  |
|     121      |            JO                  |
|     122      |            KE                  |
|     124      |            KW                  |
|     125      |            KG                  |
|     126      |            LA                  |
|     127      |            LB                  |
|     133      |            MK                  |
|     135      |            MW                  |
|     138      |            MT                  |
|     141      |            MU                  |
|     145      |            ME                  |
|     146      |            MA                  |
|     147      |            MZ                  |
|     148      |            NA                  |
|     150      |            NP                  |
|     151      |            NI                  |
|     153      |            NG                  |
|     154      |            OM                  |
|     155      |            PK                  |
|     157      |            PA                  |
|     159      |            PY                  |
|     167      |            SN                  |
|     172      |            LK                  |
|     176      |            TZ                  |
|     180      |            TT                  |
|     181      |            TN                  |
|     184      |            UG                  |
|     185      |            UY                  |
|     186      |            UZ                  |
|     189      |            ZM                  |
|     190      |            ZW                  |
|     219      |            MD                  |
|     224      |            PS                  |
|     225      |            RE                  |
|     246      |            PR                  |

若要以程式設計方式取得 *country* 欄位中支援的值，您可以呼叫以下 GET 方法。  對於 ```Authorization``` 標頭，以 **Bearer** &lt;*token*&gt; 形式傳遞 Azure AD 存取權杖。

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/promotion/reference/country
Authorization: Bearer <your access token>
```

下列範例顯示此方法的回應本文。

```json
{
  "Data": {
    "Country": {
      "1": "US",
      "2": "AU",
      "3": "AT",
      "4": "BE",
      "5": "BR",
      "6": "CA",
      "7": "DK",
      "8": "FI",
      "9": "FR",
      "10": "DE",
      "11": "GR",
      "12": "HK",
      "13": "IN",
      "14": "IE",
      "15": "IT",
      "16": "JP",
      "17": "LU",
      "18": "MX",
      "19": "NL",
      "20": "NZ",
      "21": "NO",
      "22": "PL",
      "23": "PT",
      "24": "SG",
      "25": "ES",
      "26": "SE",
      "27": "CH",
      "28": "TW",
      "29": "GB",
      "30": "RU",
      "31": "CL",
      "32": "CO",
      "33": "CZ",
      "34": "HU",
      "35": "ZA",
      "36": "KR",
      "37": "CN",
      "38": "RO",
      "39": "TR",
      "40": "SK",
      "41": "IL",
      "42": "ID",
      "43": "AR",
      "44": "MY",
      "45": "PH",
      "46": "PE",
      "47": "UA",
      "48": "AE",
      "49": "TH",
      "50": "IQ",
      "51": "VN",
      "52": "CR",
      "53": "VE",
      "54": "QA",
      "55": "SI",
      "56": "BG",
      "57": "LT",
      "58": "RS",
      "59": "HR",
      "60": "BH",
      "61": "LV",
      "62": "EE",
      "63": "IS",
      "64": "KZ",
      "65": "SA",
      "67": "AL",
      "68": "DZ",
      "70": "AO",
      "72": "AM",
      "73": "AZ",
      "74": "BS",
      "75": "BD",
      "76": "BB",
      "77": "BY",
      "81": "BO",
      "82": "BA",
      "83": "BW",
      "87": "KH",
      "88": "CM",
      "94": "CD",
      "95": "CI",
      "96": "CY",
      "99": "DO",
      "100": "EC",
      "101": "EG",
      "102": "SV",
      "107": "FJ",
      "108": "GA",
      "110": "GE",
      "111": "GH",
      "114": "GT",
      "118": "HT",
      "119": "HN",
      "120": "JM",
      "121": "JO",
      "122": "KE",
      "124": "KW",
      "125": "KG",
      "126": "LA",
      "127": "LB",
      "133": "MK",
      "135": "MW",
      "138": "MT",
      "141": "MU",
      "145": "ME",
      "146": "MA",
      "147": "MZ",
      "148": "NA",
      "150": "NP",
      "151": "NI",
      "153": "NG",
      "154": "OM",
      "155": "PK",
      "157": "PA",
      "159": "PY",
      "167": "SN",
      "172": "LK",
      "176": "TZ",
      "180": "TT",
      "181": "TN",
      "184": "UG",
      "185": "UY",
      "186": "UZ",
      "189": "ZM",
      "190": "ZW",
      "219": "MD",
      "224": "PS",
      "225": "RE",
      "246": "PR"
    }
  }
}
```

## <a name="related-topics"></a>相關主題

* [使用 Microsoft Store 服務執行廣告行銷活動](run-ad-campaigns-using-windows-store-services.md)
* [管理廣告行銷活動](manage-ad-campaigns.md)
* [管理廣告行銷活動的廣告播送行](manage-delivery-lines-for-ad-campaigns.md)
* [管理廣告行銷活動的廣告素材](manage-creatives-for-ad-campaigns.md)
* [取得廣告行銷活動績效資料](get-ad-campaign-performance-data.md)
