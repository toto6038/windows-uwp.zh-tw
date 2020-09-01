---
Description: App 提交程序的 [應用程式屬性] 頁面可讓您定義 app 的類別，並指出硬體喜好設定或其他宣告。
title: 輸入應用程式屬性
ms.assetid: CDE4AF96-95A0-4635-9D07-A27B810CAE26
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, 遊戲設定, 顯示模式, 系統需求, 硬體需求, 基本硬體, 建議的硬體, 隱私權原則, 支援連絡資訊, 應用程式網站, 支援資訊
ms.localizationpriority: medium
ms.openlocfilehash: f945b9908a86d660bde9713ca353f1f3a438bf90
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89167402"
---
# <a name="enter-app-properties"></a>輸入應用程式屬性

[應用程式提交程序](app-submissions.md)的 [**屬性**] 頁面可用來定義應用程式的類別，並輸入其他資訊和宣告。 請務必在此頁面上提供有關您應用程式完整且準確的詳細資料。


## <a name="category-and-subcategory"></a>類別與子類別

您必須指示 Microsoft Store 要用來分類您的應用程式的類別 (與子類別/內容類型，如果適用的話)。 您必須指定類別才能送出應用程式。

如需詳細資訊，請參閱[類別與子類別表格](category-and-subcategory-table.md)。


## <a name="support-info"></a>支援資訊

此區段可讓您提供資訊，以協助客戶更深入了解您的應用程式，以及取得支援的方式。

### <a name="privacy-policy-url"></a>隱私權原則 URL

您必須負責確定您的應用程式遵循隱私權的法律與法規，如有必要並在此處提供有效的隱私權原則 URL。

您必須在本區段中，指出您的應用程式是否會存取、收集或傳送任何[個人資訊](/legal/windows/agreements/store-policies#105-personal-information)。 如果您回答 **\[是\]**，就需要提供隱私權原則 URL。 否則，這是選用的 (但若我們判斷您的應用程式需要隱私權原則，但您並未提供的話，您的提交項目可能會無法通過認證)。

> [!NOTE]
> 如果我們偵測到您的套件所宣告的[功能](../packaging/app-capability-declarations.md)可能會允許存取、傳輸或收集個人資訊，我們會將此問題標示為 **\[是\]**，並要求您輸入隱私權原則 URL。

為了協助您判斷您的應用程式是否需要隱私權原則，請檢閱[應用程式開發人員合約](/legal/windows/agreements/app-developer-agreement)和 [Microsoft Store 原則](/legal/windows/agreements/store-policies#105-personal-information)。 

> [!NOTE]
> Microsoft 不會為您的應用程式提供預設隱私權原則。 同樣地，所有的 Microsoft 隱私權原則都不會涵蓋您的應用程式。 


### <a name="website"></a>網站

請輸入您 app 網頁的 URL。 這個 URL 必須指向您自己網站的頁面，而不是 Microsoft Store 中您 app 的網站清單。 這個欄位是選用項目，但建議使用。

### <a name="support-contact-info"></a>支援連絡方式的資訊

輸入您的客戶可取得應用程式支援的網頁 URL，或客戶可聯絡以取得支援的電子郵件地址。 我們建議您對所有提交加入這項資訊，好讓您的客戶了解在需要時如何取得支援。 請注意，Microsoft 不會向客戶提供有關您應用程式的支援。

> [!IMPORTANT]
> 如果您要在 Xbox 上提供您的應用程式或遊戲，**\[支援連絡資訊\]** 欄位為必要。 否則，這是選用的 (但建議使用)。


## <a name="game-settings"></a>遊戲設定

您選取 [**遊戲**] 做為產品類別時，此區段才會出現。 您可在此指定遊戲支援的功能。 您在本節中提供的資訊會顯示在產品的 Store 清單上。

如果您的遊戲支援任何多人遊戲選項，請務必指出一場遊戲可參與的玩家人數下限和上限。 您無法輸入超過 1,000 名玩家下限或上限。

**跨平台多人遊戲**表示，遊戲支援在 Windows 10 電腦和 Xbox 之間的多人工作階段。


## <a name="display-mode"></a>顯示模式

此區段可讓您指出您的產品的設計目的是要在電腦和/或 HoloLens 裝置上的 [Windows Mixed Reality](https://developer.microsoft.com/mixed-reality) 沈浸式 (而非 2D) 檢視中執行。 如果是這樣，您也必須：
- 在**\[屬性\]** 頁面下半部顯示的[系統需求](#system-requirements)區段中，選取 **\[Windows Mixed Reality 沉浸式頭戴裝置\]** 的 **\[基本硬體\]** 或 **\[建議硬體\]**。
- 指定 **\[界限設定\]** (如果已選取電腦)，讓使用者知這原本是要僅限以坐姿或站姿使用，或者是否允許 (或要求) 使用者在使用時四處移動。 

如果您已選取 **\[遊戲\]** 做為產品的類別，您會在 **\[顯示模式\]** 選項中看到額外選項，讓您表示產品是否支援 4K 解析度視訊輸出、高動態範圍 (HDR) 視訊輸出或可變更新頻率顯示器。

如果您的產品不支援這些顯示模式選項，請讓所有的方塊保持未選取狀態。


## <a name="product-declarations"></a>產品宣告

您可以核取此區段中的方塊以指出是否有任何宣告適用於您的 App。 這可能會影響應用程式顯示的方式、應用程式是否會提供給某些客戶，或者客戶可以使用應用程式的方式。

如需詳細資訊，請參閱 [產品宣告](./product-declarations.md)。

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

我們也建議您將指定硬體的執行階段檢查新增到您的 App 中，因為市集不一定能夠偵測到客戶的裝置缺少選取的功能，而且即使顯示警告，他們仍然能夠下載您的 App。 如果您想要完全防止 UWP 應用程式在不符合記憶體或 DirectX 層級最低需求的裝置上下載，您可以在 [STOREMANIFEST.XML XML](/uwp/schemas/storemanifest/storemanifestschema2015/schema-root)檔案中指定最小需求。

> [!TIP]
> 如果您的產品需要額外的項目才能正常運作 (例如 3D 印表機或 USB 裝置)，但該項目未在本節列出，您也可以在建立 Microsoft Store 清單時輸入[額外的系統需求](create-app-store-listings.md#additional-system-requirements)。