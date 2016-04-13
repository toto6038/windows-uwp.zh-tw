---
了解如何量身打造顯示或隱藏觸控式鍵盤時 app 的 UI。
回應觸控式鍵盤出現
ms.assetid: 70C6130E-23A2-4F9D-88E7-7060062DA988
回應觸控式鍵盤出現
template: detail.hbs
---

# 回應觸控式鍵盤出現


\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**重要 API**

-   [**AutomationPeer**](https://msdn.microsoft.com/library/windows/apps/br209185)
-   [**InputPane**](https://msdn.microsoft.com/library/windows/apps/br242255)

了解如何量身打造顯示或隱藏觸控式鍵盤時 app 的 UI。

![預設配置模式的觸控式鍵盤](images/touchkeyboard-standard.png)

<sup>預設配置模式的觸控式鍵盤</sup>

觸控式鍵盤可讓支援觸控的裝置輸入文字。 當使用者點選可編輯的輸入欄位時，通用 Windows 平台 (UWP) 文字輸入控制項預設會叫用觸控式鍵盤。 當使用者在表單中的控制項之間瀏覽時，觸控式鍵盤通常會保持可見，但是這種行為可能會因表單內的其他控制項類型而有所不同。

若要在不是衍生自標準文字輸入控制項的自訂文字輸入控制項支援對應的觸控式鍵盤行為，您必須使用 [**AutomationPeer**](https://msdn.microsoft.com/library/windows/apps/br209185) 類別以將您的控制項公開給 Microsoft UI 自動化，並且實作正確的 UI 自動化控制項模式。 請參閱[鍵盤協助工具](https://msdn.microsoft.com/library/windows/apps/mt244347)和[自訂自動化對等](https://msdn.microsoft.com/library/windows/apps/mt297667)。

一旦這項支援新增到您的自訂控制項之後，您可以適當地回應觸控式鍵盤的目前狀態。

**先決條件：**

本文建置在[鍵盤互動](keyboard-interactions.md)基礎上。

您應該有標準鍵盤互動、處理鍵盤輸入和事件及 UI 自動化的基本了解。

如果您是開發通用 Windows 平台 (UWP) App 的新手，請仔細閱讀這些主題以熟悉這裡討論的技術。

-   [建立您的第一個 app](https://msdn.microsoft.com/library/windows/apps/bg124288)
-   請參閱[事件與路由事件概觀](https://msdn.microsoft.com/library/windows/apps/mt185584)，以了解事件相關資訊

**使用者體驗指導方針：**

如需有關設計的有用提示，並且讓 app 針對鍵盤輸入最佳化，請參閱[鍵盤設計指導方針](https://msdn.microsoft.com/library/windows/apps/hh972345)。

## <span id="Touch_keyboard_and_a_custom_UI"> </span> <span id="touch_keyboard_and_a_custom_ui"> </span> <span id="TOUCH_KEYBOARD_AND_A_CUSTOM_UI"> </span>觸控式鍵盤和自訂 UI


以下是自訂文字輸入控制項的一些基本建議。

-   與您的表單互動時全程顯示觸控式鍵盤。

-   請確定自訂的控制項擁有正確的 UI 自動化[**AutomationControlType**](https://msdn.microsoft.com/library/windows/apps/br209182)，這樣在輸入文字時，如果焦點離開文字輸入欄位，鍵盤也會繼續顯示。 例如，如果有功能表在輸入文字期間開啟，而您希望鍵盤繼續顯示，則功能表必須要有功能表的 **AutomationControlType**。

-   不要操縱 UI 自動化屬性以控制觸控式鍵盤。 其他協助工具依賴 UI 自動化屬性的精確度。

-   確保使用者一律可以看到正在互動的輸入欄位。

    因為觸控式鍵盤會遮住大部分的螢幕，所以 UWP 可確保當使用者瀏覽表單的控制項時 (包括目前不在檢視中的控制項)，輸入欄位是在捲動檢視的焦點。

    自訂您的 UI 時，提供觸控式鍵盤外觀的類似行為，方法是處理 [**InputPane**](https://msdn.microsoft.com/library/windows/apps/br242255) 物件公開的 [**Showing**](https://msdn.microsoft.com/library/windows/apps/br242262) 和 [**Hiding**](https://msdn.microsoft.com/library/windows/apps/br242260) 事件。

    ![顯示和未顯示觸控式鍵盤的表單](images/touch-keyboard-pan1.png)

    在某些情況下，有些 UI 元素應該一直停留在畫面上。 設計 UI 以讓移動瀏覽區域中包含表單控制項，並讓重要的 UI 元素處於靜態。 例如：

    ![表單中包含應永久留在檢視中的區域](images/touch-keyboard-pan2.png)

## <span id="handling_events"> </span> <span id="HANDLING_EVENTS"> </span>處理顯示和隱藏事件


以下是對觸控式鍵盤 [**showing**](https://msdn.microsoft.com/library/windows/apps/br242262) 和 [**hiding**](https://msdn.microsoft.com/library/windows/apps/br242260) 事件附加事件處理常式的範例。

```CSharp
public class MyApplication
{
    public MyApplication()
    {
        // Grab the input pane for the main application window and attach
        // touch keyboard event handlers.
        Windows.Foundation.Application.InputPane.GetForCurrentView().Showing  
            += new EventHandler(_OnInputPaneShowing);
        Windows.Foundation.Application.InputPane.GetForCurrentView().Hiding 
            += new EventHandler(_OnInputPaneHiding);
    }

    private void _OnInputPaneShowing(object sender, IInputPaneVisibilityEventArgs eventArgs)
    {
        // If the size of this window is going to be too small, the app uses 
        // the Showing event to begin some element removal animations.
        if (eventArgs.OccludedRect.Top < 400)
        {
            _StartElementRemovalAnimations();

            // Don&#39;t use framework scroll- or visibility-related 
            // animations that might conflict with the app&#39;s logic.
            eventArgs.EnsuredFocusedElementInView = true; 
        }
    }

    private void _OnInputPaneHiding(object sender, IInputPaneVisibilityEventArgs eventArgs)
    {
        if (_ResetToDefaultElements())
        {
            eventArgs.EnsuredFocusedElementInView = true; 
        }
    }

    private void _StartElementRemovalAnimations()
    {
        // This function starts the process of removing elements 
        // and starting the animation.
    }

    private void _ResetToDefaultElements()
    {
        // This function resets the window&#39;s elements to their default state.
    }
}
```

## <span id="related_topics"> </span>相關文章



* [鍵盤互動](keyboard-interactions.md)
* [鍵盤協助工具](https://msdn.microsoft.com/library/windows/apps/mt244347)
* [自訂自動化對等](https://msdn.microsoft.com/library/windows/apps/mt297667)


**封存範例**
* [輸入：觸控式鍵盤範例](http://go.microsoft.com/fwlink/p/?linkid=246019)
* [回應螢幕小鍵盤外觀的範例](http://go.microsoft.com/fwlink/p/?linkid=231633)
* [XAML 文字編輯範例](http://go.microsoft.com/fwlink/p/?LinkID=251417)
* [XAML 協助工具範例](http://go.microsoft.com/fwlink/p/?linkid=238570)
 

 






<!--HONumber=Mar16_HO1-->


