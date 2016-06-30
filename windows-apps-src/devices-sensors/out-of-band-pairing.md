---
author: IvorB
ms.assetid: E9ADC88F-BD4F-4721-8893-0E19EA94C8BA
title: "頻外配對"
description: "頻外配對可讓 app 連線到服務點周邊設備，而不需要探索。"
translationtype: Human Translation
ms.sourcegitcommit: 0bf96b70a915d659c754816f4c115f3b3f0a5660
ms.openlocfilehash: d8d37b779a0f9a4bec36d73fcd2d35272c587b11

---
# 頻外配對

頻外配對可讓 app 連線到服務點周邊設備，而不需要探索。 應用程式必須使用 [**Windows.Devices.PointOfService**](https://msdn.microsoft.com/library/windows/apps/windows.devices.pointofservice.aspx) 命名空間，並將特別格式的字串 (頻外 Blob) 傳遞至所需周邊設備的適當 **FromIdAsync** 方法。 執行 **FromIdAsync** 時，主機裝置會在作業回到呼叫端前配對並連線到周邊設備。

## 頻外 Blob 格式

```json
    "connectionKind":"Network",
    "physicalAddress":"AA:BB:CC:DD:EE:FF",
    "connectionString":"192.168.1.1:9001",
    "peripheralKinds":"{C7BC9B22-21F0-4F0D-9BB6-66C229B8CD33}",
    "providerId":"{02FFF12E-7291-4A5D-ADFA-DA8FB7769CD2}",
    "providerName":"PrinterProtocolProvider.dll"
```

**connectionKind** - 連線類型。 有效值為「網路」和「藍牙」。

**physicalAddress** - 週邊設備的 MAC 位址。 例如，在網路印表機的案例中，這會是印表機測試頁所提供的 MAC 位址 (採用 AA:BB:CC:DD:EE:FF 格式)。

**connectionString** - 週邊設備的連接字串。 例如，在網路印表機的案例中，這會是印表機測試頁所提供的 IP 位址 (採用 192.168.1.1:9001 格式)。 所有藍牙周邊設備都會省略這個欄位。

**peripheralKinds** - 裝置類型的 GUID。 有效值如下：

| 裝置類型 | GUID |
| ---- | ---- |
| *POS 印表機* | C7BC9B22-21F0-4F0D-9BB6-66C229B8CD33 |
| *條碼掃描器* | C243FFBD-3AFC-45E9-B3D3-2BA18BC7EBC5 |
| *收銀機* | 772E18F2-8925-4229-A5AC-6453CB482FDA |


**providerId** - 通訊協定提供者類別的 GUID。 有效值如下：

| 通訊協定提供者類別 | GUID |
| ---- | ---- |
| *一般 ESC/POS 網路印表機* | 02FFF12E-7291-4A5D-ADFA-DA8FB7769CD2 |
| *一般 ESC/POS BT 印表機* | CCD5B810-95B9-4320-BA7E-78C223CAF418 |
| *Epson BT 印表機* | 94917594-544F-4AF8-B53B-EC6D9F8A4464 |
| *Epson 網路印表機* | 9F0F8BE3-4E59-4520-BFBA-AF77614A31CE |
| *Star 網路印表機* | 1E3A32C2-F411-4B8C-AC91-CC2C5FD21996 |
| *通訊端 BT 掃描器* | 6E7C8178-A006-405E-85C3-084244885AD2 |
| *APG 網路收銀機* | E619E2FE-9489-4C74-BF57-70AED670B9B0 |
| *APG BT 收銀機* | 332E6550-2E01-42EB-9401-C6A112D80185 |


**providerName** - 提供者 DLL 的名稱。 預設提供者如下︰

| 提供者 | DLL 名稱 |
| ---- | ---- |
| 印表機 | PrinterProtocolProvider.dll |
| 收銀機 | CashDrawerProtocolProvider.dll |
| 掃描器 | BarcodeScannerProtocolProvider.dll |

## 用法範例：網路印表機

```csharp
String oobBlobNetworkPrinter =
  "{\"connectionKind\":\"Network\"," +
  "\"physicalAddress\":\"AA:BB:CC:DD:EE:FF\"," +
  "\"connectionString\":\"192.168.1.1:9001\"," +
  "\"peripheralKinds\":\"{C7BC9B22-21F0-4F0D-9BB6-66C229B8CD33}\"," +
  "\"providerId\":\"{02FFF12E-7291-4A5D-ADFA-DA8FB7769CD2}\"," +
  "\"providerName\":\"PrinterProtocolProvider.dll\"}";

printer = await PosPrinter.FromIdAsync(oobBlobNetworkPrinter);
```

## 用法範例：藍牙印表機

```csharp
string oobBlobBTPrinter =
    "{\"connectionKind\":\"Bluetooth\"," +
    "\"physicalAddress\":\"AA:BB:CC:DD:EE:FF\"," +
    "\"peripheralKinds\":\"{C7BC9B22-21F0-4F0D-9BB6-66C229B8CD33}\"," +
    "\"providerId\":\"{CCD5B810-95B9-4320-BA7E-78C223CAF418}\"," +
    "\"providerName\":\"PrinterProtocolProvider.dll\"}";

printer = await PosPrinter.FromIdAsync(oobBlobBTPrinter);

```



<!--HONumber=Jun16_HO4-->


