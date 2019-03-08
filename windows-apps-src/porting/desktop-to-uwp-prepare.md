---
Description: 這篇文章列出想要封裝您的桌面應用程式之前所知道的事項。 您可能不需要針對應用程式的封裝程序做太多準備。
Search.Product: eADQiWindows 10XVcnh
title: 準備封裝傳統型應用程式 （傳統型橋接器）
ms.date: 05/18/20188
ms.topic: article
keywords: Windows 10, UWP
ms.assetid: 71a57ca2-ca00-471d-8ad9-52f285f3022e
ms.localizationpriority: medium
ms.openlocfilehash: 49238ba1b72cf3daf46fb076432a478e9f381bc6
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57600893"
---
# <a name="prepare-to-package-a-desktop-application"></a>準備封裝傳統型應用程式

本文列出封裝傳統型應用程式之前，您需要知道的事項。 您可能沒有執行許多作業，若要準備您的應用程式進行封裝程序，但如果任何以下項目適用於您的應用程式，您需要封裝之前解決。 請記住，Microsoft Store 會為您處理授權和自動更新，因此您可以從程式碼基底中移除與那些工作有關的任何功能。

>[!IMPORTANT]
>Windows 10 版本 1607年中引進了能夠建立您的桌面應用程式 （亦稱為傳統型橋接器） 是 Windows 應用程式套件，它僅適用於 Windows 10 Anniversary Update (10.0; 為目標的專案中組建 14393） 或更新版本，在 Visual Studio 中的。

+ __您的應用程式需要的.NET 版本 4.6.2 之前__。 您必須確定您的應用程式執行於.NET 4.6.2。 您無法要求或轉散發 4.6.2 之前的版本。 這是隨附於 Windows 10 年度更新版的 .NET 版本。 正在驗證您的應用程式適用於此版本，可確保您的應用程式，將會繼續與未來的更新，Windows 10 相容。  如果您的應用程式的目標.NET Framework 4.0 或更新版本，它必須是在.NET 4.6.2 上執行，但您仍應該測試它。

+ __以提高權限的安全性權限執行您的應用程式一律__。 您的應用程式必須在互動式使用者的身分執行時運作正常。 從 Microsoft Store 安裝應用程式的使用者可能不到系統管理員，因此需要執行提高權限的表示，就無法正確執行標準使用者應用程式。 Microsoft Store 不接受其功能的任何部分需要提高權限的應用程式。

+ __應用程式所需的核心模式驅動程式或 Windows 服務__。 橋接器適用於應用程式，但它不支援核心模式驅動程式或需要在系統帳戶下執行的 Windows 服務。 請改用[背景工作](https://msdn.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task)，而非 Windows 服務。

+ __您的應用程式模組會以同處理程序載入至不在您 Windows 應用程式套件中的處理程序__。 這是不允許的，這表示不支援同處理序擴充功能，例如 [Shell 擴充功能](https://msdn.microsoft.com/library/windows/desktop/dd758089.aspx)。 但是，如果您在同一個套件中有兩個 app，您就可以在它們之間進行處理序間通訊。

+ __您的應用程式使用自訂應用程式使用者模型識別碼 (AUMID)__。 如果您的程序呼叫[SetCurrentProcessExplicitAppUserModelID](https://msdn.microsoft.com/library/windows/desktop/dd378422.aspx)若要設定自己的 AUMID，則它可能只能使用應用程式模型環境/Windows 應用程式封裝所產生的 AUMID。 您不能定義自訂的 AUMID。

+ __您的應用程式修改的 HKEY_LOCAL_MACHINE (HKLM) 登錄區__。 任何嘗試您的應用程式來建立 HKLM 金鑰，或開啟另一個用於修改，將會導致拒絕存取時的失敗。 請記住您的應用程式有登錄它自己私用虛擬化的檢視，因此不適用整個使用者和電腦的登錄區 （也就是 HKLM） 的概念。 您需要尋找其他方式來封存您使用 HKLM 所做的動作，例如改為寫入 HKEY_CURRENT_USER (HKCU)。

+ __您的應用程式做為啟動另一個應用程式使用 ddeexec 登錄子機碼__。 在您的[應用程式套件資訊清單](https://msdn.microsoft.com/library/windows/apps/br211474.aspx)中，使用各種不同 Activatable* 擴充功能來設定時，請改用其中一個 DelegateExecute 動詞命令處理常式。

+ __您的應用程式將寫入 [AppData] 資料夾或登錄，目的是要與另一個應用程式共用資料__。 轉換之後，系統會將 AppData 重新導向至本機 app 資料存放區，此為每一個 UWP app 的私人存放區。

  您的應用程式寫入 HKEY_LOCAL_MACHINE 登錄 hive 的所有項目會重新導向至隔離的二進位檔和任何您的應用程式將寫入至 HKEY_CURRENT_USER 登錄區的項目會放入私用每個使用者，每個應用程式位置。 如需檔案和登錄重新導向的詳細資訊，請參閱[傳統型橋接器的幕後作業](desktop-to-uwp-behind-the-scenes.md)。  

  使用不同方式來進行處理序間的資料共用。 如需詳細資訊，請參閱[儲存及擷取設定和其他應用程式資料](https://msdn.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data)。

+ __您的應用程式寫入至您的應用程式的安裝目錄__。 例如，您的應用程式會寫入記錄檔，您放在您的 exe 所在的目錄中。 不支援此情況，因此您將需要尋找其他位置，例如本機應用程式資料存放區。

+ __您的應用程式安裝需要使用者互動__。 您的應用程式安裝程式必須能夠以無訊息模式，執行，它必須安裝所有先決條件，不在預設會在全新的作業系統映像上。

+ __您的應用程式會使用目前的工作目錄__。 在執行階段，封裝傳統型應用程式不會收到相同的工作目錄，您先前在您的桌面中所指定。LNK 捷徑。 您需要變更您在執行階段 CWD，如果有正確的目錄，請務必針對您的應用程式，才能正確運作。

+ __您的應用程式需要 UIAccess__。 如果您的應用程式在 UAC 資訊清單的 `requestedExecutionLevel` 元素中指定 `UIAccess=true`，則目前不支援轉換為 UWP。 如需詳細資訊，請參閱 [UI 自動化安全性概觀](https://msdn.microsoft.com/library/ms742884.aspx)。

+ __您的應用程式會公開 COM 物件__。 來自套件內的處理程序和延伸模組可以正常登錄及使用 COM 物件及 OLE 伺服器，並且在處理程序內和處理程序外 (OOP) 皆可以。  Creators Updates 新增了封裝 COM 的支援，提供了現在可在套件外部看見之 OOP COM 及 OLE 伺服器的登錄能力。  請參閱[適用於傳統型橋接器的 COM 伺服器和 OLE 文件支援](https://blogs.windows.com/buildingapps/2017/04/13/com-server-ole-document-support-desktop-bridge/#bjPyETFgtpZBGrS1.97)。

   封裝 COM 支援與現有的 COM API 相容，但無法用於依賴直接讀取登錄檔的應用程式延伸模組，因為封裝 COM 是位於一處私人位置。

+ __您的應用程式公開 GAC 組件，供其他處理序__。 在目前的版本中，您的應用程式無法公開 GAC 組件，使用來自外部可執行檔至您的 Windows 應用程式封裝的處理序。 來自套件內的處理程序可以正常登錄及使用 GAC 組件，但無法從外部看見它們。 這表示如果透過外部處理程序叫用 OLE 等互通性案例，將無法作用。

+ __您的應用程式不支援的方式連結 C 執行階段程式庫 (CRT)__。 Microsoft C/C++ 執行階段程式庫提供適用於 Microsoft Windows 作業系統的程式設計常式。 這些常式將許多常見但 C 和 C++ 語言未提供的程式設計工作自動化。 如果您的應用程式會利用 C/c + + 執行階段程式庫，您需要確定它支援的方式連結。

    Visual Studio 2017 支援同時用靜態和動態連結 (讓程式碼使用一般 DLL 檔案) 或用靜態連結 (將程式庫直接連結到程式碼中)，連結到目前的 CRT 版本。 可能的話，我們建議您的應用程式使用動態連結的 VS 2017。

    支援先前的多種 Visual Studio 版本。 如需詳細資訊，請參閱下表：

    <table>
    <th>Visual Studio 版本</td><th>動態連結</th><th>靜態連結</th></th>
    <tr><td>2005 (VC 8)</td><td>不支援</td><td>支援</td>
    <tr><td>2008 (VC 9)</td><td>不支援</td><td>支援</td>
    <tr><td>2010 (VC 10)</td><td>支援</td><td>支援</td>
    <tr><td>2012 (VC 11)</td><td>支援</td><td>不支援</td>
    <tr><td>2013 (VC 12)</td><td>支援</td><td>不支援</td>
    <tr><td>2015 和 2017 (VC 14)</td><td>支援</td><td>支援</td>
    </table>

    注意：在所有情況下，您必須連結至最新公開可用的 CRT。

+ __您的應用程式會安裝並載入組件從 Windows 並排顯示資料夾__。 例如，您的應用程式使用 C 執行階段程式庫 VC8 或 VC9，而以動態方式從 Windows [並排顯示] 資料夾中，這表示您的程式碼使用通用 DLL 檔案從共用資料夾連結。 不支援此連結方式。 您必須以靜態方式連結它們，方法是直接將可轉散發的程式庫檔案連結到您的程式碼。

+ __您的應用程式會使用 System32/SysWOW64 資料夾中的相依性__。 若要讓這些 DLL 能夠運作，您必須將它們包含於 Windows 應用程式套件的虛擬檔案系統部分。 這可確保應用程式就如同在中安裝 Dll **System32**/**SysWOW64**資料夾。 在套件的根目錄中，建立名為 **VFS** 的資料夾。 在該資料夾內，建立 **SystemX64** 和 **SystemX86** 資料夾。 然後，將 DLL 的 32 位元版本放置於 **SystemX86** 資料夾中，並將 64 位元版本放置於 **SystemX64** 資料夾中。

+ __您的 app 使用 VCLibs 架構套件__。 如果將 VCLibs 程式庫定義為 Windows 應用程式套件中的相依性，則可直接從 Microsoft Store 安裝它們。 比方說，如果您的應用程式使用 Dev11 VCLibs 封裝，請您的應用程式封裝資訊清單中進行下列變更：在下`<Dependencies>` 節點，加入：  
`<PackageDependency Name="Microsoft.VCLibs.110.00.UWPDesktop" MinVersion="11.0.24217.0" Publisher="CN=Microsoft Corporation, O=Microsoft Corporation, L=Redmond, S=Washington, C=US" />`  
從 Microsoft Store 進行安裝期間，將會在安裝 app 之前，先安裝 VCLibs 架構的適當版本 (x86 或 x64)。  
若是安裝側載應用程式，就不會安裝相依性。 若要在您的電腦上手動安裝相依性，您必須下載並安裝適用於傳統型橋接器的 VCLibs 架構套件。 如需這些案例的詳細資訊，請參閱[在 Centennial 專案中使用 Visual C++ 執行階段](https://blogs.msdn.microsoft.com/vcblog/2016/07/07/using-visual-c-runtime-in-centennial-project/)。

  **架構套件**：

  * [傳統型橋接器的 VC 14.0 framework 套件](https://www.microsoft.com/download/details.aspx?id=53175)
  * [傳統型橋接器的 VC 12.0 framework 套件](https://www.microsoft.com/download/details.aspx?id=53176)
  * [傳統型橋接器的 VC 11.0 framework 套件](https://www.microsoft.com/download/details.aspx?id=53340)


+ __您的應用程式包含自訂的跳躍清單__。 使用捷徑清單時有幾個問題和注意事項。

    - __作業系統不符合您的應用程式架構。__  捷徑清單目前無法正確運作如果不相符的應用程式和 OS 架構 (例如，x86 應用程式在 x64 上執行 Windows)。 在此階段中，沒有任何因應措施以外重新編譯您的應用程式相符的架構。

    - __您的應用程式建立跳躍清單項目並呼叫[ICustomDestinationList::SetAppID](https://msdn.microsoft.com/library/windows/desktop/dd378403(v=vs.85).aspx)或是[SetCurrentProcessExplicitAppUserModelID](https://msdn.microsoft.com/library/windows/desktop/dd378422(v=vs.85).aspx)__。 請不要透過程式設計方式在程式碼中設定 AppID。 這樣做會造成捷徑清單項目不出現。 如果您的應用程式需要自訂的識別碼，請將它指定使用資訊清單檔案。 請參閱[手動封裝傳統型應用程式](desktop-to-uwp-manual-conversion.md)如需相關指示。 應用程式的 AppID 指定於 *YOUR_PRAID_HERE* 區段中。

    - __您的應用程式新增參考封裝中的可執行檔的跳躍清單 shell 連結__。 您不能從捷徑清單直接啟動套件中的可執行檔 (應用程式專屬 .exe 的絕對路徑例外)。 相反地，註冊應用程式執行別名 （可讓您封裝的桌面應用程式以開始透過關鍵字，如同它已在路徑上），並改為設定連結的目標路徑的別名。 如需如何使用 appExecutionAlias 擴充功能的詳細資訊，請參閱[整合您的桌面應用程式與 Windows 10](desktop-to-uwp-extensions.md)。 請注意，如果您想讓捷徑清單中連結的資產符合原始 .exe，則需要使用 [**SetIconLocation**](https://msdn.microsoft.com/library/windows/desktop/bb761047(v=vs.85).aspx) 以及含 PKEY_Title 的顯示名稱來設定資產 (例如圖示)，就像其他自訂項目一樣。

    - __您的應用程式新增跳躍清單項目絕對路徑參考的應用程式封裝中的資產__。 更新應用程式的套件，變更資產 （例如圖示、 文件、 可執行檔等等） 的位置時，可能會變更應用程式的安裝路徑。 如果跳躍清單項目參考絕對路徑，這類資產，則應用程式應該重新整理其捷徑清單會定期 （例如在應用程式上啟動） 以確保正確解析路徑。 或是改成使用 UWP [**Windows.UI.StartScreen.JumpList**](https://msdn.microsoft.com/library/windows/apps/windows.ui.startscreen.jumplist.aspx) API，讓您可以使用套件相關 ms-resource URI 配置 (也可感知語言、DPI 和高對比) 來參考字串和影像資產。

+ __您的應用程式開始執行工作的公用程式__。 避免啟動命令公用程式，例如 PowerShell 和 Cmd.exe。 事實上，如果使用者安裝到執行 Windows 10 S 的系統上的應用程式，然後您的應用程式無法完全啟動它們。 這可能會因為所有的應用程式提交至 Microsoft Store 必須相容於 Windows 10 S 封鎖應用程式提交至 Microsoft Store

啟動公用程式通常可以提供一個便利的方式來取得作業系統資訊、存取登錄，或存取系統功能。 然而，您也可以改用 UWP API 來完成這類工作。 這些 Api 是更好的效能，因為他們不需要個別的可執行檔，若要執行，但更重要的是，能夠保持應用程式，使其無法到達外部封裝。 應用程式的設計保持一致，以隔離，信任和隨附的封裝，以及您的應用程式會如預期般地執行 Windows 10 s。 系統上的應用程式的安全性

+ __您的應用程式裝載增益集、 外掛程式或延伸模組__。   很多時候，只要延伸模組尚未封裝，且其本身是以完全信任的模式安裝的，COM 式的延伸模組都會繼續運作。 這是因為安裝程式可以使用其完全信任功能修改登錄，並將擴充檔，只要主機應用程式需要以找出它們。

   不過，如果這些延伸模組會封裝，而且然後安裝為 Windows 應用程式套件，它們不會運作，因為 （主應用程式和擴充功能） 的每個套件將會與彼此隔離。 若要深入了解如何應用程式是分開的系統，請參閱 <<c0> [ 傳統型橋接器在幕後](desktop-to-uwp-behind-the-scenes.md)。

 所以使用者安裝至執行 Windows 10 S 系統的應用程式和延伸模組，都必須是以 Windows 應用程式套件進行安裝。 因此如果您想要封裝您的擴充功能，或想要鼓勵您封裝的參與者，請考慮如何裝載應用程式套件與任何延伸模組套件之間的通訊可能會幫助。 要達到這個目的的其中一個可能的方式，就是使用[應用程式服務](../launch-resume/app-services.md)。

+ __您的應用程式產生的程式碼__。 您的應用程式可以在記憶體中，產生程式碼，它會取用，但避免程式碼產生的寫入至磁碟，因為 Windows 應用程式認證程序無法驗證該應用程式提交前的程式碼。 此外，寫入磁碟中的程式碼的應用程式將不會執行在執行 Windows 10 s。 系統上的正確這可能會因為所有的應用程式提交至 Microsoft Store 必須相容於 Windows 10 S 封鎖應用程式提交至 Microsoft Store

>[!IMPORTANT]
> 建立 Windows 應用程式套件之後，請測試您的應用程式，以確保它運作正常系統上執行 Windows 10 s。所有的應用程式提交至 Microsoft Store 必須相容於 Windows 10 s。 應用程式不相容，不會接受存放區中。 請參閱[針對 Windows 10 S 測試您的 Windows 應用程式](desktop-to-uwp-test-windows-s.md)。

## <a name="next-steps"></a>後續步驟

**尋找問題的解答**

有任何問題嗎？ 請在 Stack Overflow 上發問。 我們的團隊會監視這些[標記](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge)。 您也可以[在此處](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D)詢問我們。

**提供意見反應或提出功能建議**

請參閱 [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial)。

**建立桌面應用程式的 Windows 應用程式套件**

請參閱[建立 Windows 應用程式套件](desktop-to-uwp-root.md#convert)
