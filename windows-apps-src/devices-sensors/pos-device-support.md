---
author: mukin
title: "POS 裝置支援"
description: "本文包含每個 POS 裝置系列之裝置支援的相關資訊"
ms.author: mukin
ms.date: 05/17/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: 58018385082f7f7e49edba0351d919bc840ade05
ms.sourcegitcommit: 53930c9871461f6106f785ae4fabb2296eb359f1
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/23/2017
---
# <a name="pos-device-support"></a>POS 裝置支援

## <a name="barcode-scanner"></a>條碼掃描器
| 連線能力 | 支援 |
| -------------|-------------|
| USB          | <p>Windows 包含連接 USB 之條碼掃描器的預設類別驅動程式，而此預設類別驅動程式以[USB.org](http://www.usb.org/developers/hidpage/)所定義的 HID POS 掃描器使用表格 (8c) 規格為基礎。 請查看下列表格，以了解已知相容裝置的清單。  請參閱條碼掃描器的手冊，或連絡製造商以判斷是否可以使用 USB.HID.POS 掃描器模式進行設定。 </p><p>Windows 也支援實作供應商特定驅動程式，以支援不支援 USB.HID.POS 掃描器標準的其他條碼掃描器。 請與條碼掃描器製造商連絡，以了解供應商特定驅動程式可用性。</p>|
| 藍牙    | <p>Windows 支援以 SPP-SSI 為基礎的藍牙條碼掃描器。 請查看下列表格，以了解已知相容裝置的清單。</p> |

### <a name="compatible-hardware"></a>相容的硬體
| 類別 | 連線能力 | 製造商/機型 |
|--------------|-----------|-----------|
| **1D 手持掃描器** | **USB** |Honeywell Voyager 1200g<br/>Honeywell Voyager 1202g<br/>Honeywell Voyager 1202-bf<br/>Honeywell Voyager 145Xg (可升級)|
| **1D 手持掃描器** | **藍牙** |通訊端行動裝置 CHS 7Ci<br/> 通訊端行動裝置 CHS 7Di<br/> 通訊端行動裝置 CHS 7Mi<br/> 通訊端行動裝置 CHS 7Pi<br/>通訊行動裝置 DuraScan D700<br/> 通訊行動裝置 DuraScan D730<br/>通訊端行動裝置 SocketScan S800 (先前稱為 CHS 8Ci) <br/>|
|**2D 手持掃描器** | **USB** |Code Reader™ 950<br/>Code Reader™ 1021<br/>Code Reader™ 1421<br/> Honeywell Granit 198Xi<br/>Honeywell Granit 191Xi<br/>Honeywell Xenon 1900g<br/>Honeywell Xenon 1902g<br/>Honeywell Xenon 1902g-bf<br/>Honeywell Xenon 1900h<br/>Honeywell Xenon 1902h<br/>Honeywell Voyager 145Xg (可升級)<br/>Honeywell Voyager 1602g<br/>Intermec SG20|
|**2D 手持掃描器** | **藍牙** |通訊端行動裝置 SocketScan S850 (先前稱為 CHS 8Qi)|
| **簡報掃描器** | **USB** |Code Reader™ 5000<br/>Honeywell Genesis 7580g<br/>Honeywell Orbit 7190g|
| **平台式掃描器** | **USB** |Honeywell Stratos 2700|
| **掃描引擎** | **USB** | Honeywell N5680<br/>Honeywell N3680|
| **Windows 行動裝置版裝置**| **內建** |Bluebird EF400<br/>Bluebird EF500<br/>Bluebird EF500R<br/>Honeywell CT50<br/>Honeywell D75e<br/>Janam XT2<br/>Panasonic FZ-E1<br/>Panasonic FZ-F1<br/>PointMobile PM80<br/>Zebra TC700j|
| **Windows 行動裝置版裝置**| **自訂** | HP Elite X3 (含 Barcode Scanner Jacket) |

## <a name="cash-drawer"></a>收銀機
| 連線能力 | 支援 |
| -------------|-------------|
| 網路/藍牙 | <p> 視收銀機裝置的功能而定，可以透過網路或藍牙直接與收銀機進行連線。 </p>|
| DK 連接埠 | <p> 沒有網路或藍牙功能的收銀機也可以透過受支援 POS 印表機的 DK 連接埠或 Star Micronics DK-AirCash 配件進行連線。 </p>
| OPOS    | <p> 支援任何支援 OPOS 驅動程式的收銀機裝置，以及 (或) 提供 OPOS 服務物件。 根據特定裝置製造商的安裝指示，安裝 OPOS 驅動程式。 這可根據製造商規格來啟用 USB 和其他連線能力。 </p> |

**注意︰**如需 DK-AirCash 的詳細資訊，請連絡 Star Micronics。

### <a name="networkbluetooth"></a>網路/藍牙
| 製造商 |    型號 |
|--------------|-----------|
| APG 收銀機 |    NetPRO、BluePRO |

## <a name="line-display"></a>行顯示
支援任何支援 OPOS 驅動程式的行顯示裝置，以及 (或) 提供 OPOS 服務物件。 根據特定裝置製造商的安裝指示，安裝 OPOS 驅動程式。

## <a name="magnetic-stripe-reader"></a>磁條讀取器

Windows 包含連接 USB 之磁條讀取器的預設類別驅動程式，而此預設類別驅動程式以[USB.org](http://www.usb.org/developers/hidpage/)所定義的 HID POS 掃描器使用表格 (8c) 規格為基礎。

### <a name="vendor-specific-support"></a>廠商特定支援
Windows 根據廠商識別碼和產品識別碼 (VID/PID)，為 Magtek 和 IDTech 的下列磁條讀取器提供支援。

| 製造商 |     型號 |    零件編號 |
|--------------|-----------|--------------|
| IDTech | SecureMag (VID:0ACD PID:2010) | IDRE-3x5xxxx |
| |    MiniMag (VID:0ACD PID:0500) |    IDMB-3x5xxxx |
| Magtek | MagneSafe (VID:0801 PID:0011) |    210730xx |
| |    Dynamag (VID:0801 PID:0002) |    210401xx |

### <a name="custom-vendor-specific"></a>自訂的廠商特定
Windows 支援其他廠商特定驅動程式的實作，以便支援其他磁條讀取器。 請向磁條讀取器製造商洽詢可用性。

## <a name="pos-printer"></a>POS 印表機
Windows 支援使用 Epson ESC/POS 印表機控制語言列印至網路及藍牙連線收據印表機的功能。 如需 ESC/POS 的詳細資訊，請參閱[具有格式化功能的 Epson ESC/POS](https://docs.microsoft.com/en-us/windows/uwp/devices-sensors/epson-esc-pos-with-formatting)。

雖然 API 公開的類別、列舉和介面會支援收據印表機、紙條印表機和日誌印表機，但是驅動程式介面僅支援收據印表機。 目前若嘗試使用紙條印表機和日誌印表機，將會傳回未實作狀態。

| 連線能力 | 支援 |
| -------------|-------------|
| 網路/藍牙 | <p> 根據 POS 印表機裝置的功能，可以透過網路或藍牙直接與 POS 印表機進行連線。 </p>|
| OPOS    | <p> 支援任何支援 OPOS 驅動程式的 POS 印表機裝置，和/或提供 OPOS 服務物件。 根據特定裝置製造商的安裝指示，安裝 OPOS 驅動程式。 這可根據製造商規格來啟用 USB 和其他連線能力。 </p> |

### <a name="stationary-pos-printers-networkbluetooth"></a>固定式 POS 印表機 (網路/藍牙)
| 製造商 |    型號 |
|--------------|-----------|
| Epson |    TM-T88V、TM-T70、TM-T20、TM-U220 |

### <a name="mobile-pos-printers-bluetooth"></a>行動 POS 印表機 (藍牙)
| 製造商 |    型號 |
|--------------|-----------|
| Epson |    Mobilink P20 (TM-P20)、Mobilink P60 (TM-P60)、Mobilink P80 (TM-P80) |