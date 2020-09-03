---
description: 本文提供適用於 Windows 應用程式的 Visual Studio 專案和項目範本概觀。
title: 適用於 Windows 應用程式的 Visual Studio 專案和項目範本
ms.date: 07/02/2020
ms.topic: article
keywords: windows 10, uwp, windows forms, wpf, xaml islands
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: high
ms.openlocfilehash: 7e8500aab3c6eeaa1552dc61a95ea7404bf4d5d5
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174202"
---
# <a name="visual-studio-project-and-item-templates-for-windows-apps"></a>適用於 Windows 應用程式的 Visual Studio 專案和項目範本

Visual Studio 2019 提供許多專案和項目範本，協助您使用 C\# 或 C++ 來建置適用於 Windows 10 裝置的應用程式。 本主題說明範本，並協助您為您的案例選擇其中一個範本。

* 專案範本包括專案檔、程式碼檔案，以及其他資產，已設定來建置可以由應用程式載入或使用的應用程式或元件。
* 項目範本是包含常用程式碼和 XAML 的專案檔，可以新增至專案以縮短開發時間。 例如，您可以使用項目範本，將新的視窗、頁面或控制項新增至您的應用程式。

## <a name="winui-templates"></a>WinUI 範本

[Windows UI 程式庫 (WinUI)](../winui/index.md) 是適用於桌面 (.NET 和原生 Win32) 和 UWP 應用程式平台之間 Windows 應用程式的新型原生使用者介面 (UI) 平台。 [WinUI 3](../winui/winui3/index.md) (目前以開發人員預覽版形式提供) 是 WinUI 的最新主要版本，會將 WinUI 轉換成適用於桌面 Windows 應用程式的完整 UX 架構。

WinUI 3 包含 Visual Studio 2019 的 VSIX 套件，可提供專案和項目範本，協助您開始使用以 WinUI 為基礎的介面來建置應用程式。 如需 WinUI 3 VSIX 套件及其所提供專案範本的詳細資訊，請參閱[本節](../winui/winui3/index.md#install-winui-3-preview-2)。

> [!IMPORTANT]
> WinUI 3 (包括相關的 Visual Studio 範本) 目前是以開發人員預覽版形式提供，其適用於早期評估以及從開發人員社群收集意見反應。 目前「不」應該用於生產應用程式。

## <a name="uwp-templates"></a>UWP 範本

Visual Studio 提供各種專案範本，以使用 C# 或 C++ 來建置 UWP 應用程式。 若要使用這些專案範本，您必須在安裝 Visual Studio 時包含**通用 Windows 平台開發**工作負載。 針對 C++ 專案範本，您也必須包含適用於**通用 Windows 平台開發**工作負載的 **C++ (v142) 通用 Windows 平台工具**選用元件。

### <a name="project-templates-for-c-and-uwp"></a>適用於 C# 和 UWP 的專案範本

若要在 Visual Studio 中建立新專案時存取 UWP C# 專案範本，請將語言篩選為 **C#** 、將平台篩選為 **Windows**，以及將專案類型篩選為 **UWP**。

![UWP C# 專案範本](images/uwp-projects-csharp.png)

您可以使用這些專案範本來建立 C# UWP 應用程式。

| 範本 | 說明 |
|----------|-------------|
| 空白應用程式 (通用 Windows) | 建立 UWP 應用程式。 產生的專案包含一個基本頁面，該頁面是從 [Windows.UI.Xaml.Controls.Page](/uwp/api/windows.ui.xaml.controls.page) 類別衍生，您可以用來開始建置您的 UI。 |
| 單元測試應用程式 (通用 Windows) | 在 C# 中建立適用於 UWP 應用程式的單元測試專案。 如需詳細資訊，請參閱[單元測試 C# 程式碼](/visualstudio/test/unit-testing-visual-csharp-code-in-a-store-app)。 |

您可以使用這些專案範本來建置 C# UWP 應用程式的片段。

| 範本 | 說明 |
|----------|-------------|
| 類別庫 (通用 Windows) | 在 C# 中建立受控類別庫 (DLL)，可供以受控程式碼撰寫的其他 UWP 應用程式使用。 |
| Windows 執行階段元件 (通用 Windows) | 在 C# 中建立 [Windows 執行階段元件](/windows/uwp/winrt-components/)，可以由任何 UWP 應用程式取用，不論該應用程式是以何種程式設計語言撰寫。 |
| 選用程式碼套件 (通用 Windows) | 建立具可執行 C# 程式碼的選用套件，可以由應用程式載入。 如需詳細資訊，請參閱[具可執行程式碼的選用套件](/windows/msix/package/optional-packages-with-executable-code)。  |

### <a name="project-templates-for-c-and-uwp"></a>適用於 C++ 和 UWP 的專案範本

您可以使用兩種不同的技術來建置 C++ UWP 應用程式：

* 建議的技術是 [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/)。 這是完全在標頭檔中實作的 C++ 語言投影，設計的目的是提供您新式 WinRT API 的第一級存取。
* 或者，您可以使用舊版 [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx) 延伸模組集合。 C++/CX 仍然受支援，但是我們建議您改用 C++/WinRT。

若要在 Visual Studio 中建立新專案時存取 UWP C++ 專案範本，請將語言篩選為 **C++** 、將平台篩選為 **Windows**，以及將專案類型篩選為 **UWP**。 

> [!NOTE]
> 根據預設，Visual Studio 中的**通用 Windows 平台開發**工作負載只會提供 C++/CX 專案範本的存取權。 若要存取 C++/WinRT 專案範本，您必須安裝 [C++/WinRT VSIX](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package) 套件。

![UWP C++ 專案範本](images/uwp-projects-cpp.png)

您可以使用這些專案範本來建立 C++ UWP 應用程式。

| 範本 | 說明 |
|----------|-------------|
| 空白的應用程式 (C++/WinRT) | 建立具有 XAML 使用者介面的 C++/WinRT UWP 應用程式。 產生的專案包含一個基本頁面，該頁面是從 [Windows.UI.Xaml.Controls.Page](/uwp/api/windows.ui.xaml.controls.page) 類別衍生，您可以用來開始建置您的 UI。 |
| 核心應用程式 (C++/WinRT) | 建立 C++/WINRT UWP 應用程式，該應用程式使用 [CoreApplication](/uwp/api/Windows.ApplicationModel.Core.CoreApplication) (而不是 XAML 使用者介面) 與各種 UI 架構整合。 如需逐步解說，示範如何使用此專案範本來建立使用 DirectX 的簡單遊戲，請參閱[使用 DirectX 建立簡單的 UWP 遊戲](/windows/uwp/gaming/tutorial--create-your-first-uwp-directx-game)。 |
| 空白應用程式 (通用 Windows - C++/CX) | 建立具有 XAML 使用者介面的 C++/WinRT UWP 應用程式。 產生的專案包含一個基本頁面，該頁面是從 WinUI 程式庫中的 [Windows.UI.Xaml.Controls.Page](/uwp/api/windows.ui.xaml.controls.page) 類別衍生，您可以用來開始建置您的 UI。 |
| DirectX 11 和 XAML 應用程式 (通用 Windows - C++/CX) | 建立使用 DirectX 11 和 **SwapChainPanel** 的 UWP 應用程式，以便您可以使用 XAML UI 控制項。 如需詳細資訊，請參閱 [DirectX 遊戲專案範本](/windows/uwp/gaming/user-interface)。 |
| DirectX 11 應用程式 (通用 Windows - C++/CX) | 建立使用 DirectX 11 的 UWP 應用程式。 如需詳細資訊，請參閱 [DirectX 遊戲專案範本](/windows/uwp/gaming/user-interface)。 |
| DirectX 12 應用程式 (通用 Windows - C++/CX) | 建立使用 DirectX 12 的 UWP 應用程式。 如需詳細資訊，請參閱 [DirectX 遊戲專案範本](/windows/uwp/gaming/user-interface)。 |
| 單元測試應用程式 (通用 Windows - C++/CX) | 在 C++/CX 中建立適用於 UWP 應用程式的單元測試專案。 如需詳細資訊，請參閱[如何測試 C++ UWP DLL](/visualstudio/test/unit-testing-a-visual-cpp-dll-for-store-apps)。 |

您可以使用這些專案範本來建置 C++ UWP 應用程式的片段。

| 範本 | 說明 |
|----------|-------------|
| Windows 執行階段元件 (C++/WinRT) | 在 C++/WinRT 中建立 [Windows 執行階段元件](/windows/uwp/winrt-components/)，可以由任何 UWP 應用程式取用，不論該應用程式是以何種程式設計語言撰寫。 |
| Windows 執行階段元件 (通用 Windows) | 在 C++/CX 中建立 [Windows 執行階段元件](/windows/uwp/winrt-components/)，可以由任何 UWP 應用程式取用，不論該應用程式是以何種程式設計語言撰寫。 |
| DLL (通用 Windows) | 用於在 C++/CX 中建立動態連結程式庫 (DLL) (可於 UWP 應用程式中使用) 的專案。 如需詳細資訊，請參閱 [DLL (C++/CX)](/cpp/cppcx/dlls-c-cx)。 |
| 靜態程式庫 (通用 Windows) | 用於在 C++/CX 中建立靜態程式庫 (LIB) (可於 UWP 應用程式中使用) 的專案。 如需詳細資訊，請參閱[靜態程式庫 (C++/CX)](/cpp/cppcx/static-libraries-c-cx)。 |

## <a name="cwin32-templates"></a>C++/Win32 範本

Visual Studio 提供各種專案範本，可用來建置桌面 Windows 應用程式，該應用程式具有 WIN32 API 的原生 C++ 和直接存取權。 若要使用這些專案範本，您必須在安裝 Visual Studio 時包含**使用 C++ 的桌面開發**工作負載。 此工作負載包含用於建置桌面應用程式、主控台應用程式和程式庫的專案範本。

### <a name="project-templates-for-desktop-apps"></a>適用於桌面應用程式的專案範本

若要在 Visual Studio 中建立新專案時存取適用於傳統桌面應用程式的 C++ 專案範本，請將語言篩選為 **C++** 、將平台篩選為 **Windows**，以及將專案類型篩選為**桌面**。

![原生 C++ 應用程式專案範本](images/desktop-app-projects-cpp.png)

| 範本 | 說明 |
|----------|----------|
| Windows 桌面應用程式 | 使用 C++ 建立傳統 Windows 桌面應用程式。 如需詳細資訊，請參閱[逐步解說：建立傳統 Windows 桌面應用程式](/cpp/windows/walkthrough-creating-windows-desktop-applications-cpp)。 |
| Windows 桌面精靈 | 提供可讓您用來建立下列其中一種專案類型的逐步精靈：傳統 Windows 桌面應用程式、主控台應用程式、動態連結程式庫 (DLL) 或靜態程式庫。 如需詳細資訊，請參閱 [Windows 桌面精靈](/cpp/windows/windows-desktop-wizard)和[逐步解說：建立傳統 Windows 桌面應用程式](/cpp/windows/walkthrough-creating-windows-desktop-applications-cpp)。         |
| Windows 應用程式封裝專案 | 建立專案，您可以用來將桌面應用程式建置到 [MSIX 套件](/windows/msix/overview)。 這可提供新式部署體驗，透過套件擴充功能與 Windows 10 功能整合的能力，還有更多功能。 如需詳細資訊，請參閱 [Windows 應用程式封裝專案](/windows/msix/desktop/desktop-to-uwp-packaging-dot-net)。  |

### <a name="project-templates-for-console-apps"></a>適用於主控台應用程式的專案範本

若要存取適用於主控台應用程式的 C++ 專案範本，請將語言篩選為 **C++** 、將平台篩選為 **Windows**，以及將專案類型篩選為**主控台**。

![原生 C++ 主控台專案範本](images/desktop-console-projects-cpp.png)

| 範本 | 說明 |
|----------|----------|
| Windows 主控台應用程式 (C++/WinRT) | 建立沒有使用者介面的 [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/) 主控台應用程式。 如需詳細資訊，請參閱 [C++/WinRT 快速入門](/windows/uwp/cpp-and-winrt-apis/get-started#a-cwinrt-quick-start)。 此專案範本需要 [C++/WINRT VSIX](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)。  |
| 主控台應用程式 | 建立沒有使用者介面的主控台應用程式。 如需詳細資訊，請參閱[逐步解說：建立標準 C++ 程式](/cpp/windows/walkthrough-creating-a-standard-cpp-program-cpp)。 |
| 空白專案 | 用於建立應用程式、程式庫或 DLL 的空白專案。 您必須新增任何必要的程式碼或資源。 |

### <a name="project-templates-for-libraries"></a>程式庫的專案範本

若要存取適用於程式庫的 C++ 專案範本，請將語言篩選為 **C++** 、將平台篩選為 **Windows**，以及將專案類型篩選為**程式庫**。

![原生 C++ 程式庫專案範本](images/desktop-library-projects-cpp.png)

| 範本 | 說明 |
|----------|----------|
| 動態連結程式庫 (DLL) | 用於建立動態連結程式庫 (DLL) 的專案。 如需詳細資訊，請參閱[逐步解說：建立和使用動態連結程式庫](/cpp/build/walkthrough-creating-and-using-a-dynamic-link-library-cpp)。 |
| 靜態程式庫 | 用於建立靜態程式庫 (LIB) 的專案。 如需詳細資訊，請參閱[逐步解說：建立和使用靜態程式庫](/cpp/build/walkthrough-creating-and-using-a-static-library-cpp)。 |

### <a name="item-templates-for-native-c-and-win32"></a>適用於原生 C++ 和 Win32 的項目範本

C++ 專案範本包含許多項目範本，您可以用來執行像是將新檔案和資源新增至專案的工作。 如需完整的清單，請參閱[使用 Visual C++ 新增新的項目範本](/cpp/build/reference/using-visual-cpp-add-new-item-templates)。

## <a name="net-templates"></a>.NET 範本

Visual Studio 提供各種專案範本，可用來建置桌面 Windows 應用程式，該應用程式使用 .NET 和 C#。 若要使用這些專案範本，您必須在安裝 Visual Studio 時包含 **.NET 桌面開發**工作負載。

若要在 Visual Studio 中建立新專案時存取 .NET C# 專案範本，請將語言篩選為 **C#** 、將平台篩選為 **Windows**，以及將專案類型篩選為**桌面**。

![.NET C# 專案範本](images/dotnet-projects-csharp.png)

您可以使用這些專案範本，使用 C# 和 .NET 來建立應用程式。

| 範本 | 說明 |
|----------|----------|
| WPF 應用程式 (.NET Core) | 建立以 [.NET Core](/dotnet/core/) 為目標的 [WPF](/dotnet/framework/wpf/) 應用程式。 如需這個專案範本的逐步解說，請參閱[建立 WPF 應用程式](/visualstudio/get-started/csharp/tutorial-wpf)。 |
| WPF 應用程式 (.NET Framework) | 建立以 [.NET Framework](/dotnet/framework/) 為目標的 [WPF](/dotnet/framework/wpf/) 應用程式。 如需這個專案範本的逐步解說，請參閱[教學課程：建立您的第一個 WPF 應用程式](/dotnet/framework/wpf/getting-started/walkthrough-my-first-wpf-desktop-application)。 |
| Windows Forms 應用程式 (.NET Core) | 建立以 [.NET Core](/dotnet/core/) 為目標的 [Windows Forms](/dotnet/framework/winforms/) 應用程式。  |
| Windows Forms 應用程式 (.NET Framework) | 建立以 [.NET Framework](/dotnet/framework/) 為目標的 [Windows Forms](/dotnet/framework/winforms/) 應用程式。 如需此專案範本的逐步解說，請參閱[在 Visual Studio 中使用 C# 建立 Windows Forms 應用程式](/visualstudio/ide/create-csharp-winform-visual-studio)。 |
| Windows 應用程式封裝專案 | 建立專案，您可以用來將 WPF 或 Windows Forms 應用程式建置到 [MSIX 套件](/windows/msix/overview)。 這可提供新式部署體驗，透過套件擴充功能與 Windows 10 功能整合的能力，還有更多功能。 如需詳細資訊，請參閱 [Windows 應用程式封裝專案](/windows/msix/desktop/desktop-to-uwp-packaging-dot-net)。 |