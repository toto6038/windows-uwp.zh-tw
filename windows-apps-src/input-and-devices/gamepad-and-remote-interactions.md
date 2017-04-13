---
author: mijacobs
Description: TODO
title: "遊戲台與遙控器的互動"
ms.assetid: 784a08dc-2736-4bd3-bea0-08da16b1bd47
label: Gamepad and remote interactions
template: detail.hbs
isNew: True
ms.author: mijacobs
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.openlocfilehash: 1c58ef4e48a91a57643e39f7fd87a700904e77dd
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="gamepad-and-remote-control-interactions"></a>遊戲台與遙控器的互動

通用 Windows 平台 (UWP) 應用程式現在支援遊戲台與遙控器輸入。 遊戲台與遙控器是 Xbox 和 TV 體驗的主要輸入裝置。 UWP 應用程式應該針對這些輸入裝置類型進行最佳化，就像針對電腦上的鍵盤與滑鼠輸入以及針對手機和平板電腦上的觸控輸入一樣。 確定您的應用程式可使用這些輸入裝置順暢運作，是最佳化 Xbox 和 TV的最重要步驟。
您現在可以在電腦上插入並使用遊戲台與 UWP 應用程式，這樣可輕鬆地驗證工作。

若要確保使用遊戲台或遙控器時，UWP app 可提供成功且有趣的使用者體驗，您應該考慮下列各項 ︰

* [硬體按鈕](designing-for-tv.md#hardware-buttons)  -
遊戲台與遙控器提供極不同的按鈕和設定。

* [XY 焦點瀏覽和互動](designing-for-tv.md#xy-focus-navigation-and-interaction)  -
XY 焦點瀏覽可讓使用者四處瀏覽您 app 的 UI。

* [滑鼠模式](designing-for-tv.md#mouse-mode)  -
滑鼠模式可讓您的 app 在 XY 焦點瀏覽不足時模擬滑鼠體驗。
