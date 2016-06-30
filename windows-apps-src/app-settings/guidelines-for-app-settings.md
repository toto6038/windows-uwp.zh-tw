---
author: mijacobs
Description: "本文說明建立和顯示應用程式設定的最佳做法。"
title: "App 設定的指導方針"
ms.assetid: 2D765E90-3FA0-42F5-A5CB-BEDC14C3F60A
label: Guidelines
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: 59e02840c72d8bccda7e318197e4bf45ed667fa4
ms.openlocfilehash: aeccd755c5fe5df8f2ff5549950ce2d6cb74e8e4

---


# App 設定的指導方針





App 設定是 App 中使用者可自訂的部分，存在於 App 設定頁面中。 例如，新聞閱讀程式應用程式中的應用程式設定可讓使用者指定要顯示的新聞來源或畫面上顯示的欄數，而天氣應用程式設定可讓使用者選擇攝氏與華氏做為預設的度量單位。 本文說明建立和顯示應用程式設定的最佳做法。

![設定窗格範例](images/app-settings.png)

## <span id="Should_I_include_a_settings_page_in_my_app_"></span><span id="should_i_include_a_settings_page_in_my_app_"></span><span id="SHOULD_I_INCLUDE_A_SETTINGS_PAGE_IN_MY_APP_"></span>我的 App 應該包含設定頁面嗎？

以下是應用程式設定頁面中的應用程式選項範例： 

-   影響應用程式行為但不需要經常重新調整的設定選項，像是在天氣應用程式中選擇攝氏或華氏做為溫度預設單位，或變更郵件應用程式的帳戶設定、通知設定或協助工具選項。
-   以使用者喜好設定 (例如音樂、音效或色彩佈景主題) 為依據的選項。
-   不常存取的應用程式資訊 (像是隱私權原則、說明、應用程式版本或版權資訊)。

屬於一般應用程式工作流程的命令 (例如，變更繪圖應用程式中的筆刷大小) 不應該放在 \[設定\] 頁面中。 若要深入了解命令的放置位置，請參閱[命令設計基本知識](https://msdn.microsoft.com/library/windows/apps/dn958433)。

## <span id="general_principles"></span><span id="GENERAL_PRINCIPLES"></span>一般建議


-   讓設定頁面保持簡單，並使用二元 (開/關) 控制項。 [切換開關](../controls-and-patterns/toggles.md)通常是二元式設定的最佳控制項。
-   若需要讓使用者從一組最多可有 5 個的互斥相關選項中選擇一個項目的設定，請使用[選項按鈕](../controls-and-patterns/radio-button.md)。
-   在應用程式 \[設定\] 頁面中為所有應用程式設定建立進入點。
-   讓設定保持簡單。 盡可能定義智慧型預設值，保持最少的設定數目。
-   當使用者變更設定時，應用程式應立即反映變更。
-   不要包含屬於通用應用程式工作流程的命令。

## <span id="Entry_point"></span><span id="entry_point"></span><span id="ENTRY_POINT"></span>進入點


使用者存取應用程式設定頁面的方式，應該以您應用程式的配置為依據。

**瀏覽窗格**

若為瀏覽窗格配置，應用程式設定應為瀏覽選項清單的最後一個項目，而且釘選到底部。

![瀏覽窗格的應用程式設定進入點](images/appsettings-entrypoint-navpane.png)

**應用程式列**

如果您使用應用程式列或工具列，通常是中樞或索引標籤/樞紐瀏覽配置的一部分，請將進入點放在「更多」飛出視窗功能表的最後一個項目。 如果更容易找到設定進入點對您的 app 而言很重要，請將進入點直接放在應用程式列上，不要放在「更多」飛出視窗功能表中。

![應用程式列的應用程式設定進入點](images/appsettings-entrypoint-tabs.png)

**中樞**

如果您使用中樞配置，應用程式設定的進入點應放在應用程式列的「更多」飛出視窗功能表內。

**索引標籤/樞紐**

對於索引標籤或樞紐配置，我們不建議放置應用程式設定的進入點做為瀏覽的前幾個項目之一。 而是應將應用程式設定的進入點放在應用程式列的「更多」飛出視窗功能表內。

**主要/詳細資料**

不要將應用程式設定的進入點深藏在主要/詳細資料窗格內，而是使它成為主要窗格最上層的最後一個釘選項目。

## <span id="Layout"></span><span id="layout"></span><span id="LAYOUT"></span>配置​​


在桌面和行動裝置上，應用程式設定視窗應以全螢幕方式開啟，並填滿整個視窗。 如果您的應用程式設定功能表在最多四個最上層群組之間，這些群組應該重疊顯示下一欄。

桌面：

![桌面上的應用程式設定頁面配置](images/appsettings-layout-navpane-desktop.png)

行動裝置：

![手機上的 App 設定頁面配置](images/appsettings-layout-navpane-mobile.png)

## <span id="_About__section_and__Give_feedback__button"></span><span id="_about__section_and__give_feedback__button"></span><span id="_ABOUT__SECTION_AND__GIVE_FEEDBACK__BUTTON"></span>「關於」區段和「提供意見反應」按鈕


如果您的應用程式中需要「關於此應用程式」區段，請建立專用的應用程式設定頁面。 如果您想要有「提供意見反應」按鈕，請將它放在「關於此應用程式」頁面的底端。

「使用規定」和「隱私權聲明」應為文字換行的[超連結按鈕](../controls-and-patterns/hyperlinks.md)。

![具有「提供意見反應」按鈕的「關於此 App」區段](images/appsettings-about.png)

## <span id="dos_and_donts"></span><span id="DOS_AND_DONTS"></span>建議事項


## <span id="add_entry_points"></span><span id="ADD_ENTRY_POINTS"></span>應用程式設定頁面內容


有了要包含在應用程式設定頁面之項目的清單後，請考量下列指導方針：

-   群組類似或相關的設定放在一個設定標籤下。
-   嘗試將總設定數維持在最多四或五個。
-   不論應用程式內容為何，都要顯示相同的設定。 如果有些設定與某一內容不相關，請在應用程式設定飛出視窗中停用這些設定。
-   為設定使用描述性的單詞標籤。 例如，如果是帳戶相關設定，將設定命名為「帳戶」而不是「帳戶設定」。 如果您想要讓設定只有一個選項，且設定沒有描述性標籤，請使用「選項」或「預設」。
-   如果設定直接連結到網站而不是飛出視窗，則使用視覺提示告知使用者，例如以[超連結](../controls-and-patterns/hyperlinks.md)樣式顯示「說明 (線上)」或「Web 論壇」。 考慮將網站的多個連結群組到含有單一設定的飛出視窗。 例如，「關於」設定可以開啟含有使用規定、隱私權原則和應用程式支援之連結的飛出視窗。
-   將較少使用的設定結合成單一項目，讓較常用的設定能有專屬的項目。 將僅包含資訊的內容或連結放入「關於」設定。
-   不要重複 \[權限\] 窗格中的功能。 Windows 預設會提供這個窗格，且您無法修改它。

## <span id="add_settings_to_flyouts"></span><span id="ADD_SETTINGS_TO_FLYOUTS"></span> 新增設定內容到 \[設定\] 飛出視窗


-   從頂端至底部以單欄呈現內容，如有必要，可讓它捲動。 捲動的上限設定為螢幕高度的兩倍。
-   為應用程式設定使用下列控制項：

    -   [切換開關](../controls-and-patterns/toggles.md)：讓使用者將值設定為開啟或關閉。
    -   [選項按鈕](../controls-and-patterns/radio-button.md)：讓使用者從一組最多可有 5 個的互斥相關選項中選擇一個項目。
    -   [文字輸入方塊](../controls-and-patterns/text-block.md)：讓使用者輸入文字。 您使用的文字輸入方塊類型必須與要從使用者取得的文字類型對應，例如電子郵件或密碼。
    -   [超連結](../controls-and-patterns/hyperlinks.md)：將使用者帶到應用程式內的其他頁面或帶到外部網站。 當使用者按一下超連結的時候，\[設定\] 飛出視窗會關閉。
    -   按鈕：讓使用者立即起始動作，而不需要關閉目前的 \[設定\] 飛出視窗。
-   如果停用其中一個控制項，請新增描述訊息。 請將此訊息置於已停用控制項的上方。
-   完成 \[設定\] 飛出視窗和標頭的動畫之後，以單一區塊的方式產生內容和控制項的動畫。 使用向左偏移 100px 的 [**enterPage**](https://msdn.microsoft.com/library/windows/apps/br212672) 或 [**EntranceThemeTransition**](https://msdn.microsoft.com/library/windows/apps/br210288) 動畫，產生內容的動畫。
-   使用區段標頭、段落及標籤，協助組織和釐清內容 (如有必要)。
-   如果您需要重複設定，可使用額外的 UI 層級或展開/摺疊模式，但避免使用超過兩層的階層。 例如，提供每個城市設定的氣象應用程式可列出城市，然後讓使用者點選城市以開啟新的飛出視窗或展開以顯示設定選項。
-   如果載入控制項或網頁內容需要時間，請使用不確定的進度控制項，向使用者指出資訊正在載入。 如需詳細資訊，請參閱[進度控制項的指導方針](https://msdn.microsoft.com/library/windows/apps/hh465469)。
-   不要使用按鈕瀏覽或認可變更。 使用超連結瀏覽到其他頁面。與其使用按鈕來認可變更，在使用者關閉 \[設定\] 飛出視窗時，自動儲存變更到應用程式設定。

\[本文包含通用 Windows 平台 (UWP) app 與 Windows 10 專屬的資訊。 如需 Windows 8.1 指導方針，請下載 [Windows 8.1 指導方針 PDF](https://go.microsoft.com/fwlink/p/?linkid=258743)。\]

## <span id="related_topics"></span>相關主題

* [命令設計基本知識](https://msdn.microsoft.com/library/windows/apps/dn958433)
* [進度控制項的指導方針](https://msdn.microsoft.com/library/windows/apps/hh465469)
            
            **適用於開發人員 (XAML)**
* [儲存和擷取 App 資料](https://msdn.microsoft.com/library/windows/apps/mt299098)
* [
              **EntranceThemeTransition**
            ](https://msdn.microsoft.com/library/windows/apps/br210288)

�



<!--HONumber=Jun16_HO4-->


