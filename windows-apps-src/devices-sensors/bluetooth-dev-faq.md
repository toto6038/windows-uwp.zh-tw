---
title: 藍牙開發人員常見問題集
description: 本文包含有關 UWP 藍牙 API 常見問題的解答。
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: e7dee32d-3756-430d-a026-32c1ee288a85
ms.localizationpriority: medium
ms.openlocfilehash: 7d41e49f599e1fe5e835443f7c8cb732e625491e
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89168592"
---
# <a name="bluetooth-developer-faq"></a>藍牙開發人員常見問題集

本文包含 UWP 藍牙 API 常見問題的解答。

## <a name="what-apis-do-i-use-bluetooth-classic-rfcomm-or-bluetooth-low-energy-gatt"></a>我使用何種 API？ 藍牙傳統 (RFCOMM) 或藍牙低功耗 (GATT)？
這個一般主題在線上有各種討論，因此讓我們直接鎖定在與 Windows 有關的差異。 以下有幾個一般方針︰

### <a name="bluetooth-le-windowsdevicesbluetoothgenericattributeprofile"></a>藍牙 LE (Windows.Devices.Bluetooth.GenericAttributeProfile)

當您與支援藍牙低功耗的裝置進行通訊時，請使用 GATT API。 如果您的使用案例不頻繁、低頻寬或需要低功率，則會有藍牙低能源的答案。 包括這項功能的主要命名空間是 [Windows.Devices.Bluetooth.GenericAttributeProfile](/uwp/api/Windows.Devices.Bluetooth.GenericAttributeProfile)。 

**不使用藍牙 LE 時**
- 高頻寬、高頻率案例。 如果您需要一直與大量資料同步，請考慮使用藍牙傳統，或甚至使用 WiFi。 

### <a name="bluetooth-classic-windowsdevicesbluetoothrfcomm"></a>藍牙傳統 (Windows.Devices.Bluetooth.Rfcomm)

RFCOMM Api 可讓開發人員使用通訊端來執行雙向序列埠樣式的通訊。 當您取得通訊端之後，撰寫和讀取的方法相當標準。 這項作業的實作呈現於 [Rfcomm 聊天範例](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/BluetoothRfcommChat) (英文) 中。 

**不使用藍牙 Rfcomm 時** 
- 通知。 藍牙 GATT 通訊協定具有這項作業的特定命令，將導致大幅電源消耗，且更快速的回應時間。 
- 檢查鄰近性或存在偵測。 最好使用[廣告 API](/uwp/api/windows.devices.bluetooth.advertisement)，並透過藍牙 LE 連接。 


## <a name="why-does-my-bluetooth-le-device-stop-responding-after-a-disconnect"></a>為何我的藍牙 LE 裝置在中斷連線之後停止回應？

發生這種情況最常見的原因是遠端裝置已遺失配對資訊。 大量較舊的藍牙裝置不需要驗證。 為了保護使用者，從 [設定] 應用程式執行的所有配對交易都需要驗證，而某些裝置並不會以這種方式設計。 

從 Windows 10 版本1511開始，開發人員可以控制配對交握。 [裝置列舉與配對範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/DeviceEnumerationAndPairing) (英文) 有關聯新裝置之各層面的詳細資訊。

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

如果您使用 Bluetooth RFCOMM (傳統) ，就不需要配對裝置。 從 Windows 10 版本 1607 開始，您可以直接查詢附近的裝置並和它們連接。 已更新的 [RFCOMM 聊天範例](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/BluetoothRfcommChat) (英文) 說明這個功能。 

** (14393 和以下) ** 這項功能不適用於藍牙低功耗 (GATT 用戶端) ，因此您仍然必須透過 [設定] 頁面或使用 [Windows]. [列舉](/uwp/api/windows.devices.enumeration) api，才能存取這些裝置。

**(15030 和以上)** 不再需要配對藍牙裝置。 使用新的 Async API，例如 GetGattServicesAsync and GetCharacteristicsAsync，以查詢遠端裝置的目前狀態。 如需詳細資料，請參閱[用戶端文件](gatt-client.md)。 

## <a name="when-should-i-pair-with-a-device-before-communicating-with-it"></a>應該何時先與裝置配對，再與之通訊？
一般而言，如果您需要與裝置進行受信任的長期綁定，請將使用者導向至 [設定] 頁面，或使用裝置列舉和配對 Api 來與裝置配對。 如果您只需要從裝置公開公開 (溫度感應器或指標) 的資訊，請連接或接聽廣告，而不需要投入任何時間與裝置配對。 這可防止長時間執行互通性問題，因為大量裝置不支援配對。 

## <a name="do-all-windows-devices-support-peripheral-role"></a>所有 Windows 裝置都支援周邊角色嗎？

否。 這是硬體相依的功能，但提供了 BluetoothAdapter IsPeripheralRoleSupported 的方法，可查詢是否支援。  目前支援的裝置包括 8992+ 上的 Windows Phone 以及 RPi3 (Windows IoT)。 

## <a name="can-i-access-these-apis-from-win32"></a>我可以從 Win32 存取這些 API 嗎？

是，所有這些 API 應該都會運作。 這個部落格詳述呼叫[傳統型應用程式中的 Windows API](https://blogs.windows.com/buildingapps/2017/01/25/calling-windows-10-apis-desktop-application/) 的方式。 
## <a name="is-this-functionality-supposed-to-exist-on--insert-sku-here-"></a>「-在這裡插入 SKU-」** 上具有這項功能嗎？

**藍牙 LE**︰ 是，所有功能都在 OneCore 中，而且應該適用於具有正常運作之藍牙 LE 堆疊的最新裝置。 
> 警告：周邊角色與硬體相依，有些 Windows Server 版本不支援藍牙。 

**藍牙 BR/EDR (傳統) **：某些變化存在，但大多都有非常類似的設定檔層級支援。 請參閱[RFCOMM](send-or-receive-files-with-rfcomm.md)上的檔以及這些適用于[PC](https://support.microsoft.com/help/10568/windows-10-supported-bluetooth-profiles)和[手機](https://support.microsoft.com/help/10569/windows-10-mobile-supported-bluetooth-profiles)的支援設定檔檔