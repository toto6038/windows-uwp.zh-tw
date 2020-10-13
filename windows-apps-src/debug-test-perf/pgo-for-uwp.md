---
title: 在通用 Windows 平台 (UWP) app 上執行特性指引最佳化 (PGO)
description: 將「特性指引最佳化」(PGO) 套用至「通用 Windows 平台」(UWP) 應用程式的逐步指引。
ms.date: 02/08/2017
ms.localizationpriority: medium
ms.topic: article
ms.openlocfilehash: a606d87b309b130cd9bb0cdc90a2a8b3a3bcc717
ms.sourcegitcommit: a30808f38583f7c88fb5f54cd7b7e0b604db9ba6
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/06/2020
ms.locfileid: "91762862"
---
# <a name="running-profile-guided-optimization-on-universal-windows-platform-apps"></a>在通用 Windows 平台 app 上執行特性指引最佳化 
 
本主題提供將「特性指引最佳化」(PGO) 套用至「通用 Windows 平台」(UWP) app 的逐步指引。 並非所有傳統 win32 應用程式可用的步驟也都適用於 UWP app，因此我們的目標是要說明納入 PGO 讓 UWP 開發人員能夠更容易進行及使用最佳化的必要程序。

以下是一個基本的逐步解說，說明如何透過使用 Visual Studio 2015 Update 3，將 PGO 套用至預設 DirectX 11 應用程式 (UWP) 範本。
 
這整份指南中的螢幕擷取畫面是以下列新專案為基礎：![螢幕擷取畫面：顯示 [新增專案] 對話方塊，其中顯示已選取 [已安裝] > [範本] > [Visual C++]，並醒目提示 [Direct 11 應用程式] 選項。](images/pgo-001.png)

將 PGO 套用至 DirectX 11 應用程式範本：

1. 將您的方案組態設定為 [發行]  ，或選擇您要在其中產生用於發行之最佳化程式碼的方案組態。 雖然理論上您可以在偵錯組建上執行 PGO，但是使用 PGO 將一個未最佳化的組建最佳化並無效果。 
 
 ![App1 視窗](images/pgo-002.png)
 
2. 在您專案的屬性中 ([屬性]   > [C/C++]   > [最佳化]  )，確認您是搭配在 [整個程式最佳化]  選擇 /GL 旗標來進行建置 (您的組態可能已經這樣設定)。

 ![整個程式最佳化](images/pgo-003.png)

3. 進入連結器屬性 ([屬性]   > [連結器]   > [最佳化]  )，將 [連結時產生程式碼]  設定為 [特性指引最佳化 - 檢測 (LTCG:PGInstrument)]  。
 
 ![連結時產生程式碼](images/pgo-004.png)

4. 選取 [建置方案]  ，然後選取 [部署方案]  。 

 ![螢幕擷取畫面：顯示 [建置] 下拉式清單，並以紅色箭號指向 [建置方案] 和 [部署方案] 選項。](images/pgo-005.png)
 
 您可以透過查看組建輸出位置並確認是否已產生 .pgd 檔案，來仔細檢查所有項目是否都已正常運作。 在這個範例案例中，這意謂著下列檔案已隨著組建輸出一起產生︰
 
 `C:\Users\<USER>\Documents\Visual Studio 2015\Projects\App1\Release\App1\App1.pgd`

 .pgd 檔案預設會與您的可執行檔有相同的名稱。 您也可以透過變更 [特性指引資料庫]  連結器選項，來變更所產生 .pgd 檔案的名稱。 
 
5. 瀏覽至您的 Visual Studio VC 二進位檔目錄 (這預設會看起來像 `C:\Program Files (x86)\Microsoft Visual Studio 14.0\VC\bin`)。 針對 x86 可執行檔，請複製 `pgort140.dll`；針對 x64 可執行檔，請從 `amd64\pgort140.dll` 複製 x64 版本。 將適當的 `pgort140.dll` 版本貼到您部署之套件的根目錄中。 就這個範例而言，路徑是︰

 `C:\Users\<USER>\Documents\Visual Studio 2015\Projects\App1\Release\App1\AppX\`

 這個步驟是必要的，因為 UWP app 只能載入在其套件內的程式庫。

 ![[檔案總管] 視窗的螢幕擷取畫面，其中顯示 AppX 資料夾的內容。](images/pgo-006.png)
 
6. 從 [開始] 功能表執行 App，或從 Visual Studio [偵錯]  功能表選取 [啟動但不偵錯]  選項來執行 App。 

 ![螢幕擷取畫面：顯示 [偵錯] 下拉式清單，並醒目提示 [啟動但不偵錯] 選項。](images/pgo-007.png)
 
7. 系統會檢測現在執行的組建並產生 PGO 資料。 此時，您應該讓應用程式在一些您想要最佳化的最常見案例中從頭到尾執行一遍。 在程式於您想要的案例中從頭到尾執行一遍之後，請在您找到適當 `pgort140.dll` 版本的相同資料夾中尋找 pgosweep.exe 工具 。 或者，Visual Studio (x86/x64) Native Tools 命令提示字元的路徑中將會已經包含適當的版本。 若要收集 PGO 資料，請在應用程式仍在執行中時執行下列命令，以產生將包含分析資料的 .pgc 檔案︰
 
  `pgosweep.exe <executable name> <output file>` 
 
  您也可以查看 pgosweep.exe 說明 (`pgosweep.exe /help`)，來檢視其他可控制 PGO 資料收集方式的選擇性引數。
 
  將 .pgc 檔案輸出至 .pgd 所在的組建位置，以及將檔案命名為 `<PGDName>!<RunIdentifier>.pgc`，都是不錯的想法。 就這個範例而言，這意謂著：
 
  ```
  pgosweep.exe App1.exe "C:\Users\<USER>\Documents\Visual Studio 2015\Projects\App1\Release\App1\App1!1.pgc"
  ```
 
  進一步的收集也可以是 `App1!CoreScenario.pgc`、`App1!UseCase5.pgc` 等。如果 .pgc 檔案是以這種方式命名，並且是在組建輸出位置中 .pgd 的旁邊，則在步驟 9 中進行連結時，將會自動將它們合併。
 
8. 選擇性︰根據預設，所有依據步驟 7 中指定的方式命名並放在 .pgd 旁邊的 .pgc 檔案，在連結時將會合併，並具有同等的加權，但您也可以在對特定執行的加權方式上擁有更大的控制權。 為了這樣做，您將使用 **pgomgr.exe** 工具，此工具也位於您一開始找到 `pgort140.dll` 複本的相同資料夾中。 例如，若要將 `CoreScenario` 執行以其他執行的 3 倍優先順序合併，我可以使用下列命令：
 
 ```
 pgomgr.exe -merge:3 "C:\Users\<USER>\Documents\Visual Studio 2015\Projects\App1\Release\App1\App1!CoreScenario.pgc" "C:\Users\<USER>\Documents\Visual Studio 2015\Projects\App1\Release\App1\App1.pgd"
 ```
 
9. 產生一或多個 .pgc 檔案並將它們放在 .pgd 旁邊或手動合併它們 (步驟 8) 之後，我們現在即可使用連結器來建立最終的最佳化組建。 回到您的連結器屬性 ([屬性]   > [連結器]   > [最佳化]  )，將 [連結時產生程式碼]  設定為 [特性指引最佳化 - 檢測 (LTCG:PGOptimize)]  ，然後確認 [特性指引資料庫]  指向您想要使用的 .pgd (如果您尚未進行變更，所有項目應該都井然有序)。

 ![螢幕擷取畫面：App 1 [屬性頁] 對話方塊，其中顯示已選取 [設定屬性] > [連結器] > [最佳化]，並醒目提示 [連結時產生程式碼] 選項和 [特性指引最佳化 - 檢測 (LTCG:PGOptimize)] 選項。](images/pgo-009.png)
 
10. 現在，當建置專案時，連結器將會呼叫 pgomgr.exe 將任何 `<PGDName>!*.pgc` 檔案以預設權數 1 合併至 .pgd，而產生的應用程式將會根據分析資料進行最佳化。

## <a name="see-also"></a>另請參閱
- [效能](performance-and-xaml-ui.md)

 

