---
Description: 如何建立應用程式圖示/標誌，代表您的應用程式，在 [開始] 功能表、 應用程式圖格、 工作列、 Microsoft Store 中，和更多功能。
title: 應用程式圖示及標誌
template: detail.hbs
ms.date: 04/17/2018
ms.topic: article
keywords: Windows 10, UWP
design-contact: Judysa
doc-status: Published
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: b755e8d165d58ce4303d9fefe6d051abce6c9765
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57622453"
---
# <a name="app-icons-and-logos"></a>應用程式圖示及標誌 

每個應用程式圖示/標誌，表示它，而且該圖示會出現在 Windows shell 中的多個位置： 

:::row:::
    :::column:::
        * 您的應用程式視窗標題列
        * 在 [開始] 功能表中的應用程式清單
        * 工作列及 工作管理員
        * 您的應用程式圖格
        * 您的應用程式啟動顯示畫面
        * 在 Microsoft Store
    :::column-end:::
    :::column:::
        ![windows 10 start and tiles](images/assetguidance01.jpg)
    :::column-end:::
:::row-end:::

本文涵蓋建立應用程式圖示的基本概念如何使用 Visual Studio 來管理它們，以及如何管理它們以手動的方式，您應該要。
 
(這篇文章是專為代表應用程式本身，圖示的一般指引，請參閱圖示[圖示](icons.md)文章。)

## <a name="icon-types-locations-and-scale-factors"></a>圖示類型、 位置和縮放比例

根據預設，Visual Studio 會儲存您圖示的資產，資產子目錄中。 以下是不同類型的圖示，它們出現的位置，和它們要呼叫的清單。 

| 圖示名稱 | 會出現在 | 資產檔案名稱 |
| ---      | ---        | --- |
| 小型磚 | 開始功能表 |  SmallTile.png  |
| 中型磚 |[開始] 功能表，Microsoft Store 清單\*  |  Square150x150Logo.png |
| 寬形磚  | 開始功能表   | Wide310x150Logo.png |
| 大型磚   | [開始] 功能表，Microsoft Store 清單\* |  LargeTile.png  |
| 應用程式圖示 | 在 [開始] 功能表、 工作列、 工作管理員的應用程式清單 | Square44x44Logo.png |
| 啟動顯示畫面 | 應用程式的啟動顯示畫面 | SplashScreen.png  |
| 徽章標誌 | 您的應用程式圖格 | BadgeLogo.png  |
| 套件標誌/Store 標誌 | 應用程式安裝程式，合作夥伴中心 」 報告應用程式 」 中的選項的存放區，存放區中的 「 撰寫評論 」 選項 | StoreLogo.png  |

\* 除非您選擇使用[只會顯示到上傳映像存放區中的](/windows/uwp/publish/app-screenshots-and-images#display-only-uploaded-logo-images-in-the-store)。 

若要確保這些圖示看起來 sharp 在每個畫面上，您可以建立多個版本的不同的顯示縮放比例的同一個圖示。 

縮放比例會決定 UI 項目，例如文字的大小。 縮放比例因素的範圍從 100%到 400%。 較大的值建立較大的 UI 項目，使其更容易在高 DPI 顯示器上。 

:::row:::
    :::column:::
        Windows automatically sets the scale factor for each display based on its DPI (dots-per-inch) and the viewing distance of the device. 

        (Users can override the default value by going to the **Settings &gt; Display &gt; Scale and layout** page.)
    :::column-end:::
    :::column:::
        ![](images/icons/display-settings-screen.png)
    :::column-end:::
:::row-end:::  


因為應用程式圖示資產是點陣圖，因此點陣圖並未妥善調整，我們建議為每個縮放比例提供每個圖示資產版本：100%、 125%、 150%、 200%到 400%。 這是大量的圖示 ！ 幸運的是，Visual Studio 提供工具，可讓您更輕鬆地產生及更新這些圖示。 

## <a name="microsoft-store-listing-image"></a>Microsoft Store 列出映像

「 如何指定我的應用程式清單的映像在 Microsoft Store？ 」

根據預設，我們使用一些您的套件從映像存放區中，此頁面上方表格中所述 (以及其他[提交程序期間所提供的映像](https://docs.microsoft.com/en-us/windows/uwp/publish/app-screenshots-and-images))。 不過，您可以選擇防止存放區 （包括 Xbox），Windows 10 上的客戶顯示您的清單時，在您的應用程式封裝中使用的標誌影像，改為讓 使用只有您上傳的映像的存放區。 如此您更能在整個市集中掌控各種不同顯示的應用程式外觀。 （請注意，是否您的產品支援舊版的作業系統版本，這些客戶可能仍會看到映像從您的封裝，即使您使用此選項。）您可以**儲存標誌**一節**存放區清單**提交程序的步驟。

![在 應用程式提交程序期間指定市集標誌](images/app-icons/storelogodisplay.png)

當您核取此方塊時，新的區段稱為**存放區顯示映像**隨即出現。 在這裡，您可以上傳的存放區會用來從您的應用程式套件的標誌影像取代的 3 個映像大小：300 x 300，150 x 150，71x71 像素為單位。 300 x 300 大小是必要，雖然我們建議您提供所有 3 個大小。

如需詳細資訊，請參閱 <<c0> [ 只會顯示到上傳標誌映像存放區中的](/windows/uwp/publish/app-screenshots-and-images#display-only-uploaded-logo-images-in-the-store)。

<!-- ### Fallback images for the Store

The simplest way to control the Store listing image is to specify it during the app submission process. If you don't provide these images during the app submission process, the Store will use a tile image:

1. Large tile
2. Medium tile

If these images aren't provided, the Store will search all matching images of the same image type with a square aspect ratio, preferable with a height greated than the scaled requested height (scaled height is the machine's resolution scale factor * display height). If none of the images meet this criteria, the Store will ignore the scale factor and select an image based on height.  -->

<!-- You can provide screenshots, logos, and other art assets (such as trailers and promotional images to include in your app's Microsoft Store listing. Some of these are required, and some are optional (although some of the optional images are important to include for the best Store display).

The Store may also use your app's tile and other images that you include in your app's package. 

For more information, see [App screenshots, images, and trailers in the Microsoft Store](/windows/uwp/publish/app-screenshots-and-images). -->


## <a name="managing-app-icons-with-the-visual-studio-manifest-designer"></a>管理 Visual Studio 資訊清單設計工具的應用程式圖示

Visual Studio 提供非常有用的工具管理您的應用程式圖示，稱為**資訊清單設計工具**。 

> 如果您還沒有 Visual Studio 2017，有數個版本可用，包括免費版本 (Visual Studio 2017 Community Edition)，和其他版本提供免費試用版。 您可以在此下載： [https://developer.microsoft.com/windows/downloads](https://developer.microsoft.com/windows/downloads)


若要啟動資訊清單設計工具：
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
        2. 在 [**方案總管] 中**，連按兩下 Package.appmxanifest 檔案。
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
        3. 按一下 [**視覺效果資產**] 索引標籤。
    :::column-end:::
    :::column:::
        ![The Visual Assets tab](images/icons/vs-manfiest-designer-visual-assets.png)
    :::column-end:::
:::row-end:::        

## <a name="generating-all-assets-at-once"></a>一次產生的所有資產

中的第一個功能表項目**視覺效果資產**索引標籤上，**所有視覺效果資產**，完全功能其名稱示： 產生您的應用程式必須按下按鈕的每個視覺效果資產。

![Visual Studio 中產生所有視覺資產](images/app-icons/all-visual-assets.png)

您只需要為提供單一映像，並會產生小型的圖格、 中型磚、 大型磚、 寬形磚、 大型磚、 應用程式圖示、 啟動顯示畫面，Visual Studio，並將其封裝標誌資產以供每個縮放比例。

若要一次產生的所有資產：
1. 按一下  **...** 旁邊**來源**欄位，然後選取您想要使用的映像。 如果您使用點陣圖影像，請確定它是至少為 400 x 400 像素，以便您取得明確的結果。 以向量為基礎的映像適合;Visual Studio 可讓您使用 AI (Adobe Illustrator) 和 PDF 檔案。 
2. （選擇性）。在 **顯示設定**區段中，設定這些選項：

    a.  **簡短名稱**:指定您的應用程式的簡短名稱。

    b.  **顯示名稱**:指出您是否想要媒體，或大型磚上顯示的簡短名稱。 

    c. **圖格背景**:指定的十六進位值或圖格背景色彩的色彩名稱。 例如， `#464646`。 預設值為 `transparent`。

    d. **啟動顯示畫面背景**:指定啟動顯示畫面背景的十六進位值或色彩名稱。 

3. 按一下 **產生**。 

Visual Studio 會產生您的影像檔，並將它們新增至專案。 如果您想要變更您的資產，只要重複此程序。 

縮放的圖示的資產會遵循此檔案命名慣例：

*filename*-scale-*scale factor*.png

例如，

Square150x150Logo-調整-100.png、 Square150x150Logo-調整-200.png、 Square150x150Logo-調整-400.png

請注意，Visual Studio 不會預設產生徽章標誌。 這是因為徽章標誌是唯一的且可能不應該符合您其他的應用程式圖示。 如需詳細資訊，請參閱 <<c0> [ 徽章 UWP 應用程式的發行項的通知](/windows/uwp/design/shell/tiles-and-notifications/badges)。 


## <a name="more-about-app-icon-assets"></a>深入了解應用程式圖示資產
Visual Studio 會產生您的專案所需的所有應用程式圖示資產，但如果您想要自訂它們，最好先了解它們不同於其他應用程式資產。 

應用程式圖示資產會出現在許多地方： 在 Windows 工作列，[工作] 檢視、 ALT + TAB，開始磚的右下角。 因為應用程式圖示資產會出現在許多情況下，它會有一些額外的調整大小和 plating 沒有其他資產的選項: 「 目標大小 」 資產和 「 unplated"的資產。 

### <a name="target-size-app-icon-assets"></a>目標大小的應用程式圖示資產
除了標準的縮放因數大小 (「 Square44x44Logo.scale-400.png")，我們也建議您建立 「 目標大小 」 資產。 特定的大小，例如 16 像素，而不是特定的縮放比例因素，例如 400，因此，我們會呼叫這些資產的目標大小。 目標大小資產適用於不使用調整高原系統的介面：

* [開始] 畫面的捷徑清單 (桌面)
* [開始] 畫面的右下角的磚 (桌面)
* 捷徑 (桌面)
* 控制台 (桌面)

以下是清單中的目標大小資產：


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

\* 最少，我們建議您提供這些大小。 

您不需要將邊框間距新增至這些資產；如果需要，Windows 會新增邊框間距。 這些資產至少需要 16 個像素的擺設區域。 

以下是這些資產出現在 Windows 工作列上之圖示中的範例：

![Windows 工作列中的資產](images/assetguidance21.png)

### <a name="unplated-assets"></a>Unplated 的資產
根據預設，Windows 預設會使用目標為基礎的資產在彩色 backplate 之上。 如果您想，您可以提供目標為基礎的 unplated 的資產。 「 Unplated"表示資產將會顯示透明背景。 請記住這些資產會透過各種不同的背景色彩。 

![無背板和有背板資產](images/assetguidance22.png)

以下是此介面上，使用 unplated 應用程式圖示資產：
* 工作列和工作列縮圖 (桌面)
* 工作列捷徑清單
* 工作檢視
* ALT+TAB


### <a name="target-and-unplated-sizing"></a>目標和 unplated 調整大小

以下是針對目標為基礎的資產，在 100%縮放的大小建議：

![以目標為基礎的資產的大小調整 (縮放比例 100%)](images/assetguidance23.png)


## <a name="more-about-splash-screen-assets"></a>深入了解啟動顯示畫面的資產
如需啟動顯示畫面的詳細資訊，請參閱[UWP 啟動顯示畫面文章](/windows/uwp/launch-resume/splash-screens)。

## <a name="more-about-badge-logo-assets"></a>深入了解徽章標誌資產

當您使用的資產產生器來產生您所需要的所有資產時，沒有的理由，為什麼它不會都產生徽章標誌預設： 它們是非常不同於其他應用程式資產。 徽章標誌會出現在通知和應用程式的圖格上的狀態映像。 

如需詳細資訊，請參閱 <<c0> [ 徽章 UWP 應用程式的發行項的通知](/windows/uwp/design/shell/tiles-and-notifications/badges)。


## <a name="customizing-asset-padding"></a>自訂 asset 填補

根據預設，Visual Studio 資產產生器會套用建議的填補至任何映像。 如果您的映像已經包含填補，或您想要延伸到結尾 圖格的完整出血映像，您可以關閉這項功能取消勾選**套用建議的填補**核取方塊。 

### <a name="tile-padding-recommendations"></a>圖格填補建議
如果您想要提供您自己的填補，以下是我們的建議，如圖格。 

有 4 個磚的大小： 小型 (71x71)、 中型 (150 x 150)、 寬 (310 x 150) 和大型 (310x310)。 

每個磚資產都與它所在的磚等大小。

![顯示完整的圖格滲出](images/app-icons/tile-assets1.png)

如果您不想您擴充到邊緣的圖格的圖示，您可以在您的資產使用透明的像素為單位，來建立填補項目。 

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









