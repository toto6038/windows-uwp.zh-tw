---
description: 讓使用者在個別的視窗中檢視您應用程式的獨立部分，以協助他們提高生產力。
title: 顯示 app 的多重檢視
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 2b25ceaac900868479c9145b9dad5977ca213f03
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/24/2020
ms.locfileid: "91218481"
---
# <a name="show-multiple-views-for-an-app"></a>顯示 app 的多重檢視

![線框顯示了具有多個視窗的 app](images/multi-view.gif)

讓使用者在個別的視窗中檢視您 app 的獨立部分，以協助他們提高生產力。 如果您為 app 建立多個視窗，工作列會個別顯示每個視窗。 使用者可以單獨移動、調整大小、顯示及隱藏 app 視窗，也可以在 app 視窗之間切換，就像是不同的 app 一樣。

> **重要 API**：[Windows.UI.ViewManagement namespace](/uwp/api/windows.ui.viewmanagement)、[Windows.UI.WindowManagement namespace](/uwp/api/windows.ui.windowmanagement)

## <a name="when-should-an-app-use-multiple-views"></a>app 何時應該使用多重檢視？

有各種受惠於多重檢視的不同案例。 以下是一些範例：

- 電子郵件 app 可讓使用者在撰寫新電子郵件的同時，也能查看收到的郵件清單
- 通訊錄 app 可讓使用者並排比較多人的連絡資訊
- 音樂播放器 app 可讓使用者在瀏覽其他可收聽的音樂清單時，也看見正在播放的音樂
- 筆記 app 可讓使用者將資訊從筆記中的一頁複製到另一頁
- 閱讀 app 可讓使用者先大致瀏覽各則大標，開啟多篇文章待稍後閱讀

每個 app 都有獨一無二的配置，不過建議在常見位置放置「新增視窗」按鈕，例如可於新視窗中開啟的內容右上角。 也請考慮納入 [操作功能表](../controls-and-patterns/menus.md) 選項，以「在新視窗中 開啟」。

若要建立應用程式的個別執行個體 (而不是相同執行個體的個別視窗)，請參閱[建立多重執行個體 Windows 應用程式](../../launch-resume/multi-instance-uwp.md)。

## <a name="windowing-hosts"></a>視窗化主機

Windows 內容可透過不同方式裝載在應用程式內。

- [CoreWindow](/uwp/api/windows.ui.core.corewindow)/[ApplicationView](/uwp/api/windows.ui.viewmanagement.applicationview)

     應用程式檢視是執行緒與應用程式用來顯示內容之視窗的 1:1 配對。 您 app 啟動時建立的第一個檢視稱為「主要檢視」  。 每個 CoreWindow/ApplicationView 都會在本身的執行緒中運作。 如果必須在不同的 UI 執行緒上運作，可能會提高多重視窗應用程式的複雜度。

    應用程式的主要檢視一律裝載在 ApplicationView 中。 次要視窗中的內容可裝載在 ApplicationView 或 AppWindow 中。

    若要了解如何使用 ApplicationView 顯示 app 的次要視窗，請參閱[使用 ApplicationView](application-view.md)。
- [AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow)

    AppWindow 會在自身建立來源的同個 UI 執行緒上運作，因此可簡化多重視窗 Windows 應用程式的建立作業。

    從 Windows 10 版本 1903 (SDK 18362) 起，可使用 [WindowManagement](/uwp/api/windows.ui.windowmanagement) 命名空間中的 AppWindow 類別和其他 API。 如果您的 app 以舊版 Windows 10 為目標，請務必使用 ApplicationView 來建立次要視窗。

    若要了解如何使用 AppWindow 顯示 app 的次要視窗，請參閱[使用 AppWindow](app-window.md)。

    > [!NOTE]
    > AppWindow 目前為預覽狀態。 也就是您可以將使用 AppWindow 的應用程式提交至 Microsoft Store，但是已知某些平台和架構元件無法與 AppWindow 搭配使用 (請參閱[限制](/uwp/api/windows.ui.windowmanagement.appwindow#limitations))。
- [DesktopWindowXamlSource](/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) (XAML Islands)

     Win32 app 中的 UWP XAML (使用 HWND)，也稱為 XAML Islands，是裝載於 DesktopWindowXamlSource 中。

    如需 XAML Islands 的詳細資訊，請參閱[在傳統型應用程式中使用 UWP XAML 裝載 API](/windows/apps/desktop/modernize/using-the-xaml-hosting-api)

### <a name="make-code-portable-across-windowing-hosts"></a>讓程式碼可在視窗化主機之間移轉

當 XAML 內容顯示在 [CoreWindow](/uwp/api/windows.ui.core.corewindow) 中時，一律會有相關的 [ApplicationView](/uwp/api/windows.ui.viewmanagement.applicationview) 和 XAML [Window](/uwp/api/windows.ui.xaml.window)。 您可以使用這些類別的 API，以取得視窗界限等資訊。 若要擷取這些類別的執行個體，可使用靜態的 [CoreWindow.GetForCurrentThread](/uwp/api/windows.ui.core.corewindow.getforcurrentthread) 方法、[ApplicationView.GetForCurrentView](/uwp/api/windows.ui.viewmanagement.applicationview.getforcurrentview) 方法或 [Window.Current](/uwp/api/windows.ui.xaml.window.current) 屬性。 此外，有許多類別使用 `GetForCurrentView` 模式來擷取該類別的執行個體，例如 [DisplayInformation.GetForCurrentView](/uwp/api/windows.graphics.display.displayinformation.getforcurrentview)。

這些 API 能夠發揮作用，是因為 CoreWindow/ApplicationView 的 XAML 內容僅具有單一樹狀結構，所以 XAML 知道其裝載的內容是 CoreWindow/ApplicationView。

當 XAML 內容在 AppWindow 或 DesktopWindowXamlSource 中執行時，您可以在相同執行緒上同時執行 XAML 內容的多個樹狀結構。 在此情況下，這些 API 不會提供正確資訊，因為該內容不再於目前的 CoreWindow/ApplicationView (和 XAML Window) 中執行。

為了確保您的程式碼可在所有視窗化主機之間正常運作，建議使用從 [XamlRoot](/uwp/api/windows.ui.xaml.xamlroot) 類別取得其內容的新 API 來取代依賴 [CoreWindow](/uwp/api/windows.ui.core.corewindow)、[ApplicationView](/uwp/api/windows.ui.viewmanagement.applicationview) 和 [Window](/uwp/api/windows.ui.xaml.window) 的 API。
無論是 CoreWindow、AppWindow 或 DesktopWindowXamlSource，XamlRoot 類別會呈現 XAML 內容的樹狀結構，以及裝載所在內容的相關資訊。 無論 XAML 在哪個視窗化主機中執行，皆可透過此抽象層編寫相同的程式碼。

下表顯示在視窗化主機之間無法正常運作的程式碼，以及可用來更換的新可攜式程式碼，還有一些不需變更的 API。

| 目前使用： | 取代選項： |
| - | - |
| CoreWindow.GetForCurrentThread().[Bounds](/uwp/api/windows.ui.core.corewindow.bounds) | _uiElement_.XamlRoot.[Size](/uwp/api/windows.ui.xaml.xamlroot.size) |
| CoreWindow.GetForCurrentThread().[SizeChanged](/uwp/api/windows.ui.core.corewindow.sizechanged) | _uiElement_.XamlRoot.[Changed](/uwp/api/windows.ui.xaml.xamlroot.changed) |
| CoreWindow.[Visible](/uwp/api/windows.ui.core.corewindow.visible) | _uiElement_.XamlRoot.[IsHostVisible](/uwp/api/windows.ui.xaml.xamlroot.ishostvisible) |
| CoreWindow.[VisibilityChanged](/uwp/api/windows.ui.core.corewindow.visibilitychanged) | _uiElement_.XamlRoot.[Changed](/uwp/api/windows.ui.xaml.xamlroot.changed) |
| CoreWindow.GetForCurrentThread().[GetKeyState](/uwp/api/windows.ui.core.corewindow.getkeystate) | 未變更。 AppWindow 和 DesktopWindowXamlSource 支援此項目。 |
| CoreWindow.GetForCurrentThread().[GetAsyncKeyState](/uwp/api/windows.ui.core.corewindow.getasynckeystate) | 未變更。 AppWindow 和 DesktopWindowXamlSource 支援此項目。 |
| Window.[Current](/uwp/api/windows.ui.xaml.window.current) | 傳回與目前 CoreWindow 緊密繫結的主要 XAML Window 物件。 請參閱此表格後方的備註。 |
| Window.Current.[Bounds](/uwp/api/windows.ui.xaml.window.bounds) | _uiElement_.XamlRoot.[Size](/uwp/api/windows.ui.xaml.xamlroot.size) |
| Window.Current.[Content](/uwp/api/windows.ui.xaml.window.content) | UIElement root =  _uiElement_.XamlRoot.[Content](/uwp/api/windows.ui.xaml.xamlroot.content) |
| Window.Current.[Compositor](/uwp/api/windows.ui.xaml.window.compositor) | 未變更。 AppWindow 和 DesktopWindowXamlSource 支援此項目。 |
| VisualTreeHelper.[FindElementsInHostCoordinates](/uwp/api/windows.ui.xaml.media.visualtreehelper.findelementsinhostcoordinates)<br>雖然 UIElement 參數是選擇性的，但如果在 Island 上裝載時未提供 UIElement，此方法就會引發例外狀況。 | 請指定 _uiElement_.XamlRoot 作為 UIElement，而不是讓其保持空白。 |
| VisualTreeHelper.[GetOpenPopups](/uwp/api/windows.ui.xaml.media.visualtreehelper.getopenpopups)<br/>在 XAML Islands app 中，這會擲回錯誤。 在 AppWindow app 中，這將會在主視窗中傳回開啟的快顯視窗。 | VisualTreeHelper.[GetOpenPopupsForXamlRoot](/uwp/api/windows.ui.xaml.media.visualtreehelper.getopenpopupsforxamlroot)(_uiElement_.XamlRoot) |
| FocusManager.[GetFocusedElement](/uwp/api/windows.ui.xaml.input.focusmanager.getfocusedelement) | FocusManager.[GetFocusedElement](/uwp/api/windows.ui.xaml.input.focusmanager.getfocusedelement#Windows_UI_Xaml_Input_FocusManager_GetFocusedElement_Windows_UI_Xaml_XamlRoot_)(_uiElement_.XamlRoot) |
| contentDialog.ShowAsync() | contentDialog.[XamlRoot](/uwp/api/windows.ui.xaml.uielement.xamlroot) = _uiElement_.XamlRoot;<br/>contentDialog.ShowAsync(); |
| menuFlyout.ShowAt(null, new Point(10, 10)); | menuFlyout.[XamlRoot](/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase.xamlroot) = _uiElement_.XamlRoot;<br/>menuFlyout.ShowAt(null, new Point(10, 10)); |

> [!NOTE]
> 若是 DesktopWindowXamlSource 中的 XAML 內容，執行緒上會有一個 CoreWindow/Window，但一律為隱藏狀態，且大小為 1x1。 app 雖然仍可存取它，但不會傳回有意義的界限或可見度。
>
>若是 AppWindow 中的 XAML 內容，在相同執行緒上一律只有一個 CoreWindow。 如果呼叫 `GetForCurrentView` 或 `GetForCurrentThread` API，該 API 會傳回一個物件，反映執行緒上的 CoreWindow 狀態，而不是可能在該執行緒上執行的任何 AppWindows。


## <a name="dos-and-donts"></a>可行與禁止事項

- 利用「開啟新視窗」字符，提供清楚的次要檢視進入點。
- 與使用者溝通次要檢視的用途。
- 確保您的 app 在單一檢視中可正常運作，而使用者僅為方便才開啟次要檢視。
- 不要仰賴次要檢視提供通知或其他暫時性視覺效果。

## <a name="related-topics"></a>相關主題

- [使用 AppWindow](app-window.md)
- [使用 ApplicationView](application-view.md)
- [ApplicationViewSwitcher](/uwp/api/Windows.UI.ViewManagement.ApplicationViewSwitcher)
- [CreateNewView](/uwp/api/windows.applicationmodel.core.coreapplication.createnewview)
