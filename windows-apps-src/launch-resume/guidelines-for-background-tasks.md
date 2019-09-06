---
title: 背景工作的指導方針
description: 確保您的應用程式符合執行背景工作的需求。
ms.assetid: 18FF1104-1F73-47E1-9C7B-E2AA036C18ED
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10，uwp，背景工作
ms.localizationpriority: medium
ms.openlocfilehash: 5f7e64dca67f0327b6366cbca072e83d4bf4be60
ms.sourcegitcommit: d38e2f31c47434cd6dbbf8fe8d01c20b98fabf02
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/06/2019
ms.locfileid: "70393571"
---
# <a name="guidelines-for-background-tasks"></a>背景工作的指導方針


確保您的應用程式符合執行背景工作的需求。

## <a name="background-task-guidance"></a>背景工作指引

在開發背景工作時，以及在發佈應用程式之前，請考慮下列指引。

如果您使用背景工作在背景播放媒體，請參閱[在背景播放媒體](https://docs.microsoft.com/windows/uwp/audio-video-camera/background-audio)，以了解有關 Windows 10 版本 1607 中讓此操作更容易進行的改進功能詳細資訊。

**同進程與跨進程的背景工作：** Windows 10 版本1607引進了同[進程背景](create-and-register-an-inproc-background-task.md)工作，可讓您在與前景應用程式相同的進程中執行背景程式碼。 決定要使用同處理序或跨處理序的背景工作時，請考慮下列因素︰

|考量 | 影響 |
|--------------|--------|
|復原能力   | 如果您的背景處理序在另一個處理序中執行，背景處理序毀損並不會關閉您的前景應用程式。 此外，如果背景活動執行超過執行時間限制，即使在您的應用程式內，也可以終止。 當前景和背景處理序不需要彼此通訊 (因為同處理序背景工作的主要優點之一，就是處理程間不需要通訊) 時，最好選擇將背景工作和前景應用程式的工作分開。 |
|簡潔    | 同處理序背景工作不需要跨處理序通訊，撰寫上也比較不複雜。  |
|可用的觸發程序 | 同進程背景工作不支援下列觸發程式：[DeviceUseTrigger](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.deviceusetrigger?f=255&MSPPError=-2147217396)、 [DeviceServicingTrigger](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.deviceservicingtrigger)和**IoTStartupTask**。 |
|VoIP | 同處理序背景工作不支援在應用程式內啟用 VoIP 背景工作。 |  

**觸發程式實例數目的限制：** 應用程式可以註冊的一些觸發程式實例有多個限制。 App 只能針對其每個執行個體註冊 [ApplicationTrigger](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.ApplicationTrigger)、[MediaProcessingTrigger](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.mediaprocessingtrigger) 和 [DeviceUseTrigger](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.deviceusetrigger?f=255&MSPPError=-2147217396) 一次。 如果 App 超過此限制，註冊將會擲回例外狀況。

**CPU 配額：** 背景工作受限於根據觸發程式類型取得的時鐘使用時間量。 大部分的觸發程序有 30 秒的實際執行使用限制，而部分觸發程序擁有最多 10 分鐘的執行時間，以完成需要大量運算資源的工作。 背景工作應該要輕量，才能延長電池使用時間並為前景應用程式提供較佳的使用者體驗。 請參閱[使用背景工作支援應用程式](support-your-app-with-background-tasks.md)，以了解套用至背景工作的資源限制。

**管理背景工作：** 您的應用程式應該會取得已註冊的背景工作清單、註冊進度和完成處理常式，並適當地處理這些事件。 您的背景工作類別應該報告進度、取消和完成。 如需詳細資訊，請參閱[處理已取消的背景工作](handle-a-cancelled-background-task.md)和[監視背景工作進度和完成](monitor-background-task-progress-and-completion.md)。

**使用[BackgroundTaskDeferral](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskDeferral)：** 如果您的背景工作類別執行非同步程式碼，請務必使用延期。 否則，當[Run](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.ibackgroundtask.run)方法傳回時，您的背景工作可能會提前終止（或在同進程背景工作的情況下， [OnBackgroundActivated](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onbackgroundactivated)方法）。 如需詳細資訊，請參閱[建立及註冊跨處理序背景工作](create-and-register-a-background-task.md)。

或者，要求一個延遲並使用 **async/await** 來完成非同步方法呼叫。 請在 **await** 方法呼叫後關閉延遲。

**更新應用程式資訊清單：** 對於跨進程執行的背景工作，請在應用程式資訊清單中宣告每個背景工作，以及用來搭配使用的觸發程式類型。 否則您的應用程式將無法在執行階段註冊背景工作。

如果您有多個背景工作，請考慮是否應該先執行相同的主機程序，或分散到不同的主機處理程序。 如果您擔心一個背景工作失敗會擔擱其他背景工作，請分敗主機處理程序。  使用資訊清單設計工具中的**資源群組**項目，將背景工作分組到不同的主機的處理程序。 

若要設定**資源群組**，請開啟 Package.appxmanifest 設計工具，選擇 **/[宣告/]** ，並新增 **/[應用程式服務/]** 宣告：

![資源群組設定](images/resourcegroup.png)

請參閱[應用程式架構參考](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-application)以取得有關資源群組設定的詳細資訊。

與前景應用程式在相同處理序中執行的背景工作，不需要在應用程式資訊清單中宣告自己。 如需在資訊清單中宣告在跨處理序中執行的背景工作的詳細資訊，請參閱[在應用程式資訊清單中宣告背景工作](declare-background-tasks-in-the-application-manifest.md)。

**準備應用程式更新：** 如果您的應用程式將會更新，請建立並註冊**ServicingComplete**背景工作（請參閱[SystemTriggerType](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SystemTriggerType)），以取消註冊繼承應用程式的背景工作，並註冊新版本的背景工作。 除了在前景內執行的內容以外，此時也很適合執行可能需要的應用程式更新。

**執行背景工作的要求：**

> **重要事項**  從 Windows 10 開始，應用程式不再需要在鎖定畫面上做為執行背景工作的先決條件。

通用 Windows 平台 (UWP) 應用程式可以在不釘選到鎖定畫面上的情況下，執行所有支援的工作類型。 不過，應用程式必須先呼叫 [**RequestAccessAsync**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccessasync)，才能登錄任何類型的背景工作。 如果使用者已明確地在裝置設定中拒絕您應用程式的背景工作權限，則這個方法會傳回 [**BackgroundAccessStatus.DeniedByUser**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundAccessStatus)。 如需使用者的背景活動與省電模式選項的詳細資訊，請參閱[最佳化背景活動](https://docs.microsoft.com/windows/uwp/debug-test-perf/optimize-background-activity)。 
## <a name="background-task-checklist"></a>背景工作檢查清單

*同時適用于同進程和跨進程背景工作*

-   將背景工作與正確的觸發程序關聯。
-   新增條件以協助確認背景工作順利執行。
-   處理背景工作進度、完成和取消。
-   於應用程式啟動期間，重新登錄背景工作。 這可確保它們在第一次啟動應用程式時進行註冊。 它也提供了一種方式，偵測使用者是否 (在事件註冊失敗時) 已停用應用程式的背景執行功能。
-   檢查背景工作登錄是否有錯誤。 在適當狀況下，嘗試以不同的參數值再次登錄背景工作。
-   針對桌上型電腦以外的所有裝置系列，如果裝置的記憶體變成不足，背景工作可能就會終止。 如果沒有顯示記憶體不足的例外狀況，或是應用程式沒有處理該狀況，背景工作將會在沒有警告也沒有引發 OnCanceled 事件的情況下終止。 這有助於確保前景應用程式的使用者體驗。 您的背景工作應該要設計成能夠處理這種情況。

*僅適用于跨進程背景工作*

-   在 Windows 執行階段元件中建立背景工作。
-   請勿顯示背景工作快顯通知、磚以及徽章更新以外的 UI。
-   在 [Run](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.ibackgroundtask.run) 方法中，針對每個非同步方法呼叫要求延遲，並在方法完成時關閉它們。 或者，搭配 **async/await** 使用單一延遲。
-   使用永久性存放裝置在背景工作和應用程式之間分享資料。
-   在應用程式資訊清單中宣告每個背景工作，以及它所使用的觸發程序類型。 確認進入點與觸發程序類型是否正確。
-   除非您要使用應該與應用程式在相同內容中執行的觸發程序 (例如 [**ControlChannelTrigger**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger))，否則請勿在資訊清單中指定 Executable 元素。

*僅適用于同進程背景工作*

- 取消工作時，請確認 `BackgroundActivated` 事件處理常式在取消發生或終止整個處理序之前結束。
-   撰寫短期背景工作。 背景工作的使用時間是限制為 30 秒時鐘時間。
-   請勿仰賴背景工作的使用者互動。

## <a name="related-topics"></a>相關主題

* [建立及註冊同處理序序背景工作](create-and-register-an-inproc-background-task.md)。
* [建立及註冊跨處理序的背景工作](create-and-register-a-background-task.md)
* [在應用程式資訊清單中宣告背景工作](declare-background-tasks-in-the-application-manifest.md)
* [在背景播放媒體](https://docs.microsoft.com/windows/uwp/audio-video-camera/background-audio)
* [處理已取消的背景工作](handle-a-cancelled-background-task.md)
* [監視背景工作進度和完成](monitor-background-task-progress-and-completion.md)
* [註冊背景工作](register-a-background-task.md)
* [使用背景工作回應系統事件](respond-to-system-events-with-background-tasks.md)
* [設定執行背景工作的條件](set-conditions-for-running-a-background-task.md)
* [從背景工作更新動態磚](update-a-live-tile-from-a-background-task.md)
* [使用維護觸發程序](use-a-maintenance-trigger.md)
* [在計時器上執行背景工作](run-a-background-task-on-a-timer-.md)
* [偵錯背景工作](debug-a-background-task.md)
* [如何在 UWP 應用程式中觸發暫止、繼續和背景事件（在進行調試時）](https://go.microsoft.com/fwlink/p/?linkid=254345)

 

 
