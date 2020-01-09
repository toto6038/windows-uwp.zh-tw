---
title: 藍牙低功耗
description: 本主題提供 UWP app 中藍牙 LE 的快速概觀。
ms.date: 03/19/2017
ms.topic: article
keywords: windows 10, uwp, 藍牙, 藍牙 LE, 低功耗, gatt, gap, 中央, 周邊, 用戶端, 伺服器, 監看員, 發行者
ms.localizationpriority: medium
ms.openlocfilehash: 4859dfb540b252f379a0ec3cbfe52985c0776fd9
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2020
ms.locfileid: "75684850"
---
# <a name="bluetooth-low-energy"></a>藍牙低功耗
藍牙低功耗 (LE) 是一種規格，定義通訊協定來進行探索以及在省電裝置之間進行通訊。 透過泛型存取設定檔 (間隔) 通訊協定完成裝置探索。 在探索之後，透過泛型屬性 (GATT) 通訊協定完成裝置間的通訊。 本主題提供 UWP app 中藍牙 LE 的快速概觀。 若要查看藍牙 LE 的詳細資料，請參閱引進藍牙 LE 的[藍牙核心規格](https://www.bluetooth.com/specifications/bluetooth-core-specification/)版本 4.0。 

![藍牙 LE 角色](images/gatt-roles.png)

*GATT 和 GAP 角色是在 Windows 10 1703 版中引進的*

使用下列命名空間，即可在 UWP app 中實作 GATT 和 GAP 通訊協定。
- [Windows.Devices.Bluetooth.GenericAttributeProfile](https://docs.microsoft.com/uwp/api/windows.devices.bluetooth.genericattributeprofile)
- [Windows. Bluetooth. 廣告](https://docs.microsoft.com/uwp/api/windows.devices.bluetooth.advertisement)

## <a name="central-and-peripheral"></a>中央和周邊
探索的兩個主要角色稱為「中央」和「周邊」。 一般而言，Windows 會以中央模式運作，並連線至不同的周邊裝置。 

## <a name="attributes"></a>屬性
您在 Windows 藍牙 API 中看到的常見縮寫是泛型屬性 (GATT)。 GATT 設定檔定義資料結構以及兩個藍牙 LE 裝置的通訊作業模式。 屬性是 GATT 的主要建置組塊。 主要類型的屬性是服務、特性和描述元。 這些屬性在用戶端與伺服器之間的執行方式不同，因此，更適合在相關小節中討論其互動。 

![一般設定檔中的一般屬性階層](images/gatt-service.png)

*核心速率服務以 GATT 伺服器 API 形式表示*

## <a name="client-and-server"></a>用戶端和伺服器
建立連線之後，包含資料的裝置 (通常是小型 IoT 感應器或穿戴式裝置) 稱為「伺服器」。 使用該資料來執行功能的裝置稱為「用戶端」。 例如，Windows 電腦 (用戶端) 會讀取心率監視器 (伺服器) 中的資料，來追蹤使用者是否以最佳狀態進行鍛鍊。 如需詳細資訊，請參閱[GATT 用戶端](gatt-client.md)和[GATT 伺服器](gatt-server.md)主題。

## <a name="watchers-and-publishers-beacons"></a>監看員和發行者 (指標)
除了「中央」和「周邊」角色之外，還有「觀察者」和「廣播者」角色。 廣播者通常稱為「指標」，它們不透過 GATT 進行通訊，因為它們使用廣告封包中所提供的有限空間進行通訊。 同樣地，觀察者不需要建立連線即可接收資料，它會掃描附近的廣告。 若要設定 Windows 觀察附近的廣告，請使用 [BluetoothLEAdvertisementWatcher](https://docs.microsoft.com/uwp/api/windows.devices.bluetooth.advertisement.bluetoothleadvertisementwatcher) 類別。 若要廣播指標承載，請使用[BluetoothLEAdvertisementPublisher](https://docs.microsoft.com/uwp/api/windows.devices.bluetooth.advertisement.bluetoothleadvertisementpublisher)類別。 如需詳細資訊，請參閱[廣告](ble-beacon.md)主題。

## <a name="see-also"></a>請參閱
- [Windows.Devices.Bluetooth.GenericAttributeProfile](https://docs.microsoft.com/uwp/api/windows.devices.bluetooth.genericattributeprofile)
- [Windows. Bluetooth. 廣告](https://docs.microsoft.com/uwp/api/windows.devices.bluetooth.advertisement)
- [藍牙核心規格](https://www.bluetooth.com/specifications/bluetooth-core-specification)
