---
ms.assetid: 404783BA-8859-4BFB-86E3-3DD2042E66F5
title: Bluetooth
description: 本節包含如何將藍牙整合至通用 Windows 平台 (UWP) 應用程式的文章，包括如何使用 RFCOMM、GATT 及低功耗 (LE) 廣告。
ms.date: 06/26/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: aad368adf500c5968bf6374a5855a7e0dab0a044
ms.sourcegitcommit: 015291bdf2e7d67076c1c85fc025f49c840ba475
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/27/2020
ms.locfileid: "85469563"
---
# <a name="bluetooth"></a>Bluetooth
本節包含如何將藍牙整合到通用 Windows 平台 (UWP) 應用程式的文章。 您可以選擇要在應用程式中執行的兩種不同藍牙技術。

> [!Important]
> 您必須在*package.appxmanifest.xml*中宣告 "bluetooth" 功能。
>
> `<Capabilities> <DeviceCapability Name="bluetooth" /> </Capabilities>`

## <a name="classic-bluetooth-rfcomm"></a>傳統型藍牙 (RFCOMM)
在藍牙 LE 之前，裝置一般會使用此通訊協定來使用藍牙進行通訊。 這個通訊協定十分簡單且適用於裝置間的通訊，而不需要省電。 如需這個通訊協定的詳細資訊，包括程式碼範例，請參閱[藍牙 RFCOMM](send-or-receive-files-with-rfcomm.md)主題。

## <a name="bluetooth-low-energy-le"></a>藍牙低功耗 (LE)
藍牙低功耗 (LE) 是一種規格，定義通訊協定來進行探索以及在具有具效率能源使用需求的裝置之間進行通訊。 如需詳細資訊，包括程式碼範例，請參閱[藍牙低功耗](bluetooth-low-energy-overview.md)主題。

## <a name="see-also"></a>另請參閱
- [藍牙開發人員常見問題集](bluetooth-dev-faq.md)