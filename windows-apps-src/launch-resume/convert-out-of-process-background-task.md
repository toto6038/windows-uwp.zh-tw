---
title: 將跨處理序背景工作移植到同處理序背景工作
description: 您的前景應用程式處理序內執行同處理序背景工作處理序外背景工作的連接埠。
ms.date: 09/19/2018
ms.topic: article
keywords: windows 10、 uwp、 背景工作、 應用程式服務
ms.assetid: 5327e966-b78d-4859-9b97-5a61c362573e
ms.localizationpriority: medium
ms.openlocfilehash: 97dd249165877591743892a136d51e0969dd902a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57601203"
---
# <a name="port-an-out-of-process-background-task-to-an-in-process-background-task"></a>將跨處理序背景工作移植到同處理序背景工作

連接埠至處理序中的活動您放大處理序 (OOP) 背景活動的最簡單方式是將您[IBackgroundTask.Run](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.ibackgroundtask.run.aspx?f=255&MSPPError=-2147217396)方法在您的應用程式程式碼，並起始從[OnBackgroundActivated](/uwp/api/windows.ui.xaml.application.onbackgroundactivated). 此處所述的技巧不需從 OOP 背景工作建立填充碼，以同處理序背景工作它的關於重寫 （或將移植） 以同處理序版本的 OOP 版本。

如果您的 app 有多個背景工作，[背景啟用範例](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/BackgroundActivation)會示範如何使用 `BackgroundActivatedEventArgs.TaskInstance.Task.Name` 來識別正在初始化哪些工作。

如果您目前在背景與前景處理序之間進行通訊，可以將狀態管理與通訊程式碼移除。

## <a name="background-tasks-and-trigger-types-that-cannot-be-converted"></a>無法轉換的背景工作與觸發程序類型

* 同處理序背景工作不支援啟用 VoIP 背景工作。
* 同處理序背景工作不支援下列觸發程序：[DeviceUseTrigger](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.deviceusetrigger.aspx?f=255&MSPPError=-2147217396)， [DeviceServicingTrigger](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.deviceservicingtrigger.aspx)和**IoTStartupTask**
