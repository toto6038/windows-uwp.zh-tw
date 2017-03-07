---
author: awkoren
Description: "本指南說明如何設定您的 Visual Studio 方案來編輯、偵錯和封裝轉換的傳統型橋接器應用程式。"
Search.Product: eADQiWindows 10XVcnh
title: ".NET 傳統型應用程式與 Visual Studio 的傳統型橋接器封裝指南"
ms.author: alkoren
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.assetid: 807a99a7-d285-46e7-af6a-7214da908907
translationtype: Human Translation
ms.sourcegitcommit: 5645eee3dc2ef67b5263b08800b0f96eb8a0a7da
ms.openlocfilehash: 8aa68312d6ce81c809c79ddcafe7732944a628be
ms.lasthandoff: 02/08/2017

---

# <a name="desktop-bridge-packaging-guide-for-net-desktop-apps-with-visual-studio"></a>.NET 傳統型應用程式與 Visual Studio 的傳統型橋接器封裝指南

Windows 10 年度更新版可讓開發人員使用傳統型橋接器封裝使用新套件模型 (.appx) 的現有 Win32 應用程式，以供市集發佈或方便側載。 本指南說明如何設定您的 Visual Studio 方案，以便能夠編輯、偵錯和封裝您的應用程式。 

若要開始使用，請填寫[透過傳統型橋接器將現有的 App 和遊戲帶入 Windows 市集](https://developer.microsoft.com/windows/projects/campaigns/desktop-bridge)的表單。 Microsoft 將會與您連絡以啟動上架程序。 一旦您的帳戶獲得核准，可提交傳統型橋接器應用程式，請依照本文件中的指示準備 appxupload 套件進行上傳。 

> 對於您使用傳統型橋接器的過程中遇到的問題，想提出意見反應嗎？ 提出功能建議的最佳位置是[Windows 開發人員 UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial)。 如有問題或需要偵錯報告，請前往[開發通用 Windows 應用程式論壇](https://social.msdn.microsoft.com/Forums/home?forum=wpdevelop)。

## <a name="default-universal-windows-platform-packages"></a>預設通用 Windows 平台套件

Visual Studio 可讓您產生偵錯和發行套件，這些套件可使用 Windows 市集或應用程式側載散發。 若要加速建立套件，Visual Studio 可協助您建立 appxupload 檔案，隨時可提交至市集。 如需詳細資訊，請參閱[封裝 UWP 應用程式](..\packaging\packaging-uwp-apps.md)。

## <a name="desktop-bridge-packages"></a>傳統型橋接器套件

[傳統型橋接器](desktop-to-uwp-root.md)可讓不同的設定在應用程式封裝 (appx) 內整合 Win32 二進位檔。 我們傳統型橋接器的演進想像成有四個關鍵步驟的旅程。 

- **步驟 1 - 轉換**︰封裝現有 Win32 二進位檔，不變更程式碼或僅做最低限度變更。
- **步驟 2 - 增強**︰在現有應用程式中加入一些基本 UWP 功能 (例如動態磚)，藉由從現有的 Win32 程式碼中參考 Windows.winmd。
- **步驟 3 - 延伸**︰在現有應用程式中納入進階的 UWP 功能 (例如背景工作)。 如果您是使用受管理語言 (如 C# 或 VB.Net) 建置 UWP 和 Win32 元件，產生的封裝將擁有混合的二進位檔，需要小心處理才能保證 .NET Native 處理正確無誤。 
- **步驟 4 - 移轉**︰您已將 UI 移轉至現代化 XAML 和 C#/VB.NET，但仍有舊版 Win32 程式碼。 現在進入點是 UWP.NET 可執行檔，但您的封裝中仍有使用某些 Win32 API 的二進位檔。

下一個表格將摘要說明四個步驟中，每一個步驟對您的應用程式的差異。 

| 步驟 | 二進位檔 | EntryPoint | .NET Native | F5 偵錯 |
|---|---|---|---|---|
| 1 (轉換) | Win32 | Win32 | 不適用 | VS 延伸模組 |
| 2 (增強) | 參考 WinMD | Win32 | 不適用 | VS 延伸模組 |
| 3 (延伸) | Win32 + CoreCLR (*) | Win32 | 依使用者 (**) | VS 延伸模組 |
| 4 (移轉)    | CoreCLR (*) + Win32 | UWP | 依使用者 (**) | VS |
| 5 (UWP) | CoreCLR | UWP |依市集 | VS |

(*)[CoreCLR](https://github.com/dotnet/coreclr) 是指以受管理語言 (C#/VB.NET) 撰寫的 UWP 元件倚賴的 .NET Core 執行階段。 這些元件也會需要 .NET Native 處理。

(**) 在步驟 3 和 4 中，使用者應在發行至市集之前處理 CoreCLR 組件，以產生 .NET Native 二進位檔和對應的符號。

## <a name="configure-your-visual-studio-solution"></a>設定您的 Visual Studio 方案

Visual Studio 包含您要設定應用程式封裝所需的工具，例如資訊清單編輯器和封裝建立精靈。 若要使用這些工具，您需要 UWP 專案，用來做為應用程式的 appx 容器。 雖然您可以使用任何 UWP 專案 (包括 C#、VB.NET、C++ 或 JavaScript)，但是 C#、VB.NET 和 C++ 專案有一些已知問題 (請參閱本文件稍後的[已知問題](#known-issues-anchor)一節)，所以這個範例中我們將使用 JavaScript。 

如果您要在 appx 應用程式模型內容中對您的應用程式進行偵錯，將會需要新增另一個專案來啟用 F5 appx 偵錯。 如需詳細資訊，請參閱[對傳統型橋接器應用程式進行偵錯](#debugging-anchor)。

讓我們開始進行旅程中的步驟 1。

### <a name="step-1-convert"></a>步驟 1︰轉換

這個步驟說明如何從現有的 Win32 專案建立傳統型橋接器應用程式。 在這個範例中，我們將使用基本的 WinForm 專案執行登錄上的讀取和寫入作業。

![](images/desktop-to-uwp/net-1.png)

#### <a name="add-the-uwp-project"></a>新增 UWP 專案 

若要建立傳統型橋接器套件，新增 JavaScript UWP 專案至相同解決方案中。

> 注意：即使我們使用 JavaScript UWP 範本，但仍不會撰寫任何 JavaScript 程式碼。 我們只使用專案做為工具。

![](images/desktop-to-uwp/net-2.png)

#### <a name="add-the-win32-binaries-to-the-win32-folder"></a>新增 Win32 二進位檔至 win32 資料夾

所有 Win32 二進位檔都將儲存在 UWP 專案中，稱為 win32 的資料夾中 (不過並不需要這個完全相同的名稱；您可以使用任何想要的名稱)。

如果您使用 Visual Studio，可以自動化專案，在每個組建之後複製檔案，藉此改善您的開發工作流程。 編輯您的專案檔 (在此範例中為 .csproj) 以納入 AfterBuild 目標，將所有 Win32 輸出檔複製到 UWP 專案中的 win32 資料夾，如下所示： 

```xml
  <Target Name="AfterBuild">
    <PropertyGroup>
      <TargetUWP>..\MyDesktopApp.Package\win32\</TargetUWP>
    </PropertyGroup>
     <ItemGroup>
       <Win32Binaries Include="$(TargetDir)\*" />
     </ItemGroup>
    <Copy SourceFiles="@(Win32Binaries)" DestinationFolder="$(TargetUWP)" />
  </Target>
```

如果您使用其他工具製作 Win32 二進位檔，只要在執行階段將所有必要的檔案複製到 win32 資料夾即可。 

#### <a name="edit-the-app-manifest-to-enable-the-desktop-bridge-extensions"></a>編輯應用程式資訊清單，啟用傳統型橋接器擴充功能

此範本包括 package.appxmanifest，您可以用來新增傳統型橋接器擴充功能。 若要編輯此檔案，以滑鼠右鍵按一下並選取 [檢視程式碼]，然後新增或修改這些項目： 

- `<Package xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10" xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10" xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities" IgnorableNamespaces="uap rescap">`

- `<TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.14393.0" MaxVersionTested="10.0.14393.0" />`

- `<rescap:Capability Name="runFullTrust" />`

- `<Application Id="MyDesktopAppStep1" Executable="win32\MyDesktopApp.exe" EntryPoint="Windows.FullTrustApplication">`

以下是完整的資訊清單檔案範例： 

```xml
<?xml version="1.0" encoding="utf-8"?>
<Package xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
        xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"

        xmlns:mp="http://schemas.microsoft.com/appx/2014/phone/manifest"
        xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"
        IgnorableNamespaces="uap rescap mp">
  <Identity Name="MyDesktopAppStep1"
            ProcessorArchitecture="x64"
            Publisher="CN=Microsoft Corporation, O=Microsoft Corporation, L=Redmond, S=Washington, C=US"
            Version="1.0.0.5" />
  <mp:PhoneIdentity PhoneProductId="6f6600a4-6da1-4d91-b493-35808d01f8de" PhonePublisherId="00000000-0000-0000-0000-000000000000" />
  <Properties>
    <DisplayName>MyDesktopAppStep1</DisplayName>
    <PublisherDisplayName>CN=Test</PublisherDisplayName>
    <Logo>Assets\SampleAppx.150x150.png</Logo>
  </Properties>
  <Resources>
    <Resource Language="en-us" />
  </Resources>
  <Dependencies>
    <TargetDeviceFamily Name="Windows.Desktop" 
                        MinVersion="10.0.14393.0" 
                        MaxVersionTested="10.0.14393.0" />
  </Dependencies>
  <Capabilities>
    <rescap:Capability Name="runFullTrust" />
  </Capabilities>
  <Applications>
    <Application Id="MyDesktopAppStep1" 
                 Executable="win32\MyDesktopApp.exe" 
                 EntryPoint="Windows.FullTrustApplication">
      <uap:VisualElements DisplayName="MyDesktopAppStep1" 
                          Description="MyDesktopAppStep1" 
                          BackgroundColor="#777777" 
                          Square150x150Logo="Assets\SampleAppx.150x150.png" 
                          Square44x44Logo="Assets\SampleAppx.44x44.png">
      </uap:VisualElements>
    </Application>
  </Applications>
</Package>
```

#### <a name="configure-the-win32-binaries"></a>設定 Win32 二進位檔

若要在輸出套件中包含應用程式所需的二進位檔，請在 Visual studio 中選取每個檔案。 將其屬性設定為 [內容]，並將其組建行為設定為 [較新時複製]。 

![](images/desktop-to-uwp/net-3.png)

如果您想要避免對原始程式碼儲存機制認可二進位檔，可以使用 .gitignore 檔將 win32 資料夾中的所有檔案排除。 

#### <a name="optional-use-wildcards-to-specify-the-files-in-your-win32-folder"></a>選用：使用萬用字元指定 win32 資料夾中的檔案

如果您的 Win32 應用程式需要數個檔案，可以編輯專案檔來指定萬用字元，根據萬用字元運算式指定哪些檔案要標示為 [內容]。 您需要使用文字編輯器 開啟 .jsproj 檔，並加入您要的檔案，如下所示：

```xml
<Content Include="win32\*.dll">
  <CopyToOutputDirectory>PreserveNewest</CopyToOutputDirectory>
</Content>
<Content Include="win32\*.exe">
  <CopyToOutputDirectory>PreserveNewest</CopyToOutputDirectory>
</Content>
<Content Include="win32\*.config">
  <CopyToOutputDirectory>PreserveNewest</CopyToOutputDirectory>
</Content>
<Content Include="win32\*.pdb">
  <CopyToOutputDirectory>PreserveNewest</CopyToOutputDirectory>
</Content>
```

### <a name="step-2-enhance"></a>步驟 2︰增強

如果您想要呼叫 Win32 程式碼中提供的 UWP API，則需要新增 `\Program Files (x86)\Windows Kits\10\UnionMetadata\Windows.winmd` 的參考。 可供您的應用程式使用的完整 UWP API 清單於此文件中列出：[使用傳統型橋接器轉換的應用程式支援的 UWP API](desktop-to-uwp-supported-api.md)。  

由於 Windows 10 中不需要此檔案，因此您不需要散發它。 在參考屬性中，將 [複製本機] 屬性設定為 false。

![](images/desktop-to-uwp/net-4.png)

若要新增 Win32 二進位檔，請使用步驟 1 中的相同指示。 

### <a name="step-3-extend"></a>步驟 3︰延伸 

如這個範例所示，我們將以背景工作延伸 Win32 應用程式。 這需要在 UWP 應用程式的 package.appxmanifest 中註冊背景工作，以及新增實作背景工作的專案參考，如下所示。

```xml
<Extensions>
  <Extension Category="windows.backgroundTasks" 
              EntryPoint="BackgroundTasks.MyBackgroundTask">
    <BackgroundTasks>
      <Task Type="timer" />
    </BackgroundTasks>
  </Extension>
</Extensions>
```

如果背景工作是透過 C# 或 VB.NET 實作，產生的輸出將包含 CoreCLR 二進位檔，需要藉由 .NET Native 工具鏈處理，才能提交至市集，如步驟 3 和 4 中所述。 建立具有混合二進位檔的 appxupload。

### <a name="step-4-migrate"></a>步驟 4：移轉

此案例已有 C# UWP 進入點，所以不需要新增額外的 UWP 專案。 不過，您需要依照步驟 1 中所述的步驟加入及設定 Win32 二進位檔。

若要執行 Win32 處理序，請使用[**FullTrustProcessLauncher**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.FullTrustProcessLauncher) API。 您將需要新增桌面擴充功能和 *fullTrustProcess* 功能至應用程式的資訊清單中，才能使用 API，像這樣： 

```xml
..
xmlns:desktop=http://schemas.microsoft.com/appx/manifest/desktop/windows10
..
<desktop:Extension Category="windows.fullTrustProcess" 
                    Executable="win32\MyDesktopApp.exe" />
```

## <a name="generate-packages-for-your-desktop-bridge-app"></a>產生適用於傳統型橋接器應用程式的套件

一旦您依照上述指示完成操作後，應該就準備好使用 Visual Studio 產生套件，如[封裝 UWP 應用程式](..\packaging\packaging-uwp-apps.md)中所述。 

### <a name="steps-1-and-2-create-appxupload-with-win32-binaries"></a>步驟 1 和 2：建立含有 Win32 二進位檔的 appxupload

若要提交具有*fullTrust*功能的套件，您需要產生 appxupload 檔案，當中的 appxsym 檔包含每一個平台的符號，以及包含 appx 平台套件的套件組合。

在步驟 1 和 2 中，您的套件不包含任何 CoreCLR 二進位檔，因此您不必擔心要選擇哪一個平台。 選取 [中性] 和 [發行 (任何 CPU)]，如下圖所示。

![](images/desktop-to-uwp/net-5.png)

選取 [產生市集套件] 選項之後，精靈將會產生 appxupload 檔，並且可立即提交至市集。

### <a name="step-3-and-4-create-appxupload-with-mixed-binaries"></a>步驟 3 和 4︰建立含有混合二進位檔的 appxupload

您也應該建置發行的版本，並此情況下必須指定要做為目標的平台，因為 .NET Native 需要它才能產生每個平台的原生二進位檔。

![](images/desktop-to-uwp/net-6.png)

為了建立新的 appxupload 檔案，我們將建立新的 zip 封存來加入 _Test 資料夾中產生的 appxsym 和 appxbundle。

建立新的 zip 檔案，當中包含 appxsym 和 appxbundle 檔案，然後將副檔名重新命名為 appxupload。

![](images/desktop-to-uwp/net-7.png)

<span id="debugging-anchor" />
## <a name="debugging-your-desktop-bridge-app"></a>對傳統型橋接器應用程式進行偵錯

雖然您可以從 Visual Studio 啟動專案而不進行偵錯 (Ctrl + F5)，但有一個已知問題指出 Visual Studio 無法自動連接至執行中的處理序。 不過，您稍後可以使用下列其中一種連接方法進行連接：

### <a name="attach-to-the-running-app"></a>連接至執行中的應用程式

#### <a name="attach-to-an-existing-process"></a>連接至現有處理序

使用 Ctrl + F5 成功啟動應用程式後，您就可以連接至 Win32 處理序；不過，您將無法對 .NET Native 模組進行偵錯。 

![](images/desktop-to-uwp/net-8.png)

#### <a name="attach-to-an-installed-app"></a>連接至已安裝的應用程式

您也可以使用 \[偵錯\] -> \[其他偵錯目標\] -> \[偵錯已安裝的應用程式套件\] 選項來連接至任何現有的 Appx 套件。

![](images/desktop-to-uwp/net-9.png)

您可以選取本機電腦，或連接至遠端電腦。

![](images/desktop-to-uwp/net-10.png)

使用這個選項就應該能偵錯 .NET Native 程式碼。

### <a name="use-visual-studio-extension-to-debug-your-desktop-bridge-app"></a>使用 Visual Studio 擴充功能偵錯傳統型橋接器應用程式 

如果您偏好使用 F5 偵錯應用程式，則需要從 Visual Studio 組件庫安裝 Visual Studio 2017 擴充功能[傳統型橋接器偵錯專案](https://marketplace.visualstudio.com/items?itemName=VisualStudioProductTeam.DesktoptoUWPPackagingProject)。

這個專案可讓您使用 Visual Studio 偵錯移轉至 UWP 的任何 Win32 應用程式 (如本文件所述)，或是使用傳統型應用程式轉換器。

#### <a name="add-the-debugging-project-to-your-solution"></a>新增偵錯專案至您的解決方案

若要開始進行，將新的傳統型橋接器偵錯專案新增至您的專案和解決方案。

![](images/desktop-to-uwp/net-11.png)

若要設定這個專案，您需要在每個要用於偵錯的設定或平台的屬性視窗中定義 PackageLayout 屬性。
若要對 Debug/x86 進行設定，我們會將封裝配置屬性設定至 UWP 專案的 bin\x86\debug 資料夾，使用相對路徑：`..\MyDesktopApp.Package\bin\x86\Debug`。 

![](images/desktop-to-uwp/net-12.png)

然後編輯 AppXFileLayout.xml 檔案來指定您的進入點：

```xml
<?xml version="1.0" encoding="utf-8"?>
<Project ToolsVersion="14.0" 
         xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
  <PropertyGroup>
    <MyProjectOutputPath>$(PackageLayout)</MyProjectOutputPath>
  </PropertyGroup>
  <ItemGroup>
    <LayoutFile Include="$(MyProjectOutputPath)\win32\MyDesktopApp.exe">
      <PackagePath>$(PackageLayout)\win32\MyDesktopApp.exe</PackagePath>
    </LayoutFile>
  </ItemGroup>
</Project>
```

最後，您應該設定解決方案相依性，確認專案依照正確的順序建置。 

例如，讓我們檢閱步驟 3 中建立的解決方案。

![](images/desktop-to-uwp/net-13.png)

若要設定建置順序，您可以使用專案相依性設定。 以滑鼠右鍵按一下方案，然後選取 \[專案相依性\] 選項。 設定正確的相依性之後，您可以驗證建置順序，如下所示 (適用步驟 3)：

![](images/desktop-to-uwp/net-14.png)

<span id="known-issues-anchor" />
## <a name="known-issues-with-cvbnet-and-c-uwp-projects"></a>C#/VB.NET 和 C++ UWP 專案的已知問題

如果您偏好使用 C# 專案封裝您的應用程式，則須注意下列已知問題。 

- **建置偵錯中的應用程式會導致錯誤：Microsoft.Net.CoreRuntime.targets(235,5)：錯誤：不支援使用自訂進入點可執行檔的應用程式。 請檢查套件資訊清單中，應用程式項目的可執行檔屬性。** 解決方法是改用發行模式。

- **發行版本中會移除 UWP 專案根資料夾中儲存的 Win32 二進位檔**。 如果您不使用資料夾儲存您的 Win32 二進位檔，.NET Native 編譯器將會從最終套件中移除這些檔案，導致資訊清單驗證錯誤，因為找不到可執行檔進入點。


