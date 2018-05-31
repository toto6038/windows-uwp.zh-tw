---
description: 本文說明如何在您的通用 Windows 平台 (UWP) app 中新增拖放功能。
title: 拖放
ms.assetid: A15ED2F5-1649-4601-A761-0F6C707A8B7E
author: msatranjr
ms.author: misatran
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 7854c0bb541c04ec3cc109ad32ce30ba1c8bd52e
ms.sourcegitcommit: 9f059b37e45099b4314c96a0b604449e966d3c3c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/04/2018
ms.locfileid: "1704645"
---
# <a name="drag-and-drop"></a>拖放



本文說明如何在您的通用 Windows 平台 (UWP) app 中新增拖放功能。 拖放是一種與影像和檔案之類的內容進行互動的傳統、原始方式。 實作之後，拖放不論以哪一個方向都能順暢運作，包括 App 間、App 到傳統型應用程式，以及傳統型應用程式到 App。

## <a name="set-valid-areas"></a>設定有效區域

使用 [**AllowDrop**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.AllowDrop) 和 [**CanDrag**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.CanDrag) 屬性，指定 app 的有效拖放區域。

下列標記示範如何使用 XAML 中的 [**AllowDrop**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.AllowDrop) 將 app 的特定區域設為有效的放下區域。 如果使用者嘗試在其他地方放下，系統將不會允許這麼做。 如果您希望使用者可以在 app 中的任何位置放下項目，請將整個背景設定為放下目標。

[!code-xml[Main](./code/drag_drop/cs/MainPage.xaml#SnippetDropArea)]

針對拖曳，您通常會希望可以拖曳的項目非常明確。 使用者會希望能拖曳特定項目 (例如圖片)，而非您 app 中的所有項目。 以下是如何使用 XAML 設定 [**CanDrag**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.CanDrag) 的方法。

[!code-xml[Main](./code/drag_drop/cs/MainPage.xaml#SnippetDragArea)]

您不需要進行任何其他工作即可允許拖曳，除非您希望自訂 UI (本文章稍後將會說明)。 放下則需要較多的步驟。

## <a name="handle-the-dragover-event"></a>處理 DragOver 事件

當使用者拖曳項目到您的 App 上，但尚未放下時，會引發 [**DragOver**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.DragOver) 事件。 在這個處理常式中，您需要使用 [**AcceptedOperation**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.DragEventArgs.AcceptedOperation) 屬性指定 app 支援何種類型的作業。 最常見的是複製。

[!code-cs[Main](./code/drag_drop/cs/MainPage.xaml.cs#SnippetGrid_DragOver)]

## <a name="process-the-drop-event"></a>處理 Drop 事件

當使用者在有效的放下區域釋放項目時會發生 [**Drop**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.Drop) 事件。 請使用 [**DataView**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.DragEventArgs.DataView) 屬性處理它們。

為了簡單起見，下面的範例假設使用者置放單一相片並直接存取。 事實上，使用者可以同時置放多個不同格式的項目。 您的應用程式應該檢查置放了什麼類型的檔案，以及其中有多少檔案，以便處理這種可能情況，然後相應處理每個項目。 您也應該考慮通知使用者是否要嘗試執行您的應用程式不支援的動作。

[!code-cs[Main](./code/drag_drop/cs/MainPage.xaml.cs#SnippetGrid_Drop)]

## <a name="customize-the-ui"></a>自訂 UI

系統會提供用於拖放的預設 UI。 不過，您也可以選擇透過設定自訂標題與字符來自訂 UI 的各個部分，或選擇完全不顯示 UI。 若要自訂 UI，請使用 [**DragEventArgs.DragUIOverride**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.DragEventArgs.DragUIOverride) 屬性。

[!code-cs[Main](./code/drag_drop/cs/MainPage.xaml.cs#SnippetGrid_DragOverCustom)]

## <a name="open-a-context-menu-on-an-item-you-can-drag-with-touch"></a>在您可以使用觸控方式拖曳的項目上開啟操作功能表

使用觸控時，拖曳 [**UIElement**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement) 和開啟其操作功能表會共用類似的觸控手勢；每個手勢都是以長按做為開始。 以下說明系統如何釐清您 app 中同時支援以下兩個項目之元素的兩個動作： 

* 如果使用者長按某個項目並開始在 500 毫秒內拖曳它，就會拖曳該項目且不會顯示操作功能表。 
* 如果使用者長按該項目但沒有在 500 毫秒內拖曳它，就會開啟操作功能表。 
* 在操作功能表開啟之後，如果使用者嘗試拖曳項目 (手指沒有舉起離開螢幕)，就會關閉操作功能表並開始拖曳。

## <a name="designate-an-item-in-a-listview-or-gridview-as-a-folder"></a>將 ListView 或 GridView 中的項目指定為資料夾

您可以指定 [**ListViewItem**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.ListViewItem) 或 [**GridViewItem**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.GridViewItem) 做為資料夾。 對於樹狀檢視和檔案總管情況而言，這特別有用。 若要這樣做，請將該項目上的 [**AllowDrop**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.AllowDrop) 屬性明確設為 **True**。 

系統將會針對拖曳到資料夾中和非資料夾項目，自動顯示適當的動畫。 您的 app 程式碼必須在資料夾 (以及非資料夾項目) 上繼續處理 [**Drop**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.Drop) 事件，以更新料來源並將放下的項目新增到目標資料夾。

## <a name="see-also"></a>另請參閱

* [App 間通訊](index.md)
* [AllowDrop](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.allowdrop.aspx)
* [CanDrag](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.candrag.aspx)
* [DragOver](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.dragover.aspx)
* [AcceptedOperation](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.drageventargs.acceptedoperation.aspx)
* [DataView](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.drageventargs.dataview.aspx)
* [DragUIOverride](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.drageventargs.draguioverride.aspx)
* [Drop](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.drop.aspx)
* [IsDragSource](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.isdragsource.aspx)
