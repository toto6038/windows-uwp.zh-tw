---
title: "取得 GitHub 中的通用 Windows 平台 (UWP) 範例"
description: "了解如何下載 GitHub 中的 UWP 功能範例"
author: JoshuaPartlow
translationtype: Human Translation
ms.sourcegitcommit: 27f98124d8360c3d0266645660b35cf040af0ef0
ms.openlocfilehash: dbd2d5d7924cfbfd48d20f48ccfcd4bfe62db645

---

#取得 GitHub 中的通用 Windows 平台 (UWP) 範例
UWP app 範例都可透過 GitHub 上的存放庫取得。 如果這是您第一次使用 UWP，您可以先從 [Microsoft/Windows-universal-samples](https://github.com/Microsoft/Windows-universal-samples) 存放庫開始，其中包含示範所有 UWP 功能及其 API 使用模式的範例。  
![GitHub UWP 範例存放庫](images/GitHubUWPSamplesPage.png) 使用開發人員中心的[範例](https://developer.microsoft.com/windows/samples)區段可以找到額外的範例。  

##下載程式碼
若要下載範例，請移至[存放庫](https://github.com/Microsoft/Windows-universal-samples)並選取 \[Clone or download (複製或下載)\]，然後選取 \[Download ZIP (下載 ZIP)\]。 或者按一下[這裡](https://github.com/Microsoft/Windows-universal-samples/archive/master.zip)。

Zip 檔案一定會有最新的範例。 您不需要 GitHub 帳戶就可以下載。 當 SDK 更新發行時或如果您想要挑選任何最近的變更/新增項目，只要查看最新的 zip 檔案即可。

![範例下載](images/SamplesDownloadButton.png)


> **注意**：UWP 範例需有 Visual Studio 2015 和 Windows SDK 才能開啟、建置以及執行。 如果您沒有安裝 Visual Studio，可以從[這裡](http://go.microsoft.com/fwlink/p/?LinkID=280676)取得支援建置 UWP 應用程式的免費 Visual Studio 2015 社群版本。  
>
> 此外，請務必將整個封存而不只是個別的範例解壓縮。 所有相關的範例都在封存的 SharedContent 資料夾中。 UWP 功能範例在 Visual Studio 中使用連結的檔案，以減少重複的常見檔案，包括範例範本檔案與影像資產。 這些常見的檔案都會儲存在存放庫根目錄的 SharedContent 資料夾中，而且使用連結指向專案檔案。

下載 zip 檔案之後，請在 Visual Studio 中開啟範例︰

1.  將封存解壓縮之前，請以滑鼠右鍵按一下封存，選取 \[內容\] > \[解除封鎖\] > \[套用\]。 然後，將封存解壓縮到您電腦上的本機資料夾。

    ![解壓縮的封存](images/SamplesUnzip1.png)
2.  在範例資料夾中，您會看到許多資料夾，每一個都包含 UWP 功能範例。

    ![範例資料夾](images/SamplesUnzip2.png)

3.  選取範例，例如高度表，您會看到多個資料夾指出支援的語言。

    ![語言資料夾](images/SamplesUnzip3.png)

4.  選取您想要使用的語言，例如適用於 C\# 的 CS，您會看到一個可以在 Visual Studio 中開啟的 Visual Studio 方案檔。

    ![VS 方案](images/SamplesUnzip4.png)

## 提供意見反應、提出問題，並報告問題

如果您有問題，只要使用存放庫的 \[Issues\] 索引標籤即可建立新的問題，我們將會盡可能提供幫助。

![意見反應影像](images/GitHubUWPSamplesFeedback.png)



<!--HONumber=Nov16_HO1-->


