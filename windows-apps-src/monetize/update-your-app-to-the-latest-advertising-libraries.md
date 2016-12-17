---
author: mcleanbyron
description: "了解如何將您的 App 更新成使用最新支援的 Microsoft Advertising 程式庫，並確定您的 App 會繼續接收橫幅廣告。"
title: "將您的 App 更新到最新的 Microsoft Advertising 程式庫"
translationtype: Human Translation
ms.sourcegitcommit: 9bd83a41ea4ec4ec7a75ef89e9c92f73d86cc731
ms.openlocfilehash: 710a5f4a3ae566550939fe783af7e97d20dd1b28


---

# 將您的 App 更新到最新的 Microsoft Advertising 程式庫

從 2017 年 1 月開始，我們將不再為使用 Microsoft 舊版廣告 SDK 版本的 App 提供橫幅廣告。 如果您現有的 App (已經在「市集」中或仍在開發中) 使用 **AdControl** 或 **AdMediatorControl** 來顯示橫幅廣告，您可能需要將您的 App 更新成使用最新的廣告 SDK，您的 App 才能繼續在 2017 年 1 月接收橫幅廣告。 請依照本文中的指示來判斷您的 App 是否受到這項變更影響，以及了解如何更新您的 App (如有必要)。

如果您的 App 受到這項變更影響，而您未將 App 更新成使用最新的廣告 SDK，從 2017 年 1 月開始，您將會看到下列行為：

* 將不會再為您 App 中的任何 **AdControl** 或 **AdMediatorControl** 控制項提供橫幅廣告，您將無法再從這些控制項賺取廣告收入。

* 當您 App 中的 **AdControl** 或 **AdMediatorControl** 要求新的廣告時，將會引發控制項的 **ErrorOccurred** 事件，而事件引數的 **ErrorCode** 屬性值將會是 **NoAdAvailable**。

為了提供有關這項變更的一些額外內容，我們將針對不支援一組最基本功能 (包括透過來自美國互動廣告局 (Interactive Advertising Bureau, IAB) 的[行動多媒體廣告介面定義 (Mobile Rich-media Ad Interface Definitions, MRAID) 1.0 規格](http://www.iab.com/wp-content/uploads/2015/08/IAB_MRAID_VersionOne.pdf)提供 HTML5 多媒體的功能) 的舊版廣告 SDK 版本移除支援。 許多我們的廣告商都尋求這些功能，而我們正在進行這項變更來協助讓我們的 App 生態體系對廣告商更具吸引力，並最終為您賺取更多收入。

如果您遭遇任何問題或需要協助，請[聯絡支援服務](http://go.microsoft.com/fwlink/?LinkId=393643)。

>**注意**
              如果您先前將 App 更新成使用 [Microsoft Store Services SDK](http://aka.ms/store-services-sdk) (適用於 UWP app) 或[適用於 Windows 和 Windows Phone 8.x 的 Microsoft Advertising SDK](http://aka.ms/store-8-sdk) (適用於 Windows 8.1 和 Windows Phone 8.x App)，即表示您的 App 已經使用最新的可用廣告 SDK，您不需要對您的 App 進行任何進一步的變更。

## 先決條件

* 適用於使用 **AdControl** 或 **AdMediatorControl** 之 App 的完整原始程式碼和 Visual Studio 專案檔。

* App 的 .appx 或 .xap 套件。

  >**注意**
              如果您已經沒有 App 的 .appx 或 .xap 套件，但仍然有開發電腦，其中具備用來建置 App 的 Visual Studio 和廣告 SDK 版本，您就可以在 Visual Studio 中重新產生 .appx 或 .xap 套件。

<span id="part-1" />
## 第 1 部分︰判斷是否需要更新您的 App

請依照下列各節中的指示來判斷是否需要更新您的 App。

### 您的 App 使用 AdControl

如果您的 App 使用 **AdControl** 來顯示橫幅廣告，請依照下列指示操作。

**適用於 Windows 10 的 UWP app**

1. 建立您 App 的 .appx 套件複本以免影響到正本，將複本重新命名，使其副檔名為 .zip，然後將該檔案的內容解壓縮。

2. 檢查您 App 套件的解壓縮內容：

  * 如果您看到 Microsoft.Advertising.dll 檔案，即表示您的 App 使用舊 SDK，您必須依照下列各節中的指示來更新您的專案。 繼續進行[第 2 部分](update-your-app-to-the-latest-advertising-libraries.md#part-2)。

  * 如果您沒看到 Microsoft.Advertising.dll 檔案，即表示您的 UWP app 已經使用最新的可用廣告 SDK，您不需要對您的專案進行任何變更。

<span/>

**Windows 8.1 或 Windows Phone 8.x App**

1. 建立您 App 的 .appx 或 .xap 套件複本以免影響到正本，將複本重新命名，使其副檔名為 .zip，然後將該檔案的內容解壓縮。

2. 針對 XAML 或 JavaScript/HTML App，檢查您 App 套件的解壓縮內容：

  * 如果您看到 Microsoft.Advertising.winmd 檔案，但未看到 UniversalXamlAdControl.\*.dll 檔案 (適用於 XAML App) 或 UniversalSharedLibrary.Windows.dll 檔案 (適用於 JavaScript/HTML App)，即表示您的 App 使用舊 SDK，您必須依照下列各節中的指示來更新您的專案。 繼續進行[第 2 部分](update-your-app-to-the-latest-advertising-libraries.md#part-2)。

  * 否則，繼續進行接下來的步驟。

2. 開啟 Windows PowerShell，輸入下列命令，然後將 ```-Path``` 引數指派至您 App 套件之解壓縮內容的完整路徑。 此命令會顯示您專案所參考的所有廣告程式庫，以及每個程式庫的版本。

    ```
    get-childitem -Path "<path to your extracted package>" * -Recurse -include *advert*.dll,*admediator*.dll,*xamladcontrol*.dll,*universalsharedlibrary*.dll | where-object {$_.Name -notlike "*resources*" -and $_.Name -notlike "*design*" } | foreach-object { "{0}`t{1}" -f $_.FullName, [System.Diagnostics.FileVersionInfo]::GetVersionInfo($_).FileVersion }
    ```
2. 找出下表中針對您 App 的目標平台列出的檔案，然後將該檔案的版本與此表格中列出的版本做比較。

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

**Windows 8.0 App**

* 從 2017 年 1 月開始，將不再為以 Windows 8.0 為目標的 App 提供橫幅廣告。 為了避免失去廣告曝光機會，建議您將專案轉換成以 Windows 10 為目標的 UWP app。 大多數 Windows 8.0 App 流量現在都是在搭載 Windows 10 的裝置上執行。

<span/>

**Windows Phone 7.x App**

* 從 2017 年 1 月開始，將不再為以 Windows Phone 7.x 為目標的 App 提供橫幅廣告。 為了避免失去廣告曝光機會，建議您將專案轉換成以 Windows Phone 8.1 app 為目標，或是轉換成以 Windows 10 為目標的 UWP app。 大多數 Windows 7.x App 流量現在都是在搭載 Windows Phone 8.1 或 Windows 10 的裝置上執行。

<span/>

### 您的 App 使用 AdMediatorControl

如果您的 App 使用 **AdMediatorControl** 來顯示橫幅廣告，請依照下列指示來判斷是否需要更新您的 App。

**適用於 Windows 10 的 UWP app**

* 針對 UWP app 已不再支援 **AdMediatorControl**。 您必須依照下列各節中的指示來移轉成使用 **AdControl**。 繼續進行[第 2 部分](update-your-app-to-the-latest-advertising-libraries.md#part-2)。

<span/>

**Windows 8.1 或 Windows Phone 8.1 App**

1. 建立您 App 的 .appx 或 .xap 套件複本以免影響到正本，將複本重新命名，使其副檔名為 .zip，然後將該檔案的內容解壓縮。

2. 開啟 Windows PowerShell，輸入下列命令，然後將 ```-Path``` 引數指派至您 App 套件之解壓縮內容的完整路徑。 此命令會顯示您專案所參考的所有廣告程式庫，以及每個程式庫的版本。

    ```
    get-childitem -Path "<path to your extracted package>" * -Recurse -include *advert*.dll,*admediator*.dll,*xamladcontrol*.dll,*universalsharedlibrary*.dll | where-object {$_.Name -notlike "*resources*" -and $_.Name -notlike "*design*" } | foreach-object { "{0}`t{1}" -f $_.FullName, [System.Diagnostics.FileVersionInfo]::GetVersionInfo($_).FileVersion }
    ```

2. 如果輸出中所列的 Microsoft.AdMediator.\*.dll 檔案版本是 2.0.1603.18005 或更新版本，您就不需要對您的專案進行任何變更。

  如果這些檔案的版本號碼較低，您就必須依照下列各節中的指示來更新您的專案。 繼續進行[第 2 部分](update-your-app-to-the-latest-advertising-libraries.md#part-2)。

<span id="part-2" />
## 第 2 部分：安裝最新的 SDK

如果您的 App 使用舊的 SDK 版本，請依照下列指示來確定您開發電腦上的 SDK 是最新的。

1. 確定您的開發電腦已安裝 Visual Studio 2015 (適用於UWP、Windows 8.1 或 Windows Phone 8.x 專案) 或 Visual Studio 2013 (適用於 Windows 8.1 或 Windows Phone 8.x 專案)。

  >**注意**
              如果 Visual Studio 已在您的開發電腦上開啟，請先將它關閉，再執行下列步驟。

1.  將您開發電腦上的所有舊版 Microsoft Advertising SDK 和 Ad Mediator SDK 解除安裝。

2.  開啟 \[命令提示字元\] 視窗，然後執行下列命令，以清除可能已與 Visual Studio 一起安裝但未出現在電腦上已安裝程式清單中的任何 SDK 版本：

  ```
  MsiExec.exe /x{5C87A4DB-31C7-465E-9356-71B485B69EC8}
  MsiExec.exe /x{6AB13C21-C3EC-46E1-8009-6FD5EBEE515B}
  MsiExec.exe /x{6AC81125-8485-463D-9352-3F35A2508C11}
  ```

3.  為您的 App 安裝最新的 SDK：
  * 針對 Windows 10 上的 UWP app，請安裝 [Microsoft Store Services SDK](http://aka.ms/store-services-sdk)。
  * 針對以舊版 OS 為目標 App，請安裝[適用於 Windows 和 Windows Phone 8.x 的 Microsoft Advertising SDK](http://aka.ms/store-8-sdk)。

## 第 3 部分：更新您的專案

請依照下列指示來更新您的專案。

### 適用於 Windows 10 的 UWP 專案

<span/>

如果您的 App 使用 **AdMediatorControl**，請改為[將您的 App 重構成使用 AdControl](migrate-from-admediatorcontrol-to-adcontrol.md)。 針對 UWP app 已不再支援 **AdMediatorControl**。

如果您的 App 使用 **AdControl**，請從專案中移除所有現有的 Microsoft Advertising 程式庫參考，然後依照 [AdControl in XAML](adcontrol-in-xaml-and--net.md) 或 [AdControl in HTML](adcontrol-in-html-5-and-javascript.md) 指示來新增必要的參考。 這將確保您的專案使用正確的程式庫。 您可以保留現有的 XAML 標記和程式碼。

<span/>

### Windows 8.1 或 Windows Phone 8.1 (XAML 或 JavaScript/HTML) 專案

<span/>

1. 從您的專案中移除所有 Microsoft.Advertising.\* 和 Microsoft.AdMediator.\* 參考。 如果您使用的是通用專案範本，您可能有兩種參考 (一種用於 Windows，一種用於 Windows Phone)。

2. 如果您的 App 使用 **AdMediatorControl**，請依照[新增和使用廣告流量分配控制項](https://msdn.microsoft.com/library/windows/apps/xaml/dn864355.aspx)中的指示，將程式庫參考加回。 如果您的 App 使用 **AdControl**，請依照 [XAML 中的 AdControl](adcontrol-in-xaml-and--net.md) 或 [HTML 中的 AdControl](adcontrol-in-html-5-and-javascript.md) 中的指示，將程式庫參考加回。

<span/>

請注意下列事項：

* 如果您的 App 先前已編譯成適用於「任何 CPU」平台，您將必須把專案重新編譯成適用於架構特定平台 (x86、x64 或 ARM)。

* 如果您有 Windows Phone 8.x XAML App，而此 App 先前使用了 **AdControl** 類別是定義在 **Microsoft.Advertising.Mobile.UI** 命名空間中的 SDK 版本，您將必須把程式碼更新成參考 **Microsoft.Advertising.WinRT.UI** 命名空間中的 **AdControl** 類別 (此類別在較新的 SDK 版本中已移動命名空間)。

* 除了先前的問題以外，您可以保留現有的 XAML 標記和程式碼。

<span/>

### Windows Phone 8.x Silverlight 專案

<span/>

1. 從您的專案中移除所有 Microsoft.Advertising.\* 和 Microsoft.AdMediator.\* 參考。

2. 如果您的 App 使用 **AdMediatorControl**，請依照[新增和使用廣告流量分配控制項](https://msdn.microsoft.com/library/windows/apps/xaml/dn864355.aspx)中的指示，將程式庫參考加回。 如果您的 App 使用 **AdControl**，請依照 [Windows Phone Silverlight 中的 AdControl](adcontrol-in-windows-phone-silverlight.md) 中的指示，將程式庫參考加回。

<span/>

請注意下列事項：

* 您可以保留現有的 XAML 標記和程式碼。

* 從 \[方案總管\]，檢查您專案中 **Microsoft.Advertising.Mobile.UI** 參考的屬性。 如果您 App 的目標是 Windows Phone 8.0，它就應該是版本 6.2.40501.0，如果您的 App 目標是 Windows Phone 8.1，則它應該是 8.1.50112.0。

* 針對 Windows Phone 8.x Silverlight App，不支援在模擬器上測試實際執行單元。 建議您在裝置上測試。

## 第 4 部分：測試並重新發佈您的 App

請測試您的 App 以確定它如預期般顯示橫幅廣告。

如果「市集」中已經有您的舊版 App，請在「Windows 開發人員中心」儀表板中，為已更新的 App [建立新的提交項目](https://msdns.microsoft.com/windows/uwp/publish/app-submissions)來重新發佈您的 App。





 



<!--HONumber=Nov16_HO1-->


