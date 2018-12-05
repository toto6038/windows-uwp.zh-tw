---
title: Xbox 最佳做法
description: 如何針對 Xbox 最佳化應用程式。
ms.date: 10/12/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 6e64feb8938be3e7338c87acdf8fd18fb13e525b
ms.sourcegitcommit: c01c29cd97f1cbf050950526e18e15823b6a12a0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/05/2018
ms.locfileid: "8701992"
---
# <a name="xbox-best-practices"></a>Xbox 最佳做法

根據預設，所有 UWP App 都可在 Xbox One 上執行，而不需要您進行任何額外的工作。 不過，如果您希望使 App 受到注目、讓您的顧客滿意，並和 Xbox 上最佳的 App 體驗進行競爭，您應該遵循下列做法。
  > [!NOTE]
  > 開始之前，請先查看[針對 Xbox 和電視進行設計](../design/devices/designing-for-tv.md)中的設計指導方針。   

## <a name="to-build-the-best-experiences-for-xbox-one"></a>建置 Xbox One 的最佳體驗

### <a name="do-turn-off-mouse-mode"></a>*請務必：* 關閉滑鼠模式

Xbox 使用者喜愛他們的控制器。 若要最佳化控制器輸入，請[停用滑鼠模式](how-to-disable-mouse-mode.md)並啟用方向導覽 (也稱為 [X-Y 焦點](../design/devices/designing-for-tv.md#xy-focus-navigation-and-interaction))。 注意焦點陷阱和無法存取的 UI。

### <a name="do-draw-a-focus-rectangle-that-is-appropriate-for-a-10-foot-experience"></a>*請務必：* 繪製一個適合 10 英呎體驗的焦點矩形

大多數 Xbox 使用者會坐在客廳電視的另一端，因此請留意標準的焦點矩形很難在 10 英呎的距離外看清楚。 若要確保具有輸入焦點的 UI 元素可以一直清楚地顯示給使用者，請遵循[焦點視覺](../design/devices/designing-for-tv.md#focus-visual)指導方針。 在 XAML 中，您會在您的 App 於 Xbox 上執行時免費獲得這項行為，但是 HTML App 將需要使用自訂 CSS 樣式。

### <a name="do-integrate-with-the-systemmediatransportcontrols-class"></a>*請務必：* 整合 SystemMediaTransportControls 類別

Xbox 使用者希望透過 Xbox Media Remote、Cortana (特別是「播放」和「暫停」語音命令) 及 Xbox SmartGlass 控制媒體 App。 若要免費取得這些功能，您的 App 應該使用 [SystemMediaTransportControls](https://msdn.microsoft.com/library/windows/apps/windows.media.systemmediatransportcontrols.aspx) 類別，這個類別會自動包含在 Xbox 媒體控制項中。 如果您的 App 有自訂的媒體控制項，請務必整合 **SystemMediaTransportControls** 類別以將這些功能提供給您的使用者。 如果您正在建立背景音樂 App，請整合 **SystemMediaTransportControls** 類別以確保背景音樂控制項在 Xbox 多工處理索引標籤中能正常運作。

<!-- ### *Do:* Use adaptive UI to account for snapped apps
One of the unique features of Xbox One is that users can snap apps such as Cortana next to any other app, so your app should respond gracefully when it runs in *fill mode*. Implement [adaptive UI](../get-started/universal-application-platform-guide.md#design-adaptive-ui-with-adaptive-panels) and make sure to test your app during development by snapping an app next to it. -->

### <a name="consider-draw-to-the-edge-of-the-screen"></a>*請考慮︰* 繪製到螢幕的邊緣

許多電視會截斷顯示器的邊緣，因此您 App 所有的重要內容應該要顯示在[電視安全區域](../design/devices/designing-for-tv.md#tv-safe-area)內。 UWP 使用*溢出掃描*來將內容保持在電視安全區域內顯示，但是這個預設行為會在您 App 的周圍繪製一個明顯的邊界。 若要提供最佳體驗，請關閉預設行為並遵循[如何在螢幕邊緣繪製 UI](turn-off-overscan.md) 的指示。
> [!IMPORTANT]
  > 如果您停用溢出掃描，您必須負責確保互動式元素和文字都能維持在電視安全區域內。 

### <a name="consider-use-tv-safe-colors"></a>*請考慮︰* 使用電視安全色彩

電視無法像電腦監視器一樣處理極端的色調。 請避免在您的 App 中使用高濃度色彩，這樣使用者才不會看到奇怪的條狀效果或褪色的影像。 此外請注意，由於電視之間的差異，在*您的*電視上看起來很棒的色彩，在使用者的電視上看起來可能會非常不同。 閱讀以了解如何讓您的應用程式看起來很棒，為每個人的[色彩](../design/devices/designing-for-tv.md#colors)！

### <a name="remember-you-can-disable-scaling"></a>*請記住：* 您可以停用縮放比例

UWP App 會自動縮放，以確保 UI 元素 (例如控制項與字型) 可在所有裝置上辨識。 使用 XAML 的 App 會放大 200%，而使用 HTML 的 App 則會放大 150%。 如果您想要取得更多 Xbox 上的 App 外觀控制項，請停用預設縮放比例以使用 HDTV 的實際像素維度 (1920x1080)。 如需有關自訂 App 以在 Xbox 上看起來更美觀的詳細資訊，請查看[如何關閉縮放比例](disable-scaling.md)及[有效像素與縮放](../design/basics/design-and-ui-intro.md#effective-pixels-and-scaling)。

如果您想要了解一下這些適用於 UWP app 的做法，請觀查這段影片！
</br>
</br>
<iframe src="https://channel9.msdn.com/Blogs/One-Dev-Minute/Tailoring-your-UWP-app-for-Xbox/player" width="960" height="540" allowFullScreen frameBorder="0"></iframe>

## <a name="channel-9"></a>Channel 9

下列在 [Channel 9](https://channel9.msdn.com/) 上的討論，是在 Xbox 上建置優秀 App 的絕佳資訊來源：

- [建置適用於 Xbox 的絕佳通用 Windows 平台 (UWP) App](https://channel9.msdn.com/Events/Build/2016/B883)
- [針對 Xbox One 與電視調整您的 App](https://channel9.msdn.com/Events/Build/2016/T651-R1)
- [UWP 開發 1：建置調適型 UI](https://channel9.msdn.com/Events/Build/2016/L724-R1)
- [瀏覽器以外的 Web 應用程式：跨平台與跨裝置](https://channel9.msdn.com/Events/Build/2016/B888)

## <a name="app-dev-on-xbox"></a>在 Xbox 上的應用程式開發

**在 Xbox 上的應用程式開發人員**事件是適用於開發人員在 Xbox 上建置應用程式的絕佳起點。

* [觀賞錄製工作階段](https://developer.microsoft.com/windows/projects/campaigns/app-dev-on-xbox-event#WatchNow)
* [閱讀部落格文章](https://developer.microsoft.com/windows/projects/campaigns/app-dev-on-xbox-event#BlogSeries)

## <a name="see-also"></a>請參閱

- [Xbox One 上的 UWP](index.md)
- [針對 Xbox 和電視進行設計](../design/devices/designing-for-tv.md)
