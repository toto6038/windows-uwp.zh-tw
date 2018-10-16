---
author: jnHs
Description: Once you've created your app by reserving a name, you can start working on getting it published. The first step is to create a submission.
title: 應用程式提交
ms.assetid: 363BB9E4-4437-4238-A80F-ABDFC70D96E4
keywords: 檢查清單, windows, uwp, 提交項目, 提交, 遊戲, 應用程式
ms.author: wdg-dev-content
ms.date: 08/07/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: 9802577f9252b590657406bcb59b0c28adeb4781
ms.sourcegitcommit: 9354909f9351b9635bee9bb2dc62db60d2d70107
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/16/2018
ms.locfileid: "4691134"
---
# <a name="app-submissions"></a>App 提交


一旦您已[透過保留名稱來建立您的 app](create-your-app-by-reserving-a-name.md)，您就可以開始進行發行。 第一個步驟是建立**提交**。

您可以在您的 app 完成並準備好發行時開始提交，或您可以開始輸入資訊，甚至是在撰寫一行程式碼之前。 儲存您對您的提交進行的更新，讓您可以回來，並在其上運作，每當您準備好。

> [!NOTE]
> 您必須有[開發人員帳戶](http://go.microsoft.com/fwlink/p/?LinkId=615100)才能存取[Windows 開發人員中心 」](https://partner.microsoft.com/dashboard)並提交到 Microsoft 網上商店的應用程式。

發佈您的 app 之後，就可以在儀表板中建立其他提交來發佈更新版本。 建立新的提交，讓您能夠在需要時進行變更並加以發佈，而不論您正在上傳的是新套件，或只是變更像是價格或類別等詳細資料。 若要為已發行的應用程式建立新的提交，請按一下 [應用程式概觀] 頁面上所顯示最新提交旁的 [**更新**]。 您也可以[移除從 microsoft Store 應用程式](guidance-for-app-package-management.md#removing-an-app-from-the-store)若要這樣做 （，然後使其可再次更新的版本，如果您想要）。

> [!NOTE]
> 本文件章節描述如何在開發人員中心儀表板上建立應用程式提交。 或者，您可以使用 [Microsoft Store 提交 API](../monetize/create-and-manage-submissions-using-windows-store-services.md)，將應用程式提交自動化。

## <a name="app-submission-checklist"></a>App 提交檢查清單

以下是您在建立 app 提交時可提供的詳細資料，以及指向更多資訊的連結。

您必須提供或指定的項目如下所示。 某些區域是選用的，或者會提供預設值，但您可視需要予以變更。 您不需要在此處所列的順序這些區段上運作。

### <a name="pricing-and-availability-page"></a>定價和可用性頁面
| 欄位名稱                    | 注意事項                                       | 如需詳細資訊                                                             |
|-------------------------------|---------------------------------------------|---------------------------------------------------------------------------|
| **市場**                   | 預設：所有可能的市場  | [制定價格和選擇市場](define-pricing-and-market-selection.md)         |
| **對象**                | 預設：公開對象 | [對象](choose-visibility-options.md#audience) |
| **可搜尋性**                | 預設：在 Microsoft Store 推出此應用程式並可供搜尋 | [可搜尋性](choose-visibility-options.md#discoverability) |
| **排程**                  | 預設：儘速發行        | [設定精確發行時間表](configure-precise-release-scheduling.md) |
| **基本價格**                | 必要                                    | [設定與排程應用程價格](set-and-schedule-app-pricing.md)              |
| **免費試用**                | 預設：沒有免費試用                      | [免費試用](set-app-pricing-and-availability.md#free-trial)              |
| **銷售定價**              | 選用                                    | [促銷應用程式和附加元件](put-apps-and-add-ons-on-sale.md)           |
| **組織授權**    | 預設：允許組織大量取得 | [組織授權選項](organizational-licensing.md)        |
      |


### <a name="properties-page"></a>內容頁面

| 欄位名稱                    | 注意事項                                       | 如需詳細資訊                                                             |
|-------------------------------|---------------------------------------------|---------------------------------------------------------------------------|
| **類別與子類別**  | 必要                                    | [類別與子類別表格](category-and-subcategory-table.md)       |
| **隱私權原則 URL**            | 對許多應用程式為必要項目。 請參閱[應用程式開發人員合約](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement)和 [Microsoft Store 原則](https://docs.microsoft.com/en-us/legal/windows/agreements/store-policies#105-personal-information) | [隱私權原則 URL](enter-app-properties.md#privacy-policy-url)        |
| **網站**                   | 選用                                    | [網站](enter-app-properties.md#website)                   |
| **支援連絡資訊**      | 如果您的產品要在 Xbox 上提供則為必要項目，否則為選用 (但建議使用)                                   | [支援連絡資訊](enter-app-properties.md#support-contact-info)              |
| **遊戲設定**             | 選用 (僅適用於遊戲)         | [遊戲設定](enter-app-properties.md#game-settings) |
| **顯示模式**             | 選用                   | [顯示模式](enter-app-properties.md#display-mode) |
| **產品宣告**          | 預設：客戶可將此應用程式安裝至備用磁碟機或抽取式存放裝置；Windows 可將此應用程式的資料自動備份至 OneDrive。 | [產品宣告](app-declarations.md) |
| **系統需求**      | 選用                                    | [系統需求](enter-app-properties.md#system-requirements)      |

<span/>

### <a name="age-ratings-page"></a>年齡分級頁面

| 欄位名稱                    | 注意事項                                       | 如需詳細資訊                          |
|-------------------------------|---------------------------------------------|----------------------------------------|
| **年齡分級**               | 必要                                    | [年齡分級](age-ratings.md)          |

<span/>

### <a name="packages-page"></a>套件頁面

| 欄位名稱                    | 注意事項                                  | 如需詳細資訊                          |
|-------------------------------|----------------------------------------|----------------------------------------|
| **套件上傳控制項**    | 必要 (至少需要一個套件)        | [上傳應用程式套件](upload-app-packages.md) |
| **裝置系列可用性** | 預設值︰取決於您的套件       | [裝置系列可用性](device-family-availability.md) |
| **漸進式套件推出**   | 選用 (僅適用於更新)            | [漸進式套件推出](gradual-package-rollout.md) |
| **強制更新**          | 選用 (僅適用於更新)            | [強制更新](upload-app-packages.md#mandatory-update)


### <a name="store-listings"></a>市集清單

您需要 App 所支援之至少一種語言的所有必要資訊。 建議以應用程式支援的所有語言提供 [Store 清單](create-app-store-listings.md)，而且您也可以[使用其他語言提供 Store 清單](create-app-store-listings.md#store-listing-languages)。 為了更容易管理相同產品的多個清單，您可以[匯入和匯出 Store 清單](import-and-export-store-listings.md)。

| 欄位名稱                    | 注意事項                                       | 如需詳細資訊                                                     |
|-------------------------------|---------------------------------------------|-------------------------------------------------------------------|
| **說明**               | 必要                                    | [撰寫一份出色的應用程式介紹](write-a-great-app-description.md) |
| **此版本中的新功能**   | 選用                                 | [版本資訊](create-app-store-listings.md#whats-new-in-this-version)       |
| **應用程式功能**              | 選用                                    | [應用程式功能](create-app-store-listings.md#app-features)         |
| **螢幕擷取畫面**               | 必要 (至少一個螢幕擷取畫面，建議提供四個或更多)          | [螢幕擷取畫面](app-screenshots-and-images.md#screenshots)          |
| **Microsoft Store 標誌**               | 建議使用；某些 OS 版本為必要 | [Microsoft Store 標誌](app-screenshots-and-images.md#store-logos)             |
| **其他美工圖案資產**     | 建議使用 (尤其對某些 OS 版本)         | [其他美工圖案資產](app-screenshots-and-images.md#additional-art-assets) |
| **預告片**                  | 選用                                    | [預告片](app-screenshots-and-images.md#trailers)                | 
| **補充的欄位**  | 選用                                    | [補充資訊](create-app-store-listings.md#supplemental-fields) 
| **搜尋詞彙**              | 選用                                    | [搜尋詞彙](create-app-store-listings.md#search-terms)         |
| **著作權與商標資訊** | 選用                                 | [著作權與商標資訊](create-app-store-listings.md#copyright-and-trademark-info) |
| **其他授權條款**  | 選用                                    | [其他授權條款](create-app-store-listings.md#additional-license-terms) |
| **開發者**              | 選用                                    | [開發者](create-app-store-listings.md#developed-by)                   |
| **平台專屬的 Store 清單** | 選用                               | [建立平台專屬的 Store 清單](create-platform-specific-store-listings.md)  |

<span/>

### <a name="submission-options-page"></a>提交選項頁面

| 欄位名稱                    | 注意事項                                       | 如需詳細資訊                                                     |
|-------------------------------|---------------------------------------------|-------------------------------------------------------------------|
| **發佈暫停選項**     | 預設︰通過認證之後就立即發佈此提交項目 (或根據您在 \[排程\] 區段中選取的日期)      | [發佈暫停選項](manage-submission-options.md#publishing-hold-options)    
| **認證注意事項**     | 推薦項目          | [認證注意事項](notes-for-certification.md)             |
| **受限制的功能**     | 如果您的產品宣告任何[受限制的功能](../packaging/app-capability-declarations.md#restricted-capabilities)所需    | [受限制的功能](manage-submission-options.md#publishing-hold-options)       

<span/>

> [!NOTE]
> 如需直接將企業營運 (LOB) 應用程式發佈到企業的相關資訊，請參閱[將 LOB App 發佈到企業](distribute-lob-apps-to-enterprises.md)。
