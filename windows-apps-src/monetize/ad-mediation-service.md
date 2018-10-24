---
author: Xansky
description: Microsoft 廣告流量分配服務可以透過顯示來自多個廣告網路的廣告，讓您獲得最大的廣告收益並充分發揮應用程式促銷功能。
title: Microsoft 廣告流量分配服務
ms.author: mhopkins
ms.date: 06/05/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, ads, advertising, ad mediation, 廣告, 廣告流量分配
ms.localizationpriority: medium
ms.openlocfilehash: cfcb2402a9a0246060a619cbc65337e2b2c69d78
ms.sourcegitcommit: 4b97117d3aff38db89d560502a3c372f12bb6ed5
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/23/2018
ms.locfileid: "5437111"
---
# <a name="microsoft-ad-mediation-service"></a>Microsoft 廣告流量分配服務

當您使用 [Microsoft Advertising SDK](http://aka.ms/ads-sdk-uwp) [在您的 App 顯示廣告](display-ads-in-your-app.md)，您也可以選擇使用 Microsoft 廣告流量分配服務來最大化您的廣告收益。 本文提供廣告流量分配服務及其目標的概觀。

廣告流量分配服務是 [Microsoft 廣告營收平台](https://developer.microsoft.com/windows/ad-monetization-platform)的一部分。 平台由下列部分組成。

![addreferences](images/ad-mediation-service.png)

廣告流量分配服務旨在透過建置這些功能來最大化您的 App 的廣告收益。

## <a name="diversity-of-demand-by-market-and-format"></a>市場和格式需求的多樣性

廣告流量分配服務會跨不同廣告格式 (橫幅、插播式橫幅、插播式視訊和原生) 整合各種廣告網路。 廣告流量分配服務與各個廣告網路的運作，可確保他們能夠以最高的潛力提供廣告以吸引使用者。 我們將會繼續加入不同的廣告合作夥伴，以擴展需求的多樣性和品質。

## <a name="manage-complexity-of-ad-network-relationships"></a>管理廣告網路關係的複雜性  

廣告流量分配服務會整合各種廣告網路，因此您不需要執行這項工作。 在您使用 Microsoft Advertising SDK 在您的 App 中顯示廣告之後，您可以[使用開發人員中心儀表板](../publish/in-app-ads.md#mediation-settings)修改您的廣告流量分配設定，以顯示多個廣告網路的廣告。 您可以從新的廣告網路取得的廣告獲益，而不需要變更您的程式碼。

我們代表您管理與廣告網路的端對端關係。 透過廣告網路整合以提供廣告、報告和支付的所有事項均由我們負責處理，您無須額外費心。

## <a name="machine-learning-based-yield-optimization-algorithms"></a>機器學習式的利潤最佳化演算法

廣告流量分配服務的運作可為開發人員產生最高的利潤。 為如此做，於是採用機器學習式的利潤最佳化演算法。 對於每個廣告呼叫，演算法會判斷最好的*瀑布*訂單，其中需要呼叫廣告網路，為您將收益最大化。 這需要考慮許多因素，包括：

* 提出要求的使用者。
* 所要求的應用程式。
* 廣告網路的過去效能。
* 要求來源的廣告格式和市場。
* 廣告呼叫的時間。
* App 內容的類別。
* 使用者區段。
* 使用者的位置。

新的廣告網路會自動包含在內並透過學習預算評估效能。 在很短時間內，它們會在瀑布中找到它們的位置。 這會讓廣告網路更具競爭力，並協助開發人員透過 App 充分運用獲利。

我們高度建議使用我們[建議的流量分配設定](../publish/in-app-ads.md#mediation-settings)以最大化從應用程式中的廣告獲得的收益。 這可讓我們的演算法，得以為您的應用程式產生最佳利潤。 不過，您也可以在開發人員中心儀表板中選擇您自己的流量分配設定，對提供廣告和訂單的廣告網路有更多的控制權。

## <a name="rich-data-and-signals"></a>豐富的資料和訊號

廣告流量分配服務與各種廣告網路的運作，可改善使用者的目標設定，為每位使用者顯示更相關的廣告。 這可透過傳送更豐富的使用者和 App 相關訊號到廣告網路達成，同時時刻記住隱私權的需求。

## <a name="related-topics"></a>相關主題

* [Microsoft Advertising SDK](http://aka.ms/ads-sdk-uwp)
* [流量分配設定](../publish/in-app-ads.md#mediation-settings)
* [廣告績效報告](../publish/advertising-performance-report.md)
