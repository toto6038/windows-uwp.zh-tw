---
author: normesta
Description: Fix issues that prevent your desktop application from running in an MSIX container
Search.Product: eADQiWindows 10XVcnh
title: 修正問題，使您無法在 MSIX 容器中執行的傳統型應用程式
ms.author: normesta
ms.date: 07/02/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: d4b4cae2e135f7a66cd68192faabeffdb309a909
ms.sourcegitcommit: 933e71a31989f8063b020746fdd16e9da94a44c4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/11/2018
ms.locfileid: "4534196"
---
# <a name="apply-runtime-fixes-to-an-msix-package-by-using-the-package-support-framework"></a>使用套件支援 Framework MSIX 封裝套用執行階段的修正程式

套件支援架構是開放原始碼套件，可協助您將套用修正程式現有的 win32 應用程式時您不需要的存取權的原始程式碼，讓它可以 MSIX 容器中執行。 套件支援架構可協助您的應用程式遵循的現代化的執行階段環境的最佳做法。

若要深入了解，請參閱[套件支援的架構](https://docs.microsoft.com/windows/msix/package-support-framework-overview)。

本指南將協助您找出應用程式相容性問題，並尋找、 套用和延伸執行階段來修正造成滿足他們的。

<a id="identify" />

## <a name="identify-packaged-application-compatibility-issues"></a>找出已封裝的應用程式相容性問題

首先，建立您的應用程式套件。 然後，將其安裝、 執行，並觀察其行為。 您可能會收到錯誤訊息，可協助您找出相容性問題。 您也可以使用[處理程序監視器](https://docs.microsoft.com/en-us/sysinternals/downloads/procmon)，以找出問題。  常見的問題與應用程式假設有關的工作目錄及程式路徑權限。

### <a name="using-process-monitor-to-identify-an-issue"></a>使用來找出問題的處理程序監視器

[處理程序監視器](https://docs.microsoft.com/en-us/sysinternals/downloads/procmon)是功能強大的公用程式觀察應用程式的檔案和登錄作業，以及它們的結果。  這可協助您了解應用程式相容性問題。  開啟處理程序監視器之後, 新增篩選器 (篩選 > 篩選...) 包含只從應用程式可執行檔的事件。

![ProcMon 應用程式篩選器](images/desktop-to-uwp/procmon_app_filter.png)

事件清單會顯示。 針對許多這些事件，為文字**成功**會出現在 [**結果**] 欄位。

![ProcMon 事件](images/desktop-to-uwp/procmon_events.png)

或者，您可以篩選事件，只顯示 [僅限失敗。

![ProcMon 排除成功](images/desktop-to-uwp/procmon_exclude_success.png)

如果您懷疑檔案系統權限失敗，搜尋 System32/SysWOW64 或套件檔案路徑下的失敗事件。 篩選器也可以協助在這裡，太。 啟動底部的這份清單，然後向上捲動。 最近發生失敗的底部的這份清單會顯示。 大部分注意的錯誤，包含字串，例如 「 拒絕存取 」 和 「 路徑/名稱找不到 」，並且忽略看起來可疑的項目。 [PSFSample](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/samples/PSFSample/)有兩個問題。 您可以看到下列影像中出現的清單中的這些問題。

![ProcMon Config.txt](images/desktop-to-uwp/procmon_config_txt.png)

在第一個問題出現在此映像中，應用程式失敗位於 「 C:\Windows\SysWOW64 「 路徑 」 Config.txt 」 檔案中讀取。 也不太可能應用程式正在嘗試直接參考該路徑。 最有可能，它嘗試讀取該檔案，藉由使用相對路徑，並根據預設，「 System32/SysWOW64 」 是應用程式的工作目錄。 這會建議應用程式預期其目前的工作目錄某處設定套件中。 尋找 appx 內，我們可以看到該檔案位於與可執行檔相同的目錄。

![應用程式 Config.txt](images/desktop-to-uwp/psfsampleapp_config_txt.png)

在下列影像中，會顯示第二個問題。

![ProcMon 記錄檔](images/desktop-to-uwp/procmon_logfile.png)

這個問題，請在應用程式無法將其封裝路徑中寫入.log 檔案。 這會建議的檔案重新導向修復可能會幫助。

<a id="find" />

## <a name="find-a-runtime-fix"></a>尋找執行階段修正程式

PSF 包含執行階段的修正程式，您可以在這個時候，使用像是檔案重新導向修復。

### <a name="file-redirection-fixup"></a>檔案重新導向修復

您可以使用[檔案重新導向修復](https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/develop/FileRedirectionShim)重新導向嘗試寫入或讀取不是從 MSIX 容器中執行的應用程式存取目錄中的資料。

例如，如果您的應用程式寫入的記錄檔是位於與您的應用程式可執行檔相同的目錄，然後您可以使用[檔案重新導向修復](https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/develop/FileRedirectionShim)在另一個位置，例如本機應用程式資料存放區中建立該記錄檔。

### <a name="runtime-fixes-from-the-community"></a>執行階段從社群的修正程式

請務必檢閱社群比重被到我們的[GitHub](https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/develop)頁面。 它是可能的其他開發人員已解決類似於您的問題，並有共用的執行階段修正。

## <a name="apply-a-runtime-fix"></a>適用於執行階段修正程式

從 Windows SDK 中，並依照下列步驟，您可以套用現有的執行階段修正的幾個簡單的工具。

> [!div class="checklist"]
> * 建立套件配置資料夾
> * 取得套件支援的架構檔案
> * 將它們新增到您的套件
> * 修改套件資訊清單
> * 建立設定檔

讓我們瀏覽每個工作。

### <a name="create-the-package-layout-folder"></a>建立套件配置資料夾

如果您已經有.msix （或是.appx） 檔案，您可以到配置資料夾，將會做為您的套件預備區域 unpack 其內容。  您可以從執行此操作**x64 適用於 VS 2017 原生工具的命令提示字元中**，或手動使用的可執行檔的搜尋路徑中的 SDK bin 路徑。

```
makemsix unpack /p PSFSamplePackage_1.0.60.0_AnyCPU_Debug.msix /d PackageContents

```

這將讓您看起來如下所示的項目。

![封裝配置](images/desktop-to-uwp/package_contents.png)

如果您沒有.msix （或是.appx） 檔案開始畫面，您可以從頭開始建立套件資料夾和檔案。

### <a name="get-the-package-support-framework-files"></a>取得套件支援的架構檔案

您可以透過使用 Visual Studio 取得 PSF Nuget 套件。 您也可以使用獨立 Nuget 命令列工具來取得它。

#### <a name="get-the-package-by-using-visual-studio"></a>透過使用 Visual Studio，以取得套件

在 Visual Studio 中，您的方案或專案節點上按一下滑鼠右鍵並選擇其中一項管理 Nuget 套件命令。  搜尋**Microsoft.PackageSupportFramework**或**PSF**尋找 Nuget.org 套件。然後，安裝它。

#### <a name="get-the-package-by-using-the-command-line-tool"></a>透過使用命令列工具，以取得套件

安裝 Nuget 命令列工具從這個位置： https://www.nuget.org/downloads。 然後，從 Nuget 命令列中，執行下列命令：

```
nuget install Microsoft.PackageSupportFramework
```

### <a name="add-the-package-support-framework-files-to-your-package"></a>將套件支援的架構檔案新增到您的套件

將所需的 32 位元和 64 位元 PSF Dll 和可執行檔新增到套件目錄中。 使用下表做為指引。 您也會想要包含您所需要的任何執行階段修正程式。 在範例中，我們需要的檔案重新導向執行階段修正。

| 應用程式可執行檔是 x64 | 應用程式可執行檔是 x86 |
|-------------------------------|-----------|
| [PSFLauncher64.exe](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/ShimLauncher/readme.md) |  [PSFLauncher32.exe](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/ShimLauncher/readme.md) |
| [PSFRuntime64.dll](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/ShimRuntime/readme.md) | [PSFRuntime32.dll](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/ShimRuntime/readme.md) |
| [PSFRunDll64.exe](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/ShimRunDll/readme.md) | [PSFRunDll32.exe](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/ShimRunDll/readme.md) |

現在您套件的內容應該看起來像這樣。

![套件二進位檔](images/desktop-to-uwp/package_binaries.png)

### <a name="modify-the-package-manifest"></a>修改套件資訊清單

在文字編輯器中開啟您的封裝資訊清單，然後設定`Executable`屬性`Application`項目 PSF 啟動可執行檔的名稱。  如果您知道您的目標應用程式的架構，選取適當的版本，PSFLauncher32.exe 或 PSFLauncher64.exe。  如果沒有，PSFLauncher32.exe 能在所有情況下。  這裡提供一個範例。

```xml
<Package ...>
  ...
  <Applications>
    <Application Id="PSFSample"
                 Executable="PSFLauncher32.exe"
                 EntryPoint="Windows.FullTrustApplication">
      ...
    </Application>
  </Applications>
</Package>
```

### <a name="create-a-configuration-file"></a>建立設定檔

建立檔案名稱``config.json``，並將該檔案儲存到您的套件的根資料夾。 修改 config.json 檔案的宣告的應用程式識別碼，將它指向您只是取代的可執行檔。 使用您所獲得使用處理程序監視器的知識，您也可以設定工作目錄，以及使用檔案重新導向修復重新導向至.log 檔案的相對套件的 「 PSFSampleApp 」 目錄下的讀取/寫入。

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
            "fixups": [
                {
                    "dll": "FileRedirectionFixup.dll",
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
| applications | id |  使用的值，`Id`屬性`Application`套件資訊清單中的項目。 |
| applications | 可執行檔 | 您想要啟動的可執行檔套件相對路徑。 在大部分情況下，您可以從您的封裝資訊清單檔案取得此值，才能加以修改。 它是值的`Executable`屬性`Application`項目。 |
| applications | workingDirectory | （選擇性）若要使用做為工作目錄的應用程式的啟動與套件相對的路徑。 如果您未設定此值，作業系統會使用`System32`目錄做為應用程式的工作目錄。 |
| 處理程序 | 可執行檔 | 在大部分情況下，這會是名稱`executable`上述設定已移除的路徑和檔案副檔名。 |
| 修復 | dll | 修復，載入.msix/.appx 套件相對路徑。 |
| 修復 | 設定 | （選擇性）控制修復 dl 的運作方式。 此值的確切格式而有所不同修復的修復為基礎，為每個修復可解譯此 「 blob 」 想。 |

`applications`， `processes`，以及`fixups`鍵是陣列。 這表示，您可以使用 config.json 檔案來指定多個應用程式、 處理程序，以及修復 DLL。


### <a name="package-and-test-the-app"></a>套件和測試應用程式

接下來，建立的套件。

```
makeappx pack /d PackageContents /p PSFSamplePackageFixup.msix
```

然後，登入。

```
signtool sign /a /v /fd sha256 /f ExportedSigningCertificate.pfx PSFSamplePackageFixup.msix
```

如需詳細資訊，請參閱[如何建立套件簽署憑證](https://docs.microsoft.com/en-us/windows/desktop/appxpkg/how-to-create-a-package-signing-certificate)，以及[如何使用 signtool 簽署套件](https://docs.microsoft.com/en-us/windows/desktop/appxpkg/how-to-sign-a-package-using-signtool)

使用 PowerShell，安裝套件。

>[!NOTE]
> 請記得要先解除安裝套件。

```
powershell Add-MSIXPackage .\PSFSamplePackageFixup.msix
```

執行應用程式，並觀察的行為與執行階段套用的修正程式。  重複執行診斷與封裝為所需的步驟。

### <a name="use-the-trace-fixup"></a>使用追蹤修復

若要診斷已封裝的應用程式相容性問題的替代技術是使用追蹤修復。 這個 DLL 隨附於 PSF，並提供詳細的診斷檢視的應用程式的行為，類似於處理程序監視器。  此外，它是特別設計顯色的應用程式相容性問題。  使用追蹤修復、 將 DLL 新增到 「 套件 」，將下列片段新增到您 config.json，然後將封裝和安裝和您的應用程式。

```json
{
    "dll": "TraceFixup.dll",
    "config": {
        "traceLevels": {
            "filesystem": "allFailures"
        }
    }
}
```

根據預設，追蹤修復會篩選掉可能會被視為 「 必須 」 的失敗。  例如，應用程式可能會嘗試無條件刪除檔案，而不檢查以確認是否已經存在，略過結果。 這會有某些非預期的錯誤可能會篩選出來，不幸結果，因此在上述範例中，我們選擇從檔案系統的函式會收到所有失敗。 因為我們知道從之前，先嘗試讀取 Config.txt 檔案中會失敗訊息 「 找不到檔案 」，我們可以這麼做。 這是很頻繁觀察到，且通常不會假設為非預期的失敗。 實際上它是最有可能出篩選，只會以預期的失敗，以及然後後援的所有失敗，仍無法識別的問題是否啟動。

根據預設，追蹤修復的輸出傳送到在附加偵錯工具。 針對此範例，我們將不是移至附加偵錯工具，並將會改為使用 SysInternals [DebugView](https://docs.microsoft.com/en-us/sysinternals/downloads/debugview)程式來檢視其輸出。 在之後執行該應用程式，我們可以看到相同的失敗像之前一樣，這會我們指向相同的執行階段修正程式。

![找不到 TraceShim 檔案](images/desktop-to-uwp/traceshim_filenotfound.png)

![TraceShim 存取被拒](images/desktop-to-uwp/traceshim_accessdenied.png)

## <a name="debug-extend-or-create-a-runtime-fix"></a>偵錯、 延長或建立執行階段修正程式

您可以使用 Visual Studio 偵錯的執行階段修正、 延伸執行階段修正程式，或建立一個從頭。 您將需要執行下列動作，才能成功。

> [!div class="checklist"]
> * 新增封裝專案
> * 新增適用於執行階段修正的專案
> * 新增專案啟動可執行檔 PSF 啟動程式
> * 在設定封裝專案

當您完成後時，您的解決方案看起來會像這樣。

![已完成的解決方案](images/desktop-to-uwp/runtime-fix-project-structure.png)

我們來看看每個專案，在此範例中。

| 專案 | 用途 |
|-------|-----------|
| DesktopApplicationPackage | 這個專案以[Windows 應用程式封裝專案](desktop-to-uwp-packaging-dot-net.md)為基礎，它會輸出 MSIX 封裝。 |
| Runtimefix | 這是 c + + Dynamic-Linked 文件庫專案，其中包含一或多個取代函式，做為執行階段修正程式。 |
| PSFLauncher | 這是 c + + 空白專案。 這個專案是以收集套件支援架構的執行階段散發檔案的位置。 它會將輸出的可執行檔。 該可執行檔是第一個啟動方案時執行。 |
| WinFormsDesktopApplication | 這個專案包含傳統型應用程式的原始碼。 |

若要查看完整的範例，其中包含所有這些類型的專案，請參閱[PSFSample](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/samples/PSFSample/)。

讓我們逐步解說建立並設定您的方案中的每個這些專案的步驟。


### <a name="create-a-package-solution"></a>建立的封裝解決方案

如果還沒有一個解決方案，為您的傳統型應用程式，請在 Visual Studio 中建立新的**空白方案**。

![空白方案](images/desktop-to-uwp/blank-solution.png)

您也可能會想要新增您有任何應用程式專案。

### <a name="add-a-packaging-project"></a>新增封裝專案

如果您還沒有**Windows 應用程式封裝專案**，建立租用戶，並將它新增到您的方案。

![封裝專案範本](images/desktop-to-uwp/package-project-template.png)

如需 Windows 應用程式封裝專案的詳細資訊，請參閱[封裝您的應用程式，使用 Visual Studio](desktop-to-uwp-packaging-dot-net.md)。

在 [**方案總管]** 中，封裝專案上按一下滑鼠右鍵，選取 [**編輯**]，然後新增這到底部的專案檔案：

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

### <a name="add-project-for-the-runtime-fix"></a>新增適用於執行階段修正的專案

將 c + +**動態連結程式庫 (DLL)** 專案新增至方案。

![執行階段修正程式庫](images/desktop-to-uwp/runtime-fix-library.png)

以滑鼠右鍵按一下該專案，並選擇 [**屬性**。

在屬性頁中找到的**標準 c + + 語言**的欄位，然後在下拉式清單旁邊該欄位中，選取**ISO c++17 標準 (/ /std: + + 17)** 選項。

![ISO 17 選項](images/desktop-to-uwp/iso-option.png)

該專案中，以滑鼠右鍵按一下，然後在操作功能表中，選擇 [**管理 Nuget 套件**的選項。 請確定 [**封裝來源**選項設定為**所有**或**nuget.org**。

按一下設定圖示接下來該欄位。

搜尋*PSF** Nuget 套件，然後再安裝它這個專案。

![nuget 套件](images/desktop-to-uwp/psf-package.png)

如果您想要偵錯，或將現有的執行階段修正延伸，新增您取得使用本指南中[找到的執行階段修正](#find)所述的指導方針的執行階段修正檔案。

如果您想要建立全新的修正，不要加入任何項目到這個專案只是尚未。 我們將協助您稍後在本指南中的這個專案中加入的正確的檔案。 現在，我們將會繼續設定您的方案。

### <a name="add-a-project-that-starts-the-psf-launcher-executable"></a>新增專案啟動可執行檔 PSF 啟動程式

將 c + +**空白專案**專案新增至方案。

![空白專案](images/desktop-to-uwp/blank-app.png)

將**PSF** Nuget 套件新增到此專案中，使用上一節中所述的相同的指導方針。

開啟專案，並在**一般**設定 \] 頁面中的屬性頁面**目標名稱**將屬性設定為``PSFLauncher32``或``PSFLauncher64``取決於您的應用程式的架構。

![PSF 啟動程式參考](images/desktop-to-uwp/shim-exe-reference.png)

在您的方案中新增執行階段修正專案的專案參考。

![執行階段修正參考](images/desktop-to-uwp/reference-fix.png)

參考，以滑鼠右鍵按一下，然後在 [**屬性**] 視窗中，將套用這些值。

| 屬性 | 值 |
|-------|-----------|
| 複製本機 | True |
| 複製本機衛星組件 | True |
| 參考組件輸出 | True |
| 連結程式庫相依性 | False |
| 連結程式庫相依性輸入 | False |

### <a name="configure-the-packaging-project"></a>在設定封裝專案

在封裝專案的 **\[應用程式\]** 資料夾上按一下滑鼠右鍵，然後選擇 **\[加入參考\]**。

![加入專案參考](images/desktop-to-uwp/add-reference-packaging-project.png)

選擇 PSF 啟動程式專案和您的傳統型應用程式專案，然後選擇 [**確定**] 按鈕。

![桌面專案](images/desktop-to-uwp/package-project-references.png)

>[!NOTE]
> 如果您不需要原始碼到您的應用程式，只要選擇 PSF 啟動程式專案。 我們將示範如何建立設定檔時，參考可執行檔。

在**應用程式**] 節點中，PSF 啟動程式應用程式，以滑鼠右鍵按一下，然後選擇 [**設為進入點**。

![設定進入點](images/desktop-to-uwp/set-startup-project.png)

新增一個名為檔案``config.json``封裝專案，然後複製並貼到的檔案的下列 json 文字。 將**封裝 Action**屬性設定為 [**內容**。

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
            "fixups": [
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
提供每個索引鍵的值。 使用此表格做為指引。

| 陣列 | key | 值 |
|-------|-----------|-------|
| applications | id |  使用的值，`Id`屬性`Application`套件資訊清單中的項目。 |
| applications | 可執行檔 | 您想要啟動的可執行檔套件相對路徑。 在大部分情況下，您可以從您的封裝資訊清單檔案取得此值，才能加以修改。 它是值的`Executable`屬性`Application`項目。 |
| applications | workingDirectory | （選擇性）若要使用做為工作目錄的應用程式的啟動與套件相對的路徑。 如果您未設定此值，作業系統會使用`System32`目錄做為應用程式的工作目錄。 |
| 處理程序 | 可執行檔 | 在大部分情況下，這會是名稱`executable`上述設定已移除的路徑和檔案副檔名。 |
| 修復 | dll | 修復載入的 DLL 套件相對路徑。 |
| 修復 | 設定 | （選擇性）控制修復 DLL 的運作方式。 此值的確切格式而有所不同修復的修復為基礎，為每個修復可解譯此 「 blob 」 想。 |

當您完成後時，您``config.json``檔案看起來會像這樣。

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
      "fixups": [ { "dll": "RuntimeFix.dll" } ]
    }
  ]
}

```

>[!NOTE]
> `applications`， `processes`，以及`fixups`鍵是陣列。 這表示，您可以使用 config.json 檔案來指定多個應用程式、 處理程序，以及修復 DLL。

### <a name="debug-a-runtime-fix"></a>偵錯的執行階段修正

在 Visual Studio 中，按下 F5 來啟動偵錯工具。  啟動的第一件事是 PSF 啟動程式應用程式，接著，會啟動傳統型應用程式目標。  若要偵錯目標的傳統型應用程式，您必須手動附加的傳統型應用程式處理程序選擇 [**偵錯**->**附加至處理序**，然後選取 [應用程式處理程序。 若要允許使用的原生執行階段修正 DLL 的.NET 應用程式的偵錯，選取 （混合的模式偵錯） 的 managed 和原生程式碼類型。  

一旦您已將它設定好，您可以在傳統型應用程式程式碼和執行階段修正專案中設定中斷點旁邊幾行程式碼。 如果您不需要您的應用程式的原始碼，您將無法在執行階段修正專案中設定中斷點，僅限旁幾行程式碼。

>[!NOTE]
> Visual Studio 可讓您在最簡單的開發和偵錯經驗，有一些限制，而稍後在本指南中，我們會討論您可以套用其他偵錯技巧。

## <a name="create-a-runtime-fix"></a>建立執行階段修正程式

如果沒有，但在執行階段修正問題，您想要解決，您可以建立新的執行階段修正撰寫取代函式，並包含任何設定資料，會比較合理。 我們來看看每個部分。

### <a name="replacement-functions"></a>取代函式

首先，找出哪些 MSIX 容器中執行的應用程式時，呼叫會失敗的函式。 然後，您可以建立您會想要改為呼叫，執行階段管理員取代函式。 這會讓您有機會，將函式的實作取代符合現代化的執行階段環境中的規則的行為。

在 Visual Studio 中，開啟您稍早在本指南中建立執行階段修正專案。

宣告``FIXUP_DEFINE_EXPORTS``巨集，然後加入陳述式包含`fixup_framework.h`頂端的每個。您想要將您的執行階段修正程式的函式的位置 CPP 檔案。

```c++
#define FIXUP_DEFINE_EXPORTS
#include <fixup_framework.h>
```
>[!IMPORTANT]
>請確定`FIXUP_DEFINE_EXPORTS`巨集出現之前包含陳述式。

建立具有相同的簽章的函式的函式誰有您想要修改的行為。 以下是範例函式，以取代`MessageBoxW`函式。

```c++
auto MessageBoxWImpl = &::MessageBoxW;
int WINAPI MessageBoxWFixup(
    _In_opt_ HWND hwnd,
    _In_opt_ LPCWSTR,
    _In_opt_ LPCWSTR caption,
    _In_ UINT type)
{
    return MessageBoxWImpl(hwnd, L"SUCCESS: This worked", caption, type);
}

DECLARE_FIXUP(MessageBoxWImpl, MessageBoxWFixup);
```

呼叫`DECLARE_FIXUP`地圖`MessageBoxW`函式，以新的取代函式。 您的應用程式嘗試呼叫`MessageBoxW`函式，它會呼叫取代函式改為。

#### <a name="protect-against-recursive-calls-to-functions-in-runtime-fixes"></a>抵禦遞迴呼叫函式在執行階段的修正程式

您可以選擇性地將套用`reentrancy_guard`您抵禦函式在執行階段的修正程式遞迴呼叫的函式的類型。

例如，您可能會產生的取代函式`CreateFile`函式。 您的實作可能會呼叫`CopyFile`函式，但的實作`CopyFile`函式可能會呼叫`CreateFile`函式。 這可能會導致無限遞迴循環，呼叫的`CreateFile`函式。

如需詳細資訊`reentrancy_guard`請參閱[authoring.md](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/Authoring.md)

### <a name="configuration-data"></a>設定資料

如果您想要將設定資料新增到您的執行階段修正程式，請考慮新增至``config.json``。 如此一來，您可以使用`FixupQueryCurrentDllConfig`輕鬆地分析該資料。 這個範例會剖析該設定檔的字串和布林值。

```c++
if (auto configRoot = ::FixupQueryCurrentDllConfig())
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

## <a name="other-debugging-techniques"></a>其他偵錯技巧

當 Visual Studio 可讓您在最簡單的開發和偵錯經驗時，有一些限制。

首先，F5 偵錯執行應用程式的部署的套件配置資料夾路徑的鬆散檔案，而不是從.msix 安裝 /.appx 套件。  配置資料夾通常沒有相同的安全性限制為已安裝的套件資料夾。 如此一來，它可能無法重現之前套用的執行階段修正套件路徑存取拒絕錯誤。

若要解決此問題，請使用.msix /.appx 套件部署，而不是 F5 鬆散檔案部署。  若要建立.msix /.appx 套件檔案中，使用 Windows SDK 中，從[MakeMSIX](https://docs.microsoft.com/en-us/windows/desktop/appxpkg/make-appx-package--makeappx-exe-)公用程式，如上文所述。 或者，從 Visual Studio 中，您的應用程式的專案節點上按一下滑鼠右鍵，然後選取**存放區**->**建立應用程式套件**。

使用 Visual Studio 的另一個問題是它不需要附加至任何子處理程序啟動偵錯工具的內建支援。   這會讓您難以偵錯目標應用程式，必須以手動方式連結 Visual studio 啟動後將啟動路徑中的邏輯。

若要解決此問題，使用支援子處理程序在偵錯工具附加。  請注意，它通常不可能只是時間 (JIT) 偵錯工具附加到目標應用程式。  這是因為大部分的 JIT 技術涉及啟動偵錯工具來取代目標應用程式，透過 ImageFileExecutionOptions 登錄機碼。  這會導致 PSFLauncher.exe 用於 FixupRuntime.dll 插入目標應用程式的 detouring 機制變成無效。  WinDbg，包含在 「[適用於 Windows 偵錯工具](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/index)中，並從[Windows SDK](https://developer.microsoft.com/en-US/windows/downloads/windows-10-sdk)中，取得附加支援子處理程序。  它現在也支援直接[啟動和偵錯 UWP 應用程式](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/debugging-a-uwp-app-using-windbg#span-idlaunchinganddebuggingauwpappspanspan-idlaunchinganddebuggingauwpappspanspan-idlaunchinganddebuggingauwpappspanlaunching-and-debugging-a-uwp-app)。

若要偵錯目標為子處理程序的應用程式啟動時，開始``WinDbg``。

```
windbg.exe -plmPackage PSFSampleWithFixup_1.0.59.0_x86__7s220nvg1hg3m -plmApp PSFSample
```

在``WinDbg``提示、 啟用偵錯的子系，並設定適當的中斷點。

```
.childdbg 1
g
```
（執行，直到目標應用程式啟動和偵錯工具會中斷）

```
sxe ld fixup.dll
g
```
（執行的修復 DLL 載入之前）

```
bp ...
```

>[!NOTE]
> [PLMDebug](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/plmdebug)也可用來偵錯工具附加到應用程式在啟動時，也會包含在[適用於 Windows 偵錯工具](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/index)。  不過，它是比現在 WinDbg 所提供的直接支援使用更複雜。

## <a name="support-and-feedback"></a>支援與意見反應

**尋找您的問題解答**

有任何問題嗎？ 請在 Stack Overflow 上發問。 我們的團隊會監視這些[標記](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge)。 您也可以[在此處](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D)詢問我們。

