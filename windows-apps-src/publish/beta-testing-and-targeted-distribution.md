---
Description: Partner Center gives you several options to let testers try out your app before you offer it to the public.
title: 搶鮮版 (Beta) 測試和特定對象的發佈
ms.assetid: 38E4ED22-D6C1-40D8-9B16-6B3E51BD962E
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, 搶鮮版 (Beta) 測試, 限量發行, 搶鮮版, beta, 測試, 測試人員
ms.localizationpriority: medium
ms.openlocfilehash: 1560da53ebcc2b24bc9bc13034431c3a2208dfe5
ms.sourcegitcommit: a3dc929858415b933943bba5aa7487ffa721899f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/07/2018
ms.locfileid: "8787150"
---
# <a name="beta-testing-and-targeted-distribution"></a>搶鮮版 (Beta) 測試和特定對象的發佈

無論您測試應用程式時多麼謹慎，都比不上讓他人實際使用來得真切。 測試人員可能會發現您沒注意到的問題，例如拼字錯誤、令人混淆的應用程式流程，或甚至是導致應用程式當機的錯誤。 這樣您就有機會能夠在提交正式發行之前修正問題，進而產生更完美的最終產品。 

合作夥伴中心提供您數個選項，讓測試人員在公開提供之前試用您的應用程式。

無論您選擇哪種方法，以下是當您對您的應用程式進行搶鮮版 (Beta) 測試時應該牢記的一些事項。

- 測試人員下載應用程式之後，您無法撤銷對應用程式的存取。 測試人員下載應用程式之後，就可以繼續使用，並將可取得您後續發行的任何更新。
- 您必須決定向測試人員收集意見反應的方式。 請考慮提供連結，讓您的測試人員可以輕鬆透過電子郵件 (或透過[意見反應中樞](../monetize/launch-feedback-hub-from-your-app.md)，如果您不擔心機密性) 提供意見反應。 
- 您可以檢閱應用程式的[分析報告](analytics.md)，包含使用量和運作狀況報告，以及測試人員留下的任何評分或評論。
- 將您的應用程式發佈給測試人員時，您可以併入附加元件。 由於您可能不想對測試人員收取附加元件的費用，所以您可以[產生促銷碼](generate-promotional-codes.md)並將促銷碼散發給您的測試人員，讓他們免費取得附加元件，您也可以在測試期間將附加元件的價格設定為**免費** (然後在將應用程式提供給其他客戶之前，建立新的附加元件提交以變更其價格)。 請注意，每個 Microsoft 帳戶只能購買一次附加元件，因此相同的測試人員無法測試超過一次的附加元件取得程序。 
- 使用新套件建立新的提交，即可隨時為測試人員提供更新版本的應用程式。 更新通過認證程序之後，您的測試人員就能取得更新，與他們取得原始套件的方式相同，但其他任何人都無法取得 (除非您進行其他變更，例如將應用程式從**私人對象**移動到**公開對象**，或變更可取得應用程式之群組的成員資格)。

## <a name="private-audience"></a>私人對象

如果您在將應用程式提供給其他人使用之前想先讓測試人員使用，並想確定其他人都看不到應用程式清單，請使用 [\[可見度\]](choose-visibility-options.md) 下方的 **\[私人對象\]** 選項 (位在您提交的 **\[定價和可用性\]** 頁面上)。 這是唯一可讓您散發應用程式給測試人員，同時完全防止其他人看到應用程式 Store 清單 (即使他們可以輸入直接連結) 的方式。 

**私人對象**選項只可用時不已發佈您的應用程式給公開對象。 您可以使用此選項以任何作業系統版本中，為目標的應用程式，但您的測試人員必須執行 Windows 10 版本 1607年或更高版本 （包括 Xbox One)，且必須登入您所提供之電子郵件地址相關聯的 Microsoft 帳戶。

如需詳細資訊，請參閱[私人對象](choose-visibility-options.md#audience)。


## <a name="package-flights"></a>套件正式發行前小眾測試版

如果您已經發佈 app，您可以建立套件正式發行前小眾測試版，來向您指定的人員發佈不同的套件集。 您甚至可以為相同的 app 建立多個套件正式發行前小眾測試版以供不同群組的人員使用。 這是同時嘗試不同套件的絕佳方式，而且如果您決定哪些套件已經準備就緒可發佈給每一個人，您可以將套件從正式發行前小眾測試版拉進非正式發行前小眾測試版提交中。

套件正式發行前小眾測試版可用於以任何作業系統版本為目標的應用程式，但您的測試人員必須執行 Windows.Desktop 組建 10586 或更新版本、Windows.Mobile 組建 10586.63 或更新版本，或 Xbox One，才能取得應用程式。

如需詳細資訊，請參閱[套件正式發行前小眾測試版](package-flights.md)。


<span id="hide" />

## <a name="hiding-the-app-in-the-store-and-using-promotional-codes"></a>在 Microsoft Store 中隱藏應用程式及使用促銷碼

此選項提供限制散佈應用程式給只有特定群組的測試人員，同時防止任何人探索您的應用程式在市集中的另一種方式 （或取得它沒有促銷碼）。 不過，與私人對象選項不同的是，如果某人有直接連結，他們可以看得到您應用程式的清單。 如果機密性對您的提交很重要，我們建議改為發佈至私人對象。

隱藏應用程式和使用促銷碼可用於以任何作業系統版本為目標的應用程式，但您的測試人員必須執行 Windows 10，才能取得應用程式。

若要使用此選項：

- 在 **\[定價和可用性\]** 頁面的 **\[可見度\]** 區段，選取 [\[可搜尋性\]](choose-visibility-options.md#discoverability) 下方的 **\[在 Microsoft Store 推出此產品，但不供搜尋\]**。 選擇此選項 [**停止取得︰擁有直接連結的任何客戶可以看到產品的市集清單，但除非他們已擁有該產品，或是有促銷碼而且使用 Windows 10 裝置，才能下載**]。 
- 在應用程式通過認證之後，為應用程式[產生促銷碼](generate-promotional-codes.md)並提供給您的測試人員。 在六個月內您可以為單一應用程式產生最多允許 1600 次兌換的促銷碼。 這些代碼會提供測試者 app 清單的直接連結，並讓他們免費下載，即使在您建立提交時已經為它設定價格。
- 當您準備好公開提供您的應用程式時，請建立新的提交，並將 **\[可見度\]** 選項變更為 **\[在 Microsoft Store 推出此產品並可供搜尋\]** (以及您想要的其他任何變更) 即可。


## <a name="targeted-distribution-with-a-link-to-your-apps-listing"></a>以具備您的應用程式清單連結的客戶為發佈目標

與上述選項不同的是，此選項適用於 Windows Phone 8.1 以及 Windows 10 的客戶 (但不適用於 Windows 8.x)。 沒有客戶可以透過搜尋或瀏覽 Microsoft Store 找到應用程式，但具備 Store 清單直接連結的任何人，都可以在執行 Windows Phone 8.1 或較舊版本或 Windows 10 的裝置上下載它。 請記住，為了讓測試人員可以免費下載應用程式，您必須將價格設為 [**免費**]。

若要使用此選項：
- 在 [**定價和可用性**] 頁面的 [**可見度**] 區段，選取[可搜尋性](choose-visibility-options.md#discoverability)下方的 [**在 Microsoft Store 推出此產品，但不供搜尋**]。 選擇 [**僅限直接連結︰擁有產品清單的直接連結的客戶都可以下載，但是 Windows 8.x 例外**] 的選項。
- 在您發佈產品之後，將連結 ([應用程式身分識別頁面](view-app-identity-details.md)上的 **URL**) 散發給您的測試人員，讓他們可以試用。
- 當您準備好公開提供您的應用程式時，請建立新的提交，並將 **\[可見度\]** 選項變更為 **\[在 Microsoft Store 推出此產品並可供搜尋\]** (以及您想要的其他任何變更) 即可。

> [!IMPORTANT]
> 截至 2018 年 10 月 31 剛建立的產品不能包含套件目標為 Windows Phone 8.x 或更舊版本。 如需詳細資訊，請參閱此[部落格文章](https://blogs.windows.com/buildingapps/2018/08/20/important-dates-regarding-apps-with-windows-phone-8-x-and-earlier-and-windows-8-8-1-packages-submitted-to-microsoft-store/#SzKghBbqDMlmAO4c.97)。

## <a name="targeted-distribution-to-windows-phone-customers-with-specified-email-addresses"></a>以具備指定的電子郵件地址的 Windows Phone 客戶為散佈目標

> [!IMPORTANT]
> 這個選項不適用於新提交。 如果您之前已針對以 Windows Phone 8.1 或更早版本為目標的應用程式選取此選項，您將能繼續對該應用程式使用此選項。 您可以透過建立新的提交來變更測試人員清單 (最多 10,000 人)。 

使用此選項，使用您所指定電子郵件地址的人員可以利用應用程式清單的直接連結來下載您的應用程式 (僅限執行 Windows Phone 8.1 或更早版本的裝置上)。 即使其他客戶擁有連結，仍無法下載應用程式，他們也無法透過搜尋或瀏覽在 Microsoft Store 中尋找應用程式。 為了讓測試人員能夠下載應用程式，您需要提供連結 ([應用程式身分識別頁面](view-app-identity-details.md)上的 **URL**)，他們也必須使用與您提供之電子郵件地址相關聯的 Microsoft 帳戶登入。 您可以也讓應用程式供測試者在 windows 10 裝置上[產生促銷碼](generate-promotional-codes.md);利用其中一個應用程式的促銷碼的任何人都可以下載它在 windows 10 裝置上，即使您沒有輸入其電子郵件以下。
