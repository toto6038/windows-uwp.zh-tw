---
Description: The JavaScript API for the Microsoft Take a Test app allows you to do secure assessments. Take a Test provides a secure browser that prevents students from using other computer or internet resources during a test.
title: 進行測驗 JavaScript API。
ms.assetid: 9bff6318-504c-4d0e-ba80-1a5ea45743da
ms.date: 08/08/2018
ms.topic: article
keywords: windows 10，uwp，教育版
ms.localizationpriority: medium
ms.openlocfilehash: 9f308e42e1dbb1d3654d3fc557a9d5e29ef6f6b0
ms.sourcegitcommit: b5c9c18e70625ab770946b8243f3465ee1013184
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/28/2018
ms.locfileid: "7975646"
---
# <a name="take-a-test-javascript-api"></a>進行測驗 JavaScript API

[進行測驗](https://technet.microsoft.com/edu/windows/take-tests-in-windows-10)是轉譯鎖定線上評定針對高度利害攸關，瀏覽器型 UWP 應用程式可讓授課者專注在評量內容，而非如何提供安全的考試環境。 為了達成此目的，此應用程式採用任何 Web 應用程式都能使用的 JavaScript API。 「進行測驗」API 支援適用於重大通用核心考試的 [SBAC 瀏覽器 API 標準](http://www.smarterapp.org/documents/SecureBrowserRequirementsSpecifications_0-3.pdf)。

如需有關 App 本身的詳細資訊，請參閱[進行測驗 App 技術參考](https://technet.microsoft.com/edu/windows/take-a-test-app-technical?f=255&MSPPError=-2147217396)。 如需疑難排解協助，請參閱[使用事件檢視器對 [Microsoft 進行測驗] 進行疑難排解](troubleshooting.md)。

## <a name="reference-documentation"></a>參考文件
進行測驗 API 存在於下列命名空間。 請注意，所有的 API 都相依於全域 `SecureBrowser` 物件。

| 命名空間 | 描述 |
|-----------|-------------|
|[安全性命名空間](#security-namespace)|包含可讓您鎖定裝置進行考試和強制執行考試環境的 API。 |

### <a name="security-namespace"></a>安全性命名空間

安全性命名空間可讓您鎖定裝置、 檢查使用者與系統處理程序的清單、 取得 MAC 和 IP 位址，以及清除快取的 web 資源。

| 方法 | 描述   |
|--------|---------------|
|[lockDown](#lockDown) | 鎖定裝置以進行考試。 |
|[isEnvironmentSecure](#isEnvironmentSecure) | 判斷是否依舊將鎖定內容套用至裝置。 |
|[getDeviceInfo](#getDeviceInfo) | 取得有關考試應用程式執行所在平台的詳細資訊。 |
|[examineProcessList](#examineProcessList)|取得執行中使用者及系統處理程序的清單。|
|[close](#close) | 關閉瀏覽器並將裝置解除鎖定。 |
|[getPermissiveMode](#getPermissiveMode)|檢查寬鬆模式是開啟還是關閉。|
|[setPermissiveMode](#setPermissiveMode)|將寬鬆模式切換為開啟或關閉。|
|[emptyClipBoard](#emptyClipBoard)|清除系統剪貼簿。|
|[getMACAddress](#getMACAddress)|取得裝置 MAC 位址的清單。|
|[getStartTime](#getStartTime) | 取得啟動考試應用程式的時間。 |
|[getCapability](#getCapability) | 查詢功能已啟用還是已停用。 |
|[setCapability](#setCapability)|啟用或停用指定的功能。| 
|[isRemoteSession](#isRemoteSession) | 檢查目前的工作階段是否已在遠端登入。 |
|[isVMSession](#isVMSession) | 檢查目前的工作階段是否正在虛擬機器中執行。 |

---

<span id="lockDown"/>

### <a name="lockdown"></a>lockDown
鎖定裝置。 也可以用來將裝置解除鎖定。 考試 Web 應用程式會在允許學生開始考試之前叫用此呼叫。 實施者必須任何必要的動作來確保考試環境安全。 保護環境安全所需採用的步驟是取決於裝置，例如，包括停用螢幕擷取、處於安全模式時停用語音交談、清除系統剪貼簿、進入 kiosk 模式、在 OSX 10.7+ 裝置中停用 Spaces 虛擬桌面等層面。考試應用程式會在評量開始前啟用鎖定，並在學生已完成評量且離開安全測驗時時停用鎖定。

**語法**  
`void SecureBrowser.security.lockDown(Boolean enable, Function onSuccess, Function onError);`

**參數**  
* `enable` - **true** 表示要在鎖定畫面上執行 [進行測驗] App 並套用本[文件](https://technet.microsoft.com/edu/windows/take-a-test-app-technical?f=255&MSPPError=-2147217396)中討論的原則。 **false** 會在鎖定畫面上停止執行 [進行測驗] 並將其關閉 (除非未將 App 鎖定)；在這種情況下沒有任何作用。  
* `onSuccess` - [選用] 要在鎖定已順利啟用或停用之後呼叫的函式。 這必須是 `Function(Boolean currentlockdownstate)` 的格式。  
* `onError` - [選用] 鎖定作業失敗時所要呼叫的函式。 這必須是 `Function(Boolean currentlockdownstate)` 的格式。  

**需求**  
Windows 10 版本 1709

---

<span id="isEnvironmentSecure" />

### <a name="isenvironmentsecure"></a>isEnvironmentSecure
判斷鎖定內容是否仍已套用到裝置。 考試 Web 應用程式會在允許學生開始考試之前，以及定時在測驗期間，叫用此方法。

**語法**  
`void SecureBrowser.security.isEnvironmentSecure(Function callback);`

**參數**  
* `callback` - 要在此函數完成時呼叫的函式。 這必須是 `Function(String state)` 的格式，其中 `state` 是包含兩個欄位的 JSON 字串。 第一個是 `secure` 欄位，只有在所有必要的鎖定都已啟用 (或功能已停用) 來啟用安全考試環境，而且自從 App 進入鎖定模式後，這些鎖定無一遭到入侵時，此欄位才會顯示 `true`。 另一個欄位 `messageKey` 包含廠商特定的其他詳細資料或資訊。 這裡的意圖是要允許廠商加入增強布林值 `secure` 旗標的額外資訊：

```JSON
{
    'secure' : "true/false",
    'messageKey' : "some message"
}
```

**需求**  
Windows 10 版本 1709

---

<span id="getDeviceInfo" />

### <a name="getdeviceinfo"></a>getDeviceInfo
取得有關考試應用程式執行所在平台的詳細資訊。 這是用來增強任何可從使用者代理程式辨別的資訊。

**語法**  
`void SecureBrowser.security.getDeviceInfo(Function callback);`

**參數**  
* `callback` - 要在此函數完成時呼叫的函式。 這必須是 `Function(String infoObj)` 的格式，其中 `infoObj` 是包含數個欄位的 JSON 字串。 必須支援下列欄位：
    * `os` 表示作業系統類型 (例如：Windows、macOS、Linux、iOS、Android 等)。
    * `name` 表示作業系統版本名稱 (如果有的話，例如：Sierra、Ubuntu)。
    * `version` 表示作業系統版本 (例如：10.1、10 專業版等)。
    * `brand` 表示安全瀏覽器商標 (例如：OAKS、CA、SmarterApp 等)。
    * `model` 僅表示行動裝置的裝置型號；null/unused 表示傳統型瀏覽器。

**需求**  
Windows 10 版本 1709

---

<span id="examineProcessList" />

### <a name="examineprocesslist"></a>examineProcessList
取得所有在使用者所擁有用戶端電腦上執行的處理程序清單。 考試應用程式會叫用此方法來檢查清單，並將此清單與在考試週期期間視為已列入封鎖清單的處理程序清單做比較。 應該在評量開始時，以及定時在學生進行評量期間叫用此呼叫。 如果偵測到已列入封鎖清單的處理程序，則應停止評量以保持測驗完整性。

**語法**  
`void SecureBrowser.security.examineProcessList(String[] blacklistedProcessList, Function callback);`

**參數**  
* `blacklistedProcessList` - 考試應用程式已將其列入封鎖清單之處理程序的清單。  
`callback` - 找到的作用中處理程序後所要叫用的函式。 這必須使用下列格式：`Function(String foundBlacklistedProcesses)`，其中 `foundBlacklistedProcesses` 的格式為：`"['process1.exe','process2.exe','processEtc.exe']"`。 如果找不到任何列入封鎖清單的處理程序，這會是空白。 如果為 null，這表示原始函式呼叫發生錯誤。

**備註**：清單不包含系統處理程序。

**需求**  
Windows 10 版本 1709

---

<span id="close"/>

### <a name="close"></a>close
關閉瀏覽器並將裝置解除鎖定。 當使用者選擇結束瀏覽器時，考試應用程式應該叫用此方法。

**語法**  
`void SecureBrowser.security.close(restart);`

**參數**  
* `restart` - 此參數會遭忽略但必須提供。

**備註**：在 Windows 10 版本 1607 中，一開始必須鎖定裝置。 在較新版本中，這個方法會關閉瀏覽器，不論是否鎖定裝置。

**需求**  
Windows 10 版本 1709

---

<span id="getPermissiveMode" />

### <a name="getpermissivemode"></a>getPermissiveMode
考試 Web 應用程式應該叫用此方法來判斷寬鬆模式是開啟還是關閉。 在寬鬆模式中，瀏覽器預期會放鬆其嚴格安全性勾點，以允許輔助技術使用安全瀏覽器。 例如，積極防止其他應用程式 UI 出現在其本身上方的瀏覽器，可能需要在寬鬆模式下放鬆此方法。 

**語法**  
`void SecureBrowser.security.getPermissiveMode(Function callback)`

**參數**  
* `callback` - 要在此呼叫完成時叫用的函式。 這必須使用下列格式：`Function(Boolean permissiveMode)`，其中 `permissiveMode` 表示瀏覽器目前是否處於寬鬆模式。 如果未定義或為 null，則會在取得作業中發生錯誤。

**需求**  
Windows 10 版本 1709

---

<span id="setPermissiveMode" />

### <a name="setpermissivemode"></a>setPermissiveMode
考試 Web 應用程式應該叫用此方法，將寬鬆模式切換為啟或關閉。 在寬鬆模式中，瀏覽器預期會放鬆其嚴格安全性勾點，以允許輔助技術使用安全瀏覽器。 例如，積極防止其他應用程式 UI 出現在其本身上方的瀏覽器，可能需要在寬鬆模式下放鬆此方法。 

**語法**  
`void SecureBrowser.security.setPermissiveMode(Boolean enable, Function callback)`

**參數**  
* `enable` - 表示預定寬鬆模式狀態的布林值。  
* `callback` - 要在此呼叫完成時叫用的函式。 這必須使用下列格式：`Function(Boolean permissiveMode)`，其中 `permissiveMode` 表示瀏覽器目前是否處於寬鬆模式。 如果未定義或為 null，則會在設定作業中發生錯誤。

**需求**  
Windows 10 版本 1709

---

<span id="emptyClipBoard"/>

### <a name="emptyclipboard"></a>emptyClipBoard
清除系統剪貼簿。 考試應用程式應該叫用此方法，強制清除任何可能儲存在系統剪貼簿中的資料。 **[lockDown](#lockDown)** 函數也會執行此作業。

**語法**  
`void SecureBrowser.security.emptyClipBoard();`

**需求**  
Windows 10 版本 1709

---

<span id="getMACAddress" />

### <a name="getmacaddress"></a>getMACAddress
取得裝置 MAC 位址的清單。 考試應用程式應該叫用此方法來協助診斷。 

**語法**  
`void SecureBrowser.security.getMACAddress(Function callback);`

**參數**  
* `callback` - 要在此呼叫完成時叫用的函式。 這必須使用下列格式：`Function(String addressArray)`，其中 `addressArray` 的格式為：`"['00:11:22:33:44:55','etc']"`。

**備註**  
依賴來源 IP 位址區分考試伺服器內的終端使用者電腦並不容易，因為學校的防火牆/NAT/Proxy 通常都在使用中。 MAC 位址可讓應用程式區分一般診斷用防火牆後方的終端用戶端電腦。

**需求**  
Windows 10 版本 1709

---

<span id="getStartTime" />

### <a name="getstarttime"></a>getStartTime
取得啟動考試應用程式的時間。

**語法**  
`DateTime SecureBrowser.settings.getStartTime();`

**傳回**  
表示考試應用程式啟動時間的 DateTime 物件。

**需求**  
Windows 10 版本 1709

---

<span id="getCapability"/>

### <a name="getcapability"></a>getCapability
查詢功能已啟用還是已停用。 

**語法**  
`Object SecureBrowser.security.getCapability(String feature)`

**參數**  
`feature` - 字串，用以判斷要查詢的功能。 有效的功能字串是 "screenMonitoring"、"printing" 和 "textSuggestions" (區分大小寫)。

**傳回值**  
此函式會傳回 JavaScript 物件或常值，格式為：`{<feature>:true|false}`。 如果啟用查詢的功能，則為**true**，如果未啟用功能，或功能字串無效，則為**false**。

**需求**Windows 10 版本 1703

---

<span id="setCapability"/>

### <a name="setcapability"></a>setCapability
在瀏覽器上啟用或停用特定功能。

**語法**  
`void SecureBrowser.security.setCapability(String feature, String value, Function onSuccess, Function onError)`

**參數**  
* `feature` - 字串，用以判斷要設定的功能。 有效的功能字串是 `"screenMonitoring"`、`"printing"` 和 `"textSuggestions"` (區分大小寫)。  
* `value` - 功能的預定設定值。 這必須是 `"true"` 或 `"false"`。  
* `onSuccess` - [選用] 要在設定作業已順利完成之後呼叫的函式。 這必須使用 `Function(String jsonValue)` 的格式：其中 *jsonValue* 的格式為：`{<feature>:true|false|undefined}`。  
* `onError` - [選用] 設定作業失敗時所要呼叫的函式。 這必須使用 `Function(String jsonValue)` 的格式：其中 *jsonValue* 的格式為：`{<feature>:true|false|undefined}`。

**備註**  
如果目標功能是瀏覽器未知的功能，則此函式會將 `undefined` 的值傳遞給回呼函式。

**需求**Windows 10 版本 1703

---

<span id="isRemoteSession"/>

### <a name="isremotesession"></a>isRemoteSession
檢查目前的工作階段是否已在遠端登入。

**語法**  
`Boolean SecureBrowser.security.isRemoteSession();`

**傳回值**  
如果目前的工作階段在遠端，則為 **true**，否則為 **false**。

**需求**  
Windows 10 版本 1709

---

<span id="isVMSession"/>

### <a name="isvmsession"></a>isVMSession
檢查目前的工作階段是否正在虛擬機器中執行。

**語法**  
`Boolean SecureBrowser.security.isVMSession();`

**傳回值**  
如果目前的工作階段正在虛擬機器中執行，則為 **true**，否則為 **false**。

**備註**  
此 API 檢查只能偵測實作適當 API 之特定 Hypervisor 內正在執行的 VM 工作階段

**需求**  
Windows 10 版本 1709

---