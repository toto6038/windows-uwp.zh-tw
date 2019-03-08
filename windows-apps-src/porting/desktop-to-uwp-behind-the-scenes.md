---
Description: 本文提供傳統型橋接器背後運作方式的深入解析。
title: 傳統型橋接器的幕後作業
ms.date: 05/25/2017
ms.topic: article
keywords: Windows 10, UWP
ms.assetid: a399fae9-122c-46c4-a1dc-a1a241e5547a
ms.localizationpriority: medium
ms.openlocfilehash: 644139f800caa2ead61ce19d63d4408c01575025
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57603643"
---
# <a name="behind-the-scenes-of-your-packaged-desktop-application"></a>在幕後的桌面應用程式封裝

這篇文章提供更深入的了解當您建立桌面應用程式的 Windows 應用程式套件時，檔案和登錄項目會發生什麼事。

現代的主要目標是封裝的將從系統狀態盡量維持與其他應用程式的相容性的應用程式狀態。 橋接器會透過將應用程式放入通用 Windows 平台 (UWP) 套件中，然後偵測它在執行階段對檔案系統與登錄所做的某些變更並重新導向，來達到此目標。

您建立您的桌面應用程式套件是僅限桌面、 完全信任應用程式，並不是虛擬化或沙箱化。 這可讓它們以和傳統桌面應用程式的相同方式與其他 App 互動。

## <a name="installation"></a>安裝

應用程式套件會安裝於 *C:\Program Files\WindowsApps\package_name* 之下，可執行檔名為 *app_name.exe*。 每個套件資料夾都包含資訊清單 (名稱為 AppxManifest.xml)，其中包含已封裝應用程式的特殊 XML 命名空間。 該資訊清單檔案內部是一個 ```<EntryPoint>``` 項目，它會參考完全信任的 App。 當啟動該應用程式時，它不會在應用程式容器內執行，但改為執行與使用者如往常地。

部署之後，套件檔案會由作業系統標示為唯讀並嚴密地鎖定。 如果這些檔案遭到竄改，Windows 會防止這些 App 啟動。

## <a name="file-system"></a>檔案系統

若要包含應用程式的狀態，會擷取應用程式對 AppData 的變更。 所有寫入到使用者 [AppData] 資料夾 (例如 *C:\Users\user_name\AppData*) 的動作 (包括建立、刪除和更新) 都會在寫入時複製到私人的各個使用者、各個 App 位置。 這會建立封裝的應用程式正在編輯實際 AppData，它實際上修改私用複本時的假象。 透過這種方式將寫入重新導向，系統就可以追蹤 App 執行的所有檔案修改。 這可讓應用程式解除安裝時，清除這些檔案系統，減少系統 「 rot"，並提供更好的應用程式移除因此使用者體驗。

除了將重新導向 AppData，Windows 的已知資料夾 （System32、 Program Files (x86) 等等） 以動態方式會與應用程式套件中的對應目錄合併。 每個套件在其根目錄都會包含名為「VFS」的資料夾。 在 VFS 目錄中讀取的任何目錄或檔案，都會在執行階段與各自的原生對應項目合併。 例如，應用程式可能會包含*C:\Program Files\WindowsApps\package_name\VFS\SystemX86\vc10.dll*屬於其應用程式套件，但該檔案會出現安裝*C:\Windows\System32\vc10.dll*。  這樣可維持與可能預期檔案位於非套件位置的傳統型應用程式的相容性。

不允許在應用程式套件中對檔案/資料夾進行寫入。 只要使用者具有權限，橋接器就會略過並允許對不屬於套件的檔案和資料夾進行的寫入作業。

### <a name="common-operations"></a>常見作業

這個簡短的參考資料表說明了常見的檔案系統作業及橋接器處理作業的方式。

操作 | 結果 | 範例
:--- | :--- | :---
讀取或列舉已知的 Windows 檔案或資料夾 | 動態合併 *C:\Program Files\package_name\VFS\well_known_folder* 與本機系統對應項目。 | 讀取 *C:\Windows\System32* 會傳回 *C:\Windows\System32* 的內容，加上 *C:\Program Files\WindowsApps\package_name\VFS\SystemX86* 的內容。
在 AppData 下寫入 | 寫入時複製到各個使用者、各個 App 位置。 | AppData 通常位於 *C:\Users\user_name\AppData*。  
在套件內部寫入 | 不允許。 套件是唯讀的。 | 不允許在 *C:\Program Files\WindowsApps\package_name* 之下寫入。
在套件外部寫入 | 若使用者具有權限則會允許。 | 如果套件不包含 *C:\Program Files\WindowsApps\package_name\VFS\SystemX86\foo.dll* 且使用者具有權限，就會允許寫入到 *C:\Windows\System32\foo.dll*。

### <a name="packaged-vfs-locations"></a>已封裝的 VFS 位置

下表會顯示其中做為套件一部分傳送的檔案會在 App 適用的系統上重疊。 您的應用程式將會察覺到實際上為內部重新導向的位置時，會列出的系統位置中，這些檔案*C:\Program Files\WindowsApps\package_name\VFS*。 FOLDERID 位置是來自 [**KNOWNFOLDERID**](https://msdn.microsoft.com/library/windows/desktop/dd378457.aspx) 常數。

系統位置 | 重新導向位置 (在 [PackageRoot] \VFS\) | 有效的架構
 :--- | :--- | :---
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

## <a name="registry"></a>登錄

應用程式套件包含 registry.dat 檔案，它會做為實際登錄中 *HKLM\Software* 的邏輯對等項目。 在執行階段，此虛擬登錄會將此登錄區的內容合併至原生系統登錄區，以提供兩者的單一檢視。 例如，如果 registry.dat 包含單一機碼 "Foo"，則執行階段的 *HKLM\Software* 讀取也會顯示包含 "Foo" (除了所有原生系統機碼之外)。

只有 *HKLM\Software* 之下的機碼是套件的一部分；*HKCU* 或登錄的其他部分之下的機碼則不是。 不允許對套件中的機碼或值進行寫入。 寫入索引鍵或值不允許封裝的一部分，只要使用者有權限。

在 HKCU 之下的所有寫入會在寫入時複製到私人的各個使用者、各個 App 位置。 傳統上來說，解除安裝程式無法清除 *HKEY_CURRENT_USER*，因為已登出使用者的登錄資料無法卸載且無法使用。

所有 writes 會保存在封裝升級期間，並完全移除應用程式時，才刪除。

### <a name="common-operations"></a>常見作業

這個簡短的參考資料表說明了常見的登錄作業及橋接器處理作業的方式。

操作 | 結果 | 範例
:--- | :--- | :---
讀取或列舉 *HKLM\Software* | 套件登錄區與本機系統對應項目的動態合併。 | 如果 registry.dat 包含單一機碼 "Foo"，在執行階段讀取 *HKLM\Software* 時將會顯示 *HKLM\Software* 加上 *HKLM\Software\Foo* 兩者的內容。
在 HKCU 下寫入 | 寫入時會複製到各個使用者、各個 App 的私人位置。 | 與 AppData 的檔案處理方式相同。
在套件內部寫入。 | 不允許。 套件是唯讀的。 | 若相對應的機碼/值存在於套件登錄區中，就不允許在 *HKLM\Software* 之下寫入。
在套件外部寫入 | 橋接器會略過。 若使用者具有權限則會允許。 | 只要套件登錄區中沒有相對應的機碼/值存在，且使用者擁有正確的存取權限，就會允許在 *HKLM\Software* 之下寫入。

## <a name="uninstallation"></a>解除安裝

由使用者解除安裝封裝時，所有檔案和資料夾都位於*C:\Program Files\WindowsApps\package_name*會移除，以及 AppData 或登錄期間所擷取的任何重新導向寫入封裝程序。

## <a name="next-steps"></a>後續步驟

**尋找問題的解答**

有任何問題嗎？ 請在 Stack Overflow 上發問。 我們的團隊會監視這些[標記](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge)。 您也可以[在此處](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D)詢問我們。

**提供意見反應或提出功能建議**

請參閱 [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial)。
