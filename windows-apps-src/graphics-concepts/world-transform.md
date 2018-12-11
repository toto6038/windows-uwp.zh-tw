---
title: 世界矩陣轉換
description: 世界矩陣轉換將座標從模型空間（其中頂點是相對於模型的區域原點定義的）變更為世界空間。
ms.assetid: 767B032C-69D0-4583-8FEB-247F4C41E31D
keywords:
- 世界矩陣轉換
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: af694ed32d845e518f4189f75309f1f371743f90
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/11/2018
ms.locfileid: "8920343"
---
# <a name="world-transform"></a>世界矩陣轉換


世界矩陣轉換將座標從模型空間（其中頂點是相對於模型的區域原點定義的）變更為世界空間。 在世界空間，頂點是相對於場景中所有物件之通用原點定義的。 世界矩陣轉換將模型放置到世界。

## <span id="What_Is_a_World_Transform"></span><span id="what_is_a_world_transform"></span><span id="WHAT_IS_A_WORLD_TRANSFORM"></span>


下圖顯示世界座標系統和模型區域座標系統之間的關係。

![世界座標及區域座標如何相關的簡圖](images/worldcrd.png)

世界矩陣轉換可以包含轉移、旋轉和縮放的任何組合。

## <a name="span-idsettingupaworldmatrixxmlspansetting-up-a-world-matrix"></a><span id="SETTING_UP_A_WORLD_MATRIX.XML"></span>設定世界矩陣


如同任何其他轉換，藉由將一系列矩陣串連成一個包含效果總和的矩陣，建立世界矩陣轉換。 在最簡單的情形，當模型在世界原點，而且其區域座標軸的方向與世界空間相同時，世界矩陣是單位矩陣。 世界矩陣更常是轉移至世界空間以及可能一或多個旋轉 (視需要旋轉模型) 的組合。

Direct3D 使用世界和檢視矩陣，讓您設定數個內部資料結構。 每當您設定世界或檢視矩陣，系統會重新計算相關內部結構。 頻繁設定這些矩陣 (例如，每個畫面數千次) 通常在計算上耗費時間。 您可以串連世界和檢視矩陣成為世界檢視矩陣 (您將其設為世界矩陣)，然後將檢視矩陣設為單位矩陣，將所需的計算次數最小化。 保留個別世界和檢視矩陣的快取複本，讓您可以視需要修改、串連並重設世界矩陣。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相關主題


[轉換](transforms.md)

 

 




