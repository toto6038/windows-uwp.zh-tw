---
Description: To view performance data for the ad units in your apps, use the advertising performance report in Partner Center.
title: 廣告績效報告
ms.assetid: 32E555C3-C34D-4503-82BB-4C3F5CAE4500
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: a96f6f6593a8ccc6714f67b6f825a6416750b432
ms.sourcegitcommit: c01c29cd97f1cbf050950526e18e15823b6a12a0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/05/2018
ms.locfileid: "8703722"
---
# <a name="advertising-performance-report"></a>廣告績效報告


在[合作夥伴中心](https://partner.microsoft.com/dashboard)**的廣告績效報告**顯示您的[廣告單元](in-app-ads.md)的表現，包括社群廣告。 此報告會包含來自 UWP 應用程式中多個使用[廣告流量分配](in-app-ads.md#mediation)的廣告提供者的資料。

若要檢視這份報告，請展開左側瀏覽功能表的 **\[分析\]**，然後選取 **\[廣告效益\]**。 您可以在合作夥伴中心中檢視此資料，或下載報告資料以便離線檢視按一下頁面上的箭號圖示。 或者，您也可透過程式設計方式使用我們的[分析 REST API](../monetize/access-analytics-data-using-windows-store-services.md) 中的[取得廣告效益資料](../monetize/get-ad-performance-data.md)方法來擷取此資料。

檢視廣告效益報告時，請注意過去三天的報告資料可能會隨著我們從不同來源接收並處理新的資料而有所變更。 此外，過去 90 天的資料都可能發生資料明細重列。

## <a name="apply-filters"></a>套用篩選

在頁面頂端附近，您可以選取您想要顯示資料的時間週期。 預設選項是 30D（30 天），但您可以選擇在 3、6 或 12 個月期間顯示資料，或指定自訂的日期範圍。

您也可以展開 **\[篩選條件\]**，依廣告單元、應用程式、廣告提供者和裝置類型篩選此頁面上的所有資料。 您可以選擇下列選項：

* **彙總**：選擇如何彙總資料，以及如何進一步篩選。 根據預設，這個篩選條件會設為 **\[所有廣告單位\]**。 您也可以選擇將此篩選條件變更為 **\[所有應用程式\]** 或 **\[所有廣告提供者\]**，或者您也可以選擇依照您在其中使用廣告的特定應用程式來彙總。
* **廣告提供者**︰篩選報告以顯示特定[廣告提供者](in-app-ads.md#paid-networks)的績效資料。 根據預設，報告會來自所有廣告提供者的資料。 如果您選擇 **\[彙總\]** 下拉式清單中的 **\[所有應用程式\]**，此選項將會停用。
* **裝置**︰篩選報告以顯示特定裝置類型的績效資料。 根據預設，報告會顯示所有裝置類型的資料。

## <a name="overall-performance"></a>整體效能

本區段會根據您在報告篩選器中選取的廣告單元、應用程式以及廣告提供者，在圖表或世界地圖檢視中顯示廣告效能計量。

若要切換至不同的資料檢視，請按一下 **\[整體效能\]** 區段頂端的 **\[圖表\]** 或 **\[地圖\]**。 在地圖檢視中，比較深色的網底表示較高的值而比較淺色的網底表示較低的值。 您可以將游標暫留在地圖上特定的國家或地區上，以便分析所選取的衡量指標值。 您也可以放大地圖上的任何區域，以檢視較小國家/地區的資料。

您可以按一下 **\[圖表\]** 或 **\[地圖\]** 下拉式清單旁邊的篩選器圖示，縮小圖表或在地圖中顯示的資料範圍。 此篩選器可讓您選擇最多六個不同的廣告單元、應用程式或廣告提供者，以在圖表或在地圖檢視中進行比較。 您可以在此篩選器中選擇的資料類型，取決於您在報告頂端的 **\[彙總\]** 最上層篩選器中選取的項目。


## <a name="overall-performance-breakdown"></a>整體效能明細

這個區段會顯示表格，其中包含您在報告中選取的篩選器所指定資料集的所有廣告效能計量。

## <a name="performance-metrics"></a>效能計量

**廣告效能**報告包含下列效能計量資料。 某些計量只會針對某些廣告提供者顯示。

|  計量  |  描述  |
|----------|---------------|
| 估計營收  |  您從應用程式上播放廣告所收到的估計金額。 |
| eCPM  |  每千個曝光數的有效成本。 |
| 要求數  | 從您的應用程式傳送廣告要求的次數。  |
| 曝光數  | 廣告顯示在您應用程式中的次數。  |
| 投放率  | 從顯示廣告所在的應用程式傳送廣告要求的百分比。  |
| 點閱數  |  使用者點閱您應用程式中的廣告的次數。 |
| CTR  |  點閱率，即廣告點閱數除以曝光數。 |
| 可見性 | 可在您的應用程式中檢視廣告曝光數的百分比。 如需有關這個值如何計算的詳細資訊，請參閱[最佳化您的廣告單元的可見性](../monetize/optimize-ad-unit-viewability.md)。 |
| 賺取的點數  | 如果您執行[社群廣告](https://docs.microsoft.com/windows/uwp/publish/about-community-ads)行銷活動，這是指您藉由在應用程式中顯示社群廣告所賺得的促銷廣告空間點數。  |
| 花費的點數  | 如果您執行[社群廣告](https://docs.microsoft.com/windows/uwp/publish/about-community-ads)行銷活動，這是指您在應用程式廣告上花費的點數。  |

## <a name="related-topics"></a>相關主題

* [應用程式內廣告](in-app-ads.md)
* [使用 Microsoft Advertising SDK 停用應用程式中的廣告](../monetize/display-ads-in-your-app.md)
* [最佳化您的廣告單元的可見性](../monetize/optimize-ad-unit-viewability.md)


 
