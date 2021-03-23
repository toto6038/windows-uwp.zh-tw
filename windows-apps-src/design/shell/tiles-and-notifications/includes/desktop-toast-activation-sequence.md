---
ms.openlocfilehash: 62470070b726aa7101b8299296dc1f4ef67f30e4
ms.sourcegitcommit: 5e718720d1032a7089dea46a7c5aefa6cda3385f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/12/2021
ms.locfileid: "104804316"
---
當使用者按一下您的任何通知 (或通知) 上的按鈕時，將會發生下列情況：

**如果您的應用程式目前正在** 執行 .。。

1. **ToastNotificationManagerCompat. OnActivated** 事件將會在背景執行緒上叫用。

**如果您的應用程式目前已關閉**.。。

1. 您應用程式的 EXE 將會啟動，並 `ToastNotificationManagerCompat.WasCurrentProcessToastActivated()` 會傳回 true，表示進程是因為新式啟用而啟動，而且很快就會叫用事件處理常式。
1. 然後，會在背景執行緒上叫用 **ToastNotificationManagerCompat. OnActivated** 事件。