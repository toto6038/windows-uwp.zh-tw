---
author: mcleblanc
ms.assetid: 83b4be37-6613-4d00-a48a-0451a24a30fb
title: "資料繫結"
description: "資料繫結可讓您的應用程式 UI 顯示資料，以及選擇性地與該資料保持同步。"
translationtype: Human Translation
ms.sourcegitcommit: 3144758352b99f8c145a3c7be8a6c43d6a002104
ms.openlocfilehash: e26b9753b3169e124c710a3f2e20907c749e5ec3

---

# 資料繫結

\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

資料繫結可讓您的 app UI 顯示資料，以及選擇性地與該資料保持同步。 資料繫結可讓您將資料與 UI 分開考量，為 app 建構更簡單的概念模型，以及更好的可讀性、測試性和維護性。 在標記中，您可以選擇使用 [{x:Bind} 標記延伸](https://msdn.microsoft.com/library/windows/apps/Mt204783)或 [{Binding} 標記延伸](https://msdn.microsoft.com/library/windows/apps/Mt204782)。 您甚至可以在相同的 app 中將兩者混用 (甚至在相同的 UI 元素上)。 {x:Bind} 是 Windows 10 新增的標記，效能更好。

| 主題 | 描述 |
|-------|-------------|
| [資料繫結概觀](data-binding-quickstart.md) | 本主題示範如何在通用 Windows 平台 (UWP) app 中將控制項 (或其他 UI 元素) 繫結到單一項目，或將項目控制項繫結到項目集合。 此外，我們還會說明如何控制項目的呈現、根據選擇來實作詳細資料檢視、以及轉換資料以供顯示。 如需詳細資訊，請參閱[深入了解資料繫結](data-binding-in-depth.md)。 | 
| [深入了解資料繫結](data-binding-in-depth.md) | 這個主題將提供資料繫結功能的詳細說明。 |
| [設計介面上適用於原型設計的範例資料](displaying-data-in-the-designer.md) | 為了在 Visual Studio 設計工具中讓您的控制項能夠填入資料 (讓您能夠在 app 的配置、範本及其他視覺化屬性上運作)，系統提供了各種不同方式，讓您可以使用設計階段的範例資料。 如果您正在建置草圖 (或原型) app，則範例資料也可以是非常實用且省時的。 您可以在執行階段於草圖或原型中使用範例資料來說明您的想法，而不需連線到實際的即時資料。 |
| [繫結階層式資料並建立主要/詳細資料檢視](how-to-bind-to-hierarchical-data-and-create-a-master-details-view.md) | 您可以將項目控制項繫結到已繫結成一個鏈的 [<strong>CollectionViewSource</strong>](https://msdn.microsoft.com/library/windows/apps/BR209833) 執行個體，以建立階層式資料的多層主要/詳細資料 (又稱為清單/詳細資料) 檢視。 |




<!--HONumber=Aug16_HO5-->


