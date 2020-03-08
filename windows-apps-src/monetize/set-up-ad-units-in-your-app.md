---
ms.assetid: bb105fbe-bbbd-4d78-899b-345af2757720
description: 瞭解如何將應用程式識別碼和 ad 單位識別碼值從合作夥伴中心新增至您的應用程式，然後再將應用程式提交至存放區。
title: 在您的應用程式中設定廣告單元
ms.date: 02/18/2020
ms.topic: article
keywords: Windows 10, uwp, 廣告, 廣告單元, 測試
ms.localizationpriority: medium
ms.openlocfilehash: c7bafdc7d21814a03d6f7da7132d8017d7f238e5
ms.sourcegitcommit: 0426013dc04ada3894dd41ea51ed646f9bb17f6d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2020
ms.locfileid: "78852567"
---
# <a name="set-up-ad-units-in-your-app"></a>在您的應用程式中設定廣告單元

>[!WARNING]
> 從2020年6月1日起，適用于 Windows UWP 應用程式的 Microsoft Ad 營收平臺將會關閉。 [深入了解](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

通用 Windows 平台 (UWP) app 中的每個控制項都有對應的*廣告單元*，是由我們的服務用來提供廣告給控制項。 每個廣告單元包含*廣告單元識別碼*和*應用程式識別碼*，您必須將其指派給應用程式中的程式碼。

我們提供[測試廣告單元值](#test-ad-units)，您可以在測試期間用來確認您的應用程式可以顯示測試廣告。 這些測試值只能在您應用程式的測試版本中使用。 如果您嘗試在應用程式發佈後使用測試值，您的實際應用程式將不會收到廣告。

在您完成 UWP 應用程式的測試並準備好將它提交給合作夥伴中心之後，您必須從合作夥伴中心的 [[應用程式內廣告](../publish/in-app-ads.md)] 頁面[建立即時 ad 單位](#live-ad-units)，並更新您的應用程式程式碼，以使用此 ad 單位的 [應用程式識別碼] 和 [ad 單位識別碼] 值。

如需有關在您的應用程式程式碼中指派應用程式識別碼和廣告單元識別碼值，請參閱下列文件：
* [XAML 和 .NET 中的 AdControl](adcontrol-in-xaml-and--net.md)
* [HTML 5 和 JAVAscript 中的 AdControl](adcontrol-in-html-5-and-javascript.md)
* [插播式廣告](../monetize/interstitial-ads.md)
* [原生廣告](../monetize/native-ads.md)

<span id="test-ad-units" />

## <a name="test-ad-units"></a>測試廣告單元

當您開發您的應用程式時，使用本節中的測試應用程式識別碼和廣告單元識別碼值，以查看您的應用程式於測試期間呈現廣告的方式。

### <a name="banner-ads-using-the-adcontrol-class"></a>橫幅廣告 (使用 AdControl 類別)

* Ad 單位識別碼： ```test```
* 應用程式識別碼： ```3f83fe91-d6be-434d-a0ae-7351c5a997f1```

    > [!IMPORTANT]
    > **AdControl** 即時廣告的大小是由 **Width** 和 **Height** 屬性定義。 為獲得最佳結果，請確定程式碼中的 **Width** 和 **Height** 屬性是[橫幅廣告支援的廣告大小](supported-ad-sizes-for-banner-ads.md)其中之一。 **Width** 和 **Height** 屬性不會隨即時廣告的大小變更。

### <a name="interstitial-ads-and-native-ads"></a>插播式廣告以及原生廣告

* Ad 單位識別碼： ```test```
* 應用程式識別碼： ```d25517cb-12d4-4699-8bdc-52040c712cab```

<span id="live-ad-units" />

## <a name="live-ad-units"></a>即時廣告單元

若要從合作夥伴中心取得即時廣告單位，並在您的應用程式中使用它：

1.  在合作夥伴中心的 [**應用程式內廣告**] 頁面上[建立 ad 單位](../publish/in-app-ads.md#create-ad-unit)。 請務必指定您的應用程式中正在使用的正確廣告控制項的廣告單元類型。
    > [!NOTE]
    > 您可以選擇為廣告單元啟用廣告流量分配，方法是在 [\[流量分配設定\]](../publish/in-app-ads.md#mediation) 區段進行設定。 廣告流量分配可讓您獲得最大的廣告收益並充分發揮應用程式促銷功能，透過顯示來自多個廣告網路的廣告，包括其他付費廣告網路的廣告，以及 Microsoft 應用程式促銷活動的廣告。 根據預設，我們會使用機器學習演算法自動設定廣告流量分配，來協助您的 app 在支援的所有市場，將您的廣告收入最大化，但是您可以選擇手動設定廣告流量分配。

2.  建立新的 ad 單位之後，請在 [**銷售**&gt;**應用程式內廣告**] 頁面中，取出可用 ad 單位的資料表中的 Ad 單位的**應用程式識別碼**和**ad 單位識別碼**。
    > [!NOTE]
    > 測試廣告單元和即時 UWP 廣告單元的應用程式識別碼值有不同的格式。 測試應用程式識別碼值為 GUID。 當您在合作夥伴中心建立即時 UWP ad 單位時，ad 單位的 [應用程式識別碼] 值一律會符合應用程式的商店識別碼（範例存放區識別碼值看起來像9NBLGGH4R315）。

3.  在您的應用程式代碼中，指派應用程式識別碼和廣告單元識別碼值。 如需詳細資訊，請參閱下列文章：
    * [XAML 和 .NET 中的 AdControl](adcontrol-in-xaml-and--net.md)
    * [HTML 5 和 JAVAscript 中的 AdControl](adcontrol-in-html-5-and-javascript.md)
    * [插播式廣告](../monetize/interstitial-ads.md)
    * [原生廣告](../monetize/native-ads.md)

<span id="manage" />

## <a name="manage-ad-units-for-multiple-ad-controls-in-your-app"></a>管理您應用程式中多個廣告控制項的廣告單元

您可以在單一 App 中使用多個橫幅、插播式廣告及原生廣告控制項。 在本案例中，我們建議您將不同的廣告單元指派給每個控制項。 針對每個控制項使用不同的廣告單元，可讓您為每個控制項個別[進行流量分配設定](../publish/in-app-ads.md#mediation)，並取得不同的[報告資料](../publish/advertising-performance-report.md)。 這也可讓我們的服務更能最佳化提供給應用程式的廣告。

> [!IMPORTANT]
> 每個廣告單元只能在一個應用程式中使用。 如果您在多個 app 中使用廣告單元，不會提供廣告給該廣告單元。

## <a name="related-topics"></a>相關主題

* [XAML 和 .NET 中的 AdControl](adcontrol-in-xaml-and--net.md)
* [HTML 5 和 JAVAscript 中的 AdControl](adcontrol-in-html-5-and-javascript.md)
* [插播式廣告](interstitial-ads.md)
* [原生廣告](native-ads.md)


 

 
