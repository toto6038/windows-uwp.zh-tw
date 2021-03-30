---
description: 本指南將說明如何開始使用 WinUI 3 UI 來建立 UWP 應用程式。
title: '開始使用適用于 UWP 應用程式的 WinUI 3 (Preview) '
ms.date: 03/19/2021
ms.topic: article
keywords: windows 10, uwp, winui
ms.localizationpriority: high
ms.custom: 19H1
ms.openlocfilehash: 0f6f8a17a4f3d6ca684854a64de05da87b27c44c
ms.sourcegitcommit: 7f2a09e8d5d37cb5860a5f2ece5351ea6907b94c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/29/2021
ms.locfileid: "105730532"
---
# <a name="get-started-with-winui-3-for-uwp-apps-preview"></a>開始使用適用于 UWP 應用程式的 WinUI 3 (Preview) 

您可以使用隨附于 [WinUI 3-Project 留尼旺島 0.5 Preview](release-notes/winui3-project-reunion-0.5-preview.md)的專案範本，建立 WinUI 3 的通用 WINDOWS 平臺 (UWP) 應用程式。 當您使用這些專案範本來建立應用程式時，應用程式的整個使用者介面都會使用 WinUI 3 提供的視窗、控制項和樣式來實作。 如需受支援 WinUI 3 專案範本的完整清單，請參閱[適用於 WinUI 3 的專案範本](winui-project-templates-in-visual-studio.md#project-templates-for-winui-3)。

WinUI 3 隨附于專案留尼旺島套件中。 如需有關專案留尼旺島的詳細資訊，請參閱 [使用 Project 留尼旺島0.5 建立桌面 Windows 應用程式](../../project-reunion/index.md)。

> [!NOTE] 
> WinUI 3 建立 UWP 應用程式的支援目前為預覽狀態，而且不是生產環境就緒。 您將無法將 WinUI 3 UWP 應用程式寄送給 Microsoft Store。

## <a name="prerequisites"></a>必要條件

若要使用 WinUI 3 for UWP Preview 專案範本，請遵循「 [設定您的開發環境](../../project-reunion/get-started-with-project-reunion.md#set-up-your-development-environment) 指南」中的指示來設定您的開發電腦，以進行 project 留尼旺島。 

> [!NOTE]
> 您無法使用專案留尼旺島 0.5 VSIX 來建立 WinUI 3 UWP 應用程式。 您必須下載 [Project 留尼旺島 0.5 **Preview** VSIX](https://aka.ms/projectreunion/previewdownload) 來取得 uwp Preview 專案範本，並使用 WinUI 3 建立 uwp 應用程式。 

## <a name="create-a-winui-3-app-in-uwp-for-c"></a>針對 C# 建立 "WinUI 3 app in UWP"

1. 使用 Visual Studio 2019 建立新的專案。
   - 如果 Visual Studio 已在執行中，請選取 [檔案]  ->  [新增]  ->  [專案]。

       :::image type="content" source="images/WinUI-and-UWP/vs2019-menu-file-new-project.png" alt-text="Visual Studio 2019 - 檔案 -> 新增 -> 專案功能表":::

   - 否則，請啟動 Visual Studio 然後選取 [建立新專案]。

       :::image type="content" source="images/WinUI-and-UWP/vs2019-splash-new-project.png" alt-text="Visual Studio 2019 - 建立新專案":::

2. 在 [建立新專案] 對話方塊中，分別從專案下拉式篩選器選取 [C#]、[Windows] 和 [WinUI]。

3. **在 UWP) -Preview 專案類型中，選取 (WinUI 的空白應用程式**，然後按 **[下一步]**。

    :::image type="content" source="images/WinUI-and-UWP/vs2019-create-new-project-dialog.png" alt-text="Visual Studio 2019 - 建立新專案對話方塊":::

4. 輸入專案名稱，視需要選擇任何其他選項，然後按一下 [建立]。

    :::image type="content" source="images/WinUI-and-UWP/vs2019-configure-new-project-dialog.png" alt-text="[設定您的新專案] 對話方塊的螢幕擷取畫面，其中已醒目提示 [位置] 文字方塊和 [建立] 選項。":::

5. 在下列對話方塊中，將 **目標版本** 設為 Windows 10，1903版 (組建 18362) 和 **最低版本** Windows 10 版本 1809 (組建 17763) 然後按一下 **[確定]**。

    :::image type="content" source="images/WinUI-min-target-version.png" alt-text="目標和最低版本對話方塊":::

6. Visual Studio 會使用下列物件，產生 **WinUI in UWP** 專案：

    - ***專案名稱* (通用 Windows)** ：包含您的應用程式程式碼。 這是您專案解決方案的預設啟始專案。

        :::image type="content" source="images/WinUI-and-UWP/vs2019-project.png" alt-text="已醒目提示通用 Windows 解決方案的方案總管面板螢幕擷取畫面。":::

    - **Package.appxmanifest**：包含系統部署、顯示或更新應用程式所需的資訊。 如需詳細資訊，請參閱[應用程式封裝資訊清單](/uwp/schemas/appxpackage/appx-package-manifest)

        :::image type="content" source="images/WinUI-and-UWP/vs2019-file-package-manifest.png" alt-text="Visual Studio 2019 - 應用程式封裝資訊清單":::

    - **App.xaml/App.xaml.cs**：程式碼檔案，定義 `Application` 類別，其代表您的應用程式執行個體。

        :::image type="content" source="images/WinUI-and-UWP/vs2019-file-app-xaml.png" alt-text="Visual Studio 2019 - App.xaml 檔案":::

        :::image type="content" source="images/WinUI-and-UWP/vs2019-file-app-xaml-cs.png" alt-text="Visual Studio 2019 - App.xaml.cs 檔案":::

    - **MainPage.xaml/MainPage.xaml.cs**：程式碼檔案，代表您的應用程式所顯示主要視窗。 這些類別衍生自 **Microsoft.UI.Xaml** 命名空間 (由 WinUI 提供) 中的類型。

        :::image type="content" source="images/WinUI-and-UWP/vs2019-file-mainpage-xaml.png" alt-text="Visual Studio 2019 - MainPage.xaml 檔案":::

        :::image type="content" source="images/WinUI-and-UWP/vs2019-file-mainpage-xaml-cs.png" alt-text="Visual Studio 2019 - MainPage.xaml.cs 檔案":::

7. 若要將新項目新增至您的應用程式專案，請以滑鼠右鍵按一下 [方案總管] 中的 **專案名稱 (通用 Windows)** 專案節點，然後選取 [新增]  ->  [新增項目]。 在 [新增項目] 對話方塊中選取 [WinUI] 索引標籤，選擇您要新增的專案，然後按一下 [新增]。 如需可用項目的詳細資訊，請參閱 [WinUI 3 的項目範本](winui-project-templates-in-visual-studio.md#item-templates-for-winui-3)。

    :::image type="content" source="images/WinUI-and-UWP/vs2019-add-new-item-dialog.png" alt-text="Visual Studio 2019 - 新增新項目對話方塊":::

8. 建置、部署和啟動您的應用程式，查看其外觀。

    1. 您可以在本機電腦、模擬器或遠端裝置上進行應用程式的偵錯。 從下拉式清單中選取您的目標裝置。

        :::image type="content" source="images/WinUI-and-UWP/vs2019-menu-target-device.png" alt-text="[本機電腦] 下拉式清單的螢幕擷取畫面。":::

    1. 按下 F5 鍵，按一下 [建置] 按鈕，或選取 [偵錯 -> 開始偵錯]，以建置並執行您的解決方案，然後確認應用程式執行無誤。

        :::image type="content" source="images/WinUI-and-UWP/vs2019-project-running.png" alt-text="執行中應用程式的螢幕擷取畫面，顯示 [按這裡] 按鈕。":::

## <a name="known-issues-and-limitations"></a>已知的問題和限制

請參閱[WINDOWS UI 程式庫 3-Project 留尼旺島 0.5 Preview](release-notes/winui3-project-reunion-0.5-preview.md)的[限制和已知問題](index.md#limitations-and-known-issues)一節。

## <a name="related-topics"></a>相關主題

- [Windows UI 程式庫 3-Project 留尼旺島 0.5 Preview](release-notes/winui3-project-reunion-0.5-preview.md)
- [建立您的第一個應用程式](/windows/uwp/get-started/your-first-app)
