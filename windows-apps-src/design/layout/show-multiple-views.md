---
Description: 在不同的視窗中, 查看應用程式的不同部分。
title: 顯示 app 的多重檢視
ms.date: 05/19/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: ee49b5fe5b5956e9069ea196c4d2e029b3a15763
ms.sourcegitcommit: 3cc6eb3bab78f7e68c37226c40410ebca73f82a9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/02/2019
ms.locfileid: "68729513"
---
# <a name="show-multiple-views-for-an-app"></a>顯示 app 的多重檢視

![線框顯示含有多個視窗的 App](images/multi-view.gif)

讓使用者在個別的視窗中檢視您 App 的獨立部分，以協助他們提高生產力。 當您為應用程式建立多個視窗時, 工作列會分別顯示每個視窗。 使用者可以單獨移動、調整大小、顯示及隱藏 app 視窗，也可以在 app 視窗之間切換，就像是不同的 app 一樣。

> **重要 API**：[Windows.ui.viewmanagement 命名空間](/uwp/api/windows.ui.viewmanagement)、 [windows. ui. WindowManagement 命名空間](/uwp/api/windows.ui.windowmanagement)

## <a name="when-should-an-app-use-multiple-views"></a>App 何時應該使用多個檢視？

有各種不同的案例可受益于多個視圖。 以下是一些範例：

- 電子郵件 App 可讓使用者在撰寫新的電子郵件時檢視所接收郵件的清單
- 通訊錄 App 可讓使用者並排比較多人的連絡資訊
- 音樂播放程式 App 可讓使用者在瀏覽其他可用音樂的清單時看到正在播放的音樂
- 筆記紀錄 App 可讓使用者將資訊從一頁筆記複製到另一頁
- 閱讀 App 可讓使用者在有機會細讀所有高階標題之後，開啟幾個文章供稍後閱讀

每個 App 配置是獨一無二的，我們建議在可預測的位置包括「新視窗」按鈕，例如可在新視窗中開啟的內容的右上角。 也請考慮包含 [在新視窗中開啟] 的[快顯功能表](../controls-and-patterns/menus.md)選項。

若要針對相同的實例建立個別的應用程式實例 (而不是不同的視窗), 請參閱[建立多重實例 UWP 應用程式](../../launch-resume/multi-instance-uwp.md)。

## <a name="windowing-hosts"></a>視窗化主機

UWP 內容可以裝載于應用程式內的方式有很多種。

- [CoreWindow](/uwp/api/windows.ui.core.corewindow)/ [ApplicationView](/uwp/api/windows.ui.viewmanagement.applicationview)

     應用程式檢視是執行緒與應用程式用來顯示內容之視窗的 1:1 配對。 您 app 啟動時建立的第一個檢視稱為「主要檢視」。 每個 CoreWindow/ApplicationView 都會在自己的執行緒中運作。 必須在不同的 UI 執行緒上工作, 才能使多視窗應用程式變得複雜。

    應用程式的主要視圖一律裝載于 ApplicationView 中。 次要視窗中的內容可以裝載于 ApplicationView 或 AppWindow 中。

    若要瞭解如何使用 ApplicationView 在您的應用程式中顯示次要視窗, 請參閱[使用 ApplicationView](application-view.md)。
- [AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow)

    AppWindow 可簡化多視窗 UWP 應用程式的建立作業, 因為它會在其建立來源的相同 UI 執行緒上運作。

    從 Windows 10 版本 1903 (SDK 18362) 開始, 可以使用[WindowManagement](/uwp/api/windows.ui.windowmanagement)命名空間中的 AppWindow 類別和其他 api。 如果您的應用程式是以舊版的 Windows 10 為目標, 則必須使用 ApplicationView 來建立次要視窗。

    若要瞭解如何使用 AppWindow 在您的應用程式中顯示次要視窗, 請參閱[使用 AppWindow](app-window.md)。

    > [!NOTE]
    > AppWindow 目前為預覽狀態。 這表示您可以將使用 AppWindow 的應用程式提交至存放區, 但某些平臺和架構元件已知無法與 AppWindow 搭配使用 (請參閱[限制](/uwp/api/windows.ui.windowmanagement.appwindow#limitations))。
- [DesktopWindowXamlSource](/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource)(XAML 島)

     Win32 應用程式 (使用 HWND) 中的 UWP XAML 內容 (也稱為 XAML 島) 會裝載于 DesktopWindowXamlSource 中。

    如需有關 XAML Islands 的詳細資訊, 請參閱[在桌面應用程式中使用 UWP XAML 裝載 API](/windows/apps/desktop/modernize/using-the-xaml-hosting-api)

### <a name="make-code-portable-across-windowing-hosts"></a>使程式碼可在視窗化主機間攜

當 XAML 內容顯示在[CoreWindow](/uwp/api/windows.ui.core.corewindow)中時, 一律會有相關聯的[ApplicationView](/uwp/api/windows.ui.viewmanagement.applicationview)和 XAML[視窗](/uwp/api/windows.ui.xaml.window)。 您可以使用這些類別上的 Api 來取得資訊, 例如視窗界限。 若要抓取這些類別的實例, 您可以使用靜態[CoreWindow. GetForCurrentThread](/uwp/api/windows.ui.core.corewindow.getforcurrentthread)方法、 [ApplicationView. GetForCurrentView](/uwp/api/windows.ui.viewmanagement.applicationview.getforcurrentview)方法或[Window. Current](/uwp/api/windows.ui.xaml.window.current)屬性。 此外, 還有許多類別使用`GetForCurrentView`模式來抓取類別的實例, 例如[DisplayInformation. GetForCurrentView](/uwp/api/windows.graphics.display.displayinformation.getforcurrentview)。

這些 Api 的作用是因為 CoreWindow/ApplicationView 只有一個 XAML 內容的樹狀結構, 因此 XAML 知道其裝載所在的環境是 CoreWindow/ApplicationView。

當 XAML 內容在 AppWindow 或 DesktopWindowXamlSource 內執行時, 您可以在同一個執行緒上同時執行多個 XAML 內容樹狀結構。 在此情況下, 這些 Api 不會提供正確的資訊, 因為內容不再于目前的 CoreWindow/ApplicationView (和 XAML 視窗) 內執行。

為了確保您的程式碼可在所有視窗化主機上正常運作, 您應該將依賴[CoreWindow](/uwp/api/windows.ui.core.corewindow)、 [ApplicationView](/uwp/api/windows.ui.viewmanagement.applicationview)和[Window](/uwp/api/windows.ui.xaml.window)的 api 取代為新的 api, 以從[XamlRoot](/uwp/api/windows.ui.xaml.xamlroot)類別取得其內容。
XamlRoot 類別代表 XAML 內容的樹狀結構, 以及其裝載所在內容的相關資訊, 不論是 CoreWindow、AppWindow 或 DesktopWindowXamlSource。 此抽象層可讓您撰寫相同的程式碼, 而不論 XAML 執行所在的視窗化主機為何。

下表顯示在視窗化主機上無法正確運作的程式碼, 以及您可以用來取代的新可攜程式碼, 以及一些不需要變更的 Api。

| 如果您使用 。 | 取代為 。 |
| - | - |
| CoreWindow. GetForCurrentThread ()。[界限](/uwp/api/windows.ui.core.corewindow.bounds) | _uiElement_。XamlRoot.[大小](/uwp/api/windows.ui.xaml.xamlroot.size) |
| CoreWindow. GetForCurrentThread ()。[SizeChanged](/uwp/api/windows.ui.core.corewindow.sizechanged) | _uiElement_。XamlRoot.[已變更](/uwp/api/windows.ui.xaml.xamlroot.changed) |
| CoreWindow.[可見](/uwp/api/windows.ui.core.corewindow.visible) | _uiElement_。XamlRoot.[IsHostVisible](/uwp/api/windows.ui.xaml.xamlroot.ishostvisible) |
| CoreWindow.[VisibilityChanged](/uwp/api/windows.ui.core.corewindow.visibilitychanged) | _uiElement_。XamlRoot.[已變更](/uwp/api/windows.ui.xaml.xamlroot.changed) |
| CoreWindow. GetForCurrentThread ()。[GetKeyState](/uwp/api/windows.ui.core.corewindow.getkeystate) | 任何. AppWindow 和 DesktopWindowXamlSource 都支援此功能。 |
| CoreWindow. GetForCurrentThread ()。[GetAsyncKeyState](/uwp/api/windows.ui.core.corewindow.getasynckeystate) | 任何. AppWindow 和 DesktopWindowXamlSource 都支援此功能。 |
| 範圍.[目前](/uwp/api/windows.ui.xaml.window.current) | 傳回與目前 CoreWindow 緊密系結的主要 XAML 視窗物件。 請參閱此表格之後的注意事項。 |
| 視窗. 目前。[界限](/uwp/api/windows.ui.xaml.window.bounds) | _uiElement_。XamlRoot.[大小](/uwp/api/windows.ui.xaml.xamlroot.size) |
| 視窗. 目前。[內容](/uwp/api/windows.ui.xaml.window.content) | UIElement root = _uielement_。XamlRoot.[內容](/uwp/api/windows.ui.xaml.xamlroot.content) |
| 視窗. 目前。[複合](/uwp/api/windows.ui.xaml.window.compositor)器 | 任何. AppWindow 和 DesktopWindowXamlSource 都支援此功能。 |
| VisualTreeHelper.[GetOpenPopups](/uwp/api/windows.ui.xaml.media.visualtreehelper.getopenpopups)<br/>在 XAML 孤島應用程式中, 這將會擲回錯誤。 在 AppWindow apps 中, 這會在主視窗上傳回開啟的快顯視窗。 | VisualTreeHelper.[GetOpenPopupsForXamlRoot](/uwp/api/windows.ui.xaml.media.visualtreehelper.getopenpopupsforxamlroot)(_uiElement_。XamlRoot) |
| System.windows.input.focusmanager>.[System.windows.input.focusmanager.getfocusedelement](/uwp/api/windows.ui.xaml.input.focusmanager.getfocusedelement) | System.windows.input.focusmanager>.[System.windows.input.focusmanager.getfocusedelement](/uwp/api/windows.ui.xaml.input.focusmanager.getfocusedelement#Windows_UI_Xaml_Input_FocusManager_GetFocusedElement_Windows_UI_Xaml_XamlRoot_)(_uiElement_。XamlRoot) |
| contentDialog.ShowAsync() | contentDialog.[XamlRoot](/uwp/api/windows.ui.xaml.uielement.xamlroot) =  _uiElement_。XamlRoot;<br/>contentDialog.ShowAsync(); |
| menuFlyout. ShowAt (null, 新點 (10, 10)); | menuFlyout.[XamlRoot](/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase.xamlroot) =  _uiElement_。XamlRoot;<br/>menuFlyout. ShowAt (null, 新點 (10, 10)); |

> [!NOTE]
> 若為 DesktopWindowXamlSource 中的 XAML 內容, 執行緒上會有一個 CoreWindow/視窗, 但它一律不可見, 且大小為1x1。 應用程式仍可存取它, 但不會傳回有意義的界限或可見度。
>
>針對 AppWindow 中的 XAML 內容, 相同的執行緒上一律只會有一個 CoreWindow。 如果您呼叫`GetForCurrentView`或`GetForCurrentThread` api, 該 api 會傳回物件, 反映執行緒上的 CoreWindow 狀態, 而不是任何可能在該執行緒上執行的 AppWindows。


## <a name="dos-and-donts"></a>可行與禁止事項

- 利用「開啟新視窗」字符，提供清楚的次要檢視進入點。
- 與使用者溝通次要檢視的用途。
- 請確定您的應用程式在單一視圖中可完整運作, 而且使用者只會開啟次要視圖以方便使用。
- 不要仰賴次要檢視，提供通知或其他暫時性視覺效果。

## <a name="related-topics"></a>相關主題

- [使用 AppWindow](app-window.md)
- [使用 ApplicationView](application-view.md)
- [ApplicationViewSwitcher](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.ApplicationViewSwitcher)
- [CreateNewView](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplication.createnewview)
