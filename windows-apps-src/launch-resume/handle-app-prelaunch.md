---
author: mcleblanc
title: 處理 App 預先啟動
description: 了解如何透過覆寫 OnLaunched 方法來處理 App 預先啟動。
ms.assetid: A4838AC2-22D7-46BA-9EB2-F3C248E22F52
---

# 處理 App 預先啟動


\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**重要 API**

-   [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335)

了解如何透過覆寫 [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335) 方法來處理 App 預先啟動。

## 簡介


在可用的系統資源允許的情況下，可以藉由在背景中主動啟動使用者最常用的應用程式，提升「Windows 市集」應用程式的啟動效能。 預先啟動的應用程式在啟動不久後就會立即進入暫停狀態。 當使用者叫用應用程式時，應用程式會從暫停狀態切換到執行中狀態來繼續執行，這比應用程式冷啟動快。

在 Windows 10 之前，應用程式不會自動利用預先啟動。 從 Windows 10 開始，「通用 Windows 平台」(UWP) 應用程式會自動利用預先啟動。

大多數應用程式應該都不需任何變更即可以預先啟動方式運作。 不過，某些類型的應用程式可能需要變更其啟動行為，才能以預先啟動方式正常運作。 例如，會在啟動期間變更使用者線上可見度的訊息中心應用程式，或是會假設使用者存在並在應用程式啟動時顯示精美視覺效果的遊戲。

## 預先啟動與 app 週期


預先啟動應用程式之後，它很快就會進入暫停的狀態。 (請參閱[處理 app 暫停](suspend-an-app.md))。

## 偵測和處理預先啟動


App 在啟動期間會收到 [**LaunchActivatedEventArgs.PrelaunchActivated**](https://msdn.microsoft.com/library/windows/apps/dn263740) 旗標。 請使用這個旗標來決定是否要執行應該只有在使用者明確啟動 app 時才執行的操作，如以下來自 [**Application.OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335) 的摘錄所示。

```cs
protected override void OnLaunched(LaunchActivatedEventArgs e)
{
    Frame rootFrame = Window.Current.Content as Frame;

    // Do not repeat app initialization when the Window already has content - rather just ensure that the window is active
    if (rootFrame == null)
    {
        // Create a Frame to act as the navigation context and navigate to the first page
        rootFrame = new Frame();

        rootFrame.NavigationFailed += OnNavigationFailed;

        if (e.PreviousExecutionState == ApplicationExecutionState.Terminated)
        {
            //TODO: Load state from previously suspended application
        }

        if (!e.PrelaunchActivated)
        {
            // TODO: This is not a prelaunch activation. Perform operations which
            // assume that the user explicitly launched the app such as updating
            // the online presence of the user on a social network, updating a 
            // what's new feed, etc.
        }

        // Place the frame in the current Window
        Window.Current.Content = rootFrame;
    }

    if (rootFrame.Content == null)
    {
        // When the navigation stack isn't restored navigate to the first page,
        // configuring the new page by passing required information as a navigation parameter
        rootFrame.Navigate(typeof(MainPage), e.Arguments);
    }
    // Ensure the current window is active
    Window.Current.Activate();
}
```

**提示** 如果您想要退出預先啟動，請檢查 [**LaunchActivatedEventArgs.PrelaunchActivated**](https://msdn.microsoft.com/library/windows/apps/dn263740) 旗標。 如果已設定，請在執行任何工作之前返回 OnLaunched() 以建立框架或啟動視窗。

 

## 使用 VisibilityChanged 事件


使用者看不到預先啟動所啟動的應用程式。 當使用者切換到它們時，才會顯示。 您可能會想要將某些操作延遲到應用程式主視窗顯示之後才進行。 例如，如果您的 app 會從摘要中顯示最新動向項目清單，您可以在 [**VisibilityChanged**](https://msdn.microsoft.com/library/windows/apps/hh702458) 事件期間更新該清單，而不用倚賴預先啟動 app 時所建置，並在使用者啟用 app 時可能已過時的清單。 下列程式碼會處理 **MainPage** 的 **VisibilityChanged** 事件：

```cs
public sealed partial class MainPage : Page
{
    public MainPage()
    {
        this.InitializeComponent();

        Window.Current.VisibilityChanged += WindowVisibilityChangedEventHandler;
    }

    void WindowVisibilityChangedEventHandler(System.Object sender, Windows.UI.Core.VisibilityChangedEventArgs e)
    {
        // Perform operations that should take place when the application becomes visible rather than 
        // when it is prelaunched, such as building a what's new feed 
    }
}
```

## 指導方針


-   應用程式不應該在預先啟動期間執行長時間執行的操作，因為如果無法快速暫停應用程式，應用程式就會終止。
-   當預先啟動應用程式時，應用程式不應初始化 [**Application.OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335) 的音訊播放，因為將會看不到應用程式，所以無法知道為什麼會播放音訊。
-   假設使用者可以看見應用程式，或假設應用程式是由使用者明確啟動，那麼應用程式在啟動期間不應該執行任何操作。 由於應用程式現在不需明確的使用者動作即可在背景中啟動，因此開發人員應該考量隱私權、使用者體驗及可能的效能影響。
    -   隱私權考量舉例：社交應用程式應該在何時將使用者狀態變更為線上。 它應該等到使用者切換到應用程式，而不是在預先啟動應用程式時變更狀態。
    -   使用者體驗考量舉例：如果您的應用程式 (例如遊戲) 會在啟動時顯示一個開場片段，則您可以將開場片段延遲到使用者切換到該應用程式之後才顯示。
    -   效能影響舉例：您可以等到使用者切換到應用程式才擷取當前的天氣資訊，而不是在預先啟動應用程式時就載入該資訊，然後在應用程式變成可見時，需要再次載入該資訊來確定該資訊是當前的。
-   如果您的應用程式會在啟動時清除其動態磚，請將此操作延遲到可見度變更事件發生之後才進行。
-   您 App 的遙測應該要能區分一般磚啟用和預先啟動啟用，以便讓您能夠在問題發生時識別狀況。
-   如果您有 Microsoft Visual Studio 2015 Update 1 和 Windows 10 版本 1511，您可以藉由選擇 [偵錯]**** &gt; [其他偵錯目標]**** &gt; [偵錯 Windows 通用 App 預先啟動]**** 在Visual Studio 2015中模擬預先啟動您的 App。

## 相關主題

* [App 週期](app-lifecycle.md)

 

 





<!--HONumber=May16_HO2-->


