---
author: msatranjr
title: 藍牙低功耗
description: 本主題提供 Bluetooth LE UWP 應用程式中的快速概觀。
ms.author: misatran
ms.date: 03/15/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10、 uwp、 bluetooth、 bluetooth LE、 低能源、 gatt、 缺口、 管理中心、 周邊、 用戶端、 伺服器、 監看員、 publisher
ms.localizationpriority: medium
ms.openlocfilehash: 0d6cc1fb5a0b3cb96748b99c490b23a9e1df128f
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/13/2018
ms.locfileid: "304640"
---
# <a name="bluetooth-low-energy"></a>藍牙低功耗
Bluetooth 低能源 (LE) 會定義為探索及 power 有效率裝置之間的通訊的通訊協定的規格。 探索裝置可以透過將泛用存取設定檔 （缺口） 通訊協定。 裝置的通訊之後探索，您可以透過一般屬性 (GATT) 通訊協定。 本主題提供 Bluetooth LE UWP 應用程式中的快速概觀。 若需 Bluetooth LE 的詳細資訊，請參閱[Bluetooth 核心規格](https://www.bluetooth.com/specifications/bluetooth-core-specification)4.0 版、 推出 Bluetooth LE。 

![Bluetooth LE 角色](images/gatt-roles.png)

*Windows 10 版 1703年中所引進 GATT 和缺口角色*

UWP 應用程式可以實作 GATT 與缺口通訊協定使用的下列命名空間。
- [Windows.Devices.Bluetooth.GenericAttributeProfile](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.genericattributeprofile)
- [Windows.Devices.Bluetooth.Advertisement](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.genericattributeprofile)

## <a name="central-and-peripheral"></a>在中央與周邊
管理中心和周邊稱為探索的兩個主要角色。 一般而言，Windows 都會在中央模式，並連接至各種周邊裝置。 

## <a name="attributes"></a>屬性
您會看到中 Windows Bluetooth Api 通用縮寫為一般屬性 (GATT)。 GATT 設定檔會定義資料的結構和作業的兩個 Bluetooth LE 裝置進行通訊模式。 屬性是 GATT 主要建置區塊。 屬性的主要類型為服務、 特性及描述元。 這些屬性用戶端與伺服器之間執行以不同方式，讓更多有用討論及其相關的各節中的互動。 

![公用設定檔中的一般屬性合作夥伴階層](images/gatt-service.png)

*心形率服務以 GATT Server API 表單*

## <a name="client-and-server"></a>用戶端和伺服器
建立連線之後，包含的資料 （通常是小型的 IoT 接收器或可穿戴式） 的裝置稱為伺服器。 若要執行函數會使用該資料的裝置稱為用戶端。 例如，Windows PC （用戶端） 讀取資料從心形率監視器 （伺服器） 來追蹤使用者以最佳方式使用。 如需詳細資訊，請參閱[GATT 用戶端](gatt-client.md)和[GATT 伺服器](gatt-server.md)主題。

## <a name="watchers-and-publishers-beacons"></a>觀看者和發行者 （指標）
除了 [管理中心] 與 [周邊的角色，有觀察者 」 與 「 廣播者角色。 廣播者通常稱為 「 指標 」，它們不透過通訊 GATT 因為時使用通訊通告封包所提供的有限的空間。 同樣地，觀察者沒有建立接收資料連線，其所掃描的附近廣告。 若要設定 Windows 到觀察附近廣告，使用[BluetoothLEAdvertisementWatcher](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.advertisement.bluetoothleadvertisementwatcher)類別。 若要廣播信標負載、 使用[BluetoothLEAdvertisementPublisher](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.advertisement.bluetoothleadvertisementpublisher)類別。 如需詳細資訊，請參閱[通告](ble-beacon.md)主題。

## <a name="see-also"></a>請參閱
- [Windows.Devices.Bluetooth.GenericAttributeProfile](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.genericattributeprofile)
- [Windows.Devices.Bluetooth.Advertisement](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.genericattributeprofile)
- [Bluetooth 核心規格](https://www.bluetooth.com/specifications/bluetooth-core-specification)