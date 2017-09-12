---
author: jnHs
Description: "如果您的應用程式使用廣告流量分配或顯示使用 Microsoft Store Services SDK 的橫幅或插播式廣告，請使用 [利用廣告獲利] 頁面來管理廣告的使用方式。"
title: "利用廣告獲利"
ms.assetid: 09970DE3-461A-4E2A-88E3-68F2399BBCC8
ms.author: wdg-dev-content
ms.date: 07/06/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.openlocfilehash: 6ecd37e54de266764570606ceaa575614dfb050c
ms.sourcegitcommit: 10f8dcf69d37cdb61562fc9f4d268ccb499c368f
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/07/2017
---
# <a name="monetize-with-ads"></a>利用廣告獲利

在您的儀表板中的每個 app 都包含 **\[創造營收\]** &gt; **\[利用廣告獲利\]** 頁面。 使用此頁面管理下列 App 開發案例的廣告用途：

* 您的 UWP app 使用來自 [Microsoft Advertising SDK](http://aka.ms/ads-sdk-uwp) 的 [AdControl](https://msdn.microsoft.com/en-us/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx)、[InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) 或 [NativeAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.nativead.aspx)。
* 您的 Windows 8.x 或 Windows Phone 8.x 應用程式使用 **AdControl** 或 **InterstitialAd**，來自[適用於 Windows 和 Windows Phone 8.x 的 Microsoft Advertising SDK](http://aka.ms/store-8-sdk)。
* 您的 Windows 8.x 或 Windows Phone 8.x 應用程式使用 **AdMediatorControl**，來自 [適用於 Windows 和 Windows Phone 8.x 的 Microsoft Advertising SDK](http://aka.ms/store-8-sdk)。

如需這些開發案例的詳細資訊，請參閱[使用 Microsoft Advertising SDK 在您的應用程式中顯示廣告](../monetize/display-ads-in-your-app.md)。

<span id="create-ad-unit" />
## <a name="create-ad-units"></a>建立廣告單元

使用此區段針對下列案例建立廣告單元：

* 您的應用程式會使用 **AdControl** 顯示橫幅廣告。 如需詳細資訊，請參閱 [XAML 和 .NET 中的 AdControl](../monetize/adcontrol-in-xaml-and--net.md) 和 [HTML5 和 JavaScript 中的 AdControl](../monetize/adcontrol-in-html-5-and-javascript.md)。
* 您的應用程式會使用 **InterstitialAd** 顯示插播式影片廣告或插播式橫幅廣告。 如需詳細資訊，請參閱[插播式廣告](../monetize/interstitial-ads.md)。
* 您的應用程式會使用 **NativeAd** 顯示原生廣告。 如需詳細資訊，請參閱[原生廣告](../monetize/native-ads.md)。

若要建立廣告單元：

1.  在 **\[廣告單元名稱\]** 欄位中，輸入廣告單元的名稱。 這可以是任何您想要的描述性字串，用來識別廣告單元用於報告。
2.  在 **\[廣告單元類型\]** 下拉式清單中，選取對應您要在控制項中顯示的廣告的廣告單元類型。 可用的選項包括：**\[橫幅\]**、**\[插播式橫幅\]**、**\[插播式影片\]** 和 **\[原生\]**。
    > [!NOTE]
    > 目前僅有加入試驗計劃的選定開發人員才可以建立**原生**廣告單元，但我們很快就要將這項功能提供給所有的開發人員。 如果您想要加入我們的試驗計劃，請透過電子郵件與我們連絡：aiacare@microsoft.com。

3.  在 **\[裝置系列\]** 下拉式清單中，選取使用廣告單元所在應用程式的目標裝置系列。 可用的選項包括：**\[UWP (Windows 10)\]**、**\[電腦/平板電腦 (Windows 8.1)\]** 或 **\[行動裝置 (Windows Phone 8.x)\]**。
4.  按一下 **\[建立廣告單元\]**。

新的廣告單元會出現在此頁面上 **\[可用的廣告單元\]** 區段中清單的頂端。 如需在您的應用程式中使用廣告單元的相關詳細資訊，請參閱[在您的應用程式中設定廣告單元](../monetize/set-up-ad-units-in-your-app.md)。

> [!NOTE]
> 如果您的 Windows 8.x 或 Windows Phone 8.x 應用程式使用 **AdMediatorControl** 顯示橫幅廣告，則不需要在此處要求廣告單元。 在這個案例中，廣告單元會自動產生。

<span id="available-ad-units" />
## <a name="available-ad-units"></a>可用的廣告單元

您的廣告單元會出現在此區段底部的表格中。 您會看到每個廣告單元的**應用程式識別碼**和**廣告單元識別碼**。 若要在 app 中顯示廣告，您需要在程式碼中使用這些值：

-   如果您的 App 顯示橫幅廣告，請將這些值指派給 [AdControl](https://msdn.microsoft.com/library/mt313154.aspx) 物件的 [ApplicationId](https://msdn.microsoft.com/library/mt313174.aspx) 和 [AdUnitId](https://msdn.microsoft.com/library/mt313171.aspx) 屬性。 如需詳細資訊，請參閱 [XAML 和 .NET 中的 AdControl](../monetize/adcontrol-in-xaml-and--net.md) 和 [HTML 5 和 JavaScript 中的 AdControl](../monetize/adcontrol-in-html-5-and-javascript.md)。
-   如果您的 App 顯示插播式廣告，請將這些值傳遞給 [InterstitialAd](https://msdn.microsoft.com/library/mt313189.aspx) 物件的 [RequestAd](https://msdn.microsoft.com/library/mt313192.aspx) 方法。 如需詳細資訊，請參閱[插播式廣告](../monetize/interstitial-ads.md)。
-   如果您的 app 顯示原生廣告，請將這些值指派給 [NativeAdsManager](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.nativeadsmanager.nativeadsmanager.aspx) 建構函式的 *applicationId* 和 *adUnitId* 參數。 如需詳細資訊，請參閱[原生廣告](../monetize/native-ads.md)。

> [!IMPORTANT]
> 每個廣告單元只能在一個 app 中使用。 如果您在多個 app 中使用廣告單元，不會提供廣告給該廣告單元。

> [!NOTE]
> 您可以在單一 App 中使用多個橫幅、插播式廣告及原生廣告控制項。 在本案例中，我們建議您將不同的廣告單元指派給每個控制項。 針對每個控制項使用不同的廣告單元，可讓您為每個控制項個別[進行流量分配設定](../publish/monetize-with-ads.md#mediation)，並取得不同的[報告資料](../publish/advertising-performance-report.md)。 這也可讓我們的服務更能最佳化提供您的 App 的廣告。

<span id="mediation" />
## <a name="ad-mediation"></a>廣告流量分配

如果您的應用程式是適用於 Windows 10 的 UWP 應用程式，您可以使用此區段中的選項，為應用程式中橫幅廣告、插播式廣告或原生廣告的相關 UWP 廣告單元啟用廣告流量分配。 廣告流量分配可讓您獲得最大的廣告收益並充分發揮應用程式促銷功能，透過顯示來自多個廣告網路的廣告，包括其他付費廣告網路的廣告，以及 Microsoft 應用程式促銷活動的非營收產生廣告， 我們負責處理來自您選擇的廣告網路的橫幅廣告要求的流量分配。

如果您的 App 中的橫幅廣告、插播式廣告或原生廣告已經與 UWP 廣告單元相關聯，則不需要改變 App 的程式碼就能啟用廣告流量分配。

> [!NOTE]
> 本節描述 UWP 應用程式套件的 [**廣告流量分配**]。 如果您的應用程式套件目標為 Windows 8.x 或 Windows Phone 8.x，並且使用來自 [適用於 Windows 和 Windows Phone 8.x 的 Microsoft Advertising SDK](http://aka.ms/store-8-sdk) 的 **AdMediatorControl**，則儀表板中的 [**廣告流量分配**] 區段會顯示一組不同的選項。 如需有關針對使用 **AdMediatorControl** 的 Windows 8.x 或 Windows Phone 8.x 應用程式套件設定流量分配設定的詳細資訊，請參閱[此文章](https://msdn.microsoft.com/library/windows/apps/mt219689)。

若要針對 App 中的 UWP 廣告單元設定廣告流量分配設定：

1. 在 **\[設定下列項目的流量分配\]** 下拉式清單中，選取包含您要設定之橫幅、插播式或原生廣告單元的 UWP app 套件。

2. 在 **\[廣告單元類型\]** 下拉式清單中，選取您要設定的廣告單元類型 (**\[橫幅\]**、**\[插播式橫幅\]**、**\[插播式影片\]** 和 **\[原生\]**)。

3. 在 **\[廣告單元\]** 下拉式清單中，選取您要設定的 UWP 廣告單元的名稱。
    > [!NOTE]
    > 當您啟用 UWP 廣告單元的廣告流量分配時，不需要從協力廠商廣告網路取得廣告單元。 我們的廣告流量分配服務會自動建立任何必要的協力廠商廣告單元。

4. 根據預設，**讓 Microsoft 為您的應用程式選擇最佳流量分配設定**] 核取方塊已選取。 這個選項會使用機器學習演算法為您的應用程式自動選擇廣告流量分配設定，幫助您在應用程式支援的所有市場獲得最大的廣告收益。 建議您使用此選項。 否則，如果您想要選擇自己的廣告流量分配設定，請清除此核取方塊。
    > [!NOTE]
    > 本節中的其餘步驟僅於您要清除此核取方塊，並選擇自己的廣告流量分配設定時適用。

5. 在 [**目標**] 下拉式清單中選擇 [**基準**]，為您的廣告流量分配設定預設設定。 這個預設設定將適用於所有市場，除了您定義市場特定設定的市場之外。

6. 接著指定要在控制項中顯示付費網路 (依據廣告效果支付收益給您) 和其他廣告網路 (不依據廣告效果支付收益給您) 的廣告比率。 若要這樣做，請在 [**付費廣告網路**] 和 [**其他廣告網路**] 的 [**權重**] 欄位中輸入介於 0 到 100 之間的值。  

7. 在 [**付費廣告網路**] 區段中，選取 [**作用中**] 欄中每個您要使用的付費網路的核取方塊，然後使用 [**排名**] 欄中的箭號依排名順序排列網路 (這會指定控制項使用每個網路的頻率)。

    以下是目前支援的付費網路。 請注意，當中有些網路[並非所有市場都有提供](#network-markets)。

    -   **AOL 和 AppNexus**。 這是 Microsoft 管理的廣告網路，會透過我們的合作夥伴網路 AOL 和 AppNexus 提供廣告。 此網路支援 **\[橫幅\]** 和 **\[插播式影片\]** 廣告單元。
        > [!NOTE]
        > **AOL 和 AppNexus** 在 **\[橫幅\]** 廣告單元的 **\[付費廣告網路\]** 清單中始終排在第一，而且無法變更至這類廣告的較低排名。

    -   **AppNexus (直接)**︰選取此選項可從 [AppNexus](https://www.appnexus.com) 提供插播式影片廣告。 此網路支援 **\[插播式影片\]** 和 **\[原生\]** 廣告單元。

    -   **Microsoft 應用程式安裝廣告**。 選取此選項可提供 Windows 生態系統中其他[為自己的 App 建立促銷廣告活動](create-an-ad-campaign-for-your-app.md)的開發人員建立的應用程式安裝廣告或應用程式重新佔用廣告。 此網路支援 **\[橫幅\]**、**\[插播式橫幅\]** 和 **\[原生\]** 廣告單元。

    -   **Smaato**：選取此選項可從 [Smaato](https://www.smaato.com/) 提供廣告。 此網路支援 **\[橫幅\]** 廣告單元。

    -   **smartclip**︰選取此選項可從 [smartclip](http://www.smartclip.com/) 提供廣告。 此網路支援 **\[插播式影片\]** 廣告單元。

    -   **SpotX**︰選取此選項可從 [SpotX](https://www.spotx.tv/) 提供廣告。 此網路支援 **\[插播式影片\]** 廣告單元。

    -   **Taboola**︰選取此選項可從 [Taboola](https://www.taboola.com/) 提供廣告。 此網路支援 **\[橫幅\]** 廣告單元。

8. 如果您選取 **\[橫幅\]** 或 **\[插播式橫幅\]** 廣告單元，您會看到名為 **\[其他廣告網路\]** 的區段。 此區段中的網路不會依廣告效果讓您賺取收益。 這些網路會從來源顯示廣告，像是應用程式促銷活動。 

    在 **\[其他廣告網路\]** 區段中，選取 **\[作用中\]** 欄中您要使用的每個其他網路的核取方塊，然後使用 **\[排名\]** 欄中的箭號依排名順序排列網路 (這會指定控制項使用每個網路的頻率)。 以下是目前支援的其他網路：

    -   **Microsoft Community 廣告**。 如果您[建立其中一個應用程式的促銷廣告活動](create-an-ad-campaign-for-your-app.md)，並將此行銷活動設定為[社群廣告活動](about-community-ads.md)，則選取此選項可從這個行銷活動顯示廣告。 

    -   **Microsoft 自家廣告**。 如果您[建立其中一個應用程式的促銷廣告活動](create-an-ad-campaign-for-your-app.md)，並將此行銷活動設定為[自家廣告活動](about-house-ads.md)，則選取此選項可從這個行銷活動顯示廣告。

9. 對於您要覆寫預設流量分配設定的每個市場，在 [**目標**] 下拉式清單中選取市場，然後更新廣告網路選取項目和排名。

10. 按一下 [**儲存**]。

<span id="network-markets" />
### <a name="supported-markets-for-ad-networks"></a>廣告網路支援的市場

可用的廣告網路會在 UWP 應用程式的所有[支援的市場](define-pricing-and-market-selection.md#windows-store-consumer-markets)中提供廣告，但以下為例外狀況。

|  廣告網路  |  支援的市場  |
|--------------|---------------------|
| Smaato | 巴西、加拿大、法國、德國、義大利、日本、西班牙、英國、美國 |
| smartclip | 奧地利、比利時、丹麥、芬蘭、德國、義大利、荷蘭、挪威、瑞典、瑞士  |


## <a name="microsoft-affiliate-ads"></a>Microsoft 聯盟廣告

如果您想要在應用程式中顯示 Microsoft 聯盟廣告，請勾選此區段中的方塊。 如果您勾選此方塊，沒有來自其他廣告網路的廣告可用時，將提供市集中產品的廣告 (包括音樂、遊戲、電影、應用程式、硬體和軟體) 給您的應用程式。 當客戶在指定的屬性視窗內按一下市集中的廣告並購買產品時，您就會獲得核可購買項目的佣金。

如果您變更此選取項目，不需要重新發佈您的 app，變更就會生效。 如需 Microsoft 聯盟廣告的詳細資訊，請參閱[關於聯盟廣告](about-affiliate-ads.md)。

## <a name="coppa-compliance"></a>COPPA 規範

基於兒童線上隱私權保護法 (「 COPPA 」) 的立法宗旨，如果您的 app 是針對 13 歲以下的兒童，則您必須通知 Microsoft。 如果您使用開發人員中心來通知 Microsoft 有關您 app 是針對 13 歲以下的兒童，當傳送廣告到您 app 時，Microsoft 會採取步驟來停用其行為廣告服務。 如果您的 app 是針對 13 歲以下的兒童，基於 COPPA 的規範，您必須承擔某些義務。

如需有關 COPPA 所規範權利義務的詳細資訊，請參閱[此頁面](http://go.microsoft.com/fwlink/p/?linkid=536558)。
