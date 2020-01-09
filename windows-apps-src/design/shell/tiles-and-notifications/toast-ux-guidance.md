---
Description: 瞭解如何建立有效且以使用者為焦點的通知，讓您的使用者擁有生產力和滿意度。
title: 快顯 UX 指引
label: Toast UX Guidance
template: detail.hbs
ms.date: 05/18/2018
ms.topic: article
keywords: windows 10，uwp，通知，集合，群組，ux，ux 指引，指引，動作，快顯，動作中心，noninterruptive，有效通知，不造成干擾通知，可操作，管理，組織
ms.localizationpriority: medium
ms.openlocfilehash: 9e970b5ae2019dcc0290dc6b0b72b48208eb5c12
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2020
ms.locfileid: "75684604"
---
# <a name="toast-notification-ux-guidance"></a>快顯通知 UX 指引
通知是現代生活的必要部分;其可協助使用者更具生產力，並與應用程式和網站搭配使用，並隨時掌握最新的更新。 不過，如果不是以使用者為中心的方式設計，通知就可以快速地從實用的張牙舞爪和侵入性。 您的通知會以滑鼠右鍵按一下，而不是關閉，不太可能會在關閉後再次開啟。  因此，請確定您的通知會重視使用者的螢幕空間和時間，讓您可以讓此 engagement 通道保持開啟。

> **重要 api**： [Windows 社區工具組通知 nuget 套件](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)

我們已分析我們的 Windows 遙測，以及其他第一方和協力廠商個案研究，以四個規則來建立絕佳的通知案例。  我們確信這些規則是通用的，不論平臺為何，都能協助您的通知對您的使用者有正面的影響。

## <a name="1-actionable-notifications"></a>1. 可採取動作的通知
可採取動作的通知讓您的使用者有生產力，而不需要開啟您的應用程式。  雖然讓應用程式啟動變得很好，但這不是唯一的成功量，而讓使用者不必進入您的應用程式就能完成小型工作，而在讓滿意使用者時也是非常強大的工具。

![具有輸入文字方塊和按鈕的可操作通知，用以設定提醒並回應通知](images/actionable-notification-example01.png)

以上是利用動作的通知範例。 完成工作的感受是一項普遍的假像，您可以傳送具有可採取動作之內容的通知，讓您的應用程式或網站感到滿意。 可採取動作的通知也會藉由減少使用者執行這些較小的工作所需的時間，協助提升企業和消費者案例的產能。 我們建議您包含使用者定期採取的動作，或您嘗試訓練使用者以進行的事項。  一些範例包括：
* 喜好、常用、標記或主演內容
* 核准或拒絕支出報表、時間關閉、許可權等。
* 內嵌回復郵件、電子郵件、群組聊天室、留言等等。
* 使用[擱置更新](toast-pending-update.md)完成訂單
* 設定其他時間的警示或提醒，以及行事曆上可能的預約時間

可採取動作的通知是一種非常強大的工具，可協助您的使用者感受到生產力、完成工作，並讓您的應用程式或網站擁有絕佳且有效率的體驗。  這裡有許多機會！ 如果您想要協助集體討論想法，歡迎您與 windows 通知小組聯繫。

## <a name="2-timing-and-urgency"></a>2. 計時和緊急性
與我們經常思考通知的方式相反，即時不一定是最佳的！ 我們強烈建議開發人員考慮使用者，以及他們傳送的通知是否為緊急資訊。 使用者很容易就會有太多的資訊超載，如果它們在嘗試專注時被中斷，就會得到挫折。 Windows 提供幾個選項，可讓您考慮要傳送的通知行程干擾程度：

**原始通知：** 使用[原始通知](raw-notification-overview.md)可能很有説明，尤其是在最小化使用者的中斷情形時。  傳送原始通知會在背景喚醒您的應用程式，因此您可以評估通知是否適合立即在應用程式的內容中傳遞。 如果這是您覺得應該立即向使用者顯示的東西，您就可以從該處彈出[本機](send-local-toast.md)快顯通知。  如果這是使用者現在不需要看到的東西，您就可以建立[排程](https://blogs.msdn.microsoft.com/tiles_and_toasts/2016/09/30/quickstart-sending-an-alarm-in-windows-10/)的快顯，稍後將會引發。


准刪除通知 **：** 您也可以引發會略過螢幕右下角的通知，而改為直接將通知傳送至 [行動中心]。 這是藉由將[SupressPopup 屬性](https://docs.microsoft.com/uwp/api/windows.ui.notifications.toastnotification.suppresspopup)設定為 True 來完成。 雖然可能會有一些懷疑不會在行動中心以外推出通知，但我們會看到2-3 倍的快顯通知會比上線快顯的快速服務在執行中。  當使用者準備好接收通知，並可控制其何時中斷，這也是為什麼在 noninvasively 通知使用者時，行動中心的內容可能會更有效的原因。

## <a name="3-clear-out-the-clutter"></a>3. 清除雜亂的工作
通知可以保存在行動中心相當長的時間（預設為三天）。  請務必確定在此處的內容是最新的，而且每次使用者開啟「動作中心」時都有相關的關聯性。 您會浪費使用者的螢幕空間，並佔用可供其他人使用的插槽。  假設使用者安裝您的電子郵件管理應用程式，並收到十封電子郵件和十個通知以及這些電子郵件。  視您想要的經驗而定，如果使用者已閱讀對應的電子郵件，或將應用程式開啟為從「動作中心」移除舊雜亂的方式，您可能會考慮清除這些通知。

我們有一系列的[ToastNotificationHistory](https://docs.microsoft.com/uwp/api/windows.ui.notifications.toastnotificationhistory) api，可讓您查看作用中中心的內容，以及管理這些通知。 請重視使用者的螢幕空間，請注意，您只會向使用者顯示相關和目前的內容。

## <a name="4-keeping-organized"></a>4. 保持組織
如先前所述，「行動中心」中的內容會保存三天。  為了協助您的使用者可以快速地挑選所需的資訊，請使用[標頭](https://docs.microsoft.com/windows/uwp/design/shell/tiles-and-notifications/toast-headers)或[集合](https://docs.microsoft.com/uwp/api/windows.ui.notifications.toastcollection)來組織「動作中心」中的通知。 您可以在下方看到標頭的範例。

![標頭標示為 ' 露營！！ ' 的快顯範例](images/toast-headers-action-center.png)

以某種方式將這些通知分組，讓相關內容保持在一起（也就是考慮將運動應用程式中不同的運動 leagues，或依群組聊天來排序訊息）。 集合是更明顯地分組通知的方式，而標頭則較為微妙，但兩者都可讓使用者更快速地分級和挑選通知。



上述四個重點是我們已透過自己的遙測分析，以及透過第一個和協力廠商實驗所發現的有效指引。 不過，請記住，這些指導方針只是：方針。  我們確信這些規則有助於提升您的通知的參與度和生產力，但不能取代以使用者為中心的想法，也不能從您自己的資料學習。  

## <a name="related-topics"></a>相關主題

* [快顯內容](adaptive-interactive-toasts.md)
* [原始通知](raw-notification-overview.md)
* [暫止更新](toast-pending-update.md)
* [GitHub 上的通知程式庫（Windows 社區工具組的一部分）](https://github.com/windows-toolkit/WindowsCommunityToolkit/tree/master/Microsoft.Toolkit.Uwp.Notifications)
