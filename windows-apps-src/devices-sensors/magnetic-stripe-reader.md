---
author: mukin
title: "磁條讀取器"
description: "本文包含磁條讀取器服務點裝置系列的相關資訊"
ms.author: wdg-dev-content
ms.date: 02/21/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: a11fe7a63c0444ac986e7bfe0d50472249e5196e
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="magnetic-stripe-reader"></a>磁條讀取器

可讓應用程式開發人員存取[磁條讀取器](https://docs.microsoft.com/en-us/uwp/api/windows.devices.pointofservice.magneticstripereader)，從支援磁條的卡片 (例如信用卡/轉帳卡、忠誠卡、門禁卡等) 擷取資料。

## <a name="requirements"></a>需求
需要此命名空間的應用程式必須將 "pointOfService" [DeviceCapability](https://msdn.microsoft.com/library/4353c4fd-f038-4986-81ed-d2ec0c6235ef) 新增至應用程式套件資訊清單。

## <a name="device-support"></a>裝置支援
### <a name="usb"></a>USB
### <a name="supported-vendor-specific"></a>支援的廠商特定
Windows 根據廠商識別碼和產品識別碼 (VID/PID)，為 Magtek 和 IDTech 的下列磁條讀取器提供支援。

| 製造商 |     型號 |    零件編號 |
|--------------|-----------|--------------|
| IDTech | SecureMag (VID:0ACD PID:2010) | IDRE-3x5xxxx |
| |    MiniMag (VID:0ACD PID:0500) |    IDMB-3x5xxxx |
| Magtek | MagneSafe (VID:0801 PID:0011) |    210730xx |
| |    Dynamag (VID:0801 PID:0002) |    210401xx |

### <a name="custom-vendor-specific"></a>自訂的廠商特定
Windows 支援其他廠商特定驅動程式的實作，以便支援其他磁條讀取器。 請向磁條讀取器製造商洽詢可用性。

## <a name="examples"></a>範例
請參閱磁條讀取器範例以取得範例實作。
+    [磁條讀取器範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MagneticStripeReader)
