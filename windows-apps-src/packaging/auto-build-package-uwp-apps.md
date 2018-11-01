---
author: laurenhughes
title: 為您的 UWP app 設定自動化組建
description: 如何設定您的自動化組建以產生側載及/或儲存套件。
ms.author: lahugh
ms.date: 09/30/2018
ms.topic: article
keywords: Windows 10, UWP
ms.assetid: f9b0d6bd-af12-4237-bc66-0c218859d2fd
ms.localizationpriority: medium
ms.openlocfilehash: 5fd82e4574ab6c6aff30d762f2eacc0d62fd6bdf
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/31/2018
ms.locfileid: "5870453"
---
# <a name="set-up-automated-builds-for-your-uwp-app"></a>設定您的 UWP app 的自動化組建

您可以使用 Visual Studio Team Services (VSTS) 來建立 UWP 專案的自動化組建。 在本文中，我們會說明執行此操作的不同方式。  我們也會說明如何透過使用命令列來執行這些工作，這樣您就可以與其他建置系統 (例如 AppVeyor) 整合。 

## <a name="select-the-right-type-of-build-agent"></a>選取正確的組建代理程式類型

選擇您希望 VSTS 在執行建置處理序時使用的組建代理程式類型。 託管的組建代理程式是使用最常見的工具與 SDK 進行部署，且大部分情況下都可運作，請參閱[託管的組建伺服器上的軟體](https://www.visualstudio.com/docs/build/admin/agents/hosted-pool#software)一文。 不過，如果您需要對建置步驟有更多控制權，您可以建立自訂的組建代理程式。 您可以使用下表來協助您決定。

| **案例** | **自訂的代理程式** | **託管的組建代理程式** |
|-------------|----------------|----------------------|
| 基本 UWP 組建 (包含 .NET Native)| :white_check_mark: | :white_check_mark: |
| 產生側載套件| :white_check_mark: | :white_check_mark: |
| 產生 [Microsoft Store] 提交套件| :white_check_mark: | :white_check_mark: |
| 使用自訂憑證| :white_check_mark: | |
| 以自訂 Windows SDK 為目標的組建| :white_check_mark: |  |
| 執行單元測試| :white_check_mark: |  |
| 使用累加建置| :white_check_mark: |  |

#### <a name="create-a-custom-build-agent-optional"></a>建立自訂的組建代理程式 (選用)

如果您選擇建立自訂的組建代理程式，您將需要通用 Windows 平台工具。 這些工具是 Visual Studio 的一部分。 您可以使用 Visual Studio Community 版。

若要深入了解，請參閱[在 Windows 上部署代理程式](https://www.visualstudio.com/docs/build/admin/agents/v2-windows)。 

若要執行 UWP 單元測試，您必須執行下列動作： 
- 部署和啟動您的 app 
- 在互動模式中執行 VSTS 代理程式 
- 將您的代理程式設定為在重新開機之後自動登入 

## <a name="set-up-an-automated-build"></a>設定自動化組建
我們將從 VSTS 中可用的預設 UWP 組建定義開始，然後為您說明如何設定該定義，以讓您完成更多進階組建工作。

**將您的專案憑證新增到原始程式碼儲存機制**

VSTS 可同時搭配以 TFS 和 GIT 為基礎的程式碼儲存機制運作。  
如果您使用 Git 儲存機制，請將您專案的憑證檔案新增到儲存機制，這樣一來組建代理程式就可以簽署應用程式套件。 如果您沒有這麼做，Git 儲存機制會略過憑證檔案。 若要將憑證檔案新增到您的儲存機制，請以滑鼠右鍵按一下 [方案總管] 中的憑證檔案，然後在捷徑功能表中選擇 [將忽略的檔案加入原始檔控制] 命令。 

![如何包含憑證](images/building-screen1.png)

我們稍後在本指南中將討論[進階憑證管理](#certificates-best-practices)。 

若要在 VSTS 中建立您的第一個組建定義，請瀏覽到 [組建] 索引標籤，然後選取 [+] 按鈕。

![建立組建定義](images/building-screen2.png)

在組建定義範本清單中，選擇 *\[通用 Windows 平台\]* 範本。

![選取 UWP 組建](images/building-screen3.png)

此組建定義包含下列的建置工作︰

- NuGet restore **\*.sln
- Build solution **\*.sln
- 發行符號
- 發行構件：拖放

#### <a name="configure-the-nuget-restore-build-task"></a>設定 NuGet 還原建置工作

這個工作會還原您專案中定義的 NuGet 套件。 有些套件需要 NuGet.exe 的自訂版本。 如果您使用需要 NuGet.exe 自訂版本的套件，請參考在您儲存機制中該版本的 NuGet.exe，然後在 *NuGet.exe 路徑*進階屬性中參考它。

![預設的組建定義](images/building-screen4.png)

#### <a name="configure-the-build-solution-build-task"></a>設定建置方案建置工作

這項工作會編譯任何方案中為二進位檔的工作資料夾並產生輸出應用程式套件檔案。 這個工作會使用 MSbuild 引數。  您必須指定那些引數的值。 使用下表做為指引。 

|**MSBuild 引數**|**值**|**說明**|
|--------------------|---------|---------------|
|AppxPackageDir|$(Build.ArtifactStagingDirectory)\AppxPackages|定義要儲存所產生構件的資料夾。|
|AppxBundlePlatforms|$(Build.BuildPlatform)|可讓您定義套件組合中要包含的平台。|
|AppxBundle|一律|使用指定平台的 appx 檔案建立 appxbundle。|
|**UapAppxPackageBuildMode**|StoreUpload|定義要產生的應用程式套件的類型。 (預設為不包含)|


如果您想要使用命令列，或使用任何其他建置系統建置您的方案，請使用這些引數執行 msbuild。

```
/p:AppxPackageDir="$(Build.ArtifactStagingDirectory)\AppxPackages\\"  
/p:UapAppxPackageBuildMode=StoreUpload 
/p:AppxBundlePlatforms="$(Build.BuildPlatform)"
/p:AppxBundle=Always
```

使用 $() 語法定義的參數為組建定義中定義的變數，且在其他的建置系統中將會變更。

![預設變數](images/building-screen5.png)

若要檢視所有預先定義的變數，請參閱[使用建置變數](https://www.visualstudio.com/docs/build/define/variables)。

#### <a name="configure-the-publish-artifact-build-task"></a>設定發行構件建置工作 
這個工作會將已產生構件儲存在 VSTS 中。 您可以在建置結果頁面的 [構件] 索引標籤中看到它們。 VSTS 會使用我們之前已定義的 `$(Build.ArtifactStagingDirectory)\AppxPackages` 資料夾。

![構件](images/building-screen6.png)

因為我們已將 `UapAppxPackageBuildMode` 屬性設定為 `StoreUpload`，構件資料夾包含建議提交至Microsoft Store的套件 (.appxupload)。 請注意，您也可以提交一般應用程式套件 (.appx/.msix) 或應用程式套件組合 (.appxbundle/.msixbundle) 至市集。 根據本文的用途，我們將使用 .appxupload 檔案。


>[!NOTE]
> 根據預設，VSTS 代理程式會維持所產生的最新應用程式套件。 如果您只想要儲存目前組建的構件，請設定組建以清除二進位檔目錄。 若要這樣做，請新增一個名為 `Build.Clean` 的變數，然後將它的值設定為 `all`。 若要深入了解，請參閱[指定儲存機制](https://www.visualstudio.com/docs/build/define/repository#how-can-i-clean-the-repository-in-a-different-way)。

#### <a name="the-types-of-automated-builds"></a>自動化組建類型
接下來，您將使用您的組建定義建立自動化組建。 下表說明您可以建立的每一種自動化組建。 

|**建置類型**|**構件**|**建議頻率**|**說明**|
|-----------------|------------|-------------------------|---------------|
|連續整合|組建記錄檔，測試結果|每個認可|這種類型的組建很快速，且一天可以執行數次。|
|用於側載的連續部署組建|部署套件|每天 |這種類型的組建可包含單元測試，但需要較長的時間。 它允許手動測試，且您可以將它與其他工具整合 (例如 HockeyApp)。|
|將套件提交至Microsoft Store的連續部署組建|發佈套件|隨選安裝|這種類型的組建會建立您可以發行至Microsoft Store的套件。|

讓我們看看每個套件的設定方式。


## <a name="set-up-a-continuous-integration-ci-build"></a>設定連續整合 (CI) 組建 
這種類型的組建可協助您快速診斷程式碼相關問題。 它們通常僅針對單一平台執行，且不需要由 .NET 原生工具鏈處理。 此外，使用 CI 組建，您可以執行可產生測試結果報告的單元測試。  

如果您想要執行 UWP 單元測試做為您 CI 組建的一部分，您需要使用自訂組建代理程式而非裝載的組建代理程式。

>[!NOTE]
> 如果您在同一個方案中組合多個應用程式，您可能會收到錯誤。 請參閱下列主題，以取得解決錯誤的說明︰[解決在同一個方案中組合多個 App 時出現的錯誤。](#bundle-errors) 


### <a name="configure-a-ci-build-definition"></a>設定 CI 組建定義
使用預設 UWP 範本來建立組建定義。 然後，將 [觸發程序] 設定為在每次登入時執行。  

![ci 觸發程序](images/building-screen7.png)

因為 CI 組建不會部署到使用者，所以最好維持不同的版本編號以避免混淆 CD 組建。 例如：
`$(BuildDefinitionName)_0.0.$(DayOfYear)$(Rev:.r)`


#### <a name="configure-a-custom-build-agent-for-unit-testing"></a>設定單元測試的自訂組建代理程式

1. 在您的電腦上啟用 [開發人員模式]。 如需詳細資訊，請參閱[啟用您的裝置以用於開發](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development)。 
2. 啟用以互動式處理程序執行的服務。 若要深入了解，請參閱[在 Windows 上部署代理程式](https://docs.microsoft.com/vsts/build-release/actions/agents/v2-windows)。 
3. 將簽署憑證部署到代理程式。

若要部署簽署憑證，請按兩下 `.cer` 檔案、選擇 **\[本機電腦\]**，然後選擇 **\[受信任的人存放區\]**。

<span id="uwp-unit-tests" />

### <a name="configure-the-build-definition-to-run-uwp-unit-tests"></a>設定組建定義以執行 UWP 單元測試
若要執行單元測試，請使用 Visual Studio 測試建置步驟。


![新增單元測試](images/building-screen8.png)

UWP 單元測試會在指定 appxrecipe 檔案的內容中執行，因此您無法使用產生的套件組合。 此外，您必須指定具體平台 appxrecipe 檔案的路徑。 例如：

```
$(Build.ArtifactStagingDirectory)\AppxPackages\MyUWPApp.UnitTest\x86\MyUWPApp.UnitTest_$(AppxVersion)_x86.appxrecipe
```

為了讓測試執行，主控台參數需要新增到 vstest.console.exe。 可透過以下方式提供此參數：**執行選項 => 其他主控台選項**。 請新增以下參數： 

```
/framework:FrameworkUap10
```

>[!NOTE]
> 使用下列命令從命令列在本機執行單元測試：
`"%ProgramFiles(x86)%\Microsoft Visual Studio 14.0\Common7\IDE\CommonExtensions\Microsoft\TestWindow\vstest.console.exe"`

#### <a name="access-test-results"></a>存取測試結果
在 VSTS 中，組建摘要頁面會顯示每個執行單元測試之組建的測試結果。 在這裡，您可以開啟 **\[測試結果\]** 頁面，以查看有關測試結果的詳細資料。 

![測試結果](images/building-screen9.png)

#### <a name="improve-the-speed-of-a-ci-build"></a>改善 CI 組建的速度
如果您僅想使用 CI 組建來監視您的簽入品質，您可以減少建置次數。

#### <a name="to-improve-the-speed-of-a-ci-build"></a>改善 CI 組建的速度
1.  僅適用單一平台的組建
2.  編輯 BuildPlatform 變數，以僅使用 x86。 ![設定 ci](images/building-screen10.png) 
3.  在建置步驟中，將 /p:AppxBundle=Never 新增到 MSBuild 引數屬性中，然後設定 [平台] 屬性。. ![設定平台](images/building-screen11.png)
4.  在單元測試專案中，停用 .NET 原生項目。 

若要這樣做，請開啟專案檔案，然後在專案屬性中將 `UseDotNetNativeToolchain` 屬性設定為 `false`。

使用 .NET 原生工具鏈是工作流程中重要的一部分，因此您仍應該使用它來測試發行組建。 

<span id="bundle-errors" />

#### <a name="address-errors-that-appear-when-you-bundle-more-than-one-app-in-the-same-solution"></a>解決在同一個方案中組合多個 App 時出現的錯誤 
如果您將一個以上的 UWP 專案新增至您的方案，然後嘗試建立套件組合，您可能會收到以下錯誤︰ 

```
MakeAppx(0,0): Error : Error info: error 80080204: The package with file name "AppOne.UnitTests_0.1.2595.0_x86.appx" and package full name "8ef641d1-4557-4e33-957f-6895b122f1e6_0.1.2595.0_x86__scrj5wvaadcy6" is not valid in the bundle because it has a different package family name than other packages in the bundle
```

會顯示此錯誤是因為在方案層級，仍不清楚套件組合中應顯示哪個 App。 若要解決此問題，請開啟每個專案檔案，並在第一個 `<PropertyGroup>` 項目的結尾處新增下列屬性：

|**專案**|**屬性**|
|-------|----------|
|App|`<AppxBundle>Always</AppxBundle>`|
|UnitTests|`<AppxBundle>Never</AppxBundle>`|

然後，從建置步驟中移除 `AppxBundle` msbuild 引數。

## <a name="set-up-a-continuous-deployment-build-for-sideloading"></a>設定用於側載的連續部署組建
完成這種類型的組建時，使用者可以從組建結果頁面的構件區段下載應用程式套件組合檔案。 如果您想透過建立更完整的通訊群組對 App 進行 Beta 測試，您可以使用 HockeyApp 服務。 此服務提供進行 Beta 測試、使用者分析以及損毀診斷的進階功能。

### <a name="applying-version-numbers-to-your-builds"></a>對您的組建套用版本號碼

資訊清單檔案中包含應用程式版本號碼。  更新原始檔控制儲存機制中的資訊清單檔案，以變更版本號碼。 更新 App 版本號碼的另一種方式是使用 VSTS 產生的組建編號，然後在您編譯 App 之前修改應用程式資訊清單。 但是不要認可對原始程式碼儲存機制的變更。

您必須在組建定義中定義您的版本管理組建編號格式，然後在您編譯之前，使用產生的版本號碼更新 AppxManifest 以及 AssemblyInfo.cs (選擇性) 檔案。

在組建定義的 *\[一般\]* 索引標籤中定義組建編號格式。

![組建版本](images/building-screen12.png) 

例如，如果您將組建編號格式設定為以下的值：
``` 
$(BuildDefinitionName)_1.1.$(DayOfYear)$(Rev:r).0 
```

VSTS 會產生如下所示的版本號碼：
```
CI_MyUWPApp_1.1.2501.0
```

>[!NOTE]
>Microsoft Store 將會要求版本中最後一個數字為 0。

因此，您可以擷取版本號碼，並將它套用到資訊清單及/或 `AssemblyInfo` 檔案，使用自訂 PowerShell 指令碼 (可在[此處](https://go.microsoft.com/fwlink/?prd=12560&pver=14&plcid=0x409&clcid=0x9&ar=DevCenter&sar=docs)取得)。 該指令碼會從環境變數 `BUILD_BUILDNUMBER` 讀取版本號碼，然後修改 AssemblyInfo 與 AppxManifest 檔案。 請確定將此指令碼新增到您的來源儲存機制，然後設定 PowerShell 組建工作，如下所示︰


![更新版本](images/building-screen13.png) 


            `$(AppxVersion)` 變數包含版本號碼。 您可以在其他建置步驟中使用該號碼。 


#### <a name="optional-integrate-with-hockeyapp"></a>選用︰整合 HockeyApp
首先，安裝 [HockeyApp](https://marketplace.visualstudio.com/items?itemName=ms.hockeyapp) Visual Studio 擴充功能。 您必須以 VSTS 系統管理員身分安裝此擴充功能。 

![hockey app](images/building-screen14.png) 

接著，使用本指南設定 HockeyApp 連線：[如何使用 HockeyApp 搭配 Visual Studio Team Services (VSTS) 或 Team Foundation Server (TFS)](https://support.hockeyapp.net/kb/third-party-bug-trackers-services-and-webhooks/how-to-use-hockeyapp-with-visual-studio-team-services-vsts-or-team-foundation-server-tfs)。 您可以使用您的 Microsoft 帳戶、社交媒體帳戶或電子郵件地址來設定您的 HockeyApp 帳戶。 免費方案隨附兩個 App、一位擁有者，而且沒有資料限制。

然後，您可以建立 HockeyApp app，以手動方式，或上傳現有的應用程式套件檔案。 若要深入了解，請參閱[如何建立新的 App](https://support.hockeyapp.net/kb/app-management-2/how-to-create-a-new-app)。  

若要使用現有的應用程式套件檔案，新增建置步驟中，並設定該建置步驟的二進位檔案路徑參數。 

![設定 hockey app](images/building-screen15.png) 

若要設定此參數，請將 App 名稱、AppxVersion 變數以及支援的平台結合為一個字串，例如：

``` 
$(Build.ArtifactStagingDirectory)\AppxPackages\MyUWPApp_$(AppxVersion)_Test\MyUWPApp_$(AppxVersion)_x86_x64_ARM.appxbundle
```

雖然 HockeyApp 工作允許您指定符號檔案的路徑，它會是最佳做法中包含符號套件組合。

## <a name="set-up-a-continuous-deployment-build-that-submits-a-package-to-the-store"></a>設定將套件提交至Microsoft Store的連續部署組建 

若要產生Microsoft Store提交套件，請在 Visual Studio 中使用 [Microsoft Store關聯精靈]，將您的 App 與Microsoft Store產生關聯。

![關聯到Microsoft Store](images/building-screen16.png) 

Microsoft Store 關聯精靈會產生名稱為 Package.StoreAssociation.xml 的檔案，其中包含 Microsoft Store 關聯資訊。 如果您在公用儲存機制 (例如 GitHub) 儲存您的原始程式碼，此檔案將包含該帳戶所有保留的應用程式名稱。 您可以在公開之前，先排除或刪除此檔案。

如果您沒有用來發行 App 之開發人員中心帳戶的存取權，您可以依照以下文件中的指示執行作業：[要針對協力廠商建置 App 嗎？如何封裝其 Microsoft Store 應用程式](https://blogs.windows.com/buildingapps/2015/12/15/building-an-app-for-a-3rd-party-how-to-package-their-store-app/#e35YzR5aRG6uaBqK.97)。 

然後，您需要確認建置步驟包含下列參數︰

```
/p:UapAppxPackageBuildMode=StoreUpload 
```

這將會產生可提交至市集的上傳檔案。


#### <a name="configure-automatic-store-submission"></a>設定自動Microsoft Store提交

使用適用於 Microsoft Store 的 Visual Studio Team Services 擴充功能來整合 Microsoft Store API，並將應用程式套件傳送到 Microsoft Store。

您需要將開發人員中心帳戶與 Azure Active Directory (AD) 連接，然後在您的 AD 中建立 App 以驗證要求。 您可以依照 [擴充功能] 頁面中的指示來完成該作業。 

一旦您設定好擴充功能，您可以新增建置工作，並使用您的應用程式識別碼和上傳檔案的位置進行設定。

![設定開發人員中心](images/building-screen17.png) 

其中 `Package File` 參數的值會是：

```
$(Build.ArtifactStagingDirectory)\
AppxPackages\MyUWPApp__$(AppxVersion)_x86_x64_ARM_bundle.appxupload
```

您必須手動啟用此組建。 您可以使用它來更新現有的 App，但是您不能使用它來進行對Microsoft Store的第一次提交。 如需詳細資訊，請參閱[使用 Microsoft Store 服務建立與管理 Microsoft Store 提交](https://msdn.microsoft.com/windows/uwp/monetize/create-and-manage-submissions-using-windows-store-services)。

## <a name="best-practices"></a>最佳做法

<span id="sideloading-best-practices"/>

### <a name="best-practices-for-sideloading-apps"></a>側載應用程式的最佳做法

如果您想要在不發佈到Microsoft Store的情況下發佈您的 App，只要這些裝置信任用來簽署應用程式套件的憑證，您就可以將您的 App 直接側載到裝置。 

使用 `Add-AppDevPackage.ps1` PowerShell 指令碼來安裝 App。 這個指令碼會將憑證新增到在本機電腦、 信任的根憑證] 區段，並接著將會安裝或更新應用程式套件檔案。

#### <a name="sideloading-your-app-with-the-windows-10-anniversary-update"></a>使用 Windows10 年度更新版側載您的 App
在 Windows 10 年度更新版，您可以按兩下應用程式套件檔案，並在對話方塊中選擇 [安裝] 按鈕來安裝您的應用程式。 

![在 rs1 側載](images/building-screen18.png) 

>[!NOTE]
> 此方法不會安裝憑證或相關的相依性。

如果您想要發佈您的 Windows 應用程式套件從例如 VSTS 或 HockeyApp 網站，您將需要將該網站新增到您的瀏覽器中信任的網站清單。 否則，Windows 會將檔案標示為已鎖定。 

<span id="certificates-best-practices"/>

### <a name="best-practices-for-signing-certificates"></a>簽署憑證的最佳做法 
Visual Studio 會產生每個專案的憑證。 這會讓您難以維護有效憑證的規劃清單。 如果您打算建立數個 App，您可以建立可簽署您所有 App 的單一憑證。 然後，信任您憑證的每部裝置都將能夠側載您任何的 App 而不需要安裝其他憑證。 若要深入了解，請參閱[建立套件簽署的憑證](https://docs.microsoft.com/windows/uwp/packaging/create-certificate-package-signing)。


#### <a name="create-a-signing-certificate"></a>建立簽署憑證
使用 [MakeCert.exe](https://msdn.microsoft.com/library/windows/desktop/ff548309.aspx) 工具來建立憑證。

以下範例是透過使用 MakeCert.exe 工具建立憑證。

```
MakeCert /n publisherName /r /h 0 /eku "1.3.6.1.5.5.7.3.3,1.3.6.1.4.1.311.10.3.13" /e expirationDate /sv MyKey.pvk MyKey.cer
```

然後您可以使用 Pvk2Pfx 工具來產生 PFX 檔案，其中包含以密碼保護的私密金鑰。

提供這些憑證給每個電腦角色︰

|**電腦**|**使用方式**|**憑證**|**憑證存放區**|
|-----------|---------|---------------|---------------------|
|開發人員/建置電腦|簽署組建|MyCert.PFX|目前的使用者/個人|
|開發人員/建置電腦|Run|MyCert.cer|本機電腦/受信任的人|
|使用者|Run|MyCert.cer|本機電腦/受信任的人|

>注意︰您也可以使用您的使用者已經信任的企業憑證。

#### <a name="sign-your-uwp-app"></a>簽署您的 UWP app
Visual Studio 和 MSBuild 提供不同的選項來管理您用來簽署 App 的憑證：

其中一個選項是在方案中包含憑證與私密金鑰 (通常為 .PFX 檔案的形式)，然後在專案檔案中參考 pfx。 您可以透過使用資訊清單編輯器的 [套件] 索引標籤來管理。


![建立憑證](images/building-screen19.png) 

另一個選項是將憑證安裝到建置電腦 (目前使用者/個人)，然後使用 [由憑證存放區挑選] 選項。 這會指定專案檔案中憑證的指紋，這樣一來憑證應該就會安裝在將用來建置專案的所有電腦上。

#### <a name="trust-the-signing-certificate-in-the-target-devices"></a>信任目標裝置中的簽署憑證
目標裝置必須先信任憑證，才能安裝 App。 

註冊本機電腦憑證存放區中 [受信任的人] 或 [信任根目錄] 位置中憑證的公開金鑰。

註冊憑證最快速的方式是按兩下 .cer 檔案，然後依照精靈中的步驟將憑證儲存在 **\[本機電腦\]** 與 **\[受信任的人\]** 存放區。

## <a name="related-topics"></a>相關主題
* [建置適用於 Windows 的.NET App](https://www.visualstudio.com/docs/build/get-started/dot-net) 
* [封裝 UWP app](https://msdn.microsoft.com/windows/uwp/packaging/packaging-uwp-apps)
* [在 Windows 10 中側載 LOB 應用程式](https://technet.microsoft.com/itpro/windows/deploy/sideload-apps-in-windows-10)
* [建立套件簽署的憑證](https://docs.microsoft.com/windows/uwp/packaging/create-certificate-package-signing)
