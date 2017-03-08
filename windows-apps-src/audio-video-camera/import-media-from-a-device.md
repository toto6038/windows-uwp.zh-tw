---
author: drewbatgit
ms.assetid: dd2a1e01-c284-4d62-963e-f59f58dca61a
description: "本文描述從裝置匯入媒體的方式，包括搜尋可用媒體來源、匯入如相片和側車檔案的檔案，以及從來源裝置上刪除已匯入的檔案。"
title: "匯入媒體"
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
translationtype: Human Translation
ms.sourcegitcommit: 5645eee3dc2ef67b5263b08800b0f96eb8a0a7da
ms.openlocfilehash: 4deda6efa9b9b9ea03bee76855e30c8e9a290480
ms.lasthandoff: 02/08/2017

---

# <a name="import-media-from-a-device"></a>從裝置匯入媒體

本文描述從裝置匯入媒體的方式，包括搜尋可用媒體來源、匯入如影片、相片和側車檔案的檔案，以及從來源裝置上刪除已匯入的檔案。

> [!NOTE] 
> 本文中的程式碼是採用 [**MediaImport UWP App 範例**](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MediaImport)的程式碼。 您可以從[**通用 Windows App 範例 Git 儲存機制**](https://github.com/Microsoft/Windows-universal-samples)複製或下載此範例，以在內容中查看程式碼，或將它做為建立 App 的起點使用。

## <a name="create-a-simple-media-import-ui"></a>建立簡單的媒體匯入 UI
本文中的範例會使用精簡的 UI 來啟用核心媒體匯入案例。 如果要查看如何建立更加健全的媒體匯入 App UI，請參閱 [**MediaImport 範例**](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MediaImport)。 下列 XAML 能建立具有下列控制項的堆疊面板：
* 一個用來初始化搜尋媒體匯入來源的「[按鈕****](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.Button)」。
* 一個用來列出找到的媒體匯入來源，並從中進行選取的 [**ComboBox**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.ComboBox)。
* 一個用來顯示來自選取匯入來源的媒體項目，並從中進行選取的 [**ListView**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.ListView) 控制項。
* 一個用來初始化從選取來源匯入媒體項目的「按鈕」****。
* 一個用來初始化從選取來源刪除已匯入項目的「按鈕」****。
* 一個用來取消非同步媒體匯入作業的的「按鈕」****。

[!code-xml[ImportXAML](./code/PhotoImport_Win10/cs/MainPage.xaml#SnippetImportXAML)]

## <a name="set-up-your-code-behind-file"></a>設定您的程式碼後置檔案
新增 *using* 指示詞以包含本範例所用且未包含在預設專案範本中的命名空間。

[!code-cs[Using](./code/PhotoImport_Win10/cs/MainPage.xaml.cs#SnippetUsing)]

## <a name="set-up-task-cancellation-for-media-import-operations"></a>為媒體匯入作業設定工作取消

由於媒體匯入作業可能會需要較長的時間，它們會使用 [**IAsyncOperationWithProgress**](https://msdn.microsoft.com/library/windows/apps/br206594.aspx) 以非同步的方式執行。 宣告會用來在使用者按一下取消按鈕時，取消進行中作業的類型 [**CancellationTokenSource**](https://msdn.microsoft.com/library/system.threading.cancellationtokensource) 類別成員變數。

[!code-cs[DeclareCts](./code/PhotoImport_Win10/cs/MainPage.xaml.cs#SnippetDeclareCts)]

實作取消按鈕的處理常式 於本文後續示範的範例，將會在作業開始時初始化 **CancellationTokenSource**，並在作業完成時將它設定為 null。 在取消按鈕處理常式中，請查看權杖是否為 null，如果不是，請呼叫 [**Cancel**](https://msdn.microsoft.com/library/dd321955) 以取消作業。

[!code-cs[OnCancel](./code/PhotoImport_Win10/cs/MainPage.xaml.cs#SnippetOnCancel)]

## <a name="data-binding-helper-classes"></a>資料繫結協助程式類別

在一般的媒體匯入案例中，您會向使用者顯示可匯入媒體項目的清單。清單上可選擇的媒體數目可能會很龐大，且您通常會想要顯示每個媒體項目的縮圖。 因此，此範例會使用三個協助程式類別，來隨著使用者向下捲動清單以累加的方式將項目載入 ListView 控制項。

* **IncrementalLoadingBase** 類別 - 實作 [**IList**](https://msdn.microsoft.com/library/system.collections.ilist)、[**ISupportIncrementalLoading**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.data.isupportincrementalloading) 及 [**INotifyCollectionChanged**](https://msdn.microsoft.com/library/windows/apps/system.collections.specialized.inotifycollectionchanged(v=vs.105).aspx) 以提供基礎累加載入行為。
* **GeneratorIncrementalLoadingClass** 類別 - 提供累加載入基底類別的實作。
* **ImportableItemWrapper** 類別 - 包覆 [**PhotoImportItem**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportItem) 類別的精簡型包裝函式，以為每個已匯入項目的縮圖影像新增可繫結的 [**BitmapImage**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Media.Imaging.BitmapImage) 屬性。

這些類別皆於 [**MediaImport 範例**](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MediaImport)中提供，並可以不需修改變新增到您的專案中。 新增協助程式類別到專案之後，請宣告稍後將於本範例中使用的類型 **GeneratorIncrementalLoadingClass** 類別成員變數。

[!code-cs[GeneratorIncrementalLoadingClass](./code/PhotoImport_Win10/cs/MainPage.xaml.cs#SnippetGeneratorIncrementalLoadingClass)]


# <a name="find-available-sources-from-which-media-can-be-imported"></a>尋找可匯入媒體的可用來源

在尋找來源按鈕的 click 處理常式中，呼叫靜態方法 [**PhotoImportManager.FindAllSourcesAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportManager.FindAllSourcesAsync) 來讓系統開始搜尋可以匯入媒體的來源裝置。 等候作業完成之後，請循環顯示傳回清單中的每個 [**PhotoImportSource**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportSource) 物件並新增項目到 **ComboBox**，將 **Tag** 屬性設定到來源物件本身，來在使用者做出選取時可以輕易擷取它。

[!code-cs[FindSourcesClick](./code/PhotoImport_Win10/cs/MainPage.xaml.cs#SnippetFindSourcesClick)]

宣告類別成員變數以儲存使用者的選取匯入來源

[!code-cs[DeclareImportSource](./code/PhotoImport_Win10/cs/MainPage.xaml.cs#SnippetDeclareImportSource)]

在匯入來源 **ComboBox** 的 [**SelectionChanged**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.Primitives.Selector.SelectionChanged) 處理常式中，將類別成員變數設定到選取的來源，然後呼叫將於本文後續示範的 **FindItems** 協助程式方法。 

[!code-cs[SourcesSelectionChanged](./code/PhotoImport_Win10/cs/MainPage.xaml.cs#SnippetSourcesSelectionChanged)]

# <a name="find-items-to-import"></a>尋找可匯入的項目

新增類型 [**PhotoImportSession**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportSession) 和 [**PhotoImportFindItemsResult**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportFindItemsResult) 的類別成員變數，這將會在後續步驟中使用。

[!code-cs[DeclareImport](./code/PhotoImport_Win10/cs/MainPage.xaml.cs#SnippetDeclareImport)]

在 FindItems 方法中，初始化 **CancellationTokenSource** 變數，來使它可以在必要的情況下用於取消尋找作業。 在 **try** 區塊內，透過在由使用者選取的 [**PhotoImportSource**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportSource) 物件上呼叫 [**CreateImportSession**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportSource.CreateImportSession) 來建立新的匯入工作階段。 建立新的 [**Progress**](https://msdn.microsoft.com/library/hh193692.aspx) 物件來提供回呼以顯示尋找作業的進度。 接下來，請呼叫 [**FindItemsAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportSession.FindItemsAsync(Windows.Media.Import.PhotoImportContentTypeFilter,Windows.Media.Import.PhotoImportItemSelectionMode) 以開始尋找作業。 提供 [**PhotoImportContentTypeFilter**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportContentTypeFilter) 值以指定是否應該傳回相片或影片，或是兩者皆應該傳回。 提供 [**PhotoImportItemSelectionMode**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportItemSelectionMode) 值以指定應該要將哪些媒體項目的 [**IsSelected**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportItem.IsSelected) 屬性設定為 true 並回傳 (所有媒體項目、沒有媒體項目，或是僅新的媒體項目)。 此屬性在我們的 ListBox 項目範本中已繫結至每個媒體項目的核取方塊上。

**FindItemsAsync** 會傳回 [**IAsyncOperationWithProgress**](https://msdn.microsoft.com/library/windows/apps/br206594.aspx)。 延伸方法 [**AsTask**](https://msdn.microsoft.com/library/hh779750.aspx) 是用來建立可等候、可透過取消權杖取消，以及可使用提供的 **Progress** 物件報告進度的工作。

接下來將會初始化資料繫結協助程式類別 **GeneratorIncrementalLoadingClass**。 當 **FindItemsAsync** 從等候中傳回時，將會傳回 [**PhotoImportFindItemsResult**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportFindItemsResult) 物件。 此物件包含有關尋找作業的狀態資訊，包括作業的成功，以及已找到媒體項目的類型計數。 [**FoundItems**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportFindItemsResult.FoundItems) 屬性包含代表已找到媒體項目的 [**PhotoImportItem**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportItem) 物件清單。 **GeneratorIncrementalLoadingClass** 建構函式接受將會以累加方式載入的項目總數做為引數，以及會視需求產生要載入之新項目的函式。 在這個情況下，提供的 Lambda 運算式會建立 **ImportableItemWrapper** 的新執行個體，該執行個體將會包裝 **PhotoImportItem** 並包含每個項目的縮圖。 一旦初始化累加載入類別，請將它設定到 UI 中 **ListView** 控制項的 [**ItemsSource**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.ItemsControl.ItemsSource) 屬性。 現在，已找到媒體項目將會以累加的方式載入，並顯示在清單上。

接下來，尋找作業的狀態資訊將會輸出。 典型的 App 會在 UI 中向使用者顯示此資訊，但此範例只會將該資訊輸出到偵錯主控台。 最後，由於作業已完成，請將取消權杖設定為 null。

[!code-cs[FindItems](./code/PhotoImport_Win10/cs/MainPage.xaml.cs#SnippetFindItems)]

## <a name="import-media-items"></a>匯入媒體項目

在實作匯入作業之前，請先宣告 [**PhotoImportImportItemsResult**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportImportItemsResult) 物件以儲存匯入作業的結果。 這將會於稍後用來刪除已成功從來源匯入的媒體項目。

[!code-cs[DeclareImportResult](./code/PhotoImport_Win10/cs/MainPage.xaml.cs#SnippetDeclareImportResult)]

在開始媒體匯入作業之前，請透過將 [**ProgressBar**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.ProgressBar) 控制項的值設定為 0 來初始化 **CancellationTokenSource** 變數。

如果 **ListView** 控制項中沒有任何選取的項目，便沒有任何可匯入的項目。 否則，請初始化 [**Progress**](https://msdn.microsoft.com/library/hh193692.aspx) 物件來提供進度回呼，這將會更新進度列控制項的值。 針對由尋找作業所傳回之 [**PhotoImportFindItemsResult**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportFindItemsResult) 的 [**ItemImported**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportFindItemsResult.ItemImported) 事件註冊處理常式。 此事件將會在匯入項目時引發，並在此範例中會將每個已匯入檔案的名稱輸出到偵錯主控台上。

呼叫 [**ImportItemsAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportFindItemsResult.ImportItemsAsync) 以開始匯入作業。 和尋找作業相同，[**AsTask**](https://msdn.microsoft.com/library/hh779750.aspx) 延伸方法會被用來將傳回的作業轉換成可等候、報告進度，以及可取消的工作。

當匯入作業完成之後，便可以從由 [**ImportItemsAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportFindItemsResult.ImportItemsAsync) 所傳回的 [**PhotoImportImportItemsResult**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportImportItemsResult) 物件取得作業狀態。 此範例會將狀態資訊輸出到偵錯主控台，並於最後將取消權杖設定為 null。

[!code-cs[ImportClick](./code/PhotoImport_Win10/cs/MainPage.xaml.cs#SnippetImportClick)]

## <a name="delete-imported-items"></a>刪除已匯入項目
如果要從已成功匯入項目的來源刪除那些項目，請先初始化取消權杖以確保刪除作業可以被取消，並將進度列值設定為 0。 請確定從 [**ImportItemsAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportFindItemsResult.ImportItemsAsync) 傳回的 [**PhotoImportImportItemsResult**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportImportItemsResult) 不是 null。 如果不是，請再次建立 [**Progress**](https://msdn.microsoft.com/library/hh193692.aspx) 物件以為刪除作業提供進度回呼。 呼叫 [**DeleteImportedItemsFromSourceAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportImportItemsResult.DeleteImportedItemsFromSourceAsync) 以開始刪除已匯入項目。 使用 **AsTask** 以將結果轉換成具有進度和取消功能的可等候工作。 等候完畢之後，傳回的 [**PhotoImportDeleteImportedItemsFromSourceResult**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportDeleteImportedItemsFromSourceResult) 物件可以用來取得並顯示有關刪除作業的狀態資訊。

[!code-cs[DeleteClick](./code/PhotoImport_Win10/cs/MainPage.xaml.cs#SnippetDeleteClick)]








 



