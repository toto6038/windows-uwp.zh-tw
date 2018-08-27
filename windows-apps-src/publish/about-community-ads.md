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
ms.sourcegitcommit: 753dfcd0f9fdfc963579dd0b217b445c4b110a18
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2018
ms.locfileid: "2864877"
---
# <a name="about-community-ads"></a>關於社群廣告

如果您應用程式[會顯示橫幅插入式廣告](../monetize/display-ads-in-your-app.md)，您可以跨-提升您的應用程式與其他開發人員的 Microsoft 市集的應用程式與免費。 我們將此功能稱為「社群廣告」**。  

以下是此計劃的運作方式：

* 您選擇集後至社群 ads 如下所示，您可以[建立免費社群 ad campaign](create-an-ad-campaign-for-your-app.md)。 您的應用程式然後將會與其他開發人員也加入社群 ads 共用促銷 ad 空間。 您的應用程式將顯示參與社群廣告之其他開發人員所發佈的應用程式的廣告，而他們的應用程式也將會顯示您應用程式的廣告。
* 您可以透過在您的應用程式中顯示社群廣告來賺取其他應用程式中之推銷廣告空間的點數。 點數會依據以下程序計算：
  * 對於可使用提供社群廣告之應用程式的每個國家與地區，國家或地區的目前市場利率 eCPM (每千個曝光數有效成本) 值會乘以您的應用程式在該國家或地區的社群廣告要求數。 此值就是您為您的應用程式在該國家或地區賺取的點數。
  * 您在指定期間賺取的總點數等於您提供社群廣告的每個應用程式在每個國家或地區賺取之所有點數的總和。
* 您的點數會平均分配給所有正在進行的社群廣告行銷活動，並且會依據您社群廣告行銷活動所針對之國家的目前市場利率 eCPM 值，轉換成您應用程式的廣告曝光數。
* 若要追蹤您 App 中社群廣告的績效，請參閱[廣告效益報告](advertising-performance-report.md)。

### <a name="opt-in-to-community-ads"></a>選擇加入社群廣告

您可以為其中一個您的應用程式中建立社群 ad campaign 之前，您必須選擇加入上**Monetize** &gt; Windows 開發人員中心儀表板中**的應用程式 ads** ] 頁面。

加入社群 ads UWP 應用程式：

1. 在 [**應用程式中 ads** ] 頁面的 [**中繼設定**] 區段中選取要在應用程式中使用 ad 單位。
2. 如果選取 [**讓 Microsoft 選擇您的應用程式的最佳中繼設定**] 選項，社群 ads 已啟用 ad 單位自動。 否則請在 [**目標**] 下拉式清單中選取基準設定] 或 [市場特有設定，然後檢查**其他 ad 網路**清單中的 [ **Microsoft 社群 ads**方塊。

    > [!NOTE]
    > 您可以使用**權數**欄位來指定您想要顯示文件付費的網路和其他 ad 網路包含社群 ads ads 的比例。

Windows 加入社群 ads 8.x 或 Windows Phone 8.x 應用程式

1. 在 [**應用程式中 ads** ] 頁面核取 [**顯示在 「 我的應用程式中的社群 ads**方塊。

完成選擇後，您不需要重新發行應用程式。 一旦您已選擇加入，您可以在[建立廣告活動](create-an-ad-campaign-for-your-app.md)時選取**\[社群廣告 (免費)\]** 做為行銷活動類型。

### <a name="related-topics"></a>相關主題

* [應用程式內廣告](in-app-ads.md)
* [為您的 app 建立廣告行銷活動](create-an-ad-campaign-for-your-app.md)
