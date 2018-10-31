---
author: mijacobs
Description: Use drag-and-drop animations when users move objects, such as moving an item within a list, or dropping an item on top of another.
title: UWP app 中的拖曳動畫
ms.assetid: 6064755F-6E24-4901-A4FF-263F05F0DFD6
label: Motion--Drag and drop
template: detail.hbs
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: d271d0b9c8e7ce73835457789aca3fa2cb5eda97
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/31/2018
ms.locfileid: "5866268"
---
# <a name="drag-animations"></a>拖曳動畫




在使用者移動物件 (例如移動清單內的項目或將項目放到另一個清單的頂端) 時，使用拖放動畫。

> **重要 API**: [**DragItemThemeAnimation 類別**](https://msdn.microsoft.com/library/windows/apps/br243174)


## <a name="dos-and-donts"></a>可行與禁止事項


**拖曳開始動畫**

-   在使用者開始移動物件時使用拖曳開始動畫。
-   只有在拖放作業會影響到其他物件時，才能在動畫中包含受影響的物件。
-   使用拖曳結束動畫，完成以拖曳開始動畫開始的任何動畫序列。 這樣可回復拖曳開始動畫所導致的拖曳物件大小變更。

**拖曳結束動畫**

-   在使用者放下被拖曳的物件時使用拖曳結束動畫。
-   使用拖曳結束動畫搭配新增和刪除清單的動畫。
-   若要在拖曳結束動畫中包含受影響的物件，拖曳開始動畫中也必須包含這些相同的受影響物件。
-   如果沒有先使用拖曳開始動畫，不要使用拖曳結束動畫。 您必須同時使用這兩個動畫，才能在完成拖曳序列之後將物件回復到原來的大小。

**拖曳到物件之間進入動畫**

-   當使用者將拖曳來源拖曳到兩個其他物件之間可以放下的放下區域時，使用拖曳到物件之間進入動畫。
-   選擇合理的放置目標區域。 這個區域不應太小，否則使用者很難放置要放下的拖曳來源。
-   移動受影響物件以顯示放下區域的建議方向是直接將彼此拉開。 要進行垂直或水平移動則會根據受影響物件彼此之間的方向而定。
-   請勿在無法放置拖曳來源的區域使用拖曳到物件之間進入動畫。 拖曳到物件之間進入動畫會告訴使用者拖曳來源可以在受影響物件之間放下。

**拖曳到物件之間離開動畫**

-   當使用者將物件拖曳移開兩個其他物件之間可能放下的區域時，使用拖曳到物件之間離開動畫。
-   如果沒有先使用拖曳到物件之間進入動畫，則不要使用拖曳到物件之間離開動畫。


## <a name="related-articles"></a>相關文章

**適用於開發人員**
* [動畫概觀](https://msdn.microsoft.com/library/windows/apps/mt187350)
* [讓拖放序列產生動畫效果](https://msdn.microsoft.com/library/windows/apps/xaml/jj649427)
* [快速入門：使用動畫庫讓 UI 產生動畫效果](https://msdn.microsoft.com/library/windows/apps/xaml/hh452703)
* [**DragItemThemeAnimation 類別**](https://msdn.microsoft.com/library/windows/apps/br243174)
* [**DropTargetItemThemeAnimation 類別**](https://msdn.microsoft.com/library/windows/apps/br243186)
* [**DragOverThemeAnimation 類別**](https://msdn.microsoft.com/library/windows/apps/br243180)


 




