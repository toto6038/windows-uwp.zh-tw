---
ms.assetid: DD8FFA8C-DFF0-41E3-8F7A-345C5A248FC2
本主題說明如何將 PlayReady 保護的媒體內容新增到您的通用 Windows 平台 (UWP) app。
PlayReady DRM
---

# PlayReady DRM

\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


本主題說明如何將 PlayReady 保護的媒體內容新增到您的通用 Windows 平台 (UWP) app。

PlayReady DRM 讓開發人員可以建立 UWP app，能夠為使用者提供 PlayReady 內容，同時強制執行內容提供者所定義的存取規則。 本節說明對於適用於 Windows 10 的 Microsoft PlayReady DRM 所做的變更，以及如何修改 PlayReady UWP app 來支援從舊版 Windows 8.1 到 Windows 10 版本所做的變更。
 
| 主題                                                                     | 說明                                                                                                                                                                                                                                                                             |
|---------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [硬體 DRM](hardware-drm.md)                                           | 本主題概觀說明如何將以 PlayReady 硬體為基礎的數位版權管理 (DRM) 新增到 UWP app。                                                                                                                                                                 |
| [搭配使用彈性資料流與 PlayReady](adaptive-streaming-with-playready.md) | 本文章說明如何將包含 Microsoft PlayReady 內容保護的彈性資料流多媒體內容新增到通用 Windows 平台 (UWP) app。 本功能目前支援 HTTP 即時資料流 (HLS) 與 HTTP 動態資料流 (DASH) 內容播放。 |

## PlayReady DRM 的新功能

下列清單說明適用於 Windows 10 的 PlayReady DRM 的新功能與所做的變更。

-   已新增硬體數位版權管理 (DRM)。

    硬體式內容保護支援能夠在多個裝置的平台上安全播放高畫質 (HD) 和超高畫質 (UHD) 的內容。 金鑰內容 (包括私密金鑰、內容金鑰，以及其他用來衍生或解除鎖定上述金鑰的金鑰內容)，以及已解密的壓縮和未壓縮的視訊範例會利用硬體安全性來進行保護。 使用硬體 DRM 時，由於 HWDRM 管線一定知道要使用的輸出，因此未知的啟用器 (播放到未知裝置 / 使用 downres 播放到未知裝置) 是不具意義的。 如需詳細資訊，請參閱[硬體 DRM](hardware-drm.md)。

-   PlayReady 不再是 appX 架構元件，而是內建的作業系統元件。 命名空間已從 **Microsoft.Media.PlayReadyClient** 變更為 [**Windows.Media.Protection.PlayReady**](https://msdn.microsoft.com/library/windows/apps/dn986454)。
-   下列定義 PlayReady 錯誤碼的標頭現在是 Windows 軟體開發套件 (SDK) 的一部分：Windows.Media.Protection.PlayReadyErrors.h 和 Windows.Media.Protection.PlayReadyResults.h。
-   提供主動取得非永久性授權。

    舊版的 PlayReady DRM 不支援主動取得非永久性授權。 這個功能已新增到這個版本。 這可減少第一個畫面的時間。 如需詳細資訊，請參閱[播放之前主動取得非永久性授權](#proactively_acquire_a_non_persistent_license_before_playback)。

-   提供在一則訊息中取得多個授權的功能。

    允許用戶端 App 在一則授權取得訊息中取得多個非永久性授權。 這樣可藉由取得多個內容片段的授權來減少第一個畫面的時間，同時使用者仍可瀏覽您的內容庫；這可在使用者選取要播放的內容時，防止授權取得延遲。 此外，它可藉由啟用包含多個金鑰識別碼 (KID) 的內容標頭，來讓音訊和視訊串流以不同的金鑰加密；這樣讓單一授權取得可針對內容檔案中的所有串流取得所有授權，而不需使用自訂邏輯和多個授權取得要求來達到相同結果。

-   已新增即時到期支援或限時授權 (LDL)。

    能夠在授權上設定即時到期，並在播放期間順暢地從到期的授權轉換為另一個 (有效) 授權。 在一則訊息中結合取得多個授權的功能時，這讓 App 能夠以非同步方式取得數個 LDL，同時使用者仍能瀏覽內容庫，而且一旦使用者選取要播放的內容之後，只會取得較長持續時間的授權。 之後，就能以更快速的方式開始播放 (因為授權已經可以使用)，而且，因為 App 已在 LDL 到期之前取得較長持續時間的授權，所以能夠流暢地繼續播放到內容結尾而不會中斷。

-   已新增非永久性授權鏈結。
-   已在非永久性授權上新增以時間為基礎之限制 (包括到期、在第一次播放之後到期，以及即時到期) 的支援。
-   已新增 HDCP 類型 1 (版本 2.2) 原則的支援。

    如需詳細資訊，請參閱[要考慮的事項](#things_to_consider)。

-   現已內含 Miracast 做為輸出。
-   已新增安全停止功能。

    安全停止功能讓 PlayReady 裝置能夠確實向媒體串流處理服務宣告任何指定內容的媒體播放已停止。 此功能確保您的媒體串流處理服務能在不同裝置上，針對指定帳戶正確地強制執行和報告使用限制。

-   已新增音訊與視訊各別授權。

    分離曲目可防止視訊被解碼為音訊，進而使內容保護能力更為健全。 新的標準要求音訊與視覺曲目要個別提供金鑰。

-   新增 MaxResDecode。

    即使擁有功能更強的金鑰 (但非授權)，這項新功能仍限制以最高解析度來播放內容。 其支援使用單一金鑰編碼多個資料流大小的情況。

已將下列的新介面、類別及列舉新增到 PlayReady DRM：

-   [
            **IPlayReadyLicenseAcquisitionServiceRequest**](https://msdn.microsoft.com/library/windows/apps/dn986077) 介面
-   [
            **IPlayReadyLicenseSession**](https://msdn.microsoft.com/library/windows/apps/dn986080) 介面
-   [
            **IPlayReadySecureStopServiceRequest**](https://msdn.microsoft.com/library/windows/apps/dn986090) 介面
-   [
            **PlayReadyLicenseSession**](https://msdn.microsoft.com/library/windows/apps/dn986309) 類別
-   [
            **PlayReadySecureStopIterable**](https://msdn.microsoft.com/library/windows/apps/dn986371) 類別
-   [
            **PlayReadySecureStopIterator**](https://msdn.microsoft.com/library/windows/apps/dn986375) 類別
-   [
            **PlayReadyHardwareDRMFeatures**](https://msdn.microsoft.com/library/windows/apps/dn986265) 列舉程式

已建立新的範例來示範如何使用 PlayReady DRM 的新功能。 此範例可從 [http://go.microsoft.com/fwlink/p/?linkid=331670&clcid=0x409](http://go.microsoft.com/fwlink/p/?linkid=331670) 下載。

## 要考慮的事項

-   PlayReady DRM 現在支援 HDCP 類型 1 (版本 2.2 或更新版本)。 PlayReady 會在適用於裝置的授權中包含要強制執行的原則。 此功能可以在 PlayReady 伺服器 3.0 版 SDK 授權中啟用 (此伺服器會使用播放啟用器 **GUID**，在授權中控制此原則)。 如需詳細資訊，請參閱 [PlayReady 規範和穩健性規則](http://www.microsoft.com/playready/licensing/compliance/)。
-   硬體 DRM 不支援 Windows Media 視訊 (也稱為 VC-1)，請參閱[覆寫硬體 DRM](hardware-drm.md#override-hardware-drm)。
-   PlayReady DRM 現在支援高效率視訊編碼 (HEVC /H.265) 視訊壓縮標準。 若要支援 HEVC，您的 app 必須使用常見加密配置 (CENC) 版本 2 內容，以便讓內容的切割標頭不加密。 如需詳細資訊，請參閱 ISO/IEC 23001-7 資訊技術 -- MPEG 系統技術 -- 第 7 部分：以 ISO 為基礎的媒體檔案格式檔案中的常見加密 (需要規格版本 ISO/IEC 23001-7:2015 或更高版本)。 Microsoft 也建議針對所有 HWDRM 內容使用 CENC 版本 2。 此外，某些硬體 DRM 將支援 HEVC，但有些不支援 (請參閱[覆寫硬體 DRM](hardware-drm.md#override-hardware-drm))。
-   利用某些新的 PlayReady 3.0 功能 (包括但不限於適用於硬體式用戶端的 SL3000、在一個授權取得訊息中取得多個非永久性授權，以及非永久性授權上的時間限制)，PlayReady 伺服器必須是 Microsoft PlayReady 伺服器軟體開發套件 v3.0.2769 版本或更新版本。
-   根據內容授權中指定的輸出保護原則，如果使用者連接的輸出不支援這些需求，則媒體播放將會失敗。 下表列出一組所產生的常見錯誤。 如需詳細資訊，請參閱 [PlayReady 規範和穩健性規則](http://www.microsoft.com/playready/licensing/compliance/)。

| 錯誤                                                   | 值      | 說明                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|---------------------------------------------------------|------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ERROR\_GRAPHICS\_OPM\_OUTPUT\_DOES\_NOT\_SUPPORT\_HDCP  | 0xC0262513 | 授權的輸出保護原則要求監視器採用 HDCP，但卻無法採用 HDCP。                                                                                                                                                                                                                                                                                                                                                                                              |
| MF\_E\_POLICY\_UNSUPPORTED                              | 0xC00D7159 | 授權的輸出保護原則要求監視器採用 HDCP 類型 1，但卻無法採用 HDCP 類型 1。                                                                                                                                                                                                                                                                                                                                                                                |
| DRM\_E\_TEE\_OUTPUT\_PROTECTION\_REQUIREMENTS\_NOT\_MET | 0x8004CD22 | 只有在硬體 DRM 下執行時，才會發生這個錯誤碼。 授權的輸出保護原則要求監視器採用 HDCP 或降低內容的有效解析度，但卻無法採用 HDCP 且無法降低內容的有效解析度，因為硬體 DRM 不支援降低內容的解析度。 在軟體 DRM 下，可播放該內容。 請參閱[使用硬體 DRM 的考量](hardware-drm.md#considerations-for-using-hardware-drm)。 |
| ERROR\_GRAPHICS\_OPM\_NOT\_SUPPORTED                    | 0xc0262500 | 圖形驅動程式不支援輸出保護。 例如，監視器是透過 VGA 所連接，或者未安裝適用於數位輸出的適當圖形驅動程式。 在後者的情況下，所安裝的一般驅動程式是 Microsoft 基本顯示卡，而安裝適當的圖形驅動程式就能解決問題。                                                                                                                                                  |

## 先決條件

開始建立 PlayReady 保護的 UWP app 之前，需要在系統上安裝下列軟體：

-   Windows 10。
-   如果您正在針對適用於 UWP app 的 PlayReady DRM 編譯任何範例，就必須使用 Microsoft Visual Studio 2015 或更新版本來編譯範例。 您仍然可以使用 Microsoft Visual Studio 2013，來編譯任何來自適用於 Windows 8.1 市集 app 的 PlayReady DRM 的範例。

如果您計劃在 app 上播放 MPEG-2/H.262 內容，也必須下載並安裝 [Windows 8.1 Media Center Pack](http://go.microsoft.com/fwlink/p/?LinkId=626876)。

## PlayReady Windows 市集 app 移轉指南

本節包含如何將現有的 PlayReady Windows 8.x 市集 app 移轉到 Windows 10 的相關資訊。

Windows 10 上適用於 PlayReady UWP app 的命名空間已從 **Microsoft.Media.PlayReadyClient** 變更為 [**Windows.Media.Protection.PlayReady**](https://msdn.microsoft.com/library/windows/apps/dn986454)。 這表示您必須在程式碼中搜尋舊的命名空間，並取代為新的命名空間。 您仍將參考 winmd 檔案。 它是 Windows 10 作業系統上 windows.media.winmd 的一部分。 它屬於 Windows SDK 中的 windows.winmd。 對於 UWP，會在 windows.foundation.univeralappcontract.winmd 中參照它。

若要播放 PlayReady 保護的高畫質 (HD) 內容 (1080p) 及超高畫質 (UHD) 內容，您需要實作 PlayReady 硬體 DRM。 如需如何實作 PlayReady 硬體 DRM 的相關資訊，請參閱[硬體 DRM](hardware-drm.md)。

硬體 DRM 不支援某些內容。 如需停用硬體 DRM 和啟用軟體 DRM 的相關資訊，請參閱[覆寫硬體 DRM](hardware-drm.md#override-hardware-drm)。

關於媒體保護管理員，請確定您的程式碼包含下列設定 (如果程式碼中還未包含)。

``` syntax
var mediaProtectionManager = new Windows.Media.Protection.MediaProtectionManager();

mediaProtectionManager.properties["Windows.Media.Protection.MediaProtectionSystemId"] = 
             '{F4637010-03C3-42CD-B932-B48ADF3A6A54}'
var cpsystems = new Windows.Foundation.Collections.PropertySet();
cpsystems["{F4637010-03C3-42CD-B932-B48ADF3A6A54}"] = 
                "Windows.Media.Protection.PlayReady.PlayReadyWinRTTrustedInput";
mediaProtectionManager.properties["Windows.Media.Protection.MediaProtectionSystemIdMapping"] = cpsystems;

mediaProtectionManager.properties["Windows.Media.Protection.MediaProtectionContainerGuid"] = 
                "{9A04F079-9840-4286-AB92-E65BE0885F95}";
```

## 播放之前主動取得非永久性授權

本節說明如何在開始播放之前，主動取得非永久性授權。

在舊版的 PlayReady DRM 中，非永久性授權只能在播放期間被動取得。 在這個版本中，您可以在開始播放之前，主動取得非永久性授權。

1.  主動建立播放工作階段，讓非永久性授權可儲存於其中。 例如：

    ``` syntax
    var cpsystems = new Windows.Foundation.Collections.PropertySet();       
    cpsystems["{F4637010-03C3-42CD-B932-B48ADF3A6A54}"] = "Windows.Media.Protection.PlayReady.PlayReadyWinRTTrustedInput"; // PlayReady

    var pmpSystemInfo = new Windows.Foundation.Collections.PropertySet();
    pmpSystemInfo["Windows.Media.Protection.MediaProtectionSystemId"] = "{F4637010-03C3-42CD-B932-B48ADF3A6A54}";
    pmpSystemInfo["Windows.Media.Protection.MediaProtectionSystemIdMapping"] = cpsystems;
    var pmpServer = new Windows.Media.Protection.MediaProtectionPMPServer( pmpSystemInfo );
    ```

2.  將該播放工作階段繫結到授權取得類別。 例如：

    ``` syntax
    var licenseSessionProperties = new Windows.Foundation.Collections.PropertySet();
    licenseSessionProperties["Windows.Media.Protection.MediaProtectionPMPServer"] = pmpServer;
    var licenseSession = new Windows.Media.Protection.PlayReady.PlayReadyLicenseSession( licenseSessionProperties );
    ```

3.  建立授權服務要求。 例如：

    ``` syntax
    var laSR = licenseSession.CreateLAServiceRequest();
    ```

4.  使用從步驟 3 所建立的服務要求來執行授權取得。 授權會儲存在播放工作階段中。
5.  將該播放工作階段繫結到媒體來源，以進行播放。 例如：

    ``` syntax
    licenseSession.configureMediaProtectionManager( mediaProtectionManager );
    videoPlayer.msSetMediaProtectionManager( mediaProtectionManager );
    ```
    
## 新增安全停止功能

本節說明如何將安全停止功能新增到您的 UWP app。

安全停止功能讓 PlayReady 裝置能夠確實向媒體串流處理服務宣告任何指定內容的媒體播放已停止。 此功能確保您的媒體串流處理服務能在不同裝置上，針對指定帳戶正確地強制執行和報告使用限制。

傳送安全停止挑戰的主要案例有兩種：

-   當媒體呈現因已到達內容結尾，或當使用者在中間的某處停止呈現媒體而停止。
-   當先前的工作階段意外結束 (例如，因為系統或 App 當機)。 App 將需要針對任何待處理的安全停止工作階段進行查詢 (可能是在開機或關機時)，並從任何其他媒體播放個別傳送挑戰。

如需安全停止功能的實作範例，請參閱 PlayReady 範例中的 securestop.cs 檔案，其位於 [http://go.microsoft.com/fwlink/p/?linkid=331670&clcid=0x409](http://go.microsoft.com/fwlink/p/?linkid=331670)。

 

 






<!--HONumber=Mar16_HO1-->


