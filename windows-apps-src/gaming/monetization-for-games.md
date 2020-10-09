---
title: 遊戲的獲利
description: 在 Windows 10 上針對通用 Windows 平台 (UWP) 遊戲實作橫幅廣告、插入式影片廣告，及在應用程式內購買。
ms.assetid: 79f4e177-d8e7-45d3-8a78-31d4c2fe298a
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, 遊戲, 獲利
ms.localizationpriority: medium
ms.openlocfilehash: c827c257947ea0f365bafe497e627841b501d40d
ms.sourcegitcommit: 5d84d8fe60e83647fa363b710916cf8b92c6e331
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/09/2020
ms.locfileid: "91878581"
---
#  <a name="monetization-for-games"></a>遊戲的獲利

身為遊戲開發人員，您必須知道可行的獲利選項才能維持您的事業，並繼續做您充滿熱情的事：創作絕佳的遊戲。 本文提供通用 Windows 平台 (UWP) 遊戲獲利方法的概觀，以及實作它們的方法。

在過去，您會為遊戲標上價格，然後等著客人在商店購買它。 但現在您有其他選項。 您可以選擇在實體店面上架遊戲、線上銷售遊戲 (不論是實體版或軟體版)，或是讓每個人都能免費玩該遊戲，但加入一些廣告或可購買的遊戲內項目。 遊戲也不再只是獨立產品。 除了主要遊戲之外，它們通常還伴隨可購買的額外內容。

您可以用下列其中一個或多個方法來宣傳 UWP 遊戲並透過它獲利：
* 將您的遊戲放在 Microsoft Store，這是安全的線上商店，提供 [全球散發](#worldwide-distribution-channel)。 全世界的玩家都能[以您設定的價格](#set-a-price-for-your-game)在線上購買您的遊戲。
* 使用 Windows SDK 中的 API 建立[遊戲內購買](#in-game-purchases)。 玩家可以在您的遊戲內購買項目，或購買其他內容，例如額外的裝備、外觀、地圖或遊戲關卡。
* 使用 [Microsoft Advertising SDK](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK) 中的 API 顯示來自廣告網路的廣告。 您可以在[遊戲中顯示廣告](#display-ads-in-your-game)，並提供選項讓玩家選擇以觀看影片廣告來交換遊戲內獎勵。
* [透過廣告活動最大化您的遊戲潛力](#maximize-your-games-potential-through-ad-campaigns)。 使用付費、社群 (免費)，或自家 (免費) 廣告活動來讓使用者數量成長。

## <a name="worldwide-distribution-channel"></a>全球的通路

此 Microsoft Store 可讓您的遊戲在全球超過200個國家和地區進行下載，並支援透過各種形式的付款（包括簽證、MasterCard 和 PayPal）進行計費。 如需國家/地區和區域的完整清單，請參閱 [定義市場選擇](../publish/define-market-selection.md)。

## <a name="set-a-price-for-your-game"></a>為您的遊戲設定價格

發佈至商店的 UWP 遊戲可以是 _付費_ 或 _免費_。 付費遊戲可讓玩家先以您設定的價格購買遊戲，免費遊戲則允許使用者下載並直接玩遊戲而不需要付費。

以下是有關您的遊戲在市集中之價格的重要概念。

### <a name="base-price"></a>基本價格

遊戲的基本價格取決於您的遊戲是否分類為 _付費_ 或 _免費_。 您可以使用 [合作夥伴中心](https://partner.microsoft.com/dashboard) 根據國家/地區和地區設定基本價格。
決定價格的程序可能包括您[在不同國家銷售時的納稅義務](../publish/tax-details-for-paid-apps.md)，以及[特定市場的成本考量](../publish/define-market-selection.md)。 您也可以[針對特定市場設定自訂價格](../publish/set-and-schedule-app-pricing.md#override-base-price-for-specific-markets)。

### <a name="sale-price"></a>銷售價格

宣傳您遊戲的其中一個方法是在有限的一段時間內降低其價格。 您也可以將銷售價格設定為 [ __免費__ ]，以允許您的遊戲在不付款的情況下下載。
您可以藉由事先設定好銷售的開始日期和結束日期，來排程銷售活動。 如需詳細資訊，請參閱[促銷應用程式和附加元件](../publish/put-apps-and-add-ons-on-sale.md)。

## <a name="in-game-purchases"></a>遊戲內購買

遊戲內購買是在遊戲中購買的產品。 它們一般通稱為 _App 內購買_。 在 Microsoft Store 中，這些產品稱為 _附加_元件。 [附加元件會](../publish/add-on-submissions.md) 透過合作夥伴中心發佈。 您也會需要在遊戲的程式碼中啟用附加元件。

### <a name="types-of-add-ons"></a>附加元件的類型

您可以在存放區中建立兩種類型的附加元件： _durables_ 或 _耗材_。 耐久品是可以持續一段指定時間的項目，且在它到期之前只能購買一次。 消費性產品是可以不斷重複購買的項目。

建立耗材時，請決定您要如何追蹤其是否為 &mdash; _開發人員管理_ 或 _儲存受控_ (這項功能從 Windows 10 1607 版) 開始提供。 使用開發人員管理的取用，您必須負責追蹤玩家的專案餘額。使用存放區管理的取用時，Microsoft Store 會持續追蹤專案的餘額。 如需詳細資訊，請參閱[消費性附加元件的概觀](../monetize/enable-consumable-add-on-purchases.md)。

### <a name="create-in-game-purchases"></a>建立遊戲內購買

最新的 App 內購買和授權資訊 API 是 Windows SDK 中 [Windows.Services.Store](/uwp/api/windows.services.store) 命名空間的一部分 (自 Windows 10 版本 1607 開始)。 如果您正在開發的新遊戲其目標是 1607 或更新版本，則我們建議您使用 __Windows.Services.Store__ 命名空間，因為它支援最新的附加元件類型且效能更好。
它也設計成與合作夥伴中心和商店所支援的未來產品和功能類型相容。 當針對舊版的 Windows 10 開發時，請改為使用 [Windows.ApplicationModel.Store](/uwp/api/windows.applicationmodel.store) 命名空間。

如需詳細資訊，請移至 [App 內購買和試用版](../monetize/in-app-purchases-and-trials.md)。

#### <a name="simplified-purchase-example"></a>簡化的購買範例

本節使用簡化的購買範例來說明使用不同方法呼叫實作購買流程的方式。

|遊戲內的動作 / 活動                                                | 遊戲的背景工作                  |
|--------------------------------------------------------------------------|----------------------------------------|
|玩家進入商店。 跳出商店功能表以顯示可取得的附加元件和購買價格 |  遊戲會[抓取附加元件的產品資訊](../monetize/get-product-info-for-apps-and-add-ons.md)，[判斷附加元件是否有適當的授權](../monetize/get-license-info-for-apps-and-add-ons.md)，並顯示可供玩家在商店功能表購買的附加元件。                           |
|玩家按一下 __購買__ 即可購買專案             |__購買__動作會傳送要求以購買專案，並開始付款程式來取得它。 實作會根據項目類型而有所不同。 如果它是[耐久品或一次性購買項目](../monetize/enable-in-app-purchases-of-apps-and-add-ons.md)，則在它到期之前客戶只能擁有一個該項目。 如果它是[消費性產品](../monetize/enable-consumable-add-on-purchases.md)，則客戶可以擁有一或多個該項目。 |

### <a name="test-in-game-purchases-during-game-development"></a>在遊戲開發期間測試遊戲內購買

因為附加元件必須與遊戲相關聯才能建立，所以您的遊戲必須已經發佈，且可在市集中取得。 本節中的步驟說明如何於遊戲仍在開發時就建立附加元件。
(如果完成的遊戲已經在市集上架，您可以略過前三個步驟直接到[在市集中建立附加元件](#create-an-add-on-in-the-store))。

遊戲仍在開發時就建立附加元件：
1. [建立套件](#create-a-package)
2. [將遊戲以隱藏的方式發佈](#publish-the-game-as-hidden)
3. [將 Visual Studio 中的遊戲方案與市集建立關聯](#associate-your-game-solution-with-the-store)
4. [在市集中建立附加元件](#create-an-add-on-in-the-store)

#### <a name="create-a-package"></a>建立套件

對於任何將要發佈的遊戲，都必須符合「Windows 應用程式認證」的最低要求。 您可以使用 [Windows 應用程式認證套件](../debug-test-perf/windows-app-certification-kit.md) (是 Windows 10 SDK 的一部分)，針對遊戲執行測試以確認它隨時可發佈到市集。 如果您還沒下載包含 Windows 應用程式認證套件的 Windows 10 SDK，請移至 [Windows 10 SDK](https://developer.microsoft.com/windows/downloads/windows-10-sdk)。

建立可上傳到市集的檔案：

1. 在 Visual Studio 中開啟您的遊戲方案。
2. 在 Visual Studio 中，移至__Project__  >  __Store__的 [  >  __建立應用程式套件__]。
3. 針對 [ __您要建立封裝以上傳至 Microsoft Store 嗎？__ ] 選項，選取 __[是]__。
4. 登入您的 [合作夥伴中心](https://partner.microsoft.com/dashboard) 開發人員帳戶。 如果您還沒有開發人員帳戶，可以[註冊](https://developer.microsoft.com/store/register)一個。
5. 選取一個要建立上傳套件的 App。 如果您尚未建立 App 提交，請提供新的 App 名稱以建立新的提交。 如需詳細資訊，請參閱[透過保留名稱建立您的 App](../publish/create-your-app-by-reserving-a-name.md)。
6. 成功建立封裝之後，請按一下 [ __啟動 Windows 應用程式認證套件__ 啟動測試程式。
7. 修正任何錯誤以建立遊戲套件。

#### <a name="publish-the-game-as-hidden"></a>將遊戲以隱藏的方式發佈

1. 移至 [合作夥伴中心](https://partner.microsoft.com/dashboard) 並登入。
2. 從 __儀表板總覽__ 或 __所有應用程式__ ] 頁面中，按一下您想要使用的應用程式。 如果您尚未建立應用程式提交，請按一下 [ __建立新的應用程式__ ] 並保留名稱。
3. 在 [ __應用程式總覽__ ] 頁面上，按一下 [ __開始提交__]。
4. 設定這個新的提交。 在提交頁面上：
    * 按一下 [ __定價和可用性__]。 在 [ __可見度__ ] 區段中，選擇 [__隱藏此應用程式並防止取得 ...__]，以確保只有您的開發小組可以存取該遊戲。 如需詳細資訊，請移至[配送和可見性](../publish/set-app-pricing-and-availability.md)。
    * 按一下 __[屬性]__ 。 在 [ __類別目錄和子類別目錄__ ] 區段中，選擇 [ __遊戲__ ]，然後選擇適合您遊戲的子類別。
    * 按一下 [ __年齡分級__]。 準確地填寫問卷。
    * 按一下 [ __封裝__]。 上傳在先前步驟中建立的遊戲套件。
5. 遵循儀表板中的任何其他提交提示，您就能成功發佈該遊戲並讓它維持對大眾隱藏。
6. 按一下 __[提交至存放區__]。

如需詳細資訊，請移至 [App 提交](../publish/app-submissions.md)。

將遊戲提交至市集之後，它會進入[應用程式認證程序](../publish/the-app-certification-process.md)。 在遊戲列出之前，這個程序最多可能需要 16 個小時。

#### <a name="associate-your-game-solution-with-the-store"></a>將遊戲方案與市集建立關聯

在 Visual Studio 中開啟您的遊戲方案：

1. 前往__Project__  >  __store__  >  __將應用程式與存放區建立關聯__.。。
2. 登入您的合作夥伴中心開發人員帳戶，然後選取要與此方案產生關聯的應用程式名稱。
3. 按兩下 __Package.appxmanifest.xml__ 檔案，然後移至 [ __封裝__ ] 索引標籤，檢查是否已正確關聯該遊戲。

如果您已經將方案與已經在市集上架的遊戲建立關聯，您的方案將會有使用中的授權，且離為遊戲建立附加元件又更近一些。 如需詳細資訊，請參閱[封裝 app](../packaging/index.md)。

#### <a name="create-an-add-on-in-the-store"></a>在市集中建立附加元件

在建立附加元件的時候，請確認您將他們與正確的遊戲提交建立關聯。 如需設定與附加元件關聯之各資訊的方法，請參閱[附加元件提交](../publish/add-on-submissions.md)。

1. 移至 [合作夥伴中心](https://partner.microsoft.com/dashboard) 並登入。
2. 從 __儀表板總覽__ 或 __所有應用程式__ 頁面，按一下您想要為其建立附加元件的應用程式。
3. 在 [ __應用程式總覽__ ] 頁面的 [ __附加__ 元件] 區段中，選取 [ __建立新的附加__元件]。
4. 選取附加元件的產品類型： __由開發人員管理__的取用、 __商店管理的__取用或 __持久__。
5. 輸入唯一的產品識別碼，將此附加元件整合到遊戲程式碼中的時候，會用它當作字串變數。 消費者不會看到這個識別碼。 如需詳細資訊，請參閱[設定您的 App 產品類型與產品識別碼](../publish/set-your-add-on-product-id.md)。

附加元件的其他設定包括：
* [屬性](../publish/enter-add-on-properties.md)
* [價格與可用性](../publish/set-add-on-pricing-and-availability.md)
* [Store 清單](../publish/create-add-on-store-listings.md)

如果您的遊戲有許多附加元件，您可以使用 __Microsoft Store 提交 API__以程式設計方式建立它們。 如需詳細資訊，請參閱 [使用 Microsoft Store 服務來建立和管理提交](../monetize/create-and-manage-submissions-using-windows-store-services.md)。

## <a name="display-ads-in-your-game"></a>在您的遊戲中顯示廣告

Microsoft Advertising SDK 中的程式庫和工具可協助您在遊戲中設定服務，以從其他廣告網路接收廣告。 您的玩家將會看到即時廣告，當您的玩家觀看或與顯示的廣告互動時，您將可以獲利。
如需詳細資訊，請參閱[在您的應用程式中顯示廣告](../monetize/display-ads-in-your-app.md)。

### <a name="ad-formats"></a>廣告格式

使用 Microsoft Advertising SDK 可以顯示數種類型的廣告：

* 橫幅廣告 &mdash; 廣告會占用一部分您的遊戲畫面且通常是置於遊戲中。
* 插入式影片廣告 &mdash; 全螢幕廣告，用於關卡之間非常有效。 若適當實作，他們會比橫幅廣告來得不突兀。
* 原生廣告 &mdash; 以元件為基礎的廣告，其中每一項廣告創意 (例如標題、影像、描述和喚起行動文字) 都會當做可整合到您應用程式的個別元素傳送到您的應用程式。

### <a name="which-ads-are-displayed"></a>顯示那些廣告？

根據預設，您的應用程式會顯示 Microsoft 付款廣告網路中的廣告。 若要讓您有最佳的廣告收入，您可以為您的廣告單元啟用廣告流量分配，以顯示其他付費廣告網路的廣告。 如需目前供應內容的詳細資訊，請參閱我們的[廣告流量分配](../publish/in-app-ads.md#mediation)指導方針。

### <a name="which-markets-allow-ads-to-be-displayed"></a>哪些市場允許顯示廣告？

如需支援廣告之國家與地區的完整清單，請參閱[支援的廣告網路市場](../publish/in-app-ads.md#network-markets)。

### <a name="apis-for-displaying-ads"></a>用來顯示廣告的 API

Microsoft Advertising SDK 中的 [AdControl](/uwp/api/microsoft.advertising.winrt.ui.adcontrol)、[InterstitialAd](/uwp/api/microsoft.advertising.winrt.ui.interstitialad) 和 [NativeAd](/uwp/api/microsoft.advertising.winrt.ui.nativead) 類別用於協助在遊戲中顯示廣告。

若要開始使用，請下載並安裝隨附 Visual Studio 2015 或更新版本的 [Microsoft Advertising SDK](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK)。 如需詳細資訊，請參閱[安裝 Microsoft Advertising SDK](../monetize/install-the-microsoft-advertising-libraries.md)。

#### <a name="implementation-guides"></a>實作指南

這些逐步解說將示範如何使用 __AdControl__、__InterstitialAd__ 和 __NativeAd__ 實作廣告：

* [在 XAML 與 .NET 中建立橫幅廣告](../monetize/adcontrol-in-xaml-and--net.md)
* [在 HTML5 與 JavaScript 中建立橫幅廣告](../monetize/adcontrol-in-html-5-and-javascript.md)
* [建立插播式廣告](../monetize/interstitial-ads.md)
* [建立原生廣告](../monetize/native-ads.md)

在開發期間，您可以使用[測試廣告單元值](../monetize/set-up-ad-units-in-your-app.md) 來查看廣告呈現的方式。 這些測試廣告單元值也用於上述的逐步解說。

以下是一些在設計與實作程序中有助於您的最佳做法。

* [橫幅廣告的最佳做法](../monetize/ui-and-user-experience-guidelines.md)
* [插播式廣告的最佳做法](../monetize/ui-and-user-experience-guidelines.md)

如需一般開發問題 (如未顯示廣告、閃爍和消失的黑色方塊，或廣告未重新整理) 的解決方案，請參閱[疑難排解指南](../monetize/known-issues-for-the-advertising-libraries.md)。

### <a name="prepare-for-release-by-replacing-ad-unit-test-values"></a>取代廣告單元測試值以準備發行

當您準備好要進行即時測試或在已發佈的遊戲中接收廣告時，您必須將測試的廣告單元值更新為針對您的遊戲提供的實際值。 若要為您的遊戲建立廣告單元，請參閱[在您的 App 中設定廣告單元](../monetize/set-up-ad-units-in-your-app.md)。

### <a name="other-ad-networks"></a>其他廣告網路

這些是提供 SDK 以向 UWP 應用程式和遊戲提供廣告的其他廣告網路。

#### <a name="vungle"></a>Vungle

適用於 Windows 的 Vungle SDK 提供 App 內和遊戲內的影片廣告。 若要下載 SDK，請移至 [Vungle SDK](https://publisher.vungle.com/sdk/)。

#### <a name="smaato"></a>Smaato

Smaato 可讓您將橫幅廣告整合到 UWP App 和遊戲。 下載 [SDK](https://www.smaato.com/resources/sdks/)，如需詳細資訊，請參閱[文件](https://wiki.smaato.com/display/SPX/Windows+Phone) (英文)。

#### <a name="adduplex"></a>AdDuplex

您可以使用 AdDuplex 在遊戲中實作橫幅廣告或插入式廣告。

如需直接將 AdDuplex 整合到 Windows 10 XAML 專案的詳細資訊，請移至 AdDuplex 網站：
* 橫幅廣告：[適用於 XAML 的 Windows 10 SDK](https://adduplex.zendesk.com/hc/articles/204849031-Windows-10-SDK-for-XAML-apps-installation-and-usage) (英文)
* 插入式廣告：[Windows 10 XAML AdDuplex 插入式廣告的安裝與用法](https://adduplex.zendesk.com/hc/articles/204849091-Windows-10-XAML-AdDuplex-Interstitial-Ad-Installation-and-Usage) (英文)

如需將 AdDuplex SDK 整合到使用 Unity 建立的 Windows 10 UWP 遊戲中的詳細資訊，請參閱 [適用於 Unity App 之 Windows 10 SDK 的安裝與用法](https://adduplex.zendesk.com/hc/articles/207279435-Windows-10-SDK-for-Unity-apps-installation-and-usage) (英文)。

## <a name="maximize-your-games-potential-through-ad-campaigns"></a>透過廣告活動將遊戲的潛力最大化

針對使用廣告宣傳您的遊戲採取進一步的步驟。 當您為遊戲[建立廣告活動](../monetize/index.md)時，其他遊戲就會顯示宣傳您的遊戲的廣告。

從數種類型中選擇能協助增加遊戲玩家族群的活動類型。

|活動類型             | 您遊戲的廣告出現在...                                                                                                                                                                   |
|--------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|已支付                      |符合您遊戲的裝置或類別的 App。                                                                                                                                                   |
|免費社群            |其他同為加入社群廣告活動的開發人員所發佈的 App。 如需詳細資訊，請參閱[關於社群廣告](../monetize/index.md)。|
|自家免費                |僅限您已經發佈的 App。 如需詳細資訊，請參閱[關於自家廣告](../monetize/index.md)。                                                            |

## <a name="related-links"></a>相關連結

* [獲得報酬](../publish/getting-paid-apps.md)
* [帳戶類型、位置和費用](../publish/account-types-locations-and-fees.md)
* [分析](../publish/analytics.md)
* [全球化和當地語系化](../design/globalizing/globalizing-portal.md)
* [實作應用程式的試用版](../monetize/implement-a-trial-version-of-your-app.md)
* [使用 A/B 測試執行應用程式實驗](../monetize/run-app-experiments-with-a-b-testing.md)