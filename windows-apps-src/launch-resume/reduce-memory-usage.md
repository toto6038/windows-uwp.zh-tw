---
author: TylerMSFT
ms.assetid: 3a3ea86e-fa47-46ee-9e2e-f59644c0d1db
description: 本文說明如何在 App 移至背景時減少記憶體使用量。
title: 當應用程式移至背景狀態時減少記憶體使用量
ms.author: twhitney
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: eef1edc4e5c725756cdef788bf555f706621741d
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/05/2018
ms.locfileid: "6038732"
---
# <a name="free-memory-when-your-app-moves-to-the-background"></a>當應用程式移至背景時釋出記憶體

本文說明如何在 App 移至背景狀態時減少其記憶體使用量，讓它不會被暫停及可能被終止。

## <a name="new-background-events"></a>新的背景事件

Windows 10 版本 1607 導入兩個新的應用程式週期事件：[**EnteredBackground**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Core.CoreApplication.EnteredBackground) 和 [**LeavingBackground**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Core.CoreApplication.LeavingBackground)。 這些事件可讓您的 App 知道它何時進入和離開背景。

當您的 App 移至背景時，系統所強制執行的記憶體限制可能會變更。 請使用這些事件來檢查您目前的記憶體耗用量並釋出資源，以便維持低於限制的狀態，如此當您的 App 在背景中時，才不會被暫停及可能被終止。

### <a name="events-for-controlling-your-apps-memory-usage"></a>可控制您 App 記憶體使用量的事件

引發 [MemoryManager.AppMemoryUsageLimitChanging](https://msdn.microsoft.com/library/windows/apps/windows.system.memorymanager.appmemoryusagelimitchanging.aspx) 的時機是在 App 所能使用的總記憶體限制正要變更之前。 例如，當 App 移至背景，而 Xbox 上的記憶體限制從 1024 MB 變更為 128 MB 時。  
這是為了讓平台不暫停或終止 App，而需處理的最重要事件。

引發 [MemoryManager.AppMemoryUsageIncreased](https://msdn.microsoft.com/library/windows/apps/windows.system.memorymanager.appmemoryusageincreased.aspx) 的時機是在 App 的記憶體耗用量已增加到 [AppMemoryUsageLevel](https://msdn.microsoft.com/library/windows/apps/windows.system.appmemoryusagelevel.aspx) 列舉中較高的值時。 例如，從 **Low** 到 **Medium**。 處理此事件是選擇性的，但建議處理，因為應用程式仍然要負責保持低於限制。

引發 [MemoryManager.AppMemoryUsageDecreased](https://msdn.microsoft.com/library/windows/apps/windows.system.memorymanager.appmemoryusagedecreased.aspx) 的時機是在 App 的記憶體耗用量已降低到 **AppMemoryUsageLevel** 列舉中較低的值時。 例如，從 **High** 到 **Low**。 處理此事件是選擇性的，但這表示應用程式可能能夠視需要配置額外的記憶體。

## <a name="handle-the-transition-between-foreground-and-background"></a>處理前景與背景之間的轉換

當 App 從前景移至背景時，會引發 [**EnteredBackground**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Core.CoreApplication.EnteredBackground) 事件。 當您的 App 回到前景時，會引發 [**LeavingBackground**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Core.CoreApplication.LeavingBackground) 事件。 當已建立好您的 App 時，您可以為這些事件註冊處理常式。 在預設專案範本中，是在 App.xaml.cs 的 **App** 類別中執行此動作。

由於在背景中執行會減少系統允許您 App 保留的記憶體資源，因此您應該一併註冊 [**AppMemoryUsageIncreased**](https://msdn.microsoft.com/library/windows/apps/Windows.System.MemoryManager.AppMemoryUsageIncreased) 和 [**AppMemoryUsageLimitChanging**](https://msdn.microsoft.com/library/windows/apps/Windows.System.MemoryManager.AppMemoryUsageLimitChanging) 事件，這些事件可用來檢查您 App 目前的記憶體使用量和目前的限制。 下列範例將示範這些事件的處理常式。 如需有關 UWP app 的應用程式週期詳細資訊，請參閱 [App 週期](..//launch-resume/app-lifecycle.md)。

[!code-cs[RegisterEvents](./code/ReduceMemory/cs/App.xaml.cs#SnippetRegisterEvents)]

引發 [**EnteredBackground**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Core.CoreApplication.EnteredBackground) 事件時，請設定追蹤變數，以指出您目前正在背景中執行。 當您撰寫用來減少記憶體使用量的程式碼時，這會相當有用。

[!code-cs[EnteredBackground](./code/ReduceMemory/cs/App.xaml.cs#SnippetEnteredBackground)]

當您的 App 轉換到背景時，系統會降低 App 的記憶體限制，以確保目前的前景 App 有足夠的資源來提供立即回應的使用者體驗。

[**AppMemoryUsageLimitChanging**](https://msdn.microsoft.com/library/windows/apps/Windows.System.MemoryManager.AppMemoryUsageLimitChanging) 事件處理常式可讓您的 App 了解分配給它的記憶體已經降低，並且在傳入處理常式的事件引數中提供新的限制。 將事件引數的 [**MemoryManager.AppMemoryUsage**](https://msdn.microsoft.com/library/windows/apps/Windows.System.MemoryManager.AppMemoryUsage) 屬性 (提供您 App 目前的使用量) 與 [**NewLimit**](https://msdn.microsoft.com/library/windows/apps/Windows.System.AppMemoryUsageLimitChangingEventArgs.NewLimit) 屬性 (指定新的限制) 做比較。 如果您的記憶體使用量超過限制，就需要降低記憶體使用量。

這個範例是在協助程式方法 **ReduceMemoryUsage** 中完成此動作，此方法會在本文後續內容中加以定義。

[!code-cs[MemoryUsageLimitChanging](./code/ReduceMemory/cs/App.xaml.cs#SnippetMemoryUsageLimitChanging)]

> [!NOTE]
> 有些裝置設定可讓應用程式在超出新記憶體限制的情況下繼續執行，直到系統感受到資源壓力為止，而有些則不行。 特別的是在 Xbox 上，如果 App 沒有在 2 秒內將記憶體降到低於新限制，App 就會被暫停或終止。 這表示您可以藉由使用這個事件，在引發事件後的 2 秒內將資源使用量降到低於限制，在最大範圍的裝置上提供最佳體驗。

可能的情況是，雖然您 App 的記憶體使用量在它一開始轉換到背景時，就背景 App 而言目前低於記憶體限制，但是它可能會隨著時間增加其記憶體耗用量而開始接近限制。 [**AppMemoryUsageIncreased**](https://msdn.microsoft.com/library/windows/apps/Windows.System.MemoryManager.AppMemoryUsageIncreased) 處理常式會讓您有機會在目前使用量增加時進行檢查，並視需要釋出記憶體。

檢查 [**AppMemoryUsageLevel**](https://msdn.microsoft.com/library/windows/apps/Windows.System.AppMemoryUsageLevel) 是否為 **High** 或 **OverLimit**，如果是，請降低記憶體使用量。 在此範例中，這是由協助程式方法 **ReduceMemoryUsage** 來處理。 您也可以訂閱 [**AppMemoryUsageDecreased**](https://msdn.microsoft.com/library/windows/apps/Windows.System.MemoryManager.AppMemoryUsageDecreased) 事件，檢查您的 App 是否低於限制，如果是，您就會知道您可以配置其他資源。

[!code-cs[MemoryUsageIncreased](./code/ReduceMemory/cs/App.xaml.cs#SnippetMemoryUsageIncreased)]

**ReduceMemoryUsage** 是一個協助程式方法，當您的 App 在背景中執行而超出使用量限制時，您可以實作此方法來釋出記憶體。 釋出記憶體的方式取決於您 App 的特性，但有一個釋出記憶體的建議方式，就是處置您的 UI 及與您 App 檢視關聯的其他資源。 若要這樣做，請確定您是在背景狀態下執行，然後將 App 視窗的 [**Content**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Window.Content) 屬性設定為 `null`，並將您的 UI 事件處理常式移除註冊，以及移除任何其他您對該頁面可能有的參考。 如果無法移除 UI 事件處理常式的註冊及清除任何其他您對該頁面可能有的參考，將會導致無法釋出頁面資源。 然後呼叫 **GC.Collect** 以立即回收釋出的記憶體。 一般而言，您不用強制記憶體回收，因為系統會替您處理。 在這個特殊案例中，我們正在減少此應用程式所取用的記憶體量，因為應用程式會到背景中作業，以降低系統判定應終止應用程式以回收記憶體的可能性。

[!code-cs[UnloadViewContent](./code/ReduceMemory/cs/App.xaml.cs#SnippetUnloadViewContent)]

收集到視窗內容時，每個 Frame 就會開始其中斷連線處理程序。 如果視覺物件樹狀結構中的視窗內容下有 Page，這些項目就會開始引發它們的 Unloaded 事件。 除非已移除對 Page 的所有參考，否則將無法從記憶體中完全清除它們。 在 Unloaded 回呼中，請執行下列動作以確保會快速釋出記憶體︰
* 清除 Page 中的任何大型資料結構並將它們設定為 `null`。
* 將 Page 內具有回呼方法的所有事件處理常式移除註冊。 請確定會在 Page 的 Loaded 事件處理常式期間登錄這些回呼。 引發 Loaded 事件的時機是在已重新建構 UI，並且已將 Page 新增到視覺物件樹狀結構時。
* 在 Unloaded 回呼結尾呼叫 `GC.Collect`，以針對您剛設定為 `null` 的任何大型資料結構快速進行記憶體回收。 再次強調，您不用強制記憶體回收，因為系統會替您處理。 在這個特殊案例中，我們正在減少此應用程式所取用的記憶體量，因為應用程式會到背景中作業，以降低系統判定應終止應用程式以回收記憶體的可能性。

[!code-cs[MainPageUnloaded](./code/ReduceMemory/cs/App.xaml.cs#SnippetMainPageUnloaded)]

在 [**LeavingBackground**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Core.CoreApplication.LeavingBackground) 事件處理常式中，請設定追蹤變數 (`isInBackgroundMode`) 以指出您的應用程式已不在背景中執行。 接下來，檢查目前視窗的 [**Content**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Window.Content) 是否為 `null` -- 如果您在於背景中執行時處置了 App 檢視來清除記憶體，就會是這個值。 如果視窗內容為 `null`，請重建您的 App 檢視。 在這個範例中，視窗內容是在協助程式方法 **CreateRootFrame** 中建立。

[!code-cs[LeavingBackground](./code/ReduceMemory/cs/App.xaml.cs#SnippetLeavingBackground)]

**CreateRootFrame** 協助程式方法會重新建立您應用程式的檢視內容。 這個方法中的程式碼與預設專案範本中提供的 [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335) 處理常式程式碼幾乎完全相同。 唯一的差異在於 **Launching** 處理常式會從 [**LaunchActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Activation.LaunchActivatedEventArgs) 的 [**PreviousExecutionState**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Activation.LaunchActivatedEventArgs.PreviousExecutionState) 屬性判斷先前的執行狀態，而 **CreateRootFrame** 方法則是直接取得以引數形式傳入的先前執行狀態。 若要將重複的程式碼縮減到最少，您可以重構預設的 **Launching** 事件處理常式程式碼來呼叫 **CreateRootFrame**。

[!code-cs[CreateRootFrame](./code/ReduceMemory/cs/App.xaml.cs#SnippetCreateRootFrame)]

## <a name="guidelines"></a>指導方針

### <a name="moving-from-the-foreground-to-the-background"></a>從前景移至背景

當 App 從前景移至背景時，系統並不會代表 App 進行工作來釋出背景中不需要的資源。 例如，UI 架構會排清已快取的紋理，而視訊子系統會代表 App 釋出已配置的記憶體。 不過，App 仍然需要謹慎監視其記憶體使用量，才能避免被系統暫停或終止。

當 App 從前景移至背景時，它會先取得 **EnteredBackground** 事件，然後取得 **AppMemoryUsageLimitChanging** 事件。

- 請**務必**使用 **EnteredBackground** 事件來釋出您知道 App 在背景中執行時不需要的 UI 資源。 例如，您可以釋出一首歌曲的封面影像。
- 請**務必**使用 **AppMemoryUsageLimitChanging** 事件來確保 App 使用的記憶體低於新的背景限制。 如果不是，請務必釋出資源。 如果沒有這樣做，系統可能會根據裝置特定原則來暫停或終止您的 App。
- 如果當引發 **AppMemoryUsageLimitChanging** 事件時，您的 App 超出新的記憶體限制，請**務必**手動叫用記憶體回收行程。
- 如果當 App 在背景中執行時，您預期 App 的記憶體使用量會有變更，請**務必**使用 **AppMemoryUsageIncreased** 事件來繼續監視 App 的記憶體使用量。 如果 **AppMemoryUsageLevel** 是 **High** 或 **OverLimit**，請務必釋出資源。
- 請**考慮**在 **AppMemoryUsageLimitChanging** 事件處理常式 (而不是 **EnteredBackground** 處理常式) 中釋出 UI 資源來進行效能最佳化。 使用 **EnteredBackground/LeavingBackground** 事件處理常式中設定的布林值，來追蹤應用程式是在背景中還是前景中。 然後，在 **AppMemoryUsageLimitChanging** 事件處理常式中，如果 **AppMemoryUsage** 超出限制，而 App 在背景中 (根據布林值判斷)，您便可以釋出 UI 資源。
- 請**勿**在 **EnteredBackground** 事件中執行長時間執行的作業，因為這可能導致應用程式之間的轉換對使用者來說顯得緩慢。

### <a name="moving-from-the-background-to-the-foreground"></a>從背景移至前景

當 App 從背景移至前景時，它會先取得 **AppMemoryUsageLimitChanging** 事件，然後取得 **LeavingBackground** 事件。

- 請**務必**使用 **LeavingBackground** 事件來重新建立 App 在移至背景時已捨棄的 UI 資源。

## <a name="related-topics"></a>相關主題

* [背景媒體播放範例](http://go.microsoft.com/fwlink/p/?LinkId=800141) - 說明如何在 App 移至背景狀態時釋出記憶體。
* [診斷工具](https://blogs.msdn.microsoft.com/visualstudioalm/2015/01/16/diagnostic-tools-debugger-window-in-visual-studio-2015/) - 使用診斷工具來觀察記憶體回收事件，並驗證您的 App 以您預期的方式釋出記憶體。
