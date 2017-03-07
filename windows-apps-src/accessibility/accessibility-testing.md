---
author: Xansky
Description: "確定通用 Windows 平台 (UWP) app 可以提供無障礙功能的測試程序。"
ms.assetid: 272D9C9E-B179-4F5A-8493-926D007A0225
title: "協助工具測試"
label: Accessibility testing
template: detail.hbs
ms.author: mhopkins
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: bf56b564b383ee90e276416bf1dda29f55bb771c
ms.lasthandoff: 02/07/2017

---

# <a name="accessibility-testing"></a>協助工具測試  

確定通用 Windows 平台 (UWP) app 可以提供無障礙功能的測試程序。

<span id="run_accessibility_testing_tools"/>
<span id="RUN_ACCESSIBILITY_TESTING_TOOLS"/>
## <a name="run-accessibility-testing-tools"></a>執行協助工具測試工具  
Windows 軟體開發套件 (SDK) 包含多種協助工具測試工具，例如 [**AccScope**](https://msdn.microsoft.com/library/windows/desktop/Dn433239)、[**Inspect**](https://msdn.microsoft.com/library/windows/desktop/Dd318521) 和 [**UI Accessibility Checker**](https://msdn.microsoft.com/library/windows/desktop/Hh920985)。 這些工具可以協助您確認應用程式的協助工具。 請務必確認所有的 app 案例以及 UI 元素。

您可以從 Microsoft Visual Studio 命令提示字元或從 Windows SDK 工具資料夾 (您開發電腦上安裝 Windows SDK 所在的 bin 子目錄) 啟動協助工具測試工具。

<span id="AccScope"/>
<span id="accscope"/>
<span id="ACCSCOPE"/>
### **<a name="accscope"></a>AccScope**  

[**AccScope**](https://msdn.microsoft.com/library/windows/desktop/Dn433239) 工具可讓開發人員和測試人員在 App 開發和設計期間 (有可能在早期原型階段，而不是 App 開發週期的晚期測試階段) 評估 App 的協助工具。 這是特別針對 App 的朗讀程式協助工具案例測試所設計。

<span id="inspect"/>
<span id="INSPECT"/>
### **<a name="inspect"></a>Inspect**  

[**Inspect**](https://msdn.microsoft.com/library/windows/desktop/Dd318521) 可以讓您選取任何 UI 元素以及查看它的協助工具資料。 您可以檢視 Microsoft 使用者介面自動化屬性和控制項模式，以及為使用者介面自動化樹狀目錄的自動化元素測試瀏覽結構。 當您開發 UI 時，請使用 **Inspect** 確認協助工具屬性如何在使用者介面自動化中公開。 在某些情況下，屬性來自已經為預設 XAML 控制項實作的使用者介面自動化支援。 在其他情況下，屬性來自已經在 XAML 標記中設定的特定值，如 [**AutomationProperties**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.automationproperties) 附加屬性。

以下影像顯示 [**Inspect**](https://msdn.microsoft.com/library/windows/desktop/Dd318521) 工具正在查詢「記事本」中 [編輯]**** 功能表元素的使用者介面自動化屬性。

![Inspect 工具螢幕擷取畫面。](./images/inspect.png)

<span id="ui_accessibility_checker"/>
<span id="UI_ACCESSIBILITY_CHECKER"/>
### **<a name="ui-accessibility-checker"></a>UI 協助工具檢查程式**  
**UI 協助工具檢查程式 (AccChecker)** 可協助您找出在執行階段的協助工具問題。 當 UI 設計完成而且功能正常後，請使用 **AccChecker** 測試不同的案例、確認執行階段協助工具資訊是否正確，以及發現執行階段發生的問題。 您可以在 UI 或命令列模式中執行 **AccChecker**。 若要執行 UI 模式工具，請開啟 Windows SDK bin 目錄中的 **AccChecker** 目錄，執行 acccheckui.exe，然後按一下 [說明]**** 功能表。

<span id="ui_automation_verify"/>
<span id="UI_AUTOMATION_VERIFY"/>
### **<a name="ui-automation-verify"></a>使用者介面自動化確認**  
「使用者介面自動化驗證 (UIA 驗證)」**** 是一種使用者介面自動化實作的自動測試和驗證架構。 「UIA 驗證」****可以整合到測試程式碼中，並執行使用者介面自動化案例的一般自動測試或抽樣檢查。 若要執行「UIA 驗證」****，請從 [UIAVerify] 子目錄執行 VisualUIAVerifyNative.exe。

<span id="accessible_event_watcher"/>
<span id="ACCESSIBLE_EVENT_WATCHER"/>
### **<a name="accessible-event-watcher"></a>協助工具事件監控程式**  
當 UI 發生變更時，[**Accessible Event Watcher (AccEvent)**](https://msdn.microsoft.com/library/windows/desktop/Dd317979) 會測試應用程式的 UI 元素是否引發正確的使用者介面自動化以及 Microsoft Active Accessibility 事件。 當焦點變更，或者當叫用、選取 UI 元素，或 UI 元素的狀態或屬性變更時，就會發生 UI 變更。

> [!NOTE]
> 文件中提及的大部分協助工具測試工具都是在電腦上執行，而不是在手機上執行。 您可以在開發和使用模擬器的同時執行某些工具，不過其中大部分的工具都無法在模擬器內公開使用者介面自動化樹狀目錄。

<span id="test_keyboard_accessibility"/>
<span id="TEST_KEYBOARD_ACCESSIBILITY"/>
## <a name="test-keyboard-accessibility"></a>測試鍵盤協助工具  
測試鍵盤協助工具最好的方法是拔掉滑鼠，或者如果您使用平板電腦裝置時，使用螢幕小鍵盤。 使用 _Tab_ 鍵，測試鍵盤協助工具瀏覽功能。 使用 _Tab_ 鍵的時候，您應該可以在所有互動式 UI 元素之間循環移動。 至於複合 UI 元素，請使用方向鍵，確認可以在元素組件之間移動。 例如，您應該能夠使用鍵盤上的按鍵瀏覽項目清單。 最後，確定當互動式 UI 元素具有焦點時，您可以使用鍵盤 (通常是使用 Enter 或空格鍵) 呼叫所有元素。

<span id="verify_the_contrast_ratio_of_visible_text"/>
<span id="VERIFY_THE_CONTRAST_RATIO_OF_VISIBLE_TEXT"/>
## <a name="verify-the-contrast-ratio-of-visible-text"></a>驗證顯示文字的對比率  
使用色彩對比工具確定可見文字的對比率是否可被接受。 例外狀況包括非作用中的 UI 元素、標誌，以及不會傳達任何資訊且在重新排列後，意思仍然不變的修飾性文字。 如需對比率與例外狀況的詳細資訊，請參閱[協助工具文字需求](accessible-text-requirements.md)。 請參閱 [WCAG 2.0 G18 的技術 (資源小節)](http://www.w3.org/TR/WCAG20-TECHS/G18.html#G18-resources)，了解可以測試對比率的工具。

> [!NOTE]
> 《WCAG 2.0 的技術》中 G18 所列出的一些工具不能與 Windows 市集應用程式互動使用。 您可能需要在工具中手動輸入前景和背景色彩值，擷取應用程式 UI 的螢幕，然後透過螢幕擷取影像執行對比率工具，或在影像編輯程式中開啟來源點陣圖檔案時 (而非應用程式載入影像時) 執行此工具。

<span id="verify_your_app_in_high_contrast"/>
<span id="VERIFY_YOUR_APP_IN_HIGH_CONTRAST"/>
## <a name="verify-your-app-in-high-contrast"></a>在高對比中檢查您的應用程式  
在高對比佈景主題中使用您的應用程式，確認所有 UI 元素可以正常顯示。 所有文字應該可以分辨，而且所有影像應該都很清楚。 調整 XAML 佈景主題字典資源或控制項範本，以更正控制項造成的任何佈景主題問題。 如果主要的高對比問題並不是來自佈景主題或控制項 (例如來自影像檔案)，請在高對比佈景主題為使用中時，提供其他的版本。

<span id="verify_your_app_with_make_everything_on_your_screen_bigger"/>
<span id="VERIFY_YOUR_APP_WITH_MAKE_EVERYTHING_ON_YOUR_SCREEN_BIGGER"/>
## <a name="verify-your-app-with-display-settings"></a>使用顯示設定驗證應用程式  
使用系統顯示選項來調整顯示器的 DPI 值，並確定在 DPI 值變更時，app UI 能夠正確縮放。 (某些使用者會變更 DPI 值來做為其無障礙輔助，您可以在 [輕鬆存取]**** 中變更該選項，以及顯示屬性)。如果發現任何問題，請按照[配置縮放指導方針](https://msdn.microsoft.com/library/windows/apps/Dn611863)的做法，同時為不同的縮放比例提供額外的資源。

<span id="verify_main_app_scenarios_by_using_narrator"/>
<span id="VERIFY_MAIN_APP_SCENARIOS_BY_USING_NARRATOR"/>
## <a name="verify-main-app-scenarios-by-using-narrator"></a>使用朗讀程式，確認主 App 操作正常  
執行以下步驟，使用朗讀程式測試應用程式的螢幕助讀使用體驗。

**透過下列步驟，使用朗讀程式搭配滑鼠和鍵盤來測試您的應用程式：**
1.  按 _Windows 標誌鍵 + Enter 鍵_來啟動朗讀程式。
2.  使用 _Tab_ 鍵、方向鍵及 _Caps Lock + 方向鍵_，利用鍵盤來瀏覽您的應用程式。
3.  瀏覽應用程式時，聆聽朗讀程式朗讀 UI 的元素，並確認下列各項：
    * 對於每個控制項，確保朗讀程式朗讀所有顯示的內容。 此外，還需確保朗讀程式朗讀每個控制項的名稱、所有適當的狀態 (已勾選、已選取等)，以及控制項類型 (按鈕、核取方塊、清單項目等)。
    * 如果元素是互動的，請確認您可以使用朗讀程式，透過按 _Caps Lock + Enter 鍵_來叫用它的動作。
    * 對於每個表格，確保朗讀程式正確朗讀表格名稱、表格說明 (如果可用的話)，以及列與欄標題。

4.  按 _Caps Lock + Enter 鍵_來搜尋您的應用程式，並確認您的所有控制項都會出現在搜尋清單中，而且控制項名稱都已當地語系化且可閱讀。
5.  關閉您的顯示器，並嘗試只使用鍵盤和朗讀程式來完成主應用程式案例。 若要取得朗讀程式命令和捷徑的完整清單，請按 _Caps Lock + F1_。

從 Windows 10 版本 1607 開始，我們在朗讀程式中導入了新的開發人員模式。 在朗讀程式已經在執行時，按 _Caps Lock + Shift + F12_ 開啟開發人員模式。 啟用開發人員模式之後，螢幕將會被遮住且將會以醒目方式只顯示可存取的物件和透過程式設計方式向朗讀程式揭露的相關文字。 這可以透過很好的視覺方式向您展示向朗讀程式揭露的資訊。

**利用這些步驟，使用朗讀程式的觸控模式來測試您的應用程式：**

> [!NOTE]
> 朗讀程式會自動在支援 4 個以上觸控點的裝置進入觸控模式。 朗讀程式不支援多個顯示器案例，也不支援主要螢幕上的多點觸控數位板。

1.  熟悉 UI 並探索配置。

    * **使用單指撥動手勢在 UI 之間瀏覽。** 使用向左或向右撥動以在項目之間移動，並使用向上或向下撥動來變更瀏覽的項目類別。 類別包含所有項目、連結、表格、標頭等。 利用單指撥動手勢進行瀏覽類似於使用 _Caps Lock + 方向鍵_來瀏覽。
    * **使用 Tab 鍵手勢來於可設定焦點的元素之間瀏覽。** 使用三指向右或向左撥動，就和使用鍵盤上的 _Tab_ 鍵與 _Shift + Tab_ 鍵來瀏覽一樣。
    * **使用單指大範圍地查看 UI。** 使用單指向上和向下拖曳，或是向左和向右拖曳，可以讓朗讀程式閱讀您手指下方的項目。 您可以使用滑鼠做為替代選項，因為滑鼠會使用和單指拖曳相同的點擊測試邏輯。
    * **使用三指向上撥動來朗讀整個視窗及其所有內容**。 這相當於使用 _Caps Lock + W_ 鍵。

    如果有您無法觸及的重要 UI，那麼您可能有協助工具問題。

2.  與控制項互動，以測試它的主要和次要動作及其捲動行為。

    主要動作包含像是啟用按鈕、放置文字插入點，以及將焦點設定到控制項等事項。 次要動作包含像是選取清單項目或展開提供數個選項的按鈕等動作。

    * 若要測試主要動作：點兩下，或者使用單指按下並使用另一指點選。
    * 若要測試次要動作：點三下，或者使用單指按下並使用另一指點兩下。
    * 若要測試捲動行為：使用兩指撥動，朝所需的方向捲動。

    部分控制項提供其他動作。 若要顯示完整清單，請使用四指點一下。

    如果控制項可以回應滑鼠或鍵盤，但是不能回應主要或次要觸控互動，則該控制項可能需要實作其他 [UI 自動化](https://msdn.microsoft.com/library/windows/desktop/Ee684009)控制項模式。

您也應該考慮使用 [**AccScope**](https://msdn.microsoft.com/library/windows/desktop/Dn433239) 工具，測試 app 的朗讀程式協助工具案例。 [**AccScope 工具主題**](https://msdn.microsoft.com/library/windows/desktop/Dn433239)描述如何設定 **AccScope** 以測試朗讀程式案例。

<span id="Examine_the_UI_Automation_representation_for_your_app"/>
<span id="examine_the_ui_automation_representation_for_your_app"/>
<span id="EXAMINE_THE_UI_AUTOMATION_REPRESENTATION_FOR_YOUR_APP"/>
## <a name="examine-the-ui-automation-representation-for-your-app"></a>檢查適合您應用程式的使用者介面自動化表示法  
先前提及的數個使用者介面自動化測試工具提供一種方式，以刻意不考量應用程式外觀的方式來檢視您的應用程式，並改以使用者介面自動化元素的結構來呈現應用程式。 這就是協助工具案例中使用者介面自動化用戶端 (主要輔助技術) 將如何與您應用程式進行互動的方式。

[**AccScope**](https://msdn.microsoft.com/library/windows/desktop/Dn433239) 工具可為您的應用程式提供特別有趣的檢視，因為您能看見以視覺表示法或清單形式呈現的使用者介面自動化元素。 如果您使用視覺效果，則可利用能與應用程式 UI 視覺化外觀產生關聯的方式，向下切入到組件中。 您甚至可以先測試最早 UI 原型的協助工具，然後將所有邏輯指派到 UI，確定應用程式的視覺化互動與協助工具案例瀏覽可以達成平衡。

您可以測試的一個層面是，是否有任何您不想讓其出現在使用者介面自動化元素檢視中的元素出現在其中。 如果您在檢視中發現您想要省略的元素，或反之遺漏了任何元素，則可以使用 [**AutomationProperties.AccessibilityView**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.automationproperties.accessibilityview) XAML 附加屬性，來調整 XAML 控制項出現在協助工具檢視中的方式。 在您看過基本協助工具檢視之後，在使用方向鍵啟用時，這也是個重新檢查 Tab 順序或部分瀏覽的好時機，可確定使用者能到達控制項檢視中可互動且已公開的每一個組件。

<span id="related_topics"/>
## <a name="related-topics"></a>相關主題  
* [協助工具](accessibility.md)
* [應避免的做法](practices-to-avoid.md)
* [UI 自動化](https://msdn.microsoft.com/library/windows/desktop/Ee684009)
* [Windows 中的協助工具](http://go.microsoft.com/fwlink/p/?LinkId=320802) 

