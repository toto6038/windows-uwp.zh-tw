---
title: 藍牙開發人員常見問題集
description: 本文包含有關 UWP 藍牙 API 常見問題的解答。
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.assetid: e7dee32d-3756-430d-a026-32c1ee288a85
ms.localizationpriority: medium
ms.openlocfilehash: f61f2a0889cd5a2b2b95063e6009530951b49cbd
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/21/2019
ms.locfileid: "67321598"
---
# <a name="bluetooth-developer-faq"></a>藍牙開發人員常見問題集

本文包含 UWP 藍牙 API 常見問題的解答。

## <a name="what-apis-do-i-use-bluetooth-classic-rfcomm-or-bluetooth-low-energy-gatt"></a>我使用何種 API？ 藍牙傳統 (RFCOMM) 或藍牙低功耗 (GATT)？
這個一般主題在線上有各種討論，因此讓我們直接鎖定在與 Windows 有關的差異。 以下是一些一般指導方針︰

### <a name="bluetooth-le-windowsdevicesbluetoothgenericattributeprofile"></a>藍牙 LE (Windows.Devices.Bluetooth.GenericAttributeProfile)

當您與支援藍牙低功耗的裝置進行通訊時，請使用 GATT API。 如果您不常使用、低頻寬或需要低電源，則請使用藍牙低功耗。 包括這項功能的主要命名空間是 [Windows.Devices.Bluetooth.GenericAttributeProfile](https://docs.microsoft.com/en-us/uwp/api/Windows.Devices.Bluetooth.GenericAttributeProfile)。 

**何時不要使用 Bluetooth LE**
- 高頻寬、高頻率案例。 如果您需要一直與大量資料同步，請考慮使用藍牙傳統，或甚至使用 WiFi。 

### <a name="bluetooth-classic-windowsdevicesbluetoothrfcomm"></a>藍牙傳統 (Windows.Devices.Bluetooth.Rfcomm)

RFCOMM API 提供通訊端，讓開發人員執行雙向序列埠樣式通訊。 當您取得通訊端之後，在其中寫入和讀取的方法就相當標準。 這項作業的實作呈現於 [Rfcomm 聊天範例](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/BluetoothRfcommChat) (英文) 中。 

**何時不要使用藍芽 Rfcomm** 
- 通知。 藍牙 GATT 通訊協定具有這項作業的特定命令，將導致大幅電源消耗，且更快速的回應時間。 
- 檢查鄰近性或存在偵測。 最好使用[廣告 API](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.advertisement)，並透過藍牙 LE 連接。 


## <a name="why-does-my-bluetooth-le-device-stop-responding-after-a-disconnect"></a>為何我的藍牙 LE 裝置在中斷連線之後停止回應？

發生此問題的常見原因是遠端裝置遺失了配對資訊。 許多早期的藍牙裝置不需要驗證。 為了保護使用者，所有來自「設定」App 的配對程序都需要驗證，而有些裝置不知道如何處理驗證。 

從 Windows 10 版本 1511 開始，開發人員可以控制配對程序。 [裝置列舉與配對範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/DeviceEnumerationAndPairing) (英文) 有關聯新裝置之各層面的詳細資訊。

在這個範例中，我們會起始與未加密裝置的配對。 請注意，只有當遠端裝置不需加密或驗證就能運作的時候，此做法才會有用。

```csharp
// Get ceremony type and protection level selections
// You must select at least ConfirmOnly or the pairing attempt will fail
    DevicePairingKinds ceremonySelected = DevicePairingKinds.ConfirmOnly;

//  Workaround remote devices losing pairing information
    DevicePairingProtectionLevel protectionLevel = DevicePairingProtectionLevel.None

    DeviceInformationCustomPairing customPairing = deviceInfoDisp.DeviceInformation.Pairing.Custom;

// Declare an event handler - you don't need to do much in PairingRequestedHandler since the ceremony is "None"
    customPairing.PairingRequested += PairingRequestedHandler;
    DevicePairingResult result = await customPairing.PairAsync(ceremonySelected, protectionLevel);
```

## <a name="do-i-have-to-pair-bluetooth-devices-before-using-them"></a>我是否需要在使用藍牙裝置之前配對它們？

若為藍牙 RFCOMM (傳統) 裝置則不需要。 從 Windows 10 版本 1607 開始，您可以直接查詢附近的裝置並和它們連接。 已更新的 [RFCOMM 聊天範例](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/BluetoothRfcommChat) (英文) 說明這個功能。 

**(14393 和以下)** 藍牙低功耗 (GATT 用戶端) 不適用這項功能，因此您還是需要透過 [設定] 頁面，或使用 [Windows.Devices.Enumeration](https://docs.microsoft.com/uwp/api/windows.devices.enumeration) API 來配對，才能存取這類裝置。

**(15030 和以上)** 不再需要配對藍牙裝置。 使用新的 Async API，例如 GetGattServicesAsync and GetCharacteristicsAsync，以查詢遠端裝置的目前狀態。 如需詳細資料，請參閱[用戶端文件](gatt-client.md)。 

## <a name="when-should-i-pair-with-a-device-before-communicating-with-it"></a>應該何時先與裝置配對，再與之通訊？
一般而言，如果您需要具有與裝置的信任長期約定，則請與它配對 (指示使用者到達設定頁面或使用裝置列舉和配對 API)。 如果您只需要讀取資訊之裝置的公開 （溫度感應器或指標），然後連接，或接聽公告的而不進行任何努力與裝置配對。 這會防止長期執行的交互操作問題，因為許多裝置都不支援配對。 

## <a name="do-all-windows-devices-support-peripheral-role"></a>所有 Windows 裝置都支援周邊角色嗎？

否，這是硬體相關的功能，但提供方法 (BluetoothAdapter.IsPeripheralRoleSupported) 來查詢是否支援它。  目前支援的裝置包括 8992+ 上的 Windows Phone 以及 RPi3 (Windows IoT)。 

## <a name="can-i-access-these-apis-from-win32"></a>我可以從 Win32 存取這些 API 嗎？

是，所有這些 API 應該都會運作。 這個部落格詳述呼叫[傳統型應用程式中的 Windows API](https://blogs.windows.com/buildingapps/2017/01/25/calling-windows-10-apis-desktop-application/) 的方式。 
## <a name="is-this-functionality-supposed-to-exist-on--insert-sku-here-"></a>「-在這裡插入 SKU-」  上具有這項功能嗎？

**藍牙 LE**:是，所有的功能是 OneCore 中，而且應該搭配運作的 Bluetooth LE 堆疊的最新裝置上使用。 
> 警告：週邊角色硬體相依，而且某些 Windows Server 版本不支援藍芽。 

**藍芽 b R/EDR （傳統）** :有一些變化，但一般來說，有非常類似的設定檔層級的支援。 請參閱 [RFCOMM](send-or-receive-files-with-rfcomm.md)上的文件，以及[電腦](https://support.microsoft.com/en-us/help/10568/windows-10-supported-bluetooth-profiles)和[手機](https://support.microsoft.com/products/windows?os=windows-10-mobile)的這些受支援設定檔文件

