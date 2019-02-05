---
author: Karl-Bridge-Microsoft
Description: Optimize your app for input from Xbox gamepad and remote control.
title: 遊戲台與遙控器的互動
ms.assetid: 784a08dc-2736-4bd3-bea0-08da16b1bd47
label: Gamepad and remote interactions
template: detail.hbs
isNew: true
ms.author: kbridge
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 022724064ad1e7f5551b6676bf256ca5cf6e4b8e
ms.sourcegitcommit: bf600a1fb5f7799961914f638061986d55f6ab12
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/05/2019
ms.locfileid: "9048555"
---
# <a name="gamepad-and-remote-control-interactions"></a>遊戲台與遙控器的互動

![鍵盤與遊戲台影像](images/keyboard/keyboard-gamepad.jpg)

***遊戲台、 遠端控制與鍵盤之間共用一般互動模式***

確定您的 app 可使用遊戲台和遙控器順暢運作，是最佳化 10 英呎體驗最重要的步驟。 您可以對遊戲台和遙控器進行一些特定的增強功能，在使用者動作有某種程度受限的裝置上，最佳化使用者的互動體驗。

通用 Windows 平台 (UWP) 應用程式現在支援遊戲台與遙控器輸入。 

遊戲台與遙控器是 Xbox 和 TV 體驗的主要輸入裝置。 

UWP 應用程式應該針對這些輸入裝置類型進行最佳化，就像針對電腦上的鍵盤與滑鼠輸入以及針對手機和平板電腦上的觸控輸入一樣。 

確定您的應用程式可使用這些輸入裝置順暢運作，是最佳化 Xbox 和 TV的最重要步驟。

您現在可以在電腦上插入並使用遊戲台與 UWP 應用程式，這樣可輕鬆地驗證工作。

若要確保使用遊戲台或遙控器時，UWP app 可提供成功且有趣的使用者體驗，您應該考慮下列各項 ︰

* [硬體按鈕](../devices/designing-for-tv.md#hardware-buttons) - 遊戲台與遙控器提供極不同的按鈕和設定。

* [XY 焦點瀏覽和互動](../devices/designing-for-tv.md#xy-focus-navigation-and-interaction) - XY 焦點瀏覽可讓使用者四處瀏覽您應用程式的 UI。

* [滑鼠模式](../devices/designing-for-tv.md#mouse-mode) - 滑鼠模式可讓您的應用程式在 XY 焦點瀏覽不足時模擬滑鼠體驗。
