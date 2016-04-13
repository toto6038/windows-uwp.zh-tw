---
Description: 讓使用者在個別的視窗中檢視您 app 的多個獨立部分，以協助他們提高生產力。
title: 顯示 app 的多重檢視
ms.assetid: BAF9956F-FAAF-47FB-A7DB-8557D2548D88
label: 顯示 app 的多重檢視
template: detail.hbs
---

# 顯示 app 的多重檢視


\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


您可以讓使用者在個別的視窗中檢視您 app 的多個獨立部分，以協助他們提高生產力。 典型的範例是電子郵件 app，其主要 UI 會顯示電子郵件清單和所選電子郵件的預覽。 但是使用者也可以在個別的視窗中開啟訊息，並以並列的方式檢視它們。

**重要 API**

-   [**ApplicationViewSwitcher**](https://msdn.microsoft.com/library/windows/apps/dn281094)
-   [**CreateNewView**](https://msdn.microsoft.com/library/windows/apps/dn297278)

如果您為 app 建立多個視窗，則每個視窗的行為都是各自獨立的。 工作列會個別顯示每個視窗。 使用者可以單獨移動、調整大小、顯示及隱藏 app 視窗，也可以在 app 視窗之間切換，就像是不同的 app 一樣。 每個視窗都會以自己的執行緒運作。

## <span id="What_is_a_view_"> </span> <span id="what_is_a_view_"> </span> <span id="WHAT_IS_A_VIEW_"> </span>什麼是檢視？


App 檢視是執行緒與 app 用來顯示內容之視窗的 1:1 配對。 它是由 [**CoreApplicationView**](https://msdn.microsoft.com/library/windows/apps/br225017) 物件表示。

檢視是由 [**CoreApplication**](https://msdn.microsoft.com/library/windows/apps/br225016) 物件管理。 您可以呼叫 [**CoreApplication.CreateNewView**](https://msdn.microsoft.com/library/windows/apps/dn297278) 來建立 [**CoreApplicationView**](https://msdn.microsoft.com/library/windows/apps/br225017) 物件。 **CoreApplicationView** 會將 [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) 與 [**CoreDispatcher**](https://msdn.microsoft.com/library/windows/apps/br208211) 組合在一起 (儲存在 [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br225019) 與 [**Dispatcher**](https://msdn.microsoft.com/library/windows/apps/dn433264) 屬性中)。 您可以將 **CoreApplicationView** 想像成 Windows 執行階段用來與核心 Windows 系統互動的物件。

您通常不會直接使用 [**CoreApplicationView**](https://msdn.microsoft.com/library/windows/apps/br225017)。 取而代之的是，「Windows 執行階段」會在 [**Windows.UI.ViewManagement**](https://msdn.microsoft.com/library/windows/apps/br242295) 命名空間中提供 [**ApplicationView**](https://msdn.microsoft.com/library/windows/apps/hh701658) 類別。 這個類別會提供您當 app 與視窗化系統互動時，所需使用的屬性、方法及事件。 若要使用**ApplicationView**，請呼叫靜態 [**ApplicationView.GetForCurrentView**](https://msdn.microsoft.com/library/windows/apps/hh701672) 方法，此方法會取得繫結到目前 **CoreApplicationView** 的執行緒的 **ApplicationView** 執行個體。

XAML 架構同樣也會將 [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) 物件包裝在 [**Windows.UI.XAML.Window**](https://msdn.microsoft.com/library/windows/apps/br209041) 物件中。 在 XAML app 中，您通常是與 **Window** 物件互動，而不是直接使用 **CoreWindow**。

## <span id="Show_a_new_view"> </span> <span id="show_a_new_view"> </span> <span id="SHOW_A_NEW_VIEW"> </span>顯示新的檢視


在繼續深入之前，讓我們看看建立新檢視的步驟。 這裡會啟動新檢視來回應按鈕點選。

```CSharp
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

**顯示新的檢視**

1.  呼叫 [**CoreApplication.CreateNewView**](https://msdn.microsoft.com/library/windows/apps/dn297291) 來為新內容建立新視窗和執行緒。

```    CSharp
CoreApplicationView newView = CoreApplication.CreateNewView();</code></pre></td>
    </tr>
    </tbody>
    </table>
```

2.  追蹤新檢視的 [**Id**](https://msdn.microsoft.com/library/windows/apps/dn281120)。 您將在稍後使用它來顯示檢視。

    您可能會考慮在 app 中建立一些基礎結構，以協助追蹤您所建立的檢視。 如需範例，請參閱 [MultipleViews 範例](http://go.microsoft.com/fwlink/p/?LinkId=620574)中的 `ViewLifetimeControl` 類別。

    <span codelanguage="CSharp"></span>
```    CSharp
    <colgroup>
    <col width="100%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th align="left">C#</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
int newViewId = 0;</code></pre></td>
    </tr>
    </tbody>
    </table>
```

3.  在新的執行緒上，填入視窗。

    您可以使用 [**CoreDispatcher.RunAsync**](https://msdn.microsoft.com/library/windows/apps/hh750317) 方法，在 UI 執行緒上為新檢視排定工作。 您可以使用 [Lambda 運算式](http://go.microsoft.com/fwlink/p/?LinkId=389615)，將函式當成引數傳送給 **RunAsync** 方法。 您在 Lamdba 函式中進行的工作會在新檢視的執行緒上發生。

    在 XAML 中，您通常會將 [**Frame**](https://msdn.microsoft.com/library/windows/apps/br242682) 新增到 [**Window**](https://msdn.microsoft.com/library/windows/apps/br209041) 的 [**Content**](https://msdn.microsoft.com/library/windows/apps/br209051) 屬性，然後將 **Frame** 導覽到您已定義 app 內容的 [**Page**](https://msdn.microsoft.com/library/windows/apps/br227503)。 如需詳細資訊，請參閱[兩個頁面之間的對等瀏覽](peer-to-peer-navigation-between-two-pages.md)。

    在填入新的 [**Window**](https://msdn.microsoft.com/library/windows/apps/br209041) 之後，您必須呼叫 **Window** 的 [**Activate**](https://msdn.microsoft.com/library/windows/apps/br209046) 方法，以便在稍後顯示 **Window**。 這項工作會在新檢視的執行緒上發生，因此會啟用新的 **Window**。

    最後，取得您稍後用來顯示檢視的新檢視 [**Id**](https://msdn.microsoft.com/library/windows/apps/dn281120)。 這項工作同樣是在新檢視的執行緒上進行，因此 [**ApplicationView.GetForCurrentView**](https://msdn.microsoft.com/library/windows/apps/hh701672) 會取得新檢視的 **Id**。

    <span codelanguage="CSharp"></span>
```    CSharp
    <colgroup>
    <col width="100%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th align="left">C#</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
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

4.  呼叫 [**ApplicationViewSwitcher.TryShowAsStandaloneAsync**](https://msdn.microsoft.com/library/windows/apps/dn281101) 來顯示新的檢視。

    建立新的檢視之後，您可以呼叫 [**ApplicationViewSwitcher.TryShowAsStandaloneAsync**](https://msdn.microsoft.com/library/windows/apps/dn281101) 方法以將它顯示在新視窗中。 此方法的 *viewId* 參數是可一個唯一識別您 app 中每個檢視的整數。 您可以使用 **ApplicationView.Id** 屬性或 [**ApplicationView.GetApplicationViewIdForWindow**](https://msdn.microsoft.com/library/windows/apps/dn281109) 方法來擷取檢視 [**Id**](https://msdn.microsoft.com/library/windows/apps/dn281120)。

```    CSharp
bool viewShown = await ApplicationViewSwitcher.TryShowAsStandaloneAsync(newViewId);</code></pre></td>
    </tr>
    </tbody>
    </table>
```

## <span id="The_main_view"> </span> <span id="the_main_view"> </span> <span id="THE_MAIN_VIEW"> </span>主要檢視


您 app 啟動時建立的第一個檢視稱為「*主要檢視*」。 這個檢視是儲存在 [**CoreApplication.MainView**](https://msdn.microsoft.com/library/windows/apps/hh700465) 屬性中，並且其 [**IsMain**](https://msdn.microsoft.com/library/windows/apps/hh700452) 屬性為 true。 您不需建立這個檢視；它是由 app 所建立。 主要檢視的執行緒會做為 app 的管理員，而所有的 app 啟用事件都會在這個執行緒上傳遞。

開啟次要檢視時，可以隱藏主要檢視的視窗 (例如按一下視窗標題列中的關閉 (x) 按鈕)，但是其執行緒會維持為使用中。 在主要檢視的 [**Window**](https://msdn.microsoft.com/library/windows/apps/br209041) 上呼叫 [**Close**](https://msdn.microsoft.com/library/windows/apps/br209049) 會導致發生 **InvalidOperationException**。 (使用 [**Application.Exit**](https://msdn.microsoft.com/library/windows/apps/br242327) 關閉您的 app)。如果將主要檢視的執行緒終止，app 就會關閉。

## <span id="Secondary_views"> </span> <span id="secondary_views"> </span> <span id="SECONDARY_VIEWS"> </span>次要檢視


其他檢視 (包括您在 app 程式碼中呼叫 [**CreateNewView**](https://msdn.microsoft.com/library/windows/apps/dn297278) 來建立的所有檢視) 都是次要檢視。 主要檢視與次要檢視都是儲存在 [**CoreApplication.Views**](https://msdn.microsoft.com/library/windows/apps/br205861) 集合中。 通常您建立次要檢視是為了回應使用者動作。 在某些情況下，系統會為您的 app 建立次要檢視。

**注意** 您可以使用 Windows「*受指派的存取權*」功能，以 [kiosk 模式](https://technet.microsoft.com/library/mt219050.aspx)執行 app。 當您這樣做時，系統會建立次要檢視以在鎖定畫面上呈現您的 app UI。 系統並不允許有 app 建立的次要檢視，因此如果您嘗試以 kiosk 模式顯示您自己的次要檢視，將會擲回例外狀況。

 

## <span id="Switch_from_one_view_to_another"> </span> <span id="switch_from_one_view_to_another"> </span> <span id="SWITCH_FROM_ONE_VIEW_TO_ANOTHER"> </span>從一個檢視切換到另一個檢視


您必須提供一個方法，讓使用者能夠從次要視窗瀏覽回主要視窗。 若要這樣做，請使用 [**ApplicationViewSwitcher.SwitchAsync**](https://msdn.microsoft.com/library/windows/apps/dn281097) 方法。 您可以從原來的視窗執行緒呼叫此方法，然後傳送要切換的目的地視窗的檢視識別碼。

<span codelanguage="CSharp"></span>
```CSharp
<colgroup>
<col width="100%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">C#</th>
</tr>
</thead>
<tbody>
<tr class="odd">
await ApplicationViewSwitcher.SwitchAsync(viewIdToShow);</code></pre></td>
</tr>
</tbody>
</table>
```

使用 [**SwitchAsync**](https://msdn.microsoft.com/library/windows/apps/dn281097) 時，您可以指定 [**ApplicationViewSwitchingOptions**](https://msdn.microsoft.com/library/windows/apps/dn281105) 的值來選擇是否要關閉初始視窗並將它從工作列中移除。

 

 






<!--HONumber=Mar16_HO1-->


