---
author: mcleblanc
description: "如果您具有使用 Microsoft Visual Studio 2015 RC 建立的 Windows 10 專案，您在將專案檔更新為適用於 Visual Studio 2015 RTM 的格式時將有兩種選擇。"
title: "將您的 UWP Microsoft Visual Studio 2015 RC 專案更新到 RTM"
ms.assetid: 104E36CE-36DE-4E9C-A944-711C200B44EF
ms.author: markl
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: ad7754a55a34ba3921d9455209cc76ab8ae0757b
ms.lasthandoff: 02/07/2017

---

# <a name="update-your-uwp-microsoft-visual-studio-2015-rc-project-to-rtm"></a>將您的 UWP Microsoft Visual Studio 2015 RC 專案更新到 RTM

\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

如果您具有使用 Microsoft Visual Studio 2015 RC 建立的 Windows 10 專案，您在將專案檔更新為適用於 Visual Studio 2015 RTM 的格式時將有兩種選擇。 建議的方法是在 Visual Studio 2015 RTM 中建立新的 Windows 10 專案，並將檔案複製到其中。 或者，您可以依照進階的文件編輯現有的專案檔，並將其移轉至新的格式。

## <a name="what-you-see-when-you-open-a-windows-10visual-studio-2015-rc-project-in-visual-studio-2015-rtm"></a>您在 Visual Studio 2015 RTM 中開啟 Windows 10Visual Studio 2015 RC 專案時所將看見的內容

當您在 Visual Studio 2015 RTM 中開啟 Windows 10 Visual Studio 2015 RC 專案時，您會在 [**方案總管**] 中看見「需要更新」訊息。

![需要更新](images/vsrc-to-rtm/solution-explorer.png)

如果您在 [**方案總管**] 中存取專案的操作功能表，並選擇 [**重新載入專案**]，您將會看到這個對話方塊。

![需要 Visual Studio 更新](images/vsrc-to-rtm/reload-project.png)

## <a name="create-a-new-project-and-copy-files-into-it"></a>建立新專案並將檔案複製到其中

1.  啟動 Visual Studio 2015 RTM，並建立新的空白應用程式 (Windows 通用) 專案。 請記住，您的新專案依預設會建置以「通用」裝置系列為目標的應用程式套件 (appx 檔案)。 如果您要以一或多個特定裝置系列為目標，請加以變更。
2.  在您的 Visual Studio 2015 RC 專案中，找出您想要複製過去的所有原始程式碼檔案及視覺資產檔案。 使用 [檔案總管]，將資料模型、檢視模型、視覺資產、資源字典、資料夾結構，以及任何您想要的其他項目 (包括 AssemblyInfo.cs) 複製到新專案。 視需要在磁碟上複製或建立子資料夾。
3.  將檢視 (例如 MainPage.xaml 和 MainPage.xaml.cs) 一併複製到新專案。 同樣地，請視需要建立新的子資料夾，然後從專案移除現有的檢視。 但是在您覆寫或移除 Visual Studio 產生的檢視之前，請保留一份複本，因為可能稍後可用來供參考。
4.  在 [**方案總管**] 中，確定 [**顯示所有檔案**] 已切換成開啟。 選取您複製的檔案，在這些檔案上按一下滑鼠右鍵，然後按一下 [加入至專案]****。 這將會自動包含它們的容器資料夾。 然後您可以視需要將 [**顯示所有檔案**] 切換成關閉。 如果您想要的話，也可以選擇替代的工作流程，就是先在 Visual Studio [**方案總管**] 中建立任何必要的子資料夾，然後使用 [**加入現有項目**]命令。 仔細檢查您視覺資產的 [**建置動作**] 是否已設定為 [**內容**]，而 [**複製到輸出目錄**] 是否已設定為 [**不要複製**]。
5.  將參考新增至您在 RC 專案中參考的任何延伸 SDK，然後將您先前的 Package.appxmanifest 中的任何變更 (例如您已宣告的任何功能) 複製到新 RTM 專案中的檔案。

## <a name="advanced-edit-your-existing-project-files"></a>進階：編輯您現有的專案檔

Visual Studio 2015 RC 與 Visual Studio 2015 RTM 的 Windows 10 專案格式有一項重大差異，即 RTM 格式使用 [NuGet](http://docs.nuget.org/) 第 3 版。 如果您想要手動更新您的專案，請留意此差異。

如果您想要手動更新您的專案，或如果您想知道 Visual Studio 2015 RC 與 Visual Studio 2015 RTM 的專案格式有何差異，請參閱[將 app 移轉至通用 Windows 平台 (UWP)](http://msdn.microsoft.com/library/mt148501.aspx)。


