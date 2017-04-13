---
author: jnHs
Description: "提交附加元件時，[屬性] 頁面上的選項有助於決定提供給客戶時附加元件的行為。"
title: "輸入附加元件屬性"
ms.assetid: 26D2139F-66FD-479E-940B-7491238ADCAE
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.openlocfilehash: 186088f249c2e6fe116c970bd1969fcb59863ba6
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="enter-add-on-properties"></a>輸入附加元件屬性


提交附加元件時，**\[屬性\]** 頁面上的選項有助於決定提供給客戶時附加元件的行為。

## <a name="product-type"></a>產品類型

第一次[建立附加元件](set-your-add-on-product-id.md)時您必須選取產品類型。 這裡會顯示您選取的產品類型，但無法進行變更。

> **注意**  如果您還沒有發佈附加元件。 如果您需要選擇其他產品類型，您可以刪除該提交並重新開始。 

根據您選取的產品類型而定，您可能會看到下列其中一個欄位︰

### <a name="product-lifetime"></a>產品存留期
如果您選取的產品類型是 **\[耐久品\]**，則此處會顯示 **\[產品存留期\]**。 耐久型附加元件的預設 **\[產品存留期\]** 為 **\[永久\]**，這表示附加元件永久有效。 若有需要，您可以設定 **\[產品存留期\]**，讓附加元件在設定的期間後到期 (可選擇 1-365 天)。 

### <a name="quantity"></a>數量
如果您選取的產品類型是 **\[市集管理的消費性產品\]**，則此處會顯示 **\[數量\]**。 您將需要輸入 1 到 1000000 之間的數字。 當客戶取得您的附加元件時，會將此數量授與客戶，而且當 App 報告客戶取用的附加元件時，市集將會追蹤餘額。

## <a name="content-type"></a>內容類型

不論您的附加元件是哪種產品類型，您還需要表明您所提供的內容類型。 對大部分的附加元件而言，內容類型應為 **\[電子軟體下載\]**。 如果清單中的其他選項更適用於您的附加元件 (例如，如果您提供音樂下載或電子書)，請改為選取該選項。 

以下是可能的附加元件內容類型選項：

-   電子軟體下載
-   電子書
-   電子雜誌單期刊物
-   電子報單期刊物
-   音樂下載
-   音樂串流
-   線上資料儲存/服務
-   影片下載
-   影片串流
-   軟體即服務

## <a name="keywords"></a>關鍵字

您可以選擇為您所提交的每個附加元件提供最多 10 個關鍵字 (字元數上限為 30)。 接著，您的 App 可以查詢符合這些單字的附加元件。 此功能可讓您在 App 中建置能夠載入附加元件的畫面，而不必直接在 App 的程式碼中指定產品識別碼。 之後，您可以隨時變更附加元件的關鍵字，不需要變更 App 中的程式碼或重新提交 App。

> **注意**  關鍵字不適用於以 Windows 8 和 Windows 8.1 為目標的套件。

## <a name="custom-developer-data"></a>自訂開發人員資料

您最多可輸入 3000 個字元在 **\[自訂開發人員資料\]** 欄位中 (舊稱為**「標記」**)，提供應用程式內產品的額外內容。 更常見的狀況是使用 XML 字串形式，但您可以在此欄位中輸入任何內容。

若要查詢這個欄位，使用 [Windows.Services.Store 命名空間](https://msdn.microsoft.com/en-us/library/windows/apps/windows.services.store.aspx)中的 [StoreSku.CustomDeveloperData](https://msdn.microsoft.com/en-us/library/windows/apps/windows.services.store.storesku.customdeveloperdata.aspx) 屬性。 (或者，如果您使用的是 [Windows.ApplicationModel.Store 命名空間](https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.store.aspx)，則使用 [ProductListing.Tag](https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.store.productlisting.tag.aspx) 屬性)。

例如，假設您有一個遊戲，而您要以附加元件形式賣出一袋金幣。 使用 **\[自訂開發人員資料\]** 欄位，App 就能夠查詢這袋金幣。 您可以隨時更新附加元件的 **\[自訂開發人員資料\]** 欄位中的資訊，藉以調整此值 (在此案例中為袋子中的金幣數)，而不必變更 App 中的程式碼或重新提交 App。

> **注意**：**\[自訂開發人員資料\]** 欄位不適合用於 Windows 8 和 Windows 8.1 為目標的套件。

 

 

 




