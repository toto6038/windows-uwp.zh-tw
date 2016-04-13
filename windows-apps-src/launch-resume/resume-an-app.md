---
處理 app 繼續執行
了解如何在系統繼續執行您的 app 時，重新整理已顯示的內容。
ms.assetid: DACCC556-B814-4600-A10A-90B82664EA15
---

# 處理 app 繼續執行


\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**重要 API**

-   [**繼續**](https://msdn.microsoft.com/library/windows/apps/br242339)

了解如何在系統繼續執行您的 app 時，重新整理已顯示的內容。 這個主題中的範例會登錄 [**Resuming**](https://msdn.microsoft.com/library/windows/apps/br242339) 事件的事件處理常式。

**藍圖：**這個主題與其他主題的相關性？ 請參閱：

-   [使用 C# 或 Visual Basic 建立 Windows 執行階段應用程式的藍圖](https://msdn.microsoft.com/library/windows/apps/br229583)
-   [使用 C++ 建立 Windows 執行階段 app 的藍圖](https://msdn.microsoft.com/library/windows/apps/hh700360)

## 登錄繼續事件處理常式

登錄以處理 [**Resuming**](https://msdn.microsoft.com/library/windows/apps/br242339) 事件，它指示使用者跳出然後又返回您的 app。

> [!div class="tabbedCodeSnippets"]
```cs
partial class MainPage
{
   public MainPage()
   {
      InitializeComponent();
      Application.Current.Resuming += new EventHandler<Object>(App_Resuming);
   }
}
```
```vb
Public NonInheritable Class MainPage

   Public Sub New()
      InitializeComponent() 
      AddHandler Application.Current.Resuming, AddressOf App_Resuming
   End Sub

End Class
```
```cpp
MainPage::MainPage()
{
    InitializeComponent();
    Application::Current->Resuming += 
        ref new EventHandler<Platform::Object^>(this, &amp;MainPage::App_Resuming);
}
```

## 暫停之後重新整理顯示的內容

當您的 app 處理 [**Resuming**](https://msdn.microsoft.com/library/windows/apps/br242339) 事件時，它有機會來重新整理自己已顯示的內容。

> [!div class="tabbedCodeSnippets"]
```cs
partial class MainPage
{
    private void App_Resuming(Object sender, Object e)
    {
        // TODO: Refresh network data
    }
}
```
```vb
Public NonInheritable Class MainPage

    Private Sub App_Resuming(sender As Object, e As Object)
 
        ' TODO: Refresh network data

    End Sub

End Class
```
```cpp
void MainPage::App_Resuming(Object^ sender, Object^ e)
{
    // TODO: Refresh network data
}
```

> **注意** 因為 [**Resuming**](https://msdn.microsoft.com/library/windows/apps/br242339) 事件不是從 UI 執行緒引發，所以調派程式必須用來取得 UI 執行緒並將更新置入 UI (如果處理常式中有需要處理的項目)。

## 備註


當使用者切換至另一個應用程式或桌面時，系統會暫停您的應用程式。 當使用者切換回您的 app 時，系統就會繼續執行 app 。 當系統繼續執行您的 app 時，您的變數和資料結構內容和系統暫停 app 之前一樣，沒有變化。 系統會將 app 回復成暫停之前的相同狀態，如此使用者會以為 app 一直在背景中執行。 不過，app 可能已經暫停一段時間，所以必須重新整理 app 暫停期間可能已變更的顯示內容，例如新聞摘要或使用者的位置。

如果 app 沒有任何要重新整理的顯示內容，就不需要為它處理 [**Resuming**](https://msdn.microsoft.com/library/windows/apps/br242339) 事件。

**有關使用 Visual Studio 進行偵錯的注意事項：** 當您的 app 連接至 Visual Studio 偵錯工具時，您可以傳送給它一個 **Resume** 事件。 確定 [偵錯位置工具列]**** 已經顯示，然後按一下 [暫停]**** 圖示旁邊的下拉式清單。 然後選擇 [**繼續**]。

> **注意** 就 Windows Phone 市集 app 而言，[**Resuming**](https://msdn.microsoft.com/library/windows/apps/br242339) 事件的後面一律跟著 [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335)，即使在 app 目前已被暫停，而使用者從主要磚或 app 清單重新啟動 app 的情況下，也是如此。 如果目前的視窗中已有設定的內容，app 可以略過初始化程序。 您可以檢查 [**LaunchActivatedEventArgs.TileId**](https://msdn.microsoft.com/library/windows/apps/br224736) 屬性，以判斷 app 是從主要磚還是次要磚啟動，然後根據該資訊，決定您是要呈現全新的 app 體驗，還是繼續 app 體驗。

## 相關主題

* [處理 app 啟用](activate-an-app.md)
* [處理 app 暫停](suspend-an-app.md)
* [App 暫停和繼續執行的指導方針](https://msdn.microsoft.com/library/windows/apps/hh465088)
* [App 週期](app-lifecycle.md)




<!--HONumber=Mar16_HO1-->


