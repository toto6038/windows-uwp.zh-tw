---
title: 檢視轉換
description: 檢視轉換將檢視器放置在世界空間中，並將頂點轉換成相機空間。
ms.assetid: DA4C2051-4C28-4ABF-8C06-241C8CB87F2F
keywords:
- 檢視轉換
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 63660883f327547a82eac4a3accec475995a651a
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/05/2018
ms.locfileid: "8734924"
---
# <a name="view-transform"></a>檢視轉換


*檢視轉換*將檢視器放置在世界空間中，並將頂點轉換成相機空間。 在相機空間，相機或檢視器位於原點，朝著正 z 方向看。 檢視矩陣圍繞相機位置 (相機空間的起點) 將物件重新放置在世界空間中。 Direct3D 使用左手座標系統，因此場景中 z 是正數。

有許多的方式可建立檢視矩陣。 在所有情況下，相機在世界空間有特定邏輯位置和方向，做為起點用來建立檢視矩陣，套用到場景中的模型。 檢視矩陣平移並旋轉物件，將它們置於相機空間中 (相機位於原點)。 建立檢視矩陣的一個方式是為每個軸結合旋轉矩陣和轉移矩陣。 下列一般矩陣方程式適用於這種方式。

![檢視轉換的方程式](images/viewtran.png)

在這個公式中，V 是所建立的檢視矩陣，T 是將物件重新置放在世界空間中的轉移矩陣，而 Rₓ 到 R<sub>z</sub> 則是沿著 x、y 與 z 軸旋轉物件的旋轉矩陣。 轉移矩陣與旋轉矩陣以世界空間中的相機邏輯位置和方向為基礎。 因此，如果相機在世界空間中的邏輯位置是 &lt;10,20,100&gt;，轉移矩陣的目的是沿著 x、y 與 z 軸將物件分別移動 -10 單位、-20 單位及 -100 單位。 在公式中旋轉矩陣以相機方向為基礎，亦即相機空間的軸旋轉多少，不對齊世界空間。 例如，如果前面所提到的相機往下指，其 z 軸是 90 度（pi/2 弧度），不對齊世界空間的 z 軸，如下圖所示。

![相機檢視空間相對於世界空間的圖例](images/camtop.png)

旋轉矩陣對場景中模型套用相等但相反的旋轉大小。 這個相機的檢視矩陣包含圍繞 x 軸 -90 度的旋轉。 旋轉矩陣與轉移矩陣結合，建立調整場景中物件之位置和方向的檢視矩陣，使其頂端面向相機，提供相機在模型上方的表面現象。

## <a name="span-idsettingupaviewmatrixspanspan-idsettingupaviewmatrixspanspan-idsettingupaviewmatrixspansetting-up-a-view-matrix"></a><span id="Setting_Up_a_View_Matrix"></span><span id="setting_up_a_view_matrix"></span><span id="SETTING_UP_A_VIEW_MATRIX"></span>設定檢視矩陣


Direct3D 使用世界和檢視矩陣來設定數個內部資料結構。 每當您設定世界或檢視矩陣，系統會重新計算相關內部結構。 頻繁設定這些矩陣通常在計算上耗費時間。 您可以串連世界和檢視矩陣成為世界檢視矩陣 (您將其設為世界矩陣)，然後將檢視矩陣設為單位矩陣，將所需的計算次數最小化。 保留個別世界和檢視矩陣的快取複本，您可以視需要修改、串連並重設世界矩陣。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相關主題


[轉換](transforms.md)

 

 




