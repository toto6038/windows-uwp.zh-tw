---
author: TylerMSFT
title: 將跨處理序背景工作轉換成同處理序背景工作
description: 將跨處理序背景工作轉換成，在前景應用程式處理序內執行的同處理序背景工作。
ms.author: twhitney
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10，uwp，背景工作，應用程式服務
ms.assetid: 5327e966-b78d-4859-9b97-5a61c362573e
ms.localizationpriority: medium
ms.openlocfilehash: 1144443f943f134991d050dea1457f252eaaf36d
ms.sourcegitcommit: 53ba430930ecec8ea10c95b390fe6e654fe363e1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/06/2018
ms.locfileid: "3418242"
---
# <a name="convert-an-out-of-process-background-task-to-an-in-process-background-task"></a>將跨處理序背景工作轉換成同處理序背景工作

將跨處理序背景活動轉換成同處理序活動的最簡單方法是，將 [IBackgroundTask.Run](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.ibackgroundtask.run.aspx?f=255&MSPPError=-2147217396) 方法程式碼放入應用程式內，並從 [OnBackgroundActivated](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.application.onbackgroundactivated.aspx) 加以初始化。

如果您的 app 有多個背景工作，[背景啟用範例](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/BackgroundActivation)會示範如何使用 `BackgroundActivatedEventArgs.TaskInstance.Task.Name` 來識別正在初始化哪個工作。

如果您目前在背景與前景處理序之間進行通訊，可以將狀態管理與通訊程式碼移除。

## <a name="background-tasks-and-trigger-types-that-cannot-be-converted"></a>無法轉換的背景工作與觸發程序類型

* 同處理序背景工作不支援啟用 VoIP 背景工作。
* 同處理序背景工作不支援下列觸發程序︰[DeviceUseTrigger](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.deviceusetrigger.aspx?f=255&MSPPError=-2147217396)、[DeviceServicingTrigger](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.deviceservicingtrigger.aspx) 和 **IoTStartupTask**。
