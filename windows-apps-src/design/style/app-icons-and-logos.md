---
Description: How to create app icons/logos that represent your app in the Start menu, app tiles, the taskbar, the Microsoft Store, and more.
title: 應用程式圖示及標誌
template: detail.hbs
ms.date: 04/17/2018
ms.topic: article
keywords: Windows 10, UWP
design-contact: Judysa
doc-status: Published
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 7083152efb4cf871f8abebf6d2970d2da4ba06e9
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/30/2018
ms.locfileid: "8216102"
---
# <a name="app-icons-and-logos"></a>應用程式圖示及標誌 

每個 app 都代表它，圖示/標誌，並且該圖示會出現在 Windows 殼層中的多個位置： 

:::row:::
    :::column:::
        * 您的應用程式視窗的標題列
        * 在 [開始] 功能表中的應用程式清單
        * 工作列和工作管理員
        * 您的應用程式磚
        * 您的應用程式啟動顯示畫面
        * 在 Microsoft Store 中
    :::column-end:::
    :::column:::
        ![windows 10 start and tiles](images/assetguidance01.jpg)
    :::column-end:::
:::row-end:::

本文涵蓋建立應用程式圖示的基本知識如何使用 Visual Studio 管理它們，以及如何手動，管理，如果您需要將。
 
（這篇文章是專為代表應用程式本身; 如一般圖示的指導方針，請參閱[圖示](icons.md)文章的圖示）。

## <a name="icon-types-locations-and-scale-factors"></a>圖示類型、 位置和縮放比例

根據預設，Visual Studio 會資產子目錄中，儲存您的圖示資產。 以下是不同類型的圖示，它們出現的位置，以及它們正在呼叫的清單。 

| 圖示名稱 | 出現在 | 資產檔案名稱 |
| ---      | ---        | --- |
| 小型磚 | [開始] 功能表 |  SmallTile.png  |
| 中型磚 |[開始] 功能表中，Microsoft Store listing\ *  |  Square150x150Logo.png |
| 寬形磚  | [開始] 功能表   | Wide310x150Logo.png |
| 大型磚   | [開始] 功能表中，Microsoft Store listing\ * |  LargeTile.png  |
| 應用程式圖示 | 在 [開始] 功能表、 工作列、 工作管理員中的應用程式清單 | Square44x44Logo.png |
| 啟動顯示畫面 | 應用程式的啟動顯示畫面 | SplashScreen.png  |
| 徽章標誌 | 您的應用程式磚 | BadgeLogo.png  |
| 套件標誌/microsoft Store 標誌 | 應用程式安裝程式，合作夥伴中心，在市集中存放區中的 「 撰寫評論 「 選項 」 報告應用程式 」 選項 | StoreLogo.png  |

\ * 使用，除非您選擇[只顯示上傳的存放區中的影像](/windows/uwp/publish/app-screenshots-and-images#display-only-uploaded-logo-images-in-the-store)。 

若要確保這些圖示看起來很銳利每個畫面上，您可以建立多個版本的相同的圖示，針對不同的顯示縮放比例。 

縮放比例決定 UI 元素，例如文字的大小。 縮放比例因素介於 400%到 100%。 較大的值建立較大的 UI 元素，讓它們更容易地查看在高 DPI 顯示器上。 

:::row:::
    :::column:::
        Windows automatically sets the scale factor for each display based on its DPI (dots-per-inch) and the viewing distance of the device. 

        (Users can override the default value by going to the **Settings &gt; Display &gt; Scale and layout** page.)
    :::column-end:::
    :::column:::
        ![](images/icons/display-settings-screen.png)
    :::column-end:::
:::row-end:::  


因為應用程式圖示資產為點陣圖，而且不正常調整點陣圖，建議您提供一個版本，每個圖示資產的每個縮放係數： 100%、 125%、 150%、 200%及 400%。 這就是許多圖示 ！ Fortunatly，Visual Studio 提供的工具，可讓您輕鬆產生並更新這些圖示。 

## <a name="microsoft-store-listing-image"></a>Microsoft Store 清單影像

「 如何指定我的應用程式清單的影像在 Microsoft Store 中？ 」

根據預設，我們使用一些您的套件中的影像，在市集中頂端 （以及其他[您在提交程序期間提供的映像](https://docs.microsoft.com/en-us/windows/uwp/publish/app-screenshots-and-images)），此頁面的表格中所述。 不過，您可以選擇防止在市集中對 （包括 Xbox） 的 Windows 10 的客戶顯示清單時使用您的應用程式套件中的標誌影像，並改為讓 microsoft Store 使用您上傳的影像。 這可讓您更多控制您的應用程式外觀在各種不同顯示整個 microsoft Store 中。 （請注意，是否您的產品支援較舊版本的作業系統版本，這些客戶可能仍然看到映像從您的套件，即使您使用此選項）。**市集標誌**一節的提交程序的**市集清單**步驟中，您可以執行這項操作。

![在應用程式提交程序期間指定市集標誌](images/app-icons/storelogodisplay.png)

當您勾選此方塊時，新的章節，稱為**市集顯示影像**會出現。 在這裡，您可以上傳 3 個市集將會使用您的應用程式套件的標誌影像取代的影像大小： 300 x 300、 150 x 150 及 71 x 71 像素。 雖然我們建議您提供所有的 3 個大小，是必要的只 300 x 300 大小。

如需詳細資訊，請參閱[僅顯示上傳的標誌影像，在市集中](/windows/uwp/publish/app-screenshots-and-images#display-only-uploaded-logo-images-in-the-store)。

<!-- ### Fallback images for the Store

The simplest way to control the Store listing image is to specify it during the app submission process. If you don't provide these images during the app submission process, the Store will use a tile image:

1. Large tile
2. Medium tile

If these images aren't provided, the Store will search all matching images of the same image type with a square aspect ratio, preferable with a height greated than the scaled requested height (scaled height is the machine's resolution scale factor * display height). If none of the images meet this criteria, the Store will ignore the scale factor and select an image based on height.  -->

<!-- You can provide screenshots, logos, and other art assets (such as trailers and promotional images to include in your app's Microsoft Store listing. Some of these are required, and some are optional (although some of the optional images are important to include for the best Store display).

The Store may also use your app's tile and other images that you include in your app's package. 

For more information, see [App screenshots, images, and trailers in the Microsoft Store](/windows/uwp/publish/app-screenshots-and-images). -->


## <a name="managing-app-icons-with-the-visual-studio-manifest-designer"></a>管理應用程式圖示與 Visual Studio 資訊清單設計工具

Visual Studio 提供非常有用的工具來管理您的應用程式圖示稱為**資訊清單設計工具**。 

> 如果您還沒有 Visual Studio 2017，有數個版本供使用，包括可用的版本，（Visual Studio 2017 社群版本），以及其他版本提供免費試用。 您可以它們從這裡下載：[https://developer.microsoft.com/windows/downloads](https://developer.microsoft.com/windows/downloads)


啟動 [資訊清單設計工具：
<!-- 1. Use Visual Studio to open a UWP project.
2. In the **Solution Explorer**, double-click the package.appmanifest file. 

    ![The Visual Studio 2017 Solution Explorer](images/icons/vs-solution-explorer.png)

    Visual Studio displays the manifest designer.

    ![The Visual Studio 2017 manifest designer](images/icons/vs-manfiest-designer.png)
3. Click the **Visual Assets** tab.

    ![The Visual Assets tab](images/icons/vs-manfiest-designer-visual-assets.png) -->


:::row:::
    :::column:::
        1. 使用 Visual Studio 中開啟 UWP 專案。
    :::column-end:::
    :::column:::
        
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        2. 在**方案總管]** 中，按兩下 Package.appmxanifest 檔案。
    :::column-end:::
    :::column:::
        ![The Visual Studio 2017 Manifest Designer](images/icons/vs-solution-explorer.png)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
            Visual Studio displays the Manifest Designer.
    :::column-end:::
    :::column:::
            ![The Visual Assets tab](images/icons/vs-manfiest-designer.png)
    :::column-end:::
:::row-end:::    
:::row:::
    :::column:::
        3. 按一下 [**視覺資產**] 索引標籤。
    :::column-end:::
    :::column:::
        ![The Visual Assets tab](images/icons/vs-manfiest-designer-visual-assets.png)
    :::column-end:::
:::row-end:::        

## <a name="generating-all-assets-at-once"></a>一次產生所有資產

第一個功能表項目，**視覺資產**] 索引標籤中，**所有的視覺資產**，沒有正好什麼其名稱建議： 產生您的 app 需要按下按鈕與每個視覺資產。

![在 Visual Studio 中產生所有視覺資產](images/app-icons/all-visual-assets.png)

您只需要是提供單一影像，以及 Visual Studio 會產生小型磚，中型磚、 大型磚、 寬形磚、 大型磚、 應用程式圖示，啟動顯示畫面，並針對每個縮放比例的標誌資產套件。

若要一次產生所有資產：
1. 按一下 **...** 旁邊的**來源**欄位，並選取您想要使用的影像。 如果您使用點陣圖影像，請確定它是至少 400 400 個像素，這樣您取得銳利的結果。 向量影像達到最佳運作狀況;Visual Studio 可讓您使用 AI (Adobe Illustrator) 和 PDF 檔案。 
2. （選用）。在 [**顯示設定**] 區段中，設定下列選項：

    a.  **簡短名稱**： 指定您的應用程式的簡短名稱。

    b。  **顯示名稱**： 指出您是否要在中型、 寬或大型磚上顯示的簡短名稱。 

    c. **磚背景**： 指定十六進位值或磚背景色彩的色彩名稱。 例如，`#464646`。 預設值為 `transparent`。

    d. **Spash 畫面背景**： 指定 spash 畫面背景的十六進位值或色彩名稱。 

3. 按一下**產生**。 

Visual Studio 產生您的影像檔案，並將其新增至專案。 如果您想要變更您的資產，只需重複此程序。 

縮放的圖示資產，請遵循此檔案命名慣例：

*檔案名稱*-縮放比例-*縮放比例*.png

例如，

Square150x150Logo-縮放比例-100.png、 Square150x150Logo-縮放比例-200.png、 Square150x150Logo-縮放比例-400.png

請注意，Visual Studio 預設不會產生徽章標誌。 這是因為您的徽章標誌是唯一的且可能不應該符合您的應用程式圖示。 如需詳細資訊，請參閱[適用於 UWP 應用程式的文章的徽章通知](/windows/uwp/design/shell/tiles-and-notifications/badges)。 


## <a name="more-about-app-icon-assets"></a>了解應用程式圖示資產
Visual Studio 將會產生您的專案所需的所有應用程式圖示資產，但如果您想要自訂它們，它有助於了解如何在不同於其他應用程式的資產。 

應用程式圖示資產出現在許多的位置： 在 Windows 工作列、 工作檢視、 ALT + TAB 及及開始畫面磚的右下角。 應用程式圖示資產出現在許多，因為它會有一些額外的調整大小和 plating 沒有其他資產的選項: 「 目標大小 」 資產和 「 無背板 」 的資產。 

### <a name="target-size-app-icon-assets"></a>目標大小的應用程式圖示資產
除了標準的縮放比例大小 (「 Square44x44Logo.scale-400.png 」)，我們也建議您建立 「 目標大小 」 資產。 我們會呼叫這些資產的目標大小，因為它們以特定的大小，例如 16 個像素，而不是特定的縮放比例，例如 400 為目標。 目標大小的資產的表面，不要使用縮放倍數系統如下：

* [開始] 畫面的捷徑清單 (桌面)
* [開始] 畫面的右下角的磚 (桌面)
* 捷徑 (桌面)
* 控制台 (桌面)

以下是目標大小的資產的清單：


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

\ * 最低限度，建議您提供這些大小。 

您不需要將邊框間距新增至這些資產；如果需要，Windows 會新增邊框間距。 這些資產至少需要 16 個像素的擺設區域。 

以下是這些資產出現在 Windows 工作列上之圖示中的範例：

![Windows 工作列中的資產](images/assetguidance21.png)

### <a name="unplated-assets"></a>無背板的資產
根據預設，Windows 預設會使用以目標為基礎的資產在彩色背板上方。 如果您想要您可以提供目標為基礎的無背板的資產。 「 無背板 」 表示資產將會顯示在透明背景上。 請記住，這些資產將會顯示在各種不同的背景色彩。 

![無背板和有背板資產](images/assetguidance22.png)

以下是使用無背板的應用程式圖示資產的表面：
* 工作列和工作列縮圖 (桌面)
* 工作列捷徑清單
* 工作檢視
* ALT+TAB


### <a name="target-and-unplated-sizing"></a>目標 」 和 「 無背板的大小調整

以下是以目標為基礎的資產，縮放比例 100%的大小建議：

![以目標為基礎的資產的大小調整 (縮放比例 100%)](images/assetguidance23.png)


## <a name="more-about-splash-screen-assets"></a>了解啟動顯示畫面資產
如需啟動顯示畫面的詳細資訊，請參閱[UWP 啟動顯示畫面的文章](/windows/uwp/launch-resume/splash-screens)。

## <a name="more-about-badge-logo-assets"></a>了解徽章標誌資產

當您使用的資產產生器來產生您需要的所有資產時，它為什麼預設不會都產生徽章標誌的原因是： 它們很大的其他應用程式的資產。 徽章標誌是出現在通知和 app 的磚上的狀態映像。 

如需詳細資訊，請參閱[適用於 UWP 應用程式的文章的徽章通知](/windows/uwp/design/shell/tiles-and-notifications/badges)。


## <a name="customizing-asset-padding"></a>自訂資產邊框間距

根據預設，Visual Studio 資產產生器適用於任何影像建議的邊框間距。 如果您的映像已經包含邊框間距，或您想要延伸到磚的結束的頁映像，您可以關閉此功能核**套用建議的邊框間距**核取方塊。 

### <a name="tile-padding-recommendations"></a>磚邊框間距建議
如果您想要提供您自己的邊框間距，以下是我們建議針對磚。 

有 4 個磚大小： 小型 (71 x 71)、 中型 (150 x 150)、 全 (310 x 150)，以及大型 (310 x 310)。 

每個磚資產都與它所在的磚等大小。

![磚顯示頁](images/app-icons/tile-assets1.png)

如果您不想您的圖示，延伸到磚的邊緣，您可以使用您的資產中透明的像素建立邊框間距。 

![磚和背板](images/assetguidance05.png)

對於小型磚，將圖示寬度和高度限制為磚大小的 66%：

![小型磚大小調整比例](images/assetguidance09.png)

對於中型磚，將圖示寬度限制為磚大小的 66%，將高度限制為磚大小的 50%。 這樣可以防止商標列中的元素重疊：

![中型磚大小調整比例](images/assetguidance10.png)

對於寬型磚，將圖示寬度限制為磚大小的 66%，將高度限制為磚大小的 50%。 這樣可以防止商標列中的元素重疊：

![寬型磚大小調整比例](images/assetguidance11.png)

對於大型磚，將圖示寬度限制為磚大小的 66%，將高度限制為磚大小的 50%：

![大型磚大小比例](images/assetguidance12.png)

有些圖示是水平或垂直方向，但其他圖示卻有著更複雜的圖形，使其無法恰好符合目標尺寸。 看起來像置中的圖示可能偏向另一側。 在這個案例中，假如圖示的視覺大小與恰好符合的圖示相同，則部分圖示可能會超出建議的擺設區域：

![三個置中的圖示](images/assetguidance13.png)

對於跨頁資產，請考量在磚的邊界和邊緣內互動的元素。 將邊界維持在至少是磚高或磚寬的 16%。 如為最小型磚大小，這個百分比表示邊界寬度的兩倍：

![含邊界的跨頁磚](images/assetguidance14.png)

在這個範例中，邊界太擠了：

![邊界過小的跨頁磚](images/assetguidance15.png)









