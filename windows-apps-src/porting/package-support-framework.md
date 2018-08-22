---
author: normesta
Description: Fix issues that prevent your desktop application from running in an MSIX container
Search.Product: eADQiWindows 10XVcnh
title: 修正防止桌面應用程式執行 MSIX 容器中的問題
ms.author: normesta
ms.date: 07/02/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 46d5705233af9e8254b9ac89a2d6e9891e90701f
ms.sourcegitcommit: f2f4820dd2026f1b47a2b1bf2bc89d7220a79c1a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/22/2018
ms.locfileid: "2791553"
---
# <a name="apply-runtime-fixes-to-an-msix-package-by-using-the-package-support-framework"></a>執行階段修正使用套用至 MSIX 套件套件支援架構

套件支援架構是可協助您修正您現有的 win32 應用程式時套用您不需要存取來源的程式碼，使其能夠繼續運作 MSIX 容器中的開放原始碼套件。 套件支援架構可協助您依照摩登的執行階段環境的最佳作法的應用程式。

若要建立套件支援 Framework，我們利用[課程](https://www.microsoft.com/en-us/research/project/detours)技術也就是開發由 Microsoft Research (MSR) 開放原始碼架構有助於 API 重新導向及攔截。

此架構是開啟來源、 輕量，以及您可以使用它來解決應用程式問題快速。 它也讓您有機會洽詢遍、 社群並建置上的其他人投資的功能。

## <a name="a-quick-look-inside-of-the-package-support-framework"></a>快速瀏覽內部套件支援架構

套件支援架構包含可執行檔、 執行階段 manager DLL 及一組的執行階段修正項目。

![套件支援架構](images/desktop-to-uwp/package-support-framework.png)

它的運作方式如下： 您將建立會指定您想要套用至您的應用程式 fix(s) 設定檔。 然後，您將會修改您指向 shim 啟動器可執行檔的套件。

當使用者開始您的應用程式時、 shim 啟動器是執行的第一部可執行檔。 它會讀取您的設定檔，並會執行階段 fix(s)] 和 [執行階段管理員 DLL 將插入應用程式程序。

![套件支援架構 DLL 插入](images/desktop-to-uwp/package-support-framework-2.png)

執行階段 manager 套用修正視需要執行內部 MSIX 容器應用程式。

此指南可協助您識別應用程式相容性問題，並尋找、 apply、 及擴充 runtime 修正，予以處理。

<a id="identify" />

## <a name="identify-packaged-application-compatibility-issues"></a>找出封裝應用程式相容性問題

首先，建立應用程式套件。 然後將它安裝、 執行，並觀察其行為。 您可能會收到錯誤訊息，可協助您識別相容性問題。 您也可以使用以找出問題的[處理程序監視器](https://docs.microsoft.com/en-us/sysinternals/downloads/procmon)。  一般問題與應用程式假設有關使用目錄和程式路徑權限。

### <a name="using-process-monitor-to-identify-an-issue"></a>使用以識別問題的處理程序監視器

[程序監視](https://docs.microsoft.com/en-us/sysinternals/downloads/procmon)是功能強大的公用程式觀察應用程式檔案及登錄作業和其結果。  這可協助您了解應用程式相容性問題。  在開啟處理程序監視器後新增篩選 (篩選 > 篩選器...) 包含只從應用程式可執行檔的事件。

![ProcMon App 篩選](images/desktop-to-uwp/procmon_app_filter.png)

清單中的事件會出現。 許多這些事件的字**成功**會出現在 [**結果**] 欄中。

![ProcMon 事件](images/desktop-to-uwp/procmon_events.png)

（選用） 您可以篩選僅顯示 [僅限失敗事件。

![ProcMon 排除成功](images/desktop-to-uwp/procmon_exclude_success.png)

如果您懷疑檔案系統存取失敗時，搜尋 [System32/SysWOW64] 或 [套件檔案路徑下的失敗事件。 篩選器也可以協助 here 太。 這份清單底部開始，向上捲動。 最近發生失敗的出現在此清單的底部。 大部分都来包含如 「 拒絕存取，"字串的錯誤和"路徑/名稱找不到"，並忽略看起來可疑的事項。 [PSFSample](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/samples/PSFSample/)有兩個問題。 您可以看到這些問題會出現在下列圖像清單中。

![ProcMon Config.txt](images/desktop-to-uwp/procmon_config_txt.png)

中會出現在此影像中的第一個問題，位於 「 C:\Windows\SysWOW64"路徑的"Config.txt"檔案的讀取失敗之應用程式。 很可能互不應用程式嘗試直接參照的路徑。 很有可能嘗試從該檔案中讀取使用相對路徑，且根據預設，"System32/SysWOW64"的應用程式工作目錄。 這可能表示應用程式預期被設定成某套件中其目前工作目錄。 尋找內部 appx，我們可以看到檔案存在於可執行檔相同的目錄。

![應用程式 Config.txt](images/desktop-to-uwp/psfsampleapp_config_txt.png)

第二個問題會出現在下列映像中。

![ProcMon 記錄檔](images/desktop-to-uwp/procmon_logfile.png)

此問題，在應用程式失敗.log 檔案寫入其封裝路徑。 這會建議可協助檔案重新導向 shim。

<a id="find" />

## <a name="find-a-runtime-fix"></a>尋找 runtime 修正程式

PSF 包含您可以使用正，例如檔案重新導向 shim 的執行階段修正。

### <a name="file-redirection-shim"></a>檔案重新導向 Shim

您可以使用[檔案重新導向 Shim](https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/develop/FileRedirectionShim)重新導向嘗試寫入或讀取無法從 MSIX 容器以執行應用程式目錄中的資料。

例如，如果位於相同的目錄您可執行的應用程式記錄檔中寫入您的應用程式，然後您可以使用[檔案重新導向 Shim](https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/develop/FileRedirectionShim)在另一個位置，例如本機應用程式資料儲存區中建立的記錄檔。

### <a name="runtime-fixes-from-the-community"></a>從社群的執行階段修正程式

請務必檢閱社群捐款我們[GitHub](https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/develop) ] 頁面上。 有可能其他開發人員已解析類似於其他問題與共用的執行階段修正。

## <a name="apply-a-runtime-fix"></a>適用於執行階段修正程式

您可以從 Windows sdk （英文），然後依照下列步驟來套用現有的執行階段修正一些簡單的工具。

> [!div class="checklist"]
> * 建立套件版面配置資料夾
> * 取得套件支援架構檔案
> * 將其新增至您的套件
> * 修改套件資訊清單
> * 建立設定檔

我們會經歷每一項工作。

### <a name="create-the-package-layout-folder"></a>建立套件版面配置資料夾

如果您已經有.appx 檔案，您可以將會做為您的套件臨時區域的版面配置資料夾來解壓縮其內容。  您可以這麼做從**x64 VS 2017 的原生工具的命令提示字元處**，或手動 SDK bin 路徑中的可執行的搜尋路徑。

```
makeappx unpack /p PSFSamplePackage_1.0.60.0_AnyCPU_Debug.appx /d PackageContents

```

這會提供您看起來如下所示的內容。

![套件版面配置](images/desktop-to-uwp/package_contents.png)

如果您沒有.appx 檔案開始使用，您可以從零開始建立套件資料夾和檔案。

### <a name="get-the-package-support-framework-files"></a>取得套件支援架構檔案

您可以使用 Visual Studio 來取得 PSF Nuget 封裝 （英文）。 您也可以取得它使用獨立 Nuget 命令列工具。

#### <a name="get-the-package-by-using-visual-studio"></a>使用 Visual Studio 取得封裝 （英文）

在 Visual Studio 中，您解決方案] 或 [專案] 節點上按一下滑鼠右鍵，並選擇其中一個管理 Nuget 套件命令。  搜尋**Microsoft.PackageSupportFramework**或**PSF** Nuget.org 上尋找封裝 （英文）。然後安裝它。

#### <a name="get-the-package-by-using-the-command-line-tool"></a>使用命令列工具，取得封裝 （英文）

從下列位置安裝 Nuget 命令列工具： https://www.nuget.org/downloads。 然後，在 Nuget 命令列中執行下列命令：

```
nuget install Microsoft.PackageSupportFramework
```

### <a name="add-the-package-support-framework-files-to-your-package"></a>將套件支援架構檔案新增至您的套件

將必要的 32 位元和 64 位元 PSF Dll 及可執行檔新增至套件目錄。 使用下表做為指引。 您也將想要包含任何您需要的執行階段修正項目。 在我們的範例中，我們需要檔案重新導向 runtime 修正程式。

| 應用程式可執行檔是 x64 | 應用程式可執行檔是 x86 |
|-------------------------------|-----------|
| [ShimLauncher64.exe](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/ShimLauncher/readme.md) |  [ShimLauncher32.exe](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/ShimLauncher/readme.md) |
| [ShimRuntime64.dll](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/ShimRuntime/readme.md) | [ShimRuntime32.dll](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/ShimRuntime/readme.md) |
| [ShimRunDll64.exe](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/ShimRunDll/readme.md) | [ShimRunDll32.exe](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/ShimRunDll/readme.md) |

套件內容現在看起來應類似如下。

![套件二進位檔案](images/desktop-to-uwp/package_binaries.png)

### <a name="modify-the-package-manifest"></a>修改套件資訊清單

在文字編輯器中開啟封裝資訊清單和則設`Executable`屬性`Application`元素 shim 啟動器可執行檔的名稱。  如果您知道您的目標應用程式的架構，選取適當的版本、 ShimLauncher32.exe 或 ShimLauncher64.exe。  如果沒有，ShimLauncher32.exe 會在所有的情況下運作。  範例如下。

```xml
<Package ...>
  ...
  <Applications>
    <Application Id="PSFSample"
                 Executable="ShimLauncher32.exe"
                 EntryPoint="Windows.FullTrustApplication">
      ...
    </Application>
  </Applications>
</Package>
```

### <a name="create-a-configuration-file"></a>建立設定檔

建立的檔案名稱``config.json``，並將該檔案儲存至您的套件的根目錄。 修改 config.json 檔案以指向您剛取代的可執行檔的宣告應用程式識別碼。 使用您無法使用處理序監視器所獲得的知識，您也可以設定工作目錄為以及使用檔案重新導向 shim 重新導向至套件相對"PSFSampleApp"目錄下的.log 檔案的讀取/寫入。

```json
{
    "applications": [
        {
            "id": "PSFSample",
            "executable": "PSFSampleApp/PSFSample.exe",
            "workingDirectory": "PSFSampleApp/"
        }
    ],
    "processes": [
        {
            "executable": "PSFSample",
            "shims": [
                {
                    "dll": "FileRedirectionShim.dll",
                    "config": {
                        "redirectedPaths": {
                            "packageRelative": [
                                {
                                    "base": "PSFSampleApp/",
                                    "patterns": [
                                        ".*\\.log"
                                    ]
                                }
                            ]
                        }
                    }
                }
            ]
        }
    ]
}
```
以下是 config.json 結構描述的指南：

| 陣列 | key | 值 |
|-------|-----------|-------|
| applications | id |  使用的值`Id`屬性`Application`套件資訊清單中的項目。 |
| applications | 可執行檔 | 您想要啟動的可執行檔套件相對路徑。 在大多數情況下，您可以從您的套件資訊清單檔案取得這個值之前您加以修改。 它是值`Executable`屬性`Application`元素。 |
| applications | workingDirectory | （選用）作為啟動的應用程式工作目錄套件相對路徑。 如果您未設定此值，會使用作業系統`System32`目錄為應用程式的工作目錄。 |
| 程序 | 可執行檔 | 在大多數情況下，這將是名稱`executable`上方路徑和檔案副檔名為移除已設定。 |
| 相容性修正 | dll | 載入 shim.appx 套件相對路徑。 |
| 相容性修正 | config | （選用）控制 shim dl 的行為。 為每個 shim 可解譯此"blob"它想 shim 由 shim 為基礎而異此值的確切的格式。 |

`applications`、 `processes`，及`shims`機碼是陣列。 這表示您可以使用 config.json 檔案指定一個以上的應用程式、 程序及 shim DLL。


### <a name="package-and-test-the-app"></a>封裝並測試應用程式

接下來，建立套件。

```
makeappx pack /d PackageContents /p PSFSamplePackageFixup.appx
```

然後，登入。

```
signtool sign /a /v /fd sha256 /f ExportedSigningCertificate.pfx PSFSamplePackageFixup.appx
```

如需詳細資訊，請參閱 ＜[如何建立簽署憑證的封裝](https://docs.microsoft.com/en-us/windows/desktop/appxpkg/how-to-create-a-package-signing-certificate)並[簽署使用簽署工具套件](https://docs.microsoft.com/en-us/windows/desktop/appxpkg/how-to-sign-a-package-using-signtool)

使用 PowerShell，安裝套件。

>[!NOTE]
> 請記得先解除安裝該套件。

```
powershell Add-AppxPackage .\PSFSamplePackageFixup.appx
```

執行應用程式並觀察行為與套用的執行階段修正程式。  重複的診斷與封裝為所需的步驟。

### <a name="use-the-trace-shim"></a>使用追蹤 Shim

若要診斷封裝應用程式相容性問題替代技術是使用追蹤 Shim。 此 DLL 隨附 PSF 並提供類似程序監視的應用程式的行為的詳細診斷檢視。  它是專門設計來顯示應用程式相容性問題。  若要使用追蹤 Shim、 DLL 新增到套件，將下列片段新增至您 config.json，然後套件安裝應用程式。

```json
{
    "dll": "TraceShim.dll",
    "config": {
        "traceLevels": {
            "filesystem": "allFailures"
        }
    }
}
```

根據預設，追蹤 Shim 篩選出可能會被視為"預期"的失敗。  例如，應用程式可能會嘗試無條件刪除檔案而不檢查是否它已經存在，忽略結果。 這有一些未預期的失敗可能會取得篩選掉、 不幸後果，因此在上述範例中，我們加入確認程序接收所有失敗的檔案系統的功能。 因為我們知道從之前的嘗試從 Config.txt 檔案中讀取失敗訊息 「 找不到檔案"，我們可以這麼做。 這是經常觀察且未通常假設其值為未預期的失敗。 作法是在很可能適合僅篩選在未預期的失敗，並再回頭所有失敗如果問題仍然無法識別出啟動。

根據預設，追蹤 Shim 的輸出會取得傳送給附加偵錯工具。 此範例中，我們未移至附加偵錯工具，和將會改用從 SysInternals [DebugView](https://docs.microsoft.com/en-us/sysinternals/downloads/debugview)程式來檢視其輸出。 之後執行應用程式，我們可以看到相同失敗為之前，這會指向我們向相同的執行階段修正程式。

![找不到 TraceShim 檔案](images/desktop-to-uwp/traceshim_filenotfound.png)

![TraceShim 存取遭拒](images/desktop-to-uwp/traceshim_accessdenied.png)

## <a name="debug-extend-or-create-a-runtime-fix"></a>偵錯、 擴充或建立的執行階段修正

您可以使用 Visual Studio 偵錯的執行階段修正、 擴充 runtime 修正程式或建立 web 應用程式從零開始。 您需要執行這些動作設為 [成功。

> [!div class="checklist"]
> * 新增封裝專案
> * 新增執行階段修正的專案
> * 新增啟動 Shim 啟動器可執行的專案
> * 設定封裝專案

當您完成時，您的解決方案會尋找類似如下。

![完成的方案](images/desktop-to-uwp/runtime-fix-project-structure.png)

讓我們看看在本例中的每個專案。

| 專案 | 用途 |
|-------|-----------|
| DesktopApplicationPackage | 這個專案會根據[Windows 應用程式封裝專案](desktop-to-uwp-packaging-dot-net.md)，它會輸出 MSIX 封裝 （英文）。 |
| Runtimefix | 這是包含一或多個取代函數做為執行階段修正 c + + Dynamic-Linked 文件庫專案。 |
| ShimLauncher | 這是 c + + 空專案。 這個專案是要收集的套件支援 Framework runtime 可散發套件檔案的位置。 它會輸出可執行檔。 該可執行檔會最先會在您開始解決方案時執行。 |
| WinFormsDesktopApplication | 這個專案包含桌面應用程式來源程式碼。 |

若要查看完整的範例包含所有這些類型的專案，請參閱[PSFSample](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/samples/PSFSample/)。

我們逐步解說中建立和設定這些專案的每個解決方案的步驟。


### <a name="create-a-package-solution"></a>建立套件方案

如果您已經沒有解決方案桌面應用程式，請在 Visual Studio 建立新的**空白的解決方案**。

![空白的解決方案](images/desktop-to-uwp/blank-solution.png)

您也可能會想要新增您有任何應用程式專案。

### <a name="add-a-packaging-project"></a>新增封裝專案

如果您沒有已在**Windows 應用程式封裝專案**、 建立 web 應用程式並將其新增至您的解決方案。

![套件專案範本](images/desktop-to-uwp/package-project-template.png)

如需有關 Windows 應用程式封裝專案的詳細資訊，請參閱[套件使用 Visual Studio 應用程式](desktop-to-uwp-packaging-dot-net.md)。

在**方案總管**] 中以滑鼠右鍵按一下 [封裝專案、 選取 [**編輯**]，然後將此專案檔案的底部加入：

```
<Target Name="PSFRemoveSourceProject" AfterTargets="ExpandProjectReferences" BeforeTargets="_ConvertItems">
<ItemGroup>
  <FilteredNonWapProjProjectOutput Include="@(_FilteredNonWapProjProjectOutput)">
  <SourceProject Condition="'%(_FilteredNonWapProjProjectOutput.SourceProject)'=='<your runtime fix project name goes here>'" />
  </FilteredNonWapProjProjectOutput>
  <_FilteredNonWapProjProjectOutput Remove="@(_FilteredNonWapProjProjectOutput)" />
  <_FilteredNonWapProjProjectOutput Include="@(FilteredNonWapProjProjectOutput)" />
</ItemGroup>
</Target>
```

### <a name="add-project-for-the-runtime-fix"></a>新增執行階段修正的專案

C + +**動態連結程式庫 (DLL)** 將專案新增至解決方案。

![執行階段修正程式庫](images/desktop-to-uwp/runtime-fix-library.png)

以滑鼠右鍵按一下，專案，然後選擇 [**內容**。

在 [屬性] 頁面中尋找**C + + 語言標準**的欄位，然後在下拉式清單中按一下該欄位旁，選取 [ **ISO C + + 17 標準 (/ std:c + + 17)** ] 選項。

![ISO 17 選項](images/desktop-to-uwp/iso-option.png)

以滑鼠右鍵按一下該專案，並再快顯功能表中選擇 [**管理 Nuget 套件**] 選項。 確定**所有**或**nuget.org**設為 [**套件來源**] 選項。

按一下 [設定] 圖示下一步] 的欄位。

搜尋*PSF** Nuget 封裝，然後再安裝這個專案。

![nuget 套件](images/desktop-to-uwp/psf-package.png)

如果您想要進行偵錯或擴充現有的執行階段修正，新增使用本指南[找出 runtime 修正](#find)一節中所述的指導您取得 runtime 修正檔。

如果您想要建立全新的修正程式，沒有任何項目此專案加入只是尚未。 我們將協助您稍後的本指南此專案中加入了正確的檔案。 現在，我們就會繼續設定您的解決方案。

### <a name="add-a-project-that-starts-the-shim-launcher-executable"></a>新增啟動 Shim 啟動器可執行的專案

C + +**空專案**將專案新增至解決方案。

![空專案](images/desktop-to-uwp/blank-app.png)

使用上一節所述的相同指導此專案新增**PSF** Nuget 套件。

開啟專案，並在 [**一般**設定] 頁面中的屬性頁**目標名稱**將屬性設定為``ShimLauncher32``或``ShimLauncher64``根據您的應用程式的架構。

![shim 啟動器參考 （英文)](images/desktop-to-uwp/shim-exe-reference.png)

在解決方案中新增 runtime 修正專案的專案參照。

![執行階段修正參考 （英文)](images/desktop-to-uwp/reference-fix.png)

參考 （英文）、 上按一下滑鼠右鍵，然後套用 [**屬性**] 視窗中的 [這些值。

| 屬性 | 值 |
|-------|-----------|-------|
| 複製本機 | True |
| 複製本機附屬組件 | True |
| 參照組件輸出 | True |
| 連結文件庫相依性 | False |
| 連結文件庫相依性輸入 | False |

### <a name="configure-the-packaging-project"></a>設定封裝專案

在封裝專案的 **\[應用程式\]** 資料夾上按一下滑鼠右鍵，然後選擇 **\[加入參考\]**。

![加入專案參考](images/desktop-to-uwp/add-reference-packaging-project.png)

選擇 shim 啟動器專案與桌面應用程式專案，然後選擇 [**確定**] 按鈕。

![桌面專案](images/desktop-to-uwp/package-project-references.png)

>[!NOTE]
> 如果您不需要您的應用程式來源程式碼，剛選擇 shim 啟動器專案。 我們將示範如何建立設定檔時參照您可執行檔。

在 [**應用程式**] 節點上按一下滑鼠右鍵 shim 啟動器應用程式，，然後選擇 [**設為進入點**。

![設定進入點](images/desktop-to-uwp/set-startup-project.png)

新增名為 csv 檔案``config.json``封裝專案，然後複製並貼入該檔案中的下列 json 文字。 **套件巨集指令**將屬性設定為**內容**。

```json
{
    "applications": [
        {
            "id": "",
            "executable": "",
            "workingDirectory": ""
        }
    ],
    "processes": [
        {
            "executable": "",
            "shims": [
                {
                    "dll": "",
                    "config": {
                    }
                }
            ]
        }
    ]
}
```
提供每個索引鍵的值。 使用此表作為指引。

| 陣列 | key | 值 |
|-------|-----------|-------|
| applications | id |  使用的值`Id`屬性`Application`套件資訊清單中的項目。 |
| applications | 可執行檔 | 您想要啟動的可執行檔套件相對路徑。 在大多數情況下，您可以從您的套件資訊清單檔案取得這個值之前您加以修改。 它是值`Executable`屬性`Application`元素。 |
| applications | workingDirectory | （選用）作為啟動的應用程式工作目錄套件相對路徑。 如果您未設定此值，會使用作業系統`System32`目錄為應用程式的工作目錄。 |
| 程序 | 可執行檔 | 在大多數情況下，這將是名稱`executable`上方路徑和檔案副檔名為移除已設定。 |
| 相容性修正 | dll | Shim DLL 載入套件相對路徑。 |
| 相容性修正 | config | （選用）控制 shim dl 的行為。 為每個 shim 可解譯此"blob"它想 shim 由 shim 為基礎而異此值的確切的格式。 |

當您完成時，您``config.json``檔案將外觀類似如下。

```json
{
  "applications": [
    {
      "id": "DesktopApplication",
      "executable": "DesktopApplication/WinFormsDesktopApplication.exe",
      "workingDirectory": "WinFormsDesktopApplication"
    }
  ],
  "processes": [
    {
      "executable": ".*App.*",
      "shims": [ { "dll": "RuntimeFix.dll" } ]
    }
  ]
}

```

>[!NOTE]
> `applications`、 `processes`，及`shims`機碼是陣列。 這表示您可以使用 config.json 檔案指定一個以上的應用程式、 程序及 shim DLL。

### <a name="debug-a-runtime-fix"></a>偵錯 runtime 修正程式

在 Visual Studio 中，按 f5 鍵來啟動偵錯工具。  啟動的第一個項重點是依次啟動目標桌面應用程式的 shim 啟動器應用程式。  偵錯目標桌面應用程式，您必須手動附加至桌面應用程式的處理序選擇 [**偵錯**->**附加至處理序**，然後再選取 [應用程式程序。 若要允許使用 DLL 的原生 runtime 修正.NET 應用程式的偵錯，選取 managed 和原生程式碼類型 （混合的模式中偵錯）。  

一旦您已設定此，您可以設定行中的程式碼] 旁的符號點桌面應用程式碼和執行階段修正專案中。 如果您沒有來源程式碼應用程式，您將能夠在執行階段修正專案中設定僅限旁的程式碼行符號點。

>[!NOTE]
> 雖然 Visual Studio 可讓您最簡單的開發及偵錯經驗時，有一些限制，因此稍後的這個輔助線，我們將討論您可以套用其他偵錯技巧。

## <a name="create-a-runtime-fix"></a>建立執行階段修正程式

如果沒有尚未執行階段修正的問題您想要解析，您可以建立新的執行階段修正撰寫取代的功能和包括任何組態資料的合理。 我們查看每個部分。

### <a name="replacement-functions"></a>取代的功能

首先，識別哪些函數呼叫失敗時您的應用程式執行 MSIX 容器中。 然後，您可以建立您想要改用呼叫的執行階段管理員的取代功能。 這讓您有機會之函數的實作取代符合規則的現代的執行階段環境的行為。

在 Visual Studio 中，開啟您稍早在本指南中建立的執行階段修正專案。

宣告``SHIM_DEFINE_EXPORTS``巨集，然後再新增的 include 陳述式`shim_framework.h`頂端的每個。您要加入的執行階段修正函數 CPP 檔案。

```c++
#define SHIM_DEFINE_EXPORTS
#include <shim_framework.h>
```
>[!IMPORTANT]
>請確定`SHIM_DEFINE_EXPORTS`巨集顯示之前的 include 陳述式。

建立具有相同的簽章之函數的函數誰具有您要修改的行為。 以下是範例函數，會取代`MessageBoxW`函數。

```c++
auto MessageBoxWImpl = &::MessageBoxW;
int WINAPI MessageBoxWShim(
    _In_opt_ HWND hwnd,
    _In_opt_ LPCWSTR,
    _In_opt_ LPCWSTR caption,
    _In_ UINT type)
{
    return MessageBoxWImpl(hwnd, L"SUCCESS: This worked", caption, type);
}

DECLARE_SHIM(MessageBoxWImpl, MessageBoxWShim);
```

在呼叫`DECLARE_SHIM`對應`MessageBoxW`函數，以新的取代函數。 您的應用程式嘗試呼叫`MessageBoxW`函數，它會呼叫取代函數改用。

#### <a name="protect-against-recursive-calls-to-functions-in-runtime-fixes"></a>防止遞迴來電在執行階段修正項目中的功能

您可以選擇性地將套用`reentrancy_guard`防止在執行階段修正函數遞迴呼叫您函數的類型。

例如，您可能會產生的取代函數`CreateFile`函數。 實作可能會呼叫`CopyFile`函數，但實作的`CopyFile`函數可能會呼叫`CreateFile`函數。 這可能會導致呼叫無限遞迴週期`CreateFile`函數。

如需詳細資訊`reentrancy_guard`查看[authoring.md](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/Authoring.md)

### <a name="configuration-data"></a>組態資料

如果您想要將組態資料新增至執行階段修正，請考慮將它新增``config.json``。 如此一來，您可以使用`ShimQueryCurrentDllConfig`輕鬆剖析該資料。 本範例會剖析該設定檔的布林值和字串值。

```c++
if (auto configRoot = ::ShimQueryCurrentDllConfig())
{
    auto& config = configRoot->as_object();

    if (auto enabledValue = config.try_get("enabled"))
    {
        g_enabled = enabledValue->as_boolean().get();
    }

    if (auto logPathValue = config.try_get("logPath"))
    {
        g_logPath = logPathValue->as_string().wstring();
    }
}
```

## <a name="other-debugging-techniques"></a>其他偵錯技巧 （英文)

雖然 Visual Studio 可讓您最簡單的開發和偵錯經驗，有一些限制。

首先，F5 偵錯執行應用程式部署寬鬆檔案從套件版面配置的資料夾路徑，而不是從.appx 套件安裝。  [版面配置] 資料夾通常沒有相同的安全性限制為已安裝的套件的資料夾。 因此，其可能無法重現之前先套用的執行階段修正封裝路徑存取拒絕錯誤。

若要解決此問題，使用.appx 套件部署而不是 F5 寬鬆檔案部署。  若要建立.appx 套件檔案，使用[MakeAppx](https://docs.microsoft.com/en-us/windows/desktop/appxpkg/make-appx-package--makeappx-exe-)公用程式從 Windows sdk （英文），如前文所述。 或者，從 Visual Studio 內應用程式的 [專案] 節點上按一下滑鼠右鍵並選取**儲存區**->**建立應用程式套件**。

使用 Visual Studio 的另一個問題是它沒有內建支援將附加至由偵錯程式啟動任何子程序。   這難偵錯的目標應用程式必須手動附加的 Visual Studio 啟動後啟動路徑中的邏輯。

若要解決此問題，使用支援子程序偵錯程式附加。  請注意其通常不可以將剛剛-時間 (JIT) 偵錯工具附加至的目標應用程式。  這是因為大部分 JIT 技術涉及啟動以取代目標應用程式，透過 ImageFileExecutionOptions 登錄機碼偵錯工具。  這就失去了 ShimLauncher.exe 用以目標應用程式中插入 ShimRuntime.dll detouring 機制。  WinDbg 中包含[的 Windows 偵錯工具](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/index)、 和取自[Windows sdk （英文)](https://developer.microsoft.com/en-US/windows/downloads/windows-10-sdk)、 attach 支援子程序。  它也現在支援直接[啟動及偵錯 UWP 應用程式](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/debugging-a-uwp-app-using-windbg#span-idlaunchinganddebuggingauwpappspanspan-idlaunchinganddebuggingauwpappspanspan-idlaunchinganddebuggingauwpappspanlaunching-and-debugging-a-uwp-app)。

偵錯為子程序的目標應用程式啟動，啟動``WinDbg``。

```
windbg.exe -plmPackage PSFSampleWithFixup_1.0.59.0_x86__7s220nvg1hg3m -plmApp PSFSample
```

在``WinDbg``提示、 啟用子偵錯及設定適當的中斷點。

```
.childdbg 1
g
```
（執行直到目標應用程式啟動並中斷偵錯）

```
sxe ld fixup.dll
g
```
（要等到載入 DLL 修復工具執行）

```
bp ...
```

>[!NOTE]
> [PLMDebug](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/plmdebug)也可用來將偵錯程式附加至時啟動時，應用程式，而且也包含在[For Windows 偵錯工具](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/index)。  不過，它是使用比現在 WinDbg 所提供的直接支援較複雜。

## <a name="support-and-feedback"></a>支援與意見反應

**尋找您的問題解答**

有任何問題嗎？ 請在 Stack Overflow 上發問。 我們的團隊會監視這些[標記](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge)。 您也可以[在此處](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D)詢問我們。
