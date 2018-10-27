---
title: 取得 UWP app 範例
description: 了解如何下載 GitHub 中的 UWP 程式碼範例
author: JoshuaPartlow
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, sample code, code samples, 範例程式碼, 程式碼範例
ms.assetid: 393c5a81-ee14-45e7-acd7-495e5d916909
ms.localizationpriority: medium
ms.openlocfilehash: ef8f99ade3fa5e4d9f12b8670bf22242e7c4e585
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/27/2018
ms.locfileid: "5699418"
---
# <a name="get-uwp-app-samples"></a>取得 UWP app 範例

通用 Windows 平台 (UWP) app 範例可透過 GitHub 上的存放庫取得。 如需可搜尋的分類清單，請參閱[範例](https://developer.microsoft.com/windows/samples "Dev Center samples")或瀏覽 [Microsoft/Windows-universal-samples](https://github.com/Microsoft/Windows-universal-samples "通用 Windows 平台應用程式範例 GitHub 存放庫")存放庫，其包含示範所有 UWP 功能及其 API 使用模式的範例。  
![GitHub UWP 範例存放庫](images/GitHubUWPSamplesPage.png)

## <a name="download-the-code"></a>下載程式碼

若要下載範例，請移至[存放庫](https://github.com/Microsoft/Windows-universal-samples "通用 Windows 平台應用程式範例 GitHub 存放庫")並選取 **\[Clone or download\]** (複製或下載)，然後是 **\[下載 ZIP\]**。 或者，只要按一下[這裡](https://github.com/Microsoft/Windows-universal-samples/archive/master.zip "通用 Windows 平台應用程式範例壓縮檔下載")。

壓縮檔一定會有最新的範例。 您不需要 GitHub 帳戶就可以下載。 當 SDK 更新發行時或如果您想要挑選任何最近的變更/新增項目，只要查看最新的 zip 檔案即可。

![範例下載](images/SamplesDownloadButton.png)


> [!NOTE]
> UWP 範例需 Visual Studio 2015 或更新版本和 Windows SDK 才能開啟、建置及執行。 您可以取得免費的 Visual Studio 社群支援以在[此處](http://go.microsoft.com/fwlink/p/?LinkID=280676 "Windows 開發工具下載")建置 UWP app。  
>
> 此外，請務必將整個封存而不只是個別的範例解壓縮。 所有相關的範例都在封存的 SharedContent 資料夾中。 UWP 功能範例在 Visual Studio 中使用連結的檔案，以減少重複的常見檔案，包括範例範本檔案與影像資產。 這些常見的檔案都會儲存在存放庫根目錄的 SharedContent 資料夾中，而且使用連結指向專案檔案。

下載 zip 檔案之後，請在 Visual Studio 中開啟範例︰

1.  將封存解壓縮之前，請以滑鼠右鍵按一下封存，選取 \[內容\]**** > ** \[解除封鎖\]** > **** \[套用\]。 然後，將封存解壓縮到您電腦上的本機資料夾。

    ![解壓縮的封存](images/SamplesUnzip1.png)
2.  在範例資料夾中，您會看到許多資料夾，每一個都包含 UWP 功能範例。

    ![範例資料夾](images/SamplesUnzip2.png)

3.  選取範例，例如高度表，您會看到多個資料夾指出支援的語言。

    ![語言資料夾](images/SamplesUnzip3.png)

4.  選取您想要使用的語言，例如適用於 C\# 的 CS，您會看到一個可以在 Visual Studio 中開啟的 Visual Studio 方案檔。

    ![VS 方案](images/SamplesUnzip4.png)

## <a name="give-feedback-ask-questions-and-report-issues"></a>提供意見反應、提出問題，並報告問題

如果您有問題，只要使用存放庫的 \[Issues\] 索引標籤即可建立新的問題，我們將會盡可能提供幫助。

![意見反應影像](images/GitHubUWPSamplesFeedback.png)
