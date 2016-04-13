---
Description: 本文列出 Microsoft 使用者介面自動化控制項模式、用戶端用來存取控制項模式的類別，以及介面提供者用來實作控制項模式的類別。
title: 控制項模式和介面
ms.assetid: 2091883C-5D0C-44ED-936A-709022926A42
label: Control patterns and interfaces
template: detail.hbs
---

控制項模式和介面
===================================================================================================

\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

本文列出 Microsoft 使用者介面自動化控制項模式、用戶端用來存取控制項模式的類別，以及介面提供者用來實作控制項模式的類別。

這個主題中的表格說明 Microsoft UI 自動化控制項模式。 表格也列出使用者介面自動化用戶端用來存取控制項模式的類別，以及使用者介面自動化提供者用來實作控制項模式的介面。 **控制項模式**欄顯示從使用者介面自動化用戶端觀點的模式名稱，如同 [**Control Pattern Availability Property Identifiers**](https://msdn.microsoft.com/library/windows/desktop/Ee671199) 中所列的常數值。 從使用者介面自動化提供者觀點來看，這其中每一個模式都是一個 [**PatternInterface**](https://msdn.microsoft.com/library/windows/apps/BR242496) 常數名稱。 **類別提供者介面**欄顯示介面的名稱，提供者會實作該介面來為自訂的 XAML 控制項提供此模式。

如需如何實作公開控制項模式的自訂自動化對等和實作介面的詳細資訊，請參閱[自訂自動化對等](custom-automation-peers.md)。

當您實作控制項模式時，也應該查閱使用者介面自動化提供者文件，這類文件會說明用戶端對於擁有控制項模式的部分期望，而不論是使用哪一個 UI 架構來實作。 一般使用者介面自動化提供者文件中列出的部分資訊，將會影響您實作對等項目和正確支援該模式的方式。 請參閱[實作使用者介面自動化控制項模式](https://msdn.microsoft.com/library/windows/desktop/Ee671292)，並檢視您想要實作之模式的頁面。

| 控制項模式       | 類別 提供者介面                                                         | 說明                                                                                                                                                                                                                                                      |
|-----------------------|----------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Annotation**        | [**IAnnotationProvider**](https://msdn.microsoft.com/library/windows/apps/Hh738493)               | 用於公開文件中的註解屬性。                                                                                                                                                                                                    |
| **Dock**              | [**IDockProvider**](https://msdn.microsoft.com/library/windows/apps/BR242565)                           | 用於可停駐在停駐容器中的控制項。 例如，工具列或工具板。                                                                                                                                                             |
| **Drag**              | [**IDragProvider**](https://msdn.microsoft.com/library/windows/apps/Hh750322)                           | 用於支援可拖曳的控制項，或者含有可拖曳項目的控制項。                                                                                                                                                                                            |
| **DropTarget**        | [**IDropTargetProvider**](https://msdn.microsoft.com/library/windows/apps/Hh750327)               | 用於支援可成為拖放作業目標的控制項。                                                                                                                                                                                    |
| **ExpandCollapse**    | [**IExpandCollapseProvider**](https://msdn.microsoft.com/library/windows/apps/BR242568)       | 用於支援視覺上可展開以顯示更多內容，或可摺疊以隱藏內容的控制項。                                                                                                                                                              |
| **Grid**              | [**IGridProvider**](https://msdn.microsoft.com/library/windows/apps/BR242578)                           | 用於支援格線功能 (例如調整大小及移動至指定的儲存格) 的控制項。 請注意，「格線」本身不會實作此模式，因為它提供配置而非控制項。                                                          |
| **GridItem**          | [**IGridItemProvider**](https://msdn.microsoft.com/library/windows/apps/BR242572)                   | 用於格線中有儲存格的控制項。                                                                                                                                                                                                                  |
| **Invoke**            | [**IInvokeProvider**](https://msdn.microsoft.com/library/windows/apps/BR242582)                       | 用於可叫用的控制項，例如 [**Button**](https://msdn.microsoft.com/library/windows/apps/BR209265)。                                                                                                                                                                            |
| **ItemContainer**     | [**IItemContainerProvider**](https://msdn.microsoft.com/library/windows/apps/BR242583)         | 讓應用程式尋找容器中的元素，例如虛擬的清單。                                                                                                                                                                              |
| **MultipleView**      | [**IMultipleViewProvider**](https://msdn.microsoft.com/library/windows/apps/BR242585)           | 用於可以在相同資訊、資料或子系集合中，切換多種表示方式的控制項。                                                                                                                                            |
| **ObjectModel**       | [**IObjectModelProvider**](https://msdn.microsoft.com/library/windows/apps/Dn251815)             | 用於公開對於文件基本物件模型的指標。                                                                                                                                                                                           |
| **RangeValue**        | [**IRangeValueProvider**](https://msdn.microsoft.com/library/windows/apps/BR242590)               | 用於範圍值可以套用至控制項的控制項。 例如，包含年份的微調控制項範圍可能是 1900 到目前年份，而另一個代表月份的微調控制項的範圍值則是 1 至 12。 |
| **Scroll**            | [**IScrollProvider**](https://msdn.microsoft.com/library/windows/apps/BR242601)                       | 用於可捲動的控制項。 例如具有捲軸的控制項，在資訊超出控制項可檢視區域所能顯示的範圍時，就會啟用捲軸。                                                                         |
| **ScrollItem**        | [**IScrollItemProvider**](https://msdn.microsoft.com/library/windows/apps/BR242599)               | 用於可捲動清單中有個別項目的控制項。 例如，在捲動清單中 (例如，下拉式方塊控制項) 有個別項目的清單控制項。                                                                                      |
| **Selection**         | [**ISelectionProvider**](https://msdn.microsoft.com/library/windows/apps/BR242616)                 | 用於選取範圍容器控制項。 例如，[**ListBox**](https://msdn.microsoft.com/library/windows/apps/BR242868) 和 [**ComboBox**](https://msdn.microsoft.com/library/windows/apps/BR209348)。                                                                                                                           |
| **SelectionItem**     | [**ISelectionItemProvider**](https://msdn.microsoft.com/library/windows/apps/BR242610)         | 用於選取範圍容器控制項 (例如清單方塊和下拉式方塊) 中的個別項目。                                                                                                                                                                   |
| **Spreadsheet**       | [**ISpreadsheetProvider**](https://msdn.microsoft.com/library/windows/apps/Dn251821)             | 用於公開試算表或其他以格線為基礎的文件的內容。                                                                                                                                                                                       |
| **SpreadsheetItem**   | [**ISpreadsheetItemProvider**](https://msdn.microsoft.com/library/windows/apps/Dn251817)     | 用於公開試算表或其他以格線為基礎的文件中儲存格的屬性。                                                                                                                                                                           |
| **Styles**            | [**IStylesProvider**](https://msdn.microsoft.com/library/windows/apps/Dn251823)                       | 用於說明含有特定樣式、填滿色彩、填滿圖樣或形狀的 UI 元素。                                                                                                                                                                     |
| **SynchronizedInput** | [**ISynchronizedInputProvider**](https://msdn.microsoft.com/library/windows/apps/Dn279198) | 讓使用者介面自動化用戶端應用程式能夠將滑鼠或鍵盤輸入導向特定的 UI 元素。                                                                                                                                                                |
| **Table**             | [**ITableProvider**](https://msdn.microsoft.com/library/windows/apps/BR242623)                         | 用於有格線以及標題資訊的控制項。 例如，表格式行事曆控制項。                                                                                                                                                       |
| **TableItem**         | [**ITableItemProvider**](https://msdn.microsoft.com/library/windows/apps/BR242620)                 | 用於表格中的項目。                                                                                                                                                                                                                                       |
| **文字**              | [**ITextProvider**](https://msdn.microsoft.com/library/windows/apps/BR242627)                           | 用於公開文字資訊的編輯控制項及文件。 另請參閱 [**ITextRangeProvider**](https://msdn.microsoft.com/library/windows/apps/BR242634) 和 [**ITextProvider2**](https://msdn.microsoft.com/library/windows/apps/BR2426272)。                                                    |
| **TextChild**         | [**ITextChildProvider**](https://msdn.microsoft.com/library/windows/apps/Hh701839)                 | 用於存取最接近元素且支援 **Text** 控制項模式的上階。                                                                                                                                                                         |
| **TextEdit**          | 沒有可用的 Managed 類別                                                       | 提供可修改文字的控制項存取權，例如，執行自動校正或透過輸入法 (IME) 啟用輸入組合的控制項。                                                                                          |
| **TextRange**         | [**ITextRangeProvider**](https://msdn.microsoft.com/library/windows/apps/BR242634)                 | 提供實作 [**ITextProvider**](https://msdn.microsoft.com/library/windows/apps/BR242627) 的文字容器中橫跨多行之連續文字的存取權。 另請參閱 [**ITextRangeProvider2**](https://msdn.microsoft.com/library/windows/apps/BR2426342)。                                            |
| **切換**            | [**IToggleProvider**](https://msdn.microsoft.com/library/windows/apps/BR242653)                       | 用於可切換狀態的控制項。 例如，[**CheckBox**](https://msdn.microsoft.com/library/windows/apps/BR209316) 及可核取的功能表項目。                                                                                                                       |
| **Transform**         | [**ITransformProvider**](https://msdn.microsoft.com/library/windows/apps/BR242656)                 | 用於可調整大小、移動和旋轉的控制項。 Transform 控制項模式通常用於設計工具、表單、圖形編輯器以及繪圖應用程式。                                                                                  |
| **值**             | [**IValueProvider**](https://msdn.microsoft.com/library/windows/apps/BR242663)                         | 允許用戶端對不支援範圍值的控制項，取得或設定一個值。                                                                                                                                                                          |
| **VirtualizedItem**   | [**IVirtualizedItemProvider**](https://msdn.microsoft.com/library/windows/apps/BR242668)     | 公開容器內被虛擬化且必須當做使用者介面自動化元素被完全存取的項目。                                                                                                                                             |
| **Window**            | [**IWindowProvider**](https://msdn.microsoft.com/library/windows/apps/BR242670)                       | 公開視窗特定的資訊 (Microsoft Windows 作業系統的基本概念)。 例如子視窗和對話方塊，就是本身為視窗的控制項。                                                                                   |

 

**注意** 您將不需要在現有的 XAML 控制項中尋找所有這類模式的實作。 部分模式單獨擁有介面，利用模式的一般使用者介面自動化架構定義來支援同位，以及支援自動化對等案例，這類案例需要完全自訂的實作以支援該模式。

 

**注意** Windows Phone 市集應用程式不支援此處列出的所有使用者介面自動化控制項模式。 **Annotation**、**Dock**、**Drag**、**DropTarget**、**ObjectModel** 為部分不支援的模式。

<span id="related_topics"> </span>相關主題
-----------------------------------------------

* [自訂自動化對等](custom-automation-peers.md)
* [協助工具](accessibility.md)
 

 





<!--HONumber=Mar16_HO3-->


