---
ms.assetid: 24351dad-2ee3-462a-ae78-2752bb3374c2
title: 最佳化背景活動
description: 建立 UWP app，與系統一同以省電方式來使用背景工作。
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 71a56bc23b4b727d5be2ed35fb77afae03f0689c
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57613223"
---
# <a name="optimize-background-activity"></a>最佳化背景活動

通用 Windows app 應該以一致的方式在所有裝置系列上正常執行。 在電池供電的裝置上，耗用電力是影響使用者對您 app 整體體驗的重要因素。 全天候的電池使用時間對每位使用者而言是令人滿意的功能，但還是需要裝置上安裝的所有軟體 (包括您自己的軟體) 所提供的效率。 

在應用程式的總能源成本方面，背景工作行為可以說是最重要的因素。 背景工作是任何已向系統註冊且不需開啟 app 就能執行的程式活動。 如需詳細資訊，請參閱[建立及註冊跨處理序的背景工作](https://msdn.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task)。

## <a name="background-activity-permissions"></a>背景活動權限

在執行 Windows 10 版本 1607 或更新版本的傳統型和行動裝置上，使用者可以在 [設定] 應用程式的 [電池] 區段中檢視其 [依 App 的電池使用情況]。 他們將會在這裡看到應用程式清單，以及每個應用程式已耗用的電池使用時間百分比 (自上次充電以來已使用的電池使用時間量)。 針對這份清單上的 UWP app，使用者可以選取應用程式以開啟與背景活動相關的控制項。

![依 App 的電池使用情況](images/battery-usage-by-app.png)

### <a name="background-permissions-on-mobile"></a>行動裝置的背景權限

在行動裝置上，使用者會看到一份選項按鈕，以指定該應用程式的背景工作權限設定。 背景活動可以設定為 [一律允許]、[永不允許] 或 [由 Windows 管理]，表示系統根據一些因素來規範應用程式的背景活動。 

![背景工作權限選項按鈕](images/background-task-permissions.png)

### <a name="background-permissions-on-desktop"></a>傳統型的背景權限

在傳統型裝置上，[由 Windows 管理] 設定會呈現為切換開關，而且預設會設定為**開啟**。 如果使用者切換至**關閉**，則會呈現一個核取方塊，以手動定義背景活動權限。 當核取該方塊時，隨時都允許應用程式執行背景工作。 取消核取該方塊時，將會停用背景活動。

![開啟背景工作權限](images/background-task-permissions-on.png)

![關閉背景工作權限](images/background-task-permissions-off.png)

在您的應用程式中，您可以使用[**BackgroundExecutionManager.RequestAccessAsync()**](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.backgroundexecutionmanager.requestaccessasync.aspx)方法呼叫所傳回的[**BackgroundAccessStatus**](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.background.backgroundaccessstatus)列舉值，以判斷其目前的背景活動權限設定。

總括來說，如果您的 app 未實作負責任的背景活動管理，使用者可能會一併拒絕您 app 的背景權限，而這並非任一方樂意見到的。 如果您的應用程式已拒絕在背景執行的權限，但需要背景活動完成使用者的動作，則可以通知使用者，並將他們指向 [設定] 應用程式。 這項作業的完成方式是透過[啟動 Windows 設定應用程式](https://docs.microsoft.com/en-us/windows/uwp/launch-resume/launch-settings-app)到 [背景應用程式] 或 [電池使用量詳細資料] 頁面。

## <a name="work-with-the-battery-saver-feature"></a>使用省電模式功能
省電模式是使用者可在 \[設定\] 中設定的系統層級功能。 它會在電池電量低於使用者定義的閾值時截斷所有 app 的所有背景活動，但已設定為「一律允許」的 app 的背景活動除外。

藉由參考 [**PowerManager.EnergySaverStatus**](https://docs.microsoft.com/en-us/uwp/api/windows.system.power.energysaverstatus) 屬性，從您的應用程式中查看省電模式的狀態。 它是列舉值︰ 任一**EnergySaverStatus.Disabled**、**EnergySaverStatus.Off**或**EnergySaverStatus.On**。 如果您的應用程式需要背景活動，並且未設定為 [一律允許]，則應該會處理**EnergySaverStatus.On**，方法是通知使用者，在省電模式關閉之前不會執行指定的背景工作。 儘管背景活動管理是省電模式功能的主要目的，您的 app 還是可以進行額外調整，進一步在省電模式開啟時節省能源。  在省電模式開啟的情況下，您的 app 可以減少其對於動畫的使用、停止位置輪詢，或延遲同步和備份。 

## <a name="further-optimize-background-tasks"></a>進一步最佳化背景工作
以下是您可以在登錄背景工作時採取的額外步驟，以使其更省電。

### <a name="use-a-maintenance-trigger"></a>使用維護觸發程序 
不使用 [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.systemtrigger.aspx) 物件，改用 [**MaintenanceTrigger**](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.maintenancetrigger.aspx) 物件來判斷背景工作啟動時機。 使用維護觸發程序的工作只有在裝置連接上 AC 電源時才能執行，並且能讓它們執行更長的時間。 如需指示，請參閱[使用維護觸發程序](https://msdn.microsoft.com/windows/uwp/launch-resume/use-a-maintenance-trigger)。

### <a name="use-the-backgroundworkcostnothigh-system-condition-type"></a>使用 **BackgroundWorkCostNotHigh** 系統條件類型
必須符合系統條件，才能執行背景工作 (如需詳細資訊，請參閱[設定執行背景工作的條件](https://msdn.microsoft.com/windows/uwp/launch-resume/set-conditions-for-running-a-background-task))。 背景工作成本是一種度量單位，可代表執行背景工作的「相對」能源影響。 將裝置接上 AC 電源時正在執行的工作會標示為**低** (對電腦有一些/沒有影響)。 當裝置是以電池電力執行且螢幕為關閉狀態時正在執行的工作會標示為**高**，因為想必當時在裝置上執行的程式活動應該很少，所以背景工作會有更大的相對成本。 當裝置是以電池電力執行且螢幕為「開啟」狀態時正在執行的工作會標示為*中*，因為想必當時已經有一些程式活動正在執行，而背景工作會多增加一些能源成本。 **BackgroundWorkCostNotHigh** 系統條件只會延遲您工作的執行功能，直到螢幕開啟或裝置已連接上 AC 電源為止。

## <a name="test-battery-efficiency"></a>測試電池效率

請務必針對任何耗用高電量的案例，在裝置上測試您的應用程式。 最好是在各種不同網路強度的環境中，在許多不同的裝置上，藉由開啟和關閉省電模式來測試您的 app。

## <a name="related-topics"></a>相關主題

* [建立並註冊跨處理序背景工作](https://msdn.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task)  
* [規劃效能](https://msdn.microsoft.com/windows/uwp/debug-test-perf/planning-and-measuring-performance)  

