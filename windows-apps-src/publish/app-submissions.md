---
Description: 一旦您已透過保留名稱來建立您的 app，您可以開始進行發行。 第一個步驟是建立提交項。
title: 應用程式提交
ms.assetid: 363BB9E4-4437-4238-A80F-ABDFC70D96E4
keywords: 檢查清單, windows, uwp, 提交項目, 提交, 遊戲, 應用程式
ms.date: 10/31/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 1359fb530dec1a35b2ab2994442b65ec441cc0ac
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89158062"
---
# <a name="app-submissions"></a>應用程式提交


當您藉 [由保留名稱來建立應用程式](create-your-app-by-reserving-a-name.md)之後，就可以開始著手進行發佈。 第一個步驟是建立**提交**。

您可以在您的 app 完成並準備好發行時開始提交，或您可以開始輸入資訊，甚至是在撰寫一行程式碼之前。 系統會儲存您對提交所做的更新，讓您可以在準備好時隨時回來處理。

> [!NOTE]
> 您必須要有使用中的 [開發人員帳戶](https://developer.microsoft.com/store/register) ，才能將應用程式提交至 Microsoft Store 的 [合作夥伴中心](https://partner.microsoft.com/dashboard) 。

發佈您的應用程式之後，您可以在合作夥伴中心中建立另一個提交來發佈更新的版本。 建立新的提交，讓您能夠在需要時進行變更並加以發佈，而不論您正在上傳的是新套件，或只是變更像是價格或類別等詳細資料。 若要為已發佈的應用程式建立新提交，請按一下 [**總覽**] 頁面上顯示的最新提交旁的 [**更新**]。 如果需要的話，您也可以 [從存放區移除應用程式](guidance-for-app-package-management.md#removing-an-app-from-the-store) (然後在稍後讓它可供使用) 。

> [!NOTE]
> 本檔的這一節說明如何在合作夥伴中心中建立應用程式提交。 或者，您可以使用 [Microsoft Store 提交 API](../monetize/create-and-manage-submissions-using-windows-store-services.md)，將應用程式提交自動化。

> [!IMPORTANT]
> 您無法再上傳使用 Windows Phone 8.x SDK (s) 建立的新 XAP 套件。 已存在於 XAP 套件中的應用程式將會繼續在 Windows 10 行動裝置版裝置上運作。 如需詳細資訊，請參閱這 [篇 blog 文章](https://blogs.windows.com/windowsdeveloper/2018/08/20/important-dates-regarding-apps-with-windows-phone-8-x-and-earlier-and-windows-8-8-1-packages-submitted-to-microsoft-store)。

## <a name="app-submission-checklist"></a>App 提交檢查清單

以下是您在建立 app 提交時可提供的詳細資料，以及指向更多資訊的連結。

您必須提供或指定的項目如下所示。 某些區域是選用的，或者會提供預設值，但您可視需要予以變更。 並不需要按照此處所列的順序來處理這些區段。

### <a name="pricing-and-availability-page"></a>定價和可用性頁面
| 欄位名稱                    | 備註                                       | 如需詳細資訊                                                             |
|-------------------------------|---------------------------------------------|---------------------------------------------------------------------------|
| **市場**                   | 預設：所有可能的市場  | [制定價格和選擇市場](./define-market-selection.md)         |
| **目標對象**                | 預設：公開對象 | [目標對象](choose-visibility-options.md#audience) |
| **可搜尋性**                | 預設：在 Microsoft Store 推出此應用程式並可供搜尋 | [可搜尋性](choose-visibility-options.md#discoverability) |
| **[排程]**                  | 預設：儘速發行        | [設定精確發行時間表](configure-precise-release-scheduling.md) |
| **基本價格**                | 必要                                    | [設定與排程應用程價格](set-and-schedule-app-pricing.md)              |
| **免費試用**                | 預設：沒有免費試用                      | [免費試用](set-app-pricing-and-availability.md#free-trial)              |
| **銷售定價**              | 選擇性                                    | [促銷應用程式和附加元件](put-apps-and-add-ons-on-sale.md)           |
| **組織授權**    | 預設：允許組織大量取得 | [組織授權選項](organizational-licensing.md)        |
      |


### <a name="properties-page"></a>屬性頁面

| 欄位名稱                    | 備註                                       | 如需詳細資訊                                                             |
|-------------------------------|---------------------------------------------|---------------------------------------------------------------------------|
| **類別與子類別**  | 必要                                    | [類別與子類別表格](category-and-subcategory-table.md)       |
| **隱私權原則 URL**            | 對許多應用程式為必要項目。 請參閱[應用程式開發人員合約](/legal/windows/agreements/app-developer-agreement)和 [Microsoft Store 原則](store-policies.md#105-personal-information) | [隱私權原則 URL](enter-app-properties.md#privacy-policy-url)        |
| **網站**                   | 選擇性                                    | [網站](enter-app-properties.md#website)                   |
| **支援連絡方式的資訊**      | 如果您的產品要在 Xbox 上提供則為必要項目，否則為選用 (但建議使用)                                   | [支援連絡方式的資訊](enter-app-properties.md#support-contact-info)              |
| **遊戲設定**             | 選用 (僅適用於遊戲)         | [遊戲設定](enter-app-properties.md#game-settings) |
| **顯示模式**             | 選擇性                   | [顯示模式](enter-app-properties.md#display-mode) |
| **產品宣告**          | 預設：客戶可將此應用程式安裝至備用磁碟機或抽取式存放裝置；Windows 可將此應用程式的資料自動備份至 OneDrive。 | [產品宣告](./product-declarations.md) |
| **系統需求**      | 選擇性                                    | [系統需求](enter-app-properties.md#system-requirements)      |

<span/>

### <a name="age-ratings-page"></a>年齡分級頁面

| 欄位名稱                    | 備註                                       | 如需詳細資訊                          |
|-------------------------------|---------------------------------------------|----------------------------------------|
| **年齡分級**               | 必要                                    | [年齡分級](age-ratings.md)          |

<span/>

### <a name="packages-page"></a>套件頁面

| 欄位名稱                    | 備註                                  | 如需詳細資訊                          |
|-------------------------------|----------------------------------------|----------------------------------------|
| **套件上傳控制項**    | 必要 (至少需要一個套件)        | [上傳應用程式套件](upload-app-packages.md) |
| **裝置系列可用性** | 預設值︰取決於您的套件       | [裝置系列可用性](device-family-availability.md) |
| **漸進式套件推出**   | 選用 (僅適用於更新)            | [漸進式套件推出](gradual-package-rollout.md) |
| **強制更新**          | 選用 (僅適用於更新)            | [強制更新](upload-app-packages.md#mandatory-update)


### <a name="store-listings"></a>市集清單

您需要 App 所支援之至少一種語言的所有必要資訊。 建議以應用程式支援的所有語言提供 [Store 清單](create-app-store-listings.md)，而且您也可以[使用其他語言提供 Store 清單](create-app-store-listings.md#store-listing-languages)。 為了更容易管理相同產品的多個清單，您可以[匯入和匯出 Store 清單](import-and-export-store-listings.md)。

| 欄位名稱                    | 備註                                       | 如需詳細資訊                                                     |
|-------------------------------|---------------------------------------------|-------------------------------------------------------------------|
| **描述**               | 必要                                    | [撰寫一份出色的應用程式介紹](write-a-great-app-description.md) |
| **此版本的新功能**   | 選擇性                                 | [版本資訊](create-app-store-listings.md#whats-new-in-this-version)       |
| **應用程式功能**              | 選擇性                                    | [產品功能](create-app-store-listings.md#product-features)         |
| **螢幕擷取畫面**               | 必要 (至少一個螢幕擷取畫面，建議提供四個或更多)          | [螢幕擷取畫面](app-screenshots-and-images.md#screenshots)          |
| **儲存標誌**               | 建議使用；某些 OS 版本為必要 | [儲存標誌](app-screenshots-and-images.md#store-logos)             |
| **預告片**                  | 選擇性                                    | [預告片](app-screenshots-and-images.md#trailers)                | 
| **Windows 10 和 Xbox 影像 (16:9 超級主角美工圖案)**     | 建議        | [Windows 10 和 Xbox 影像 (16:9 超級主角美工圖案)](app-screenshots-and-images.md#windows-10-and-xbox-image-169-super-hero-art) |
| **Xbox 影像**     | 當您發行至 Xbox 時需要適當的顯示        | [Xbox 影像](app-screenshots-and-images.md#xbox-images) |
| **補充欄位**  | 選擇性                                    | [補充欄位](create-app-store-listings.md#supplemental-fields) 
| **搜尋詞彙**              | 選擇性                                    | [搜尋詞彙](create-app-store-listings.md#search-terms)         |
| **著作權與商標資訊** | 選擇性                                 | [著作權與商標資訊](create-app-store-listings.md#copyright-and-trademark-info) |
| **其他授權條款**  | 選擇性                                    | [其他授權條款](create-app-store-listings.md#additional-license-terms) |
| **開發者**              | 選擇性                                    | [開發者](create-app-store-listings.md#developed-by)                   |


<span/>

### <a name="submission-options-page"></a>提交選項頁面

| 欄位名稱                    | 備註                                       | 如需詳細資訊                                                     |
|-------------------------------|---------------------------------------------|-------------------------------------------------------------------|
| **發佈暫停選項**     | 預設︰通過認證之後就立即發佈此提交項目 (或根據您在 \[排程\] 區段中選取的日期)      | [發佈暫停選項](manage-submission-options.md#publishing-hold-options)    
| **認證注意事項**     | 建議          | [認證注意事項](notes-for-certification.md)             |
| **受限功能**     | 如果您的產品宣告任何[受限的功能](../packaging/app-capability-declarations.md#restricted-capabilities)，則為必要    | [受限功能](manage-submission-options.md#publishing-hold-options)       

<span/>

> [!NOTE]
> 如需直接將企業營運 (LOB) 應用程式發佈到企業的相關資訊，請參閱[將 LOB App 發佈到企業](distribute-lob-apps-to-enterprises.md)。