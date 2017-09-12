---
author: jnHs
Description: "一旦您已透過保留名稱來建立您的 app，您可以開始進行發行。 第一個步驟是建立提交項。"
title: "App 提交"
ms.assetid: 363BB9E4-4437-4238-A80F-ABDFC70D96E4
keywords: "檢查清單"
ms.author: wdg-dev-content
ms.date: 08/03/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.openlocfilehash: fdef30d07386a1c5ab7dc6bd62b9507ff5852194
ms.sourcegitcommit: 968187e803a866b60cda0528718a3d31f07dc54c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/03/2017
---
# <a name="app-submissions"></a>App 提交


一旦您已[透過保留名稱來建立您的 app](create-your-app-by-reserving-a-name.md)，您就可以開始進行發行。 第一個步驟是建立**提交**。

您可以在您的 app 完成並準備好發行時開始提交，或您可以開始輸入資訊，甚至是在撰寫一行程式碼之前。 提交將儲存於儀表板中，如此一來，每當您準備好時，即可加以使用。

發佈您的 app 之後，就可以在儀表板中建立其他提交來發佈更新版本。 建立新的提交，讓您能夠在需要時進行變更並加以發佈，而不論您正在上傳的是新套件，或只是變更像是價格或類別等詳細資料。 若要為已發行的應用程式建立新的提交，請按一下 [應用程式概觀] 頁面上所顯示最新提交旁的 [**更新**]。

> [!NOTE]
> 本文件章節描述如何在開發人員中心儀表板上建立應用程式提交。 或者，您可以使用 [Windows 市集提交 API](../monetize/create-and-manage-submissions-using-windows-store-services.md)，將 App 提交自動化。

## <a name="app-submission-checklist"></a>App 提交檢查清單

以下是您在建立 app 提交時可提供的詳細資料，以及指向更多資訊的連結。

您必須提供或指定的項目如下所示。 某些區域是選用的，或者會提供預設值，但您可視需要予以變更。

### <a name="pricing-and-availability-page"></a>定價和可用性頁面
| 欄位名稱                    | 注意事項                                       | 如需詳細資訊                                                             |
|-------------------------------|---------------------------------------------|---------------------------------------------------------------------------|
| **市場**                   | 預設：所有可能的市場，  | [制定價格和選擇市場](define-pricing-and-market-selection.md)         |
| **可見度**                | 預設：在市集推出此應用程式並可供搜尋 | [可見度](set-app-pricing-and-availability.md#visibility) |
| **排程**                  | 預設：儘速發行        | [設定精確發行時間表](configure-precise-release-scheduling.md) |
| **基本價格**                | 必要                                    | [設定與排程應用程價格](set-and-schedule-app-pricing.md)              |
| **免費試用**                | 預設：沒有免費試用                      | [免費試用](set-app-pricing-and-availability.md#free-trial)              |
| **銷售定價**              | 選用                                    | [促銷應用程式和附加元件](put-apps-and-add-ons-on-sale.md)           |
| **組織授權**    | 預設：允許組織大量取得 | [組織授權選項](organizational-licensing.md)        |
| **發佈日期**                | 預設值：儘速發佈      | [發佈日期](set-app-pricing-and-availability.md#publish-date)          |

<span/>

### <a name="properties-page"></a>屬性頁面

| 欄位名稱                    | 注意事項                                       | 如需詳細資訊                                                             |
|-------------------------------|---------------------------------------------|---------------------------------------------------------------------------|
| **類別與子類別**  | 必要                                    | [類別與子類別表格](category-and-subcategory-table.md)       |
| **系統需求**      | 選用                                    | [系統需求](enter-app-properties.md#system-requirements)      |
| **產品宣告**          | 預設：客戶可將此應用程式安裝至備用磁碟機或抽取式存放裝置；Windows 可將此應用程式的資料自動備份至 OneDrive。 | [產品宣告](app-declarations.md) |

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
| **裝置系列可用性** | 預設值︰取決於您的套件       | [裝置系列可用性](upload-app-packages.md#device-family-availability) |
| **漸進式套件推出**   | 選用 (僅適用於更新)            | [漸進式套件推出](gradual-package-rollout.md) |
| **強制更新**          | 選用 (僅適用於更新)            | [強制更新](upload-app-packages.md#mandatory-update)

<span/>

### <a name="store-listings"></a>市集清單

您需要 App 所支援之至少一種語言的所有必要資訊。 建議以 App 支援的所有語言提供[市集清單](create-app-store-listings.md)，而且您也可以[使用其他語言提供市集清單](create-app-store-listings.md#store-listing-languages)。

| 欄位名稱                    | 注意事項                                       | 如需詳細資訊                                                     |
|-------------------------------|---------------------------------------------|-------------------------------------------------------------------|
| **說明**               | 必要                                    | [撰寫一份出色的 app 介紹](write-a-great-app-description.md) |
| **版本資訊**             | 選用                                    | [版本資訊](create-app-store-listings.md#release-notes)       |
| **螢幕擷取畫面**               | 必要 (至少需要一個螢幕擷取畫面)          | [螢幕擷取畫面](app-screenshots-and-images.md#screenshots)          |
| **市集標誌**               | 選用，但強烈建議用於 Windows Phone 8.1 和較舊版本 | [市集標誌](app-screenshots-and-images.md#store-logos)             |
| **宣傳影像**        | 選用                                    | [宣傳影像](app-screenshots-and-images.md#promotional-images) |
| **Xbox 影像**               | 選用                                    | [Xbox 影像](app-screenshots-and-images.md#xbox-images)              |
| **選用宣傳影像**       | 選用                            | [選用宣傳影像](app-screenshots-and-images.md#optional-promotional-images)       |
| **預告片**                  | 選用                                    | [預告片](app-screenshots-and-images.md#trailers)                | 
| **應用程式功能**              | 選用                                    | [功能](create-app-store-listings.md#app-features)             |
| **其他系統需求**      | 選用                                    | [其他系統需求](create-app-store-listings.md#additional-system-requirements) 
| **搜尋詞彙**              | 選用                                    | [搜尋詞彙](create-app-store-listings.md#search-terms)         |
| **隱私權原則**            | 部分 app 的必要項目。 請參閱[應用程式開發人員合約](https://msdn.microsoft.com/library/windows/apps/hh694058)和 [Windows 市集原則](https://msdn.microsoft.com/library/windows/apps/dn764944.aspx#pol_10_5_1) | [隱私權原則](create-app-store-listings.md#privacy-policy)        |
| **著作權與商標資訊** | 選用                                 | [著作權與商標資訊](create-app-store-listings.md#copyright-and-trademark-info) |
| **其他授權條款**  | 選用                                    | [其他授權條款](create-app-store-listings.md#additional-license-terms) |
| **網站**                   | 選用                                    | [網站](create-app-store-listings.md#website)                   |
| **支援連絡方式的資訊**      | 選用                                    | [支援連絡方式的資訊](create-app-store-listings.md)              |
| **平台專屬的市集清單** | 選用                               | [建立平台專屬的市集清單](create-platform-specific-store-listings.md)  |

<span/>

### <a name="notes-for-certification-page"></a>認證注意事項頁面

| 欄位名稱                    | 注意事項                                       | 如需詳細資訊                                                     |
|-------------------------------|---------------------------------------------|-------------------------------------------------------------------|
| **注意事項**                     | 選用                                    | [認證注意事項](notes-for-certification.md)             |

<span/>

**注意**&nbsp;&nbsp;如需直接將企業營運 (LOB) App 發佈到企業的相關資訊，請參閱[將 LOB App 發佈到企業](distribute-lob-apps-to-enterprises.md)。
