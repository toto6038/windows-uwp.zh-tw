---
author: manoskow
Description: Learn how to create effective and user-focused notifications that make your users productive and happy.
title: 快顯通知的 UX 指導方針
label: Toast UX Guidance
template: detail.hbs
ms.author: mijacobs
ms.date: 05/18/2018
ms.topic: article
keywords: windows 10，uwp，通知，集合、 群組、 ux，ux 指導方針，指導方針、 動作、 快顯通知，控制中心、 noninterruptive、 有效的通知、 干擾通知，可採取動作，管理，組織
ms.localizationpriority: medium
ms.openlocfilehash: 849c8ffc66661546a088a3d89747e6690a763e71
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/15/2018
ms.locfileid: "6971265"
---
# <a name="toast-notification-ux-guidance"></a>快顯通知的 UX 指導方針
通知是必要的新式生命;它們能協助使用者生產力與預約與應用程式和網站，以及掌握目前使用的任何更新。 不過，從有用 overbearing 和覺得受干擾，如果不是設計的使用者為中心的方式可以快速關閉通知。 通知有一個以滑鼠右鍵按一下離正在關閉，並不太可能會關閉後，他們將會開啟一次。  因此請確定您的通知是公開的使用者的螢幕空間和時間，因此您可以保留這個佔用通道開啟。

> **重要 Api**: [Windows Community Toolkit Notifications nuget 套件](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)

我們已經分析我們 Windows 遙測後，以及其他第一個和第三方案例研究，來想出周圍得以絕佳通知故事的四個規則。  我們有信心這些規則通用適用於，無論平台，並將協助您 notificaitons 對於您的使用者有正面影響。

## <a name="1-actionable-notifications"></a>1.可採取動作的通知
可採取動作的通知可讓您的使用者不需要開啟您的應用程式可提高生產力。  雖然它非常有應用程式啟動、 這不是成功的唯一的量值，以及啟用使用者才會完成小型工作，而不需要移至您的應用程式可以是成功的非常強大的工具 delighting 您的使用者。

![包含文字輸入的方塊和設定提醒，及回應通知的按鈕可採取動作的通知](images/actionable-notification-example01.png)

上述是通知的運用動作的範例。 完成工作的感覺都是正面的感覺，且您可以藉由將它們中有可採取動作的內容的通知傳送到您的 app 或網站提供該感覺。 可採取動作的通知也可以協助提高生產力，同時在企業與消費者案例中，藉由減少使用者動作的時間瀏覽來完成這些較小的工作。 我們建議您包含您的使用者定期執行，動作或您嘗試訓練您的使用者執行的動作。  一些範例包括：
* 根據自己的喜愛 favoriting，加上旗標，或主演內容
* 核准或拒絕任何費用報告、 關閉、 權限等的時間。
* 內嵌回覆訊息，電子郵件、 群組聊天的註解等等。
* 完成使用[擱置中更新](toast-pending-update.md)的訂單
* 另一次，設定警示或提醒，以及潛在預約在行事曆上的時間

ctionable 通知是一個很強大的工具，可協助您的使用者感覺生產力、 完成工作，以及有很棒且有效率的體驗，與您的應用程式或網站。  有許多機會以下 ！ 如果您想腦力激盪想法的說明，請放心地連絡 windows 通知團隊。  您 

## <a name="2-timing-and-urgency"></a>2.計時和緊急性
相反如何我們通常認為有關通知，即時不一定是最佳 ！ 我們在或不敦促開發人員要思考使用者和他們要傳送的通知是否緊急資訊。 使用者可以輕鬆地使用太多資訊多載，而感到沮喪如果他們正在中斷時嘗試焦點。 Windows 提供如何干擾程度的您傳送的通知，請考慮的幾個選項：

**原始通知：** 使用[原始通知](raw-notification-overview.md)將實用有許多原因，尤其是最小化干擾使用者的選單鍵。  傳送原始通知將喚醒您的應用程式在背景中，因此您可以評估是否通知會立即提供您的應用程式內容中的比較合理。 如果它是您感覺起來應顯示給使用者立即，便能在要快顯從該處[本機快顯通知](send-local-toast.md)。  如果這是使用者不需要查看現在，您就可以建立在稍後將會引發[排程快顯通知](https://blogs.msdn.microsoft.com/tiles_and_toasts/2016/09/30/quickstart-sending-an-alarm-in-windows-10/)。

**幽靈車快顯通知：** 您也可以引發會略過彈出在右下角的畫面，並改為直接與重要訊息中心傳送通知的通知。 這被透過[SuppressPopup 屬性](https://docs.microsoft.com/en-us/uwp/api/windows.ui.notifications.toastnotification.suppresspopup)設定為 True。 可能有一些周圍不在控制中心以外彈出通知的態度，雖然我們看到 2-3 次更高版本透過 live 在控制中心的快顯通知的佔用取出快顯通知。  使用者可更具回應性，當它們準備好接收 notificaitons 且可以控制它們會中斷時，這是控制中心中的內容可以是適用於非侵入式通知使用者更有效的原因。

## <a name="3-clear-out-the-clutter"></a>3.清除出雜亂度
通知可以保留在控制中心中，相當長的時間 （預設值的三天）。  請務必確定將位於這裡的內容是最新狀態及相關每次使用者開啟控制中心。 您是浪費使用者的螢幕空間，且佔用可能會使用於項更保持最新狀態的插槽。  假設使用者安裝您的電子郵件管理應用程式，而會接收十個電子郵件和十個通知，以及這些電子郵件。  根據您所需的體驗，您可以考慮清除這些通知，如果使用者已讀取相對應的電子郵件，或開啟應用程式的方式，從重要訊息中心移除舊的雜亂度。

我們有一系列的[ToastNotificationHistory](https://docs.microsoft.com/en-us/uwp/api/windows.ui.notifications.toastnotificationhistory)可讓您查看哪些內容是在控制中心，Api，以及管理這些通知。 會公開的使用者的螢幕空間，請小心，您只對使用者顯示與內容相關的目前內容。

## <a name="4-keeping-organized"></a>4.保持組織
如先前所述，在控制中心中的內容會保存三天。  為了協助您挑選出它們快速尋找資訊的使用者，組織中使用[標頭](https://docs.microsoft.com/en-us/windows/uwp/design/shell/tiles-and-notifications/toast-headers)或[集合](https://docs.microsoft.com/en-us/uwp/api/windows.ui.notifications.toastcollection)的控制中心的通知。 您可以看到標頭下面的的範例。

![快顯通知標頭使用的範例標示為 'Camping!!'](images/toast-headers-action-center.png)

這兩個這些 gorup 通知，因此相關的內容保持在一起的方式 （亦即想像分開的運動應用程式，在不同的運動聯盟或排序群組聊天訊息）。 集合是群組 notificaitons 更明顯方式，而標頭是更細微的但同時允許使用者分級，以及更快速挑選出通知。 

## <a name="other-resources"></a>其他資源
上述這些四個點是遙測的我們有找到有效透過自己，分析，以及透過第一個和第三方實驗的指導方針。 請記住，不過，這些指導方針就只是： 指導方針。  我們有信心這些規則將會協助提高吸引力與您的通知，生產力，但任何項目可以以使用者為中心 thinking，和了解可以從您自己的資料。  

如果您傳送通知給您的 UWP app 目前，您可以檢視分析您在[合作夥伴中心](https://partner.microsoft.com/dashboard)的通知發生了什麼事 ！ 使用[Store Services SDK](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftStoreServicesSDK)或[WNS Api](https://docs.microsoft.com/en-us/windows/uwp/design/shell/tiles-and-notifications/windows-push-notification-services--wns--overview)時，這項資料來自免費。 這些計量會提供您更深入您在 windows 平台上的通知會發生什麼事以及如何通知正在互動的使用者。 在左側互動上移至功能表中存取此資料 > 通知，然後按一下 [通知] 頁面中的 「 分析 」] 索引標籤上。  這位於相同的位置，您會移至從合作夥伴中心傳送通知。

## <a name="related-topics"></a>相關主題

* [快顯通知內容](adaptive-interactive-toasts.md)
* [原始通知](raw-notification-overview.md)
* [擱置中更新](toast-pending-update.md)
* [GitHub 上的 Notifications 程式庫 (屬於 Windows 社群工具組)](https://github.com/Microsoft/UWPCommunityToolkit/tree/master/Microsoft.Toolkit.Uwp.Notifications)
