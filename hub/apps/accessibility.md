---
title: Windows 10 中的協助工具
description: 此頁面提供的資訊可讓您開始開發無障礙 Windows 應用程式。
ms.topic: article
ms.date: 09/12/2019
keywords: Windows 10 中的協助工具, 建置無障礙 win32 應用程式, 建置無障礙 UWP 應用程式, 建置無障礙 WPF 應用程式, 建置無障礙 WinForms 應用程式
ms.author: kbridge
author: Karl-Bridge-Microsoft
ms.openlocfilehash: 2739546061febcfc96403ed5520fdcbd8bc2c462
ms.sourcegitcommit: 8a88a05ad89aa180d41a93152632413694f14ef8
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2020
ms.locfileid: "76725931"
---
# <a name="accessibility-in-windows-10"></a>Windows 10 中的協助工具

![協助工具主角圖像](images/hero-accessibility-bar-smaller.png)

## <a name="build-accessibility-into-your-applications-to-empower-people-of-all-abilities"></a>在您的應用程式中建置協助工具，賦予人員各種功能

當產品和服務 (包括電子媒體) 設計為盡可能提供最多人的完整和成功體驗時，就可以進行存取。

建置無障礙和內含的 Windows 應用程式，其具有針對殘障人士 (暫時性和永久) 改善的功能和使用性、個人喜好設定、特定工作樣式或環境限制 (例如共用的工作空間、駕駛、烹飪、反光等等)。 某些常見的解決方案包括以替代格式 (例如影片中的字幕) 來提供資訊，或啟用輔助技術 (例如螢幕助讀程式) 的使用。

**每個人都應該可以存取大樓中相同的房間，不論他們是否需要使用階梯或電梯。**

本頁面提供相關資訊，說明各種 Windows 開發架構如何為建置 Windows 應用程式的開發人員、建置螢幕助讀程式和放大鏡這類工具的輔助技術開發人員，以及建立自動化指令碼來測試應用程式的軟體測試工程師提供協助工具支援。

## <a name="platform-specific-documentation"></a>特定平台的文件

:::row:::
   :::column:::
      ![通用 Windows 平台 (UWP)](images/platform-uwp.png)

      **通用 Windows 平台 (UWP)**

      在任何裝置 (包括電腦、手機、Xbox One、HoloLens 等等) 上，於適用於 Windows 10 應用程式和遊戲 的新式平台上，開發無障礙的應用程式和工具，並將其發佈至 Microsoft Store。

      [設計全人軟體](https://docs.microsoft.com/windows/uwp/accessibility/designing-inclusive-software)

      [開發全人 Windows 應用程式](https://docs.microsoft.com/windows/uwp/accessibility/developing-inclusive-windows-apps)

      [協助工具測試](https://docs.microsoft.com/windows/uwp/accessibility/accessibility-testing)

      [Store 中的協助工具](https://docs.microsoft.com/windows/uwp/accessibility/accessibility-in-the-store)
   :::column-end:::
   :::column:::
      ![Win32 平台應用程式](images/platform-win32.png)

      **Win32 平台**

      在適用於 C/C++ Windows 應用程式的原始平台上，開發無障礙的應用程式和工具。

      [Windows 協助工具和自動化的新增功能](https://docs.microsoft.com/windows/desktop/accessibility-whatsnew)

      [開發適用於 Windows 的無障礙應用程式](https://docs.microsoft.com/windows/desktop/accessibility-appdev)

      [開發適用於 Windows 的無障礙 UI 架構](https://docs.microsoft.com/windows/desktop/accessibility-uiframeworkdev)

      [開發 Windows 的輔助技術](https://docs.microsoft.com/windows/desktop/accessibility-atdev)

      [測試協助工具](https://docs.microsoft.com/windows/desktop/accessibility-testwithuia)

      [舊版協助工具和自動化技術 - MSAA 至 UI 自動化](https://docs.microsoft.com/windows/desktop/accessibility-legacy)

      [Windows 協助工具功能](https://docs.microsoft.com/windows/desktop/winauto/about-windows-accessibility-features)

      [設計協助工具應用程式的指導方針](https://docs.microsoft.com/windows/desktop/uxguide/inter-accessibility)
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      ![WPF 平台](images/platform-wpf2-small.png)

      **Windows Presentation Foundation (WPF)**

      在針對受管理 Windows 應用程式建立的平台上，利用 XAML UI 模型和 .NET Framework 開發無障礙的應用程式和工具。

      [協助工具最佳作法](https://docs.microsoft.com/dotnet/framework/ui-automation/accessibility-best-practices)

      [UI 自動化基礎](https://docs.microsoft.com/dotnet/framework/ui-automation/index)

      [適用於 Managed 程式碼的 UI 自動化提供者](https://docs.microsoft.com/dotnet/framework/ui-automation/ui-automation-providers-for-managed-code)

      [適用於 Managed 程式碼的 UI 自動化用戶端](https://docs.microsoft.com/dotnet/framework/ui-automation/ui-automation-clients-for-managed-code)

      [UI 自動化控制項模式](https://docs.microsoft.com/dotnet/framework/ui-automation/ui-automation-control-patterns)

      [UI 自動化文字模式](https://docs.microsoft.com/dotnet/framework/ui-automation/ui-automation-text-pattern)

      [UI 自動化控制項類型](https://docs.microsoft.com/dotnet/framework/ui-automation/ui-automation-control-types)

      [ 自動化規格和社群承諾](https://docs.microsoft.com/dotnet/framework/ui-automation/ui-automation-specification-and-community-promise)
   :::column-end:::
   :::column:::
      ![Windows Forms 平台應用程式](images/platform-winforms.png)

      **Windows Forms (WinForms)**

      利用 XAML UI 模型和 .NET Framework，為受管理 Windows 應用程式開發無障礙的應用程式和工具。

      [Windows Forms 協助工具](https://docs.microsoft.com/dotnet/framework/winforms/advanced/windows-forms-accessibility)

      [建立無障礙 Windows 應用程式](https://docs.microsoft.com/dotnet/framework/winforms/advanced/walkthrough-creating-an-accessible-windows-based-application)

      [Windows Form 控制項上支援協助工具指導方針的屬性](https://docs.microsoft.com/dotnet/framework/winforms/advanced/properties-on-windows-forms-controls-that-support-accessibility-guidelines)

      [為 Windows Form 上的控制項提供協助工具資訊](https://docs.microsoft.com/dotnet/framework/winforms/controls/providing-accessibility-information-for-controls-on-a-windows-form)
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="2":::
      **Web 協助工具**

      設計、建置及測試 Microsoft Edge 中的無障礙網站。
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      [Web 協助工具簡介](https://docs.microsoft.com/microsoft-edge/accessibility)

      [設計無障礙網站](https://docs.microsoft.com/microsoft-edge/accessibility/design)
   :::column-end:::
   :::column:::
      [建置無障礙網站](https://docs.microsoft.com/microsoft-edge/accessibility/build)

      [測試無障礙網站](https://docs.microsoft.com/microsoft-edge/accessibility/test)
   :::column-end:::
:::row-end:::

## <a name="samples"></a>範例

下載並執行示範各種協助工具特性和功能的完整 Windows 範例。

:::row:::
   :::column:::
      [程式碼範例瀏覽器](https://docs.microsoft.com/samples/browse/)

      取代 MSDN 程式碼庫的新範例瀏覽器。
   :::column-end:::
   :::column:::
      [MSDN 程式碼庫 (已淘汰)](https://code.msdn.microsoft.com/site/search?query=accessibility&f%5B0%5D.Value=accessibility&f%5B0%5D.Type=SearchText&ac=2)

      下載適用於 Windows、Windows Phone、Microsoft Azure、Office、SharePoint、Silverlight 和其他產品的範例。
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      [GitHub 上的 Windows 傳統範例](https://github.com/microsoft/Windows-classic-samples/search?q=accessibility&unscoped_q=accessibility)

      這些範例示範 Windows 和 Windows Server 的功能和程式設計模型。 
   :::column-end:::
   :::column:::
      [GitHub 上的通用 Windows 平台 (UWP) 範例](https://github.com/microsoft/Windows-universal-samples/search?q=accessibility&unscoped_q=accessibility)

      這些範例示範適用於 Windows 10 的 Windows 軟體開發套件 (SDK) 中通用 Windows 平台 (UWP) 的 API 使用模式。
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="2":::
      [XAML 控制項庫](https://github.com/microsoft/Xaml-Controls-Gallery)

      此應用程式示範 Fluent Design 系統中支援的各種 Xaml 控制項。
   :::column-end:::
:::row-end:::

## <a name="videos"></a>視訊

有各種影片，涵蓋如何因應一般協助工具考量建置無障礙 Windows 應用程式，以及 Microsoft 如何處理這些考量。

:::row:::
   :::column:::
      **如何開始使用 Windows 應用程式中的協助工具**
   :::column-end:::
   :::column:::
      **開發人員短片：開發無障礙應用程式**
   :::column-end:::
   :::column:::
      **身心障礙人士和協助工具簡介**
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      > [!VIDEO https://channel9.msdn.com/Events/Build/2017/P4072/player]
   :::column-end:::
   :::column:::
      > [!VIDEO https://channel9.msdn.com/Blogs/One-Dev-Minute/Developing-Apps-for-Accessibility/player]
   :::column-end:::
   :::column:::
      > [!VIDEO https://www.youtube.com/embed/Kl4CT4DaypM]
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      **從破解到產品，以眼球控制 Windows 10**
   :::column-end:::
   :::column:::
      **Windows 10 上的協助工具**
   :::column-end:::
   :::column:::
      **簡介建置無障礙 UWP 應用程式**
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      > [!VIDEO https://www.youtube.com/embed/AShNPfmAkvY]
   :::column-end:::
   :::column:::
      > [!VIDEO https://channel9.msdn.com/Events/Build/2016/P541/player]
   :::column-end:::
   :::column:::
      > [!VIDEO https://channel9.msdn.com/Events/Build/2016/P497/player]
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      **設計 Windows 協助工具功能**
   :::column-end:::
   :::column:::
      **Windows 10 協助工具功能讓每個人都能使用 Windows 10**
   :::column-end:::
   :::column:::
      **使滑鼠指標更容易看到**
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      > [!VIDEO https://www.youtube.com/embed/Y_NJbE7wxlk]
   :::column-end:::
   :::column:::
      > [!VIDEO https://www.youtube.com/embed/BseTf-4q9GA]
   :::column-end:::
   :::column:::
      > [!VIDEO https://www.youtube.com/embed/4UzaF7_T3bw]
   :::column-end:::
:::row-end:::

## <a name="other-resources"></a>其他資源

:::row:::
   :::column span="3":::
      **部落格和新聞**

      來自 Microsoft 協助工具世界的最新消息。
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      [在新聞中](https://news.microsoft.com/presskits/accessibility/)
   :::column-end:::
   :::column:::
      [協助工具部落格](https://blogs.microsoft.com/accessibility/)
   :::column-end:::
   :::column:::
      [Windows UI 自動化部落格](https://blogs.msdn.microsoft.com/winuiautomation/)
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="3":::
      **社群與支援**

      Windows 開發人員與使用者碰面和一起學習的地方。
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      [Windows 社群 - 協助工具](https://community.windows.com/search?q=accessibility)
   :::column-end:::
   :::column:::
      [Windows 協助工具和自動化開發論壇](https://social.msdn.microsoft.com/Forums/windows/home?forum=windowsaccessibilityandautomation)
   :::column-end:::
   :::column:::
      [身心障礙人士 Answer Desk](https://www.microsoft.com/Accessibility/disability-answer-desk)
   :::column-end:::
:::row-end:::
