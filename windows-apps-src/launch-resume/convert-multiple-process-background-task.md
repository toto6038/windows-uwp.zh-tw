---
author: TylerMSFT
title: "將多處理序的背景工作轉換成單一處理序的背景工作"
description: "將在個別處理序中執行的背景工作轉換成在前景 app 處理序內執行的背景工作。"
translationtype: Human Translation
ms.sourcegitcommit: 2c34ca40d3c930254500477ab5a2e41e5206d823
ms.openlocfilehash: e342667347cf3b89a5aa193495cbf7195263b276

---

# 將多處理序的背景工作轉換成單一處理序的背景工作

將多處理序背景活動轉換成單一處理序的最簡單方法是，將 [IBackgroundTask.Run](https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.background.ibackgroundtask.run.aspx?f=255&MSPPError=-2147217396) 方法程式碼放入應用程式內，並從 [OnBackgroundActivated](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.application.onbackgroundactivated.aspx) 加以初始化。

如果您的 app 有多個背景工作，[背景啟用範例](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/BackgroundActivation)會示範如何使用 `BackgroundActivatedEventArgs.TaskInstance.Task.Name` 來識別正在初始化哪些工作。

如果您目前在背景與前景處理序之間進行通訊，可以將狀態管理與通訊程式碼移除。

## 無法轉換的背景工作與觸發程序類型

* 單一處理序背景工作不支援啟用 VoIP 背景工作。
* 單一處理序背景工作不支援下列的觸發程序︰[DeviceUseTrigger](https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.background.deviceusetrigger.aspx?f=255&MSPPError=-2147217396)、[DeviceServicingTrigger](https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.background.deviceservicingtrigger.aspx) 和 **IoTStartupTask**



<!--HONumber=Aug16_HO3-->


