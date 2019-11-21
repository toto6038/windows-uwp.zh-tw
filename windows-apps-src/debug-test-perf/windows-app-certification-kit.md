---
ms.assetid: 78D833B9-E528-4BCA-9C48-A757F17E6C22
title: Windows 應用程式認證套件
description: 為了讓您的應用程式能順利在 Microsoft Store 上發行或成為 Windows 認證，請在送出以進行認證之前，先在本機進行驗證和測試。 本主題示範如何安裝和執行 Windows 應用程式認證套件。
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, app certification
ms.localizationpriority: medium
ms.openlocfilehash: 4772edb9c99426396b7fa3a8734e2f45391c3a0f
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/20/2019
ms.locfileid: "74257829"
---
# <a name="windows-app-certification-kit"></a>Windows 應用程式認證套件



To get your app [Windows Certified](https://msdn.microsoft.com/windows/desktop/jj134964.aspx) or prepare it for [publication to the Microsoft Store](https://docs.microsoft.com/windows/uwp/publish/app-submissions), you should validate and test it locally first. This topic shows you how to install and run the [Windows App Certification Kit](https://msdn.microsoft.com/en-US/windows/apps/bg127575) to ensure your app is safe and efficient.

## <a name="prerequisites"></a>必要條件

測試通用 Windows app 的先決條件：

-   You must install and run Windows 10.
-   You must install [Windows App Certification Kit version 10]( https://go.microsoft.com/fwlink/p/?LinkID=309666), which is included in the Windows Software Development Kit (SDK) for Windows 10.
-   您必須[啟用您的裝置以進行開發](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development)。
-   您必須將要測試的 Windows 應用程式部署至電腦。

**A note about in-place upgrades**

安裝更新版本的 [Windows 應用程式認證套件]( https://go.microsoft.com/fwlink/p/?LinkID=309666)，將會取代電腦上安裝的任何舊版套件。

## <a name="validate-your-windows-app-using-the-windows-app-certification-kit-interactively"></a>以互動方式使用 Windows 應用程式認證套件來驗證 Windows 應用程式

1.  從 **\[開始\]** 功能表中，搜尋 **\[應用程式\]** ，找到 **\[Windows 套件\]** ，然後按一下 **\[Windows 應用程式認證套件\]** 。

2.  從 [Windows 應用程式認證套件] 中，選取您要執行的驗證類別。 例如：如果您要驗證 Windows 應用程式，請選取 **\[驗證 Windows 應用程式\]** 。

    您可以直接瀏覽到要測試的 app，或從 UI 中的清單中選擇 app。 首次執行 Windows 應用程式認證套件時，UI 會列出已安裝在您電腦上的所有 Windows 應用程式。 其後每次執行時，UI 將會顯示您最近已驗證過的 Windows 應用程式。 如果沒有列出您要測試的 app，可以按一下 **\[我的 app 未列在裡面\]** ，以取得系統上已安裝的所有 app 的完整清單。

3.  輸入或選取要測試的 app 之後，請按一下 **\[下一步\]** 。

4.  在下一個畫面中，您將會看到與您要測試之應用程式類型對應的測試工作流程。 若清單中的測試呈現灰色，表示該測試不適用於您的環境。 例如，若您在 Windows 7 上測試 Windows 10 應用程式，只有靜態測試會套用到工作流程。 Note that the Microsoft Store may apply all tests from this workflow. 選取要執行的測試，然後按一下 **\[下一步\]** 。

    Windows 應用程式認證套件隨即開始驗證該應用程式。

5.  測試之後，在提示字元輸入您要儲存測試報告的資料夾路徑。

    Windows 應用程式認證套件會建立一個 HTML 以及一份 XML 報告，並將它儲存到這個資料夾。

6.  開啟報告檔案，然後檢閱測試結果。

> [!NOTE]
> If you're using Visual Studio, you can run the Windows App Certification Kit when you create your app package. 若要深入了解，請參閱[封裝 UWP app](/windows/msix/package/packaging-uwp-apps)。

 

## <a name="validate-your-windows-app-using-the-windows-app-certification-kit-from-a-command-line"></a>從命令列使用 Windows 應用程式認證套件來驗證 Windows app

> [!IMPORTANT]
> The Windows App Certification Kit must be run within the context of an active user session.

1.  在命令視窗中，瀏覽到包含 Windows 應用程式認證套件的目錄。

    **Note**   The default path is C:\\Program Files\\Windows Kits\\10\\App Certification Kit\\.

2.  依序輸入下列命令，以測試電腦上已安裝的 app：

    `appcert.exe reset`

    `appcert.exe test -packagefullname [package full name] -reportoutputpath [report file name]`

    或者，若應用程式未安裝，您可以使用下列命令。 Windows 應用程式認證套件將開啟套件，並套用適當的測試工作流程：

    `appcert.exe reset`

    `appcert.exe test -appxpackagepath [package path] -reportoutputpath [report file name]`

3.  測試完成之後，開啟名為 `[report file name]` 的報告檔案，然後檢閱測試結果。

**Note**  The Windows App Certification Kit can be run from a service, but the service must initiate the kit process within an active user session and cannot be run in Session0.

**Note**   For more info about the Windows App Certification Kit command line, enter the command `appcert.exe /?`

## <a name="testing-with-a-low-power-computer"></a>使用低功率電腦進行測試

Windows 應用程式認證套件的效能測試閾值是以低功率電腦的效能為基礎。

執行測試之電腦的特性會影響測試結果。 To determine if your app’s performance meets the [Microsoft Store Policies](https://docs.microsoft.com/legal/windows/agreements/store-policies), we recommend that you test your app on a low-power computer, such as an Intel Atom processor-based computer with a screen resolution of 1366x768 (or higher) and a rotational hard drive (as opposed to a solid-state hard drive).

隨著低功率電腦不斷演進，其效能特性可能會隨時間改變。 Refer to the most current [Microsoft Store Policies](https://docs.microsoft.com/legal/windows/agreements/store-policies) and test your app with the most current version of the Windows App Certification Kit to make sure that your app complies with the latest performance requirements.

## <a name="related-topics"></a>相關主題

* [Windows App Certification Kit tests](windows-app-certification-kit-tests.md)
* [Microsoft Store 原則](https://docs.microsoft.com/legal/windows/agreements/store-policies)
 

 




