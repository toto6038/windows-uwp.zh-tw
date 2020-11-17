---
ms.assetid: 9322B3A3-8F06-4329-AFCB-BE0C260C332C
description: 本文會引導您完成以各種部署和偵錯目標為目標的步驟。
title: 部署和偵錯通用 Windows 平台 (UWP) app
ms.date: 04/08/2019
ms.topic: article
keywords: Windows 10, uwp, 偵錯, 測試, 效能
ms.localizationpriority: medium
ms.openlocfilehash: 1d537ca64e94d68a8cc9bbe9c59d341d04821cd9
ms.sourcegitcommit: aaa72ddeb01b074266f4cd51740eec8d1905d62d
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/06/2020
ms.locfileid: "94339745"
---
# <a name="deploying-and-debugging-uwp-apps"></a>部署和偵錯 UWP 應用程式

本文會引導您完成以各種部署和偵錯目標為目標的步驟。

Microsoft Visual Studio 可讓您在各種不同的 Windows 10 裝置上進行「通用 Windows 平台」(UWP) app 部署和偵錯。 Visual Studio 會處理在目標裝置上建立及註冊 App 的處理程序。

## <a name="picking-a-deployment-target"></a>挑選部署目標

若要挑選目標，請移至 [開始偵錯]  按鈕旁的偵錯目標下拉式清單，並選擇您想要部署應用程式的目標。 選取目標之後，請選取 [開始偵錯 (F5)]  以在該目標上部署並偵錯，或選取 **Ctrl+F5** 以只部署到該目標。

![偵錯裝置目標清單](images/debug-device-target-list.png)

- [模擬器]  會將應用程式部署到您目前開發電腦上的模擬環境。 只有當您應用程式的 **目標平台最低版本** 小於或等於您開發電腦上的作業系統時，才可以使用此選項。
- [本機電腦]  會將應用程式部署到您目前的開發電腦。 只有當您應用程式的 **目標平台最低版本** 小於或等於您開發電腦上的作業系統時，才可以使用此選項。
- [遠端電腦]  會讓您指定用來部署應用程式的遠端目標。 如需有關部署到遠端電腦的詳細資訊，請參閱[指定遠端裝置](#specifying-a-remote-device)。
- [裝置]  會將應用程式部署到已透過 USB 連接的裝置。 裝置必須已由開發人員解除鎖定，並且畫面已解除鎖定。
- [模擬器]  目標會開機，並以名稱中指定的設定將應用程式部署到模擬器。 模擬器僅在執行 Windows 8.1 或更新版本且支援 Hyper-V 的電腦上才有提供。

## <a name="debugging-deployed-apps"></a>偵錯已部署的 App

也可以依序選取 [偵錯]  、[附加至處理序]  ，將 Visual Studio 附加至任何執行中的 UWP 應用程式處理序。 附加至執行中的處理序不需要原始的 Visual Studio 專案，但偵錯沒有原始程式碼的處理序時，載入處理序的[符號](#symbols)將會有顯著的幫助。  

此外，任何已安裝的應用程式套件皆可被附加或偵錯，方法是選取[偵錯]  、[其他]  ，然後選取 [偵錯已安裝的應用程式套件]  。

![[偵錯已安裝的應用程式套件] 對話方塊](images/gs-debug-uwp-apps-002.png)

選取 [不啟動，但在我的程式碼啟動時進行偵錯]  會在您於自訂時間啟動 UWP 應用程式時，讓 Visual Studio 偵錯工具附加到您的 UWP 應用程式。 這是偵錯來自[不同啟動方法](../xbox-apps/automate-launching-uwp-apps.md)之控制路徑的有效方式，例如使用自訂參數的通訊協定啟用。  

UWP app 可在 Windows 8.1 或更新版本上開發及編譯，但需要 Windows 10 才能執行。 如果您在 Windows 8.1 電腦上開發 UWP app，而假設主機和目標電腦在相同的 LAN 上，您就可以遠端偵錯在另一部 Windows 10 裝置上執行的 UWP app。 若要這樣做，請在兩部電腦上都下載並安裝 [Visual Studio 遠端工具](https://visualstudio.microsoft.com/downloads/)。 安裝的版本必須符合您已安裝的現有 Visual Studio 版本，且所選取的架構 (x86、x64) 也必須與您目標 App 的架構相符。

## <a name="package-layout"></a>封裝配置

自 Visual Studio 2015 Update 3 開始，我們新增了可供開發人員指定其 UWP 應用程式配置路徑的選項。 這會決定當您建置 App 時，要將封裝配置複製到磁碟上的哪個位置。 這個屬性預設會設定為專案根目錄的相對位置。 如果您沒有修改這個屬性，行為將會維持與它在舊版 Visual Studio 時相同。

您可以在專案的 **Debug** 屬性中修改這個屬性。

如果您想要在為您的 App 建立封裝時在封裝中包含所有配置檔案，您就必須新增專案屬性 `<IncludeLayoutFilesInPackage>true</IncludeLayoutFilesInPackage>`。

新增這個屬性：

1. 在專案上按一下滑鼠右鍵，然後選取 [卸載專案]  。
2. 在專案上按一下滑鼠右鍵，然後選取 [編輯 [projectname].xxproj]  (.xxproj 隨專案語言而異)。
3. 新增該屬性，然後重新載入專案。

## <a name="specifying-a-remote-device"></a>指定遠端裝置

### <a name="c-and-microsoft-visual-basic"></a>C# 和 Microsoft Visual Basic

若要為 C# 或 Microsoft Visual Basic 應用程式指定遠端電腦，請從偵錯目標下拉式清單中選取 [遠端電腦]  。 [遠端連線]  對話方塊會隨即出現，這可讓您指定 IP 位址或選取已探索到的裝置。 預設會選取 [通用]  驗證模式。 若要判斷要使用哪一種驗證模式，請參閱[驗證模式](#authentication-modes)。

![[遠端連線] 對話方塊](images/debug-remote-connections.png)

若要返回這個對話方塊，您可以開啟專案屬性並移至 [偵錯]  索引標籤。從該處選取 [遠端電腦]  旁的 [尋找]  ：

![[偵錯] 索引標籤](images/debug-remote-machine-config.png)

若要將應用程式部署到 Creators Update 發行之前的遠端電腦，您也需要在目標電腦上下載並安裝「Visual Studio 遠端工具」。 如需完整指示，請參閱[遠端電腦指示](#remote-pc-instructions)。  不過，從 Creators Update 開始，電腦也會支援遠端部署。  

### <a name="c-and-javascript"></a>C++ 和 JavaScript

為 C++ 或 JavaScript UWP app 指定遠端電腦目標：

1. 開啟 [方案總管]  ，在專案上按一下滑鼠右鍵，然後按一下 [屬性]  。
2. 移至 [偵錯]  設定，在 [要啟動的偵錯工具]  底下選取 [遠端電腦]  。
3. 輸入 [電腦名稱]  (或按一下 [尋找]  來尋找電腦名稱)，然後設定 [驗證類型]  屬性。

![偵錯屬性頁面](images/debug-property-pages.png)

指定電腦之後，您可以從偵錯目標下拉式清單中選取 [遠端電腦]  以返回到該指定電腦。 一次只能選取一部遠端電腦。

### <a name="remote-pc-instructions"></a>遠端電腦指示

> [!NOTE]
> 只有舊版 Windows 10 才需要遵循這些指示。  從 Creators Update 開始，可以將電腦視為 Xbox。  也就是，藉由啟用電腦 [開發人員模式] 功能表中的 [裝置探索]，並透過使用通用驗證，可與電腦進行 PIN 配對和連線。

若要部署到 Creators Update 發行之前的遠端電腦，目標電腦必須先安裝 Visual Studio 遠端工具。 遠端電腦必須也執行大於或等於應用程式 **Target Platform Min.Version** 屬性的 Windows 版本。 安裝遠端工具之後，您必須啟動目標電腦上的遠端偵錯工具。

若要這樣做，請在 [開始]  功能表中搜尋 [遠端偵錯工具]  、開啟它，如果出現提示，請允許偵錯工具設定您的防火牆設定。 偵錯工具預設會使用 Windows 驗證啟動。 如果兩部電腦上的登入使用者不相同，這將會要求提供使用者認證。

若要將它變更為 [無驗證]  ，請在 [遠端偵錯工具]  中移至 [工具]   -&gt; [選項]  ，然後將它設定為 [無驗證]  。 設定遠端偵錯工具之後，您也必須確定已將主機裝置設定為 [開發人員模式](/windows/apps/get-started/enable-your-device-for-development)。 之後，您便可以從您的開發電腦進行部署。

如需詳細資訊，請參閱 [Visual Studio 下載中心](https://visualstudio.microsoft.com/downloads/)頁面。

## <a name="passing-command-line-debug-arguments"></a>傳遞命令列偵錯引數

在 Visual Studio 2019 中，您可以在開始偵錯 UWP應用程式時傳遞命令列偵錯引數。 您可以從 *args* 參數存取命令列偵錯引數，而此參數位於 [**Application**](/uwp/api/windows.ui.xaml.application) 類別的 **OnLaunched** 方法中。 若要指定命令列偵錯引數，請開啟專案屬性，並導覽至 [偵錯]  索引標籤。

> [!NOTE]
> 這是在 Visual Studio 2017 (版本 15.1) 中提供，適用於 C#、VB、C++ 。 JavaScript 會在較新版本中提供。 命令列偵錯引數適用於所有部署類型，但模擬器除外。

針對 C# 和 VB UWP 專案，您會在 [開始選項]  下方看到 [命令列引數:]  欄位。

![命令列引數](images/command-line-arguments.png)

針對 C++ 和 JS UWP 專案，您會看到 [命令列引數]  是 [偵錯屬性]  中的一個欄位。

![已選取 [設定屬性] > [偵測] 選項的 [應用程式 4] 屬性頁螢幕擷取畫面，其中顯示資料表中的命令列引數屬性 isted。](images/command-line-arguments-cpp.png)

指定命令列引數之後，即可存取應用程式 **OnLaunched** 方法中的引數值。 [**LaunchActivatedEventArgs**](/uwp/api/windows.applicationmodel.activation.launchactivatedeventargs) 物件 *args* 的 **Arguments** 屬性值會設為 [命令列引數]  欄位中的文字。

![C++ 和 JS 的命令列引數螢幕擷取畫面。](images/command-line-arguments-debugging.png)

## <a name="authentication-modes"></a>驗證模式

有三種遠端電腦部署的驗證模式：

- **通用 (未加密的通訊協定)** ：每當您部署至遠端裝置時，請使用此驗證模式。 目前這適用於 IoT 裝置、Xbox 裝置、HoloLens 裝置，以及 Creators Update 電腦或較新的電腦。 [通用 (未加密通訊協定)] 應該只有在受信任的網路上才使用。 偵錯連接容易受到惡意使用者的攻擊，這些使用者可以攔截與變更在開發電腦與遠端電腦之間傳送的資料。
- **Windows**：此驗證模式只適用於執行 Visual Studio 遠端工具的遠端電腦 (桌上型電腦或膝上型電腦)。 當您能夠存取目標電腦登入使用者的認證時，請使用這個驗證模式。 這是最安全的遠端部署通道。
- **無**：此驗證模式只適用於執行 Visual Studio 遠端工具的遠端電腦 (桌上型電腦或膝上型電腦)。 當您在已有測試帳戶登入的環境中安裝了測試電腦，但無法輸入認證時，請使用這個驗證模式。 請確定遠端偵錯工具設定已設定為接受非驗證。

## <a name="advanced-remote-deployment-options"></a>進階遠端部署選項

自 Visual Studio 2015 Update 3 發行版和 Windows 10 年度更新版開始，為特定 Windows 10 裝置提供了新的進階遠端部署選項。 您可以在專案屬性的 [偵錯]  功能表上找到進階遠端部署選項。

新屬性包括：

- 部署類型
- 封裝註冊路徑
- 保留裝置上的所有檔案，包括不再屬於您的配置者

### <a name="requirements"></a>需求

若要利用進階遠端部署選項，您必須滿足下列需求：

- 已安裝 Visual Studio 2015 Update 3 或更新的 Visual Studio 發行版以及 Windows 10 Tools 1.4.1 或更新版本 (包含 Windows 10 年度更新 SDK) ，我們建議您使用最新版的 Visual Studio 更新，以確保您取得所有最新的開發與安全性功能。
- 以 Windows 10 年度更新版 Xbox 遠端裝置或 Windows 10 Creators Update 電腦為目標
- 使用「通用驗證」模式

### <a name="properties-pages"></a>屬性頁面

C# 或 Visual Basic UWP app 的屬性頁面會看起來如下。

![CS 或 VB 屬性](images/advanced-remote-deploy-cs.png)

C++ UWP app 的屬性頁面會看起來如下。

![Cpp 屬性](images/advanced-remote-deploy-cpp.png)

### <a name="copy-files-to-device"></a>將檔案複製到裝置

[將檔案複製到裝置]  會將檔案實際透過網路傳輸到遠端裝置。 它會將已建置的封裝配置複製並註冊到 [配置資料夾路徑]  。 Visual Studio 會讓複製到裝置的檔案與您 Visual Studio 專案中的檔案保持同步；不過，還有一個 [保留裝置上的所有檔案，包括不再屬於您的配置者]  選項。 選取這個選項意謂著任何先前複製到遠端裝置上但已不再屬於您專案的檔案，將會保留在遠端裝置上。

您在 [將檔案複製到裝置]  時指定的 [封裝註冊路徑]  是作為檔案複製目的地之遠端裝置上的實體位置。 此路徑可以指定為任何相對路徑。 部署檔案的位置會是開發檔案根目錄的相對位置，此根目錄會依目標裝置而有不同。 如果多位開發人員共用相同裝置並處理含有一些組建差異的封裝，則指定此路徑會相當有用。

> [!NOTE]
> 執行 Windows 10 年度更新版的 Xbox 以及執行 Windows 10 Creators Update 的電腦目前支援 [將檔案複製到裝置]  。

在遠端裝置上，會將配置複製到下列預設位置︰`\\MY-DEVKIT\DevelopmentFiles\PACKAGE-REGISTRATION-PATH`

### <a name="register-layout-from-network"></a>從網路登錄配置

當您選擇從網路登錄配置時，您可以將您的封裝配置建置到網路共用，然後直接從網路在遠端裝置上登錄該配置。 這會要求您指定一個可從遠端裝置存取的配置資料夾路徑 (網路共用)。 [配置資料夾路徑]  屬性是設定成與執行 Visual Studio 的電腦相對的路徑，雖然 [封裝註冊路徑]  屬性是相同的路徑，但指定成遠端裝置的相對路徑。

若要從網路順利登錄配置，您必須先將 [配置資料夾路徑]  設定為共用網路資料夾。 若要這樣做，請在「檔案總管」中的資料夾上按一下滑鼠右鍵，選取 [共用對象] > [特定人員]  ，然後選擇您想要共用資料夾的使用者。 當您嘗試從網路登錄配置時，系統會提示您輸入認證，以確保您是以能夠存取共用的使用者身分登錄。

如需相關說明，請參閱下列範例：

- 範例 1 (本機配置資料夾，以網路共用的形式可供存取)︰
  - **配置資料夾路徑** = `D:\Layouts\App1`
  - **封裝註冊路徑** = `\\NETWORK-SHARE\Layouts\App1`

- 範例 2 (網路配置資料夾)：
  - **配置資料夾路徑** = `\\NETWORK-SHARE\Layouts\App1`
  - **封裝註冊路徑** = `\\NETWORK-SHARE\Layouts\App1`

當您第一次從網路登錄配置時，系統會將您的認證快取在目標裝置上，讓您不需要重複地登入。 若要移除已快取的認證，您可以使用來自 Windows 10 SDK 的 [WinAppDeployCmd.exe 工具](../packaging/install-universal-windows-apps-with-the-winappdeploycmd-tool.md) 搭配 **deletecreds** 命令。

當您從網路登錄配置時，無法選取 [保留裝置上的所有檔案]  ，因為沒有任何檔案被實際複製到遠端裝置上。

> [!NOTE]
> 執行 Windows 10 年度更新版的 Xbox 以及執行 Windows 10 Creators Update 的電腦目前支援 **從網路登錄配置**。

在遠端裝置上，會將配置登錄到下列依裝置系列而異的預設位置︰`Xbox: \\MY-DEVKIT\DevelopmentFiles\XrfsFiles` - 這是 **封裝註冊路徑** 的符號連結；電腦不會使用符號連結，而是直接登錄 **封裝註冊路徑**。

## <a name="debugging-options"></a>偵錯選項

在 Windows 10 上，藉由使用稱為[預先啟動](../launch-resume/handle-app-prelaunch.md)的技術，先主動啟動應用程式再予以暫停，讓 UWP 應用程式的啟動效能獲得提升。 許多 App 將不需要特別執行任何動作即可在此模式下工作，但某些 App 可能需要調整它們的行為。 若要協助針對這些程式碼路徑中的任何問題進行偵錯，您可以在預先啟動模式下，從 Visual Studio 開始針對 App 進行偵錯。

Visual Studio 專案以及已安裝在電腦上的應用程式皆支援偵錯，前者是從 [偵錯]   -&gt; [其他偵錯目標]   -&gt; [偵錯通用 Windows 應用程式預先啟動]  進行設定，後者則是要選取 [使用「預先啟動」啟用應用程式]  核取方塊，並從 [偵錯]   -&gt; [其他偵錯目標]   -&gt; [偵錯已安裝的應用程式套件]  進行設定。 如需詳細資訊，請參閱[偵錯 UWP 預先啟動](https://blogs.msdn.com/b/visualstudioalm/archive/2015/11/30/debug-uwp-prelaunch-with-vs2015.aspx)。

您可以在啟始專案的 [偵錯]  屬性頁面上設定下列部署選項：

- **允許區域網路回送**

  基於安全性考量，系統不允許以標準方式安裝的 UWP app 對其安裝所在的裝置進行網路呼叫。 根據預設，Visual Studio 部署會從這個已部署的應用程式規則建立免套用原則。 這個免套用原則可讓您在單一電腦上測試通訊程序。 將應用程式送出到 Microsoft Store 之前，您應該在沒有免套用原則的情況下測試您的應用程式。

  從 App 移除網路回送豁免：

  - 在 C# 和 Visual Basic 的 **[偵錯]** 屬性頁面上，取消選取 **[允許區域網路回送]** 核取方塊。
  - 在 JavaScript 和 C++ 的 [偵錯]  屬性頁面上，將 [允許區域網路回送]  的值設定為 [否]  。

- **不啟動，但在我的程式碼啟動時進行偵錯 / 啟動應用程式**

  將部署設定成在啟動 App 時自動啟動偵錯工作階段：

  - 在 C# 和 Visual Basic 的 **[偵錯]** 屬性頁面上，選取 **[不啟動，但在我的程式碼啟動時進行偵錯]** 核取方塊。
  - 在 JavaScript 和 C++ 的 [偵錯]  屬性頁面上，將 [啟動應用程式]  值設定為 [是]  。

## <a name="symbols"></a>符號

偵錯程式碼時，符號檔案包含各種非常有用的資料，例如變數、函式名稱和進入點位址，可讓您更清楚地了解意外狀況和呼叫堆疊執行順序。 大多數各種不同 Windows 版本的符號您都可以透過 [Microsoft 符號伺服器](https://msdl.microsoft.com/download/symbols)取得，或從[下載 Windows 符號套件](/windows-hardware/drivers/debugger/debugger-download-symbols)下載以進行更快速的離線查閱。

若要設定 Visual Studio 的符號選項，請選取 [工具] > [選項]  ，然後在對話方塊視窗中移至 [偵錯] > [符號]  。

![選項對話方塊](images/gs-debug-uwp-apps-004.png)

若要使用 [WinDbg](#windbg) 在偵錯工作階段中載入符號，請將 **sympath** 變數設定到符號套件位置。 例如，執行下列命令將會從「Microsoft 符號伺服器」載入符號，然後將它們快取到 C:\Symbols 目錄中：

```cmd
.sympath SRV*C:\Symbols*http://msdl.microsoft.com/download/symbols
.reload
```

您可以利用 `‘;’` 分隔符號，或使用 `.sympath+` 命令來新增更多路徑。 如需了解其他使用 WinDbg 的進階符號作業，請參閱[公用與專用符號](/windows-hardware/drivers/debugger/public-and-private-symbols)。

## <a name="windbg"></a>WinDbg

WinDbg 是功能強大的偵錯工具，隨附於 Windows 偵錯工具套件，此套件包含在 [Windows SDK](https://developer.microsoft.com/) 中。 Windows SDK 安裝可讓您將 Windows 偵錯工具安裝為獨立產品。 雖然 WinDbg 在進行原生程式碼偵錯時非常有用，但我們並不建議將 WinDbg 用於以 Managed 程式碼或 HTML 5 撰寫的 App。

若要將 WinDbg 與 UWP app 搭配使用，您必須先使用 PLMDebug 將您應用程式套件的「處理程序生命週期管理」(PLM) 停用，如[處理程序生命週期管理 (PLM) 的測試與偵錯工具](testing-debugging-plm.md)所述。

```cmd
plmdebug /enableDebug [PackageFullName] ""C:\Program Files\Debugging Tools for Windows (x64)\WinDbg.exe\" -server npipe:pipe=test"
```

相較於 Visual Studio，大部分 WinDbg 核心功能都依賴將命令提供給命令視窗。 提供的命令可讓您檢視執行狀態、調查使用者模式損毀傾印，並在不同的模式中偵錯。

WinDbg 當中最常用的其中一個命令是 `!analyze -v`，這是用來擷取目前例外狀況的相關詳細資訊量，包括：

- FAULTING_IP：錯誤時間的指令指標
- EXCEPTION_RECORD：目前例外狀況的位址、程式碼和旗標
- STACK_TEXT：例外狀況之前的堆疊追蹤

如需所有 WinDbg 命令的完整清單，請參閱[偵錯工具命令](/windows-hardware/drivers/debugger/debugger-commands)。

## <a name="related-topics"></a>相關主題

- [處理程序生命週期管理 (PLM) 的測試與偵錯工具](testing-debugging-plm.md)
- [偵錯、測試及效能](index.md)