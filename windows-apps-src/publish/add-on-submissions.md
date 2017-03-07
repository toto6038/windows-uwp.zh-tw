---
author: jnHs
Description: "您需透過 Windows 開發人員中心儀表板發佈附加元件。"
title: "附加元件提交"
ms.assetid: E175AF9E-A1D4-45DF-B353-5E24E573AE67
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: e7d13a55ba545758e01452103c3380ac67ad6610
ms.lasthandoff: 02/07/2017

---

# <a name="add-on-submissions"></a>附加元件提交

附加元件 (有時也稱為應用程式內產品) 是可供客戶購買之應用程式的補充項目。 附加元件可以是有趣的新附加元件功能、新的遊戲等級，或任何您認為將能持續吸引使用者的項目。 附加元件不僅是賺錢的絕佳方式，也可以協助促進客戶的互動和投入。

您要透過 Windows 開發人員中心儀表板發佈附加元件。 您也需要在您的 App 程式碼中[啟用附加元件](../monetize/in-app-purchases-and-trials.md)。

附加元件提交程序的第一步是透過[定義產品類型及產品識別碼](set-your-add-on-product-id.md)來於儀表板中建立附加元件。 之後，您就可以建立提交作業，讓您的附加元件可透過 Windows 市集購買。 您在[提交 App](app-submissions.md) 的同時就可以提交附加元件，也可以分別處理。 App 進到市集之後，您可以對附加元件[進行更新](#updating-an-add-on-after-submission)，不需要重新提交 App。

> **注意**&nbsp;&nbsp;本文件章節描述如何在開發人員中心儀表板上提交附加元件。 或者，您可以使用 [Windows 市集提交 API](../monetize/create-and-manage-submissions-using-windows-store-services.md)，將附加元件提交自動化。

## <a name="checklist-for-submitting-an-add-on"></a>提交附加元件的檢查清單

以下是您在建立附加元件提交時提供之資訊的清單。 您需要提供的項目如下所示。 某些項目是選擇性的，或者已經有預設值，但您可以視需要加以變更。

### <a name="create-a-new-add-on-page"></a>建立新的附加元件頁面
| 欄位名稱                    | 注意事項                            |
|-------------------------------|----------------------------------|
| [**產品類型**](set-your-add-on-product-id.md#product-type)      | 必要。 如果是 **\[耐久\]**，就必須有 **\[產品存留期\]**。 |  
| [**產品識別碼**](set-your-add-on-product-id.md#product-id)          | 必要 |        

<span/>

### <a name="properties-page"></a>屬性頁面
| 欄位名稱                    | 注意事項                              |   
|-------------------------------|------------------------------------|
| [**產品存留期**](enter-add-on-properties.md#product-lifetime)  | 如果產品類型是 **\[耐久品\]**，則為必要。 不適用於其他產品類型。 |
| [**數量**](enter-add-on-properties.md#quantity)  | 如果產品類型是 **\[市集管理的消費性產品\]**，則為必要。 不適用於其他產品類型。
| [**內容類型**](enter-add-on-properties.md#content-type)          | 必要       |               
| [**關鍵字**](enter-add-on-properties.md#keywords)                  | 選擇性 (最多 10 個關鍵字，每個關鍵字最多 30 個字元) |
| [**自訂開發人員資料**](enter-add-on-properties.md#custom-developer-data)                               | 選擇性 (最多 3000 個字元)             |

<span/>

### <a name="pricing-and-availability-page"></a>定價和可用性頁面
| 欄位名稱                    | 注意事項                                       |
|-------------------------------|---------------------------------------------|
| [**基本價格**](set-add-on-pricing-and-availability.md#base-price)                | 必要                                    |
| [**市場和自訂價格**](set-add-on-pricing-and-availability.md#markets-and-custom-prices)  | 預設：所有可能的市場都能取得 |
| [**銷售定價**](put-apps-and-add-ons-on-sale.md)               | 選用                             |
| [**配送和可見性**](set-add-on-pricing-and-availability.md#distribution-and-visibility)   | 預設：客戶瀏覽或搜尋市集可找到附加元件 |
| [**發佈日期**](set-add-on-pricing-and-availability.md#publish-date)                | 預設：附加元件通過認證之後立即發佈 |

<span/>

### <a name="store-listings"></a>市集清單
需要一個市集清單。 我們建議針對 App 支援的每個[語言](create-add-on-store-listings.md#languages)提供市集清單。

| 欄位名稱                    | 注意事項                                       |
|-------------------------------|---------------------------------------------|
| [**標題**](create-add-on-store-listings.md#title)                    | 必要 (最多 100 個字元)              |
| [**說明**](create-add-on-store-listings.md#description)       | 選擇性 (最多 200 個字元)              |
| [**圖示**](create-add-on-store-listings.md#icon)                    | 選擇性 (300x300 像素的 .png)             |

<span/>

當您輸入完此資訊，請按一下 **\[送出到市集\]**。 在大多數情況下，認證程序大概需要一個小時。 之後，您的附加元件就會發佈到市集，供客戶購買。

>**注意**&nbsp;&nbsp;：您也必須在您的應用程式程式碼中實作附加元件。 如需詳細資訊，請參閱 [App 內購買和試用版](../monetize/in-app-purchases-and-trials.md)。


## <a name="updating-an-add-on-after-publication"></a>發佈之後更新附加元件

您可以隨時對已發佈的附加元件進行變更。 附加元件變更的提交與發佈獨立於您的 App 之外，因此您通常不需要更新整個 App，就可以對附加元件進行變更，例如更新 App 的價格或描述。

> **重要**&nbsp;&nbsp;：如果您的應用程式可供 Windows 8.x 的客戶使用，您必須建立並發佈新的應用程式提交作業，這些客戶才能看見附加元件的更新。 同樣地，如果您在以 Windows 8.x 為目標的 App 發佈後，將新的附加元件新增到 App，您必須更新您的 App 程式碼以參考這些附加元件，然後重新提交 App。 否則，Windows 8.x 的客戶將無法看見新的附加元件。

若要提交更新，請移至儀表板的附加元件頁面，然後按一下 **\[更新\]**。 這會使用您前一個提交作業的資訊做為起點，建立一個新的附加元件提交作業。 視需要變更資訊，然後按一下 **\[提交至市集\]**。

如果您想要移除先前提供的附加元件，請建立新的提交並且將 [配送和可見性](set-add-on-pricing-and-availability.md) 選項變更為 **\[不再提供購買。未顯示在您的 App 清單中\]**。 請務必視需要更新您 App 的程式碼，以便同時移除附加元件的參考。

