---
author: msatranjr
title: "藍牙開發人員常見問題集"
description: "本文包含有關 UWP 藍牙 API 常見問題的解答。"
translationtype: Human Translation
ms.sourcegitcommit: e4c95448262c6c62956fcb50581c98d8c34d6dc0
ms.openlocfilehash: 2afc1250aa9d7a6cf6c9c8cb45dd2379b9d36984

---
# 藍牙開發人員常見問題集

本文包含 UWP 藍牙 API 常見問題的解答。

## 為何我的藍牙 LE 裝置在中斷連線之後停止回應？

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

## 我是否需要在使用藍牙裝置之前配對它們？

若為藍牙 RFCOMM (傳統) 裝置則不需要。 從 Windows 10 版本 1607 開始，您可以直接查詢附近的裝置並和它們連接。 已更新的 [RFCOMM 聊天範例](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/BluetoothRfcommChat) (英文) 說明這個功能。 

藍牙低功耗 (GATT 用戶端) 不適用這項功能，因此您還是需要透過「設定」頁面，或使用 [Windows.Devices.Enumeration](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.enumeration.aspx) API 來配對，才能存取這類裝置。




<!--HONumber=Aug16_HO3-->


