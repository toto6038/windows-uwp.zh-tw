---
author: mcleanbyron
ms.assetid: cc24ba75-a185-4488-b70c-fd4078bc4206
description: "了解如何使用 AdScheduler 類別將廣告新增至影片內容。"
title: "使用 HTML 5 與 JavaScript，將廣告新增至影片內容"
translationtype: Human Translation
ms.sourcegitcommit: f88a71491e185aec84a86248c44e1200a65ff179
ms.openlocfilehash: 5573d37068a6b6c10ff9e380bb8b34bb83516c35

---

# <a name="add-advertisements-to-video-content-in-html-5-and-javascript"></a>使用 HTML 5 與 JavaScript，將廣告新增至影片內容


本文會逐步說明如何使用 [AdScheduler](https://msdn.microsoft.com/library/windows/apps/mt732197.aspx) 類別在使用 JavaScript 和 HTML 撰寫的通用 Windows 平台 (UWP) App 的影片內容中新增廣告。

>**注意**&nbsp;&nbsp;目前只有使用 JavaScript 與 HTML 撰寫的 UWP app 支援此功能。

[AdScheduler](https://msdn.microsoft.com/library/windows/apps/mt732197.aspx) 可搭配漸進式和串流處理媒體，並使用 IAB 標準 Video Ad Serving Template (VAST) 2.0/3.0 和 VMAP 承載格式。 如果使用標準，[AdScheduler](https://msdn.microsoft.com/library/windows/apps/mt732197.aspx) 就無從得知所互動的廣告服務。

影片內容的廣告根據程式少於 10 分鐘 (短時間形式) 或超過 10 分鐘 (長時間形式) 而有所不同。 雖然要在服務上設定後者比較複雜，但是寫入用戶端程式碼的方式實際上沒有任何差異。 如果 [AdScheduler](https://msdn.microsoft.com/library/windows/apps/mt732197.aspx) 收到含單一廣告而不是資訊清單的 VAST 承載，就會被視為如同為單一預載廣告 (在時間 00:00 的一個中斷) 呼叫的資訊清單。

## <a name="prerequisites"></a>先決條件

* 安裝 [Microsoft Store Services SDK](http://aka.ms/store-em-sdk) 與 Visual Studio 2015。

* 您的專案必須使用 [MediaPlayer](https://github.com/Microsoft/TVHelpers/wiki/MediaPlayer-Overview) 控制項來播放將排定廣告的影片內容。 此控制項可從 GitHub 上 Microsoft 提供的 [TVHelpers](https://github.com/Microsoft/TVHelpers) 程式庫集合中取得。

  下列範例顯示如何在 HTML 標記中宣告 [MediaPlayer](https://github.com/Microsoft/TVHelpers/wiki/MediaPlayer-Overview)。 這個標記通常屬於 index.html 檔案 (或其他 html 檔案，視您的專案而定) 中的 `<body>` 區段。

  > [!div class="tabbedCodeSnippets"]
  ``` html
  <div id="MediaPlayerDiv" data-win-control="TVJS.MediaPlayer">
    <video src="URL to your content">
    </video>
  </div>
  ```

  下列範例顯示如何在 JavaScript 程式碼中建立 [MediaPlayer](https://github.com/Microsoft/TVHelpers/wiki/MediaPlayer-Overview)。

  > [!div class="tabbedCodeSnippets"]
  [!code-javascript[TrialVersion](./code/AdvertisingSamples/AdSchedulerSamples/js/js/main.js#Snippet1)]

## <a name="how-to-use-the-adscheduler-class-in-your-code"></a>如何在您的程式碼中使用 AdScheduler 類別

1. 在 Visual Studio 中，開啟您的專案或建立新專案。

2. 如果專案的目標是 [任何 CPU]****，請將您的專案更新成使用架構特定的建置輸出 (例如，[x86]****)。 如果專案的目標是 [任何 CPU]****，您將無法於下列步驟中成功加入 Microsoft 廣告庫的參考。 如需詳細資訊，請參閱[專案中因目標為 [任何 CPU] 所造成的參考錯誤](known-issues-for-the-advertising-libraries.md#reference_errors)。

3. 將 **Microsoft Advertising SDK for JavaScript** 程式庫的參考新增至您的專案。

  a. 在 [方案總管]**** 視窗中的 [參考]**** 上按一下滑鼠右鍵，然後選取 [加入參考]****。

  b. 在 [參考管理員]**** 中，展開 [通用 Windows]****、按一下 [擴充功能]****，然後選取 [Microsoft Advertising SDK for JavaScript]**** (Version 10.0) 旁邊的核取方塊。

  c. 在 [參考管理員]**** 中，按一下 [確定]。

4.  將 AdScheduler.js 檔案新增到您的專案︰

  a.  在 Visual Studio 中，按一下 [專案]**** 和 [管理 NuGet 套件]****。

  b.  在搜尋方塊中，輸入 **Microsoft.StoreServices.VideoAdScheduler** 並安裝 Microsoft.StoreServices.VideoAdScheduler 套件。 AdScheduler.js 檔案會新增到您專案中的 ../js 子目錄。

5.  開啟 index.html 檔案 (或其他 html 檔案，視您的專案而定)。 在 `<head>` 區段中，在專案的 default.css 和 main.js JavaScript 參考後面新增 ad.js 和 adscheduler.js 的參考。

  > [!div class="tabbedCodeSnippets"]
  ``` html
  <script src="//Microsoft.Advertising.JavaScript/ad.js"></script>
  <script src="/js/adscheduler.js"></script>
  ```

  <span/>
  > **注意**&nbsp;&nbsp; 這一行必須放在 `<head>` 區段所包含的 main.js 之後，否則建置專案時會發生錯誤。

6.  在專案的 main.js 檔案中，新增建立新 [AdScheduler](https://msdn.microsoft.com/library/windows/apps/mt732197.aspx) 物件的程式碼。 傳入裝載影片內容的 **MediaPlayer**。 程式碼必須放在 [WinJS.UI.processAll](https://msdn.microsoft.com/library/windows/apps/hh440975.aspx) 後面執行。

  > [!div class="tabbedCodeSnippets"]
  [!code-javascript[TrialVersion](./code/AdvertisingSamples/AdSchedulerSamples/js/js/main.js#Snippet2)]

7.  使用 [requestSchedule](https://msdn.microsoft.com/library/windows/apps/mt732208.aspx) 或 [requestScheduleByUrl](https://msdn.microsoft.com/library/windows/apps/mt732210.aspx) 方法向伺服器要求廣告排程，並將它插入 **MediaPlayer** 時間軸，然後播放影片媒體。

  * 如果您是獲得向 Microsoft 廣告伺服器要求廣告排程權限的 Microsoft 合作夥伴，請使用 [requestSchedule](https://msdn.microsoft.com/library/windows/apps/mt732208.aspx) 並指定由您的 Microsoft 代表提供給您的應用程式識別碼和單位識別碼。 這個方法會採用 **Promise** 的形式，這是非同步的建構，其中會傳入兩個函式指標以分別處理成功和失敗的情況。 如需詳細資訊，請參閱[使用 JavaScript 的 UWP 中的非同步模式](https://msdn.microsoft.com/windows/uwp/threading-async/asynchronous-programming-universal-windows-platform-apps#asynchronous-patterns-in-uwp-using-javascript)。

      > [!div class="tabbedCodeSnippets"]
      [!code-javascript[TrialVersion](./code/AdvertisingSamples/AdSchedulerSamples/js/js/main.js#Snippet3)]

  * 若要向非 Microsoft 廣告伺服器要求廣告排程，請使用 [requestScheduleByUrl](https://msdn.microsoft.com/library/windows/apps/mt732210.aspx)，然後傳入伺服器 URL。 這個方法也採用 **Promise** 的形式。

      > [!div class="tabbedCodeSnippets"]
      [!code-javascript[TrialVersion](./code/AdvertisingSamples/AdSchedulerSamples/js/js/main.js#Snippet4)]

    <span/>
    >**注意**&nbsp;&nbsp;即使函式失敗，您還是必須呼叫 **play**，因為 [AdScheduler](https://msdn.microsoft.com/library/windows/apps/mt732197.aspx) 會告訴 **MediaPlayer** 略過廣告並直接移動到內容。 您可能有不同的商務需求，例如若無法從遠端成功擷取廣告，則插入內建的廣告。

8.  在播放時，您可以處理其他事件，這些事件可讓您的應用程式追蹤進度和/或初始廣告符合處理程序之後可能發生的錯誤。 下列程式碼顯示其中一部分事件，包含 [onPodStart](https://msdn.microsoft.com/library/windows/apps/mt732206.aspx)、[onPodEnd](https://msdn.microsoft.com/library/windows/apps/mt732205.aspx)、[onPodCountdown](https://msdn.microsoft.com/library/windows/apps/mt732204.aspx)、[onAdProgress](https://msdn.microsoft.com/library/windows/apps/mt732201.aspx)、[onAllComplete](https://msdn.microsoft.com/library/windows/apps/mt732202.aspx) 和 [onErrorOccurred](https://msdn.microsoft.com/library/windows/apps/mt732203.aspx)。

  > [!div class="tabbedCodeSnippets"]
  [!code-javascript[TrialVersion](./code/AdvertisingSamples/AdSchedulerSamples/js/js/main.js#Snippet5)]



<!--HONumber=Dec16_HO2-->


