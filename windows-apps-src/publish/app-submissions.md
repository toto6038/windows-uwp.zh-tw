---
Description: 一旦您已透過保留名稱來建立您的 app，您可以開始進行發行。 第一個步驟是建立提交項。
title: 應用程式提交
ms.assetid: 363BB9E4-4437-4238-A80F-ABDFC70D96E4
keywords: 檢查清單, windows, uwp, 提交項目, 提交, 遊戲, 應用程式
ms.date: 10/31/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: b98ea7f1d28c4fcd63cd2d4706905578b240e126
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57643283"
---
# <a name="app-submissions"></a>應用程式提交


一旦您已[透過保留名稱來建立您的 app](create-your-app-by-reserving-a-name.md)，您就可以開始進行發行。 第一個步驟是建立**提交**。

您可以在您的 app 完成並準備好發行時開始提交，或您可以開始輸入資訊，甚至是在撰寫一行程式碼之前。 您對您的提交的更新會儲存，因此您可以回到這裡並在其上運作，您可以隨時。

> [!NOTE]
> 您必須具備作用[開發人員帳戶](https://go.microsoft.com/fwlink/p/?LinkId=615100)中[合作夥伴中心](https://partner.microsoft.com/dashboard)才能提交至 Microsoft Store 應用程式。

發行您的應用程式之後，您可以藉由在合作夥伴中心內建立另一個提交來發佈更新的版本。 建立新的提交，讓您能夠在需要時進行變更並加以發佈，而不論您正在上傳的是新套件，或只是變更像是價格或類別等詳細資料。 若要建立已發佈的應用程式的新送出，請按一下**更新**旁顯示的最新送出其**概觀**頁面。 您也可以[移除存放區中的應用程式](guidance-for-app-package-management.md#removing-an-app-from-the-store)如果您要執行這項操作 （，然後將它提供一次更新的版本，如果您想要）。

> [!NOTE]
> 本節的文件說明如何在合作夥伴中心內建立應用程式提交。 或者，您可以使用 [Microsoft Store 提交 API](../monetize/create-and-manage-submissions-using-windows-store-services.md)，將應用程式提交自動化。

> [!IMPORTANT]
> 自 2018 年 10 月 31 日起，新建立的產品不能包含封裝目標 Windows 8.x/Windows Phone 8.x 或更早版本。 如需詳細資訊，請參閱此[部落格文章](https://blogs.windows.com/buildingapps/2018/08/20/important-dates-regarding-apps-with-windows-phone-8-x-and-earlier-and-windows-8-8-1-packages-submitted-to-microsoft-store/#SzKghBbqDMlmAO4c.97)。

## <a name="app-submission-checklist"></a>App 提交檢查清單

以下是您在建立 app 提交時可提供的詳細資料，以及指向更多資訊的連結。

您必須提供或指定的項目如下所示。 某些區域是選用的，或者會提供預設值，但您可視需要予以變更。 您不需要作用於這些章節，此處所列的順序。

### <a name="pricing-and-availability-page"></a>定價和可用性頁面
| 欄位名稱                    | 附註                                       | 如需詳細資訊                                                             |
|-------------------------------|---------------------------------------------|---------------------------------------------------------------------------|
| **市場**                   | Default：所有可能的市場  | [定義定價和市場的選取範圍](define-pricing-and-market-selection.md)         |
| **對象**                | Default：公開對象 | [對象](choose-visibility-options.md#audience) |
| **可搜尋性**                | Default：在市集中可用且可探索製作此應用程式 | [可搜尋性](choose-visibility-options.md#discoverability) |
| **排程**                  | Default：儘速版本        | [設定精確的發行排程](configure-precise-release-scheduling.md) |
| **基本價格**                | 必要                                    | [設定和排程應用程式的定價](set-and-schedule-app-pricing.md)              |
| **免費試用**                | Default：沒有免費試用                      | [免費試用](set-app-pricing-and-availability.md#free-trial)              |
| **銷售定價**              | 選擇性                                    | [促銷 App 和附加元件](put-apps-and-add-ons-on-sale.md)           |
| **組織的授權**    | Default：允許組織的大量擷取 | [組織的授權選項](organizational-licensing.md)        |
      |


### <a name="properties-page"></a>屬性頁面

| 欄位名稱                    | 附註                                       | 如需詳細資訊                                                             |
|-------------------------------|---------------------------------------------|---------------------------------------------------------------------------|
| **類別目錄和子類別目錄**  | 必要                                    | [類別目錄和子類別目錄的資料表](category-and-subcategory-table.md)       |
| **隱私權原則 URL**            | 對許多應用程式為必要項目。 請參閱[應用程式開發人員合約](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement)和 [Microsoft Store 原則](https://docs.microsoft.com/en-us/legal/windows/agreements/store-policies#105-personal-information) | [隱私權原則 URL](enter-app-properties.md#privacy-policy-url)        |
| **網站**                   | 選擇性                                    | [網站](enter-app-properties.md#website)                   |
| **支援連絡資訊**      | 如果您的產品要在 Xbox 上提供則為必要項目，否則為選用 (但建議使用)                                   | [支援連絡資訊](enter-app-properties.md#support-contact-info)              |
| **遊戲的設定**             | 選用 (僅適用於遊戲)         | [遊戲的設定](enter-app-properties.md#game-settings) |
| **顯示模式**             | 選擇性                   | [顯示模式](enter-app-properties.md#display-mode) |
| **產品宣告**          | Default：客戶可在此應用程式安裝到其他磁碟機或抽取式存放裝置;Windows 可以包含在自動備份到 OneDrive 中的此應用程式的資料 | [產品宣告](app-declarations.md) |
| **系統需求**      | 選擇性                                    | [系統需求](enter-app-properties.md#system-requirements)      |

<span/>

### <a name="age-ratings-page"></a>年齡分級頁面

| 欄位名稱                    | 附註                                       | 如需詳細資訊                          |
|-------------------------------|---------------------------------------------|----------------------------------------|
| **年齡分級**               | 必要                                    | [年齡分級](age-ratings.md)          |

<span/>

### <a name="packages-page"></a>套件頁面

| 欄位名稱                    | 附註                                  | 如需詳細資訊                          |
|-------------------------------|----------------------------------------|----------------------------------------|
| **封裝上傳控制項**    | 必要 (至少需要一個套件)        | [上傳應用程式套件](upload-app-packages.md) |
| **裝置系列的可用性** | 預設值︰取決於您的套件       | [裝置系列的可用性](device-family-availability.md) |
| **漸進式封裝首度發行**   | 選用 (僅適用於更新)            | [漸進式封裝首度發行](gradual-package-rollout.md) |
| **強制更新**          | 選用 (僅適用於更新)            | [強制更新](upload-app-packages.md#mandatory-update)


### <a name="store-listings"></a>市集清單

您需要 App 所支援之至少一種語言的所有必要資訊。 建議以 App 支援的所有語言提供[市集清單](create-app-store-listings.md)，而且您也可以[使用其他語言提供市集清單](create-app-store-listings.md#store-listing-languages)。 為了更容易管理相同產品的多個清單，您可以[匯入和匯出 Store 清單](import-and-export-store-listings.md)。

| 欄位名稱                    | 附註                                       | 如需詳細資訊                                                     |
|-------------------------------|---------------------------------------------|-------------------------------------------------------------------|
| **描述**               | 必要                                    | [寫入的絕佳應用程式的描述](write-a-great-app-description.md) |
| **在這個版本中最新消息**   | 選擇性                                 | [版本資訊](create-app-store-listings.md#whats-new-in-this-version)       |
| **應用程式功能**              | 選擇性                                    | [產品功能](create-app-store-listings.md#product-features)         |
| **螢幕擷取畫面**               | 必要 (至少一個螢幕擷取畫面，建議提供四個或更多)          | [螢幕擷取畫面](app-screenshots-and-images.md#screenshots)          |
| **市集標誌**               | 建議使用；某些 OS 版本為必要 | [市集標誌](app-screenshots-and-images.md#store-logos)             |
| **結尾**                  | 選擇性                                    | [結尾](app-screenshots-and-images.md#trailers)                | 
| **Windows 10 和 Xbox 的映像 （16:9 進階 hero 圖案）**     | 推薦項目        | [Windows 10 和 Xbox 映像 （16:9 進階 hero 圖案）
] (app-screenshots-and-images.md#windows-10-and-xbox-image-169-super-hero-art) |
| **Xbox 映像**     | 如果您發行至 Xbox，需要正確地顯示        | [Xbox 映像
] (應用程式-螢幕擷取畫面-和-images.md #xbox 映像) |
| **補充的欄位**  | 選擇性                                    | [補充的欄位](create-app-store-listings.md#supplemental-fields) 
| **搜尋字詞**              | 選擇性                                    | [搜尋字詞](create-app-store-listings.md#search-terms)         |
| **著作權及商標資訊** | 選擇性                                 | [著作權及商標資訊](create-app-store-listings.md#copyright-and-trademark-info) |
| **其他授權條款**  | 選擇性                                    | [其他授權條款](create-app-store-listings.md#additional-license-terms) |
| **由開發**              | 選擇性                                    | [由開發](create-app-store-listings.md#developed-by)                   |


<span/>

### <a name="submission-options-page"></a>提交選項頁面

| 欄位名稱                    | 附註                                       | 如需詳細資訊                                                     |
|-------------------------------|---------------------------------------------|-------------------------------------------------------------------|
| **發行的保留選項**     | Default：發行此提交作業，只要它會傳遞認證 （或您選取 [排程] 區段中每個日期）      | [發行的保留選項](manage-submission-options.md#publishing-hold-options)    
| **憑證的相關資訊**     | 推薦項目          | [憑證的相關資訊](notes-for-certification.md)             |
| **受限制的功能**     | 如果您的產品會宣告任何所需[限制功能](../packaging/app-capability-declarations.md#restricted-capabilities)    | [受限制的功能](manage-submission-options.md#publishing-hold-options)       

<span/>

> [!NOTE]
> 如需直接將企業營運 (LOB) 應用程式發佈到企業的相關資訊，請參閱[將 LOB App 發佈到企業](distribute-lob-apps-to-enterprises.md)。
