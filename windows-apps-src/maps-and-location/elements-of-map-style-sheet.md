---
description: 瞭解如何使用地圖樣式表單來定義地圖的外觀，例如在 Windows Store 應用程式的隨 mapcontrol 中顯示的地圖。
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: 地圖樣式表參考
ms.date: 03/19/2017
ms.topic: article
keywords: Windows 10, uwp, 地圖, 地圖樣式表
ms.localizationpriority: medium
ms.openlocfilehash: f40f3e64e4fe08d5216a08d125828a96b56cc2af
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89155682"
---
# <a name="map-style-sheet-reference"></a>地圖樣式表參考

Microsoft 地圖技術會使用 [地圖樣式表單](/BingMaps/styling/map-style-sheets) 來定義地圖的外觀，包括顯示在 Windows 市集中應用程式的 [隨 mapcontrol](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol)中。  地圖樣式表單是使用 JavaScript 物件標記法 (JSON) 透過 [MapStyleSheet. ParseFromJson](/uwp/api/windows.ui.xaml.controls.maps.mapstylesheet.parsefromjson#Windows_UI_Xaml_Controls_Maps_MapStyleSheet_ParseFromJson_System_String_) 方法來定義。

樣式表單包含的 [專案](/BingMaps/styling/map-style-sheet-entries) ，其屬性會被覆寫來變更地圖的最終外觀。

例如，下列 JSON 可用來讓水區域以紅色顯示，水線標籤會以綠色顯示，而 land 區域則會以藍色顯示：

```json
    {"version":"1.*",
        "settings":{"landColor":"#0000FF"},
        "elements":{"water":{"fillColor":"#FF0000","labelColor":"#00FF00"}}
    }
```

您可以使用 [ [地圖樣式表單編輯器](https://www.microsoft.com/p/map-style-sheet-editor/9nbhtcjt72ft) ] 應用程式，以互動方式建立樣式表單。