---
author: mcleanbyron
ms.assetid: bb105fbe-bbbd-4d78-899b-345af2757720
description: "了解在提交 app 到市集之前，如何從 Windows 開發人員中心儀表板新增應用程式識別碼和廣告單元識別碼。"
title: "在您的應用程式中設定廣告單元"
ms.author: mcleans
ms.date: 06/26/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Windows 10, uwp, 廣告, 廣告單位"
ms.openlocfilehash: f96e81079764682a9f603fe93a9c123a69690507
ms.sourcegitcommit: 8c4d50ef819ed1a2f8cac4eebefb5ccdaf3fa898
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/27/2017
---
# <a name="set-up-ad-units-in-your-app"></a>在您的應用程式中設定廣告單元

您應用程式中每個橫幅廣告、插播式廣告或原生廣告控制項都有對應的*廣告單元*，是由我們的服務用來提供廣告給控制項。 每個廣告單元包含*廣告單元識別碼*和*應用程式識別碼*。 當您開發您的應用程式時，請指派[測試應用程式識別碼和廣告單元識別碼值](test-mode-values.md)至您的控制項，以確認您的應用程式可以顯示測試廣告。 這些測試值只能在您應用程式的測試版本中使用。

在您完成測試您的應用程式且準備好將應用程式提交至 Windows 開發人員中心時，您必須更新您的應用程式程式碼，以使用 Windows 開發人員中心儀表板的 [\[利用廣告獲利\]](../publish/monetize-with-ads.md) 頁面中提供的應用程式識別碼和廣告單位識別碼值。 如果您嘗試在實際 App 中使用測試值，您的 App 將不會收到即時廣告。

若要為您的實際應用程式設定廣告單元：

1.  在 Windows 開發人員中心儀表板上，選取您的應用程式然後按一下 **\[創造營收\] > \[利用廣告獲利\]**。

2.  在 **\[建立廣告單元\]** 區段中的 **\[廣告單元名稱\]** 欄位，輸入廣告單元的名稱。

3. 在 **\[廣告單元類型\]** 下拉式清單中，選取對應於您要在控制項中顯示之廣告的廣告單元類型：

    -   如果您在應用程式中使用 [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) 顯示橫幅廣告，請選取 **\[橫幅\]**。

    -   如果您的 app 使用 [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) 顯示插播式影片廣告或插播式橫幅廣告，選取 **\[插播式影片\]** 或 **\[插播式橫幅\]**（請務必針對您想要顯示的插播式廣告類型選取適當選項）。

    -   如果您在應用程式中使用 [NativeAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.nativead.aspx) 顯示原生廣告，請選取 **\[原生\]**。
      > [!NOTE]
      > 目前僅有加入試驗計劃的選定開發人員才可以建立**原生**廣告單元，但我們很快就要將這項功能提供給所有的開發人員。 如果您想要加入我們的試驗計劃，請透過電子郵件與我們連絡：aiacare@microsoft.com。

4.  在 **\[裝置系列\]** 下拉式清單中，選取使用廣告單元所在應用程式的目標裝置系列。 可用的選項包括：**\[UWP (Windows 10)\]**、**\[電腦/平板電腦 (Windows 8.1)\]** 或 **\[行動裝置 (Windows Phone 8.x)\]**。

5.  按一下 **\[建立廣告單元\]**。 新的廣告單元會出現在此頁面上 **\[可用的廣告單元\]** 中清單的頂端。

6.  您會看到每個已產生的廣告單元的**「應用程式識別碼」**和**「廣告單元識別碼」**。 若要在 app 中顯示廣告，您需要在 app 程式碼中使用這些值：

    -   如果您的 app 顯示橫幅廣告，請將這些值指派給 **AdControl** 物件的 [ApplicationId](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.applicationid.aspx) 和 [AdUnitId](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.adunitid.aspx) 屬性。 如需詳細資訊，請參閱 [XAML 和 .NET 中的 AdControl](adcontrol-in-xaml-and--net.md) 和 [HTML 5 和 Javascript 中的 AdControl](adcontrol-in-html-5-and-javascript.md)。

    -   如果您的 App 顯示插播式廣告，請將這些值傳遞給 **InterstitialAd** 物件的 [RequestAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.requestad.aspx) 方法。 如需詳細資訊，請參閱[插播式廣告](interstitial-ads.md)。

    -   如果您的 app 顯示原生廣告，請將這些值指派給 [NativeAdsManager](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.nativeadsmanager.nativeadsmanager.aspx) 建構函式的 *applicationId* 和 *adUnitId* 參數。 如需詳細資訊，請參閱[原生廣告](../monetize/native-ads.md)。

7. 如果您的 app 是適用於 Windows 10 的 UWP app，您可以選擇為 **AdControl** 控制項啟用廣告流量分配，方法是在 [\[廣告流量分配\]](../publish/monetize-with-ads.md#mediation) 區段進行設定。 廣告流量分配可讓您獲得最大的廣告收益並充分發揮應用程式促銷功能，透過顯示來自多個廣告網路的廣告，包括其他付費廣告網路如 Taboola 和 Smaato 的廣告，以及 Microsoft 應用程式促銷活動廣告。 根據預設，我們會使用機器學習演算法自動設定廣告流量分配，來協助您的 app 在支援的所有市場，將您的廣告收入最大化，但是您可以選擇手動設定廣告流量分配。

<span id="manage" />
## <a name="manage-ad-units-for-multiple-ad-controls-in-your-app"></a>管理您應用程式中多個廣告控制項的廣告單元

您可以在單一 App 中使用多個橫幅、插播式廣告及原生廣告控制項。 在本案例中，我們建議您將不同的廣告單元指派給每個控制項。 針對每個控制項使用不同的廣告單元，可讓您為每個控制項個別[進行流量分配設定](../publish/monetize-with-ads.md#mediation)，並取得不同的[報告資料](../publish/advertising-performance-report.md)。 這也可讓我們的服務更能最佳化提供您的 App 的廣告。

> [!IMPORTANT]
> 每個廣告單元只能在一個 app 中使用。 如果您在多個 app 中使用廣告單元，不會提供廣告給該廣告單元。

## <a name="related-topics"></a>相關主題

* [測試模式值](test-mode-values.md)
* [XAML 和 .NET 中的 AdControl](adcontrol-in-xaml-and--net.md)
* [HTML 5 和 Javascript 中的 AdControl](adcontrol-in-html-5-and-javascript.md)
* [插播式廣告](interstitial-ads.md)
* [原生廣告](../monetize/native-ads.md)


 

 
