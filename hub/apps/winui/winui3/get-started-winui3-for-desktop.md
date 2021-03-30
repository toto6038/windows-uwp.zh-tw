---
description: 本指南將說明如何開始使用 WinUI 3 UI 來建立 .NET 和 C++/Win32 桌面應用程式。
title: 開始使用適用於桌面應用程式的 WinUI 3
ms.date: 03/19/2021
ms.topic: article
keywords: windows 10, uwp, windows forms, wpf, xaml islands
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: high
ms.custom: 19H1
ms.openlocfilehash: 8f8462a3645aff1113918f92bf53adb672ae3a61
ms.sourcegitcommit: 7f2a09e8d5d37cb5860a5f2ece5351ea6907b94c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/29/2021
ms.locfileid: "105730552"
---
# <a name="get-started-with-winui-3-for-desktop-apps"></a>開始使用適用於桌面應用程式的 WinUI 3

WinUI 3-Project 留尼旺島0.5 包含專案範本，可讓您使用完整 WinUI 型使用者介面建立 managed c #/.NET 5 和原生 c + +/Win32 desktop 應用程式。 當您使用這些專案範本來建立應用程式時，應用程式的整個使用者介面都會使用 WinUI 3 提供的視窗、控制項和其他 UI 類型來實作。 如需專案範本的完整清單，請參閱 [WinUI 3 的專案範本](winui-project-templates-in-visual-studio.md#project-templates-for-winui-3)。

WinUI 3 隨附于專案留尼旺島套件中。 如需專案留尼旺島的詳細資訊，請參閱 [使用 Project 留尼旺島 0.5 (3 月 2021) 建立桌面 Windows 應用程式 ](../../project-reunion/index.md)。

## <a name="prerequisites"></a>必要條件

若要使用本文所述的 WinUI 3 for desktop 專案範本，請設定您的開發電腦，並安裝 Project 留尼旺島 0.5 Visual Studio 延伸模組。 如需詳細資訊，請參閱 [設定您的開發環境](../../project-reunion/get-started-with-project-reunion.md#set-up-your-development-environment)。

## <a name="create-a-winui-3-desktop-app-for-c-and-net-5"></a>建立適用於 C# 和 .NET 5 的 WinUI 3 桌面應用程式

1. 在 Visual Studio 2019 中，選取 [檔案] -> [新增] -> [專案]。

2. 在 [專案] 下拉式篩選器中，分別選取 [C#]、[Windows] 和 [WinUI]。

3. 選取 [ **桌面) 專案類型] 中 (WinUI 3 的空白應用程式，** 然後按 **[下一步]**。

    ![[建立新的專案嚮導] 的螢幕擷取畫面，其中已反白顯示 [桌面) ] 選項中的空白應用程式封裝 (Win UI。](images/WinUI3-csharp-newproject.png)

4. 輸入專案名稱，視需要選擇任何其他選項，然後按一下 [建立]。

5. 在下列對話方塊中，將 **目標版本** 設為 Windows 10，2004版 (組建 19041) 和 **最低版本** Windows 10 版本 1809 (組建 17763) 然後按一下 **[確定]**。

    ![目標和最低版本](images/WinUI3-minversion.png)

6. 此時，Visual Studio 會產生兩個專案：

    * **專案名稱 (桌面)** ：此專案包含您的應用程式程式碼。 **App.xaml** 檔案和 **App.xaml.cs** 程式碼後置檔案會定義一個 `Application` 類別，代表您的應用程式執行個體。 **Mainwindow.xaml** 檔案和 **MainWindow.xaml.cs** 程式碼後置檔案會定義一個 `MainWindow` 類別，代表您應用程式所顯示的主要視窗。 這些類別衍生自 **Microsoft.UI.Xaml** 命名空間 (由 WinUI 提供) 中的類型。

        ![Visual Studio 的螢幕擷取畫面，其中顯示 [方案總管] 窗格和主要 Windows XAML dot CS 檔案的內容。](images/WinUI-csharp-appproject.png)

    * **專案名稱 (封裝)** ：這是 [Windows 應用程式封裝專案](/windows/msix/desktop/desktop-to-uwp-packaging-dot-net)，其已設定為將應用程式建置到 [MSIX 套件](/windows/msix/overview)。 這可提供新式部署體驗，透過套件擴充功能與 Windows 10 功能整合的能力，還有更多功能。 此專案包含您應用程式的[封裝資訊清單](/uwp/schemas/appxpackage/uapmanifestschema/schema-root)，而且是您解決方案預設的啟始專案。

        ![Visual Studio 的螢幕擷取畫面，其中顯示 [方案總管] 窗格和套件應用程式 x 資訊清單檔案的內容。](images/WinUI-csharp-packageproject.png)

7. 若要將新項目新增至您的應用程式專案，請以滑鼠右鍵按一下 [方案總管] 中的 **專案名稱 (桌面)** 專案節點，然後選取 [新增] -> [新增項目]。 在 [新增項目] 對話方塊中選取 [WinUI] 索引標籤，選擇您要新增的專案，然後按一下 [新增]。 如需可用項目的詳細資訊，請參閱 [WinUI 3 的項目範本](winui-project-templates-in-visual-studio.md#item-templates-for-winui-3)。

    ![[新增項目] 對話方塊的螢幕擷取畫面，其中已選取 [已安裝 > Visual C sharp 項目 > Win UI] 並且醒目提示 [空白頁面] 選項。](images/winui3-addnewitem.png)

8. 建立並執行您的解決方案，以確認應用程式可在沒有錯誤的情況下執行。

## <a name="create-a-winui-3-desktop-app-for-cwin32"></a>建立適用於 C++/Win32 的 WinUI 3 桌面應用程式

1. 在 Visual Studio 2019 中，選取 [檔案] -> [新增] -> [專案]。

2. 在 [專案] 下拉式篩選器中，選取 [C++]、[Windows] 和 [WinUI]。

3. 選取 [ **桌面) 專案類型] 中 (WinUI 3 的空白應用程式，** 然後按 **[下一步]**。

    ![建立新專案精靈的另一個螢幕擷取畫面，其中已醒目提示空白應用程式已封裝 (桌面中的 Win UI) 選項。](images/WinUI3-newproject-cpp.png)

4. 輸入專案名稱，視需要選擇任何其他選項，然後按一下 [建立]。

5. 在下列對話方塊中，將 **目標版本** 設為 Windows 10，20004版 (組建 19041) 和 **最低版本** Windows 10 版本 1809 (組建 17763) 然後按一下 **[確定]**。

    ![目標和最低版本](images/WinUI3-minversion.png)

6. 此時，Visual Studio 會產生兩個專案：

    * **專案名稱 (桌面)** ：此專案包含您的應用程式程式碼。 **App.xaml** 和各種 **App** 程式碼檔案會定義代表您應用程式執行個體的 `Application` 類別，而 **MainWindow.xaml** 和各種 **MainWindow** 程式碼檔案會定義 `MainWindow` 類別來代表您應用程式所顯示的主要視窗。 這些類別衍生自 **Microsoft.UI.Xaml** 命名空間 (由 WinUI 提供) 中的類型。

        ![Visual Studio 的螢幕擷取畫面，其中顯示 [方案總管] 窗格和主要 Windows XAML 檔案的內容。](images/WinUI-csharp-appproject.png)

    * **專案名稱 (封裝)** ：這是 [Windows 應用程式封裝專案](/windows/msix/desktop/desktop-to-uwp-packaging-dot-net)，其已設定為將應用程式建置到 [MSIX 套件](/windows/msix/overview)。 這可提供新式部署體驗，透過套件擴充功能與 Windows 10 功能整合的能力，還有更多功能。 此專案包含您應用程式的[封裝資訊清單](/uwp/schemas/appxpackage/uapmanifestschema/schema-root)，而且是您解決方案預設的啟始專案。

        ![Visual Studio 的另一個螢幕擷取畫面，其中顯示 [方案總管] 窗格和套件應用程式 x 資訊清單檔案的內容。](images/WinUI-cpp-packageproject.png)

7. 若要將新項目新增至您的應用程式專案，請以滑鼠右鍵按一下 [方案總管] 中的 **專案名稱 (桌面)** 專案節點，然後選取 [新增] -> [新增項目]。 在 [新增項目] 對話方塊中選取 [WinUI] 索引標籤，選擇您要新增的專案，然後按一下 [新增]。 如需可用項目的詳細資訊，請參閱 [WinUI 3 的項目範本](winui-project-templates-in-visual-studio.md#item-templates-for-winui-3)。

    ![新增項目](images/winui3-addnewitem-cpp.png)

8. 建立並執行您的解決方案，以確認應用程式可在沒有錯誤的情況下執行。

   > [!NOTE]
   > 只有封裝的專案會啟動，因此請確定已將其設定為啟始專案。


## <a name="localizing-your-winui-desktop-app"></a>將 WinUI 桌面應用程式當地語系化

若要在 WinUI 傳統型應用程式中支援多種語言，並確保封裝專案的適當當地語系化，請將適當的資源新增至專案 (查看 [應用程式資源和資源管理系統](/windows/uwp/app-resources/)) ，並在專案的檔案中宣告每個支援的語言 `package.appxmanifest` 。 建置專案時，會將指定的語言新增至產生的應用程式資訊清單 (`AppxManifest.xml`)，並使用對應的資源。

1. 在文字編輯器中開啟 wapproj 的 `package.appxmanifest`，並找出下列區段：

    ```xml
    <Resources>
        <Resource Language="x-generate"/>
    </Resources>
    ```

2. 針對每個支援的語言，將 `<Resource Language="x-generate">` 取代為 `<Resource />` 元素。 例如，下列標記會指定 "en-US" 和 "es-ES" 當地語系化的資源可供使用：

    ```xml
    <Resources>
        <Resource Language="en-US"/>
        <Resource Language="es-ES"/>
    </Resources>
    ```


## <a name="known-issues-and-limitations"></a>已知的問題和限制

請參閱[WINDOWS UI 程式庫 3-Project 留尼旺島 0.5](index.md)的[限制和已知問題](index.md#limitations-and-known-issues)一節。

## <a name="related-topics"></a>相關主題

[Windows UI 程式庫 3-Project 留尼旺島0。5](index.md)
