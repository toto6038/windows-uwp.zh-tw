---
description: 本文說明如何在您的桌面C++ Win32 應用程式中裝載 UWP XAML UI。
title: 在 C++ Win32 應用程式中使用 UWP XAML 裝載 API
ms.date: 03/23/2020
ms.topic: article
keywords: windows 10，uwp，windows forms，wpf，win32，xaml 群島
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 36c3aeb7a51c84e92c5bca461aee7efe50740237
ms.sourcegitcommit: c660def841abc742600fbcf6ed98e1f4f7beb8cc
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/24/2020
ms.locfileid: "80218458"
---
# <a name="using-the-uwp-xaml-hosting-api-in-a-c-win32-app"></a>在 C++ Win32 應用程式中使用 UWP XAML 裝載 API

從 Windows 10 版本1903開始，非 UWP 桌面應用程式（包括C++ WIN32、WPF 和 Windows Forms 應用程式）可以使用*UWP XAML 裝載 API* ，將 uwp 控制項裝載于與視窗控制碼（HWND）相關聯的任何 UI 專案中。 此 API 可讓非 UWP 桌面應用程式使用僅透過 UWP 控制項提供的最新 Windows 10 UI 功能。 例如，非 UWP 桌面應用程式可以使用此 API 來裝載使用[流暢設計系統](/windows/uwp/design/fluent-design-system/index)並支援[WINDOWS Ink](/windows/uwp/design/input/pen-and-stylus-interactions)的 UWP 控制項。

UWP XAML 裝載 API 提供更廣泛的控制項集合的基礎，讓開發人員能夠將流暢的 UI 帶入非 UWP 桌面應用程式。 這項功能稱為「 *XAML 島*」。 如需這項功能的總覽，請參閱[在桌面應用程式中裝載 UWP XAML 控制項（Xaml 島）](xaml-islands.md)。

> [!NOTE]
> 如果您有關於 XAML Islands 的意見反應，請在 [Microsoft.Toolkit.Win32 存放庫](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/issues)中建立新問題，並留下您的意見。 如果您想要私下提交意見反應，可以將意見反應傳送到 XamlIslandsFeedback@microsoft.com。 您的深入解析和案例對我們而言非常重要。

## <a name="is-the-uwp-xaml-hosting-api-the-right-choice-for-your-desktop-app"></a>UWP XAML 裝載 API 是您桌面應用程式的正確選擇嗎？

UWP XAML 裝載 API 提供了在桌面應用程式中裝載 UWP 控制項的低層級基礎結構。 某些類型的桌面應用程式可以選擇使用替代、更方便的 Api 來完成此目標。

* 如果您有C++ Win32 桌面應用程式，而且想要在應用程式中裝載 uwp 控制項，則必須使用 UWP XAML 裝載 API。 這些類型的應用程式沒有任何替代方案。

* 針對 WPF 和 Windows Forms 應用程式，強烈建議您在 Windows 社區工具組中使用[XAML 島 .net 控制項](xaml-islands.md#wpf-and-windows-forms-applications)，而不是直接使用 UWP XAML 裝載 API。 如果您直接使用 UWP XAML 裝載 API，包括鍵盤導覽和版面配置變更，這些控制項就會在內部使用 UWP XAML 裝載 API，並實作為您必須自行處理的所有行為。

因為我們建議只有C++ win32 應用程式使用 UWP XAML 裝載 API，本文主要會提供C++ Win32 應用程式的指示和範例。 不過，如果您選擇，您可以在 WPF 和 Windows Forms 應用程式中使用 UWP XAML 裝載 API。 本文指向適用于 WPF 的[主控制項](xaml-islands.md#host-controls)和 Windows Forms 在 Windows 社區工具組中的相關原始程式碼，讓您可以看到 UWP XAML 裝載 API 如何由這些控制項使用。

## <a name="learn-how-to-use-the-xaml-hosting-api"></a>瞭解如何使用 XAML 裝載 API

若要依照逐步指示進行，以瞭解在 Win32 應用程式中C++使用 XAML 裝載 API 的程式碼範例，請參閱下列文章：

* [裝載標準 UWP 控制項](host-standard-control-with-xaml-islands-cpp.md)
* [裝載自訂 UWP 控制項](host-custom-control-with-xaml-islands-cpp.md)
* [Advanced 案例](advanced-scenarios-xaml-islands-cpp.md)

## <a name="samples"></a>Samples

您在程式碼中使用 UWP XAML 裝載 API 的方式，取決於您的應用程式類型、應用程式的設計和其他因素。 為了協助說明如何在完整應用程式的內容中使用此 API，本文會參考下列範例中的程式碼。

### <a name="c-win32"></a>C++32

下列範例示範如何在C++ Win32 應用程式中使用 UWP XAML 裝載 API：

* [簡單的 XAML 島範例](https://github.com/microsoft/Xaml-Islands-Samples/tree/master/Standalone_Samples/CppWinRT_Basic_Win32App)。 這個範例示範在未封裝C++的 Win32 應用程式（也就是未內建于 MSIX 套件中的應用程式）中裝載 UWP 控制項的基本部署。

* [具有自訂控制項範例的 XAML 島](https://github.com/microsoft/Xaml-Islands-Samples/tree/master/Samples/Win32)。 這個範例示範在封裝C++的 Win32 應用程式中裝載自訂 UWP 控制項的完整執行，以及處理其他行為，例如鍵盤輸入和焦點導覽。

### <a name="wpf-and-windows-forms"></a>WPF 和 Windows Form

Windows 社區工具組中的[WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost)控制項，做為在 WPF 和 Windows Forms 應用程式中使用 UWP 裝載 API 的參考範例。 原始程式碼可在下列位置取得：

* 若為 WPF 版本的控制項，請[移至這裡](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Wpf.UI.XamlHost)。 WPF 版本衍生自[HwndHost](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost)。

* 如需控制項的 Windows Forms 版本，請[移至這裡](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Forms.UI.XamlHost)。 Windows Forms 版本衍生自[system.web](https://docs.microsoft.com/dotnet/api/system.windows.forms.control)。

> [!NOTE]
> 強烈建議您在 Windows 社區工具組中使用[XAML 島 .net 控制項](xaml-islands.md#wpf-and-windows-forms-applications)，而不是直接在 WPF 和 Windows Forms 應用程式中使用 UWP XAML 裝載 API。 本文中的 WPF 和 Windows Forms 範例連結僅供說明之用。

## <a name="architecture-of-the-api"></a>API 的架構

UWP XAML 裝載 API 包含這些主要的 Windows 執行階段類型和 COM 介面。

|  類型或介面 | 描述 |
|--------------------|-------------|
| [WindowsXamlManager](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager) | 此類別代表 UWP XAML 架構。 此類別提供單一靜態[InitializeForCurrentThread](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager.initializeforcurrentthread)方法，可在桌面應用程式中的目前線程上初始化 UWP XAML 架構。 |
| [DesktopWindowXamlSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) | 此類別代表您在桌面應用程式中裝載之 UWP XAML 內容的實例。 這個類別最重要的成員是[Content](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.content)屬性。 您會將這個屬性指派給您要裝載的[Windows.](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) node.js。 這個類別也有其他成員，可將鍵盤焦點導覽傳入和傳出 XAML 島。 |
| IDesktopWindowXamlSourceNative | 這個 COM 介面提供**AttachToWindow**方法，您可以用它將應用程式中的 XAML 島附加至父 UI 元素。 每個**DesktopWindowXamlSource**物件都會執行此介面。 這個介面是在 desktopwindowxamlsource 中宣告的。 |
| IDesktopWindowXamlSourceNative2 | 這個 COM 介面提供**PreTranslateMessage**方法，可讓 UWP XAML 架構正確地處理特定的 Windows 訊息。 每個**DesktopWindowXamlSource**物件都會執行此介面。 這個介面是在 desktopwindowxamlsource 中宣告的。 |

下圖說明在傳統型應用程式中裝載的 XAML 島中，物件的階層結構。

* 在基底層級是您的應用程式中，您想要在其中裝載 XAML 島的 UI 元素。 這個 UI 元素必須有視窗控制碼（HWND）。 您可以在其中裝載 XAML 島的 UI 元素範例C++包括 Win32 應用程式的[視窗](https://docs.microsoft.com/windows/desktop/winmsg/about-windows)、 [HwndHost](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost) for WPF 應用程式，以及適用于 Windows Forms 應用程式的[system.web 控制項](https://docs.microsoft.com/dotnet/api/system.windows.forms.control)。

* 下一個層級是**DesktopWindowXamlSource**物件。 這個物件會提供裝載 XAML 島的基礎結構。 您的程式碼負責建立此物件，並將它附加至父 UI 元素。

* 當您建立**DesktopWindowXamlSource**時，此物件會自動建立原生子視窗來裝載 UWP 控制項。 這個原生子視窗大多會從您的程式碼中抽象化，但如有必要，您可以存取其控制碼（HWND）。

* 最後，最上層是您想要在桌面應用程式中裝載的 UWP 控制項。 這可以是衍生自[Windows](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement)的任何 uwp 物件，包括 Windows SDK 所提供的任何 uwp 控制項，以及自訂使用者控制項。

![DesktopWindowXamlSource 架構](images/xaml-islands/xaml-hosting-api-rev2.png)

> [!NOTE]
> 當您在桌面應用程式中裝載 XAML Island 時，您可以在相同的執行緒上同時執行 XAML 內容的多個樹狀結構。 若要在 XAML Island 中存取 XAML 內容樹狀結構的根元素，並取得其裝載所在內容的相關資訊，請使用 [XamlRoot](https://docs.microsoft.com/uwp/api/windows.ui.xaml.xamlroot) 類別。 [CoreWindow](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow)、 [ApplicationView](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationview)和[Window](https://docs.microsoft.com/uwp/api/windows.ui.xaml.window) API 不會提供正確的 XAML 島資訊。 如需詳細資訊，請參閱[本節](xaml-islands.md#window-host-context-for-xaml-islands)。

## <a name="troubleshooting"></a>疑難排解

### <a name="error-using-uwp-xaml-hosting-api-in-a-uwp-app"></a>在 UWP 應用程式中使用 UWP XAML 裝載 API 時發生錯誤

| 問題 | 解析度 |
|-------|------------|
| 您的應用程式會收到**COMException** ，並顯示下列訊息：「無法啟動 DesktopWindowXamlSource。 此類型不能用在 UWP 應用程式中。」 或「無法啟用 WindowsXamlManager。 此類型不能用在 UWP 應用程式中。」 | 此錯誤表示您正嘗試使用 uwp XAML 裝載 API （具體而言，您嘗試在 UWP 應用程式中具現化[DesktopWindowXamlSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource)或[WindowsXamlManager](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager)類型）。 UWP XAML 裝載 API 僅適用于非 UWP 桌面應用程式，例如 WPF、Windows Forms 和C++ Win32 應用程式。 |

### <a name="error-trying-to-use-the-windowsxamlmanager-or-desktopwindowxamlsource-types"></a>嘗試使用 WindowsXamlManager 或 DesktopWindowXamlSource 類型時發生錯誤

| 問題 | 解析度 |
|-------|------------|
| 您的應用程式會收到例外狀況，並顯示下列訊息：「WindowsXamlManager 和 DesktopWindowXamlSource 支援以 Windows 版本10.0.18226.0 和更新版本為目標的應用程式。 請檢查應用程式資訊清單或套件資訊清單，並確定 MaxTestedVersion 屬性已更新。」 | 此錯誤表示您的應用程式嘗試在 UWP XAML 裝載 API 中使用**WindowsXamlManager**或**DesktopWindowXamlSource**類型，但 OS 無法判斷應用程式是否已建立成以 Windows 10 1903 版或更新版本為目標。 UWP XAML 裝載 API 最初是在舊版 Windows 10 中引進為預覽版本，但只有從 Windows 10 版本1903開始才支援。</p></p>若要解決此問題，請建立應用程式的 MSIX 套件，並從封裝執行它，或在您的專案中安裝[Microsoft. 工具](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.SDK)組. SDK NuGet 套件。  |

### <a name="error-attaching-to-a-window-on-a-different-thread"></a>附加至不同執行緒上的視窗時發生錯誤

| 問題 | 解析度 |
|-------|------------|
| 您的應用程式會收到**COMException** ，並顯示下列訊息：「AttachToWindow 方法失敗，因為指定的 HWND 是在不同的執行緒上建立的。」 | 此錯誤表示您的應用程式呼叫**IDesktopWindowXamlSourceNative：： AttachToWindow**方法，並將在不同執行緒上建立之視窗的 HWND 傳遞給它。 您必須將在與您呼叫方法的程式碼相同的執行緒上建立的視窗 HWND，傳遞給這個方法。 |

### <a name="error-attaching-to-a-window-on-a-different-top-level-window"></a>附加至另一個最上層視窗的視窗時發生錯誤

| 問題 | 解析度 |
|-------|------------|
| 您的應用程式會收到**COMException** ，並顯示下列訊息：「AttachToWindow 方法失敗，因為指定的 hwnd 是從另一個最上層視窗遞減，而不是先前在同一個執行緒上傳遞至 ATTACHTOWINDOW 的 hwnd。」 | 此錯誤表示您的應用程式呼叫**IDesktopWindowXamlSourceNative：： AttachToWindow**方法，並將視窗的 HWND 傳遞至與您在同一個執行緒上先前呼叫這個方法所指定的視窗相比，從另一個最上層視窗遞減。</p></p>當您的應用程式在特定執行緒上呼叫**AttachToWindow**之後，相同執行緒上的所有其他[DesktopWindowXamlSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource)物件都只能附加至 windows，而這些子系是在第一次呼叫**AttachToWindow**時所傳遞的同一個最上層視窗的子系。 當特定執行緒的所有**DesktopWindowXamlSource**物件都關閉時，下一個**DesktopWindowXamlSource**就可以自由附加到任何視窗。</p></p>若要解決此問題，請關閉系結至這個執行緒上其他最上層視窗的所有**DesktopWindowXamlSource**物件，或為此**DesktopWindowXamlSource**建立新的執行緒。 |

## <a name="related-topics"></a>相關主題

* [在桌面應用程式中裝載 UWP XAML 控制項（XAML 島）](xaml-islands.md)
* [在C++ Win32 應用程式中裝載標準 UWP 控制項](host-standard-control-with-xaml-islands-cpp.md)
* [在C++ Win32 應用程式中裝載自訂 UWP 控制項](host-custom-control-with-xaml-islands-cpp.md)
* [Win32 應用程式中C++ XAML 孤島的 Advanced 案例](advanced-scenarios-xaml-islands-cpp.md)
* [XAML 島程式碼範例](https://github.com/microsoft/Xaml-Islands-Samples)
* [C++Win32 XAML 群島範例](https://github.com/microsoft/Xaml-Islands-Samples/tree/master/Samples/Win32/SampleCppApp)
