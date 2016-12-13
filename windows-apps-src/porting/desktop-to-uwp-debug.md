---
author: awkoren
Description: "使用傳統型轉 UWP 橋接器，部署從 Windows 傳統型應用程式 (Win32、WPF 及 Windows Forms) 轉換而來的通用 Windows 平台 (UWP) App 及針對這些 App 進行偵錯。"
Search.Product: eADQiWindows 10XVcnh
title: "針對使用傳統型橋接器轉換的 App 進行偵錯"
translationtype: Human Translation
ms.sourcegitcommit: dba00371b29b3179a6dc3bdd96a092437331e61a
ms.openlocfilehash: 537ac8e83d5f54bf83ec0e05b71be354651000f2

---

# <a name="debug-apps-converted-with-the-desktop-bridge"></a>針對使用傳統型橋接器轉換的 App 進行偵錯

本主題包含的資訊可協助您在使用傳統型轉 UWP 橋接器轉換 App 之後順利偵錯該 App。 您有數個選項可用來偵錯已轉換的 App。

## <a name="attach-to-process"></a>附加至處理序

當您「以系統管理員身分」執行 Microsoft Visual Studio 時，將可針對已轉換的 App 專案使用「開始偵錯」和「啟動但不偵錯」命令，但是已啟動的 App 將會以[中度整合層級](https://msdn.microsoft.com/library/bb625963)來執行 (也就是將不會有提升的權限)。 若要為啟動的 App 授與系統管理員權限，首先您必須透過捷徑或磚「以系統管理員身分」來啟動。 當應用程式正在執行時，從「以系統管理員身分」執行的 Microsoft Visual Studio 執行個體中叫用__附加至處理序__，然後從對話方塊中選取應用程式的處理序。

## <a name="f5-debug"></a>F5 偵錯

Visual Studio 現在支援新的封裝專案。 新的專案可讓您將在建置應用程式時所做的任何更新，自動複製到應用程式安裝程式上轉換器所建立的 AppX 套件。 在設定封裝專案之後，您現在也可以使用 F5，直接對 AppX 套件進行偵錯。 

>注意︰您也可以使用選項來對現有的 Appx 套件進行偵錯，方法是使用選項 \[偵錯\] -&gt; \[其他偵錯目標\] -&gt; \[偵錯已安裝的應用程式套件\]。

以下是如何開始使用的方式： 

1. 首先，確認您已設定為使用 Desktop App Converter。 如需相關指示，請參閱 [Desktop App Converter 預覽](desktop-to-uwp-run-desktop-app-converter.md)。

2. 針對您的 Win32 應用程式依序執行轉換器和安裝程式。 轉換器會擷取配置以及對登錄所做的任何變更，並輸出含有資訊清單與 registery.dat 的 Appx 來將登錄虛擬化︰

![alt](images/desktop-to-uwp/debug-1.png)

3. 安裝和啟動 [Visual Studio 2017 RC](https://www.visualstudio.com/downloads/#visual-studio-community-2017-rc)。 

4. 從 [Visual Studio 組件庫](http://go.microsoft.com/fwlink/?LinkId=797871)安裝 Desktop to UWP Packaging VSIX 專案。 

5. 開啟已在 Visual Studio 中轉換的對應 Win32 方案。
 
6. 在方案上按一下滑鼠右鍵，然後選擇 [加入新的專案]，將新的封裝專案加入至您的方案。 接著在 [安裝和部署] 下方選取 [Desktop to UWP Packaging Project]：

    ![alt](images/desktop-to-uwp/debug-2.png)

    產生的專案將加入至您的方案︰

    ![alt](images/desktop-to-uwp/debug-3.png)

    在封裝專案中，AppXFileList 會在 AppX 配置中提供檔案對應。 參考一開始是空的，但是應該手動設定為 .exe 專案，以做為建置順序。 

7. DesktopToUWPPackaging 專案有一個屬性頁面，可讓您設定 AppX 套件根目錄以及要執行哪些磚︰

    ![alt](images/desktop-to-uwp/debug-4.png)

    將 PackageLayout 設定為由轉換器所建立之 AppX 的根位置 (上圖)。 然後選取要執行的磚。

8.  開啟並編輯 AppXFileList.xml。 這個檔案會定義如何將 Win32 偵錯組建的輸出複製到轉換器建置的 AppX 配置。 根據預設，我們在檔案中有預留位置來使用範例標記與註解︰

    ```XML
    <?xml version="1.0" encoding="utf-8"?>
    <Project ToolsVersion="4.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
      <ItemGroup>
    <!— Use the following syntax to copy debug output to the AppX layout
       <AppxPackagedFile Include="$(outdir)\App.exe">
          <PackagePath>App.exe</PackagePath>
        </AppxPackagedFile> 
        See http://etc...
    -->
      </ItemGroup>
    </Project>
    ```

    以下是建立對應的範例。 在此案例中，我們會將 .exe 和 .dll 從 Win32 組建位置複製到套件配置位置。 

    ```XML
    <?xml version="1.0" encoding=utf-8"?>
    <Project ToolsVersion=14.0" xmlns="http://scehmas.microsoft.com/developer/msbuild/2003">
        <PropertyGroup>
            <MyProjectOutputPath>{relativepath}</MyProjectOutputPath>
        </PropertyGroup>
        <ItemGroup>
            <LayoutFile Include="$(MyProjectOutputPath)\ProjectTracker.exe">
                <PackagePath>$(PackageLayout)\VFS\Program Files (x86)\Contoso Software\Project Tracker\ProjectTracker.exe</PackagePath>
            </LayoutFile>
            <LayoutFile Include="$(MyProjectOutputPath)\ProjectTracker.Models.dll">
                <PackagePath>$(PackageLayout)\VFS\Program Files (x86)\Contoso Software\Project Tracker\ProjectTracker.Models.dll</PackagePath>
            </LayoutFile>
        </ItemGroup>
    </Project>
    ```

    檔案的定義如下： 

    首先，我們定義 *MyProjectOutputPath* 指向建置 Win32 專案的位置︰

    ```XML
    <?xml version="1.0" encoding="utf-8"?>
    <Project ToolsVersion="14.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
        <PropertyGroup>
            <MyProjectOutputPath>..\ProjectTracker\bin\DesktopUWP</MyProjectOutputPath>
        </PropertyGroup>
    ```

    接著，每個 *LayoutFile* 會指定要從 Win32 組建位置複製到 Appx 套件配置的檔案。 在這個案例中，會依序複製 .exe 和 .dll。 

    ```XML
        <ItemGroup>
            <LayoutFile Include="$(MyProjectOutputPath)\ProjectTracker.exe">
                <PackagePath>$(PackageLayout)\VFS\Program Files (x86)\Contoso Software\Project Tracker\ProjectTracker.exe</PackagePath>
            </LayoutFile>
            <LayoutFile Include="$(MyProjectOutputPath)\ProjectTracker.Models.dll">
                <PackagePath>$(PackageLayout)\VFS\Program Files (x86)\Contoso Software\Project Tracker\ProjectTracker.Models.dll</PackagePath>
            </LayoutFile>
        </ItemGroup>
    </Project>
    ```

9. 將封裝專案設定為啟始專案。 這會將 Win32 檔案複製到 AppX，然後在建置並執行專案時啟動偵錯工具。  

    ![alt](images/desktop-to-uwp/debug-5.png)

10. 最後，您現在可以在 Win32 程式碼中設定中斷點，並按 F5 來啟動偵錯工具。 它會複製您在 AppX 套件中對 Win32 應用程式所做的任何更新，並可讓您直接從 Visual Studio 偵錯。

11. 如果您更新應用程式，將需要使用 MakeAppX，再次重新封裝您的應用程式。 如需詳細資訊，請參閱[應用程式封裝工具 (MakeAppx.exe)](https://msdn.microsoft.com/library/windows/desktop/hh446767(v=vs.85).aspx)。 

如果您有多個組建組態 (例如針對發行和偵錯)，就可以將下列項目新增至 AppXFileList.xml 檔案，從不同位置複製 Win32 組建︰

```XML
<PropertyGroup>
    <MyProjectOutputPath Condition="$(Configuration) == 'DesktopUWP'">C:\Users\peterfar\Desktop\ProjectTracker\ProjectTracker\bin\DesktopUWP>
    </MyProjectOutputPath>
    <MyProjectOutputPath Condition="$(Configuration) == 'ReleaseDesktopUWP'"> C:\Users\peterfar\Desktop\ProjectTracker\ProjectTracker\bin\ReleaseDesktopUWP</MyProjectOutputPath>
</PropertyGroup>
```

如果您將應用程式更新到 UWP，但仍想要建置它以使其適用於 Win32，您也可以使用條件式編譯來啟用特定的程式碼路徑。 

1.  在下列範例中，程式碼將只會針對 DesktopUWP 進行編譯，而且將使用 WinRT API 來顯示磚。 

    ```C#
    [Conditional("DesktopUWP")]
    private void showtile()
    {
        XmlDocument tileXml = TileUpdateManager.GetTemplateContent(TileTemplateType.TileSquare150x150Text01);
        XmlNodeList textNodes = tileXml.GetElementsByTagName("text");
        textNodes[0].InnerText = string.Format("Welcome to DesktopUWP!");
        TileNotification tileNotification = new TileNotification(tileXml);
        TileUpdateManager.CreateTileUpdaterForApplication().Update(tileNotification);
    }
    ```

2.  您可以使用 Configuration Manager 來新增新的組建組態︰

    ![alt](images/desktop-to-uwp/debug-6.png)

    ![alt](images/desktop-to-uwp/debug-7.png)

3.  然後在專案屬性下方，新增對條件式編譯符號的支援︰

    ![alt](images/desktop-to-uwp/debug-8.png)

4.  如果您想要將建置目標設為您新增的 UWP API，您現在可以將組建目標切換為 DesktopUWP。

## <a name="plmdebug"></a>PLMDebug 

如果您要在 app 執行時對其進行偵錯，Visual Studio F5 和附加至處理序相當實用。 不過，在某些情況下，您可能想對偵錯處理序進行更細微的控制，包括能夠在啟動 app 之前偵錯。 針對這些更進階的情況，請使用 [**PLMDebug**](https://msdn.microsoft.com/library/windows/hardware/jj680085(v=vs.85).aspx)。 此工具可讓您使用 Windows 偵錯工具對已轉換的應用程式進行偵錯，並提供應用程式生命週期的完全控制權，包括暫停、繼續及終止。 

PLMDebug 隨附於 Windows SDK。 如需詳細資訊，請參閱 [**PLMDebug**](https://msdn.microsoft.com/library/windows/hardware/jj680085(v=vs.85).aspx)。 

## <a name="run-another-process-inside-the-full-trust-container"></a>在完全信任的容器內執行另一個處理序 

您可以在指定應用程式套件的容器內叫用自訂處理序。 這對於測試案例非常有用 (例如，如果您擁有自訂的測試載入器，而且想要測試 app 的輸出)。 若要這樣做，請使用 ```Invoke-CommandInDesktopPackage``` PowerShell Cmdlet： 

```CMD
Invoke-CommandInDesktopPackage [-PackageFamilyName] <string> [-AppId] <string> [-Command] <string> [[-Args]
    <string>]  [<CommonParameters>]
```



<!--HONumber=Dec16_HO1-->


