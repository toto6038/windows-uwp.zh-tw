---
author: joannaleecy
title: "遊戲的獲利"
description: "在 Windows10 上針對通用 Windows 平台 (UWP) 遊戲實作橫幅廣告、插入式影片廣告，及在應用程式內購買。"
ms.assetid: 79f4e177-d8e7-45d3-8a78-31d4c2fe298a
ms.author: joanlee
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Windows 10, UWP, 遊戲, 獲利"
ms.openlocfilehash: eccff6f037890fdd375eb150520db99a67aa718d
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
#  <a name="monetization-for-games"></a>遊戲的獲利

身為遊戲開發人員，您必須知道可行的獲利選項才能維持您的事業，並繼續做您充滿熱情的事：創作絕佳的遊戲。 本文提供通用 Windows 平台 (UWP) 遊戲獲利方法的概觀，以及實作它們的方法。

在過去，您會為遊戲標上價格，然後等著客人在商店購買它。 但現在您有其他選項。 您可以選擇在實體店面上架遊戲、線上銷售遊戲 (不論是實體版或軟體版)，或是讓每個人都能免費玩該遊戲，但加入一些廣告或可購買的遊戲內項目。 遊戲也不再只是獨立產品。 除了主要遊戲之外，它們通常還伴隨可購買的額外內容。 

您可以用下列其中一個或多個方法來宣傳 UWP 遊戲並透過它獲利：
* 將您的遊戲放在 Windows 市集，它是安全且提供[全球發佈](#worldwide-distribution-channel)的線上商店。 全世界的玩家都能[以您設定的價格](#set-a-price-for-your-game)在線上購買您的遊戲。
* 使用 Windows SDK 中的 API 建立[遊戲內購買](#in-game-purchases)。 玩家可以在您的遊戲內購買項目，或購買其他內容，例如額外的裝備、外觀、地圖或遊戲關卡。
* 使用 [Microsoft Store Services SDK](https://visualstudiogallery.msdn.microsoft.com/229b7858-2c6a-4073-886e-cbb79e851211) 中的 API 顯示來自廣告網路的廣告。 您可以在[遊戲中顯示廣告](#display-ads-in-your-game)，並提供選項讓玩家選擇以觀看影片廣告來交換遊戲內獎勵。
* [透過廣告活動將遊戲的潛力最大化](#maximize-your-games-potential-through-ad-campaigns)。 使用付費、社群 (免費)，或自家 (免費) 廣告活動來讓使用者數量成長。
 
## <a name="worldwide-distribution-channel"></a>全球的通路

Windows 市集可讓您的遊戲在全球超過 200 個國家與地區提供下載，且支援各種付款方式，包括 Visa、MasterCard 和 PayPal。 如需國家與地區的完整清單，請參閱[市場和自訂價格](https://msdn.microsoft.com/windows/uwp/publish/define-pricing-and-market-selection#markets-and-custom-prices)。

## <a name="set-a-price-for-your-game"></a>為您的遊戲設定價格 

發佈到市集的 UWP 遊戲可以是_付費_或_免費_的。 付費遊戲可讓玩家先以您設定的價格購買遊戲，免費遊戲則允許使用者下載並直接玩遊戲而不需要付費。

以下是有關您的遊戲在市集中之價格的重要概念。

### <a name="base-price"></a>基本價格 

遊戲的基本價格會決定您的遊戲是歸類為_付費_或_免費_。 您可以使用[開發人員中心儀表板](https://developer.microsoft.com/windows)來根據國家與地區設定基本價格。 決定價格的程序可能包括您[在不同國家銷售時的納稅義務](https://msdn.microsoft.com/windows/uwp/publish/tax-details-for-paid-apps)，以及[特定市場的成本考量](https://msdn.microsoft.com/windows/uwp/publish/define-pricing-and-market-selection#price-considerations-for-specific-markets)。 您也可以[針對特定市場設定自訂價格](https://msdn.microsoft.com/windows/uwp/publish/define-pricing-and-market-selection#markets-and-custom-prices)。 如需詳細資訊，請參閱[制定價格和選擇市場](https://msdn.microsoft.com/windows/uwp/publish/define-pricing-and-market-selection)。

### <a name="sale-price"></a>銷售價格

宣傳您遊戲的其中一個方法是在有限的一段時間內降低其價格。 您也可以將銷售價格設為__「免費」__使該遊戲可以免費下載而不需要付款。
您可以藉由事先設定好銷售的開始日期和結束日期，來排程銷售活動。 如需詳細資訊，請參閱[促銷 App 和附加元件](https://msdn.microsoft.com/windows/uwp/publish/put-apps-and-add-ons-on-sale)。

## <a name="in-game-purchases"></a>遊戲內購買

遊戲內購買是在遊戲中購買的產品。 它們一般通稱為 _App 內購買_。 在 Windows 市集中，這些產品稱為_附加元件_。 您可以透過 Windows 開發人員中心儀表板[發佈附加元件](https://msdn.microsoft.com/windows/uwp/publish/add-on-submissions)。 您也會需要在遊戲的程式碼中啟用附加元件。

### <a name="types-of-add-ons"></a>附加元件的類型

您在市集中可建立的附加元件有兩種：_耐久品_或_消費性產品_。 耐久品是可以持續一段指定時間的項目，且在它到期之前只能購買一次。 消費性產品是可以不斷重複購買的項目。

在建立消費性產品的時候，可以決定您想要追蹤它們的方式 &mdash; 亦即它們是_開發人員管理_或是_市集管理_(此功能自 Windows10 版本 1607 開始可供使用)。 對於開發人員管理的消費性產品，您有責任為玩家追蹤項目的餘額；對於市集管理的消費性產品，Windows 市集會為您追蹤項目的餘額。 如需詳細資訊，請參閱[消費性附加元件的概觀](https://msdn.microsoft.com/windows/uwp/monetize/enable-consumable-add-on-purchases#overview-of-consumable-add-ons)。

### <a name="create-in-game-purchases"></a>建立遊戲內購買

最新的 App 內購買和授權資訊 API 是 Windows SDK 中 [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) 命名空間的一部分 (自 Windows10 版本 1607 開始)。 如果您正在開發的新遊戲其目標是 1607 或更新版本，則我們建議您使用 __Windows.Services.Store__ 命名空間，因為它支援最新的附加元件類型且效能更好。
它也被設計成與未來的 Windows 開發人員中心和市集所支援的產品類型及功能相容。 當針對舊版的 Windows10 開發時，請改為使用 [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx) 命名空間。

如需詳細資訊，請移至 [App 內購買和試用版](https://msdn.microsoft.com/windows/uwp/monetize/in-app-purchases-and-trials)。

#### <a name="simplified-purchase-example"></a>簡化的購買範例

本節使用簡化的購買範例來說明使用不同方法呼叫實作購買流程的方式。

|遊戲內的動作 / 活動                                                | 遊戲的背景工作                  |
|--------------------------------------------------------------------------|----------------------------------------|
|玩家進入商店。 跳出商店功能表以顯示可取得的附加元件和購買價格 |  遊戲會[抓取附加元件的產品資訊](https://msdn.microsoft.com/windows/uwp/monetize/get-product-info-for-apps-and-add-ons)，[判斷附加元件是否有適當的授權](https://msdn.microsoft.com/windows/uwp/monetize/get-license-info-for-apps-and-add-ons)，並顯示可供玩家在商店功能表購買的附加元件。                           |
|玩家按一下 __\[購買\]__ 購買某個項目             |__「購買」__的動作會傳送要求以購買該項目，並開始付款程序以取得它。 實作會根據項目類型而有所不同。 如果它是[耐久品或一次性購買項目](https://msdn.microsoft.com/windows/uwp/monetize/enable-in-app-purchases-of-apps-and-add-ons)，則在它到期之前客戶只能擁有一個該項目。 如果它是[消費性產品](https://msdn.microsoft.com/windows/uwp/monetize/enable-consumable-add-on-purchases)，則客戶可以擁有一或多個該項目。 |

### <a name="test-in-game-purchases-during-game-development"></a>在遊戲開發期間測試遊戲內購買

因為附加元件必須與遊戲相關聯才能建立，所以您的遊戲必須已經發佈，且可在市集中取得。 本節中的步驟說明如何於遊戲仍在開發時就建立附加元件。
(如果完成的遊戲已經在市集上架，您可以略過前三個步驟直接到[在市集中建立附加元件](#create-an-add-on-in-the-store))。

遊戲仍在開發時就建立附加元件： 
1. [建立套件](#create-a-package)
2. [將遊戲以隱藏的方式發佈](#publish-the-game-as-hidden)
3. [將 Visual Studio 中的遊戲方案與市集建立關聯](#associate-your-game-solution-with-the-store)
4. [在市集中建立附加元件](#create-an-add-on-in-the-store)

#### <a name="create-a-package"></a>建立套件

對於任何將要發佈的遊戲，都必須符合「Windows 應用程式認證」的最低要求。 您可以使用 [Windows 應用程式認證套件](https://msdn.microsoft.com/windows/uwp/debug-test-perf/windows-app-certification-kit) (是 Windows10 SDK 的一部分)，針對遊戲執行測試以確認它隨時可發佈到市集。 如果您還沒下載包含 Windows 應用程式認證套件的 Windows10 SDK，請移至 [Windows10 SDK](https://developer.microsoft.com/windows/downloads/windows-10-sdk)。

建立可上傳到市集的檔案：

1. 在 Visual Studio 中開啟您的遊戲方案。
2. 在 Visual Studio 中，移至 __\[專案\]__ > __\[市集\]__ > __\[建立應用程式套件\]__
3. 針對 __\[您要建置套件以上傳到 Windows 市集嗎?\]__ 選取 __\[是\]__。
4. 登入您的「開發人員中心」的開發人員帳戶。 如果您還沒有開發人員帳戶，可以[註冊](https://developer.microsoft.com/store/register)一個。
5. 選取一個要建立上傳套件的 App。 如果您尚未建立 App 提交，請提供新的 App 名稱以建立新的提交。 如需詳細資訊，請參閱[透過保留名稱建立您的 App](https://msdn.microsoft.com/windows/uwp/publish/create-your-app-by-reserving-a-name)。
6. 成功建立套件之後，按一下 __\[啟動 Windows 應用程式認證套件\]__ 以開始測試程序。
7. 修正任何錯誤以建立遊戲套件。

#### <a name="publish-the-game-as-hidden"></a>將遊戲以隱藏的方式發佈

1. 移至[開發人員中心](https://developer.microsoft.com/store)並登入。
2. 從 __\[儀表板總覽\]__ 或 __\[所有應用程式\]__ 頁面，按一下您要處理的 App。 如果您尚未建立 App 提交，按一下 __\[建立新應用程式\]__ 並保留名稱。
3. 在 __\[應用程式概觀\]__ 頁面上，按一下 __\[開始您的提交\]__。
4. 設定這個新的提交。 在提交頁面上： 
    * 按一下 __\[定價和可用性\]__。 在 __\[可見度\]__ 區段中，選擇 __\[隱藏此應用程式並防止被取得\]__ 以確保只有您的開發團隊有該遊戲的存取權。 如需詳細資訊，請移至[配送和可見性](https://msdn.microsoft.com/windows/uwp/publish/set-app-pricing-and-availability#distribution-and-visibility)。
    * 按一下 __\[屬性\]__。 在 __\[類別和子類別\]__ 區段中，選擇 __\[遊戲\]__，再選取適合您遊戲的子類別。
    * 按一下 __\[年齡分級\]__。 準確地填寫問卷。
    * 按一下 __\[套件\]__。 上傳在先前步驟中建立的遊戲套件。
5. 遵循儀表板中的任何其他提交提示，您就能成功發佈該遊戲並讓它維持對大眾隱藏。
6. 按一下 __\[提交至市集\]__。

如需詳細資訊，請移至 [App 提交](https://msdn.microsoft.com/windows/uwp/publish/app-submissions)。

將遊戲提交至市集之後，它會進入[應用程式認證程序](https://msdn.microsoft.com/windows/uwp/publish/the-app-certification-process)。 在遊戲列出之前，這個程序最多可能需要 16 個小時。

#### <a name="associate-your-game-solution-with-the-store"></a>將遊戲方案與市集建立關聯

在 Visual Studio 中開啟您的遊戲方案：

1. 移至 __\[專案\]__ > __\[市集\]__ > __\[將應用程式與市集建立關聯\]__
2. 登入您的「開發人員中心」的開發人員帳戶，並選取要與這個方案關聯的 App 名稱。
3. 按兩下 __Package.appxmanifest.xml file__ 然後移至 __\[套件\]__ 索引標籤以檢查遊戲已正確建立關聯。

如果您已經將方案與已經在市集上架的遊戲建立關聯，您的方案將會有使用中的授權，且離為遊戲建立附加元件又更近一些。 如需詳細資訊，請參閱[封裝 App](https://msdn.microsoft.com/windows/uwp/packaging/index)。

#### <a name="create-an-add-on-in-the-store"></a>在市集中建立附加元件

在建立附加元件的時候，請確認您將他們與正確的遊戲提交建立關聯。 如需設定與附加元件關聯之各資訊的方法，請參閱[附加元件提交](https://msdn.microsoft.com/windows/uwp/publish/add-on-submissions)。

1. 移至[開發人員中心](https://developer.microsoft.com/store)並登入。
2. 從 __\[儀表板總覽\]__ 或 __\[所有應用程式\]__ 頁面，按一下您要建立附加元件的 App。
3. 在 __\[應用程式概觀\]__ 頁面上，於 __\[附加元件\]__ 區段中，選取 __\[建立新的附加元件\]__。
4. 選取附加元件的產品類型：__\[開發人員管理的消費性產品\]__、__\[市集管理的消費性產品\]__，或 __\[耐久品\]__。
5. 輸入唯一的產品識別碼，將此附加元件整合到遊戲程式碼中的時候，會用它當作字串變數。 消費者不會看到這個識別碼。 如需詳細資訊，請參閱[設定您的 App 產品類型與產品識別碼](https://msdn.microsoft.com/windows/uwp/publish/set-your-add-on-product-id)。

附加元件的其他設定包括：
* [屬性](https://msdn.microsoft.com/windows/uwp/publish/enter-add-on-properties)
* [定價和可用性](https://msdn.microsoft.com/windows/uwp/publish/set-add-on-pricing-and-availability)
* [市集清單](https://msdn.microsoft.com/windows/uwp/publish/create-add-on-store-listings)

如果您的遊戲有很多附加元件，您可以使用 __Windows 市集提交 API__ 以程式設計的方式建立它們。 如需詳細資訊，請參閱[使用 Windows 市集服務建立和管理提交](https://msdn.microsoft.com/windows/uwp/monetize/create-and-manage-submissions-using-windows-store-services)。

## <a name="display-ads-in-your-game"></a>在您的遊戲中顯示廣告

Microsoft Store Services SDK 中的程式庫和工具可協助您在遊戲中設定服務，以從其他廣告網路接收廣告。 您的玩家將會看到即時廣告，當您的玩家觀看或與顯示的廣告互動時，您將可以獲利。 如需詳細資訊，請參閱[建立包含廣告之 App 的工作流程](https://msdn.microsoft.com/windows/uwp/monetize/workflows-for-creating-apps-with-ads)。

### <a name="ad-formats"></a>廣告格式

使用 Microsoft Store Services SDK 可以顯示兩種類型的廣告：

* 橫幅廣告 &mdash; 廣告會占用一部分您的遊戲畫面且通常是置於遊戲中。
* 插入式影片廣告 &mdash; 全螢幕廣告，用於關卡之間非常有效。 若適當實作，他們會比橫幅廣告來得不突兀。

### <a name="which-ads-are-displayed"></a>顯示那些廣告？

使用 Microsoft Store Services SDK 時，廣告目前是透過合作夥伴網路提供。 如需目前供應內容的詳細資訊，請參閱[利用廣告從您的 App 獲利](https://developer.microsoft.com/store/monetize/ads-in-apps)。
如果您使用 AdControl 來顯示廣告，您可以選擇加入以顯示[聯盟廣告](https://msdn.microsoft.com/windows/uwp/publish/about-affiliate-ads)，藉此拓展您的遊戲中顯示的廣告。

### <a name="which-markets-allow-ads-to-be-displayed"></a>哪些市場允許顯示廣告？

於特定的國家可以向使用者顯示橫幅廣告和插入式影片廣告。 如需支援廣告之國家與地區的完整清單，請參閱 [Microsoft Advertising 支援的市場](https://msdn.microsoft.com/windows/uwp/monetize/supported-markets-for-microsoft-advertising)。

### <a name="apis-for-displaying-ads"></a>用來顯示廣告的 API

Microsoft Store Services SDK 中的 [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) 和 [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx)，及部分的 [Microsoft.Advertising.WinRT.UI](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.aspx) 命名空間都用來協助在遊戲中顯示廣告。

若要開始使用，請下載並安裝 [Microsoft Store Services SDK](http://aka.ms/store-em-sdk) 與 Visual Studio 2015。 如需詳細資訊，請參閱 [SDK 中提供的功能](https://msdn.microsoft.com/windows/uwp/monetize/microsoft-store-services-sdk#features-available-in-the-sdk)。

#### <a name="implementation-guides"></a>實作指南

這些逐步解說將示範如何使用 __AdControl__ 和 __InterstitialAd__ 實作廣告：

* [在 XAML 和 .NET 中使用 AdControl 類別建立橫幅廣告](https://msdn.microsoft.com/windows/uwp/monetize/adcontrol-in-xaml-and--net)
* [在 HTML5 和 JavaScript 中使用 AdControl 類別建立橫幅廣告](https://msdn.microsoft.com/windows/uwp/monetize/adcontrol-in-html-5-and-javascript)
* [使用 InterstitialAd 類別建立插入式影片廣告](https://msdn.microsoft.com/windows/uwp/monetize/interstitial-ads)

在開發期間，您可以利用這些測試值來查看廣告呈現的方式。 同樣的值也用於上述的逐步解說。

|AdType             | AdUnitId  | AppId                              |
|-------------------|-----------|------------------------------------|
|橫幅廣告         |10865270   |3f83fe91-d6be-434d-a0ae-7351c5a997f1|
|插入式廣告    |11389925   |d25517cb-12d4-4699-8bdc-52040c712cab|

以下是一些在設計與實作程序中有助於您的最佳做法。

* [使用 AdControl 類別的橫幅廣告最佳做法](https://msdn.microsoft.com/windows/uwp/monetize/ui-and-user-experience-guidelines)
* [使用 InterstitialAd 類別的插入式廣告最佳做法](https://msdn.microsoft.com/windows/uwp/monetize/ui-and-user-experience-guidelines#interstitialbestpractices10)

如需一般開發問題 (如未顯示廣告、閃爍和消失的黑色方塊，或廣告未重新整理) 的解決方案，請參閱[疑難排解指南](https://msdn.microsoft.com/windows/uwp/monetize/troubleshooting-guides)。

### <a name="prepare-for-release-by-replacing-ad-unit-test-values"></a>取代廣告單元測試值以準備發行

當您準備好要進行即時測試或在已發佈的遊戲中接收廣告時，您必須將測試的廣告單元值更新為針對您的遊戲提供的實際值。
若要為您的遊戲建立廣告單元，請參閱[在您的 App 中設定廣告單元](https://msdn.microsoft.com/windows/uwp/monetize/set-up-ad-units-in-your-app)。

### <a name="other-ad-networks"></a>其他廣告網路

這些是支援向 UWP App 和遊戲提供廣告的其他廣告網路。

#### <a name="vungle"></a>Vungle

適用於 Windows 的 Vungle SDK 提供 App 內和遊戲內的影片廣告。 若要下載 SDK，請移至 [Vungle SDK](https://v.vungle.com/sdk)。

#### <a name="smaato"></a>Smaato

Smaato 可讓您將橫幅廣告整合到 UWP App 和遊戲。 下載 [SDK](https://www.smaato.com/resources/sdks/)，如需詳細資訊，請參閱[文件](https://wiki.smaato.com/display/SPX/Windows+Phone) (英文)。

#### <a name="adduplex"></a>AdDuplex

您可以使用 AdDuplex 在遊戲中實作橫幅廣告或插入式廣告。

如需直接將 AdDuplex 整合到 Windows10 XAML 專案的詳細資訊，請移至 AdDuplex 網站：
* 橫幅廣告：[適用於 XAML 的 Windows10 SDK](https://adduplex.zendesk.com/hc/en-us/articles/204849031-Windows-10-SDK-for-XAML-apps-installation-and-usage) (英文) 
* 插入式廣告：[Windows10 XAML AdDuplex 插入式廣告的安裝與用法](https://adduplex.zendesk.com/hc/en-us/articles/204849091-Windows-10-XAML-AdDuplex-Interstitial-Ad-Installation-and-Usage) (英文)

如需將 AdDuplex SDK 整合到使用 Unity 建立的 Windows10 UWP 遊戲中的詳細資訊，請參閱 [適用於 Unity App 之 Windows10 SDK 的安裝與用法](https://adduplex.zendesk.com/hc/en-us/articles/207279435-Windows-10-SDK-for-Unity-apps-installation-and-usage) (英文)。

## <a name="maximize-your-games-potential-through-ad-campaigns"></a>透過廣告活動將遊戲的潛力最大化

針對使用廣告宣傳您的遊戲採取進一步的步驟。 當您為遊戲[建立廣告活動](https://msdn.microsoft.com/windows/uwp/publish/create-an-ad-campaign-for-your-app)時，其他遊戲就會顯示宣傳您的遊戲的廣告。 

從數種類型中選擇能協助增加遊戲玩家族群的活動類型。

|活動類型             | 您遊戲的廣告出現在...                                                                                                                                                                   |
|--------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|付費                      |符合您遊戲的裝置或類別的 App。                                                                                                                                                   |
|免費社群               |其他同為加入社群廣告活動的開發人員所發佈的 App。 如需詳細資訊，請參閱[關於社群廣告](https://msdn.microsoft.com/windows/uwp/publish/about-community-ads)。|
|自家免費                   |僅限您已經發佈的 App。 如需詳細資訊，請參閱[關於自家廣告](https://msdn.microsoft.com/windows/uwp/publish/about-house-ads)。                                                            |

## <a name="related-links"></a>相關連結

* [獲得報酬](https://msdn.microsoft.com/windows/uwp/publish/getting-paid-apps)
* [帳戶類型、位置和費用](https://msdn.microsoft.com/windows/uwp/publish/account-types-locations-and-fees)
* [分析](https://msdn.microsoft.com/windows/uwp/publish/analytics)
* [全球化與當地語系化](https://msdn.microsoft.com/windows/uwp/globalizing/globalizing-portal)
* [實作 App 的試用版](https://msdn.microsoft.com/windows/uwp/monetize/implement-a-trial-version-of-your-app)
* [使用 A/B 測試執行 App 實驗](https://msdn.microsoft.com/windows/uwp/monetize/run-app-experiments-with-a-b-testing)