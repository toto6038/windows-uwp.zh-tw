---
Description: 本節可協助您了解如何在不同的應用程式平台上支援某些索引鍵的 Windows 功能，以及如何開始使用您的程式碼中的功能。
title: 功能與技術
ms.topic: article
ms.date: 05/08/2019
ms.openlocfilehash: ff91d8c01e6832e645cc857b638851e1833fc3f9
ms.sourcegitcommit: f0f933d5cf0be734373a7b03e338e65000cc3d80
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/21/2019
ms.locfileid: "65984978"
---
# <a name="features-and-technologies-for-windows-apps"></a>功能與技術的 Windows 應用程式

無論何種應用程式您是建置或您的目標的裝置，Windows 會支援許多功能，是重要的應用程式案例的重要建置組塊。 其中某些功能會公開到通用 Windows 平台 (UWP)，Win32 (Windows API) 和其他應用程式平台，以不同的方式。 下列文章可協助您了解如何在不同的應用程式平台上支援某些 Windows 功能，以及如何開始使用您的程式碼中的功能。

這篇文章提供量身訂做的文章的清單，若要深入了解有關如何存取重要的 Windows 功能和 UWP，Win32 中的技術 (Windows API)，WPF 中，與 Windows Form 應用程式平台。 完成每個平台的開發功能的相關資訊，請參閱下列資源：

* [UWP 文件](/windows/uwp/index)
* [Win32 (Windows API) 文件](/windows/desktop/index)
* [WPF 文件](https://docs.microsoft.com/dotnet/framework/wpf/index)
* [Windows Form 文件](https://docs.microsoft.com/dotnet/framework/winforms/index)

## <a name="key-windows-features-and-technologies"></a>索引鍵的 Windows 功能和技術

下列各節強調幾個重要的 Windows 功能和技術，可讓您傳遞現代化，並為您的客戶提供吸引人的經驗。

### <a name="windows-ink"></a>Windows Ink

![Surface 手寫筆](images/hero-small.png)  

Windows Ink 平台搭配手寫筆裝置之後，使用者就可以自然的方式建立數位手寫筆記、繪圖以及註解。 此平台支援擷取數位板輸入做為筆墨資料、產生筆墨資料、管理筆墨資料、在輸出裝置上將筆墨資料轉譯為筆劃，以及透過手寫辨識將筆墨轉換為文字。

如需 Windows 應用程式中使用 Windows 筆跡的不同方式的詳細資訊，請參閱[Windows Ink](/windows/uwp/design/input/pen-and-stylus-interactions)。

### <a name="speech-interactions"></a>語音互動

![以 SGRS 文法檔為基礎之限制的初始辨識畫面](images/speech-listening-initial.png)

![以 SGRS 文法檔為基礎之限制的最終辨識畫面](images/speech-listening-complete.png)

Windows 會提供許多方式來整合語音辨識和文字轉換語音 （也稱為 TTS 或語音合成） 直接將您的應用程式的使用者經驗。 語音可能是強固且愉悅的方式，與您的應用程式、 互補的或甚至取代，鍵盤、 滑鼠、 觸控及手勢互動的人員。

如需不同的方式，以用於 Windows 應用程式中的語音互動的詳細資訊，請參閱[語音互動](/windows/uwp/design/input/speech-interactions)。

### <a name="windows-ai"></a>Windows AI

![Windows AI](images/windows-ai.png)

我們提供數個不同的 AI 解決方案，可用來增強您的 Windows 應用程式。 與 Windows Machine Learning，您可以將定型的機器學習服務模型，您的應用程式整合，並在裝置上進行本機執行。 Windows 辨識技術可讓您使用預先建置的程式庫來完成常見的映像處理工作，或建立您自己自訂的解決方案。 DirectML 提供低階，DirectX 樣式 Api，可讓您充分利用的硬體。

如需有關在 Windows 應用程式中整合 AI 的不同方式的詳細資訊，請參閱[Windows AI](https://docs.microsoft.com/windows/ai/)。

## <a name="features-and-technologies-by-platform"></a>功能與技術平台

下列各節會提供有用的連結，以深入了解如何整合與核心 Windows 功能和技術，從我們的主要應用程式平台：UWP、 Win32 (Windows API)、 WPF 和 Windows Form。

### <a name="user-interface-and-accessibility"></a>使用者介面和協助工具

|  UWP  |  Win32 (Windows API) |  WPF  |  Windows Forms  |
|-------|----------------------|-------|-----------------|
| [設計](/windows/uwp/design/basics/)<br/><br/>[版面配置](/windows/uwp/design/layout/)<br/><br/>[控制項](/windows/uwp/design/controls-and-patterns/)<br/><br/>[輸入](/windows/uwp/design/input/)<br/><br/>[磚](/windows/uwp/design/shell/tiles-and-notifications/creating-tiles)<br/><br/>[視覺層](/windows/uwp/composition/visual-layer)<br/><br/>[XAML 平台](/windows/uwp/xaml-platform/)<br/><br/>[啟動、繼續和背景工作](/windows/uwp/launch-resume/)<br/><br/>[Windows 協助工具](/windows/uwp/design/accessibility/accessibility)<br/><br/>  |  [桌面的使用者介面](/windows/desktop/windows-application-ui-development)<br/><br/>[桌面環境和 shell](/windows/desktop/user-interface)<br/><br/>[windows 控制項](/windows/desktop/controls/window-controls)<br/><br/>[UWP 控制項，在傳統型應用程式 （XAML 群島）](/windows/apps/desktop/modernize/xaml-islands)<br/><br/>[桌面應用程式中的 UWP 視覺圖層](/windows/apps/desktop/modernize/visual-layer-in-desktop-apps)<br/><br/>[Windows 和訊息](/windows/desktop/winmsg/windowing)<br/><br/>[功能表和其他資源](/windows/desktop/menurc/resources)<br/><br/>[高 DPI](/windows/desktop/hidpi/high-dpi-desktop-application-development-on-windows)<br/><br/>[協助工具](/windows/desktop/accessibility)<br/><br/>  |  [在 WPF 中的 Windows](https://docs.microsoft.com/dotnet/framework/wpf/app-development/windows-in-wpf-applications)<br/><br/>[瀏覽概觀](https://docs.microsoft.com/dotnet/framework/wpf/app-development/navigation-overview)<br/><br/>[在 WPF 中的 XAML](https://docs.microsoft.com/dotnet/framework/wpf/advanced/xaml-in-wpf)<br/><br/>[控制項](https://docs.microsoft.com/dotnet/framework/wpf/controls/)<br/><br/>[視覺分層程式設計](https://docs.microsoft.com/dotnet/framework/wpf/graphics-multimedia/visual-layer-programming)<br/><br/>[輸入](https://docs.microsoft.com/dotnet/framework/wpf/advanced/input-wpf)<br/><br/>[協助工具](https://docs.microsoft.com/dotnet/framework/ui-automation/)<br/><br/>  | [建立 Windows Form](https://docs.microsoft.com/dotnet/framework/winforms/creating-a-new-windows-form)<br/><br/>[控制項](https://docs.microsoft.com/dotnet/framework/winforms/controls/)<br/><br/>[對話方塊](https://docs.microsoft.com/dotnet/framework/winforms/dialog-boxes-in-windows-forms)<br/><br/>[使用者輸入](https://docs.microsoft.com/dotnet/framework/winforms/user-input-in-windows-forms)<br/><br/>[Windows Forms 協助工具](https://docs.microsoft.com/dotnet/framework/winforms/advanced/windows-forms-accessibility)<br/><br/> |

### <a name="audio-video-and-graphics"></a>音訊、 視訊和圖形

|  UWP  |  Win32 (Windows API) |  WPF  |  Windows Forms  |
|-------|----------------------|-------|-----------------|
| [音訊、視訊和相機](/windows/uwp/audio-video-camera/)<br/><br/>[媒體播放](/windows/uwp/audio-video-camera/media-playback/)<br/><br/>[視覺層](/windows/uwp/composition/visual-layer)<br/><br/>[XAML 平台](/windows/uwp/xaml-platform/) |  [音訊與視訊](/windows/desktop/audio-and-video)<br/><br/>[圖形和遊戲](/windows/desktop/graphics-and-multimedia)<br/><br/>[DirectX](/windows/desktop/getting-started-with-directx-graphics)<br/><br/>[Direct2D](/windows/desktop/direct2d/direct2d-portal)<br/><br/>[Direct3D](/windows/desktop/direct3d)<br/><br/>[Windows GDI](/windows/desktop/gdi/windows-gdi)<br/><br/>[GDI+](/windows/desktop/gdiplus/-gdiplus-gdi-start)  |  [圖形](https://docs.microsoft.com/dotnet/framework/wpf/graphics-multimedia/graphics)<br/><br/>[Mutimedia](https://docs.microsoft.com/dotnet/framework/wpf/graphics-multimedia/multimedia-overview)  |  [圖形和繪圖](https://docs.microsoft.com/dotnet/framework/winforms/advanced/graphics-and-drawing-in-windows-forms?view=netframework-4.8)<br/><br/>[SoundPlayer 類別](https://docs.microsoft.com/dotnet/framework/winforms/controls/soundplayer-class-overview)  |

### <a name="data-access-and-app-resources"></a>資料存取和應用程式資源

|  UWP  |  Win32 (Windows API) |  WPF  |  Windows Forms  |
|-------|----------------------|-------|-----------------|
| [資料存取](/windows/uwp/data-access/)<br/><br/>[資料繫結](/windows/uwp/data-binding/)<br/><br/>[檔案、資料夾和媒體櫃](/windows/uwp/files/)<br/><br/>[應用程式資源](/windows/uwp/app-resources/) |  [資料存取和儲存體](/windows/desktop/data-access-and-storage)<br/><br/>[本機檔案系統](/windows/desktop/fileio/file-systems)<br/><br/>[資源概觀](/windows/desktop/menurc/resources-overviews)</li>  |  [資料與模型化](https://docs.microsoft.com/dotnet/framework/data/)<br/><br/>[資料繫結](https://docs.microsoft.com/dotnet/framework/wpf/data/data-binding-wpf)<br/><br/>[.NET 應用程式中的資源](https://docs.microsoft.com/dotnet/framework/resources/?view=netframework-4.8)<br/><br/>[應用程式資源、 內容和資料檔案](https://docs.microsoft.com/dotnet/framework/wpf/app-development/wpf-application-resource-content-and-data-files)  |  [資料與模型化](https://docs.microsoft.com/dotnet/framework/data/)<br/><br/>[資料繫結](https://docs.microsoft.com/dotnet/framework/winforms/windows-forms-data-binding)<br/><br/>[.NET 應用程式中的資源](https://docs.microsoft.com/dotnet/framework/resources/?view=netframework-4.8)<br/><br/>[應用程式設定](https://docs.microsoft.com/dotnet/framework/winforms/advanced/application-settings-for-windows-forms)  |

### <a name="devices-documents-and-printing"></a>裝置、 文件和列印

|  UWP  |  Win32 (Windows API) |  WPF  |  Windows Forms  |
|-------|----------------------|-------|-----------------|
| [啟用裝置功能](/windows/uwp/devices-sensors/enable-device-capabilities)<br/><br/>[列舉裝置](/windows/uwp/devices-sensors/enumerate-devices)<br/><br/>[感應器](/windows/uwp/devices-sensors/sensors)<br/><br/>[藍牙](/windows/uwp/devices-sensors/bluetooth)<br/><br/>[列印與掃描](/windows/uwp/devices-sensors/printing-and-scanning)<br/><br/>[NFC](/windows/uwp/devices-sensors/nfc) | [感應器 API](/windows/desktop/sensorsapi/portal)<br/><br/>[列印](/desktop/printdocs/printdocs-printing)<br/><br/>[UPnP Api](/desktop/upnp/universal-plug-and-play-start-page) |  [列印和列印系統管理](https://docs.microsoft.com/dotnet/framework/wpf/advanced/printing-and-print-system-management)  |  [列印支援](https://docs.microsoft.com/dotnet/framework/winforms/advanced/windows-forms-print-support)  |

### <a name="system-network-and-power"></a>系統、 網路和電源

|  UWP  |  Win32 (Windows API) |  WPF  |  Windows Forms  |
|-------|----------------------|-------|-----------------|
| [列舉裝置](/windows/uwp/devices-sensors/enumerate-devices)<br/><br/>[取得電池資訊](/windows/uwp/devices-sensors/get-battery-info)<br/><br/>[執行緒和非同步程式設計](/windows/uwp/threading-async/)<br/><br/>[網路和 Web 服務](/windows/uwp/networking/) | [系統服務](/windows/desktop/system-services)<br/><br/>[記憶體管理](/windows/desktop/memory/memory-management)<br/><br/>[電源管理](/windows/desktop/power/power-management-portal)<br/><br/>[處理序和執行緒](/windows/desktop/procthread/processes-and-threads)<br/><br/>[網路和網際網路](/windows/desktop/networking)<br/><br/>[Windows 系統資訊](/windows/desktop/sysinfo/windows-system-information) |  [執行緒模型](https://docs.microsoft.com/dotnet/framework/wpf/advanced/threading-model)<br/><br/>[以.NET Framework 進行網路程式設計](https://docs.microsoft.com/dotnet/framework/network-programming/)  |  [系統資訊](https://docs.microsoft.com/dotnet/framework/winforms/advanced/system-information-and-windows-forms)<br/><br/>[電源管理](https://docs.microsoft.com/dotnet/framework/winforms/advanced/power-management-in-windows-forms)<br/><br/>[以.NET Framework 進行網路程式設計](https://docs.microsoft.com/dotnet/framework/network-programming/)<br/><br/>[Windows Forms 中的網路](https://docs.microsoft.com/dotnet/framework/winforms/advanced/networking-in-windows-forms-applications)  |

### <a name="packaging-and-deployment"></a>封裝和部署

|  UWP  |  Win32 (Windows API) |  WPF  |  Windows Forms  |
|-------|----------------------|-------|-----------------|
| [封裝應用程式](/windows/uwp/packaging/)<br/><br/>[MSIX](https://docs.microsoft.com/windows/msix/)<br/><br/>[應用程式封裝資訊清單結構描述](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/schema-root) | [封裝 Windows 傳統型應用程式 (MSIX)](/windows/msix/desktop/desktop-to-uwp-root)<br/><br/>[應用程式安裝和服務](/windows/desktop/application-installing-and-servicing)<br/><br/>[Windows Installer](/windows/desktop/msi/windows-installer-portal) |  [封裝 Windows 傳統型應用程式 (MSIX)](/windows/msix/desktop/desktop-to-uwp-root)<br/><br/>[部署.NET Framework 和應用程式](https://docs.microsoft.com/dotnet/framework/deployment/)<br/><br/>[部署 WPF 應用程式](https://docs.microsoft.com/dotnet/framework/wpf/app-development/deploying-a-wpf-application-wpf)  |  [封裝 Windows 傳統型應用程式 (MSIX)](/windows/msix/desktop/desktop-to-uwp-root)<br/><br/>[部署.NET Framework 和應用程式](https://docs.microsoft.com/dotnet/framework/deployment/)<br/><br/>[Windows Form 的 ClickOnce 部署](https://docs.microsoft.com/dotnet/framework/winforms/clickonce-deployment-for-windows-forms)  |
