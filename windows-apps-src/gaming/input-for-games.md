---
title: 遊戲的輸入
description: 本節示範如何使用通用 Windows 平台 (UWP) 遊戲的遊戲台和其他輸入裝置。
ms.assetid: 2DD0B384-8776-4599-9E52-4FC0AA682735
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, 遊戲, 輸入
ms.localizationpriority: medium
ms.openlocfilehash: 1f1daac8bc94d49c501307728c1e966ba89435f9
ms.sourcegitcommit: c01c29cd97f1cbf050950526e18e15823b6a12a0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/05/2018
ms.locfileid: "8690944"
---
# <a name="input-for-games"></a>遊戲的輸入

本節說明有哪些不同類型的輸入裝置可用於 Windows 10 和 Xbox One 上的通用 Windows 平台 (UWP) 遊戲、示範基本用法，以及建議在遊戲中有效輸入程式設計所需的模式與技巧。

> **注意**    現有及可用於 UWP 遊戲的其他類型輸入裝置，例如自訂輸入裝置，可能風格特異，或可能為遊戲專用。 本節將不討論這類裝置及其程式設計。 如需有關用於協助自訂輸入裝置的詳細資訊，請參閱 [Windows.Gaming.Input.Custom](https://docs.microsoft.com/uwp/api/windows.gaming.input.custom) 命名空間。

## <a name="gaming-input-devices"></a>遊戲輸入裝置

在 UWP 遊戲和 Windows 10 及 Xbox One 的應用程式中，遊戲輸入裝置受到 [Windows.Gaming.Input](https://docs.microsoft.com/uwp/api/windows.gaming.input) 命名空間的支援。

### <a name="gamepads"></a>遊戲台

遊戲台是 Xbox One 的標準輸入裝置，而且是不愛用鍵盤和滑鼠的 Windows 遊戲玩家的共通選擇。 其提供各種數位及類比控制，幾乎所有類型的遊戲皆適用，同時還透過內嵌震動馬達提供了觸感反應。

如需如何在 UWP 遊戲中使用遊戲台的詳細資訊，請參閱[遊戲台與震動](gamepad-and-vibration.md)。

### <a name="arcade-sticks"></a>機台搖桿

機台搖桿是重新產生直立式機台風格的重要全數位輸入裝置，而且是面對面格鬥和其他機台樣式遊戲的完美輸入裝置。

如需如何在 UWP 遊戲中使用遊戲台的詳細資訊，請參閱[機台搖桿](arcade-stick.md)。

### <a name="racing-wheels"></a>賽車方向盤

賽車方向盤是與真正賽車的駕駛艙感覺類似的輸入裝置，對於以汽車或卡車為主的任何賽車遊戲而言，都是完美的輸入裝置。 許多賽車方向盤配備可配備真正的力回饋，也就是說，它們可以在控制軸上施加實際力量 (像是方向盤)，而不只是簡單的震動。

如需如何在 UWP 遊戲中使用賽車方向盤的詳細資訊，請參閱[賽車方向盤與力回饋](racing-wheel-and-force-feedback.md)。

### <a name="flight-sticks"></a>飛行桿

飛行桿是遊戲輸入的裝置，重現飛機或太空船駕駛艙中的飛行桿的感覺。 它們是快速且準確控制飛行的完美輸入裝置。

如需如何使用您的 UWP 遊戲中的飛行桿的詳細資訊，請參閱[飛行桿](flight-stick.md)。

### <a name="raw-game-controllers"></a>原始遊戲控制器

原始遊戲控制器是遊戲控制器的一般呈現，具有在許多不同類型的常見遊戲控制器上找到的輸入。 這些輸入作為未命名的按鈕、切換裝置和軸的簡單陣列而公開。 您可以使用原始遊戲控制器，讓客戶建立自訂輸入的對應，不論使用哪種類型的控制器。

如需如何在您的 UWP 遊戲中使用原始遊戲控制器的詳細資訊，請參閱[原始遊戲控制器](raw-game-controller.md)。

### <a name="ui-navigation-controllers"></a>UI 瀏覽控制器

UI 瀏覽控制器是邏輯輸入裝置，存在目的是為 UI 瀏覽命令提供通用詞彙，促使在各種不同遊戲和實體輸入裝置間能夠一致的使用者經驗。 遊戲的使用者介面應使用 UINavigationController 介面，而不是裝置特定的介面。

如需如何在 UWP 遊戲中使用 UI 瀏覽控制器的詳細資訊，請參閱 [UI 瀏覽控制器](ui-navigation-controller.md)。

### <a name="headsets"></a>耳機

耳機是音訊擷取和播放裝置，當透過其輸入裝置連線時，就會和特定使用者產生關聯。 線上遊戲普遍用它進行語音交談，同時也可用於加強沉浸的感受，或提供線上和離線遊戲功能。

如需如何在 UWP 遊戲中使用耳機的詳細資訊，請參閱[耳機](headset.md)

### <a name="users"></a>使用者

每一種輸入裝置及其連線的耳機，可與特定使用者產生關聯，以將使用者的身分識別連結到他們的遊戲。 使用者身分識別也是從實體輸入裝置輸入的相互關聯方式，如此才能從其邏輯 UI 瀏覽控制器輸入。

如需如何管理使用者及其輸入裝置的詳細資訊，請參閱[追蹤使用者及其裝置](input-practices-for-games.md#tracking-users-and-their-devices)。

## <a name="see-also"></a>另請參閱

* [遊戲的輸入練習](input-practices-for-games.md)
* [Windows.Gaming.Input 命名空間](https://docs.microsoft.com/uwp/api/windows.gaming.input)
* [Windows.Gaming.Input.Custom 命名空間](https://docs.microsoft.com/uwp/api/windows.gaming.input.custom)