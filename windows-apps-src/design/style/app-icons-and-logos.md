---
Description: 如何在 [開始] 功能表、應用程式磚、工作列、Microsoft Store 等中建立代表您的應用程式的應用程式圖示/標誌。
title: 應用程式圖示及標誌
template: detail.hbs
ms.date: 04/17/2018
ms.topic: article
keywords: windows 10, uwp
design-contact: Judysa
doc-status: Published
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 25d9df392d6ed2725b171fe6513334a39458410b
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/29/2020
ms.locfileid: "75684590"
---
# <a name="app-icons-and-logos"></a>應用程式圖示及標誌 

每個應用程式都有一個代表它的圖示/標誌，該圖示會出現在 Windows 殼層中的多個位置： 

:::row:::
    :::column:::
        * [開始] 功能表中的應用程式清單
        * 工作列及工作管理員
        * 您的應用程式磚
        * 您的應用程式啟動顯示畫面
        * 在 Microsoft Store 中
    :::column-end:::
    :::column:::
        ![Windows 10 的開始畫面和磚](images/assetguidance01.jpg)
    :::column-end:::
:::row-end:::

本文涵蓋建立應用程式圖示的基本概念、如何使用 Visual Studio 來管理它們，以及如何在需要時以手動的方式管理它們。
 
(本文是特別針對代表應用程式本身的圖示；如需有關圖示的一般指引，請參閱[圖示](icons.md)一文。)

## <a name="icon-types-locations-and-scale-factors"></a>圖示類型、位置和縮放比例

根據預設，Visual Studio 會將您的圖示資產儲存在資產子目錄中。 以下是不同類型的圖示、它們出現的位置，以及它們要呼叫之項目的清單。 

| 圖示名稱 | 顯示位置 | 資產檔案名稱 |
| ---      | ---        | --- |
| 小型磚 | 開始功能表 |  SmallTile.png  |
| 中型磚 |[開始] 功能表，Microsoft Store 清單\*  |  Square150x150Logo.png |
| 寬型磚  | 開始功能表   | Wide310x150Logo.png |
| 大型磚   | [開始] 功能表，Microsoft Store 清單\* |  LargeTile.png  |
| 應用程式圖示 | [開始] 功能表、工作列、工作管理員中的應用程式清單 | Square44x44Logo.png |
| 啟動顯示畫面 | 應用程式的啟動顯示畫面 | SplashScreen.png  |
| 徽章標誌 | 您的應用程式磚 | BadgeLogo.png  |
| 套件標誌/Store 標誌 | 應用程式安裝程式、合作夥伴中心、Store 中的 [回報應用程式] 選項、Store 中的 [撰寫評論] 選項 | StoreLogo.png  |

除非您選擇[僅顯示 Store 中上傳的影像](/windows/uwp/publish/app-screenshots-and-images#display-only-uploaded-logo-images-in-the-store)，否則將使用 \*。 

若要確保這些圖示在每個畫面上看起來清晰，您可以為不同的顯示縮放比例建立相同圖示的多個版本。 

縮放比例決定 UI 元素的大小，例如文字。 縮放比例的範圍從 100% 到 400%。 較大的值會建立較大的 UI 元素，使其更容易在高 DPI 顯示器上看到。 

:::row:::
   :::column:::
      Windows 會根據 DPI (每英吋點數) 以及裝置的檢視距離，自動設定每個螢幕的縮放比例 
      (使用者可以藉由前往 **[設定] &gt; [顯示] &gt; [縮放與版面配置]** 頁面來覆寫預設值)。
   :::column-end:::
   :::column:::
      ![](images/icons/display-settings-screen.png)
   :::column-end:::
:::row-end:::  


因為應用程式圖示資產是點陣圖，且點陣圖無法妥善縮放，我們建議為每個縮放比例提供每個圖示資產的版本：100%、125%、150%、200% 和 400%。 好多圖示啊！ 幸運的是，Visual Studio 提供了一個工具，可讓您更輕鬆地產生及更新這些圖示。 

## <a name="microsoft-store-listing-image"></a>Microsoft Store 清單影像

「如何在 Microsoft Store 中為我的應用程式清單指定影像？」

根據預設，我們會使用 Store 中來自您的套件中的一些影像，如本頁頂端表格中所述 (以及您在提交程序期間所提供的其他[影像](https://docs.microsoft.com/windows/uwp/publish/app-screenshots-and-images) \(英文\))。 不過，您也可以選擇性地為 Windows 10 (包含 Xbox) 客戶顯示清單時，防止 Microsoft Store 使用您應用程式套件中的標誌影像，指定讓 Microsoft Store 只使用您上傳的影像。 如此您更能在整個 Store 中掌控各種不同顯示的應用程式外觀。 (請注意，如果您的產品支援舊版的作業系統版本，那麼即使您使用此選項，這些客戶仍可能會看到套件中的影像。)您可以在提交程序的 [Store 清單]  步驟的 [Store 標誌]  區段執行此操作。

![在應用程式提交程序中指定 Store 標誌](images/app-icons/storelogodisplay.png)

勾選此方塊時，就會出現稱為 [Store 顯示影像]  的新區段。 在這裡，您可以上傳 Store 將使用的 3 種圖片尺寸來取代應用程式套件中的標誌影像：300 x 300、150 x 150 和 71 x 71 像素。 雖然我們建議提供所有 3 種大小，但僅需要 300 x 300 大小。

如需詳細資訊，請參閱[僅顯示 Store 中上傳的標誌影像](/windows/uwp/publish/app-screenshots-and-images#display-only-uploaded-logo-images-in-the-store)。

<!-- ### Fallback images for the Store

The simplest way to control the Store listing image is to specify it during the app submission process. If you don't provide these images during the app submission process, the Store will use a tile image:

1. Large tile
2. Medium tile

If these images aren't provided, the Store will search all matching images of the same image type with a square aspect ratio, preferable with a height greated than the scaled requested height (scaled height is the machine's resolution scale factor * display height). If none of the images meet this criteria, the Store will ignore the scale factor and select an image based on height.  -->

<!-- You can provide screenshots, logos, and other art assets (such as trailers and promotional images to include in your app's Microsoft Store listing. Some of these are required, and some are optional (although some of the optional images are important to include for the best Store display).

The Store may also use your app's tile and other images that you include in your app's package. 

For more information, see [App screenshots, images, and trailers in the Microsoft Store](/windows/uwp/publish/app-screenshots-and-images). -->


## <a name="managing-app-icons-with-the-visual-studio-manifest-designer"></a>使用 Visual Studio 資訊清單設計工具管理應用程式圖示

Visual Studio 提供了一個非常有用的工具來管理名為**資訊清單設計工具**的應用程式圖示。 

> 如果您還沒有 Visual Studio 2019，有數個版本可用，包括免費版本 (Visual Studio 2019 Community Edition)，其他版本則提供免費試用。 您可以在此下載：[https://developer.microsoft.com/windows/downloads](https://developer.microsoft.com/windows/downloads)


啟動資訊清單設計工具：
<!-- 1. Use Visual Studio to open a UWP project.
2. In the **Solution Explorer**, double-click the package.appmanifest file. 

    ![The Visual Studio 2019 Solution Explorer](images/icons/vs-solution-explorer.png)

    Visual Studio displays the manifest designer.

    ![The Visual Studio 2019 manifest designer](images/icons/vs-manfiest-designer.png)
3. Click the **Visual Assets** tab.

    ![The Visual Assets tab](images/icons/vs-manfiest-designer-visual-assets.png) -->


:::row:::
    :::column:::
        1. 使用 Visual Studio 開啟 UWP 專案。
    :::column-end:::
    :::column:::
        
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        2. 在[方案總管]  中，按兩下Package.appmxanifest 檔案。
    :::column-end:::
    :::column:::
        ![Visual Studio 2019 資訊清單設計工具](images/icons/vs-solution-explorer.png)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
            Visual Studio 顯示資訊清單設計工具。
    :::column-end:::
    :::column:::
            ![[視覺資產] 索引標籤](images/icons/vs-manfiest-designer.png)
    :::column-end:::
:::row-end:::    
:::row:::
    :::column:::
        3. 按一下 [視覺資產]  索引標籤。
    :::column-end:::
    :::column:::
        ![[視覺資產] 索引標籤](images/icons/vs-manfiest-designer-visual-assets.png)
    :::column-end:::
:::row-end:::        

## <a name="generating-all-assets-at-once"></a>一次產生所有資產

[視覺資產]  索引標籤中的第一個功能表項目 [所有視覺資產]  可執行的動作完全符合其名稱：只需按一下按鈕即可產生您的應用程式所需的每個視覺資產。

![在 Visual Studio 中產生所有視覺資產](images/app-icons/all-visual-assets.png)

您只需要提供單一影像，Visual Studio 將為每個縮放比例產生小型磚、中型磚、大型磚、寬型磚、大型磚、應用程式圖示、啟動顯示畫面和套件標誌資產。

若要一次產生所有資產：
1. 按一下 [來源]  欄位旁邊的 [...]  ，然後選取您想要使用的影像。 如果您使用點陣圖影像，請確定它至少為 400 x 400 像素，以便您取得清晰的效果。 以向量為基礎的影像效果最好；Visual Studio 允許您使用 AI (Adobe Illustrator) 和 PDF 檔案。 
2. (選用。)在 [顯示設定]  區段中，設定以下選項：

    a.  **簡短名稱**：指定您應用程式的簡短名稱。

    b.  **顯示名稱**：指出您是否要在中型、寬型或大型磚上顯示簡短名稱。 

    c. **磚背景**：指定磚背景色彩的十六進位值或色彩名稱。 例如，`#464646`。 預設值為 `transparent`。

    d. **啟動顯示畫面背景**：指定啟動顯示畫面背景的十六進位值或色彩名稱。 

3. 按一下 [產生]  。 

Visual Studio 會產生您的影像檔，並將它們新增至專案中。 如果您想要變更您的資產，只需重複此程序即可。 

縮放圖示資產會遵循此檔案命名慣例：

*檔名*-縮放-*縮放比例*.png

例如，

Square150x150Logo-scale-100.png、Square150x150Logo-scale-200.png、Square150x150Logo-scale-400.png

請注意，Visual Studio 預設不會產生徽章標誌。 這是因為您的徽章標誌是獨一無二的，可能與您的其他應用程式圖示不符合。 如需詳細資訊，請參閱 [UWP 應用程式的徽章通知](/windows/uwp/design/shell/tiles-and-notifications/badges)一文。 


## <a name="more-about-app-icon-assets"></a>深入了解應用程式圖示資產
Visual Studio 會產生專案所需的所有應用程式圖示資產，但如果您想要自訂它們，最好先了解它們與其他應用程式資產的區別。 

應用程式圖示資產會出現在許多地方：Windows 工作列、[工作] 檢視、ALT + TAB 和 [開始] 磚的右下角。 因為應用程式圖示資產會出現在很多地方，所以它會有一些其他資產所沒有的額外調整大小和背板選項：「目標大小」資產和「無背板」資產。 

### <a name="target-size-app-icon-assets"></a>目標大小應用程式圖示資產
除了標準的縮放比例大小 ("Square44x44Logo.scale-400.png") 之外，我們也建議您建立「目標大小」資產。 我們將這些資產稱為目標大小，因為它們針對的是特定的大小，例如 16 像素，而不是特定的縮放比例，例如 400。 目標大小資產適用於不使用縮放倍數系統的曲面：

* [開始] 畫面的捷徑清單 (桌面)
* [開始] 畫面右下角的磚 (桌面)
* 捷徑 (桌面)
* 控制台 (桌面)

以下是目標大小資產的清單：


| 資產大小 | 檔案名稱範例                  |
|------------|------------------------------------|
| 16x16\*    | Square44x44Logo.targetsize-16.png  |
| 24x24\*    | Square44x44Logo.targetsize-24.png  |
| 32x32\*    | Square44x44Logo.targetsize-32.png  |
| 48x48\*    | Square44x44Logo.targetsize-48.png  |
| 256x256\*  | Square44x44Logo.targetsize-256.png |
| 20x20      | Square44x44Logo.targetsize-20.png  |
| 30x30      | Square44x44Logo.targetsize-30.png  |
| 36x36      | Square44x44Logo.targetsize-36.png  |
| 40x40      | Square44x44Logo.targetsize-40.png  |
| 60x60      | Square44x44Logo.targetsize-60.png  |
| 64x64      | Square44x44Logo.targetsize-64.png  |
| 72x72      | Square44x44Logo.targetsize-72.png  |
| 80x80      | Square44x44Logo.targetsize-80.png  |
| 96x96      | Square44x44Logo.targetsize-96.png  |

\* 我們建議至少提供這些大小。 

您不需要將邊框間距新增至這些資產；如果需要，Windows 會新增邊框間距。 這些資產至少需要 16 個像素的擺設區域。 

以下是這些資產出現在 Windows 工作列上圖示中的範例：

![Windows 工作列中的資產](images/assetguidance21.png)

### <a name="unplated-assets"></a>無背板資產
根據預設，Windows 預設會在彩色背板上使用以目標為基礎的資產。 如果需要，您可以提供以目標為基礎的無背板資產。 「無背板」表示資產將顯示在透明背景上。 請記住，這些資產會透過各種背景色彩顯示。 

![無背板和有背板資產](images/assetguidance22.png)

以下是使用無背板應用程式圖示資產的曲面：
* 工作列和工作列縮圖 (桌面)
* 工作列捷徑清單
* 工作檢視
* ALT+TAB

### <a name="unplated-assets-and-themes"></a>無背板資產和佈景主題

使用者選取的佈景主題決定工作列的色彩。 如果無背板資產未針對目前的佈景主題特別限定，則系統將檢查資產的對比度。 如果工作列有足夠的對比，則系統會使用它。 否則，系統會尋找高對比度版本的資產。 如果系統找不到任何高對比度版本的資產，則系統會繪製背板形式的資產。 


### <a name="target-and-unplated-sizing"></a>目標和無背板調整大小

以下是以目標為基礎的資產的大小建議 (縮放比例 100%)：

![縮放比例 100%，以目標為基礎的資產](images/assetguidance23.png)


## <a name="more-about-splash-screen-assets"></a>深入了解啟動顯示畫面資產
如需啟動顯示畫面的詳細資訊，請參閱 [UWP 啟動顯示畫面](/windows/uwp/launch-resume/splash-screens)一文。

## <a name="more-about-badge-logo-assets"></a>深入了解徽章標誌資產

當您使用資產產生器來產生所需的所有資產時，預設情況下它不會產生徽章標誌的原因是：它們與其他應用程式資產非常不同。 徽章標誌會出現在通知和應用程式磚上的狀態影像。 

如需詳細資訊，請參閱 [UWP 應用程式的徽章通知](/windows/uwp/design/shell/tiles-and-notifications/badges)一文。


## <a name="customizing-asset-padding"></a>自訂資產邊框間距

根據預設，Visual Studio 資產產生器會將建議的邊框間距套用至任何影像。 如果您的影像已包含邊框間距或您想要延伸到磚結尾的跨頁影像，則可以透過取消勾選 [套用建議的填補]  核取方塊來關閉這項功能。 

### <a name="tile-padding-recommendations"></a>磚邊框間距建議
如果您想要提供您自己的邊框間距，以下是我們對磚的建議。 

磚有 4 種大小：小型 (71 x 71)、中型 (150 x 150)、寬型 (310 x 150) 和大型 (310 x 310)。 

每個磚資產的大小與其所在的磚一樣。

![磚會跨頁顯示](images/app-icons/tile-assets1.png)

如果您不希望圖示延伸到磚的邊緣，則可以在資產中使用透明像素來建立邊框間距。 

![磚和背板](images/assetguidance05.png)

對於小型磚，將圖示寬度和高度限制為磚大小的 66%：

![小型磚的大小比例](images/assetguidance09.png)

對於中型磚，將圖示寬度限制為磚大小的 66%，將高度限制為磚大小的 50%。 這可防止品牌列中的元素重疊：

![中型磚的大小比例](images/assetguidance10.png)

對於寬型磚，將圖示寬度限制為磚大小的 66%，將高度限制為磚大小的 50%。 這可防止品牌列中的元素重疊：

![寬型磚的大小比例](images/assetguidance11.png)

對於大型磚，將圖示寬度限制為磚大小的 66%，將高度限制為磚大小的 50%：

![大型磚的大小比例](images/assetguidance12.png)

有些圖示是水平或垂直方向，但其他圖示卻有著更複雜的圖形，使其無法恰好符合目標尺寸。 看起來像置中的圖示可能偏向另一側。 在這個案例中，假如圖示的視覺大小與恰好符合的圖示相同，則部分圖示可能會超出建議的擺設區域：

![三個置中的圖示](images/assetguidance13.png)

對於跨頁資產，請考量在磚的邊界和邊緣內互動的元素。 將邊界維持在至少是磚高或磚寬的 16%。 如為最小型磚大小，這個百分比表示邊界寬度的兩倍：

![含邊界的跨頁磚](images/assetguidance14.png)

在這個範例中，邊界太擠了：

![邊界過小的跨頁磚](images/assetguidance15.png)


## <a name="optimizing-for-specific-themes-languages-and-other-conditions"></a>針對特定佈景主題、語言和其他條件進行最佳化 

本文說明如何為特定的縮放比例建立資產，但您也可以建立各種不同條件及條件組合的資產。 例如，您可以針對高對比度顯示器或淺色佈景主題和深色佈景主題建立圖示。 您甚至可以為特定語言建立資產。

如需相關指示，請參閱[針對語言、縮放比例、高對比及其他限定詞量身打造您的資源](../../app-resources/tailor-resources-lang-scale-contrast.md))。













