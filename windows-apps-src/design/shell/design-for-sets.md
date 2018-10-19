---
author: jwmsft
description: 了解如何最佳化您的應用程式，提供集合 UI 中的最佳體驗。
title: 針對集合的設計
template: detail.hbs
ms.author: jimwalk
ms.date: 05/07/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 標題列
doc-status: Draft
ms.localizationpriority: medium
ms.openlocfilehash: 7c3e0e6ec7331e860c9153e2a2e29a51fb5848bd
ms.sourcegitcommit: 310a4555fedd4246188a98b31f6c094abb33ec60
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/19/2018
ms.locfileid: "5128579"
---
# <a name="designing-for-sets"></a>針對集合的設計

> [!IMPORTANT]
> 本文說明尚未發佈但可能在正式發行前大幅度修改的功能。 Microsoft 對此處提供的資訊，不做任何明確或隱含的瑕疵擔保。

開始使用 Windows Insider Preview，「集合」功能已提供應用程式的使用者。 使用「集合」，在與其他應用程式共用的視窗中繪製您的應用程式，且每個應用程式在標題列中取得自己的索引標籤。

在本文中，我們會介紹您想要最佳化應用程式的區域，提供集合 UI 中的最佳體驗。

> [!TIP]
> 如需「集合」的詳細資訊，請參閱[集合簡介](https://insider.windows.com/en-us/articles/introducing-sets/)部落格文章和[集合的開發](https://developer.microsoft.com/events/build/content/developing-for-sets-on-windows-10) Microsoft Build 2018 的討論。

## <a name="customizing-tab-visuals"></a>自訂索引標籤視覺效果

根據預設，系統為使用中時，會嘗試替應用程式的索引標籤選取適當的文字與圖示色彩。 如此可以確保您花最少的力氣讓應用程式的索引標籤看起來美觀，或甚至您不用進行任何「集合」的最佳化。

不過，有些情況下，您最好自訂應用程式的索引標籤色彩。 在本節中，我們將解釋預設的索引標籤行為，並討論您什麼情況下應該或不應該修改應用程式的索引標籤行為。

### <a name="tab-states"></a>索引標籤狀態

您的應用程式是一個集合時，其索引標籤可以為下列三種狀態之一：選取且使用中、選取但非使用中，或未選取且非使用中。

- **選取-使用中**：從一組群組的視窗中選取一個索引標籤，且位在使用中前景視窗裡。

    (在本文件中，任何修改_使用中_索引標籤的討論皆表示索引標籤為選取-使用中。)
- **選取-非使用中**：從一組群組的視窗中選取一個索引標籤，但不位在使用中前景視窗裡。
- **未選取-非使用中**：未從一組群組的視窗中選取一個索引標籤。

根據系統佈景主題，系統已更新且維護了非使用中索引標籤的色彩。 從您的應用程式已無法有任何影響。

根據預設，選取且使用中的索引標籤根據使用者在 Windows 設定中指定的系統佈景主題色彩。 只有在索引標籤使用中時，您才能自訂應用程式的索引標籤色彩。

![集合中的索引標籤狀態](images/sets-tab-states.jpg)

### <a name="coloring-of-active-tabs"></a>使用中索引標籤的色彩

透過應用程式中您設定的值，或系統設定，來判斷使用中索引標籤的色彩。 應用程式使用中時，使用索引標籤色彩的判斷，如下所示：

- 如果您在應用程式中指定索引標籤色彩，會有最高優先順序。 不管系統設定，應用程式使用中時，會使用您在應用程式中指定的索引標籤色彩。
- 否則，如果使用者在 Windows 設定中選取選項在標題列上顯示輔色，則會使用系統輔色。
  - 在個人化 Windows 設定應用程式中找到此設定 > 色彩 > 在下列介面上顯示輔色：標題列。
- 最後，如果不套用任何應用程式或使用者設定，則索引標籤會使用目前的系統佈景主題色彩。

### <a name="considerations-when-you-modify-tab-colors"></a>您修改索引標籤色彩的考量

以下，我們針對您可能想修改應用程式索引標籤色彩的情況做討論，以及在這些案例中您必須考量的事物。 我們也會討論在哪些情況下不建議您修改索引標籤色彩，而是讓系統來管理。

#### <a name="match-your-brand-colors"></a>符合您品牌的色彩

一般而言，判斷您是否修改索引標籤色彩的覆寫要素，是希望符合您品牌的顏色。 當您修改應用程式索引標籤以符合品牌色彩時，應該根據本節中其他概述的考量來測試其外觀，例如應用程式版面配置或不同系統佈景主題色彩。

#### <a name="horizontal-layout"></a>水平的版面配置

如果您的應用程式版面配置包含在上方水平執行的純色（非-壓克力）色帶，通常會透過使用對稱色彩來連接應用程式與其索引標籤。 替索引標籤選擇您覺得與應用程式相連的一個純色，最好透過提供一些連續性給應用程式上方所使用的色彩。

#### <a name="horizontal-layout-with-acrylic"></a>壓克力風格的水平版面配置

如果您的應用程式使用能在上方水平執行的壓克力素材頻帶，我們建議您讓系統判斷索引標籤的色彩。

此處，我們也建議您使用應用程式中壓克力風格，而不是使用背景壓克力風格，讓應用程式背景流向此區域，而非建立透過應用程式或此頻帶背後桌面顯示的條紋效果。

請注意，在此案例中，使用者可設定這些索引標籤使用輔色，所以它們可能會出現淺色/深色佈景主題或輔色。

#### <a name="vertical-layout"></a>垂直版面配置

如果您的應用程式版面配置包含垂直執行的純色垂直窗格，我們建議您不要自訂索引標籤色彩。 使用者可隨時變更應用程式上方的索引標籤位置，因此您無法仰賴應用程式上方部分與索引標籤之間的色彩連續性。系統會使用其他視覺提示，例如陰影，來連接應用程式與索引標籤。

### <a name="how-to-modify-tab-colors"></a>如何修改索引標籤色彩

使用中的索引標籤色彩使用現有的標題列自訂 API。 如果您已經自訂應用程式的標題列色彩，當您的應用程式是一個集合時，應用程式索引標籤也會套用此變更。

若要修改索引標籤色彩，請設定 [ApplicationViewTitleBar](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationviewtitlebar) 屬性來指定：

- 您索引標籤的純色背景色彩。
- 您索引標籤文字的純色前景色彩。

此範例示範如何取得 ApplicationViewTitleBar 的執行個體並設定其色彩屬性。

```csharp
// using Windows.UI.ViewManagement;
var titleBar = ApplicationView.GetForCurrentView().TitleBar;

// Set active window colors
titleBar.ForegroundColor = Windows.UI.Colors.White;
titleBar.BackgroundColor = Windows.UI.Colors.Green;
```

如需詳細資訊，請參閱 [標題列自訂](title-bar.md#simple-color-customization) 文章的 _簡單色彩自訂_ 章節和 [標題列自訂範例](http://go.microsoft.com/fwlink/p/?LinkId=620613)。

### <a name="considerations-for-full-title-bar-customization"></a>完整標題列自訂的考量

如 [標題列自訂](title-bar.md#full-customization) 文章的 _完全自訂_ 章節所述，您也可以完全自訂應用程式的標題列。 您通常會執行此動作[將壓克力風格擴充到標題列](../style/acrylic.md#extend-acrylic-into-the-title-bar)，或在標題列中放置自訂內容。 如果採用此方法，請務必依照下列全螢幕與平板電腦模式的指導方針，且當 [CoreApplicationViewTitleBar.IsVisible](/uwp/api/windows.applicationmodel.core.coreapplicationviewtitlebar.isvisible) 是 ** true** 時，只顯示您的自訂標題列內容。

您的應用程式在一個集合中執行時，CoreApplicationViewTitleBar.IsVisible 為 **false**，且不會顯示標題列內容。 不過，如果您未依照本指導方針隱藏自訂標題列內容時，則會出現在您的應用程式索引標籤下方，而不是在標題列區域中。

如果您已在自訂標題列 UI 放置內容或功能，請考慮將它用於應用程式中的其他 UI 介面。

### <a name="how-to-modify-the-tab-icon"></a>如何修改索引標籤圖示

若要確認集合中您的應用程式圖示是否為最佳外觀，您應該針對應用程式提供替代、無背板的圖示。 （您應用程式索引標籤中使用的應用程式圖示與工作列中使用的相同。）替代圖示的目的是為了在任何背景色彩下都有良好外觀。 如果有的話，將會使用替代圖示。

在應用程式清單中，除了您的一般圖示外，請指定一個其他形式無背板的圖示。 如需詳細資訊，請參閱[應用程式圖示及標誌](/windows/uwp/design/style/app-icons-and-logos)。 指定圖示文章的[了解應用程式圖示資產](/windows/uwp/design/style/app-icons-and-logos#more-about-app-icon-assets)區段中記錄為 「 無背板的目標大小清單資產 」。

如果您在應用程式資訊清單中不指定替代圖示，系統會用索引標籤色彩重新版面您的磚圖示，並使用它。

![用於集合中的圖示](images/sets-icons.png)

> 在工作列與應用程式索引標籤中使用相同的圖示。

## <a name="restore-previous-sets-with-user-activities"></a>依據使用者活動還原先前的集合

集合的優點為，在他們啟動一個應用程式或開啟文件時，可以讓您的使用者替應用程式與網站內容還原先前的開啟索引標籤。 （如需詳細資訊，請參閱[集合簡介](https://insider.windows.com/en-us/articles/introducing-sets/) 部落格文章裡的影片。）可透過_使用者活動_啟用。

根據預設，系統會代表您的應用程式建立使用者活動，當使用者啟動應用程式或開啟文件時，可還原您的應用程式至索引標籤。 不過，系統所建立的預設使用者活動僅能在其預設狀態中啟動您的應用程式。 它無法將您的應用程式還原到先前作為集合中的一部分的狀態。

您可以透過提供自訂使用者活動，最佳化您集合的應用程式。 您提供的使用者活動深層連結到您的應用程式，將它還原到狀態上次進行還原集合的一部分。

提供自訂使用者活動：

- 使用適當的 [UserActivity](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivity)，回應 OS 初始化 [UserActivityRequest](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivityrequest)。
- UserActivity 包含啟用深層連結 URI，系統使用其啟動有特定內容的應用程式。

如需詳細資訊，請參閱 [UserActivityRequestManager.UserActivityRequested 事件](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivityrequestmanager.useractivityrequested)，[處理 URI 啟用](https://docs.microsoft.com/windows/uwp/launch-resume/handle-uri-activation)，以及 [即使在裝置上仍繼續使用者活動](https://docs.microsoft.com/windows/uwp/launch-resume/useractivities)。

## <a name="enable-multi-instance-for-uwp-apps"></a>為 UWP 應用程式啟用多個執行個體

從 Windows 10、版本 1803 開始，UWP 應用程式支援多個執行個體。 預設中，UWP 仍然是單一執行個體，您必須在每個您想要多個執行個體的應用程式中明確選擇。

如果您將應用程式做多個執行個體，其提供您可讓使用者同時在多個集合中執行應用程式的優點。 一個集合中一次只能執行一個單一執行個體的應用程式。

如需如何為您的 UWP 應用程式啟用多個執行個體的詳細資訊，請參閱 [建立多個執行個體通用 Windows 應用程式](https://docs.microsoft.com/windows/uwp/launch-resume/multi-instance-uwp)。

## <a name="use-an-in-app-back-button"></a>使用應用程式中的返回按鈕

若要在應用程式中實作向後瀏覽，我們建議您根據 [返回按鈕指導方針](../basics/navigation-history-and-backwards-navigation.md) 在應用程式的 UI 中放置返回按鈕。 如果您的應用程式使用 NavigationView 控制項，則您必須使用 NavigationView 的內建返回按鈕。

如果您的應用程式使用系統的返回按鈕，您必須用應用程式中的返回按鈕來取代。 如此可確保使用者有一致的返回按鈕使用體驗，不管是否在集合中執行應用程式，同時也可確保應用程式的返回按鈕視覺效果維持一致。

如需整合應用程式內返回按鈕的相關詳細指導方針，請參閱[瀏覽歷史與向後瀏覽](../basics/navigation-history-and-backwards-navigation.md)。

### <a name="support-for-the-system-back-button-in-sets"></a>集合中系統返回按鈕的支援

如果您的應用程式使用系統的返回按鈕，而不是應用程式內的按鈕，系統 UI 仍會轉譯系統返回按鈕來確保回溯相容性

- 如果您的應用程式不在集合中，會在標題列中轉譯返回按鈕。 返回按鈕的視覺體驗與使用者互動並不會變更。
- 如果您的應用程式在集合中，則會在系統返回列中轉譯返回按鈕。

系統背面列是「色調」，在索引標籤色調和應用程式內容區域間插入。 色調跨越了整個應用程式的寬度，與左邊緣的返回按鈕。 色調有足以確保返回按鈕的適當觸控目標大小的垂直高度。

![集合中的系統背面列](images/sets-system-back-bar.png)

> 應用程式中顯示的系統背面列。

根據返回按鈕可見度，動態顯示系統背面列。 可看見返回按鈕時，插入系統背面列，轉移應用程式內容到索引標籤色調下方。 隱藏返回按鈕時，動態移除系統背面列，向上轉移應用程式內容符合索引標籤色調。

若要避免您應用程式的 UI 向上或向下轉移，我們建議您使用應用程式內的返回按鈕，而非系統的返回按鈕。

應用程式索引標籤與系統背面列沿用標題列自訂。 如果您使用 ApplicationViewTitleBar Api 指定背景和前景色彩屬性，則索引標籤和系統背面列套用此色彩。

## <a name="related-articles"></a>相關文章

- [標題列自訂](title-bar.md)
- [瀏覽歷程記錄和向後瀏覽](../basics/navigation-history-and-backwards-navigation.md)
- [色彩](../style/color.md)
