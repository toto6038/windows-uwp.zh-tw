---
author: TerryWarwick
title: PointOfService device objects
description: 深入了解 PointOfService 裝置物件的建立
ms.author: jken
ms.date: 06/19/2018
ms.topic: article
keywords: windows 10, uwp, point of service, pos, 服務點
ms.localizationpriority: medium
ms.openlocfilehash: 31af943ab4a9231f58fb2e3d5489e9ae80d8d565
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/02/2018
ms.locfileid: "5937697"
---
# <a name="pointofservice-device-objects"></a>PointOfService device objects

## <a name="creating-a-device-object"></a>建立裝置物件
一旦您識別出您想要使用的 PointOfService 裝置，您只要使用以程式設計方式選擇的 [**DeviceID**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.id)，從最新的列舉或儲存的 DeviceID 呼叫 [**FromIdAsync**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.fromidasync)，或者使用者已選取來建立新的服務點裝置物件。

這個範例嘗試使用 DeviceID 以 FromIdAsync 建立新的 BarcodeScanner 物件。 如果建立物件時失敗，會寫入偵錯的訊息。

```Csharp

    BarcodeScanner barcodeScanner = await BarcodeScanner.FromIdAsync(DeviceId);

    if(barcodeScanner != null)
    {
        // after successful creation, claim the scanner for exclusive use and enable it to exchange data
    }
    else
    {
        Debug.WriteLine("Failure to create barcodeScanner object");
    }
    
```

在您擁有裝置物件後，您就可以再存取裝置的方法、內容和活動。  

## <a name="device-object-lifecycle"></a>裝置物件週期
Windows 8 推出前，App 的週期很簡單。 Win32 與 .NET App 執行或不執行，通常會為完整的 App 週期宣告 PointOfService peripehrals。 當使用者將它們縮至最小或切換到其他 App 時，它們會繼續執行。 這原本不成問題，直到可攜式裝置和電源管理變得越來越重要。

Windows 8 引進的新應用程式模型提供 UWP 應用程式。 增加一種新的高階暫停狀態。 UWP app 會在使用者將其縮至最小或切換到其他應用程式時，很快暫停。 這表示 App 的執行緒已停止，除非作業系統需要重新宣告資源，並代表 PointOfService 周邊的任何裝置物件自動關閉以允許其他應用程式存取周邊，否則會 App 會留在記憶體中。 當使用者切換回 App 時，它可以快速還原到執行狀態，並只要他們仍可恢復繼續使用，就會還原 PointOfService 周邊連接。

您可以使用 <DeviceObject>.Closed 事件處理常式偵測物件是否因為任何原因關閉，然後記下裝置 ID 以便未來重新建立連接。   或者，您可能想要在「App 暫停」通知上處理這個，以保留裝置 ID 用於在「App 繼續執行」通知上重新建立裝置連接。  請確定您不會在 <DeviceObject>.Closed 和「App 暫停」二者上對裝置物件重複執行事件處理常式和重複的動作。

> [!TIP]
> 有關 Windows 10 通用 Windows 平台 (UWP) 應用程式週期的詳細資訊，請參閱下列主題：
> - [Windows 10 通用 Windows 平台 (UWP) 應用程式週期](../launch-resume/app-lifecycle.md)
> - [處理 App 暫停](../launch-resume/suspend-an-app.md)
> - [處理 App 繼續執行](../launch-resume/resume-an-app.md)
