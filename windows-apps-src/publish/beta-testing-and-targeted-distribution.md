---
author: jnHs
Description: "Windows 開發人員中心儀表板會提供選項，讓您的 app 僅供指定人員使用，這樣您就可以在公開提供之前，讓測試者試試看。"
title: "搶鮮版 (Beta) 測試和特定對象的發佈"
ms.assetid: 38E4ED22-D6C1-40D8-9B16-6B3E51BD962E
ms.author: wdg-dev-content
ms.date: 08/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.openlocfilehash: 7dd0e346e6be147935503eeb0b685568bfce726c
ms.sourcegitcommit: 6c6f3c265498d7651fcc4081c04c41fafcbaa5e7
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/09/2017
---
# <a name="beta-testing-and-targeted-distribution"></a>搶鮮版 (Beta) 測試和特定對象的發佈

無論您測試 app 時多麼謹慎，都比不上讓他人實際使用來得真切。 Windows 開發人員中心儀表板會提供選項，讓您的應用程式提交僅供指定人員使用，這樣您就可以在公開提供之前，讓測試者試試看。 測試者可能會發現您沒注意到的問題，例如拼字錯誤、令人混淆的 app 流程，或甚至是導致 app 當機的錯誤。 這樣您就有機會能夠在提交正式發行之前修正問題，進而產生更完美的最終產品。

我們提供幾種方式來限制將應用程式只發佈給您的測試人員，而不需要以不同的名稱及版本識別來建立個別的應用程式版本。 (當然，如果您想要，可以建立個別的 App 僅供測試。 如果您這麼做，請務必使其名稱與您想做為最終公用 App 使用的名稱不同。)

將 app 發佈給測試人員的方法，依您 app 的目標作業系統而定。 下方您可以找到適用於 Windows 10 與 Windows 8.1 及更早版本的選項。

## <a name="making-your-app-available-to-testers-on-windows-10-devices"></a>讓您的 app 可供 Windows 10 裝置上的測試者使用

我們提供兩種選項供您限制將 app 只發佈給使用 Windows 10 裝置的特定人員。

### <a name="package-flights"></a>套件正式發行前小眾測試版

如果您已經發佈 app，您可以建立套件正式發行前小眾測試版，來向您指定的人員發佈不同的套件集。 您甚至可以為相同的 app 建立多個套件正式發行前小眾測試版以供不同群組的人員使用。 這是同時嘗試不同套件的絕佳方式，而且如果您決定哪些套件已經準備就緒可發佈給每一個人，您可以將套件從正式發行前小眾測試版拉進非正式發行前小眾測試版提交中。

如需詳細資訊，請參閱[套件正式發行前小眾測試版](package-flights.md)。

> [!NOTE]
> 若要將特定套件散佈給隨機選取的指定百分比 Windows 10 客戶，而不是散佈給指定的一組特定客戶，您可以使用[漸進式套件推出](gradual-package-rollout.md)。 如果您想要以漸進的方式，將更新散佈到您其中一個正式發行前小眾測試版群組，您也可以將推出結合套件正式發行前小眾測試版。

<span id="hide" />
### <a name="hiding-the-app-in-the-store-and-using-promotional-codes"></a>在市集中隱藏 App 及使用促銷碼

如果您想要限制將 App 只散佈給特定群組的測試人員，而**不**先發佈廣泛提供使用的提交，您可以使用和您提交任何 App 時相同的 [App 提交程序](app-submissions.md)。 若要僅允許某些人員免費取得 App，並避免其他客戶看到 App 清單或下載 App，請執行以下作業：

-   在您的提交中，在 [**價格和可用性**] 頁面的[可見度](set-app-pricing-and-availability.md#visibility)區段中選取 [**在市集推出此產品，但不供搜尋**]。  選擇此選項 [**停止取得︰擁有直接連結的任何客戶可以看到產品的市集清單，但除非他們已擁有該產品，或是有促銷碼而且使用 Windows 10 裝置，才能下載**]。 這樣可以防止任何人在市集中透過搜尋或瀏覽來尋找您的應用程式。
-   在應用程式通過認證之後，為應用程式[產生促銷碼](generate-promotional-codes.md)並提供給您的測試者。 在六個月內您可以為單一應用程式產生最多允許 1600 次兌換的促銷碼。 這些代碼會提供測試者 app 清單的直接連結，並讓他們免費下載，即使在您建立提交時已經為它設定價格。

提供促銷碼連結給您的測試者後，他們就能試用，並提供可協助您改善應用程式的意見反應。 然後，當您準備好公開提供您的 App 時，請建立新的提交，並將 **\[可見性\]** 選項變更為 **\[在市集推出此應用程式並可供搜尋\]** (以及您想要的其他任何變更) 即可。

以下是執行此動作時應該牢記的一些事項：

-   建立新的提交即可隨時為測試者提供更新版本的應用程式。 請務必使用 [**停止取得**] 選項將 [**可見度**] 選項保持設為 [**在市集推出此產品，但不供搜尋**]。 在通過認證程序之後，測試者會收到更新，但其他人將無法取得。
-   您的測試者必須有 Windows 10 裝置，才能在其上安裝 app。 (不過，您的 App 不需加入 Windows 10 套件，即可使用這個測試方法)。
-   您可以建立多個[促銷碼](generate-promotional-codes.md)，以隨時散佈 (每六個月每個應用程式最多 1600 次兌換)。
-   在測試者下載應用程式之後，您無法撤銷對應用程式的存取。 下載 app 之後，他們可以繼續使用，並將可取得您後續發行的任何更新。
-   您必須決定向測試者收集意見反應的方式。 請考慮在 Beta 版 app 中提供連結，讓您的測試人員可以輕鬆透過電子郵件或[意見反應中樞](../monetize/launch-feedback-hub-from-your-app.md)提供意見反應。
-   您可以檢閱 App 的[分析報告](analytics.md)，包含使用量和健康情況報告，以及測試者留下的任何評分或評論。
-   將您的 App 發佈給測試者時，您可以併入附加元件。 因為您可能不想要向他們收費，請務必在進行測試時將附加元件的價格設定為 [**免費**]。 然後，將 App 提供給其他客戶時，您可以為每個附加元件建立新的提交，以變更其價格。


## <a name="other-methods-for-distributing-apps-to-testers"></a>散佈 App 給測試者的其他方法

您也可以限制將應用程式只散佈給目標群組人員，方法是在提交應用程式時使用 [**價格和可用性**] 頁面的 [可見度](set-app-pricing-and-availability.md#visibility) 區段中的額外選項。 請記住，並非所有作業系統版本的客戶都能使用這些選項。 具體而言，這些都不適用於使用 Windows 8 或 Windows 8.1 的客戶。

### <a name="targeted-distribution-to-customers-with-a-link-to-your-apps-listing"></a>以具備您的應用程式清單連結的客戶為發佈目標

使用這個選項時，只有擁有您 App 清單直接連結的人員可以下載它。 您可以在儀表板的 [App 身分識別](view-app-identity-details.md) 頁面上找到這個 **URL**。 沒有客戶可以透過搜尋或瀏覽市集找到應用程式，但具備連結的任何人都可以在執行 Windows Phone 8.1 或較舊版本或 Windows 10 的裝置上下載它。 

> [!NOTE]
> 您必須將價格設為 [**免費**]，測試者才能免費下載應用程式。

若要使用此選項，在 [**價格和可用性**] 頁面的[可見度](set-app-pricing-and-availability.md#visibility)區段中選取 [**在市集推出此產品，但不供搜尋**]。 然後選擇此選項 [**僅限直接連結︰擁有產品清單的直接連結的客戶都可以下載，但是 Windows 8.x 例外**]。  


### <a name="targeted-distribution-to-customers-with-specified-email-addresses"></a>以具備指定的電子郵件地址的客戶為散佈目標

僅針對**在 Windows Phone 8.1 及更舊版本上**的測試，此選項提供限制散佈您 App 的方法。 只有您在方塊中輸入其電子郵件地址 (與其 Microsoft 帳戶相關聯) 的人員可以使用指向其清單的直接連結來下載您的 App。

> [!IMPORTANT]
> 擁有您輸入之電子郵件地址的人員將只能在執行 Windows Phone 8.1 或更舊版本的裝置上下載應用程式。
 
您可以在儀表板的 [App 身分識別](view-app-identity-details.md) 頁面上找到 App 的直接連結。 沒有客戶可以透過搜尋或瀏覽市集找到 App，而即使他們擁有 App 清單的連結，他們也將無法下載 App，除非他們使用的 Microsoft 帳戶與您提交此 App 時提供的電子郵件地址相關聯。

> [!NOTE]
如果您使用此選項，您仍然可以透過[產生促銷碼](generate-promotional-codes.md) (如上所述) 讓應用程式供測試者在 Windows 10 裝置上使用。 擁有您的其中一個 app 促銷碼的任何人都可以在 Windows 10 裝置上下載它，即使您沒有在這裡輸入其電子郵件。

若要使用此選項，在 [**價格和可用性**] 頁面的[可見度](set-app-pricing-and-availability.md#visibility)區段中選取 [**在市集推出此產品，但不供搜尋**]。 然後選擇此選項 [**僅限使用 Windows Phone 8.x 的個人：只有您在下方指定的人員可以在 Windows Phone 8.x 裝置上下載此產品。任何人只要有直接連結和促銷碼，就可以在 Windows 10 裝置上下載產品**]。 

如果您選擇這個選項，請記住下列各項：

-   只有當您先前發佈應用程式時，未將[可見度](set-app-pricing-and-availability.md#visibility) 選項設定為 [**在市集推出此應用程式**]，才能夠選取此選項。
-   您 App 的價格必須是**免費**的，測試者才能免費下載。
-   您的測試者只可以在 Windows Phone 8.1 和更早版本上下載 app。 測試者必須有零售版 Windows Phone 裝置，才能使用 app，但裝置不必解除鎖定或是進行註冊。
-   測試者必須有 Microsoft 帳戶，才能存取 Windows 市集並下載您的 app。 您必須知道每位測試者的 Microsoft 帳戶相關聯電子郵件地址，才能將他們加入到您的清單中。 如果要建立新的 Microsoft 帳戶，測試者可以前往 [Microsoft 帳戶設定](http://go.microsoft.com/fwlink/p/?LinkId=618945)。
-   您可以在文字方塊中提供最多 10,000 個電子郵件地址。
-   電子郵件地址必須以分號分隔。
-   您可以稍後新增其他地址，但需要建立新的送出才能執行此動作。
-   在測試者下載 app 之後，您無法撤銷對 app 的存取。 一旦他們下載 app，便可以繼續使用，並將可取得您送出的更新。
