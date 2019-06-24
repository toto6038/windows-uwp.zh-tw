---
Description: 了解如何建立有效且以使用者為主的通知，讓您的使用者，有生產力並享受。
title: 快顯 UX 指導方針
label: Toast UX Guidance
template: detail.hbs
ms.date: 05/18/2018
ms.topic: article
keywords: windows 10、 uwp、 通知、 集合、 群組、 ux、 ux 指導方針，指導方針、 動作、 快顯通知、 行動作業中心、 noninterruptive、 有效的通知、 非干擾式的通知，可採取動作，管理，組織
ms.localizationpriority: medium
ms.openlocfilehash: 327a2add84343be3b972f7bb1f232298e7ef92ad
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/21/2019
ms.locfileid: "67320733"
---
# <a name="toast-notification-ux-guidance"></a>快顯通知的 UX 指導方針
通知是現代生活; 不可或缺的一部分它們可以協助使用者更具生產力、 參與應用程式和網站，以及掌握最新消息與任何更新。 不過，通知可以快速開啟從實用張牙舞爪和具侵入性，如果不是設計以使用者為中心的方式。 您的通知離開正在關閉，一個以滑鼠右鍵按一下，而且不太可能會關閉之後，它們將會開啟一次。  因此請確定您的通知會令人尊敬使用者的螢幕空間和時間，因此您可以保留此 engagement 通道開啟。

> **重要的 Api**:[Windows 社群工具組通知 nuget 套件](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)

我們已進行分析，我們的 Windows 遙測，以及其他第一個和第三方案例研究，想出如何成為很棒的通知本文四個規則。  我們確信這些規則都通用，無論平台，並可協助您對您的使用者有正面的通知。

## <a name="1-actionable-notifications"></a>1.可採取動作的通知
可採取動作的通知可讓您的使用者，而不需開啟您的應用程式提高產能。  很不錯了，應用程式會啟動，這不是成功的唯一的量值時，才可讓使用者完成小型工作，而無須瀏覽至您的應用程式可以是成功的功能強大的工具，在滿意的使用者。

![可採取動作的通知，輸入的文字方塊和按鈕來設定通知，並回應通知](images/actionable-notification-example01.png)

以上是通知的範例會利用動作。 完成工作的感覺都是正面的感覺，而您可以將該感覺帶入您的應用程式或網站傳送通知，其中有可採取動作的內容。 可採取動作的通知也有助於提高產能，同時在企業和消費者案例中，方法是減少使用者動作的時間通過才能完成這些較小的工作。 建議您包含動作，您的使用者定期取得或想要訓練您的使用者執行的項目。  一些範例包括：
* 加入我的最愛，加上旗標，或主演內容的喜好
* 核准或拒絕經費支出報表、 關閉、 權限等的時間。
* 回覆電子郵件的郵件群組聊天，註解等的內嵌。
* 完成排序使用[暫止更新](toast-pending-update.md)
* 另一次，設定警示或提醒，以及可能預約行事曆上的時間

可採取動作的通知是功能強大的工具，以協助使用者覺得生產力、 完成工作，並具有絕佳且有效率的經驗，與您的應用程式或網站。  有許多機會這裡 ！ 如果您需要的想法的腦力激盪的說明，歡迎 windows 通知小組連絡。

## <a name="2-timing-and-urgency"></a>2.計時和急迫性
相對於經常思考如何通知，不一定是最佳即時 ！ 與否，我們鼓勵開發人員思考使用者，以及如果他們傳送的通知是緊急的資訊。 使用者可以輕鬆地多載使用太多的資訊，並取得受挫，如果要將它們中斷時嘗試焦點。 Windows 會提供如何考量您要傳送通知的干擾程度的幾個選項：

**未經處理的通知：** 使用[未經處理的通知](raw-notification-overview.md)能帶來好處有許多原因，尤其是要給使用者的干擾降至最低。  傳送未經處理的通知會喚醒您的應用程式在背景進行，因此您可以評估是否將立即在您的應用程式內容中的通知合理。 如果您認為的項目應該會顯示給使用者，就能夠快顯[本機的快顯](send-local-toast.md)從該處。  如果它是使用者不需要以查看現在以滑鼠右鍵，您就可以建立[排程快顯通知](https://blogs.msdn.microsoft.com/tiles_and_toasts/2016/09/30/quickstart-sending-an-alarm-in-windows-10/)，稍後就會引發。


**準刪除快顯通知：** 您也可以引發一則通知，將會略過 彈出螢幕的右下角中，並改為將通知傳送至行動作業中心的直接。 這可以藉由設定[SupressPopup 屬性](https://docs.microsoft.com/en-us/uwp/api/windows.ui.notifications.toastnotification.suppresspopup)設為 True。 雖然可能有一些懷疑論周圍不快顯通知，行動作業中心之外，我們看到 2-3 倍的更高版本適用於透過位於 行動作業中心的快顯通知 engagement 快顯的快顯通知。  當他們準備好接收通知，而且可以控制他們會中斷時，這也是為什麼重要訊息中心的內容可以更有效地以非侵入式通知使用者，使用者會更快的回應。

## <a name="3-clear-out-the-clutter"></a>3.清除混亂的情形
通知可以保存在行動作業中心很長的時間 （預設值三天）。  請務必確定位於這裡的內容是最新狀態而相關的每次使用者開啟行動作業中心。 您會浪費使用者的螢幕空間，並佔用了較新的項目可用的位置。  假設使用者安裝您電子郵件管理的應用程式，並會接收十則電子郵件和十個通知，以及這些電子郵件。  根據您想要的體驗，您可以考慮清除這些通知，如果使用者具有讀取相對應的電子郵件，或開啟應用程式來移除重要訊息中心中的舊混亂的情形。

我們有一連串[ToastNotificationHistory](https://docs.microsoft.com/en-us/uwp/api/windows.ui.notifications.toastnotificationhistory) Api，讓您查看哪些內容位於行動作業中心，以及管理這些通知。 是令人尊敬使用者的螢幕空間，而且負責您只對使用者顯示相關且目前的內容。

## <a name="4-keeping-organized"></a>4.讓組織
如先前所述，行動作業中心中的內容未達三天。  為了協助您挑選他們要快速尋找資訊的使用者，組織中行動中心使用的通知[標頭](https://docs.microsoft.com/en-us/windows/uwp/design/shell/tiles-and-notifications/toast-headers)或是[集合](https://docs.microsoft.com/en-us/uwp/api/windows.ui.notifications.toastcollection)。 您可以看到下列標頭的範例。

![標頭的快顯通知範例標記為 'Camping!!'](images/toast-headers-action-center.png)

群組這些通知的方式，使相關的內容保持在一起 （也就是將分隔出不同的運動 leagues 運動應用程式中，或透過群組聊天排序訊息）。 集合會是比較明顯的作法，群組的通知，而標頭是更細微的但兩者均允許分級，並更快速地挑出通知的使用者。

## <a name="other-resources"></a>其他資源
上述這些四個點都是我們發現有效透過我們自己的分析遙測，以及透過第一個和第三方實驗的指引。 記住，不過，這些指導方針僅止於此： 指導方針。  我們確信這些規則有助於增加參與程度和產能的通知，但不是可以取代使用者為中心的思考，並學習您自己的資料。  

如果目前您的 UWP 應用程式傳送通知，您可以檢視分析您的通知中發生了什麼事[合作夥伴中心](https://partner.microsoft.com/dashboard)！ 使用時，此資料來自免費[存放區服務 SDK](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftStoreServicesSDK)或[WNS Api](https://docs.microsoft.com/en-us/windows/uwp/design/shell/tiles-and-notifications/windows-push-notification-services--wns--overview)。 這些計量可讓您更多深入了解您的通知，在 windows 平台，會發生什麼情況以及如何與通知互動使用者。 在左手邊接洽上移到功能表來存取此資料 > 通知，然後按一下 [通知] 頁面中 [分析] 索引標籤上。  這位於相同的位置，您會移至從合作夥伴中心傳送通知。

## <a name="related-topics"></a>相關主題

* [快顯內容](adaptive-interactive-toasts.md)
* [原始通知](raw-notification-overview.md)
* [暫止更新](toast-pending-update.md)
* [GitHub （Windows 社群工具組的一部分） 上的通知程式庫](https://github.com/windows-toolkit/WindowsCommunityToolkit/tree/master/Microsoft.Toolkit.Uwp.Notifications)
