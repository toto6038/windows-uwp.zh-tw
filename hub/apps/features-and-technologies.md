---
Description: 本節協助您了解如何在不同應用程式平台中支援特定重要 Windows 功能，以及如何開始在您的程式碼中使用這些功能。
title: 功能和技術
ms.topic: article
ms.date: 05/08/2019
ms.localizationpriority: medium
ms.author: mcleans
author: mcleanbyron
ms.openlocfilehash: ac779bf57e51b13051fa25293606daab05540fd1
ms.sourcegitcommit: 8a88a05ad89aa180d41a93152632413694f14ef8
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2020
ms.locfileid: "76726021"
---
# <a name="features-and-technologies-for-windows-apps"></a>Windows 應用程式的功能和技術

無論您正在建置何種類型的應用程式，或是以哪個裝置作為目標，Windows 都會支援許多功能，而這些功能正是重要應用程式案例的關鍵建置組塊。 其中一些功能會以不同的方式顯示在通用 Windows 平台 (UWP)、Win32 (Windows API) 和其他應用程式平台中。 下列文章協助您了解如何在不同應用程式平台中支援特定 Windows 功能，以及如何開始在您的程式碼中使用這些功能。

本文提供量身打造的文章清單，以深入閱讀如何存取 UWP、Win32 (Windows API)、WPF 和 Windows Forms 應用程式平台中的重要 Windows 功能與技術。 如需每個平台開發功能的完整資訊，請參閱下列資源：

* [UWP 文件](/windows/uwp/index)
* [Win32 (Windows API) 文件](/windows/desktop/index)
* [WPF 文件](https://docs.microsoft.com/dotnet/framework/wpf/index)
* [Windows Forms 文件](https://docs.microsoft.com/dotnet/framework/winforms/index)

## <a name="key-windows-features-and-technologies"></a>重要的 Windows 功能和技術

下列各節強調幾個重要的 Windows 功能和技術，可讓您為客戶提供現代化和引人注目的體驗。

### <a name="windows-ink"></a>Windows Ink

![Surface 手寫筆](images/hero-small.png)  

Windows Ink 平台搭配手寫筆裝置之後，使用者就可以自然的方式建立數位手寫筆記、繪圖以及註解。 此平台支援擷取數位板輸入做為筆墨資料、產生筆墨資料、管理筆墨資料、在輸出裝置上將筆墨資料轉譯為筆劃，以及透過手寫辨識將筆墨轉換為文字。

如需在 Windows 應用程式中以不同方式使用 Windows Ink 的詳細資訊，請參閱 [Windows Ink](/windows/uwp/design/input/pen-and-stylus-interactions)。

### <a name="speech-interactions"></a>語音互動

![以 SGRS 文法檔為基礎之限制的初始辨識畫面](images/speech-listening-initial.png)

![以 SGRS 文法檔為基礎之限制的最終辨識畫面](images/speech-listening-complete.png)

Windows 提供多種方式，將語音辨識和文字轉換語音 (也稱為 TTS，或語音合成) 直接整合到應用程式的使用者體驗。 語音可以成為使用者與您應用程式互動的可靠又有趣方式，可補充或甚至取代鍵盤、滑鼠、觸控和手勢。

如需在 Windows 應用程式中以不同方式使用語音互動的詳細資訊，請參閱 [Windows 10 中的語音 (Speech)、語音 (Voice) 和交談](speech.md)。

### <a name="windows-ai"></a>Windows AI

![Windows AI](images/windows-ai.png)

我們提供了數種不同的 AI 解決方案，可讓您用來增強 Windows 應用程式。 透過 Windows Machine Learning，您可以將定型的機器學習模型整合至您的應用程式，並在本機裝置上執行。 Windows Vision Skill 可讓您使用預先建置的程式庫來完成一般影像處理工作，或建立您自己的自訂解決方案。 DirectML 提供低階 DirectX 樣式 API，可讓您充分利用硬體。

如需在 Windows 應用程式中以不同方式整合 AI 的詳細資訊，請參閱 [Windows AI](https://docs.microsoft.com/windows/ai/)。

## <a name="features-and-technologies-by-platform"></a>依平台的功能和技術

下列各節提供有用的連結，以深入了解如何與主要應用程式平台中的核心 Windows 功能和技術整合：UWP、Win32 (Windows API)、WPF 和 Windows Forms。

### <a name="user-interface-and-accessibility"></a>使用者介面和協助工具

|  UWP  |  Win32 (Windows API) |  WPF  |  Windows Forms  |
|-------|----------------------|-------|-----------------|
| [設計](/windows/uwp/design/basics/)<br/><br/>[配置](/windows/uwp/design/layout/)<br/><br/>[控制項](/windows/uwp/design/controls-and-patterns/)<br/><br/>[輸入](/windows/uwp/design/input/)<br/><br/>[磚](/windows/uwp/design/shell/tiles-and-notifications/creating-tiles)<br/><br/>[視覺層](/windows/uwp/composition/visual-layer)<br/><br/>[XAML 平台](/windows/uwp/xaml-platform/)<br/><br/>[啟動、繼續和背景工作](/windows/uwp/launch-resume/)<br/><br/>[Windows 協助工具](/windows/uwp/design/accessibility/accessibility)<br/><br/>[語音互動](https://docs.microsoft.com/windows/uwp/design/input/speech-interactions)<br/><br/> |  [傳統型使用者介面](/windows/desktop/windows-application-ui-development)<br/><br/>[傳統型環境和命令介面](/windows/desktop/user-interface)<br/><br/>[Windows 控制項](/windows/desktop/controls/window-controls)<br/><br/>[傳統型應用程式中的 UWP 控制項 (XAML Islands)](/windows/apps/desktop/modernize/xaml-islands)<br/><br/>[傳統型應用程式中的 UWP 視覺層](/windows/apps/desktop/modernize/visual-layer-in-desktop-apps)<br/><br/>[Windows 和訊息](/windows/desktop/winmsg/windowing)<br/><br/>[功能表和其他資源](/windows/desktop/menurc/resources)<br/><br/>[高 DPI](/windows/desktop/hidpi/high-dpi-desktop-application-development-on-windows)<br/><br/>[協助工具](/windows/desktop/accessibility)<br/><br/>[Microsoft 語音平台 - 軟體開發套件 (SDK) (版本 11)](https://www.microsoft.com/download/details.aspx?id=27226)<br/><br/>[Microsoft 語音 SDK，版本 5.1](https://www.microsoft.com/download/details.aspx?id=10121)<br/><br/>  |  [WPF 中的視窗](https://docs.microsoft.com/dotnet/framework/wpf/app-development/windows-in-wpf-applications)<br/><br/>[瀏覽概觀](https://docs.microsoft.com/dotnet/framework/wpf/app-development/navigation-overview)<br/><br/>[WPF 中的 XAML](https://docs.microsoft.com/dotnet/framework/wpf/advanced/xaml-in-wpf)<br/><br/>[控制項](https://docs.microsoft.com/dotnet/framework/wpf/controls/)<br/><br/>[視覺層程式設計](https://docs.microsoft.com/dotnet/framework/wpf/graphics-multimedia/visual-layer-programming)<br/><br/>[輸入](https://docs.microsoft.com/dotnet/framework/wpf/advanced/input-wpf)<br/><br/>[協助工具](https://docs.microsoft.com/dotnet/framework/ui-automation/)<br/><br/>[適用於 .NET Framework 的 System.Speech 程式設計指南](https://docs.microsoft.com/previous-versions/office/developer/speech-technologies/hh361625(v=office.14))<br/><br/>  | [建立 Windows Form](https://docs.microsoft.com/dotnet/framework/winforms/creating-a-new-windows-form)<br/><br/>[控制項](https://docs.microsoft.com/dotnet/framework/winforms/controls/)<br/><br/>[對話方塊](https://docs.microsoft.com/dotnet/framework/winforms/dialog-boxes-in-windows-forms)<br/><br/>[使用者輸入](https://docs.microsoft.com/dotnet/framework/winforms/user-input-in-windows-forms)<br/><br/>[Windows Forms 協助工具](https://docs.microsoft.com/dotnet/framework/winforms/advanced/windows-forms-accessibility)<br/><br/>[適用於 .NET Framework 的 System.Speech 程式設計指南](https://docs.microsoft.com/previous-versions/office/developer/speech-technologies/hh361625(v=office.14))<br/><br/> |

### <a name="audio-video-and-graphics"></a>音訊、視訊和圖形

|  UWP  |  Win32 (Windows API) |  WPF  |  Windows Forms  |
|-------|----------------------|-------|-----------------|
| [音訊、視訊和相機](/windows/uwp/audio-video-camera/)<br/><br/>[媒體播放](/windows/uwp/audio-video-camera/media-playback/)<br/><br/>[視覺層](/windows/uwp/composition/visual-layer)<br/><br/>[XAML 平台](/windows/uwp/xaml-platform/) |  [音訊與視訊](/windows/desktop/audio-and-video)<br/><br/>[圖形和遊戲](/windows/desktop/graphics-and-multimedia)<br/><br/>[DirectX](/windows/desktop/getting-started-with-directx-graphics)<br/><br/>[Direct2D](/windows/desktop/direct2d/direct2d-portal)<br/><br/>[Direct3D](/windows/desktop/direct3d)<br/><br/>[Windows GDI](/windows/desktop/gdi/windows-gdi)<br/><br/>[GDI+](/windows/desktop/gdiplus/-gdiplus-gdi-start)  |  [圖形](https://docs.microsoft.com/dotnet/framework/wpf/graphics-multimedia/graphics)<br/><br/>[多媒體](https://docs.microsoft.com/dotnet/framework/wpf/graphics-multimedia/multimedia-overview)  |  [圖形和繪圖](https://docs.microsoft.com/dotnet/framework/winforms/advanced/graphics-and-drawing-in-windows-forms)<br/><br/>[SoundPlayer 類別](https://docs.microsoft.com/dotnet/framework/winforms/controls/soundplayer-class-overview)  |

### <a name="data-access-and-app-resources"></a>資料存取和應用程式資源

|  UWP  |  Win32 (Windows API) |  WPF  |  Windows Forms  |
|-------|----------------------|-------|-----------------|
| [資料存取](/windows/uwp/data-access/)<br/><br/>[資料繫結](/windows/uwp/data-binding/)<br/><br/>[檔案、資料夾和媒體櫃](/windows/uwp/files/)<br/><br/>[應用程式資源](/windows/uwp/app-resources/) |  [資料存取與儲存](/windows/desktop/data-access-and-storage)<br/><br/>[本機檔案系統](/windows/desktop/fileio/file-systems)<br/><br/>[資源概觀](/windows/desktop/menurc/resources-overviews)</li>  |  [資料與模型化](https://docs.microsoft.com/dotnet/framework/data/)<br/><br/>[資料繫結](https://docs.microsoft.com/dotnet/framework/wpf/data/data-binding-wpf)<br/><br/>[.NET 應用程式中的資源](https://docs.microsoft.com/dotnet/framework/resources/)<br/><br/>[應用程式資源、內容和資料檔案](https://docs.microsoft.com/dotnet/framework/wpf/app-development/wpf-application-resource-content-and-data-files)  |  [資料與模型化](https://docs.microsoft.com/dotnet/framework/data/)<br/><br/>[資料繫結](https://docs.microsoft.com/dotnet/framework/winforms/windows-forms-data-binding)<br/><br/>[.NET 應用程式中的資源](https://docs.microsoft.com/dotnet/framework/resources/)<br/><br/>[應用程式設定](https://docs.microsoft.com/dotnet/framework/winforms/advanced/application-settings-for-windows-forms)  |

### <a name="devices-documents-and-printing"></a>裝置、文件和列印

|  UWP  |  Win32 (Windows API) |  WPF  |  Windows Forms  |
|-------|----------------------|-------|-----------------|
| [啟用裝置功能](/windows/uwp/devices-sensors/enable-device-capabilities)<br/><br/>[列舉裝置](/windows/uwp/devices-sensors/enumerate-devices)<br/><br/>[感應器](/windows/uwp/devices-sensors/sensors)<br/><br/>[Bluetooth](/windows/uwp/devices-sensors/bluetooth)<br/><br/>[列印與掃描](/windows/uwp/devices-sensors/printing-and-scanning)<br/><br/>[NFC](/windows/uwp/devices-sensors/nfc) | [感應器 API](/windows/desktop/sensorsapi/portal)<br/><br/>[列印](/desktop/printdocs/printdocs-printing)<br/><br/>[UPnP API](/desktop/upnp/universal-plug-and-play-start-page) |  [列印和列印系統管理](https://docs.microsoft.com/dotnet/framework/wpf/advanced/printing-and-print-system-management)  |  [列印支援](https://docs.microsoft.com/dotnet/framework/winforms/advanced/windows-forms-print-support)  |

### <a name="system-network-and-power"></a>系統、網路和電源

|  UWP  |  Win32 (Windows API) |  WPF  |  Windows Forms  |
|-------|----------------------|-------|-----------------|
| [列舉裝置](/windows/uwp/devices-sensors/enumerate-devices)<br/><br/>[取得電池資訊](/windows/uwp/devices-sensors/get-battery-info)<br/><br/>[執行緒和非同步程式設計](/windows/uwp/threading-async/)<br/><br/>[網路和 Web 服務](/windows/uwp/networking/) | [系統服務](/windows/desktop/system-services)<br/><br/>[記憶體管理](/windows/desktop/memory/memory-management)<br/><br/>[電源管理](/windows/desktop/power/power-management-portal)<br/><br/>[處理序和執行緒](/windows/desktop/procthread/processes-and-threads)<br/><br/>[網路功能和網際網路](/windows/desktop/networking)<br/><br/>[Windows 系統資訊](/windows/desktop/sysinfo/windows-system-information) |  [執行緒模型](https://docs.microsoft.com/dotnet/framework/wpf/advanced/threading-model)<br/><br/>[以 .NET Framework 進行網路程式設計](https://docs.microsoft.com/dotnet/framework/network-programming/)  |  [系統資訊](https://docs.microsoft.com/dotnet/framework/winforms/advanced/system-information-and-windows-forms)<br/><br/>[電源管理](https://docs.microsoft.com/dotnet/framework/winforms/advanced/power-management-in-windows-forms)<br/><br/>[以 .NET Framework 進行網路程式設計](https://docs.microsoft.com/dotnet/framework/network-programming/)<br/><br/>[Windows Forms 中的網路功能](https://docs.microsoft.com/dotnet/framework/winforms/advanced/networking-in-windows-forms-applications)  |

### <a name="packaging-and-deployment"></a>封裝和部署

|  UWP  |  Win32 (Windows API) |  WPF  |  Windows Forms  |
|-------|----------------------|-------|-----------------|
| [封裝應用程式](/windows/uwp/packaging/)<br/><br/>[MSIX](https://docs.microsoft.com/windows/msix/)<br/><br/>[應用程式套件資訊清單結構描述](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/schema-root) | [封裝 Windows 傳統型應用程式 (MSIX)](/windows/msix/desktop/desktop-to-uwp-root)<br/><br/>[應用程式安裝和維護](/windows/desktop/application-installing-and-servicing)<br/><br/>[Windows Installer](/windows/desktop/msi/windows-installer-portal) |  [封裝 Windows 傳統型應用程式 (MSIX)](/windows/msix/desktop/desktop-to-uwp-root)<br/><br/>[部署 .NET Framework 和應用程式](https://docs.microsoft.com/dotnet/framework/deployment/)<br/><br/>[部署 WPF 應用程式](https://docs.microsoft.com/dotnet/framework/wpf/app-development/deploying-a-wpf-application-wpf)  |  [封裝 Windows 傳統型應用程式 (MSIX)](/windows/msix/desktop/desktop-to-uwp-root)<br/><br/>[部署 .NET Framework 和應用程式](https://docs.microsoft.com/dotnet/framework/deployment/)<br/><br/>[Windows Forms 的 ClickOnce 部署](https://docs.microsoft.com/dotnet/framework/winforms/clickonce-deployment-for-windows-forms)  |
