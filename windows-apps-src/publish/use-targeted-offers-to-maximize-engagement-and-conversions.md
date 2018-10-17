---
author: JnHs
Description: Target specific segments of your customers with personalized content to increase engagement, retention, and monetization.
title: 使用針對性優惠提高吸引力與轉換數
ms.author: wdg-dev-content
ms.date: 11/10/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP, 針對性優惠, 優惠, 通知
ms.localizationpriority: medium
ms.openlocfilehash: 727c438bacf51fd2ead03df72421363116c4701b
ms.sourcegitcommit: 1c6325aa572868b789fcdd2efc9203f67a83872a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/17/2018
ms.locfileid: "4745614"
---
# <a name="use-targeted-offers-to-maximize-engagement-and-conversions"></a>使用針對性優惠提高吸引力與轉換數

以特定客戶區隔為目標，透過有吸引力的個人化內容提高吸引力、留住客戶及創造營生。

> [!IMPORTANT]
> 針對性優惠只能用於包括附加元件的 UWP 應用程式。

## <a name="targeted-offer-overview"></a>針對性優惠概觀

有三件重要的事必須執行，您才能使用針對性優惠：

1. **在您的儀表板中建立優惠。** 瀏覽至 **\[互動 > 針對性優惠\]** 頁面建立優惠。 有關這個程序的詳細資訊如下所述。
2. **實作應用程式內的購買選項體驗。** 使用*Microsoft Store 針對性優惠 API*在您的應用程式程式碼中擷取指定使用者可用的優惠。 您也需要對針對性優惠建立應用程式內體驗。 如需詳細資訊，請參閱[使用市集服務管理針對性優惠](../monetize/manage-targeted-offers-using-windows-store-services.md)。
3. **將您的應用程式提交到市集。** 為了讓客戶看見您的優惠，您需要在實作應用程式內的購買選項體驗之後發行應用程式。

完成這些步驟之後，使用您的應用程式的客戶將會看到他們當時可用的優惠，根據其在該優惠相關區隔中的會員資格。 請注意，雖然我們盡力對您的客戶顯示所有可用優惠，但有時可能會發生問題而影響優惠的可用性。


## <a name="to-create-and-send-a-targeted-offer"></a>建立和傳送針對性優惠

請依照下列步驟在儀表板中建立針對性優惠。

1.  在 Windows 開發人員中心儀表板中，展開左側瀏覽功能表的 **\[互動\]**，然後選取 **\[針對性優惠\]**。
2.  在 **\[針對性優惠\]** 頁面中，檢閱可用的優惠。 針對您想要實作的任何優惠選取 **\[建立新優惠\]**。

    > [!NOTE]
    > 您看見的可用優惠可能隨時間及依據帳戶準則而改變。

3.  在可用優惠底下出現的新列中，選擇適用優惠的產品 (App)。 然後，選取要與優惠產生關聯的附加元件。
4.  如果您想要建立其他優惠，請重複步驟 2 和 3。 您可以針對相同應用程式多次實作相同的優惠類型，只要您針對每一個優惠選取不同的附加元件。 此外，您也可以將相同的附加元件與多種優惠類型產生關聯。
5.  當您完成建立優惠時，按一下 **\[儲存\]**。

在您實作優惠之後，可以返回儀表板中的 **\[針對性優惠\]** 頁面，檢視每個優惠的轉換總數。

如果您決定不使用優惠 (或不想再繼續使用)，請按一下 **\[刪除\]**。

> [!IMPORTANT]
> 務必發行程式碼來擷取指定使用者的可用優惠，並建立應用程式內體驗。 如需詳細資訊，請參閱[使用市集服務管理針對性優惠](../monetize/manage-targeted-offers-using-windows-store-services.md)。
>
> 考慮針對性優惠的內容時，請記住，就像所有應用程式內容一樣，優惠的內容必須符合市集[內容原則](https://docs.microsoft.com/en-us/legal/windows/agreements/store-policies)的規範。
>
> 另請注意，如果使用您的 App (並且在確定區隔會員資格後使用其 Microsoft 帳戶登入) 的客戶將裝置交給其他人使用，那麼其他人可能會看到針對原來客戶的優惠。
