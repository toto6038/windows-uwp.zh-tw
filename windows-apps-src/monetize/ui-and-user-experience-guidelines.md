---
author: Xansky
ms.assetid: 7a38a352-6e54-4949-87b1-992395a959fd
description: 了解關於在應用程式中廣告的 UI 和使用者體驗指導方針。
title: 廣告的 UI 和使用者體驗指導方針
ms.author: mhopkins
ms.date: 05/11/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, 廣告, 指導方針, 最佳做法
ms.localizationpriority: medium
ms.openlocfilehash: c7f5e762593773e529610989741274d9fb5b9be7
ms.sourcegitcommit: 72835733ec429a5deb6a11da4112336746e5e9cf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/19/2018
ms.locfileid: "5164671"
---
# <a name="ui-and-user-experience-guidelines-for-ads"></a>廣告的 UI 和使用者體驗指導方針

本文提供的指引可讓您在應用程式中提供絕佳的橫幅廣告、插播式廣告以及原生廣告體驗。 如需如何設計應用程式外觀與操作方式的一般指引，請參閱[設計與 UI](https://developer.microsoft.com/windows/apps/design)。

> [!IMPORTANT]
> 您應用程式中的任何廣告使用都必須符合 Microsoft Store 原則，包括但不限於[原則 10.10](https://docs.microsoft.com/legal/windows/agreements/store-policies#1010-advertising-conduct-and-content) (廣告行為與內容)。 尤其，您應用程式的橫幅或插入式廣告實作必須符合 Microsoft Store 原則[原則 10.10.1](https://docs.microsoft.com/legal/windows/agreements/store-policies#1010-advertising-conduct-and-content) 中的需求。 本文包含會違反此原則的實作範例。 這些範例僅用以提供資訊所用，協助您更加了解原則。 這些範例並非全面性，有許多其他方式也可能會違反 Microsoft Store 原則而未列於本文中。

## <a name="general-best-practices"></a>一般最佳做法

檢閱本文中不同廣告類型的指導方針之前，請先查看這些一般最佳做法，以提升您的廣告收益。

* [仔細計劃您的廣告位置](https://blogs.windows.com/buildingapps/2017/04/10/monetizing-app-advertisement-placement/)。 參閱[最佳化您的廣告單元的可見性](optimize-ad-unit-viewability.md)的相關指導方針。
* [作為插播式廣告做為插播式影片廣告的後援](https://blogs.windows.com/buildingapps/2017/04/17/monetizing-app-use-interstitial-banner-fallback-interstitial-video)。
* [了解您的使用者以提供更好的目標廣告](https://blogs.windows.com/buildingapps/2017/05/17/monetize-app-know-user-serve-better-targeted-ads/)。
* [使用最新的廣告程式庫](https://blogs.windows.com/buildingapps/2017/05/22/earn-money-moving-latest-advertising-libraries/)。
* [為您的應用程式設定正確的 COPPA 設定](https://blogs.windows.com/buildingapps/2017/06/21/monetizing-app-set-coppa-settings-app)。


## <a name="guidelines-for-banner-ads"></a>橫幅廣告指南

以下章節提供如何使用 [AdControl](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol) 在您的應用程式中實作[橫幅廣告](banner-ads.md)的建議，以及違反 Microsoft Store 原則[原則 10.10.1](https://docs.microsoft.com/legal/windows/agreements/store-policies#1010-advertising-conduct-and-content) 的實作範例。

### <a name="best-practices"></a>最佳做法

建議您在應用程式中實作橫幅廣告時，依照這些最佳做法：

* [使用美國互動廣告局大小](https://blogs.windows.com/buildingapps/2017/04/03/monetizing-app-use-interactive-advertising-bureau-ad-sizes)以容納於裝置的配置。

* 將您應用程式多數 UI 用於功能控制項及內容。

* 將廣告設計融入您的體驗中。 提供設計人員範例廣告，以規劃廣告的外觀。 應用程式中兩個良好規劃的廣告範例是「廣告即內容」配置和分割配置。

  若要在開發與測試階段查看不同廣告大小在您應用程式中的外觀與功能，您可以利用[測試廣告單位](set-up-ad-units-in-your-app.md#test-ad-units)。 完成測試後，請記得在提交應用程式進行認證之前，[使用即時廣告單位更新您的應用程式](set-up-ad-units-in-your-app.md#live-ad-units)。

* 針對沒有可用廣告的情況進行計畫。 有時候廣告可能無法傳送到您的應用程式。 請以無論是否展示廣告都能展現極佳外觀的方式，配置您的頁面。 如需詳細資訊，請參閱[處理廣告錯誤](error-handling-with-advertising-libraries.md)。

* 若您的警示使用者案例最好使用覆疊處理，請於顯示覆疊時呼叫 [AdControl.Suspend](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol.suspend)，接著在完成警示案例時呼叫 [AdControl.Resume](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol.resume)。

### <a name="practices-to-avoid"></a>應避免的做法

建議您在應用程式中實作橫幅廣告時，避免這些做法：

* 請勿將廣告閂入開放可用空間。 廣告空間不應該放進您可找到的第一塊開放空間。 相反地，它應該整合到您應用程式的整體設計中。

* 請勿使用過多廣告和塞滿應用程式。 若應用程式中有太多廣告，其外觀和可用性會受影響。 您想要透過廣告獲利，但不應犧牲應用程式本身。

* 請勿混淆使用者的核心工作。 主要的焦點應一律在應用程式上。 廣告空間應受到整合，以便讓它維持為次要焦點。

### <a name="examples-of-policy-violations"></a>原則違反範例

本章節提供會違反 Microsoft Store 原則[原則 10.10.1](https://docs.microsoft.com/legal/windows/agreements/store-policies#1010-advertising-conduct-and-content) 的橫幅廣告案例範例。 這些範例僅用以提供指引所用，協助您更加了解原則。 這些範例並非全面性，有需多其他方式也可能會違反原則 10.10.1 而未列於此。

* 竭盡所能影響使用者檢視橫幅廣告的能力，例如變更 [AdControl](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol) 的不透明度，或在  **AdControl** 上放置其他控制項 (而未先呼叫 [AdControl.Suspend](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol.suspend))。

* 要求使用者需按一下橫幅廣告才能在您的應用程式中完成工作，或將應用程式設計為強制使用者按一下橫幅廣告。

* 用任何方式略過橫幅廣告的內建最小重新整理計時器，包括但不限於交換 [AdControl](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol) 物件，或強制頁面在沒有使用者互動的情況下重新整理。

* 於開發及測試期間或模擬器中使用即時廣告單位 (意即您自 Windows 開發人員中心儀表板所取得的廣告單位)。

* 撰寫或散發的程式碼透過您應用程式中執行的 Microsoft 廣告程式庫以外的方式呼叫廣告服務。

* 與 Microsoft 廣告程式庫建立的未記載介面或子物件互動，例如 **WebView** 或 **MediaElement**。

<span id="interstitialbestpractices10" />

## <a name="guidelines-for-interstitial-ads"></a>插入式廣告指南

巧妙地使用[插播式廣告](interstitial-ads.md)可以大幅提高您應用程式的收益，而不會對使用者滿意的產生負面影響。 當使用不當時，這類廣告會有完全相反的效果。

以下章節提供如何使用 [InterstitialAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad) 在您的應用程式中實作插播式影片廣告及標準橫幅廣告的建議，以及違反 Microsoft Store 原則[原則 10.10.1](https://docs.microsoft.com/legal/windows/agreements/store-policies#1010-advertising-conduct-and-content) 的實作範例。 由於您比任何人都了解您的應用程式，除非原則考量，我們會將它保留給您來做出最佳的最終決策。 請務必牢記，您應用程式的評分與收益緊密結合。

### <a name="best-practices"></a>最佳做法

建議您在應用程式中實作插入式廣告時，依照這些最佳做法：

* 讓插入式廣告符合應用程式的自然流程 (例如在遊戲關卡之間)。

* 將廣告與好的一面關聯，例如：

    * 完成關卡的提示。

    * 重試關卡的額外時間。

    * 自訂虛擬人偶的特色，例如刺青或帽子。

* 如果您的應用程式必須看完插播式影片廣告，請先提到這項規則，如此使用者才不會對按下關閉按鈕時所發生的錯誤訊息感到意外。

* 在您需要顯示廣告前，預先擷取廣告 (藉由呼叫 [InterstitialAd.RequestAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad))，理想的情況為 30 秒到 60 秒。

* Subscribe to all four events exposed in the [InterstitialAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad) class (**Canceled**, **Completed**, **AdReady**, and **ErrorOccurred**) and use them to make the right decisions for your app.

* 有一些內建的體驗可以用來取代伺服器比對的廣告。 您會在以下的一些範例中發現這很有用：

    * 離線模式 (當無法連線到廣告伺服器時)。

    * When the **ErrorOccurred** event fires.

    * 如果您選擇根據 [ConnectionProfile](https://docs.microsoft.com/uwp/api/Windows.Networking.Connectivity.ConnectionProfile) 儲存使用者的頻寬時，在 **ConnectionProfile** 類別中有 API 可協助您。

* 使用預設 (30 秒) 逾時，除非您有合理的理由設定其他值，在該情況下不低於 10 秒。 插播式影片廣告比標準橫幅廣告需要更長的時間下載，特別是在沒有高速連線的市場中。

* 請留意使用者的行動數據方案。 例如，在接近/超過其行動數據方案的行動裝置上提供插播式影片廣告前，不要顯示或是警告使用者。 [ConnectionProfile](https://docs.microsoft.com/uwp/api/Windows.Networking.Connectivity.ConnectionProfile) 類別中有 API 可協助您。

* 在初始提交後，請繼續改善您的應用程式。 查看[廣告報表](../publish/advertising-performance-report.md)，然後變更設計以改進覆蓋率與插播式影片廣告完成率。

### <a name="practices-to-avoid"></a>應避免的做法

建議您在應用程式中實作插入式廣告時，避免這些做法：

* 請勿做過頭。 請勿強制廣告超過 5 分鐘，除非使用者明確地被遊戲外的選擇性好處吸引。

* 請勿在應用程式啟動時顯示插播式廣告，因為使用者可能會認為他們按到了錯誤的磚。

* 請勿在結束時顯示插播式廣告。 這是不好的編排，因為完成率會趨近於零。

* 請勿連續顯示二或多個插入式廣告。 使用者對於廣告進度列重設到開始點會感到不愉快。 許多人會認為這是程式碼或廣告服務錯誤。

* 在呼叫 [InterstitialAd.Show](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.show) 前，請勿擷取超過 5 分鐘的插播式影片廣告。 良好的編排會最大化預先擷取的廣告至可計費曝光數的轉換。

* 請勿對廣告服務失敗 (例如沒有可用廣告) 的使用者給予不利影響。 例如，如果您顯示 UI 選項 [觀看廣告以取得 *xxx*]，您應該在使用者這麼做之後提供 *xxx*。 要考慮的兩個選項︰

    * 除非引發 [InterstitialAd.AdReady](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.adready) 事件，否則不要包含該選項。

    * 讓包含內建體驗的應用程式可以產生與廣告相同的好處。

* 請勿使用插入式廣告讓使用者可在多人遊戲中取得競爭優勢。 例如，請勿透過在第一人稱射擊遊戲中提供更好的槍來引誘使用者檢視插入式廣告。 玩家虛擬人偶上的自訂上衣也不錯，只要不會提供隱身效果！

### <a name="examples-of-policy-violations"></a>原則違反範例

本章節提供會違反 Microsoft Store 原則[原則 10.10.1](https://docs.microsoft.com/legal/windows/agreements/store-policies#1010-advertising-conduct-and-content) 的插入式廣告案例範例。 這些範例僅用以提供指引所用，協助您更加了解原則。 這些範例並非全面性，有需多其他方式也可能會違反原則 10.10.1 而未列於此。

* 將 UI 元素置於插入式廣告容器。

* 在使用者與應用程式互動時呼叫 [InterstitialAd.Show](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.show)。

* 使用插入式廣告來取得任何可當作貨幣來消費或可與其他使用者交易的項目。

* Requesting a new interstitial ad in the context of the event handler for the [InterstitialAd.ErrorOccurred](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.erroroccurred) event. 這可能導致無限迴圈，而可能造成廣告服務發生操作問題。

* 僅為了讓瀑布式廣告序列有備用廣告而要求插入式廣告。 如果您要求插入式廣告，然後收到 [InterstitialAd.AdReady](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.adready) 事件，在應用程式中顯示的下一個插入式廣告就必須是已經準備好透過 [InterstitialAd.Show](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.show) 方法顯示的廣告。

* 於開發及測試期間或模擬器中使用即時廣告單位 (意即您自 Windows 開發人員中心儀表板所取得的廣告單位)。

* 撰寫或散發的程式碼透過您應用程式中執行的 Microsoft 廣告程式庫以外的方式呼叫廣告服務。

* 與 Microsoft 廣告程式庫建立的未記載介面或子物件互動，例如 **WebView** 或 **MediaElement**。

## <a name="guidelines-for-native-ads"></a>原生廣告指南

[原生廣告](native-ads.md)讓您對於如何呈現廣告給使用者擁有極大的掌控權。 請依照這些需求與指導方針，協助確保廣告商的訊息可以觸及您的使用者，同時也可協助避免提供令人混淆的原生廣告體驗給使用者。

### <a name="register-the-container-for-your-native-ad"></a>註冊原生廣告的容器

在您的程式碼中，您必須呼叫 [NativeAdV2](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.nativeadv2) 物件的 [RegisterAdContainer](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.nativeadv2.registeradcontainer) 方法來註冊做為原生廣告容器的 UI 元素，並可選擇您想要的任何特定控制項來註冊做為廣告的可點選目標。 這樣才能正確追蹤廣告的曝光次數和點擊數。

**RegisterAdContainer** 方法有兩個多載可供使用：

* 如果您希望所有個別原生廣告元素的整個容器都是可點選的，請呼叫 **RegisterAdContainer(FrameworkElement)** 方法並傳遞容器控制項給方法。 例如，如果您在全部裝載於 **StackPanel** 的不同控制項中顯示所有原生廣告元素，而您希望整個**StackPanel** 都是可點選的，那麼請傳遞 **StackPanel** 給此方法。

* 如果您只希望某些原生廣告元素是可點選的，請呼叫 **RegisterAdContainer (FrameworkElement、IVector(FrameworkElement))** 方法。 只有傳遞給第二個參數的控制項是可點選的。

### <a name="required-native-ad-elements"></a>必要的原生廣告元素

您至少必須一律顯示下列 [NativeAdV2](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.nativeadv2) 物件的屬性所提供的原生廣告元素給您原生廣告設計中的使用者。 如果未加入下列元素，您的廣告單元可能會績效不佳、收入低。

1. 一律顯示原生廣告標題 (在 **Title** 屬性中提供)。 提供可顯示至少 25 個字元的空間。 如果標題較長，以省略符號取代其他文字。
2. 一律顯示至少一個下列元素，協助區分原生廣告體驗和您應用程式的其餘部分，並明確呼叫廣告商提供的內容：
    * 可區分的*廣告*圖示 (在 **AdIcon** 屬性中提供)。 此圖示由 Microsoft 提供。
    * *贊助廠商*文字 (在 **SponsoredBy** 屬性中提供)。 此文字由廣告商提供。
    * 另一個顯示*贊助廠商*文字的方法是由您選擇顯示某些其他文字，以協助區分原生廣告體驗和您應用程式的其餘部分，例如「贊助內容」、「促銷內容」、「建議內容」等。

### <a name="user-experience"></a>使用者體驗

原生廣告應該要與您應用程式的其餘部分有明確界定，且周圍有空間以避免意外點按。 使用邊框、不同的背景或其他 UI 來區隔廣告內容和您應用程式的其餘部分。 請記住，意外點擊廣告長期下來對您的廣告收益或一般使用者體驗不會有幫助。

### <a name="description"></a>描述

如果您選擇要顯示廣告的描述 (於 **NativeAdV2** 物件的 **Description**屬性提供)，請提供至少可顯示 75 個字元的空間。 我們建議您使用動畫來顯示廣告描述的完整內容。

### <a name="call-to-action"></a>喚起行動

*喚起行動*文字 (於 **NativeAdV2** 物件的 **CallToAction** 屬性提供) 是廣告的關鍵元件。 如果您選擇要顯示此文字，請依照下列指導方針操作︰

* 一律在可點選控制項 (例如按鈕或超連結) 上顯示*喚起行動*文字給使用者。
* 一律完整顯示*喚起行動*文字。
* 確保*喚起行動*文字與廣告商的其餘促銷文字區隔開來。
