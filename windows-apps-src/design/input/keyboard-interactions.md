---
Description: 了解如何設計和最佳化您的 UWP app，使其可以同時為鍵盤使用者及行動不便或有其他可存取性需求的人士提供最佳的使用者體驗。
title: 鍵盤互動
ms.assetid: FF819BAC-67C0-4EC9-8921-F087BE188138
label: Keyboard interactions
template: detail.hbs
keywords: keyboard, accessibility, navigation, focus, text, input, user interactions, gamepad, remote, 鍵盤, 協助工具, 瀏覽, 焦點, 文字, 輸入, 使用者互動, 遊戲台, 遙控器
ms.date: 03/29/2017
ms.topic: article
pm-contact: chigy
design-contact: kimsea
dev-contact: niallm
doc-status: Published
ms.openlocfilehash: 449f0c81bdd54d99ef0977ca1c9b6ba10ba5eae7
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/20/2019
ms.locfileid: "74258353"
---
# <a name="keyboard-interactions"></a>鍵盤互動

![鍵盤主角影像](images/keyboard/keyboard-hero.jpg)

了解如何設計和最佳化您的 UWP app，使其可以同時為鍵盤使用者及行動不便或有其他可存取性需求的人士提供最佳的使用者體驗。

跨裝置，鍵盤輸入是整體通用 Windows 平台 (UWP) 互動體驗很重要的一部分。 精心設計的鍵盤體驗可讓使用者有效率瀏覽您的應用程式 UI，不需要從鍵盤移開手，就能存取應用程式的完整功能。

![鍵盤與遊戲台影像](images/keyboard/keyboard-gamepad.jpg)

***Common interaction patterns are shared between keyboard and gamepad***

本主題中，我們會著重在針對電腦鍵盤的 UWP app 設計上。 However, a well-designed keyboard experience is important for supporting accessibility tools such as Windows Narrator, using [software keyboards](#software-keyboard) such as the touch keyboard and the On-Screen Keyboard (OSK), and for handling other input device types, such as the Xbox gamepad and remote control.

本文討論的許多指導方針和建議，包括[對焦視覺效果](#focus-visuals)、[便捷鍵](#access-keys)和 [UI 瀏覽](#navigation)，也適用於這些其他案例。

**注意**  雖然硬體和軟體鍵盤是用於文字輸入，但是本主題的焦點在於瀏覽與互動。

## <a name="built-in-support"></a>內建支援

滑鼠和鍵盤是最常使用的電腦周邊裝置，因此是電腦使用體驗的基礎部分。 電腦使用者期望來自系統和個別應用程式完整且一致的體驗，以回應鍵盤輸入。

所有 UWP 控制項都包括豐富鍵盤體驗與使用者互動的內建支援，而平台本身則為您建立最適合自訂控制項和應用程式的鍵盤體驗，提供了廣泛的基礎。

![鍵盤與手機的影像](images/keyboard/keyboard-phone.jpg)

***UWP supports keyboard with any device***

## <a name="basic-experiences"></a>基本體驗
![以焦點為基礎的裝置](images/keyboard/focus-based-devices.jpg)

如前所述，例如 Xbox 遊戲台與遙控器等輸入裝置，以及例如「朗讀程式」等協助工具，都共用瀏覽和命令功能的許多鍵盤輸入體驗。 在輸入類型和工具之間的這個共同體驗減少您的額外工作，並對通用 Windows 平台的「建置一次，隨處執行」目標做出貢獻。

Where necessary, we'll identify key differences you should be aware of and describe any mitigations you should consider.

以下是本主題中討論的裝置和工具：

| 裝置/工具                       | 說明     |
|-----------------------------------|-----------------|
|鍵盤 (硬體與軟體)   |In addition to the standard hardware keyboard, UWP applications support two software keyboards: the [touch (or software) keyboard](#software-keyboard) and the [On-Screen Keyboard](#on-screen-keyboard).|
|遊戲台與遙控器         |Xbox 遊戲台與遙控器是[10 英呎經驗](../devices/designing-for-tv.md)中的基本輸入裝置。 如需 UWP 支援遊戲台與遙控器的特定詳細資訊，請參閱[遊戲台與遙控器的互動](gamepad-and-remote-interactions.md)。|
|螢幕助讀程式（朗讀程式）          |「朗讀程式」是 Windows 內建螢幕助讀程式，提供唯一互動體驗和功能，但仍依賴基本鍵盤瀏覽和輸入。 如需朗讀程式詳細資訊，請參閱[開始使用朗讀程式](https://support.microsoft.com/help/22798/windows-10-complete-guide-to-narrator)。|

## <a name="custom-experiences-and-efficient-keyboarding"></a>自訂體驗與有效的鍵盤
如前所述，若要確定應用程式能適用於不同技能、能力及期望的使用者，鍵盤支援是不可或缺的一部分。 建議您優先進行以下作業。
- 支援鍵盤瀏覽與互動
    - 請確定會將可採取動作的項目視為定位停駐點 (非可採取動作的項目則不是)，且瀏覽順序是有邏輯且可預期的 (請參閱[定位停駐點](#tab-stops))
    - 將最初的焦點設定在最合乎邏輯的元素 (請參閱[最初的焦點](#initial-focus))
    - 為「內部瀏覽」提供方向鍵瀏覽 (請參閱[瀏覽](#navigation))
- 支援鍵盤快速鍵
    - 提供快速控制項目提供快速鍵 (請參閱[加速鍵](#accelerators))
    - Provide access keys to navigate your application's UI (see [Access keys](access-keys.md))

### <a name="focus-visuals"></a>Focus visuals

UWP 支援單一焦點視覺效果設計，適用於所有輸入類型和體驗。
![Focus visual](images/keyboard/focus-visual.png)

焦點視覺效果：

- 當 UI 項目從鍵盤和/或遊戲台/遙控器取得焦點時顯示
- 呈現為反白顯示的 UI 項目框線，表示可以執行動作
- 協助使用者瀏覽 app UI，而不迷失
- 可以針對您的應用程式自訂 (請參閱[高可見度焦點視覺效果](guidelines-for-visualfeedback.md#high-visibility-focus-visuals))

**注意** UWP 焦點視覺效果與「朗讀程式」焦點矩形不同。

### <a name="tab-stops"></a>定位停駐點

若要搭配使用控制項 (包括瀏覽元素) 與鍵盤，控制項必須有焦點。 One way for a control to receive keyboard focus is to make it accessible through tab navigation by identifying it as a tab stop in your application's tab order.

For a control to be included in the tab order, the [IsEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_IsEnabled) property must be set to **true** and the [IsTabStop](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Control#Windows_UI_Xaml_Controls_Control_IsTabStop) property must be set to **true**.

To specifically exclude a control from the tab order, set the [IsTabStop](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Control#Windows_UI_Xaml_Controls_Control_IsTabStop) property to **false**.

根據預設，Tab 順序反映 UI 元素建立的順序。 例如，如果 `StackPanel` 包含 `Button`、`Checkbox` 和 `TextBox`，Tab 順序就是 `Button`、`Checkbox` 和 `TextBox`。

You can override the default tab order by setting the [TabIndex](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Control#Windows_UI_Xaml_Controls_Control_TabIndex) property.

#### <a name="tab-order-should-be-logical-and-predictable"></a>Tab 順序應該有邏輯與可預測的

精心設計的鍵盤瀏覽，使用有邏輯與可預測的 Tab 順序，可讓您的應用程式更直覺，可協助使用者更有效且更有效率瀏覽、探索及存取功能。

All interactive controls should have tab stops (unless they are in a [group](#control-group)), while non-interactive controls, such as labels, should not.

避免讓焦點在應用程式中四處跳躍的自訂定位順序。 例如，表單中的控制項清單應該會有由上至下和由左至右的定位順序 (取決於地區)。

See [Keyboard accessibility](../accessibility/keyboard-accessibility.md) for more details about customizing tab stops.

#### <a name="try-to-coordinate-tab-order-and-visual-order"></a>試著調整 Tab 順序和視覺順序

Coordinating tab order and visual order (also referred to as reading order or display order) helps reduce confusion for users as they navigate through your application's UI.

試著先同時依 Tab 順序和視覺順序排列並呈現最重要的命令、控制項及內容。 不過，實際的顯示位置可能取決於上層配置容器，以及會影響配置之子元素的某些屬性。 特別的是，使用格線隱喻或表格隱喻的配置可以有與 Tab 順序完全不同的視覺順序。

**注意** 視覺順序也是視地區設定和語言而定。

### <a name="initial-focus"></a>最初的焦點

最初的焦點指定當應用程式或網頁第一次啟動或啟用時取得焦點的 UI 項目。 When using a keyboard, it is from this element that a user starts interacting with your application's UI.

For UWP apps, initial focus is set to the element with the highest [TabIndex](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Control#Windows_UI_Xaml_Controls_Control_TabIndex) that can receive focus. 忽略容器控制項的子元素。 在平局，視覺化樹狀結構上的第一個元素會接收焦點。

#### <a name="set-initial-focus-on-the-most-logical-element"></a>將最初的焦點設定在最合乎邏輯的元素

將最初的焦點設定在使用者啟動您的 app 或瀏覽網頁時，最可能會採取的第一個或主要動作的 UI 元素上。 一些範例包括：
-   相片 app，焦點設定到影像中心的第一個項目
-   音樂 app，焦點設定到播放按鈕

#### <a name="dont-set-initial-focus-on-an-element-that-exposes-a-potentially-negative-or-even-disastrous-outcome"></a>Don't set initial focus on an element that exposes a potentially negative, or even disastrous, outcome

This level of functionality should be a user's choice. 最初的焦點設定到會產生重要結果的元素，可能會造成意外的資料遺失或系統存取。 For example, don't set focus to the delete button when navigating to an e-mail.

See [Focus navigation](focus-navigation.md) for more details about overriding tab order.

### <a name="navigation"></a>瀏覽

鍵盤瀏覽通常是透過 Tab 鍵和方向鍵來支援。

![Tab 鍵與方向鍵](images/keyboard/tab-and-arrow.png)

根據預設，UWP 控制項依循這些基本鍵盤行為：
-   **Tab 鍵**依 Tab 順序在可動作/使用中控制項之間瀏覽。
-   **Shift + Tab** 依反向的 Tab 順序瀏覽控制項。 如果使用者已使用方向鍵瀏覽到控制項內部，焦點設為控制項內部的最後一個已知的值。
-   **方向鍵**公開控制項特定的「內部瀏覽」。當使用者進入「內部瀏覽」時，方向鍵不會瀏覽移出控制項。 一些範例包括：
    -   Up/Down arrow key moves focus inside `ListView` and `MenuFlyout`
    -   Modify currently selected values for `Slider` and `RatingsControl`
    -   Move caret inside `TextBox`
    -   Expand/collapse items inside `TreeView`

Use these default behaviors to optimize your application's keyboard navigation.

#### <a name="use-inner-navigation-with-sets-of-related-controls"></a>使用「內部瀏覽」搭配相關控制項組

Providing arrow key navigation into a set of related controls reinforces their relationship within the overall organization of your application's UI.

例如，此處顯示的 `ContentDialog` 控制項為水平列按鈕預設提供內部瀏覽 (對於自訂控制項，請參閱[控制項群組](#control-group)一節)。

![對話方塊範例](images/keyboard/dialog.png)

***Interaction with a collection of related buttons is made easier with arrow key navigation***

如果在單欄中顯示項目時，向上/向下鍵瀏覽項目。 在單列中顯示項目時，向左/向右鍵瀏覽項目。 如果是多欄項目，所有 4 個方向鍵瀏覽。

#### <a name="define-a-single-tab-stop-for-a-collection-of-related-controls"></a>Define a single tab stop for a collection of related controls

By defining a single tab stop for a collection of related, or complementary, controls, you can minimize the number of overall tab stops in your app.

例如，下列影像顯示兩個堆疊的 `ListView` 控制項。 在左側的影像會顯示搭配定位停駐點使用方向鍵在 `ListView` 之間瀏覽，右邊的影像會顯示，透過不使用 tab 鍵周遊父控制項，子元素之間的瀏覽變得更容易且更有效率。


<table>
  <td><img src="images/keyboard/arrow-and-tab.png" alt="arrow and tab" /></td>
  <td><img src="images/keyboard/arrow-only.png" alt="arrow only" /></td>
</table>

***Interaction with two stacked ListView controls can be made easier and more efficient by eliminating the tab stop and navigating with just arrow keys.***

瀏覽[控制項群組](#control-group)一節，以了解如何將最佳化範例套用至您的應用程式 UI。

### <a name="interaction-and-commanding"></a>互動和命令

一旦控制項具有焦點時，使用者可以使用特定鍵盤輸入，與它互動和叫用任何相關的功能。

#### <a name="text-entry"></a>文字輸入

對於專為文字輸入而設計的控制項如 `TextBox` 和 `RichEditBox`，所有鍵盤輸入都是用於輸入或瀏覽文字，優先順序高於其他鍵盤命令。 例如，`AutoSuggestBox` 控制項的下拉式功能表不會辨識**空格**鍵為選取命令。

![文字輸入](images/keyboard/text-entry.png)

#### <a name="space-key"></a>空格鍵

不在文字輸入模式下，**空格**鍵會叫用與已取得焦點之控制項相關的動作或命令 (就像是使用觸控方式輕觸，或使用滑鼠按一下)。

![空格鍵](images/keyboard/space-key.png)

#### <a name="enter-key"></a>Enter 鍵

**Enter** 鍵可執行各種一般的使用者互動，視具有輸入焦點的控制項而定：
-   啟動命令控制項例如 `Button` 或 `Hyperlink`。 為了避免使用者困擾，**Enter** 鍵也啟動看起來像是命令控制項之類的控制項，例如 `ToggleButton` 或 `AppBarToggleButton`。
-   顯示控制項例如 `ComboBox` 和 `DatePicker` 的選擇器 UI。 **Enter** 鍵也認可及關閉選擇器 UI。
-   啟動清單控制項例如 `ListView`、`GridView` 和 `ComboBox`。
    -   **Enter** 鍵為清單及方格項目執行選取動作，如同**空格**鍵，除非這些項目另有相關聯的其他動作（開啟新視窗）。
    -   如果與控制項有相關的其他動作，**Enter** 鍵執行其他動作，而**空格**鍵執行選取動作。

**注意** **Enter** 鍵和**空格**鍵不一定執行相同的動作，但通常大致相同。

![Enter 鍵](images/keyboard/enter-key.png)

Esc 鍵可讓使用者取消暫時性 UI（以及該 UI 中的任何持續動作）。

這個體驗的範例包括：
-   使用者開啟具有選取值的 `ComboBox`，使用方向鍵來將焦點選取範圍移到新的值。 按下 Esc 鍵關閉 `ComboBox`，並將選取的值重設回原始值。
-   使用者叫用電子郵件永久刪除動作，並出現 `ContentDialog` 提示確認動作。 使用者決定這不是所要的動作，並按下 **Esc** 鍵來關閉對話方塊。 因為 **Esc** 鍵與**取消**按鈕相關聯，所以會關閉對話方塊和取消動作。 **Esc** 鍵只會影響暫時性 UI，不會關閉 (或向後瀏覽) app UI。

![Esc 鍵](images/keyboard/esc-key.png)

#### <a name="home-and-end-keys"></a>Home 和 End 鍵

**Home** 和 **End** 鍵可以讓使用者捲動到 UI 區域的開頭或結尾。

這個體驗的範例包括：
-   對於 `ListView` 和 `GridView` 控制項，**Home** 鍵將焦點移至第一個項目，並將它捲動至檢視中，而 **End** 鍵將焦點移至最後一個項目，並將它捲動至檢視中。
-   對於 `ScrollView` 控制項，**Home** 鍵捲動至區域頂端，而 **End** 鍵捲動到區域底部（不會變更焦點）。

![Home 和 End 鍵](images/keyboard/home-and-end.png)

#### <a name="page-up-and-page-down-keys"></a>Page up 和 Page down 鍵

**Page** 鍵可以讓使用者以不同增量捲動 UI 區域。

例如，對於 `ListView` 和 `GridView` 控制項，**Page up** 鍵向上捲動區域「一頁」（通常是檢視區高度），並將焦點移至區域上方。 或者，**Page down** 鍵向下捲動區域一頁，並將焦點移至區域底部。

![Page up 和 Page down 鍵](images/keyboard/page-up-and-down.png)

### <a name="keyboard-shortcuts"></a>鍵盤快速鍵

Keyboard shortcuts can make your app easier to use by providing both enhanced support for accessibility and improved efficiency for keyboard users.

In addition to supporting keyboard navigation and activation in your app, it is also good practice to provide shortcuts for your application's functionality. Tab navigation provides a good, basic level of keyboard support, but with more complex UI you might want to add support for shortcut keys as well. 

鍵盤快速鍵是一種鍵盤組合，可讓使用者更有效率地存取應用程式功能，提高工作效率。 目前有兩種捷徑：
-   [Accelerators](#accelerators) are shortcuts that invoke an app command. Your app may or may not provide specific UI that corresponds to the command. Accelerators typically consist of the Ctrl key plus a letter key.
-   [Access keys](#access-keys) are shortcuts that set focus to specific UI in your application. Access keys typicaly consist of the Alt key plus a letter key.

Providing consistent keyboard shortcuts that support similar tasks across applications makes them much more useful and powerful and helps users remember them.

#### <a name="accelerators"></a>加速器

Accelerators help users perform common actions in an application much more quickly and efficiently. 

快速鍵的範例：
-   Pressing Ctrl + N key anywhere in the **Mail** app launches a new mail item.
-   Pressing Ctrl + E key anywhere in Microsoft Edge (and many Microsoft Store applications) launches search.

加速鍵具有下列特性：
-   They primarily use Ctrl and Function key sequences (Windows system shortcut keys also use Alt + non-alphanumeric keys and the Windows logo key).
-   它們只會指派給最常用的命令。
-   它們需要被記住，而且只會記載於功能表、工具提示和說明。
-   They have effect throughout the entire application, when supported.
-   They should be assigned consistently as they are memorized and not directly documented.

#### <a name="access-keys"></a>便捷鍵

如需使用 UWP 支援便捷鍵更深入的資訊，請參閱[便捷鍵](access-keys.md)頁面。

便捷鍵協助運動功能障礙的使用者能夠一次按一個按鍵，在 UI 中的特定項目上執行動作。 此外，便捷鍵可用來傳達其他快速鍵，協助進階使用者快速執行動作。

便捷鍵具有下列特性：
-   它們使用 Alt 鍵和英數字元按鍵。
-   它們是主要的協助工具。
-   They are documented directly in the UI, adjacent to the control, through [Key Tips](access-keys.md).
-   它們只在目前的視窗中有作用，可瀏覽到對應的功能表項目或控制項。
-   Access keys should be assigned consistently to commonly used commands (especially commit buttons), whenever possible.
-   它們會當地語系化。

#### <a name="common-keyboard-shortcuts"></a>常用的鍵盤快速鍵

The following table is a small sample of frequently used keyboard shortcuts. 

| [動作]                               | 按鍵命令                                      |
|--------------------------------------|--------------------------------------------------|
| 全選                           | Ctrl+A                                           |
| 連續選取                  | Shift+方向鍵                                  |
| [儲存]                                 | Ctrl+S                                           |
| 尋找                                 | Ctrl+F                                           |
| Print                                | Ctrl+P                                           |
| [複製]                                 | Ctrl+C                                           |
| 剪下                                  | Ctrl+X                                           |
| 貼上                                | Ctrl+V                                           |
| 復原                                 | Ctrl+Z                                           |
| 下一個 Tab 點                             | Ctrl+Tab                                         |
| 關閉索引標籤                            | Ctrl+F4 或 Ctrl+W                                |
| 語意式縮放                        | Ctrl++ 或 Ctrl+-                                 |

For a comprehensive list of Windows system shortcuts, see [keyboard shortcuts for Windows](https://support.microsoft.com/help/12445/windows-keyboard-shortcuts). For common application shortcuts, see [keyboard shortcuts for Microsoft applications](https://support.microsoft.com/help/13805/windows-keyboard-shortcuts-in-apps).

## <a name="advanced-experiences"></a>進階體驗

在本節中，我們討論 UWP app 支援的一些更複雜鍵盤互動體驗，以及您的 app 在不同的裝置上使用和搭配不同的工具時，您應該要知道的一些行為。

### <a name="control-group"></a>Control group

您可以在「控制項群組」（或方向區域）中群組一組相關 (或補充) 控制項，如此可啟用使用方向鍵的「內部瀏覽」。 控制項群組是一個啟用使用，或者您可以在控制項群組中指定多個啟用使用。

#### <a name="arrow-key-navigation"></a>方向鍵瀏覽

UI 區域中有類似、相關控制項的群組時，使用者期望有方向鍵瀏覽的支援：
-   `AppBarButtons` in a `CommandBar`
-   `ListItems` or `GridItems` inside `ListView` or `GridView`
-   `Buttons` inside `ContentDialog`

UWP 控制項預設支援方向鍵瀏覽。 對於自訂配置和控制項群組，使用 `XYFocusKeyboardNavigation="Enabled"` 提供類似的行為。

Consider adding support for arrow key navigation when using the following controls:

<table>
  <tr>
    <td>
      <p><img src="images/keyboard/dialog.png" alt="Dialog buttons"/></p>
      <p><sup>Dialog buttons</sup></p>
      <p><img src="images/keyboard/radiobutton.png" alt="Radio buttons"/></p>
      <p><sup>RadioButtons</sup></p>     
    </td>
    <td>
      <p><img src="images/keyboard/appbar.png" alt="AppBar buttons"/></p>
      <p><sup>AppBarButtons</sup></p>
      <p><img src="images/keyboard/list-and-grid-items.png" alt="List and Grid items"/></p>
      <p><sup>ListItems and GridItems</sup></p>
    </td>    
  </tr>
</table>

#### <a name="tab-stops"></a>定位停駐點

Depending on your application's functionality and layout, the best navigation option for a control group might be a single tab stop with arrow navigation to child elements, multiple tab stops, or some combination.

##### <a name="use-multiple-tab-stops-and-arrow-keys-for-buttons"></a>對按鈕使用多個定位停駐點和方向鍵

協助工具使用者依賴建立良好鍵盤瀏覽規則，這通常是不使用方向鍵瀏覽按鈕集合。 不過，非視障使用者可能會覺得這是自然的行為。

在本案例中預設 UWP 行為範例為 `ContentDialog`。 雖然可以使用方向鍵在按鈕之間瀏覽，每個按鈕也是定位停駐點。

##### <a name="assign-single-tab-stop-to-familiar-ui-patterns"></a>將單一定位停駐點指派給熟悉的 UI 模式

如果您的配置依循控制項群組的已知 UI 模式，指派一個定位停駐點至群組，可以改善使用者的瀏覽效率。

範例包含：
-   `RadioButtons`
-   Multiple `ListViews` that look like and behave like a single `ListView`
-   外觀及操作像磚格線的任何 UI（例如 [開始] 功能表磚）

#### <a name="specifying-control-group-behavior"></a>指定控制項群組行為

使用下列 API 來支援自訂控制項群組行為（本主題稍後將會詳細討論全部行為）：

-   [XYFocusKeyboardNavigation](focus-navigation.md#2d-directional-navigation-for-keyboard) 可讓方向鍵在控制項之間瀏覽
-   [TabFocusNavigation](focus-navigation.md#tab-navigation) 指示有多個定位停駐點或單一定位停駐點
-   [FindFirstFocusableElement 和 FindLastFocusableElement](focus-navigation-programmatic.md#find-the-first-and-last-focusable-element) 使用 **Home** 鍵將焦點設定在第一個項目上，使用**End** 鍵將焦點設定在最後一個項目上

下圖顯示相關選項按鈕控制項群組的直覺鍵盤瀏覽行為。 在此情況下，我們建議控制項群組使用單一定位停駐點、使用方向鍵在選項按鈕之間進行內部瀏覽、**Home** 鍵繫結至第一個選項按鈕，以及 **End** 鍵繫結至最後一個選項按鈕。

![總結](images/keyboard/putting-it-all-together.png)

### <a name="keyboard-and-narrator"></a>鍵盤與朗讀程式

「朗讀程式」是一個針對鍵盤使用者（也會支援其他輸入類型）設計的 UI 協助工具。 不過，「朗讀程式」功能超出 UWP app 支援的鍵盤互動，為朗讀程式設計 UWP app 時須特別留意。 ([朗讀程式基本資訊頁面](https://support.microsoft.com/help/22808/windows-10-narrator-basics)會引導您完成朗讀程式的使用者體驗。)

UWP 鍵盤行為以及「朗讀程式」支援的鍵盤行為之間的一些差異包括：
-   額外的按鍵組合，用來瀏覽到未透過標準鍵盤瀏覽所公開的 UI 元素，例如 Caps lock + 方向鍵來朗讀控制項標籤。
-   瀏覽到停用的項目。 根據預設，停用的項目不會透過標準鍵盤瀏覽公開。
-   根據 UI 細微度，更快速地瀏覽的控制項「檢視」。 使用者可以瀏覽到項目、字元、字詞、行、段落、連結、標題、表格、地標及建議。 標準鍵盤瀏覽將這些物件公開為一般清單，這可能會使瀏覽變得麻煩，除非您提供快速鍵。

#### <a name="case-study--autosuggestbox-control"></a>案例研究 – AutoSuggestBox 控制項

`AutoSuggestBox` 的搜尋按鈕對於使用 tab 與方向鍵標準鍵盤瀏覽無法容易，因為使用者可以按下 **Enter** 鍵來提交搜尋查詢。 不過，當使用者按 Caps Lock + 方向鍵，可以透過朗讀程式存取它。

![自動建議鍵盤焦點](images/keyboard/auto-suggest-keyboard.png)

*With keyboard, users press the* ***Enter*** *key to submit search query*

<table>
  <tr>
    <td>
      <p><img src="images/keyboard/auto-suggest-narrator-1.png" alt="autosuggest narrator focus"/></p>
      <p><em>With Narrator, users press the <strong>Enter</strong> key to submit search query</em></p>
    </td>
    <td>
      <p><img src="images/keyboard/auto-suggest-narrator-2.png" alt="autosuggest narrator focus on search"/></p>
      <p><em>With Narrator, users are also able to access the search button using the <strong>Caps Lock + Right arrow key</strong>, then pressing <strong>Space</strong> key</em></p>
    </td>
  </tr>
</table>

### <a name="keyboard-and-the-xbox-gamepad-and-remote-control"></a>鍵盤與 Xbox 遊戲台與遙控器

Xbox 遊戲台與遙控器支援許多 UWP 鍵盤行為和體驗。 不過，因為缺少鍵盤的各種按鍵選項，遊戲台與遙控器缺少許多鍵盤最佳化（遙控器比遊戲台更受限）。

See [Gamepad and remote control interactions](gamepad-and-remote-interactions.md) for more detail on UWP support for gamepad and remote control input.

以下顯示鍵盤、遊戲台與遙控器之間的一些按鍵對應。

| **鍵盤**  | **遊戲台**                         | **Remote control**  |
|---------------|-------------------------------------|---------------------|
| 空格鍵         | A 按鍵                            | 選取按鈕       |
| Enter         | A 按鍵                            | 選取按鈕       |
| ESC 鍵        | B 按鍵                            | [上一頁] 按鈕         |
| Home/End      | 無                                 | 無                 |
| Page Up/Down  | 發射鍵用於垂直捲動，緩衝鍵用於水平捲動   | 無                 |

設計 UWP 應用程式使用於遊戲台與遙控器時，應該留意的一些重要差異包括：
-   文字輸入需要使用者按 A 啟動文字控制項。
-   焦點瀏覽不限於控制項群組，使用者可以自由瀏覽到 app 中任何可設定焦點的 UI 元素。

    **注意** 焦點可以移到按下按鍵方向中任何可設定焦點的 UI 元素，除非它是在重疊 UI 中或已指定[焦點佔用](gamepad-and-remote-interactions.md#focus-engagement)，這會防止焦點鍵入/離開區域，直到使用 A 按鍵佔用/離開。 如需詳細資料，請參閱[方向瀏覽](#directional-navigation)一節。
-   方向鍵和左搖桿按鍵用來在控制項之間移動焦點，以及用於內部瀏覽。

    **注意** 遊戲台與遙控器只瀏覽至與所按下方向鍵的視覺順序相同的項目。 沒有可接收焦點的後續元素時，該方向的瀏覽會停用。 根據情形，鍵盤使用者不一定有該限制。 請參閱[內建的鍵盤最佳化](#built-in-keyboard-optimization)一節，以取得詳細資訊。

#### <a name="directional-navigation"></a>Directional navigation

方向瀏覽是由 UWP[焦點管理員](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.FocusManager)協助程式類別所管理，它接受所按下的方向鍵 (鍵盤方向鍵、遊戲台與遙控器方向鍵)，並嘗試在對應的視覺的方向中移動焦點。

Unlike the keyboard, when an app opts out of [Mouse Mode](gamepad-and-remote-interactions.md#mouse-mode), directional navigation is applied across the entire application for gamepad and remote control. See [Gamepad and remote control interactions](gamepad-and-remote-interactions.md) for more detail on directional navigation optimization.

**注意** 使用鍵盤 Tab 鍵瀏覽並不是方向瀏覽。 如需詳細資訊，請參閱[定位停駐點](#tab-stops)一節。

<table>
  <tr>
    <td>
      <p><img src="images/keyboard/directional-navigation.png" alt="directional navigation"/></p>
      <p><em><strong>Directional navigation supported</strong></br>Using directional keys (keyboard arrows, gamepad and remote control D-pad), user can navigate between different controls.</em></p>
    </td>
    <td>
      <p><img src="images/keyboard/no-directional-navigation.png" alt="no directional navigation"/></p>
      <p><em><strong>Directional navigation not supported</strong> </br>使用者無法使用方向鍵在不同的控制項之間瀏覽。 Other methods of navigating between controls (tab key) are not impacted.</em></p>
    </td>
  </tr>
</table>

### <a name="built-in-keyboard-optimization"></a>Built in keyboard optimization

根據使用的配置和控制項，UWP app 可以專為鍵盤輸入進行最佳化。

下列範例示範已指派給一個定位停駐點的清單項目、方格項目，以及功能表項目的群組 (請參閱[定位停駐點](#tab-stops)一節)。 當群組有焦點時，使用方向鍵依對應的視覺順序執行內部瀏覽 (請參閱[瀏覽](#navigation)一節)。

![單欄方向鍵瀏覽](images/keyboard/single-column-arrow.png)

***Single Column Arrow Key Navigation***

![單列方向鍵瀏覽](images/keyboard/single-row-arrow.png)

***Single Row Arrow Key Navigation***

![多欄和列方向鍵瀏覽](images/keyboard/multiple-column-and-row-navigation.png)

***Multiple Column/Row Arrow Key Navigation***

#### <a name="wrapping-homogeneous-list-and-grid-view-items"></a>換行同質清單和方格檢視項目

方向瀏覽不一定是瀏覽多列和欄 List 和 GridView 項目最有效的方式。

**注意** 功能表項目通常是單欄清單，但有時可能適用特殊焦點規則 (請參閱[快顯 UI](#popup-ui))。

可以使用多列和欄建立 List 和 Grid 物件。 這些通常都是以列為主（項目先填滿整列，再填入下一列）或以欄為主（項目先填滿整欄，再填入下一欄）順序。 以列為主或以欄為主順序取決於捲動方向，您應確定項目順序並不會與這個方向衝突。

以列為主順序（項目依序由左至右、由上至下填入），當焦點位於列中的最後一個項目上，按下向右鍵時，焦點移至下一列中的第一個項目。 反向發生此行為：當焦點設定至列中的第一個項目上，按下向左鍵時，焦點移至上一列的最後一個項目。

以欄為主順序（項目依序由上至下、由左至右填入），當焦點位於欄的最後一個項目上，使用者按下向下鍵時，焦點移至下一欄中的第一個項目。 反向發生此行為：當焦點設定至欄中的第一個項目上，按下向上鍵時，焦點移至上一欄的最後一個項目。

<table>
  <tr>
    <td>
      <p><img src="images/keyboard/row-major-keyboard.png" alt="row major keyboard navigation"/></p>
      <p><em>Row major keyboard navigation</em></p>
    </td>
    <td>
      <p><img src="images/keyboard/column-major-keyboard.png" alt="column major keyboard navigation"/></p>
      <p><em>Column major keyboard navigation</em></p>
    </td>
  </tr>
</table>

#### <a name="popup-ui"></a>Popup UI

As mentioned, you should try to ensure directional navigation corresponds to the visual order of the controls in your application's UI.

Some controls (such as the context menu, CommandBar overflow menu, and AutoSuggest menu) display a menu popup in a location and direction (downwards by default) relative to the primary control and available screen space. Note that the opening direction can be affected by a variety of factors at run time.

<table>
  <td><img src="images/keyboard/command-bar-open-down.png" alt="command bar opens down with down arrow key" /></td>
  <td><img src="images/keyboard/command-bar-open-up.png" alt="command bar opens up with down arrow key" /></td>
</table>

For these controls, when the menu is first opened (and no item has been selected by the user), the Down arrow key always sets focus to the first item while the Up arrow key always sets focus to the last item on the menu. 

If the last item has focus and the Down arrow key is pressed, focus moves to the first item on the menu. Similarly, if the first item has focus and the Up arrow key is pressed, focus moves to the last item on the menu. This behavior is referred to as *cycling* and is useful for navigating popup menus that can open in unpredictable directions.

> [!NOTE]
> Cycling should be avoided in non-popup UIs where users might come to feel trapped in an endless loop. 

We recommend that you emulate these same behaviors in your custom controls. Code sample on how to implement this behavior can be found in [Programmatic focus navigation](focus-navigation-programmatic.md#find-the-first-and-last-focusable-element) documentation.

## <a name="test-your-app"></a>測試您的 App

在所有支援的輸入裝置上測試您的 app，以確保以一致且直覺式的方式瀏覽至 UI 元素，且沒有非預期的元素會干擾您想要的 Tab 順序。

## <a name="related-articles"></a>相關文章
* [Keyboard events](keyboard-events.md)
* [識別輸入裝置](identify-input-devices.md)
* [Respond to the presence of the touch keyboard](respond-to-the-presence-of-the-touch-keyboard.md)
* [焦點視覺效果範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlFocusVisuals) \(英文\)

## <a name="appendix"></a>附錄

### <a name="software-keyboard"></a>Software keyboard

軟體鍵盤是顯示在螢幕上的鍵盤，使用者可用來取代實體鍵盤，透過觸控、滑鼠、畫筆/手寫筆或其他指標裝置 (不需要觸控式螢幕) 來輸入資料。 在觸控式螢幕上，也可以直接觸控這些鍵盤來輸入文字。 在 Xbox One 裝置上，需要藉由移動焦點視覺效果或使用遊戲台與遙控器的快速鍵，選取個別的按鍵。

![Windows 10 觸控式鍵盤](images/keyboard/default.png)

***Windows 10 Touch Keyboard***

![Xbox one 螢幕小鍵盤](images/keyboard/xbox-onscreen-keyboard.png)

***Xbox One Onscreen Keyboard***

根據裝置而定，當文字欄位或其他可編輯的文字控制項取得焦點時，或當使用者透過**通知中心**以手動方式啟用時，軟體鍵盤就會出現：

![[通知中心] 的觸控式鍵盤圖示](images/keyboard/touch-keyboard-notificationcenter.png)

如果您的應用程式以程式設計方式將焦點設為文字輸入控制項，則不會叫用觸控式鍵盤。 這可消除不是由使用者直接進行的非預期行為。 不過，當焦點透過程式設計方式移至非文字輸入控制項時，鍵盤會自動隱藏。

當使用者在表單中的控制項之間瀏覽時，觸控式鍵盤通常會保持可見。 這種行為可能會因表單內的其他控制項類型而有所不同。

下面所列的非編輯控制項可以在文字輸入工作階段使用觸控式鍵盤接受焦點，而不用關閉鍵盤。 觸控式鍵盤會持續留在檢視中，不會無意義地變換 UI 讓使用者感到混淆，因為使用者可能會使用觸控式鍵盤，在控制項與文字項目之間做來回切換。

-   核取方塊
-   下拉式方塊
-   選項按鈕
-   捲軸
-   樹狀目錄
-   樹狀目錄項目
-   Menu
-   功能表列
-   功能表項目
-   工具列
-   清單
-   清單項目

以下是觸控式鍵盤各種模式的範例。 第一個影像是預設版面配置，第二個是拇指版面配置 (不是所有語言都有提供)。

![預設配置模式的觸控式鍵盤](images/keyboard/default.png)

***The touch keyboard in default layout mode***

![展開配置模式的觸控式鍵盤](images/keyboard/extendedview.png)

***The touch keyboard in expanded layout mode***

成功的鍵盤互動讓使用者只利用鍵盤就可以完成基本的應用程式操作；也就是說，使用者可以使用所有的互動式元素以及啟動預設的功能。 有許多因素會影響成功的程度，包括鍵盤瀏覽、協助工具的便捷鍵，以及進階使用者的快速鍵。

**NOTE**  The touch keyboard does not support toggle and most system commands.

#### <a name="on-screen-keyboard"></a>螢幕小鍵盤
如同軟體鍵盤，螢幕小鍵盤是視覺化的軟體鍵盤，可用來取代實體鍵盤，透過觸控、滑鼠、畫筆/手寫筆或其他指標裝置 (不需要觸控式螢幕) 來輸入資料。 提供螢幕小鍵盤的用意是針對沒有實體鍵盤的系統，或者使用者因行動不便而無法使用傳統實體輸入裝置的情況。 螢幕小鍵盤會模擬絕大部分的硬體鍵盤功能。

螢幕小鍵盤可以從 [設定] &gt; [輕鬆存取] 中的 [鍵盤] 頁面開啟。

**注意** 螢幕小鍵盤的優先順序高於觸控式鍵盤，如果顯示螢幕小鍵盤，就不會顯示觸控式鍵盤。

![螢幕小鍵盤](images/keyboard/osk.png)

***On-Screen Keyboard***

如需有關螢幕小鍵盤的詳細資訊，請瀏覽[螢幕小鍵盤頁面](https://support.microsoft.com/help/10762/windows-use-on-screen-keyboard)。

## <a name="related-articles"></a>相關文章

- [鍵盤協助工具](../accessibility/keyboard-accessibility.md)
