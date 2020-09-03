---
description: 本文說明如何在傳統型 C++ Win32 應用程式中裝載 UWP XAML UI。
title: 在 C++ Win32 應用程式中使用 UWP XAML 裝載 API
ms.date: 03/23/2020
ms.topic: article
keywords: windows 10, uwp, windows forms, wpf, win32, xaml islands
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 8427519dd010553eb1f4f00f951dcc747a94b0c0
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174182"
---
# <a name="using-the-uwp-xaml-hosting-api-in-a-c-win32-app"></a>在 C++ Win32 應用程式中使用 UWP XAML 裝載 API

從 Windows 10 (版本 1903) 開始，非 UWP 傳統型應用程式 (包括 C++ Win32、WPF 和 Windows Forms 應用程式) 可使用 *UWP XAML 裝載 API*，在與視窗控制代碼 (HWND) 相關聯的任何 UI 元素中，裝載 UWP 控制項。 透過此 API，非 UWP 的傳統型應用程式可使用僅由 UWP 控制項提供的最新 Windows 10 UI 功能。 例如，非 UWP 傳統型應用程式可使用此 API 來裝載 UWP 控制項，這些控制項使用 [Fluent Design 系統](/windows/uwp/design/fluent-design-system/index)，並且支援 [Windows Ink](/windows/uwp/design/input/pen-and-stylus-interactions)。

UWP XAML 裝載 API 提供更廣泛的控制項集合的基礎，讓開發人員能夠將 Fluent UI 帶入非 UWP 傳統型應用程式。 此功能稱為 *XAML Islands*。 如需此功能的概觀，請參閱[在傳統型應用程式中裝載 UWP XAML 控制項 (XAML Islands)](xaml-islands.md)。

> [!NOTE]
> 如果您有關於 XAML Islands 的意見反應，請在 [Microsoft.Toolkit.Win32 存放庫](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/issues)中建立新問題，並留下您的意見。 如果您想要私下提交意見反應，可以將意見反應傳送到 XamlIslandsFeedback@microsoft.com。 您的深入解析和案例對我們而言非常重要。

## <a name="is-the-uwp-xaml-hosting-api-the-right-choice-for-your-desktop-app"></a>UWP XAML 裝載 API 是否為傳統型應用程式的合適選擇？

UWP XAML 裝載 API 可提供低階的基礎結構，用於在傳統型應用程式中裝載 UWP 控制項。 某些類型的傳統型應用程式可選擇更方便的替代 API 來完成此目標。

* 如果您有 C++ Win32 傳統型應用程式，而且想要在應用程式中裝載 UWP 控制項，則必須使用 UWP XAML 裝載 API。 這些類型的應用程式沒有任何替代項目。

* 對於 WPF 和 Windows Forms 應用程式，強烈建議您使用 Windows 社群工具組中的 [XAML Island .NET 控制項](xaml-islands.md#wpf-and-windows-forms-applications)，而非直接使用 UWP XAML 裝載 API。 這些控制項會在內部使用 UWP XAML 裝載 API，並在您直接使用 UWP XAML 裝載 API (包括鍵盤瀏覽和版面配置變更) 的情況下，實作您需要自行處理的所有行為。

因為我們建議僅 C++ Win32 應用程式使用 UWP XAML 裝載 API，所以本文主要提供 C++ Win32 應用程式的指示和範例。 不過，您可以選擇在 WPF 和 Windows Forms 應用程式中使用 UWP XAML 裝載 API。 本文針對 Windows 社群工具組中的 WPF 和 Windows Forms，指向[主控制項](xaml-islands.md#host-controls)的相關原始程式碼，以便您可以了解這些控制項如何使用 UWP XAML 裝載 API。

## <a name="learn-how-to-use-the-xaml-hosting-api"></a>了解如何使用 XAML 裝載 API

若要依照逐步指示進行，以了解在 C++ Win32 應用程式中使用 XAML 裝載 API 的程式碼範例，請參閱下列文章：

* [裝載標準 UWP 控制項](host-standard-control-with-xaml-islands-cpp.md)
* [裝載自訂 UWP 控制項](host-custom-control-with-xaml-islands-cpp.md)
* [進階案例](advanced-scenarios-xaml-islands-cpp.md)

## <a name="samples"></a>範例

您在程式碼中使用 UWP XAML 裝載 API 的方式，取決於您的應用程式類型、應用程式的設計以及其他因素。 為了協助說明如何在完整應用程式的內容中使用此 API，本文參照以下範例中的程式碼。

### <a name="c-win32"></a>C++ Win32

以下範例示範如何在 C++ Win32 應用程式中使用 UWP XAML 裝載 API：

* [簡單的 XAML Island 範例](https://github.com/microsoft/Xaml-Islands-Samples/tree/master/Standalone_Samples/CppWinRT_Basic_Win32App)。 這個範例示範在未封裝 C++ Win32 應用程式 (也就是 MSIX 套件中未內建的應用程式) 中裝載 UWP 控制項的基本實作。

* [具有自訂控制項的 XAML Island 範例](https://github.com/microsoft/Xaml-Islands-Samples/tree/master/Samples/Win32)。 此範例示範在封裝 C++ Win32 應用程式中，裝載自訂的 UWP 控制項，以及處理其他行為 (例如鍵盤輸入和焦點瀏覽) 的完整實作。

### <a name="wpf-and-windows-forms"></a>WPF 和 Windows Forms

Windows 社群工具組中的[WindowsXamlHost](/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) 控制項做為在 WPF 和 Windows Forms 應用程式中，使用 UWP 裝載 API 的參考範例。 原始程式碼可於下列位置取得：

* 如需 WPF 版本的控制項，請[前往這裡](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Wpf.UI.XamlHost)。 WPF 版本衍生自 [System.Windows.Interop.HwndHost](/dotnet/api/system.windows.interop.hwndhost)。

* 如需 Windows Forms 版本的控制項，請[前往這裡](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Forms.UI.XamlHost)。 Windows Forms 版本衍生自 [System.Windows.Forms.Control](/dotnet/api/system.windows.forms.control)。

> [!NOTE]
> 我們強烈建議您使用 Windows 社群工具組中的 [XAML Island .NET 控制項](xaml-islands.md#wpf-and-windows-forms-applications)，而非直接使用 WPF 和 Windows Forms 應用程式中的 UWP XAML 裝載 API。 本文中的 WPF 和 Windows Forms 範例連結僅供說明目的。

## <a name="architecture-of-the-api"></a>API 的架構

UWP XAML 裝載 API 包含這些主要 Windows 執行階段類型和 COM 介面。

|  介面類型 | 說明 |
|--------------------|-------------|
| [WindowsXamlManager](/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager) | 此類別表示 UWP XAML 架構。 此類別提供單一靜態 [InitializeForCurrentThread](/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager.initializeforcurrentthread) 方法，該方法可在傳統型應用程式中的目前執行緒初始化 UWP XAML 架構。 |
| [DesktopWindowXamlSource](/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) | 這個類別表示您在傳統型應用程式中所裝載 UWP XAML 內容的執行個體。 這個類別最重要的成員是 [Content](/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.content) 屬性。 您可以將此屬性指派給要裝載的 [Windows.UI.Xaml.UIElement](/uwp/api/windows.ui.xaml.uielement)。 這個類別也有其他成員，可將鍵盤焦點瀏覽路由傳入和傳出 XAML Islands。 |
| IDesktopWindowXamlSourceNative | 此 COM 介面提供 **AttachToWindow** 方法，可供用來將應用程式中的 XAML Island 連結至父系 UI 元素。 每個 **DesktopWindowXamlSource** 物件都會實作此介面。 此介面是在 windows.ui.xaml.hosting.desktopwindowxamlsource.h 中宣告。 |
| IDesktopWindowXamlSourceNative2 | 此 COM 介面提供  **PreTranslateMessage** 方法，可讓 UWP XAML 架構正確處理特定的 Windows 訊息。 每個 **DesktopWindowXamlSource** 物件都會實作此介面。 此介面是在 windows.ui.xaml.hosting.desktopwindowxamlsource.h 中宣告。 |

下圖說明在裝載於傳統型應用程式 XAML Island 中的物件階層。

* 底層是要裝載 XAML Island 的應用程式中的 UI 元素。 該 UI 元素必須具視窗控制代碼 (HWND)。 您可以在其中裝載 XAML Island 的 UI 元素範例，包括適用於 C++ Win32 應用程式的 [window](/windows/desktop/winmsg/about-windows)、適用於 WPF 應用程式的 [System.Windows.Interop.HwndHost](/dotnet/api/system.windows.interop.hwndhost)，以及適用於 Windows Forms 應用程式的 [System.Windows.Forms.Control](/dotnet/api/system.windows.forms.control)。

* 下一個層級是 **DesktopWindowXamlSource** 物件。 此物件提供裝載 XAML Island 的基礎結構。 您的程式碼負責建立此物件，並將其連結至父系 UI 元素。

* 當您建立 **DesktopWindowXamlSource** 時，此物件會自動建立原生子視窗，以裝載您的 UWP 控制項。 這個原生子視窗主要是從您的程式碼中提取出來的，但是如有需要，您可以存取其控制代碼 (HWND)。

* 最後，最上層是要裝載於傳統型應用程式的 UWP 控制項。 此控制項可能是衍生自 [Windows.UI.Xaml.UIElement](/uwp/api/windows.ui.xaml.uielement) 的任何 UWP 物件，包括 Windows SDK 提供的任何 UWP 控制項，以及自訂使用者控制項。

![DesktopWindowXamlSource 架構](images/xaml-islands/xaml-hosting-api-rev2.png)

> [!NOTE]
> 當您在桌面應用程式中裝載 XAML Island 時，您可以在相同的執行緒上同時執行 XAML 內容的多個樹狀結構。 若要在 XAML Island 中存取 XAML 內容樹狀結構的根元素，並取得其裝載所在內容的相關資訊，請使用 [XamlRoot](/uwp/api/windows.ui.xaml.xamlroot) 類別。 [CoreWindow](/uwp/api/windows.ui.core.corewindow)、[ApplicationView](/uwp/api/windows.ui.viewmanagement.applicationview) 以及 [Window](/uwp/api/windows.ui.xaml.window) API 不會提供 XAML Islands 的正確資訊。 如需詳細資訊，請參閱[本節](xaml-islands.md#window-host-context-for-xaml-islands)。

## <a name="troubleshooting"></a>疑難排解

### <a name="error-using-uwp-xaml-hosting-api-in-a-uwp-app"></a>在 UWP 應用程式中使用 UWP XAML 裝載 API 時發生錯誤

| 問題 | 解決方法 |
|-------|------------|
| 您的應用程式收到包含下列訊息的 **COMException**：「無法啟用 DesktopWindowXamlSource。 此類型無法用於 UWP 應用程式。」 或「無法啟用 WindowsXamlManager。 此類型無法用於 UWP 應用程式。」 | 此錯誤表示您嘗試在 UWP 應用程式中使用 UWP XAML 裝載 API (具體而言，您正嘗試將 [DesktopWindowXamlSource](/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) 或 [WindowsXamlManager](/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager) 類型具現化)。 UWP XAML 裝載 API 僅適用於非 UWP 傳統型應用程式，例如 WPF、Windows Forms，以及 C++ Win32 應用程式。 |

### <a name="error-trying-to-use-the-windowsxamlmanager-or-desktopwindowxamlsource-types"></a>嘗試使用 WindowsXamlManager 或 DesktopWindowXamlSource 類型時發生錯誤

| 問題 | 解決方法 |
|-------|------------|
| 您的應用程式收到包含下列訊息的例外：「針對以 Windows 版本10.0.18226.0 和更新版本為目標的應用程式，支援 WindowsXamlManager 和 DesktopWindowXamlSource。 請檢查應用程式資訊清單或封裝資訊清單，並確保已更新 MaxTestedVersion 屬性。」 | 此錯誤表示您的應用程式嘗試在 UWP XAML 裝載 API 中使用 **WindowsXamlManager** 或 **DesktopWindowXamlSource** 類型，但作業系統無法判斷應用程式是針對 Windows 10，版本 1903 或更新版本建置。 UWP XAML 裝載 API 最初在舊版 Windows 10 中是以預覽推出的，但僅從 Windows 10，版本 1903 開始才受支援。</p></p>為了解決此問題，請為應用程式建立 MSIX 套件，並從該封裝執行，或在您的專案中安裝 [Microsoft.Toolkit.Win32.UI.SDK](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.SDK) NuGet 套件。  |

### <a name="error-attaching-to-a-window-on-a-different-thread"></a>連結至不同執行緒上的視窗時發生錯誤

| 問題 | 解決方法 |
|-------|------------|
| 您的應用程式收到包含下列訊息的 **COMException**：「AttachToWindow 方法失敗，因為指定的 HWND 是在不同的執行緒上建立的。」 | 此錯誤表示您的應用程式已呼叫 **IDesktopWindowXamlSourceNative::AttachToWindow** 方法，並將視窗 (該視窗建立於其他執行緒上) 的 HWND 傳遞給該方法。 您必須將視窗的 HWND (該視窗建立於與呼叫方法的程式碼相同的執行緒上) 傳遞給此方法。 |

### <a name="error-attaching-to-a-window-on-a-different-top-level-window"></a>連結至不同最上層視窗的視窗時發生錯誤

| 問題 | 解決方法 |
|-------|------------|
| 您的應用程式會收到具有下列訊息的 **COMException**：「AttachToWindow 方法失敗，因為指定的 HWND 與先前在相同執行緒傳遞至 AttachToWindow 的 HWND ，繼承自不同的最上層視窗。」 | 此錯誤表示您的應用程式呼叫 **IDesktopWindowXamlSourceNative::AttachToWindow** 方法，並將繼承自不同最上層視窗之視窗的 HWND (與先前在相同執行緒上呼叫該方法所指定的視窗相比)，傳遞給該方法。</p></p>當您的應用程式在特定執行緒上呼叫 **AttachToWindow** 之後，相同執行緒上所有其他 [DesktopWindowXamlSource](/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) 物件只能連結至初次呼叫 **AttachToWindow** 中傳遞之相同最上層視窗的子系視窗。 當特定執行緒的所有 **DesktopWindowXamlSource** 物件關閉時，下一個 **DesktopWindowXamlSource** 就可以自由地連結至任何視窗。</p></p>若要解決此問題，請關閉繫結至該執行緒上其他最上層視窗的所有 **DesktopWindowXamlSource** 物件，或為此 **DesktopWindowXamlSource** 建立新的執行緒。 |

## <a name="related-topics"></a>相關主題

* [在傳統型應用程式中裝載 UWP XAML 控制項 (XAML Islands)](xaml-islands.md)
* [在 C++ Win32 應用程式中裝載標準 UWP 控制項](host-standard-control-with-xaml-islands-cpp.md)
* [在 C++ Win32 應用程式中裝載自訂 UWP 控制項](host-custom-control-with-xaml-islands-cpp.md)
* [C++ Win32 應用程式中適用於 XAML Islands 的進階案例](advanced-scenarios-xaml-islands-cpp.md)
* [XAML Islands 程式碼範例](https://github.com/microsoft/Xaml-Islands-Samples)
* [C++ Win32 XAML Islands 範例](https://github.com/microsoft/Xaml-Islands-Samples/tree/master/Samples/Win32/SampleCppApp)