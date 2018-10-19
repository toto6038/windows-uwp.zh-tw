---
author: PatrickFarley
ms.assetid: 1526FF4B-9E68-458A-B002-0A5F3A9A81FD
title: Windows 應用程式認證套件測試
description: Windows 應用程式認證套件包含一些測試，可協助確保您的應用程式已準備好可以在 Microsoft Store 上發佈。
ms.author: pafarley
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10，uwp，應用程式認證
ms.localizationpriority: medium
ms.openlocfilehash: 49ecc472c8c1d4adebd8376fce9d2d5e6e2a955e
ms.sourcegitcommit: e16c9845b52d5bd43fc02bbe92296a9682d96926
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/19/2018
ms.locfileid: "4964229"
---
# <a name="windows-app-certification-kit-tests"></a>Windows 應用程式認證套件測試


[Windows 應用程式認證套件](windows-app-certification-kit.md)包含一些測試，可協助確保您的應用程式已準備好發佈至 Microsoft Store。 測試如下所示使用他們的準則的詳細資訊，以及建議在失敗的動作。

## <a name="deployment-and-launch-tests"></a>部署和啟動測試

在認證測試期間監視 app，記錄該 app 何時發生當機或停滯情形。

### <a name="background"></a>背景

停止回應或當機的應用程式會導致使用者遺失資料並產生不良的使用經驗。

我們希望 app 即使不使用 Windows 相容模式、AppHelp 訊息和/或相容性修正程式，也可以完全正常運作。

app 不可以在 HKEY\-LOCAL\-MACHINE\\Software\\Microsoft\\Windows NT\\CurrentVersion\\Windows\\AppInit\-DLLs 登錄機碼中列出要載入的 DLL。

### <a name="test-details"></a>測試詳細資料

我們會透過認證測試來測試 app 的復原能力和穩定性。

Windows 應用程式認證套件會呼叫 [**IApplicationActivationManager::ActivateApplication**](https://msdn.microsoft.com/library/windows/desktop/Hh706903) 以啟動 app。 為了讓 **ActivateApplication** 啟動 app，必須啟用使用者帳戶控制 (UAC)，而且螢幕解析度至少必須為 1024 x 768 或 768 x 1024。 如果任一條件不符合，您的 app 就無法通過這個測試。

### <a name="corrective-actions"></a>修正動作

確定測試電腦上的 UAC 已啟用。

確定您在螢幕夠大的電腦上執行測試。

如果您的 app 無法啟動，但測試平台符合 [**ActivateApplication**](https://msdn.microsoft.com/library/windows/desktop/Hh706903) 的先決條件，則您可以檢閱啟用事件記錄檔以疑難排解問題。 在事件記錄檔中找到這些項目：

1.  開啟 eventvwr.exe，並瀏覽到 Application and Services Log\\Microsoft\\Windows\\Immersive-Shell 資料夾。
2.  篩選檢視，顯示事件識別碼：5900-6000。
3.  查閱記錄項目，尋找說明為什麼應用程式無法啟動的資訊。

疑難排解有問題的檔案，並尋找和修正問題。 重新建置並重新測試應用程式。 您也可以檢查 Windows 應用程式認證套件記錄檔資料夾中是否已產生可用來偵錯應用程式的傾印檔案。

## <a name="platform-version-launch-test"></a>平台版本啟動測試

檢查 Windows 應用程式是否能在未來版本的作業系統執行。 此測試過去只適用於「傳統型 app」工作流程，但現在也適用於「市集」和通用 Windows 平台 (UWP) 工作流程。

### <a name="background"></a>背景

作業系統版本資訊的用途限於 Microsoft 網上商店。 這常被 app 錯誤地用於檢查作業系統版本，使 app 可以提供使用者作業系統版本特定的功能。

### <a name="test-details"></a>測試詳細資料

Windows 應用程式認證套件會使用 HighVersionLie 偵測應用程式如何檢查作業系統版本。 如果應用程式當機，就無法通過這項測試。

### <a name="corrective-action"></a>修正動作

應用程式應該使用「版本 API」協助程式函式進行檢查。 如需詳細資訊，請參閱[作業系統版本](https://msdn.microsoft.com/library/windows/desktop/ms724832)。

## <a name="background-tasks-cancellation-handler-validation"></a>背景工作取消處理常式驗證

這會驗證 app 是否有已宣告之背景工作的取消處理常式。 必須有當工作取消時會呼叫的專用函式。 這項測試只適用於已部署的應用程式。

### <a name="background"></a>背景

市集應用程式可以登錄一個在背景執行的處理程序。 例如，電子郵件應用程式可能會不時對伺服器進行 Ping。 但如果作業系統需要這些資源，它將會取消背景工作，且應用程式應順利處理這項取消。 若應用程式沒有取消處理常式，當使用者嘗試關閉應用程式時，應用程式可能會當機或無法關閉。

### <a name="test-details"></a>測試詳細資料

應用程式會啟動、暫停，然後應用程式的非背景部分會終止。 接著，與應用程式關聯的背景工作會被取消。 最後會檢查作業系統的狀態，如果應用程式仍在執行，它將無法通過此測試。

### <a name="corrective-action"></a>修正動作

新增取消處理常式到 app。 如需詳細資訊，請參閱[使用背景工作支援 app](https://msdn.microsoft.com/library/windows/apps/Mt299103)。

## <a name="app-count"></a>應用程式計數

這會確認一個應用程式套件 (APPX、應用程式套件組合) 包含一個應用程式。 這在套件中變更為獨立的測試。

### <a name="background"></a>背景

這項測試是根據「市集原則」而實作。

### <a name="test-details"></a>測試詳細資料

針對 Windows Phone 8.1 App，測試會確認套件組合中的 appx 套件總數為 &lt; 512、套件組合中只有一個主套件，以及套件組合中的主套件架構已標示為 ARM 或中性。

針對 Windows 10 App，測試會驗證套件組合版本中的版本號碼已設定為 0。

### <a name="corrective-action"></a>修正動作

請確定應用程式套件和套件組合符合上述「測試詳細資料」中的需求。

## <a name="app-manifest-compliance-test"></a>應用程式資訊清單規範測試

測試應用程式資訊清單的內容，確定內容正確無誤。

### <a name="background"></a>背景

應用程式必須包含格式正確的應用程式資訊清單。

### <a name="test-details"></a>測試詳細資料

檢查 App 資訊清單，確認內容是正確的，如 [App 套件需求](https://msdn.microsoft.com/library/windows/apps/Mt148525)中所述。

-   **副檔名與通訊協定**

    您的應用程式可以宣告它要產生關聯的副檔名。 如使用不當，應用程式可能會宣告大量的副檔名，而其中的大多數可能甚至不會用到，因而造成不佳的使用者經驗。 這個測試將新增一項檢查，以限制應用程式可以產生關聯的副檔名數目。

-   **架構相依性規則**

    這個測試會強制要求 app 與 UWP 有適當的相依性。 如果有不適當的相依性，這個測試就會失敗。

    如果 app 套用的作業系統版本與建立架構相依性的作業系統版本不符，測試將會失敗。 如果應用程式參照任何預覽版本的架構 DLL，則測試也會失敗。

-   **處理程序間通訊 (IPC) 驗證**

    這項測試會強制執行的 UWP 應用程式不會在桌面元件的應用程式容器外部與通訊的需求。 處理程序間通訊僅適用於側載 App。 將 [**ActivatableClassAttribute**](https://msdn.microsoft.com/library/windows/apps/BR211414) 的名稱指定為 "DesktopApplicationPath" 的 App 將無法通過這個測試。

### <a name="corrective-action"></a>修正動作

按照 [App 套件需求](https://msdn.microsoft.com/library/windows/apps/Mt148525)中所述的需求來檢閱 App 的資訊清單。

## <a name="windows-security-features-test"></a>Windows 安全性功能測試

### <a name="background"></a>背景

變更預設的 Windows 安全性保護會讓客戶面臨較高的風險。

### <a name="test-details"></a>測試詳細資料

執行 [BinScope 二元分析器](#binscope-binary-analyzer-tests)以測試 app 的安全性。

BinScope 二元分析器測試會檢驗應用程式的二進位檔，檢查編碼和建置做法是否讓應用程式較不容易受到攻擊或是做為攻擊媒介。

BinScope 二元分析器測試會檢查是否正確使用下列安全性相關的功能：

-   BinScope 二元分析器測試
-   私用程式碼簽署

### <a name="binscope-binary-analyzer-tests"></a>BinScope 二元分析器測試

[BinScope 二元分析器](https://www.microsoft.com/en-us/download/details.aspx?id=44995)測試會檢驗 app 的二進位檔，檢查編碼和建置做法是否讓 app 較不容易受到攻擊或是做為攻擊媒介。

BinScope 二元分析器測試會檢查是否正確使用下列安全性相關的功能：

-   [AllowPartiallyTrustedCallersAttribute](#binscope-1)
-   [/SafeSEH 例外狀況處理保護](#binscope-2)
-   [資料執行防止](#binscope-3)
-   [位址空間配置隨機載入](#binscope-4)
-   [讀取/寫入共用的 PE 區段](#binscope-5)
-   [AppContainerCheck](#appcontainercheck)
-   [ExecutableImportsCheck](#binscope-7)
-   [WXCheck](#binscope-8)

### <a name="span-idbinscope-1spanallowpartiallytrustedcallersattribute"></a><span id="binscope-1"></span>AllowPartiallyTrustedCallersAttribute

**Windows 應用程式認證套件錯誤訊息：** APTCACheck 測試失敗

AllowPartiallyTrustedCallersAttribute (APTCA) 屬性可從已簽署組件中部分信任的程式碼，存取可完全信任的程式碼。 當您對組件套用 APTCA 屬性時，部分信任的呼叫者可以在該組件存留期內存取組件，如此會危害安全性。

**如果您的 app 未通過這個測試，應該怎麼辦？**

請勿在強式命名的組件上使用 APTCA 屬性，除非您的專案需要，而且您充分了解風險。 若有必要，請務必使用適當的程式碼存取安全性要求來保護所有 API。 當組件是通用 Windows 平台 (UWP) app 的一部分時，APTCA 沒有任何作用。

**備註**

這個測試只會在 Managed 程式碼 (C#、.NET 等) 執行。

### <a name="span-idbinscope-2spansafeseh-exception-handling-protection"></a><span id="binscope-2"></span>/SafeSEH 例外狀況處理保護

**Windows 應用程式認證套件錯誤訊息：** SafeSEHCheck 測試失敗

當應用程式發生像是除以零錯誤的例外狀況時，例外處理常式就會執行。 因為呼叫函式時，在堆疊儲存例外處理常式的位址，所以如果有些惡意軟體意欲覆寫該堆疊，就會讓緩衝區溢位攻擊者有機可趁。

**如果您的 app 未通過這個測試，應該怎麼辦？**

建置您的應用程式時，在連結器命令中啟用 /SAFESEH 選項。 在 Visual Studio 的發行組態中，這個選項預設會處於開啟。 針對您應用程式中的所有可執行檔模組，確認建置指示中的這個選項已經啟用。

**備註**

測試不會在 64 位元的二進位檔或 ARM 晶片組二進位檔上執行，原因是兩者不會在堆疊上儲存例外處理常式位址。

### <a name="span-idbinscope-3spandata-execution-prevention"></a><span id="binscope-3"></span>資料執行防止

**Windows 應用程式認證套件錯誤訊息：** NXCheck 測試失敗

這個測試會確認應用程式不會執行儲存在資料區段中的程式碼。

**如果您的 app 未通過這個測試，應該怎麼辦？**

建置您的應用程式時，在連結器命令中啟用 /NXCOMPAT 選項。 在支援資料執行防止 (DEP) 的連結器版本中，這個選項預設會處於開啟。

**備註**

建議您在支援 DEP 的 CPU 上測試應用程式，並修正由 DEP 所導致的任何失敗。

### <a name="span-idbinscope-4spanaddress-space-layout-randomization"></a><span id="binscope-4"></span>位址空間配置隨機載入

**Windows 應用程式認證套件錯誤訊息：** DBCheck 測試失敗

位址空間配置隨機載入 (ASLR) 會將可執行檔映像載入記憶體中無法預期的位置，使惡意軟體難以預測會在哪個特定虛擬位址載入程式來運作。 您的應用程式與其所使用的所有元件都必須支援 ASLR。

**如果您的 app 未通過這個測試，應該怎麼辦？**

建置您的應用程式時，在連結器命令中啟用 /DYNAMICBASE 選項。 確認您應用程式使用的所有模組也都使用這個連結器選項。

**備註**

通常 ASLR 不會影響效能。 但在某些情況下，可稍微改善 32 位元系統上的效能。 系統若將許多影像載入多個不同的記憶體位置而發生嚴重壅塞，可能會使效能降低。

這個測試只能在以 Unmanaged 語言 (如 C 或 C++) 撰寫的應用程式上執行。

### <a name="span-idbinscope-5spanreadwrite-shared-pe-section"></a><span id="binscope-5"></span>讀取/寫入共用的 PE 區段

**Windows 應用程式認證套件錯誤訊息：** SharedSectionsCheck 測試失敗。

含有標示為共用可寫入區段的二進位檔便是一個安全性威脅。 除非必要，否則不要建置含有共用可寫入區段的應用程式。 使用 [**CreateFileMapping**](https://msdn.microsoft.com/library/windows/desktop/Aa366537) 或 [**MapViewOfFile**](https://msdn.microsoft.com/library/windows/desktop/Aa366761) 建立有適當安全保護的共用記憶體物件。

**如果您的 app 未通過這個測試，應該怎麼辦？**

從 app 中移除任何共用的區段，然後搭配適當的安全性屬性來呼叫 [**CreateFileMapping**](https://msdn.microsoft.com/library/windows/desktop/Aa366537) 或 [**MapViewOfFile**](https://msdn.microsoft.com/library/windows/desktop/Aa366761) 以建立共用的記憶體物件，然後重新建置您的 app。

**備註**

這個測試只能在以 Unmanaged 語言 (如 C 或 C++) 撰寫的應用程式上執行。

### <a name="appcontainercheck"></a>AppContainerCheck

**Windows 應用程式認證套件錯誤訊息：** AppContainerCheck 測試失敗。

AppContainerCheck 會確認可執行二進位檔的可攜式執行檔 (PE) 標頭中的 **appcontainer** 位元已設定。 app 必須在所有 .exe 檔案和所有 Unmanaged DLL 上設定 **appcontainer** 位元才能正確執行。

**如果您的 app 未通過這個測試，應該怎麼辦？**

如果原始可執行檔未通過這個測試，請確定您使用了最新的編譯器和連結器來建立檔案，並在連結器上使用 */appcontainer* 旗標。

如果 managed 可執行檔未通過這個測試，請確定您使用的最新的編譯器和連結器，例如 Microsoft Visual Studio 中，來建置 UWP 應用程式。

**備註**

這個測試會在所有 .exe 檔案和 Unmanaged DLL 上執行。

### <a name="span-idbinscope-7spanexecutableimportscheck"></a><span id="binscope-7"></span>ExecutableImportsCheck

**Windows 應用程式認證套件錯誤訊息：** ExecutableImportsCheck 測試失敗。

如果可攜式執行檔 (PE) 映像的匯入表格被放置到可執行程式碼區段中，就無法通過這個測試。 如果您將 Visual C++ 連結器的 */merge* 旗標設成 */merge:.rdata=.text* 以對 PE 映像啟用 .rdata 合併，就會發生這種情形。

**如果您的 app 未通過這個測試，應該怎麼辦？**

不要將匯入表格合併到可執行程式碼區段中。 確定 Visual C++ 連結器的 */merge* 旗標沒有設定為將 ".rdata" 區段合併到程式碼區段中。

**備註**

這個測試會在完全 Managed 組件以外的所有二進位程式碼上執行。

### <a name="span-idbinscope-8spanwxcheck"></a><span id="binscope-8"></span>WXCheck

**Windows 應用程式認證套件錯誤訊息：** WXCheck 測試失敗。

這個檢查有助於確保二進位檔沒有任何對應為可寫入或可執行的頁面。 如果二進位檔有可寫入和可執行的區段，或如果二進位檔的 *SectionAlignment* 小於 *PAGE\-SIZE*，就會發生這種情形。

**如果您的 app 未通過這個測試，應該怎麼辦？**

確認二進位檔沒有可寫入或可執行的區段，而且二進位檔的 *SectionAlignment* 值至少等於其 *PAGE\-SIZE*。

**備註**

這個測試會在所有 .exe 檔案和原生的 Unmanaged DLL 上執行。

如果可執行檔在建置時啟用了 編輯後繼續 (/ZI)，則可執行檔便可能有可寫入和可執行的區段。 停用 \[編輯後繼續\] 即可不顯示無效的區段。

可執行檔的 *SectionAlignment* 預設為 *PAGE\-SIZE*。

### <a name="private-code-signing"></a>私用程式碼簽署

測試私用程式碼簽署二進位檔是否存在於應用程式套件。

### <a name="background"></a>背景

私用程式碼簽署檔案應該保持私用，因為若遭到洩露，可能會被用於惡意用途。

### <a name="test-details"></a>測試詳細資料

測試應用程式套件內的檔案是否存在 .pfx 或.snk，以指出已包含私用簽署金鑰。

### <a name="corrective-actions"></a>修正動作

從套件中移除任何私用程式碼簽署金鑰 (例如，.pfx 和 .snk 檔案)。

## <a name="supported-api-test"></a>支援的 API 測試

測試 app 是否使用不相容的 API。

### <a name="background"></a>背景

應用程式必須使用 Api，適用於 UWP app （Windows 執行階段或支援的 Win32 Api），Microsoft Store 進行認證。 這個測試也會識別 Managed 二進位檔案相依於核准的設定檔外部函式的狀況。

### <a name="test-details"></a>測試詳細資料

-   確認應用程式套件內的每個二進位檔案都相依於 Win32 API 所支援的 UWP 應用程式開發檢查的二進位檔匯入位址表。
-   確認應用程式套件內的每個受管理二進位檔案不會相依於核准的設定檔外部的函式。

### <a name="corrective-actions"></a>修正動作

確定 app 是編譯為發行組建而不是偵錯組建。

> **注意：** 應用程式的偵錯組建即使只[適用於 UWP app 的 Api](https://msdn.microsoft.com/library/windows/apps/xaml/bg124285.aspx)會使用應用程式，無法通過這個測試。

檢閱錯誤訊息，找出 API 不是[適用於 UWP app 的 API](https://msdn.microsoft.com/library/windows/apps/xaml/bg124285.aspx)應用程式使用。

> **注意：** 在偵錯組態中內建的 c + + app 無法通過這個測試，即使組態只使用來自 Windows SDK 的 Api，適用於 UWP app。 請參閱， [UWP app 中的 Windows Api 的替代方法](http://go.microsoft.com/fwlink/p/?LinkID=244022)，如需詳細資訊。

## <a name="performance-tests"></a>效能測試

應用程式必須快速回應使用者的互動及系統命令，才能提供快速且流暢的使用者經驗。

執行測試之電腦的特性會影響測試結果。 設定應用程式認證的效能測試閾值，讓低功率電腦能夠符合客戶預期的快速和流暢使用經驗。 若要判斷應用程式的效能，建議您在低功率電腦上進行測試，例如採用 Intel Atom 處理器且具備 1366x768 (或更高) 螢幕解析度與旋轉式硬碟 (而非固態硬碟) 的電腦。

### <a name="bytecode-generation"></a>位元組程式碼產生

當效能最佳化加快 JavaScript 執行時間時，結尾為 .js 副檔名的 JavaScript 檔案會在部署應用程式時產生位元組程式碼。 這個最佳化作業會大幅改善 JavaScript 操作的啟動和持續執行時間。

### <a name="test-details"></a>測試詳細資料

檢查應用程式部署，確認所有 .js 檔案都已轉換成位元組程式碼。

### <a name="corrective-action"></a>修正動作

如果這個測試失敗，解決問題時請考量下列各項：

-   確認是否啟用事件記錄。
-   確認所有 JavaScript 檔案的語法是否有效。
-   確認已解除安裝應用程式的所有先前版本。
-   從應用程式套件中排除已識別的檔案。

### <a name="optimized-binding-references"></a>最佳化的繫結參考

使用繫結時，WinJS.Binding.optimizeBindingReferences 應設定為 true 以最佳化記憶體使用量。

### <a name="test-details"></a>測試詳細資料

確認 WinJS.Binding.optimizeBindingReferences 的值。

### <a name="corrective-action"></a>修正動作

將 app JavaScript 中的 WinJS.Binding.optimizeBindingReferences 設為 **true**。

## <a name="app-manifest-resources-test"></a>app 資訊清單資源測試

### <a name="app-resources-validation"></a>應用程式資源驗證

如果應用程式資訊清單中宣告的字串或影像不正確，就無法安裝應用程式。 如果應用程式安裝時包含這些錯誤，就無法正確顯示應用程式的標誌或所使用的其他影像。

### <a name="test-details"></a>測試詳細資料

檢查應用程式資訊清單中定義的資源，確定它們存在並且有效。

### <a name="corrective-action"></a>修正動作

使用下表做為指引。

<table>
<tr><th>錯誤訊息</th><th>註解</th></tr>
<tr><td>
<p>影像 {image name} 同時定義 Scale 和 TargetSize 限定詞二者；一次只能定義一個限定詞。</p>
</td><td>
<p>您可以針對不同的解析度自訂影像。</p>
<p>在實際訊息中，{image name} 包含有錯誤的影像名稱。</p>
<p> 請確定每個影像都將 Scale 或 TargetSize 定義為限定詞。</p>
</td></tr>
<tr><td>
<p>影像 {image name} 不符合大小限制。</p>
</td><td>
<p>請確定所有應用程式影像都遵守適當的大小限制。</p>
<p>在實際訊息中，{image name} 包含有錯誤的影像名稱。</p>
</td></tr>
<tr><td>
<p>套件中沒有影像 {image name}。</p>
</td><td>
<p>缺少必要的影像。</p>
<p>在實際訊息中，{image name} 包含缺少的影像名稱。</p>
</td></tr>
<tr><td>
<p>影像 {image name} 不是有效的影像檔。</p>
</td><td>
<p>確認所有應用程式影像都遵守適當的檔案格式類型限制。</p>
<p>在實際訊息中，{image name} 包含無效的影像名稱。</p>
</td></tr>
<tr><td>
<p>影像 "BadgeLogo" 在位置 (x, y) 的 ABGR 值 {value} 無效。 像素必須是白色 (##FFFFFF) 或透明 (00######)</p>
</td><td>
<p>徽章標誌是出現在徽章通知旁邊的影像，用以在鎖定畫面識別應用程式。 這個影像必須為單色 (只能包含白色和透明像素)。</p>
<p>在實際訊息中，{value} 包含影像中無效的色彩值。</p>
</td></tr>
<tr><td>
<p>影像 "BadgeLogo" 在位置 (x, y) 的 ABGR 值 {value} 對於高對比白色影像無效。 像素必須是 (##2A2A2A) 或較深，或透明 (00######)。</p>
</td><td>
<p>徽章標誌是出現在徽章通知旁邊的影像，用以在鎖定畫面識別應用程式。   因為在高對比白色中，徽章標誌會出現在白色背景上，所以它必須是正常徽章標誌的深色版本。 在高對比白色中，徽章標誌只能包含比 (##2A2A2A) 深的像素或透明。</p>
<p>在實際訊息中，{value} 包含影像中無效的色彩值。</p>
</td></tr>
<tr><td>
<p>影像至少必須定義一個不含 TargetSize 限定詞的變數。 它必須定義 Scale 限定詞，或不指定 Scale 和 TargetSize，預設值為 Scale-100。</p>
</td><td>
<p>如需詳細資訊，請參閱 <a href="https://msdn.microsoft.com/library/windows/apps/xaml/dn958435.aspx">UWP app 回應設計 101</a> 和 <a href="https://msdn.microsoft.com/library/windows/apps/xaml/hh465241.aspx">app 資源的指導方針</a>。</p>
</td></tr>
<tr><td>
<p>套件缺少 "resources.pri" 檔案。</p>
</td><td>
<p>如果您的 app 資訊清單中有可當地語系化的內容，app 套件中務必包含有效的 resources.pri 檔案。</p>
</td></tr>
<tr><td>
<p>"resources.pri" 檔案必須包含資源對應，且名稱符合套件名稱 {package full name}</p>
</td><td>
<p>如果資訊清單已變更，而 resources.pri 中的資源對應名稱不再符合資訊清單中的套件名稱，就會發生這個錯誤。</p>
<p>在實際訊息中，{package full name} 包含 resources.pri 必須包含的套件名稱。</p>
<p>若要更正此錯誤，您需要重建 resources.pri，最簡單的方式就是重建 app 的套件。</p>
</td></tr>
<tr><td>
<p>"resources.pri" 檔案不能啟用 AutoMerge。</p>
</td><td>
<p>MakePRI.exe 支援一個稱為 <strong>AutoMerge</strong> 的選項。 <strong>AutoMerge</strong> 的預設值為 <strong>off</strong>。 啟用時，<strong>AutoMerge</strong> 會在執行期間將 app 的語言套件資源合併到單一 resources.pri 中。 我們不建議這樣的應用程式，您要透過 Microsoft 網上商店散布。 透過 Microsoft 網上商店散發應用程式的 resources.pri 必須位於應用程式套件的根目錄，並包含應用程式支援的所有語言參考。</p>
</td></tr>
<tr><td>
<p>字串 {string} 不符合 {number} 個字元的長度上限限制。</p>
</td><td>
<p>請參閱 <a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt148525.aspx">app 套件需求</a>。</p>
<p>在實際訊息中，{string} 會以發生錯誤的字串取代，而 {number} 包含長度上限。</p>
</td></tr>
<tr><td>
<p>字串 {string} 的開頭/結尾不得具有空白字元。</p>
</td><td>
<p>應用程式資訊清單中的元素結構描述不允許前後有空白字元。</p>
<p>在實際訊息中，{string} 會以有錯誤的字串取代。</p>
<p>確定 resources.pri 中的資訊清單欄位沒有任何當地語系化的值前後有空白字元。</p>
</td></tr>
<tr><td>
<p>字串必須為非空白 (長度大於零)</p>
</td><td>
<p>如需詳細資訊，請參閱 <a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt148525.aspx">app 套件需求</a>。</p>
</td></tr>
<tr><td>
<p>"resources.pri" 檔案中沒有指定預設資源。</p>
</td><td>
<p>如需詳細資訊，請參閱 <a href="https://msdn.microsoft.com/library/windows/apps/xaml/hh465241.aspx">app 資源的指導方針</a>。</p>
<p>在預設建置組態中，當產生套件組合、將其他資源放在資源套件中時，Visual Studio 只會在 app 套件中包含縮放比例-200 影像資源。 請確定您包含縮放比例-200 影像資源，或將您的專案設定為包含您所擁有的資源。</p>
</td></tr>
<tr><td>
<p>"resources.pri" 檔案中沒有指定資源值。</p>
</td><td>
<p>請確定 app 資訊清單會在 resource.pri 中定義有效的資源。</p>
</td></tr>
<tr><td>
<p>影像檔 {filename} 必須小於 204800 個位元組。\*\*</p>
</td><td>
<p>請降低所指示之影像的大小。</p>
</td></tr>
<tr><td>
<p>{filename} 檔案不應包含反向對應區段。\*\*</p>
</td><td>
<p>若呼叫 makepri.exe 時在 Visual Studio「F5 偵錯」期間產生反向對應，則可藉由在產生 pri 檔案時，執行 makepri.exe 但不加上 /m 參數來移除它。</p>
</td></tr>
<tr><td colspan="2">
<p>\*\* 指出已在適用於 Windows 8.1 的 Windows 應用程式認證套件 3.3 中新增測試，而且只有在使用該套件版本或更新版本時才適用。</p>
</td></tr>
</table>



 

### <a name="branding-validation"></a>商標驗證

UWP 應用程式應該要完整且功能正常。 使用預設影像 (來自範本或 SDK 範例) 的應用程式會呈現不佳的使用者經驗，而且在市集型錄中也不容易識別。

### <a name="test-details"></a>測試詳細資料

這個測試會驗證應用程式所使用的影像不是來自 SDK 範例或 Visual Studio 的預設影像。

### <a name="corrective-actions"></a>修正動作

以更特別且更能代表您應用程式的影像取代預設影像。

## <a name="debug-configuration-test"></a>偵錯設定測試

測試應用程式，確定它不是偵錯版。

### <a name="background"></a>背景

若要通過 Microsoft Store 認證，應用程式必須不可以針對編譯偵錯，而且不可以參照可執行檔檔案的偵錯版本。 此外，您必須針對您的應用程式建置最佳化的程式碼以便通過此測試。

### <a name="test-details"></a>測試詳細資料

測試應用程式，確定不是偵錯組建，而且沒有連結到任何偵錯架構。

### <a name="corrective-actions"></a>修正動作

-   在您提交到 Microsoft 網上商店之前，請為發行組建建置應用程式。
-   確定您已安裝正確的 .NET Framework 版本。
-   確認應用程式並未連結到偵錯版本的架構，且是利用發行版本所建置。 如果這個應用程式包含 .NET 元件，請確認您已安裝正確的 .NET Framework 版本。

## <a name="file-encoding-test"></a>檔案編碼測試

### <a name="utf-8-file-encoding"></a>UTF-8 檔案編碼

### <a name="background"></a>背景

必須使用對應的位元順序標記 (BOM)，以 UTF-8 格式為 HTML、CSS 和 JavaScript 檔案編碼，如此即能從位元組程式碼快取中獲益，並可避免其他執行階段錯誤情況。

### <a name="test-details"></a>測試詳細資料

測試應用程式套件的內容，確定它們使用正確的檔案編碼。

### <a name="corrective-action"></a>修正動作

開啟受影響的檔案，然後從 Visual Studio 的 \[**檔案**\] 功能表中選取 \[**另存新檔**\]。 選取 \[**儲存**\] 按鈕旁的下拉式控制項，然後選取 \[**以編碼方式儲存**\]。 從 \[**進階**\] 儲存選項對話方塊中選擇 \[Unicode (UTF-8 有簽章)\] 選項，然後按一下 \[**確定**\]。

## <a name="direct3d-feature-level-test"></a>Direct3D 功能層級測試

### <a name="direct3d-feature-level-support"></a>Direct3D 功能層級支援

測試 Microsoft Direct3D app，以確定它們不會在具有較舊圖形硬體的裝置上當機。

### <a name="background"></a>背景

Microsoft Store 需要使用 Direct3D 正確呈現，或正常功能層級 9 \-1 圖形卡上的所有應用程式。

因為使用者可以在安裝 app 之後變更裝置中的圖形硬體，如果您選擇高於 9\-1 的最低功能層級，您的 app 必須在啟動時偵測目前的硬體是否符合最低需求。 若不符合最低需求，該 app 必須向使用者顯示訊息，以詳細說明 Direct3D 的需求。 此外，如果客戶在不相容的裝置上下載 app，該 app 必須在啟動時偵測執行環境，並顯示訊息給客戶，以詳細說明需求。

### <a name="test-details"></a>測試詳細資料

這個測試會驗證 app 是否在功能層級 9\-1 上正確顯示。

### <a name="corrective-action"></a>修正動作

請確保您的 app 正確顯示在 Direct3D 功能層級 9\-1 上，即使您希望它在更高層級上執行。 如需詳細資訊，請參閱[針對不同的 Direct3D 功能層級進行開發](http://go.microsoft.com/fwlink/p/?LinkID=253575)。

### <a name="direct3d-trim-after-suspend"></a>暫停後的 Direct3D 修剪

> **注意：** 這項測試僅適用於開發適用於 Windows 8.1 及更新版本的 UWP 應用程式。

### <a name="background"></a>背景

如果 app 未在本身的 Direct3D 裝置上呼叫 [**Trim**](https://msdn.microsoft.com/library/windows/desktop/Dn280346)，該 app 就不會釋放它為較早的 3D 工作所配置的記憶體。 這會增加應用程式由於系統記憶體壓力而終止的風險。

### <a name="test-details"></a>測試詳細資料

檢查 app 是否符合 d3d 需求，確保 app 在暫停回呼時，呼叫新的 [**Trim**](https://msdn.microsoft.com/library/windows/desktop/Dn280346) API。

### <a name="corrective-action"></a>修正動作

每當 App 即將暫停時，都應該在它的 [**IDXGIDevice3**](https://msdn.microsoft.com/library/windows/desktop/Dn280345) 介面上呼叫 [**Trim**](https://msdn.microsoft.com/library/windows/desktop/Dn280346) API。

## <a name="app-capabilities-test"></a>App 功能測試

### <a name="special-use-capabilities"></a>特殊用途功能

### <a name="background"></a>背景

特殊用途功能適用於非常特殊的情況。 僅限公司帳戶使用這些功能。

### <a name="test-details"></a>測試詳細資料

驗證應用程式是否宣告下列任一功能：

-   EnterpriseAuthentication
-   SharedUserCertificates
-   DocumentsLibrary

如果宣告了這些功能的其中之一，測試就會顯示警告給使用者。

### <a name="corrective-actions"></a>修正動作

請考慮移除 app 不需要的特殊用途功能。 此外，這類功能的使用方式需接受其他上架原則審查。

## <a name="windows-runtime-metadata-validation"></a>Windows 執行階段中繼資料驗證

### <a name="background"></a>背景

確保 app 隨附的元件符合 UWP 類型系統。

### <a name="test-details"></a>測試詳細資料

確認套件中的 **.winmd** 檔案符合 UWP 規則。

### <a name="corrective-actions"></a>修正動作

-   **ExclusiveTo 屬性測試：** 確保 UWP 類別不會實作已標示為 ExclusiveTo 其他類別的介面。
-   **類型位置測試：** 確保所有 UWP 類型的中繼資料都位於 app 套件中命名空間相符名稱最長的 winmd 檔案中。
-   **類型名稱區分大小寫測試：** 確保所有 UWP 類型在 app 套件內都具有唯一且不區分大小寫的名稱。 同時也確保 app 套件內的命名空間名稱均未使用 UWP 類型名稱。
-   **類型名稱正確性測試：** 確保全域命名空間或 Windows 最上層命名空間中，不存在任何 UWP 類型。
-   **一般中繼資料正確性測試：** 確保您用來產生類型的編譯器符合最新的 UWP 規格。
-   **屬性測試：** 確保 UWP 類別的所有屬性都有 get 方法 (set 方法為選用)。 針對 UWP 類型的所有屬性，確保 get 方法傳回值的類型與 set 方法輸入參數的類型相符。

## <a name="package-sanity-tests"></a>套件例行性測試

### <a name="platform-appropriate-files-test"></a>平台適用檔案測試

視使用者的處理器架構而定，安裝混合二進位檔案的 app 可能會損毀或無法正確執行。

### <a name="background"></a>背景

這個測試驗證應用程式套件中的二進位檔案是否會發生架構衝突。 應用程式套件不應包含無法在資訊清單中所指定的處理器架構上使用的二進位檔案。 包含不支援的二進位檔案會導致應用程式毀損，或是以非必要方式增加應用程式套件的大小。

### <a name="test-details"></a>測試詳細資料

驗證每個檔案的 PE 標頭中的「位元」都適合與應用程式套件處理器架構宣告交叉參考

### <a name="corrective-action"></a>修正動作

遵循這些指導方針，以確保您的應用程式套件只包含應用程式資訊清單中指定之架構所支援的檔案：

-   如果 app 的目標處理器架構是 Neutral 處理器類型，則應用程式套件不得包含 x86、x64 或 ARM 二進位檔案或映像類型檔案。

-   如果 app 的目標處理器架構是 x86 處理器類型，則應用程式套件必須只能包含 x86 二進位檔案或映像類型檔案。 如果套件包含 x64 或 ARM 二進位檔案或映像類型檔案，這個測試將會失敗。

-   如果 app 的目標處理器架構是 x64 處理器類型，則應用程式套件必須能包含 x64 二進位檔案或映像類型檔案。 請注意，在這個情況下，套件也可以包含 x86 檔案，但是主要的應用程式經驗應該運用 x64 二進位檔案。

    不過，如果套件包含 ARM 二進位檔案或映像類型檔案，或者只包含 x86 二進位檔案或映像類型檔案，則這個測試將會失敗。

-   如果應用程式的目標處理器架構是 ARM 處理器類型，則應用程式套件必須只能包含 ARM 二進位檔案或映像類型檔案。 如果套件包含 x64 或 x86 二進位檔案或映像類型檔案，這個測試將會失敗。

### <a name="supported-directory-structure-test"></a>支援的目錄結構測試

驗證應用程式在安裝期間建立的子目錄長度不超過 MAX\-PATH。

### <a name="background"></a>背景 

OS 元件 (包括 Trident、WWAHost 等) 的檔案系統路徑在內部限制為 MAX\-PATH；若路徑較長，將無法正確運作。

### <a name="test-details"></a>測試詳細資料

確認 App 安裝目錄內的路徑不超過 MAX\-PATH。

### <a name="corrective-action"></a>修正動作

使用較短的目錄結構或檔案名稱。

## <a name="resource-usage-test"></a>資源使用狀況測試

### <a name="winjs-background-task-test"></a>WinJS 背景工作測試

WinJS 背景工作測試可確保 JavaScript 應用程式具備適當的 close 陳述式，以免應用程式耗用電池電力。

### <a name="background"></a>背景 

具備 JavaScript 背景工作的應用程式必須呼叫 Close() 做為其背景工作的最後一個陳述式。 不執行此動作的應用程式可能會使系統無法返回連線待命模式，導致電池電力耗盡。

### <a name="test-details"></a>測試詳細資料

如果應用程式沒有資訊清單中指定的背景工作檔案，將會通過測試。 否則，測試將會剖析應用程式套件中指定的 JavaScript 背景工作檔案，並尋找 Close() 陳述式。 如果找到，將會通過測試；否則測試就會失敗。

### <a name="corrective-action"></a>修正動作

更新背景 JavaScript 程式碼以正確呼叫 Close()。


## <a name="related-topics"></a>相關主題

* [Windows 傳統型橋接器應用程式測試](windows-desktop-bridge-app-tests.md)
* [Microsoft Store 原則](https://msdn.microsoft.com/library/windows/apps/Dn764944)
 
