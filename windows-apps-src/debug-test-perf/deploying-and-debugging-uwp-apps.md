---
author: mcleblanc
ms.assetid: 9322B3A3-8F06-4329-AFCB-BE0C260C332C
description: "本文會引導您完成以各種部署和偵錯目標為目標的步驟。"
title: "部署和偵錯通用 Windows 平台 (UWP) app"
translationtype: Human Translation
ms.sourcegitcommit: 14f6684541716034735fbff7896348073fa55f85
ms.openlocfilehash: e2209e90080c7346bb363304b1a28f6446300332

---

# 部署和偵錯通用 Windows 平台 (UWP) app

\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

本文會引導您完成以各種部署和偵錯目標為目標的步驟。

Microsoft Visual Studio 可讓您在各種不同的 Windows 10 裝置上部署和偵錯您的通用 Windows 平台 (UWP) app。 Visual Studio 會處理在目標裝置上建立並登錄應用程式的處理程序。

## 挑選部署目標

若要挑選一個目標，請瀏覽到 [開始偵錯]**** 按鈕旁的偵錯目標下拉式清單，並選取您想要部署應用程式的目標。 選取目標之後，請選擇 [開始偵錯 (F5)]**** 在該目標上部署與偵錯，或按 **Ctrl+F5** 只部署到該目標。

![](images/debug-device-target-list.png)

-   **本機電腦**會將應用程式部署到您目前的開發電腦。 如果您應用程式的「目標平台最小版本」****小於或等於您開發電腦上的作業系統，才能使用此選項。
-   **模擬器**會將應用程式部署到您目前的開發電腦上的模擬環境。 如果您應用程式的「目標平台最小版本」****小於或等於您開發電腦上的作業系統，才能使用此選項。
-   **裝置**會將應用程式部署到已連接的 USB 裝置。 裝置必須是開發人員解除鎖定，並且解除鎖定畫面。
-   **模擬器**目標會開機，並以名稱中指定的設定將應用程式部署到模擬器。 模擬器僅在執行 Windows 8.1 或以上版本的 Hyper-V 電腦上提供使用。
-   **遠端電腦**可讓您指定遠端目標來部署應用程式。 如需部署到遠端電腦的詳細資訊，請參閱[指定遠端裝置](#specifying-a-remote-device)。

## 偵錯已部署的應用程式
Visual Studio 也可以依序選取 [偵錯]****、[附加至處理序]****，來附加至任何執行中的 UWP app 處理序。 附加至執行中的處理序不需要原始的 Visual Studio 專案，但偵錯沒有原始程式碼的處理序時，載入處理序的[符號](#symbols)將會有顯著的幫助。  
  
此外，任何已安裝的應用程式套件皆可被附加或偵錯，方法是選取 [偵錯]****，[其他]****，然後選取 [偵錯已安裝的應用程式套件]****。   
 
![[偵錯已安裝的應用程式套件] 對話方塊](images/gs-debug-uwp-apps-002.png)  

選取 [不啟動，但在我的程式碼啟動時進行偵錯]**** 會在您於自訂時間啟動 UWP app 時，讓 Visual Studio 偵錯工具附加到您的 UWP app。 這是偵錯來自[不同啟動方法](../xbox-apps/automate-launching-uwp-apps.md)之控制路徑的有效方式，例如使用自訂參數的通訊協定啟用。  

UWP app 可在 Windows 8.1 或更新版本上開發及編譯，但需要 Windows 10 才能執行。 如果您在 Windows 8.1 電腦上開發 UWP app，而假設主機和目標電腦在相同的 LAN 上，您就可以遠端偵錯在另一部 Windows 10 裝置上執行的 UWP app。 若要這樣做，請在兩部電腦上下載並安裝 [Visual Studio 遠端工具](http://aka.ms/remotedebugger)。 安裝的版本必須符合您現有已安裝的 Visual Studio 版本，且選取的架構 (x86、x64) 必須與您的目標應用程式相符。   
  

## 指定遠端裝置

### C# 和 Microsoft Visual Basic

若要針對 C# 或 Microsoft Visual Basic app 指定遠端電腦，請選取偵錯目標下拉式清單中的 [遠端電腦]****。 [遠端連線]**** 對話方塊會隨即出現，這可讓您指定 IP 位址，或選取探索到的裝置。 根據預設，會選取 [通用]**** 驗證模式。 若要判斷要使用何種驗證模式，請參閱[驗證模式](#authentication-modes)。

![](images/debug-remote-connections.png)

若要返回這個對話方塊，您可以開啟專案內容並瀏覽到 [偵錯]**** 索引標籤。 從該處選取 [尋找]**** ([遠端電腦:]**** 旁邊)

![](images/debug-remote-machine-config.png)

若要將應用程式部署到遠端電腦，您也需要在目標電腦上下載並安裝 Visual Studio 遠端工具。 如需完整指示，請參閱[遠端電腦指示](#remote-pc-instructions)。

### C++ 和 JavaScript

若要針對 C++ 或 JavaScript UWP app 指定遠端電腦目標，請在 [方案總管]**** 中的專案上按一下滑鼠右鍵，並按一下 [屬性]**** 移至專案屬性。 瀏覽到 [偵錯]**** 設定，並將 [要啟動的偵錯工具]**** 變更為 [遠端電腦]****。 然後填入 [電腦名稱]**** (或按一下 [尋找]**** 來尋找) 並設定 [驗證類型]**** 屬性。

![](images/debug-property-pages.png)
指定電腦之後，您可以選取偵錯目標下拉式清單中的 [遠端電腦]****，返回到指定的電腦。 一次只能選取一個遠端電腦。

### 遠端電腦指示

若要部署到遠端電腦，目標電腦必須先安裝 Visual Studio 遠端工具。 遠端電腦也必須執行大於或等於 app 的「目標平台最小版本」**** 屬性的 Windows 版本。 一旦安裝遠端工具之後，您必須啟動目標電腦上的遠端偵錯工具。 若要這樣做，請在 [開始]**** 功能表中搜尋 [遠端偵錯工具]**** 加以啟動，如果系統提示您，請允許偵錯工具設定您的防火牆設定。 根據預設，偵錯工具會使用 Windows 驗證啟動。 如果兩部電腦上的登入使用者不相同，將會需要使用者認證。 若要將它變更成 [非驗證]****，請移至 [遠端偵錯工具]**** 中的 [工具]**** -&gt;[選項]****，然後將它設定為 [非驗證]****。 設定遠端偵錯工具之後，您可以從您的開發電腦進行部署。

如需詳細資訊，請參閱 [Visual Studio 遠端工具]( http://go.microsoft.com/fwlink/?LinkId=717039)下載頁面。

## 驗證模式

有三種遠端電腦部署的驗證模式：

- **通用 (未加密的通訊協定)**：Use this authentication mode 每當您部署至非 Windows 電腦 (桌上型電腦或膝上型電腦) 的遠端裝置時，請使用此驗證模式。 目前僅限 IoT 裝置。 「通用 (未加密通訊協定)」只能使用在受信任的網路。 偵錯連接容易受到惡意使用者的攻擊，這些使用者可以攔截與變更在開發電腦與遠端電腦之間傳送的資料。
- **Windows**：此驗證模式只適用於遠端電腦部署 (桌上型電腦或膝上型電腦)。 當您有權存取目標電腦之登入使用者的認證時，請使用這個驗證模式。 這是進行遠端部署最安全的通道。
- **無**：此驗證模式只適用於遠端電腦部署 (桌上型電腦或膝上型電腦)。 當您在使用測試帳戶登入的環境中有安裝測試電腦，而您無法輸入認證時，請使用這個驗證模式。 請確定遠端偵錯工具設定為接受非驗證。

## 偵錯選項

在 Windows 10 上，藉由使用稱為[預先啟動](https://msdn.microsoft.com/library/windows/apps/Mt593297)的技術來主動啟動並暫停應用程式，UWP 應用程式的啟動效能比以前更好。 許多應用程式不需要特別在此模式中執行任何動作，但某些應用程式可能需要調整它們的行為。 若要協助偵錯這些程式碼路徑中的任何問題，您可以在預先啟動模式中從 Visual Studio 開始偵錯 app。 偵錯同時支援從 Visual Studio 專案 ([偵錯]**** -&gt;[其他偵錯目標]**** -&gt;[偵錯通用 Windows 應用程式預先啟動]****)，和已安裝在電腦上的 app ([偵錯]**** -&gt;[其他偵錯目標]**** -&gt;[偵錯安裝的應用程式套件]****，並選取 [使用預先啟動來啟動應用程式]**** 方塊)。 如需詳細資訊，請閱讀關於如何[偵錯 UWP 預先啟動]( http://go.microsoft.com/fwlink/?LinkId=717245)。

您可以在啟始專案的 [偵錯]**** 屬性頁面，設定下列部署選項。

**允許網路回送**

基於安全性考量，以標準方式安裝的 UWP 應用程式不允許對其安裝所在之裝置進行網路呼叫。 根據預設，Visual Studio 部署會從這個已部署的應用程式規則建立免套用原則。 這個免套用原則可讓您在單一電腦上測試通訊程序。 將應用程式送出到 Windows 市集之前，您應該在沒有免套用原則的情況下測試您的應用程式。

移除 app 的網路回送免套用原則：

-   在 C# 和 Visual Basic 的 [偵錯]**** 屬性頁面上，清除 [允許網路回送]**** 核取方塊。
-   在 JavaScript 和 C++ 的 [偵錯]**** 屬性頁面上，將 [允許網路回送]**** 值設定為 [否]****。

**不要啟動，但在啟動時 (C# 和 Visual Basic) / 啟動應用程式 (JavaScript 和 C++) 時偵錯我的程式碼**

設定部署於啟動應用程式時自動啟動偵錯工作階段：

-   在 C# 和 Visual Basic 的 [偵錯]**** 屬性頁面上，核取 [不啟動，但在我的程式碼啟動時進行偵錯]**** 核取方塊。
-   在 JavaScript 和 C++ 的 [偵錯]**** 屬性頁面上，將 [啟動應用程式]**** 值設定為 [是]****。

## 符號

偵錯程式碼時，符號檔案包含各種非常有用的資料，例如變數、函式名稱和進入點位址，可讓您更清楚地了解意外狀況和呼叫堆疊執行順序。 針對 Windows 大部分變體的符號，可透過[Microsoft 符號伺服器](http://msdl.microsoft.com/download/symbols)取得，或在[下載 Windows 符號套件](http://aka.ms/winsymbols)下載以進行更快速的離線查閱。

若要設定 Visual Studio 的符號選項，請選取 [工具] &gt; [選項]****，然後在對話方塊視窗中瀏覽至 [偵錯] &gt; [符號]****。

**圖 4. [選項] 對話方塊。** 
![[選項] 對話方塊](images/gs-debug-uwp-apps-004.png)

若要使用 [WinDbg](#windbg) 在偵錯工作階段中載入符號，請將 **sympath** 變數設定到符號套件位置。 例如，執行下列命令將會從 Microsoft 符號伺服器載入符號，然後將它們快取到 C:\Symbols 目錄中：

```
.sympath SRV*C:\Symbols*http://msdl.microsoft.com/download/symbols
.reload
```

您可以利用 ‘;’ 分隔符號，或使用 `.sympath+` 命令新增更多路徑。 如需使用 WinDbg 的進階符號作業詳細資訊，請參閱[公用與專用符號](https://msdn.microsoft.com/library/windows/hardware/ff553493)。

## WinDbg

WinDbg 是功能強大的偵錯工具，隨附於 Windows 偵錯工具套件，此套件包含在 [Windows SDK](http://go.microsoft.com/fwlink/p?LinkID=271979) 中。 Windows SDK 安裝可讓您將 Windows 偵錯工具安裝為獨立產品。 雖然偵錯原生程式碼時非常有用，我們並不建議將 WinDbg 用於以 Managed 程式碼或 HTML 5 撰寫的應用程式。 

若要對 UWP app 使用 WinDbg，您必須先使用 PLMDebug 來停用您應用程式套件的 PLM，如先前小節中所述。 

```
plmdebug /enableDebug [PackageFullName] "\"C:\Program Files\Debugging Tools for Windows (x64)\WinDbg.exe\" -server npipe:pipe=test"
```

相較於 Visual Studio，大部分 WinDbg 的核心功能依賴將命令提供給命令視窗。 提供的命令可讓您檢視執行狀態、調查使用者模式損毀傾印，並在不同的模式中偵錯。 

WinDbg 當中最常用的其中一個命令是 `!analyze -v`，這是用來擷取目前例外狀況的相關詳細資訊量，包括：

- FAULTING_IP：錯誤時間的指令指標
- EXCEPTION_RECORD：目前例外狀況的位址、程式碼和旗標
- STACK_TEXT：例外狀況之前的堆疊追蹤

如需所有 WinDbg 命令的完整清單，請參閱[偵錯工具命令](https://msdn.microsoft.com/library/ff540507)。

## 相關主題
- [處理程序生命週期管理 (PLM) 的測試與偵錯工具](testing-debugging-plm.md)
- [偵錯、測試及效能](index.md)



<!--HONumber=Jun16_HO4-->


