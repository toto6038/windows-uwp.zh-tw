---
author: jnHs
Description: TSet restrictions on how your app can be discovered and acquired, including whether people can find your app in the Store or see its Store listing at all.
title: 選擇可見度選項
ms.author: wdg-dev-content
ms.date: 03/28/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 可見度, 私人對象, 可用, 可搜尋
ms.localizationpriority: high
ms.openlocfilehash: ae4e467305f91f9faedd7aa1a28a591216a93ee1
ms.sourcegitcommit: 1eabcf511c7c7803a19eb31f600c6ac4a0067786
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="choose-visibility-options"></a>選擇可見度選項


[\[價格與可用性\] 頁面](set-app-pricing-and-availability.md)的 **\[可見度\]** 區段可讓您設定探索及取得您應用程式之方式的限制。 這能讓您指定人員是否可以在 Microsoft Store 中找到您的應用程式或甚至是查看其 Store 清單。

\[可見度\] 區段中有兩個不同的區段：**\[對象\]** 和 **\[可搜尋性\]**。 

## <a name="audience"></a>對象

\[對象\] 區段可讓您指定是否要限制提交項目對您定義之特定對象的可見度。


### <a name="public-audience"></a>公開對象

根據預設，您應用程式的 Store 清單會顯示給**公開對象**。 這適用於大多數提交，除非您想將誰可以查看您的應用程式清單限制給特定人員。 如果您想，也可以使用 [\[可搜尋性\]](#discoverability) 區段中的選項來限制可搜尋性。

> [!IMPORTANT]
> 如果提交產品時將此選項設定為 **\[公開對象\]**，您無法在稍後的提交中選擇 **\[私人對象\]**。


### <a name="private-audience"></a>私人對象

如果您只想將您的應用程式清單顯示給您指定的特定人員，請選擇 **\[私人對象\]**。 使用此選項，除了您指定群組中的人員，其他人都無法找到或使用此應用程式。 這個選項通常適用於 [Beta 測試](beta-testing-and-targeted-distribution.md)，因為您可以將應用程式散發給測試人員，而其他任何人都無法取得應用程式，甚至也無法查看其 Store 清單 (即使在 Store 清單 URL 中輸入名稱也看不到)。

當您選擇 **\[私人對象\]**，您需要指定至少一個應取得應用程式的人員群組。 您可以從現有的[已知使用者群組](create-known-user-groups.md)中選擇，也可以選取 **\[建立新的群組\]** 來定義新群組。 您將需要輸入您想要包含在群組中之每個人的 Microsoft 帳戶相關聯電子郵件地址。 如需詳細資訊，請參閱[建立已知的使用者群組](create-known-user-groups.md)。

您的提交發佈之後，您指定群組中的人員將能夠檢視應用程式的清單及下載應用程式，只要他們以您輸入電子郵件地址相關聯的 Microsoft 帳戶登入，並執行 Windows 10 (版本 1607) 或更新版本 (包括 Xbox One)。 不過，不在私人對象中的人員將無法檢視應用程式清單或下載應用程式，無論他們執行哪一個作業系統版本。 您可以發佈更新後的提交至私人對象，這會以一般應用程式更新的相同方式散發給這些對象中的成員 (但仍無法提供給不在私人對象中的人員，除非您變更對象選擇)。 

如果您想在特定的日期和時間將應用程式提供給公開對象，您可以在建立提交時選取標示為 **\[在以下時間公開此產品\]** 方塊。 輸入您要讓產品公開可用的日期和時間 (UTC 時區)。 請記住下列幾點：

- 您選取的日期和時間會套用到所有市場。 如果您想要自訂不同市場的發行日程，請勿使用此方塊。 請改為建立新提交並將設定變更為 **\[公開對象\]**，然後使用 [\[排程\]](configure-precise-release-scheduling.md) 選項來指定發行時機。
- 為 **\[在以下時間公開此產品\]** 輸入日期並不適用於商務用 Microsoft Store 和/或教育用 Microsoft Store。 若要允許我們透過組織授權將您的應用程式提供給這些客戶，您將需要建立新提交並選取 **\[公開對象\]** (以及啟用[組織授權](organizational-licensing.md))。
- 在您選取的日期和時間之後，所有未來提交將會使用**公開對象**。

如果您未指定讓您的應用程式提供給公開對象的日期和時間，您可以稍後隨時執行，方法是建立新的提交並將對象設定從 **\[私人對象\]** 變更為 **\[公開對象\]**。 當您這樣做時，請記住，您的應用程式可能會經過其他認證程序，因此請準備好處理可能遇到的任何新認證問題。 

以下是選擇將您的應用程式散發給私人對象時，請記住的一些重要事項：
- 私人對象中的人員可以使用連往您應用程式 Store 清單的特定連結來取得應用程式，但他們需要登入他們的 Microsoft 帳戶才能檢視該應用程式。 當您選取 **\[私人對象\]** 時就會提供此連結。 您也可以在 [\[應用程式身分識別\]](view-app-identity-details.md) 頁面的 **\[URL，如果您的應用程式只顯示給特定人員 (需要驗證)\]** 下方找到此連結。 請務必將此連結提供給您的測試人員，而不是 Store 清單的一般 URL。  
- 除非您在 **\[可搜尋性\]** 中選擇防止選項，否則私人對象中的人員可以透過在 Microsoft Store 應用程式中搜尋來找到您的應用程式。 不過，網站清單無法透過搜尋找到，即使是對象中的人員也無法搜尋到。 
- 您將無法在 **\[定價和可用性\] 頁面**的 [\[排程\] 區段中設定發行日期](configure-precise-release-scheduling.md)，因為您的應用程式無法發行給私人對象以外的客戶。
- 您選取的其他選項會套用至此對象中的人員。 例如，如果您選擇 **\[免費\]** 以外的價格，私人對象中的人員將需要支付價格才能取得應用程式。 
- 如果您想將不同的套件散發給私人對象中的不同人，在初始提交之後您可以使用[套件正式發行前小眾測試版](package-flights.md)將不同的套件更新散發給私人對象的子集。 您可以建立其他已知的使用者群組，來定義哪些人應該取得特定的套件正式發行前小眾測試版。
- 您可以編輯私人對象中已知使用者群組成員資格。 不過，請記住，如果您移除先前在群組中且已下載應用程式的某個人，他們仍能使用應用程式，但無法取得您提供的任何更新 (除非您在日後選擇 **\[公開對象\]**)。
- 您的應用程式無法透過商務用 Microsoft Store 和/或教育用 Microsoft Store 提供 (即使是提供給私人對象中的人員)，無論您的組織授權設定為何。
- 雖然 Microsoft Store 可以確保您的應用程式只顯示且提供給您已新增到私人對象並以 Microsoft 帳戶登入的人員，但我們無法防止這些人將資訊或螢幕擷取畫面分享到私人對象外部。 如果您很重視機密性，請確定您的私人對象只包含您信任他們不會與其他人分享應用程式相關詳細資訊的人員。
- 請務必讓您的測試人員了解如何提供意見反應給您。 您可能不會希望他們透過 \[意見反應中樞\] 留下意見反應，因為任何其他客戶可能會看到該意見反應。 請考慮提供讓他們傳送電子郵件或以其他方式提供意見反應的連結。
- 您的測試人員所撰寫的任何評論會出現在[評論報告](reviews-report.md)中。 不過，即使您的提交移至 **\[公開對象\]** 後，這些評論仍不會發佈至您應用程式的 Store 清單。  
- 當您將應用程式從 **\[私人對象\]** 移動至 **\[公開對象\]**，Store 清單中顯示的 **\[發行日期\]** 將是它首次發行給公開對象的日期 (除非您指定了其他的[顯示發行日期](set-app-pricing-and-availability.md#display-release-date))。

## <a name="discoverability"></a>可搜尋性

**\[可搜尋性\]** 區段中的選取項目會指出客戶如何探索並取得您的應用程式。 

> [!IMPORTANT]
> 如果您選取 **\[私人對象\]**，您的 **\[可搜尋性\]** 選取項目僅適用於私人對象中的人員。 不在您指定群組中的客戶將無法取得應用程式，甚至也無法看到其清單。 


### <a name="make-this-product-available-and-discoverable-in-the-store"></a>在 Microsoft Store 推出此產品並且可供搜尋

此為預設選項。 如果您想讓您的應用程式列於 Microsoft Store 中，讓客戶能夠透過應用程式的直接連結和/或其他方法來尋找，包括搜尋、瀏覽和包含於經過挑選的清單中，請保留選取此選項。 

### <a name="make-this-product-available-but-not-discoverable-in-the-store"></a>在 Microsoft Store 推出此產品，但不供搜尋

當您選取此選項時，客戶在 Microsoft Store 中搜尋或瀏覽將無法找到您的應用程式，能取得您應用程式清單的唯一方式是透過直接連結。 

> [!TIP]
> 如果您不想讓公眾檢視您的清單 (即使透過直接連結)，請在 **\[對象\]** 區段中選擇 **\[私人對象\]**，如上文所述。

您還必須選擇下列其中一個選項，指定取得您應用程式的方式：


>[!IMPORTANT]
> 每一個選項都會限制客戶可取得您的應用程式的作業系統版本。 請詳細閱讀描述，確實了解支援的作業系統版本。 

- **僅限直接連結︰擁有產品清單的直接連結的客戶都可以下載，但是 Windows 8.x 例外。** 透過直接連結前往您的應用程式清單的客戶可在執行 Windows 10 的裝置或執行 Windows Phone 8.1 和較舊版本的裝置 (但執行 Windows 8.x 的裝置除外) 上進行下載。
- **停止取得︰擁有直接連結的任何客戶可以看到產品的市集清單，但除非他們已擁有該產品，或是有促銷碼而且使用 Windows 10 裝置，才能下載。** 即使客戶有直接連結，除非有[促銷碼](generate-promotional-codes.md)且使用 Windows 10 裝置，否則仍無法下載應用程式。 如果客戶有促銷碼，他們可以使用代碼免費取得您的應用程式 (僅限在 Windows 10 上)，即使您未提供給任何其他客戶。 除了使用促銷碼之外，任何人沒有其他方式可取得您的應用程式。

> [!TIP]
> 如果您想要完全停止為新客戶提供某個應用程式，您可選取其概觀頁面的 **\[停止提供應用程式\]**。 在您確認想要停止提供該應用程式之後，該應用程式在數小時內便無法在 Microsoft Store 中讓別人看見，而所有的新客戶都將無法取得它 (除非他們有[促銷碼](generate-promotional-codes.md)且使用 Windows 10 裝置)。 這個動作會覆寫您提交中的 **\[可見度\]** 選取項目。 若要再次為新客戶提供該應用程式 (根據您的 **\[可見度\]** 選取項目)，您可隨時按一下概觀頁面中的 **\[提供 app\]**。 如需詳細資訊，請參閱[從 Microsoft Store 移除應用程式](guidance-for-app-package-management.md#removing-an-app-from-the-store)。



