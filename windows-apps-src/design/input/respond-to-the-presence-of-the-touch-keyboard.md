---
description: 了解如何量身打造顯示或隱藏觸控式鍵盤時 app 的 UI。
title: 回應觸控式鍵盤的出現
ms.assetid: 70C6130E-23A2-4F9D-88E7-7060062DA988
label: Respond to the presence of the touch keyboard
template: detail.hbs
keywords: 鍵盤、協助工具、瀏覽、焦點、文字、輸入、使用者互動
ms.date: 09/24/2020
ms.topic: article
ms.openlocfilehash: c3431fcafb86428ce5eddb8ea7a6b0187b64a5e5
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2020
ms.locfileid: "93035081"
---
# <a name="respond-to-the-presence-of-the-touch-keyboard"></a>回應觸控式鍵盤的出現

了解如何量身打造顯示或隱藏觸控式鍵盤時 app 的 UI。

### <a name="important-apis"></a>重要 API

- [AutomationPeer](/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationPeer)
- [InputPane](/uwp/api/Windows.UI.ViewManagement.InputPane)

![預設配置模式的觸控式鍵盤](images/keyboard/default.png)

<sup>預設版面配置模式中的觸控鍵盤</sup>

觸控式鍵盤可讓支援觸控的裝置輸入文字。 當使用者按下可編輯的輸入欄位時，Windows 應用程式文字輸入控制項預設會叫用觸控鍵盤。 當使用者在表單中的控制項之間瀏覽時，觸控式鍵盤通常會保持可見，但是這種行為可能會因表單內的其他控制項類型而有所不同。

若要在不是衍生自標準文字輸入控制項的自訂文字輸入控制項支援對應的觸控式鍵盤行為，您必須使用 <a href="/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationPeer">AutomationPeer</a> 類別以將您的控制項公開給 Microsoft UI 自動化，並且實作正確的 UI 自動化控制項模式。 請參閱[鍵盤協助工具](../accessibility/keyboard-accessibility.md)和[自訂自動化對等](../accessibility/custom-automation-peers.md)。

一旦這項支援新增到您的自訂控制項之後，您可以適當地回應觸控式鍵盤的目前狀態。

**必要條件：**

本文建置在[鍵盤互動](keyboard-interactions.md)基礎上。

您應該有標準鍵盤互動、處理鍵盤輸入和事件及 UI 自動化的基本了解。

如果您是開發 Windows 應用程式的新手，請參閱這些主題，以瞭解此處所討論的技術。

- [建立您的第一個應用程式](../../get-started/your-first-app.md)
- 請參閱[事件與路由事件概觀](../../xaml-platform/events-and-routed-events-overview.md)，以了解事件相關資訊

**使用者經驗指導方針：**

如需有關設計適合鍵盤輸入的實用且吸引人的應用程式的實用秘訣，請參閱 [鍵盤互動](./keyboard-interactions.md) 。

## <a name="touch-keyboard-and-a-custom-ui"></a>觸控式鍵盤和自訂 UI

以下是自訂文字輸入控制項的一些基本建議。

- 與您的表單互動時全程顯示觸控式鍵盤。

- 請確定自訂的控制項擁有正確的 UI 自動化[AutomationControlType](/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationControlType)，這樣在輸入文字時，如果焦點離開文字輸入欄位，鍵盤也會繼續顯示。 例如，如果有功能表在輸入文字期間開啟，而您希望鍵盤繼續顯示，則功能表必須要有功能表的 **AutomationControlType** 。

- 不要操縱 UI 自動化屬性以控制觸控式鍵盤。 其他協助工具依賴 UI 自動化屬性的精確度。

- 確保使用者一律可以看到正在互動的輸入欄位。

    由於觸控鍵盤會 occludes 大部分的螢幕，因此 Windows 會確定具有焦點的輸入欄位會在使用者流覽表單上的控制項時滾動，包括目前不在視野中的控制項。

    自訂您的 UI 時，提供觸控式鍵盤外觀的類似行為，方法是處理 [InputPane](/uwp/api/windows.ui.viewmanagement.inputpane.showing) 物件公開的 [Showing](/uwp/api/windows.ui.viewmanagement.inputpane.hiding) 和 [**Hiding**](/uwp/api/Windows.UI.ViewManagement.InputPane) 事件。

    ![顯示和未顯示觸控式鍵盤的表單](images/touch-keyboard-pan1.png)

    在某些情況下，有些 UI 元素應該一直停留在畫面上。 設計 UI 以讓移動瀏覽區域中包含表單控制項，並讓重要的 UI 元素處於靜態。 例如：

    ![表單中包含應永久留在檢視中的區域](images/touch-keyboard-pan2.png)

## <a name="handling-the-showing-and-hiding-events"></a>處理顯示和隱藏事件

以下是附加觸控[鍵盤事件之事件處理](/uwp/api/windows.ui.viewmanagement.inputpane.hiding)程式[的範例](/uwp/api/windows.ui.viewmanagement.inputpane.showing)。

```csharp
using Windows.UI.ViewManagement;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Media;
using Windows.Foundation;
using Windows.UI.Xaml.Navigation;

namespace SDKTemplate
{
    /// <summary>
    /// Sample page to subscribe show/hide event of Touch Keyboard.
    /// </summary>
    public sealed partial class Scenario2_ShowHideEvents : Page
    {
        public Scenario2_ShowHideEvents()
        {
            this.InitializeComponent();
        }

        protected override void OnNavigatedTo(NavigationEventArgs e)
        {
            InputPane currentInputPane = InputPane.GetForCurrentView();
            // Subscribe to Showing/Hiding events
            currentInputPane.Showing += OnShowing;
            currentInputPane.Hiding += OnHiding;
        }

        protected override void OnNavigatedFrom(NavigationEventArgs e)
        {
            InputPane currentInputPane = InputPane.GetForCurrentView();
            // Unsubscribe from Showing/Hiding events
            currentInputPane.Showing -= OnShowing;
            currentInputPane.Hiding -= OnHiding;
        }

        void OnShowing(InputPane sender, InputPaneVisibilityEventArgs e)
        {
            LastInputPaneEventRun.Text = "Showing";
        }
        
        void OnHiding(InputPane sender, InputPaneVisibilityEventArgs e)
        {
            LastInputPaneEventRun.Text = "Hiding";
        }
    }
}
```

```cppwinrt
...
#include <winrt/Windows.UI.ViewManagement.h>
...
private:
    winrt::event_token m_showingEventToken;
    winrt::event_token m_hidingEventToken;
...
Scenario2_ShowHideEvents::Scenario2_ShowHideEvents()
{
    InitializeComponent();
}

void Scenario2_ShowHideEvents::OnNavigatedTo(Windows::UI::Xaml::Navigation::NavigationEventArgs const& e)
{
    auto inputPane{ Windows::UI::ViewManagement::InputPane::GetForCurrentView() };
    // Subscribe to Showing/Hiding events
    m_showingEventToken = inputPane.Showing({ this, &Scenario2_ShowHideEvents::OnShowing });
    m_hidingEventToken = inputPane.Hiding({ this, &Scenario2_ShowHideEvents::OnHiding });
}

void Scenario2_ShowHideEvents::OnNavigatedFrom(Windows::UI::Xaml::Navigation::NavigationEventArgs const& e)
{
    auto inputPane{ Windows::UI::ViewManagement::InputPane::GetForCurrentView() };
    // Unsubscribe from Showing/Hiding events
    inputPane.Showing(m_showingEventToken);
    inputPane.Hiding(m_hidingEventToken);
}

void Scenario2_ShowHideEvents::OnShowing(Windows::UI::ViewManagement::InputPane const& /*sender*/, Windows::UI::ViewManagement::InputPaneVisibilityEventArgs const& /*args*/)
{
    LastInputPaneEventRun().Text(L"Showing");
}

void Scenario2_ShowHideEvents::OnHiding(Windows::UI::ViewManagement::InputPane const& /*sender*/, Windows::UI::ViewManagement::InputPaneVisibilityEventArgs const& /*args*/)
{
    LastInputPaneEventRun().Text(L"Hiding");
}
```

```cpp
#include "pch.h"
#include "Scenario2_ShowHideEvents.xaml.h"

using namespace SDKTemplate;
using namespace Platform;
using namespace Windows::Foundation;
using namespace Windows::UI::ViewManagement;
using namespace Windows::UI::Xaml;
using namespace Windows::UI::Xaml::Controls;
using namespace Windows::UI::Xaml::Navigation;

Scenario2_ShowHideEvents::Scenario2_ShowHideEvents()
{
    InitializeComponent();
}

void Scenario2_ShowHideEvents::OnNavigatedTo(NavigationEventArgs^ e)
{
    auto inputPane = InputPane::GetForCurrentView();
    // Subscribe to Showing/Hiding events
    showingEventToken = inputPane->Showing +=
        ref new TypedEventHandler<InputPane^, InputPaneVisibilityEventArgs^>(this, &Scenario2_ShowHideEvents::OnShowing);
    hidingEventToken = inputPane->Hiding +=
        ref new TypedEventHandler<InputPane^, InputPaneVisibilityEventArgs^>(this, &Scenario2_ShowHideEvents::OnHiding);
}

void Scenario2_ShowHideEvents::OnNavigatedFrom(NavigationEventArgs^ e)
{
    auto inputPane = Windows::UI::ViewManagement::InputPane::GetForCurrentView();
    // Unsubscribe from Showing/Hiding events
    inputPane->Showing -= showingEventToken;
    inputPane->Hiding -= hidingEventToken;
}

void Scenario2_ShowHideEvents::OnShowing(InputPane^ /*sender*/, InputPaneVisibilityEventArgs^ /*args*/)
{
    LastInputPaneEventRun->Text = L"Showing";
}

void Scenario2_ShowHideEvents::OnHiding(InputPane^ /*sender*/, InputPaneVisibilityEventArgs ^ /*args*/)
{
    LastInputPaneEventRun->Text = L"Hiding";
}
```

## <a name="related-articles"></a>相關文章

- [鍵盤互動](keyboard-interactions.md)
- [鍵盤協助工具](../accessibility/keyboard-accessibility.md)
- [自訂自動化對等](../accessibility/custom-automation-peers.md)

### <a name="samples"></a>範例

- [觸控式鍵盤範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/TouchKeyboard)

### <a name="archive-samples"></a>封存範例

- [輸入：觸控式鍵盤範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/TouchKeyboard)
- [回應螢幕小鍵盤外觀的範例](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Responding%20to%20the%20appearance%20of%20the%20on-screen%20keyboard%20sample)
- [XAML 文字編輯範例](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BVB%5D-Windows%208%20app%20samples/VB/Windows%208%20app%20samples/XAML%20text%20editing%20sample%20(Windows%208))
- [XAML 協助工具範例](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/XAML%20accessibility%20sample)
