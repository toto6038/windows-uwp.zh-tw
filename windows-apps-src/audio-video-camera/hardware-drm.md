---
author: drewbatgit
ms.assetid: A7E0DA1E-535A-459E-9A35-68A4150EE9F5
description: 本主題概觀說明如何將以 PlayReady 硬體為基礎的數位版權管理 (DRM) 新增到通用 Windows 平台 (UWP) App。
title: 硬體 DRM
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 160a4ab0ff5bdc40ea46ff6d8fb9fd8e47f560e3
ms.sourcegitcommit: 49aab071aa2bd88f1c165438ee7e5c854b3e4f61
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/09/2018
ms.locfileid: "4461747"
---
# <a name="hardware-drm"></a>硬體 DRM


本主題概觀說明如何將以 PlayReady 硬體為基礎的數位版權管理 (DRM) 新增到通用 Windows 平台 (UWP) App。

> [!NOTE] 
> 眾多裝置皆支援以硬體為基礎的 PlayReady DRM，包括諸如電視套組、手機和平板電腦等各種 Windows 與非 Windows 裝置。 針對支援 PlayReady 硬體 DRM 的 Windows 裝置，它必須執行 Windows 10 且具有支援的硬體設定。

內容提供者正不斷地朝向針對授與權限以播放 App 中最高價值的內容，提供硬體式保護的方向努力。 健全的密碼編譯核心硬體實作支援已新增到 PlayReady，以符合此需求。 此支援能夠在多個裝置的平台上安全播放高畫質 (1080p) 和超高畫質 (UHD) 的內容。 金鑰內容 (包括私密金鑰、內容金鑰，以及其他用來衍生或解除鎖定上述金鑰的金鑰內容)，以及已解密的壓縮和未壓縮的視訊範例會利用硬體安全性來進行保護。

## <a name="windows-tee-implementation"></a>Windows TEE 實作

本主題提供 Windows 10 如何實作受信任的執行環境 (TEE) 的簡要概觀。

Windows TEE 實作的細節不在本文件的討論範圍內。 不過，對於標準移植套件 TEE 連接埠和 Windows 連接埠間差異的簡短討論是很有助益的。 Windows 會實作 OEM Proxy 層，並將序列化的 PRITEE 函式呼叫傳輸到 Windows 媒體基礎子系統中的使用者模式驅動程式。 這最終將路由傳送到 Windows TrEE (受信任的執行環境) 驅動程式或 OEM 的圖形驅動程式。 這些方法的細節都不在本文件的討論範圍內。 下圖顯示適用於 Windows 連接埠的一般元件互動。 如果您想要開發 Windows PlayReady TEE 實作，您可以連絡 <WMLA@Microsoft.com>。

![Windows TEE 元件圖表](images/windowsteecomponentdiagram720.jpg)

## <a name="considerations-for-using-hardware-drm"></a>使用硬體 DRM 的考量

本主題提供一份簡短清單，其中包含開發設計來使用硬體 DRM 的 App 時應考慮的項目。 如同在 [PlayReady DRM](playready-client-sdk.md#output-protection) 中所述，透過適用於 Windows 10 的 PlayReady HWDRM 可在整個 Windows TEE 實作過程中強制執行所有輸出保護，因此會產生一些輸出保護行為：

-   **適用於未壓縮數位視訊 270 的輸出保護層級 (OPL) 支援：** 適用於 Windows 10 的 PlayReady HWDRM 不支援向下解析度，且會強制執行 HDCP。 建議適用於 HWDRM 的高畫質內容應具備超過 270 的 OPL (雖然並非必要)。 此外，建議您在授權 (Windows 10 為 HDCP 2.2 版) 中設定 HDCP 類型限制。
-   **不同於軟體 DRM (SWDRM)，系統會根據功能最少的監視器，在所有監視器上強制執行輸出保護。** 例如，如果使用者連接了兩台監視器，其中一台監視器支援 HDCP，而另一台不支援，則如果授權要求 HDCP (即使只在支援 HDCP 監視器上呈現內容)，播放都將失敗。 在軟體 DRM 中，只要內容僅在支援 HDCP 的監視器上顯示，就能播放內容。
-   **除非內容金鑰和授權符合下列條件**，否則不保證 HWDRM 可供用戶端使用且安全：
    -   適用於視訊內容的授權，其最低安全性層級屬性必須為 3000。
    -   音訊必須以有別於視訊的內容金鑰加密，且適用於音訊的授權其最低安全性層級屬性必須為 2000。 否則音訊可能不會進行加密處理。
    
此外，您在使用 HWDRM 時應考量下列事項：

-   不支援受保護的媒體處理序 (PMP)。
-   不支援 Windows Media 視訊 (亦稱 VC-1) (請參閱[覆寫硬體 DRM](#override-hardware-drm))。
-   不支援多個圖形處理器 (GPU) 的永久性授權。

若要在具有多個 GPU 的電腦上處理永久性授權，請考慮下列案例：

1.  客戶購買配備整合式圖形卡的新電腦。
2.  客戶使用的 App 會在使用硬體 DRM 時取得永久性授權。
3.  永久性授權現在已繫結至該圖形卡的硬體金鑰。
4.  客戶接著安裝新的圖形卡。
5.  雜湊資料存放區 (HDS) 中的所有授權都繫結至整合式視訊卡，但是客戶現在想要使用新安裝的圖形卡來播放受保護的內容。

為了防止播放因為硬體無法解密授權而失敗，PlayReady 會針對它遇到的每張圖形卡，使用不同的 HDS。 這將導致 PlayReady 嘗試針對 PlayReady 通常已具有授權的一部分內容取得授權 (也就是在軟體 DRM 案例或任何沒有硬體變更的案例中，PlayReady 不需要重新取得授權)。 因此，如果 App 在使用硬體 DRM 時取得永久性授權，則您的 App 必須能夠處理下列情況：如果使用者安裝 (或解除安裝) 圖形卡，授權便會有效地「遺失」。 因為這不是常見案例，所以，您可能決定當內容在硬體變更之後不再播放時再來處理支援請求，而不是想出如何在用戶端/伺服器程式碼中處理硬體變更的方式。

## <a name="override-hardware-drm"></a>覆寫硬體 DRM

本節說明如果要播放的內容不支援硬體 DRM，則該如何覆寫硬體 DRM (HWDRM)。

根據預設，如果系統支援硬體 DRM，就會使用它。 不過，硬體 DRM 不支援某些內容。 這其中一個範例是雞尾酒內容。 有一個範例是所有使用 H.264 和 HEVC 以外之視訊轉碼器的內容。 另一個範例則是 HEVC 內容，因為有些硬體 DRM 將支援 HEVC，而有些不會。 因此，如果您想要播放部分內容，但硬體 DRM 在有問題的系統上不支援該內容，則您可以選擇不使用硬體 DRM。

下列範例示範如何選擇不使用硬體 DRM。 切換之前，您只需執行此動作。 此外，請確定記憶體中沒有任何 PlayReady 物件，否則會發生未定義的行為。

```js
var applicationData = Windows.Storage.ApplicationData.current;
var localSettings = applicationData.localSettings.createContainer("PlayReady", Windows.Storage.ApplicationDataCreateDisposition.always);
localSettings.values["SoftwareOverride"] = 1;
```

若要切換回硬體 DRM，可將 **SoftwareOverride** 值設定為 **0**。

每次播放媒體時，您都必須將 **MediaProtectionManager** 設定為：

```js
mediaProtectionManager.properties["Windows.Media.Protection.UseSoftwareProtectionLayer"] = true;
```

如果您正在使用硬體 DRM 或軟體 DRM 是查看 C:\\Users\\ 的最佳方式&lt;使用者名稱&gt;\\AppData\\Local\\Packages\\&lt;應用程式名稱&gt;\\LocalCache\\PlayReady\\\*

-   如果有 mspr.hds 檔案，就表示您正在使用軟體 DRM。
-   如果還有另一個 *.hds 檔案，表示您在使用硬體 DRM。
-   您也可以刪除整個 PlayReady 資料夾，然後重試您的測試。

## <a name="detect-the-type-of-hardware-drm"></a>偵測硬體 DRM 的類型

本節說明如何偵測系統上支援何種類型的硬體 DRM。

您可以使用 [**PlayReadyStatics.CheckSupportedHardware**](https://msdn.microsoft.com/library/windows/apps/dn986441) 方法，來判斷系統是否支援特定的硬體 DRM 功能。 例如：

```csharp
bool isFeatureSupported = PlayReadyStatics.CheckSupportedHardware(PlayReadyHardwareDRMFeatures.HEVC);
```

[**PlayReadyHardwareDRMFeatures**](https://msdn.microsoft.com/library/windows/apps/dn986265) 列舉包含可查詢之硬體 DRM 功能值的有效清單。 若要判斷是否支援硬體 DRM，可在查詢中使用 **HardwareDRM** 成員。 若要判斷硬體是否支援高效率視訊編碼 (HEVC)/H.265 轉碼器，可在查詢中使用 **HEVC** 成員。

您也可以使用 [**PlayReadyStatics.PlayReadyCertificateSecurityLevel**](https://msdn.microsoft.com/library/windows/apps/windows.media.protection.playready.playreadystatics.playreadycertificatesecuritylevel.aspx) 屬性來取得用戶端憑證的安全性層級，以判斷是否支援硬體 DRM。 除非傳回的憑證安全性層級大於或等於 3000，否則用戶端就不會進行個人化或佈建 (在此案例中，這個屬性會傳回 0)，或者硬體 DRM 不會處於使用狀態 (在此案例中，這個屬性會傳回小於 3000 的值)。

### <a name="detecting-support-for-aes128cbc-hardware-drm"></a>偵測對 AES128CBC 硬體 DRM 的支援
從 Windows 10 版本 1709 開始，您可以透過呼叫 **[PlayReadyStatics.CheckSupportedHardware](https://msdn.microsoft.com/library/windows/apps/dn986441)** 並指定列舉值 [**PlayReadyHardwareDRMFeatures.Aes128Cbc**](https://msdn.microsoft.com/library/windows/apps/dn986265) 來偵測裝置是否支援 AES128CBC 硬體加密。 在舊版 Windows 10 中，指定這個值會造成例外狀況。 基於這個原因，在呼叫 **CheckSupportedHardware** 之前，您應該先透過呼叫 **[ApiInformation.IsApiContractPresent](https://docs.microsoft.com/uwp/api/windows.foundation.metadata.apiinformation.isapicontractpresent)** 並指定主要合約版本 5 來檢查是否存在列舉值。

```csharp
bool supportsAes128Cbc = ApiInformation.IsApiContractPresent("Windows.Foundation.UniversalApiContract", 5);

if (supportsAes128Cbc)
{
    supportsAes128Cbc = PlayReadyStatics.CheckSupportedHardware(PlayReadyHardwareDRMFeatures.Aes128Cbc);
}
```

## <a name="see-also"></a>請參閱
- [PlayReady DRM](playready-client-sdk.md)
