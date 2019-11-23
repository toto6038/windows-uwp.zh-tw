---
title: 從背景工作更新動態磚
description: 使用背景工作來更新含有最新內容的 app 動態磚。
Search.SourceType: Video
ms.assetid: 9237A5BD-F9DE-4B8C-B689-601201BA8B9A
ms.date: 01/11/2018
ms.topic: article
keywords: windows 10，uwp，背景工作
ms.localizationpriority: medium
ms.openlocfilehash: df2fad68fd1aab9b3b056e962736f3d37f749e63
ms.sourcegitcommit: d38e2f31c47434cd6dbbf8fe8d01c20b98fabf02
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/06/2019
ms.locfileid: "70393539"
---
# <a name="update-a-live-tile-from-a-background-task"></a>從背景工作更新動態磚

**重要 API**

-   [**IBackgroundTask**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.IBackgroundTask)
-   [**BackgroundTaskBuilder**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder)

使用背景工作來更新含有最新內容的 app 動態磚。

以下是說明如何將動態磚新增至應用程式的影片。

<iframe src="https://channel9.msdn.com/Blogs/One-Dev-Minute/Updating-a-live-tile-from-a-background-task/player" width="720" height="405" allowFullScreen="true" frameBorder="0"></iframe>

## <a name="create-the-background-task-project"></a>建立背景工作專案  

若要為您的應用程式啟用動態磚，請將新的 Windows 執行階段元件專案新增至您的方案。 這是使用者安裝應用程式時，OS 在背景載入和執行的個別組件。

1.  在 \[方案總管\] 中，以滑鼠右鍵按一下方案，按一下 **\[新增\]** ，然後按一下 **\[新增專案\]** 。
2.  在 **\[加入新的專案\]** 對話方塊中，選取 **\[已安裝\]**  \[其他語言\]  **\[Visual C#\] &gt; \[Windows 市集\]&gt; 區段中的 &gt;\[Windows 執行階段元件\]** 範本。
3.  將專案命名為 BackgroundTasks，然後按一下或點選 [確定]。 Microsoft Visual Studio 會將新專案新增至方案。
4.  在主要專案中，將參考新增至 BackgroundTasks 專案。

## <a name="implement-the-background-task"></a>實作背景工作


實作 [**IBackgroundTask**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.IBackgroundTask) 介面以建立更新應用程式動態磚的類別。 您的背景工作會在 Run 方法中執行。 在這種情況下，工作會取得 MSDN 部落格的同步發佈摘要。 為避免工作在非同步程式碼仍執行時提早關閉，請取得延遲。

1.  在 [方案總管] 中，將自動產生的 Class1.cs 檔案重新命名為 BlogFeedBackgroundTask.cs。
2.  在 BlogFeedBackgroundTask.cs 中，使用 **BlogFeedBackgroundTask** 類別的 stub 程式碼取代自動產生的程式碼。
3.  在 Run 方法實作中，新增 **GetMSDNBlogFeed** 和 **UpdateTile** 方法的程式碼。

```cs
using System;
using System.Collections.Generic;
using System.Diagnostics;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

// Added during quickstart
using Windows.ApplicationModel.Background;
using Windows.Data.Xml.Dom;
using Windows.UI.Notifications;
using Windows.Web.Syndication;

namespace BackgroundTasks
{
    public sealed class BlogFeedBackgroundTask  : IBackgroundTask
    {
        public async void Run( IBackgroundTaskInstance taskInstance )
        {
            // Get a deferral, to prevent the task from closing prematurely
            // while asynchronous code is still running.
            BackgroundTaskDeferral deferral = taskInstance.GetDeferral();

            // Download the feed.
            var feed = await GetMSDNBlogFeed();

            // Update the live tile with the feed items.
            UpdateTile( feed );

            // Inform the system that the task is finished.
            deferral.Complete();
        }

        private static async Task<SyndicationFeed> GetMSDNBlogFeed()
        {
            SyndicationFeed feed = null;

            try
            {
                // Create a syndication client that downloads the feed.  
                SyndicationClient client = new SyndicationClient();
                client.BypassCacheOnRetrieve = true;
                client.SetRequestHeader( customHeaderName, customHeaderValue );

                // Download the feed.
                feed = await client.RetrieveFeedAsync( new Uri( feedUrl ) );
            }
            catch( Exception ex )
            {
                Debug.WriteLine( ex.ToString() );
            }

            return feed;
        }

        private static void UpdateTile( SyndicationFeed feed )
        {
            // Create a tile update manager for the specified syndication feed.
            var updater = TileUpdateManager.CreateTileUpdaterForApplication();
            updater.EnableNotificationQueue( true );
            updater.Clear();

            // Keep track of the number feed items that get tile notifications.
            int itemCount = 0;

            // Create a tile notification for each feed item.
            foreach( var item in feed.Items )
            {
                XmlDocument tileXml = TileUpdateManager.GetTemplateContent( TileTemplateType.TileWideText03 );

                var title = item.Title;
                string titleText = title.Text == null ? String.Empty : title.Text;
                tileXml.GetElementsByTagName( textElementName )[0].InnerText = titleText;

                // Create a new tile notification.
                updater.Update( new TileNotification( tileXml ) );

                // Don't create more than 5 notifications.
                if( itemCount++ > 5 ) break;
            }
        }

        // Although most HTTP servers do not require User-Agent header, others will reject the request or return
        // a different response if this header is missing. Use SetRequestHeader() to add custom headers.
        static string customHeaderName = "User-Agent";
        static string customHeaderValue = "Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)";

        static string textElementName = "text";
        static string feedUrl = @"http://blogs.msdn.com/b/MainFeed.aspx?Type=BlogsOnly";
    }
}
```

## <a name="set-up-the-package-manifest"></a>設定套件資訊清單


若要設定套件資訊清單，請將它開啟然後新增背景工作宣告。 將工作的進入點設定為類別名稱，包括其命名空間。

1.  在 [方案總管] 中，開啟 Package.appxmanifest。
2.  按一下或點選 [宣告] 索引標籤。
3.  在 [可用宣告] 下，選取 [BackgroundTasks]，然後按一下 [加入]。 Visual Studio 會在 [支援的宣告] 下新增 [BackgroundTasks]。
4.  在 **\[支援的工作類型\]** 下，確定已選取 **\[計時器\]** 。
5.  在 [應用程式設定] 下，將進入點設定成 [BackgroundTasks.BlogFeedBackgroundTask]。
6.  按一下或點選 [應用程式 UI] 索引標籤。
7.  將 [鎖定畫面通知] 設定成 [徽章與文字並排]。
8.  在 [徽章標誌] 欄位中，設定 24x24 像素圖示的路徑。
    **重要**  此圖示必須僅使用單色和透明圖元。
9.  在 [小標誌] 欄位中，設定 30x30 像素圖示的路徑。
10. 在 [寬標誌] 欄位中，設定 310x150 像素圖示的路徑。

## <a name="register-the-background-task"></a>登錄背景工作


建立 [**BackgroundTaskBuilder**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) 以登錄您的工作。

> **注意**  從 Windows 8.1 開始，在註冊時，會驗證背景工作註冊參數。 如果有任一個登錄參數無效，就會傳回錯誤。 您的應用程式必須能夠處理背景工作登錄失敗的狀況，例如使用條件式陳述式來檢查登錄是否有錯誤，接著使用不同的參數值來重試已失敗的登錄。
 

在應用程式的主頁面中，新增 **RegisterBackgroundTask** 方法，然後在 **OnNavigatedTo** 事件處理常式中進行呼叫。

```cs
using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Threading.Tasks;
using Windows.ApplicationModel.Background;
using Windows.Data.Xml.Dom;
using Windows.Foundation;
using Windows.Foundation.Collections;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Controls.Primitives;
using Windows.UI.Xaml.Data;
using Windows.UI.Xaml.Input;
using Windows.UI.Xaml.Media;
using Windows.UI.Xaml.Navigation;
using Windows.Web.Syndication;

// The Blank Page item template is documented at https://go.microsoft.com/fwlink/p/?LinkID=234238

namespace ContosoApp
{
    /// <summary>
    /// An empty page that can be used on its own or navigated to within a Frame.
    /// </summary>
    public sealed partial class MainPage : Page
    {
        public MainPage()
        {
            this.InitializeComponent();
        }

        /// <summary>
        /// Invoked when this page is about to be displayed in a Frame.
        /// </summary>
        /// <param name="e">Event data that describes how this page was reached.  The Parameter
        /// property is typically used to configure the page.</param>
        protected override void OnNavigatedTo( NavigationEventArgs e )
        {
            this.RegisterBackgroundTask();
        }


        private async void RegisterBackgroundTask()
        {
            var backgroundAccessStatus = await BackgroundExecutionManager.RequestAccessAsync();
            if( backgroundAccessStatus == BackgroundAccessStatus.AllowedMayUseActiveRealTimeConnectivity ||
                backgroundAccessStatus == BackgroundAccessStatus.AllowedWithAlwaysOnRealTimeConnectivity )
            {
                foreach( var task in BackgroundTaskRegistration.AllTasks )
                {
                    if( task.Value.Name == taskName )
                    {
                        task.Value.Unregister( true );
                    }
                }

                BackgroundTaskBuilder taskBuilder = new BackgroundTaskBuilder();
                taskBuilder.Name = taskName;
                taskBuilder.TaskEntryPoint = taskEntryPoint;
                taskBuilder.SetTrigger( new TimeTrigger( 15, false ) );
                var registration = taskBuilder.Register();
            }
        }

        private const string taskName = "BlogFeedBackgroundTask";
        private const string taskEntryPoint = "BackgroundTasks.BlogFeedBackgroundTask";
    }
}
```

## <a name="debug-the-background-task"></a>偵錯背景工作


若要偵錯背景工作，請在工作的 Run 方法中設定中斷點。 在 [偵錯位置] 工具列中，選取您的背景工作。 這會讓系統立即呼叫 Run 方法。

1.  在工作的 Run 方法中設定中斷點。
2.  按 F5 或點選 [偵錯]  **[開始偵錯]&gt;** ，部署和執行 App。
3.  App 啟動後，切換回 Visual Studio。
4.  確定可看到 [偵錯位置] 工具列。 此工具列位於 [檢視]  **[工具列]&gt;** 功能表。
5.  在 [偵錯位置] 工具列，按一下 [暫停] 下拉式清單，然後選取 [BlogFeedBackgroundTask]。
6.  Visual Studio 會在中斷點暫停執行。
7.  按 F5 或點選 [偵錯]  **[繼續]&gt;** ，繼續執行 App。
8.  按 Shift+F5 或點選 **\[偵錯\] &gt; \[停止偵錯\]** ，停止偵錯。
9.  回到 [開始] 畫面上 App 的磚。 數秒之後，磚通知會顯示在應用程式磚上。

## <a name="related-topics"></a>相關主題


* [**BackgroundTaskBuilder**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder)
* [**TileUpdateManager**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.TileUpdateManager)
* [**TileNotification**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.TileNotification)
* [使用背景工作支援您的應用程式](support-your-app-with-background-tasks.md)
* [磚和徽章的指導方針和檢查清單](https://docs.microsoft.com/windows/uwp/controls-and-patterns/tiles-and-notifications-creating-tiles)

 

 
