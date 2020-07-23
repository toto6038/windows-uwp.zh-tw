---
description: 本指南將說明如何開始使用 WinUI 3 UI 來建立 UWP 應用程式。
title: 開始使用適用於 UWP 應用程式的 WinUI 3
ms.date: 07/13/2020
ms.topic: article
keywords: windows 10, uwp, winui
ms.localizationpriority: high
ms.custom: 19H1
ms.openlocfilehash: 0b62595850c2b3354c59d199f879d7e3b55cd3a2
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/20/2020
ms.locfileid: "86493916"
---
# <a name="get-started-with-winui-3-for-uwp-apps"></a>開始使用適用於 UWP 應用程式的 WinUI 3

WinUI 3 預覽版 2 引進了新的專案範本，可讓您使用完全建置在 WinUI 上的使用者介面，建立通用 Windows 平台 (UWP) 應用程式。 當您使用這些專案範本來建立應用程式時，應用程式的整個使用者介面都會使用 WinUI 3 提供的視窗、控制項和樣式來實作。 如需受支援 WinUI 3 專案範本的完整清單，請參閱[適用於 WinUI 3 的專案範本](index.md#project-templates-for-winui-3)。

## <a name="prerequisites"></a>必要條件

若要使用本文所述適用於 UWP 的 WinUI 3 專案範本，請設定您的開發電腦以及[安裝 WinUI 3 預覽版 2](index.md#install-winui-3-preview-2)。

## <a name="create-a-winui-3-app-in-uwp-for-c"></a>針對 C# 建立 "WinUI 3 app in UWP"

1. 使用 Visual Studio 2019 建立新的專案。
   - 如果 Visual Studio 已在執行中，請選取 [檔案]  ->  [新增]  ->  [專案]。

   :::image type="content" source="images/WinUI-and-UWP/vs2019-menu-file-new-project.png" alt-text="Visual Studio 2019 - 檔案 -> 新增 -> 專案功能表":::

   - 否則，請啟動 Visual Studio 然後選取 [建立新專案]。

   :::image type="content" source="images/WinUI-and-UWP/vs2019-splash-new-project.png" alt-text="Visual Studio 2019 - 建立新專案":::

2. 在 [建立新專案] 對話方塊中，分別從專案下拉式篩選器選取 [C#]、[Windows] 和 [WinUI]。

3. 選取 [空白應用程式 (WinUI in UWP)] 專案類型，然後按 [下一步]。

:::image type="content" source="images/WinUI-and-UWP/vs2019-create-new-project-dialog.png" alt-text="Visual Studio 2019 - 建立新專案對話方塊":::

4. 輸入專案名稱，視需要選擇任何其他選項，然後按一下 [建立]。

:::image type="content" source="images/WinUI-and-UWP/vs2019-configure-new-project-dialog.png" alt-text="Visual Studio 2019 - 設定新專案對話方塊":::

5. 在下列對話方塊中，將 [目標版本] 設定為 Windows 10 1903 版 (組建 18362)，並將 [最低版本] 設定為 Windows 10 1803 版 (組建 17134)，然後按一下 [確定]。

:::image type="content" source="images/WinUI-min-target-version.png" alt-text="目標和最低版本對話方塊":::

6. Visual Studio 會使用下列物件，產生 **WinUI in UWP** 專案：

    - ***專案名稱* (通用 Windows)** ：包含您的應用程式程式碼。 這是您專案解決方案的預設啟始專案。

    :::image type="content" source="images/WinUI-and-UWP/vs2019-project.png" alt-text="Visual Studio 2019 - 設定新專案對話方塊":::

    - **Package.appxmanifest**：包含系統部署、顯示或更新應用程式所需的資訊。 如需詳細資訊，請參閱[應用程式封裝資訊清單](https://docs.microsoft.com/uwp/schemas/appxpackage/appx-package-manifest)

    :::image type="content" source="images/WinUI-and-UWP/vs2019-file-package-manifest.png" alt-text="Visual Studio 2019 - 應用程式封裝資訊清單":::

    - **App.xaml/App.xaml.cs**：程式碼檔案，定義 `Application` 類別，其代表您的應用程式執行個體。

    :::image type="content" source="images/WinUI-and-UWP/vs2019-file-app-xaml.png" alt-text="Visual Studio 2019 - App.xaml 檔案":::

    :::image type="content" source="images/WinUI-and-UWP/vs2019-file-app-xaml-cs.png" alt-text="Visual Studio 2019 - App.xaml.cs 檔案":::

    - **MainPage.xaml/MainPage.xaml.cs**：程式碼檔案，代表您的應用程式所顯示主要視窗。 這些類別衍生自 **Microsoft.UI.Xaml** 命名空間 (由 WinUI 提供) 中的類型。

    :::image type="content" source="images/WinUI-and-UWP/vs2019-file-mainpage-xaml.png" alt-text="Visual Studio 2019 - MainPage.xaml 檔案":::

    :::image type="content" source="images/WinUI-and-UWP/vs2019-file-mainpage-xaml-cs.png" alt-text="Visual Studio 2019 - MainPage.xaml.cs 檔案":::

7. 若要將新項目新增至您的應用程式專案，請以滑鼠右鍵按一下 [方案總管] 中的 **專案名稱 (通用 Windows)** 專案節點，然後選取 [新增]  ->  [新增項目]。 在 [新增項目] 對話方塊中選取 [WinUI] 索引標籤，選擇您要新增的專案，然後按一下 [新增]。 如需可用項目的詳細資訊，請參閱 [WinUI 3 的項目範本](index.md#item-templates-for-winui-3)。

    :::image type="content" source="images/WinUI-and-UWP/vs2019-add-new-item-dialog.png" alt-text="Visual Studio 2019 - 新增新項目對話方塊":::

8. 建置、部署和啟動您的應用程式，查看其外觀。

    1. 您可以在本機電腦、模擬器或遠端裝置上進行應用程式的偵錯。 從下拉式清單中選取您的目標裝置。

        :::image type="content" source="images/WinUI-and-UWP/vs2019-menu-target-device.png" alt-text="Visual Studio 2019 - 目標裝置功能表":::

    1. 按下 F5 鍵，按一下 [建置] 按鈕，或選取 [偵錯 -> 開始偵錯]，以建置並執行您的解決方案，然後確認應用程式執行無誤。

        :::image type="content" source="images/WinUI-and-UWP/vs2019-project-running.png" alt-text="Visual Studio 2019 - 目標裝置功能表":::

## <a name="known-issues-and-limitations"></a>已知的問題和限制

如需已知問題和限制的清單，請參閱[本節](index.md#preview-2-limitations-and-known-issues)。

## <a name="related-topics"></a>相關主題

- [WinUI 3](index.md)
- [建立您的第一個應用程式](/windows/uwp/get-started/your-first-app)
