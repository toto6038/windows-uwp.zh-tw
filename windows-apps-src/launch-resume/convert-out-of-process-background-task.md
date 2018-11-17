---
author: TylerMSFT
title: 將跨處理序背景工作移植到同處理序背景工作
description: 跨處理序背景工作移植到您的前景 app 處理序內執行的同處理序背景工作。
ms.author: twhitney
ms.date: 09/19/2018
ms.topic: article
keywords: windows 10、 uwp、 背景工作、 應用程式服務
ms.assetid: 5327e966-b78d-4859-9b97-5a61c362573e
ms.localizationpriority: medium
ms.openlocfilehash: 47008fd7ba0b7724aa8fbdc2dd6cbd55288faea0
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/17/2018
ms.locfileid: "7166370"
---
# <a name="port-an-out-of-process-background-task-to-an-in-process-background-task"></a>將跨處理序背景工作移植到同處理序背景工作

移植您的處理程序 (OOP) 背景活動，同處理序活動的最簡單方式是將應用程式內您[IBackgroundTask.Run](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.ibackgroundtask.run.aspx?f=255&MSPPError=-2147217396)方法的程式碼，並從[OnBackgroundActivated](/uwp/api/windows.ui.xaml.application.onbackgroundactivated)起始它。 以下所述的技術不需建立從 OOP 的背景工作的填充碼到同處理序背景工作;它的關於重寫 （或移植） 來處理程序版本 OOP 版本。

如果您的 app 有多個背景工作，[背景啟用範例](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/BackgroundActivation)會示範如何使用 `BackgroundActivatedEventArgs.TaskInstance.Task.Name` 來識別正在初始化哪個工作。

如果您目前在背景與前景處理序之間進行通訊，可以將狀態管理與通訊程式碼移除。

## <a name="background-tasks-and-trigger-types-that-cannot-be-converted"></a>無法轉換的背景工作與觸發程序類型

* 同處理序背景工作不支援啟用 VoIP 背景工作。
* 同處理序背景工作不支援下列觸發程序︰[DeviceUseTrigger](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.deviceusetrigger.aspx?f=255&MSPPError=-2147217396)、[DeviceServicingTrigger](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.deviceservicingtrigger.aspx) 和 **IoTStartupTask**。
