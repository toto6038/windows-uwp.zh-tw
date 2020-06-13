---
title: Windows 10 UWP App 週期
description: 本主題描述 Windows 10 通用 Windows 平台 (UWP) app 從啟用到關閉為止的整個週期。
keywords: app 週期暫停繼續啟動啟用
ms.assetid: 6C469E77-F1E3-4859-A27B-C326F9616D10
ms.date: 01/23/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 0098e3d0ab31c8a3756ec7d4bf05844ace95d555
ms.sourcegitcommit: 90fe7a9a5bfa7299ad1b78bbef289850dfbf857d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/13/2020
ms.locfileid: "84756564"
---
# <a name="windows-10-universal-windows-platform-uwp-app-lifecycle"></a>Windows 10 通用 Windows 平台 (UWP) app 週期


本主題描述通用 Windows 平台 (UWP) 應用程式從啟動到關閉為止的整個週期。

## <a name="a-little-history"></a>歷史概述

Windows 8 推出前，App 的週期很簡單。 Win32 與.NET app 不是在執行中，就是並未執行。 當使用者將它們縮至最小或切換到其他 App 時，它們會繼續執行。 這原本不成問題，直到可攜式裝置和電源管理變得越來越重要。

Windows 8 引進的新應用程式模型提供 UWP 應用程式。 增加一種新的高階暫停狀態。 UWP app 會在使用者將其縮至最小或切換到其他應用程式時，很快暫停。 這表示 app 的執行緒會停止，而且除非作業系統需要回收資源，否則會將 app 保留在記憶體中。 當使用者切換回 app 時，它會快速還原到正在執行的狀態。

當 app 在背景時，需要繼續執行的的 app 會有各種不同的形態，像是[背景工作](support-your-app-with-background-tasks.md)、[延伸執行](https://docs.microsoft.com/uwp/api/windows.applicationmodel.extendedexecution)及活動贊助執行 (例如，允許 app 繼續[在背景播放媒體](https://docs.microsoft.com/windows/uwp/audio-video-camera/background-audio) 的 **BackgroundMediaEnabled** 功能)。 此外，即使您的 app 已遭到暫停或甚至終止時，背景傳輸作業仍會繼續執行。 如需詳細資訊，請參閱[如何下載檔案](https://docs.microsoft.com/previous-versions/windows/apps/jj152726(v=win.10))。

根據預設，不在前景的 app 會暫停，藉以產生省電效果，並讓目前在前景 app 有更多的資源可用。

對於身為開發人員的您而言，因為作業系統可能會選擇終止暫停的 app 以釋出資源，所以暫停的狀態會增加新的需求。 工具列中仍會顯示終止的 app。 因為使用者不會注意到系統已將 app 關閉，所以當使用者按一下 app 時，app 必須將其還原至終止之前的狀態。 他們會認為 app 始終在背景中等待使用者做別的事，並預期 app 會處於他們離開時的相同狀態。 在本主題中，我們將著眼於如何完成這個動作。

Windows 10 版本1607引進兩個以上的應用程式模型狀態：**在前景中**執行並**在背景中運作**。 我們也將在下面各節研究一下這些新狀態。

## <a name="app-execution-state"></a>App 執行狀態

這個圖例表示一開始在 Windows 10 (版本 1607) 中可能的應用程式模型狀態。 讓我們逐步解說典型的 UWP 應用程式週期。

![狀態圖例，顯示應用程式執行狀態之間的轉換](images/updated-lifecycle.png)

應用程式會在啟動或啟用時，進入背景執行狀態時。 如果應用程式因為前景應用程式啟動，而必須移至前景，則應用程式會取得 [**LeavingBackground**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplication.leavingbackground) 事件。

雖然「啟動」與「啟用」這兩個詞彙看起來很類似，但指的是作業系統讓應用程式開啟的不同方式。 讓我們先來看看啟動 app。

## <a name="app-launch"></a>App 啟動

App 啟動時，會呼叫 [**OnLaunched**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onlaunched) 方法。 傳遞所提供的 [**LaunchActivatedEventArgs**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Activation.LaunchActivatedEventArgs) 參數，一起傳遞到 app 的其他項目，還有引數、磚 (啟動該應用程式) 的識別碼，以及先前的 app 狀態。

從會傳回 [ApplicationExecutionState](https://docs.microsoft.com/uwp/api/windows.applicationmodel.activation.applicationexecutionstate) 的 [LaunchActivatedEventArgs.PreviousExecutionState](https://docs.microsoft.com/uwp/api/windows.applicationmodel.activation.launchactivatedeventargs.previousexecutionstate)，取得 app 先前的狀態。 其值和根據該狀態採取的適當動作，如下所示︰

| ApplicationExecutionState | 說明 | 要採取的動作 |
|-------|-------------|----------------|
| **未執行** | 自使用者上次重新開機或登入之後，都未曾啟動的 app 可能會處於這個狀態。 如果正在執行卻發生損毀，或因為使用者稍早即關閉，也會處於這種狀態。| 如同第一次在目前的使用者工作階段中執行一般，將 app 初始化。 |
|**暫止** | 使用者將 app 最小化或切換到其他 app，卻未在數秒內返回。 | App 暫停時，其狀態仍留在記憶體中。 您只需要重新取得應用程式暫停時釋出的任何檔案控制代碼或其他資源。 |
| **解雇** | App 先前遭到暫停，但之後卻在某個時間點因為系統需要回收記憶體而遭到關閉。 | 還原到使用者離開 app 時的狀態。|
|**ClosedByUser** | 使用者在平板電腦模式中，利用關閉手勢或 Alt+F4 關閉 app。 當使用者關閉 app 時，其會遭到暫停，然後才予以終止。 | 因為 app 基本上都是經由相同步驟進入 Terminated 狀態，所以請採用和 Terminated 狀態相同的方式來處理。|
|**執行中** | 當使用者嘗試再次啟動時，app 早已開啟。 | 不執行任何動作。 請注意，並不會啟動另一個 app 執行個體。 只會啟用已在執行中的執行個體。 |

**注意：**  *目前的使用者工作階段*是以 Windows 登入為基礎。 只要目前的使用者沒有登出、關機，或重新啟動 Windows，目前的使用者工作階段就會跨事件 (例如鎖定畫面驗證、切換使用者等等) 持續存在。 

但有一個重要的情況需要注意，如果裝置有足夠的資源，作業系統會預先啟動經常使用且已選擇加入該行為的 app，以讓回應性達到最佳。 預先啟動的 app 會在背景啟動，隨後迅速暫停，以便使用者切換至該 app 時，以比啟動 app 更快的速度繼續。

由於會預先啟動，所以會由系統初始化 app 的 **OnLaunched()** 方法，而不是使用者。 因為 app 是在背景預先啟動，所以您需要採取的動作可能和 **OnLaunched()** 不同。 例如，如果您的 app 會在啟動時開始播放音樂，因為已在背景預先載入 app，所以並不會知道其來自何處。 在背景預先啟動 app 之後，隨即會呼叫 **Application.Suspending**。 然後會在使用者實際啟動 app 時，叫用繼續事件以及 **OnLaunched()** 方法。 如需有關如何處理預先啟動案例的詳細資訊，請參閱[處理 app 預先啟動](handle-app-prelaunch.md)。 只有選擇加入的 app 才會預先啟動。

Windows 會在 app 啟動時顯示啟動顯示畫面。 如果要設定啟動顯示畫面，請參閱[新增啟動顯示畫面](https://docs.microsoft.com/previous-versions/windows/apps/hh465331(v=win.10))。

顯示啟動顯示畫面時，app 應登錄事件處理常式，並設定初始頁面所需的任何自訂 UI。 查看這些工作是否在應用程式的建構函式中執行，而且 **OnLaunched()** 會在數秒 內完成，否則系統可能會認為 app 已停止回應而予以終止。 如果 app 必須從網路取得資料，或必須從磁碟抓取大量資料，那麼您應該在啟動以外的時間完成這些動作。 App 可以使用自己的自訂載入 UI 或是延長式啟動顯示畫面，同時等待長時間執行的操作完成。 如需詳細資訊，請參閱[延長顯示啟動顯示畫面](create-a-customized-splash-screen.md)和[啟動顯示畫面範例](https://github.com/microsoft/Windows-universal-samples/tree/master/Samples/SplashScreen)。

App 完成啟動之後會進入 **Running** 狀態，啟動顯示畫面隨之消失，並會清除所有啟動顯示畫面資源和物件。

## <a name="app-activation"></a>App 啟用

和由使用者啟動相反，app 也可以由系統啟用。 App 可由協定啟用，例如分享協定。 或加以啟用來處理自訂 URI 通訊協定，或已登錄要處理該類副檔名的檔案。 如需可用以啟用 app 的方式清單，請參閱[**ActivationKind**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Activation.ActivationKind)。

[**Windows.UI.Xaml.Application**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Application) 類別定義您可以覆寫以處理各種不同 app 啟用方式的方法。
[**OnActivated**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onactivated) 可以處理所有可能的啟用類型。 不過，我們更常使用特定的方法來處理最常見的啟用類型，並且對較不常用的啟用類型使用 **OnActivated** 做為後援方法。 以下是其他的特定啟用方法：

[**OnCachedFileUpdaterActivated**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.oncachedfileupdateractivated)  
[**OnFileActivated**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onfileactivated)  
[**OnFileOpenPickerActivated**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onfileopenpickeractivated)  [**OnFileSavePickerActivated**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onfilesavepickeractivated)  
[**OnSearchActivated**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onsearchactivated)  
[**OnShareTargetActivated**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onsharetargetactivated)

這些方法的事件資料包括上面見到的相同 [**PreviousExecutionState**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.activation.iactivatedeventargs.previousexecutionstate) 屬性，可讓您知道應用程式啟用之前的狀態。 解譯狀態以及您同樣應採取的方式，如上面的 [App 啟動](#app-launch)一節中所述。

**注意**  如果您使用電腦的系統管理員帳戶登入，則無法啟用 UWP 應用程式。

## <a name="running-in-the-background"></a>在背景執行 ##

從 Windows 10 版本 1607 開始，app 可以在與 app 本身相同的處理序內執行背景工作。 如需深入瞭解，請參閱 [Background activity with the Single Process Model (單一處理序模型的背景活動)](https://blogs.windows.com/buildingapps/2016/06/07/background-activity-with-the-single-process-model/#tMmI7wUuYu5CEeRm.99)。 我們將不會在本文中談及同處理序背景處理，但這對 app 週期的影響是新增兩個和 app 在背景時有關的新事件。 分別是︰[**EnteredBackground**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplication.enteredbackground) 和 [**LeavingBackground**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplication.leavingbackground)。

這些事件也會反映出使用者是否能看到 app 的 UI。

在背景執行是應用程式已啟動、啟用，或繼續執行的預設狀態。 處於這個狀態，尚無法看到您的應用程式 UI。

## <a name="running-in-the-foreground"></a>在前景執行 ##

在前景執行表示可以見到 app 的 UI。

在可看到您的應用程式的 UI 之前，以及在進入前景執行之前，都會觸發 **LeavingBackground** 事件。 還有當使用者切換回到您的 app 時也會觸發。

之前，載入 UI 資產的最佳位置是在 **Activated** 或 **Resuming** 事件處理常式中。 現在 **LeavingBackground** 才是驗證 UI 是否準備好的最佳位置。

請務必檢查視覺資產此時是否已準備好，因為在讓使用者看見您的應用程式之前，這是最後一個可執行工作的機會。 因為會影響使用者體驗的啟動和繼續執行時間，所以在這個事件處理常式中運作的所有 UI 應會很快完成。 **LeavingBackground** 會是確定第一個 UI 畫面準備好的時機。 接著，應以非同步的方式處理長時間執行的儲存空間或網路呼叫，以讓事件處理常式傳回。

當使用者切換到其他應用程式時，您的 app 會重新進入在背景執行的狀態。

## <a name="reentering-the-background-state"></a>重新進入背景狀態

**EnteredBackground** 事件指出 app 已不會再出現在前景。 桌面會在 app 縮到最小時觸發 **EnteredBackground**，而手機會在切換到主畫面或另一個應用程式時觸發。

### <a name="reduce-your-apps-memory-usage"></a>減少 app 的記憶體使用量

由於使用者已看不見 app，最好於此停止 UI 轉譯工作和動畫。 您可以使用 **LeavingBackground**，再次啟動該工作。

如果您要在背景執行工作，請在此處準備。 最好檢查 [MemoryManager.AppMemoryUsageLevel](https://docs.microsoft.com/uwp/api/windows.system.memorymanager.appmemoryusagelevel)，並視需要減少 app 在背景中執行時的記憶體使用量，避免 app 遭到系統終止以釋出資源。

如需詳細資訊，請參閱[減少當 app 移至背景狀態時的記憶體使用量](reduce-memory-usage.md)。

### <a name="save-your-state"></a>儲存您的狀態

暫停的事件處理常式會是儲存 app 狀態的最佳位置。 如果您正在背景執行工作 (例如，播放音訊、使用延伸執行工作階段或同處理序背景工作)，那麼透過 **EnteredBackground** 事件處理常式以非同步方式儲存您的資料，也是很好的做法。 這是因為 app 可能會在背景中以低優先順序執行時遭到終止。 此外，app 不會在上述情況中進入暫停狀態，因此您的資料也將遺失。

在背景活動開始之前，先在 **EnteredBackground** 事件處理常式中儲存資料，可確保使用者將 app 帶回到前景時獲得良好的使用者經驗。 您可以使用應用程式資料 API 儲存資料與設定。 如需詳細資訊，請參閱[儲存及擷取設定和其他應用程式資料](https://docs.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data)。

儲存資料之後，如果您超過記憶體使用量限制，可以將資料從記憶體釋出，稍後重新載入即可。 釋放背景活動所需的記憶體，以供資產使用。

請注意，如果您的 app 有進行中的背景活動，其會不進入暫停狀態，直接從在背景執行的狀態變成在前景執行的狀態。

### <a name="asynchronous-work-and-deferrals"></a>非同步工作和延遲

如果您在處理常式內進行非同步呼叫，控制權會立即從該非同步呼叫交回。 這表示執行可隨後從事件處理常式傳回，即使非同步呼叫尚未完成，app 也會移至下一個狀態。 針對會傳送給事件處理常式的 [**EnteredBackgroundEventArgs**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel?redirectedfrom=MSDN) 物件使用 [**GetDeferral**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.suspendingoperation.getdeferral) 方法以延遲暫停，一直到您針對傳回的 [**Windows.Foundation.Deferral**](https://docs.microsoft.com/uwp/api/windows.foundation.deferral) 物件呼叫 [**Complete**](https://docs.microsoft.com/uwp/api/windows.foundation.deferral.complete) 方法之後。

延遲不會提高在應用程式終止之前所需執行的程式碼數量。 只會延遲終止，直到呼叫延遲的 *Complete* 方法或期限到期 (*視何者先發生*) 為止。

如果您需要更多時間來儲存狀態，請調查在 app 進入背景狀態之前的階段儲存狀態的做法，這樣要在 **EnteredBackground** 事件處理常式中儲存者會較少。 或者您可以要求 [ExtendedExecutionSession](https://msdn.microsoft.com/magazine/mt590969.aspx)，以取得更多的時間。 不過並不保證會准您所請，因此最好還是想辦法讓儲存狀態所需的時間量縮至最短。

## <a name="app-suspend"></a>App 暫停

當使用者將 app 縮至最小時，Windows 會先等候幾秒鐘，看使用者是否會切換回 app。 如果沒有在這個時間範圍內切換回來，也沒有任何延伸執行、背景工作或活動贊助執行在使用中，Windows 就會暫停該 app。 當鎖定畫面出現時，只要該 app 中沒有延伸執行工作階段等在使用中，也同樣會暫停 app。

當 app 暫停時，其會叫用 [**Application.Suspending**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.suspending) 事件。 Visual Studio 的 UWP 專案範本為此事件在 **App.xaml.cs** 中提供的處理常式稱為 **OnSuspending**。 在 Windows 10 (版本 1607) 之前，您會在此處放置儲存狀態的程式碼。 現在建議您在進入背景狀態之前儲存狀態，如上所述。

您應該釋放獨占資源及檔案控制代碼，這樣當您的 app 暫停時，其他 app 仍然可以使用它們。 獨占資源的範例包括相機、I/O 裝置、外部裝置及網路資源。 明確釋放獨占資源及檔案控制代碼，有助於確保當您的 app 暫停時，其他 app 仍然可以使用它們。 當應用程式繼續執行時，應要重新取得獨占資源和檔案控制代碼。

### <a name="be-aware-of-the-deadline"></a>注意期限

為了確保裝置運作快速且有回應，因此在暫停事件處理常式中執行程式碼的時間長度會有限制。 每部裝置各有不同，您可以使用會呼叫期限的 [**SuspendingOperation**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.SuspendingOperation) 物件屬性來找出。

如同使用 **EnteredBackground** 事件處理常式，如果您從處理常式進行非同步呼叫，控制權會立即從該非同步呼叫交回。 這表示執行可隨後從事件處理常式傳回，即使非同步呼叫尚未完成，app 也會移至暫停狀態。 針對 [**SuspendingOperation**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.SuspendingOperation) 物件 (透過事件引數提供) 使用 [**GetDeferral**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.suspendingoperation.getdeferral) 方法，來延遲進入暫停狀態，直到您針對傳回的 [**SuspendingDeferral**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.SuspendingDeferral) 物件呼叫 [**Complete**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.suspendingdeferral.complete) 方法為止。

如果您需要更多時間，可以要求 [ExtendedExecutionSession](https://msdn.microsoft.com/magazine/mt590969.aspx)。 不過並不保證會准您所請，因此最好還是想辦法讓 **Suspended** 事件處理常式中所需的時間量縮至最短。

### <a name="app-terminate"></a>App 終止

當 app 暫停時，系統會嘗試讓 app 及其資料保留在記憶體中。 不過，如果系統沒有資源可將 app 保存在記憶體中，便會終止您的 app。 App 不會收到它們遭到終止的通知，因此，您唯一可以儲存 app 資料的機會是在 **OnSuspension** 事件處理常式中，或從 **EnteredBackground** 處理常式以非同步方式儲存。

當 app 判斷它在遭到終止後又再度啟用時，應該會載入所儲存的應用程式資料，以讓 app 處於和終止之前相同的狀態。 當使用者切換回遭到終止的暫停 app 時，app 應該在其 [**OnLaunched**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onlaunched) 方法中還原自己的 app 資料。 系統不會在 app 終止時提供通知，所以 app 必須在暫停之前儲存應用程式資料並釋放獨占資源及檔案控制代碼，並在終止狀態結束後再次啟用時還原這些項目。

**有關使用 Visual Studio 進行調試的注意事項：** Visual Studio 可防止 Windows 暫停附加至偵錯工具的應用程式。 這是為了讓使用者在 app 執行時可以檢視 Visual Studio 偵錯 UI。 當您正在對某個 app 偵錯時，您可以使用 Visual Studio 傳送一個暫停事件給該 app。 請確定正在顯示 [**調試位置**] 工具列，然後按一下 [**暫**止] 圖示。

## <a name="app-resume"></a>app 繼續執行

當使用者切換到暫停的 app，或當裝置脫離電源不足的狀態時，使用中的正好是該 app，暫停的 app 就可以繼續執行。

當應用程式從「**暫停**」狀態繼續時，它會進入**以背景**狀態執行，而系統會將應用程式從中斷的地方還原，使其向使用者顯示，就好像它已同時執行。 儲存在記憶體中的 app 資料都不會遺失。 因此，大部分應用程式繼續執行時都不需要還原狀態，但是都應該重新取得在暫停時所釋出的任何檔案或裝置控制代碼，並將 app 暫停時明確釋出的任何狀態還原。

您的 app 可能會暫停長達數小時或數天。 如果 app 的內容或網路連線已過時，應要在 app 繼續執行時重新整理。 如果 app 已登錄 [**Application.Resuming**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.resuming) 事件的事件處理常式，當 app 從 **Suspended** 狀態繼續執行時，就會呼叫這個事件處理常式。 您可以在這個事件處理常式內重新整理 app 的內容和資料。

如果是為了加入應用程式協定或延伸，而將暫停的應用程式啟用，它會先收到 **Resuming** 事件，再收到 **Activated** 事件。

如果暫停的應用程式已經終止，就不會有 **繼續** 事件，而改以 **Terminated** 的 **ApplicationExecutionState** 來呼叫 **OnLaunched()**。 因為您已在 app 暫停時儲存狀態，所以您可以在 **OnLaunched()** 期間還原該狀態，如此使用者會以為 app 一直維持在使用者切換到其他 app 時的狀態。

當 app 遭到暫停時，它不會接收到原先登錄要接收的任何網路事件。 這些網路事件不會排入佇列，但是會遺失。 因此，您的 app 在繼續時必須測試網路狀態。

**注意**   因為繼續[**事件不**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.resuming)是從 UI 執行緒引發，所以如果繼續處理常式中的程式碼與您的 ui 通訊，就必須使用發送器。 如需如何進行的程式碼範例，請參閱[從背景執行緒更新 UI 執行緒](https://github.com/Microsoft/Windows-task-snippets/blob/master/tasks/UI-thread-access-from-background-thread.md)。

如需一般指導方針，請參閱 [App 暫停和繼續執行的指導方針](https://docs.microsoft.com/windows/uwp/launch-resume/index)。

## <a name="app-close"></a>App 關閉

使用者通常不需要關閉 app，交由 Windows 管理即可。 不過，使用者可以在 Windows Phone 上，選擇使用關閉手勢，或按 Alt+F4 或使用工作切換器，來關閉 app。

沒有事件可指出使用者已關閉 app。 由使用者關閉 app 時，會先予以暫停，讓您有機會儲存其狀態。 在 Windows 8.1 和更新版本中，使用者關閉 app 之後，只會從畫面和切換清單中移除該 app，但不會明確終止 app。

**使用者已關閉的行為：**   如果您的應用程式在使用者關閉時需要執行其他動作，而不是由 Windows 關閉，您可以使用啟用事件處理常式來判斷應用程式是否由使用者或 Windows 終止。 請參閱 [**ApplicationExecutionState**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Activation.ApplicationExecutionState) 列舉參考資料中有關 **ClosedByUser** 與 **Terminated** 狀態的描述。

建議您除非絕對有必要，否則不要讓 app 以程式設計的方式自行關閉。 例如，如果 app 偵測到記憶體流失，就可以自行關閉以保護使用者個人資料的安全。

## <a name="app-crash"></a>App 毀損

系統毀損經驗的設計旨在讓使用者儘快返回原先正在進行的作業。 您不應該提供警告對話方塊或其他通知，因為這會造成使用者的延遲。

如果您的 app 毀損、停止回應或產生例外狀況，系統將會依據使用者的[意見反應與診斷設定](https://support.microsoft.com/help/4468236/diagnostics-feedback-and-privacy-in-windows-10-microsoft-privacy)將問題報告傳送給 Microsoft。 Microsoft 會在給您的問題報告中提供錯誤資料的子集，讓您用來改善 app。 您可以在您的儀表板中 app 的 \[品質\] 頁面上看到這些資料。

當使用者在 app 毀損之後再次啟用，其啟用事件處理常式會收到 **NotRunning** 的 [**ApplicationExecutionState**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Activation.ApplicationExecutionState) 值，而且應該顯示其初始 UI 和資料。 損毀之後，請不要依照之前針對 **Resuming** 和 **Suspended** 的方式使用 app 資料，因為該資料可能已經損毀，請參閱 [App 暫停和繼續執行的指導方針](https://docs.microsoft.com/windows/uwp/launch-resume/index)。

## <a name="app-removal"></a>應用程式移除

當使用者刪除 app 後，app 會連同所有本機資料一併移除。 移除 app 並不會影響儲存在常見位置中的使用者資料，例如文件庫或圖片庫。

## <a name="app-lifecycle-and-the-visual-studio-project-templates"></a>App 週期與 Visual Studio 專案範本

Visual Studio 專案範本中會提供與 app 週期相關的基本程式碼。 基本的 app 會處理啟動的啟用，提供一個讓您可以還原 app 資料的位置，甚至可在您新增自己的程式碼之前，顯示其主要的 UI。 如需詳細資訊，請參閱 [App 的 C#、VB 及 C++ 專案範本](https://docs.microsoft.com/previous-versions/windows/apps/hh768232(v=win.10))。

## <a name="key-application-lifecycle-apis"></a>重要的應用程式週期 API

-   [**Windows.ApplicationModel**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel) 命名空間
-   [**Windows.ApplicationModel.Activation**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Activation) 命名空間
-   [**Windows.ApplicationModel.Core**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Core) 命名空間
-   [**Windows.UI.Xaml.Application**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Application) 類別 (XAML)
-   [**Windows.UI.Xaml.Window**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Window) 類別 (XAML)

## <a name="related-topics"></a>相關主題

* [**ApplicationExecutionState**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Activation.ApplicationExecutionState)
* [App 暫停和繼續執行的指導方針](https://docs.microsoft.com/windows/uwp/launch-resume/index)
* [處理應用程式預先啟動](handle-app-prelaunch.md)
* [處理應用程式啟用](activate-an-app.md)
* [處理應用程式暫停](suspend-an-app.md)
* [處理應用程式繼續執行](resume-an-app.md)
* [Background activity with the Single Process Model (單一處理序模型的背景活動)](https://blogs.windows.com/buildingapps/2016/06/07/background-activity-with-the-single-process-model/#tMmI7wUuYu5CEeRm.99)
* [在背景播放媒體](https://docs.microsoft.com/windows/uwp/audio-video-camera/background-audio)

 

 
