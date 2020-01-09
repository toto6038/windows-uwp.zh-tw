---
title: 藍牙開發人員常見問題集
description: 本文包含有關 UWP 藍牙 API 常見問題的解答。
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: e7dee32d-3756-430d-a026-32c1ee288a85
ms.localizationpriority: medium
ms.openlocfilehash: 584d327fc4882db6d3bf8d0cfd2a84b13023c6f4
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2020
ms.locfileid: "75684844"
---
# <a name="bluetooth-developer-faq"></a>藍牙開發人員常見問題集

本文包含 UWP 藍牙 API 常見問題的解答。

## <a name="what-apis-do-i-use-bluetooth-classic-rfcomm-or-bluetooth-low-energy-gatt"></a>我使用何種 API？ 藍牙傳統 (RFCOMM) 或藍牙低功耗 (GATT)？
這個一般主題在線上有各種討論，因此讓我們直接鎖定在與 Windows 有關的差異。 以下是一些一般指導方針︰

### <a name="bluetooth-le-windowsdevicesbluetoothgenericattributeprofile"></a>藍牙 LE (Windows.Devices.Bluetooth.GenericAttributeProfile)

當您與支援藍牙低功耗的裝置進行通訊時，請使用 GATT API。 如果您的使用案例不頻繁、低頻寬，或需要低電力，則藍牙低能量是解答。 包括這項功能的主要命名空間是 [Windows.Devices.Bluetooth.GenericAttributeProfile](https://docs.microsoft.com/uwp/api/Windows.Devices.Bluetooth.GenericAttributeProfile)。 

**不使用 Bluetooth LE 的時機**
- 高頻寬、高頻率案例。 如果您需要一直與大量資料同步，請考慮使用藍牙傳統，或甚至使用 WiFi。 

### <a name="bluetooth-classic-windowsdevicesbluetoothrfcomm"></a>藍牙傳統 (Windows.Devices.Bluetooth.Rfcomm)

RFCOMM Api 為開發人員提供通訊端來執行雙向序列埠樣式通訊。 取得通訊端之後，寫入和讀取的方法相當標準。 這項作業的實作呈現於 [Rfcomm 聊天範例](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/BluetoothRfcommChat) (英文) 中。 

**不使用藍牙 Rfcomm 時** 
- 通知。 藍牙 GATT 通訊協定具有這項作業的特定命令，將導致大幅電源消耗，且更快速的回應時間。 
- 檢查鄰近性或存在偵測。 最好使用[廣告 API](https://docs.microsoft.com/uwp/api/windows.devices.bluetooth.advertisement)，並透過藍牙 LE 連接。 


## <a name="why-does-my-bluetooth-le-device-stop-responding-after-a-disconnect"></a>為何我的藍牙 LE 裝置在中斷連線之後停止回應？

這種情況最常見的原因是遠端裝置已遺失配對資訊。 大量較舊的 Bluetooth 裝置不需要驗證。 若要保護使用者，從設定應用程式執行的所有配對交易都需要驗證，而且某些裝置不會考慮這一點。 

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

使用 Bluetooth RFCOMM （傳統）時，您不需要配對裝置，就可以使用它們。 從 Windows 10 版本 1607 開始，您可以直接查詢附近的裝置並和它們連接。 已更新的 [RFCOMM 聊天範例](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/BluetoothRfcommChat) (英文) 說明這個功能。 

**(14393 和以下)** 藍牙低功耗 (GATT 用戶端) 不適用這項功能，因此您還是需要透過 [設定] 頁面，或使用 [Windows.Devices.Enumeration](https://docs.microsoft.com/uwp/api/windows.devices.enumeration) API 來配對，才能存取這類裝置。

**(15030 和以上)** 不再需要配對藍牙裝置。 使用新的 Async API，例如 GetGattServicesAsync and GetCharacteristicsAsync，以查詢遠端裝置的目前狀態。 如需詳細資料，請參閱[用戶端文件](gatt-client.md)。 

## <a name="when-should-i-pair-with-a-device-before-communicating-with-it"></a>應該何時先與裝置配對，再與之通訊？
一般而言，如果您需要裝置的受信任、長期的債券，請將使用者導向至 [設定] 頁面，或使用裝置列舉和配對 Api 來配對。 如果您只需要從公開公開的裝置（溫度感應器或指標）讀取資訊，請連線或接聽廣告，而不需努力與裝置配對。 這會防止長時間執行的互通性問題，因為大量裝置不支援配對。 

## <a name="do-all-windows-devices-support-peripheral-role"></a>所有 Windows 裝置都支援周邊角色嗎？

不。 這是與硬體相關的功能，但提供了方法（BluetoothAdapter. IsPeripheralRoleSupported）來查詢是否受支援。  目前支援的裝置包括 8992+ 上的 Windows Phone 以及 RPi3 (Windows IoT)。 

## <a name="can-i-access-these-apis-from-win32"></a>我可以從 Win32 存取這些 API 嗎？

是，所有這些 API 應該都會運作。 這個部落格詳述呼叫[傳統型應用程式中的 Windows API](https://blogs.windows.com/buildingapps/2017/01/25/calling-windows-10-apis-desktop-application/) 的方式。 
## <a name="is-this-functionality-supposed-to-exist-on--insert-sku-here-"></a>「-在這裡插入 SKU-」 上具有這項功能嗎？

**藍牙 LE**︰ 是，所有功能都在 OneCore 中，而且應該適用於具有正常運作之藍牙 LE 堆疊的最新裝置。 
> 警告：週邊設備角色與硬體相關，而有些 Windows Server 版本不支援藍牙。 

**BLUETOOTH BR/EDR （傳統）** ：有一些變化存在，但大部分的設定檔層級支援非常相似。 請參閱[RFCOMM](send-or-receive-files-with-rfcomm.md)上的檔，以及這些適用于[PC](https://support.microsoft.com/help/10568/windows-10-supported-bluetooth-profiles)和[手機](https://support.microsoft.com/help/10569/windows-10-mobile-supported-bluetooth-profiles)的支援設定檔
