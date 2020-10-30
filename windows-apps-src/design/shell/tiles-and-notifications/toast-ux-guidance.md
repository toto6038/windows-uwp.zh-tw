---
description: 瞭解如何建立有效且以使用者為主的通知，讓您的使用者更具生產力。
title: 快顯 UX 指導方針
label: Toast UX Guidance
template: detail.hbs
ms.date: 05/18/2018
ms.topic: article
keywords: windows 10、uwp、通知、收集、群組、ux、ux 指引、指引、動作、快顯、控制中心、noninterruptive、有效通知、不造成干擾通知、可操作、管理、組織
ms.localizationpriority: medium
ms.openlocfilehash: d6ad253ccfa744864caa8d0229d09ba40d066771
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2020
ms.locfileid: "93033041"
---
# <a name="toast-notification-ux-guidance"></a>快顯通知 UX 指導方針
通知是現代化生命的必要部分;它們可協助使用者更具生產力，並與應用程式和網站聯繫，並隨時掌握最新的更新。 但是，如果不是以使用者為中心的方式設計，通知就可以快速地變成張牙舞爪和侵入式。 您的通知是以滑鼠右鍵按一下而不是關閉的，而且不太可能會在關閉後重新開啟。  因此，請確定您的通知會尊重使用者的螢幕空間和時間，讓您可以讓這個 engagement 通道保持開啟狀態。

> **重要 api** ： [Windows 社群工具組通知 nuget 套件](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)

我們已分析過 Windows 遙測，以及其他第一方和協力廠商個案研究，以產生絕佳通知故事的四項規則。  無論平臺為何，這些規則都是通用的，而且會協助您的通知對您的使用者有正面的影響。

## <a name="1-actionable-notifications"></a>1. 可操作的通知
可操作的通知可讓您的使用者不需要開啟您的應用程式，即可提高生產力。  雖然讓應用程式啟動變得很好，但這不是唯一的成功量值，而且讓使用者不必進入您的應用程式就可以完成小型工作，而不需要移至您的應用程式，是讓滿意使用者的強大工具。

![具有輸入文字方塊和按鈕的可操作通知，可設定提醒並回應通知](images/actionable-notification-example01.png)

以上是利用動作的通知範例。 完成工作的感覺是一項普遍的正面感覺，而且您可以藉由傳送具有可採取動作內容的通知，將感覺帶到您的應用程式或網站。 可操作的通知也有助於提高生產力（在企業和消費者案例中），方法是縮短使用者完成這些較小工作的時間。 建議您包含使用者定期採取的動作，或您想要用來訓練使用者的專案。  部分範例包括：
* 喜好、常用、標記或主演內容
* 核准或拒絕 expense reports、time off、許可權等。
* 以內嵌方式回復訊息、電子郵件、群組聊天室、留言等等。
* 使用[擱置更新](toast-pending-update.md)完成訂單
* 設定其他時間的警示或提醒，以及行事曆上可能的預約時間

可採取動作的通知是一項功能強大的工具，可協助您的使用者感受到生產力、完成工作，並讓您的應用程式或網站擁有絕佳且有效率的體驗。  這裡有許多機會！ 如果您想要協助靈感靈感，請隨時與 windows 通知小組聯繫。

## <a name="2-timing-and-urgency"></a>2. 計時和緊急
相反地，我們通常會考慮通知的方式，這不一定是最理想的做法！ 我們鼓勵開發人員思考使用者，如果他們傳送的通知是緊急資訊，則為。 使用者很容易就能使用太多資訊來多載，如果他們在嘗試專注時遭到中斷，就會感到挫折。 Windows 提供幾個選項，讓您瞭解要傳送的通知行程干擾程度：

**原始通知：** 使用 [原始通知](raw-notification-overview.md) 可能很有説明，尤其是在將使用者的中斷降到最低時。  傳送原始通知會在背景喚醒您的應用程式，因此您可以評估通知是否適合在您的應用程式內容中立即傳遞。 如果您覺得應該立即向使用者顯示，您可以從該處取出 [本機](send-local-toast.md) 快顯通知。  如果這是使用者目前不需要看到的內容，您可以建立 [排程](/archive/blogs/tiles_and_toasts/quickstart-sending-an-alarm-in-windows-10) 的快顯，稍後將會引發。


准刪除快顯通知 **：** 您也可以引發通知，以略過畫面右下角的快顯視窗，改為將通知直接傳送至控制中心。 將 [SupressPopup 屬性](/uwp/api/windows.ui.notifications.toastnotification.suppresspopup) 設定為 True 即可完成這項作業。 雖然可能有一些 skepticism 不會在控制中心之外推出通知，但我們2-3 看到快顯通知的互動速度比推出快顯快顯的控制中心更高。  當使用者準備好接收通知，而且可以控制其何時中斷時，這也是為什麼控制中心中的內容可以更有效地 noninvasively 通知使用者的原因。

## <a name="3-clear-out-the-clutter"></a>3. 清除雜亂
通知可以在「動作中心」中保存一段相當長的時間， (預設值三天) 。  當使用者開啟控制中心時，請務必確定存留于此處的內容是最新的，並且相關。 您將會浪費使用者的螢幕空間，並佔用可用於較新內容的位置。  假設使用者安裝您的電子郵件管理應用程式，並收到10封電子郵件和十個通知以及這些電子郵件。  根據您想要的體驗，如果使用者已讀取對應的電子郵件，或將應用程式開啟以移除控制中心的舊雜亂，您可能會考慮清除這些通知。

我們有一系列 [ToastNotificationHistory](/uwp/api/windows.ui.notifications.toastnotificationhistory) api 可讓您查看控制中心中的內容，以及管理這些通知。 尊重使用者的螢幕空間，請注意，您只會向使用者顯示相關和目前的內容。

## <a name="4-keeping-organized"></a>4. 保持組織
如先前所述，「動作中心」中的內容會持續三天。  為了協助您的使用者在需要時快速挑選他們所需的資訊，請使用 [標頭](./toast-headers.md) 或 [集合](/uwp/api/windows.ui.notifications.toastcollection)將通知組織在控制中心中。 您可以在下方看到標頭的範例。

![標頭標記為 ' 露營！！ ' 的快顯範例](images/toast-headers-action-center.png)

以某種方式將這些通知分組，讓相關的內容保持在一起 (也就是思考如何分隔運動應用程式中的不同運動 leagues，或依群組聊天) 來排序訊息。 集合是更明顯地分組通知的方式，而標頭則更微妙，但兩者都可讓使用者更快速地分級和挑選通知。



上述的四個重點是我們透過自己的遙測分析，以及透過第一方和協力廠商實驗，所發現的有效指引。 不過請記住，這些指導方針只是：指導方針。  我們確信這些規則將有助於提高您的通知的參與度和生產力，但不會取代以使用者為中心的思考，並從您自己的資料中學習。  

## <a name="related-topics"></a>相關主題

* [快顯通知內容](adaptive-interactive-toasts.md)
* [原始通知](raw-notification-overview.md)
* [擱置中更新](toast-pending-update.md)
* [GitHub 上的 Notifications 程式庫 (屬於 Windows 社群工具組)](https://github.com/windows-toolkit/WindowsCommunityToolkit/tree/master/Microsoft.Toolkit.Uwp.Notifications)
