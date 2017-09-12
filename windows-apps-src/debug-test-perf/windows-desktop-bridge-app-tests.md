---
author: PatrickFarley
ms.assetid: 2f76c520-84a3-4066-8eb3-ecc0ecd198a7
title: "Windows 傳統型橋接器應用程式測試"
description: TBD
ms.author: pafarley
ms.date: 06/30/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: f97b406d534315fcc1128d23059a2a10e6bdb9d2
ms.sourcegitcommit: ca060f051e696da2c1e26e9dd4d2da3fa030103d
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="windows-desktop-bridge-app-tests"></a>Windows 傳統型橋接器應用程式測試

[傳統型橋接器應用程式](https://docs.microsoft.com/en-us/windows/uwp/porting/desktop-to-uwp-root)是已使用[傳統型橋接器](https://developer.microsoft.com/en-us/windows/bridges/desktop)轉換為通用 Windows 平台 (UWP) app 的 Windows 傳統型應用程式。 轉換之後，Windows 傳統型應用程式就會以目標為 Windows10 Desktop 的 UWP app 套件形式 (.appx 或 .appxbundle) 來封裝、提供服務及部署。

## <a name="required-versus-optional-tests"></a>必要與選擇性測試
Windows 傳統型橋接器應用程式有一些選擇性測試的新概念，其僅供參考，並不會用來在 Windows 市集上架期間評估您的應用程式。 建議調查這些測試結果，以製作品質良好的應用程式。 市集上架的整體成功/失敗條件是由必要測試所決定，而非這些選擇性測試。

## <a name="current-optional-tests"></a>目前選擇性測試

### <a name="1-digitally-signed-file-test"></a>1.數位簽署的檔案測試 
**背景 **  
這項測試會確認所有可攜式執行檔 (PE) 都包含有效的簽章。 如果有數位簽署的檔案，即可讓使用者知道軟體是正版。

**測試詳細資料**  
測試會掃描套件中的所有可攜式執行檔，並檢查其標頭是否有簽章。 建議對所有 PE 檔案進行數位簽署。 如果未簽署任何 PE 檔案，則會產生警告。
 
**修正動作**  
一律建議具有數位簽署的檔案。 如需詳細資訊，請參閱[程式碼簽署簡介](https://msdn.microsoft.com/en-us/library/ms537361(v=vs.85).aspx)。

### <a name="2-file-association-verbs"></a>2. 檔案關聯動詞 
**背景 **  
此測試會掃描套件登錄，確認是否登錄任何檔案關聯動詞。 

**測試詳細資料**  
運用各式各樣的通用 Windows 平台 API，可以增強已轉換的傳統型應用程式。 此測試會檢查應用程式中的 UWP 二進位檔案未呼叫非 UWP API。 UWP 二進位檔案已設定**AppContainer**旗標。

**修正動作**  
請參閱[傳統型轉 UWP 橋接器：應用程式延伸模組](https://docs.microsoft.com/en-us/windows/uwp/porting/desktop-to-uwp-extensions)，以取得這些延伸模組的解釋，以及如何正確地使用它們。 

### <a name="3-debug-configuration-test"></a>3. 偵錯組態測試
這項測試會確認 appx 不是偵錯組建。
 
**背景 **  
若要通過 Windows 市集的認證，應用程式不可以針對偵錯進行編譯，而且不可以參照可執行檔案的偵錯版本。 此外，您必須針對您的應用程式建置最佳化的程式碼以便通過此測試。
 
**測試詳細資料**  
測試應用程式，確定不是偵錯組建，而且沒有連結到任何偵錯架構。
 
**修正動作**  
* 將應用程式送出到 Windows 市集之前，將它建置為版本組建。
* 確定您已安裝正確的 .NET Framework 版本。
* 確認應用程式並未連結到偵錯版本的架構，且是利用發行版本所建置。 如果這個應用程式包含 .NET 元件，請確認您已安裝正確的 .NET Framework 版本。

### <a name="4-package-sanity-test"></a>4. 套件例行性測試
#### <a name="41-archive-files-usage"></a>4.1 封存檔案的使用方式

**背景**  
這項測試可協助您建置更好傳統型橋接器應用程式，以便在 [Windows 10 S](https://www.microsoft.com/windows/windows-10-s) 電腦上執行。

**測試詳細資料**  
這項測試會檢查封存檔案或自我解壓縮內容中所有的可執行檔。 因為這種類型的內容內含的可執行檔在上架到 Windows 市集時未經簽署，App 可能無法如預期般在 Windows 10 S 系統上執行。
 
**修正動作**
* 請考慮評估測試已標幟的檔案，判斷是否會對 Windows 10 S 環境中執行的 App 產生影響。
* 如果您的 App 會受影響，請從封存檔案移除可執行檔，而且不要使用自我解壓縮封存將可執行檔放在磁碟上。 這樣應該可避免 App 功能喪失。

#### <a name="42-blocked-executables"></a>4.2 封鎖的可執行檔

**背景**  
這項測試可協助您建置更好傳統型橋接器應用程式，以便在 [Windows 10 S](https://www.microsoft.com/windows/windows-10-s) 電腦上執行。 

**測試詳細資料**  
此測試會檢查 App 是否在嘗試啟動可執行檔，這種嘗試在 Windows 10 S 系統受到限制。 需依賴此功能的 App 可能無法如預期般在 Windows 10 S 系統上執行。 

**修正動作**  
* 識別測試中的哪些已標幟的項目表示要啟動不屬於 App 之可執行檔的呼叫，然後移除這些呼叫。 
* 如果已標幟的檔案屬於您的應用程式的一部分，則可以忽略警告。


## <a name="current-required-tests"></a>目前的必要測試

### <a name="1-app-capabilities-test-special-use-capabilities"></a>1. 應用程式功能測試 (特殊用途功能)

**背景 **  
特殊用途功能適用於非常特殊的情況。 僅限公司帳戶使用這些功能。 

**測試詳細資料**  
驗證應用程式是否宣告下列任一功能： 
* EnterpriseAuthentication
* SharedUserCertificates
* DocumentsLibrary

如果宣告了這些功能的其中之一，測試就會顯示警告給使用者。 

**修正動作**  
請考慮移除應用程式不需要的特殊用途功能。 此外，這些功能的使用方式需接受其他上架原則審查。

### <a name="2-app-manifest-resources-tests"></a>2. 應用程式資訊清單資源測試 
#### <a name="21-app-resources-validation"></a>2.1 應用程式資源驗證
如果應用程式資訊清單中宣告的字串或影像不正確，就無法正確地安裝應用程式。 如果應用程式安裝時包含這些錯誤，就無法正確地顯示應用程式的標誌或其他影像。    

**測試詳細資料**  
檢查應用程式資訊清單中定義的資源，確定它們存在並且有效。

**修正動作**  
使用下表作為指引。

錯誤訊息 | 註解
--------------|---------
影像 {image name} 同時定義 Scale 和 TargetSize 限定詞二者；一次只能定義一個限定詞。 | 您可以針對不同的解析度自訂影像。 在實際訊息中，{image name} 包含有錯誤的影像名稱。 請確定每個影像都將 Scale 或 TargetSize 定義為限定詞。 
影像 {image name} 不符合大小限制。  | 請確定所有應用程式影像都遵守適當的大小限制。 在實際訊息中，{image name} 包含有錯誤的影像名稱。 
套件中沒有影像 {image name}。  | 缺少必要的影像。 在實際訊息中，{image name} 包含缺少的影像名稱。 
影像 {image name} 不是有效的影像檔。  | 確認所有應用程式影像都遵守適當的檔案格式類型限制。 在實際訊息中，{image name} 包含無效的影像名稱。 
影像 "BadgeLogo" 在位置 (x, y) 的 ABGR 值 {value} 無效。 像素必須是白色 (##FFFFFF) 或透明 (00######)  | 徽章標誌是出現在徽章通知旁邊的影像，用以在鎖定畫面識別應用程式。 這個影像必須為單色 (只能包含白色和透明像素)。 在實際訊息中，{value} 包含影像中無效的色彩值。 
影像 "BadgeLogo" 在位置 (x, y) 的 ABGR 值 {value} 對於高對比白色影像無效。 像素必須是 (##2A2A2A) 或較深，或透明 (00######)。  | 徽章標誌是出現在徽章通知旁邊的影像，用以在鎖定畫面識別應用程式。 因為在高對比白色中，徽章標誌會出現在白色背景上，所以它必須是正常徽章標誌的深色版本。 在高對比白色中，徽章標誌只能包含比 (##2A2A2A) 深的像素或透明。 在實際訊息中，{value} 包含影像中無效的色彩值。 
影像至少必須定義一個不含 TargetSize 限定詞的變數。 它必須定義 Scale 限定詞，或不指定 Scale 和 TargetSize，預設值為 Scale-100。  | 如需詳細資訊，請參閱[回應式設計](https://msdn.microsoft.com/library/windows/apps/xaml/dn958435.aspx)和[應用程式資源](https://docs.microsoft.com/en-us/windows/uwp/app-settings/store-and-retrieve-app-data)的指南。 
套件缺少 "resources.pri" 檔案。  | 如果您的 app 資訊清單中有可當地語系化的內容，app 套件中務必包含有效的 resources.pri 檔案。 
"resources.pri" 檔案必須包含資源對應，且名稱符合套件名稱 {package full name}  | 如果資訊清單已變更，而 resources.pri 中的資源對應名稱不再符合資訊清單中的套件名稱，就會發生這個錯誤。 在實際訊息中，{package full name} 包含 resources.pri 必須包含的套件名稱。 若要更正此錯誤，您需要重建 resources.pri，最簡單的方式就是重建 app 的套件。 
"resources.pri" 檔案不能啟用 AutoMerge。  | MakePRI.exe 支援一個稱為 AutoMerge 的選項。 AutoMerge 的預設值為 off。 啟用時，AutoMerge 會在執行期間將 app 的語言套件資源合併到單一 resources.pri 中。 我們不建議您在想要透過 Windows 市集發佈的 app 使用這個選項。 透過 Windows 市集發行的 app 的 resources.pri 必須位於 app 套件的根目錄，而且包含 app 支援的所有語言參考。 
字串 {string} 不符合 {number} 個字元的長度上限限制。  | 請參閱 [app 套件需求](https://docs.microsoft.com/en-us/windows/uwp/publish/app-package-requirements)。 在實際訊息中，{string} 會以發生錯誤的字串取代，而 {number} 包含長度上限。 
字串 {string} 的開頭/結尾不得具有空白字元。  | 應用程式資訊清單中的元素結構描述不允許前後有空白字元。 在實際訊息中，{string} 會以有錯誤的字串取代。 確定 resources.pri 中的資訊清單欄位沒有任何當地語系化的值前後有空白字元。 
字串必須為非空白 (長度大於零)  | 如需詳細資訊，請參閱 [app 套件需求](https://docs.microsoft.com/en-us/windows/uwp/publish/app-package-requirements)。 
"resources.pri" 檔案中沒有指定預設資源。  | 如需詳細資訊，請參閱 [應用程式資源](https://docs.microsoft.com/en-us/windows/uwp/app-settings/store-and-retrieve-app-data)的指南。 在預設建置組態中，當產生套件組合、將其他資源放在資源套件中時，Visual Studio 只會在 app 套件中包含縮放比例-200 影像資源。 請確定您包含縮放比例-200 影像資源，或將您的專案設定為包含您所擁有的資源。 
"resources.pri" 檔案中沒有指定資源值。  | 請確定 app 資訊清單會在 resource.pri 中定義有效的資源。 
影像檔 {filename} 必須小於 204800 個位元組。  | 請降低所指示之影像的大小。 
{filename} 檔案不應包含反向對應區段。  | 若呼叫 makepri.exe 時在 Visual Studio「F5 偵錯」期間產生反向對應，則可藉由在產生 pri 檔案時，執行 makepri.exe 但不加上 /m 參數來移除它。 



#### <a name="22-branding-validation"></a>2.2 商標驗證
**背景 **  
傳統型橋接器應用程式應該要完整且功能正常。 使用預設影像 (來自範本或 SDK 範例) 的應用程式會呈現不佳的使用者經驗，而且在市集型錄中也不容易識別。

**測試詳細資料**  
這個測試會驗證應用程式所使用的影像不是來自 SDK 範例或 Visual Studio 的預設影像。 

**修正動作**  
以更特別且更能代表您應用程式的影像取代預設影像。

### <a name="3-package-compliance-tests"></a>3.套件相容性測試
#### <a name="31-app-manifest"></a>3.1 應用程式資訊清單
測試應用程式資訊清單的內容，確定內容正確無誤。

**背景**  
應用程式必須包含格式正確的應用程式資訊清單。

**測試詳細資料**  
檢查應用程式資訊清單，確認內容正確無誤，如[應用程式套件需求](https://docs.microsoft.com/en-us/windows/uwp/publish/app-package-requirements)中所述。 這個測試會完成下列檢查︰
* **副檔名與通訊協定**  
應用程式可能會宣告可與其相關聯的檔案類型。 大量不常見檔案類型的宣告，會產生較差的使用者體驗。 這個測試會限制應用程式可以產生關聯的副檔名數目。
* **架構相依性規則**  
這個測試會強制要求應用程式需要宣告與 UWP 的適當相依性。 如果有不適當的相依性，這個測試就會失敗。 如果應用程式設為目標的作業系統版本與建立架構相依性的作業系統版本不符，測試將會失敗。 如果應用程式參照任何「預覽」版本的架構 DLL，則測試也會失敗。
* **處理程序間通訊 (IPC) 驗證**  
這個測試強制要求傳統型橋接器應用程式不會在應用程式容器外部與傳統型元件通訊。 處理程序間通訊僅適用於側載應用程式。 將 [**ActivatableClassAttribute**](https://msdn.microsoft.com/library/windows/apps/BR211414) 的名稱指定為 `DesktopApplicationPath` 的應用程式將無法通過這個測試。  

**修正動作**  
按照 [應用程式套件需求](https://docs.microsoft.com/en-us/windows/uwp/publish/app-package-requirements)中所述的需求來檢閱應用程式的資訊清單。


#### <a name="32-application-count"></a>3.2 應用程式計數
這項測試會確認一個應用程式套件 (.appx、應用程式套件組合) 包含一個應用程式。 

**背景 **  
這項測試是根據「市集原則」而實作。 

**測試詳細資料**  
這項測試會確認套件組合中的.appx 套件總數小於 512，而且套件組合中只有一個「主要」套件。 它也會確認套件組合版本的版本號碼設定為 0。 

**修正動作**  
請確定應用程式套件和套件組合符合**測試詳細資料**中所述的需求。


#### <a name="33-registry-checks"></a>3.3 登錄檢查
**背景 **  
這項測試會檢查應用程式安裝還是更新任何新的服務或驅動程式。

**測試詳細資料**  
這項測試會查看 registry.dat 檔案中是否有特定登錄位置的更新，指出將登錄新的服務或驅動程式。 如果應用程式嘗試安裝驅動程式或服務，測試會失敗。  

**修正動作**  
檢閱失敗，並移除有問題且不需要的服務或驅動程式。 如果應用程式與其相依，則您想要上架到市集時，需要修正應用程式。


### <a name="4-platform-appropriate-files-test"></a>4. 平台適用檔案測試
視使用者的處理器架構而定，安裝混合二進位檔案的應用程式可能會損毀或無法正確執行。 

**背景 **  
這項測試會掃描應用程式套件中的二進位檔案是否會發生架構衝突。 應用程式套件不應包含無法在資訊清單中所指定的處理器架構上使用的二進位檔案。 包含不支援的二進位檔案會導致應用程式毀損，或是以非必要方式增加應用程式套件的大小。 

**測試詳細資料**  
驗證每個檔案的可攜式執行檔標頭中的「位元」都適合與應用程式套件處理器架構宣告交叉參考。 

**修正動作**  
遵循這些指導方針，以確保您的應用程式套件只包含應用程式資訊清單中指定之架構所支援的檔案： 
* 如果應用程式的目標處理器架構是 Neutral 處理器類型，則應用程式套件不得包含 x86、x64 或 ARM 二進位檔案或映像類型檔案。
* 如果 app 的目標處理器架構是 x86 處理器類型，則應用程式套件必須只能包含 x86 二進位檔案或映像類型檔案。 如果套件包含 x64 或 ARM 二進位檔案或映像類型檔案，這個測試將會失敗。
* 如果 app 的目標處理器架構是 x64 處理器類型，則應用程式套件必須能包含 x64 二進位檔案或映像類型檔案。 請注意，在這個情況下，套件也可以包含 x86 檔案，但是主要的應用程式經驗應該運用 x64 二進位檔案。 如果套件包含 ARM 二進位檔案或映像類型檔案，或者*只*包含 x86 二進位檔案或映像類型檔案，則這個測試將會失敗。
* 如果應用程式的目標處理器架構是 ARM 處理器類型，則應用程式套件必須只能包含 ARM 二進位檔案或映像類型檔案。 如果套件包含 x64 或 x86 二進位檔案或映像類型檔案，這個測試將會失敗。 

### <a name="5-supported-api-test"></a>5. 支援的 API 測試
檢查應用程式是否使用不相容的 API。 

**背景 **  
傳統型橋接器應用程式可以使用某些舊版的 Win32 API 與現代化 API (UWP 元件)。 這項測試會識別可使用不受支援之 API 的受管理二進位檔案。
 
**測試詳細資料**  
這項測試會檢查應用程式中的所有 UWP 元件︰
* 檢查二進位檔案的匯入位址表，確認應用程式套件內的每個受管理二進位檔案都相依於 Windows 市集應用程式開發所支援的 Win32 API。
* 確認應用程式套件內的每個受管理二進位檔案不會相依於核准的設定檔外部的函式。 

**修正動作**  
確保應用程式已編譯為發行組建，而非偵錯組建，即可進行修正。 

> [!NOTE]
> 應用程式偵錯組建即使只使用適用於 [Windows 市集應用程式的 API](https://msdn.microsoft.com/library/windows/apps/xaml/bg124285.aspx)，還是無法通過這個測試。 檢閱錯誤訊息，找出 API 有哪些不屬於 Windows 市集應用程式的 API。 

> [!NOTE]
> 即使偵錯組態只使用來自適用於 Windows 市集應用程式的 Windows SDK 的 API，該設定內建的 C++ 應用程式也無法通過這個測試。 如需詳細資訊，請參閱 [Windows 市集應用程式中 Windows API 的替代方法](https://msdn.microsoft.com/library/windows/apps/hh464945.aspx)。

### <a name="6-user-account-control-uac-test"></a>6. 使用者帳戶控制 (UAC) 測試  

**背景 **  
確保應用程式不會在執行階段要求使用者帳戶控制。

**測試詳細資料**  
應用程式無法根據 Windows 市集原則來要求系統管理員權限提高或 UIAccess。 不支援安全性權限提高。 

**修正動作**  
必須以互動式使用者身分執行應用程式。 如需詳細資料，請參閱 [UI 自動化安全性概觀](https://go.microsoft.com/fwlink/?linkid=839440)。

 
### <a name="7-windows-runtime-metadata-validation"></a>7. Windows 執行階段中繼資料驗證
**背景 **  
確保應用程式隨附的元件符合 UWP 類型系統。

**測試詳細資料**  
這項測試會擲回數個與正確類型用法相關的旗標。

**修正動作**  
* **ExclusiveTo 屬性**  
確保 UWP 類別不會實作已標示為 ExclusiveTo 其他類別的介面。
* **一般中繼資料正確性**  
確保您用來產生類型的編譯器符合最新的 UWP 規格。
* **屬性**  
確保 UWP 類別中的所有屬性都有`get`方法 (`set`是選擇性方法)。 針對所有屬性，確定`get`方法所傳回的類型符合`set`方法輸入參數的類型。
* **類型位置**  
確保所有 UWP 類型的中繼資料都位於應用程式套件中命名空間相符名稱最長的 .winmd 檔案中。
* **類型名稱區分大小寫**  
請確定應用程式套件中的所有 UWP 類型都會有唯一且不區分大小寫的名稱。 同時也確保應用程式套件內的命名空間名稱均未使用 UWP 類型名稱。
* **類型名稱正確性**  
確保全域命名空間或 Windows 最上層命名空間中，不存在任何 UWP 類型。
 

### <a name="8-windows-security-features-tests"></a>8. Windows 安全性功能測試
變更預設的 Windows 安全性保護會讓客戶面臨較高的風險。 

#### <a name="81-banned-file-analyzer"></a>8.1 禁止的檔案分析器
**背景 **  
特定的檔案已更新成具有重要安全性、可靠性或其他改良功能。 Windows 傳統型橋接器應用程式必須包含這些檔案的最新版本，因為舊版本有其風險。 Windows 應用程式認證套件會封鎖這些檔案，確保所有應用程式都使用目前的版本。

**測試詳細資料**  
Windows 應用程式認證套件中的「禁止的檔案檢查」目前會檢查下列檔案︰
* *Bing.Maps.JavaScript\js\veapicore.js*  
如果應用程式要使用檔案的「發行預覽」版本，而非最新正式發行，則這項檢查通常會失敗。 

**修正動作**  
若要修正這個問題，請使用適用於 Windows 市集應用程式之 [Bing Maps SDK](http://go.microsoft.com/fwlink/p/?linkid=614880) 的最新版本。

#### <a name="82-private-code-signing"></a>8.2 私用程式碼簽署
測試私用程式碼簽署二進位檔是否存在於應用程式套件。 

**背景**  
私用程式碼簽署檔案應該保持私用，因為若遭到洩露，可能會被用於惡意用途。 

**測試詳細資料**  
檢查應用程式套件內的檔案是否存在 .pfx 或 .snk，以指出已包含私用簽署金鑰。 

**修正動作**  
從套件中移除任何私用程式碼簽署金鑰 (例如，.pfx 和 .snk 檔案)。


## <a name="related-topics"></a>相關主題

* [Windows 市集原則](https://msdn.microsoft.com/library/windows/apps/Dn764944)
