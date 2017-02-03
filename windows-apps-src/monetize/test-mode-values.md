---
author: mcleanbyron
ms.assetid: 2ed21281-f996-402d-a968-d1320a4691df
description: "使用本文中的測試應用程式識別碼和廣告單位識別碼值，以查看您的應用程式於測試期間呈現廣告的方式。"
title: "測試模式值"
translationtype: Human Translation
ms.sourcegitcommit: 2b5dbf872dd7aad48373f6a6df3dffbcbaee8090
ms.openlocfilehash: dcc83c3d654cfb290981f27ec2923fd3b37c5a8f

---

# <a name="test-mode-values"></a>測試模式值

當您在應用程式中使用 [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) 或 [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) 來顯示廣告時，您必須指定應用程式識別碼和廣告單元識別碼。 當您開發您的應用程式時，使用本文中的測試應用程式識別碼和廣告單元識別碼值，以查看您的應用程式於測試期間呈現廣告的方式。


如果您嘗試在應用程式發佈後使用測試值，您的實際應用程式將不會收到廣告。 若要再已發佈的應用程式中收到廣告，您必須更新您的程式碼，使用 Windows 開發人員中心儀表板所提供的應用程式識別碼和廣告單元識別碼。 如需詳細資訊，請參閱[在您的應用程式中設定廣告單元](set-up-ad-units-in-your-app.md)。
 
以下是影片插入式和橫幅廣告的測試值。

* 影片插入式廣告：

    <table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th align="left">AdUnitId</th>
    <th align="left">AppId</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td align="left"><p>11389925</p></td>
    <td align="left"><p>d25517cb-12d4-4699-8bdc-52040c712cab</p></td>
    </tr>
    </tbody>
    </table>

     
* 橫幅廣告︰

    <table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th align="left">AdUnitId</th>
    <th align="left">AppId</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td align="left"><p>10865270</p></td>
    <td align="left"><p>3f83fe91-d6be-434d-a0ae-7351c5a997f1</p></td>
    </tr>
    </tbody>
    </table>


> **重要**&nbsp;&nbsp;即時廣告的大小是由 **AdControl** 的 **Width** 和 **Height** 屬性定義。 為獲得最佳結果，請確定程式碼中的 **Width** 和 **Height** 屬性是[橫幅廣告支援的廣告大小](supported-ad-sizes-for-banner-ads.md)其中之一。 **Width** 和 **Height** 屬性不會隨即時廣告的大小變更。



 

 



<!--HONumber=Dec16_HO2-->


