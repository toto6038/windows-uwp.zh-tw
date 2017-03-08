---
author: PatrickFarley
ms.assetid: 24351dad-2ee3-462a-ae78-2752bb3374c2
title: "利用省電功能"
description: "建立 UWP 應用程式，與系統一同以省電方式來使用背景工作。"
ms.author: pafarley
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 045dfeb4696a4854b114d88da2a2cbb75d621a58
ms.lasthandoff: 02/07/2017

---

# <a name="optimize-background-activity"></a>最佳化背景活動

通用 Windows app 應該以一致的方式在所有裝置系列上正常執行。 在電池供電的裝置上，耗用電力是影響使用者對您 app 整體體驗的重要因素。 全天候的電池使用時間對每位使用者而言是令人滿意的功能，但還是需要裝置上安裝的所有軟體 (包括您自己的軟體) 所提供的效率。 

在 app 的總能源成本方面，背景工作行為可以說是最棒的因素。 背景工作是任何已向系統註冊且不需開啟 app 就能執行的程式活動。 如需詳細資訊，請參閱[建立及註冊跨處理序的背景工作](https://msdn.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task)

## <a name="background-activity-allowance"></a>背景活動允許使用時間

在 Windows 10 (版本 1607) 中，使用者可以在 \[設定\] app 的 **\[電池\]** 區段中檢視「依 App 的電池使用情況」。 他們將會在這裡看到 app 清單，以及每個 app 已耗用的電池使用時間百分比 (自上次充電以來已使用的電池使用時間量)。 

![依 App 的電池使用情況](images/battery-usage-by-app.png)

針對此清單上的 UWP app，使用者可以控制系統如何處理其背景活動。 背景活動可以指定為「一律允許」、「由 Windows 管理」(預設設定) 或「永遠不允許」(以下將提供更多詳細資料)。 使用從 [**BackgroundExecutionManager.RequestAccessAsync()**](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.backgroundexecutionmanager.requestaccessasync.aspx) 方法傳回的 **BackgroundAccessStatus** 列舉值，來查看 您的 app 有哪些背景活動允許使用時間。

![背景工作權限](images/background-task-permissions.png)

總括來說，如果您的 app 未實作負責任的背景活動管理，使用者可能會一併拒絕您 app 的背景權限，而這並非任一方樂意見到的。

## <a name="work-with-the-battery-saver-feature"></a>使用省電模式功能
省電模式是使用者可在 \[設定\] 中設定的系統層級功能。 它會在電池電量低於使用者定義的閾值時截斷所有 app 的所有背景活動，但已設定為「一律允許」的 app 的背景活動*除外*。

如果您的 app 已標示為「由 Windows 管理」，並會在開啟省電模式時呼叫 **BackgroundExecutionManager.RequestAccessAsync()** 來登錄背景活動，則它將會傳回 **DeniedSubjectToSystemPolicy** 值。 您的 app 應用來處理此動作的方式是通知使用者，指定的背景工作在關閉省電模式且重新向系統登錄之前將不會執行。 如果背景工作已經登錄要執行，而且省電模式在其觸發程序執行時已開啟，則工作將不會執行，而使用者將不會收到通知。 若要減少發生此現象的機會，最好的做法是以程式設計方式讓您的 app 在每次開啟時重新登錄它的背景工作。

儘管背景活動管理是省電模式功能的主要目的，您的 app 還是可以進行額外調整，進一步在省電模式開啟時節省能源。 藉由參考 [**PowerManager.PowerSavingMode**](https://msdn.microsoft.com/library/windows/apps/windows.phone.system.power.powermanager.powersavingmode.aspx) 屬性，從您的 app 中查看省電模式的狀態。 它是一個列舉值︰可能是 **PowerSavingMode.Off** 或 **PowerSavingMode.On**。 在省電模式開啟的情況下，您的 app 可以減少其對於動畫的使用、停止位置輪詢，或延遲同步和備份。 

## <a name="further-optimize-background-tasks"></a>進一步最佳化背景工作
以下是您可以在登錄背景工作時採取的額外步驟，以使其更省電。

使用維護觸發程序。 不使用 [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.systemtrigger.aspx) 物件，改用 [**MaintenanceTrigger**](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.maintenancetrigger.aspx) 物件來判斷背景工作啟動時機。 使用維護觸發程序的工作只有在裝置連接上 AC 電源時才能執行，並且能讓它們執行更長的時間。 如需指示，請參閱[使用維護觸發程序](https://msdn.microsoft.com/windows/uwp/launch-resume/use-a-maintenance-trigger)。

使用 **BackgroundWorkCostNotHigh** 系統條件類型。 必須符合系統條件，才能執行背景工作 (如需詳細資訊，請參閱[設定執行背景工作的條件](https://msdn.microsoft.com/windows/uwp/launch-resume/set-conditions-for-running-a-background-task))。 背景工作成本是一種度量單位，可代表執行背景工作的*「相對」*能源影響。 將裝置接上 AC 電源時正在執行的工作會標示為**低** (對電腦有一些/沒有影響)。 當裝置是以電池電力執行且螢幕為關閉狀態時正在執行的工作會標示為**高**，因為想必當時在裝置上執行的程式活動應該很少，所以背景工作會有更大的相對成本。 當裝置是以電池電力執行且螢幕為*「開啟」*狀態時正在執行的工作會標示為**中**，因為想必當時已經有一些程式活動正在執行，而背景工作會多增加一些能源成本。 **BackgroundWorkCostNotHigh** 系統條件只會延遲您工作的執行功能，直到螢幕開啟或裝置已連接上 AC 電源為止。

## <a name="test-battery-efficiency"></a>測試電池效率

請務必針對任何耗用高電量的案例，在裝置上測試您的 app。 最好是在各種不同網路強度的環境中，在許多不同的裝置上，藉由開啟和關閉省電模式來測試您的 app。

## <a name="related-topics"></a>相關主題

* [建立及註冊跨處理序的背景工作](https://msdn.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task)  
* [規劃效能](https://msdn.microsoft.com/windows/uwp/debug-test-perf/planning-and-measuring-performance)  


