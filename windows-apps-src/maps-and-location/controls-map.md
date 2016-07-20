---
author: msatranjr
Description: "地圖控制項可顯示道路地圖和空照圖檢視、方向、搜尋結果及交通路況。"
title: "地圖的指導方針"
ms.assetid: 7B5B6BC9-D1EC-4978-8876-20B78EF44797
translationtype: Human Translation
ms.sourcegitcommit: 92285ce32548bd6035c105e35c2b152432f8575a
ms.openlocfilehash: 3c1d37a119eca88ee443772292f8cfe97cb3c1ac

---

# 地圖控制項


\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


地圖控制項可顯示地圖、空照圖、3D 圖、檢視、方向、搜尋結果及交通路況。 您可以在地圖上顯示使用者的位置、方向，以及感興趣的地點。 地圖也可以顯示空照圖 3D 檢視、街景檢視、交通路況、大眾運輸和當地企業。

![地圖基本檢視的範例](./images/win10fa/controls-maps-basic.jpg)

## 這是正確的控制項嗎？


當您需要在 app 內有一份地圖時，使用地圖控制項讓使用者能夠檢視 app 專屬或一般地理資訊。 在 app 中有地圖控制項，表示使用者不需要離開您的 app 便可取得該資訊。

**注意** 如果您不介意使用者離開您的 app，請考慮使用 Windows 地圖 app 來提供該資訊。 您的 app 可以啟動 Windows 地圖 app 來顯示特定的地圖、方向以及搜尋結果。 如需詳細資訊，請參閱[啟動 Windows 地圖 app](https://msdn.microsoft.com/library/windows/apps/mt228341)。

## 範例


這個範例會示範具有街景檢視的地圖：

![地圖控制項街景檢視的範例](./images/win10fa/controls-maps-streetside.jpg)

 

這個範例會顯示具有空照圖 3D 檢視的地圖：

![地圖控制項 3D 檢視的範例](./images/win10fa/controls-maps-3dview.jpg)

 

這個範例會顯示同時具有空照圖 3D 檢視和街景檢視的 app：

![具有街景檢視的 3D 地圖檢視範例](./images/win10fa/controls-maps-3dstreetview.png)


## 建議


-   使用充裕的螢幕空間 (或整個螢幕) 來顯示地圖，讓使用者不需要過度平移和縮放即可檢視地理資訊。

-   如果地圖只用來呈現靜態的參考檢視，使用較小的地圖可能更適合。 如果您使用較小的靜態地圖，請根據可用性設定尺寸—設為較小的尺寸以保留足夠的螢幕實際可用空間，但尺寸也要夠大以保持清晰易讀。

-   使用 [**map elements**](https://msdn.microsoft.com/library/windows/apps/dn637034) 在地圖場景中內嵌感興趣的地點；任何額外的資訊都可以透過與地圖場景重疊的暫時性 UI 方式顯示。

## 相關主題


* [顯示地圖的 2D、3D 和 Streetside 檢視](https://msdn.microsoft.com/library/windows/apps/mt219695)
* [在地圖上顯示興趣點 (POI)](https://msdn.microsoft.com/library/windows/apps/mt219696)
* [Bing 地圖服務開發人員中心](https://www.bingmapsportal.com/)
* [UWP 地圖範例](http://go.microsoft.com/fwlink/p/?LinkId=619977)
* [//Build 2015 影片：跨手機、平板電腦和電腦運用 Windows App 中的地圖與位置功能](https://channel9.msdn.com/Events/Build/2015/2-757)
* [啟動 Windows 地圖 app](https://msdn.microsoft.com/library/windows/apps/mt228341)
 

 







<!--HONumber=Jun16_HO5-->


