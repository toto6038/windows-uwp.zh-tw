---
author: mcleanbyron
description: "了解如何將您的應用程式更新成使用最新支援的 Microsoft Advertising 程式庫，並確定您的應用程式會繼續接收橫幅廣告。"
title: "將您的應用程式更新到最新的 Microsoft Advertising 程式庫"
translationtype: Human Translation
ms.sourcegitcommit: 2b5dbf872dd7aad48373f6a6df3dffbcbaee8090
ms.openlocfilehash: 5333c3f8ab834a4646c63499565ef28a634f850d


---

# <a name="update-your-app-to-the-latest-microsoft-advertising-libraries"></a>將您的應用程式更新到最新的 Microsoft Advertising 程式庫

從 2017 年 1 月開始，我們將不再為使用 Microsoft 舊版廣告 SDK 版本的應用程式提供橫幅廣告。 如果您現有的應用程式 (已經在「市集」中或仍在開發中) 使用 **AdControl** 或 **AdMediatorControl** 來顯示橫幅廣告，您可能需要將您的應用程式更新成使用最新的廣告 SDK，您的應用程式才能繼續在 2017 年 1 月接收橫幅廣告。 請依照本文中的指示來判斷您的應用程式是否受到這項變更影響，以及了解如何更新您的應用程式 (如有必要)。

如果您的應用程式受到這項變更影響，而您未將應用程式更新成使用最新的廣告 SDK，從 2017 年 1 月開始，您將會看到下列行為：

* 將不會再為您應用程式中的任何 **AdControl** 或 **AdMediatorControl** 控制項提供橫幅廣告，您將無法再從這些控制項賺取廣告收入。

* 當您應用程式中的 **AdControl** 或 **AdMediatorControl** 要求新的廣告時，將會引發控制項的 **ErrorOccurred** 事件，而事件引數的 **ErrorCode** 屬性值將會是 **NoAdAvailable**。

為了提供有關這項變更的一些額外內容，我們將針對不支援一組最基本功能 (包括透過來自美國互動廣告局 (Interactive Advertising Bureau, IAB) 的[行動多媒體廣告介面定義 (Mobile Rich-media Ad Interface Definitions, MRAID) 1.0 規格](http://www.iab.com/wp-content/uploads/2015/08/IAB_MRAID_VersionOne.pdf)提供 HTML5 多媒體的功能) 的舊版廣告 SDK 版本移除支援。 許多我們的廣告商都尋求這些功能，而我們正在進行這項變更來協助讓我們的應用程式生態體系對廣告商更具吸引力，並最終為您賺取更多收入。

如果您遭遇任何問題或需要協助，請[聯絡支援服務](http://go.microsoft.com/fwlink/?LinkId=393643)。

>**備註**&nbsp;&nbsp;若您先前已有將應用程式更新為使用 [Microsoft Store Services SDK](http://aka.ms/store-services-sdk) (適用於 UWP 應用程式) 或 [適用於 Windows 及 Windows Phone 8.x 的 Microsoft Advertising SDK](http://aka.ms/store-8-sdk) (適用於 Windows 8.1 及 Windows Phone 8.x 應用程式)，則您的應用程式已在使用最新的可用廣告 SDK，且您無須對應用程式作更進一步的變更。

## <a name="prerequisites"></a>先決條件

* 適用於使用 **AdControl** 或 **AdMediatorControl** 之應用程式的完整原始程式碼和 Visual Studio 專案檔。

* 應用程式的 .appx 或 .xap 套件。

  >**備註**&nbsp;&nbsp;若您不再擁有應用程式的 .appx 或 .xap 套件，但開發電腦仍具備用來建置應用程式的 Visual Studio 版本及廣告 SDK，則您可在 Visual Studio 中重新產生 .appx 或 .xap 套件。

<span id="part-1" />
## <a name="part-1-determine-whether-you-need-to-update-your-app"></a>第 1 部分︰判斷是否需要更新您的應用程式

請依照下列各節中的指示來判斷是否需要更新您的應用程式。

### <a name="your-app-uses-adcontrol"></a>您的應用程式使用 AdControl

如果您的應用程式使用 **AdControl** 來顯示橫幅廣告，請依照下列指示操作。

**適用於 Windows 10 的 UWP 應用程式**

1. 建立您應用程式的 .appx 套件複本以免影響到正本，將複本重新命名，使其副檔名為 .zip，然後將該檔案的內容解壓縮。

2. 檢查您應用程式套件的解壓縮內容：

  * 如果您看到 Microsoft.Advertising.dll 檔案，即表示您的應用程式使用舊 SDK，您必須依照下列各節中的指示來更新您的專案。 繼續進行[第 2 部分](update-your-app-to-the-latest-advertising-libraries.md#part-2)。

  * 如果您沒看到 Microsoft.Advertising.dll 檔案，即表示您的 UWP 應用程式已經使用最新的可用廣告 SDK，您不需要對您的專案進行任何變更。

<span/>

**Windows 8.1 或 Windows Phone 8.x 應用程式**

1. 建立您應用程式的 .appx 或 .xap 套件複本以免影響到正本，將複本重新命名，使其副檔名為 .zip，然後將該檔案的內容解壓縮。

2. 針對 XAML 或 JavaScript/HTML 應用程式，檢查您應用程式套件的解壓縮內容：

  * 如果您看到 Microsoft.Advertising.winmd 檔案，但未看到 UniversalXamlAdControl.\*.dll 檔案 (適用於 XAML 應用程式) 或 UniversalSharedLibrary.Windows.dll 檔案 (適用於 JavaScript/HTML 應用程式)，即表示您的應用程式使用舊的 SDK，您必須依照下列各節中的指示來更新您的專案。 繼續進行[第 2 部分](update-your-app-to-the-latest-advertising-libraries.md#part-2)。

  * 否則，繼續進行接下來的步驟。

2. 開啟 Windows PowerShell，輸入下列命令，然後將 ```-Path``` 引數指派至您應用程式套件之解壓縮內容的完整路徑。 此命令會顯示您專案所參考的所有廣告程式庫，以及每個程式庫的版本。

  > [!div class="tabbedCodeSnippets"]
  ```syntax
  get-childitem -Path "<path to your extracted package>" * -Recurse -include *advert*.dll,*admediator*.dll,*xamladcontrol*.dll,*universalsharedlibrary*.dll | where-object {$_.Name -notlike "*resources*" -and $_.Name -notlike "*design*" } | foreach-object { "{0}`t{1}" -f $_.FullName, [System.Diagnostics.FileVersionInfo]::GetVersionInfo($_).FileVersion }
  ```

2. 找出下表中針對您應用程式的目標平台列出的檔案，然後將該檔案的版本與此表格中列出的版本做比較。

  <table>
    <colgroup>
      <col width="33%" />
      <col width="33%" />
      <col width="33%" />
    </colgroup>
    <thead>
      <tr class="header">
        <th align="left">目標平台</th>
        <th align="left">檔案</th>
        <th align="left">版本</th>
      </tr>
    </thead>
    <tbody>
      <tr class="odd">
        <td align="left"><p>Windows 8.1 XAML</p></td>
        <td align="left"><p>UniversalXamlAdControl.Windows.dll</p></td>
        <td align="left"><p>8.5.1601.07018</p></td>
      </tr>
      <tr class="odd">
        <td align="left"><p>Windows Phone 8.1 XAML</p></td>
        <td align="left"><p>UniversalXamlAdControl.WindowsPhone.dll</p></td>
        <td align="left"><p>8.5.1601.07018</p></td>
      </tr>
      <tr class="odd">
        <td align="left"><p>Windows 8.1 JavaScript/HTML<br/>Windows Phone 8.1 JavaScript/HTML</p></td>
        <td align="left"><p>UniversalSharedLibrary.Windows.dll</p></td>
        <td align="left"><p>8.5.1601.07018</p></td>
      </tr>
      <tr class="odd">
        <td align="left"><p>Windows Phone 8.1 Silverlight</p></td>
        <td align="left"><p>Microsoft.Advertising.\*.dll</p></td>
        <td align="left"><p>8.1.50112.0</p></td>
      </tr>
      <tr class="odd">
        <td align="left"><p>Windows Phone 8.0 Silverlight</p></td>
        <td align="left"><p>Microsoft.Advertising.\*.dll</p></td>
        <td align="left"><p>6.2.40501.0</p></td>
      </tr>
    </tbody>
  </table>

3. 如果該檔案的版本等於或高於上表中所列的版本，您就不需要對您的專案進行任何變更。

  如果該檔案的版本號碼較低，您就必須依照下列各節中的指示來更新您的專案。 繼續進行[第 2 部分](update-your-app-to-the-latest-advertising-libraries.md#part-2)。

<span/>

**Windows 8.0 應用程式**

* 從 2017 年 1 月開始，將不再為以 Windows 8.0 為目標的應用程式提供橫幅廣告。 為了避免失去廣告曝光機會，建議您將專案轉換成以 Windows 10 為目標的 UWP 應用程式。 大多數 Windows 8.0 應用程式流量現在都是在搭載 Windows 10 的裝置上執行。

<span/>

**Windows Phone 7.x 應用程式**

* 從 2017 年 1 月開始，將不再為以 Windows Phone 7.x 為目標的應用程式提供橫幅廣告。 為了避免失去廣告曝光機會，建議您將專案轉換成以 Windows Phone 8.1 應用程式為目標，或是轉換成以 Windows 10 為目標的 UWP 應用程式。 大多數 Windows 7.x 應用程式流量現在都是在搭載 Windows Phone 8.1 或 Windows 10 的裝置上執行。

<span/>

### <a name="your-app-uses-admediatorcontrol"></a>您的應用程式使用 AdMediatorControl

如果您的應用程式使用 **AdMediatorControl** 來顯示橫幅廣告，請依照下列指示來判斷是否需要更新您的應用程式。

**適用於 Windows 10 的 UWP 應用程式**

* 針對 UWP 應用程式已不再支援 **AdMediatorControl**。 您必須依照下列各節中的指示來移轉成使用 **AdControl**。 繼續進行[第 2 部分](update-your-app-to-the-latest-advertising-libraries.md#part-2)。

<span/>

**Windows 8.1 或 Windows Phone 8.1 應用程式**

1. 建立您應用程式的 .appx 或 .xap 套件複本以免影響到正本，將複本重新命名，使其副檔名為 .zip，然後將該檔案的內容解壓縮。

2. 開啟 Windows PowerShell，輸入下列命令，然後將 ```-Path``` 引數指派至您應用程式套件之解壓縮內容的完整路徑。 此命令會顯示您專案所參考的所有廣告程式庫，以及每個程式庫的版本。

  > [!div class="tabbedCodeSnippets"]
  ```syntax
  get-childitem -Path "<path to your extracted package>" * -Recurse -include *advert*.dll,*admediator*.dll,*xamladcontrol*.dll,*universalsharedlibrary*.dll | where-object {$_.Name -notlike "*resources*" -and $_.Name -notlike "*design*" } | foreach-object { "{0}`t{1}" -f $_.FullName, [System.Diagnostics.FileVersionInfo]::GetVersionInfo($_).FileVersion }
  ```

2. 如果輸出中所列的 Microsoft.AdMediator.\*.dll 檔案版本是 2.0.1603.18005 或更新版本，您就不需要對您的專案進行任何變更。

  如果這些檔案的版本號碼較低，您就必須依照下列各節中的指示來更新您的專案。 繼續進行[第 2 部分](update-your-app-to-the-latest-advertising-libraries.md#part-2)。

<span id="part-2" />
## <a name="part-2-install-the-latest-sdk"></a>第 2 部分：安裝最新的 SDK

如果您的應用程式使用舊的 SDK 版本，請依照下列指示來確定您開發電腦上的 SDK 是最新的。

1. 確定您的開發電腦已安裝 Visual Studio 2015 (適用於UWP、Windows 8.1 或 Windows Phone 8.x 專案) 或 Visual Studio 2013 (適用於 Windows 8.1 或 Windows Phone 8.x 專案)。

  >**備註**&nbsp;&nbsp;若 Visual Studio 在您的開發電腦中為開啟狀態，請先將其關閉再執行下列步驟。

1.  將您開發電腦上的所有舊版 Microsoft Advertising SDK 和 Ad Mediator SDK 解除安裝。

2.  開啟 \[命令提示字元\] 視窗，然後執行下列命令，以清除可能已與 Visual Studio 一起安裝但未出現在電腦上已安裝程式清單中的任何 SDK 版本：

  > [!div class="tabbedCodeSnippets"]
  ```syntax
  MsiExec.exe /x{5C87A4DB-31C7-465E-9356-71B485B69EC8}
  MsiExec.exe /x{6AB13C21-C3EC-46E1-8009-6FD5EBEE515B}
  MsiExec.exe /x{6AC81125-8485-463D-9352-3F35A2508C11}
  ```

3.  為您的應用程式安裝最新的 SDK：
  * 針對 Windows 10 上的 UWP 應用程式，請安裝 [Microsoft Store Services SDK](http://aka.ms/store-services-sdk)。
  * 針對以舊版 OS 為目標應用程式，請安裝[適用於 Windows 和 Windows Phone 8.x 的 Microsoft Advertising SDK](http://aka.ms/store-8-sdk)。

## <a name="part-3-update-your-project"></a>第 3 部分：更新您的專案

請依照下列指示來更新您的專案。

### <a name="uwp-projects-for-windows-10"></a>適用於 Windows 10 的 UWP 專案

<span/>

如果您的應用程式使用 **AdMediatorControl**，請改為[將您的應用程式重構成使用 AdControl](migrate-from-admediatorcontrol-to-adcontrol.md)。 針對 UWP 應用程式已不再支援 **AdMediatorControl**。

如果您的應用程式使用 **AdControl**，請從專案中移除所有現有的 Microsoft Advertising 程式庫參考，然後依照 [AdControl in XAML](adcontrol-in-xaml-and--net.md) 或 [AdControl in HTML](adcontrol-in-html-5-and-javascript.md) 指示來新增必要的參考。 這將確保您的專案使用正確的程式庫。 您可以保留現有的 XAML 標記和程式碼。

<span/>

### <a name="windows-81-or-windows-phone-81-xaml-or-javascripthtml-projects"></a>Windows 8.1 或 Windows Phone 8.1 (XAML 或 JavaScript/HTML) 專案

<span/>

1. 從您的專案中移除所有 Microsoft.Advertising.\* 和 Microsoft.AdMediator.\* 參考。 如果您使用的是通用專案範本，您可能有兩種參考 (一種用於 Windows，一種用於 Windows Phone)。

2. 如果您的應用程式使用 **AdMediatorControl**，請依照[新增和使用廣告流量分配控制項](https://msdn.microsoft.com/library/windows/apps/xaml/dn864355.aspx)中的指示，將程式庫參考加回。 如果您的應用程式使用 **AdControl**，請依照 [XAML 中的 AdControl](adcontrol-in-xaml-and--net.md) 或 [HTML 中的 AdControl](adcontrol-in-html-5-and-javascript.md) 中的指示，將程式庫參考加回。

<span/>

請注意下列事項：

* 如果您的應用程式先前已編譯成適用於「任何 CPU」平台，您將必須把專案重新編譯成適用於架構特定平台 (x86、x64 或 ARM)。

* 如果您有 Windows Phone 8.x XAML 應用程式，而此應用程式先前使用了 **AdControl** 類別是定義在 **Microsoft.Advertising.Mobile.UI** 命名空間中的 SDK 版本，您將必須把程式碼更新成參考 **Microsoft.Advertising.WinRT.UI** 命名空間中的 **AdControl** 類別 (此類別在較新的 SDK 版本中已移動命名空間)。

* 除了先前的問題以外，您可以保留現有的 XAML 標記和程式碼。

<span/>

### <a name="windows-phone-8x-silverlight-projects"></a>Windows Phone 8.x Silverlight 專案

<span/>

1. 從您的專案中移除所有 Microsoft.Advertising.\* 和 Microsoft.AdMediator.\* 參考。

2. 如果您的應用程式使用 **AdMediatorControl**，請依照[新增和使用廣告流量分配控制項](https://msdn.microsoft.com/library/windows/apps/xaml/dn864355.aspx)中的指示，將程式庫參考加回。 如果您的應用程式使用 **AdControl**，請依照 [Windows Phone Silverlight 中的 AdControl](adcontrol-in-windows-phone-silverlight.md) 中的指示，將程式庫參考加回。

<span/>

請注意下列事項：

* 您可以保留現有的 XAML 標記和程式碼。

* 從 \[方案總管\]，檢查您專案中 **Microsoft.Advertising.Mobile.UI** 參考的屬性。 如果您應用程式的目標是 Windows Phone 8.0，它就應該是版本 6.2.40501.0，如果您的應用程式目標是 Windows Phone 8.1，則它應該是 8.1.50112.0。

* 針對 Windows Phone 8.x Silverlight 應用程式，不支援在模擬器上測試實際執行單元。 建議您在裝置上測試。

## <a name="part-4-test-and-republish-your-app"></a>第 4 部分：測試並重新發佈您的應用程式

請測試您的應用程式以確定它如預期般顯示橫幅廣告。

如果「市集」中已經有您的舊版應用程式，請在「Windows 開發人員中心」儀表板中，為已更新的應用程式[建立新的提交項目](https://msdns.microsoft.com/windows/uwp/publish/app-submissions)來重新發佈您的應用程式。





 



<!--HONumber=Dec16_HO2-->


