---
author: jnHs
Description: The App properties page of the app submission process lets you define your app's category and indicate hardware preferences or other declarations.
title: 輸入應用程式屬性
ms.assetid: CDE4AF96-95A0-4635-9D07-A27B810CAE26
ms.author: wdg-dev-content
ms.date: 01/24/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 遊戲設定, 顯示模式, 系統需求, 硬體需求, 基本硬體, 建議的硬體
ms.localizationpriority: high
ms.openlocfilehash: 8ecdeb0dd4ebba83a387666ab87067ff419a9303
ms.sourcegitcommit: 8d9d4f17e272b78e38b346f846b96260c922bbb2
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
# <a name="enter-app-properties"></a>輸入應用程式屬性

[應用程式提交程序](app-submissions.md)的 [**屬性**] 頁面可用來定義應用程式的類別，並輸入其他資訊和宣告。 請務必在此頁面上提供有關您應用程式完整且準確的詳細資料。


## <a name="category-and-subcategory"></a>類別與子類別

您必須指示 Microsoft Store 要用來分類您的應用程式的類別 (與子類別/內容類型，如果適用的話)。 您必須指定類別才能送出應用程式。

如需詳細資訊，請參閱[類別與子類別表格](category-and-subcategory-table.md)。


## <a name="game-settings"></a>遊戲設定

您選取 [**遊戲**] 做為產品類別時，此區段才會出現。 您可在此指定遊戲支援的功能。 您在此區段中提供的所有資訊，都將在產品的市集清單中顯示。

如果您的遊戲支援任何多人遊戲選項，請務必指出一場遊戲可參與的玩家人數下限和上限。 您無法輸入超過 1,000 名玩家下限或上限。

**跨平台多人遊戲**表示，遊戲支援在 Windows 10 電腦和 Xbox 之間的多人工作階段。


## <a name="display-mode"></a>顯示模式

此區段可讓您指出您的產品的設計目的是要在電腦和/或 HoloLens 裝置上的 [Windows Mixed Reality](https://developer.microsoft.com/windows/mixed-reality) 沈浸式 (而非 2D) 檢視中執行。 如果是這樣，您也必須：
- 在**\[屬性\]** 頁面下半部顯示的[系統需求](#system-requirements)區段中，選取 **\[Windows Mixed Reality 沉浸式頭戴裝置\]** 的 **\[基本硬體\]** 或 **\[建議硬體\]**。
- 指定 **\[界限設定\]** (如果已選取電腦)，讓使用者知這原本是要僅限以坐姿或站姿使用，或者是否允許 (或要求) 使用者在使用時四處移動。 

如果您已選取 **\[遊戲\]** 做為產品的類別，您會在 **\[顯示模式\]** 選項中看到額外選項，讓您表示產品是否支援 4K 解析度視訊輸出、高動態範圍 (HDR) 視訊輸出或可變更新頻率顯示器。

如果您的產品不支援這些顯示模式選項，請讓所有的方塊保持未選取狀態。。


## <a name="product-declarations"></a>產品宣告

您可以核取此區段中的方塊以指出是否有任何宣告適用於您的 App。 這可能會影響應用程式顯示的方式、應用程式是否會提供給某些客戶，或者客戶可以使用應用程式的方式。

如需詳細資訊，請參閱 [產品宣告](app-declarations.md)。

## <a name="system-requirements"></a>系統需求

在此區段中，您可以選擇指定是否需要或建議特定硬體功能，才能讓您的 App 正常執行和互動。 您可以針對您要指定 **\[最低硬體\]** 和/或 **\[建議的硬體\]** 的每個硬體項目核取方塊 (或指出適當的選項)。

如果您選取 **\[建議的硬體\]**，這些項目將會顯示在您產品的市集清單中，做為 Windows 10 1607 版或更新版本客戶的建議硬體。 舊版作業系統的客戶不會看到這項資訊。

如果您選取 **\[最低硬體\]**，這些項目將會顯示在您產品的市集清單中，做為 Windows 10 1607 版或更新版本客戶的必要硬體。 舊版作業系統的客戶不會看到這項資訊。 市集也可以對正在沒有必要硬體的裝置上檢視您 App 清單的客戶顯示警告。 這不會防止使用者將您的 App 下載到不具備適當硬體的裝置，但他們將無法在那些裝置上對您的 App 進行評分或評論。 

客戶的行為將會依特定的需求與客戶的 Windows 版本而有所不同︰

- **對於 Windows 10 1607 版或更新版本的客戶︰**
     - 所有最低需求和建議的需求都將顯示在市集清單中。
     - 市集將會檢查所有最低需求，而且將會對不符合需求之裝置上的客戶顯示警告。
- **對於舊版 Windows 10 的客戶：**
     - 針對大多數的客戶，所有最低硬體需求和建議的硬體需求都將顯示在市集清單中 (雖然檢視舊版市集用戶端的客戶只會看到最低硬體需求)。
     - 市集將會嘗試驗證您指定為 **\[最低硬體\]** 的項目，但 **\[記憶體\]**、**\[DirectX\]**、**\[視訊記憶體\]**、**\[圖形\]** 和 **\[處理器\]** 除外；這些項目將不會經過驗證，而且客戶將不會在不符合那些需求的裝置上看到任何警告。 
- **對於 Windows 8.x 和更舊版本或 Windows Phone 8.x 和更舊版本的客戶︰**
     - 如果您為 **\[觸控螢幕\]** 核取 **\[最低硬體\]** 方塊，此需求將會顯示在您 App 的市集清單中，而且如果沒有觸控螢幕的裝置上的客戶嘗試下載 App，將會看到一個警告。 在您的市集清單中將不會驗證或顯示其他任何需求。

我們也建議您將指定硬體的執行階段檢查新增到您的 App 中，因為市集不一定能夠偵測到客戶的裝置缺少選取的功能，而且即使顯示警告，他們仍然能夠下載您的 App。 如果您希望完全避免您的 UWP 應用遲是在不符合記憶體或 DirectX 層級最低需求的裝置上下載，您可以在 [StoreManifest XML 檔案](https://docs.microsoft.com/uwp/schemas/storemanifest/storemanifestschema2015/schema-root)中指定最低需求。

> [!TIP]
> 如果您的產品需要額外的項目才能正常運作 (例如 3D 印表機或 USB 裝置)，但該項目未在本節列出，您也可以在建立 Microsoft Store 清單時輸入[額外的系統需求](create-app-store-listings.md#additional-system-requirements)。





