---
title: 取得 Windows 應用程式範例
description: 了解如何從 GitHub 下載 Windows 程式碼範例。
ms.date: 06/30/2020
ms.topic: article
keywords: windows 10, 範例程式碼, 程式碼範例
ms.assetid: 393c5a81-ee14-45e7-acd7-495e5d916909
ms.localizationpriority: medium
ms.openlocfilehash: 285388c4c1b4791e1ca271a853476416b6d13c13
ms.sourcegitcommit: 179f8098d10e338ad34fa84934f1654ec58161cd
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/01/2020
ms.locfileid: "85757615"
---
# <a name="get-windows-app-samples"></a>取得 Windows 應用程式範例

許多官方 Windows 程式碼範例都可在各種 GitHub 存放庫中取得，包括[通用 Windows 平台 (UWP) 應用程式範例](https://github.com/microsoft/Windows-universal-samples)、[Windows 傳統範例](https://github.com/microsoft/Windows-classic-samples)，以及 [Windows 開發人員文件範例](#windows-developer-documentation-samples)集合。 這些範例會示範大部分的 Windows 功能及其 API 使用模式。

:::image type="content" source="images/github-windows-samples-page.png" alt-text="GitHub Windows 通用範例存放庫":::

若要更輕鬆地尋找特定範例，您可以透過[範例瀏覽器](https://docs.microsoft.com/samples/browse/)，瀏覽及搜尋各種 Microsoft 開發人員工具和技術的已分類程式碼範例集合。

:::image type="content" source="images/samples-browser-windows.png" alt-text="Microsoft 範例瀏覽器":::

### <a name="windows-developer-documentation-samples"></a>Windows 開發人員文件範例

以下是專為支援 Windows 開發人員文件所建立的迷你應用程式範例清單。 除非另有說明，否則下列範例是已更新為使用最新 [WinUI 2.4](/windows/apps/winui/winui2/release-notes/winui-2.4) 控制項的所有通用 Windows 平台 (UWP) 應用程式。

- [Rss 助讀程式](https://github.com/Microsoft/Windows-appsample-rssreader) - 擷取 RSS 摘要及檢視文章
- [Family Notes (家庭記事本)](https://github.com/Microsoft/Windows-appsample-familynotes) - 探索不同的輸入形式和使用者感知案例
- [客戶訂單](https://github.com/Microsoft/Windows-appsample-customers-orders-database) - 對企業開發人員有幫助的功能，例如 Azure Active Directory (AAD) 驗證、UI 控制項 (包括資料格)、Sqlite 與 SQL Azure 資料庫整合、Entity Framework 和雲端 API 服務等
- [午餐排程器](https://github.com/Microsoft/Windows-appsample-lunch-scheduler) - 排程您與朋友和同事共用午餐
- [著色本](https://github.com/Microsoft/Windows-appsample-coloringbook) - Windows Ink (包括 Windows Ink 工具列) 及星形控制器 (適用於轉盤裝置，例如 Surface Dial) 功能
- [網路協助程式 (測驗遊戲)](https://github.com/Microsoft/Windows-appsample-networkhelper) - 網路探索與通訊
- [HUE 燈光控制器](https://github.com/Microsoft/Windows-appsample-huelightcontroller) - 使用 Cortana 和藍牙低功耗 (藍牙 LE) 的智能居家自動化功能
- [Marble Maze (大理石迷宮)](https://github.com/Microsoft/Windows-appsample-marble-maze) - 使用 DirectX 的基本3D 遊戲
- [PhotoLab](https://github.com/Microsoft/Windows-appsample-photo-lab) - 檢視和編輯影像檔案

## <a name="download-the-code"></a>下載程式碼

若要下載範例，請移至其中一個 Microsoft 存放庫，例如[通用 Windows 平台 (UWP) 應用程式範例](https://github.com/microsoft/Windows-universal-samples)。 選取 [Clone or download] \(複製或下載\)，然後選取 [Download ZIP] \(下載 ZIP\)。

![範例下載](images/SamplesDownloadButton.png)

範例下載 .zip 檔案一律會有最新範例。 您不需要 GitHub 帳戶就可以下載該檔案。 當 SDK 更新發行時或如果您想要挑選任何最近的變更和新增項目，只要下載最新的 zip 檔案即可。

> [!NOTE]
> 若要開啟、建置和執行 Windows 範例，您必須具有 Visual Studio 和 Windows SDK。 您可以取得[免費的 Visual Studio Community](https://www.microsoft.com/?ref=go)。  
>
> 若要讓範例正確運作，請務必將整個封存而不只是個別的範例解壓縮。 許多範例都取決於 *SharedContent* 資料夾中的常見檔案，並使用連結的檔案 (包括範例範本檔案與影像資產) 來減少重複。

## <a name="open-the-samples"></a>開啟範例

下載 .zip 檔案之後，請在 Visual Studio 中開啟範例。

1. 將封存解壓縮之前，請以滑鼠右鍵按一下該檔案，選取 [屬性] > [解除封鎖] > [套用]。 然後，將封存解壓縮到您電腦上的本機資料夾。

    ![解壓縮的封存](images/SamplesUnzip1.png)

2. Samples 資料夾中的每個資料夾都包含 Windows 功能範例。

    ![範例資料夾](images/SamplesUnzip2.png)

3. 選取範例。 支援的語言會以特定語言的子資料夾來表示。

    ![語言資料夾](images/SamplesUnzip3.png)

4. 選取所要使用語言的資料夾。 在資料夾內容中，您會看到可在 Visual Studio 中開啟的 Visual Studio 解決方案 (.sln) 檔案。

    ![VS 方案](images/SamplesUnzip4.png)

## <a name="give-feedback-ask-questions-and-report-issues"></a>提供意見反應、提出問題，並報告問題

如果您有問題，請使用存放庫的 \[Issues\] 索引標籤來建立新的問題。

![意見反應影像](images/GitHubUWPSamplesFeedback.png)
