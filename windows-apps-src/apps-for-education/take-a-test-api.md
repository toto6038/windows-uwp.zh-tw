---
author: TylerMSFT
Description: "Microsoft 進行測驗 app 的 JavaScript API 可讓您執行安全性評定。 「進行測驗」提供了安全的瀏覽器，可防止學生在測驗期間使用其他電腦或網際網路資源。"
title: "進行測驗 JavaScript API。"
ms.author: twhitney
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.assetid: 9bff6318-504c-4d0e-ba80-1a5ea45743da
translationtype: Human Translation
ms.sourcegitcommit: 5645eee3dc2ef67b5263b08800b0f96eb8a0a7da
ms.openlocfilehash: ac1a9b38a9857ae536025e682f98d01135850a19
ms.lasthandoff: 02/08/2017

---

# <a name="take-a-test-javascript-api"></a>進行測驗 JavaScript API

「[進行測驗](https://technet.microsoft.com/edu/windows/take-tests-in-windows-10)」是一個針對高度利害攸關測驗轉譯鎖定線上評定的瀏覽器架構 App。 它支援 SBAC 瀏覽器 API 標準，以用於高度利害攸關常見核心測試，並且可讓您將焦點放在評估內容，而不是如何鎖定 Windows。

進行測驗，是由 Microsoft 的 Edge 瀏覽器提供，它提供了 Web 應用程式可以用來提供鎖定體驗以進行測驗的 JavaScript API。

API (以 [常見核心 SBAC API](http://www.smarterapp.org/documents/SecureBrowserRequirementsSpecifications_0-3.pdf) 為基礎) 可提供文字轉換語音功能，以及提供查詢裝置是否已鎖定及正在執行的使用者和系統處理序等等事項的查詢功能。

請參閱[進行測驗 app 技術參考](https://technet.microsoft.com/edu/windows/take-a-test-app-technical)以了解 app 本身的相關資訊。

> [!Important]
> 這些 API 無法在遠端工作階段中運作。  

如需疑難排解協助，請參閱[使用事件檢視器對「Microsoft 進行測驗」進行疑難排解](troubleshooting.md)。

## <a name="reference-documentation"></a>參考文件
進行測驗 API 包含下列命名空間。 

| 命名空間 | 描述 |
|-----------|-------------|
|[安全性命名空間](#security-namespace)|可讓您鎖定裝置|
|[tts 命名空間](#tts-namespace)|文字轉換語音功能|


 ### <a name="security-namespace"></a>安全性命名空間

安全性命名空間可讓您鎖定裝置、檢查使用者與系統處理序清單、取得 MAC 和 IP 位址，以及清除快取的 Web 資源。

| 方法 | 描述   |
|--------|---------------|
|[clearCache](#clearCache) | 清除快取的 Web 資源 |
|[close](#close) | 關閉瀏覽器並將裝置解除鎖定 |
|[enableLockDown](#enableLockDown) | 鎖定裝置。 也可以用來將裝置解除鎖定 |
|[getIPAddressList](#getIPAddressList) | 取得裝置 IP 位址的清單 |
|[getMACAddress](#getMACAddress)|取得裝置 MAC 位址的清單|
|[getProcessList](#getProcessList)|取得正在執行的使用者與系統處理程序的清單|
|[isEnvironmentSecure](#isEnvironmentSecure)|判斷鎖定內容是否仍已套用到裝置|  

---
<span id="clearCache"/>
### <a name="void-clearcache"></a>void clearCache()
清除快取的 Web 資源。

**語法**  
`browser.security.clearCache();`

**參數**  
`None`

**傳回值**  
`None`

**需求**  
Windows 10 (版本 1607)

---

<span id="close"/>
### <a name="closeboolean-restart"></a>close(boolean restart)
關閉瀏覽器並將裝置解除鎖定。

**語法**  
`browser.security.close(false);`

**參數**  
`restart` - 此參數會被忽略但必須提供。

**傳回值**  
`None`

**需求**  
Windows 10 (版本 1607)

---

<span id="enableLockDown"/>
### <a name="enablelockdownboolean-lockdown"></a>enableLockdown(boolean lockdown)
鎖定裝置。 也可以用來將裝置解除鎖定。

**語法**  
`browser.security.enableLockDown(true|false);`

**參數**  
`lockdown` - `true` 在鎖定畫面上執行進行測驗 app 並套用此[文件](https://technet.microsoft.com/edu/windows/take-a-test-app-technical?f=255&MSPPError=-2147217396)中討論的原則。 `False` 在鎖定畫面上停止執行進行測驗並將它關閉 (除非 app 沒有被鎖定)；在這種情況下沒有任何作用。

**傳回值**  
`None`

**需求**  
Windows 10 (版本 1607)

---

<span id="getIPAddressList"/>
### <a name="string-getipaddresslist"></a>string[] getIPAddressList()
取得裝置 IP 位址的清單。

**語法**  
`browser.security.getIPAddressList();`

**參數**  
`None`

**傳回值**  
`An array of IP addresses.`

---

<span id="getMACAddress" />
### <a name="string-getmacaddress"></a>string[] getMACAddress()
取得裝置 MAC 位址的清單。

**語法**  
`browser.security.getMACAddress();`

**參數**  
`None`

**傳回值**  
`An array of MAC addresses.`

**需求**  
Windows 10 (版本 1607)

---

<span id="getProcessList" />
### <a name="string-getprocesslist"></a>string[] getProcessList()
取得使用者正在執行的處理程序的清單。

**語法**  
`browser.security.getProcessList();`

**參數**  
`None`

**傳回值**  
`An array of running process names.`

**備註** 清單中不包含系統處理程序。

**需求**  
Windows 10 (版本 1607)

---

<span id="isEnvironmentSecure" />
### <a name="boolean-isenvironmentsecure"></a>boolean isEnvironmentSecure()
判斷鎖定內容是否仍已套用到裝置。

**語法**  
`browser.security.isEnvironmentSecure();`

**參數**  
`None`

**傳回值**  
`True indicates that the lockdown context is applied to the device; otherwise false.`

**需求**  
Windows 10 (版本 1607)

---

### <a name="tts-namespace"></a>tts 命名空間

tts 命名空間會處理 App 的文字轉換語音功能。

| 方法 | 描述 |
|--------|-------------|
|[getStatus](#getStatus) | 取得語音播放狀態|
|[getVoices](#getVoices) | 取得可用語音套件的清單|
|[pause](#pause)|暫停語音合成|
|[resume](#resume)|繼續暫停的語音合成|
|[speak](#speak)|用戶端文字轉換語音合成|
|[stop](#stop)|停止語音合成|

> [!Tip]
> [Microsoft Edge 語音合成 API](https://blogs.windows.com/msedgedev/2016/06/01/introducing-speech-synthesis-api/) 是 [W3C 語音 API](https://dvcs.w3.org/hg/speech-api/raw-file/tip/webspeechapi.html) 的實作，且我們建議開發人員盡可能使用此 API。

---

<span id="getStatus" />
### <a name="string-getstatus"></a>string getStatus()
取得語音播放狀態。

**語法**  
`browser.tts.getStatus();`

**參數**  
`None`

**傳回值**  
`The speech playback status. Possible values are: “available”, “idle”, “paused”, and “speaking”.`

**需求**  
Windows 10 (版本 1607)

---

<span id="getVoices" />
### <a name="string-getvoices"></a>string[] getVoices()
取得可用語音套件的清單。

**語法**  
`browser.tts.getVoices();`

**參數**  
`None`

**傳回值**  
`The available voice packs. For example: “Microsoft Zira Mobile”, “Microsoft Mark Mobile”`

**需求**  
Windows 10 (版本 1607)

---

<span id="pause" />
### <a name="void-pause"></a>void pause()

暫停語音合成。

**語法**  
`browser.tts.pause();`

**參數**

`None`

**傳回值**

`None`

**需求**  
Windows 10 (版本 1607)

---

<span id="resume" />
### <a name="void-resume"></a>void resume()
繼續暫停的語音合成。

**語法**  
`browser.tts.resume();`

**參數**
`None`

**傳回值**
`None`

**需求**  
Windows 10 (版本 1607)

---

<span id="speak" />
### <a name="void-speakstring-text-object-options-function-callback"></a>void speak(string text, object options, function callback)
啟動用戶端文字轉換語音合成。

**語法**  
`void browser.tts.speak(“Hello world”, options, callback);`

**參數**  
`Speech options such as gender, pitch, rate, volume. For example:`  
```
var options = {
   'gender': this.currentGender,
   'language': this.currentLanguage,
   'pitch': 1,
   'rate': 1,
   'voice': this.currentVoice,
   'volume': 1
};
```

**傳回值**  
`None`

**備註** 選項變數必須是小寫。 性別、語言及語音參數是採用字串。
音量、音調及速率必須在語音合成標記語言檔案 (SSML) 中標記，而不是在選項物件中標記。
選項物件必須遵循上述範例中所示的順序、命名和大小寫規則。

**需求**  
Windows 10 (版本 1607)

---

<span id="stop" />
### <a name="void-stop"></a>void stop()
停止語音合成。

**語法**  
`void browser.tts.speak(“Hello world”, options, callback);`

**參數**  
`None`

**傳回值**  
`None`

**需求**  
Windows 10 (版本 1607)

