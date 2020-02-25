---
Description: 您可以交叉推銷您的應用程式和其他開發人員發佈的應用程式。 我們將此功能稱為社群廣告。
title: 關於社群廣告
ms.assetid: F55CE478-99AF-4B70-90D1-D16419562136
ms.date: 02/18/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 48d63dae76113c70273f6ff2cba46348b1e24a10
ms.sourcegitcommit: 71f9013c41fc1038a9d6c770cea4c5e481c23fbc
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/20/2020
ms.locfileid: "77507012"
---
# <a name="about-community-ads"></a>關於社群廣告

>[!WARNING]
> 從2020年6月1日起，適用于 Windows UWP 應用程式的 Microsoft Ad 營收平臺將會關閉。 [深入了解](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

如果您的應用程式[顯示橫幅或橫幅插入式廣告](../monetize/display-ads-in-your-app.md)，您可以免費將應用程式與其他開發人員在 Microsoft Store 中的應用程式交叉推廣。 我們將此功能稱為「社群廣告」。  

以下是此計劃的運作方式：

* 依照下面所述加入宣告社區廣告之後，您就可以[建立免費的社區廣告行銷活動](create-an-ad-campaign-for-your-app.md)。 然後，您的應用程式會與其他也加入宣告廣告的開發人員共用促銷廣告空間。 您的應用程式將顯示參與社群廣告之其他開發人員所發佈的應用程式的廣告，而他們的應用程式也將會顯示您應用程式的廣告。
* 您可以透過在您的應用程式中顯示社群廣告來賺取其他應用程式中之推銷廣告空間的點數。 點數會依據以下程序計算：
  * 對於可使用提供社群廣告之應用程式的每個國家與地區，國家或地區的目前市場利率 eCPM (每千個曝光數有效成本) 值會乘以您的應用程式在該國家或地區的社群廣告要求數。 此值就是您為您的應用程式在該國家或地區賺取的點數。
  * 您在指定期間賺取的總點數等於您提供社群廣告的每個應用程式在每個國家或地區賺取之所有點數的總和。
* 您的點數會平均分配給所有正在進行的社群廣告行銷活動，並且會依據您社群廣告行銷活動所針對之國家的目前市場利率 eCPM 值，轉換成您應用程式的廣告曝光數。
* 若要追蹤您 App 中社群廣告的績效，請參閱[廣告效益報告](advertising-performance-report.md)。

### <a name="opt-in-to-community-ads"></a>選擇加入社群廣告

在您可以為其中一個應用程式建立社區廣告行銷活動之前，您必須先在[合作夥伴中心](https://partner.microsoft.com/dashboard)的 [**銷售**&gt;**應用程式內廣告**] 頁面中加入宣告。

若要加入宣告 UWP 應用程式的廣告：

1. 選取您要在應用程式中使用的 ad 單位，並向下流覽至 [中繼**設定**]。
2. 如果已選取 [**讓 Microsoft 優化我的設定**]，則會自動為您的 ad 單位啟用「社區廣告」。 否則，請在 [目標] 下拉式清單中選取基準**設定**或市場特定設定，然後勾選 [**其他 ad 網路**] 清單中的 [ **Microsoft 社區 ads** ] 方塊。

    > [!NOTE]
    > 您可以使用**權數**欄位來指定您想要從付費網路和其他廣告網路（包括社區廣告）顯示的廣告比率。

完成選擇後，您不需要重新發行應用程式。 一旦您已選擇加入，您可以在**建立廣告活動**時選取[\[社群廣告 (免費)\]](create-an-ad-campaign-for-your-app.md) 做為行銷活動類型。

### <a name="related-topics"></a>相關主題

* [應用程式內廣告](in-app-ads.md)
* [為您的應用程式建立廣告行銷活動](create-an-ad-campaign-for-your-app.md)
