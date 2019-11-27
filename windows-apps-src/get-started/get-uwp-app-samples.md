---
title: 取得 UWP 應用程式範例
description: 了解如何從 GitHub 下載 UWP 程式碼範例。
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, sample code, code samples, 範例程式碼, 程式碼範例
ms.assetid: 393c5a81-ee14-45e7-acd7-495e5d916909
ms.localizationpriority: medium
ms.openlocfilehash: ac3c99bc364e81386a362f1d1b5530bee9d462c4
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259518"
---
# <a name="get-uwp-app-samples"></a>取得 UWP 應用程式範例

通用 Windows 平台 (UWP) 應用程式範例可在 GitHub 上的存放庫取得。 如需可搜尋的分類清單，請參閱[範例](https://developer.microsoft.com/windows/samples)，或者，您也可以瀏覽 [Microsoft/Windows-universal-sample 存放庫](https://github.com/Microsoft/Windows-universal-samples "通用 Windows 平台應用程式範例 GitHub 存放庫")。 Windows-universal-samples 存放庫包含示範所有 UWP 功能及其 API 使用模式的範例。

![GitHub UWP 範例存放庫](images/GitHubUWPSamplesPage.png)

## <a name="download-the-code"></a>下載程式碼

若要下載範例，請移至[存放庫](https://github.com/Microsoft/Windows-universal-samples "通用 Windows 平台應用程式範例 GitHub 存放庫")。 選取 [Clone or download]  \(複製或下載\)，然後選取 [Download ZIP]  \(下載 ZIP\)。 

![範例下載](images/SamplesDownloadButton.png)

您可以從本文章[下載範例](https://github.com/Microsoft/Windows-universal-samples/archive/master.zip "通用 Windows 平台應用程式範例壓縮檔下載")。

範例下載 .zip 檔案一律會有最新範例。 您不需要 GitHub 帳戶就可以下載該檔案。 當 SDK 更新發行時或如果您想要挑選任何最近的變更和新增項目，只要下載最新的 zip 檔案即可。

> [!NOTE]
> 若要開啟、建置和執行 UWP 範例，您必須具有 Visual Studio 2015 或更新版本以及 Windows SDK。 您可以取得[免費的 Visual Studio Community](https://www.microsoft.com/?ref=go)。 Visual Studio Community 支援建置 UWP 應用程式。  
>
> 若要讓範例正確運作，請務必將整個封存而不只是個別的範例解壓縮。 所有相關的範例都在封存的 SharedContent 資料夾中。 UWP 功能範例在 Visual Studio 中使用連結的檔案，以減少重複的常見檔案，包括範例範本檔案與影像資產。 常見檔案會儲存在存放庫根目錄的 SharedContent 資料夾中。 專案檔案會使用連結來參照常見檔案。
> 

## <a name="open-the-samples"></a>開啟範例

下載 .zip 檔案之後，請在 Visual Studio 中開啟範例。

1.  將封存解壓縮之前，請以滑鼠右鍵按一下該檔案，然後選取 [屬性]   > [解除封鎖]   > [套用]  。 然後，將封存解壓縮到您電腦上的本機資料夾。

    ![解壓縮的封存](images/SamplesUnzip1.png)
2.  Samples 資料夾中的每個資料夾都包含 UWP 功能範例。

    ![範例資料夾](images/SamplesUnzip2.png)
3.  選取範例，例如 Altimeter。 子資料夾會指出支援的語言。

    ![語言資料夾](images/SamplesUnzip3.png)
4.  選取所要使用語言的資料夾。 在資料夾內容中，您會看到可在 Visual Studio 中開啟的 Visual Studio 解決方案 (.sln) 檔案。 例如，Altimeter.sln  。

    ![VS 方案](images/SamplesUnzip4.png)

## <a name="give-feedback-ask-questions-and-report-issues"></a>提供意見反應、提出問題，並報告問題

如果您有問題，請使用存放庫的 \[Issues\]  索引標籤來建立新的問題。 我們將會盡可能提供幫助。

![意見反應影像](images/GitHubUWPSamplesFeedback.png)
