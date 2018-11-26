---
Description: Describes the landmarks and headings features of accessibility.
ms.assetid: 019CC63D-D915-4EBD-9442-DE899AB973C9
title: 地標和標題
label: Landmarks and Headings
template: detail.hbs
ms.date: 01/24/2018
ms.topic: article
keywords: Windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: d81957c379bd948a50d08b980ff20debc6c223c5
ms.sourcegitcommit: 681c70f964210ab49ac5d06357ae96505bb78741
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/26/2018
ms.locfileid: "7719654"
---
# <a name="landmarks-and-headings"></a>地標和標題

使用者介面通常以視覺上有效率的方式組織，讓目視使用者快速略讀有興趣的內容，而不需減慢速度閱讀*所有*內容。 螢幕助讀程式使用者需用有像這樣的略讀能力。 地標和標題定義可協助輔助技術 (AT) 使用者更有效率地瀏覽使用者介面區段。 將內容標記到地標和標題內，可讓螢幕助讀程式使用者選擇略讀內容，就像目視使用者一樣。

[ARIA 地標](https://www.w3.org/WAI/GL/wiki/Using_ARIA_landmarks_to_identify_regions_of_a_page)、[ARIA 標題](https://www.w3.org/TR/WCAG20-TECHS/ARIA12.html)和 [HTML 標題](https://www.w3.org/TR/2016/NOTE-WCAG20-TECHS-20161007/H42.html) 的概念應用在網頁內容已行之有年，可讓螢幕助讀程式使用者更快瀏覽。 網頁利用地標和標題提供更實用的內容，讓 AT 使用者能快速前往大區塊 (地標) 和小區塊 (標題)。 具體而言，螢幕助讀程式提供一些命令讓使用者在地標之間和標題之間跳躍 (下一個/上一個，或特定標題層級)。 基於這些理由，請務必考慮在您的應用程式中納入地標和標題。

地標可將內容依不同類別分組，像是搜尋、瀏覽、主要內容等。 分組後，AT 使用者就能快速瀏覽不同的群組。 這項快速瀏覽功能可讓使用者略過可能很大量的內容，這些內容先前須逐項瀏覽，因而導致不良體驗。 

例如，使用索引標籤面板時，可考慮這個瀏覽地標。 使用搜尋編輯方塊時，可將此視為搜尋地標，並考慮將主要內容設定為主要內容地標。 無論在地標內或甚至在地標外，請考慮採用邏輯標題層級將子元素設定為標題。 

考慮在 Windows「設定」app 中的 **\[輕鬆存取\]** 頁面。 

![Windows「設定」app 中的 \[輕鬆存取\] 頁面](images/EaseOfAccessSettings.png)  

搜尋編輯方塊會包裝在搜尋地標內。 左側的瀏覽元素會包裝在瀏覽地標內，右側的主要內容則包裝在主要內容地標內。 進一步來說，瀏覽地標內有一個主要群組標題，稱為**輕鬆存取**，也就是標題層級 1。 底下有子選項 **\[視力\]**、**\[聽力\]** 等。 這些屬於標題層級 2。 設定標題會在主要內容內繼續進行，並再次將主要主體 **\[顯示\]** 設定為標題層級 1，將 **\[全部放大\]** 這類子群組設定為標題層級 2。 

「設定」app 提供無障礙功能，不須經由地標和標題，但有這些會更實用。 螢幕助讀程式使用者可快速、輕鬆地前往所需的群組 (地標)，然後同樣可快速前往子群組 (標題)。 

使用 [AutomationProperties.LandmarkTypeProperty](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.automationproperties.LandmarkTypeProperty) 將 UI 元素設定為您希望的[地標類型](https://msdn.microsoft.com/library/windows/desktop/mt759299)。 這個地標 UI 元素會封裝適合該地標的所有其他 UI 元素。 

使用 [AutomationProperties.LocalizedLandmarkTypeProperty](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.automationproperties.LocalizedLandmarkTypeProperty) 可具體為地標命名。 如果您選取預先定義的地標類型，例如主要或瀏覽，這些名稱會做為地標名稱。 不過，如果您將地標類型設定為自訂，則必須透過此屬性具體為地標命名。 您也可以使用此屬性覆寫非自訂地標類型的預設名稱。 

使用 [AutomationProperties.HeadingLevel](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.automationproperties.headinglevelproperty) 將 UI 元素設定為特定層級 *Level1* 到 *Level9* 的標題。

