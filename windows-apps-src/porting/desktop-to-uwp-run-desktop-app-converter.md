---
author: normesta
Description: "執行 Desktop Converter App，封裝 Windows 傳統型應用程式 (例如，Win32、WPF 及 Windows Forms)。"
Search.Product: eADQiWindows 10XVcnh
title: "使用 Desktop App Converter 封裝應用程式 (傳統型橋接器)"
ms.author: normesta
ms.date: 05/25/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: 74c84eb6-4714-4e12-a658-09cb92b576e3
ms.openlocfilehash: 6aa469d0080412f48de511315684d365d4e90c99
ms.sourcegitcommit: 6c6f3c265498d7651fcc4081c04c41fafcbaa5e7
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/09/2017
---
# <a name="package-an-app-using-the-desktop-app-converter-desktop-bridge"></a>使用 Desktop App Converter 封裝應用程式 (傳統型橋接器)

[取得 Desktop App Converter](https://aka.ms/converter)

您可以使用 Desktop App Converter (DAC) 將您的傳統型應用程式移至通用 Windows 平台 (UWP)。 這包括 Win32 應用程式，以及您使用 .NET 4.6.1 建立的應用程式。

<div style="float: left; padding: 10px">
    ![DAC 圖示](images/desktop-to-uwp/dac.png)
</div>

雖然此工具的名稱中出現「Converter」(轉換器) 這個詞，但它並不會轉換您的應用程式。 您的應用程式會保持不變。 不過，此工具會產生含有套件識別資料並可呼叫大範圍 WinRT API 的 Windows 應用程式套件。

您可在開發電腦上使用 Add-AppxPackage PowerShell Cmdlet 安裝該套件。

這個轉換器會在隔離的 Windows 環境中，使用全新基礎映像 (當作轉換器下載的一部分來提供) 來執行桌面安裝程式。 它會擷取桌面安裝程式所製作的任何登錄與檔案系統 I/O，並將它重新封裝為輸出的一部分。

> [!NOTE]
> 請查看<a href="https://mva.microsoft.com/en-US/training-courses/developers-guide-to-the-desktop-bridge-17373?l=oZG0B1WhD_8406218965/">此系列</a>由 Microsoft Virtual Academy 發行的簡短影片。 這些影片會帶領您逐步操作並探索一些常見的 Desktop App Converter 使用方式。

## <a name="the-dac-does-more-than-just-generate-a-package-for-you"></a>DAC 不只是為您產生套件

以下是一些它也能為您做到的事情。

**Windows 10 Creators Update**

<div style="float: left; ">
    ![檢查](images/desktop-to-uwp/check.png)
</div>

自動註冊您的預覽處理常式、縮圖處理常式、屬性處理常式、防火牆規則、URL 旗標。

<div style="float: left; ">
    ![檢查](images/desktop-to-uwp/check.png)
</div>

自動註冊檔案類型對應，讓使用者可以在檔案總管中使用 **\[類型\]** 欄位群組檔案。

<div style="float: left; ">
    ![檢查](images/desktop-to-uwp/check.png)
</div>

註冊您的 COM 伺服器。

**Windows 10 年度更新版或以上版本**

<div style="float: left; ">
    ![檢查](images/desktop-to-uwp/check.png)
</div>

自動簽署您的套件，讓您可以測試您的應用程式。

<div style="float: left; ">
    ![檢查](images/desktop-to-uwp/check.png)
</div>

驗證您的應用程式是否違反傳統型橋接器和 Windows 市集的需求。

若要尋找選項的完整清單，請參閱本文的[參數](#command-reference)一節。

當您準備好要建立套件時，我們就可以開始進行操作。

## <a name="first-consider-how-youll-distribute-your-app"></a>首先，請先考慮您將會如何散布您的應用程式
若您打算將您的應用程式發行至 [Windows 市集](https://www.microsoft.com/store/apps)，請先從填寫[此表單](https://developer.microsoft.com/windows/projects/campaigns/desktop-bridge)開始。 Microsoft 將會與您連絡以啟動上架程序。 作為這個程序的一部分，您將會在市集中保留一個名稱，並取得您需要用來封裝您應用程式的資訊。

## <a name="make-sure-that-your-system-can-run-the-converter"></a>請確定您的系統可以執行轉換器

請確定您的系統符合下列需求︰

* Windows10 年度更新版 (10.0.14393.0 和更新版本) 專業版或企業版。
* 64 位元 (x64) 處理器
* 硬體協助虛擬化
* 第二層位址轉譯 (SLAT)
* [適用於 Windows 10 的 Windows 軟體開發套件 (SDK)](https://go.microsoft.com/fwlink/?linkid=821375)。


## <a name="start-the-desktop-app-converter"></a>啟動 Desktop App Converter

1. 下載並安裝 [Desktop App Converter](https://aka.ms/converter)。

2. 以系統管理員身分執行 Desktop App Converter。

    ![以系統管理員身分執行 DAC](images/desktop-to-uwp/run-converter.png)

   隨即顯示主控台視窗。 您將會使用該主控台視窗執行命令。

## <a name="set-a-few-things-up-apps-with-installers-only"></a>進行幾項設定 (僅限有安裝程式的應用程式)

若您的應用程式沒有安裝程式，您可以跳到下一節。

1. 識別您作業系統的版本號碼。

   您可以藉由開啟在您電腦上的 **系統資訊應用程式**，找出您的作業系統版本號碼。

    ![在系統資訊應用程式中的作業系統版本](images/desktop-to-uwp/os-version.png)

2. 下載適合的 [Desktop App Converter 基礎映像檔](https://aka.ms/converterimages)。

   請確定出現在檔案名稱中的版本號碼符合您的 Windows 組建版本號碼。

   ![符合的基礎映像版本](images/desktop-to-uwp/base-image-version.png)

   將下載的檔案放在您電腦上的任何一個位置，以便您稍後可以找到它。

3. 在啟動 Desktop App Converter 時出現的主控台視窗中執行此命令︰```Set-ExecutionPolicy bypass```。
4.  執行此命令以安裝轉換器︰```DesktopAppConverter.exe -Setup -BaseImage .\BaseImage-1XXXX.wim -Verbose```。
5.  若系統提示您重新啟動您的電腦，請重新開機。

    轉換器展開基礎映像時於主控台視窗中出現的狀態訊息。 若您並未看到任何狀態訊息，請按任意鍵。 這可能會重新整理主控台視窗中的內容。

    ![主控台視窗中的狀態訊息](images/desktop-to-uwp/bas-image-setup.png)

    當基礎映像完全展開後，請移至下一節。

## <a name="package-an-app"></a>封裝應用程式

若要封裝您的應用程式，請在啟動 Desktop App Converter 時開啟的主控台視窗中執行 ``DesktopAppConverter.exe`` 命令。  

您將會使用參數來指定應用程式的套件名稱、發行者，以及版本號碼。

> [!NOTE]
> 若您已經在 Windows 市集中保留了您應用程式的名稱，您可以使用 Windows 開發人員中心儀表板取得套件和發行者的名稱。 若您想要在其他系統上側載您的應用程式，您可以為他們提供您自己的名稱，只要其發行者名稱與您選擇用來簽署您應用程式之憑證上的名稱相同即可。

### <a name="a-quick-look-at-command-parameters"></a>快速查看命令參數

以下是必要的參數。

```CMD
DesktopAppConverter.exe
-Installer <String>
-Destination <String>
-PackageName <String>
-Publisher <String>
-Version <Version>
```
您可以在[這裡](#command-reference)查看每一個參數的相關資訊。

### <a name="examples"></a>範例

以下是一些封裝應用程式的常見方式。

* [封裝有安裝程式 (.msi) 檔案的應用程式](#installer-conversion)
* [封裝有安裝程式執行檔的應用程式](#setup-conversion)
* [封裝沒有安裝程式的應用程式](#no-installer-conversion)
* [封裝應用程式、簽署應用程式，並準備提交市集](#optional-parameters)

<span id="installer-conversion" />
#### <a name="package-an-app-that-has-an-installer-msi-file"></a>封裝有安裝程式 (.msi) 檔案的應用程式

使用 ``Installer`` 參數指向安裝程式檔案。

```cmd
DesktopAppConverter.exe -Installer C:\Installer\MyAppSetup.msi -Destination C:\Output\MyApp -PackageName "MyApp" -Publisher "CN=MyPublisher" -Version 0.0.0.1
```

> [!NOTE]
> 確定您的安裝程式位於一個獨立的資料夾中，且相同的資料夾中只有其他與安裝程式相關的檔案。 轉換器會將該資料夾中的所有內容複製到隔離 Windows 環境中。

**影片**

<iframe src="https://mva.microsoft.com/en-US/training-courses-embed/developers-guide-to-the-desktop-bridge-17373/Demo-Convert-an-Application-That-Has-an-MSI-Installer-Kh1UU2WhD_7106218965" width="636" height="480" allowFullScreen frameBorder="0"></iframe>

<span id="setup-conversion" />
#### 封裝有安裝程式執行檔的應用程式

使用 ``Installer`` 參數指向安裝程式執行檔。

```cmd
DesktopAppConverter.exe -Installer C:\Installer\MyAppSetup.exe -InstallerArguments "/S" -Destination C:\Output\MyApp -PackageName "MyApp" -Publisher "CN=MyPublisher" -Version 0.0.0.1
```

``InstallerArguments`` 是一項選擇性參數。 然而，由於 Desktop App Converter 需要您的安裝程式才能在自動模式下執行，您可能需要使用這項參數，才能確保您的應用程式能使無訊息 (silent) 旗標正常運作。 ``/S`` 旗標是一個相當常見的無訊息旗標，但您使用的旗幟可能會因您所使用的安裝技術而有所不同。

**影片**

<iframe src="https://mva.microsoft.com/en-US/training-courses-embed/developers-guide-to-the-desktop-bridge-17373/Demo-Convert-an-Application-That-Has-a-Setup-exe-Installer-amWit2WhD_5306218965" width="636" height="480" allowFullScreen frameBorder="0"></iframe>

<span id="no-installer-conversion" />
#### 封裝沒有安裝程式的應用程式

在這個範例中，您可以使用 ``Installer`` 參數指向您應用程式的根資料夾。

使用 `AppExecutable` 參數，指向您應用程式的可執行檔。

```cmd
DesktopAppConverter.exe -Installer C:\Installer\MyApp\ -AppExecutable MyApp.exe -Destination C:\Output\MyApp -PackageName "MyApp" -Publisher "CN=MyPublisher" -Version 0.0.0.1
```

**影片**

<iframe src="https://mva.microsoft.com/en-US/training-courses-embed/developers-guide-to-the-desktop-bridge-17373/Demo-Convert-a-No-Installer-Application-agAXF2WhD_3506218965" width="636" height="480" allowFullScreen frameBorder="0"></iframe>

<span id="optional-parameters" />
#### 封裝應用程式、簽署應用程式，並對套件執行驗證檢查

這個範例與第一個範例相似，然而其不同點在它會告訴您如何簽署您的應用程式以供本機進行測試。並且驗證您的應用程式，確定其並未違反傳統型橋接器和 Windows 市集的需求。

```cmd
DesktopAppConverter.exe -Installer C:\Installer\MyAppSetup.exe -InstallerArguments "/S" -Destination C:\Output\MyApp -PackageName "MyApp" -Publisher "CN=MyPublisher" -Version 0.0.0.1 -MakeAppx -Sign -Verbose -Verify
```

``Sign`` 參數會產生一個憑證，然後以此憑證簽署您的應用程式。 若要執行您的應用程式，您必須先安裝該產生的憑證。 若要了解如何執行，請參閱本文中[執行封裝後的應用程式](#run-app)一節。

您可以使用 ``Verify`` 參數來驗證您的應用程式。

### <a name="a-quick-look-at-optional-parameters"></a>快速查看選用參數

``Sign`` 和 ``Verify`` 參數為選用參數。 除此之外，還有許多選用參數。  以下是一些較常使用的選用參數。

```CMD
[-ExpandedBaseImage <String>]
[-AppExecutable <String>]
[-AppFileTypes <String>]
[-AppId <String>]
[-AppDisplayName <String>]
[-AppDescription <String>]
[-PackageDisplayName <String>]
[-PackagePublisherDisplayName <String>]
[-MakeAppx]
[-LogFile <String>]
[<CommonParameters>]
```
您可以在下一節了解他們的相關資訊。

<span id="command-reference" />
### <a name="parameter-reference"></a>參數參考

以下是您執行 Desktop App Converter 時可以使用的參數完整清單 (依分類排序)。

* [安裝](#setup-params)
* [轉換](#conversion-params)
* [套件識別資料](#identity-params)
* [封裝資訊清單](#manifest-params)
* [清除](#cleanup-params)
* [架構](#architecture-params)
* [其他](#other-params)

您也可以透過在應用程式主控台視窗執行 ``Get-Help`` 命令來檢視整個清單。   

||||
|-------------|-----------|-------------|
|<span id="setup-params" /> <strong>安裝參數</strong>  ||
|-Setup [&lt;SwitchParameter&gt;] |必要 |在安裝模式中執行 DesktopAppConverter。 安裝模式支援展開提供的基礎映像。|
|-BaseImage &lt;String&gt; | 必要 |未展開之基礎映像的完整路徑。 如果指定了 -Setup，則此為必要參數。|
| -LogFile &lt;String&gt; |選用 |指定記錄檔。 如果省略，則會建立記錄檔暫時位置。|
|-NatSubnetPrefix &lt;String&gt; |選用 |可供 Nat 執行個體使用的首碼值。 通常，只有在您的主機電腦附加到與轉換器的 NetNat 相同的子網路範圍時，您才會想要變更此值。 您可以使用 **Get-NetNat** Cmdlet 來查詢目前的轉換器 NetNat 組態。 |
|-NoRestart [&lt;SwitchParameter&gt;] |必要 |執行安裝時不顯示重新開機的提示 (需要重新開機才能啟用容器功能)。 |
|<span id="conversion-params" /> <strong>轉換參數</strong>|||
|-AppInstallPath &lt;String&gt;  |選用 |您的應用程式在安裝之後，其已安裝檔案根資料夾的完整路徑 (例如 "C:\Program Files (x86)\MyApp")。|
|-Destination &lt;String&gt; |必要 |適用於轉換器 appx 輸出的所需目的地，如果尚未存在，DesktopAppConverter可以建立此位置。|
|-Installer &lt;String&gt; |必要 |應用程式的安裝程式路徑，必須能夠自動執行或以無訊息方式執行。 無安裝程式碼轉換，這是應用程式檔案根目錄的路徑。 |
|-InstallerArguments &lt;String&gt; |選用 |以逗號分隔的引數清單或字串，可強制您的安裝程式自動執行或以無訊息方式執行。 如果您的安裝程式是 msi，則此為選用參數。 若要取得安裝程式的記錄，請提供此處的安裝程式記錄引數，然後使用路徑 &lt;log_folder&gt;，此為轉換器會使用適當路徑來取代的語彙基元。 <br><br>**注意**︰自動/無訊息旗標和記錄引數會隨著安裝程式技術而不同。 <br><br>此參數的一個用法範例︰-InstallerArguments "/silent /log &lt;log_folder&gt;\install.log" 另一個不會產生記錄檔的範例可能看起來如下︰```-InstallerArguments "/quiet", "/norestart"``` 同樣地，如果您想要轉換器擷取它並將它放在最後一個記錄資料夾中，就必須照字面將任何記錄引導至語彙基元路徑 &lt;log_folder&gt;。|
|-InstallerValidExitCodes &lt;Int32&gt; |選用 |以逗號分隔的結束代碼清單，表示您的安裝程式已成功執行 (例如︰0, 1234, 5678)。  針對非 msi，這個值預設是 0，針對 msi 則為 0, 1641, 3010。|
|<span id="identity-params" /><strong>套件識別資料參數</strong>||
|-PackageName &lt;String&gt; |必要 |通用 Windows app 套件的名稱 |
|-Publisher &lt;String&gt; |必要 |通用 Windows app 套件的發行者 |
|-Version &lt;Version&gt; |必要 |通用 Windows app 套件的版本號碼 |
|<span id="manifest-params" /><strong>封裝資訊清單參數</strong>||
|-AppExecutable &lt;String&gt; |選用 |應用程式主要可執行檔 (例如 "MyApp.exe") 的名稱。 此參數是無安裝程式轉換的必要參數。 |
|-AppFileTypes &lt;String&gt;|選用 |以逗號分隔的檔案類型清單，應用程式將會與其產生關聯。 使用範例：-AppFileTypes "'.md', '.markdown'"。|
|-AppId &lt;String&gt; |選擇性 |在 Windows 應用程式套件資訊清單中指定要為應用程式識別碼所設定的值。 如果未指定，它將會設定為針對 *PackageName* 傳入的值。|
|-AppDisplayName &lt;String&gt;  |選用 |指定要在 Windows 應用程式套件資訊清單中設定應用程式顯示名稱的值。 如果未指定，它將會設定為針對 *PackageName* 傳入的值。 |
|-AppDescription &lt;String&gt; |選用 |在 Windows 應用程式套件資訊清單中指定要為應用程式描述所設定的值。 如果未指定，它將會設定為針對 *PackageName* 傳入的值。|
|-PackageDisplayName &lt;String&gt; |選用 |指定要在 Windows 應用程式套件資訊清單中設定套件顯示名稱的值。 如果未指定，它將會設定為針對 *PackageName* 傳入的值。 |
|-PackagePublisherDisplayName &lt;String&gt; |選用 |指定要在 Windows 應用程式套件資訊清單中設定套件發行者顯示名稱的值。 如果未指定，它將會設定為針對 *Publisher* 傳入的值。 |
|<span id="cleanup-params" /><strong>清理參數</strong>|||
|-Cleanup [&lt;Option&gt;] |必要 |執行 DesktopAppConverter 成品的清理。 Cleanup 模式有 3 個有效的選項。 |
|-Cleanup All | |刪除所有展開的基本映像、移除任何暫存轉換器檔案、移除容器網路，並停用選用的 Windows 功能、容器。 |
|-Cleanup WorkDirectory |必要 |移除所有的暫存轉換器檔案。 |
|-Cleanup ExpandedImage |必要 |刪除所有已展開且安裝在主機電腦上的基本映像。 |
|<span id="architecture-params" /><strong>套件架構參數</strong>|||
|-PackageArch &lt;String&gt; |必要 |產生具有指定架構的套件。 有效的選項是 'x86' 或 'x64'；例如，-PackageArch x86。 此為選擇性參數。 如果沒有指定，傳統型應用程式轉換器將會嘗試自動偵測套件架構。 如果自動偵測失敗，將預設為 x64 套件。 |
|<span id="other-params" /><strong>其他參數</strong>|||
|-ExpandedBaseImage &lt;String&gt;  |選用 |已經展開之基礎映像的完整路徑。|
|-MakeAppx [&lt;SwitchParameter&gt;]  |選用 |一個參數，如果顯示，即會通知這個指令碼呼叫輸出上的 MakeAppx。 |
|-LogFile &lt;String&gt;  |選用 |指定記錄檔。 如果省略，則會建立記錄檔暫時位置。 |
| -Sign [&lt;SwitchParameter&gt;] |選用 |告訴此指令碼使用為測試用途產生的憑證簽署輸出的 Windows 應用程式套件。 這個參數應該出現在參數 ```-MakeAppx``` 旁邊。 |
|&lt;一般參數&gt; |必要 |這個 Cmdlet 支援一般參數：*Verbose*、*Debug*、*ErrorAction*、*ErrorVariable*、*WarningAction*、*WarningVariable*、*OutBuffer*、*PipelineVariable* 及 *OutVariable*。 如需詳細資訊，請參閱 [about_CommonParameters](http://go.microsoft.com/fwlink/?LinkID=113216)。 |
| -Verify [&lt;SwitchParameter&gt;] |選擇性 |一個參數，如果存在，這個參數會指示 DAC 根據傳統型橋接器及 Windows 市集需求來驗證應用程式套件。 結果會是驗證報告「VerifyReport.xml」，這在瀏覽器中呈現最佳視覺效果。 這個參數應該出現在參數 `-MakeAppx` 旁邊。 |
|-PublishComRegistrations| 選用| 掃描所有由您的安裝程式建立的公用 COM 註冊，並發行資訊清單中有效的項目。 只有當您想要讓其他應用程式使用這些註冊時，才使用此旗標。 若只有您的應用程式才會使用到這些註冊，則您就不需要使用此旗標。 <br><br>請參閱[本文](https://blogs.windows.com/buildingapps/2017/04/13/com-server-ole-document-support-desktop-bridge/#lDg5gSFxJ2TDlpC6.97)以確定您的 COM 註冊在您封裝應用程式之後，其行為會跟預期中的一樣。

<span id="run-app" />
## <a name="run-the-packaged-app"></a>執行封裝後的應用程式

您有兩種方式執行您的應用程式。

其中一個方式是開啟 PowerShell 命令提示字元，並輸入此命令︰```Add-AppxPackage –Register AppxManifest.xml```。 這可能是執行您的應用程式最簡單的方式，因為應用程式不需要先經過簽署。

另一個方式則是使用一個憑證簽署您的應用程式。 若您使用了 ```sign``` 參數，Desktop App Converter 會為您產生一個憑證，並以該憑證簽署您的應用程式。 該憑證的檔名為 **auto-generated.cer**，您可以在封裝後應用程式的根資料夾中找到該檔案。

請依照下列步驟來安裝產生的認證，然後再執行您的應用程式。

1. 按兩下 **auto-generated.cer** 以安裝憑證。

   ![產生的憑證檔案](images/desktop-to-uwp/generated-cert-file.png)

   > [!NOTE]
   > 如果系統提示您輸入密碼，請使用預設密碼「123456」。

2. 在 **\[憑證\]** 對話方塊中，選擇 **\[安裝憑證\]** 按鈕。
3. 在 **\[憑證匯入精靈\]** 中，將憑證安裝到 **\[本機電腦\]**，並將憑證放入 **\[受信任的人\]** 憑證存放區。

   ![受信任的人存放區](images/desktop-to-uwp/trusted-people-store.png)

5. 在您已封裝應用程式的根資料夾中，按兩下 Windows 應用程式套件檔案。

   ![Windows 應用程式套件檔案](images/desktop-to-uwp/windows-app-package.png)

6. 選擇 **\[安裝\]** 按鈕以安裝應用程式。

   ![安裝按鈕](images/desktop-to-uwp/install.png)


## <a name="modify-the-packaged-app"></a>修改已封裝的應用程式

您可能會想要變更您已封裝的應用程式，處理 BUG、增加視覺資產，或是透過現代化的體驗 (例如動態磚) 來美化您的應用程式。

在您完成變更之後，您不需要再執行一次轉換器。 您可以使用 MakeAppx 工具和 DAC 為您應用程式產生的 appxmanifest.xml 檔案重新封裝您的應用程式。 請參閱[產生 Windows 應用程式套件](desktop-to-uwp-manual-conversion.md#make-appx)。

> [!NOTE]
> 若您變更了您應用程式建立的登錄設定，則您就必須再執行一次 Desktop App Converter 以揀選那些變更。

**影片**

|修改和重新封裝輸出 |示範：修改和重新封裝輸出|
|---|---|
|<iframe src="https://mva.microsoft.com/en-US/training-courses-embed/developers-guide-to-the-desktop-bridge-17373/Video-Modifying-and-Repackaging-Output-from-Desktop-App-Converter-OwpAJ3WhD_6706218965" width="426" height="472" allowFullScreen frameBorder="0"></iframe>|<iframe src="https://mva.microsoft.com/en-US/training-courses-embed/developers-guide-to-the-desktop-bridge-17373/Demo-Modify-Output-from-Desktop-App-Converter-gEnsa3WhD_8606218965" width="426" height="472" allowFullScreen frameBorder="0"></iframe>|

下列兩個小節描述了幾個您可能會考慮套用至您已封裝應用程式的選用修正內容。

### <a name="delete-unnecessary-files-and-registry-keys"></a>刪除不需要的檔案和登錄機碼

Desktop App Converter 採用極為保守的方式來篩選出容器中的檔案和系統雜訊。

若您需要的話，您可以檢視 VFS 資料夾，並刪除您安裝程式不需要的任何檔案。  您也可以檢閱 Reg.dat 內容，並刪除應用程式未安裝/不需要的任何機碼。

### <a name="fix-corrupted-pe-headers"></a>修正損毀的 PE 標頭

轉換過程中，DesktopAppConverter 會自動執行 PEHeaderCertFixTool，以修正任何損毀的 PE 標頭。 不過，您也可以在 UWP Windows 應用程式套件、鬆散檔案或特定的二進位檔上執行 PEHeaderCertFixTool。 這裡提供一個範例。

```CMD
PEHeaderCertFixTool.exe <binary file>|<.appx package>|&lt;folder> [/c] [/v]
 /c   -- check for corrupted certificate but do not fix (optional)
 /v   -- verbose (optional)
example1: PEHeaderCertFixTool app.exe
example2: PEHeaderCertFixTool c:\package.appx /c
example3: PEHeaderCertFixTool c:\myapp /c /v
```

## <a name="known-issues-and-disclosures"></a>已知的問題和公開揭示

### <a name="known-issues-and-workarounds"></a>已知的問題和因應措施

以下是部分已知的問題，以及您可以嘗試進行的一些解決辦法。

#### <a name="ecreatingisolatedenvfailed-an-estartingisolatedenvfailed-errors"></a>E_CREATING_ISOLATED_ENV_FAILED an E_STARTING_ISOLATED_ENV_FAILED 錯誤    

若您接收到其中任何一項錯誤，請確定您使用的是[下載中心](https://aka.ms/converterimages)內的有效基礎映像。
如果您使用的是正確的基礎映像，請嘗試在您的命令中使用 ``-Cleanup All``。
若此方法仍然無法解決您的問題，請將您的記錄檔傳送至 converter@microsoft.com，以幫助我們調查。

#### <a name="new-containernetwork-the-object-already-exists-error"></a>New-ContainerNetwork︰物件已經存在錯誤

當您設定一個新的基礎映像時，可能會接收到這個錯誤。 若您在先前已安裝 Desktop App Converter 的程式開發人員電腦上擁有 Windows 測試人員正式發行前小眾測試版，則可能會發生此問題。

若要修正此問題，請嘗試在提升權限的命令提示字元中執行命令 `Netsh int ipv4 reset`，並重新啟動您的電腦。

#### <a name="your-net-app-is-compiled-with-the-anycpu-build-option-and-fails-to-install"></a>您的 .NET 應用程式使用「AnyCPU」組建選項進行編譯，但安裝仍然失敗

若主要可執行檔或任何相依性檔案是存放於 **Program Files** 或 **Windows\System32** 資料夾階層底下，就有可能會發生此問題。

若要修正此問題，請嘗試使用特定架構的桌面安裝程式 (32 位元或 64 位元) 產生 Windows 應用程式套件。

#### <a name="publishing-public-side-by-side-fusion-assemblies-wont-work"></a>發行公用的並列 Fusion 組件無法運作

 在安裝期間，應用程式可以發行公用的並列 Fusion 組件，可存取任何其他處理程序。 在處理序啟用內容建立期間，這些組件是透過名為 CSRSS.exe 的系統處理序來擷取。 針對已轉換的處理程序完成此動作之後，這些組件的啟用內容建立和模組載入將會失敗。 並列 Fusion 組件會登錄於下列位置︰
  + 登錄： `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\SideBySide\Winners`
  + 檔案系統︰%windir%\SideBySide

這是已知的限制，並且目前沒有任何因應措施。 但是，由於收件匣組件 (例如 ComCtl) 均隨附於作業系統，因此取得和它們的相依性是安全的。

#### <a name="error-found-in-xml-the-executable-attribute-is-invalid---the-value-myappexe-is-invalid-according-to-its-datatype"></a>XML 中找到錯誤。 'Executable' 屬性無效：根據其資料類型，數值 'MyApp.EXE' 無效

若您應用程式中包含了副檔名為大寫 **.EXE** 的可執行檔，就有可能發生這個錯誤。 雖然副檔名的大小寫並不會影響您應用程式的執行，但這仍有可能會使 DAC 產生這個錯誤。

若要修正此問題，請嘗試在封裝時指定 **-AppExecutable** 旗標，並使用小寫的「.exe」作為您主要可執行檔的副檔名 (例如：MYAPP.exe)。    或者，您也可以將您應用程式當中所有可執行檔的副檔名，從小寫變更為大寫 (例如：從 .EXE 改為 .exe)。


### <a name="telemetry-from-desktop-app-converter"></a>來自 Desktop App Converter 的遙測資訊

傳統型應用程式轉換器可能會收集您及您軟體使用方式的相關資訊，並將這項資訊傳送給 Microsoft。 您可以在產品文件和 [Microsoft 隱私權聲明](http://go.microsoft.com/fwlink/?LinkId=521839)中深入了解 Microsoft 的資料收集及使用方式。 您同意遵守 Microsoft 隱私權聲明的所有適用條款。

預設會針對傳統型應用程式轉換器啟用遙測。 新增下列登錄機碼來設定所需設定的遙測︰  

```cmd
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\DesktopAppConverter
```
+ 使用設為 1 的 DWORD 來新增或編輯 *DisableTelemetry* 值。
+ 若要啟用遙測，請移除此機碼或將值設為 0。

### <a name="language-support"></a>語言支援

Desktop App Converter 不支援 Unicode；因此，您無法在工具上使用中文字元或非 ASCII 字元。

## <a name="next-steps"></a>後續步驟

**執行您的應用程式/找出並修正問題**

請參閱[執行、偵錯以及測試封裝的傳統型橋接器應用程式 (傳統型橋接器)](desktop-to-uwp-debug.md)

**散布您的應用程式**

請參閱[散佈封裝的傳統型應用程式 (傳統型橋接器)](desktop-to-uwp-distribute.md)

**尋找特定問題的解答**

我們的團隊會監視這些 [StackOverflow 標記](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge)。

**提供有關本文的意見反應**

使用下方的留言區塊。
