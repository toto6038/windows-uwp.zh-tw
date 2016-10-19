---
author: awkoren
Description: "準備您的 Windows 傳統型應用程式 (Win32、WPF 及 Windows Forms)，以便使用桌面轉換擴充功能轉換為通用 Windows 平台 (UWP) 應用程式。"
Search.Product: eADQiWindows 10XVcnh
title: "將您的傳統型應用程式轉換為通用 Windows 平台 (UWP) 應用程式"
translationtype: Human Translation
ms.sourcegitcommit: ff8cd3ab5e38cfc3a2b5fabaad15f78a5f2620f2
ms.openlocfilehash: 6cf6367ed0f6acea0f87ac36a050e874425423b1

---

# 將您的傳統型應用程式轉換為通用 Windows 平台 (UWP) 應用程式

\[正式發行前可能會進行大幅度修改之發行前版本產品的一些相關資訊。 Microsoft 對此處提供的資訊，不提供任何明確或隱含的瑕疵擔保。\]

準備您的 Windows 傳統型應用程式 (Win32、WPF 及 Windows Forms)，以便使用桌面轉換擴充功能轉換為通用 Windows 平台 (UWP) 應用程式。

## 將應用程式轉換為 UWP 的好處

使用桌面轉換擴充功能的 UWP 是可讓您將 Windows 傳統型應用程式 (例如 Win32、Windows Forms 和 WPF) 或遊戲轉換為通用 Windows 平台 (UWP) app 或遊戲的橋樑。 請參閱 [UWP app 指南](https://msdn.microsoft.com/library/windows/apps/dn894631.aspx)。 轉換之後，您的 Windows 傳統型應用程式就會以目標為 Windows 10 Desktop 的 UWP app 套件形式 (.appx 或 .appxbundle) 來封裝、提供服務及部署。

有兩個部分的技術可將傳統型應用程式轉換為 UWP 套件。 第一個部分是傳統型應用程式轉換器，它會取得您現有的二進位檔並將它們重新封裝為 UWP 套件。 您的程式碼仍然相同，只是以不同方式封裝。 第二個部分是由 Windows 年度更新版中的執行階段技術所組成，可讓 UWP 套件擁有以完全信任方式執行而不是在應用程式容器中執行的可執行檔。 這項技術也會為已轉換的應用程式提供套件識別資料，需要有此識別資料才能使用某些 UWP API。

以下是一些轉換 Windows 傳統型應用程式的好處。

* 可針對您的 app 為客戶提供更順暢的安裝體驗。 您可以使用側載來將它部署到電腦 (請參閱[在 Windows 10 中側載 LOB App](https://technet.microsoft.com/library/mt269549.aspx))，並且在解除安裝之後不會留下任何追蹤。 期限較長，您也能夠將您的應用程式發佈到 Windows 市集。

* 由於已轉換的應用程式具有套件識別資料，因此，您甚至可從完全信任的磁碟分割呼叫比以前更多的 UWP API。 請參閱[已轉換的傳統型應用程式支援的 UWP API](desktop-to-uwp-supported-api.md) 的完整清單。 

* 依照您自己的步調，您可以將 UWP 功能新增到您的應用程式套件，例如 XAML 使用者介面、動態磚更新、UWP 背景工作、應用程式服務，以及其他更多項目。 所有可供任何其他 UWP app 使用的功能都可供您的 app 使用。

* 如果您選擇要將所有 app 功能移出 app 完全信任的磁碟分割，然後移入 app 容器磁碟分割，則您的 app 將能在任何 Windows 10 裝置上執行。

* 做為 UWP app，您的 app 能夠執行的動作，就像它做為 Windows 傳統型應用程式時所做的一樣。 它會與登錄和檔案系統的虛擬化檢視互動，這很難與實際登錄和檔案系統區分。

* 您的應用程式可以參與 Windows 市集的內建授權和自動更新功能。 由於自動更新只會下載檔案已變更的部分，因此是一項非常可靠又有效率的機制。

## 準備將傳統型應用程式轉換為 UWP

您可能不需要針對 app 的轉換程式做太多準備。 請記住，Windows 市集會為您處理授權和自動更新，因此您可以從程式碼基底中移除這些功能。 如果您的應用程式符合下列任一情況，您就需要在轉換之前先解決此問題。

+ __您的應用程式是使用 4.6.1 之前的 .NET 版本__。 僅支援 .NET 4.6.1。 您必須先將應用程式重新定位至 .NET 4.6.1 才能轉換。 

+ __您的應用程式一律會以提升的安全性權限執行__。 在以互動使用者身分執行時您的應用程式必須能夠運作。 從 Windows 市集安裝您應用程式的使用者可能不是系統管理員，因此需要以提升的權限來執行您的應用程式，表示它無法針對標準使用者正常執行。

+ __您的應用程式需要核心模式驅動程式或 Windows 服務__。 橋接器適用於應用程式，但它不支援核心模式驅動程式或需要在系統帳戶下執行的 Windows 服務。 請改用[背景工作](https://msdn.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task)，而非 Windows 服務。

+ __您的應用程式模組會以同處理序載入至不在您 AppX 套件中的處理序__。 這是不允許的，這表示不支援同處理序擴充功能，例如 [Shell 擴充功能](https://msdn.microsoft.com/library/windows/desktop/dd758089.aspx)。 但是，如果您在同一個套件中有兩個 app，您就可以在它們之間進行處理序間通訊。

+ __您的 app 呼叫 [SetDllDirectory](https://msdn.microsoft.com/library/windows/desktop/ms686203) 或 [AddDllDirectory](https://msdn.microsoft.com/library/windows/desktop/hh310513)__。 已轉換的應用程式目前不支援這些功能。 我們正致力於在未來版本中新增支援。 因應措施是，您可以將使用這些功能找到的所有 .dll 複製到套件根目錄。 

+ __您的 app 會使用自訂的應用程式使用者模型識別碼 (AUMID)__。 如果您的處理序會呼叫 [SetCurrentProcessExplicitAppUserModelID](https://msdn.microsoft.com/library/windows/desktop/dd378422.aspx) 來設定自己的 AUMID，則它可能只會使用應用程式模型環境/AppX 套件為它產生的 AUMID。 您不能定義自訂的 AUMID。

+ __您的應用程式會修改 HKEY_LOCAL_MACHINE (HKLM) 登錄區__。 應用程式對於建立 HKLM 機碼或開啟某一個機碼進行修改的任何嘗試都將產生存取遭拒的失敗。 請記住，您的應用程式有自己私用的登錄虛擬化檢視，因此，不適用全部使用者和整部電腦登錄區的概念 (這是 HKLM 的概念)。 您需要尋找其他方式來封存您使用 HKLM 所做的動作，例如改為寫入 HKEY_CURRENT_USER (HKCU)。

+ __您的應用程式會使用 ddeexec 登錄子機碼做為啟動另一個應用程式的方式__。 在您的[應用程式套件資訊清單](https://msdn.microsoft.com/library/windows/apps/br211474.aspx)中，使用各種不同 Activatable* 擴充功能來設定時，請改用其中一個 DelegateExecute 動詞命令處理常式。

+ __您的應用程式會寫入 AppData 資料夾，以便與其他應用程式共用資料__。 轉換之後，系統會將 AppData 重新導向至本機 app 資料存放區，此為每一個 UWP app 的私人存放區。 使用不同方式來進行處理序間的資料共用。 如需詳細資訊，請參閱[儲存及擷取設定和其他應用程式資料](https://msdn.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data)。

+ __您的應用程式會寫入您應用程式的安裝目錄__。 例如，您的應用程式所寫入的記錄檔是放在與您的 exe 相同的目錄中。 不支援此情況，因此您將需要尋找其他位置，例如本機應用程式資料存放區。

+ __您的應用程式安裝需要使用者互動__。 您的應用程式安裝程式必須能夠以無訊息方式執行，而且它需要安裝所有預設不在全新作業系統映像上的必要條件。

+ __您的應用程式會使用目前的工作目錄__。 在執行階段，已轉換的應用程式將不會取得您先前在桌面 .LNK 捷徑中指定的相同工作目錄。 如果您的應用程式必須擁有正確的目錄才能正確運作，則您需要在執行階段變更 CWD。

+ __您的應用程式需要 UIAccess__。 如果您的應用程式在 UAC 資訊清單的 `requestedExecutionLevel` 元素中指定 `UIAccess=true`，則目前不支援轉換為 UWP。 如需詳細資訊，請參閱 [UI 自動化安全性概觀](https://msdn.microsoft.com/library/ms742884.aspx)。

+ __您的 app 已經是完整的 UWP app，而且想要從應用程式套件內叫用完全信任的處理程序__。 不支援反向使用橋接器，而且嘗試叫用完全信任處理程序的 UWP app 將無法通過市集認證。 若要在側載應用程式中啟用此案例，請先在您的應用程式資訊清單中包含 ```<desktop:Extension>``` 宣告 (其中包含值為 *Windows.FullTrustApplication* 的 *EntryPoint* 屬性)。 接著，將 app 的 ```<TargetDeviceFamily>``` 設定為 *Windows.Desktop*。 這表示 app 的唯一目標是傳統型裝置系列，而不是所有 UWP 裝置。 如果您的 app 傳回錯誤「APPX0501︰驗證錯誤」**且這些設定均已就緒，請確認您的目標裝置系列；app 的目標很可能是 *Windows.Universal* 系列，而非 *Windows.Desktop*。 

+ __您的 app 公開 COM 物件或 GAC 組件，以供其他處理程序使用__。 在目前版本中，您的應用程式不能公開 COM 物件或 GAC 組件給來自 AppX 套件外部的可執行檔處理程序使用。 來自套件內的處理程序可以正常登錄及使用 COM 物件和 GAC 組件，但無法從外部看見它們。 這表示如果透過外部處理程序叫用 OLE 等互通性案例，將無法作用。 

+ __不支援您的應用程式連結 C 執行階段程式庫的方式__。 Microsoft C/C++ 執行階段程式庫提供適用於 Microsoft Windows 作業系統的程式設計常式。 這些常式將許多常見但 C 和 C++ 語言未提供的程式設計工作自動化。 如果您的應用程式利用 C/C++ 執行階段程式庫，您必須確定以支援的方式連結它。 
    
    Visual Studio 2015 支援用動態連結 (讓程式碼使用一般 DLL 檔案) 或用靜態連結 (將程式庫直接連結到程式碼中)，連結到目前的 CRT 版本。 可能的話，我們建議您的應用程式搭配 VS 2015 使用動態連結。 

    支援先前的多種 Visual Studio 版本。 如需詳細資訊，請參閱下表： 

    <table>
    <th>Visual Studio 版本</td><th>動態連結</th><th>靜態連結</th></th>
    <tr><td>2005 (VC 8)</td><td>不支援</td><td>支援</td>
    <tr><td>2008 (VC 9)</td><td>不支援</td><td>支援</td>
    <tr><td>2010 (VC 10)</td><td>支援</td><td>支援</td>
    <tr><td>2012 (VC 11)</td><td>支援</td><td>不支援</td>
    <tr><td>2013 (VC 12)</td><td>支援</td><td>不支援</td>
    <tr><td>2015 (VC 14)</td><td>支援</td><td>支援</td>
    </table>
    
    注意：在任何情況下，您都必須連結到最新可公開取得的 CRT。

+ __您的應用程式會從 Windows 並列資料夾安裝與載入組件__。 例如，您的應用程式使用 C 執行階段程式庫 VC8 或 VC9 且從 Windows 並列資料夾動態連結它們，表示您的程式碼是使用來自共用資料夾的一般 DLL 檔。 不支援此連結方式。 您必須以靜態方式連結它們，方法是直接將可轉散發的程式庫檔案連結到您的程式碼。

## 開始轉換處理程序

您有數個不同選項可用來轉換 app。

* **Desktop App Converter (DAC)**。 DAC 是會為您自動轉換並簽署 app 的工具。 使用 DAC 相當簡便且自動化，如果您的 app 會進行許多系統修改，或者您對於安裝程式所做的一切有任何不確定性，這相當有用。 若要開始使用 DAC，請參閱 [Desktop App Converter](desktop-to-uwp-run-desktop-app-converter.md)。 

* **手動轉換**。 如果您的 app 是使用 xcopy 所安裝，或是您已經熟悉安裝程式對系統所做的變更，則手動轉換可能是更簡單的選擇。 這包括建立資訊清單檔案、執行 MakeAppx.exe 工具，然後簽署應用程式套件。 如需如何手動轉換的詳細資訊，請參閱[將您的 Windows 傳統型應用程式手動轉換為通用 Windows 平台 (UWP) app](desktop-to-uwp-manual-conversion.md)。 

* **協力廠商安裝程式**。 有三個常見的協力廠商安裝程式 ([由 Flexera 所提供的 InstallShield](http://www.flexerasoftware.com/producer/products/software-installation/installshield-software-installer)、[由 FireGiant 所提供的 WiX](https://www.firegiant.com/r/appx) 及[由 Caphyon 所提供的 Advanced Installer](http://www.advancedinstaller.com/uwp-app-package)) 現在支援桌面橋接器，而且只需要按幾下就能產生 MSI 安裝程式和已轉換的應用程式套件。 如需詳細資訊，請個別瀏覽每個安裝程式的網站。 

## 取得支援並提供意見反應

如果您在執行轉換應用程式時發生問題，您可以造訪[論壇](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/home?forum=wpdevelop)以尋求協助。 

若要提供意見反應或提出功能建議，請查看 [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial)。 

## 本節內容

| 主題 | 描述 |
|-------|-------------|
| [Desktop App Converter](desktop-to-uwp-run-desktop-app-converter.md) | 說明如何執行 Desktop App Converter。 您可能不需要針對 app 的轉換處理程序做太多準備 (如果有的話)。 |
| [手動轉換傳統型應用程式](desktop-to-uwp-manual-conversion.md) | 了解如何手動建立應用程式套件和資訊清單。 |
| [已轉換的傳統型應用程式擴充功能](desktop-to-uwp-extensions.md) | 使用擴充功能來增強已轉換的應用程式，以啟用像是啟動工作、檔案總管整合及更多的擴充功能。 |
| [已轉換的傳統型應用程式支援的 UWP API](desktop-to-uwp-supported-api.md) | 查看您可以針對已轉換的應用程式使用哪些 UWP API。 |
| [簽署已轉換的傳統型應用程式](desktop-to-uwp-signing.md) | 了解如何使用憑證簽署已轉換的 .appx 套件。 |
| [部署和偵錯已轉換的 UWP App](desktop-to-uwp-deploy-and-debug.md) | 取得在轉換 app 之後進行部署和偵錯的相關說明。 此外，如果您想探究一些桌面轉換擴充功能的本質，本主題非常適合您。 |
| [傳統型應用程式橋接至 UWP 的程式碼範例](https://github.com/Microsoft/DesktopBridgeToUWP-Samples) | GitHub 上示範轉換 app 功能的程式碼範例。 |


<!--HONumber=Sep16_HO3-->


