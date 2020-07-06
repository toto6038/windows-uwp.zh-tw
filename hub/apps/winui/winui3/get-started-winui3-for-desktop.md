---
description: 本指南將說明如何開始使用 WinUI 3 UI 來建立 .NET 和 C++/Win32 桌面應用程式。
title: 開始使用適用於桌面應用程式的 WinUI 3
ms.date: 05/19/2020
ms.topic: article
keywords: windows 10, uwp, windows forms, wpf, xaml islands
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: high
ms.custom: 19H1
ms.openlocfilehash: 8775113c22716259f9449899b577481738dc6c0f
ms.sourcegitcommit: da1c0ae251883987f105bc2919b2d67846194bc5
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/22/2020
ms.locfileid: "85198516"
---
# <a name="get-started-with-winui-30-for-desktop-apps"></a>開始使用適用於桌面應用程式的 WinUI 3.0

WinUI 3.0 預覽版 1 引進了新的專案範本，可讓您使用完全以 WinUI 為基礎的使用者介面來建立受控的桌面版 C#/.NET 和原生 C++/Win32 桌面應用程式。 當您使用這些專案範本來建立應用程式時，應用程式的整個使用者介面都會使用 WinUI 3.0 提供的視窗、控制項和其他 UI 類型來實作。

WinUI 3.0 Preview 1 會將下列專案範本新增至 Visual Studio 2019，以便建置使用 WinUI 3.0 的桌面應用程式：

* 適用於以 .NET 5 為目標的 C# 應用程式和程式庫的專案範本：
  * **已封裝的空白應用程式 (WinUI in Desktop)**
  * **類別庫 (WinUI in Desktop)**

* 適用於 C++ /Win32 應用程式的專案範本：
  * **已封裝的空白應用程式 (WinUI in Desktop)**

應用程式專案範本會產生 WinUI 應用程式專案和 [Windows 應用程式封裝專案](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net)，其已設定為將應用程式建立至 [MSIX 套件](https://docs.microsoft.com/windows/msix/overview)以進行部署。

## <a name="prerequisites"></a>必要條件

若要使用本文所述的桌面板 WinUI 3 專案範本，請遵循下列指示來設定您的開發電腦：

1. 請確定您的開發電腦已安裝 Windows 10 1803 版 (組建 17134) 或更新版本。 適用於桌面應用程式的 WinUI 3 需要 1803 版或更新版本的作業系統。

2. 安裝 Visual Studio 2019 (16.7 預覽版 1 版本)。 如需詳細資訊，請參閱[這些指示](index.md#configure-your-dev-environment)。

3. 安裝 .NET 5 預覽版 4 的 x64 和 x86 版本：
    * x64：[https://aka.ms/dotnet/net5/preview4/Sdk/dotnet-sdk-win-x64.exe](https://aka.ms/dotnet/net5/preview4/Sdk/dotnet-sdk-win-x64.exe)
    * x86：[https://aka.ms/dotnet/net5/preview4/Sdk/dotnet-sdk-win-x86.exe](https://aka.ms/dotnet/net5/preview4/Sdk/dotnet-sdk-win-x86.exe)

    > [!NOTE]
    > WinUI 3.0 Preview 1 需要 .NET 5 Preview 4。 WinUI 3.0 Preview 1 不支援較新的 .NET 5 預覽版本。

4. 安裝 VSIX 擴充功能，其中包含適用於 Visual Studio 2019 的 WinUI 3.0 預覽版 1 專案範本。 如需詳細資訊，請參閱[這些指示](index.md#visual-studio-project-templates)。

## <a name="create-a-winui-3-desktop-app-for-c-and-net-5"></a>建立適用於 C# 和 .NET 5 的 WinUI 3 桌面應用程式

1. 在 Visual Studio 2019 中，選取 [檔案] -> [新增] -> [專案]。

2. 在 [專案] 下拉式篩選器中，分別選取 [C#]、[Windows] 和 [WinUI]。

3. 選取 [已封裝的空白應用程 (WinUI in Desktop)] 專案類型，然後按 [下一步]。

    ![空白應用程式專案範本](images/WinUI-csharp-newproject.png)

4. 輸入專案名稱，視需要選擇任何其他選項，然後按一下 [建立]。

5. 在下列對話方塊中，將 [目標版本] 設定為 Windows 10 1903 版 (組建 18362)，並將 [最低版本] 設定為 Windows 10 1803 版 (組建 17134)，然後按一下 [確定]。

    ![目標和最低版本](images/WinUI-min-target-version.png)

6. 此時，Visual Studio 會產生兩個專案：

    * **專案名稱 (桌面)** ：此專案包含您的應用程式程式碼。 **App.xaml.cs** 程式碼檔案會定義代表您應用程式執行個體的 `Application` 類別，而 **MainWindow.xaml.cs** 程式碼檔案會定義 `MainWindow` 類別來代表您應用程式所顯示的主要視窗。 這些類別衍生自 **Microsoft.UI.Xaml** 命名空間 (由 WinUI 提供) 中的類型。

        ![應用程式專案](images/WinUI-csharp-appproject.png)

    * **專案名稱 (封裝)** ：這是 [Windows 應用程式封裝專案](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net)，其已設定為將應用程式建立至 MSIX 套件以進行部署。 此專案包含您應用程式的[封裝資訊清單](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/schema-root)，而且是您解決方案預設的啟始專案。

        ![應用程式專案](images/WinUI-csharp-packageproject.png)

7. 若要將新項目新增至您的應用程式專案，請以滑鼠右鍵按一下 [方案總管] 中的 **專案名稱 (桌面)** 專案節點，然後選取 [新增] -> [新增項目]。 在 [新增項目] 對話方塊中選取 [WinUI] 索引標籤，選擇您要新增的專案，然後按一下 [新增]。 您可以選擇下列項目類型：

    * **空白頁**
    * **空白視窗**
    * **自訂控制項**
    * **資源字典**
    * **資源檔**
    * **使用者控制項**

    ![新增項目](images/WinUI-csharp-newitem.png)

8. 建立並執行您的解決方案，以確認應用程式可在沒有錯誤的情況下執行。

## <a name="create-a-winui-3-desktop-app-for-cwin32"></a>建立適用於 C++/Win32 的 WinUI 3 桌面應用程式

1. 在 Visual Studio 2019 中，選取 [檔案] -> [新增] -> [專案]。

2. 在 [專案] 下拉式篩選器中，選取 [C++]、[Windows] 和 [WinUI]。

3. 選取 [已封裝的空白應用程 (WinUI in Desktop)] 專案類型，然後按 [下一步]。

    ![空白應用程式專案範本](images/WinUI-cpp-newproject.png)

4. 輸入專案名稱，視需要選擇任何其他選項，然後按一下 [建立]。

5. 在下列對話方塊中，將 [目標版本] 設定為 Windows 10 1903 版 (組建 18362)，並將 [最低版本] 設定為 Windows 10 1803 版 (組建 17134)，然後按一下 [確定]。

    ![目標和最低版本](images/WinUI-min-target-version.png)

6. 此時，Visual Studio 會產生兩個專案：

    * **專案名稱 (桌面)** ：此專案包含您的應用程式程式碼。 **App.xaml** 和各種 **App** 程式碼檔案會定義代表您應用程式執行個體的 `Application` 類別，而 **MainWindow.xaml** 和各種 **MainWindow** 程式碼檔案會定義 `MainWindow` 類別來代表您應用程式所顯示的主要視窗。 這些類別衍生自 **Microsoft.UI.Xaml** 命名空間 (由 WinUI 提供) 中的類型。

        ![應用程式專案](images/WinUI-cpp-appproject.png)

    * **專案名稱 (封裝)** ：這是 [Windows 應用程式封裝專案](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net)，其已設定為將應用程式建立至 MSIX 套件以進行部署。 此專案包含您應用程式的[封裝資訊清單](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/schema-root)，而且是您解決方案預設的啟始專案。

        ![封裝專案](images/WinUI-cpp-packageproject.png)

7. 若要將新項目新增至您的應用程式專案，請以滑鼠右鍵按一下 [方案總管] 中的 **專案名稱 (桌面)** 專案節點，然後選取 [新增] -> [新增項目]。 在 [新增項目] 對話方塊中選取 [WinUI] 索引標籤，選擇您要新增的專案，然後按一下 [新增]。 您可以選擇下列項目類型：

    * **空白頁**
    * **空白視窗**
    * **自訂控制項**
    * **資源字典**
    * **資源檔**
    * **使用者控制項**

    ![新增項目](images/WinUI-cpp-newitem.png)

8. 建立並執行您的解決方案，以確認應用程式可在沒有錯誤的情況下執行。

## <a name="known-issues-and-limitations"></a>已知的問題和限制

如需預覽版 1 中的已知問題和限制清單，請參閱[本節](index.md#preview-1-limitations-and-known-issues)。

## <a name="related-topics"></a>相關主題

* [WinUI 3.0](index.md)
