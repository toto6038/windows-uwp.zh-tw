---
title: App 週期
description: 本主題描述通用 Windows 平台 (UWP) app 從啟用到關閉為止的整個週期。
ms.assetid: 6C469E77-F1E3-4859-A27B-C326F9616D10
---

# App 週期


\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**重要 API**

-   [**Windows.UI.Xaml.Application 類別**](https://msdn.microsoft.com/library/windows/apps/br242324)
-   [**Windows.ApplicationModel.Activation 命名空間**](https://msdn.microsoft.com/library/windows/apps/br224766)

本主題描述通用 Windows 平台 (UWP) app 從啟用到關閉為止的整個週期。 許多使用者會將他們的工作與遊戲分布在多個裝置和 app 中。 因為使用者會在其裝置上進行多工處理，所以他們現在期望您的 app 可以記住狀態。 例如，他們預期頁面會捲動到相同的位置，而所有的控制項都會處於與先前相同的狀態。 在了解 app 週期的啟動、暫停和繼續之後，您可以提供這類型的順暢行為。

## app 執行狀態


這個圖例表示 app 執行狀態之間的轉換。 我們會在以下幾節描述這些狀態和事件。 如需每個狀態轉換以及 app 該如何回應的詳細資訊，請參閱 [**ApplicationExecutionState**](https://msdn.microsoft.com/library/windows/apps/br224694) 列舉的參考資料。

![狀態圖例，顯示 app 執行狀態之間的轉換](images/state-diagram.png)

## 部署


如果要啟用 app，必須先部署 app。 當使用者安裝您的 app，或者當您在開發和測試期間使用 Visual Studio 建置並執行 app 時，系統便會開始部署您的 app。 如需有關上述基本部署和進階部署案例的詳細資訊，請參閱[app 套件與部署](https://msdn.microsoft.com/library/windows/apps/hh464929)。

## app 啟動


當 app 處於 **NotRunning** 狀態且使用者點選 [開始] 畫面或應用程式清單上的 app 磚時，app 便會啟動。 您也可以預先啟動常用的 app 來達到最佳回應速度 (請參閱[處理 app 預先啟動](handle-app-prelaunch.md))。 應用程式可能因為從未被啟動、之前執行過但是毀損，或者已暫停但是無法保留於記憶體中且被系統終止而處於 **NotRunning** 狀態。 啟動和啟用是兩回事。 啟用是指您的 app 透過協定或擴充功能 (例如「搜尋」協定) 進行啟動。

當 app 啟動時 (包括 app 目前已在記憶體中暫停時)，會呼叫 [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335) 方法。 [
            **LaunchActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br224731) 參數包含 app 之前的狀態以及啟用引數。

當使用者切換到已終止的 app 時，系統會傳送 [**LaunchActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br224731) 引數，並將 [**Kind**](https://msdn.microsoft.com/library/windows/apps/br224728) 設定成 **Launch**，以及將 [**PreviousExecutionState**](https://msdn.microsoft.com/library/windows/apps/br224729) 設定成 **Terminated** 或 **ClosedByUser**。 App 必須載入它已儲存的應用程式資料，並且重新整理已顯示的內容。

如果 [**PreviousExecutionState**](https://msdn.microsoft.com/library/windows/apps/br224729) 的值是 **NotRunning**，則 app 必須從頭開始，如同它剛被初始啟動。

App 啟動後，Windows 會顯示 App 的啟動顯示畫面。 如果要設定啟動顯示畫面，請參閱[新增啟動顯示畫面](https://msdn.microsoft.com/library/windows/apps/xaml/hh465331)。

顯示啟動顯示畫面時，您的 app 應該準備好其使用者介面。 App 的主要工作，是登錄事件處理常式，以及設定載入初始頁面所需的任何自訂 UI。 這些工作應該只需要幾秒鐘的時間。 如果 app 必須從網路取得資料，或必須從磁碟抓取大量資料，那麼您應該在啟用以外的時間完成這些動作。 App 可以使用自己的自訂載入 UI 或是延長式啟動顯示畫面，同時等待這些長時間執行的操作完成。 如需詳細資訊，請參閱[延長顯示啟動顯示畫面](create-a-customized-splash-screen.md)和[啟動顯示畫面範例](http://go.microsoft.com/fwlink/p/?linkid=234889)。 App 完成啟用之後會進入 **Running** 狀態，啟動顯示畫面會消失 (它的所有資源和物件也都會清除)。

## App 啟用


使用者可以透過各種擴充功能和協定 (例如「共用」協定) 來啟用 app。 如需可以啟用 app 的方式清單，請參閱[**ActivationKind**](https://msdn.microsoft.com/library/windows/apps/br224693)。

[
            **Windows.UI.Xaml.Application**](https://msdn.microsoft.com/library/windows/apps/br242324) 類別定義了您可以覆寫來處理不同啟用類型的方法。 數種啟用類型有您可以覆寫的特定方法，例如 [**OnFileActivated**](https://msdn.microsoft.com/library/windows/apps/br242331) 與 [**OnSearchActivated**](https://msdn.microsoft.com/library/windows/apps/br242336) 等。對於其他啟用類型，請覆寫 [**OnActivated**](https://msdn.microsoft.com/library/windows/apps/br242330) 方法。

您可以測試 App 的啟用程式碼，了解啟用的原因以及是否已進入 **Running** 狀態。

在作業系統終止 app，然後使用者將它重新啟動的情況下，您的 app 可以在啟用期間還原先前儲存的資料。 當您的 app 由於一些原因而被暫停時，Windows 可能會終止您的 app。 使用者可能會手動關閉或登出您的 app，否則系統在執行時可能會資源不足。 如果使用者在 Windows 終止您的 app 之後將它重新啟動，則 app 會收到 [**Application.OnActivated**](https://msdn.microsoft.com/library/windows/apps/br242330) 回呼，且使用者會看見 app 的啟動顯示畫面，直到啟用 app 為止。 您可以使用這個事件判斷您的 app 是否需要還原前次暫停時已儲存的資料，或您是否必須載入 app 的預設資料。 因為啟動顯示畫面已開啟，所以您的 app 程式碼可以花費一些處理時間來完成上述作業，使用者不會感到任何明顯的延遲，但是如果您重新啟動或繼續執行，也同樣會有先前提及長時間執行操作的問題。

[
            **OnActivated**](https://msdn.microsoft.com/library/windows/apps/br242330) 事件資料包含 [**PreviousExecutionState**](https://msdn.microsoft.com/library/windows/apps/br224729) 屬性，可讓您知道 app 在啟用之前的狀態。 這個屬性是來自 [**ApplicationExecutionState**](https://msdn.microsoft.com/library/windows/apps/br224694) 列舉的其中一個值：

| 終止的原因                                                        | [
            **PreviousExecutionState**](https://msdn.microsoft.com/library/windows/apps/br224729) 屬性的值 | 要採取的行動          |
|-------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------|-------------------------|
| 被系統終止 (例如，因為資源限制)       | **Terminated**                                                                                          | 還原工作階段資料    |
| 由使用者關閉或由使用者終止處理程序                             | **ClosedByUser**                                                                                        | 以預設資料啟動 |
| 意外終止，或 app 尚未在*目前的使用者工作階段*期間執行 | **NotRunning**                                                                                          | 以預設資料啟動 |

 

**注意：***目前的使用者工作階段*是以 Windows 登入為基礎。 只要目前的使用者沒有明確登出、關機，或者 Windows 尚未因為其他原因重新啟動，目前的使用者工作階段會跨事件 (例如鎖定畫面驗證、切換使用者等等) 持續存在。

 

[
            **PreviousExecutionState**](https://msdn.microsoft.com/library/windows/apps/br224729) 也可以有 **Running** 或 **Suspended** 值，但是前提是您的 app 之前未被終止，因為所有內容已經存在於記憶體中，因此您不需要還原任何資料。

**注意**  

如果您使用電腦的 Administrator 帳戶登入，將無法啟用任何 UWP app。

如需詳細資訊，請參閱 [app 延伸](https://msdn.microsoft.com/library/windows/apps/hh464906)。

### **OnActivated** 與特定啟用

[
            **OnActivated**](https://msdn.microsoft.com/library/windows/apps/br242330) 方法可處理所有可能的啟用類型。 不過，我們更常使用不同的方法來處理最常見的啟用類型，並且只對較不常用的啟用類型使用 **OnActivated** 做為後援方法。 例如，當 [**ActivationKind**](https://msdn.microsoft.com/library/windows/apps/br224693) 為 **Launch** 時，會叫用 [**Application**](https://msdn.microsoft.com/library/windows/apps/br242324) 的 [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335) 方法做為回呼，而這是大部分 app 的典型啟用。 特定啟用還有 6 種 **On\*** 方法：[**OnCachedFileUpdaterActivated**](https://msdn.microsoft.com/library/windows/apps/hh701797)、[**OnFileActivated**](https://msdn.microsoft.com/library/windows/apps/br242331)、[**OnFileOpenPickerActivated**](https://msdn.microsoft.com/library/windows/apps/hh701799)、[**OnFileSavePickerActivated**](https://msdn.microsoft.com/library/windows/apps/hh701801)、[**OnSearchActivated**](https://msdn.microsoft.com/library/windows/apps/br242336)、[**OnShareTargetActivated**](https://msdn.microsoft.com/library/windows/apps/hh701806)。 適用於 XAML app 的啟動範本包括 **OnLaunched** 的實作和 [**Suspending**](https://msdn.microsoft.com/library/windows/apps/br242341) 的處理常式。

## App 暫停


當使用者切換至另一個 app、桌面或 [開始] 畫面時，系統會暫停您的 app。 當裝置進入電源不足狀態時，您的 app 也有可能會暫停。 當使用者切換回您的 app 時，系統就會繼續執行 app。 當系統繼續執行您的 app 時，您的變數和資料結構內容和系統暫停 app 之前一樣，沒有變化。 系統會將 app 回復成暫停之前的相同狀態，如此使用者會以為 app 一直在背景中執行。

當使用者將 app 移至背景時，Windows 會等待數秒鐘，看看使用者是否會立即返回 app，因此如果是的話，轉場將會很快完成。 如果使用者沒有在這個時間範圍內切換回來，Windows 就會暫停該 app。

如果您需要在 app 暫停時進行非同步工作，就必須延遲完成暫停，直到工作完成為止。 您可以將 [**GetDeferral**](https://msdn.microsoft.com/library/windows/apps/br224690) 方法用於 [**SuspendingOperation**](https://msdn.microsoft.com/library/windows/apps/br224688) 物件 (透過事件引數提供) 來延遲完成暫停，直到您在傳回的 [**SuspendingDeferral**](https://msdn.microsoft.com/library/windows/apps/br224684) 物件上呼叫 [**Complete**](https://msdn.microsoft.com/library/windows/apps/br224685) 方法為止。

當 app 暫停時，系統會嘗試讓 app 及其資料保留在記憶體中。 不過，如果系統沒有資源可將 app 保存在記憶體中，系統將終止您的 app。 App 不會收到它們正被終止的通知，因此，您唯一可以儲存 app 資料的機會是在暫停期間。 當 app 判斷它是在被終止後又再度啟用時，它應該會載入在暫停期間所儲存的 app 資料，因此，app 會處於與暫停之前相同的狀態。 當使用者切換回已被終止的暫停 app 時，app 應該在其 [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335) 方法中還原其 app 資料。 系統不會在 app 終止時提供通知，所以 app 必須在暫停時儲存應用程式資料並釋放獨占資源及檔案控制代碼，並在終止狀態結束後重新啟用時還原這些項目。

如果 app 已登錄 [**Application.Suspending**](https://msdn.microsoft.com/library/windows/apps/br242341) 事件的事件處理常式，則在暫停 app 之前會立即呼叫這個程式碼。 您可以使用事件處理常式來儲存 app 和使用者資料。 建議您使用應用程式資料 API 進行，因為使用它們可以確保在 app 進入 **Suspended** 狀態之前完成工作。 如需詳細資訊，請參閱[儲存及擷取設定和其他 app 資料](https://msdn.microsoft.com/library/windows/apps/mt299098)。

您也應該釋放獨占資源及檔案控制代碼，這樣當您的 app 不使用這些資源時，其他 app 仍然可以使用它們。 獨占資源的範例包括相機、I/O 裝置、外部裝置及網路資源。 明確釋放獨占資源及檔案控制代碼，有助於確保當您的 app 不使用這些資源時，其他 app 仍然可以使用它們。 如果在終止後啟用 app，則 app 應該會重新開啟其獨占資源和檔案控制代碼。

一般而言，您的 app 應該在處理暫停事件時，立即儲存其狀態並釋放資源及檔案控制代碼，而程式碼應該在 1 秒內完成這些動作。 如果 app 未在幾秒內從暫停事件中返回，Windows 就會假設該 app 已停止回應並將其終止。

有些 app 案例是 app 必須繼續執行以完成背景工作。 例如，您的 app 可以繼續在背景播放音訊；如需詳細資訊，請參閱[背景音訊](https://msdn.microsoft.com/library/windows/apps/mt282140)。 此外，即使您的 app 已被暫停或甚至終止，背景傳輸作業仍會繼續執行；如需詳細資訊，請參閱[如何下載檔案](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/jj152726.aspx#downloading_a_file_using_background_transfer)。

如需指導方針，請參閱 [App 暫停和繼續執行的指導方針](https://msdn.microsoft.com/library/windows/apps/hh465088)。

**有關使用 Visual Studio 進行偵錯的注意事項：**Visual Studio 會防止 Windows 暫停已連接至偵錯工具的 app。 這是為了讓使用者在 app 執行時可以檢視 Visual Studio 偵錯 UI。 當您正在對某個 app 偵錯時，您可以使用 Visual Studio 傳送一個暫停事件給該 app。 確定 [偵錯位置]**** 工具列已經顯示，然後按一下 [暫停]**** 圖示。

## app 可見度


當使用者從您的 app 切換到另一個 app 時，就不會再看見您的 app，但該 app 仍會處於 **Running** 狀態，直到 Windows 將它暫停為止。 如果使用者跳出您的 app，但在作業系統暫停該 app 之前就啟用或返回，則該 app 仍會維持 **Running** 狀態。

當 app 的可見度變更時，就不會收到啟用事件，因為 app 仍在執行中。 Windows 只在必要時來回切換 app。 如果 app 必須在使用者跳出與返回時執行一些動作，請處理 [**Window.VisibilityChanged**](https://msdn.microsoft.com/library/windows/apps/hh702458) 事件。

請勿依賴這些事件的特定順序。

## app 繼續執行


當使用者返回 app，或當裝置脫離電源不足狀態後，它是使用中的 app 時，暫停的 app 就可以繼續執行。

如需您的 app 繼續時可能進入的狀態列舉，請參閱 [**ApplicationExecutionState**](https://msdn.microsoft.com/library/windows/apps/br224694)。 當應用程式從 **Suspended** 狀態繼續時，它會進入 **Running** 狀態，並從暫停時的位置繼續。 不會遺失儲存在記憶體中的 app 資料。 因此，大部分 app 繼續執行時都不需要做任何事。 不過，app 可能已經暫停了幾個小時甚至好幾天。 如果 app 的內容或網路連線已過時，就必須在 app 繼續執行時重新整理它們。 如果 app 已登錄 [**Application.Resuming**](https://msdn.microsoft.com/library/windows/apps/br242339) 事件的事件處理常式，當 app 從 **Suspended** 狀態繼續執行時，就會呼叫這個事件處理常式。 您可以使用這個事件處理常式來重新整理 app 的內容和資料。

如果啟用暫停的應用程式是要加入應用程式協定或延伸，它會先收到 **Resuming** 事件，再收到 **Activated** 事件。

當 app 被暫停時，它不會接收到原先登錄要接收的任何網路事件。 這些網路事件不會排入佇列，但是會遺失。 因此，您的 app 在繼續時必須測試網路狀態。

**注意：**因為不會從 UI 執行緒引發 [**Resuming**](https://msdn.microsoft.com/library/windows/apps/br242339) 事件，如果繼續處理常式中的程式碼與 UI 進行通訊，則必須使用發送器。

 

如需指導方針，請參閱 [App 暫停和繼續執行的指導方針](https://msdn.microsoft.com/library/windows/apps/hh465088)。

## app 關閉


使用者通常不需要關閉 app，可以讓 Windows 管理它們。 但是，使用者可以選擇使用關閉手勢，或者在 Windows 上按 Alt+F4 或在 Windows Phone 上使用工作切換器，來關閉 app。

沒有特殊事件可指出使用者已關閉 app。

使用者關閉 app 之後，該 app 會先被暫停然後終止，接著進入 **NotRunning** 狀態。

在 Windows 8.1 和更新版本中，使用者關閉 app 之後，只會從畫面和切換清單中移除 app，不會明確終止 app。

如果 app 已登錄 **Suspending** 事件的事件處理常式，則 app 暫停時會呼叫此事件處理常式。 您可以使用這個事件處理常式，將相關的應用程式與使用者資料儲存至永續性儲存體。

**由使用者關閉的行為：**如果 app 在由使用者關閉時執行的動作，不同於由 Windows 關閉時執行的動作，您可以使用啟用事件處理常式，判斷 app 是由使用者或由 Windows 終止。 請參閱 [**ApplicationExecutionState**](https://msdn.microsoft.com/library/windows/apps/br224694) 列舉參考資料中有關 **ClosedByUser** 與 **Terminated** 狀態的描述。

建議您除非絕對有必要，否則不要讓 app 以程式設計的方式自行關閉。 例如，如果 app 偵測到記憶體流失，就可以自行關閉以保護使用者個人資料的安全。 當您以程式設計的方式關閉 app 時，系統會將此情形視為 app 毀損。

## App 毀損


系統毀損經驗的設計旨在讓使用者儘快返回原先正在進行的作業。 您不應該提供警告對話方塊或其他通知，因為這會造成使用者的延遲。

如果您的 app 毀損、停止回應或產生例外狀況，系統將會依據使用者的[意見反應與診斷設定](http://go.microsoft.com/fwlink/p/?LinkID=614828)將問題報告傳送給 Microsoft。 Microsoft 會在給您的問題報告中提供錯誤資料的子集，讓您用來改善 app。 您可以在您的儀表板中 app 的 [品質] 頁面上看到這些資料。

當使用者在 app 毀損之後再次啟用，其啟用事件處理常式會收到 **NotRunning** 的 [**ApplicationExecutionState**](https://msdn.microsoft.com/library/windows/apps/br224694) 值，而且應該顯示其初始 UI 和資料。 損毀之後，請不要依照之前針對 **Resuming** 和 **Suspended** 的方式使用 app 資料，因為該資料可能已經損毀，請參閱 [App 暫停和繼續執行的指導方針](https://msdn.microsoft.com/library/windows/apps/hh465088)。

## App 移除


當使用者刪除 app 後，app 會連同所有本機資料一併移除。 移除 app 並不會影響儲存在常見位置中的使用者資料，例如文件庫或圖片庫。

## app 週期與 Visual Studio 專案範本


啟動 Visual Studio 專案範本時都會提供與 app 週期相關的基本程式碼。 基本的 app 會處理啟動的啟用，提供一個讓您可以還原 app 資料的位置，並在您新增自己的程式碼之前，會顯示其主要的 UI。 如需詳細資訊，請參閱 [app 的 C#、VB 及 C++ 專案範本](https://msdn.microsoft.com/library/windows/apps/hh768232)。

## 應用程式週期重要 API


-   [
            **Windows.ApplicationModel**](https://msdn.microsoft.com/library/windows/apps/br224691) 命名空間
-   [
            **Windows.ApplicationModel.Activation**](https://msdn.microsoft.com/library/windows/apps/br224766) 命名空間
-   [
            **Windows.ApplicationModel.Core**](https://msdn.microsoft.com/library/windows/apps/br205865) 命名空間
-   [
            **Windows.UI.Xaml.Application**](https://msdn.microsoft.com/library/windows/apps/br242324) 類別 (XAML)
-   [
            **Windows.UI.Xaml.Window**](https://msdn.microsoft.com/library/windows/apps/br209041) 類別 (XAML)

**注意**  
本文章適用於撰寫通用 Windows 平台 (UWP) app 的 Windows 10 開發人員。 如果您是為 Windows 8.x 或 Windows Phone 8.x 進行開發，請參閱[封存文件](http://go.microsoft.com/fwlink/p/?linkid=619132)。

 

## 相關主題


* [App 暫停和繼續執行的指導方針](https://msdn.microsoft.com/library/windows/apps/hh465088)
* [處理 app 預先啟動](handle-app-prelaunch.md)
* [處理 app 啟用](activate-an-app.md)
* [處理 app 暫停](suspend-an-app.md)
* [處理 app 繼續執行](resume-an-app.md)

 

 





<!--HONumber=Mar16_HO1-->


