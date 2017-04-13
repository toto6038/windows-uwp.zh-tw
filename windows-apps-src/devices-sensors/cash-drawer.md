---
author: mukin
title: "收銀機"
description: "本文包含收銀機服務點裝置系列的相關資訊"
ms.author: wdg-dev-content
ms.date: 02/21/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: 
ms.openlocfilehash: 376272356cf720ddd9519f0077e771a1016abb1e
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="cash-drawer"></a>收銀機

可讓應用程式開發人員與[收銀機](https://docs.microsoft.com/en-us/uwp/api/windows.devices.pointofservice.cashdrawer)進行互動。

## <a name="requirements"></a>需求
需要此命名空間的應用程式必須將 "pointOfService" [DeviceCapability](https://msdn.microsoft.com/library/4353c4fd-f038-4986-81ed-d2ec0c6235ef) 新增至應用程式套件資訊清單。

## <a name="device-support"></a>裝置支援
視收銀機裝置的功能而定，可以透過網路或藍牙直接與收銀機進行連線。 此外，沒有網路或藍牙功能的收銀機也可以透過受支援 POS 印表機的 DK 連接埠或 Star Micronics DK-AirCash 配件進行連線。 目前還無法支援透過 USB 或序列埠連接收銀機。

**注意︰**如需 DK-AirCash 的詳細資訊，請連絡 Star Micronics。

### <a name="networkbluetooth"></a>網路/藍牙
| 製造商 |    型號 |
|--------------|-----------|
| APG 收銀機 |    NetPRO、BluePRO |

## <a name="examples"></a>範例
請參閱收銀機範例以取得範例實作。
+    [收銀機範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CashDrawer)
