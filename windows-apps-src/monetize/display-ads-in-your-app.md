---
ms.assetid: 63A9EDCF-A418-476C-8677-D8770B45D1D7
description: Microsoft Advertising SDK 提供您數種方式來透過廣告讓您的 App 獲利。
title: 使用 Microsoft Advertising SDK 停用應用程式中的廣告
ms.date: 02/18/2020
ms.topic: article
keywords: Windows 10, uwp, 廣告, 橫幅, 廣告控制項, 插播式
ms.localizationpriority: medium
ms.openlocfilehash: 4e9a67bb26d47d0bd9cc26c56df90efac5e2daa5
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89164832"
---
# <a name="display-ads-in-your-app-with-the-microsoft-advertising-sdk"></a>使用 Microsoft Advertising SDK 停用應用程式中的廣告

>[!WARNING]
> 從2020年6月1日起，適用于 Windows UWP 應用程式的 Microsoft Ad 營收平臺將會關閉。 [深入了解](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

使用 Microsoft Advertising SDK，在您的適用於 Windows 10 的通用 Windows 平台 (UWP) app 中投放廣告，提高您的獲利商機。 我們的 ad 營收平臺提供各種 ad 格式，可順暢地整合到您的應用程式，並支援使用許多熱門 ad 網路進行中繼。 我們的平臺符合 OpenRTB、龐大的2.x、MRAID 2 和 VPAID 3 標準，而且與護城河和 IAS 相容。 

<br/>

<table style="border: none !important;">
<colgroup>
<col width="10%" />
<col width="23%" />
<col width="10%" />
<col width="23%" />
<col width="10%" />
<col width="23%" />
</colgroup>
<tbody>
<tr>
<td align="left"><img src="images/install-sdk.png" alt="Install SDK icon" /></td>
<td align="left"><b>開始使用</b><br/><br/>
    <a href="https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK">安裝 Microsoft Advertising SDK</a>
</td>
<td align="left"><img src="images/write-code.png" alt="Develop icon" /></td>
<td align="left"><b>開發人員指南</b><br/><br/>
    <a href="banner-ads.md">橫幅廣告</a>
    <br/>
    <a href="interstitial-ads.md">插入式 ads</a>
    <br/>
    <a href="native-ads.md">原生廣告</a>
    </td>
<td align="left"><img src="images/api-reference.png" alt="API ref icon" /></td>
<td align="left"><b>其他資源</b><br/><br/>
    <a href="set-up-ad-units-in-your-app.md">在您的應用程式中設定 ad 單位</a>
    <br/>
    <a href="best-practices-for-ads-in-apps.md">最佳做法</a>
    <br/>
    <a href="/uwp/api/overview/advertising">API 參考</a>
    </td>
</tr>
</tbody>
</table>

## <a name="step-1-install-the-microsoft-advertising-sdk"></a>步驟 1：安裝 Microsoft Advertising SDK

若要開始，請在您用來建立應用程式的開發電腦上安裝 [Microsoft Advertising SDK](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK)。 如需安裝指示，請參閱[本文](install-the-microsoft-advertising-libraries.md)。

## <a name="step-2-implement-ads-in-your-app"></a>步驟 2：在您的應用程式中實作廣告

Microsoft Advertising SDK 提供數種不同的廣告控制項類型，供您用在您的應用程式中。 選擇最適合您案例的廣告類型，然後再新增顯示廣告的程式碼到您的應用程式中。 在此步驟中，您將使用測試廣告單元，以查看您的應用程式於測試期間呈現廣告的方式。

### <a name="banner-ads"></a>橫幅廣告

這些是靜態顯示廣告，是使用應用程式中頁面的矩形部分來顯示促銷內容。 這些廣告會定期自動更新。 如果您是在應用程式中使用廣告的新手，這是入門的好地方。

如需指示和程式碼範例，請參閱[本文](adcontrol-in-xaml-and--net.md)。

![addreferences](images/banner-ad.png)

### <a name="interstitial-video-and-interstitial-banner-ads"></a>插播式影片和插播式橫幅廣告

這些是全螢幕廣告，通常要求使用者觀賞影片或點選連結，才能在應用程式或遊戲中繼續。 我們支援兩種類型的插播式廣告：影片和橫幅。

如需指示和程式碼範例，請參閱[本文](interstitial-ads.md)。

![addreferences](images/interstitial-ad.png)

### <a name="native-ads"></a>原生廣告

這些是元件型廣告。 每一項廣告創意 (例如標題、影像、描述和喚起行動文字) 都會當做個別元素傳送到您的應用程式，而您可使用自己的字型、色彩和其他 UI 元件，將這些元素整合到您的應用程式中。

如需指示和程式碼範例，請參閱[本文](native-ads.md)。

![addreferences](images/native-ad.png)

<span id="ad-mediation"/>

## <a name="step-3-create-an-ad-unit-and-configure-mediation"></a>步驟 3：建立廣告單元和設定流量分配

在您完成應用程式的測試，並準備好將它提交至商店之後，請在合作夥伴中心的 [應用程式內 ads](../publish/in-app-ads.md) 頁面上建立 ad 單位。 然後，更新您的應用程式程式碼為使用此廣告單元，讓您的應用程式可以收到即時廣告。 如需詳細資訊，請參閱[在您的 App 中設定廣告單元](set-up-ad-units-in-your-app.md#live-ad-units)。

根據預設，您的應用程式會顯示 Microsoft 付款廣告網路中的廣告。 若要讓您有最佳的廣告收入，您可以為您的廣告單元啟用[廣告流量分配](ad-mediation-service.md)，以顯示其他付費廣告網路如 Taboola 和 Smaato 的廣告。 您也可以提供來自 Microsoft 應用程式促銷活動的廣告，提高您應用程式的促銷功能。

若要開始在您的 UWP app 中使用廣告流量分配，請針對您的廣告單元[設定廣告流量分配設定](../publish/in-app-ads.md#mediation-settings)。 根據預設，我們會使用機器學習演算法自動設定廣告流量分配，來協助您的 app 在支援的所有市場，將您的廣告收入最大化。 不過，您也可以手動選擇所要使用網路的選項。 無論是哪一種方式，廣告流量分配完全在我們的伺服器上設定；您不需要在您的 app 中進行程式碼變更。    

## <a name="step-4-submit-your-app-and-review-performance"></a>步驟 4：提交您的應用程式，並檢閱績效

當您完成使用 ads 開發應用程式之後，您可以在合作夥伴中心中 [提交已更新的應用程式](../publish/app-submissions.md) ，使其可在存放區中使用。 顯示廣告的應用程式必須符合 [Microsoft Store 原則 10.10 節](/legal/windows/agreements/store-policies#1010-advertising-conduct-and-content)和[＜應用程式開發人員合約＞中＜附件 E＞](/legal/windows/agreements/app-developer-agreement)的額外要求。

當您的應用程式已發佈且可在存放區中使用之後，您可以在合作夥伴中心中檢查您的 [廣告效能報表](../publish/advertising-performance-report.md) ，並繼續對您的中繼設定進行變更，以優化廣告的效能。 您的廣告收益包含在[付款摘要](../publish/payout-summary.md)中。

<span id="additional-help" />

## <a name="additional-help"></a>其他說明

如需使用 Microsoft Advertising SDK 的其他說明，請使用下列資源。

|  工作    | 資源 |               
|----------|-------|
| 回報錯誤或取得針對廣告的支援。     | 請造訪[支援頁面](https://developer.microsoft.com/windows/support)，然後選擇 **\[用程式內廣告\]**。        |
| 取得社群支援     | 造訪[論壇](https://social.msdn.microsoft.com/forums/windowsapps/en-US/home?category=windowsapps)。       |
| 下載示範如何將橫幅廣告和插入式廣告新增到應用程式的範例專案。     | 請參閱 [GitHub 上的廣告範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Advertising)。       |
| 了解最新的 Windows App 獲利機會     | 瀏覽[從您的應用程式獲利](https://developer.microsoft.com/store/monetize)。        |

## <a name="windows-81-and-windows-phone-8x-apps"></a>Windows 8.1 和 Windows Phone 8.x 應用程式

對於 Windows 8.1 和 Windows Phone 8.x 應用程式，我們提供 [適用於 Windows 和 Windows Phone 8.x 的 Microsoft Advertising SDK](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDKforWindowsandWindowsPhone8x)。 如需有關如何在 Windows 8.1 和 Windows Phone 8.x 應用程式中使用此 SDK 顯示廣告的詳細資訊，請參閱[本文](/previous-versions/windows/apps/dn792120(v=win.10))。

## <a name="related-topics"></a>相關主題

* [Microsoft Advertising SDK](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK)
* [廣告績效報告](../publish/advertising-performance-report.md)
* [Windows Premium 廣告發行者計畫](windows-premium-ads-publishers-program.md)