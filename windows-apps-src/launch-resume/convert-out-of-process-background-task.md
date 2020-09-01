---
title: 將跨處理序背景工作移植到同處理序背景工作
description: 將跨進程背景工作移植到在您的前景應用程式進程中執行的同進程背景工作。
ms.date: 09/19/2018
ms.topic: article
keywords: windows 10，uwp，背景工作，app service
ms.assetid: 5327e966-b78d-4859-9b97-5a61c362573e
ms.localizationpriority: medium
ms.openlocfilehash: f500be50c8b57415f5ad2fe62c28cc21f4b57b68
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89162652"
---
# <a name="port-an-out-of-process-background-task-to-an-in-process-background-task"></a>將跨處理序背景工作移植到同處理序背景工作

將跨進程 (OOP) 背景活動移植到同進程活動最簡單的方式，就是將 [IBackgroundTask](/uwp/api/windows.applicationmodel.background.ibackgroundtask.run?f=255&MSPPError=-2147217396) 方法程式碼帶到您的應用程式內，並從 [OnBackgroundActivated](/uwp/api/windows.ui.xaml.application.onbackgroundactivated)起始程式碼。 此處所述的技術並不是要從 OOP 背景工作建立填充碼至同進程背景工作，這是關於重寫 (或移植) OOP 版本到同進程版本。

如果您的 app 有多個背景工作，[背景啟用範例](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/BackgroundActivation)會示範如何使用 `BackgroundActivatedEventArgs.TaskInstance.Task.Name` 來識別正在初始化哪個工作。

如果您目前在背景與前景處理序之間進行通訊，可以將狀態管理與通訊程式碼移除。

## <a name="background-tasks-and-trigger-types-that-cannot-be-converted"></a>無法轉換的背景工作與觸發程序類型

* 同處理序背景工作不支援啟用 VoIP 背景工作。
* 同進程背景工作不支援下列觸發程式：  [DeviceUseTrigger](/uwp/api/windows.applicationmodel.background.deviceusetrigger?f=255&MSPPError=-2147217396)、 [DeviceServicingTrigger](/uwp/api/windows.applicationmodel.background.deviceservicingtrigger) 和 **IoTStartupTask**