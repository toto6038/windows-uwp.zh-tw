---
author: drewbatgit
ms.assetid: 
description: "本文說明當您的 app 在背景執行時如何播放媒體。"
title: "在背景播放媒體"
translationtype: Human Translation
ms.sourcegitcommit: c8cbc538e0979f48b657197d59cb94a90bc61210
ms.openlocfilehash: a477827553ac1780ac625deeee08d84ab638d4c2

---

# 在背景播放媒體
本文說明如何設定您的 app，當 app 從前景移至背景時，該媒體仍能繼續播放。 這表示即使使用者已最小化您的 App、回到主畫面，或以其他方式從您的 App 離開之後，您的 App 仍可繼續播放音訊。 

背景音訊播放的案例包含：

-   **長時間執行的播放清單：**使用者會短暫叫用前景 app 來選取和開始播放清單，完成這些動作後，使用者預期播放清單會在背景持續播放。

-   **使用工作切換器：**使用者會短暫叫用前景 app 來開始播放音訊，然後使用工作切換器，切換到另一個開啟的 app。 使用者預期音訊會在背景持續播放。

本文中描述的背景音訊實作可讓您的 app 在所有 Windows 裝置上執行，包括行動裝置、桌上型電腦及 Xbox。

> [!NOTE]
> 本文中的程式碼是採用 UWP [背景音訊範例](http://go.microsoft.com/fwlink/p/?LinkId=800141)的程式碼。

## 一個處理程序模型的說明。
從 Windows 10 版本 1607 開始，引進了新的單一處理程序模型，可大幅簡化啟用背景音訊的處理程序。 之前，除了前景 app 之外還要求您的 app 能夠管理背景處理程序，然後在這兩個處理程序之間手動傳遞狀態變更。 在新模型中，您只需將背景音訊功能新增到您的應用程式資訊清單，而您的 app 將會在移至背景時自動繼續播放音訊。 有兩個新的應用程式週期事件 ([**EnteredBackground**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Core.CoreApplication.EnteredBackground) 和 [**LeavingBackground**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Core.CoreApplication.LeavingBackground)) 可讓您的 app 知道它進入和離開背景的時機。 當您的 app 移到轉換為背景或轉換為前景的處理程序時，系統強制執行的記憶體限制可能會變更，讓您能夠使用這些事件來檢查您目前的記憶體耗用量並釋出資源，以維持在限制之下。

藉由消除複雜的跨處理程序通訊與狀態管理，新模型可讓您藉由大幅減少程式碼，更快速地實作背景音訊。 不過，目前的回溯相容性版本仍然支援兩個處理程序的模型。 如需詳細資訊，請參閱[舊版的背景音效模型](background-audio.md)。

## 背景音訊的需求
當您的 app 處於背景時，該 app 必須符合下列需求才能播放音訊。

* 將**背景媒體播放**功能新增到您的應用程式資訊清單，如本文稍後所述。
* 如果您的 app 會停用 **MediaPlayer** 與系統媒體傳輸控制項 (SMTC) 的自動整合 (例如，將 [**CommandManager.IsEnabled**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackCommandManager.IsEnabled) 屬性設為 false)，則您必須實作與 SMTC 的手動整合，以啟用背景媒體播放。 如果您使用 **MediaPlayer** 以外的 API (例如 [**AudioGraph**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Audio.AudioGraph)) 來播放音訊，而且想要在您的 app 移至背景時繼續播放音訊，也必須手動與 SMTC 整合。 最低的 SMTC 整合需求請參閱[系統媒體傳輸控制項的手動控制項](system-media-transport-controls.md)的＜針對背景音訊使用系統媒體傳輸控制項＞一節中所述。
* 當您的 app 處於背景時，您必須維持在系統針對背景 app 所設定的記憶體使用量限制之下。 本文稍後將提供處於背景時用於管理記憶體的指導方針。

## 背景媒體播放資訊清單功能
若要啟用背景音訊，您必須將背景媒體播放功能新增到應用程式資訊清單檔案 Package.appxmanifest。 

**使用資訊清單設計工具以將功能新增到應用程式資訊清單**

1.  在 Microsoft Visual Studio 中，按兩下 [方案總管]**** 中的 **package.appxmanifest** 項目，開啟應用程式資訊清單的設計工具。
2.  選取 [功能]**** 索引標籤。
3.  選取 [背景媒體播放]**** 核取方塊。

若要藉由手動編輯應用程式資訊清單 xml 來設定功能，請先確認已在 **Package** 元素中定義 *uap3* 命名空間前置詞。 如果沒有，請新增它，如下所示。
```xml
<Package
  xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
  xmlns:mp="http://schemas.microsoft.com/appx/2014/phone/manifest"
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  IgnorableNamespaces="uap uap3 mp">
```

接下來，將 *backgroundMediaPlayback* 功能新增到 **Capabilities** 元素︰
```xml
<Capabilities>
    <uap3:Capability Name="backgroundMediaPlayback"/>
</Capabilities>
```

##處理前景與背景之間的轉換
當您的 app 從前景移到背景時，會引發 [**EnteredBackground**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Core.CoreApplication.EnteredBackground) 事件。 當您的 app 回到前景時，會引發 [**LeavingBackground**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Core.CoreApplication.LeavingBackground) 事件。 因為這些是應用程式週期事件，您應該在建立 app 時登錄這些事件的處理常式。 在預設專案範本中，這表示將它新增到 App.xaml.cs 中的 **App** 類別建構函式。 因為在背景中執行將會減少系統允許您 app 保留的記憶體資源，所以，您也應該登錄 [**AppMemoryUsageIncreased**](https://msdn.microsoft.com/library/windows/apps/Windows.System.MemoryManager.AppMemoryUsageIncreased) 和 [**AppMemoryUsageLimitChanging**](https://msdn.microsoft.com/library/windows/apps/Windows.System.MemoryManager.AppMemoryUsageLimitChanging) 事件，這將用來檢查您 app 目前的記憶體使用量與目前的限制。 下列範例將示範這些事件的處理常式。 如需 UWP app 的應用程式週期詳細資訊，請參閱 [App 週期](../\launch-resume\app-lifecycle.md)。

[!code-cs[RegisterEvents](./code/BackgroundAudio_RS1/cs/App.xaml.cs#SnippetRegisterEvents)]

建立變數以追蹤您目前是否正在背景中執行。

[!code-cs[DeclareBackgroundMode](./code/BackgroundAudio_RS1/cs/App.xaml.cs#SnippetDeclareBackgroundMode)]

引發 [**EnteredBackground**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Core.CoreApplication.EnteredBackground) 事件時，請設定追蹤變數，以指出您目前正在背景中執行。 您不應該在 **EnteredBackground** 事件中執行長時間執行的工作，因為這會在轉換到背景時導致使用者看見緩慢的轉換過程。

[!code-cs[EnteredBackground](./code/BackgroundAudio_RS1/cs/App.xaml.cs#SnippetEnteredBackground)]

當您的 app 轉換到背景時，系統會降低 app 的記憶體限制，以便確保目前的前景 app 有足夠的資源來提供立即回應的使用者體驗。 [**AppMemoryUsageLimitChanging**](https://msdn.microsoft.com/library/windows/apps/Windows.System.MemoryManager.AppMemoryUsageLimitChanging) 事件處理常式可讓您的 app 了解其分配的記憶體已經降低，並且在傳入處理常式的事件引數中提供新的限制。 比較事件引數的 [**MemoryManager.AppMemoryUsage**](https://msdn.microsoft.com/library/windows/apps/Windows.System.MemoryManager.AppMemoryUsage) 屬性 (提供您 app 目前的使用量) 與 [**NewLimit**](https://msdn.microsoft.com/library/windows/apps/Windows.System.AppMemoryUsageLimitChangingEventArgs.NewLimit) 屬性 (指定新的限制)。 如果您的記憶體使用量超過限制，就需要降低記憶體使用量。 這個範例是在協助程式方法 **ReduceMemoryUsage** 中完成此動作，此方法會在本文後續內容中加以定義。

[!code-cs[MemoryUsageLimitChanging](./code/BackgroundAudio_RS1/cs/App.xaml.cs#SnippetMemoryUsageLimitChanging)]

> [!NOTE] 
> 有些裝置設定可讓應用程式在新的記憶體限制下繼續執行，直到系統感受到資源壓力為止，而有些則不行。 特別的是在 Xbox 上，如果 app 未在將 2 &gt; &gt; &gt; 秒內將記憶體降到新限制之下，將會被暫停或終止。 這表示，您可以使用這個事件，在引發事件後的 2 秒內將資源使用量降到限制之下，以便在範圍最廣泛的裝置上提供最佳體驗。


可能的情況是，當您的 app 第一次轉換到背景時，它的記憶體使用量低於背景 app 的記憶體限制，但之後它的使用量會在後續某個時間點增加，而其開始處理限制。 處理常式 [**AppMemoryUsageIncreased**](https://msdn.microsoft.com/library/windows/apps/Windows.System.MemoryManager.AppMemoryUsageIncreased) 讓您有機會在其增加時檢查您目前的使用量，並視需要釋出記憶體。 檢查 [**AppMemoryUsageLevel**](https://msdn.microsoft.com/library/windows/apps/Windows.System.AppMemoryUsageLevel) 是否為 **High** 或 **OverLimit**，如果是，請降低記憶體使用量。 同樣地，在此範例中，這個處理程序是由協助程式方法 **ReduceMemoryUsage** 來處理。 您也可以訂閱 [**AppMemoryUsageDecreased**](https://msdn.microsoft.com/library/windows/apps/Windows.System.MemoryManager.AppMemoryUsageDecreased) 事件，檢查您的 app 是否低於限制，如果是，則您就會知道您可以配置其他資源。

[!code-cs[MemoryUsageIncreased](./code/BackgroundAudio_RS1/cs/App.xaml.cs#SnippetMemoryUsageIncreased)]

**ReduceMemoryUsage** 是一個協助程式方法，當您的 app 超過可在背景執行之 app 的使用量限制時，您可以實作此方法來釋出記憶體。 釋出記憶體的方式取決於您 app 的特性，但有一個釋出記憶體的建議方式是處置您的 UI 以及與您 app 檢視相關聯的其他資源。 首先，請確定您是在背景模式中執行，接著將 app 視窗的 [**Content**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Window.Content) 屬性設為 null。 呼叫 **GC.Collect**，以指示系統立即回收釋出的出記憶體。

[!code-cs[UnloadViewContent](./code/BackgroundAudio_RS1/cs/App.xaml.cs#SnippetUnloadViewContent)]

收集到視窗內容時，每個 Frame 就會開始中斷連線處理程序。 如果視窗內容下的視覺物件樹狀結構中具有 Page，這些項目就會開始觸發它們的 [**Unloaded**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.FrameworkElement.Unloaded) 事件。 除非已移除對 Page 的所有參考，否則將無法從記憶體中完全清除它們。 在 **Unloaded** 回呼中，執行下列動作，以確定會快速釋放記憶體︰
* 清除並設定 Page 中的任何大型資料結構為 null。
* 取消登錄 Page 內具有回呼方法的所有事件處理常式。 請確定會在 Page 的 Loaded 事件處理常式期間登錄這些回呼。 已重新建置 UI 且已將 Page 新增到視覺物件樹狀結構時，即會引發 Loaded 事件。
* 在 Unloaded 回呼結尾呼叫 **GC.Collect**，針對您剛剛設定為 null 的任何大型資料結構快速進行記憶體回收。

[!code-cs[Unloaded](./code/BackgroundAudio_RS1/cs/MainPage.xaml.cs#SnippetUnloaded)]

在 [**LeavingBackground**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Core.CoreApplication.LeavingBackground) 事件處理常式中，您應該設定追蹤變數，以指出您的 app 已不在背景執行。 接下來，檢查目前視窗的 [**Content**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Window.Content) 是否為 null，如果您正在背景中執行，且您已處置了 app 檢視以清除記憶體，則其會是 null。 如果視窗內容為 null，請重建您的 app 檢視。 在這個範例中，視窗內容是在協助程式方法 **CreateRootFrame** 中所建立。

[!code-cs[LeavingBackground](./code/BackgroundAudio_RS1/cs/App.xaml.cs#SnippetLeavingBackground)]

**CreateRootFrame** 協助程式方法會重新建立您 app 的檢視內容。 這個方法中的程式碼與預設專案範本中提供的 [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335) 處理常式程式碼完全相同。 唯一的差異在於 **Launching** 處理常式會從 [**LaunchActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Activation.LaunchActivatedEventArgs) 的 [**PreviousExecutionState**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Activation.LaunchActivatedEventArgs.PreviousExecutionState) 屬性判斷先前的執行狀態，而 **CreateRootFrame** 方法只需取得以引數形式傳入的先前執行狀態。 若要將重複的程式碼最小化，您可以視需要重構預設的 **Launching** 事件處理常式程式碼來呼叫 **CreateRootFrame**。

[!code-cs[CreateRootFrame](./code/BackgroundAudio_RS1/cs/App.xaml.cs#SnippetCreateRootFrame)]

## 背景媒體 app 的網路可用性
所有網路感知的媒體來源 (不是從資料流或檔案建立的媒體來源) 會在擷取遠端內容時，讓網路連線保持使用中狀態，並且在不需擷取遠端內容時釋放網路連線。 [**MediaStreamSource**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.MediaStreamSource) 特別依賴應用程式使用 [**SetBufferedRange**](https://msdn.microsoft.com/library/windows/apps/dn282762)，正確地向平台報告正確的緩衝範圍。 針對整個內容完整進行緩衝處理之後，就不會再代替 app 保留網路了。

如果您需要在無法下載媒體時，於背景中進行網路呼叫，則必須將它們包裝於適當的工作中，例如 [**ApplicationTrigger**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Background.ApplicationTrigger)、[**MaintenanceTrigger**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Background.MaintenanceTrigger) 或 [**TimeTrigger**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Background.TimeTrigger)。 如需詳細資訊，請參閱[使用背景工作支援 app](https://msdn.microsoft.com/en-us/windows/uwp/launch-resume/support-your-app-with-background-tasks)。

## 相關主題
* [媒體播放](media-playback.md)
* [使用 MediaPlayer 播放音訊和視訊](play-audio-and-video-with-mediaplayer.md)
* [與系統媒體傳輸控制項整合](integrate-with-systemmediatransportcontrols.md)
* [背景音訊範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundMediaPlayback)

 

 







<!--HONumber=Aug16_HO3-->


