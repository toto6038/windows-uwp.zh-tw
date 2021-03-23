---
ms.assetid: 78D833B9-E528-4BCA-9C48-A757F17E6C22
title: Windows 應用程式認證套件
description: 為了讓您的應用程式能順利在 Microsoft Store 上發行或成為 Windows 認證，請在送出以進行認證之前，先在本機進行驗證和測試。 本主題示範如何安裝和執行 Windows 應用程式認證套件。
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 應用程式認證
ms.localizationpriority: medium
ms.openlocfilehash: 6f63ad960993ade83bdfa52283a33b76e2d80db2
ms.sourcegitcommit: 6661f4d564d45ba10e5253864ac01e43b743c560
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/23/2021
ms.locfileid: "104804812"
---
# <a name="windows-app-certification-kit"></a>Windows 應用程式認證套件

若要讓您的應用程式取得 [Windows 認證](/windows/win32/win_cert/windows-certification-portal)或做好[發行到 Microsoft Store](../publish/app-submissions.md) 的準備，您應該先在本機進行驗證和測試。 本主題說明如何安裝和執行 [Windows 應用程式認證套件](https://developer.microsoft.com/windows/develop/app-certification-kit)，以確保您的應用程式安全且有效率。

## <a name="prerequisites"></a>必要條件

測試通用 Windows app 的先決條件：

- 您必須安裝和執行 Windows 10。
- 您必須安裝 [Windows 應用程式認證套件](https://developer.microsoft.com/windows/downloads/windows-10-sdk/)，此套件包含在適用於 Windows 10 的 Windows 軟體開發套件 (SDK) 中。
- 您必須[啟用您的裝置以進行開發](/windows/apps/get-started/enable-your-device-for-development)。
- 您必須將要測試的 Windows App 部署至電腦。

> [!NOTE]
> **就地升級：** 安裝較新的 [Windows 應用程式認證套件](https://developer.microsoft.com/windows/develop/app-certification-kit)會取代任何先前安裝的套件版本。

## <a name="whats-new"></a>最新消息

套件現在支援 Windows [傳統型橋接器應用程式](/windows/msix/desktop/source-code-overview) 的測試。 [Windows 傳統型橋接器應用程式測試](./windows-desktop-bridge-app-tests.md) 可讓您的應用程式在 Microsoft Store 或獲得認證時，獲得最大的機會。

您現在可以將套件整合到自動化測試中，其中沒有任何互動式使用者會話可用。

不再支援應用程式預先啟動驗證測試。

## <a name="known-issues"></a>已知問題

以下是 Windows 應用程式認證套件的已知問題清單：

在測試期間，如果安裝程式終止，但讓使用中的進程或 windows 正在執行，則應用程式認證套件可能會偵測到安裝程式仍有工作。 在此情況下，套件會顯示為執行「處理安裝追蹤檔案」工作，而且無法使用 UI 繼續進行。

**解決方式：** 在安裝程式完成之後，請手動關閉安裝程式所產生的任何作用中進程或 windows。

若為 ARM UWA 或任何不是以裝置系列桌面或 OneCore 為目標的 UWA 應用程式，則會在最後一份報告中出現一則訊息，指出「並非所有測試都是在驗證期間執行。 這可能會影響您的商店提交。」。 當使用者未手動取消選取測試時，此訊息不適用。

**解決方式：** n/a

針對使用 Windows SDK 版本10.0.15063 的傳統型橋接器應用程式，請略過應用程式資訊清單資源測試中的任何失敗，如果這些維度只在一個圖元之間，則會標示您的影像未確認為預期的維度。 測試應該具有 +/-1 圖元的容錯。 例如 在125% 的小磚會 88.75 x 88.75 px 如果進位至89x89px，這會導致88x88px 的大小限制失敗。

**解決方式：** n/a

## <a name="validate-your-windows-app-using-the-windows-app-certification-kit-interactively"></a>以互動方式使用 Windows 應用程式認證套件來驗證 Windows 應用程式

1. 從 [開始]  功能表中，搜尋 [應用程式]  ，找到 [Windows 套件]  ，然後按一下 [Windows 應用程式認證套件]  。

2. 從 [Windows 應用程式認證套件] 中，選取您要執行的驗證類別。 例如：如果您要驗證 Windows 應用程式，請選取 [驗證 Windows 應用程式]  。

    您可以直接瀏覽到要測試的 app，或從 UI 中的清單中選擇 app。 首次執行 Windows 應用程式認證套件時，UI 會列出已安裝在您電腦上的所有 Windows 應用程式。 其後每次執行時，UI 將會顯示您最近已驗證過的 Windows 應用程式。 如果沒有列出您要測試的 app，可以按一下 [我的 app 未列在裡面]  ，以取得系統上已安裝的所有 app 的完整清單。

3. 輸入或選取要測試的 app 之後，請按一下 [下一步]  。

4. 在下一個畫面中，您將會看到與您要測試之應用程式類型對應的測試工作流程。 若清單中的測試呈現灰色，表示該測試不適用於您的環境。 例如，若您在 Windows 7 上測試 Windows 10 應用程式，只有靜態測試會套用到工作流程。 請注意，Microsoft Store 可能會套用來自此工作流程的所有測試。 選取要執行的測試，然後按一下 [下一步]  。

    Windows 應用程式認證套件隨即開始驗證該應用程式。

5. 測試之後，在提示字元輸入您要儲存測試報告的資料夾路徑。

    Windows 應用程式認證套件會建立一個 HTML 以及一份 XML 報告，並將它儲存到這個資料夾。

6. 開啟報告檔案，然後檢閱測試結果。

> [!NOTE]
> 如果您使用的是 Visual Studio，可以在建立應用程式套件時執行 Windows 應用程式認證套件。 若要深入了解，請參閱[封裝 UWP app](/windows/msix/package/packaging-uwp-apps)。

## <a name="validate-your-windows-app-using-the-windows-app-certification-kit-from-a-command-line"></a>從命令列使用 Windows 應用程式認證套件來驗證 Windows app

> [!IMPORTANT]
> Windows 應用程式認證套件必須在使用中的使用者工作階段內容中執行。

1. 在命令視窗中，瀏覽到包含 Windows 應用程式認證套件的目錄。

    **注意** 預設路徑為 C:\\Program Files\\Windows Kits\\10\\App Certification Kit\\。

2. 依序輸入下列命令，以測試電腦上已安裝的 app：

    `appcert.exe reset`

    `appcert.exe test -packagefullname [package full name] -reportoutputpath [report file name]`

    或者，若應用程式未安裝，您可以使用下列命令。 Windows 應用程式認證套件將開啟套件，並套用適當的測試工作流程：

    `appcert.exe reset`

    `appcert.exe test -appxpackagepath [package path] -reportoutputpath [report file name]`

3. 測試完成之後，開啟名為 `[report file name]` 的報告檔案，然後檢閱測試結果。

**注意** Windows 應用程式認證套件可以從服務中執行，但是該服務必須在使用中的使用者工作階段內初始套件處理程序，而且無法在 Session0 中執行。

**注意** 如需 Windows 應用程式認證套件命令列的詳細資訊，請輸入 `appcert.exe /?` 命令

## <a name="testing-with-a-low-power-computer"></a>使用低功率電腦進行測試

Windows 應用程式認證套件的效能測試閾值是以低功率電腦的效能為基礎。

執行測試之電腦的特性會影響測試結果。 若要判斷您應用程式的效能是否符合 [Microsoft Store 原則](/legal/windows/agreements/store-policies)，建議您在低功率電腦上測試應用程式，例如 Intel Atom 處理器電腦搭配使用 1366x768 (或更高) 的螢幕解析度與旋轉式硬碟 (而非固態硬碟)。

隨著低功率電腦不斷演進，其效能特性可能會隨時間改變。 請參閱最新的 [Microsoft Store 原則](/legal/windows/agreements/store-policies)，並使用最新版的 Windows 應用程式認證套件來測試應用程式，以確保您的應用程式符合最新的效能需求。

## <a name="related-topics"></a>相關主題

- [使用 Windows 應用程式認證套件](/windows/win32/win_cert/using-the-windows-app-certification-kit)
- [Windows 桌面應用程式的認證需求](/windows/win32/win_cert/certification-requirements-for-windows-desktop-apps)
- [Windows 應用程式認證套件測試](windows-app-certification-kit-tests.md)
- [Microsoft Store 原則](/legal/windows/agreements/store-policies)