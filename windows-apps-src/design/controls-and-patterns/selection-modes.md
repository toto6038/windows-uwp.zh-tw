---
Description: 選取模式可讓使用者選取一或多個項目，並且採取動作。
title: 選取模式
label: Selection mode
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: anawish
design-contact: kimsea
dev-contact: ranjeshj
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: d7b781e6074468fbe73446e4057e36ff31266d05
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/29/2020
ms.locfileid: "72165095"
---
# <a name="selection-mode-overview"></a>選取模式概觀

選取模式可讓使用者選取一或多個項目，並且採取動作。 它可以透過操作功能表 (在項目上以 CTRL + 按一下或 SHIFT + 按一下) 叫用，或在圖庫檢視中將游標移至項目上的目標。 啟動選取模式時，每個清單項目旁邊都會顯示核取方塊，且在螢幕頂端或底部可能會顯示動作。

## <a name="different-selection-modes"></a>不同的選取模式
有三種選取模式：

- 單一：使用者一次只能選取一個項目。
- 多重：使用者不需要使用輔助按鍵就能選取多個項目。
- 延伸：使用者可以使用輔助按鍵選取多個項目，例如按住 SHIFT 鍵。

## <a name="selection-mode-examples"></a>選取模式範例
### <a name="here-is-a-listview-with-single-selection-mode-enabled"></a>以下是已啟用單一選取模式的 ListView。
![具有單一選取模式的清單檢視](images/listview-selection-single.png)

已選取 Art Venere 項目，且目前正停留在 Mitsue Tollner 項目上。

### <a name="here-is-a-listview-with-multiple-selection-mode-enabled"></a>以下是已啟用多重選取模式的 ListView：
![具有多重選取模式的清單檢視](images/listview-selection-multiple.png)

已選取三個項目，且目前正停留在 Donette Foller 項目上。

### <a name="here-is-a-gridview-with-single-selection-mode-enabled"></a>以下是已啟用單一選取模式的 GridView：
![具有單一選取模式的方格檢視](images/gridview-selection-single.png)

已選取項目 7，且目前正停留在項目 3 上。

### <a name="here-is-a-gridview-with-multiple-selection-mode-enabled"></a>以下是已啟用多重選取模式的 GridView：
![具有多重選取模式的方格檢視](images/gridview-selection-multiple.png)

已選取項目 2、4 和 5，且目前正停留在項目 7 上。

## <a name="behavior-and-interaction"></a>行為和互動
在項目上點選任何位置即可選取項目。 點選命令列巨集指令會影響所有選取項目。 如果未選取任何項目，命令列動作應該為非使用中狀態 (除了 [全選] 以外)。

選取模式並沒有消失關閉模型；點選選取模式在使用中之框架的外側並不會取消模式。 這可以避免意外停用模式。 按一下 [上一頁] 按鈕關閉多重選取模式。

在選取巨集指令時顯示視覺化確認。 請考慮針對某些動作顯示確認對話方塊，特別是破壞性動作 (例如刪除)。

選取模式只會影響已啟用該模式的頁面，所以不會影響該頁面以外的任何項目。

選取模式的進入點應該與受其影響的內容並列。

如需命令列的建議，請參閱[命令列的指導方針](app-bars.md)。

若要深入了解特定控制項的選取模式和處理選取事件，請造訪該控制項的指導方針頁面。
