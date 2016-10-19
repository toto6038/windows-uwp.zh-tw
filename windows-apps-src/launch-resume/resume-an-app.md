---
author: TylerMSFT
title: "處理 app 繼續執行"
description: "了解如何在系統繼續執行您的 app 時，重新整理已顯示的內容。"
ms.assetid: DACCC556-B814-4600-A10A-90B82664EA15
translationtype: Human Translation
ms.sourcegitcommit: 231161ba576a140859952a7e9a4e8d3bd0ba4596
ms.openlocfilehash: 2813a112f9d60c5b133284903c98a152bd027bee

---

# 處理 app 繼續執行

\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

**重要 API**

-   [**繼續**](https://msdn.microsoft.com/library/windows/apps/br242339)

了解當系統繼續執行您的 App 時，要從何處重新整理 UI。 這個主題中的範例會登錄 [**Resuming**](https://msdn.microsoft.com/library/windows/apps/br242339) 事件的事件處理常式。

## 登錄繼續事件處理常式

登錄以處理 [**Resuming**](https://msdn.microsoft.com/library/windows/apps/br242339) 事件，此事件會指出使用者已切換離開您的 App 然後又返回。

> [!div class="tabbedCodeSnippets"]
> ```cs
> partial class MainPage
> {
>    public MainPage()
>    {
>       InitializeComponent();
>       Application.Current.Resuming += new EventHandler<Object>(App_Resuming);
>    }
> }
> ```
> ```vb
> Public NonInheritable Class MainPage
>
>    Public Sub New()
>       InitializeComponent()
>       AddHandler Application.Current.Resuming, AddressOf App_Resuming
>    End Sub
>
> End Class
> ```
> ```cpp
> MainPage::MainPage()
> {
>     InitializeComponent();
>     Application::Current->Resuming +=
>         ref new EventHandler<Platform::Object^>(this, &MainPage::App_Resuming);
> }
> ```

## 重新整理已顯示的內容並重新取得資源

在使用者切換到另一個 App 或切換到桌面時，系統會將您的 App 暫停幾秒鐘。 當使用者切換回您的 App 時，系統就會繼續執行您的 App。 當系統繼續執行您的 App 時，您的變數內容和資料結構會與系統暫停 App 之前一樣。 系統會從停止 App 的地方恢復執行。 對使用者來說，App 看起來就像是在背景執行一樣。

當您的 App 處理 [**Resuming**](https://msdn.microsoft.com/library/windows/apps/br242339) 事件時，它可能已經被暫停數個小時或數天。 它應該重新整理在暫停期間可能已變成過時的所有內容，例如新聞摘要或使用者的位置。

這也是還原您在 App 暫停期間所發行之任何獨佔資源的好時機，例如檔案控制代碼、相機、I/O 裝置、外部裝置及網路資源。

> [!div class="tabbedCodeSnippets"]
> ```cs
> partial class MainPage
> {
>     private void App_Resuming(Object sender, Object e)
>     {
>         // TODO: Refresh network data, perform UI updates, and reacquire resources like cameras, I/O devices, etc.
>     }
> }
> ```
> ```vb
> Public NonInheritable Class MainPage
>
>     Private Sub App_Resuming(sender As Object, e As Object)
>  
>         ' TODO: Refresh network data, perform UI updates, and reacquire resources like cameras, I/O devices, etc.
>
>     End Sub
>
> End Class
> ```
> ```cpp
> void MainPage::App_Resuming(Object^ sender, Object^ e)
> {
>     // TODO: Refresh network data, perform UI updates, and reacquire resources like cameras, I/O devices, etc.
> }
> ```

> **注意**：由 [**Resuming**](https://msdn.microsoft.com/library/windows/apps/br242339) 事件不是從 UI 執行緒引發，因此必須在您的處理常式中使用發送器，才能將任何呼叫發送到您的 UI。

## 備註

當您的 App 連接至 Visual Studio 偵錯工具時，系統將不會暫停它。 不過，您可以從偵錯工具暫停它，然後將 **Resume** 事件傳送給它，以便對您的程式碼進行偵錯。 請確定可看見 [偵錯位置工具列]****，然後按一下 [暫停]**** 圖示旁邊的下拉式清單。 然後選擇 [繼續]****。

就「Windows Phone 市集」應用程式而言，[**Resuming**](https://msdn.microsoft.com/library/windows/apps/br242339) 事件的後面一律跟著 [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335)，即使在應用程式目前已被暫停，而使用者從主要磚或應用程式清單重新啟動應用程式的情況下，也是如此。 如果目前的視窗中已有設定的內容，App 便可以略過初始化程序。 您可以檢查 [**LaunchActivatedEventArgs.TileId**](https://msdn.microsoft.com/library/windows/apps/br224736) 屬性，以判斷 app 是從主要磚還是次要磚啟動，然後根據該資訊，決定您是要呈現全新的 app 體驗，還是繼續 app 體驗。

## 相關主題

* [App 週期](app-lifecycle.md)
* [處理 app 啟用](activate-an-app.md)
* [處理 app 暫停](suspend-an-app.md)



<!--HONumber=Aug16_HO3-->


