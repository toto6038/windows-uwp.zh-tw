---
author: TylerMSFT
title: "啟動 Windows 地圖 app"
description: "了解如何從您的應用程式啟動 Windows 地圖應用程式。"
ms.assetid: E363490A-C886-4D92-9A64-52E3C24F1D98
translationtype: Human Translation
ms.sourcegitcommit: 39a012976ee877d8834b63def04e39d847036132
ms.openlocfilehash: a2f09aa510c9c3db6b8eca25f4c8cee98fa0eb46

---

# 啟動 Windows 地圖 app


\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


了解如何從您的應用程式啟動 Windows 地圖應用程式。 本主題描述 **bingmaps:**、**ms-drive-to:**、**ms-walk-to:** 和 *ms-settings:* 統一資源識別項 (URI) 配置。 使用這些 URI 配置，可針對特定的地圖、方向和搜尋結果啟動 Windows 地圖應用程式，或者從設定應用程式下載 Windows 地圖離線地圖。

**提示** 若要深入了解如何從您的應用程式啟動 Windows 地圖應用程式，請從 GitHub 的 [Windows-universal-samples 儲存機制](http://go.microsoft.com/fwlink/p/?LinkId=619979)下載[通用 Windows 平台 (UWP) 地圖範例](http://go.microsoft.com/fwlink/p/?LinkId=619977)。

## URI 簡介

URI 配置可讓您按一下超連結 (或在 app 中以程式設計方式) 開啟 app。 就像您可以使用 **mailto:** 建立新的電子郵件，或使用 **http:** 開啟網頁瀏覽器一樣，您可以使用 **bingmaps:**、**ms-drive-to:** 和 **ms-walk-to:** 來開啟 Windows 地圖 app。

-   **bingmaps:** URI 可提供位置、搜尋結果、方向及交通的地圖。
-   **ms-drive-to:** URI 可提供從您目前的所在位置出發的轉向建議導航行駛路線指引。
-   **ms-walk-to:** URI 可提供從您目前的所在位置出發的轉向建議導航步行路線指引。

例如，下列 URI 會開啟 Windows 地圖 app，並顯示以紐約市為中心的地圖。

```xml
<bingmaps:?cp=40.726966~-74.006076>
```

![以紐約市為中心的地圖。](images/mapnyc.png)

以下是此 URI 配置的描述：

**bingmaps:?query**

在此 URI 配置中，*query* 是一系列的「參數名稱/值」組：

**&amp;param1=value1&amp;param2=value2 …**

如需完整的可用參數清單，請參閱 [bingmaps:](#bingmaps)、[ms-drive-to:](#msdriveto) 和 [ms-walk-to:](#mswalkto) 參數參考。 本主題稍後也提供相關範例。

## 從您的 app 啟動 URI


若要從您的 app 啟動 Windows 地圖 app，請使用 **bingmaps:**、**ms-drive-to:** 或 **ms-walk-to:** URI 呼叫 [**LaunchUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701476) 方法。 下列範例會啟動與前一個範例中相同的 URI。 如需關於透過 URI 啟動 app 的詳細資訊，請參閱[啟動 URI 的預設 app](launch-default-app.md)。

```cs
// Center on New York City
var uriNewYork = new Uri(@"bingmaps:?cp=40.726966~-74.006076");

// Launch the Windows Maps app
var launcherOptions = new Windows.System.LauncherOptions();
launcherOptions.TargetApplicationPackageFamilyName = "Microsoft.WindowsMaps_8wekyb3d8bbwe";
var success = await Windows.System.Launcher.LaunchUriAsync(uriNewYork, launcherOptions);
```

在這個範例中，會使用 [**LauncherOptions**](https://msdn.microsoft.com/library/windows/apps/hh701435) 類別確保 Windows 地圖 app 可啟動。

## 顯示已知位置


有數種方式可控制地圖中心點和縮放比例。 使用 *cp* (中心點) 和 *lvl* (縮放比例) 參數是最簡單的方法，而且會產生可預料的結果。 使用 *bb* 參數 (指定以緯度和經度值為界的區域) 較難預料，因為它會考量螢幕解析度，並根據所提供的座標決定地圖中心點和縮放比例。 當三個參數 (*bb*、*cp* 和 *lvl*) 都存在時，會忽略 *bb* 參數。

若要控制檢視類型，請使用 *ss* (街景) 和 *sty* (樣式) 與參數。 *ss* 參數會將地圖放入街景檢視中。 *sty* 參數可讓您在道路、空照圖與 3D 檢視之間切換。 使用 3D 樣式時，可使用 *hdg*、*pit* 和 *rad* 參數來指定 3D 檢視。 *hdg* 會指定檢視的朝向、*pit* 會指定檢視的上下移動，而 *rad* 會指定在檢視中顯示的與中心點之間的距離。 如需這些與其他參數的詳細資訊，請參閱 [bingmaps: 參數參考](#bingmaps)。

| URI 範例                                                                 | 結果                                                                                                                                                                                                   |
|----------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| bingmaps:?                                                                 | 開啟地圖 app。                                                                                                                                                                                       |
| bingmaps:?cp=40.726966~-74.006076                                          | 顯示以紐約市為中心的地圖。                                                                                                                                                               |
| bingmaps:?cp=40.726966~-74.006076&amp;lvl=10                                   | 顯示縮放比例 10 以紐約市為中心的地圖。                                                                                                                                       |
| bingmaps:?bb=39.719\_-74.52~41.71\_-73.5                                   | 以螢幕大小當成週框方塊顯示紐約市地圖。                                                                                                                          |
| bingmaps:?bb=39.719\_-74.52~41.71\_-73.5&amp;cp=47~-122                        | 顯示紐約市地圖，這是週框方塊引數中指定的區域。 會略過以 **cp** 引數指定的西雅圖中心點。                                      |
| bingmaps:?bb=39.719\_-74.52~41.71\_-73.5&amp;cp=47~-122&amp;lvl=8                  | 顯示紐約的地圖，這是 **bb** 引數中指定的區域。 **cp** 引數 (指定西雅圖) 會被略過，因為在已指定 **bb** 的情況下，會忽略 **cp** 和 **lvl**。 |
| bingmaps:?collection=point.36.116584\_-115.176753\_Caesars%20Palace&amp;lvl=16 | 將縮放比例設定為 16 來顯示含有 Caesar Palace (拉斯維加斯) 地點名稱的地圖。                                                                                                            |
| bingmaps:?collection=point.40.726966\_-74.006076\_Some%255FBusiness        | 顯示含有 Some\_Business (拉斯維加斯) 地點名稱的地圖。                                                                                                                                          |
| bingmaps:?cp=40.726966~-74.006076&amp;trfc=1&amp;sty=a                             | 顯示具有「交通」資訊和「空照圖」地圖樣式的紐約市地圖。                                                                                                                                               |
| bingmaps:?cp=47.6204~-122.3491&amp;sty=3d                                      | 顯示太空針塔的 3D 檢視。                                                                                                                                                                   |
| bingmaps:?cp=47.6204~-122.3491&amp;sty=3d&amp;rad=200&amp;pit=75&amp;hdg=165               | 顯示半徑為 200 公尺、上下移動為 75 度、朝向為 165 度的太空針塔 3D 檢視。                                                                                        |
| bingmaps:?cp=47.6204~-122.3491&amp;ss=1                                        | 顯示太空針塔的街景檢視。                                                                                                                                                           |

 

## 顯示搜尋結果


建議在執行商店搜尋時使用 *q* 參數，請儘可能使用具體的字詞，並將它與 *cp* 或 *where* 參數搭配使用以指定位置。 如果使用者尚未將其位置的使用權限提供給地圖 app，而且您也未指定商店搜尋的位置，可能會導致在國家/地區層級執行搜尋，而不會傳回有意義的結果。 搜尋結果會顯示在最適當的地圖檢視中，因此，除非需要設定 *lvl* (縮放比例)，否則建議您讓地圖 app 自行決定。 如需這些與其他參數的詳細資訊，請參閱 [bingmaps: 參數參考](#bingmaps)。

| URI 範例                                                    | 結果                                                                                                                                         |
|---------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------|
| bingmaps:?where=1600%20Pennsylvania%20Ave,%20Washington,%20DC | 顯示地圖，並搜尋華盛頓特區白宮的地址。                                                              |
| bingmaps:?cp=40.726966~-74.006076&amp;lvl=10&amp;where=New%20York     | 搜尋靠近指定中心點的紐約，在地圖上顯示結果並將縮放比例設定為 10。                            |
| bingmaps:?lvl=10&amp;where=New%20York                             | 搜尋紐約，並將縮放比例設定為 10 來顯示結果。                                                                                    |
| bingmaps:?cp=40.726966~-74.006076&amp;lvl=14.5&amp;q=pizza            | 搜尋靠近指定中心點 (在紐約) 的披薩店，在地圖上顯示結果並將縮放比例設定為 14.5。 |
| bingmaps:?q=coffee&amp;where=Seattle                              | 搜尋西雅圖市的咖啡廳。                                                                                                                 |

 

## 顯示多個點


使用 *collection* 參數可在地圖上顯示一組自訂的點。 如果有多個點，則會顯示點清單。 一個集合可以有多達 25 個點，並會依提供的順序列出。 集合的優先順序高於搜尋與路線指引要求。 如需關於此參數與其他參數的詳細資訊，請參閱[bingmaps: 參數參考](#bingmaps)。

| URI 範例                                                                                                                                                         | 結果                                                                                                                   |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------|
| bingmaps:?collection=point.36.116584\_-115.176753\_Caesars%20Palace                                                                                                | 搜尋拉斯維加斯的 Caesar's Palace，然後以最佳的地圖檢視在地圖上顯示結果。                         |
| bingmaps:?collection=point.36.116584\_-115.176753\_Caesars%20Palace&amp;lvl=16                                                                                         | 將縮放比例設定為 16 來顯示位於拉斯維加斯名為 Caesars Palace 的圖釘。                                               |
| bingmaps:?collection=point.36.116584\_-115.176753\_Caesars%20Palace~point.36.113126\_-115.175188\_The%20Bellagio&amp;lvl=16&amp;cp=36.114902~-115.176669                   | 將縮放比例設定為 16 來顯示位於拉斯維加斯名為 Caesars Palace 和名為 The Bellagio 的圖釘。              |
| bingmaps:?collection=point.40.726966\_-74.006076\_Fake%255FBusiness%255Fwith%255FUnderscore                                                                        | 以紐約為範圍，顯示名為 Fake\_Business\_with\_Underscore 的圖釘。                                                  |
| bingmaps:?collection=name.Hotel%20List~point.36.116584\_-115.176753\_Caesars%20Palace~point.36.113126\_-115.175188\_The%20Bellagio&amp;lvl=16&amp;cp=36.114902~-115.176669 | 將縮放比例設定為 16 來顯示名為 Hotel List 的清單，以及兩個代表位於拉斯維加斯之 Caesars Palace 和 The Bellagio 的圖釘。 |

 

## 顯示路線指引和交通狀況


您可以使用 *rtp* 參數顯示兩個點之間的路線；這些點可以是地址或緯度和經度座標。 使用 *trfc* 參數可顯示交通資訊。 若要指定路線類型 (開車、步行或運輸工具)，請使用 *mode* 參數。 若未指定 *mode*，則會以使用者的交通喜好設定模式提供路線指引。 如需這些參數與其他參數的詳細資訊，請參閱 [bingmaps: 參數參考](#bingmaps)。

![路線指引範例](images/windowsmapgcdirections.png)

| URI 範例                                                                                                              | 結果                                                                                                                                                         |
|-------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------|
| bingmaps:?rtp=pos.44.9160\_-110.4158~pos.45.0475\_-109.4187                                                             | 顯示點對點的路線地圖。 由於未指定 *mode*，因此會以使用者的交通喜好設定模式提供路線指引。 |
| bingmaps:?cp=43.0332~-87.9167&amp;trfc=1                                                                                    | 顯示以威斯康辛州密爾瓦基市為中心和交通的地圖。                                                                                                        |
| bingmaps:?rtp=adr.One Microsoft Way, Redmond, WA 98052~pos.39.0731\_-108.7238                                           | 顯示從指定地址到指定位置的路線地圖。                                                                            |
| bingmaps:?rtp=adr.1%20Microsoft%20Way,%20Redmond,%20WA,%2098052~pos.36.1223\_-111.9495\_Grand%20Canyon%20northern%20rim | 顯示從 1 Microsoft Way, Redmond, WA, 98052 到大峽谷北緣的路線。                                                                |
| bingmaps:?rtp=adr.Davenport, CA~adr.Yosemite Village                                                                    | 顯示從指定位置到指定地標的駕駛路線地圖。                                                                   |
| bingmaps:?rtp=adr.Mountain%20View,%20CA~adr.San%20Francisco%20International%20Airport,%20CA&amp;mode=d                      | 顯示從加州山景城到加州舊金山國際機場的駕駛路線。                                                                  |
| bingmaps:?rtp=adr.Mountain%20View,%20CA~adr.San%20Francisco%20International%20Airport,%20CA&amp;mode=w                      | 顯示從加州山景城到加州舊金山國際機場的步行路線。                                                                  |
| bingmaps:?rtp=adr.Mountain%20View,%20CA~adr.San%20Francisco%20International%20Airport,%20CA&amp;mode=t                      | 顯示從加州山景城到加州舊金山國際機場的運輸工具路線。                                                                  |

 

## 顯示轉向建議導航路線指引


**ms-drive-to:** 和 **ms-walk-to:** URI 配置可讓您直接啟動至轉向建議導航路線檢視。 這些 URI 配置只能提供從使用者目前所在位置出發的路線指引。 如果您必須提供不包含使用者目前所在位置的兩點之間的路線指引，請使用上一節所說明的 **bingmaps:** URI 配置。 如需這些 URI 配置的詳細資訊，請參閱 [ms-drive-to:](#msdriveto) 和 [ms-walk-to:](#mswalkto) 參數參考。

> **重要** 當 **ms-drive-to:** 或 **ms-walk-to:** URI 配置啟動時，地圖 app 會檢查裝置是否曾經修正 GPS 位置。 如果有，地圖 app 就會前往轉向建議導航路線指引。 如果還未修正，app 將會顯示路線概觀，如[顯示路線指引和交通狀況](#directions)中所述。

 

![轉向建議導航路線指引範例](images/windowsmapsappdirections.png)

| URI 範例                                                                                                | 結果                                                                                       |
|-----------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------|
| ms-drive-to:?destination.latitude=47.680504&amp;destination.longitude=-122.328262&amp;destination.name=Green Lake | 顯示一個地圖，內含從您目前的所在位置到 Green Lake 的詳細行駛路線。 |
| ms-walk-to:?destination.latitude=47.680504&amp;destination.longitude=-122.328262&amp;destination.name=Green Lake  | 顯示一個地圖，內含從您目前的所在位置到 Green Lake 的詳細步行路線。 |


## 下載離線地圖


**ms-settings:** URI 配置可讓您直接啟動到設定應用程式中的特定頁面。 雖然 **ms-settings:** URI 配置不會啟動到地圖應用程式，但是允許您直接啟動到設定應用程式中的 [離線地圖] 頁面，並且顯示確認對話方塊來下載地圖應用程式所使用的離線地圖。 URI 配置接受緯度和經度所指定的值，並自動判斷是否有包含該點之地區可用的離線地圖。  如果緯度和經度剛好落在多個下載地區內，確認對話方塊會讓使用者挑選要下載其中哪一個區域。 如果包含該點的地區沒有離線地圖，則設定應用程式中的 [離線地圖] 頁面會顯示錯誤對話方塊。

| URI 範例                                                                                                | 結果                                                                                       |
|-----------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------|
| ms-settings:maps-downloadmaps?latlong=47.6,-122.3 | 將設定應用程式開啟到 [離線地圖] 頁面，並顯示確認對話方塊來下載包含所指定經緯度點之地區的地圖。 |
 

## bingmaps: 參數參考


此表格中每個參數的語法是使用擴充巴克斯格式 (Augmented Backus–Naur Form, ABNF) 來示範的。

<table>
<colgroup>
<col width="25%" />
<col width="25%" />
<col width="25%" />
<col width="25%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">參數</th>
<th align="left">定義</th>
<th align="left">ABNF 定義和範例</th>
<th align="left">詳細資料</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>**cp**</p></td>
<td align="left"><p>中心點</p></td>
<td align="left"><p>cp = "cp=" cpval</p>
<p>cpval = degreeslat "~" degreeslon</p>
<p>degreeslat = ["-"] 1*3DIGIT ["." 1*7DIGIT]</p>
<p>degreeslon = ["-"] 1*2DIGIT ["." 1*7DIGIT]</p>
<p>範例：</p>
<p>cp=40.726966~-74.006076</p></td>
<td align="left"><p>這兩個值都必須以十進位度數表示，並以波狀符號 (**~**) 分隔。</p>
<p>有效的經度值介於 -180 (含) 到 +180 (含)。</p>
<p>有效的緯度值介於 -90 (含) 到 +90 (含)。</p></td>
</tr>
<tr class="even">
<td align="left"><p>**bb**</p></td>
<td align="left"><p>週框方塊</p></td>
<td align="left"><p>bb = "bb=" southlatitude "_" westlongitude "~" northlatitude "_" eastlongitude</p>
<p>southlatitude = degreeslat</p>
<p>northlatitude = degreeslat</p>
<p>westlongitude = degreeslon</p>
<p>eastlongitude = degreeslon</p>
<p>degreeslat = ["-"] 13DIGIT ["." 17DIGIT]</p>
<p>degreeslon = ["-"] 12DIGIT ["." 17DIGIT]</p>
<p>範例：</p>
<p>bb=39.719_-74.52~41.71_-73.5</p></td>
<td align="left"><p>一個以十進位度數表示來指定週框方塊的矩形區域，使用波狀符號 (**~**) 來分隔左下角和右上角。 每個週框方塊的經緯度會以底線 (**_**) 分隔。</p>
<p>有效的經度值介於 -180 (含) 到 +180 (含)。</p>
<p>有效的緯度值介於 -90 (含) 到 +90 (含)。</p><p>提供週框方塊時，會忽略 cp 和 lvl 參數。</p></td>
</tr>
<tr class="odd">
<td align="left"><p>**where**</p></td>
<td align="left"><p>位置</p></td>
<td align="left"><p>where = "where=" whereval</p>
<p>whereval = 1*( ALPHA / DIGIT / "-" / "." / "_" / pct-encoded / "!" / "$" / "'" / "(" / ")" / "*" / "+" / "," / ";" / ":" / "@" / "/" / "?")</p>
<p>範例：</p>
<p>where=1600%20Pennsylvania%20Ave,%20Washington,%20DC</p></td>
<td align="left"><p>特定位置、地標或地點的搜尋字詞。</p></td>
</tr>
<tr class="even">
<td align="left"><p>**q**</p></td>
<td align="left"><p>查詢字詞</p></td>
<td align="left"><p>q = "q="</p>
<p>whereval</p>
<p>範例：</p>
<p>q=mexican%20restaurants</p></td>
<td align="left"><p>當地商店或商店類別的搜尋字詞。</p></td>
</tr>
<tr class="odd">
<td align="left"><p>**lvl**</p></td>
<td align="left"><p>縮放比例</p></td>
<td align="left"><p>lvl = "lvl=" 1*2DIGIT ["." 1*2DIGIT]</p>
<p>範例：</p>
<p>lvl=10.50</p></td>
<td align="left"><p>定義地圖檢視的縮放比例。 有效值為 1-20，其中 1 是縮到最小。</p></td>
</tr>
<tr class="even">
<td align="left"><p>**sty**</p></td>
<td align="left"><p>樣式</p></td>
<td align="left"><p>sty = "sty=" ("a" / "r"/"3d")</p>
<p>範例：</p>
<p>sty=a</p></td>
<td align="left"><p>定義地圖樣式。 此參數的有效值包括：</p>
<ul>
<li>**a**：顯示地圖的空照圖檢視。</li>
<li>**r**：顯示地圖的道路圖檢視。</li>
<li>**3d**：顯示地圖的立體檢視。 與 **cp** 參數搭配使用，還可以選擇性地搭配 **rad** 參數。</li>
</ul>
<p>在 Windows 10 中，空照圖檢視和 3D 檢視樣式相同。</p>
<div class="alert">
**注意** 省略 **sty** 參數會產生與 sty=r 相同的結果。
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p>**rad**</p></td>
<td align="left"><p>半徑</p></td>
<td align="left"><p>rad = "rad=" 1*8DIGIT</p>
<p>範例：</p>
<p>rad=1000</p></td>
<td align="left"><p>指定所需地圖檢視的圓形區域。 半徑值以公尺為單位。</p></td>
</tr>
<tr class="even">
<td align="left"><p>**pit**</p></td>
<td align="left"><p>上下移動</p></td>
<td align="left"><p>pit = "pit=" pitch</p>
<p>範例：</p>
<p>pit=60</p></td>
<td align="left"><p>指出檢視地圖的角度；90 會遠眺地平線 (最大)，0 會筆直俯瞰 (最小)。</p><p>有效的上下移動值介於 0 (含) 到 90 (含)。</td>
</tr>
<tr class="odd">
<td align="left"><p>**hdg**</p></td>
<td align="left"><p>朝向</p></td>
<td align="left"><p>hdg = "hdg=" heading</p>
<p>範例：</p>
<p>hdg=180</p></td>
<td align="left"><p>以度為單位指出地圖所朝的方向；0 或 360 = 北、90 = 東、180 = 南、270 = 西。</p></td>
</tr>
<tr class="even">
<td align="left"><p>**ss**</p></td>
<td align="left"><p>街景</p></td>
<td align="left"><p>ss = "ss=" BIT</p>
<p>範例：</p>
<p>ss=1</p></td>
<td align="left"><p>指出當 <code>ss=1</code> 時就顯示街景圖。 省略 **ss** 參數會產生與 <code>ss=0</code> 相同的結果。 與 **cp** 參數搭配使用來指定街景檢視的位置。</p>
<div class="alert">
> **注意** 並非所有地區都能使用街景圖。
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p>**trfc**</p></td>
<td align="left"><p>交通</p></td>
<td align="left"><p>trfc = "trfc=" BIT</p>
<p>範例：</p>
<p>trfc=1</p></td>
<td align="left"><p>指定是否要在地圖上包含交通資訊。 省略 trfc 參數會產生與 <code>trfc=0</code> 相同的結果。</p>
<div class="alert">
> **注意** 並非所有地區都有提供交通資料。
</div>
<div>
 
</div></td>
</tr>
<tr class="even">
<td align="left"><p>**rtp**</p></td>
<td align="left"><p>路線</p></td>
<td align="left"><p>rtp = "rtp=" (waypoint "~" [waypoint]) / ("~" waypoint)</p>
<p>waypoint = ("pos." point ) / ("adr." whereval)</p>
<p>point = "point." pointval ["_" title]</p>
<p>pointval = degreeslat "" degreeslon</p>
<p>degreeslat = ["-"] 13DIGIT ["." 17DIGIT]</p>
<p>degreeslon = ["-"] 12DIGIT ["." 17DIGIT]</p>
<p>title = whereval</p>
<p>whereval = 1( ALPHA / DIGIT / "-" / "." / "_" / pct-encoded / "!" / "$" / "'" / "(" / ")" / "" / "+" / "," / ";" / ":" / "@" / "/" / "?")</p>


<p>範例：</p>
<p>rtp=adr.Mountain%20View,%20CA~adr.SFO</p>
<p>rtp=adr.One%20Microsoft%20Way,%20Redmond,%20WA~pos.45.23423_-122.1232 _My%20Picnic%20Spot</p></td>
<td align="left"><p>定義要在地圖上繪製的路線起點和終點，以波狀符號 (**~**) 分隔。 每個導航點都是由使用緯度、經度和選擇性標題或地址識別碼的位置來定義。</p>
<p>完整的路線會正好包含兩個導航點。 例如，<code>rtp="A"~"B"</code> 會定義具有兩個導航點的路線。</p>
<p>也可以接受指定不完整的路線。 例如，您可以使用 <code>rtp="A"~</code> 僅定義路線的起點。 在此情況下，顯示路線指引輸入時，[從]**** 欄位中會有所提供的導航點，而 [到]**** 欄位則為焦點所在。</p>
<p>如果只指定路線的終點，如同<code>rtp=~"B"</code>，則在顯示路線指引面板時，[到]**** 欄位中會有提供的導航點。 如果有正確的目前位置，將會在具有焦點的 [從]**** 欄位中預先填入目前所在位置。</p>
<p>提供的路線不完整時，不會繪製任何路線圖。</p>
<p>與 **mode** 參數搭配使用可指定交通模式 (開車、運輸工具或步行)。 若未指定 **mode**，則會以使用者的交通喜好設定模式提供路線指引。</p>
<div class="alert">
**注意** 如果利用 **pos** 參數值指定位置，就能將標題當成位置使用。 系統將顯示標題，而不是緯度和經度。
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p>**mode**</p></td>
<td align="left"><p>交通模式</p></td>
<td align="left"><p>mode = "mode=" ("d" / "t" / "w")</p>
<p>範例：</p>
<p>mode=d</p></td>
<td align="left"><p>定義交通模式。 此參數的有效值包括：</p>
<ul>
<li>**d**：顯示行駛路線的路線概觀</li>
<li>**t**：顯示大眾運輸路線的路線概觀</li>
<li>**w**：顯示步行路線的路線概觀</li>
</ul>
<p>與 **rtp** 參數搭配使用，提供交通路線指引。 若未指定 **mode**，則會以使用者的交通喜好設定模式提供路線指引。 **mode** 可以與 no route 參數一起提供，來輸入該模式從目前位置的路線指引輸入。</p></td>
</tr>

<tr class="even">
<td align="left"><p>**collection**</p></td>
<td align="left"><p>集合</p></td>
<td align="left"><p>collection = "collection="(name"~"/)point["~"point]</p>
<p>name = "name." whereval </p>
<p>whereval = 1( ALPHA / DIGIT / "-" / "." / "_" / pct-encoded / "!" / "$" / "'" / "(" / ")" / "" / "+" / "," / ";" / ":" / "@" / "/" / "?") </p>
<p>point = "point." pointval ["_" title] </p>
<p>pointval = degreeslat "" degreeslon </p>
<p>degreeslat = ["-"] 13DIGIT ["." 17DIGIT] </p>
<p>degreeslon = ["-"] 12DIGIT ["." 17DIGIT] </p>
<p>title = whereval</p>


<p>範例：</p>
<p>collection=name.My%20Trip%20Stops~point.36.116584_-115.176753_Las%20Vegas~point.37.8268_-122.4798_Golden%20Gate%20Bridge</p></td>
<td align="left"><p>要新增至地圖和清單中的點集合。 使用 name 參數，可以指定點集合。 使用緯度、經度和選擇性標題指定一個點。</p>
<p>使用波狀符號 (**~**) 區隔名稱與多個點。</p>
<p>如果您指定的項目包含波狀符號，請務必要將波狀符號以 <code>%7E</code> 編碼。 如果沒有與「中心點」與「縮放比例」參數搭配使用，集合將會提供最適當的地圖檢視。</p>

<p>**重要** 如果您所指定的項目包含底線，請確定將底線以 %255F 雙重編碼。</p>

<p>如果您所指定的項目包含底線，請確定將底線以 %255F 雙重編碼。</p></td>
</tr>
</tbody>
</table>

 

## ms-drive-to: 參數參考


啟動轉向建議駕駛路線要求的 URI 不需要進行編碼，並且具有下列格式。

> **注意** 您並未在此 URI 配置中指定起點。 起點一律會假設為目前的位置。 如果您需要指定目前所在位置以外的起點，請參閱[顯示路線和交通狀況](#directions)。

 

| 參數 | 定義 | 範例 | 詳細資料 |
|------------|-----------|---------|---------|
| **destination.latitude** | 目的地緯度 | 範例：destination.latitude=47.6451413797194 | 目的地的緯度。 有效的緯度值介於 -90 (含) 到 +90 (含)。 |
| **destination.longitude** | 目的地經度 | 範例：destination.longitude=-122.141964733601 | 目的地的經度。 有效的經度值介於 -180 (含) 到 +180 (含)。 |
| **destination.name** | 目的地的名稱 | 範例：destination.name=Redmond, WA | 目的地的名稱。 您不需要編碼 **destination.name** 值。 |

 

## ms-walk-to: 參數參考


啟動轉向建議步行路線要求的 URI 不需要進行編碼，並且具有下列格式。

> **注意** 您並未在此 URI 配置中指定起點。 起點一律會假設為目前的位置。 如果您需要指定目前所在位置以外的起點，請參閱[顯示路線和交通狀況](#directions)。

 

| 參數 | 定義 | 範例 | 詳細資料 |
|-----------|------------|---------|----------|
| **destination.latitude** | 目的地緯度 | 範例：destination.latitude=47.6451413797194 | 目的地的緯度。 有效的緯度值介於 -90 (含) 到 +90 (含)。 |
| **destination.longitude** | 目的地經度 | 範例：destination.longitude=-122.141964733601 | 目的地的經度。 有效的經度值介於 -180 (含) 到 +180 (含)。 |
| **destination.name** | 目的地的名稱 | 範例：destination.name=Redmond, WA | 目的地的名稱。 您不需要編碼 **destination.name** 值。 |

 
## ms-settings: 參數參考


**ms-settings:** URI 配置的地圖應用程式特定參數的語法定義如下。 **maps-downloadmaps** 是與 **ms-settings:** URI 一起指定，格式為 **ms-settings:maps-downloadmaps?**，以指示離線地圖設定頁面。

 

| 參數 | 定義 | 範例 | 詳細資料 |
|-----------|------------|---------|----------|
| **latlong** | 定義離線地圖區域的點。 | 範例：latlong=47.6,-122.3 | 地理點是以逗號分隔的緯度和經度來指定。 有效的緯度值介於 -90 (含) 到 +90 (含)。 有效的經度值介於 -180 (含) 到 +180 (含)。 |
 

 



<!--HONumber=Aug16_HO3-->


