---
author: mukin
title: "條碼掃描器"
description: "本文包含條碼掃描器服務點裝置系列的相關資訊"
ms.author: wdg-dev-content
ms.date: 02/21/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: 8d9ef9bc08fa666c2af1348450f7a5fb0a0c7b65
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="barcode-scanner"></a>條碼掃描器
可讓應用程式開發人員存取[條碼掃描器](https://docs.microsoft.com/en-us/uwp/api/windows.devices.pointofservice.barcodescanner)，根據硬體提供的支援來擷取各種條碼碼制 (例如 UPC 和 QR 代碼) 的解碼資料。 如需支援的碼制完整清單，請參閱 [BarcodeSymbologies](https://docs.microsoft.com/en-us/uwp/api/windows.devices.pointofservice.barcodesymbologies)類別。

## <a name="requirements"></a>需求
需要此命名空間的應用程式必須將 "pointOfService" [DeviceCapability](https://msdn.microsoft.com/library/4353c4fd-f038-4986-81ed-d2ec0c6235ef) 新增至應用程式套件資訊清單。

## <a name="device-support"></a>裝置支援

### <a name="usb"></a>USB

#### <a name="hidscanner"></a>HID.Scanner
Windows 包含以 USB.org 所定義 HID.Scanner (8C) 使用方式頁面為依據實作的條碼掃描器類別驅動程式。 這個類別驅動程式支援任何實作此標準的條碼掃描器，例如：製造商    型號 Honeywell    1900GSR-2、1200g-2 Intermec    SG20

請參閱條碼掃描器的手冊，或連絡製造商以判斷是否可將該裝置當做 USB.HID.Scanner 進行設定。

#### <a name="hidvendor-specific"></a>HID.Vendor 特定
Windows 支援廠商特定驅動程式實作以支援其他條碼掃描器。 如果不支援裝置與隨附的 USB.HID.Scanner 搭配使用，請向條碼掃描器製造商洽詢可用性。

### <a name="bluetooth"></a>藍牙
#### <a name="serial-port-protocol-spp--simple-serial-interface-ssi"></a>序列埠通訊協定 (SPP) – 簡單序列介面 (SSI)
Windows 支援以 SPP-SSI 為基礎的藍牙條碼掃描器。

| 製造商 |    型號 |
|--------------|-----------|
| 通訊端行動裝置 |    **CHS 7 系列：** <br/> 7 Ci 1D Imager 條碼掃描器 <br/> 7Di 1D Durable、Imager 條碼掃描器 <br/> 7Mi 1D 雷射條碼掃描器 <br/> 7Pi 1D 耐用型雷射條碼掃描器 <br/> **DuraScan 700 系列︰** <br/> D700 1D Imager 條碼掃描器 <br/> D730 1D 雷射條碼掃描器 <br/> **SocketScan 800 系列** <br/> S800 1D Imager 條碼掃描器 (舊稱 CHS 8Ci) <br/> S850 2D Imager 條碼掃描器 (舊稱 CHS 8Qi)

## <a name="examples"></a>範例
請參閱條碼掃描器範例以取得範例實作。
+    [條碼掃描器範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BarcodeScanner)
