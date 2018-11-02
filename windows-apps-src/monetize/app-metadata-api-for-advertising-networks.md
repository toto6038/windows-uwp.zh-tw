---
author: Xansky
description: 了解如何使用 App 中繼資料 REST API 來存取 App 的特定類型中繼資料。 此 API 是要供廣告網路用來擷取 Microsoft Store 中 App 的相關資訊，讓它們能夠改進將廣告空間銷售給廣告商的方式。
title: 廣告網路的應用程式中繼資料 API
ms.author: mhopkins
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, 廣告網路, 應用程式中繼資料
ms.assetid: f0904086-d61f-4adb-82b6-25968cbec7f3
ms.localizationpriority: medium
ms.openlocfilehash: 9533b244174cc5770a68f866c722db1781fdd544
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/02/2018
ms.locfileid: "5968848"
---
# <a name="app-metadata-api-for-advertising-networks"></a>廣告網路的應用程式中繼資料 API

廣告網路可以使用 *「App 中繼資料 API」* 來以程式設計方式擷取 Microsoft Store 中 App 的相關中繼資料，包括詳細資料，例如 App 之 Store 清單的描述和類別，以及 App 的目標是否為 13 歲以下的兒童。 目前僅限已獲 Microsoft 授與此 API 權限的開發人員才能存取此 API。

本文提供的指示包括如何使用 [App 中繼資料 API 入口網站](https://admetadata.portal.azure-api.net/)來要求此 API 的存取權、如何取得您的訂用帳戶金鑰來存取此 API，以及如何呼叫此 API。

## <a name="request-access"></a>要求存取權

廣告網路可以依照下列指示來要求 App 中繼資料 API 的存取權：

1. 移至 App 中繼資料 API 入口網站的 [https://admetadata.portal.azure-api.net/signup](https://admetadata.portal.azure-api.net/signup) 頁面。
2. 輸入必要的資訊，然後按一下 **\[Sign up\]** (註冊) 按鈕。
3. 在相同的網站上，按一下 **\[Products\]** (產品) 索引標籤，然後按一下 **\[App details for advertising\]** (用於廣告的 App 詳細資料)。
4. 在下一頁上，按一下 **\[Subscribe\]** (訂閱) 按鈕。 這會將您要存取 App 中繼資料 API 的要求提交給 Microsoft。

在提交您的要求之後，您會在大約 24 小時內收到一封電子郵件，通知您該要求是已被允許還是拒絕。

<span id="get-key" />

## <a name="get-your-subscription-key"></a>取得您的訂用帳戶金鑰

如果已授權您存取 App 中繼資料 API，請依照下列指示來取得您的訂用帳戶金鑰。 您必須在對 API 的呼叫要求表頭中傳遞此金鑰。

1. 移至 App 中繼資料 API 入口網站的 [https://admetadata.portal.azure-api.net/signin](https://admetadata.portal.azure-api.net/signin) 頁面，並使用您的電子郵件和密碼登入。
2. 按一下網站右上角中您的名稱，然後按一下 **\[Profile\]** (設定檔)。
3. 在頁面的 **\[Your subscriptions\]** (您的訂用帳戶) 區段中，按一下 **\[Primary key\]** (主要金鑰) 旁邊的 **\[Show\]** (顯示)。 這就是您的訂用帳戶金鑰。 請複製該金鑰，以便在稍後呼叫 API 時可以使用。

<span id="call-the-api" />

## <a name="call-the-api"></a>呼叫 API

在您有了訂用帳戶金鑰之後，您便可以從所選擇的程式設計語言中使用 HTTP REST 語法來呼叫 API。 如需有關 API 語法的資訊，請參閱下方的 [API 語法](#syntax)一節。 若要查看 C#、JavaScript、Python 及數個其他語言中的程式碼範例，請按一下 App 中繼資料 API 入口網站的 **\[APIs\]** (API) 索引標籤，按一下 **\[App details\]** (App 詳細資料)，然後查看頁面底部的 **\[Code samples\]** (程式碼範例) 區段。

或者，您也可以使用 App 中繼資料 API 入口網站所提供的 UI 來呼叫 API：
  1. 在入口網站中，按一下 **\[APIs\]** (API) 索引標籤，然後按一下 **\[App details\]** (App 詳細資料)。
  2. 在下一頁上，於 **\[app_id\]** 欄位中輸入您想要擷取中繼資料之 App 的 [app_id](#request-parameters)，然後在 **\[Ocp_Apim_Subscription-Key\]** 欄位中輸入您的訂用帳戶金鑰。
  3. 按一下 **\[Send\]** (傳送)。 回應會顯示在頁面底部。


<span id="syntax" />

## <a name="api-syntax"></a>API 語法

這個方法的要求語法如下。

| 方法 | 要求 URI                                                      |
|--------|------------------------------------------------------------------|
| GET   | ```https://admetadata.azure-api.net/v1/app/{app_id}``` |

<span/>
 

### <a name="request-header"></a>要求的標頭

| 標頭        | 類型   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Ocp-Apim-Subscription-Key | 字串 | 必要。 您[從 App 中繼資料 API 入口網站擷取](#get-key)的訂用帳戶金鑰。  |

<span/>

### <a name="request-parameters"></a>要求參數

| 名稱        | 類型   | 描述                                                                 |
|---------------|--------|-----------------------|
| app_id | 字串 | 必要。 您想要擷取中繼資料之 App 的識別碼。 這可以是下列其中一個值：<br/><br/><ul><li>App 的「Store 識別碼」。 範例「Store 識別碼」如 9NBLGGH29DM8。</li><li>原始建置的 Windows 8.x 或 Windows Phone 8.x App「產品識別碼」(有時稱為 *「App 識別碼」*)。 「產品識別碼」是 GUID。</li></ul> |

<span/>

### <a name="request-example"></a>要求範例

下列範例示範如何擷取「Store 識別碼」為 9NBLGGH29DM8 之 App 的中繼資料。

```syntax
GET https://admetadata.azure-api.net/v1/app/9NBLGGH29DM8 HTTP/1.1
Ocp-Apim-Subscription-Key: <subscription key>
```

### <a name="response-body"></a>回應主體

下列範例示範成功呼叫此方法時的 JSON 回應主體。

```json
{
    "storeId":"9NBLGGH29DM8",
    "name":"Contoso Sports App",
    "description":"Catch the latest scores and replays using Contoso Sports App!",
    "phoneStoreGuid":"920217d7-90da-4019-99e8-46e4a6bd2594",
    "windowsStoreGuid":"d7e982e7-fbf3-42b5-9dad-72b65bd4e248",
    "storeCategory":"Sports",
    "iabCategory":"Sports",
    "iabCategoryId":"IAB17",
    "coppa":false,
    "downloadUrl":"https://www.microsoft.com/store/apps/9nblggh29dm8",
    "isLive":true,
    "iconUrls":[
      "//store-images.microsoft.com/image/apps.15753.13510798883747357.b6981489-6fdd-4fec-b655-a822247d5a76.33529096-a723-4204-9da9-a5dd8b328d4e"
    ],
    "type":"App",
    "devices":[
      "PC",
      "Phone"
    ],
    "platformVersions":[
      "Windows.Universal"
    ],
    "screenshotUrls":[
      "//store-images.microsoft.com/image/apps.15901.19810723133740207.c9781069-6fef-5fba-a055-c922051d59b6.40129896-d083-5604-ab79-aba68bfa084f"
    ]
}
```

如需有關回應主體中各個值的詳細資訊，請參閱下表。

| 值      | 類型   | 描述    |
|------------|--------|--------------------|
| storeId           | 字串  | App 的「Store 識別碼」。 範例「Store 識別碼」如 9NBLGGH29DM8。     |  
| name           | 字串  | App 的名稱。   |
| description           | 字串  | 來自 App 之 Store 清單的描述。  |
| phoneStoreGuid           | 字串  | App 的「產品識別碼」(Windows Phone 8.x)。 這是 GUID。  |
| windowsStoreGuid           | 字串  | App 的「產品識別碼」(Windows 8.x)。 這是 GUID。 |
| storeCategory           | 字串  | 「Microsoft Store」中 App 的類別。 如需了解有哪些支援的值，請參閱 Microsoft Store 中 App 的[類別與子類別表格](../publish/category-and-subcategory-table.md)。  |
| iabCategory           | 字串  | 美國互動廣告局 (Interactive Advertising Bureau, IAB) 所定義的 App 內容類別。 例如，**News** (新聞) 或 **Sports** (體育)。 如需內容類別的清單，請參閱 IAB 網站上的 [IAB Tech Lab Content Taxonomy (IAB 科技實驗室內容分類)](https://www.iab.com/guidelines/iab-quality-assurance-guidelines-qag-taxonomy) 頁面。   |
| iabCategoryId           | 字串  | App 的內容類別識別碼。 例如，**IAB12** 是 News (新聞) 類別的識別碼，**IAB17** 是 Sports (體育) 類別的識別碼。 如需內容類別識別碼的清單，請參閱 [OpenRTB API Specification (OpenRTB API 規格)](http://www.iab.com/wp-content/uploads/2015/05/OpenRTB_API_Specification_Version_2_3_1.pdf) 頁面上的 5.1 節。 |
| coppa           | 布林值  | 如果 App 的目標對象是 13 歲以下的兒童，則為 true，也因此需受「兒童線上隱私權保護法」(Children's Online Privacy Protection Act, COPPA) 約束；否則，為 false。  |
| downloadUrl           | 字串  | 「Microsoft Store」中 App 清單的連結。 這個連結的格式為 ```https://www.microsoft.com/store/apps/<Store ID>```。  |
| isLive           | 布林值  | 如果 app 目前在 Microsoft Store 中可用，則為 true，否則為 false。  |
| iconUrls           | array  |  一或多個字串的陣列，包含與 app 相關圖示 URL 的相對路徑。 若要擷取圖示，請在 URL 前面加上 *http* 或 *https*。  |
| type           | 字串  | 下列其中一個字串：**App** 或 **Game**。  |
| 裝置           |  array  | 下列一或多個字串的陣列，指定 app 支援的裝置類型：**PC**、**Phone**、**Xbox**、**IoT**、**Server** 和 **Holographic**。  |
| platformVersions           | array  |  下列一或多個字串的陣列，指定 app 支援的平台：**Windows.Universal**、**Windows.Windows8x** 和 **Windows.WindowsPhone8x**。  |
| screenshotUrls           | array  | 一或多個字串的陣列，包含與 app 相關螢幕擷取畫面 URL 的相對路徑。 若要擷取螢幕擷取畫面，請在 URL 前面加上 *http* 或 *https*。  |

<span/>



 

 
