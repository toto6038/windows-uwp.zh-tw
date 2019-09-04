---
title: Windows 10 中的協助工具
description: 本頁面提供的資訊可讓您開始開發可存取的 Windows 應用程式。
ms.topic: article
ms.date: 04/03/2019
ms.localizationpriority: medium
ms.author: kbridge
author: Karl-Bridge-Microsoft
keywords: Windows 10 中的協助工具、建立可存取的 win32 應用程式、建立可存取的 UWP 應用程式、建立可存取的 WPF 應用程式、建立可存取的 WinForms apps
ms.openlocfilehash: 63d9b321f71019c164fe4de238a618f2730b2052
ms.sourcegitcommit: 81213db9de2c12400d2e4d176346b466ee0794c2
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/03/2019
ms.locfileid: "70237785"
---
# <a name="accessibility-in-windows-10"></a>Windows 10 中的協助工具

![hero-accessibility-bar-smaller .png](images/hero-accessibility-bar-smaller.png)

可存取的應用程式的設計目的, 是要藉由改善最多人的可用性, 包括殘障、個人喜好設定、特定的工作樣式, 或環境條件約束 (例如駕駛、烹飪、防眩等等)。

此頁面提供各種 Windows 開發架構如何為建立 Windows 應用程式的開發人員支援協助工具的相關資訊、輔助技術開發人員建立螢幕閱讀程式和放大鏡等工具, 以及軟體測試建立自動化腳本以測試應用程式的工程師。

## <a name="platform-specific-documentation"></a>特定平台的文件

:::row:::
   :::column:::
      ![通用 Windows 平台 (UWP)](images/platform-uwp.png)

      **通用 Windows 平台 (UWP)**

      在適用于 Windows 10 應用程式和遊戲 (包括電腦、手機、Xbox One、HoloLens 等等) 的新式平臺上, 開發可存取的應用程式和工具, 並將其發佈至 Microsoft Store。

      [設計全人軟體](https://docs.microsoft.com/windows/uwp/accessibility/designing-inclusive-software)

      [開發全人 Windows 應用程式](https://docs.microsoft.com/windows/uwp/accessibility/developing-inclusive-windows-apps)

      [協助工具測試](https://docs.microsoft.com/windows/uwp/accessibility/accessibility-testing)

      [市集中的協助工具](https://docs.microsoft.com/windows/uwp/accessibility/accessibility-in-the-store)
   :::column-end:::
   :::column:::
      ![Win32 平臺應用程式](images/platform-win32.png)

      **Win32 平臺**

      在適用于 C/C++ Windows 應用程式的原始平臺上, 開發可存取的應用程式和工具。

      [Windows 協助工具和自動化的新功能](https://docs.microsoft.com/windows/desktop/accessibility-whatsnew)

      [開發適用于 Windows 的可存取應用程式](https://docs.microsoft.com/windows/desktop/accessibility-appdev)

      [開發適用于 Windows 的可存取 UI 架構](https://docs.microsoft.com/windows/desktop/accessibility-uiframeworkdev)

      [開發適用于 Windows 的輔助技術](https://docs.microsoft.com/windows/desktop/accessibility-atdev)

      [測試協助工具](https://docs.microsoft.com/windows/desktop/accessibility-testwithuia)

      [舊版協助工具和自動化技術-MSAA 至 UI 自動化](https://docs.microsoft.com/windows/desktop/accessibility-legacy)

      [Windows 協助工具功能](https://docs.microsoft.com/windows/desktop/winauto/about-windows-accessibility-features)

      [設計協助工具應用程式的指導方針](https://docs.microsoft.com/windows/desktop/uxguide/inter-accessibility)
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      ![WPF 平臺](images/platform-wpf.png)

      **Windows Presentation Foundation (WPF)**

      使用 XAML UI 模型和 .NET Framework, 在已建立的平臺上為受控 Windows 應用程式開發可存取的應用程式和工具。

      [協助工具最佳做法](https://docs.microsoft.com/dotnet/framework/ui-automation/accessibility-best-practices)

      [使用者介面自動化基礎觀念](https://docs.microsoft.com/dotnet/framework/ui-automation/index)

      [Managed 程式碼的 UI 自動化提供者](https://docs.microsoft.com/dotnet/framework/ui-automation/ui-automation-providers-for-managed-code)

      [Managed 程式碼的 UI 自動化用戶端](https://docs.microsoft.com/dotnet/framework/ui-automation/ui-automation-clients-for-managed-code)

      [UI 自動化控制項模式](https://docs.microsoft.com/dotnet/framework/ui-automation/ui-automation-control-patterns)

      [UI 自動化文字模式](https://docs.microsoft.com/dotnet/framework/ui-automation/ui-automation-text-pattern)

      [使用者介面自動化控制項類型](https://docs.microsoft.com/dotnet/framework/ui-automation/ui-automation-control-types)

      [使用者介面自動化規格和團體承諾](https://docs.microsoft.com/dotnet/framework/ui-automation/ui-automation-specification-and-community-promise)
      :::column-end:::
   :::column:::
      ![Windows Forms 平臺應用程式](images/platform-winforms.png)

      **Windows Forms (WinForms)**

      使用 XAML UI 模型和 .NET Framework, 為受控 Windows 應用程式開發可存取的應用程式和工具。

      [Windows Forms 協助工具](https://docs.microsoft.com/dotnet/framework/winforms/advanced/windows-forms-accessibility)

      [建立可存取的 Windows 應用程式](https://docs.microsoft.com/dotnet/framework/winforms/advanced/walkthrough-creating-an-accessible-windows-based-application)

      [支援協助工具方針之 Windows Forms 控制項的屬性](https://docs.microsoft.com/dotnet/framework/winforms/advanced/properties-on-windows-forms-controls-that-support-accessibility-guidelines)

      [提供 Windows Form 上控制項的協助工具資訊](https://docs.microsoft.com/dotnet/framework/winforms/controls/providing-accessibility-information-for-controls-on-a-windows-form)
   :::column-end:::
:::row-end:::
