---
author: muhsinking
ms.assetid: 0b891f63-02fa-4c30-b307-9fbcccac5caa
title: 裝置、感應器及電源
description: 為提供使用者豐富的使用經驗，您可能會發現有必要將外部裝置或感應器與您的應用程式整合。
ms.author: mukin
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 5ca78338b501b8c24549b1348c051a02ea62a501
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/05/2018
ms.locfileid: "6030422"
---
# <a name="devices-sensors-and-power"></a>裝置、感應器及電源


為提供使用者豐富的使用經驗，您可能會發現有必要將外部裝置或感應器與您的應用程式整合。 以下是一些功能範例，您可以使用本節所描述的技術將這些功能新增到您的 app 中。

-   提供進階的列印經驗
-   將動作和方向感應器整合到您的遊戲中
-   直接或透過網路通訊協定連線到裝置

| 主題 | 描述 |
|-------|-------------|
| [啟用裝置功能](enable-device-capabilities.md) | 本教學課程描述如何在 Microsoft Visual Studio 中宣告裝置功能。 這可以讓您的 App 使用相機、麥克風、定位感應器及其他裝置。 | 
| [以使用者模式存取 Windows IoT](enable-usermode-access.md) | 本教學課程描述如何在 Windows 10 IoT 核心版上以使用者模式存取 GPIO、I2C、SPI 及 UART。 |
| [列舉裝置](enumerate-devices.md) | 列舉命名空間可讓您尋找內部連接到系統、外部連接，或者可透過無線或網路通訊協定偵測到的裝置。 |
| [配對裝置](pair-devices.md) | 有些裝置在使用之前需要先進行配對。 [<strong>Windows.Devices.Enumeration</strong>](https://msdn.microsoft.com/library/windows/apps/BR225459) 命名空間支援三種不同方式來配對裝置。 |
| [服務點](point-of-service.md) | 本章節描述了如何與點週邊設備的服務，例如條碼掃描器、 收據印表機、 收銀機等的互動。 | 
| [感應器](sensors.md) | 感應器可讓 app 得知裝置與周遭實際環境之間的關係。 感應器會將裝置的方向、指向及動作告知您的 app。 |
| [藍牙](bluetooth.md) | 本節包含如何將藍牙整合至通用 Windows 平台 (UWP) 應用程式的文章，包括如何使用 RFCOMM、GATT 及低功耗 (LE) 廣告。 | 
| [列印與掃描](printing-and-scanning.md) | 本節描述如何從您的通用 Windows 應用程式列印與掃描。 | 
| [3D 列印](3d-printing.md) | 本節描述如何利用通用 Windows 應用程式中的 3D 列印功能。 |
| [建立 NFC 智慧卡應用程式](host-card-emulation.md) | Windows Phone 8.1 使用以 SIM 卡為基礎的安全元素來支援 NFC 卡模擬應用程式，但該模型需要安全的付款應用程式才能與行動網路運算子 (MNO) 緊密結合。 這會限制其他未結合 MNO 的商家或開發人員所提供的各種可能付款解決方案， 在 windows 10 行動裝置版中，我們已導入了新的卡片模擬技術，稱為主機卡模擬 (HCE)。 HCE 技術可讓您的應用程式直接與 NFC 卡讀卡機進行通訊。 本主題說明主機卡模擬 (HCE) 在 windows 10 行動裝置版裝置上的運作方式，以及如何開發 HCE 應用程式，以便您的客戶可以存取他們的手機而不是實體卡片透過您的服務而不需要使用 mno 共同作業。 |
| [取得電池資訊](get-battery-info.md) | 了解如何使用 [<strong>Windows.Devices.Power</strong>](https://msdn.microsoft.com/library/windows/apps/Dn895017) 命名空間中的 API 取得詳細的電池資訊。 |

