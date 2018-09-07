---
author: jnHs
Description: You can cross-promote your app with apps published by other developers. We call this feature community ads.
title: 關於社群廣告
ms.assetid: F55CE478-99AF-4B70-90D1-D16419562136
ms.author: wdg-dev-content
ms.date: 10/04/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: ada2e0610e81e986deba3ddab5e87547e05fe108
ms.sourcegitcommit: 00d27738325d6db5b5e481911ae7fac0711b05eb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/07/2018
ms.locfileid: "3663019"
---
# <a name="about-community-ads"></a>關於社群廣告

如果您的應用程式[顯示橫幅廣告或橫幅插播式廣告](../monetize/display-ads-in-your-app.md)，您可以交叉推銷您的應用程式和 Microsoft 網上商店中的應用程式與其他開發人員免費。 我們將此功能稱為「社群廣告」**。  

以下是此計劃的運作方式：

* 您選擇加入的之後加入社群廣告，如下所述，您可以[建立免費社群廣告行銷活動](create-an-ad-campaign-for-your-app.md)。 您的應用程式將會與其他同樣加入社群廣告的開發共用推銷廣告空間。 您的應用程式將顯示參與社群廣告之其他開發人員所發佈的應用程式的廣告，而他們的應用程式也將會顯示您應用程式的廣告。
* 您可以透過在您的應用程式中顯示社群廣告來賺取其他應用程式中之推銷廣告空間的點數。 點數會依據以下程序計算：
  * 對於可使用提供社群廣告之應用程式的每個國家與地區，國家或地區的目前市場利率 eCPM (每千個曝光數有效成本) 值會乘以您的應用程式在該國家或地區的社群廣告要求數。 此值就是您為您的應用程式在該國家或地區賺取的點數。
  * 您在指定期間賺取的總點數等於您提供社群廣告的每個應用程式在每個國家或地區賺取之所有點數的總和。
* 您的點數會平均分配給所有正在進行的社群廣告行銷活動，並且會依據您社群廣告行銷活動所針對之國家的目前市場利率 eCPM 值，轉換成您應用程式的廣告曝光數。
* 若要追蹤您 App 中社群廣告的績效，請參閱[廣告效益報告](advertising-performance-report.md)。

### <a name="opt-in-to-community-ads"></a>選擇加入社群廣告

您可以為您的應用程式的其中一個建立社群廣告行銷活動之前，您必須在 [**創造營收**上選擇加入&gt;在 Windows 開發人員中心儀表板中的**應用程式內廣告**頁面。

若要選擇加入社群廣告的 UWP 應用程式：

1. 在 [**應用程式內廣告**\] 頁面的**流量分配設定**區段，選取您在應用程式中使用廣告單元。
2. 如果未選取 [**讓 Microsoft 選擇最佳流量分配設定為您的 app** ] 選項，社群廣告會自動啟用適用於您的廣告單元。 否則，在**目標**下拉式清單中選取的基準設定或在特定市場的設定，然後檢查 [**其他廣告網路**清單中的 [ **Microsoft Community 廣告**] 方塊。

    > [!NOTE]
    > 您可以使用的**加權**欄位來指定您想要顯示來自付費的網路，包括社群廣告的其他廣告網路的廣告的比例。

若要選擇加入社群廣告的 Windows 8.x 或 Windows Phone 8.x 應用程式，

1. 在**應用程式內廣告**頁面上，核取 [**顯示我在 app 中的社群廣告**] 方塊。

完成選擇後，您不需要重新發行應用程式。 一旦您已選擇加入，您可以在[建立廣告活動](create-an-ad-campaign-for-your-app.md)時選取**\[社群廣告 (免費)\]** 做為行銷活動類型。

### <a name="related-topics"></a>相關主題

* [應用程式內廣告](in-app-ads.md)
* [為您的 app 建立廣告行銷活動](create-an-ad-campaign-for-your-app.md)
