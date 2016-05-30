---
author: Karl-Bridge-Microsoft
Description: 使用視覺化回饋向使用者顯示偵測、解譯以及處理與 Windows 市集應用程式互動的時間。
title: 視覺化回饋
ms.assetid: bf2f3672-95f0-4c8c-9a72-0934f2d3b767
label: Visual feedback
template: detail.hbs
---

# 視覺化回饋的指導方針

使用視覺化回饋向使用者顯示他們的互動已被偵測、解譯及處理。 視覺化回饋可以透過激發互動意願來協助使用者。 它會指出互動是否成功來改善使用者的控制感應。 它也會轉送系統狀態並減少錯誤。

**重要 API**

-   [**Windows.Devices.Input**](https://msdn.microsoft.com/library/windows/apps/br225648)
-   [**Windows.UI.Input**](https://msdn.microsoft.com/library/windows/apps/br242084)
-   [**Windows.UI.Core**](https://msdn.microsoft.com/library/windows/apps/br208383)


## <span id="Dos_and_don_ts"></span><span id="dos_and_don_ts"></span><span id="DOS_AND_DON_TS"></span>可行與禁止事項

-   無論接觸時間多短，皆應該提供視覺化回饋。 這可幫助使用者：
    -   確定觸控式螢幕是否正常。
    -   識別目標是否啟用觸控或回應觸控。
    -   識別使用者是否未命中目標。
-   立即顯示所有互動事件的回饋。
-   提供不會讓使用者分心的細微直覺式提示回饋。
-   確定觸控目標在進行所有操作的期間均不離指尖。
-   當移動瀏覽受限於某一個方向時，能夠使用撥動手勢來選取項目。
-   不要在可能會干擾應用程式的使用情況下使用觸控視覺效果。 如需詳細資訊，請參閱 [**ShowGestureFeedback**](https://msdn.microsoft.com/library/windows/apps/br241969)。
-   不要在非必要情況下顯示回饋。 除非新增的是其他地方所沒有的值，否則不要顯示視覺化回饋，以讓 UI 畫面保持乾淨、不凌亂。 一律不要顯示已經可以見到之重複文字的工具提示。 應該保留特定情況的工具提示，例如在選取項目時未顯示的截斷內容 (含省略符號的內容)，或是了解或使用應用程式所需的額外資訊。
-   不要在資訊 UI 以外的任何控制項使用長按手勢。  
    **重要** 若已同時啟用水平和垂直移動瀏覽，則可使用長按的方式來選取。    
-   不要針對內建 Windows 8 手勢自訂視覺化回饋行為，因為這可能會造成使用者經驗不一致且混淆。
-   不要在移動瀏覽或拖曳時顯示視覺化回饋；物件在螢幕上的實際移動就已經足夠了。 不過，如果內容區域沒有移動瀏覽或捲動，可以使用視覺效果來指示界限條件。 如需詳細資訊，請參閱[移動瀏覽的指導方針](guidelines-for-panning.md)。
-   不要顯示未識別為目標之控制項的回饋。 當依賴觸控輸入進行要求正確與精確位置的活動時，視覺化回饋就顯得相當重要。 每當偵測到觸控輸入時便顯示回饋，將可以幫助使用者了解應用程式與其控制項所定義的任何自訂目標鎖定啟發學習法。
-   請勿將用於一種輸入類型的回饋行為用在其他輸入類型上。 例如，鍵盤焦點矩形應該只用於鍵盤輸入，而非觸控。

## <span id="Additional_usage_guidance"></span><span id="additional_usage_guidance"></span><span id="ADDITIONAL_USAGE_GUIDANCE"></span>其他用法指導方針

接觸點視覺效果對於要求準確度和精確度的觸控互動相當重要。 例如，您的 app 必須清楚地指示點選位置，讓使用者在未命中目標時知道差了多少，以及必須要做什麼調整。

使用透過 Windows 市集應用程式語言架構 (使用 JavaScript 的 Windows 市集應用程式和使用 C++、C\# 或 Visual Basic 的 Windows 市集應用程式) 公開的平台控制項，來免費取得 Windows 8 視覺效果。 如果您的 app 包含需要自訂回饋的自訂互動，那麼您應當確保回饋是適當的、跨越輸入裝置的，而且不會讓使用者從工作上分心。 在遊戲或繪圖應用程式中這可能成為特別的問，視覺化回饋有可能與重要 UI 相衝突或遮蓋到重要 UI。

[!IMPORTANT] 我們建議您不要變更內建手勢的互動行為。 

### <span id="Feedback_UI"></span><span id="feedback_ui"></span><span id="FEEDBACK_UI"></span>回饋 UI

回饋 UI 一般取決於輸入裝置 (觸控、觸控板、滑鼠、畫筆/手寫筆、鍵盤等等)。 例如，滑鼠的內建回饋通常涉及移動與變更游標、觸控和手寫筆則需要接觸點視覺效果，而鍵盤輸入和瀏覽則使用焦點矩形和醒目提示。

使用 [**ShowGestureFeedback**](https://msdn.microsoft.com/library/windows/apps/br241969) 設定平台手勢的回饋行為。

如果自訂回饋 UI，請確定您提供支援且適合所有輸入模式的回饋。

以下是一些 Windows 內建接觸點視覺效果的範例。

| ![顯示觸控視覺效果的螢幕擷取畫面](images/feedback-touch-cursor.png) | ![顯示滑鼠視覺效果的螢幕擷取畫面](images/feedback-mouse-cursor2.png) | ![顯示手寫筆視覺效果的螢幕擷取畫面](images/feedback-pen-cursor3.png) | ![顯示鍵盤視覺效果的螢幕擷取畫面](images/feedback-keyboard-cursor.png) | 
| --- | --- | --- | --- |
| 觸控視覺效果 | 滑鼠/觸控板視覺效果 | 手寫筆視覺效果 | 鍵盤視覺效果 |

### <span id="Informational_UI"></span><span id="informational_ui"></span><span id="INFORMATIONAL_UI"></span>資訊 UI (快顯視窗)

視覺化回饋的其中一種主要形式為資訊 UI (或去除混淆 UI)。 資訊 UI 可識別並顯示物件相關資訊、描述功能與存取方式，並在必要時提供指引。

以下是 Windows 市集應用程式支援的不同資訊 UI 類型。

-   工具提示
-   豐富工具提示
-   功能表
-   訊息對話方塊
-   飛出視窗

資訊 UI 對於克服指尖閉塞 (障礙) 和增進與應用程式的觸控互動特別有幫助。 它甚至具有專屬的內建手勢：長按。

長按是一種計時互動，一般建議不要在 Windows 8 中使用。 計時互動在這樣的情況下是可以接受的，因為它是做為學習和探索的工具來使用。 建議的時間長度需視資訊 UI 類型而定。 以下為建議的時間閾值。

| 資訊 UI 類型 | 計時 | 啟用 | 使用方式 |
| --- | --- | --- | --- |
| 遮蔽工具提示 (適用於擦選和小型目標) | 0 毫秒 | 是 | 適用於快速提供動作說明。 一般用於命令。 |
| 遮蔽工具提示 (適用於動作) | 200 毫秒 | 是 | |
| 豐富工具提示 | ~2000 毫秒 | 否 | 適用於較慢、較仔細的探索和學習。 一般搭配集合項目使用。 |
| 自顯互動 | ~2000 毫秒 | 否 | |
| 操作功能表 | ~2000 毫秒 | 否 | 顯示與所選物件相關的一組有限的命令。 |
| 飛出視窗 | ~2000 毫秒 | 否 | 顯示與所選物件相關的一組有限的命令。 |

如需提供資訊 UI 的詳細資訊，請參閱[配置您的 UI](https://msdn.microsoft.com/library/windows/apps/hh465304) 和[顯示快顯視窗](https://msdn.microsoft.com/library/windows/apps/hh738362)。

### <span id="Tooltips"></span><span id="tooltips"></span><span id="TOOLTIPS"></span>工具提示

使用工具提示可在要求使用者執行動作前，先顯示控制項的更多資訊。

當使用者在控制項或物件上執行長按手勢 (或偵測到暫留事件) 時，會自動出現工具提示 ([**Tooltip**](https://msdn.microsoft.com/library/windows/apps/br229763))。 當接觸點或游標移開控制項或物件時，工具提示會消失。 工具提示可以包含文字和影像，但它無法互動。

### <span id="Occlusion_tooltips_small"></span><span id="occlusion_tooltips_small"></span><span id="OCCLUSION_TOOLTIPS_SMALL"></span>小型目標的遮蔽工具提示

遮蔽工具提示描述被遮蔽的目標。 在定位和啟動的項目小於標準觸控目標大小時 (例如網頁上的超連結)，這些工具提示相當有用。

您可以在超出特定的時間閾值之後，將這些工具提示取代為資訊快顯通知。 例如，可使用遮蔽工具提示顯示被遮蔽的超連結文字，然後將工具提示取代為包含 URL 的快顯通知。

### <span id="Occlusion_tooltips_actions"></span><span id="occlusion_tooltips_actions"></span><span id="OCCLUSION_TOOLTIPS_ACTIONS"></span>適用於動作和命令的遮蔽工具提示

這些工具提示描述當使用者舉起原本在某個元素的手指時所發生的動作或命令。 在指向和啟動按鈕或類似的控制項時，這些工具提示很有用。

小型目標工具提示可以在超出特定的時間閾值之後顯示動作工具提示。 在此情況下，應擴充小型目標工具提示，使其包含動作工具提示中的其他資訊。

### <span id="Rich_tooltip"></span><span id="rich_tooltip"></span><span id="RICH_TOOLTIP"></span>豐富工具提示

這些工具提示顯示與元素相關的次要資訊。 例如，豐富工具提示可以是影像的文字描述、被截斷標題的完整內容，或是與目標相關的其他資訊。

豐富工具提示一般包含不是立即性的資訊，而且在某些情況下，太快顯示反而會讓使用者分心。 較長的時間閾值可以讓使用者在取得資訊方面較為仔細。

顯示豐富工具提示之後，只要使用者提起手指，該物件就不再處於啟用狀態。 這是因為從工具提示獲得的資訊可能影響使用者，讓使用者不想啟用該項目。

豐富工具提示中的視覺設計和資訊應該要清楚，並且要比標準工具提示中的視覺設計和資訊更有內容。

### <span id="Context_menu"></span><span id="context_menu"></span><span id="CONTEXT_MENU"></span>操作功能表

操作功能表 ([**PopupMenu**](https://msdn.microsoft.com/library/windows/apps/br208693)) 是 Windows 市集應用程式中的輕量型功能表，可讓使用者在文字或 UI 物件上立即存取動作 (如剪貼簿命令)。

針對觸控功能最佳化的操作功能表包含兩個部分。 視覺提示會在進行按住互動之後顯示。 接著，在提示消失且使用者舉起手指之後，就會顯示操作功能表本身。

下列影像示範如何在選取範圍內或在移駐夾上點選 (也可以使用長按) 來叫用文字的預設操作功能表。

![在選取範圍內或在移駐夾上進行點選 (或長按)，以叫用操作功能表。](images/textselection-show-context.png)

請參閱[新增操作功能表](https://msdn.microsoft.com/library/windows/apps/hh465300)。

### <span id="Message_dialog"></span><span id="message_dialog"></span><span id="MESSAGE_DIALOG"></span>訊息對話方塊

使用訊息對話方塊 ([**MessageDialog**](https://msdn.microsoft.com/library/windows/apps/br208674))，根據使用者的動作或應用程式狀態，在使用者繼續之前提示他們回應。 需要明確的使用者互動，且在使用者回應之前會封鎖對 app 的輸入。

![錯誤訊息的訊息對話方塊](images/messagedialog.png)

以下是顯示訊息對話方塊的一些典型原因。

-   顯示緊急資訊
-   繼續執行前問一個問題
-   顯示錯誤訊息

請參閱[新增訊息對話方塊](https://msdn.microsoft.com/library/windows/apps/hh738361)。

### <span id="Flyout"></span><span id="flyout"></span><span id="FLYOUT"></span>飛出視窗

飛出視窗 ([**Flyout**](https://msdn.microsoft.com/library/windows/apps/br211726)) 是輕量型 UI 面板，在點選、按一下或其他啟用時顯示，用來對使用者顯示與目前活動有關的資訊、問題或功能表選項。 它可以消失關閉 (也就是在使用者觸碰或按一下飛出視窗面板或按 ESC 時消失)。 換句話說，飛出視窗可以被忽略。

它與工具提示不同，飛出視窗可以接受輸入。 與訊息對話方塊不同的是，app 仍在作用中，並可以接受輸入。

![含有確認的飛出視窗](images/flyout.png)

請參閱[新增飛出視窗和功能表](https://msdn.microsoft.com/library/windows/apps/hh465325)。

## <span id="related_topics"></span>相關文章

**適用於設計人員**
* [移動瀏覽的指導方針](guidelines-for-panning.md)

**適用於開發人員**
* [自訂使用者互動](https://msdn.microsoft.com/library/windows/apps/mt185599)

**範例**
* [基本輸入範例](http://go.microsoft.com/fwlink/p/?LinkID=620302)
* [低延遲輸入範例](http://go.microsoft.com/fwlink/p/?LinkID=620304)
* [使用者互動模式範例](http://go.microsoft.com/fwlink/p/?LinkID=619894)
* [焦點視覺效果範例](http://go.microsoft.com/fwlink/p/?LinkID=619895)

**封存範例**
* [輸入：XAML 使用者輸入事件範例](http://go.microsoft.com/fwlink/p/?linkid=226855)
* [輸入：裝置功能範例](http://go.microsoft.com/fwlink/p/?linkid=231530)
* [輸入：觸控點擊測試範例](http://go.microsoft.com/fwlink/p/?linkid=231590)
* [XAML 捲動、移動瀏覽和縮放範例](http://go.microsoft.com/fwlink/p/?linkid=251717)
* [輸入：簡化的筆跡範例](http://go.microsoft.com/fwlink/p/?linkid=246570)
* [輸入：Windows 8 手勢範例](http://go.microsoft.com/fwlink/p/?LinkId=264995)
* [輸入：操作和手勢 (C++) 範例](http://go.microsoft.com/fwlink/p/?linkid=231605)
* [DirectX 觸控輸入範例](http://go.microsoft.com/fwlink/p/?LinkID=231627)
 

 






<!--HONumber=May16_HO2-->


