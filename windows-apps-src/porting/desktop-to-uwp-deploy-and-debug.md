---
author: awkoren
Description: "使用桌面轉換擴充功能，部署和偵錯從 Windows 傳統型應用程式 (Win32、WPF 及 Windows Forms) 轉換而來的通用 Windows 平台 (UWP) 應用程式。"
Search.Product: eADQiWindows 10XVcnh
title: "部署和偵錯從 Windows 傳統型應用程式轉換而來的通用 Windows 平台 (UWP) App"
translationtype: Human Translation
ms.sourcegitcommit: 2c1a8ea38081c947f90ea835447a617c388aec08
ms.openlocfilehash: 75e176f17845bdbd618c6ca63fbbb5765bef54fb

---

# 部署和偵錯已轉換的 UWP App

\[正式發行前可能會進行大幅度修改之發行前版本產品的一些相關資訊。 Microsoft 對此處提供的資訊，不提供任何明確或隱含的瑕疵擔保。\]

本主題包含的資訊可協助您在轉換應用程式之後順利部署和偵錯該應用程式。 此外，如果您想探究桌面轉換擴充功能的一些本質，則本主題非常適合您。

## 偵錯已轉換的 UWP App

您有數個選項可用來偵錯已轉換的應用程式。

### 附加至處理序

當您「以系統管理員身分」執行 Microsoft Visual Studio 時，將可針對已轉換的應用程式專案使用__開始偵錯__和__啟動但不偵錯__命令，但是已啟動的應用程式將會以[中級整合](https://msdn.microsoft.com/library/bb625963)來執行。 也就是說，它將不__具提高的權限。 若要為啟動的應用程式授與系統管理員權限，首先您必須透過捷徑或磚「以系統管理員身分」來啟動。 當應用程式正在執行時，從「以系統管理員身分」執行的 Microsoft Visual Studio 執行個體中叫用__附加至處理序__，然後從對話方塊中選取應用程式的處理序。

### F5 偵錯

Visual Studio 現在支援新的封裝專案，可讓您將在建置應用程式時所做的任何更新，自動複製到在應用程式安裝程式上執行轉換器時所建立的 AppX 套件。 在設定封裝專案之後，您現在也可以使用 F5，直接對 AppX 套件進行偵錯。 

以下是如何開始使用的方式： 

1. 首先，確認您已設定為使用 Desktop App Converter。 如需相關指示，請參閱 [Desktop App Converter 預覽](https://msdn.microsoft.com/windows/uwp/porting/desktop-to-uwp-run-desktop-app-converter)。 

2. 針對您的 Win32 應用程式依序執行轉換器和安裝程式。 轉換器會擷取配置以及對登錄所做的任何變更，並輸出含有資訊清單與 registery.dat 的 Appx 來將登錄虛擬化︰

![alt](images/desktop-to-uwp/debug-1.png)

3. 安裝和啟動 [Visual Studio "15" Preview 2](https://www.visualstudio.com/downloads/visual-studio-next-downloads-vs.aspx)。 

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
            <MyProjectOutputPath>C:\{path}</MyProjectOutputPath>
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
            <MyProjectOutputPath>C:\Users\peterfar\Desktop\ProjectTracker\ProjectTracker\bin\DesktopUWP</MyProjectOutputPath>
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

### PLMDebug 

如果您要在 app 執行時對其進行偵錯，Visual Studio F5 和附加至處理序相當實用。 不過，在某些情況下，您可能想對偵錯處理序進行更細微的控制，包括能夠在啟動 app 之前偵錯。 針對這些更進階的情況，請使用 [**PLMDebug**](https://msdn.microsoft.com/library/windows/hardware/jj680085%28v=vs.85%29.aspx?f=255&MSPPError=-2147217396)。 此工具可讓您使用 Windows 偵錯工具對已轉換的應用程式進行偵錯，並提供應用程式生命週期的完全控制權，包括暫停、繼續及終止。 

PLMDebug 隨附於 Windows SDK。 如需詳細資訊，請參閱 [**PLMDebug**](https://msdn.microsoft.com/library/windows/hardware/jj680085%28v=vs.85%29.aspx?f=255&MSPPError=-2147217396)。 

### 在完全信任的容器內執行另一個處理序 

您可以在指定應用程式套件的容器內叫用自訂處理序。 這對於測試案例非常有用 (例如，如果您擁有自訂的測試載入器，而且想要測試 app 的輸出)。 若要這樣做，請使用 ```Invoke-CommandInDesktopPackage``` PowerShell Cmdlet： 

```CMD
Invoke-CommandInDesktopPackage [-PackageFamilyName] <string> [-AppId] <string> [-Command] <string> [[-Args]
    <string>]  [<CommonParameters>]
```

## 部署已轉換的 UWP App

有 2 種方式可用來部署已轉換的應用程式：鬆散檔案註冊和部署 appx 套件。 

若檔案配置於您可輕鬆存取與更新之位置中的磁碟上，且不需要簽署或憑證，則鬆散檔案註冊非常適合基於偵錯用途來使用。  

Appx 套件部署提供一種簡單的方式可在多部電腦上部署和側載應用程式，但需要簽署套件，並在電腦上信任憑證。

### 鬆散檔案註冊

若要在開發期間部署您的 app，請執行下列 PowerShell Cmdlet： 

```Add-AppxPackage –Register AppxManifest.xml```

若要更新應用程式的 .exe 或.dll 檔案，只需使用新檔案來取代套件中現有的檔案、提高 AppxManifest.xml 的版本號碼，然後再次執行上述命令即可。

請注意下列事項： 

* 您必須將已轉換應用程式安裝所在的任意磁碟機格式設定為 NTFS 格式。

* 已轉換的應用程式一律會以互動式使用者身分執行。

### Appx 套件部署 

部署您的 app 之前，您必須以憑證簽署它。 如需建立憑證的詳細資訊，請參閱[簽署您的 .Appx 套件](https://msdn.microsoft.com/windows/uwp/porting/desktop-to-uwp-run-desktop-app-converter#deploy-your-converted-appx)。 

以下是如何匯入您先前建立之憑證的方式。 您可以使用 CERTUTIL 直接安裝憑證，或者可以從您已簽署的 appx 中安裝它，就像客戶會做的一樣。 

若要透過 CERTUTIL 安裝憑證，請從系統管理員的命令提示字元執行下列命令：

```cmd
Certutil -addStore TrustedPeople <testcert.cer>
```

從 Appx 匯入憑證，就像客戶會做的一樣：

1.  在 [檔案總管] 中，以滑鼠右鍵按一下您使用測試憑證簽署的 appx，然後從操作功能表選擇 [屬性]****。
2.  按一下或點選 [數位簽章]**** 索引標籤。
3.  按一下或點選憑證，然後選擇 [詳細資料]****。
4.  按一下或點選 [檢視憑證]****。
5.  按一下或點選 [安裝憑證]****。
6.  在 [存放區位置]**** 群組中，選取 [本機電腦]****。
7.  按一下或點選 [下一步]**** 和 [確定]****，以確認 UAC 對話方塊。
8.  在 [憑證匯入精靈] 的下一個畫面中，將選取的選項變更為 [將所有憑證放入以下的存放區]****。
9.  按一下或點選 [瀏覽]****。 在 [選取憑證存放區] 視窗中，向下捲動並選取 [受信任的人]****，然後按一下或點選 [確定]****。
10. 按一下或點選 [下一步]****。 新畫面隨即顯示。 按一下或點選 [完成]****。
11. 應該會顯示確認對話方塊。 出現時，按一下 [確定]****。 如果出現其他對話方塊，指出憑證發生問題，您可能需要執行一些憑證疑難排解。

注意：若要使 Windows 信任憑證，憑證必須位於 [憑證 (本機電腦)] &gt; [信任的根憑證授權單位] &gt; [憑證]**** 節點或 [憑證 (本機電腦)] &gt; [受信任的人] &gt; [憑證]**** 節點上。 只有這兩個位置中的憑證可以驗證本機電腦內容中的憑證信任。 否則，會出現類似下列字串的錯誤訊息︰
```CMD
"Add-AppxPackage : Deployment failed with HRESULT: 0x800B0109, A certificate chain processed,
but terminated in a rootcertificate which is not trusted by the trust provider.
(Exception from HRESULT: 0x800B0109) error 0x800B0109: The root certificate of the signature
in the app package must be trusted."
```

現已信任憑證，您有 2 種方式可以用來安裝套件 – 透過 PowerShell 或只要按兩下 appx 套件檔案來安裝。  若要透過 PowerShell 安裝，請執行下列 Cmdlet：

```powershell
Add-AppxPackage <MyApp>.appx
```

## 幕後作業

當您執行已轉換的應用程式時，您的 UWP 應用程式套件會從 \Program Files\WindowsApps\&lt;_套件名稱_&gt;\\&lt;_appname_&gt;.exe 啟動。 如果您查看該處，即會看見應用程式具有一份應用程式套件資訊清單 (名為 AppxManifest.xml)，其參考針對已轉換應用程式所使用的特殊 xml 命名空間。 該資訊清單檔案內部是一個 __&lt;EntryPoint&gt;__ 項目，它會參考完全信任的應用程式。 啟動該應用程式時，它不會在應用程式容器內執行，而是改為以使用者身分執行，就如其平常所做的一樣。

但應用程式會在特殊環境中執行，其中應用程式對於檔案系統和登錄所做的任何存取都會重新導向。 名為 Registry.dat 的檔案就是用來進行登錄重新導向。 它是真正的登錄區，因此您可以在 Windows 登錄編輯程式 (Regedit) 中檢視它。 請注意，此機制表示您無法使用登錄來進行處理序間通訊。 在任何情況下，登錄都不是設計來且不適合用於該做法。 當它隨附於檔案系統時，唯一要重新導向的就是 AppData 資料夾，而它會被重新導向到針對所有 UWP 應用程式儲存應用程式資料的相同位置。 這個位置稱為本機應用程式資料存放區，而您可以使用 [ApplicationData.LocalFolder](https://msdn.microsoft.com/library/windows/apps/br241621) 屬性加以存取。 因此，已經在正確位置中植入您的程式碼來讀取和寫入應用程式資料，而您不需要做任何動作。 此外，您也可以直接寫入該處 檔案系統重新導向的其中一個好處是更簡潔的解除安裝體驗。

在名為 VFS 的資料夾內，您將會看到包含您應用程式具有相依性之 DLL 的資料夾。 這些 DLL 會安裝到適用於您應用程式傳統桌面版本的系統資料夾。 但是，做為 UWP 應用程式，DLL 會在您應用程式的本機上。 如此一來，在安裝和解除安裝 UWP app 時，就不會發生任何版本問題。

### 已封裝的 VFS 位置

下表會顯示其中做為套件一部分傳送的檔案會在 app 適用的系統上重疊。 您的 app 將會察覺到這些檔案位於所列出的系統位置中，但事實上它們是在 [Package Root]\VFS\ 內部重新導向的位置中。 FOLDERID 位置是來自 [**KNOWNFOLDERID**](https://msdn.microsoft.com/en-us/library/windows/desktop/dd378457.aspx) 常數。

系統位置 | 重新導向的位置 (位於 [PackageRoot]\VFS\ 下方) | 有效的架構
 :---- | :---- | :---
FOLDERID_SystemX86 | SystemX86 | x86、amd64 
FOLDERID_System | SystemX64 | amd64 
FOLDERID_ProgramFilesX86 | ProgramFilesX86 | x86、amd6 
FOLDERID_ProgramFilesX64 | ProgramFilesX64 | amd64 
FOLDERID_ProgramFilesCommonX86 | ProgramFilesCommonX86 | x86、amd64
FOLDERID_ProgramFilesCommonX64 | ProgramFilesCommonX64 | amd64 
FOLDERID_Windows | Windows | x86、amd64 
FOLDERID_ProgramData | 常見的 AppData | x86、amd64 
FOLDERID_System\catroot | AppVSystem32Catroot | x86、amd64 
FOLDERID_System\catroot2 | AppVSystem32Catroot2 | x86、amd64 
FOLDERID_System\drivers\etc | AppVSystem32DriversEtc | x86、amd64 
FOLDERID_System\driverstore | AppVSystem32Driverstore | x86、amd64 
FOLDERID_System\logfiles | AppVSystem32Logfiles | x86、amd64 
FOLDERID_System\spool | AppVSystem32Spool | x86、amd64 

## 另請參閱
[將您的傳統型應用程式轉換為通用 Windows 平台 (UWP) app](https://msdn.microsoft.com/windows/uwp/porting/desktop-to-uwp-root)

[Desktop App Converter 預覽](https://msdn.microsoft.com/windows/uwp/porting/desktop-to-uwp-run-desktop-app-converter)

[將您的 Windows 傳統型應用程式手動轉換為通用 Windows 平台 (UWP) app](https://msdn.microsoft.com/windows/uwp/porting/desktop-to-uwp-manual-conversion)

[GitHub 上的傳統型應用程式橋接至 UWP 的程式碼範例](https://github.com/Microsoft/DesktopBridgeToUWP-Samples)


<!--HONumber=Sep16_HO2-->


