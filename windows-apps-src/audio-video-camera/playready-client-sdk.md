---
ms.assetid: DD8FFA8C-DFF0-41E3-8F7A-345C5A248FC2
description: 本主題說明如何將 PlayReady 保護的媒體內容新增到您的通用 Windows 平台 (UWP) app。
title: PlayReady DRM
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 4fac02f892c66a1bcf0b08986ae00a3a162b44ca
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/03/2018
ms.locfileid: "8476513"
---
# <a name="playready-drm"></a>PlayReady DRM



本主題說明如何將 PlayReady 保護的媒體內容新增到您的通用 Windows 平台 (UWP) app。

PlayReady DRM 讓開發人員可以建立 UWP app，能夠為使用者提供 PlayReady 內容，同時強制執行內容提供者所定義的存取規則。 本章節描述了適用於 windows 10，以及如何修改 PlayReady UWP app，以支援從舊版 Windows8.1 到 windows 10 版本所做的變更 Microsoft PlayReady DRM 所做的變更。
 
| 主題                                                                     | 說明                                                                                                                                                                                                                                                                             |
|---------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [硬體 DRM](hardware-drm.md)                                           | 本主題概觀說明如何將以 PlayReady 硬體為基礎的數位版權管理 (DRM) 新增到 UWP app。                                                                                                                                                                 |
| [搭配使用彈性資料流與 PlayReady](adaptive-streaming-with-playready.md) | 本文章說明如何將包含 Microsoft PlayReady 內容保護的彈性資料流多媒體內容新增到通用 Windows 平台 (UWP) app。 本功能目前支援 HTTP 即時資料流 (HLS) 與 HTTP 動態資料流 (DASH) 內容播放。 |

## <a name="whats-new-in-playready-drm"></a>PlayReady DRM 的新功能

下列清單說明適用於 windows 10 的 PlayReady DRM 所做的變更的新功能。

-   已新增硬體數位版權管理 (HWDRM)。

    硬體式內容保護支援能夠在多個裝置的平台上安全播放高畫質 (HD) 和超高畫質 (UHD) 的內容。 金鑰內容 (包括私密金鑰、內容金鑰，以及其他用來衍生或解除鎖定上述金鑰的金鑰內容)，以及已解密的壓縮和未壓縮的視訊範例會利用硬體安全性來進行保護。 使用硬體 DRM 時，由於 HWDRM 管線一定知道要使用的輸出，因此未知的啟用器 (播放到未知裝置 / 使用 downres 播放到未知裝置) 是不具意義的。 如需詳細資訊，請參閱[硬體 DRM](hardware-drm.md)。

-   PlayReady 不再是 appX 架構元件，而是內建的作業系統元件。 命名空間已從 **Microsoft.Media.PlayReadyClient** 變更為 [**Windows.Media.Protection.PlayReady**](https://msdn.microsoft.com/library/windows/apps/dn986454)。
-   下列定義 PlayReady 錯誤碼的標頭現在是 Windows 軟體開發套件 (SDK) 的一部分：Windows.Media.Protection.PlayReadyErrors.h 和 Windows.Media.Protection.PlayReadyResults.h。
-   提供主動取得非永久性授權。

    舊版的 PlayReady DRM 不支援主動取得非永久性授權。 這個功能已新增到這個版本。 這可減少第一個畫面的時間。 如需詳細資訊，請參閱[播放之前主動取得非永久性授權](#proactively-acquire-a-non-persistent-license-before-playback)。

-   提供在一則訊息中取得多個授權的功能。

    允許用戶端 App 在一則授權取得訊息中取得多個非永久性授權。 這樣可藉由取得多個內容片段的授權來減少第一個畫面的時間，同時使用者仍可瀏覽您的內容庫；這可在使用者選取要播放的內容時，防止授權取得延遲。 此外，它可藉由啟用包含多個金鑰識別碼 (KID) 的內容標頭，來讓音訊和視訊串流以不同的金鑰加密；這樣讓單一授權取得可針對內容檔案中的所有串流取得所有授權，而不需使用自訂邏輯和多個授權取得要求來達到相同結果。

-   已新增即時到期支援或限時授權 (LDL)。

    能夠在授權上設定即時到期，並在播放期間順暢地從到期的授權轉換為另一個 (有效) 授權。 在一則訊息中結合取得多個授權的功能時，這讓 App 能夠以非同步方式取得數個 LDL，同時使用者仍能瀏覽內容庫，而且一旦使用者選取要播放的內容之後，只會取得較長持續時間的授權。 之後，就能以更快速的方式開始播放 (因為授權已經可以使用)，而且，因為 App 已在 LDL 到期之前取得較長持續時間的授權，所以能夠流暢地繼續播放到內容結尾而不會中斷。

-   已新增非永久性授權鏈結。
-   已在非永久性授權上新增以時間為基礎之限制 (包括到期、在第一次播放之後到期，以及即時到期) 的支援。
-   已新增 HDCP 類型 1 (Windows10 為版本 2.2) 原則的支援。

    如需詳細資訊，請參閱[要考慮的事項](#things-to-consider)。

-   現已內含 Miracast 做為輸出。
-   已新增安全停止功能。

    安全停止功能讓 PlayReady 裝置能夠確實向媒體串流處理服務宣告任何指定內容的媒體播放已停止。 此功能確保您的媒體串流處理服務能在不同裝置上，針對指定帳戶正確地強制執行和報告使用限制。

-   已新增音訊與視訊各別授權。

    分離曲目可防止視訊被解碼為音訊，進而使內容保護能力更為健全。 新的標準要求音訊與視覺曲目要個別提供金鑰。

-   新增 MaxResDecode。

    即使擁有功能更強的金鑰 (但非授權)，這項新功能仍限制以最高解析度來播放內容。 其支援使用單一金鑰編碼多個資料流大小的情況。

已將下列的新介面、類別及列舉新增到 PlayReady DRM：

-   [**IPlayReadyLicenseAcquisitionServiceRequest**](https://msdn.microsoft.com/library/windows/apps/dn986077) 介面
-   [**IPlayReadyLicenseSession**](https://msdn.microsoft.com/library/windows/apps/dn986080) 介面
-   [**IPlayReadySecureStopServiceRequest**](https://msdn.microsoft.com/library/windows/apps/dn986090) 介面
-   [**PlayReadyLicenseSession**](https://msdn.microsoft.com/library/windows/apps/dn986309) 類別
-   [**PlayReadySecureStopIterable**](https://msdn.microsoft.com/library/windows/apps/dn986371) 類別
-   [**PlayReadySecureStopIterator**](https://msdn.microsoft.com/library/windows/apps/dn986375) 類別
-   [**PlayReadyHardwareDRMFeatures**](https://msdn.microsoft.com/library/windows/apps/dn986265) 列舉值

已建立新的範例來示範如何使用 PlayReady DRM 的新功能。 您可以從 [http://go.microsoft.com/fwlink/p/?linkid=331670&clcid=0x409](http://go.microsoft.com/fwlink/p/?linkid=331670) 下載範例。

## <a name="things-to-consider"></a>要考慮的事項

-   PlayReady DRM 現在支援 HDCP 類型 1 (支援 HDCP 版本 2.1 或更新版本)。 PlayReady 會在適用於裝置的授權中，包含要強制執行的 HDCP 類型限制原則。 在 Windows10 上，此原則將會強制執行已經執行的 HDCP 2.2 或更新版本。 此功能可以在 PlayReady 伺服器 3.0 版 SDK 授權中啟用 (此伺服器會使用 HDCP 類型限制 GUID，在授權中控制此原則)。 如需詳細資訊，請參閱 [PlayReady 規範和穩健性規則](http://www.microsoft.com/playready/licensing/compliance/)。
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

## <a name="output-protection"></a>輸出保護

下一節會說明在 PlayReady 授權中使用適用於 Windows10 的 PlayReady DRM 搭配輸出保護原則時，所產生的行為。

PlayReady DRM 支援 **Microsoft PlayReady 可延伸媒體權利規格**中包含的輸出保護層級。 您可在 PlayReady 授權產品的隨附文件套件中，找到這份文件。

> [!NOTE]
> 授權伺服器可設定的輸出保護層級允許值，受到 [PlayReady 相容性規則](https://www.microsoft.com/playready/licensing/compliance/)所規範。

PlayReady DRM 僅允許在 PlayReady 相容性規則中指定的輸出連接器上，播放具輸出保護原則的內容。 如需有關 PlayReady 相容性規則指定連接器詞彙的詳細資訊，請參閱[適用於 PlayReady 相容性與穩健性規則的定義詞彙](https://www.microsoft.com/playready/licensing/compliance/)。

本節著重探討適用於 Windows10 之 PlayReady DRM，以及適用於 Windows10 之 PlayReady 硬體 DRM 的輸出保護案例，亦適用於部分 Windows 用戶端。 使用 PlayReady HWDRM 時，會在 Windows TEE 實作過程中強制執行所有輸出保護 (請參閱[硬體 DRM](hardware-drm.md))。 因此，與使用 PlayReady SWDRM (軟體 DRM) 時的部分行為會略有差異：

* 適用於未壓縮數位視訊 270 的輸出保護層級 (OPL) 支援：適用於 Windows10 的 PlayReady HWDRM 不支援向下解析度，且會強制執行 HDCP (高頻寬數位內容保護)。 建議適用於 HWDRM 的高畫質內容應具備超過 270 的 OPL (雖然並非必要)。 此外，您應該在授權 (HDCP 版本 2.2 或更新版本) 中設定 HDCP 類型限制。
* 不同於 SWDRM，HWDRM 會根據功能最少的監視器，在所有監視器上強制執行輸出保護。 例如，如果使用者連接了兩台監視器，其中一台監視器支援 HDCP，而另一台不支援，則如果授權要求 HDCP (即使只在支援 HDCP 監視器上呈現內容)，皆將無法播放。 在 SWDRM 中，只要內容僅在支援 HDCP 的監視器上顯示，就能播放內容。
* 除非內容金鑰和授權符合下列條件，否則不保證 HWDRM 可供用戶端使用且安全：
    * 用於視訊內容金鑰的授權，其最低安全性層級必須為 3000。
    * 音訊必須以有別於視訊的內容金鑰加密，且適用於音訊的授權其最低安全性層級必須為 2000。 否則音訊可能不會進行加密處理。
* 所有 SWDRM 案例，皆要求適用於音訊和/或視訊內容金鑰的 PlayReady 授權最低安全性等級應不超過 2000。

### <a name="output-protection-levels"></a>輸出保護層級

下表概述 PlayReady 授權中各個 OPL 間的對應，以及適用於 Windows10 的 PlayReady DRM 如何強制執行這些對應。

#### <a name="video"></a>視訊

<table>
    <tr>
        <th rowspan="2">OPL</th>
        <th>壓縮的數位視訊</th>
        <th colspan="2">未壓縮的數位視訊</th>
        <th>類比電視</th>
    </tr>
    <tr>
        <th>任何值</th>
        <th colspan="2">HDMI、DVI、DisplayPort、MHL</th>
        <th>元件、複合式</th>
    </tr>
    <tr>
        <th>100</th>
        <td rowspan="6">無\*</td>
        <td colspan="2">傳遞內容</td>
        <td>傳遞內容</td>
    </tr>
    <tr>
        <th>150</th>
        <td colspan="2" rowspan="2">無\*</td>
        <td>使用 CGMS-A CopyNever 或無法使用 CGMS-A 時傳遞內容</td>
    </tr>
    <tr>
        <th>200</th>
        <td>使用 CGMS-A CopyNever 時傳遞內容</td>
    </tr>
    <tr>
        <th>250</th>
        <td colspan="2">嘗試使用 HDCP，但無論結果如何皆傳遞內容</td>
        <td rowspan="5">無\*</td>
    </tr>
    <tr>
        <th>270</th>
        <td><b>SWDRM</b>：嘗試使用 HDCP。 若 HDCP 無法使用，則電腦會將每個畫面的有效解析度限制為 520,000 像素，並傳遞內容</td>
        <td><b>HWDRM</b>：使用 HDCP 傳遞內容。 若 HDCP 無法使用，則會將傳送到 HDMI/DVI 連接埠的播放封鎖</td>
    </tr>
    <tr>
        <th>300</th>
        <td colspan="2">
            <p>
                **「未」定義 HDCP 類型限制時：** 會使用 HDCP 來傳遞內容。 若 HDCP 無法使用，則會將傳送到 HDMI/DVI 連接埠的播放封鎖。
            </p>
            <p>
                **「已」定義 HDCP 類型限制時：** 會使用 HDCP 2.2 來傳遞內容並將內容串流類型設定為 1。 若 HDCP 無法使用或是內容串流類型無法設定為 1，則會將傳送到 HDMI/DVI 連接埠的播放封鎖。
            </p>
        </td>
    </tr>
    <tr>
        <th>400</th>
        <td rowspan="2">Windows 10 絕對不會將壓縮的數位視訊內容傳遞至輸出，無論後續 OPL 值為何皆然。 如需有關壓縮的數位視訊內容詳細資訊，請參閱<a href="https://www.microsoft.com/playready/licensing/compliance/">適用於 PlayReady 產品的相容性規則</a>。</td>
        <td colspan="2" rowspan="2">無\*</td>
    </tr>
    <tr>
        <th>500</th>
    </tr>
</table>
<br/>

\* 並非所有輸出保護層級的值都可由授權伺服器設定。 如需詳細資訊，請參閱 [PlayReady 相容性規則](https://www.microsoft.com/playready/licensing/compliance/)。

#### <a name="audio"></a>音訊

<table>
    <tr>
        <th rowspan="2">OPL</th>
        <th>壓縮的數位音訊</th>
        <th>未壓縮的數位音訊</th>
        <th>類比或 USB 音訊</th>
    </tr>
    <tr>
        <th>HDMI、DisplayPort、MHL</th>
        <th>HDMI、DisplayPort、MHL</th>
        <th>任何值</th>
    </tr>
    <tr>
        <th>100</th>
        <td rowspan="3">傳遞內容</td>
        <td>傳遞內容</td>
        <td rowspan="5">傳遞內容</td>
    </tr>
    <tr>
        <th>150</th>
        <td rowspan="4">「不」傳遞內容</td>
    </tr>
    <tr>
        <th>200</th>
    </tr>
    <tr>
        <th>250</th>
        <td>若在 HDMI、DisplayPort 或 MHL 上使用 HDCP，或是使用 SCMS 並設為 CopyNever，即會傳遞內容</td>
    </tr>
    <tr>
        <th>300</th>
        <td>若在 HDMI、DisplayPort 或 MHL 上使用 HDCP，即會傳遞內容</td>
    </tr>
</table>
<br/>

### <a name="miracast"></a>Miracast

PlayReady DRM 可讓您在使用 HDCP 2.0 或更新版本時，立即透過 Miracast 輸出播放內容。 不過在 Windows10 上，Miracast 會視為*數位*輸出。 如需關於 Miracast 案例的詳細資訊，請參閱 [PlayReady 相容性規則](https://www.microsoft.com/playready/licensing/compliance/)。 下表概述 PlayReady 授權中各個 OPL 間的對應，以及 PlayReady DRM 如何在 Miracast 輸出上強制執行這些對應。

<table>
    <tr>
        <th>OPL</th>
        <th>壓縮的數位音訊</th>
        <th>未壓縮的數位音訊</th>
        <th>壓縮的數位視訊</th>
        <th>未壓縮的數位視訊</th>
    </tr>
    <tr>
        <th>100</th>
        <td rowspan="4">使用 HDCP 2.0 或更新版本時傳遞內容。 若無法使用，則「不會」傳遞內容</td>
        <td>使用 HDCP 2.0 或更新版本時傳遞內容。 若無法使用，則「不會」傳遞內容</td>
        <td rowspan="6">無\*</td>
        <td>使用 HDCP 2.0 或更新版本時傳遞內容。 若無法使用，則「不會」傳遞內容</td>
    </tr>
    <tr>
        <th>150</th>
        <td rowspan="3">「不」傳遞內容</td>
        <td rowspan="2">無\*</td>
    </tr>
    <tr>
        <th>200</th>
    </tr>
    <tr>
        <th>250</th>
        <td rowspan="2">使用 HDCP 2.0 或更新版本時傳遞內容。 若無法使用，則「不會」傳遞內容</td>
    </tr>
    <tr>
        <th>270</th>
        <td colspan="2">無\*</td>
    </tr>
    <tr>
        <th>300</th>
        <td>使用 HDCP 2.0 或更新版本時傳遞內容。 若無法使用，則「不會」傳遞內容</td>
        <td>「不」傳遞內容</td>
        <td>
            <p>
                **「未」定義 HDCP 類型限制時：** 會在使用 HDCP 2.0 或更新版本時傳遞內容。 若無法使用，則「不會」傳遞內容。
            </p>
            <p>
                **「已」定義 HDCP 類型限制時：** 會使用 HDCP 2.2 來傳遞內容並將內容串流類型設定為 1。 若 HDCP 無法使用或是內容串流類型無法設定為 1，則「不會」傳遞內容。
            </p>        
        </td>
    </tr>
    <tr>
        <th>400</th>
        <td rowspan="2" colspan="2">無\*</td>
        <td rowspan="2">Windows 10 絕對不會將壓縮的數位視訊內容傳遞至輸出，無論後續 OPL 值為何皆然。 如需有關壓縮的數位視訊內容詳細資訊，請參閱<a href="https://www.microsoft.com/playready/licensing/compliance/">適用於 PlayReady 產品的相容性規則</a>。</td>
        <td rowspan="2">無\*</td>
    </tr>
    <tr>
        <th>500</th>
    </tr>
</table>
<br/>

\* 並非所有輸出保護層級的值都可由授權伺服器設定。 如需詳細資訊，請參閱 [PlayReady 相容性規則](https://www.microsoft.com/playready/licensing/compliance/)。

### <a name="additional-explicit-output-restrictions"></a>其他的明確輸出限制

下表說明適用於 Windows 10 的 PlayReady DRM，如何實作明確的數位視訊輸出保護限制。

<table>
    <tr>
        <th>案例</th>
        <th>GUID</th>
        <th>如果...</th>
        <th>則...</th>
    </tr>
    <tr>
        <th>最大有效解析度解碼大小</th>
        <td>9645E831-E01D-4FFF-8342-0A720E3E028F</td>
        <td>連接的輸出為：數位視訊輸出、Miracast、HDMI、DVI 等等。</td>
        <td>
            <p>
                採用下列限制時傳遞內容︰  
            </p>
            <ul>
                <li>(a) 畫面寬度必須小於或等於最大像素畫面寬度，且畫面高度小於或等於最大像素畫面高度，或</li>
                <li>(b) 畫面高度必須小於或等於最大像素畫面寬度，且畫面寬度小於或等於最大像素畫面高度</li>
            </ul>                   
        </td>
    </tr>
    <tr>
        <th>HDCP 類型限制</th>
        <td>ABB2C6F1-E663-4625-A945-972D17B231E7</td>
        <td>連接的輸出為：數位視訊輸出、Miracast、HDMI、DVI 等等。</td>
        <td>使用 HDCP 2.2 傳遞內容並將內容串流類型設為 1。 若 HDCP 2.2 無法使用或是內容串流類型無法設定為 1，則「不會」傳遞內容。 此外，也必須指定一個值大於或等於 271 的未壓縮數位視訊輸出保護層級</td>
    </tr>
</table>
<br/>

下表說明適用於 Windows10 的 PlayReady DRM 如何實作明確的類比視訊輸出保護限制。

<table>
    <tr>
        <th>案例</th>
        <th>GUID</th>
        <th>如果...</th>
        <th colspan="2">則...</th>
    </tr>
    <tr>
        <th>類比電腦監視器</th>
        <td>D783A191-E083-4BAF-B2DA-E69F910B3772</td>
        <td>連接的輸出為：VGA、DVI &ndash;類比等等。</td>
        <td><b>SWDRM：</b>電腦會將每個畫面的有效解析度限制為 520,000 像素，並傳遞內容</td>
        <td><b>HWDRM：</b>「不」傳遞內容</td>
    </tr>
    <tr>
        <th>類比色差</th>
        <td>811C5110-46C8-4C6E-8163-C0482A15D47E</td>
        <td>連接的輸出為：色差</td>
        <td><b>SWDRM：</b>電腦會將每個畫面的有效解析度限制為 520,000 像素，並傳遞內容</td>
        <td><b>HWDRM：</b>「不」傳遞內容</td>
    </tr>
    <tr>
        <th rowspan="2">類比電視輸出</th>
        <td>2098DE8D-7DDD-4BAB-96C6-32EBB6FABEA3</td>
        <td>類比電視 OPL 小於 151</td>
        <td colspan="2">必須使用 CGMS-A</td>
    </tr>
    <tr>
        <td>225CD36F-F132-49EF-BA8C-C91EA28E4369</td>
        <td>類比電視 OPL 小於 101，且授權不包含 2098DE8D-7DDD-4BAB-96C6-32EBB6FABEA3</td>
        <td colspan="2">必須嘗試使用 CGMS-A，但無論結果如何皆可播放內容</td>
    </tr>
    <tr>
        <th>自動增益控制項和色條</th>
        <td>C3FD11C6-F8B7-4D20-B008-1DB17D61F2DA</td>
        <td>傳遞內容的解析度小於或等於 520,000 像素，以類比電視輸出</td>
        <td colspan="2">解析度小於 520,000 像素時針對色差視訊和 PAL 模式設為「僅 AGC」，並在解析度小於 520,000 像素時針對 NTSC 設定「AGC」和色條資訊，根據表格 3.5.7.3 而定 (位於「相容性規則」中)。</td>
    </tr>
    <tr>
        <th>僅數位輸出</th>
        <td>760AE755-682A-41E0-B1B3-DCDF836A7306</td>
        <td>連接的輸出為類比</td>
        <td colspan="2">不傳遞內容</td>
    </tr>
</table>
<br/>

> [!NOTE]
> 播放時若使用諸如「Mini DisplayPort 轉 VGA」等轉接卡硬體鎖，則 Windows 10 會將輸出視為數位視訊輸出，而無法強制執行類比視訊原則。

下表說明可在其他情況下啟用播放之適用於 Windows 10 的 PlayReady DRM 實作。

<table>
    <tr>
        <th>案例</th>
        <th>GUID</th>
        <th>如果...</th>
        <th colspan="2">則...</th>
    </tr>
    <tr>
        <th>未知輸出</th>
        <td>786627D8-C2A6-44BE-8F88-08AE255B01A7</td>
        <td>若無法適度判斷輸出，或是無法使用圖形驅動程式建立 OPM</td>
        <td><b>SWDRM：</b>傳遞內容</td>
        <td><b>HWDRM：</b>「不」傳遞內容</td>
    </tr>
    <tr>
        <th>具限制的不明輸出</th>
        <td>B621D91F-EDCC-4035-8D4B-DC71760D43E9</td>
        <td>若無法適度判斷輸出，或是無法使用圖形驅動程式建立 OPM</td>
        <td><b>SWDRM：</b>電腦會將每個畫面的有效解析度限制為 520,000 像素，並傳遞內容</td>
        <td><b>HWDRM：</b>「不」傳遞內容</td>
    </tr>
</table>
<br/>

## <a name="prerequisites"></a>先決條件

開始建立 PlayReady 保護的 UWP app 之前，需要在系統上安裝下列軟體：

-   Windows 10。
-   如果您正在針對 PlayReady DRM 編譯任何範例適用於 UWP app，您必須使用 Microsoft Visual Studio2015 或更新版本來編譯範例。 您仍然可以使用 Microsoft Visual Studio2013 來編譯任何來自適用於 Windows8.1 市集應用程式的 PlayReady DRM 的範例。

<!--This is no longer available-->
<!--If you are planning to play back MPEG-2/H.262 content on your app, you must also download and install [Windows 8.1 Media Center Pack](http://go.microsoft.com/fwlink/p/?LinkId=626876).-->

## <a name="playready-uwp-app-migration-guide"></a>PlayReady UWP app 移轉指南

本節包含如何將您現有的 PlayReady Windows 8.x 市集 app 移轉至 windows 10 的相關資訊。

Windows 10 上的 PlayReady UWP app 的命名空間已變更為[**Windows.Media.Protection.PlayReady**](https://msdn.microsoft.com/library/windows/apps/dn986454) **Microsoft.Media.PlayReadyClient** 。 這表示您必須在程式碼中搜尋舊的命名空間，並取代為新的命名空間。 您仍將參考 winmd 檔案。 它是 windows 10 作業系統上 windows.media.winmd 的一部分。 它屬於 Windows SDK 中的 windows.winmd。 對於 UWP，會在 windows.foundation.univeralappcontract.winmd 中參照它。

若要播放 PlayReady 保護的高畫質 (HD) 內容 (1080p) 及超高畫質 (UHD) 內容，您需要實作 PlayReady 硬體 DRM。 如需如何實作 PlayReady 硬體 DRM 的相關資訊，請參閱[硬體 DRM](hardware-drm.md)。

硬體 DRM 不支援某些內容。 如需停用硬體 DRM 和啟用軟體 DRM 的相關資訊，請參閱[覆寫硬體 DRM](hardware-drm.md#override-hardware-drm)。

關於媒體保護管理員，請確定您的程式碼包含下列設定 (如果程式碼中還未包含)。

```cs
var mediaProtectionManager = new Windows.Media.Protection.MediaProtectionManager();

mediaProtectionManager.Properties["Windows.Media.Protection.MediaProtectionSystemId"] = 
             '{F4637010-03C3-42CD-B932-B48ADF3A6A54}'
var cpsystems = new Windows.Foundation.Collections.PropertySet();
cpsystems["{F4637010-03C3-42CD-B932-B48ADF3A6A54}"] = 
                "Windows.Media.Protection.PlayReady.PlayReadyWinRTTrustedInput";
mediaProtectionManager.Properties["Windows.Media.Protection.MediaProtectionSystemIdMapping"] = cpsystems;

mediaProtectionManager.Properties["Windows.Media.Protection.MediaProtectionContainerGuid"] = 
                "{9A04F079-9840-4286-AB92-E65BE0885F95}";
```

## <a name="proactively-acquire-a-non-persistent-license-before-playback"></a>播放之前主動取得非永久性授權

本節說明如何在開始播放之前，主動取得非永久性授權。

在舊版的 PlayReady DRM 中，非永久性授權只能在播放期間被動取得。 在這個版本中，您可以在開始播放之前，主動取得非永久性授權。

1.  主動建立播放工作階段，讓非永久性授權可儲存於其中。 例如：

    ```cs
    var cpsystems = new Windows.Foundation.Collections.PropertySet();       
    cpsystems["{F4637010-03C3-42CD-B932-B48ADF3A6A54}"] = "Windows.Media.Protection.PlayReady.PlayReadyWinRTTrustedInput"; // PlayReady

    var pmpSystemInfo = new Windows.Foundation.Collections.PropertySet();
    pmpSystemInfo["Windows.Media.Protection.MediaProtectionSystemId"] = "{F4637010-03C3-42CD-B932-B48ADF3A6A54}";
    pmpSystemInfo["Windows.Media.Protection.MediaProtectionSystemIdMapping"] = cpsystems;
    var pmpServer = new Windows.Media.Protection.MediaProtectionPMPServer( pmpSystemInfo );
    ```

2.  將該播放工作階段繫結到授權取得類別。 例如：

    ```cs
    var licenseSessionProperties = new Windows.Foundation.Collections.PropertySet();
    licenseSessionProperties["Windows.Media.Protection.MediaProtectionPMPServer"] = pmpServer;
    var licenseSession = new Windows.Media.Protection.PlayReady.PlayReadyLicenseSession( licenseSessionProperties );
    ```

3.  建立授權服務要求。 例如：

    ```cs
    var laSR = licenseSession.CreateLAServiceRequest();
    ```

4.  使用從步驟 3 所建立的服務要求來執行授權取得。 授權會儲存在播放工作階段中。
5.  將該播放工作階段繫結到媒體來源，以進行播放。 例如：

    ```cs
    licenseSession.configureMediaProtectionManager( mediaProtectionManager );
    videoPlayer.msSetMediaProtectionManager( mediaProtectionManager );
    ```
    
## <a name="query-for-protection-capabilities"></a>查詢保護功能
從 Windows 10 版本 1703 開始，您可以查詢 HW DRM 功能，例如解碼轉碼器、解析度和輸出保護 (HDCP)。 查詢是使用 [**IsTypeSupported**](https://docs.microsoft.com/uwp/api/windows.media.protection.protectioncapabilities.istypesupported) 方法來執行，這個方法接受表示查詢支援功能的字串以及指定查詢套用所在金鑰系統的字串。 如需支援的字串值清單，請參閱 [**IsTypeSupported**](https://docs.microsoft.com/uwp/api/windows.media.protection.protectioncapabilities.istypesupported) 的 API 參考頁面。 下列程式碼範例說明此方法的使用方式。  

    ```cs
    using namespace Windows::Media::Protection;

    ProtectionCapabilities^ sr = ref new ProtectionCapabilities();

    ProtectionCapabilityResult result = sr->IsTypeSupported(
    L"video/mp4; codecs=\"avc1.640028\"; features=\"decode-bpp=10,decode-fps=29.97,decode-res-x=1920,decode-res-y=1080\"",
    L"com.microsoft.playready");

    switch (result)
    {
        case ProtectionCapabilityResult::Probably:
        // Queue up UHD HW DRM video
        break;

        case ProtectionCapabilityResult::Maybe:
        // Check again after UI or poll for more info.
        break;

        case ProtectionCapabilityResult::NotSupported:
        // Do not queue up UHD HW DRM video.
        break;
    }
    ```
## <a name="add-secure-stop"></a>新增安全停止功能

本節說明如何將安全停止功能新增到您的 UWP app。

安全停止功能讓 PlayReady 裝置能夠確實向媒體串流處理服務宣告任何指定內容的媒體播放已停止。 此功能確保您的媒體串流處理服務能在不同裝置上，針對指定帳戶正確地強制執行和報告使用限制。

傳送安全停止挑戰的主要案例有兩種：

-   當媒體呈現因已到達內容結尾，或當使用者在中間的某處停止呈現媒體而停止。
-   當先前的工作階段意外結束 (例如，因為系統或 App 當機)。 App 將需要針對任何待處理的安全停止工作階段進行查詢 (可能是在開機或關機時)，並從任何其他媒體播放個別傳送挑戰。

如需安全停止功能的實作範例，請參閱 PlayReady 範例中的 securestop.cs 檔案，其位於 [http://go.microsoft.com/fwlink/p/?linkid=331670&clcid=0x409](http://go.microsoft.com/fwlink/p/?linkid=331670)。

## <a name="use-playready-drm-on-xbox-one"></a>在 Xbox One 上使用 PlayReady DRM

若要在 Xbox One 上 UWP app 中使用 PlayReady DRM，您首先需要註冊您的[合作夥伴中心](https://partner.microsoft.com/dashboard)帳戶，您將使用 PlayReady 授權的應用程式發行使用。 若要執行這項作業，您可以使用兩種方法：

* 請您在 Microsoft 的連絡人要求權限。
* 藉由將合作夥伴中心帳戶和公司名稱傳送到來請求授權[pronxbox@microsoft.com](mailto:pronxbox@microsoft.com)。

當您收到授權之後，您必須將額外的 `<DeviceCapability>` 新增到應用程式資訊清單。 您必須手動新增這個項目，因為目前在應用程式資訊清單設計工具中無法使用此設定。 執行下列步驟以進行設定：

1. 在 Visual Studio 中開啟專案，開啟 **\[方案總管\]**，以滑鼠右鍵按一下 **\[Package.appxmanifest\]**。
2. 選取 **\[開啟方式\]**，選擇 **\[XML (文字) 編輯器\]**，然後按一下 **\[確定\]**。
3. 在 `<Capabilities>` 標記之間，新增下列 `<DeviceCapability>`：

    ```xml
    <DeviceCapability Name="6a7e5907-885c-4bcb-b40a-073c067bd3d5" />
    ```

4. 儲存檔案。

此外，還有最後一個在 Xbox One 上使用 PlayReady 的考量：開發套件上具有 SL150 的限制 (也就是它們無法播放 SL2000 或 SL3000 內容)。 零售裝置可以播放具有更高安全性層級的內容，但若要在開發人員套件上測試 App，您必須使用 SL150 內容。 您可以使用下列其中一種方法來測試此內容：

* 使用需要 SL150 授權的策劃測試內容。
* 實作邏輯來使只有特定的已驗證測試帳戶可以針對特定內容取得 SL150 授權。

請使用最適合您公司和產品的方法。


## <a name="see-also"></a>另請參閱
- [媒體播放](media-playback.md)




