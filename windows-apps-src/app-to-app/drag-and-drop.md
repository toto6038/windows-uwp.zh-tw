---
description: "本文說明如何在您的通用 Windows 平台 (UWP) app 中新增拖放功能。"
title: "拖放"
ms.assetid: A15ED2F5-1649-4601-A761-0F6C707A8B7E
author: awkoren
ms.sourcegitcommit: 03f3f86ed1310e6e3ac5f53cc5e81ebef708a1a2
ms.openlocfilehash: ffa2f0f368a61ef4f3003c1fa03e143b26c6859b

---
# 拖放

\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


本文說明如何在您的通用 Windows 平台 (UWP) app 中新增拖放功能。 拖放是一種與影像和檔案之類的內容進行互動的傳統、原始方式。 實作之後，拖放不論以哪一個方向都能順暢運作，包括 App 間、App 到傳統型應用程式，以及傳統型應用程式到 App。

## 設定有效區域

使用 [**AllowDrop**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.AllowDrop) 和 [**CanDrag**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.CanDrag) 屬性，指定 app 的有效拖放區域。

下列標記示範如何使用 XAML 中的 [**AllowDrop**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.AllowDrop) 將 app 的特定區域設為有效的放下區域。 如果使用者嘗試在其他地方放下，系統將不會允許這麼做。 如果您希望使用者可以在 app 中的任何位置放下項目，請將整個背景設定為放下目標。

[!code-xml[主要](./code/drag_drop/cs/MainPage.xaml#SnippetDropArea)]

針對拖曳，您通常會希望可以拖曳的項目非常明確。 使用者會希望能拖曳特定項目 (例如圖片)，而非您 app 中的所有項目。 以下是如何使用 XAML 設定 [**CanDrag**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.CanDrag) 的方法。

[!code-xml[主要](./code/drag_drop/cs/MainPage.xaml#SnippetDragArea)]

您不需要進行任何其他工作即可允許拖曳，除非您希望自訂 UI (本文章稍後將會說明)。 放下則需要較多的步驟。

## 處理 DragOver 事件

當使用者拖曳項目到您的 App 上，但尚未放下時，會引發 [**DragOver**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.DragOver) 事件。 在這個處理常式中，您需要使用 [**AcceptedOperation**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.DragEventArgs.AcceptedOperation) 屬性指定 app 支援何種類型的作業。 最常見的是複製。

[!code-cs[主要](./code/drag_drop/cs/MainPage.xaml.cs#SnippetGrid_DragOver)]

## 處理 Drop 事件

當使用者在有效的放下區域釋放項目時會發生 [**Drop**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.Drop) 事件。 請使用 [**DataView**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.DragEventArgs.DataView) 屬性處理它們。

下面範例為了簡單起見，我們假設使用者放下一張照片並存取。 事實上，使用者可以同時放下不同格式的多個項目。 您的 app 應該要採用以下方式處理這種可能性：檢查放下的檔案類型並據以處理它們，並在使用者嘗試執行一些 app 不支援的動作時通知使用者。

[!code-cs[主要](./code/drag_drop/cs/MainPage.xaml.cs#SnippetGrid_Drop)]

## 自訂 UI

系統會提供用於拖放的預設 UI。 不過，您也可以選擇透過設定自訂標題與字符來自訂 UI 的各個部分，或選擇完全不顯示 UI。 若要自訂 UI，請使用 [**DragEventArgs.DragUIOverride**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.DragEventArgs.DragUIOverride) 屬性。

[!code-cs[Main](./code/drag_drop/cs/MainPage.xaml.cs#SnippetGrid_DragOverCustom)]

## 另請參閱

* [AllowDrop](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.allowdrop.aspx)
* [CanDrag](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.candrag.aspx)
* [DragOver](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.dragover.aspx)
* [AcceptedOperation](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.drageventargs.acceptedoperation.aspx)
* [DataView](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.drageventargs.dataview.aspx)
* [DragUiOverride](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.drageventargs.draguioverride.aspx)
* [Drop](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.drop.aspx)


<!--HONumber=Jun16_HO4-->


