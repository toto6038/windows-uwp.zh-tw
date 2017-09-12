---
author: mcleanbyron
ms.assetid: 2ed21281-f996-402d-a968-d1320a4691df
description: "使用本文中的測試應用程式識別碼和廣告單位識別碼值，以查看您的應用程式於測試期間呈現廣告的方式。"
title: "測試模式值"
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Windows 10, uwp, 廣告, 測試"
ms.openlocfilehash: 0c3e713d9a2bb7c10bda0d9517f5cb882d5e2e57
ms.sourcegitcommit: 6b772d2a224f8a9c557dc517c6ec0592545e9a43
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/02/2017
---
# <a name="test-mode-values"></a>測試模式值

當您在應用程式中使用 [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx)、[InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) 或 [NativeAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.nativead.aspx) 來顯示廣告時，您必須在 **AdUnitId** 和 **ApplicationId** 屬性中指定廣告單元識別碼和應用程式識別碼。 當您開發您的應用程式時，使用本文中的測試應用程式識別碼和廣告單元識別碼值，以查看您的應用程式於測試期間呈現廣告的方式。

如果您嘗試在應用程式發佈後使用測試值，您的實際應用程式將不會收到廣告。 若要再已發佈的應用程式中收到廣告，您必須更新您的程式碼，使用 Windows 開發人員中心儀表板所提供的應用程式識別碼和廣告單元識別碼。 如需詳細資訊，請參閱[在您的 App 中設定廣告單元](set-up-ad-units-in-your-app.md)。
 
以下是用於不同廣告類型的測試值。

* 插播式廣告：

    <table>
    <colgroup>
    <col width="33%" />
    <col width="33%" />
    <col width="33%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th align="left">目標 OS</th>
    <th align="left">AdUnitId</th>
    <th align="left">ApplicationId</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td align="left"><p>UWP (Windows 10)</p></td>
    <td align="left"><p>test</p></td>
    <td align="left"><p>d25517cb-12d4-4699-8bdc-52040c712cab</p></td>
    </tr>
    <tr class="odd">
    <td align="left"><p>Windows 8.x 和 Windows Phone 8.x</p></td>
    <td align="left"><p>11389925</p></td>
    <td align="left"><p>d25517cb-12d4-4699-8bdc-52040c712cab</p></td>
    </tr>
    </tbody>
    </table>

     
* 橫幅廣告︰

    <table>
    <colgroup>
    <col width="33%" />
    <col width="33%" />
    <col width="33%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th align="left">目標 OS</th>
    <th align="left">AdUnitId</th>
    <th align="left">ApplicationId</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td align="left"><p>UWP (Windows 10)</p></td>
    <td align="left"><p>test</p></td>
    <td align="left"><p>3f83fe91-d6be-434d-a0ae-7351c5a997f1</p></td>
    </tr>
    <tr class="even">
    <td align="left"><p>Windows 8.x 和 Windows Phone 8.x</p></td>
    <td align="left"><p>10865270</p></td>
    <td align="left"><p>3f83fe91-d6be-434d-a0ae-7351c5a997f1</p></td>
    </tr>
    </tbody>
    </table>

* 對於原生廣告：

    <table>
    <col width="33%" />
    <col width="33%" />
    <col width="33%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th align="left">目標 OS</th>
    <th align="left">AdUnitId</th>
    <th align="left">ApplicationId</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td align="left"><p>UWP (Windows 10)</p></td>
    <td align="left"><p>test</p></td>
    <td align="left"><p>d25517cb-12d4-4699-8bdc-52040c712cab</p></td>
    </tbody>
    </table>

> [!IMPORTANT]
> **AdControl** 即時廣告的大小是由 **Width** 和 **Height** 屬性定義。 為獲得最佳結果，請確定程式碼中的 **Width** 和 **Height** 屬性是[橫幅廣告支援的廣告大小](supported-ad-sizes-for-banner-ads.md)其中之一。 **Width** 和 **Height** 屬性不會隨即時廣告的大小變更。


 

 
