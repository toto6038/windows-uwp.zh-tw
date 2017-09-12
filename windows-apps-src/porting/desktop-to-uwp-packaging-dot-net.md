---
author: normesta
Description: "本指南說明如何設定您的 Visual Studio 方案來編輯、偵錯和封裝適用於傳統型橋接器的傳統型應用程式。"
Search.Product: eADQiWindows 10XVcnh
title: "使用 Visual Studio 封裝應用程式 (傳統型橋接器)"
ms.author: normesta
ms.date: 07/20/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp
ms.assetid: 807a99a7-d285-46e7-af6a-7214da908907
ms.openlocfilehash: d8919448b965f18ff7f8fdaeda325889e495ef85
ms.sourcegitcommit: f6dd9568eafa10ee5cb2b849c0d82d84a1c5fb93
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/02/2017
---
# <a name="package-an-app-by-using-visual-studio-desktop-bridge"></a>使用 Visual Studio 封裝應用程式 (傳統型橋接器)

您可以使用 Visual Studio 為您的傳統型應用程式產生套件。 然後，您可以將套件發行到 Windows 市集或將其側載至一部以上的電腦。

本指南顯示如何設定方案，然後為您的傳統型應用程式產生套件。

## <a name="first-consider-how-youll-distribute-your-app"></a>首先，請先考慮您將會如何散布您的應用程式

若您打算將您的應用程式發行至 [Windows 市集](https://www.microsoft.com/store/apps)，請先從填寫[此表單](https://developer.microsoft.com/windows/projects/campaigns/desktop-bridge)開始。 Microsoft 將會與您連絡以啟動上架程序。 作為這個程序的一部分，您將會在市集中保留一個名稱，並取得您需要用來封裝您應用程式的資訊。

## <a name="add-a-packaging-project-to-your-solution"></a>新增封裝專案至您的方案

1. 在 Visual Studio 中，開啟包含您的傳統型應用程式專案的方案。

2. 新增 JavaScript **[空白應用程式 (通用 Windows)\]** 專案到您的方案。

   您不需要增加任何程式碼至此專案。 它會自行為您產生套件。 我們將此專案稱為「封裝專案」。

   ![javascript UWP 專案](images/desktop-to-uwp/javascript-uwp-project.png)

   >[!IMPORTANT]
   >一般而言，您應該使用此專案的 JavaScript 版本。  C#、VB.NET 以及 C++ 版本有一些問題，但如果您想要使用這些版本，在進行之前請先參閱[已知問題](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-known-issues#known-issues-anchor)指南。

## <a name="add-the-desktop-application-binaries-to-the-packaging-project"></a>將傳統型應用程式二進位檔新增至封裝專案

將二進位檔直接新增至封裝專案。

1. 在 **\[方案總管\]** 中，展開封裝專案資料夾、建立子資料夾，並為其命名 (例如 **win32**)。

2. 在子資料夾上按一下滑鼠右鍵，然後選擇 **\[加入現有項目\]**。

3. 在 **\[加入現有項目\]** 對話方塊中，找到傳統型應用程式的輸出資料夾並從中新增檔案。 這不只包括可執行檔，也包括位於該資料夾中的任何 dll 或 .config 檔案。

   ![參考可執行檔](images/desktop-to-uwp/cpp-exe-reference.png)

   每當您變更您的傳統型應用程式專案，您都必須將這些檔案的新版本複製到封裝專案。 您可以在新增建置後事件至封裝專案的專案檔案，來自動化此程序。 這裡提供一個範例。

   ```XML
   <Target Name="PostBuildEvent">
     <Copy SourceFiles="..\MyWindowsFormsApplication\bin\Debug\MyWindowsFormsApplication.exe"
       DestinationFolder="win32" />
     <Copy SourceFiles="..\MyWindowsFormsApplication\bin\Debug\MyWindowsFormsApplication.exe.config"
       DestinationFolder="win32" />
     <Copy SourceFiles="..\MyWindowsFormsApplication\bin\Debug\MyWindowsFormsApplication.pdb"
       DestinationFolder="win32" />
     <Copy SourceFiles="..\MyWindowsFormsApplication\bin\Debug\MyBusinessLogicLibrary.dll"
       DestinationFolder="win32" />
     <Copy SourceFiles="..\MyWindowsFormsApplication\bin\Debug\MyBusinessLogicLibrary.pdb"
       DestinationFolder="win32" />
   </Target>
   ```

## <a name="modify-the-package-manifest"></a>修改套件資訊清單

封裝專案包含描述套件設定的檔案。 根據預設，此檔案描述的是 UWP 應用程式，因此您必須加以修改，讓系統了解您的套件包含在完全信任模式下執行的傳統型應用程式。  

1. 在 **\[方案總管\]** 中，展開封裝專案，以滑鼠右鍵按一下 **package.appxmanifest** 檔，然後選取 **\[檢視程式碼\]**。

   ![參考 dotnet 專案](images/desktop-to-uwp/reference-dotnet-project.png)

2. 將此命名空間新增到檔案頂端，以及新增命名空間前置詞到 ``IgnorableNamespaces`` 的清單。

   ```XML
   xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"
   ```
   完成後，您的命名空間宣告看起來會像這樣：

   ```XML
   <Package
     xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
     xmlns:mp="http://schemas.microsoft.com/appx/2014/phone/manifest"
     xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
     xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"
     IgnorableNamespaces="uap mp rescap">
   ```

3. 尋找 ``TargetDeviceFamily`` 元素，並將 ``Name`` 屬性設定為 **Windows.Desktop**、將 ``MinVersion`` 屬性設定為封裝專案的最小版本，以及將 ``MaxVersionTested`` 設定為封裝專案的目標版本。

   ```XML
   <TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.10586.0" MaxVersionTested="10.0.15063.0" />
   ```

   您可以在封裝專案的屬性頁面中找到最小版本及目標版本。

   ![最小版本和目標版本設定](images/desktop-to-uwp/min-target-version-settings.png)


4. 從 ``Application`` 元素移除 ``StartPage`` 屬性。 接著新增 ``Executable`` 與 ``EntryPoint`` 屬性。

   ``Application`` 元素看起來會像下面這樣。

   ```XML
   <Application Id="App"  Executable=" " EntryPoint=" ">
   ```

5. 將 ``Executable`` 屬性設定為傳統型應用程式可執行檔的名稱。 然後，將 ``EntryPoint`` 屬性設定為 **Windows.FullTrustApplication**。

   ``Application`` 元素看起來會像下面這樣。

   ```XML
   <Application Id="App"  Executable="win32\MyWindowsFormsApplication.exe" EntryPoint="Windows.FullTrustApplication">
   ```
6. 將 ``runFullTrust`` 功能新增到 ``Capabilities`` 元素。

   ```XML
     <rescap:Capability Name="runFullTrust"/>
   ```
   藍色曲線標記可能會出現在此宣告底下，但您可以放心地忽略。

   >[!IMPORTANT]
   如果您建立的是 C++ 傳統型應用程式的套件，則必須對資訊清單檔案進行一些額外變更，以便可以和您的應用程式一起部署 Visual C++ 執行階段。 請參閱[在傳統型橋接器專案中使用 Visual C++ 執行階段](https://blogs.msdn.microsoft.com/vcblog/2016/07/07/using-visual-c-runtime-in-centennial-project/)。

7. 組建封裝專案，以確保未出現任何錯誤。

8. 如果您想要測試您的套件，請參閱[執行、偵錯以及測試封裝的傳統型橋接器應用程式 (傳統型橋接器)](desktop-to-uwp-debug.md)。

   然後，返回本指南，並參閱下一節來產生您的套件。

## <a name="generate-a-package"></a>產生套件

若要產生封裝應用程式，請依照本主題中所述的指導方針進行：[封裝 UWP 應用程式](..\packaging\packaging-uwp-apps.md)。

到達 **\[選取並設定套件\]** 畫面時，在選取核取方塊之前，請花點時間考慮您要在套件中加入哪種二進位檔。

* 如果您已經透過新增 C#、C++ 或 VB.NET 型通用 Windows 平台專案到您的方案來[擴充](desktop-to-uwp-extend.md)您的傳統型應用程式，請選取 **\[x86\]** 和 **\[x64\]** 核取方塊。  

* 否則，請選擇 **\[中性\]** 核取方塊。

>[!NOTE]
您必須明確選擇每個受支援平台的原因是，因為您已經擴充的方案包含兩種類型的二進位檔，一個用於 UWP 專案，另一個用於傳統型專案。 這些是不同類型的二進位檔，.NET Native 需要為每個平台明確製作原生二進位檔。

如果您在嘗試產生套件時收到錯誤，請參閱[已知問題](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-known-issues#known-issues-anchor)指南，而如果您的問題未出現在清單中，請到[此處](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge)分享您的問題給我們知道。

## <a name="next-steps"></a>後續步驟

**執行您的應用程式/找出並修正問題**

請參閱[執行、偵錯以及測試封裝的傳統型橋接器應用程式 (傳統型橋接器)](desktop-to-uwp-debug.md)

**透過新增 UWP API 來增強您的傳統型應用程式**

請參閱[增強您的 Windows 10 傳統型應用程式](desktop-to-uwp-enhance.md)

**透過新增 UWP 元件來擴充您的傳統型應用程式**

請參閱[使用現代化 UWP 元件擴充您的傳統型應用程式](desktop-to-uwp-extend.md)。

**散布您的應用程式**

請參閱[散佈封裝的傳統型應用程式 (傳統型橋接器)](desktop-to-uwp-distribute.md)

**尋找特定問題的解答**

我們的團隊會監視這些 [StackOverflow 標記](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge)。

**提供有關本文的意見反應**

使用下方的留言區塊。
