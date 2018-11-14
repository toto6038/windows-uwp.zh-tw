---
author: mijacobs
Description: This article describes best practices for creating and displaying app settings.
title: 應用程式設定的指導方針
ms.assetid: 2D765E90-3FA0-42F5-A5CB-BEDC14C3F60A
label: Guidelines
template: detail.hbs
ms.author: mijacobs
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 56a952950fa9f2d9d57d5beaed397dd72f64ea54
ms.sourcegitcommit: 38f06f1714334273d865935d9afb80efffe97a17
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/09/2018
ms.locfileid: "6198983"
---
# <a name="guidelines-for-app-settings"></a>應用程式設定的指導方針



應用程式設定是應用程式中使用者可自訂的部分，存在於應用程式設定頁面中。 例如，新聞閱讀程式應用程式中的應用程式設定可讓使用者指定要顯示的新聞來源或畫面上顯示的欄數，而天氣應用程式設定可讓使用者選擇攝氏與華氏做為預設的度量單位。 本文說明建立和顯示應用程式設定的最佳做法。


## <a name="should-i-include-a-settings-page-in-my-app"></a>我的應用程式應該包含設定頁面嗎？

以下為應用程式選項範例，其屬於應用程式設定頁面：

-   可影響應用程式行為且不須經常重新調整的設定選項，範例包括將攝氏或華氏設為天氣應用程式中氣溫的預設單位、變更郵件應用程式的帳戶設定、通知的設定或協助工具選項。
-   取決於使用者偏好的選項，例如音樂、音效或色彩佈景主題。
-   不常存取的應用程式資訊 (像是隱私權原則、說明、應用程式版本或版權資訊)。

作為一般工作流程一部分的命令 (例如，變更美術應用程式的筆刷大小) 不應在設定頁面中。 如需深入了解命令的放置位置，請參閱[命令設計基本知識](https://msdn.microsoft.com/library/windows/apps/dn958433)。

## <a name="general-recommendations"></a>一般建議


-   讓設定頁面保持簡單，並使用二元 (開/關) 控制項。 [切換開關](../controls-and-patterns/toggles.md)通常是二元式設定的最佳控制項。
-   如需讓使用者從一組最多可有 5 個的互斥相關選項中選擇一個項目的設定，請使用[選項按鈕](../controls-and-patterns/radio-button.md)。
-   為您應用程式設定頁面的所有應用程式設定建立進入點。
-   讓設定保持簡單。 盡可能定義智慧型預設值，保持最少的設定數目。
-   當使用者變更設定時，應用程式應立即反映變更。
-   不要包含屬於通用應用程式工作流程的命令。

## <a name="entry-point"></a>進入點


使用者存取應用程式設定頁面的方式，應該以您應用程式的配置為依據。

**瀏覽窗格**

若為瀏覽窗格配置，應用程式設定應為瀏覽選項清單的最後一個項目，而且釘選到底部。

![瀏覽窗格的應用程式設定進入點](images/appsettings-entrypoint-navpane.png)

**應用程式列**

如果您使用[應用程式列](../controls-and-patterns/app-bars.md)或工具列，將設定的進入點放在 [更多] 功能表中的最後一個項目。 如果更容易找到設定進入點對您的應用程式而言很重要，請將進入點直接放在應用程式列上，不要放在溢位內。

![應用程式列的應用程式設定進入點](images/appsettings-entrypoint-tabs.png)

**中樞**

如果您使用中樞配置，應用程式設定的進入點應放在應用程式列的 [更多] 溢位功能表內。

**索引標籤/樞紐**

對於索引標籤或樞紐配置，我們不建議放置應用程式設定的進入點做為瀏覽的前幾個項目之一。 而是應將應用程式設定的進入點放在應用程式列的 [更多] 溢位功能表內。

**主要/詳細資料**

不要將應用程式設定的進入點深藏在主要/詳細資料窗格內，而是使它成為主要窗格最上層的最後一個釘選項目。

## <a name="layout"></a>配置


無論在桌面或行動裝置上，應用程式設定視窗都應開啟全螢幕並填滿整個視窗。 如果您的應用程式設定功能表最多有四個最上層群組，這些群組應該重疊顯示下一欄。

桌上型電腦：

![桌上型電腦的應用程式設定頁面配置](images/appsettings-layout-navpane-desktop.png)

行動電話：

![手機上的應用程式設定頁面配置](images/appsettings-layout-navpane-mobile.png)

## <a name="color-mode-settings"></a>[色彩模式] 設定


如果您的應用程式可讓使用者選擇應用程式的色彩模式，請使用[選項按鈕](../controls-and-patterns/radio-button.md)或[下拉式方塊](../controls-and-patterns/lists.md#drop-down-lists)與「選擇 app 模式」標題呈現這些選項。 選項看起來會像這樣
- 亮
- 暗
- Windows 預設

我們也建議您新增超連結至 [Windows 設定] 應用程式的 [色彩] 頁面，使用者存取和修改目前的預設 app 模式。 使用字串「Windows 色彩設定」做為超連結的文字。

![「選擇模式」區段](images/appsettings_mode.png)

<!--
<div class="microsoft-internal-note">
Detailed redlines showing preferred text strings for the "Choose a mode" section are available on [UNI](http://uni/DesignDepot.FrontEnd/#/ProductNav/2543/0/dv/?t=Windows%7CControls%7CColorMode&f=RS2).
</div>
-->

## <a name="about-section-and-feedback-button"></a>關於區段和意見反應按鈕


我們建議您將「關於此應用程式」區段做為專用頁面或做為單獨的區段放入 App 中。 如果您想要有「傳送意見反應」按鈕，請將它放在「關於此應用程式」頁面的底端。

在「法律」子標頭下，放置任何「使用規定」和「隱私權聲明」(應該是包含換行文字的[超連結按鈕](../controls-and-patterns/hyperlinks.md)) 以及其他法律資訊，例如著作權。

![具有「提供意見反應」按鈕的「關於此應用程式」區段](images/appsettings-about.png)


## <a name="recommended-page-content"></a>建議的頁面內容


當您有一系列項目想包含在應用程式設定頁面時，請將這些方針列入考量：

-   群組類似或相關的設定放在一個設定標籤下。
-   嘗試將總設定數維持在最多四或五個。
-   不論應用程式內容為何，都要顯示相同的設定。 若某些設定在特定內容中不相關，請於應用程式設定飛出視窗加以停用。
-   為設定使用描述性的單詞標籤。 例如，如果是帳戶相關設定，將設定命名為「帳戶」而不是「帳戶設定」。 如果您想要讓設定只有一個選項，且設定沒有描述性標籤，請使用「選項」或「預設」。
-   如果設定直接連結到網站而不是飛出視窗，則使用視覺提示告知使用者，例如以[超連結](../controls-and-patterns/hyperlinks.md)樣式顯示「說明 (線上)」或「Web 論壇」。 考慮將網站的多個連結群組到含有單一設定的飛出視窗。 例如，「關於」設定可以開啟含有使用規定、隱私權原則和應用程式支援之連結的飛出視窗。
-   將較少使用的設定結合成單一項目，讓較常用的設定能有專屬的項目。 將僅包含資訊的內容或連結放入「關於」設定。
-   不要重複 [權限] 窗格中的功能。 Windows 預設會提供這個窗格，且您無法修改它。

-   新增設定內容到 [設定] 飛出視窗
-   從頂端至底部以單欄呈現內容，如有必要，可讓它捲動。 捲動的上限設定為螢幕高度的兩倍。
-   為應用程式設定使用下列控制項：

    -   [切換開關](../controls-and-patterns/toggles.md)：讓使用者將值設定為開啟或關閉。
    -   [選項按鈕](../controls-and-patterns/radio-button.md)：讓使用者從一組最多可有 5 個的互斥相關選項中選擇一個項目。
    -   [文字輸入方塊](../controls-and-patterns/text-block.md)：讓使用者輸入文字。 您使用的文字輸入方塊類型必須與要從使用者取得的文字類型對應，例如電子郵件或密碼。
    -   [超連結](../controls-and-patterns/hyperlinks.md)：將使用者帶到應用程式內的其他頁面或帶到外部網站。 當使用者按一下超連結的時候，[設定] 飛出視窗會關閉。
    -   [按鈕](../controls-and-patterns/buttons.md)：讓使用者立即起始動作，而不需要關閉目前的 [設定] 飛出視窗。
-   如果停用其中一個控制項，請新增描述訊息。 請將此訊息置於已停用控制項的上方。
-   完成 [設定] 飛出視窗和標頭的動畫之後，以單一區塊的方式產生內容和控制項的動畫。 使用向左偏移 100px 的 [**enterPage**](https://msdn.microsoft.com/library/windows/apps/br212672) 或 [**EntranceThemeTransition**](https://msdn.microsoft.com/library/windows/apps/br210288) 動畫，產生內容的動畫。
-   視需要使用區段標頭、段落及標籤協助整理及釐清內容。
-   如果您需要重複設定，可使用額外的 UI 層級或展開/摺疊模式，但避免使用超過兩層的階層。 例如，提供每個城市設定的氣象應用程式可列出城市，然後讓使用者點選城市以開啟新的飛出視窗或展開以顯示設定選項。
-   如果載入控制項或網頁內容需要時間，請使用不確定的進度控制項，向使用者指出資訊正在載入。 如需詳細資訊，請參閱[進度控制項的指導方針](https://msdn.microsoft.com/library/windows/apps/hh465469)。
-   不要使用按鈕瀏覽或認可變更。 使用超連結瀏覽到其他頁面。與其使用按鈕來確認變更，不如在使用者關閉 [設定] 飛出視窗時，自動儲存對應用程式設定所做的變更。



## <a name="related-articles"></a>相關文章

* [命令設計基本知識](https://msdn.microsoft.com/library/windows/apps/dn958433)
* [進度控制項的指導方針](https://msdn.microsoft.com/library/windows/apps/hh465469)
* [儲存和擷取應用程式資料](https://msdn.microsoft.com/library/windows/apps/mt299098)
* [**EntranceThemeTransition**](https://msdn.microsoft.com/library/windows/apps/br210288)
