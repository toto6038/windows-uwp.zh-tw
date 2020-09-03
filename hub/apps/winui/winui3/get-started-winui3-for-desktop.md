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
ms.openlocfilehash: 2b946047602013704b27fb5c5565155d38dbb7f8
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89154822"
---
# <a name="get-started-with-winui-3-for-desktop-apps"></a>開始使用適用於桌面應用程式的 WinUI 3

WinUI 3 預覽版 2 引進了新的專案範本，可讓您使用完全以 WinUI 為基礎的使用者介面來建立受控的桌面版 C#/.NET Core 和原生 C++/Win32 桌面應用程式。 當您使用這些專案範本來建立應用程式時，應用程式的整個使用者介面都會使用 WinUI 3 提供的視窗、控制項和其他 UI 類型來實作。 如需專案範本的完整清單，請參閱[本節](index.md#project-templates-for-winui-3)。

## <a name="prerequisites"></a>必要條件

若要使用本文所述適用於桌面的 WinUI 3 專案範本，請遵循[這裡](index.md#install-winui-3-preview-2)的指示來設定您的開發電腦以及安裝 WinUI 3 預覽版 2。

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

    * **專案名稱 (封裝)** ：這是 [Windows 應用程式封裝專案](/windows/msix/desktop/desktop-to-uwp-packaging-dot-net)，其已設定為將應用程式建置到 [MSIX 套件](/windows/msix/overview)。 這可提供新式部署體驗，透過套件擴充功能與 Windows 10 功能整合的能力，還有更多功能。 此專案包含您應用程式的[封裝資訊清單](/uwp/schemas/appxpackage/uapmanifestschema/schema-root)，而且是您解決方案預設的啟始專案。

        ![應用程式專案](images/WinUI-csharp-packageproject.png)

7. 若要將新項目新增至您的應用程式專案，請以滑鼠右鍵按一下 [方案總管] 中的 **專案名稱 (桌面)** 專案節點，然後選取 [新增] -> [新增項目]。 在 [新增項目] 對話方塊中選取 [WinUI] 索引標籤，選擇您要新增的專案，然後按一下 [新增]。 如需可用項目的詳細資訊，請參閱[本節](index.md#item-templates-for-winui-3)。

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

    * **專案名稱 (封裝)** ：這是 [Windows 應用程式封裝專案](/windows/msix/desktop/desktop-to-uwp-packaging-dot-net)，其已設定為將應用程式建置到 [MSIX 套件](/windows/msix/overview)。 這可提供新式部署體驗，透過套件擴充功能與 Windows 10 功能整合的能力，還有更多功能。 此專案包含您應用程式的[封裝資訊清單](/uwp/schemas/appxpackage/uapmanifestschema/schema-root)，而且是您解決方案預設的啟始專案。

        ![封裝專案](images/WinUI-cpp-packageproject.png)

7. 若要將新項目新增至您的應用程式專案，請以滑鼠右鍵按一下 [方案總管] 中的 **專案名稱 (桌面)** 專案節點，然後選取 [新增] -> [新增項目]。 在 [新增項目] 對話方塊中選取 [WinUI] 索引標籤，選擇您要新增的專案，然後按一下 [新增]。 如需可用項目的詳細資訊，請參閱[本節](index.md#item-templates-for-winui-3)。

    ![新增項目](images/WinUI-cpp-newitem.png)

8. 建立並執行您的解決方案，以確認應用程式可在沒有錯誤的情況下執行。

## <a name="known-issues-and-limitations"></a>已知的問題和限制

如需已知問題和限制的清單，請參閱[本節](index.md#preview-2-limitations-and-known-issues)。

## <a name="related-topics"></a>相關主題

* [Windows UI 程式庫 3](index.md)