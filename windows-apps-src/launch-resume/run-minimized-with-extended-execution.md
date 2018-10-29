---
author: TylerMSFT
description: 了解如何使用延伸執行使應用程式在最小化時仍繼續執行
title: 透過延長執行延後應用程式暫停
ms.author: twhitney
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, extended execution, minimized, ExtendedExecutionSession, background task, application lifecycle, lock screen, 延伸執行, 最小化, ExtendedExecutionSession, 背景工作, 應用程式週期, 鎖定畫面
ms.assetid: e6a6a433-5550-4a19-83be-bbc6168fe03a
ms.localizationpriority: medium
ms.openlocfilehash: 30e05259306a222a3cb18268aeb58a8380f6d4d2
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/29/2018
ms.locfileid: "5743904"
---
# <a name="postpone-app-suspension-with-extended-execution"></a>透過延長執行延後應用程式暫停

本文章將示範如何在您的應用程式暫止時使用延伸執行來延期，以便使應用程式在最小化或在鎖定畫面的情況下仍繼續執行。

當使用者最小化或離開應用程式時，它會進入暫止狀態。  它的記憶體會保留，但是不會執行程式碼。 擁有視覺化使用者介面的所有作業系統版本都是這種狀況。 如需有關您的應用程式何時暫止的詳細資訊，請參閱[應用程式生命週期](app-lifecycle.md)。 

有許多情況可能讓 App 在使用者瀏覽離開 App 時或將其最小化時繼續執行，而不是將它暫止。 例如，即使使用者瀏覽離開以使用其他 App 時，計算 App 的步驟仍需要繼續執行並追蹤步驟。 

如果 App 必須繼續執行，則作業系統必須讓 App 繼續執行，或者 App 可以要求繼續執行。 例如在背景播放音訊時，您可以依照[背景媒體播放](../audio-video-camera/background-audio.md)的步驟進行，作業系統就可以讓應用程式執行久一點。 否則，您就必須手動要求更多執行時間。 您可取得用來執行背景執行的時間長度可能會有幾分鐘，但是您必須做好準備，才能隨時處理撤銷中的工作階段。 當 App 在偵錯工具下執行時，這些應用程式週期時間限制會停用。 基於這個原因，務必未在偵錯工具下執行，或者使用 Visual Studio 中可用的週期事件時，測試延伸執行及其他工具以延後 App 暫停。 
 
建立 [ExtendedExecutionSession](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.extendedexecution.extendedexecutionsession.aspx) 來要求更多時間，以便在背景完成作業。 您建立的 **ExtendedExecutionSession** 類型是由您建立它時提供的 [ExtendedExecutionReason](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.extendedexecution.extendedexecutionreason.aspx) 所決定。 有三個 **ExtendedExecutionReason** 列舉值：**Unspecified、LocationTracking** 和 **SavingData**。 只有一個 **ExtendedExecutionSession** 可隨時要求；已核准的工作階段要求目前正在使用中時嘗試建立另一個工作階段，將會導致例外狀況 0x8007139F 從 **ExtendedExecutionSession** 建構函式擲回，以表示該群組或資源不在正確的狀態以執行所要求的作業。 請勿使用 [ExtendedExecutionForegroundSession](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.extendedexecution.foreground.extendedexecutionforegroundsession.aspx) 和 [ExtendedExecutionForegroundReason](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.extendedexecution.foreground.extendedexecutionforegroundreason.aspx)；這些列舉值需要的功能受限，不適合在 Microsoft Store 應用程式中使用。

## <a name="run-while-minimized"></a>在最小化時執行

有兩個案例可以使用延伸執行：
- 在定期前景執行期間的任何時候，當應用程式在執行中的狀態。
- 在應用程式已經在應用程式的暫停事件處理常式中收到暫停事件之後 (作業系統將把 App 移往暫停狀態)。

這兩個案例的程式碼相同，但是各個案例中應用程式的行為略為不同。 在第一個案例中，應用程式停留在執行中狀態，即使通常會觸發暫停事件 (例如，使用者瀏覽離開應用程式)，就會發生。 執行延伸生效時，應用程式將永遠不會收到暫停事件。 在處置延伸功能時，應用程式再次變成可以暫停。

在第二個案例中，如果應用程式轉換到暫停的狀態，它將會在延伸期間保持在暫停狀態。 一旦延伸過期之後，應用程式會進入暫停的狀態，而不需要進一步的通知。

當您建立 **ExtendedExecutionSession** 時使用 **ExtendedExecutionReason.Unspecified** 以要求在應用程式移至背景前，針對某些情況 (例如媒體處理、專案編譯或保持網路連線) 給予額外的執行時間。 在執行 Windows 10 傳統型版本 (家用版、專業版、企業版和教育版) 的桌面裝置上，這個做法就能使應用程式在最小化時避免進入暫止狀態。

在啟動長時間執行的作業時要求延長時間，可延後進入**暫止中**狀態，否則應用程式就會移至背景。 在桌面裝置上，使用 **ExtendedExecutionReason.Unspecified** 建立的延伸執行工作階段具有可感知電池的時間限制。 如果裝置連接到牆上電源，則延伸執行的時間長度就沒有限制。 如果裝置使用電池電源，則在背景中延伸執行的時間最多為十分鐘。

當 **\[依 App 的電池使用情況\]** 設定中選取 **\[允許應用程式執行背景工作\]** 選項時，平板電腦或膝上型電腦使用者可以犧牲電池使用時間來獲得相同的長時間執行。 (若要在膝上型電腦上中尋找此選項，請移至 **\[設定\]** > **\[系統\]** > **\[電池\]** > **\[依 App 的電池使用情況\]** (此連結位於剩餘電池電量百分比下方) > 選取應用程式 > 關閉 **\[由 Windows 管理\]** > 選取 **\[允許應用程式執行背景工作\]**。  

在所有的作業系統版本上，當裝置進入連線待命模式時，就會停止這種延伸執行工作階段。 在執行 Windows 10 行動裝置版的行動裝置上，只要螢幕持續開啟，這種延伸執行工作階段就會一直執行。 當螢幕關閉時，裝置會立即嘗試進入低電源的連線待命模式。 在桌面裝置上，如果出現鎖定畫面，此工作階段將會繼續執行。 當螢幕關閉後，裝置在一段時間內不會進入連線待命模式。 在 Xbox 作業系統版本上，除非使用者變更預設值，否則裝置會在一個小時後進入連線待命模式。

## <a name="track-the-users-location"></a>追蹤使用者的位置

當您建立 **ExtendedExecutionSession** 時，如果您的應用程式必須定期從 [GeoLocator](https://msdn.microsoft.com/library/windows/apps/windows.devices.geolocation.geolocator.aspx) 記錄位置，請指定 **ExtendedExecutionReason.LocationTracking**。 由於健身追蹤與導航用應用程式必須定期監控使用者位置，因此應使用這個功能。

只要需要，位置追蹤延伸的執行工作階段便會執行，包括當行動裝置的螢幕鎖定時。 但是，每個裝置上只能執行一個這樣的工作階段。 位置追蹤的延伸執行工作階段只能在前景中要求，而且應用程式必須處於**執行中**狀態。 如此可確保使用者會察覺此應用程式已開始延伸位置追蹤工作階段。 當應用程式在背景時，依然可以透過背景工作或應用程式服務來使用 GeoLocator，而不必要求位置追蹤的延伸執行工作階段。

## <a name="save-critical-data-locally"></a>在本機儲存重要資料

當您建立 **ExtendedExecutionSession** 時指定 **ExtendedExecutionReason.SavingData** 可儲存使用者資料，以免應用程式終止之前未儲存資料而發生資料遺失和不愉快的使用者經驗。

請勿使用這種工作階段來延長應用程式上傳或下載資料的時間。 如果您必須上傳資料，請要求[背景傳輸](https://msdn.microsoft.com/windows/uwp/networking/background-transfers)或是登錄 **MaintenanceTrigger**，在有 AC 電源時處理傳輸。 當應用程式在前景執行並處於**執行中**狀態，或是在背景執行並處於**暫止中**狀態時，可以要求 **ExtendedExecutionReason.SavingData** 延伸執行工作階段。

**暫止中**狀態是應用程式生命週期中，應用程式在終止之前可以執行工作的最後機會。 **ExtendedExecutionReason.SavingData** 是 **ExtendedExecutionSession** 的唯一類型，可在 **Suspending** 狀態中要求該類型。 請注意，當應用程式處於**暫止中**狀態時要求 **ExtendedExecutionReason.SavingData** 延伸執行工作階段可能會產生下列問題。 如果在處於**暫止中**狀態時要求延伸執行工作階段，而且使用者要求應用程式重新啟動，則可能需要很長的時間才能啟動。 這是因為延伸執行工作階段期間必須先完成，然後舊的應用程式執行個體才可以關閉，而新的應用程式執行個體才可以啟動。 為確保使用者狀態不會遺失，因此必須犧牲啟動效能時間。

## <a name="request-disposal-and-revocation"></a>要求、處置和撤銷

延伸執行工作階段有三個基本互動：要求、處置和撤銷。  以下程式碼片段會提出要求。

### <a name="request"></a>要求

```csharp
var newSession = new ExtendedExecutionSession();
newSession.Reason = ExtendedExecutionReason.Unspecified;
newSession.Revoked += SessionRevoked;
ExtendedExecutionResult result = await newSession.RequestExtensionAsync();

switch (result)
{
    case ExtendedExecutionResult.Allowed:
        DoLongRunningWork();
        break;

    default:
    case ExtendedExecutionResult.Denied:
        DoShortRunningWork();
        break;
}
```
[查看程式碼範例](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/ExtendedExecution/cs/Scenario1_UnspecifiedReason.xaml.cs#L81-L110)  

透過作業系統來呼叫 **RequestExtensionAsync** 檢查，以了解使用者是否已核准應用程式的背景活動，以及系統是否有可用的資源來啟用背景執行。 只有一個工作階段會在任何時候針對 App 核准，導致額外呼叫 **RequestExtensionAsync** 以致工作階段被拒絕。

您可以事先檢查 [BackgroundExecutionManager](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.backgroundexecutionmanager.aspx) 來判斷 [BackgroundAccessStatus](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.backgroundaccessstatus.aspx?f=255&MSPPError=-2147217396)，它是決定您的應用程式是否可在背景執行的使用者設定。 若要深入了解這些使用者設定，請參閱[背景活動和能源意識](https://blogs.windows.com/buildingapps/2016/08/01/battery-awareness-and-background-activity/#XWK8mEgWD7JHvC10.97) (英文)。

**ExtendedExecutionReason** 會表示您的應用程式在背景執行的作業。 **Description** 字串是一般人看得懂的字串，說明您的 App 必須執行此作業的原因。 這個字串不會向使用者顯示，但可能會在未來的 Windows 版本中提供。 需要 **Revoked** 事件處理常式，這樣當使用者或系統決定不要繼續在背景執行應用程式時，延伸執行工作階段才可以正常停止。

### <a name="revoked"></a>撤銷

如果 App 擁有作用中的延伸執行工作階段，而且系統要求停止背景活動，因為前景應用程式需要資源，則此工作階段會被撤銷。 一定要先引發 **Revoked** 事件處理常式，才能終止延伸執行工作階段期間。

當針對 **ExtendedExecutionReason.SavingData** 延伸執行工作階段引發 **Revoked** 事件時，應用程式有片刻時間可完成它原本在執行的作業並結束**暫止中**狀態。

撤銷發生的原因有許多：已達到執行時間限制、已達到背景能源配額，或是必須回收記憶體，好讓使用者在前景開啟新的應用程式。

以下是 Revoked 事件處理常式的範例：

```cs
private async void SessionRevoked(object sender, ExtendedExecutionRevokedEventArgs args)
{
    await Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
    {
        switch (args.Reason)
        {
            case ExtendedExecutionRevokedReason.Resumed:
                rootPage.NotifyUser("Extended execution revoked due to returning to foreground.", NotifyType.StatusMessage);
                break;

            case ExtendedExecutionRevokedReason.SystemPolicy:
                rootPage.NotifyUser("Extended execution revoked due to system policy.", NotifyType.StatusMessage);
                break;
        }

        EndExtendedExecution();
    });
}
```
[查看程式碼範例](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/ExtendedExecution/cs/Scenario1_UnspecifiedReason.xaml.cs#L124-L141)

### <a name="dispose"></a>處置

最後一個步驟為處置延伸執行工作階段。 您應該處置工作階段及其他任何耗用大量記憶體的資產，否則應用程式在等候工作階段關閉時所使用的能源將會不利於應用程式的能源配額。 為了盡量保留多一點的能源配額給應用程式使用，當您完成工作階段的工作時一定要處置工作階段，這樣應用程式才可以更快移入**已暫止**狀態。

您應該自行處置工作階段而非等候撤銷活動，這樣可減少應用程式的能源配額使用量。 這代表在未來的工作階段中，您的應用程式可以在背景執行久一點，因為您將有更多的能源配額。 您必須維持 **ExtendedExecutionSession** 物件的參照，直到作業結束為止，這樣才可以呼叫它的 **Dispose** 方法。

處置延伸執行工作階段的程式碼片段如下：

```cs
void ClearExtendedExecution(ExtendedExecutionSession session)
{
    if (session != null)
    {
        session.Revoked -= SessionRevoked;
        session.Dispose();
        session = null;
    }
}
```
[查看程式碼範例](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/ExtendedExecution/cs/Scenario1_UnspecifiedReason.xaml.cs#L49-L63)

應用程式一次只能有一個 **ExtendedExecutionSession** 處於作用中狀態。 許多應用程式使用非同步工作來完成複雜的作業，而這些作業必須存取存放裝置、網路或網路服務等資源。 如果某項作業要求完成多個非同步工作，則必須考量每一項工作的狀態，然後才能處置 **ExtendedExecutionSession** 並允許暫止應用程式。 參照必須算出依然在執行中而且未處置工作階段的數量，直到該值變成零為止。

以下為在延伸執行工作階段期間用於管理多個工作的範例程式碼： 如需有關如何在您的 App 中使用這個的詳細資訊，請參閱下列連結的程式碼範例：

```cs
static class ExtendedExecutionHelper
{
    private static ExtendedExecutionSession session = null;
    private static int taskCount = 0;

    public static bool IsRunning
    {
        get
        {
            if (session != null)
            {
                return true;
            }
            else
            {
                return false;
            }
        }
    }

    public static async Task<ExtendedExecutionResult> RequestSessionAsync(ExtendedExecutionReason reason, TypedEventHandler<object, ExtendedExecutionRevokedEventArgs> revoked, String description)
    {
        // The previous Extended Execution must be closed before a new one can be requested.       
        ClearSession();

        var newSession = new ExtendedExecutionSession();
        newSession.Reason = reason;
        newSession.Description = description;
        newSession.Revoked += SessionRevoked;

        // Add a revoked handler provided by the app in order to clean up an operation that had to be halted prematurely
        if(revoked != null)
        {
            newSession.Revoked += revoked;
        }

        ExtendedExecutionResult result = await newSession.RequestExtensionAsync();

        switch (result)
        {
            case ExtendedExecutionResult.Allowed:
                session = newSession;
                break;
            default:
            case ExtendedExecutionResult.Denied:
                newSession.Dispose();
                break;
        }
        return result;
    }

    public static void ClearSession()
    {
        if (session != null)
        {
            session.Dispose();
            session = null;
        }

        taskCount = 0;
    }

    public static Deferral GetExecutionDeferral()
    {
        if (session == null)
        {
            throw new InvalidOperationException("No extended execution session is active");
        }

        taskCount++;
        return new Deferral(OnTaskCompleted);
    }

    private static void OnTaskCompleted()
    {
        if (taskCount > 0)
        {
            taskCount--;
        }
        
        //If there are no more running tasks than end the extended lifetime by clearing the session
        if (taskCount == 0 && session != null)
        {
            ClearSession();
        }
    }

    private static void SessionRevoked(object sender, ExtendedExecutionRevokedEventArgs args)
    {
        //The session has been prematurely revoked due to system constraints, ensure the session is disposed
        if (session != null)
        {
            session.Dispose();
            session = null;
        }
        
        taskCount = 0;
    }
}
```
[參閱程式碼範例](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/ExtendedExecution/cs/Scenario4_MultipleTasks.xaml.cs)

## <a name="ensure-that-your-app-uses-resources-well"></a>確認應用程式正常使用資源

調整應用程式的記憶體與能源使用量很重要，這樣一來當您的應用程式不再是前景應用程式時，作業系統仍將允許它繼續執行。 使用[記憶體管理 API](https://msdn.microsoft.com/library/windows/apps/windows.system.memorymanager.aspx) 來了解您的應用程式正在使用多少記憶體。 您的應用程式使用的記憶體越多，當有另一個應用程式在前景時，作業系統就越難讓您的應用程式繼續執行。 最終是由使用者控制您的應用程式可執行的所有背景活動，而且他也能看到您的應用程式對電池使用量的影響。

使用 [BackgroundExecutionManager.RequestAccessAsync](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.backgroundexecutionmanager.aspx) 可判斷使用者是否已決定限制您的應用程式的背景活動。 請留意您的電池使用量，並且只在需要完成使用者想要的動作時才在背景執行。

## <a name="see-also"></a>另請參閱

[延伸執行範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ExtendedExecution)  
[應用程式生命週期](https://msdn.microsoft.com/windows/uwp/launch-resume/app-lifecycle)  
[應用程式週期 - 利用背景工作與延伸執行使 App 繼續運作](https://msdn.microsoft.com/en-us/magazine/mt590969.aspx)
[背景記憶體管理](https://msdn.microsoft.com/windows/uwp/launch-resume/reduce-memory-usage)  
[背景傳輸](https://msdn.microsoft.com/windows/uwp/networking/background-transfers)  
[電池感知和背景活動](https://blogs.windows.com/buildingapps/2016/08/01/battery-awareness-and-background-activity/#I2bkQ6861TRpbRjr.97)  
[MemoryManager 類別](https://msdn.microsoft.com/library/windows/apps/windows.system.memorymanager.aspx)  
[在背景播放媒體](https://msdn.microsoft.com/windows/uwp/audio-video-camera/background-audio)  