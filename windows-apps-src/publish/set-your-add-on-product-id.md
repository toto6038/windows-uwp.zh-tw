---
author: jnHs
Description: When you create a new add-on in the Windows Dev Center dashboard, you need to specify a product type and assign it a product ID.
title: 設定您的附加內容產品類型與產品識別碼
ms.assetid: 59497B0F-82F0-4CEE-B628-040EF9ED8D3D
ms.author: wdg-dev-content
ms.date: 01/12/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 附加元件, iap, 耐久性, 消耗性, 訂閱, 產品類型, 產品識別碼, App 內購買, 應用程式內產品
ms.localizationpriority: medium
ms.openlocfilehash: 0673048fc9a1ed8fb7c439607ebc4197039699e9
ms.sourcegitcommit: 00d27738325d6db5b5e481911ae7fac0711b05eb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/07/2018
ms.locfileid: "3661338"
---
# <a name="set-your-add-on-product-type-and-product-id"></a>設定您的附加內容產品類型與產品識別碼

附加元件必須與您已經在儀表板中建立的 App 相關聯 (即使您尚未提交 App)。 您可以在您的應用程式的 **\[概觀\]** 頁面或其 **\[附加元件\]** 頁面上找到該按鈕，以 **\[建立新的附加元件\]**。

選取 **\[建立新的附加元件\]** 後，系統將會提示您指定產品類型，並指定您的附加元件的產品識別碼。

## <a name="product-type"></a>產品類型

首先，您必須表明您所提供的附加內容類型。 此選取項目指的是客戶可以如何使用您的附加元件。

> [!NOTE]
> 儲存此頁面建立附加元件之後，您將無法變更產品類型。 如果不慎選錯了產品類型，您隨時可以刪除進行中的附加元件提交，並建立新的附加元件以重新開始。

<span id="durable" />

### <a name="durable"></a>耐久性

如果您的附加元件通常是一次性購買，請選取 **\[耐久性\]** 產品類型。 這些附加元件通常用於解除鎖定 App 中的額外功能。

耐久型附加元件的預設 **\[產品存留期\]** 為 **\[永久\]**，這表示附加元件永久有效。 您可以在附加元件提交程序的[內容](enter-add-on-properties.md)步驟中，將 **\[產品存留期\]** 設為不同的持續時間。 若您這樣做，附加元件會在您指定的持續時間後到期（選項是 1-365 天），在此情況下客戶可以在附加元件到期之後重新購買附加元件。

<span id="consumable" />

### <a name="consumable"></a>消耗性

如果附加元件可以購買、使用 (消耗)，然後重新購買，您可能希望選取其中一種**消耗性**產品類型 消耗型附加內容通常用於遊戲內的貨幣 (金幣、錢幣等)，客戶可以購買設定的量並使用。 如需詳細資訊，請參閱[啟用消費性附加元件購買](../monetize/enable-consumable-add-on-purchases.md)。

消耗性附加元件有兩種類型：
- **開發人員管理的消費性產品**：餘額和履行必須在您的 App 內管理。 所有 OS 版本皆支援。
- **市集管理消耗型**：Microsoft 將會在客戶執行 Windows 10 版本 1607 或更新版本的所有裝置上追蹤餘額；在任何舊版 OS 上不支援。 若要使用此選項，父產品必須使用 Windows 10 SDK 版本 14393 或更新版本進行編譯。 另請注意，在父產品發佈之前，您無法將市集管理消耗性附加元件提交至 [市集] (雖然您可以在您的儀表板中建立提交並隨時開始使用它)。 您將必須在提交的 **\[內容\]** 頁面中輸入您市集管理消耗性附加元件的數量。

<span id="subscription" />

### <a name="subscription"></a>訂閱

如果您想要向客戶收取附加元件的週期性費用，請選擇 **\[訂閱\]**。

客戶取得一開始訂閱附加元件之後，將會繼續向其收取週期性費用，才能繼續使用附加元件。 客戶隨時都可以取消訂閱，避免進一步的費用。 您將需要在提交的 **\[內容\]** 步驟指定訂閱期間，以及是否提供免費試用。

訂閱附加元件僅支援執行 Windows 10 版本 1607 或更新版本的客戶。 父產品必須使用 Windows 10 SDK 版本 14393 或更新版本進行編譯，而且必須使用 **Windows.Services.Store** 命名空間中的 App 內購買 API，而不是 **Windows.ApplicationModel.Store** 命名空間中的 API。 如需詳細資訊，請參閱[啟用應用程式的訂閱附加元件](../monetize/enable-subscription-add-ons-for-your-app.md)。

您必須先提交父產品，才能將訂閱附加元件發行至 Store (雖然您可以在您的儀表板中建立提交並隨時開始使用它)。

## <a name="product-id"></a>產品識別碼

無論您選擇哪種產品類型，您都需要輸入您附加元件的唯一產品識別碼。 此名稱將會用來識別儀表板中的附加元件，您可以使用此識別碼，[在程式碼中用來參考附加元件](../monetize/in-app-purchases-and-trials.md#how-to-use-product-ids-for-add-ons-in-your-code)。

以下是一些選擇產品識別碼時值得注意的事項：

-   客戶不會看到這個產品識別碼。 (您之後可以輸入要對客戶顯示的[標題和描述](create-add-on-descriptions.md))。
-   您在附加內容發佈之後，無法變更或刪除附加內容的產品識別碼。
-   產品識別碼的長度不能超過 100 個字元。
-   產品識別碼不能包含下列任何字元：**&lt; &gt; \* % &amp; : \\ ? + ,**
-   若要在所有作業系統版本中提供您的附加內容，您只能使用英數字元、句點和/或底線。 如果您使用任何其他類型的字元，附加內容將無法供執行 Windows Phone 8.1 或更舊版本的客戶購買。
-   產品識別碼在 Microsoft Store 中不一定要是唯一的，但是對您的開發人員帳戶則必須是唯一的。
 
