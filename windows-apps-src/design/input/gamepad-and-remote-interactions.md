---
author: mijacobs
Description: TODO
title: 遊戲台與遙控器的互動
ms.assetid: 784a08dc-2736-4bd3-bea0-08da16b1bd47
label: Gamepad and remote interactions
template: detail.hbs
isNew: true
ms.author: mijacobs
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: a3e87fea7dfce66b16ccdf04ef13950c092660c8
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/01/2018
ms.locfileid: "5879992"
---
# <a name="gamepad-and-remote-control-interactions"></a>遊戲台與遙控器的互動

![遠端與方向鍵](images/dpad-remote/dpad-remote.png)

通用 Windows 平台(UWP) 應用程式現在支援遊戲台和遙控器輸入，其為 Xbox 和電視的主要輸入裝置。

UWP 應用程式應該針對這些輸入裝置類型進行最佳化，就像針對電腦上的鍵盤與滑鼠輸入以及針對手機和平板電腦上的觸控輸入一樣。

確定您的應用程式可使用這些輸入裝置順暢運作，是最佳化 Xbox 和 TV的關鍵步驟。

> [!NOTE] 
> 您現在可以在電腦上插入並使用遊戲台與 UWP 應用程式，這樣可輕鬆地驗證工作。

若要確保使用遊戲台或遙控器時，UWP app 可提供成功且有趣的使用者體驗，您應該考慮下列各項 ︰

* [硬體按鈕](../devices/designing-for-tv.md#hardware-buttons) - 遊戲台與遙控器提供極不同的按鈕和設定。

* [XY 焦點瀏覽和互動](../devices/designing-for-tv.md#xy-focus-navigation-and-interaction) - XY 焦點瀏覽可讓使用者四處瀏覽您應用程式的 UI。

* [滑鼠模式](../devices/designing-for-tv.md#mouse-mode) - 滑鼠模式可讓您的應用程式在 XY 焦點瀏覽不足時模擬滑鼠體驗。
