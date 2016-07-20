---
author: msatranjr
title: "藍牙廣告"
description: "本節包含如何透過 AdvertisementWatcher 和 AdvertisementPublisher API 的使用者，將藍牙低電源 (LE) 廣告整合到通用 Windows 平台 (UWP) 應用程式的文章。"
translationtype: Human Translation
ms.sourcegitcommit: 62e97bdb8feb78981244c54c76a00910a8442532
ms.openlocfilehash: a419ad04fe4f21867f2f1bd1664fbce39a7da792

---

# 藍牙廣告

\[ 針對 Windows 10 上的 UWP App 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

** 重要 API ** 

-   [**Windows.Devices.Bluetooth.Advertisement**](https://msdn.microsoft.com/library/windows/apps/windows.devices.bluetooth.advertisement.aspx)

這篇文章提供適用於通用 Windows 平台 (UWP) 應用程式的藍牙廣告 (指標) 概觀。  

## 概觀

有兩個開發人員可以使用 Advertisement API 執行的主要函式：

-   [Advertisement Watcher](https://msdn.microsoft.com/library/windows/apps/windows.devices.bluetooth.advertisement.bluetoothleadvertisementwatcher.aspx)：接聽附近的指標並根據承載或鄰近性篩選出指標。  
-   [Advertisement Publisher](https://msdn.microsoft.com/library/windows/apps/windows.devices.bluetooth.advertisement.bluetoothleadvertisementpublisher.aspx)：定義 Windows 承載，以代表開發人員宣傳。  

在 Github 上的[藍牙廣告範例](http://go.microsoft.com/fwlink/p/?LinkId=619990)中可以找到完整的範例程式碼



<!--HONumber=Jun16_HO5-->


