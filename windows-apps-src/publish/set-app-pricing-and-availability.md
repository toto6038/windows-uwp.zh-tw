---
Description: The Pricing and availability page of the app submission process lets you determine how much your app will cost, whether you'll offer a free trial, and how, when, and where it will be available to customers.
title: 設定 App 價格與可用性
ms.assetid: 37BE7C25-AA74-43CD-8969-CBA3BD481575
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, 價格, 提供, 可搜尋的, 免費試用, 試用, 試用版, 應用程式, 發行日期
ms.localizationpriority: medium
ms.openlocfilehash: d5fa6c3e23516a5255f8bd3252f6ded233101625
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/30/2018
ms.locfileid: "8205953"
---
# <a name="set-app-pricing-and-availability"></a>設定應用程式價格與可用性


您可以從 [app 提交程序](app-submissions.md)的 **\[價格與可用性\]** 頁面決定您的 app 價格、您是否提供免費試用，以及客戶如何、何時、何處可取得您的 app。 我們將在這裡逐步解說此頁面上的選項，以及輸入這項資訊時所應考慮的事項。


## <a name="markets"></a>市場

Microsoft Store 可觸及全球 200 多個國家與地區的客戶。 根據預設，我們會在所有可能的市場提供您的應用程式。 如果您想要的話，可以選擇想要提供應用程式的特定市場。 

如需詳細資訊，請參閱[定義市場選取項目](define-pricing-and-market-selection.md)。


## <a name="visibility"></a>可見度

**\[可見度\]** 區段可讓您設定如何搜尋和取得您的應用程式，包括人們是否可以在 Microsoft Store 中找到您的應用程式或甚至是查看其 Store 清單。

如需詳細資訊，請參閱[選擇可見度選項](choose-visibility-options.md)。


## <a name="schedule"></a>排程

根據預設 (除非您已選取[可見度](choose-visibility-options.md#discoverability)區段的其中一個 **\[在 Microsoft Store 推出此應用程式，但不供搜尋\]** 選項)，您的應用程式將在通過認證並完成發行程序後，會立即對客戶提供。 若要選擇其他日期，請選取 [**顯示選項**] 展開這個區段。 

如需詳細資訊，請參閱[設定精確發行時間表](configure-precise-release-scheduling.md)。


## <a name="pricing"></a>定價

您必須選取應用程式的基本價格 (除非您已選取[可見度](choose-visibility-options.md#discoverability)區段中，**\[在 Microsoft Store 推出此應用程式，但不供搜尋\]** 下方的 **\[停止取得\]** 選項)，選擇是否 **\[免費\]** 提供，或是其中一個可用的價格區間。 您也可以排程價格變更，表示應用程式的價格應變更的日期和時間。 此外，您可以選擇針對特定市場自訂這些變更。 

如需詳細資訊，請參閱[設定與排程應用程價格](set-and-schedule-app-pricing.md)。


## <a name="free-trial"></a>免費試用

許多開發人員選擇使用Microsoft Store提供的試用功能，讓客戶免費試用應用程式。 根據預設，[**沒有免費試用**] 已選取，且您的應用程式不會有試用。 如果您想要提供試用，可以從 **\[免費試用\]** 下拉式清單選取一個值。

有兩種類型的試用可供您選擇，而且您可以選擇設定開始和停止提供試用的日期和時間。

### <a name="time-limited"></a>限時

選擇 [**限時**]，讓客戶免費試用您的應用程式幾天，如 [**1 天**]、[**7 天**]、[**15 天**] 或 [**30 天**]。 您可以新增程式碼來[排除或限制試用版中的功能](../monetize/in-app-purchases-and-trials.md)，或者您可以讓客戶在該期間存取完整的功能。 
> [!NOTE]
> 限時試用不會對使用 Windows 10 組建 10.0.10586 或較舊版本的客戶顯示，或是使用 Windows Phone 8.1 和較舊版本的客戶。

### <a name="unlimited"></a>無限制

選擇 [**無限制**]，讓客戶無限期免費存取您的應用程式。 您會想要鼓勵他們購買完整版本，所以請務必新增程式碼以[排除或限制試用版中的功能](../monetize/in-app-purchases-and-trials.md)。

### <a name="start-and-end-dates"></a>開始和結束日期

根據預設，您的試用將在應用程式發行時提供，而且不會停止提供。 您可以依喜好指定開始和結束提供試用的日期和時間。 

>[!NOTE]
> 這些日期僅適用於使用 Windows 10 (包括 Xbox) 的客戶。 如果您的 App 適用於使用較舊作業系統版本的客戶，只要您的產品可供使用，也會持續提供試用給這些客戶。 

若要設定何時將試用提供給 Windows 10 的客戶，請將 **\[開始於\]** 和/或 **\[結束於\]** 下拉式清單變更為 **\[於\]**，然後選擇日期和時間。 如果您這樣做，可以選擇 [**UTC**]，如此您選擇的時間會是全球定位時間 (UTC)，或選擇 [**當地**]，如此將會使用市場所在時區的時間。 (請注意，對於包含多個時區的市場，只會使用該市場中的一個時區。 若是美國，東岸時區使用）。如果您想要設定不同的日期，為任何市場，您可以選取**針對特定市場自訂**。


## <a name="sale-pricing"></a>銷售定價

如果您想要以降低的價格提供 App 一段有限的時間，您可以建立及排程銷售。

如需詳細資訊，請參閱[降價銷售應用程式與附加內容](put-apps-and-add-ons-on-sale.md)。


## <a name="organizational-licensing"></a>組織授權

根據預設，您的 app 可能會提供給組織大量購買。 您可以指出是否在此區段提供您的 app 以及其方式。

如需詳細資訊，請參閱[組織授權選項](organizational-licensing.md)。


## <a name="publish-date"></a>發佈日期

之前，**\[發佈日期\]** 區段出現在這個頁面上。 這項功能現在可在[提交選項](manage-submission-options.md)頁面中找到，位於 **\[發佈暫停選項\]** 區段中。 (請注意，為了控制您的應用程式何時應發佈至 Microsoft Store，我們建議使用 **\[定價和可用性\]** 頁面的 [\[排程\]](configure-precise-release-scheduling.md) 區段)。


