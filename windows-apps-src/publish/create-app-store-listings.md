---
author: jnHs
Description: The Store listings section of the app submission process is where you provide the text and images that customers will see when viewing your app's listing in the Microsoft Store.
title: 建立應用程式 Store 清單
ms.assetid: 50D67219-B6C6-4EF0-B76A-926A5F24997D
ms.author: wdg-dev-content
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, 清單, 描述, Store 頁面, 版本資訊, 標題
ms.localizationpriority: medium
ms.openlocfilehash: ec1867e747f3458e3a9cffabe9a45535d4c27489
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/20/2018
ms.locfileid: "7421156"
---
# <a name="create-app-store-listings"></a>建立應用程式 Store 清單


您可以在[應用程式提交程序](app-submissions.md)的 **\[Store 清單\]** 區段中，提供客戶在 Microsoft Store 中檢視您應用程式清單時會看到的文字和[影像](app-screenshots-and-images.md)。

**Store 清單**中的許多欄位都是選填的，但我們建議盡可能地提供多個影像及更多資訊，讓您的清單更為出色。完成 **Store 清單**步驟的最低需求是一段文字描述，以及至少一張[螢幕擷取畫面](app-screenshots-and-images.md#screenshots).

> [!TIP]
> 您可以選擇性地[匯入及匯出市集清單](import-and-export-store-listings.md)如果您想要在.csv 檔案，而不是提供的資訊和直接在合作夥伴中心中的上傳檔案中輸入清單資訊離線。 如果您有多種語言的清單版本，使用匯入及匯出選項就很方便，因為您可以一次進行多個更新。 

如果您先前發佈的應用程式支援 Windows 8.x 和/或 Windows Phone 8.x 或更早版本，您可以對這些客戶顯示的 [[建立平台專屬的市集清單](create-platform-specific-store-listings.md)。 

## <a name="store-listing-languages"></a>Microsoft Store清單語言

您必須完成至少一種語言的 **\[Microsoft Store清單\]** 頁面。 我們建議您使用套件支援的每個語言提供一個Microsoft Store清單，但您可以彈性地移除不想提供Microsoft Store清單的語言。 您也可以使用不受您套件支援的其他語言，建立Microsoft Store清單。

> [!NOTE]
> 如果您提交的內容已經包含套件，我們會在提交概觀頁面顯示您套件支援的[語言](supported-languages.md) (除非您移除其中的任何語言)。

若要為您的Microsoft Store清單新增或移除語言，請在提交概觀頁面按一下 [**新增/移除語言**]。 若您已上傳套件，則會在 [**套件支援的語言**] 區段中看到列出這些套件的語言。 若要移除其中一或多個語言，請按一下 **\[移除\]**。 如果您稍後決定包含您先前從此區段移除的語言，您可以按一下 **\[新增\]**。

在 **\[其他Microsoft Store清單語言\]** 區段中，您可以按一下 **\[管理其他語言\]** 以新增或移除您套件*未*包含的語言。 針對您想要新增的語言核取其方塊，然後按一下 **\[更新\]**。 您已經選取的語言將會顯示在 **\[其他Microsoft Store清單語言\]** 區段中。 若要移除其中一或多個語言，按一下 **\[移除\]** (或按一下 **\[管理其他語言\]**，然後針對想要移除的語言取消核取其方塊)。

完成選擇後，按一下 **\[儲存\]**，以返回提交概觀頁面。 

## <a name="add-and-edit-store-listing-info"></a>新增和編輯市集清單資訊

若要編輯市集清單，請從提交概觀頁面中選取的語言名稱。 另外，除非您選擇要匯出您的市集清單並離線工作，然後匯入清單資料的所有一次，您必須編輯每一種語言。 如需詳細資訊的運作方式，請參閱[匯入及匯出市集清單](import-and-export-store-listings.md)。

以下是可用欄位。

## <a name="product-name"></a>產品名稱

這個下拉式方塊可讓您指定 （如果您沒有保留多個應用程式名稱），應該在市集清單中使用的名稱。

如果您已經上傳正在進行中相同的語言的市集清單，您的套件，會選取這些套件中使用的名稱。 如果您需要重新[命名應用程式](manage-app-names.md#rename-an-app-that-has-already-been-published)，它已經發佈之後，您可以選取不同保留的名稱這裡，當您建立新的提交，您已上傳套件，使用新的名稱。

如果您尚未上傳套件的語言，您使用和您已經保留多個名稱，您將需要選取其中一個已保留的 app 名稱，因為沒有該語言可從中提取名稱且相關聯的套件。

> [!NOTE]
> 您只能選取的**產品名稱**適用於您工作的市集清單的語言。 它不會影響在客戶安裝應用程式; 時所顯示的名稱該名稱來自於取得已安裝的套件資訊清單。 若要避免混淆，我們建議每一種語言的封裝和市集清單使用相同的名稱。

## <a name="description"></a>描述

[介紹] 欄位可讓您告訴客戶您的 app 的相關功能。 這個欄位是必要欄位，並會接受達 10,000 個字元的純文字。

如需讓描述更為出色的一些提示，請參閱[撰寫一份出色的應用程式描述](write-a-great-app-description.md)。

<span id="release-notes" />

## <a name="whats-new-in-this-version"></a>此版本中的新功能

如果這是您第一次提交應用程式，請將此欄位保留空白。 現有的應用程式的更新，這是您可以在此讓客戶知道什麼最新版本中變更。 這個欄位有 1500 個字元的限制。 (此欄位之前稱為「**版本資訊**」)。

## <a name="product-features"></a>產品功能

這些是您應用程式重要功能的簡短摘要。 它們會在您的應用程式 Store 清單的 [**功能**] 區段中，以項目符號清單的形式讓客戶看見，除了**描述**之外。 讓它們保持簡潔有力，每個功能只使用數個文字來說明 (不超過 200 個字元)。 您最多可包含 20 個功能。

> [!NOTE]
> 這些功能會顯示項目符號中您的市集清單，所以不要再新增您自己的項目符號。

## <a name="screenshots"></a>螢幕擷取畫面

需要有一個螢幕擷取畫面才能提交您的應用程式。 我們建議您針對應用程式所支援的每個裝置類型提供至少四個螢幕擷取畫面，如此人們就可查看應用程式在其裝置類型上如何顯示。

如需詳細資訊，請參閱[應用程式螢幕擷取畫面與影像](app-screenshots-and-images.md#screenshots)。


## <a name="store-logos"></a>Microsoft Store標誌。 

Microsoft Store標誌是選用影像，您可以上傳以強化應用程式對客戶顯示的外觀。 您也可以選擇性地指定只能將您在此處上傳的影像用於針對 Windows 10 (包括 Xbox) 客戶的應用程式 Store 清單中，而不是允許 Microsoft Store 使用您應用程式套件中的標誌影像。

> [!IMPORTANT]
> 如果您的應用程式支援 Xbox，或支援 Windows Phone 8.1 或較舊版本，您必須在此提供特定影像，清單才會正常出現在Microsoft Store中。 

如需詳細資訊，請參閱[Microsoft Store標誌](app-screenshots-and-images.md#store-logos)。


## <a name="trailers-and-additional-assets"></a>預告片和其他資產

您可以為您的產品，包括預告片和宣傳影像提交其他資產。 這些都是選用，但我們建議您上傳越多越好。 這些影像有助於客戶更了解您的產品，製作更吸引人的清單。

如需詳細資訊，請參閱[預告片和其他資產](app-screenshots-and-images.md#trailers-and-additional-assets)。

<a id="supplemental-information" />

## <a name="supplemental-fields"></a>補充的欄位

本區段中的欄位全部是選用。 請檢閱下方資訊，以判斷提供這項資訊對您的提交是否具有意義。 尤其是，針對大部分提交，建議使用**簡短描述**。 其他欄位可協助提供您的產品用於不同案例的最佳體驗。

### <a name="short-title"></a>簡短標題

產品名稱的較短版本。 若有提供，這個較短名稱可能會出現在 Xbox One 上的各種位置 (在安裝期間、在成就中等) 來取代您產品的完整標題。

這個欄位有 50 個字元的限制。


### <a name="sort-title"></a>排序標題

如果您的產品可以依字母順序排列或以其他方式拼寫，您可以在此處輸入另一個版本。 如果您的客戶在搜尋時輸入這個版本，這可讓他們更快找到您的產品。 

這個欄位有 255 個字元的限制。


### <a name="voice-title"></a>語音標題

您產品的替代名稱，若有提供，在 Xbox One 上使用 Kinect 或頭戴式裝置時，此名稱會用於音訊體驗。

這個欄位有 255 個字元的限制。


### <a name="short-description"></a>簡短描述

簡短、引人注目的描述可能用在產品 Microsoft Store 清單的頂端。 如果未提供，將會改為使用較長[描述](#description)的第一個段落（最多 500 字元）。 因為您的描述也會出現在這個文字下方，我們建議您以不同文字提供簡短描述，讓您的 Microsoft Store 清單不重複。

遊戲的簡短描述可能也會出現在 Xbox One 遊戲中心的 [資訊] 一節。

為獲得最佳結果，保留您的簡短描述下 270 的字元。 欄位有 500 個字元的限制，但在某些檢視中，只有第一次 270 字元將會顯示 （的連結可用來檢視的簡短描述的其餘部分）。


### <a name="additional-system-requirements"></a>其他系統需求

如有需要，您可以描述您的 App 為正常運作所需的硬體組態 (除了您在 [App 屬性](enter-app-properties.md#system-requirements)之 **\[系統需求\]** 區段中提供的資訊之外)。 如果不是每部電腦都有您應用程式所需的硬體，這項資訊尤其重要。 例如，如果您的應用程式必須搭配 3D 印表機或微控制器等外接式 USB 硬體才能正常運作，我們建議在此處輸入這些項目。 您在這裡輸入的資訊會向在 Windows 10 1607 版或更新版本 (包括 Xbox) 上，檢視應用程式 Store 清單的客戶顯示，而且還會顯示您在產品的 [屬性] 頁面上指示的需求。 

您最多可以為 **\[最低硬體\]** 和 **\[建議的硬體\]** 輸入 11 個項目。 它們會在您的 Store 清單中，以項目符號清單的形式讓客戶看見。 讓它們保持簡潔有力，每個項目只使用數個文字來說明 (不超過 200 個字元)。

> [!NOTE]
> 其他系統需求將在 Store 清單中以項目符號的形式出現，所以不要再新增您自己的項目符號。


<span id="shared-fields" />

## <a name="additional-information"></a>其他資訊

下述項目可幫助客戶探索及了解您的產品。 (本節先前稱為「**共用欄位**」)。

### <a name="search-terms"></a>搜尋詞彙

搜尋詞彙 (舊稱為關鍵字) 是單字或簡短的詞彙，客戶看不到，但在客戶使用這些詞彙進行搜尋時，可以協助您的應用程式在 Microsoft Store 中被找到。 您最多可以包含 7 個搜尋詞彙，每個詞彙的長度上限為 30 個字元，並且在所有搜尋詞彙中最多可以使用 21 個不同的字。

新增搜尋詞彙時，請想想看客戶在搜尋與您應用程式類似的應用程式時可能使用哪些字句，特別是如果它們不屬於您應用程式名稱的一部分。 請注意不要使用與您應用程式無關的搜尋詞彙。

### <a name="copyright-and-trademark-info"></a>著作權與商標資訊

如果您想要提供額外的著作權及/或商標資訊，請在這裡輸入。 這個欄位有 200 個字元的限制。


### <a name="additional-license-terms"></a>其他授權條款

如果您想要讓客戶根據與[應用程式開發人員合約](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement)連結的 **\[標準應用程式授權條款\]** 取得應用程式的授權，請將這個欄位保留空白。

如果您的授權條款與 **「標準應用程式授權條款」** 不同，請在這裡輸入。

如果您在此欄位輸入單一 URL，則客戶會看到連結，按一下該連結可以讀取其他授權條款。 如果您的其他授權條款非常冗長時，或者如果您想要包含可按一下的連結或格式化的其他授權條款，這非常有用。

您在此欄位最多可輸入 10,000 個字元的文字。  如果您在此欄位輸入文字，客戶會看到以純文字形式顯示的這些額外授權條款。


### <a name="developed-by"></a>開發者

在此輸入文字，如果您想要將 **\[開發者\]** 欄位包含在您的應用程式Microsoft Store清單中  (**\[發行者\]** 欄位會列出與您帳戶相關聯的發行者顯示名稱，無論您是否提供 **\[開發者\]** 欄位值。)

這個欄位有 255 個字元的限制。
 

<span id="privacy-policy" />

> [!NOTE]
> **隱私權原則**、**網站**和**支援連絡資訊**欄位現在位於[內容](enter-app-properties.md)頁面。

