---
author: andrewleader
Description: Learn about when and where you should use secondary tiles in your UWP app.
title: 次要磚
label: Secondary tiles
template: detail.hbs
ms.author: wdg-dev-content
ms.date: 05/25/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, secondary tiles, guidance, guidelines, best practices, 次要磚. 指導方針, 最佳做法
ms.localizationpriority: medium
ms.openlocfilehash: 1e3d31376b9ac155dab6bffa7739cb880af1cff9
ms.sourcegitcommit: 933e71a31989f8063b020746fdd16e9da94a44c4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/11/2018
ms.locfileid: "4534904"
---
# <a name="secondary-tile-guidance"></a>次要磚指導方針


次要磚提供一致且有效率的方式讓使用者直接從 \[開始\] 功能表上存取應用程式中的特定區域。 雖然使用者可選擇是否將次要磚「釘選」到 \[開始\] 功能表，但應用程式中的可釘選區域是由開發人員決定的。 如需更詳細的摘要，請參閱[次要磚概觀](secondary-tiles.md)。 當您在應用程式中啟用次要磚並設計相關聯的 UI 時，請考慮這些指導方針。

> [!NOTE]
> 只有使用者才能將次要磚釘選至 \[開始\] 功能表；應用程式無法以程式設計的方式釘選次要磚。 使用者也可控制磚的移除，並可將次要磚從 \[開始\] 功能表或在父項應用程式中移除。


## <a name="recommendations"></a>建議事項

當啟用您應用程式中的次要磚時，請考慮以下建議事項：

* 當焦點中的內容為可釘選時，應用程式列應包含 \[釘選到開始畫面\] 按鈕，藉此為使用者建立次要磚。
* 當使用者按一下 \[釘選到開始畫面\] 時，您應立即從 UI 執行緒將 API 呼叫到[釘選次要磚](secondary-tiles-pinning.md)。
* 如果焦點中的內容已釘選，請以 \[從開始畫面取消釘選\] 按鈕取代應用程式列上的 \[釘選到開始畫面\] 按鈕。 \[從開始畫面取消釘選\] 按鈕應移除現有的次要磚。
* 當在焦點中的內容不可釘選時，不會顯示 \[釘選到開始畫面\] 按鈕 (或顯示停用的 \[釘選到開始畫面\] 按鈕)。
* 為您的 \[釘選到開始畫面\] 與 \[從開始畫面取消釘選\] 按鈕使用系統提供的字符 (請參閱 Windows.UI.Xaml.Controls.Symbol 或 WinJS.UI.AppBarIcon 中的釘選與取消釘選成員)。
* 使用標準按鈕文字：「釘選到開始畫面」與「從開始畫面取消釘選」。 當您使用系統提供的釘選與取消釘選字符時，您必須覆寫預設文字。
* 不要使用次要磚作為虛擬命令按鈕以與父項應用程式互動，例如 \[略過至下一個追蹤\] 磚。


## <a name="additional-usage-guidance-for-devs"></a>裝置的其他使用指導方針

* 當應用程式啟動時，應一律列舉其次要磚，以防有任何不知道的新增與刪除項目。 當透過 \[開始\] 畫面應用程式列刪除次要磚時，Windows 只會移除該次要磚。 應用程式本身負責發行由次要磚使用的任何資源。 當透過雲端複製次要磚時，次要磚上的目前磚或徽章通知、排程的通知、推播通知通道及搭配定期通知使的統一資源識別項 (URI) 不會透過次要磚複製，必須再次設定。
* 應用程式應為次要磚使用有意義、可重複建立、唯一的識別碼。 使用對應用程式而言有意義的可預測次要磚識別碼，可幫助應用程式了解當在新電腦的全新安裝中顯示次要磚時可使用這些磚執行的動作。
  * 在執行階段，應用程式可以特定的磚是否存在。
  * 可要求次要磚平台傳回屬於特定應用程式的所有次要磚集。 為這些次要磚使用有意義、唯一的 ID，可幫助應用程式檢查次要磚集，並執行適當的動作。 例如對於社交媒體應用程式，識別碼可識別各個其磚已建立的連絡人。
* 次要磚 (例如 \[開始\] 畫面上的所有磚) 為動態的出口，可經常更新為新的內容。 次要磚可使用與任何其他磚相同的機制，來呈現通知與更新。 若要深入了解，請參閱[選擇通知傳遞方法](choosing-a-notification-delivery-method.md)。


## <a name="related"></a>相關

* [次要磚概觀](secondary-tiles.md)
* [釘選次要磚](secondary-tiles-pinning.md)
* [磚資產](app-assets.md)
* [磚內容文件](create-adaptive-tiles.md)
* [傳送本機磚通知](sending-a-local-tile-notification.md)
