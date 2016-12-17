---
author: mcleanbyron
ms.assetid: 7a38a352-6e54-4949-87b1-992395a959fd
description: "了解關於在 App 中廣告的 UI 和使用者體驗指導方針。"
title: "在 App 中廣告的 UI 和使用者體驗指導方針。"
translationtype: Human Translation
ms.sourcegitcommit: 148aca16104f599f3048f5965c4131a3f37799f8
ms.openlocfilehash: 97feb4f79e0592a7b54a8263b15cd2b85dd3243d


---

# <a name="ui-and-user-experience-guidelines-for-ads-in-apps"></a>在 App 中廣告的 UI 和使用者體驗指導方針。


## <a name="general-ui-resources-for-windows-apps"></a>Windows 應用程式的一般 UI 資源

您可以在[設計與 UI](https://developer.microsoft.com/windows/design) 中找到關於如何設計 App 外觀與操作方式的資訊。

## <a name="adcontrol-best-practices"></a>AdControl 最佳做法

* [AdControl 最佳做法：可行事項](#adcontrolbestpracticesdo10)
* [AdControl 最佳做法：禁止事項](#adcontrolbestpracticesdont10)

<span id="adcontrolbestpracticesdo10"/>
### <a name="adcontrol-best-practices-do"></a>AdControl 最佳做法：可行事項

* 將廣告設計融入您的體驗中。 提供設計人員範例廣告，以規劃廣告的外觀。 App 中兩個良好規劃的廣告範例是「廣告即內容」配置和分割配置。

  若要查看不同廣告大小在您 App 中的外觀與功能，您可以利用適用於 Windows Phone、Windows 8.1 和 Windows 10 的測試模式廣告單位。 完成使用測試模式廣告單元時，請記得在提交 App 進行認證之前，[使用真正的廣告單元識別碼更新您的 App](set-up-ad-units-in-your-app.md)。

* 針對沒有可用廣告的情況進行計畫。 有時候廣告可能無法傳送到您的 App。 請以無論是否展示廣告都能展現極佳外觀的方式，配置您的頁面。 如需詳細資訊，請參閱[錯誤處理](error-handling-with-advertising-libraries.md)。

<span id="adcontrolbestpracticesdont10"/>
### <a name="adcontrol-best-practices-dont"></a>AdControl 最佳做法：禁止事項

* 將廣告閂入開放可用空間。 廣告空間不應該放進您可找到的第一塊開放空間。 相反地，它應該整合到您 App 的整體設計中。

* 過多廣告和塞滿 App。 在 App 中有太多廣告會影響其外觀和可用性。 您想要透過廣告獲利，但不應犧牲 App 本身。

* 混淆使用者的核心工作。 主要的焦點應一律在 App 上。 廣告空間應受到整合，以便讓它維持為次要焦點。

<span id="interstitialbestpractices10"/>
## <a name="interstitial-best-practices-and-policies"></a>插入式廣告最佳做法與原則

* [插入式廣告最佳做法：可行事項](#interstitialbestpracticesdo10)
* [插入式廣告最佳做法：避免事項](#interstitialbestpracticesavoid10)
* [插入式廣告最佳做法：禁止事項 (原則強制執行)](#interstitialbestpracticesnever10)

巧妙地使用影片插入式廣告可以大幅提高您 App 的收益，而不會對使用者滿意的產生負面影響。 當使用不當時，這類廣告會有完全相反的效果。

在這裡我們將協助您達成目標。 由於您比任何人都了解您的 App，除非原則考量，我們會將它保留給您來做出最佳的最終決策。 請務必牢記，您 App 的評等與收益緊密結合。

<span id="interstitialbestpracticesdo10"/>
### <a name="interstitial-best-practices-do"></a>插入式廣告最佳做法：可行事項

* 讓插入式廣告符合 App 的自然流程 (例如在遊戲關卡之間)。

* 將廣告與好的一面關聯，例如：

    * 完成關卡的提示。

    * 重試關卡的額外時間。

    * 自訂虛擬人偶的功能，例如刺青或帽子。

* 如果您的 App 必須看完影片廣告，請先提到這項規則，如此使用者才不會對按下關閉按鈕時所發生的錯誤訊息感到意外。

* 在您需要顯示廣告前，預先擷取廣告 (藉由呼叫 [RequestAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.requestad.aspx))，理想的情況為 30 秒到 60 秒。

* 訂閱在 [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) 類別中公開的四個事件 (**Canceled**、**Completed**、**AdReady** 和 **ErrorOccurred**) 並使用它們來為 App 做出正確的決策。

* 有一些內建的體驗可以用來取代伺服器比對的廣告。 您會在以下的一些範例中發現這很有用：

    * 離線模式 (當無法連線到廣告伺服器時)。

    * 當引發 **ErrorOccurred** 事件時。

    * 如果您選擇根據 [ConnectionProfile](https://msdn.microsoft.com/library/windows/apps/windows.networking.connectivity.connectionprofile.aspx) 儲存使用者的頻寬時，在 **ConnectionProfile** 類別中有 API 可協助您。

* 使用預設 (30 秒) 逾時，除非您有合理的理由設定其他值，在該情況下不低於 10 秒。

    * 影片廣告比橫幅廣告需要更長的時間下載，特別是在沒有高速連線的市場中。

<span/>

* 請留意使用者的行動數據方案。 例如，在接近/超過其行動數據方案的行動裝置上提供影片廣告前，不要顯示或是警告使用者。 [ConnectionProfile](https://msdn.microsoft.com/library/windows/apps/windows.networking.connectivity.connectionprofile.aspx) 類別中有 API 可協助您。

* 在初始提交後，請繼續改善您的 App。 查看廣告報表，然後變更設計以改進覆蓋率與影片完成率。

<span id="interstitialbestpracticesavoid10"/>
### <a name="interstitial-best-practices-avoid"></a>插入式廣告最佳做法：避免事項

* 使用過度。 請勿強制廣告超過 5 分鐘，除非使用者明確地被遊戲外的選擇性好處吸引。

* 在 App 啟動時插入影片，因為使用者可能會認為他們按到了錯誤的磚。

* 在離開時插入影片。 這是不好的編排，因為完成率會趨近於零。

    * 兩個或多個接續的影片廣告。

    * 使用者對於廣告進度列重設到開始點會感到不愉快。

    * 許多人會認為這是程式碼或廣告服務錯誤。

* 在呼叫 [Show](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.show.aspx) 之前，請最少提早 5 分鐘開便開始擷取影片廣告。

  * 良好的編排會最大化預先擷取的廣告至可計費曝光數的轉換。

<span/>

* 對廣告服務失敗 (例如沒有可用廣告) 的使用者給予不利影響。 例如，如果您顯示 UI 選項 [觀看廣告已取得 *xxx*]，您應該在使用者這麼做之後提供 *xxx*。 要考慮的兩個選項︰

    * 除非引發 [AdReady](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.adready.aspx) 事件，否則不要包含該選項。

    * 讓包含內建體驗的 App 可以產生與廣告相同的好處。

* 讓使用者可以在多人遊戲中獲得競爭優勢。

    * 在第一人稱射擊遊戲中，一把更好的槍就很明顯地屬於此類。

    * 玩家虛擬人偶上的自訂上衣也不錯，只要不會提供隱身效果！

<span id="interstitialbestpracticesnever10"/>
### <a name="interstitial-best-practices-never-policy-enforced"></a>插入式廣告最佳做法：禁止事項 (原則強制執行)

* 一律不要將任何 UI 元素放在廣告容器上。

    * 廣告商已支付全螢幕費用。

<span/>

* 一律不要在使用者與 App 互動時呼叫 [Show](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.show.aspx)。

    * 因為 [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) 將會建立全螢幕重疊顯示畫面，使用者會覺得突兀。

    * 這也會導致點閱率誇大不實。

* 一律不要使用廣告來取得任何可當作貨幣來消費或可與其他使用者交易的項目。

* 一律不要在 [ErrorOccurred](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.erroroccurred.aspx) 事件的事件處理常式內容中要求新的廣告。 這可能導致無限迴圈，而可能造成廣告服務發生操作問題。

* 一律不要只是為了讓瀑布式廣告序列有備用廣告而要求插入式廣告。 如果您要求插入式廣告，然後收到 [AdReady](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.adready.aspx) 事件，在 App 中顯示的下一個插入式廣告就必須是已經準備好透過 [Show](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.show.aspx) 方法顯示的廣告。

 

 



<!--HONumber=Dec16_HO1-->


