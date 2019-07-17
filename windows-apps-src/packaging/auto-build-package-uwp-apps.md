---
title: 設定您的 UWP 應用程式的自動化組建
description: 如何設定您的自動化組建以產生側載及/或儲存套件。
ms.date: 09/30/2018
ms.topic: article
keywords: Windows 10, UWP
ms.assetid: f9b0d6bd-af12-4237-bc66-0c218859d2fd
ms.localizationpriority: medium
ms.openlocfilehash: 5837674f2cb20710a59eeac0af59498bf28b197e
ms.sourcegitcommit: a86d0bd1c2f67e5986cac88a98ad4f9e667cfec5
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/16/2019
ms.locfileid: "68229387"
---
# <a name="set-up-automated-builds-for-your-uwp-app"></a>設定您的 UWP 應用程式的自動化組建

若要建立的 UWP 專案的自動化的組建，您可以使用 Azure 的管線。 在本文中，我們將探討不同的方式，若要這樣做。 我們也會顯示您如何執行這些工作的使用命令列，以便您可以使用任何其他組建系統整合。

## <a name="create-a-new-azure-pipeline"></a>建立新的 Azure 管線

先是[註冊 Azure 管線](https://docs.microsoft.com/azure/devops/pipelines/get-started/pipelines-sign-up)如果您已經還沒有這麼做。

接下來，建立可用來建置您的程式碼的管線。 如需建置管線來建置的 GitHub 存放庫的教學課程，請參閱 <<c0> [ 建立您的第一個管線](https://docs.microsoft.com/azure/devops/pipelines/get-started-yaml)。 Azure 的管線支援所列的儲存機制類型[這篇文章中](https://docs.microsoft.com/azure/devops/pipelines/repos)。

## <a name="set-up-an-automated-build"></a>設定自動化組建

我們一開始使用 UWP 建立可用於 Azure 的 Dev Ops 的定義，並接著示範如何設定管線的預設值。

在組建定義範本清單中，選擇 **\[通用 Windows 平台\]** 範本。

![選取 [UWP] 範本](images/select-yaml-template.png)

此範本包含基本的設定，來建置您的 UWP 專案：

```yml
trigger:
- master

pool:
  vmImage: 'VS2017-Win2016'

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

預設範本會嘗試在.csproj 檔案中指定的憑證簽署封裝。 如果您想要在建置期間簽署您的套件必須有私用金鑰的存取權。 否則，您可以停用簽章加上參數`/p:AppxPackageSigningEnabled=false`至`msbuildArgs`YAML 檔案中的一節。

## <a name="add-your-project-certificate-to-the-secure-files-library"></a>將您專案的憑證新增至 安全檔案程式庫

您應該避免送出至如果可能的話，存放庫的憑證，git 會忽略它們的預設值。 若要管理安全處理的敏感性檔案，如憑證，支援 Azure DevOps[保護的檔案](https://docs.microsoft.com/azure/devops/pipelines/library/secure-files?view=azure-devops)。

若要上傳您的自動化建置的憑證：

1. 在 Azure 管線中，依序展開**管線**瀏覽窗格中按一下**程式庫**。
2. 按一下 **保護的檔案**索引標籤，然後按一下 **+ 安全檔案**。

    ![如何上傳安全檔案](images/secure-file1.png)

3. 瀏覽憑證檔案，然後按一下**確定**。
4. 上傳憑證之後，選取以檢視其屬性。 底下**管線的權限**，讓**在所有管線中使用的授權**切換。

    ![如何上傳安全檔案](images/secure-file2.png)

## <a name="configure-the-build-solution-build-task"></a>設定建置方案建置工作

這項工作會編譯二進位檔的工作資料夾中，並產生輸出的應用程式套件檔案的任何解決方案。
此工作會使用 MSBuild 引數。 您必須指定那些引數的值。 使用下表做為指引。

|**MSBuild 引數**|**值**|**描述**|
|--------------------|---------|---------------|
| AppxPackageDir | $(Build.ArtifactStagingDirectory)\AppxPackages | 定義要儲存所產生構件的資料夾。 |
| AppxBundlePlatforms | $(Build.BuildPlatform) | 可讓您定義要包含在組合中的平台。 |
| AppxBundle | 永遠 | 使用指定的平台的.msix/.appx 檔案中建立.msixbundle/.appxbundle。 |
| UapAppxPackageBuildMode | StoreUpload | 產生.msixupload/.appxupload 檔案和 **_Test**側載的資料夾。 |
| UapAppxPackageBuildMode | CI | 產生僅.msixupload/.appxupload 檔案。 |
| UapAppxPackageBuildMode | SideloadOnly | 會產生 **_Test**只側載的資料夾。 |
| AppxPackageSigningEnabled | true | 可讓封裝簽章。 |
| PackageCertificateThumbprint | 憑證指紋 | 此值**必須**比對中簽署憑證的指紋，或可以是空字串。 |
| PackageCertificateKeyFile | `Path` | 若要使用憑證的路徑。 這被擷取自安全的檔案中繼資料。 |

### <a name="configure-the-build"></a>設定組建

如果您想要使用命令列中，或使用任何其他組建系統建置您的解決方案，請使用這些引數執行 MSBuild。

```powershell
/p:AppxPackageDir="$(Build.ArtifactStagingDirectory)\AppxPackages\\"
/p:UapAppxPackageBuildMode=StoreUpload
/p:AppxBundlePlatforms="$(Build.BuildPlatform)"
/p:AppxBundle=Always
```

### <a name="configure-package-signing"></a>設定封裝簽章

若要簽署 MSIX （或 APPX） 套件管線必須擷取簽署的憑證。 若要這樣做，請加入 DownloadSecureFile 工作之前 VSBuild 工作。
這可讓您存取的簽署憑證透過```signingCert```。

```yml
- task: DownloadSecureFile@1
  name: signingCert
  displayName: 'Download CA certificate'
  inputs:
    secureFile: '[Your_Pfx].pfx'
```

接下來，更新參考的簽章憑證的 VSBuild 工作：

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
> 為求安全起見刻意 PackageCertificateThumbprint 引數設定為空字串。 如果憑證指紋在專案中設定，但不符合簽署憑證，建置將會失敗並出現錯誤： `Certificate does not match supplied signing thumbprint`。

### <a name="review-parameters"></a>檢閱參數

定義使用參數`$()`語法會在組建定義中，所定義的變數，而且將會變更其他建置系統。

![預設變數](images/building-screen5.png)

若要檢視所有預先定義的變數，請參閱[預先定義建置變數](https://docs.microsoft.com/azure/devops/pipelines/build/variables)。

## <a name="configure-the-publish-build-artifacts-task"></a>設定發行組建成品的工作

預設的 UWP 管線不會儲存產生的成品。 若要加入 YAML 定義中的發佈功能，請新增下列工作。

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

您可以看到在產生的成品**成品**選項的組建結果頁面。

![構件](images/building-screen6.png)

因為我們已設定`UapAppxPackageBuildMode`引數`StoreUpload`，[成品] 資料夾包含封裝以提交至市集 (.msixupload/.appxupload)。 請注意，您也可以提交的規則的應用程式套件 (.msix/.appx) 或應用程式套件組合 (.msixbundle/.appxbundle/) 存放區。 根據本文的用途，我們將使用 .appxupload 檔案。

## <a name="address-bundle-errors"></a>地址組合錯誤

如果您將一個以上的 UWP 專案新增至您的方案，然後再嘗試建立搭售方案，您可能會收到與下列類似錯誤。

  `MakeAppx(0,0): Error : Error info: error 80080204: The package with file name "AppOne.UnitTests_0.1.2595.0_x86.appx" and package full name "8ef641d1-4557-4e33-957f-6895b122f1e6_0.1.2595.0_x86__scrj5wvaadcy6" is not valid in the bundle because it has a different package family name than other packages in the bundle`

會顯示此錯誤是因為在方案層級，仍不清楚套件組合中應顯示哪個 App。 若要解決此問題，開啟每個專案檔，並在第一個結尾處新增下列屬性`<PropertyGroup>`項目。

|**專案**|**屬性**|
|-------|----------|
|App|`<AppxBundle>Always</AppxBundle>`|
|UnitTests|`<AppxBundle>Never</AppxBundle>`|

然後，移除`AppxBundle`MSBuild 引數，從建置步驟。

## <a name="related-topics"></a>相關主題

- [針對 Windows 建置您的.NET 應用程式](https://docs.microsoft.com/vsts/build-release/get-started/dot-net)
- [封裝 UWP 應用程式](https://docs.microsoft.com/windows/uwp/packaging/packaging-uwp-apps)
- [側載 Windows 10 中的 LOB 應用程式](https://docs.microsoft.com/windows/deploy/sideload-apps-in-windows-10)
- [建立封裝簽章的憑證](https://docs.microsoft.com/windows/uwp/packaging/create-certificate-package-signing)
