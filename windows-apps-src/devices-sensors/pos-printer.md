---
author: mukin
title: "POS 印表機"
description: "本文包含服務點印表機裝置系列的相關資訊"
ms.author: wdg-dev-content
ms.date: 02/21/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: d8340af651157bb6fae82785812f259c16d0a6c0
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="pos-printer"></a>POS 印表機

可讓應用程式開發人員使用 Epson ESC/POS 印表機控制語言列印至網路及藍牙連線[收據印表機](https://docs.microsoft.com/en-us/uwp/api/windows.devices.pointofservice.posprinter)。

## <a name="requirements"></a>需求
需要此命名空間的應用程式必須將 "pointOfService" [DeviceCapability](https://msdn.microsoft.com/library/4353c4fd-f038-4986-81ed-d2ec0c6235ef) 新增至應用程式套件資訊清單。

## <a name="device-support"></a>裝置支援
Windows 支援使用 Epson ESC/POS 印表機控制語言列印至網路及藍牙連線收據印表機的功能。 如需 ESC/POS 的詳細資訊，請參閱[具有格式化功能的 Epson ESC/POS](https://docs.microsoft.com/en-us/windows/uwp/devices-sensors/epson-esc-pos-with-formatting)。

雖然 API 公開的類別、列舉和介面會支援收據印表機、紙條印表機以及日誌印表機，但是驅動程式介面僅支援收據印表機。 目前若嘗試使用紙條印表機和日誌印表機，將會傳回未實作狀態。

目前僅支援下表列出的網路及藍牙裝置型號。 目前不支援 USB 連接的印表機。 請日後再回來查看未來要新增的其他支援。

### <a name="stationary-pos-printers-network-bluetooth"></a>固定式 POS 印表機 (網路、藍牙)
| 製造商 |    型號 |
|--------------|-----------|
| Epson |    TM-T88V、TM-T70、TM-T20、TM-U220 |

### <a name="mobile-pos-printers-bluetooth"></a>行動 POS 印表機 (藍牙)
| 製造商 |    型號 |
|--------------|-----------|
| Epson |    Mobilink P20 (TM-P20)、Mobilink P60 (TM-P60)、Mobilink P80 (TM-P80) |

## <a name="examples"></a>範例
請參閱 POS 印表機範例以取得範例實作。
+ [POS 印表機範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/PosPrinter)
