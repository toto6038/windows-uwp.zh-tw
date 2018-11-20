---
author: normesta
Description: This guide explains how to configure your Visual Studio Solution to edit, debug, and package desktop application.
Search.Product: eADQiWindows 10XVcnh
title: 使用 Visual Studio 封裝的傳統型應用程式
ms.author: normesta
ms.date: 08/30/2017
ms.topic: article
keywords: Windows 10, uwp
ms.assetid: 807a99a7-d285-46e7-af6a-7214da908907
ms.localizationpriority: medium
ms.openlocfilehash: cc71a58594c8794369bedd7f415518100892ff67
ms.sourcegitcommit: ed0304b8a214c03b8aab74b8ef12c9f82b8e3c5f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/19/2018
ms.locfileid: "7287546"
---
# <a name="package-a-desktop-application-by-using-visual-studio"></a>使用 Visual Studio 封裝的傳統型應用程式

您可以使用 Visual Studio 為您的傳統型應用程式產生套件。 然後，您可以將套件發行到 Microsoft Store 或側載它到一或多個電腦上。

最新版的 Visual Studio 提供新版的封裝專案，讓您不必執行以往封裝應用程式過程中需要執行的所有手動步驟。 只需加入封裝專案、參考桌面專案，然後按 F5 對您的應用程式進行偵錯。 不需進行任何手動調整。 這個有效率的全新體驗可以大幅改善舊版 Visual Studio 中所提供的體驗。

>[!IMPORTANT]
>在 Windows 10，版本 1607 開始，引進了建立 Windows 應用程式套件 （否則稱為傳統型橋接器） 的傳統型應用程式的能力，它只能在專案中目標為 Windows 10 年度更新版 (10.0;組建 14393） 或更新版本在 Visual Studio 中的。

## <a name="first-prepare-your-application"></a>首先，準備您的應用程式

開始建立您的應用程式套件之前，先檢閱本指南：[準備封裝傳統型應用程式](desktop-to-uwp-prepare.md)。

<a id="new-packaging-project"/>

## <a name="create-a-package"></a>建立套件

1. 在 Visual Studio 中，開啟包含您的傳統型應用程式專案的方案。

2. 新增 **\[Windows 應用程式封裝專案\]** 專案到您的方案。

   您不需要增加任何程式碼至此專案。 它會自行為您產生套件。 我們將此專案稱為「封裝專案」。

   ![封裝專案](images/desktop-to-uwp/packaging-project.png)

   >[!NOTE]
   >這個專案只會出現在 Visual Studio 2017 15.5 或更高版本。

3. 將這個專案的 **\[目標版本\]** 設定為任何您想要的版本，但請務必將 **\[最小版本\]** 設定為 **\[Windows 10 年度更新版\]**。

   ![封裝版本選取器對話方塊](images/desktop-to-uwp/packaging-version.png)

4. 在封裝專案的 **\[應用程式\]** 資料夾上按一下滑鼠右鍵，然後選擇 **\[加入參考\]**。

   ![加入專案參考](images/desktop-to-uwp/add-project-reference.png)

5. 選擇傳統型應用程式專案，然後選擇 **\[確定\]** 按鈕。

   ![桌面專案](images/desktop-to-uwp/reference-project.png)

   您可以在套件中包括多個傳統型應用程式，但當使用者選擇您的應用程式磚時只有其中一個可以啟動。 在**\[應用程式\]** 節點，以滑鼠右鍵按一下您想要使用者選擇應用程式磚時啟動的應用程式，然後選擇 **\[設為進入點\]**。

   ![設定進入點](images/desktop-to-uwp/entry-point-set.png)

6. 組建封裝專案，以確保未出現任何錯誤。  如果您收到錯誤，請開啟**Configuration Manager** ，並確保您的專案目標相同的平台。

   ![組態管理員](images/desktop-to-uwp/config-manager.png)

7. 使用[建立應用程式套件](../packaging/packaging-uwp-apps.md)精靈來產生 appxupload 檔案。

   您可以將該檔案上傳直接到市集。

**影片**

&nbsp;
> [!VIDEO https://www.youtube.com/embed/fJkbYPyd08w]

## <a name="next-steps"></a>後續步驟

**尋找您的問題解答**

有任何問題嗎？ 請在 Stack Overflow 上發問。 我們的團隊會監視這些[標記](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge)。 您也可以[在此處](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D)詢問我們。

**提供意見反應或功能建議**

請參閱 [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial)。

**執行、 偵錯或測試您的傳統型應用程式**

請參閱[執行、 偵錯以及測試封裝的傳統型應用程式](desktop-to-uwp-debug.md)

**透過新增 UWP Api 來增強您的傳統型應用程式**

請參閱[增強您的 Windows 10 傳統型應用程式](desktop-to-uwp-enhance.md)

**透過新增 UWP 專案和 Windows 執行階段元件來擴充您的傳統型應用程式**

請參閱[使用現代化 UWP 元件擴充您的傳統型應用程式](desktop-to-uwp-extend.md)。

**散布您的應用程式**

請參閱[散發的已封裝的傳統型應用程式](desktop-to-uwp-distribute.md)
