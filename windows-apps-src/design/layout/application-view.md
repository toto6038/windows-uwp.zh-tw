---
Description: 使用 ApplicationView 類別, 在不同的視窗中查看應用程式的不同部分。
title: 使用 ApplicationView 類別來顯示應用程式的次要視窗
ms.date: 07/19/2019
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: bc01894311badd9bb6e88f05c0f8b49c5824736b
ms.sourcegitcommit: 3cc6eb3bab78f7e68c37226c40410ebca73f82a9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/02/2019
ms.locfileid: "68730526"
---
# <a name="show-multiple-views-with-applicationview"></a>使用 ApplicationView 顯示多個視圖

讓使用者在個別的視窗中檢視您 App 的獨立部分，以協助他們提高生產力。 如果您為 app 建立多個視窗，則每個視窗的行為都是各自獨立的。 工作列會個別顯示每個視窗。 使用者可以單獨移動、調整大小、顯示及隱藏 app 視窗，也可以在 app 視窗之間切換，就像是不同的 app 一樣。 每個視窗都以自己的執行緒運作。

> **重要 API**：[**ApplicationViewSwitcher**](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.ApplicationViewSwitcher)、 [ **CreateNewView**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplication.createnewview)

## <a name="what-is-a-view"></a>什麼是檢視？

應用程式檢視是執行緒與應用程式用來顯示內容之視窗的 1:1 配對。 它是由 [**Windows.ApplicationModel.Core.CoreApplicationView**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Core.CoreApplicationView) 物件表示。

檢視是由 [**CoreApplication**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Core.CoreApplication) 物件管理。 您可以呼叫 [**CoreApplication.CreateNewView**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplication.createnewview) 來建立 [**CoreApplicationView**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Core.CoreApplicationView) 物件。 **CoreApplicationView** 會將 [**CoreWindow**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreWindow) 與 [**CoreDispatcher**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreDispatcher) 組合在一起 (儲存在 [**CoreWindow**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplicationview.corewindow) 與 [**Dispatcher**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplicationview.dispatcher) 屬性中)。 您可以將 **CoreApplicationView** 想像成 Windows 執行階段用來與核心 Windows 系統互動的物件。

您通常不會直接使用 [**CoreApplicationView**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Core.CoreApplicationView)。 取而代之的是，「Windows 執行階段」會在 [**Windows.UI.ViewManagement**](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.ApplicationView) 命名空間中提供 [**ApplicationView**](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement) 類別。 這個類別會提供您當 app 與視窗化系統互動時，所需使用的屬性、方法及事件。 若要使用**ApplicationView**，請呼叫靜態 [**ApplicationView.GetForCurrentView**](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationview.getforcurrentview) 方法，此方法會取得繫結到目前 **CoreApplicationView** 的執行緒的 **ApplicationView** 執行個體。

XAML 架構同樣也會將 [**CoreWindow**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreWindow) 物件包裝在 [**Windows.UI.XAML.Window**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Window) 物件中。 在 XAML app 中，您通常是與 **Window** 物件互動，而不是直接使用 **CoreWindow**。

## <a name="show-a-new-view"></a>顯示新的檢視

每個 App 配置是獨一無二的，我們建議在可預測的位置包括「新視窗」按鈕，例如可在新視窗中開啟的內容的右上角。 也請考慮包括內容功能表選項，可「在新視窗開啟」。

我們來看看建立新檢視的步驟。 這裡會啟動新檢視來回應按鈕點選。

```csharp
private async void Button_Click(object sender, RoutedEventArgs e)
{
    CoreApplicationView newView = CoreApplication.CreateNewView();
    int newViewId = 0;
    await newView.Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
    {
        Frame frame = new Frame();
        frame.Navigate(typeof(SecondaryPage), null);   
        Window.Current.Content = frame;
        // You have to activate the window in order to show it later.
        Window.Current.Activate();

        newViewId = ApplicationView.GetForCurrentView().Id;
    });
    bool viewShown = await ApplicationViewSwitcher.TryShowAsStandaloneAsync(newViewId);
}
```

**若要顯示新的視圖**

1.  呼叫 [**CoreApplication.CreateNewView**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplication.createnewview) 來為新內容建立新視窗和執行緒。

    ```csharp
    CoreApplicationView newView = CoreApplication.CreateNewView();
    ```

2.  追蹤新檢視的 [**Id**](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationview.id)。 您將在稍後使用它來顯示檢視。

    您可能會考慮在 app 中建立一些基礎結構，以協助追蹤您所建立的檢視。 如需範例，請參閱 [MultipleViews 範例](https://go.microsoft.com/fwlink/p/?LinkId=620574)中的 `ViewLifetimeControl` 類別。

    ```csharp
    int newViewId = 0;
    ```

3.  在新的執行緒上，填入視窗。

    您可以使用 [**CoreDispatcher.RunAsync**](https://docs.microsoft.com/uwp/api/windows.ui.core.coredispatcher.runasync) 方法，在 UI 執行緒上為新檢視排定工作。 您可以使用 [Lambda 運算式](https://go.microsoft.com/fwlink/p/?LinkId=389615)，將函式當成引數傳送給 **RunAsync** 方法。 您在 Lamdba 函式中進行的工作會在新檢視的執行緒上發生。

    在 XAML 中，您通常會將 [**Frame**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Frame) 新增到 [**Window**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Window) 的 [**Content**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.window.content) 屬性，然後將 **Frame** 導覽到您已定義 app 內容的 XAML [**Page**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Page)。 如需框架和頁面的詳細資訊, 請參閱[兩頁之間的對等導覽](../basics/navigate-between-two-pages.md)。

    在填入新的 [**Window**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Window) 之後，您必須呼叫 **Window** 的 [**Activate**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.window.activate) 方法，以便在稍後顯示 **Window**。 這項工作會在新檢視的執行緒上發生，因此會啟用新的 **Window**。

    最後，取得您稍後用來顯示檢視的新檢視 [**Id**](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationview.id)。 這項工作同樣是在新檢視的執行緒上進行，因此 [**ApplicationView.GetForCurrentView**](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationview.getforcurrentview) 會取得新檢視的 **Id**。

    ```csharp
    await newView.Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
    {
        Frame frame = new Frame();
        frame.Navigate(typeof(SecondaryPage), null);   
        Window.Current.Content = frame;
        // You have to activate the window in order to show it later.
        Window.Current.Activate();

        newViewId = ApplicationView.GetForCurrentView().Id;
    });
    ```

4.  呼叫 [**ApplicationViewSwitcher.TryShowAsStandaloneAsync**](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationviewswitcher.tryshowasstandaloneasync) 來顯示新的檢視。

    建立新的檢視之後，您可以呼叫 [**ApplicationViewSwitcher.TryShowAsStandaloneAsync**](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationviewswitcher.tryshowasstandaloneasync) 方法以將它顯示在新視窗中。 此方法的 *viewId* 參數是可一個唯一識別您 app 中每個檢視的整數。 您可以使用 **ApplicationView.Id** 屬性或 [**ApplicationView.GetApplicationViewIdForWindow**](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationview.getapplicationviewidforwindow) 方法來擷取檢視 [**Id**](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationview.id)。

    ```csharp
    bool viewShown = await ApplicationViewSwitcher.TryShowAsStandaloneAsync(newViewId);
    ```

## <a name="the-main-view"></a>主要檢視


您 app 啟動時建立的第一個檢視稱為「主要檢視」。 這個檢視是儲存在 [**CoreApplication.MainView**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplication.mainview) 屬性中，並且其 [**IsMain**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplicationview.ismain) 屬性為 true。 您不需建立這個檢視；它是由 app 所建立。 主要檢視的執行緒會做為 app 的管理員，而所有的 app 啟用事件都會在這個執行緒上傳遞。

開啟次要檢視時，可以隱藏主要檢視的視窗 (例如按一下視窗標題列中的關閉 (x) 按鈕)，但是其執行緒會維持為使用中。 在主要檢視的 [**Window**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Window) 上呼叫 [**Close**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.window.close) 會導致發生 **InvalidOperationException**。 (使用[**應用程式。** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.exit)結束以關閉您的應用程式。)如果主要視圖的執行緒終止, 應用程式就會關閉。

## <a name="secondary-views"></a>次要檢視


其他檢視 (包括您在 app 程式碼中呼叫 [**CreateNewView**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplication.createnewview) 來建立的所有檢視) 都是次要檢視。 主要檢視與次要檢視都是儲存在 [**CoreApplication.Views**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplication.views) 集合中。 通常您建立次要檢視是為了回應使用者動作。 在某些情況下，系統會為您的應用程式建立次要檢視。

> [!NOTE]
> 您可以使用 Windows「受指派的存取權」功能，以 [kiosk 模式](https://docs.microsoft.com/windows/manage/set-up-a-device-for-anyone-to-use)執行應用程式。 當您這樣做時，系統會建立次要檢視以在鎖定畫面上呈現您的 app UI。 系統並不允許有 app 建立的次要檢視，因此如果您嘗試以 kiosk 模式顯示您自己的次要檢視，將會擲回例外狀況。

## <a name="switch-from-one-view-to-another"></a>從一個檢視切換到另一個檢視

考慮提供一個方法，讓使用者能夠從次要視窗瀏覽回其父視窗。 若要這樣做，請使用 [**ApplicationViewSwitcher.SwitchAsync**](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationviewswitcher.switchasync) 方法。 您可以從原來的視窗執行緒呼叫此方法，然後傳送要切換的目的地視窗的檢視識別碼。

```csharp
await ApplicationViewSwitcher.SwitchAsync(viewIdToShow);
```

使用 [**SwitchAsync**](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationviewswitcher.switchasync) 時，您可以指定 [**ApplicationViewSwitchingOptions**](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.ApplicationViewSwitchingOptions) 的值來選擇是否要關閉初始視窗並將它從工作列中移除。

## <a name="related-topics"></a>相關主題

- [顯示多個視圖](show-multiple-views.md)
- [使用 AppWindow 顯示多個視圖](app-window.md)
- [ApplicationViewSwitcher](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.ApplicationViewSwitcher)
- [CreateNewView](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplication.createnewview)
