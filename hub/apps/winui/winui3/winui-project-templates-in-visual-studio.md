---
title: 建立 WinUI 3 專案
description: Visual Studio 中可用的 WinUI 3 專案和專案範本摘要。
ms.date: 03/19/2021
ms.topic: article
ms.openlocfilehash: 823a8698d2d97207a69b31655437d02b70d05857
ms.sourcegitcommit: 99f7edfd79c44768751aca02dab30a03f41d2aae
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/02/2021
ms.locfileid: "106218213"
---
# <a name="winui-3-project-templates-in-visual-studio"></a>Visual Studio 中的 WinUI 3 專案範本

安裝 Project 留尼旺島 0.5 VSIX 套件之後，您就可以使用 Visual Studio 中的其中一個 WinUI 專案範本來建立 WinUI 3 應用程式。 若要在 [建立新專案] 對話方塊中存取 WinUI 專案範本，請將語言篩選為 **C++** 或 **C#** ，將平台篩選為 **Windows**，以及將專案類型篩選為 **WinUI**。 或者，您可以搜尋「WinUI」並選取其中一個可用的 C# 或 C++ 範本。

![WinUI 專案範本](images/winui3-csharp-newproject.png)

> [!NOTE] 
> 請參閱我們最新 [的版本](../index.md) 資訊，以取得設定開發環境、已知問題等相關指引。

Project 留尼旺島0.5 版有兩個 Visual Studio 擴充功能： **Project 留尼旺島 vsix**，以及 **PROJECT 留尼旺島 Preview VSIX**。 Project 留尼旺島 VSIX 包含專案範本，可用於建立 MSIX 封裝的桌面生產應用程式。 Project 留尼旺島 Preview VSIX 包含實驗性專案範本，可用來建立 MSIX 封裝的桌面或 UWP 應用程式。 

## <a name="project-templates-for-winui-3"></a>WinUI 3 的專案範本

您可以使用這些 WinUI 專案範本來建立應用程式。

| 範本 | Language | 描述 |
|----------|----------|-------------|
| 在桌面)  (WinUI 3 的空白應用程式封裝 | C# 和 C++ | 使用以 WinUI 為基礎的使用者介面，建立桌面 .NET 5 (C#) 或原生 Win32 (C++) 應用程式。 產生的專案包含一個基本視窗，該視窗是從 WinUI 程式庫中的 **Microsoft.UI.Xaml.Window** 類別衍生，您可以用來開始建置您的 UI。 如需此專案類型的詳細資訊，請參閱[開始使用適用於桌面應用程式的 WinUI 3](get-started-winui3-for-desktop.md)。<p></p>解決方案也包含 [Windows 應用程式封裝專案](/windows/msix/desktop/desktop-to-uwp-packaging-dot-net)，其已設定為將應用程式建置到 [MSIX 套件](/windows/msix/overview)。 這可提供新式部署體驗，透過套件擴充功能與 Windows 10 功能整合的能力，還有更多功能。  |
| 在 UWP) 中 (WinUI 3 的空白應用程式，實驗性 | C# 和 C++ | 建立具有以 WinUI 為基礎的使用者介面的 UWP 應用程式。 產生的專案包含一個基本頁面，該頁面是從 WinUI 程式庫中的 **Microsoft.UI.Xaml.Controls.Page** 類別衍生，您可以用來開始建置您的 UI。 如需此專案類型的詳細資訊，請參閱[開始使用適用於 UWP 應用程式的 WinUI 3](get-started-winui3-for-uwp.md)。 |

您可以使用這些 WinUI 專案範本來建置可供以 WinUI 為基礎的應用程式載入及使用的元件。

| 範本 | Language | 描述 |
|----------|----------|-------------|
| Desktop 中的類別庫 (WinUI 3)  | 僅限 C# | 在 C# 中建立 .NET 5 受控類別庫 (DLL)，可供具有以 WinUI 為基礎的使用者介面的其他 .NET 5 桌面應用程式使用。  |
| UWP) 中的類別庫 (WinUI 3，實驗性  | 僅限 C# | 在 C# 中建立受控類別程式庫 (DLL)，可供具有以 WinUI 為基礎的使用者介面的其他 UWP 應用程式使用。 |
| Windows 執行階段元件 (WinUI 3)  | C++ | 使用以 WinUI 為基礎的使用者介面，建立以 c + +/WinRT 撰寫的 [Windows 執行階段元件](/windows/uwp/winrt-components/) ，不論應用程式在哪種程式設計語言中撰寫。 |
| Windows 執行階段元件 (UWP) 中的 WinUI 3，實驗性 | C# | 建立以 C# 撰寫的 [Windows 執行階段元件](/windows/uwp/winrt-components/)，可以由具有 WinUI 架構使者介面的任何 UWP 應用程式取用，不論該應用程式是以何種程式設計語言撰寫。 |

## <a name="item-templates-for-winui-3"></a>WinUI 3 的項目範本

您可以在 WinUI 專案中使用下列項目範本。 若要存取這些 WinUI 項目範本，請以滑鼠右鍵按一下 [方案總管] 中的專案節點，選取 [新增]  ->  [新項目]，然後按一下 [新增新項目] 對話方塊中的 [WinUI]。

![WinUI 項目範本](images/winui3-addnewitem.png)

| 範本 | Language | 描述 |
|----------|----------|-------------|
| 空白頁面 (WinUI 3)  | C# 和 C++ | 新增 XAML 檔案和程式碼檔案，該檔案會定義衍生自 WinUI 程式庫中 **Microsoft.UI.Xaml.Controls.Page** 類別的新頁面。 |
| 桌面) 中的空白視窗 (WinUI 3 | C# 和 C++ | 新增 XAML 檔案和程式碼檔案，該檔案會定義衍生自 WinUI 程式庫中 **Microsoft.UI.Xaml.Window** 類別的新視窗。 |
| 自訂控制項 (WinUI 3)  | C# 和 C++ | 新增程式碼檔案，以使用預設樣式建立樣板化控制項。 樣板化控制項衍生自 WinUI 程式庫中的 **Microsoft.UI.Xaml.Controls.Control** 類別。<p></p>如需示範如何使用此項目範本的逐步解說，請參閱 [UWP 和 WinUI 3 應用程式的樣板化 XAML 控制項 (使用 C++/WinRT)](xaml-templated-controls-cppwinrt-winui-3.md) 和 [UWP 和 WinUI 3 應用程式的樣板化 XAML 控制項 (使用 C#)](xaml-templated-controls-csharp-winui-3.md)。 如需樣板化控制項的詳細資訊，請參閱[自訂 XAML 控制項](/archive/msdn-magazine/2019/may/xaml-custom-xaml-controls)。 |
| 資源字典 (WinUI 3)  | C# 和 C++ | 新增 XAML 資源的空白索引鍵集合。 如需詳細資訊，請參閱 [ResourceDictionary 與 XAML 資源參考](/windows/uwp/design/controls-and-patterns/resourcedictionary-and-xaml-resource-references)。 |
| 資源檔 (WinUI 3)  | C# 和 C++ | 新增檔案，用於儲存應用程式的字串和條件式資源。 您可以使用此項目來協助將您的應用程式當地語系化。 如需詳細資料，請參閱[將您 UI 和應用程式套件資訊清單中的字串當地語系化](/windows/uwp/app-resources/localize-strings-ui-manifest)。 |
| 使用者控制項 (WinUI 3)  | C# 和 C++ | 新增 XAML 檔案和程式碼檔案，以建立衍生自 WinUI 程式庫中 **Microsoft.UI.Xaml.Controls.UserControl** 類別的使用者控制項。 一般而言，使用者控制項會封裝相關的現有控制項，並提供自己的邏輯。<p></p>如需使用者控制項的詳細資訊，請參閱[自訂 XAML 控制項](/archive/msdn-magazine/2019/may/xaml-custom-xaml-controls)。 |
