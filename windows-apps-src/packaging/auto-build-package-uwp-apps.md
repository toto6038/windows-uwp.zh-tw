---
title: 設定您的 UWP 應用程式的自動化組建
description: 如何設定您的自動化組建以產生側載和/或市集套件。
ms.date: 07/17/2019
ms.topic: article
keywords: windows 10, uwp
ms.assetid: f9b0d6bd-af12-4237-bc66-0c218859d2fd
ms.localizationpriority: medium
ms.openlocfilehash: 7f0e1d460ca52c659401bbb291deafa5746b7bb6
ms.sourcegitcommit: 53c00939b20d4b0a294936df3d395adb0c13e231
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/09/2020
ms.locfileid: "91933109"
---
# <a name="set-up-automated-builds-for-your-uwp-app"></a>設定您的 UWP 應用程式的自動化組建

您可以使用 Azure Pipelines 來建立 UWP 專案的自動化組建。 在此文章中，我們會說明執行此操作的不同方式。 我們也會說明如何透過使用命令列來執行這些工作，這樣您就可以與任何其他建置系統整合。

## <a name="create-a-new-azure-pipeline"></a>建立新的 Azure 管線

先從[註冊 Azure Pipelines](/azure/devops/pipelines/get-started/pipelines-sign-up) 開始 (如果尚未這樣做)。

接下來，建立一條您可以用來建立原始程式碼的管線。 如需建置管線以建置 GitHub 存放庫的教學課程，請參閱[建立您的第一條管線](/azure/devops/pipelines/get-started-yaml)。 Azure Pipelines 支援[這篇文章](/azure/devops/pipelines/repos)中列出的存放庫類型。

## <a name="set-up-an-automated-build"></a>設定自動化組建

我們將從 Azure Dev Ops 中可用的預設 UWP 組建定義開始，然後向您示範如何設定管線。

在組建定義範本清單中，選擇 [通用 Windows 平台]  範本。

![選取 UWP 範本](images/select-yaml-template.png)

此範本包括用於建置 UWP 專案的基本設定：

```yml
trigger:
- master

pool:
  vmImage: 'windows-latest'

variables:
  solution: '**/*.sln'
  buildPlatform: 'x86|x64|ARM'
  buildConfiguration: 'Release'
  appxPackageDir: '$(build.artifactStagingDirectory)\AppxPackages\\'

steps:
- task: NuGetToolInstaller@0

- task: NuGetCommand@2
  inputs:
    restoreSolution: '$(solution)'

- task: VSBuild@1
  inputs:
    platform: 'x86'
    solution: '$(solution)'
    configuration: '$(buildConfiguration)'
    msbuildArgs: '/p:AppxBundlePlatforms="$(buildPlatform)" /p:AppxPackageDir="$(appxPackageDir)" /p:AppxBundle=Always /p:UapAppxPackageBuildMode=StoreUpload'

```

預設範本會嘗試使用 .csproj 檔案中指定的憑證來簽署套件。 如果您想要在建置期間簽署套件，您必須具有私密金鑰的存取權。 否則，您可以透過將參數 `/p:AppxPackageSigningEnabled=false` 新增至 YAML 檔案中的 `msbuildArgs` 區段來停用簽署。

## <a name="add-your-project-certificate-to-the-secure-files-library"></a>將您的專案憑證新增至安全檔案程式庫

您應該儘可能避免將憑證提交至存放庫，而且 git 依預設則會忽略。 為了管理敏感檔案 (例如憑證) 的安全處理，Azure DevOps 支援[安全檔案](/azure/devops/pipelines/library/secure-files?view=azure-devops)功能。

若要上傳憑證進行自動化建置：

1. 在 Azure Pipelines 中，展開瀏覽窗格中的 [管線]  ，然後按一下 [程式庫]  。
2. 按一下 [安全檔案]  索引標籤，然後按一下 [+ 安全檔案]  。

    ![Azure 的螢幕擷取畫面，其中醒目提示 [程式庫] 選項，顯示 [安全檔案] 頁面。](images/secure-file1.png)

3. 瀏覽至憑證檔案，然後按一下 [確定]  。
4. 在您上傳憑證之後，請選取其來檢視其屬性。 在 [管線權限]  下，啟用 [授權在所有管線中使用]  的切換。

    ![[管線權限] 區段的螢幕擷取畫面，其中已選取 [授權在所有管線中使用] 選項。](images/secure-file2.png)

5. 如果憑證中的私密金鑰有密碼，建議您將密碼儲存在 [Azure Key Vault](/azure/key-vault/about-keys-secrets-and-certificates)，然後將密碼連結到[變數群組](/azure/devops/pipelines/library/variable-groups)。 您可以使用變數來存取管線中的密碼。 請注意，只有私密金鑰才支援密碼；目前不支援使用本身受到密碼保護的憑證檔案。

> [!NOTE]
> 從 Visual Studio 2019 開始，UWP 專案中不會再產生暫時憑證。 若要建立或匯出憑證，請使用[這篇文章](/windows/msix/package/create-certificate-package-signing)所述的 PowerShell Cmdlet。

## <a name="configure-the-build-solution-build-task"></a>設定建置方案建置工作

這個工作會將任何工作資料夾中的方案編譯為二進位檔，並產生輸出應用程式套件檔案。 這個工作會使用 MSBuild 引數。 您必須指定那些引數的值。 使用下表做為指引。

|**MSBuild 引數**|**值**|**描述**|
|--------------------|---------|---------------|
| AppxPackageDir | $(Build.ArtifactStagingDirectory)\AppxPackages | 定義要儲存所產生構件的資料夾。 |
| AppxBundlePlatforms | $(Build.BuildPlatform) | 可讓您定義套件組合中要包含的平台。 |
| AppxBundle | 永遠 | 針對指定的平台使用 .msix/.appx 檔案建立 .msixbundle/.appxbundle。 |
| UapAppxPackageBuildMode | StoreUpload | 產生 .msixupload/.appxupload 檔案和 **_Test** 資料夾進行側載。 |
| UapAppxPackageBuildMode | CI | 只會產生 .msixupload/.appxupload 檔案。 |
| UapAppxPackageBuildMode | SideloadOnly | 僅針對側載產生 **_Test** 資料夾。 |
| AppxPackageSigningEnabled | true | 啟用套件簽署。 |
| PackageCertificateThumbprint | 憑證指紋 | 此值**必須**符合簽署憑證中的指紋，或為空字串。 |
| PackageCertificateKeyFile | 路徑 | 要使用的憑證路徑。 這是從安全檔案中繼資料中擷取的。 |
| PackageCertificatePassword | 密碼 | 憑證中私密金鑰的密碼。 建議您將密碼儲存在 [Azure Key Vault](/azure/key-vault/about-keys-secrets-and-certificates)，並將密碼連結到[變數群組](/azure/devops/pipelines/library/variable-groups)。 您可以將變數傳遞給這個引數。 |

### <a name="configure-the-build"></a>設定組建

如果您想要使用命令列，或使用任何其他建置系統建置您的方案，請使用這些引數執行 MSBuild。

```powershell
/p:AppxPackageDir="$(Build.ArtifactStagingDirectory)\AppxPackages\\"
/p:UapAppxPackageBuildMode=StoreUpload
/p:AppxBundlePlatforms="$(Build.BuildPlatform)"
/p:AppxBundle=Always
```

### <a name="configure-package-signing"></a>設定套件簽署

若要簽署 MSIX (或 .appx) 套件，管線必須擷取簽署憑證。 若要這樣做，請在 VSBuild 工作之前新增 DownloadSecureFile 工作。
這可讓您透過 ```signingCert``` 存取簽署憑證。

```yml
- task: DownloadSecureFile@1
  name: signingCert
  displayName: 'Download CA certificate'
  inputs:
    secureFile: '[Your_Pfx].pfx'
```

接下來，更新 VSBuild 工作以參考簽署憑證：

```yml
- task: VSBuild@1
  inputs:
    platform: 'x86'
    solution: '$(solution)'
    configuration: '$(buildConfiguration)'
    msbuildArgs: '/p:AppxBundlePlatforms="$(buildPlatform)" 
                  /p:AppxPackageDir="$(appxPackageDir)" 
                  /p:AppxBundle=Always 
                  /p:UapAppxPackageBuildMode=StoreUpload 
                  /p:AppxPackageSigningEnabled=true
                  /p:PackageCertificateThumbprint="" 
                  /p:PackageCertificateKeyFile="$(signingCert.secureFilePath)"'
```

> [!NOTE]
> PackageCertificateThumbprint 引數會刻意設定為空字串做為預防措施。 如果在專案中設定指紋，但不符合簽署憑證，則建置將會失敗，並出現錯誤：`Certificate does not match supplied signing thumbprint`。

### <a name="review-parameters"></a>檢閱參數

使用 `$()` 語法定義的參數為組建定義中定義的變數，且在其他的建置系統中將會變更。

![預設變數](images/building-screen5.png)

若要檢視所有預先定義的變數，請參閱[預先定義的建置變數](/azure/devops/pipelines/build/variables)。

## <a name="configure-the-publish-build-artifacts-task"></a>設定發佈建置成品工作

預設 UWP 管線不會儲存所產生的成品。 若要將發佈功能新增至您的 YAML 定義，請新增下列工作。

```yml
- task: CopyFiles@2
  displayName: 'Copy Files to: $(build.artifactstagingdirectory)'
  inputs:
    SourceFolder: '$(system.defaultworkingdirectory)'
    Contents: '**\bin\$(BuildConfiguration)\**'
    TargetFolder: '$(build.artifactstagingdirectory)'

- task: PublishBuildArtifacts@1
  displayName: 'Publish Artifact: drop'
  inputs:
    PathtoPublish: '$(build.artifactstagingdirectory)'
```

您可以在建置結果頁面的 [成品]  選項中查看產生的成品。

![構件](images/building-screen6.png)

因為我們已將 `UapAppxPackageBuildMode` 引數設定為 `StoreUpload`，構件資料夾包含提交至 Microsoft Store 的套件 (.msixupload/.appxupload)。 請注意，您也可以提交一般應用程式套件 (.msix/.appx) 或應用程式套件組合 (.msixbundle/.appxbundle/) 至 Microsoft Store。 根據此文章的用途，我們將使用 .appxupload 檔案。

## <a name="address-bundle-errors"></a>位址組合錯誤

如果您將一個以上的 UWP 專案新增至您的方案，然後嘗試建立套件組合，您可能會收到以下錯誤。

  `MakeAppx(0,0): Error : Error info: error 80080204: The package with file name "AppOne.UnitTests_0.1.2595.0_x86.appx" and package full name "8ef641d1-4557-4e33-957f-6895b122f1e6_0.1.2595.0_x86__scrj5wvaadcy6" is not valid in the bundle because it has a different package family name than other packages in the bundle`

會顯示此錯誤是因為在方案層級，仍不清楚套件組合中應顯示哪個 App。 若要解決此問題，請開啟每個專案檔案，並在第一個 `<PropertyGroup>` 元素的結尾處新增下列屬性。

|**專案**|**屬性**|
|-------|----------|
|應用程式|`<AppxBundle>Always</AppxBundle>`|
|UnitTests|`<AppxBundle>Never</AppxBundle>`|

然後，從建置步驟中移除 `AppxBundle` MSBuild 引數。

## <a name="related-topics"></a>相關主題

- [建置適用於 Windows 的 .NET 應用程式](/vsts/build-release/get-started/dot-net) \(英文\)
- [封裝 UWP 應用程式](/windows/msix/package/packaging-uwp-apps)
- [在 Windows 10 中側載 LOB 應用程式](/windows/deploy/sideload-apps-in-windows-10) \(部分機器翻譯\)
- [建立套件簽署的憑證](/windows/msix/package/create-certificate-package-signing)