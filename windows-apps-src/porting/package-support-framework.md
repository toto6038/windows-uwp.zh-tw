---
Description: 修正防止桌面應用程式在 MSIX 容器中執行的問題
Search.Product: eADQiWindows 10XVcnh
title: 修正防止桌面應用程式在 MSIX 容器中執行的問題
ms.date: 07/02/2018
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 80f9c8bad9445bd9cfef9b09c00f99929fda37aa
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57590723"
---
# <a name="apply-runtime-fixes-to-an-msix-package-by-using-the-package-support-framework"></a>套用至 MSIX 套件的執行階段的修正程式，使用封裝支援架構

封裝支援架構是開放原始碼套件，可協助您套用修正現有的 win32 應用程式時您沒有存取權的原始程式碼，使其可以 MSIX 容器中執行。 封裝支援架構可協助您遵循的最佳做法的最新的執行階段環境的應用程式。

若要進一步了解，請參閱[封裝支援架構](https://docs.microsoft.com/windows/msix/package-support-framework-overview)。

本指南將協助您找出應用程式相容性問題，並尋找、 套用及擴充執行階段来修正可解決它們。

<a id="identify" />

## <a name="identify-packaged-application-compatibility-issues"></a>識別封裝的應用程式相容性問題

首先，建立您的應用程式的套件。 然後，將它安裝、 執行它，並觀察其行為。 您可能會收到錯誤訊息，可協助您找出相容性問題。 您也可以使用[處理序監視](https://docs.microsoft.com/en-us/sysinternals/downloads/procmon)找出問題。  常見的問題與相關的工作目錄和程式路徑權限的應用程式假設。

### <a name="using-process-monitor-to-identify-an-issue"></a>使用處理序監視，找出問題

[處理序監視](https://docs.microsoft.com/en-us/sysinternals/downloads/procmon)是功能強大的公用程式，來觀察應用程式的檔案和登錄作業，而其結果。  這可協助您了解應用程式相容性問題。  在開啟處理序監視後新增篩選條件 (篩選器 > 篩選...) 以包含只從應用程式可執行檔的事件。

![ProcMon 應用程式篩選器](images/desktop-to-uwp/procmon_app_filter.png)

事件的清單會出現。 這些事件，這個字的許多**成功**會出現在**結果**資料行。

![ProcMon 事件](images/desktop-to-uwp/procmon_events.png)

（選擇性） 您可以篩選成只顯示只有失敗的事件。

![ProcMon 排除成功](images/desktop-to-uwp/procmon_exclude_success.png)

如果您懷疑檔案系統存取失敗時，搜尋 System32/SysWOW64 或封裝檔案路徑下的失敗事件。 篩選可以也派上用場。 在這份清單底部開始，向上捲動。 最近發生失敗，出現在此清單的底部。 大部分注意錯誤包含字串，例如 「 拒絕存取 」 和 「 路徑/名稱找不到 」，並且忽略看起來不太可疑的項目。 [PSFSample](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/samples/PSFSample/)有兩個問題。 您可以看到在下圖中顯示的清單中的這些問題。

![ProcMon Config.txt](images/desktop-to-uwp/procmon_config_txt.png)

中會出現在此映像的第一個問題，應用程式無法讀取"C:\Windows\SysWOW64"路徑中的"Config.txt 」 檔案。 不可能的應用程式，嘗試直接參考該路徑。 最有可能嘗試使用相對路徑，從該檔案讀取，並根據預設，「 System32/SysWOW64"是應用程式的工作目錄。 這可能表示應用程式所預期其目前的工作目錄設為某個位置中的封裝。 尋找應用程式內，我們可以看到該檔案位於與可執行檔相同的目錄。

![應用程式 Config.txt](images/desktop-to-uwp/psfsampleapp_config_txt.png)

第二個問題會在下圖中顯示。

![ProcMon 記錄檔](images/desktop-to-uwp/procmon_logfile.png)

在本期中，應用程式無法寫入.log 檔案到其封裝路徑。 這會建議可能有助於檔案重新導向修復。

<a id="find" />

## <a name="find-a-runtime-fix"></a>尋找執行階段修正程式

PSF 包含您可以使用，例如檔案重新導向修復的執行階段修正。

### <a name="file-redirection-fixup"></a>檔案重新導向修復

您可以使用[檔案重新導向修復](https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/master/fixups/FileRedirectionFixup)重新導向嘗試寫入或讀取在無法存取從 MSIX 容器中執行的應用程式的目錄中的資料。

例如，如果您的應用程式寫入至記錄檔中與您的應用程式可執行檔相同的目錄，然後您可以使用[檔案重新導向修復](https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/master/fixups/FileRedirectionFixup)另一個位置，例如本機應用程式資料存放區中建立該記錄檔。

### <a name="runtime-fixes-from-the-community"></a>來自社群的執行階段修正

請務必先檢閱社群參與我們[GitHub](https://github.com/Microsoft/MSIX-PackageSupportFramework)頁面。 可以其他開發人員解決類似問題，並且有共用的執行階段修正。

## <a name="apply-a-runtime-fix"></a>套用執行階段修正

從 Windows SDK，並依照下列步驟，您可以套用現有的執行階段修正的幾個簡單的工具。

> [!div class="checklist"]
> * 建立封裝版面配置資料夾
> * 取得封裝的支援架構檔案
> * 將它們新增至您的套件
> * 修改套件資訊清單
> * 建立設定檔

讓我們瀏覽每個工作。

### <a name="create-the-package-layout-folder"></a>建立封裝配置資料夾

如果您已經有.msix （則為.appx） 檔案，您可以將其內容解壓縮到到版面配置資料夾將做為暫存區域，為您的封裝。 您可以從命令提示字元使用 MakeAppx 工具，根據您的安裝路徑的 sdk，這是您將在其中找到您的 Windows 10 電腦上 makeappx.exe 工具： x86:C:\Program Files (x86)\Windows Kits\10\bin\x86\makeappx.exe x64:C:\Program Files (x86)\Windows Kits\10\bin\x64\makeappx.exe

```ps
makeappx unpack /p PSFSamplePackage_1.0.60.0_AnyCPU_Debug.msix /d PackageContents

```

這可讓您看起來如下的項目。

![套件配置](images/desktop-to-uwp/package_contents.png)

如果您沒有.msix （則為.appx） 檔案開始，您可以從頭建立的封裝資料夾和檔案。

### <a name="get-the-package-support-framework-files"></a>取得封裝的支援架構檔案

使用獨立的 Nuget 命令列工具或透過 Visual Studio，您可以取得 PSF Nuget 套件。

#### <a name="get-the-package-by-using-the-command-line-tool"></a>使用命令列工具取得套件

安裝 Nuget 命令列工具，從這個位置： https://www.nuget.org/downloads。 然後從 Nuget 命令列中，執行下列命令：

```ps
nuget install Microsoft.PackageSupportFramework
```

#### <a name="get-the-package-by-using-visual-studio"></a>使用 Visual Studio 取得套件

在 Visual Studio 中，以滑鼠右鍵按一下您的方案或專案節點，並挑選其中一個 [管理 Nuget 套件] 命令。  搜尋**Microsoft.PackageSupportFramework**或是**PSF** Nuget.org 上尋找套件。然後，安裝它。

### <a name="add-the-package-support-framework-files-to-your-package"></a>將封裝的支援架構檔案新增至您的套件

將所需的 32 位元和 64 位元 PSF Dll 和可執行檔新增至套件目錄中。 使用下表做為指引。 您也會想要包含您所需要的任何執行階段修正。 在本例中，我們需要的檔案重新導向執行階段修正。

| 應用程式可執行檔是 x64 | 應用程式可執行檔為 x86 |
|-------------------------------|-----------|
| [PSFLauncher64.exe](https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/master/PsfLauncher/readme.md) |  [PSFLauncher32.exe](https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/master/PsfLauncher/readme.md) |
| [PSFRuntime64.dll](https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/master/PsfRuntime/readme.md) | [PSFRuntime32.dll](https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/master/PsfRuntime/readme.md) |
| [PSFRunDll64.exe](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/PsfRunDll/readme.md) | [PSFRunDll32.exe](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/PsfRunDll/readme.md) |

您套件的內容現在應該看起來像這樣。

![套件二進位檔](images/desktop-to-uwp/package_binaries.png)

### <a name="modify-the-package-manifest"></a>修改套件資訊清單

在文字編輯器中，開啟您的封裝資訊清單，然後設定`Executable`屬性的`Application`PSF 啟動器可執行檔名稱的項目。  如果您知道目標應用程式的架構，請選取適當的版本，PSFLauncher32.exe 或 PSFLauncher64.exe。  如果沒有，PSFLauncher32.exe 將在所有情況下運作。  這裡提供一個範例。

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

建立檔案名稱``config.json``，並將該檔案儲存到您的套件的根資料夾。 修改的 config.json 檔案宣告的應用程式識別碼，以指向您剛剛取代的可執行檔。 使用 從使用處理序監視所獲得的知識，您可以也設定工作目錄，以及使用重新導向至封裝相對"PSFSampleApp 」 目錄下的所有.log 檔案的讀取/寫入的檔案重新導向修復。

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

| 陣列 | 索引鍵 | 值 |
|-------|-----------|-------|
| applications | id |  使用值`Id`屬性的`Application`封裝資訊清單中的項目。 |
| applications | 可執行檔 | 您想要啟動之可執行檔封裝相對路徑。 在大部分情況下，您可以從您的套件資訊清單檔案取得此值，再進行修改。 值很`Executable`屬性的`Application`項目。 |
| applications | workingDirectory | （選擇性）若要啟動之應用程式的工作目錄為使用封裝相對路徑。 如果您未設定此值，就會使用作業系統`System32`為應用程式的工作目錄的目錄。 |
| 處理程序 | 可執行檔 | 在大部分情況下，這會是名稱`executable`上方移除的路徑和檔案延伸模組設定。 |
| fixups | dll | 修復，.msix/.appx 載入的封裝相對路徑。 |
| fixups | 設定 | （選擇性）控制修復 dl 的運作方式。 為每個修復可解譯此 「 blob 」 它想要修復的修復為基礎而異的確切的格式，這個值。 |

`applications`， `processes`，和`fixups`索引鍵是陣列。 這表示您可以指定多個應用程式、 處理程序，以及修復 DLL 使用的 config.json 檔案。

### <a name="package-and-test-the-app"></a>套件和測試應用程式

接下來，建立的封裝。

```ps
makeappx pack /d PackageContents /p PSFSamplePackageFixup.msix
```

然後，登入。

```ps
signtool sign /a /v /fd sha256 /f ExportedSigningCertificate.pfx PSFSamplePackageFixup.msix
```

如需詳細資訊，請參閱 <<c0> [ 如何建立套件簽署憑證](https://docs.microsoft.com/en-us/windows/desktop/appxpkg/how-to-create-a-package-signing-certificate)和[如何簽署封裝，使用 signtool](https://docs.microsoft.com/en-us/windows/desktop/appxpkg/how-to-sign-a-package-using-signtool)

使用 PowerShell，請安裝套件。

>[!NOTE]
> 請務必先解除安裝封裝。

```ps
powershell Add-AppPackage .\PSFSamplePackageFixup.msix
```

執行應用程式，並觀察行為的執行階段套用的修正。  重複的診斷和封裝為所需的步驟。

### <a name="use-the-trace-fixup"></a>使用追蹤修復

診斷封裝的應用程式相容性問題的替代方法是使用追蹤修復。 此 DLL 隨附 PSF，並提供應用程式的行為，類似於處理序監視的詳細診斷檢視。  它是特別設計來顯示應用程式相容性問題。  若要使用追蹤修復，將 DLL 加入封裝，將下列片段新增至您的 config.json，接著，封裝和安裝應用程式。

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

根據預設，追蹤修復會篩選出可能會被視為 「 預期 」 的失敗。  例如，應用程式可能會嘗試無條件地刪除檔案，而不檢查以查看它是否已經存在，正在略過的結果。 這會有一些非預期的失敗可能會取得篩選掉，不幸結果，因此在上述範例中，我們選擇要收到從檔案系統函式的所有失敗。 由於我們知道從之前，嘗試從 Config.txt 檔案讀取失敗訊息 「 找不到檔案 」，我們可以這麼做。 這是經常觀察到，和通常不會假設為非預期的失敗。 實際上它是可能的最佳開始篩選只能以非預期的失敗，並再回到所有失敗如果仍然無法識別發生問題。

根據預設，追蹤修復的輸出傳送至附加偵錯工具。 針對此範例中，我們不是要附加偵錯工具，和將改為使用[DebugView](https://docs.microsoft.com/en-us/sysinternals/downloads/debugview)即可檢視其輸出的 sysinternals 程式。 執行應用程式之後, 我們可以看到相同的失敗和以前一樣，這會向我們指出相同的執行階段修正。

![找不到 TraceShim 檔案](images/desktop-to-uwp/traceshim_filenotfound.png)

![TraceShim 存取被拒](images/desktop-to-uwp/traceshim_accessdenied.png)

## <a name="debug-extend-or-create-a-runtime-fix"></a>偵錯、 延伸或建立的執行階段的修正

您可以使用 Visual Studio 來偵錯執行階段的修正程式、 擴充執行階段的修正程式，或從從頭開始建立一個。 您必須執行這些動作，才能成功。

> [!div class="checklist"]
> * 新增封裝專案
> * 新增執行階段的修正程式的專案
> * 加入啟動 PSF 啟動器可執行檔的專案
> * 設定封裝專案

當您完成時，您的解決方案會看起來像這樣。

![已完成的方案](images/desktop-to-uwp/runtime-fix-project-structure.png)

讓我們看看在本例中的每個專案。

| Project | 用途 |
|-------|-----------|
| DesktopApplicationPackage | 此專案根據[Windows 應用程式封裝專案](desktop-to-uwp-packaging-dot-net.md)，它會輸出 MSIX 封裝。 |
| Runtimefix | 這是 c + + Dynamic-Linked 程式庫的專案，其中包含一或多個取代函式，做為執行階段修正。 |
| PSFLauncher | 這是 c + + 空白專案。 此專案是以收集封裝的支援架構的執行階段可轉散發檔案的位置。 它會輸出可執行檔。 該可執行檔是您啟動方案時執行第一件事。 |
| WinFormsDesktopApplication | 此專案包含的桌面應用程式的原始程式碼。 |

若要查看完整的範例，其中包含所有這些類型的專案，請參閱[PSFSample](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/samples/PSFSample/)。

讓我們逐步解說的步驟來建立及設定這類專案分別在您的解決方案。

### <a name="create-a-package-solution"></a>建立封裝的解決方案

如果您還沒有解決方案桌面應用程式，建立新**空白方案**Visual Studio 中。

![空白方案](images/desktop-to-uwp/blank-solution.png)

您也可以新增任何您擁有的應用程式專案。

### <a name="add-a-packaging-project"></a>新增封裝專案

如果您還沒有**Windows 應用程式封裝專案**，建立一個，並將它新增至您的方案。

![Package 專案範本](images/desktop-to-uwp/package-project-template.png)

如需有關 Windows 應用程式封裝專案的詳細資訊，請參閱 <<c0> [ 使用 Visual Studio 封裝您的應用程式](desktop-to-uwp-packaging-dot-net.md)。

在 [**方案總管] 中**，以滑鼠右鍵按一下封裝專案，然後選取**編輯**，並將其加入至專案檔的底部：

```xml
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

### <a name="add-project-for-the-runtime-fix"></a>新增執行階段的修正程式的專案

新增 c + +**動態連結程式庫 (DLL)** 專案加入方案。

![執行階段的修正程式庫](images/desktop-to-uwp/runtime-fix-library.png)

以滑鼠右鍵按一下，專案，然後選擇**屬性**。

在 屬性頁中，尋找**c + + 語言標準**欄位，，然後在該欄位旁邊下拉式清單中，選取**ISO c++17 標準 (/ /std: c + + 17)** 選項。

![ISO 17 選項](images/desktop-to-uwp/iso-option.png)

該專案中，以滑鼠右鍵按一下，然後在內容功能表中，選擇**管理 Nuget 套件**選項。 請確認**套件來源**選項設定為**所有**或是**nuget.org**。

按一下設定圖示下一步該欄位。

搜尋*PSF** Nuget 套件，然後再對這個專案進行安裝。

![nuget 套件](images/desktop-to-uwp/psf-package.png)

如果您想要偵錯，或擴充現有的執行階段修正程式，新增執行階段修正檔案，您取得使用中所述的指導方針[尋找執行階段修正](#find)一節。

如果您想要建立全新的修正程式，不要將任何項目加入這個專案還。 我們將協助您稍後在本指南的這個專案中加入正確的檔案。 現在，我們會繼續設定您的解決方案。

### <a name="add-a-project-that-starts-the-psf-launcher-executable"></a>加入啟動 PSF 啟動器可執行檔的專案

新增 c + +**空專案**專案加入方案。

![空專案](images/desktop-to-uwp/blank-app.png)

新增**PSF**使用上一節中所述的相同指導方針的這個專案的 Nuget 套件。

開啟專案，然後在屬性頁**一般**設定頁面上，設定**目標名稱**屬性設``PSFLauncher32``或``PSFLauncher64``大小取決於您的應用程式架構。

![PSF 啟動器參考](images/desktop-to-uwp/shim-exe-reference.png)

加入方案中的執行階段修正專案的專案參考。

![執行階段修正參考](images/desktop-to-uwp/reference-fix.png)

以滑鼠右鍵按一下參考，然後在**屬性** 視窗中，套用這些值。

| 屬性 | 值 |
|-------|-----------|
| 複製本機 | True |
| 複製本機附屬組件 | True |
| 參考組件輸出 | True |
| 連結程式庫相依性 | False |
| 連結程式庫相依性輸入 | False |

### <a name="configure-the-packaging-project"></a>設定封裝專案

在封裝專案的 **\[應用程式\]** 資料夾上按一下滑鼠右鍵，然後選擇 **\[加入參考\]**。

![加入專案參考](images/desktop-to-uwp/add-reference-packaging-project.png)

選擇 PSF 啟動器專案和桌面應用程式專案，然後選擇**確定** 按鈕。

![桌面專案](images/desktop-to-uwp/package-project-references.png)

>[!NOTE]
> 如果您沒有原始碼應用程式，只要選擇 PSF 啟動器專案。 我們將示範如何建立組態檔時，參考可執行檔。

在 **應用程式**節點，PSF 啟動器應用程式，以滑鼠右鍵按一下，然後選擇**設為進入點**。

![設定進入點](images/desktop-to-uwp/set-startup-project.png)

新增名為``config.json``至封裝專案，然後複製並貼入檔案中的下列 json 文字。 設定**封裝動作**屬性設**內容**。

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

提供每個索引鍵的值。 使用下表作為指南。

| 陣列 | 索引鍵 | 值 |
|-------|-----------|-------|
| applications | id |  使用值`Id`屬性的`Application`封裝資訊清單中的項目。 |
| applications | 可執行檔 | 您想要啟動之可執行檔封裝相對路徑。 在大部分情況下，您可以從您的套件資訊清單檔案取得此值，再進行修改。 值很`Executable`屬性的`Application`項目。 |
| applications | workingDirectory | （選擇性）若要啟動之應用程式的工作目錄為使用封裝相對路徑。 如果您未設定此值，就會使用作業系統`System32`為應用程式的工作目錄的目錄。 |
| 處理程序 | 可執行檔 | 在大部分情況下，這會是名稱`executable`上方移除的路徑和檔案延伸模組設定。 |
| fixups | dll | 修復 DLL 載入的封裝相對路徑。 |
| fixups | 設定 | （選擇性）控制修復 DLL 的運作方式。 為每個修復可解譯此 「 blob 」 它想要修復的修復為基礎而異的確切的格式，這個值。 |

當您完成時，您``config.json``檔案會看起來像這樣。

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
> `applications`， `processes`，和`fixups`索引鍵是陣列。 這表示您可以指定多個應用程式、 處理程序，以及修復 DLL 使用的 config.json 檔案。

### <a name="debug-a-runtime-fix"></a>偵錯執行階段的修正程式

在 Visual Studio 中，按下 f5 鍵啟動偵錯工具。  啟動第一件事是 PSF 啟動器應用程式，接著，啟動您目標的桌面應用程式。  若要偵錯目標的傳統型應用程式，您必須以手動方式選擇附加至桌面應用程式處理序**偵錯**->**附加至處理序**，然後選取 應用程式程序。 若要允許.NET 應用程式的原生執行階段修正 DLL 的偵錯，選取 （混合的模式偵錯） 的 managed 和原生程式碼類型。  

一旦您已設定此功能，您可以設定中斷點的程式碼行旁邊的桌面應用程式程式碼和執行階段修正專案中。 如果您沒有原始碼應用程式，您可以設定只旁邊幾行程式碼的中斷點，在執行階段修正專案中。

>[!NOTE]
> 雖然 Visual Studio 提供您最簡單的開發和偵錯體驗，有一些限制，因此稍後在本指南中，我們將討論您可以套用其他偵錯技術。

## <a name="create-a-runtime-fix"></a>建立執行階段的修正程式

如果您想要解決，您可以建立新的執行階段修正方法是撰寫取代函式，並包括任何組態資料，執行階段修正問題還沒有意義。 讓我們看看每個組件。

### <a name="replacement-functions"></a>取代函式

首先，識別哪些函式呼叫失敗時 MSIX 容器中執行的應用程式。 然後，您可以建立您想要改為呼叫的執行階段管理員的取代函式。 這會讓您有機會來取代符合規則的最新的執行階段環境的行為的函式的實作。

在 Visual Studio 中，開啟您稍早在本指南中建立的執行階段修正專案。

宣告``FIXUP_DEFINE_EXPORTS``巨集，然後新增 include 陳述式，如`fixup_framework.h`頂端的每個。您想要新增的執行階段修正函式的 CPP 檔案。

```c++
#define FIXUP_DEFINE_EXPORTS
#include <fixup_framework.h>
```

>[!IMPORTANT]
>請確定`FIXUP_DEFINE_EXPORTS`巨集出現在 include 陳述式之前。

建立具有相同的簽章的函式的函式誰具有您想要修改的行為。 以下是範例函式，以取代`MessageBoxW`函式。

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

若要在呼叫`DECLARE_FIXUP`對應`MessageBoxW`至新的取代函式的函式。 當您的應用程式嘗試呼叫`MessageBoxW`函式，它會取代函式改為呼叫。

#### <a name="protect-against-recursive-calls-to-functions-in-runtime-fixes"></a>防止在執行階段的修正程式中的函式的遞迴呼叫

您可以選擇性地套用`reentrancy_guard`您防止執行階段的修正程式中的函式的遞迴呼叫的函式的類型。

例如，您可能會產生函式取代`CreateFile`函式。 您的實作可能會呼叫`CopyFile`函式，但實作`CopyFile`函式可能會呼叫`CreateFile`函式。 這可能會導致無限遞迴循環，若要呼叫的`CreateFile`函式。

如需詳細資訊`reentrancy_guard`請參閱[authoring.md](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/Authoring.md)

### <a name="configuration-data"></a>設定資料

如果您想要將組態資料新增至您的執行階段修正，請考慮將它新增至``config.json``。 如此一來，您可以使用`FixupQueryCurrentDllConfig`輕易地剖析該資料。 此範例會剖析該設定檔的 boolean 和 string 值。

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

## <a name="other-debugging-techniques"></a>其他偵錯技術

雖然 Visual Studio 可提供您最簡單的開發和偵錯體驗，但有一些限制。

第一次，F5 偵錯執行應用程式部署封裝版面配置資料夾路徑，從鬆散式檔案，而不是從.msix 安裝 /.appx 套件。  配置資料夾通常並沒有相同的安全性限制為已安裝的封裝資料夾。 如此一來，它可能無法重現封裝路徑存取拒絕錯誤之前套用執行階段修正。

若要解決此問題，請使用.msix /.appx 封裝部署，而不是 F5 鬆散檔案部署。  若要建立.msix /.appx 套件檔案，使用[MakeAppx](https://docs.microsoft.com/en-us/windows/desktop/appxpkg/make-appx-package--makeappx-exe-)公用程式從 Windows SDK，如上面所述。 或者，從在 Visual Studio 中，以滑鼠右鍵按一下您的應用程式專案節點，然後選取**存放區**->**建立應用程式套件**。

使用 Visual Studio 的另一個問題是，它並沒有內建支援附加至任何啟動偵錯工具的子處理序。   這可讓您難以偵錯的目標應用程式，必須以手動方式附加 Visual studio 啟動之後的啟動路徑中的邏輯。

若要解決此問題，請使用偵錯工具的支援子處理序連結。  請注意，它通常不可能在 just-in-time (JIT) 偵錯工具附加至目標應用程式。  這是因為大部分的 JIT 技術牽涉到啟動偵錯工具來取代目標應用程式，透過 ImageFileExecutionOptions 登錄機碼。  這就失去了用來插入目標應用程式的 FixupRuntime.dll PSFLauncher.exe detouring 機制。  WinDbg，納入[的 Windows 偵錯工具](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/index)，並從取得[Windows SDK](https://developer.microsoft.com/en-US/windows/downloads/windows-10-sdk)，支援子處理序連結。  它現在也支援直接[啟動並偵錯的 UWP 應用程式](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/debugging-a-uwp-app-using-windbg#span-idlaunchinganddebuggingauwpappspanspan-idlaunchinganddebuggingauwpappspanspan-idlaunchinganddebuggingauwpappspanlaunching-and-debugging-a-uwp-app)。

若要偵錯子處理序為目標的應用程式啟動時，啟動``WinDbg``。

```ps
windbg.exe -plmPackage PSFSampleWithFixup_1.0.59.0_x86__7s220nvg1hg3m -plmApp PSFSample
```

在``WinDbg``提示、 啟用偵錯的子系，並設定適當的中斷點。

```ps
.childdbg 1
g
```

（執行，直到目標應用程式開始，並中斷偵錯工具）

```ps
sxe ld fixup.dll
g
```

（執行之前載入 DLL 的修復）

```ps
bp ...
```

>[!NOTE]
> [PLMDebug](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/plmdebug)也可用來將偵錯工具附加至應用程式啟動，且也會納入[的 Windows 偵錯工具](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/index)。  不過，它是比現在提供 WinDbg 的直接支援更複雜。

## <a name="support-and-feedback"></a>支援與意見反應

**尋找問題的解答**

有任何問題嗎？ 請在 Stack Overflow 上發問。 我們的團隊會監視這些[標記](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge)。 您也可以[在此處](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D)詢問我們。
