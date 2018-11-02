---
author: jwmsft
ms.assetid: 3A477380-EAC5-44E7-8E0F-18346CC0C92F
title: ListView 和 GridView 資料虛擬化
description: 透過資料虛擬化改善 ListView 和 GridView 的效能和啟動時間。
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 92b81c79eb1be9e21aa7c306ef31b0b3bb62e7d1
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/02/2018
ms.locfileid: "5936623"
---
# <a name="listview-and-gridview-data-virtualization"></a>ListView 和 GridView 資料虛擬化


**注意：** 如需詳細資訊，請參閱 //build/ 工作階段[與大量資料 GridView 與 ListView 中的使用者互動時大幅提升效能](https://channel9.msdn.com/Events/Build/2013/3-158)。

透過資料虛擬化改善 [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878) 和 [**GridView**](https://msdn.microsoft.com/library/windows/apps/BR242705) 的效能和啟動時間。 如需 UI 虛擬化、減少元素和漸進式更新項目的資訊，請參閱 [ListView 與 GridView UI 最佳化](optimize-gridview-and-listview.md)。

資料虛擬化方法 對於大型而無法或不應該同時儲存在記憶體的資料集是必要的。 您將初始部分載入記憶體 (從本機磁碟機、網路或雲端) 並將 UI 虛擬化套用到這個局部的資料集。 您可以稍後以遞增的方式載入資料，或視需要從主要資料集的任意點 (隨機存取) 載入。 資料虛擬化是否適合您取決於許多因素。

-   資料集大小
-   每個項目大小
-   資料集來源 (本機磁碟、網路或雲端)
-   app 的整體記憶體耗用量

**注意：** 有一項功能會啟用預設針對 ListView 和 GridView 會在使用者移動瀏覽/捲動時快速顯示暫時性預留位置視覺效果。 載入資料時，會使用您的項目範本來取代這些預留位置視覺效果。 您可以將 [**ListViewBase.ShowsScrollingPlaceholders**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.showsscrollingplaceholders) 設為 False 來關閉該功能，但是如果您這樣做，則建議您使用 x:Phase 屬性，逐漸轉譯項目範本中的元素。 請參閱[逐漸更新 ListView 與 GridView 項目](optimize-gridview-and-listview.md#update-items-incrementally)。

以下是更多關於遞增和隨機存取資料虛擬化技術的詳細資訊。

## <a name="incremental-data-virtualization"></a>遞增資料虛擬化

遞增資料虛擬化會依序載入資料。 [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878) 使用遞增資料虛擬化，可能用來檢視數百萬個項目的集合，但最初只會載入 50 個項目。 當使用者移動瀏覽/捲動時，會載入接下來的 50 個項目。 載入項目時，捲軸的捲動方塊會減少大小。 針對這種資料虛擬化類型，您使用實作這些介面的資料來源類別。

-   [**IList**](https://msdn.microsoft.com/library/windows/apps/xaml/system.collections.ilist.aspx)
-   [**INotifyCollectionChanged**](https://msdn.microsoft.com/library/windows/apps/xaml/system.collections.specialized.inotifycollectionchanged.aspx) (C#/VB) 或 [**IObservableVector&lt;T&gt;**](https://msdn.microsoft.com/library/windows/apps/BR226052) (C++/CX)
-   [**ISupportIncrementalLoading**](https://msdn.microsoft.com/library/windows/apps/Hh701916)

像這樣的資料來源是可以持續擴充的記憶體內清單。 項目控制項會要求項目使用標準 [**IList**](https://msdn.microsoft.com/library/windows/apps/xaml/system.collections.ilist.aspx) 索引子和計數屬性。 計數應該代表項目在本機的數量，而不是資料集的實際大小。

當項目控制項接近現有資料的結尾時，它會呼叫 [**ISupportIncrementalLoading.HasMoreItems**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.data.isupportincrementalloading.hasmoreitems)。 如果傳回 **true**，則它會呼叫 [**ISupportIncrementalLoading.LoadMoreItemsAsync**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.data.isupportincrementalloading.loadmoreitemsasync) 傳遞要載入的建議項目數。 根據您載入資料的來源位置 (本機磁碟機、網路或雲端)，您可以選擇載入與建議不同的項目數。 例如，如果您的服務支援 50 個項目的批次，但是項目控制項只要求 10 個，則您可以載入 50 個。 從後端載入資料、將它新增到您的清單，以及透過 [**INotifyCollectionChanged**](https://msdn.microsoft.com/library/windows/apps/xaml/system.collections.specialized.inotifycollectionchanged.aspx) 或 [**IObservableVector&lt;T&gt;**](https://msdn.microsoft.com/library/windows/apps/BR226052) 引發變更通知，項目控制項就能知道新的項目。 也會傳回您實際載入的項目計數。 如果載入比建議還少的項目，或項目控制項臨時被進一步移動瀏覽/捲動，您的資料來源會再次呼叫更多項目，且循環會繼續。 您可以藉由為 Windows8.1 下載[XAML 資料繫結範例](https://code.msdn.microsoft.com/windowsapps/Data-Binding-7b1d67b5)，並重複使用其原始程式碼，在 windows 10 應用程式中多了。

## <a name="random-access-data-virtualization"></a>隨機存取資料虛擬化

隨機存取資料虛擬化允許從資料集的任意點載入。 [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878) 使用隨機存取資料虛擬化，用來檢視數百萬個項目的集合，可以載入 100,000 – 100,050 個項目。 如果使用者接著移動到清單開頭，控制項就會載入項目 1 – 50。 捲軸的捲動方塊隨時會表示 **ListView** 包含數百萬個項目。 捲軸的捲動方塊位置是可見項目在集合內整個資料集中的相對位置。 這種類型的資料虛擬化可以大幅減少記憶體需求和集合的載入時間。 若要啟用，您必須撰寫可隨選擷取資料和管理本機快取及實作這些介面的資料來源類別。

-   [**IList**](https://msdn.microsoft.com/library/windows/apps/xaml/system.collections.ilist.aspx)
-   [**INotifyCollectionChanged**](https://msdn.microsoft.com/library/windows/apps/xaml/system.collections.specialized.inotifycollectionchanged.aspx) (C#/VB) 或 [**IObservableVector&lt;T&gt;**](https://msdn.microsoft.com/library/windows/apps/BR226052) (C++/CX)
-   (選擇性) [**IItemsRangeInfo**](https://msdn.microsoft.com/library/windows/apps/Dn877070)
-   (選擇性) [**ISelectionInfo**](https://msdn.microsoft.com/library/windows/apps/Dn877074)

[**IItemsRangeInfo**](https://msdn.microsoft.com/library/windows/apps/Dn877070) 提供控制項正在使用哪些項目的資訊。 項目控制項會在其檢視變更時呼叫這個方法，並且將會包含兩個範圍集。

-   檢視區中的項目集合。
-   控制項使用的非虛擬化項目集合可能不在檢視區中。
    -   項目控制項在檢視區周圍保留的項目緩衝區，讓觸控移動瀏覽流暢。
    -   焦點的項目。
    -   第一個項目。

藉由實作 [**IItemsRangeInfo**](https://msdn.microsoft.com/library/windows/apps/Dn877070)，您的資料來源知道要擷取和快取哪些項目，以及何時從不再需要的快取資料刪除。 **IItemsRangeInfo** 使用 [**ItemIndexRange**](https://msdn.microsoft.com/library/windows/apps/Dn877081) 物件，根據其集合中的索引來描述一組項目。 這是為何它不使用項目指標，指標可能不正確或不穩定。 **IItemsRangeInfo** 是設計僅讓項目控制項中的單一執行個體使用，因為它仰賴該項目控制項的狀態資訊。 如果多個項目控制項需要存取相同資料，則您針對每個項目將會需要資料來源的個別執行個體。 它們可以共用通用快取，但是從快取清除的邏輯會變得更複雜。

以下是您隨機存取資料虛擬化資料來源的基本策略。

-   當要求項目時
    -   如果您可在記憶體中使用，則將它傳回。
    -   如果您沒有，則傳回 Null 或預留位置項目。
    -   使用要求項目 (或從 [**IItemsRangeInfo**](https://msdn.microsoft.com/library/windows/apps/Dn877070) 的範圍資訊) 以了解所需的項目，並以非同步方式從後端擷取項目的資料。 擷取資料、透過 [**INotifyCollectionChanged**](https://msdn.microsoft.com/library/windows/apps/xaml/system.collections.specialized.inotifycollectionchanged.aspx) 或 [**IObservableVector&lt;T&gt;**](https://msdn.microsoft.com/library/windows/apps/BR226052) 引發變更通知之後，項目控制項就能知道新的項目。
-   (選擇性) 當項目控制項的檢視區變更時，透過實作 [**IItemsRangeInfo**](https://msdn.microsoft.com/library/windows/apps/Dn877070)，找出需要您的資料來源的哪些項目。

除此之外，載入資料項目的時機、載入多少，以及在記憶體中保留哪些項目的策略，取決於您的 app。 要記住的一些一般考量：

-   提出非同步要求的資料；不要封鎖 UI 執行緒。
-   在您擷取項目的批次中尋找大小的甜蜜點。 偏好厚重，不愛小巧。 量不小卻要進行太多小型要求；量不大卻要花這麼久的時間擷取。
-   請考量您要同時擱置多少要求。 一次執行一個是比較容易，但是如果往返時間長則會太慢。
-   您可以取消資料的要求嗎？
-   如果使用裝載的服務，每個交易是否需要費用？
-   當查詢結果變更時，服務會提供哪種通知？ 如果項目插入索引 33，您是否會知道？ 如果您的服務根據金鑰加位移支援查詢，可能比只使用索引更好。
-   您想要在預先擷取項目中擁有什麼樣的智慧？ 您是否要嘗試和追蹤捲動的方向和速度，以預測所需的項目？
-   您對於清除快取的積極程度？ 這是記憶體與體驗之間的取捨問題。




