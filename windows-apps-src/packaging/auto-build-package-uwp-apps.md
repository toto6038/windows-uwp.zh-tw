---
title: 設定您的 UWP 應用程式的自動化組建
description: 如何設定您的自動化組建以產生側載及/或儲存套件。
ms.date: 07/17/2019
ms.topic: article
keywords: windows 10, uwp
ms.assetid: f9b0d6bd-af12-4237-bc66-0c218859d2fd
ms.localizationpriority: medium
ms.openlocfilehash: 70415c9f3d58625cfdc651ec67c8a9f37c23cffa
ms.sourcegitcommit: 3e7a4f7605dfb4e87bac2d10b6d64f8b35229546
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/08/2020
ms.locfileid: "77089494"
---
# <a name="set-up-automated-builds-for-your-uwp-app"></a>設定您的 UWP 應用程式的自動化組建

您可以使用 Azure Pipelines 建立 UWP 專案的自動化組建。 在本文中，我們將探討不同的方法來執行這項操作。 我們也會示範如何使用命令列來執行這些工作，以便與其他任何組建系統整合。

## <a name="create-a-new-azure-pipeline"></a>建立新的 Azure 管線

請先[註冊 Azure Pipelines （](https://docs.microsoft.com/azure/devops/pipelines/get-started/pipelines-sign-up)如果您尚未這麼做）。

接下來，建立可用於建立原始程式碼的管線。 如需建立管線以建立 GitHub 存放庫的教學課程，請參閱[建立您的第一個管線](https://docs.microsoft.com/azure/devops/pipelines/get-started-yaml)。 Azure Pipelines 支援本文[中](https://docs.microsoft.com/azure/devops/pipelines/repos)所列的存放庫類型。

## <a name="set-up-an-automated-build"></a>設定自動化組建

我們會從 Azure Dev Ops 中提供的預設 UWP 組建定義開始，然後示範如何設定管線。

在組建定義範本清單中，選擇 **\[通用 Windows 平台\]** 範本。

![選取 UWP 範本](images/select-yaml-template.png)

此範本包含用來建立 UWP 專案的基本設定：

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

預設範本會嘗試使用 .csproj 檔案中指定的憑證來簽署封裝。 如果您想要在組建期間簽署封裝，您必須具有私密金鑰的存取權。 否則，您可以藉由將參數 `/p:AppxPackageSigningEnabled=false` 新增至 YAML 檔案中的 `msbuildArgs` 區段來停用簽章。

## <a name="add-your-project-certificate-to-the-secure-files-library"></a>將您的專案憑證新增至安全檔案程式庫

您應盡可能避免將憑證提交至您的存放庫（如果可能的話），而且 git 預設會忽略它們。 若要管理敏感檔案（例如憑證）的安全處理，Azure DevOps 支援[安全](https://docs.microsoft.com/azure/devops/pipelines/library/secure-files?view=azure-devops)檔案功能。

若要上傳自動化組建的憑證：

1. 在 Azure Pipelines 中，展開流覽窗格中的 [**管線**]，然後按一下 [連結**庫**]。
2. 按一下 [**安全**檔案] 索引標籤，然後按一下 [ **+ 安全**檔案]。

    ![如何上傳安全檔案](images/secure-file1.png)

3. 流覽至憑證檔案，然後按一下 **[確定]** 。
4. 當您上傳憑證之後，請選取它來查看其屬性。 在 [**管線許可權**] 下，啟用 [**授權在所有管線中使用**] 切換。

    ![如何上傳安全檔案](images/secure-file2.png)

5. 如果憑證中的私密金鑰有密碼，建議您將密碼儲存在[Azure Key Vault](https://docs.microsoft.com/azure/key-vault/about-keys-secrets-and-certificates) ，然後將密碼連結到[變數群組](https://docs.microsoft.com/azure/devops/pipelines/library/variable-groups)。 您可以使用變數來存取管線中的密碼。 請注意，只有私密金鑰才支援密碼;目前不支援使用本身受到密碼保護的憑證檔案。

> [!NOTE]
> 從 Visual Studio 2019 開始，UWP 專案中不會再產生暫時性憑證。 若要建立或匯出憑證，請使用[本文中所](/windows/msix/package/create-certificate-package-signing)述的 PowerShell Cmdlet。

## <a name="configure-the-build-solution-build-task"></a>設定建置方案建置工作

此工作會將工作資料夾中的任何解決方案編譯成二進位檔，並產生輸出應用程式封裝檔案。 這項工作會使用 MSBuild 引數。 您必須指定那些引數的值。 使用下表做為指引。

|**MSBuild 引數**|**值**|**描述**|
|--------------------|---------|---------------|
| AppxPackageDir | $(Build.ArtifactStagingDirectory)\AppxPackages | 定義要儲存所產生構件的資料夾。 |
| AppxBundlePlatforms | $(Build.BuildPlatform) | 可讓您定義要包含在組合中的平臺。 |
| AppxBundle | 一律 | 使用指定平臺的 msix/.appx 檔案建立 .msixbundle/.appxbundle。 |
| UapAppxPackageBuildMode | StoreUpload | 產生 msixupload/. .appxupload 檔案和用於側載的 **_Test**資料夾。 |
| UapAppxPackageBuildMode | CI | 只會產生 msixupload/. .appxupload 檔案。 |
| UapAppxPackageBuildMode | SideloadOnly | 僅針對側載產生 **_Test**資料夾。 |
| AppxPackageSigningEnabled | true | 啟用套件簽署。 |
| PackageCertificateThumbprint | 憑證指紋 | 此值**必須**符合簽署憑證中的指紋，或為空字串。 |
| PackageCertificateKeyFile | 路徑 | 要使用之憑證的路徑。 這會從安全的檔案中繼資料中取出。 |
| PackageCertificatePassword | Password | 憑證中私密金鑰的密碼。 我們建議您將密碼儲存在[Azure Key Vault](https://docs.microsoft.com/azure/key-vault/about-keys-secrets-and-certificates) ，並將密碼連結到[變數群組](https://docs.microsoft.com/azure/devops/pipelines/library/variable-groups)。 您可以將變數傳遞給這個引數。 |

### <a name="configure-the-build"></a>設定組建

如果您想要使用命令列或使用任何其他組建系統來建立您的方案，請使用這些引數執行 MSBuild。

```powershell
/p:AppxPackageDir="$(Build.ArtifactStagingDirectory)\AppxPackages\\"
/p:UapAppxPackageBuildMode=StoreUpload
/p:AppxBundlePlatforms="$(Build.BuildPlatform)"
/p:AppxBundle=Always
```

### <a name="configure-package-signing"></a>設定封裝簽署

若要簽署 MSIX （或 .appx）封裝，管線必須取得簽署憑證。 若要這麼做，請在 VSBuild 工作之前新增 DownloadSecureFile 工作。
這可讓您透過 ```signingCert```存取簽署憑證。

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
> PackageCertificateThumbprint 引數會刻意設定為空字串做為預防措施。 如果在專案中設定指紋，但不符合簽署憑證，組建將會失敗，並出現錯誤： `Certificate does not match supplied signing thumbprint`。

### <a name="review-parameters"></a>審核參數

以 `$()` 語法定義的參數是定義于組建定義中的變數，而且將會在其他組建系統中變更。

![預設變數](images/building-screen5.png)

若要查看所有預先定義的變數，請參閱[預先定義的組建變數](https://docs.microsoft.com/azure/devops/pipelines/build/variables)。

## <a name="configure-the-publish-build-artifacts-task"></a>設定發行組建成品工作

預設 UWP 管線不會儲存產生的成品。 若要將發佈功能新增至您的 YAML 定義，請新增下列工作。

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

您可以在 [組建結果] 頁面的 [成品 **] 選項中**看到產生的構件。

![成品](images/building-screen6.png)

因為我們已將 `UapAppxPackageBuildMode` 引數設定為 `StoreUpload`，所以 [成品] 資料夾會包含提交至存放區的封裝（. msixupload/. .appxupload）。 請注意，您也可以將一般應用程式套件（. msix/.appx）或應用程式配套（. .msixbundle/.appxbundle/）提交至存放區。 根據本文的用途，我們將使用 .appxupload 檔案。

## <a name="address-bundle-errors"></a>位址組合錯誤

如果您將多個 UWP 專案新增至您的方案，然後嘗試建立配套，您可能會收到類似這項的錯誤。

  `MakeAppx(0,0): Error : Error info: error 80080204: The package with file name "AppOne.UnitTests_0.1.2595.0_x86.appx" and package full name "8ef641d1-4557-4e33-957f-6895b122f1e6_0.1.2595.0_x86__scrj5wvaadcy6" is not valid in the bundle because it has a different package family name than other packages in the bundle`

會顯示此錯誤是因為在方案層級，仍不清楚套件組合中應顯示哪個 App。 若要解決此問題，請開啟每個專案檔，並在第一個 `<PropertyGroup>` 元素的結尾加入下列屬性。

|**專案**|**屬性**|
|-------|----------|
|應用程式|`<AppxBundle>Always</AppxBundle>`|
|UnitTests|`<AppxBundle>Never</AppxBundle>`|

然後，從 [組建] 步驟中移除 `AppxBundle` MSBuild 引數。

## <a name="related-topics"></a>相關主題

- [建立適用于 Windows 的 .NET 應用程式](https://docs.microsoft.com/vsts/build-release/get-started/dot-net)
- [封裝 UWP 應用程式](/windows/msix/package/packaging-uwp-apps)
- [在 Windows 10 中側載 LOB 應用程式](https://docs.microsoft.com/windows/deploy/sideload-apps-in-windows-10)
- [建立封裝簽署的憑證](/windows/msix/package/create-certificate-package-signing)
