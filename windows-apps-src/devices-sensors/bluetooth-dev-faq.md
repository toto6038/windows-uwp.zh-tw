---
author: msatranjr
title: 藍牙開發人員常見問題集
description: 本文包含有關 UWP 藍牙 API 常見問題的解答。
ms.author: misatran
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.assetid: e7dee32d-3756-430d-a026-32c1ee288a85
ms.localizationpriority: medium
ms.openlocfilehash: 03ee8074a64b210d33498c8de135a76900d968f0
ms.sourcegitcommit: 4d88adfaf544a3dab05f4660e2f59bbe60311c00
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/12/2018
ms.locfileid: "6443255"
---
# <a name="bluetooth-developer-faq"></a>藍牙開發人員常見問題集

本文包含 UWP 藍牙 API 常見問題的解答。

## <a name="what-apis-do-i-use-bluetooth-classic-rfcomm-or-bluetooth-low-energy-gatt"></a>請勿使用何種 Api？ 藍芽傳統 (RFCOMM) 或藍牙低功耗 (GATT)？
有各種不同的討論線上本一般主題周圍的因此讓我們讓這個回應恰好保持在相對於 Windows 的差異。 以下是一些一般指導方針：

### <a name="bluetooth-le-windowsdevicesbluetoothgenericattributeprofile"></a>藍芽 LE (Windows.Devices.Bluetooth.GenericAttributeProfile)

當您與支援藍牙低功耗裝置通訊，請使用 GATT Api。 如果您正在使用案例是不須經常執行、 低頻寬，或是需要低電源，藍牙低功耗答案。 包含這項功能的主要命名空間是[Windows.Devices.Bluetooth.GenericAttributeProfile](https://docs.microsoft.com/en-us/uwp/api/Windows.Devices.Bluetooth.GenericAttributeProfile)。 

**不使用藍牙 LE 的時機**
- 高頻寬、 高頻率案例。 如果您需要持續保持同步處理大量的資料，請考慮使用藍牙傳統或甚至 WiFi。 

### <a name="bluetooth-classic-windowsdevicesbluetoothrfcomm"></a>藍芽傳統 (Windows.Devices.Bluetooth.Rfcomm)

RFCOMM Api 提供開發人員來執行 bi 方向序列連接埠樣式通訊的通訊端。 一旦您有通訊端，寫入及讀取從它的方法是相當標準。 [Rfcomm 交談範例](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/BluetoothRfcommChat)中，會顯示的實作。 

**不使用藍牙 Rfcomm 的時機** 
- 通知。 藍牙 GATT 通訊協定為此有特定的命令，並將導致大幅較少的電力繪製及較快的回應時間。 
- 檢查鄰近性或存在偵測。 使用[廣告 Api](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.advertisement) ，並透過藍牙 LE 連線更好。 


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

**（14393 或以下）** 此功能不適用於藍牙低功耗 （GATT 用戶端），因此您仍需要進行配對，無論是透過 [設定] 頁面，或使用[Windows.Devices.Enumeration](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.enumeration.aspx) Api 中的順序存取這些裝置。

**（15030 和以上版本）** 不再需要配對藍牙裝置。 使用新的非同步 Api 才能查詢遠端裝置的目前狀態，例如 GetGattServicesAsync 及 GetCharacteristicsAsync。 [用戶端文件](gatt-client.md)如需詳細資訊，請參閱。 

## <a name="when-should-i-pair-with-a-device-before-communicating-with-it"></a>何時應該我配對某個裝置與其通訊之前？
一般而言，如果您需要信任性的長期證券與裝置時，將它進行配對 （引導使用者設定 \] 頁面或使用裝置列舉與配對 Api）。 如果您只需要關閉讀取資訊的裝置，是可公開公開 （溫度感應器或指標），然後連線或接聽的廣告，而不需要讓任何心力來與裝置配對。 這會防止互通性問題，長期因為裝置的主機不支援配對。 

## <a name="do-all-windows-devices-support-peripheral-role"></a>所有 Windows 裝置是否都支援周邊角色？

否 – 這是硬體而定的功能，但提供方法 (BluetoothAdapter.IsPeripheralRoleSupported) 來查詢是否或不支援。  目前支援的裝置包含 Windows Phone 上 8992 + 和 RPi3 (Windows IoT)。 

## <a name="can-i-access-these-apis-from-win32"></a>可以存取這些 Api 從 Win32 嗎？

是的所有這些 Api 應該運作。 此部落格的詳細資料的方式來呼叫[Windows Api，從傳統型應用程式](https://blogs.windows.com/buildingapps/2017/01/25/calling-windows-10-apis-desktop-application/)。 
## <a name="is-this-functionality-supposed-to-exist-on--insert-sku-here-"></a>這項功能應該存在於 *-以下插入 SKU-*？

**藍牙 LE**： 是的 OneCore 中所有功能，而且應該會出現在最新的裝置，以正常運作的藍芽 LE 堆疊。 
> 要注意： 周邊角色視硬體而定，某些 Windows Server 版本不支援藍牙。 

**藍芽 BR/EDR （傳統）**： 一些變化存在，但整體而言，它們有非常類似的設定檔層級支援。 [RFCOMM](send-or-receive-files-with-rfcomm.md)與這些支援的設定檔文件上查看文件，針對[電腦](https://support.microsoft.com/en-us/help/10568/windows-10-supported-bluetooth-profiles)和[手機](https://support.microsoft.com/en-us/help/10569/windows-10-mobile-supported-bluetooth-profiles)

