---
author: PatrickFarley
ms.assetid: 78D833B9-E528-4BCA-9C48-A757F17E6C22
title: Windows 應用程式認證套件
description: 若要讓您的 app 能順利在 Microsoft Store 上發行或成為 Windows 認證，驗證和測試在本機送出以進行認證之前。 本主題示範如何安裝和執行 Windows 應用程式認證套件。
ms.author: pafarley
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10，uwp，應用程式認證
ms.localizationpriority: medium
ms.openlocfilehash: b7a72a89704aa3768cc43cdfbb75b620bae303e3
ms.sourcegitcommit: 914b38559852aaefe7e9468f6f53a7465bf36e30
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/06/2018
ms.locfileid: "3390194"
---
# <a name="windows-app-certification-kit"></a>Windows 應用程式認證套件



若要取得您的應用程式[Windows 認證](https://msdn.microsoft.com/windows/desktop/jj134964.aspx)或發行[至 Microsoft Store](https://msdn.microsoft.com/library/windows/apps/Hh694062)中準備，您應該驗證和測試在本機上第一次。 本主題示範如何安裝和執行[Windows 應用程式認證套件](http://go.microsoft.com/fwlink/p/?LinkID=309666)，以確保您的應用程式是安全且有效率。

## <a name="prerequisites"></a>先決條件

測試通用 Windows app 的先決條件：

-   您必須安裝和執行 Windows 10。
-   您必須安裝 [Windows 應用程式認證套件 10 版]( http://go.microsoft.com/fwlink/p/?LinkID=309666)，此套件包含在適用於 Windows 10 的 Windows 軟體開發套件 (SDK) 中。
-   您必須[啟用您的裝置以進行開發](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development)。
-   您必須將要測試的 Windows 應用程式部署至電腦。

**關於就地升級的附註**

安裝更新版本的 [Windows 應用程式認證套件]( http://go.microsoft.com/fwlink/p/?LinkID=309666)，將會取代電腦上安裝的任何舊版套件。

## <a name="validate-your-windows-app-using-the-windows-app-certification-kit-interactively"></a>以互動方式使用 Windows 應用程式認證套件來驗證 Windows 應用程式

1.  從 **\[開始\]** 功能表中，搜尋 **\[應用程式\]**，找到 **\[Windows 套件\]**，然後按一下 **\[Windows 應用程式認證套件\]**。

2.  從 [Windows 應用程式認證套件] 中，選取您要執行的驗證類別。 例如：如果您要驗證 Windows 應用程式，請選取 **\[驗證 Windows 應用程式\]**。

    您可以直接瀏覽到要測試的 app，或從 UI 中的清單中選擇 app。 首次執行 Windows 應用程式認證套件時，UI 會列出已安裝在您電腦上的所有 Windows 應用程式。 其後每次執行時，UI 將會顯示您最近已驗證過的 Windows 應用程式。 如果沒有列出您要測試的 app，可以按一下 **\[我的 app 未列在裡面\]**，以取得系統上已安裝的所有 app 的完整清單。

3.  輸入或選取要測試的 app 之後，請按一下 **\[下一步\]**。

4.  在下一個畫面中，您將會看到與您要測試之應用程式類型對應的測試工作流程。 若清單中的測試呈現灰色，表示該測試不適用於您的環境。 例如，若您在 Windows 7 上測試 Windows 10 應用程式，只有靜態測試會套用到工作流程。 請注意 Microsoft 網上商店，可能會套用來自此工作流程的所有測試。 選取要執行的測試，然後按一下 **\[下一步\]**。

    Windows 應用程式認證套件隨即開始驗證該應用程式。

5.  測試之後，在提示字元輸入您要儲存測試報告的資料夾路徑。

    Windows 應用程式認證套件會建立一個 HTML 以及一份 XML 報告，並將它儲存到這個資料夾。

6.  開啟報告檔案，然後檢閱測試結果。

**注意** 如果您使用的是 Visual Studio，可以在建立 app 套件時執行 Windows 應用程式認證套件。 若要深入了解，請參閱[封裝 UWP app](https://msdn.microsoft.com/library/windows/apps/Mt627715)。

 

## <a name="validate-your-windows-app-using-the-windows-app-certification-kit-from-a-command-line"></a>從命令列使用 Windows 應用程式認證套件來驗證 Windows app

**重要** Windows 應用程式認證套件必須在使用中的使用者工作階段內容中執行。

1.  在命令視窗中，瀏覽到包含 Windows 應用程式認證套件的目錄。

    **注意** 預設路徑為 C:\\Program Files\\Windows Kits\\10\\App Certification Kit\\。

2.  依序輸入下列命令，以測試電腦上已安裝的 app：

    `appcert.exe reset`

    `appcert.exe test -packagefullname [package full name] -reportoutputpath [report file name]`

    或者，若應用程式未安裝，您可以使用下列命令。 Windows 應用程式認證套件將開啟套件，並套用適當的測試工作流程：

    `appcert.exe reset`

    `appcert.exe test -appxpackagepath [package path] -reportoutputpath [report file name]`

3.  測試完成之後，開啟名為 `[report file name]` 的報告檔案，然後檢閱測試結果。

**注意** Windows 應用程式認證套件可以從服務中執行，但是該服務必須在使用中的使用者工作階段內初始套件處理程序，而且無法在 Session0 中執行。

**注意** 如需 Windows 應用程式認證套件命令列的詳細資訊，請輸入下列命令 `appcert.exe /?`

## <a name="testing-with-a-low-power-computer"></a>使用低功率電腦進行測試

Windows 應用程式認證套件的效能測試閾值是以低功率電腦的效能為基礎。

執行測試之電腦的特性會影響測試結果。 若要判斷您的應用程式的效能是否符合[Microsoft Store 原則](https://msdn.microsoft.com/library/windows/apps/Dn764944)，我們建議您測試您的應用程式在低功率電腦上，例如 Intel Atom 處理器電腦搭配 1366x768 （或更高版本） 的螢幕解析度與旋轉式硬碟磁碟機 （而非固態硬碟）。

隨著低功率電腦不斷演進，其效能特性可能會隨時間改變。 請參閱最新的[Microsoft Store 原則](https://msdn.microsoft.com/library/windows/apps/Dn764944)，並測試您的 app 與最新版本的 Windows 應用程式認證套件，以確保您的 app 符合最新的效能需求。

## <a name="related-topics"></a>相關主題

* [Windows 應用程式認證套件測試](windows-app-certification-kit-tests.md)
* [Microsoft Store 原則](https://msdn.microsoft.com/library/windows/apps/Dn764944)
 

 




