---
author: Jwmsft
Description: "語意式縮放控制項可以讓使用者在相同資料集的兩個不同語意式檢視間縮放。"
title: "語意式縮放"
ms.assetid: B5C21FE7-BA83-4940-9CC1-96F6A2DC28C7
label: Semantic zoom
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: a4e9a90edd2aae9d2fd5d7bead948422d43dad59
ms.openlocfilehash: 47e39290a63408fc66783617ad2f12345eab2fa3

---

# 語意式縮放



語意式縮放控制項可讓使用者在相同內容的兩個不同檢視之間縮放，這樣就能快速瀏覽大型資料集。 放大檢視是內容的主要檢視。 您會在此檢視中顯示完整的資料集。 縮小檢視是相同內容的高階檢視。 您通常會在此檢視中顯示群組的資料集的群組標頭。 例如，檢視通訊錄時，使用者可以放大字母，然後查看與該字母相關的名稱。 

**重要 API**

-   [**SemanticZoom 類別**](https://msdn.microsoft.com/library/windows/apps/hh702601)

**功能**：

-   縮小檢視的大小受限於語意式縮放控制項的範圍。
-   點選群組標題會切換檢視。 可以啟用捏合在檢視之間切換。
-   使用中的標題可在檢視之間切換。

## 範例

相片應用程式中使用的語意式縮放。

![相片應用程式中使用的語意式縮放](images/control-examples/semantic-zoom-photos.png)

使用語意式縮放控制項可以更輕鬆瀏覽資料集，通訊錄為其中一個範例。 一種檢視是完整且按字母排序通訊錄中的人員 (左邊影像)，而在放大的檢視中，資料則是按順序顯示且包含更詳細的資訊 (右邊影像)。

![用於連絡人清單的語意式縮放範例](images/semanticzoom-win10.png)

## 建議

-   在您的應用程式中使用語意式縮放時，請不要隨縮放層級變更項目配置和移動瀏覽方向。 配置和移動瀏覽互動在縮放比例之間應該保持一致且可預測。
-   語意式縮放可以讓使用者快速跳到內容，所以請將縮小檢視模式中頁面/畫面的數量限制為三個。 過多的移動瀏覽會減少語意式縮放的實用性。
-   請避免使用語意式縮放來變更內容的範圍。 例如，相簿不應該切換到 \[檔案總管\] 的資料夾檢視。
-   使用檢視所必要的結構和語意檢視。
-   使用群組集合中項目的群組名稱。
-   針對未分組但已排序的集合使用排序 (例如依時間順序排序日期，或依字母順序排序名稱清單)。



## 相關文章

* [一般使用者互動的指導方針](https://dev.windows.com/design/inputs-devices)


**範例 (XAML)**
* [輸入：XAML 使用者輸入事件範例](http://go.microsoft.com/fwlink/p/?linkid=226855)
* [XAML 捲動、移動瀏覽和縮放範例](http://go.microsoft.com/fwlink/p/?linkid=251717)

**範例 (DirectX)**
* [DirectX 觸控輸入範例](http://go.microsoft.com/fwlink/p/?LinkID=231627)
* [輸入：操作和手勢 (C++) 範例](http://go.microsoft.com/fwlink/p/?linkid=231605)
 

 







<!--HONumber=Jun16_HO4-->


