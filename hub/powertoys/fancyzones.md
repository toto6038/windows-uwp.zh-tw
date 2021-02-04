---
title: Windows 10 的 Powertoy FancyZones 公用程式
description: 視窗管理員公用程式，可將視窗排列和貼齊有效率的版面配置
ms.date: 12/02/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: b1f417307c173e868284254c0a1721e4e6ef5536
ms.sourcegitcommit: 382ae62f9d9bf980399a3f654e40ef4f85eae328
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/04/2021
ms.locfileid: "99534427"
---
# <a name="fancyzones-utility"></a>FancyZones 公用程式

FancyZones 是一種視窗管理員公用程式，可將視窗排列和貼齊至有效率的版面配置，以加快工作流程的速度並快速地還原版面配置。 FancyZones 可讓使用者針對針對 windows 拖曳目標的桌面，定義一組視窗位置。  當使用者將視窗拖曳至區域時，視窗會調整大小並重新置放以填滿該區域。  

![FancyZones 螢幕擷取畫面](../images/pt-fancy-zones2.png)

## <a name="getting-started"></a>開始使用

### <a name="enable"></a>啟用

若要開始使用 FancyZones，您必須在 Powertoy 設定中啟用公用程式，然後再叫用 FancyZones 編輯器 UI。  

### <a name="launch-zones-editor"></a>啟動區域編輯器

使用 [powertoy 設定] 功能表中的按鈕或按 [ (<kbd>Win</kbd> ] 來啟動 [區域編輯器 + <kbd>`</kbd> ]，請注意，您可以在 [設定] 對話方塊中變更此快捷方式) 。  

![FancyZones 設定 UI](../images/pt-fancyzones-settings1.png) 

### <a name="elevated-permission-admin-apps"></a>提升許可權管理應用程式

如果您的應用程式已提高許可權，請以系統管理員模式執行， [並以系統管理員](administrator.md) 身分執行，以取得詳細資訊。

## <a name="choose-your-layout-layout-editor"></a>選擇版面配置 (配置編輯器) 

第一次啟動時，[區域編輯器] 會顯示一份版面配置清單，可由監視器上的 windows 數目來調整。 選擇版面配置會在監視器上顯示該版面配置的預覽。 選取的版面配置會自動套用。  

![FancyZones 選擇器螢幕擷取畫面](../images/pt-fancyzones-picker.png)

如果有多個顯示正在使用中，編輯器將會偵測可用的監視器，並顯示這些監視器供使用者選擇。 選擇的監視將會成為所選版面配置的目標。

![FancyZones 選擇器多個監視器](../images/pt-fancyzones-multimon.png)

### <a name="space-around-zones"></a>區域周圍的空間

[在 **區域周圍顯示空間** ] 切換可讓您判斷框線或邊界的順序將會圍繞每個 FancyZone 視窗。 [ **區域周圍的空間** ] 欄位可讓您設定框線寬度的自訂值。

**醒目提示相鄰區域的距離**，可讓您為 FancyZone 視窗之間的空間量設定自訂值，直到它們合併在一起，或在兩者反白顯示之後，讓它們合併在一起。

開啟區域編輯器之後，請在變更值之後核取並取消核取 [在 **區域周圍顯示空間** ] 方塊，以查看套用的新值。

![FancyZones 區域周圍的空間螢幕擷取畫面](../images/pt-fancyzones-spacearound.png)

### <a name="creating-a-custom-layout"></a>建立自訂版面配置

區域編輯器也支援建立和儲存自訂版面配置。 選取右下角的 [ **+ 建立新的版面** 配置] 按鈕。
  
有兩種方式可建立自訂區域配置： **格線** 版面配置和 **畫布** 版面配置。 這些也可以視為 subtractive 和加法模型。  

Subtractive **方格** 模型會以三個數據行格線開始，並允許透過分割和合併區域來建立區域，視需要調整區域間的裝訂邊大小。

若要合併兩個區域，請選取並按住滑鼠左鍵並拖曳滑鼠，直到選取了第二個區域為止，然後放開按鈕，就會顯示快顯功能表。

![FancyZones 資料表編輯器模式](../images/pt-fancyzones-grideditor.png)

加總 **畫布** 模型的開頭是空白配置，並且支援新增可以拖曳和調整大小的區域，類似于 windows。

![FancyZones 視窗編輯器模式](../images/pt-fancyzones-canvaseditor.png)

## <a name="snapping-a-window-to-two-or-more-zones"></a>將視窗貼到兩個或多個區域

如果兩個區域是連續的，則可以將視窗貼齊至其區域的總和， (四捨五入至同時包含這兩個) 的最小矩形。 當滑鼠游標接近兩個區域的一般邊緣時，就會同時啟用這兩個區域，讓您可以將視窗放入這兩個區域中。  

您也可以貼齊任意數量的區域：先拖曳視窗直到某個區域啟動為止，然後按住 `Control` 鍵，同時拖曳視窗以選取多個區域。

若只要使用鍵盤將某個視窗貼到多個區域，請先開啟這兩個選項 `Override Windows Snap hotkeys (Win+Arrow) to move between zones` 和 `Move windows based on their position` 。 將視窗貼齊至一個區域之後，請使用<kbd>Win</kbd>  +  <kbd>控制項</kbd>  +  <kbd>Alt</kbd> + 箭號，將視窗展開至多個區域。

![兩個區域啟用螢幕擷取畫面](../images/pt-fancyzones-twozones.png)

## <a name="shortcut-keys"></a>快速鍵

| 快速鍵      | 動作 |
| ----------- | ----------- |
| Win ⊞ + \` |Windows 鍵 + 倒引號 (⊞ + \` ) 會啟動編輯器 (此快捷方式可在 [設定] 對話方塊中編輯)  |
| Win ⊞ + 向左/向右箭號 | 只有在開啟設定時，才會在區域之間移動焦點視窗 (`Override Windows Snap hotkeys` ，在此情況下，只 `Windows ⊞ key + Left Arrow` 會覆 `Windows key ⊞ + Right Arrow` 寫和，而 `Win ⊞ + Up Arrow` 並 `Win ⊞ + Down Arrow` 保持正常運作)  |

FancyZones 不會覆寫 Windows 10 `Win ⊞ + Shift + Arrow` ，以快速將視窗移到連續的監視器。

## <a name="settings"></a>設定

| 設定 | 描述 |
| --------- | ------------- |
| 設定區域編輯器熱鍵 | 若要變更預設熱鍵，請按一下文字方塊， (不需要選取或刪除文字) 然後按鍵盤上所需的按鍵組合 |
| 按住 Shift 鍵以在拖曳時啟動區域 | 切換自動貼齊模式與拖曳按鍵，在拖曳和手動貼齊模式期間停用貼齊，以便在拖曳時按 shift 鍵 |
| 使用非主要滑鼠按鍵來切換區域啟用 | 當此選項開啟時，按一下非主要滑鼠按鍵會切換區域啟用 |
| 覆寫 Windows 貼齊快速鍵 (Win ⊞ + 箭號) 在區域間移動 | 當此選項為 on，且 FancyZones 正在執行時，它會覆寫兩個 Windows 嵌入式管理鍵： `Win ⊞ + Left Arrow` 和 `Win ⊞ + Right Arrow` |
| 根據視窗的位置移動視窗 | 允許使用 Win ⊞ + 向上/向下/向右/向左箭號，根據相對於區域配置的位置來對齊視窗 |
| 在所有監視器的區域之間移動視窗 | 當此選項為 off 時，與 Win ⊞ + 箭號對齊會將視窗迴圈到目前監視器上的區域，當開啟時，會將視窗迴圈顯示于所有監視器上的所有區域 |
| 螢幕解析度變更時，將 windows 保留在其區域中 | 螢幕解析度變更之後，如果啟用此設定，FancyZones 將會調整視窗的大小，並將其重新放置到先前的區域中 |
| 在區域配置變更期間，指派至區域的 windows 將符合新的大小/位置 | 當此選項為 on 時，FancyZones 會藉由維護每個視窗的先前區域編號位置，來調整視窗的大小並將其定位至新的區域版面配置 |
| 將新建立的 windows 移至最後一個已知的區域 | 自動將新開啟的視窗移至應用程式所在的最後一個區域位置 |
| 將新建立的視窗移至目前作用中的監視器 [實驗] | 當此選項為開啟狀態，且 [將新建立的 windows 移至最後一個已知的區域] 關閉時，或應用程式沒有最後一個已知的區域時，會將應用程式保留在目前使用中的監視器上 |
| 在 unsnapping 時還原原始的 windows 大小 | 當此選項為 on 時，unsnapping 視窗會將其大小還原為快照之前的大小 |
| 在多監視器環境中啟動編輯器時，請遵循滑鼠游標而非焦點 | 當此選項為 on 時，編輯器快速鍵會在監視器上啟動滑鼠游標所在的編輯器，當這個選項為 off 時，編輯器快速鍵將會啟動監視器上的編輯器，其中目前的使用中視窗是  |
| 拖曳視窗時在所有監視器上顯示區域 | 依預設，FancyZones 只會顯示目前監視器上的可用區域，在開啟時，此功能可能會對效能造成影響。 |
| 允許區域跨越監視器 (所有監視器都必須具有相同的 DPI 縮放比例)  | 此選項可讓您將所有連接的監視器視為一個大型螢幕。 若要正確運作，需要所有監視器都具有相同的 DPI 縮放比例因素 |
| 讓拖曳的視窗變成透明 | 啟用區域時，拖曳的視窗會變成透明，以改善區域可見度 |
| 區域醒目提示色彩 (預設 #008CFF)  | 區域在視窗拖曳期間變成作用中放置目標時的色彩 |
| 區域非使用中色彩 (預設 #F5FCFF)  | 當區域拖曳時，區域變成非作用中的色彩 |
| 區域框線色彩 (預設 #FFFFFF)  | 作用中和非作用中區域的框線色彩 |
| 區域不透明度 (% )  (預設值 50% )  | 作用中和非作用中區域的不透明度百分比 |
| 排除應用程式，使其無法貼齊至區域 | 新增應用程式名稱或部分名稱，每行一個 (例如，加入 `Notepad` 會同時符合 `Notepad.exe` 和 `Notepad++.exe` ，以符合只 `Notepad.exe` 加入 `.exe` 擴充功能)  |

![FancyZones 設定底部螢幕擷取畫面](../images/pt-fancyzones-settings2.png)
