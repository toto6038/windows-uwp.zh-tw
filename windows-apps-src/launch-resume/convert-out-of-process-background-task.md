---
author: TylerMSFT
title: "將跨處理序背景工作轉換成同處理序背景工作"
description: "將跨處理序背景工作轉換成在前景 App 處理序內執行的同處理序背景工作。"
translationtype: Human Translation
ms.sourcegitcommit: 7d1c160f8b725cd848bf8357325c6ca284b632ae
ms.openlocfilehash: b361a558ecef2030370590eedbef69bd04cf68bd

---

# 將跨處理序背景工作轉換成同處理序背景工作

將跨處理序背景活動轉換成同處理序活動的最簡單方法是，將 [IBackgroundTask.Run](https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.background.ibackgroundtask.run.aspx?f=255&MSPPError=-2147217396) 方法程式碼放入應用程式內，並從 [OnBackgroundActivated](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.application.onbackgroundactivated.aspx) 加以初始化。

如果您的 app 有多個背景工作，[背景啟用範例](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/BackgroundActivation)會示範如何使用 `BackgroundActivatedEventArgs.TaskInstance.Task.Name` 來識別正在初始化哪個工作。

如果您目前在背景與前景處理序之間進行通訊，可以將狀態管理與通訊程式碼移除。

## 無法轉換的背景工作與觸發程序類型

* 同處理序背景工作不支援啟用 VoIP 背景工作。
* 同處理序背景工作不支援下列觸發程序︰[DeviceUseTrigger](https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.background.deviceusetrigger.aspx?f=255&MSPPError=-2147217396)、[DeviceServicingTrigger](https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.background.deviceservicingtrigger.aspx) 和 **IoTStartupTask**



<!--HONumber=Nov16_HO1-->


