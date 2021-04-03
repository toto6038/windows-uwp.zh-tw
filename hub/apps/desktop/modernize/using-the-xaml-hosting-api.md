---
description: 本文說明如何在您的 desktop c + + Win32 應用程式中裝載 UWP XAML UI。
title: 在 C++ Win32 應用程式中使用 UWP XAML 裝載 API
ms.date: 03/23/2020
ms.topic: article
keywords: windows 10、uwp、windows forms、wpf、win32、xaml 島
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 6237fe47a6518ae7d9916be4568bf2cd4295322f
ms.sourcegitcommit: cc871be2508f52509b6a947fe879aeec360d0fd2
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/02/2021
ms.locfileid: "106270335"
---
# <a name="using-the-uwp-xaml-hosting-api-in-a-c-win32-app"></a>在 C++ Win32 應用程式中使用 UWP XAML 裝載 API

從 Windows 10 開始，1903版、非 UWP 桌面應用程式 (包括 c + + Win32、WPF 和 Windows Forms apps) 都可以使用 *UWP XAML 裝載 API* ，在與視窗控制碼相關聯的任何 UI 專案中裝載 uwp 控制項 (HWND) 。 此 API 可讓非 UWP 桌面應用程式使用只能透過 UWP 控制項使用的最新 Windows 10 UI 功能。 例如，非 UWP 桌面應用程式可以使用此 API 來裝載使用 [Fluent Design 系統](/windows/uwp/design/fluent-design-system/index) 並支援 [Windows Ink](/windows/uwp/design/input/pen-and-stylus-interactions)的 UWP 控制項。

UWP XAML 裝載 API 為一組更廣泛的控制項提供基礎，讓開發人員能夠將流暢的 UI 帶入非 UWP 桌面應用程式。 這項功能稱為 *XAML 孤島*。 如需這項功能的總覽，請參閱 [在桌面應用程式中裝載 UWP XAML 控制項 (XAML 孤島) ](xaml-islands.md)。

> [!NOTE]
> 如果您有關於 XAML Islands 的意見反應，請在 [Microsoft.Toolkit.Win32 存放庫](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/issues)中建立新問題，並留下您的意見。

## <a name="is-the-uwp-xaml-hosting-api-the-right-choice-for-your-desktop-app"></a>UWP XAML 裝載 API 是您桌面應用程式的正確選擇嗎？

UWP XAML 裝載 API 提供低層級的基礎結構，可在桌面應用程式中裝載 UWP 控制項。 有些類型的桌面應用程式可以選擇使用替代、更方便的 Api 來達成此目標。

* 如果您有 c + + Win32 傳統型應用程式，而您想要在應用程式中裝載 UWP 控制項，您必須使用 UWP XAML 裝載 API。 這些類型的應用程式沒有替代方案。

* 針對 WPF 和 Windows Forms 應用程式，我們強烈建議您在 Windows 社群工具組中使用 [XAML 島 .net 控制項](xaml-islands.md#wpf-and-windows-forms-applications) ，而不是直接使用 UWP XAML 裝載 API。 如果您直接使用 UWP XAML 裝載 API （包括鍵盤流覽和版面配置變更），這些控制項會在內部使用 UWP XAML 裝載 API，並執行您需要自行處理的所有行為。

由於我們建議只有 c + + Win32 應用程式使用 UWP XAML 裝載 API，因此本文主要提供 c + + Win32 應用程式的指示和範例。 不過，您可以在 WPF 中使用 UWP XAML 裝載 API，並在選擇的情況下 Windows Forms 應用程式。 本文指向 WPF 的 [主控制項](xaml-islands.md#host-controls) 和 Windows 社群工具組中 Windows Forms 的相關原始程式碼，讓您可以查看 UWP XAML 裝載 API 如何由這些控制項使用。

## <a name="learn-how-to-use-the-xaml-hosting-api"></a>瞭解如何使用 XAML 裝載 API

若要遵循逐步指示，以瞭解如何在 c + + Win32 應用程式中使用 XAML 裝載 API 的程式碼範例，請參閱下列文章：

* [裝載標準 UWP 控制項](host-standard-control-with-xaml-islands-cpp.md)
* [裝載自訂 UWP 控制項](host-custom-control-with-xaml-islands-cpp.md)
* [進階案例](advanced-scenarios-xaml-islands-cpp.md)

## <a name="samples"></a>範例

您在程式碼中使用 UWP XAML 裝載 API 的方式取決於您的應用程式類型、應用程式的設計，以及其他因素。 為了協助說明如何在完整應用程式的內容中使用此 API，本文參考下列範例中的程式碼。

### <a name="c-win32"></a>C + + Win32

下列範例示範如何在 c + + Win32 應用程式中使用 UWP XAML 裝載 API：

* [簡單的 XAML 島範例](https://github.com/microsoft/Xaml-Islands-Samples/tree/master/Standalone_Samples/CppWinRT_Basic_Win32App)。 這個範例示範如何將 UWP 控制項裝載在未封裝的 c + + Win32 應用程式中， (也就是未內建在 MSIX 套件中的應用程式) 。

* [具有自訂控制項範例的 XAML 島](https://github.com/microsoft/Xaml-Islands-Samples/tree/master/Samples/Win32)。 此範例示範在封裝的 c + + Win32 應用程式中裝載自訂 UWP 控制項，以及處理鍵盤輸入和焦點導覽等其他行為的完整執行。

### <a name="wpf-and-windows-forms"></a>WPF 和 Windows Forms

Windows 社群工具組中的 [WindowsXamlHost](/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) 控制項，可做為在 WPF 和 Windows Forms 應用程式中使用 UWP 裝載 API 的參考範例。 您可以在下列位置取得原始程式碼：

* 若為控制項的 WPF 版本，請 [移至這裡](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Wpf.UI.XamlHost)。 WPF 版本衍生自 [HwndHost](/dotnet/api/system.windows.interop.hwndhost)。

* 如需 Windows Forms 版本的控制項，請 [移至這裡](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Forms.UI.XamlHost)。 Windows Forms 版本衍生自 [Control](/dotnet/api/system.windows.forms.control)。

> [!NOTE]
> 強烈建議您在 Windows 社群工具組中使用 [XAML 島 .net 控制項](xaml-islands.md#wpf-and-windows-forms-applications) ，而不是直接在 WPF 和 Windows Forms 應用程式中使用 UWP XAML 裝載 API。 本文中的 WPF 和 Windows Forms 範例連結僅供說明之用。

## <a name="architecture-of-the-api"></a>API 的架構

UWP XAML 裝載 API 包含這些主要 Windows 執行階段類型和 COM 介面。

|  類型或介面 | 描述 |
|--------------------|-------------|
| [WindowsXamlManager](/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager) | 此類別代表 UWP XAML 架構。 此類別提供單一靜態 [InitializeForCurrentThread](/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager.initializeforcurrentthread) 方法，此方法會在桌面應用程式中的目前線程上初始化 UWP XAML 架構。 |
| [DesktopWindowXamlSource](/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) | 此類別代表您要在桌面應用程式中裝載的 UWP XAML 內容實例。 此類別最重要的成員是 [Content](/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.content) 屬性。 您可以將此屬性指派給您想要裝載的 [Windows. UIElement](/uwp/api/windows.ui.xaml.uielement) 。 這個類別也有其他成員可將鍵盤焦點導覽移入和移出 XAML 島。 |
| IDesktopWindowXamlSourceNative | 這個 COM 介面會提供 **AttachToWindow** 方法，您可以使用此方法將應用程式中的 XAML 島附加至父 UI 元素。 每個 **DesktopWindowXamlSource** 物件都會執行這個介面。 這個介面是在 desktopwindowxamlsource 中宣告的。 |
| IDesktopWindowXamlSourceNative2 | 這個 COM 介面提供了 **PreTranslateMessage** 方法，可讓 UWP XAML 架構正確地處理特定的 Windows 訊息。 每個 **DesktopWindowXamlSource** 物件都會執行這個介面。 這個介面是在 desktopwindowxamlsource 中宣告的。 |

下圖說明在傳統型應用程式中主控之 XAML 島中的物件階層。

* 基底層級是您應用程式中的 UI 元素，您想要在其中裝載 XAML 島。 這個 UI 元素必須有 (HWND) 的視窗控制碼。 您可以在其中裝載 XAML 島的 UI 元素範例包括適用于 c + + Win32 應用程式的[視窗](/windows/desktop/winmsg/about-windows)、適用于 WPF 應用程式的 HwndHost，以及適用于 Windows Forms 應用程式的[](/dotnet/api/system.windows.interop.hwndhost) [控制項](/dotnet/api/system.windows.forms.control)。

* 下一個層級是 **DesktopWindowXamlSource** 物件。 這個物件會提供用來裝載 XAML 島的基礎結構。 您的程式碼會負責建立這個物件，並將它附加至父 UI 元素。

* 當您建立 **DesktopWindowXamlSource** 時，此物件會自動建立原生子視窗來裝載您的 UWP 控制項。 這個原生子視窗大多是從您的程式碼抽象化，但您可以視需要存取 (HWND) 的控制碼。

* 最後，最上層是您想要在桌面應用程式中裝載的 UWP 控制項。 這可以是任何衍生自 node.js 的 UWP [物件，包括](/uwp/api/windows.ui.xaml.uielement)Windows SDK 提供的任何 uwp 控制項，以及自訂的使用者控制項。

![DesktopWindowXamlSource 架構](images/xaml-islands/xaml-hosting-api-rev2.png)

> [!NOTE]
> 當您在桌面應用程式中裝載 XAML Island 時，您可以在相同的執行緒上同時執行 XAML 內容的多個樹狀結構。 若要在 XAML Island 中存取 XAML 內容樹狀結構的根元素，並取得其裝載所在內容的相關資訊，請使用 [XamlRoot](/uwp/api/windows.ui.xaml.xamlroot) 類別。 [CoreWindow](/uwp/api/windows.ui.core.corewindow)、 [ApplicationView](/uwp/api/windows.ui.viewmanagement.applicationview)和[WINDOW](/uwp/api/windows.ui.xaml.window) api 不會提供 XAML 島的正確資訊。 如需詳細資訊，請參閱[本節](xaml-islands.md#window-host-context-for-xaml-islands)。

## <a name="troubleshooting"></a>疑難排解

### <a name="error-using-uwp-xaml-hosting-api-in-a-uwp-app"></a>在 UWP 應用程式中使用 UWP XAML 裝載 API 時發生錯誤

| 問題 | 解決方案 |
|-------|------------|
| 您的應用程式會收到 **COMException** ，並顯示下列訊息：「無法啟動 DesktopWindowXamlSource。 此類型不能用在 UWP 應用程式中。」 或「無法啟動 WindowsXamlManager。 此類型不能用在 UWP 應用程式中。」 | 此錯誤表示您嘗試使用 UWP XAML 裝載 API (具體而言，您嘗試將 [DesktopWindowXamlSource](/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) 或 [WindowsXamlManager](/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager) 類型具現化在 UWP 應用程式中) 。 UWP XAML 裝載 API 僅適用于非 UWP 桌面應用程式，例如 WPF、Windows Forms 和 c + + Win32 應用程式。 |

### <a name="error-trying-to-use-the-windowsxamlmanager-or-desktopwindowxamlsource-types"></a>嘗試使用 WindowsXamlManager 或 DesktopWindowXamlSource 類型時發生錯誤

| 問題 | 解決方案 |
|-------|------------|
| 您的應用程式會收到例外狀況，並顯示下列訊息：「以 Windows 版本10.0.18226.0 和更新版本為目標的應用程式支援 WindowsXamlManager 和 DesktopWindowXamlSource。 請檢查應用程式資訊清單或套件資訊清單，並確定 MaxTestedVersion 屬性已更新。」 | 此錯誤表示您的應用程式嘗試在 UWP XAML 裝載 API 中使用 **WindowsXamlManager** 或 **DesktopWindowXamlSource** 類型，但 OS 無法判斷應用程式是否已建立為目標 Windows 10 1903 版或更新版本。 UWP XAML 裝載 API 首次在舊版 Windows 10 中引進為預覽版本，但從1903版 Windows 10 開始才支援此功能。</p></p>若要解決此問題，請建立應用程式的 MSIX 套件，並從套件執行它，或在您 [的專案](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.SDK) 中安裝 node.js。  |

### <a name="error-attaching-to-a-window-on-a-different-thread"></a>附加至不同執行緒上的視窗時發生錯誤

| 問題 | 解決方案 |
|-------|------------|
| 您的應用程式會收到具有下列訊息的 **COMException** ：「AttachToWindow 方法失敗，因為指定的 HWND 是建立在不同的執行緒上。」 | 此錯誤表示您的應用程式呼叫 **IDesktopWindowXamlSourceNative：： AttachToWindow** 方法，並將在不同執行緒上建立之視窗的 HWND 傳遞給它。 您必須在與您呼叫方法的程式碼相同的執行緒上建立的視窗 HWND 傳遞這個方法。 |

### <a name="error-attaching-to-a-window-on-a-different-top-level-window"></a>附加至不同最上層視窗的視窗時發生錯誤

| 問題 | 解決方案 |
|-------|------------|
| 您的應用程式會收到包含下列訊息的 **COMException** ：「AttachToWindow 方法失敗，因為指定的 HWND 從與先前傳遞給相同執行緒上 ATTACHTOWINDOW 的 HWND 不同的最上層視窗進行遞減」。 | 此錯誤表示您的應用程式呼叫 **IDesktopWindowXamlSourceNative：： AttachToWindow** 方法，並將從不同最上層視窗中所指定視窗的 HWND 傳遞給它，而不是在相同執行緒上對這個方法所指定的視窗。</p></p>當您的應用程式在特定執行緒上呼叫 **AttachToWindow** 之後，相同執行緒上的所有其他 [DesktopWindowXamlSource](/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) 物件只能附加至視窗，而這些物件是在第一次呼叫 **AttachToWindow** 時所傳遞之相同最上層視窗的下階。 當特定執行緒的所有 **DesktopWindowXamlSource** 物件都關閉時，下一次 **DesktopWindowXamlSource** 就可以自由附加到任何視窗。</p></p>若要解決這個問題，請關閉系結至此執行緒上其他最上層視窗的所有 **DesktopWindowXamlSource** 物件，或為此 **DesktopWindowXamlSource** 建立新的執行緒。 |

## <a name="related-topics"></a>相關主題

* [在傳統型應用程式中裝載 UWP XAML 控制項 (XAML Islands)](xaml-islands.md)
* [在 C++ Win32 應用程式中裝載標準 UWP 控制項](host-standard-control-with-xaml-islands-cpp.md)
* [在 C++ Win32 應用程式中裝載自訂 UWP 控制項](host-custom-control-with-xaml-islands-cpp.md)
* [C++ Win32 應用程式中適用於 XAML Islands 的進階案例](advanced-scenarios-xaml-islands-cpp.md)
* [XAML Islands 程式碼範例](https://github.com/microsoft/Xaml-Islands-Samples) \(英文\)
* [C++ Win32 XAML Islands 範例](https://github.com/microsoft/Xaml-Islands-Samples/tree/master/Samples/Win32/SampleCppApp)