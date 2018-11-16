---
author: msatranjr
title: 藍牙低功耗
description: 本主題提供藍牙 LE UWP 應用程式中的快速概觀。
ms.author: misatran
ms.date: 03/15/2017
ms.topic: article
keywords: windows 10、 uwp、 藍牙、 藍牙 LE、 低功耗、 gatt、 間距、 中央、 周邊設備、 用戶端、 伺服器、 監看員、 發行者
ms.localizationpriority: medium
ms.openlocfilehash: 9e5bef16c76ee14c2abb7a5a41ab02d150a97333
ms.sourcegitcommit: e38b334edb82bf2b1474ba686990f4299b8f59c7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/15/2018
ms.locfileid: "6838557"
---
# <a name="bluetooth-low-energy"></a>藍牙低功耗
藍牙低功耗 (LE) 是定義來探索和省電裝置之間的通訊協定的規格。 探索裝置是透過一般的存取權設定檔 （間隔） 通訊協定完成。 在探索之後，裝置的通訊都是透過泛型屬性 (GATT) 通訊協定完成。 本主題提供藍牙 LE UWP 應用程式中的快速概觀。 藍芽 LE 有關的詳細資訊，請參閱[藍芽核心規格](https://www.bluetooth.com/specifications/bluetooth-core-specification)4.0 版，引進藍芽 LE。 

![藍芽 LE 角色](images/gatt-roles.png)

*在 Windows 10 版本 1703年中引進 GATT 和間距角色*

GATT 和間距通訊協定可以透過使用下列命名空間在您的 UWP app 中實作。
- [Windows.Devices.Bluetooth.GenericAttributeProfile](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.genericattributeprofile)
- [Windows.Devices.Bluetooth.Advertisement](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.genericattributeprofile)

## <a name="central-and-peripheral"></a>中央和周邊
中央與周邊裝置，都會呼叫探索的兩個主要角色。 一般情況下，Windows 會在中央模式中運作，並連線至各種不同的周邊裝置。 

## <a name="attributes"></a>屬性
您會看到 Windows 藍牙 Api 中的常見縮略字是泛型屬性 (GATT)。 GATT 設定檔會定義資料的結構與模式的作業所依據的兩個藍牙 LE 裝置通訊。 是主要建置組塊 GATT 的屬性。 主要的屬性類型是服務、 特性和描述元。 這些屬性之間的用戶端和伺服器，執行以不同的方式，因此它更有用，討論其相關的各節中的互動。 

![常見的設定檔中的一般屬性階層](images/gatt-service.png)

*GATT 伺服器 API 表單以表示心率服務*

## <a name="client-and-server"></a>用戶端和伺服器
已建立的連線後，會將包含的資料 （通常是小型 IoT 感應器或穿戴式裝置） 的裝置稱為伺服器。 執行函式會使用該資料的裝置稱為 「 用戶端。 例如，Windows 電腦 （用戶端） 讀取資料從心率監視器 （伺服器） 來追蹤，使用者正在最佳方式。 如需詳細資訊，請參閱[GATT 用戶端](gatt-client.md)和[GATT 伺服器](gatt-server.md)主題。

## <a name="watchers-and-publishers-beacons"></a>監控程式和發行者 （指標）
除了的中央與周邊設備的角色，還有觀察者和廣播者角色。 廣播者通常稱為 「 指標 」，它們不透過通訊 GATT 因為它們使用廣告封包中提供的通訊的有限的空間。 同樣地，觀察者不一定要連接到接收資料，它會掃描附近的廣告。 設定 Windows 觀察附近的廣告，請使用[BluetoothLEAdvertisementWatcher](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.advertisement.bluetoothleadvertisementwatcher)類別。 廣播指標承載，才能使用[BluetoothLEAdvertisementPublisher](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.advertisement.bluetoothleadvertisementpublisher)類別。 如需詳細資訊，請參閱 「[廣告](ble-beacon.md)」 主題。

## <a name="see-also"></a>請參閱
- [Windows.Devices.Bluetooth.GenericAttributeProfile](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.genericattributeprofile)
- [Windows.Devices.Bluetooth.Advertisement](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.genericattributeprofile)
- [藍芽核心規格](https://www.bluetooth.com/specifications/bluetooth-core-specification)