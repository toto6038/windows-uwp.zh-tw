---
author: jnHs
Description: "提交附加元件時，[屬性] 頁面上的選項有助於決定提供給客戶時附加元件的行為。"
title: "輸入附加元件屬性"
ms.assetid: 26D2139F-66FD-479E-940B-7491238ADCAE
ms.author: wdg-dev-content
ms.date: 06/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.openlocfilehash: 253e008d3622094dcfe765531d71e5f37b7777b0
ms.sourcegitcommit: de6bc8acec2cd5ebc36bb21b2ce1a9980c3e78b2
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/17/2017
---
# <a name="enter-add-on-properties"></a>輸入附加元件屬性


提交附加元件時，**\[屬性\]** 頁面上的選項有助於決定提供給客戶時附加元件的行為。

## <a name="product-type"></a>產品類型

第一次[建立附加元件](set-your-add-on-product-id.md)時您必須選取產品類型。 這裡會顯示您選取的產品類型，但無法進行變更。

> [!TIP]
> 如果您還沒有發佈附加元件，若要選擇其他產品類型時，您可以刪除該提交並重新開始。

您在此頁面看到的欄位會根據附加元件的產品類型而有所不同。

## <a name="product-lifetime"></a>產品存留期


如果您選取的產品類型是 **\[耐久性\]**，則此處會顯示 **\[產品存留期\]**。 耐久型附加元件的預設 **\[產品存留期\]** 為 **\[永久\]**，這表示附加元件永久有效。 若有需要，您可以設定 **\[產品存留期\]**，讓附加元件在設定的期間後到期 (可選擇 1-365 天)。

## <a name="quantity"></a>數量


如果您選取的產品類型是 **\[市集管理的消費性產品\]**，則此處會顯示 **\[數量\]**。 您將需要輸入 1 到 1000000 之間的數字。 當客戶取得您的附加元件時，會將此數量授與客戶，而且當 App 報告客戶取用的附加元件時，市集將會追蹤餘額。


## <a name="subscription-period"></a>訂閱期間

如果您選取的產品類型是 **\[訂閱\]**，則此處會顯示 **\[訂閱期間\]**。 您將需要選擇其中一個可用的選項 (**\[每月\]**，**\[3 個月\]**、**\[6 個月\]**、**\[每年\]** 或 **\[24 個月\]**)，以指出向客戶收取訂閱費用的頻率。 請注意，您的附加元件發行之後，您無法變更 **\[訂閱期間\]** 選項。

> [!NOTE]
> 目前，只有參與早期採用者計畫的一組開發人員帳戶才可以建立訂閱附加元件。 未來我們會讓所有開發人員帳戶都可以建立訂閱附加元件，這份初步文件現在可讓開發人員預覽此功能。 如需詳細資訊，請參閱[啟用應用程式的訂閱附加元件](../monetize/enable-subscription-add-ons-for-your-app.md)。


## <a name="free-trial"></a>免費試用

對於訂閱附加元件，此處也會顯示 **\[免費試用\]**。 您必須選擇是否要讓客戶在一段指定時間內免費使用附加元件 (**\[1 週\]** 或 **\[1 個月\]**)，或是否提供 **\[無免費試用\]**。 請注意，您的附加元件發行之後，您無法變更 **\[免費試用\]** 選項。


## <a name="content-type"></a>內容類型

不論您的附加元件是哪種產品類型，您都需要表明您所提供的內容類型。 對大部分的附加元件而言，內容類型應為 **\[電子軟體下載\]**。 如果清單中的其他選項更能描述您的附加元件 (例如，如果您提供音樂下載或電子書)，請改為選取該選項。

以下是可能的附加元件內容類型選項：

-   電子軟體下載
-   電子書
-   電子雜誌單期刊物
-   電子報單期刊物
-   音樂下載
-   音樂串流
-   線上資料儲存/服務
-   軟體即服務
-   影片下載
-   影片串流


## <a name="additional-properties"></a>其他內容

這些欄位對所有類型的附加元件都是選擇性的。

<span id="keywords" />
### <a name="keywords"></a>關鍵字

您可以選擇為您所提交的每個附加元件提供最多 10 個關鍵字 (字元數上限為 30)。 接著，您的 App 可以查詢符合這些單字的附加元件。 此功能可讓您在 App 中建置能夠載入附加元件的畫面，而不必直接在 App 的程式碼中指定產品識別碼。 之後，您可以隨時變更附加元件的關鍵字，不需要變更 App 中的程式碼或重新提交 App。

若要查詢這個欄位，使用 [Windows.Services.Store 命名空間](https://msdn.microsoft.com/en-us/library/windows/apps/windows.services.store.aspx)中的 [StoreProduct.Keywords](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct#Windows_Services_Store_StoreProduct_Keywords) 屬性  (或者，如果您使用的是 [Windows.ApplicationModel.Store 命名空間](https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.store.aspx)，則使用 [ProductListing.Keywords](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.productlisting#Windows_ApplicationModel_Store_ProductListing_Keywords) 屬性)。

> [!NOTE]
> 關鍵字不適用於以 Windows 8 和 Windows 8.1 為目標的套件。

<span id="custom-developer-data" />
### <a name="custom-developer-data"></a>自訂開發人員資料

您最多可輸入 3000 個字元在 **\[自訂開發人員資料\]** 欄位中 (舊稱為**「標記」**)，提供應用程式內產品的額外內容。 更常見的狀況是使用 XML 字串形式，但您可以在此欄位中輸入任何內容。 接著，您的應用程式可以查詢此欄位以讀取其內容（雖然應用程式無法編輯資料並傳回變更）。

例如，假設您有一個遊戲，而您要銷售附加元件，讓客戶存取其他關卡。 使用 **\[自訂開發人員資料\]** 欄位，應用程式可以查詢以查看客戶擁有此附加元件時可以使用哪些關卡。 您可以隨時更新附加元件的 **\[自訂開發人員資料\]** 欄位中的資訊，然後發行附加元件的更新提交，藉以調整此值 (在此案例中為包含的關卡)，而不必變更 App 中的程式碼或重新提交 App。

若要查詢這個欄位，使用 [Windows.Services.Store 命名空間](https://msdn.microsoft.com/en-us/library/windows/apps/windows.services.store.aspx)中的 [StoreSku.CustomDeveloperData](https://msdn.microsoft.com/en-us/library/windows/apps/windows.services.store.storesku.customdeveloperdata.aspx) 屬性。 (或者，如果您使用的是 [Windows.ApplicationModel.Store 命名空間](https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.store.aspx)，則使用 [ProductListing.Tag](https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.store.productlisting.tag.aspx) 屬性)。

> [!NOTE]
> **\[自訂開發人員資料\]** 欄位不適合用於 Windows 8 和 Windows 8.1 為目標的套件。

 

 

 
