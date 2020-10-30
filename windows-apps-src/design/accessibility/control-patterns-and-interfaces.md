---
description: 本文列出 Microsoft 使用者介面自動化控制項模式、用戶端用來存取控制項模式的類別，以及介面提供者用來實作控制項模式的類別。
ms.assetid: 2091883C-5D0C-44ED-936A-709022926A42
title: 控制項模式和介面
label: Control patterns and interfaces
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 92f2337848f3689aa8f2f6f73dd92dbbe1313257
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2020
ms.locfileid: "93032531"
---
# <a name="control-patterns-and-interfaces"></a>控制項模式和介面  



本文列出 Microsoft 使用者介面自動化控制項模式、用戶端用來存取控制項模式的類別，以及介面提供者用來實作控制項模式的類別。

這個主題中的表格說明 Microsoft UI 自動化控制項模式。 表格也列出使用者介面自動化用戶端用來存取控制項模式的類別，以及使用者介面自動化提供者用來實作控制項模式的介面。 **控制項模式** 欄顯示從使用者介面自動化用戶端觀點的模式名稱，如同 [**Control Pattern Availability Property Identifiers**](/windows/desktop/WinAuto/uiauto-control-pattern-availability-propids) 中所列的常數值。 從使用者介面自動化提供者觀點來看，這其中每一個模式都是一個 [**PatternInterface**](/uwp/api/Windows.UI.Xaml.Automation.Peers.PatternInterface) 常數名稱。 **類別提供者介面** 欄顯示介面的名稱，提供者會實作該介面來為自訂的 XAML 控制項提供此模式。

如需如何實作公開控制項模式的自訂自動化對等和實作介面的詳細資訊，請參閱[自訂自動化對等](custom-automation-peers.md)。

當您實作控制項模式時，也應該查閱使用者介面自動化提供者文件，這類文件會說明用戶端對於擁有控制項模式的部分期望，而不論是使用哪一個 UI 架構來實作。 一般使用者介面自動化提供者文件中列出的部分資訊，將會影響您實作對等項目和正確支援該模式的方式。 請參閱[實作使用者介面自動化控制項模式](/windows/desktop/WinAuto/uiauto-implementinguiautocontrolpatterns)，並檢視說明您想要實作之模式的頁面。

| 控制項模式 | 類別 提供者介面 | 描述 |
|-----------------|--------------------------|-------------|
| **註釋** | [**IAnnotationProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IAnnotationProvider) | 用於公開文件中的註解屬性。 |
| **Dock** | [**IDockProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IDockProvider) | 用於可停駐在停駐容器中的控制項。 例如工具列或工具板。 |
| **拖曳** | [**IDragProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IDragProvider) | 用於支援可拖曳的控制項，或者含有可拖曳項目的控制項。 |
| **DropTarget** | [**IDropTargetProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IDropTargetProvider) | 用於支援可成為拖放作業目標的控制項。 |
| **ExpandCollapse** | [**IExpandCollapseProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IExpandCollapseProvider) | 用於支援視覺上可展開以顯示更多內容，或可摺疊以隱藏內容的控制項。 |
| **方格** | [**IGridProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IGridProvider) | 用於支援格線功能 (例如調整大小及移動至指定的儲存格) 的控制項。 請注意，「格線」本身不會實作此模式，因為它提供配置而非控制項。 |
| **GridItem** | [**IGridItemProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IGridItemProvider) | 用於格線中有儲存格的控制項。 |
| **叫用** | [**IInvokeProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IInvokeProvider) | 用於可叫用的控制項，例如  [**按鈕**](/uwp/api/Windows.UI.Xaml.Controls.Button)。 |
| **ItemContainer** | [**IItemContainerProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IItemContainerProvider) | 讓應用程式尋找容器中的元素，例如虛擬的清單。 |
| **MultipleView** | [**IMultipleViewProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IMultipleViewProvider) | 用於可以在相同資訊、資料或子系的集合中，切換多種表示方式的控制項。 |
| **ObjectModel** | [**IObjectModelProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IObjectModelProvider) | 用於公開對於文件基本物件模型的指標。 |
| **RangeValue** | [**IRangeValueProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IRangeValueProvider) | 用於範圍值可以套用至控制項的控制項。 例如，包含年份的微調控制項範圍可能是 1900 到目前年份，而另一個代表月份的微調控制項的範圍值則是 1 至 12。 |
| **捲動** | [**IScrollProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IScrollProvider) | 用於可捲動的控制項。 例如，具有捲軸，且當包含的資訊超出控制項可檢視區中可顯示的範圍時，捲軸即成為作用中的控制項。 |
| **ScrollItem** | [**IScrollItemProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IScrollItemProvider) | 用於在可捲動的清單中具有個別項目的控制項。 例如，在捲動清單中 (例如下拉式方塊控制項) 有個別項目的清單控制項。 |
| **選取範圍** | [**ISelectionProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.ISelectionProvider) | 用於選取容器控制項。 例如， [**ListBox**](/uwp/api/Windows.UI.Xaml.Controls.ListBox) 和 [**ComboBox**](/uwp/api/Windows.UI.Xaml.Controls.ComboBox)。 |
| **SelectionItem** | [**ISelectionItemProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.ISelectionItemProvider) | 用於選取容器控制項 (例如清單方塊及下拉式方塊) 中的個別項目。 |
| **試算表** | [**ISpreadsheetProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.ISpreadsheetProvider) | 用於公開試算表或其他以格線為基礎的文件的內容。 |
| **SpreadsheetItem** | [**ISpreadsheetItemProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.ISpreadsheetItemProvider) | 用於公開試算表或其他以格線為基礎的文件中儲存格的屬性。 |
| **樣式** | [**IStylesProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IStylesProvider) | 用於說明含有特定樣式、填滿色彩、填滿圖樣或形狀的 UI 元素。 |
| **SynchronizedInput** | [**ISynchronizedInputProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.ISynchronizedInputProvider) | 讓使用者介面自動化用戶端應用程式能夠將滑鼠或鍵盤輸入導向特定的 UI 元素。 |
| **資料表** | [**ITableProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.ITableProvider) | 用於有格線以及標題資訊的控制項。 例如，表格式行事曆控制項。 |
| **TableItem** | [**ITableItemProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.ITableItemProvider) | 用於表格中的項目。 |
| **Text** | [**ITextProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.ITextProvider) | 用於公開文字資訊的編輯控制項及文件。 另請參閱 [**ITextRangeProvider**](/uwp/api/windows.ui.xaml.automation.provider.itextrangeprovider) 和 [**ITextProvider2**](/uwp/api/windows.ui.xaml.automation.provider.itextprovider2)。 |
| **TextChild** | [**ITextChildProvider**](/uwp/api/windows.ui.xaml.automation.provider.itextchildprovider) | 用於存取最接近元素且支援 **Text** 控制項模式的上階。 |
| **TextEdit** | 沒有可用的 Managed 類別 | 提供可修改文字的控制項存取權，例如，執行自動校正或透過輸入法 (IME) 啟用輸入組合的控制項。 |
| **TextRange** | [**ITextRangeProvider**](/uwp/api/windows.ui.xaml.automation.provider.itextrangeprovider) | 提供實作 [**ITextProvider**](/uwp/api/windows.ui.xaml.automation.provider.itextprovider) 的文字容器中橫跨多行之連續文字的存取權。 另請參閱 [**ITextRangeProvider2**](/uwp/api/windows.ui.xaml.automation.provider.itextrangeprovider2)。 |
| **切換** | [**IToggleProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IToggleProvider) | 用於可切換狀態的控制項。 例如， [**CheckBox**](/uwp/api/Windows.UI.Xaml.Controls.CheckBox) 及可核取的功能表項目。 |
| **轉換** | [**ITransformProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.ITransformProvider) | 用於可以調整大小、移動和旋轉的控制項。 轉換控制項模式通常使用於設計工具、表單、圖形編輯器，以及繪圖應用程式。 |
| **ReplTest1** | [**IValueProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IValueProvider) | 允許用戶端對不支援範圍值的控制項，取得或設定一個值。 |
| **VirtualizedItem** | [**IVirtualizedItemProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IVirtualizedItemProvider) | 公開容器內被虛擬化且必須當做使用者介面自動化元素被完全存取的項目。 |
| **Window** | [**IWindowProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IWindowProvider) | 公開視窗特定的資訊 (Microsoft Windows 作業系統的基本概念)。 例如子視窗和對話方塊，就是本身為視窗的控制項。 |

> [!NOTE]
> 您將不需要在現有的 XAML 控制項中尋找所有這類模式的實作。 部分模式單獨擁有介面，利用模式的一般使用者介面自動化架構定義來支援同位，以及支援自動化對等案例，這類案例需要完全自訂的實作以支援該模式。

> [!NOTE]
> Windows Phone 市集應用程式不支援此處列出的所有使用者介面自動化控制項模式。 **Annotation** 、 **Dock** 、 **Drag** 、 **DropTarget** 、 **ObjectModel** 為部分不支援的模式。

<span id="related_topics"/>

## <a name="related-topics"></a>相關主題  
* [自訂自動化對等](custom-automation-peers.md)
* [協助工具](accessibility.md)
