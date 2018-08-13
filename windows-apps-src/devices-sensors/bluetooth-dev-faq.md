---
author: msatranjr
title: 藍牙開發人員常見問題集
description: 本文包含有關 UWP 藍牙 API 常見問題的解答。
ms.author: misatran
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.assetid: e7dee32d-3756-430d-a026-32c1ee288a85
ms.localizationpriority: medium
ms.openlocfilehash: c0af6a19e17a62ed82c32e68ea1732e1f51d4641
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/13/2018
ms.locfileid: "301924"
---
# <a name="bluetooth-developer-faq"></a>藍牙開發人員常見問題集

本文包含 UWP 藍牙 API 常見問題的解答。

## <a name="what-apis-do-i-use-bluetooth-classic-rfcomm-or-bluetooth-low-energy-gatt"></a>使用何種 api （英文）？ Bluetooth 傳統 (RFCOMM) 或 Bluetooth 低能源 (GATT)？
有各種討論線上周圍本一般主題的讓我們在有關 Windows 差異跡象保留此回覆。 以下是一些通用準則：

### <a name="bluetooth-le-windowsdevicesbluetoothgenericattributeprofile"></a>Bluetooth LE (Windows.Devices.Bluetooth.GenericAttributeProfile)

您要與支援 Bluetooth 低能源裝置通訊時使用 GATT api （英文）。 如果您正在使用案例為非經常性、 低頻寬或需要低電源、 Bluetooth 低能源是答案。 包含這項功能的主要命名空間是[Windows.Devices.Bluetooth.GenericAttributeProfile](https://docs.microsoft.com/en-us/uwp/api/Windows.Devices.Bluetooth.GenericAttributeProfile)。 

**不使用時機 Bluetooth LE**
- 高頻寬、 高頻率案例。 如果您需要正比保持同步處理具有大量資料，請考慮使用 Bluetooth 傳統或也許偶數 WiFi。 

### <a name="bluetooth-classic-windowsdevicesbluetoothrfcomm"></a>Bluetooth 傳統 (Windows.Devices.Bluetooth.Rfcomm)

RFCOMM Api 授與開發人員可執行 bi 方向序列連接埠樣式通訊端。 一旦您已經有通訊端通訊協定、 寫入與讀取它的方法是許多標準。 [Rfcomm Chat 範例](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/BluetoothRfcommChat)中呈現的實作。 

**不使用時機 Bluetooth Rfcomm** 
- 通知。 Bluetooth GATT 通訊協定具有特定命令的這個及會導致大幅較少 power 繪製和更快的回應時間。 
- 檢查鄰近或目前狀態偵測。 使用[通告 Api](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.advertisement)和透過 Bluetooth LE 連線。 


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

**（14393 和下）** 這項功能不適用於 Bluetooth 低能源 （GATT 用戶端），因此您仍必須對透過 [設定] 頁面上或使用[Windows.Devices.Enumeration](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.enumeration.aspx) Api 的順序存取這些裝置。

**（15030 及以上）** 不再需要配對 Bluetooth 裝置。 使用新的非同步處理 Api，像是 GetGattServicesAsync 及 GetCharacteristicsAsync 以查詢遠端裝置的目前狀態。 請參閱[用戶端文件](gatt-client.md)如需詳細資訊。 

## <a name="when-should-i-pair-with-a-device-before-communicating-with-it"></a>何時應該我配對與裝置之前與其通訊？
一般而言，如果您需要信任、 長期證券與裝置，配對與其 （導向的使用者設定] 頁面上或使用裝置列舉與配對 Api）。 如果您只需要讀取資訊關閉裝置的公開公開 （溫度接收器或信標），然後連線或聆聽的廣告而不需要進行任何位置，與該裝置配對。 如此可防止互通性問題長期因為配對不支援的裝置的主機。 

## <a name="do-all-windows-devices-support-peripheral-role"></a>Windows 的所有裝置是否都支援周邊角色？

否 – 此為硬體相依的功能，但方法提供 (BluetoothAdapter.IsPeripheralRoleSupported) 來查詢是否或不支援。  目前支援的裝置包括 8992 + 上的 Windows Phone 和 RPi3 (Windows IoT)。 

## <a name="can-i-access-these-apis-from-win32"></a>可以存取這些 Api 從 Win32 吗？

是，所有這些 Api 應該搭配使用。 此部落格詳細說明以呼叫[Windows api （英文） 從桌面應用程式](https://blogs.windows.com/buildingapps/2017/01/25/calling-windows-10-apis-desktop-application/)的方式。 
## <a name="is-this-functionality-supposed-to-exist-on--insert-sku-here-"></a>這項功能被假設才能在 *-此處插入 SKU-* 吗？

**Bluetooth LE**： 是，所有功能中 OneCore 且應可正常運作的 Bluetooth LE 堆疊具有最新的裝置上。 
> 聲明： 周邊角色是相依的硬體和部分的 Windows Server 版本不支援 Bluetooth。 

**Bluetooth 巴西/EDR （傳統）**： 一些變化存在但總結來說有非常類似的設定檔層級的支援。 請參閱文件上[RFCOMM](send-or-receive-files-with-rfcomm.md)和這些支援的設定檔文件的[電腦](https://support.microsoft.com/en-us/help/10568/windows-10-supported-bluetooth-profiles)和[電話](https://support.microsoft.com/en-us/help/10569/windows-10-mobile-supported-bluetooth-profiles)

