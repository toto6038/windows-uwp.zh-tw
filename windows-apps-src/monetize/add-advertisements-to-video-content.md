---
ms.assetid: cc24ba75-a185-4488-b70c-fd4078bc4206
description: 瞭解如何使用 AdScheduler 類別，在使用 JavaScript 搭配 HTML 撰寫的通用 Windows 平臺 (UWP) 應用程式中，顯示影片內容中的廣告。
title: 在影片內容中顯示廣告
ms.date: 02/18/2020
ms.topic: article
keywords: Windows 10, UWP, 廣告, 影片, 排程器, JavaScript
ms.localizationpriority: medium
ms.openlocfilehash: 6baf26b083cce08557a9b09f2ba95d5ad889f4a4
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175102"
---
# <a name="show-ads-in-video-content"></a>在影片內容中顯示廣告

>[!WARNING]
> 從2020年6月1日起，適用于 Windows UWP 應用程式的 Microsoft Ad 營收平臺將會關閉。 [深入了解](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

本文會逐步說明如何使用 **AdScheduler** 類別在使用 JavaScript 和 HTML 撰寫的通用 Windows 平台 (UWP) App 的影片內容中顯示廣告。

> [!NOTE]
> 目前只有使用 JavaScript 與 HTML 撰寫的 UWP app 支援此功能。

**AdScheduler** 可搭配漸進式和串流處理媒體，並使用 IAB 標準 Video Ad Serving Template (VAST) 2.0/3.0 和 VMAP 承載格式。 如果使用標準，**AdScheduler** 就無從得知所互動的廣告服務。

影片內容的廣告根據程式少於 10 分鐘 (短時間形式) 或超過 10 分鐘 (長時間形式) 而有所不同。 雖然要在服務上設定後者比較複雜，但是寫入用戶端程式碼的方式實際上沒有任何差異。 如果 **AdScheduler** 收到含單一廣告而不是資訊清單的 VAST 承載，就會被視為如同為單一預載廣告 (在時間 00:00 的一個中斷) 呼叫的資訊清單。

## <a name="prerequisites"></a>先決條件

* 使用 Visual Studio 2015 或更新版本安裝 [Microsoft Advertising SDK](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK)。

* 您的專案必須使用 [MediaPlayer](https://github.com/Microsoft/TVHelpers/wiki/MediaPlayer-Overview) 控制項來播放將排定廣告的影片內容。 此控制項可從 GitHub 上 Microsoft 提供的 [TVHelpers](https://github.com/Microsoft/TVHelpers) 程式庫集合中取得。

  下列範例顯示如何在 HTML 標記中宣告 [MediaPlayer](https://github.com/Microsoft/TVHelpers/wiki/MediaPlayer-Overview)。 這個標記通常屬於 index.html 檔案 (或其他 html 檔案，視您的專案而定) 中的 `<body>` 區段。

  ``` html
  <div id="MediaPlayerDiv" data-win-control="TVJS.MediaPlayer">
    <video src="URL to your content">
    </video>
  </div>
  ```

  下列範例顯示如何在 JavaScript 程式碼中建立 [MediaPlayer](https://github.com/Microsoft/TVHelpers/wiki/MediaPlayer-Overview)。

  [!code-javascript[TrialVersion](./code/AdvertisingSamples/AdSchedulerSamples/js/js/main.js#Snippet1)]

## <a name="how-to-use-the-adscheduler-class-in-your-code"></a>如何在您的程式碼中使用 AdScheduler 類別

1. 在 Visual Studio 中，開啟您的專案或建立新專案。

2. 如果專案的目標是 **\[任何 CPU\]**，請將您的專案更新成使用架構特定的建置輸出 (例如，**\[x86\]**)。 如果專案的目標是 **\[任何 CPU\]**，您將無法於下列步驟中成功加入 Microsoft 廣告庫的參考。 如需詳細資訊，請參閱[專案中因目標為 [任何 CPU] 所造成的參考錯誤](known-issues-for-the-advertising-libraries.md#reference_errors)。

3. 將 **Microsoft Advertising SDK for JavaScript** 程式庫的參考新增至您的專案。

    1. 在 [方案總管]**** 視窗中的 [參考]**** 上按一下滑鼠右鍵，然後選取 [加入參考]****。
    2. 在 [參考管理員]**** 中，展開 [通用 Windows]****、按一下 [擴充功能]****，然後選取 [Microsoft Advertising SDK for JavaScript]**** (Version 10.0) 旁邊的核取方塊。
    3. 在 [參考管理員]**** 中，按一下 [確定]。

4.  將 AdScheduler.js 檔案新增到您的專案︰

    1. 在 Visual Studio 中，按一下 [專案]**** 和 [管理 NuGet 套件]****。
    2. 在搜尋方塊中，輸入 **Microsoft.StoreServices.VideoAdScheduler** 並安裝 Microsoft.StoreServices.VideoAdScheduler 套件。 AdScheduler.js 檔案會新增到您專案中的 ../js 子目錄。

5.  開啟 index.html 檔案 (或其他適用於您專案的 HTML 檔案)。 在 `<head>` 區段中，在專案的 default.css 和 main.js JavaScript 參考後面新增 ad.js 和 adscheduler.js 的參考。

    ``` html
    <script src="//Microsoft.Advertising.JavaScript/ad.js"></script>
    <script src="/js/adscheduler.js"></script>
    ```

    > [!NOTE]
    > 這一行必須放在 `<head>` 區段所包含的 main.js 之後，否則建置專案時會發生錯誤。

6.  在專案的 main.js 檔案中，新增建立新 **AdScheduler** 物件的程式碼。 傳入裝載影片內容的 **MediaPlayer**。 程式碼必須放在 [WinJS.UI.processAll](/previous-versions/windows/apps/hh440975) 後面執行。

    [!code-javascript[TrialVersion](./code/AdvertisingSamples/AdSchedulerSamples/js/js/main.js#Snippet2)]

7.  使用 **AdScheduler** 物件的 **requestSchedule** 或 **requestScheduleByUrl** 方法向伺服器要求廣告排程，並將它插入 **MediaPlayer** 時間軸，然後播放影片媒體。

    * 如果您是獲得向 Microsoft 廣告伺服器要求廣告排程權限的 Microsoft 合作夥伴，請使用 **requestSchedule** 並指定由您的 Microsoft 代表提供給您的應用程式識別碼和單位識別碼。

        這個方法採用[承諾](../threading-async/asynchronous-programming-universal-windows-platform-apps.md#asynchronous-patterns-in-uwp-using-javascript) 形式，它是一種非同步建構，其中傳遞了兩個功能指標：一個指標在承諾成功完成時呼叫**onComplete** 函數，另一個指標在遇到錯誤時呼叫 **onError** 函數。 在 **onComplete** 函數中，開始播放視訊內容。 廣告將在已排程的時間開始播放。 在您 **onError** 函數中，處理錯誤，然後開始播放視訊。 您的視訊內容在播放時不會有廣告。 **onError** 函數的引數包含下列成員的物件。

        [!code-javascript[TrialVersion](./code/AdvertisingSamples/AdSchedulerSamples/js/js/main.js#Snippet3)]

    * 若要向非 Microsoft 廣告伺服器要求廣告排程，請使用 **requestScheduleByUrl**，然後傳入伺服器 URI。 這種方法也採用 **Promise** 的形式，它接受 **onComplete** 和 **onError** 函數的指標。 從伺服器傳回的廣告承載必須符合視訊廣告服務樣板 (VAST) 或視訊多廣告播放列表 (VMAP) 承載格式。

        [!code-javascript[TrialVersion](./code/AdvertisingSamples/AdSchedulerSamples/js/js/main.js#Snippet4)]

    > [!NOTE]
    > 在開始播放 **MediaPlayer** 中的主要返回。內容之前，您應該等待 **requestSchedule** 或 **requestScheduleByUrl** 傳回。 在 **requestSchedule** 傳回之前開始播放媒體 (在預載廣告的情况下) 將導致預載中斷主視訊內容。 即使函數失敗，您還是必須呼叫 **play**，因為 **AdScheduler** 會告訴 **MediaPlayer** 略過廣告並直接移動到內容。 您可能有不同的商務需求，例如若無法從遠端成功擷取廣告，則插入內建的廣告。

8.  在播放時，您可以處理其他事件，這些事件可讓您的應用程式追蹤進度和/或初始廣告符合處理程序之後可能發生的錯誤。 下列程式碼顯示其中一部分事件，包含 **onPodStart**、**onPodEnd**、**onPodCountdown**、**onAdProgress**、**onAllComplete** 和 **onErrorOccurred**。

    [!code-javascript[TrialVersion](./code/AdvertisingSamples/AdSchedulerSamples/js/js/main.js#Snippet5)]

## <a name="adscheduler-members"></a>AdScheduler 成員

本節提供有關 **AdScheduler** 物件成員的一些詳細資訊。 如需有關這些成員的詳細資訊，請參閱專案中 AdScheduler.js 檔案中的註解和定義。

### <a name="requestschedule"></a>requestSchedule

這個方法會從 Microsoft 廣告伺服器要求廣告排程，並將其插入到傳遞給 **AdScheduler** 建構函式的 **MediaPlayer** 的時間軸中。

選用的第三個參數 (*adTags*) 是名稱/值組的 JSON 集合，可用於有進階目標的應用程式。 例如，播放各種自動連結到的影片的應用程式可能會以顯示中車輛的製造和型號來補充廣告單元 ID。 此參數僅供有收到 Microsoft 核准使用廣告標記的合作夥伴使用。

參照 *adTags* 時應注意下列事項：

* 此參數是一個很少使用的選項。 在使用 adTags 之前，發行者必須與 Microsoft 密切合作。
* 名稱和值必須在廣告服務上預先定義。 廣告標記不是開放式搜尋字詞或關鍵字。
* 支援的標籤數上限是 10。
* 標記名稱限制為 16 個字元。
* 標記值上限是 128 個字元。

### <a name="requestschedulebyuri"></a>requestScheduleByUri

這個方法會從 URI 中指定的非 Microsoft 廣告伺服器要求廣告排程，並將其插入到傳遞給 **AdScheduler** 建構函式的 **MediaPlayer** 的時間軸中。 廣告伺服器傳回的廣告承載必須符合視訊廣告服務樣板 (VAST) 或視訊多廣告播放列表 (VMAP) 承載格式。

### <a name="mediatimeout"></a>mediaTimeout

此内容取得或設定媒體必須可播放的毫秒數。 設定為 0 的值通知系統永遠不要啟動逾時功能。 預設值是 30000 毫秒 (30 秒)。

### <a name="playskippedmedia"></a>playSkippedMedia

此内容的取得或設定一個 **布林** 值，該值可判別當使用者略過已排程的開始時間時，是否會播放已排程的媒體。

廣告用戶端和媒體播放機將根據主要視訊內容的向前快轉和倒帶期間所發生的廣告情況來執行條款中規則。 在大多數情況下，應用程式開發人員不允許完全略過廣告，但希望為使用者提供合理的體驗。 下列兩個選項符合大部分開發人員的需求：

1. 允許終端使用者隨意略過廣告 Pod。
2. 允許使用者略過廣告 Pod，但在播放恢復時會播放最新的 Pod。

**PlaySkippedMedia** 屬性有下列條件：

* 廣告開始後，無法略過或向前快轉廣告。
* 廣告 Pod 中的所有廣告都會在 Pod 開始後播放。
* 一旦播放，廣告將不會在主要內容 (電影、影集等) 中播放，廣告標記會標示為播放或刪除。
* 預先推出的廣告無法略過。

當恢復包含廣告的內容時，請將 **playSkippedMedia** 設定為 **false** 以略過預先預告片，並防止最近的廣告播放。 然後，內容開始後，將 **playSkippedMedia** 設定為 **true**，以確保使用者無法向前快轉後續廣告。

> [!NOTE]
> Pod 是按順序播放的一組廣告，例如在商業廣告期間播放的一組廣告。 如需詳細資訊，請參閱 IAB 數位視訊廣告服務樣板 (VAST) 規格。

### <a name="requesttimeout"></a>requestTimeout

此内容取得或設定媒體必須可播放的毫秒數，以等待逾時之前的廣告要求回應。設定為 0 的值通知系統永遠不要啟動逾時功能。 預設值是 30000 毫秒 (30 秒)。

### <a name="schedule"></a>schedule

此屬性取得從廣告伺服器擷取的排程資料。 此物件包含與視訊廣告服務樣板 (VAST) 或視訊多廣告播放列表 (VMAP) 視訊的結構相對應的完整階層資料。

### <a name="onadprogress"></a>onAdProgress  

這個事件會在廣告播放達到四分位檢查點時引發。 事件處理常式 (*eventInfo*) 的第二個參數是 JSON 物件，其成員如下：

* **進行中**：廣告播放狀態 (在 AdScheduler.js 中定義的其中一個 **MediaProgress** 列舉值 )。
* **剪輯**：正在播放的視訊剪輯。 此物件並不適用於您的代碼。
* **adPackage**：表示與正在播放的廣告相對應的廣告有效承載部分之物件。 此物件並不適用於您的代碼。

### <a name="onallcomplete"></a>onAllComplete  

當主要內容結束時並且任何已排程的影片結束後播放的廣告也結束時，會引發此事件。

### <a name="onerroroccurred"></a>onErrorOccurred  

當 **AdScheduler** 發生錯誤會引發此事件。 有關錯誤碼值的詳細資訊，請參閱 [ErrorCode](/uwp/api/microsoft.advertising.errorcode)。

### <a name="onpodcountdown"></a>onPodCountdown

廣告播放時會引發此事件，並指出目前 Pod 的剩餘時間。 事件處理常式 (*eventData*) 的第二個參數是 JSON 物件，其成員如下：

* **remainingAdTime**：目前廣告剩餘的秒數。
* **remainingPodTime**：目前 Pod 剩餘的秒數。

> [!NOTE]
> Pod 是按順序播放的一組廣告，例如在商業廣告期間播放的一組廣告。 如需詳細資訊，請參閱 IAB 數位視訊廣告服務樣板 (VAST) 規格。

### <a name="onpodend"></a>onPodEnd  

Pod 結束時會引發此事件。 事件處理常式 (*eventData*) 的第二個參數是 JSON 物件，其成員如下：

* **startTime**：Pod 的開始時間 (秒)。
* **pod**：代表 Pod 的物件。 此物件並不適用於您的代碼。

### <a name="onpodstart"></a>onPodStart

Pod 開始時會引發此事件。 事件處理常式 (*eventData*) 的第二個參數是 JSON 物件，其成員如下：

* **startTime**：Pod 的開始時間 (秒)。
* **pod**：代表 Pod 的物件。 此物件並不適用於您的代碼。