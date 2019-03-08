---
title: 使用背景工作支援應用程式
description: 本節中的主題將示範如何在背景中執行輕量型程式碼來回應觸發程序。
ms.assetid: EFF7CBFB-D309-4ACB-A2A5-28E19D447E32
ms.date: 08/21/2017
ms.topic: article
keywords: windows 10 uwp，背景工作
ms.localizationpriority: medium
ms.openlocfilehash: 71026762933267e1cad9a1cd9b6581eed1dadbb8
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57618023"
---
# <a name="support-your-app-with-background-tasks"></a>使用背景工作支援應用程式


本節中的主題將示範如何在背景中執行輕量型程式碼來回應觸發程序。 您可以使用背景工作，在 app 被暫停或未執行時提供功能。 您也可以將背景工作用於即時通訊 App，像是 VOIP、郵件和 IM。

## <a name="playing-media-in-the-background"></a>在背景播放媒體

從 Windows 10 版本 1607 開始，在背景播放音訊變得更加容易。 如需詳細資訊，請參閱[在背景播放媒體](https://msdn.microsoft.com/windows/uwp/audio-video-camera/background-audio)。

## <a name="in-process-and-out-of-process-background-tasks"></a>同處理序與跨處理序背景工作

有兩種執行背景工作的方法：

* 同處理序：App 及其背景處理程序在同一個處理序中執行
* 跨處理序：App 及其背景處理程序在不同的處理序中執行。

同處理序背景支援是在 Windows 10 版本 1607 中引進，用來簡化背景工作的撰寫。 但是您仍然可以撰寫跨處理程序工作。 如需有關撰寫同處理程序與跨處理程序背景工作的時機建議，請參閱[背景工作指導方針](guidelines-for-background-tasks.md)。

跨處理序背景工作的復原性較佳，因為如果發生錯誤，背景處理程序無法關閉您的 App 處理序。 但復原性的代價是在 App 與背景工作之間管理跨處理序通訊的複雜度變高。

跨處理序背景工作實作為實作 [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794) 介面的輕量型類別，而這個介面會由作業系統在個別處理序 (backgroundtaskhost.exe) 中執行。 使用 [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768) 類別來註冊背景工作。 在登錄背景工作時，類別名稱可用來指定進入點。

在 Windows 10 版本 1607 中，您可以在不建立背景工作的情況下啟用背景活動。 您可以改為直接在前景應用程式的處理序內執行背景程式碼。

若要快速開始使用同處理序背景工作，請參閱[建立及註冊同處理序背景工作](create-and-register-an-inproc-background-task.md)。

若要快速開始使用跨處理序背景工作，請參閱[建立及註冊跨處理序背景工作](create-and-register-a-background-task.md)。

> [!TIP]
> 您從 Windows 10 開始，不再需要將應用程式放在鎖定畫面上，為註冊背景工作的必要元件。

## <a name="background-tasks-for-system-events"></a>系統事件的背景工作

您的應用程式可以藉由使用 [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/br224838) 類別登錄背景工作的方式，回應系統產生的事件。 App 可以使用下列任何系統事件觸發程序 (於 [**SystemTriggerType**](https://msdn.microsoft.com/library/windows/apps/br224839) 中定義)

| 觸發程序名稱                     | 描述                                                                                                    |
|----------------------------------|----------------------------------------------------------------------------------------------------------------|
| **InternetAvailable**            | 網際網路變成可用。                                                                                |
| **NetworkStateChange**           | 網路變更，例如發生費用或連線變更。                                              |
| **OnlineIdConnectedStateChange** | 與帳戶關聯的線上識別碼變更。                                                                 |
| **SmsReceived**                  | 安裝的行動式寬頻裝置收到新的 SMS 訊息。                                         |
| **TimeZoneChange**               | 裝置時區變更 (例如，系統因為日光節約時間而調整時鐘)。 |

如需詳細資料，請參閱[使用背景工作回應系統事件](respond-to-system-events-with-background-tasks.md)。

## <a name="conditions-for-background-tasks"></a>背景工作的條件

您可以新增條件來控制背景工作執行的時間，即使是觸發後也可以。 觸發之後，背景工作會等到所有條件都符合才執行。 以下為可以使用的條件 (以 [**SystemConditionType**](https://msdn.microsoft.com/library/windows/apps/br224835) 列舉表示)。

| 條件名稱           | 描述                       |
|--------------------------|-----------------------------------|
| **InternetAvailable**    | 網際網路必須為可用。   |
| **InternetNotAvailable** | 網際網路必須為無法使用。 |
| **SessionConnected**     | 工作階段必須已連線。    |
| **SessionDisconnected**  | 工作階段必須已中斷連線。 |
| **UserNotPresent**       | 使用者必須是離開狀態。            |
| **UserPresent**          | 使用者必須是上線狀態。         |

新增 **InternetAvailable** 條件至您的背景工作 [BackgroundTaskBuilder.AddCondition](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) 可延遲觸發背景工作，直到網路堆疊執行。 這個條件可以節省電源，因為直到網路可用前，背景工作都不會執行。 這個條件不提供即時啟動。

如果您的背景工作需要網路連線，請設定 [IsNetworkRequested](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) 以確保在背景工作執行時保持網路連線。 這會告訴背景工作基礎結構在工作執行時隨時保持網路連線，即使裝置已進入 [連線待命] 模式。 如果您的背景工作不會設定**IsNetworkRequested**，然後將您的背景工作將無法存取網路，在 連線待命模式 （例如，手機的螢幕關閉時。）  
如需背景工作條件的詳細資訊，請參閱[設定執行背景工作的條件](set-conditions-for-running-a-background-task.md)。

## <a name="application-manifest-requirements"></a>應用程式資訊清單需求

您必須先在應用程式資訊清單中宣告 App，App 才能成功註冊在跨處理序中執行的背景工作。 背景工作如果是在與其主控 App 相同的處理序中執行，您就不需要在應用程式資訊清單中宣告。 如需詳細資訊，請參閱[在應用程式資訊清單中宣告背景工作](declare-background-tasks-in-the-application-manifest.md)。

## <a name="background-tasks"></a>背景工作

下列即時觸發程序可用來在背景執行輕量型自訂程式碼：

| 即時觸發程序  | 描述 |
|--------------------|-------------|
| **控制通道** | 背景工作可以保持連線，並透過使用 [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032) 在控制通道上接收訊息。 如果您的 App 正在接聽通訊端，則您可以使用「通訊端代理程式」，而不使用 **ControlChannelTrigger**。 如需使用通訊端代理程式的詳細資訊，請參閱 [SocketActivityTrigger](https://msdn.microsoft.com/library/windows/apps/dn806009)。 Windows Phone 不支援 **ControlChannelTrigger**。 |
| **計時器** | 背景工作的執行頻率最高可達每 15 分鐘一次，而且只要使用 [**TimeTrigger**](https://msdn.microsoft.com/library/windows/apps/br224843) 即可設定在特定時間執行。 如需詳細資訊，請參閱[在計時器上執行背景工作](run-a-background-task-on-a-timer-.md)。 |
| **推播通知** | 背景工作會回應 [**PushNotificationTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700543) 以接收原始推播通知。 |

**注意**  

通用 Windows app 必須先呼叫 [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485)，才能登錄任何背景觸發程序類型。

為了確保您的通用 Windows app 在您發行更新之後會繼續正常執行，請呼叫 [**RemoveAccess**](https://msdn.microsoft.com/library/windows/apps/hh700471)，然後在 App 於更新後啟動時呼叫 [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485)。 如需詳細資訊，請參閱[背景工作的指導方針](guidelines-for-background-tasks.md)。

**觸發程序執行個體的數目限制：** 有一些限制的應用程式可以註冊某些觸發程序的執行個體數目。 App 只能針對其每個執行個體註冊 [ApplicationTrigger](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.ApplicationTrigger)、[MediaProcessingTrigger](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.mediaprocessingtrigger) 和 [DeviceUseTrigger](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.deviceusetrigger.aspx?f=255&MSPPError=-2147217396) 一次。 如果 App 超過此限制，註冊將會擲回例外狀況。

## <a name="system-event-triggers"></a>系統事件觸發程序

[  **SystemTriggerType**](https://msdn.microsoft.com/library/windows/apps/br224839) 列舉代表下列系統事件觸發程序：

| 觸發程序名稱            | 描述                                                       |
|-------------------------|-------------------------------------------------------------------|
| **UserPresent**         | 背景工作會在使用者變成存在時觸發。   |
| **UserAway**            | 背景工作會在使用者變成不在時觸發。    |
| **ControlChannelReset** | 背景工作會在控制通道重設時觸發。 |
| **SessionConnected**    | 背景工作會在工作階段連線時觸發。   |

   
下列系統事件觸發程序會在使用者將 App 移入或移出鎖定畫面時發出訊號。

| 觸發程序名稱                     | 描述                                  |
|----------------------------------|----------------------------------------------|
| **LockScreenApplicationAdded**   | App 磚已新增到鎖定畫面。     |
| **LockScreenApplicationRemoved** | App 磚已從鎖定畫面移除。 |

 
## <a name="background-task-resource-constraints"></a>背景工作資源限制

背景工作為輕量型。 將背景執行降至最低可確保不論是在前景 App 還是電池使用時間上，使用者都能獲得最佳體驗。 這是透過將資源限制套用至背景工作來強制執行。

背景工作的使用時間是限制為 30 秒時鐘時間。

### <a name="memory-constraints"></a>記憶體限制

由於低記憶體裝置的資源限制，因此背景工作可能會有記憶體限制，此限制會決定背景工作所能使用的記憶體量上限。 如果您的背景工作嘗試執行會超出這個限制的作業，該作業將會失敗，而可能會產生該工作可以處理的記憶體不足例外狀況。 如果該工作無法處理記憶體不足例外狀況，或所嘗試之作業的本質導致未產生記憶體不足例外狀況，則該工作將會立即終止。  

您可以使用 [**MemoryManager**](https://msdn.microsoft.com/library/windows/apps/dn633831) API 查詢目前的記憶體使用量及限制以探索您的最高限度 (如果有的話)，並且監視您的背景工作正在進行的記憶體使用量。

### <a name="per-device-limit-for-apps-with-background-tasks-for-low-memory-devices"></a>低記憶體裝置針對具有背景工作的應用程式的個別裝置限制

在記憶體受限的裝置上，會限制可安裝在裝置上的 app 數量以及在任何指定時間可使用背景工作的 app 數量。 如果超過這個數字，對 [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485) 的呼叫 (登錄所有背景工作時所需進行的呼叫) 就會失敗。

### <a name="battery-saver"></a>省電模式

除非您豁免您的 App，使其在省電模式開啟時，仍然可以執行背景工作並接收推播通知，否則當電池省電功能啟用時，只要裝置沒有連接外部電源，而且電池電量低於指定的剩餘電量，它就會阻止背景工作執行。 但是不會阻止您登錄背景工作。

不過，如 企業應用程式，將不會在 Microsoft Store 中發行的應用程式，請參閱[無限期地執行在背景中](run-in-the-background-indefinetly.md)以了解如何使用功能，在背景中執行的背景工作 」 或 「 延伸的執行工作階段無限期。

## <a name="background-task-resource-guarantees-for-real-time-communication"></a>即時通訊的背景工作資源保證

為了防止資源配額干擾即時通訊功能，使用 [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032) 與 [**PushNotificationTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700543) 之背景工作的每個執行中工作都會收到保證的 CPU 資源配額。 資源配額如以上所述，並且在這些背景工作會保持不變。

針對 [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032) 與 [**PushNotificationTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700543) 背景工作，您的 app 不必執行其他動作就可以取得保證的資源配額。 系統永遠都會將這些視為極為重要的背景工作。

## <a name="maintenance-trigger"></a>維護觸發程序

維護工作只有在裝置插入 AC 電源時才能執行。 如需詳細資訊，請參閱[使用維護觸發程序](use-a-maintenance-trigger.md)。

## <a name="background-tasks-for-sensors-and-devices"></a>感應器和裝置的背景工作

您的 App 可以使用 [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337) 類別，從背景工作存取感應器和週邊裝置。 您可以在長時間執行的操作使用這個觸發程序，例如資料同步或監控。 與系統事件的工作不同，**DeviceUseTrigger** 工作只能在您的 App 於前景中執行時觸發，而且您不能對它設定任何條件。

> [!IMPORTANT]
> **DeviceUseTrigger** 和 **DeviceServicingTrigger** 無法與同處理序背景工作搭配使用。

部分重要裝置操作 (例如長時間執行的韌體更新) 無法使用 [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337) 來執行。 這類操作只能在電腦上執行，而且只能由使用 [**DeviceServicingTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297315) 且具有特殊權限的 App 來執行。 「*具有特殊權限的 App*」是裝置製造商授權執行這些操作的 App。 裝置中繼資料可用來指定要將哪個 App (如果有的話) 指定為裝置的具有特殊權限的 App。 如需詳細資訊，請參閱[裝置同步處理和 Microsoft Store 裝置應用程式的更新](https://go.microsoft.com/fwlink/p/?LinkId=306619)

## <a name="managing-background-tasks"></a>管理背景工作

背景工作可使用事件與本機存放裝置向您的 app 報告進度、完成與取消。 您的 app 也可抓取由背景工作擲回的例外狀況，並在 app 更新期間管理背景工作登錄。 如需詳細資訊，請參閱：

[處理已取消的背景工作](handle-a-cancelled-background-task.md)  
[監視背景工作的進度和完成](monitor-background-task-progress-and-completion.md)

在 App 啟動期間檢查背景工作註冊。 確定 BackgroundTaskBuilder.AllTasks 中有 App 未分組的背景工作。 重新註冊不在其中的背景工作。 取消註冊任何不再需要的工作。 這可確保每次啟動 App 時，所有背景工作註冊都是最新的。

## <a name="related-topics"></a>相關主題

**Windows 10 中的多工的概念性指引**

* [啟動、 繼續及多工作業](index.md)

**相關的背景工作指引**

* [背景工作的指導方針](guidelines-for-background-tasks.md)
* [存取感應器與裝置，從背景工作](access-sensors-and-devices-from-a-background-task.md)
* [建立並註冊同處理序背景工作](create-and-register-an-inproc-background-task.md)
* [建立並註冊跨處理序背景工作](create-and-register-a-background-task.md)
* [將跨處理序背景工作轉換成同處理序背景工作](convert-out-of-process-background-task.md)
* [偵錯背景工作](debug-a-background-task.md)
* [宣告應用程式資訊清單中的背景工作](declare-background-tasks-in-the-application-manifest.md)
* [群組背景工作註冊](group-background-tasks.md)
* [處理已取消的背景工作](handle-a-cancelled-background-task.md)
* [如何觸發暫止、 繼續及背景事件 UWP 應用程式中的 （當偵錯）](https://docs.microsoft.com/visualstudio/debugger/how-to-trigger-suspend-resume-and-background-events-for-windows-store-apps-in-visual-studio)
* [監視背景工作的進度和完成](monitor-background-task-progress-and-completion.md)
* [在背景中播放媒體](https://msdn.microsoft.com/windows/uwp/audio-video-camera/background-audio)
* [註冊背景工作](register-a-background-task.md)
* [回應系統事件與背景工作](respond-to-system-events-with-background-tasks.md)
* [在計時器上執行背景工作](run-a-background-task-on-a-timer-.md)
* [更新您的 UWP 應用程式時，執行背景工作](run-a-background-task-during-updatetask.md)
* [無限期地在背景執行](run-in-the-background-indefinetly.md)
* [設定用於執行背景工作的條件](set-conditions-for-running-a-background-task.md)
* [觸發背景工作，從您的應用程式](trigger-background-task-from-app.md)
* [更新即時磚的背景工作](update-a-live-tile-from-a-background-task.md)
* [使用維護觸發程序](use-a-maintenance-trigger.md)
