---
author: TylerMSFT
description: "了解如何使用延伸執行使應用程式在最小化時仍繼續執行"
title: "使用延伸執行，在最小化時執行"
ms.author: twhitney
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.assetid: e6a6a433-5550-4a19-83be-bbc6168fe03a
ms.openlocfilehash: bd9ccaa4cb87a24906c531996d4fc3f88875b060
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="run-while-minimized-with-extended-execution"></a>使用延伸執行，在最小化時執行

本文章將示範如何在您的應用程式暫止時使用延伸執行來延期，以便使應用程式在最小化時仍繼續執行。

當使用者最小化或離開應用程式時，它會進入暫止狀態。  它的記憶體會保留，但是不會執行程式碼。 擁有視覺化使用者介面的所有作業系統版本都是這種狀況。 如需有關您的應用程式何時暫止的詳細資訊，請參閱[應用程式生命週期](app-lifecycle.md)。

但有許多情況必須讓應用程式在最小化時繼續執行，而不是將它暫止。 如果應用程式必須繼續執行，則作業系統必須讓應用程式繼續執行，或者應用程式可以要求繼續執行。 例如在背景播放音訊時，您可以依照[背景媒體播放](../audio-video-camera/background-audio.md)的步驟進行，作業系統就可以讓應用程式執行久一點。 否則，您就必須手動要求更多執行時間。

建立 [ExtendedExecutionSession](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.extendedexecution.extendedexecutionsession.aspx) 來要求更多時間，以便在背景完成作業。 您建立的 **ExtendedExecutionSession** 類型是由您建立它時提供的 [ExtendedExecutionReason](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.extendedexecution.extendedexecutionreason.aspx) 所決定。 有三個 **ExtendedExecutionReason** 列舉值：**Unspecified、LocationTracking** 和 **SavingData**。

## <a name="run-while-minimized"></a>在最小化時執行

當您建立 **ExtendedExecutionSession** 時指定 **ExtendedExecutionReason.Unspecified** 以要求在應用程式移至背景前，針對某些情況 (例如媒體處理、專案編譯或保持網路連線) 給予額外的執行時間。 在執行 Windows 10 傳統型版本 (家用版、專業版、企業版和教育版) 的桌面裝置上，這個做法就能使應用程式在最小化時避免進入暫止狀態。

在啟動長時間執行的作業時要求延長時間，可延後進入**暫止中**狀態，否則應用程式就會移至背景。 在桌面裝置上，使用 **ExtendedExecutionReason.Unspecified** 建立的延伸執行工作階段具有可感知電池的時間限制。 如果裝置連接到牆上電源，則延伸執行的時間長度就沒有限制。 如果裝置使用電池電源，則在背景中延伸執行的時間最多為十分鐘。 當 **\[電池使用設定\]** 中有選取 **\[一律允許\]** 選項時，平板電腦或膝上型電腦使用者可以犧牲電池使用時間來獲得相同的長時間執行。

在所有的作業系統版本上，當裝置進入連線待命模式時，就會停止這種延伸執行工作階段。 在執行 Windows 10 行動裝置版的行動裝置上，只要螢幕持續開啟，這種延伸執行工作階段就會一直執行。 當螢幕關閉時，裝置會立即嘗試進入低電源的連線待命模式。 在桌面裝置上，如果出現鎖定畫面，此工作階段將會繼續執行。 當螢幕關閉後，裝置在一段時間內不會進入連線待命模式。 在 Xbox 作業系統版本上，除非使用者變更預設值，否則裝置會在一個小時後進入連線待命模式。

## <a name="track-the-users-location"></a>追蹤使用者的位置

當您建立 **ExtendedExecutionSession** 時，如果您的應用程式必須定期從 [GeoLocator](https://msdn.microsoft.com/library/windows/apps/windows.devices.geolocation.geolocator.aspx) 記錄位置，請指定 **ExtendedExecutionReason.LocationTracking**。 由於健身追蹤與導航用應用程式必須定期監控使用者位置，因此應使用這個功能。

位置追蹤的延伸執行工作階段可視需要持續執行。 但是，每個裝置上只能執行一個這樣的工作階段。 位置追蹤的延伸執行工作階段只能在前景中要求，而且應用程式必須處於**執行中**狀態。 如此可確保使用者會察覺此應用程式已開始延伸位置追蹤工作階段。 當應用程式在背景時，依然可以透過背景工作或應用程式服務來使用 GeoLocator，而不必要求位置追蹤的延伸執行工作階段。

## <a name="save-critical-data-locally"></a>在本機儲存重要資料

當您建立 **ExtendedExecutionSession** 時指定 **ExtendedExecutionReason.SavingData** 可儲存使用者資料，以免應用程式終止之前未儲存資料而發生資料遺失和不愉快的使用者經驗。

請勿使用這種工作階段來延長應用程式上傳或下載資料的時間。 如果您必須上傳資料，請要求[背景傳輸](https://msdn.microsoft.com/windows/uwp/networking/background-transfers)或是登錄 **MaintenanceTrigger**，在有 AC 電源時處理傳輸。 當應用程式在前景執行並處於**執行中**狀態，或是在背景執行並處於**暫止中**狀態時，可以要求 **ExtendedExecutionReason.SavingData** 延伸執行工作階段。

**暫止中**狀態是應用程式生命週期中，應用程式在終止之前可以執行工作的最後機會。 請注意，當應用程式處於**暫止中**狀態時要求 **ExtendedExecutionReason.SavingData** 延伸執行工作階段可能會產生下列問題。 如果在處於**暫止中**狀態時要求延伸執行工作階段，而且使用者要求應用程式重新啟動，則可能需要很長的時間才能啟動。 這是因為延伸執行工作階段期間必須先完成，然後舊的應用程式執行個體才可以關閉，而新的應用程式執行個體才可以啟動。 為確保使用者狀態不會遺失，因此必須犧牲啟動效能時間。

## <a name="request-disposal-and-revocation"></a>要求、處置和撤銷

延伸執行工作階段有三個基本互動：要求、處置和撤銷。  以下程式碼片段會提出要求。

### <a name="request"></a>要求

```csharp
var newSession = new ExtendedExecutionSession();
newSession.Reason = ExtendedExecutionReason.Unspecified;
newSession.Description = "Raising periodic toasts";
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

透過作業系統來呼叫 **RequestExtensionAsync** 檢查，以了解使用者是否已核准應用程式的背景活動，以及系統是否有可用的資源來啟用背景執行。

您可以事先檢查 [BackgroundExecutionManager](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.backgroundexecutionmanager.aspx) 來判斷 [BackgroundAccessStatus](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.backgroundaccessstatus.aspx?f=255&MSPPError=-2147217396)，它是決定您的應用程式是否可在背景執行的使用者設定。 若要深入了解這些使用者設定，請參閱[背景活動和能源意識](https://blogs.windows.com/buildingapps/2016/08/01/battery-awareness-and-background-activity/#XWK8mEgWD7JHvC10.97) (英文)。

**ExtendedExecutionReason** 會表示您的應用程式在背景執行的作業。 **Description** 字串是一般人看得懂的字串，說明您的應用程式必須執行此作業的原因。 需要 **Revoked** 事件處理常式，這樣當使用者或系統決定不要繼續在背景執行應用程式時，延伸執行工作階段才可以正常停止。

### <a name="revoked"></a>撤銷

如果應用程式擁有作用中的延伸執行工作階段，而且系統要求停止背景活動，則此工作階段會被撤銷。 一定要先引發 **Revoked** 事件處理常式，才能終止延伸執行工作階段期間。

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

以下為在延伸執行工作階段期間用於管理多項工作的範例程式碼：

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

    public static async Task<ExtendedExecutionResult> RequestSessionAsync(ExtendedExecutionReason reason, TypedEventHandler<object, ExtendedExecutionRevokedEventArgs> revoked)
    {
        // The previous Extended Execution must be closed before a new one can be requested.       
        ClearSession();

        var newSession = new ExtendedExecutionSession();
        newSession.Reason = ExtendedExecutionReason.Unspecified;
        newSession.Description = "Running multiple tasks";
        newSession.Revoked += SessionRevoked;

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
        else if (taskCount == 0 && session != null)
        {
            ClearSession();
        }
    }

    private static void SessionRevoked(object sender, ExtendedExecutionRevokedEventArgs args)
    {
        if (session != null)
        {
            session.Dispose();
            session = null;
        }
    }
}
```
[查看程式碼範例](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/ExtendedExecution/cs/Scenario4_MultipleTasks.xaml.cs)

## <a name="ensure-that-your-app-uses-resources-well"></a>確認應用程式正常使用資源

調整應用程式的記憶體與能源使用量很重要，這樣一來當您的應用程式不再是前景應用程式時，作業系統仍將允許它繼續執行。 使用[記憶體管理 API](https://msdn.microsoft.com/library/windows/apps/windows.system.memorymanager.aspx) 來了解您的應用程式正在使用多少記憶體。 您的應用程式使用的記憶體越多，當有另一個應用程式在前景時，作業系統就越難讓您的應用程式繼續執行。 最終是由使用者控制您的應用程式可執行的所有背景活動，而且他也能看到您的應用程式對電池使用量的影響。

使用 [BackgroundExecutionManager.RequestAccessAsync](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.backgroundexecutionmanager.aspx) 可判斷使用者是否已決定限制您的應用程式的背景活動。 請留意您的電池使用量，並且只在需要完成使用者想要的動作時才在背景執行。

## <a name="see-also"></a>另請參閱

[延伸執行範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ExtendedExecution)  
[應用程式生命週期](https://msdn.microsoft.com/windows/uwp/launch-resume/app-lifecycle)  
[背景記憶體管理](https://msdn.microsoft.com/windows/uwp/launch-resume/reduce-memory-usage)  
[背景傳輸](https://msdn.microsoft.com/windows/uwp/networking/background-transfers)  
[電池感知和背景活動](https://blogs.windows.com/buildingapps/2016/08/01/battery-awareness-and-background-activity/#I2bkQ6861TRpbRjr.97)  
[MemoryManager 類別](https://msdn.microsoft.com/library/windows/apps/windows.system.memorymanager.aspx)  
[在背景播放媒體](https://msdn.microsoft.com/windows/uwp/audio-video-camera/background-audio)  