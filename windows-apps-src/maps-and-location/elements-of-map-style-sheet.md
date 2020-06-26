---
description: 地圖樣式表的項目和屬性
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: 地圖樣式表參考
ms.date: 03/19/2017
ms.topic: article
keywords: Windows 10, uwp, 地圖, 地圖樣式表
ms.localizationpriority: medium
ms.openlocfilehash: ec30fd5d63dfa522ef721d2d8a2e4950e6e8e854
ms.sourcegitcommit: c9edb164356c0843d8a9b19ab43707d486e392e1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/25/2020
ms.locfileid: "85377929"
---
# <a name="map-style-sheet-reference"></a>地圖樣式表參考

Microsoft 地圖技術會使用[地圖樣式表單](https://docs.microsoft.com/BingMaps/styling/map-style-sheets)來定義地圖的外觀，包括在 Windows Store 應用程式的[MapControl](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol)中顯示的。  地圖樣式表單是使用 JavaScript 物件標記法（JSON）透過[MapStyleSheet. ParseFromJson](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapstylesheet.parsefromjson#Windows_UI_Xaml_Controls_Maps_MapStyleSheet_ParseFromJson_System_String_)方法來定義。

樣式表單包含[專案](https://docs.microsoft.com/BingMaps/styling/map-style-sheet-entries)，其屬性會遭到覆寫以變更地圖的最後外觀。

例如，下列 JSON 可用來讓水區域以紅色顯示，水標籤會以綠色顯示，而土地區域則以藍色顯示：

```json
    {"version":"1.*",
        "settings":{"landColor":"#0000FF"},
        "elements":{"water":{"fillColor":"#FF0000","labelColor":"#00FF00"}}
    }
```

樣式表單可以使用 [[地圖樣式表單編輯器](https://www.microsoft.com/p/map-style-sheet-editor/9nbhtcjt72ft)] 應用程式以互動方式建立。
