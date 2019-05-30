---
title: 啟動顯示畫面
description: 本節說明如何設定 app 的啟動顯示畫面。
ms.assetid: 6b954bb3-e5b0-46d1-8afc-fb805536cf6d
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 09eadb8467725cbf40f3fb54d32741960fc89321
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/29/2019
ms.locfileid: "66371867"
---
# <a name="splash-screens"></a>啟動顯示畫面

所有 UWP app 都必須具備啟動顯示畫面，也就是影像與背景色彩 (兩者都可自訂) 的組合。

在使用者啟動 app 時，會立即顯示您的啟動顯示畫面。 這會在初始化 app 資源時，提供立即的回饋給使用者。 只要 app 準備好與使用者互動，啟動顯示畫面就會關閉。

設計良好的啟動顯示畫面可讓您的 app 更具吸引力。 這裡有個簡單明瞭的啟動顯示畫面：

![啟動顯示畫面範例中縮放比例 75% 的啟動顯示畫面的螢幕擷取畫面。](images/regularsplashscreen.png)

這個啟動顯示畫面結合了綠色背景色彩與透明背景的 PNG 影像。

無論您的 app 執行所在的裝置為何，具有背景色彩的簡單影像看起來都很美觀。 只會變更背景的大小來適應不同的螢幕大小。 您的影像將永遠保持不變。

此外，您可以使用 [**SplashScreen**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Activation.SplashScreen) 類別自訂 app 的啟動經驗。 您可以放置延長式啟動顯示畫面，建立這個畫面是為了讓您的 app 有更多的時間可以完成其他工作，像是準備 app UI 或是完成網路作業。 您也可以使用 **SplashScreen** 類別，在關閉啟動顯示畫面時通知您，這樣您就可以開始進入動畫。

| 主題 | 描述 |
|-------|-------------|
| [新增啟動顯示畫面](add-a-splash-screen.md) | 設定應用程式的啟動顯示畫面影像與背景色彩 |
| [延長顯示啟動顯示畫面](create-a-customized-splash-screen.md) | 您可以為應用程式建立延長式啟動顯示畫面，讓啟動顯示畫面的顯示時間變長。 這個延長的畫面是模仿您應用程式啟動時所顯示的啟動顯示畫面，您可以自訂這個畫面。 |