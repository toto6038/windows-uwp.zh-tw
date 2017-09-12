---
author: mcleanbyron
ms.assetid: 63A9EDCF-A418-476C-8677-D8770B45D1D7
description: "Microsoft Advertising SDK 提供您數種方式來透過廣告讓您的 App 獲利。"
title: "使用 Microsoft Advertising SDK 停用應用程式中的廣告"
ms.author: mcleans
ms.date: 07/20/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Windows 10, uwp, 廣告, 橫幅, 廣告控制項, 插播式"
ms.openlocfilehash: 4730ebaf55af8e7063c444d5b3bbd973b0508db2
ms.sourcegitcommit: a9e4be98688b3a6125fd5dd126190fcfcd764f95
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/21/2017
---
# <a name="display-ads-in-your-app-with-the-microsoft-advertising-sdk"></a>使用 Microsoft Advertising SDK 停用應用程式中的廣告

使用 Microsoft Advertising SDK 在您的應用程式中投放廣告，提高您的獲利商機。 我們的廣告營收平台支援各式各樣的廣告類型，也支援在各大廣告網路間流量分配的功能。

若要在您的應用程式中顯示廣告，請執行下列步驟。

## <a name="step-1-install-the-microsoft-advertising-sdk"></a>步驟 1：安裝 Microsoft Advertising SDK

Microsoft Advertising SDK 提供各式各樣的控制項，您可以在應用程式中用來顯示不同的廣告類型。 如需安裝指示，請參閱[本文](install-the-microsoft-advertising-libraries.md)。

## <a name="step-2-choose-your-ad-type-and-add-code-to-display-test-ads-in-your-app"></a>步驟 2：選擇您的廣告類型，並在您的應用程式中加入顯示測試廣告的程式碼

Microsoft Advertising SDK 提供數種不同的廣告類型，供您顯示在您的應用程式中。 選擇最適合您案例的廣告類型，然後再新增顯示廣告的程式碼到您的應用程式中。

您必須在程式碼中指定應用程式識別碼和廣告單元識別碼，以便提供廣告給您的應用程式。 開發應用程式時，您應使用[測試應用程式識別碼和廣告單元識別碼值](test-mode-values.md)，以查看您的應用程式於測試期間呈現廣告的方式。

### <a name="banner-ads"></a>橫幅廣告

這些是靜態顯示廣告，是使用應用程式的部分頁面 (通常是位於頁面頂端或底部) 的小廣告。

如需指示和程式碼範例，請參閱 [XAML 和 .NET 中的 AdControl](adcontrol-in-xaml-and--net.md) 和 [HTML 5 和 Javascript 中的 AdControl](adcontrol-in-html-5-and-javascript.md)。 如需示範如何使用 C# 和 C++ 將橫幅廣告新增到 JavaScript/HTML 應用程式及 XAML 應用程式的完整範例專案，請參閱 [GitHub 上的廣告範例](http://aka.ms/githubads)。

![addreferences](images/banner-ad.png)

### <a name="interstitial-ads"></a>插播式廣告

這些是全螢幕廣告，通常要求使用者觀賞影片或點選連結，才能在應用程式或遊戲中繼續。 我們支援兩種類型的插播式廣告：影片和橫幅。

如需指示和程式碼範例，請參閱[插播式廣告](interstitial-ads.md)。 如需示範如何使用 C# 和 C++ 將插播式廣告新增到 JavaScript/HTML 應用程式及 XAML 應用程式的完整範例專案，請參閱 [GitHub 上的廣告範例](http://aka.ms/githubads)。

![addreferences](images/interstitial-ad.png)

### <a name="native-ads"></a>原生廣告

這些是元件型廣告。 每一項廣告創意 (例如標題、影像、描述和喚起行動文字) 都會當做個別元素傳送到您的應用程式，而您可使用自己的字型、色彩和其他 UI 元件，將這些元素整合到您的應用程式中。

如需指示和程式碼範例，請參閱[原生廣告](native-ads.md)。

![addreferences](images/native-ad.png)

## <a name="step-3-get-an-ad-unit-from-dev-center-and-configure-your-app-to-receive-live-ads"></a>步驟 3：從開發人員中心取得廣告單元並設定您的應用程式接收即時廣告

在您完成測試應用程式後，即可將其提交到市集，在 Windows 開發人員中心儀表板的 [\[利用廣告獲利\]](../publish/monetize-with-ads.md) 頁面上建立廣告單元。 接著，使用此廣告單元的應用程式識別碼和廣告單元識別碼值，更新您的應用程式程式碼。 如果您嘗試在已發行至市集的應用程式版本中使用測試應用程式識別碼和廣告單元識別碼值，您的應用程式不會收到即時廣告。 如需詳細資訊，請參閱[在您的應用程式中設定廣告單元](set-up-ad-units-in-your-app.md)。

<span id="ad-mediation"/>
### <a name="configure-ad-mediation"></a>設定廣告流量分配

根據預設，橫幅廣告、插播式廣告和原生廣告會顯示來自 Microsoft 付費廣告網路的廣告。 若要讓您有最佳的廣告收入，您可以為您的廣告單元啟用廣告流量分配，以顯示其他付費廣告網路如 Taboola 和 Smaato 的廣告。 您也可以提供來自 Microsoft 應用程式促銷活動的廣告，提高您應用程式的促銷功能。

若要開始在您的 UWP app 中使用廣告流量分配，請針對您的廣告單元[設定廣告流量分配設定](../publish/monetize-with-ads.md#mediation)。 根據預設，我們會使用機器學習演算法自動設定廣告流量分配，來協助您的 app 在支援的所有市場，將您的廣告收入最大化。 不過，也有可以手動選擇所要使用網路的選項。 無論是哪一種方式，廣告流量分配完全在服務上設定；您不需要在您的應用程式中進行程式碼變更。    

<span id="8.x-mediation"/>
### <a name="mediation-in-windows-81-and-windows-phone-8x-apps"></a>Windows 8.1 和 Windows Phone 8.x app 的廣告流量分配

在 Windows 8.1 和 Windows Phone 8.x app，廣告流量分配僅適用於橫幅廣告。 若要使用廣告流量分配，您必須在[適用於 Windows 和 Windows Phone 8.x 的 Microsoft Advertising SDK](http://aka.ms/store-8-sdk) 中使用 **AdMediatorControl** 類別，而不是 **AdControl** 類別。 在您的應用程式中加入此控制項之後，您可以在儀表板上手動設定您的廣告流量分配。

如需有關如何在 Windows 8.1 或 Windows Phone 8.x app 中使用廣告流量分配的詳細資訊，請參閱[本文](https://msdn.microsoft.com/library/windows/apps/xaml/dn864359.aspx)。

> [!NOTE]
> Windows 8.1 和 Windows Phone 8.x App 的廣告流量分配已不再是開發中。 若要將您潛在的廣告收益最大化，我們建議您在橫幅廣告、插播式廣告或原生廣告中使用廣告流量分配，來建置 UWP app。

## <a name="step-4-submit-your-app-and-review-performance"></a>步驟 4：提交您的應用程式，並檢閱績效

當您完成包含廣告之應用程式的開發之後，您可以[提交更新後的應用程式](https://msdn.microsoft.com/windows/uwp/publish/app-submissions)至 Windows 開發人員中心儀表板，讓它能夠在 Windows 市集中提供。 顯示廣告的應用程式必須符合 [Windows 市集原則 10.10 節](https://msdn.microsoft.com/library/windows/apps/dn764944.aspx#pol_10_10)和[＜應用程式開發人員合約＞中＜附件 E＞](https://msdn.microsoft.com/library/windows/apps/hh694058.aspx)的額外要求。

您的應用程式發行並在市集中提供後，您可以在儀表板中檢視[廣告績效報告](../publish/advertising-performance-report.md)並繼續變更廣告流量分配設定以將廣告績效最佳化。 您的廣告收益包含在[付款摘要](../publish/payout-summary.md)中。

<span id="additional-help" />
## <a name="additional-help"></a>其他說明

如需使用 Microsoft Advertising SDK 的其他說明，請使用下列資源。

|  工作    | 資源 |               
|----------|-------|
| 回報錯誤或取得針對廣告的支援。     | 請造訪[支援頁面](https://go.microsoft.com/fwlink/p/?LinkId=331508)，然後選擇 **\[App 內廣告\]**。        |
| 取得社群支援     | 造訪[論壇](http://go.microsoft.com/fwlink/p/?LinkId=401266)。       |
| 下載示範如何將橫幅廣告和插入式廣告新增到應用程式的範例專案。     | 請參閱 [GitHub 上的廣告範例](http://aka.ms/githubads)。       |
| 了解最新的 Windows App 獲利機會     | 瀏覽[從您的應用程式獲利](https://developer.microsoft.com/store/monetize)。        |

## <a name="related-topics"></a>相關主題

* [Microsoft Advertising SDK](http://aka.ms/ads-sdk-uwp)
* [利用廣告從您的應用程式獲利](http://go.microsoft.com/fwlink/p/?LinkId=699559)
* [廣告績效報告](../publish/advertising-performance-report.md)
