---
author: jnHs
Description: "您要透過 Windows 開發人員中心儀表板發佈 IAP。"
title: "IAP 提交"
ms.assetid: E175AF9E-A1D4-45DF-B353-5E24E573AE67
translationtype: Human Translation
ms.sourcegitcommit: 97f4aee47cab9064ac053e7a6e16441d6960d41f
ms.openlocfilehash: 4a1764dfb8f94409aba973a28ba2999854179196

---

# IAP 提交


IAP (應用程式內產品) 是針對您應用程式的補充項目，可供客戶購買。 IAP 可以是有趣的新附加元件功能、新的遊戲等級，或任何您認為將能持續吸引使用者的項目。 IAP 不僅是賺錢的絕佳方式，也可以協助促進客戶的互動和投入。

您要透過 Windows 開發人員中心儀表板發佈 IAP。 您也需要在您的 app 程式碼中[啟用 IAP](../monetize/enable-in-app-product-purchases.md)。

IAP 提交程序的第一步是透過[定義產品類型及產品識別碼](set-your-iap-product-id.md)來於儀表板中建立 IAP。 之後，您就可以建立提交作業，讓您的 IAP 可透過 Windows 市集購買。 您在[提交 app](app-submissions.md) 的同時就可以提交 IAP，也可以分別處理。 app 進到市集之後，您可以對 IAP [進行更新](#updating-an-iap-after-submission)，不需要重新提交 app。

## 提交 IAP 的檢查清單

以下是您在建立 IAP 提交時提供之資訊的清單。 您需要提供的項目如下所示。 某些項目是選擇性的，或者已經有預設值，但您可以視需要加以變更。

### 建立新的 IAP 頁面
| 欄位名稱                    | 注意事項                            | 
|-------------------------------|----------------------------------|
| [**產品類型**](set-your-iap-product-id.md#product-type)      | 必要。 如果是 [耐久]****，就必須有 [產品存留期]****。 |  
| [**產品識別碼**](set-your-iap-product-id.md#product-id)          | 必要 |        

### 屬性頁面
| 欄位名稱                    | 注意事項                              |   
|-------------------------------|------------------------------------|
| [**產品存留期**](enter-iap-properties.md#product-lifetime)  | 如果產品類型是 [耐久品]**** 則為必要。 如果產品類型為 [消耗性物品]**** 則不適用。 | 
| [**內容類型**](enter-iap-properties.md#content-type)          | 必要       |               
| [**關鍵字**](enter-iap-properties.md#keywords)                  | 選擇性 (最多 10 個關鍵字，每個關鍵字最多 30 個字元) | 
| [**標記**](enter-iap-properties.md#tag)                               | 選擇性 (最多 3000 個字元)             | 

### 定價和可用性頁面 
| 欄位名稱                    | 注意事項                                       | 
|-------------------------------|---------------------------------------------|
| [**基本價格**](set-iap-pricing-and-availability.md#base-price)                | 必要                                    | 
| [**市場和自訂價格**](set-iap-pricing-and-availability.md#markets-and-custom-prices)  | 預設：所有可能的市場都能取得 | 
| [**銷售定價**](put-apps-and-iaps-on-sale.md)               | 選用                             |
| [**配送和可見性**](set-iap-pricing-and-availability.md#distribution-and-visibility)   | 預設：客戶瀏覽或搜尋市集可找到 IAP | 
| [**發佈日期**](set-iap-pricing-and-availability.md#publish-date)                | 預設：IAP 通過認證之後立即發佈 |

### 介紹頁面
需要一項描述。 我們建議針對 app 支援的每個[語言](create-iap-descriptions.md#languages)提供描述。

| 欄位名稱                    | 注意事項                                       | 
|-------------------------------|---------------------------------------------|
| [**標題**](create-iap-descriptions.md#title)                    | 必要 (最多 100 個字元)              |
| [**說明**](create-iap-descriptions.md#description)       | 選擇性 (最多 200 個字元)              |
| [**圖示**](create-iap-descriptions.md#icon)                    | 選擇性 (300x300 像素的 .png)             | 

當您輸入完此資訊，請按一下 [送出到市集]****。 在大多數情況下，認證程序大概需要一個小時。 之後，您的 IAP 就會發行到市集，供客戶購買。

**注意** 您也必須在您的 app 程式碼中實作 IAP。 如需詳細資訊，請參閱[啟用應用程式內產品購買](../monetize/enable-in-app-product-purchases.md)。


## 發佈之後更新 IAP

您可以隨時對已發佈的 IAP 進行變更。 IAP 變更的提交與發佈獨立於您的 app 之外，因此您通常不需要更新整個 app 來對 IAP 進行變更，例如更新 app 的價格或描述。

> **重要** 如果您的 app 可供 Windows 8.x 的客戶使用，您必須建立並發佈新的 app 提交作業，這些客戶才能看見 IAP 的更新。 同樣地，如果您在以 Windows 8.x 為目標的 app 發佈後，新增新的 IAP 到 app，您必須更新您的 app 程式碼以參考這些 IAP，然後重新提交 app。 否則，Windows 8.x 的客戶將無法看見新的 IAP。

若要提交更新，請移至儀表板的 IAP 頁面，然後按一下 [更新]****。 這會使用您前一個提交作業的資訊做為開始，建立一個新的 IAP 提交作業。 視需要變更資訊，然後按一下 [**提交至市集**]。

如果您想要移除先前提供的 IAP，請建立新的提交並且將 [[配送和可見性](set-iap-pricing-and-availability.md)] 選項變更為 [**不再提供購買。未顯示在您的 app 清單中**]。 請務必更新您 app 的程式碼，請視需要也移除 IAP 的參考。




<!--HONumber=Jun16_HO5-->


