---
author: PatrickFarley
ms.assetid: 9A0F1852-A76B-4F43-ACFC-2CC56AAD1C03
title: 從您的 app 列印
description: 了解如何從通用 Windows 應用程式列印文件。 本主題也示範如何列印特定頁面。
ms.author: pafarley
ms.date: 01/29/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10，uwp 列印
ms.localizationpriority: medium
ms.openlocfilehash: cff96c0b8daf9f3ef32815437b510a5b94641527
ms.sourcegitcommit: e6daa7ff878f2f0c7015aca9787e7f2730abcfbf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/03/2018
ms.locfileid: "4312580"
---
# <a name="print-from-your-app"></a>從您的應用程式列印



**重要 API**

-   [**Windows.Graphics.Printing**](https://msdn.microsoft.com/library/windows/apps/BR226489)
-   [**Windows.UI.Xaml.Printing**](https://msdn.microsoft.com/library/windows/apps/BR243325)
-   [**PrintDocument**](https://msdn.microsoft.com/library/windows/apps/BR243314)

了解如何從通用 Windows 應用程式列印文件。 本主題也示範如何列印特定頁面。 如需預覽列印 UI 的更進階變更，請參閱[自訂預覽列印 UI](customize-the-print-preview-ui.md)。

> [!TIP]
> 本主題中大部分的範例是以列印範例為基礎。 若要查看完整程式碼，請從 GitHub 的 [Windows-universal-samples 存放庫](http://go.microsoft.com/fwlink/p/?LinkId=619979)下載[通用 Windows 平台 (UWP) 列印範例](http://go.microsoft.com/fwlink/p/?LinkId=619984)。

## <a name="register-for-printing"></a>註冊列印

新增列印至 app 的第一個步驟是註冊列印協定。 您的 App 必須在要讓您的使用者能夠進行列印的每個畫面上登錄列印協定。 只有對使用者顯示的畫面可以登錄列印。 如果應用程式的某個畫面已登錄列印，則必須在結束該畫面時解除登錄列印。 如果該畫面被另一個畫面取代，則下一個畫面開啟時，必須註冊新的列印協定。

> [!TIP]
> 如果您需要在 App 內一個以上的頁面中支援列印，可以將這個列印程式碼放置於常用協助程式類別中，並讓您的 App 頁面重複使用它。 如需如何執行這項操作的範例，請參閱 [UWP 列印範例](http://go.microsoft.com/fwlink/p/?LinkId=619984)中的 `PrintHelper` 類別。

首先，宣告 [**PrintManager**](https://msdn.microsoft.com/library/windows/apps/BR226426) 和 [**PrintDocument**](https://msdn.microsoft.com/library/windows/apps/BR243314)。 **PrintManager** 類型位於 [**Windows.Graphics.Printing**](https://msdn.microsoft.com/library/windows/apps/BR226489) 命名空間以及支援其他 Windows 列印功能的類型中。 **PrintDocument** 類型位於 [**Windows.UI.Xaml.Printing**](https://msdn.microsoft.com/library/windows/apps/BR243325) 命名空間以及其他支援準備列印 XAML 內容的類型中。 您可以藉由將下列 **using** 或 **Imports** 陳述式新增到頁面中，更輕鬆地撰寫列印程式碼。

```csharp
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
```

[**PrintDocument**](https://msdn.microsoft.com/library/windows/apps/BR243314) 類別是用來處理大部分的 app 與 [**PrintManager**](https://msdn.microsoft.com/library/windows/apps/BR226426) 之間的互動，但是它會公開自己的數個回呼。 在註冊期間，建立 **PrintManager** 和 **PrintDocument** 的執行個體，並註冊其列印事件的處理常式。

在 [UWP 列印範例](http://go.microsoft.com/fwlink/p/?LinkId=619984) 中，註冊是由 `RegisterForPrinting` 方法執行。

```csharp
public virtual void RegisterForPrinting()
{
   printDocument = new PrintDocument();
   printDocumentSource = printDocument.DocumentSource;
   printDocument.Paginate += CreatePrintPreviewPages;
   printDocument.GetPreviewPage += GetPrintPreviewPage;
   printDocument.AddPages += AddPrintPages;

   PrintManager printMan = PrintManager.GetForCurrentView();
   printMan.PrintTaskRequested += PrintTaskRequested;
}
```

當使用者前往支援列印的頁面時，它會初始化 `OnNavigatedTo` 方法內的註冊。

```csharp
protected override void OnNavigatedTo(NavigationEventArgs e)
{
   // Initialize common helper class and register for printing
   printHelper = new PrintHelper(this);
   printHelper.RegisterForPrinting();

   // Initialize print content for this scenario
   printHelper.PreparePrintContent(new PageToPrint());

   // Tell the user how to print
   MainPage.Current.NotifyUser("Print contract registered with customization, use the Print button to print.", NotifyType.StatusMessage);
}
```

當使用者離開頁面時，就會中斷列印事件處理常式的連線。 如果您擁有多個頁面的 app 且未中斷列印的連線，則當使用者離開頁面，然後再回到該頁面時，系統就會擲回例外狀況。

```csharp
protected override void OnNavigatedFrom(NavigationEventArgs e)
{
   if (printHelper != null)
   {
         printHelper.UnregisterForPrinting();
   }
}
```
## <a name="create-a-print-button"></a>建立列印按鈕

將列印按鈕新增至 app 畫面中要放置按鈕的位置。 確定按鈕不會干擾您要列印的內容。

```xml
<Button x:Name="InvokePrintingButton" Content="Print" Click="OnPrintButtonClick"/>
```

接下來，將事件處理常式新增至您的 app 的程式碼，以處理 Click 事件。 使用 [**ShowPrintUIAsync**](https://msdn.microsoft.com/library/windows/apps/windows.graphics.printing.printmanager.showprintuiasync) 方法，開始從 app 列印。 **ShowPrintUIAsync** 是非同步方法，會顯示適當的列印視窗。 我們建議先呼叫 [**IsSupported**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Printing.PrintManager.IsSupported) 方法來檢查 App 是否是在支援列印的裝置上執行 (並處理不支援的情況)。 如果當下因任何其他原因而無法列印，**ShowPrintUIAsync** 會擲回例外狀況。 我們建議攔截這些例外狀況，並在無法繼續進行列印時讓使用者知道。

```csharp
async private void OnPrintButtonClick(object sender, RoutedEventArgs e)
{
    if (Windows.Graphics.Printing.PrintManager.IsSupported())
    {
        try
        {
            // Show print UI
            await Windows.Graphics.Printing.PrintManager.ShowPrintUIAsync();

        }
        catch
        {
            // Printing cannot proceed at this time
            ContentDialog noPrintingDialog = new ContentDialog()
            {
                Title = "Printing error",
                Content = "\nSorry, printing can' t proceed at this time.", PrimaryButtonText = "OK"
            };
            await noPrintingDialog.ShowAsync();
        }
    }
    else
    {
        // Printing is not supported on this device
        ContentDialog noPrintingDialog = new ContentDialog()
        {
            Title = "Printing not supported",
            Content = "\nSorry, printing is not supported on this device.",PrimaryButtonText = "OK"
        };
        await noPrintingDialog.ShowAsync();
    }
}
```

在這個範例中，事件處理常式中會針對按鈕點擊顯示列印視窗。 如果這個方法擲回例外狀況 (因為當時無法執行列印)，[**ContentDialog**](https://msdn.microsoft.com/library/windows/apps/Dn633972) 控制項會通知使用者該情況。

## <a name="format-your-apps-content"></a>將 app 的內容格式化

呼叫 **ShowPrintUIAsync** 時，會引發 [**PrintTaskRequested**](https://msdn.microsoft.com/library/windows/apps/br206597) 事件。 此步驟顯示的 **PrintTaskRequested** 事件處理常式，會藉由呼叫 [**PrintTaskRequest.CreatePrintTask**](https://msdn.microsoft.com/library/windows/apps/windows.graphics.printing.printtaskrequest.createprinttask.aspx) 方法及傳遞列印頁面的標題和 [**PrintTaskSourceRequestedHandler**](https://msdn.microsoft.com/library/windows/apps/windows.graphics.printing.printtask.source) 委派的名稱來建立 [**PrintTask**](https://msdn.microsoft.com/library/windows/apps/BR226436)。 請注意，這個範例中的 **PrintTaskSourceRequestedHandler** 是以內嵌的方式定義。 **PrintTaskSourceRequestedHandler** 會提供要列印的格式化內容，稍後會加以描述。

在這個範例中，也會定義完成處理常式以擷取錯誤。 處理完成事件是較好的做法，因為這樣一來，應用程式就可以讓使用者知道發生錯誤，並提供可能的解決方法。 同樣地，應用程式可以使用完成事件指出列印工作成功之後，使用者採取的後續步驟。

```csharp
protected virtual void PrintTaskRequested(PrintManager sender, PrintTaskRequestedEventArgs e)
{
   PrintTask printTask = null;
   printTask = e.Request.CreatePrintTask("C# Printing SDK Sample", sourceRequested =>
   {
         // Print Task event handler is invoked when the print job is completed.
         printTask.Completed += async (s, args) =>
         {
            // Notify the user when the print operation fails.
            if (args.Completion == PrintTaskCompletion.Failed)
            {
               await scenarioPage.Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal, () =>
               {
                     MainPage.Current.NotifyUser("Failed to print.", NotifyType.ErrorMessage);
               });
            }
         };

         sourceRequested.SetSource(printDocumentSource);
   });
}
```

建立列印工作之後，[**PrintManager**](https://msdn.microsoft.com/library/windows/apps/BR226426) 會藉由引發 [**Paginate**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.printing.printdocument.paginate) 事件，要求在預覽列印 UI 中顯示列印頁面集合。 這會對應至 **IPrintPreviewPageCollection** 介面的 **Paginate** 方法。 您在註冊期間建立的事件處理常式會在此時呼叫。

> [!IMPORTANT]
> 如果使用者變更列印設定，會再次呼叫編頁事件處理常式，讓您自動重排內容。 若要獲得最佳的使用者經驗，建議您先檢查設定，再自動重排內容，並避免在不必要時重新初始化已編頁的內容。

在 [**Paginate**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.printing.printdocument.paginate) 事件處理常式中 ([UWP 列印範例](http://go.microsoft.com/fwlink/p/?LinkId=619984)中的 `CreatePrintPreviewPages` 方法)，建立要在預覽列印 UI 中顯示並傳送到印表機的頁面。 您用來準備應用程式列印內容的程式碼，是應用程式和所列印內容專屬的程式碼。 請參閱 [UWP 列印範例](http://go.microsoft.com/fwlink/p/?LinkId=619984)原始程式碼，了解如何將要列印的內容格式化。

```csharp
protected virtual void CreatePrintPreviewPages(object sender, PaginateEventArgs e)
{
   // Clear the cache of preview pages
   printPreviewPages.Clear();

   // Clear the print canvas of preview pages
   PrintCanvas.Children.Clear();

   // This variable keeps track of the last RichTextBlockOverflow element that was added to a page which will be printed
   RichTextBlockOverflow lastRTBOOnPage;

   // Get the PrintTaskOptions
   PrintTaskOptions printingOptions = ((PrintTaskOptions)e.PrintTaskOptions);

   // Get the page description to deterimine how big the page is
   PrintPageDescription pageDescription = printingOptions.GetPageDescription(0);

   // We know there is at least one page to be printed. passing null as the first parameter to
   // AddOnePrintPreviewPage tells the function to add the first page.
   lastRTBOOnPage = AddOnePrintPreviewPage(null, pageDescription);

   // We know there are more pages to be added as long as the last RichTextBoxOverflow added to a print preview
   // page has extra content
   while (lastRTBOOnPage.HasOverflowContent && lastRTBOOnPage.Visibility == Windows.UI.Xaml.Visibility.Visible)
   {
         lastRTBOOnPage = AddOnePrintPreviewPage(lastRTBOOnPage, pageDescription);
   }

   if (PreviewPagesCreated != null)
   {
         PreviewPagesCreated.Invoke(printPreviewPages, null);
   }

   PrintDocument printDoc = (PrintDocument)sender;

   // Report the number of preview pages created
   printDoc.SetPreviewPageCount(printPreviewPages.Count, PreviewPageCountType.Intermediate);
}
```

當特定頁面顯示在預覽列印視窗中時，[**PrintManager**](https://msdn.microsoft.com/library/windows/apps/BR226426) 會引發 [**GetPreviewPage**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.printing.printdocument.getpreviewpage) 事件。 這會對應至 **IPrintPreviewPageCollection** 介面的 **MakePage** 方法。 您在註冊期間建立的事件處理常式會在此時呼叫。

在 [**GetPreviewPage**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.printing.printdocument.getpreviewpage) 事件處理常式 ([UWP 列印範例](http://go.microsoft.com/fwlink/p/?LinkId=619984)中的 `GetPrintPreviewPage` 方法) 中，在列印文件上設定適當的頁面。

```csharp
protected virtual void GetPrintPreviewPage(object sender, GetPreviewPageEventArgs e)
{
   PrintDocument printDoc = (PrintDocument)sender;
   printDoc.SetPreviewPage(e.PageNumber, printPreviewPages[e.PageNumber - 1]);
}
```

最後，當使用者按一下 [列印] 按鈕時，[**PrintManager**](https://msdn.microsoft.com/library/windows/apps/BR226426) 會要求傳送至印表機的頁面最終集合，方法是呼叫 **IDocumentPageSource** 介面的 **MakeDocument** 方法。 在 XAML 中，這會引發 [**AddPages**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.printing.printdocument.addpages) 事件。 您在註冊期間建立的事件處理常式會在此時呼叫。

在 [**AddPages**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.printing.printdocument.addpages) 事件處理常式 ([UWP 列印範例](http://go.microsoft.com/fwlink/p/?LinkId=619984)中的 `AddPrintPages` 方法) 中，將來自頁面集合的頁面新增至要傳送到印表機的 [**PrintDocument**](https://msdn.microsoft.com/library/windows/apps/BR243314) 物件。 如果使用者指定要列印特定的頁面或頁面範圍，您可以使用此處的資訊，只新增實際要傳送到印表機的頁面。

```csharp
protected virtual void AddPrintPages(object sender, AddPagesEventArgs e)
{
   // Loop over all of the preview pages and add each one to  add each page to be printied
   for (int i = 0; i < printPreviewPages.Count; i++)
   {
         // We should have all pages ready at this point...
         printDocument.AddPage(printPreviewPages[i]);
   }

   PrintDocument printDoc = (PrintDocument)sender;

   // Indicate that all of the print pages have been provided
   printDoc.AddPagesComplete();
}
```

## <a name="prepare-print-options"></a>準備列印選項

接下來準備列印選項。 本節將說明如何設定頁面範圍選項以允許列印特定頁面，做為範例。 如需其他進階選項，請參閱[自訂預覽列印 UI](customize-the-print-preview-ui.md)。

這個步驟會建立新的列印選項、定義選項支援的值清單，以及將選項新增至預覽列印 UI。 頁面範圍選項包含下列設定：

| 選項名稱          | 動作 |
|----------------------|--------|
| **列印全部**        | 列印文件中的所有頁面。|
| **列印選取項目**  | 僅列印使用者選取的內容。|
| **列印範圍**      | 顯示編輯控制項，讓使用者可以輸入頁面進行列印。|

首先，修改 [**PrintTaskRequested**](https://msdn.microsoft.com/library/windows/apps/br206597) 事件處理常式以新增程式碼，以便取得 [**PrintTaskOptionDetails**](https://msdn.microsoft.com/library/windows/apps/Hh701256) 物件。

```csharp
PrintTaskOptionDetails printDetailedOptions = PrintTaskOptionDetails.GetFromPrintTaskOptions(printTask.Options);
```

清除預覽列印 UI 中顯示的選項清單，並新增當使用者想要從 app 列印時要顯示的選項。

> [!NOTE]
> 這些選項會依照附加的順序出現在預覽列印 UI 中，第一個選項會在視窗的頂端顯示。

```csharp
IList<string> displayedOptions = printDetailedOptions.DisplayedOptions;

displayedOptions.Clear();
displayedOptions.Add(Windows.Graphics.Printing.StandardPrintTaskOptions.Copies);
displayedOptions.Add(Windows.Graphics.Printing.StandardPrintTaskOptions.Orientation);
displayedOptions.Add(Windows.Graphics.Printing.StandardPrintTaskOptions.ColorMode);
```

建立新的列印選項並初始化選項值的清單。

```csharp
// Create a new list option
PrintCustomItemListOptionDetails pageFormat = printDetailedOptions.CreateItemListOption("PageRange", "Page Range");
pageFormat.AddItem("PrintAll", "Print all");
pageFormat.AddItem("PrintSelection", "Print Selection");
pageFormat.AddItem("PrintRange", "Print Range");
```

新增自訂列印選項並指派事件處理常式。 自訂選項會最後附加，如此該選項就會出現在選項清單的底部。 但是您可以將它放到清單中的任意位置，自訂列印選項不一定要最後新增。

```csharp
// Add the custom option to the option list
displayedOptions.Add("PageRange");

// Create new edit option
PrintCustomTextOptionDetails pageRangeEdit = printDetailedOptions.CreateTextOption("PageRangeEdit", "Range");

// Register the handler for the option change event
printDetailedOptions.OptionChanged += printDetailedOptions_OptionChanged;
```

[**CreateTextOption**](https://msdn.microsoft.com/library/windows/apps/windows.graphics.printing.optiondetails.printtaskoptiondetails.createtextoption) 方法會建立 **\[範圍\]** 文字方塊。 當使用者選取 **\[列印範圍\]** 選項時，可以在此處輸入想要列印的特定頁面。

## <a name="handle-print-option-changes"></a>處理列印選項變更

**OptionChanged** 事件處理常式會執行兩個動作。 第一個動作是根據使用者選取的頁面範圍選項，顯示和隱藏頁面範圍的文字編輯欄位。 另一個動作是測試在頁面範圍文字方塊中輸入的文字，確定該文字代表有效的文件頁面範圍。

此範例說明 [UWP 列印範例](http://go.microsoft.com/fwlink/p/?LinkId=619984)如何處理變更事件。

```csharp
async void printDetailedOptions_OptionChanged(PrintTaskOptionDetails sender, PrintTaskOptionChangedEventArgs args)
{
   if (args.OptionId == null)
   {
         return;
   }

   string optionId = args.OptionId.ToString();

   // Handle change in Page Range Option
   if (optionId == "PageRange")
   {
         IPrintOptionDetails pageRange = sender.Options[optionId];
         string pageRangeValue = pageRange.Value.ToString();

         selectionMode = false;

         switch (pageRangeValue)
         {
            case "PrintRange":
               // Add PageRangeEdit custom option to the option list
               sender.DisplayedOptions.Add("PageRangeEdit");
               pageRangeEditVisible = true;
               await scenarioPage.Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal, () =>
               {
                     ShowContent(null);
               });
               break;
            case "PrintSelection":
               await scenarioPage.Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal, () =>
               {
                     Scenario4PageRange page = (Scenario4PageRange)scenarioPage;
                     PageToPrint pageContent = (PageToPrint)page.PrintFrame.Content;
                     ShowContent(pageContent.TextContentBlock.SelectedText);
               });
               RemovePageRangeEdit(sender);
               break;
            default:
               await scenarioPage.Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal, () =>
               {
                     ShowContent(null);
               });
               RemovePageRangeEdit(sender);
               break;
         }

         Refresh();
   }
   else if (optionId == "PageRangeEdit")
   {
         IPrintOptionDetails pageRange = sender.Options[optionId];
         // Expected range format (p1,p2...)*, (p3-p9)* ...
         if (!Regex.IsMatch(pageRange.Value.ToString(), @"^\s*\d+\s*(\-\s*\d+\s*)?(\,\s*\d+\s*(\-\s*\d+\s*)?)*$"))
         {
            pageRange.ErrorText = "Invalid Page Range (eg: 1-3, 5)";
         }
         else
         {
            pageRange.ErrorText = string.Empty;
            try
            {
               GetPagesInRange(pageRange.Value.ToString());
               Refresh();
            }
            catch (InvalidPageException ipex)
            {
               pageRange.ErrorText = ipex.Message;
            }
         }
   }
}
```

> [!TIP]
> 如需如何剖析使用者在 [範圍] 文字方塊中輸入之頁面範圍的詳細資料，請參閱 [UWP 列印範例](http://go.microsoft.com/fwlink/p/?LinkId=619984)中的 `GetPagesInRange` 方法。

## <a name="preview-selected-pages"></a>預覽已選取的頁面

將要列印的 app 內容格式化的方式，取決於您的 app 及其內容的性質。 [UWP 列印範例](http://go.microsoft.com/fwlink/p/?LinkId=619984)會使用列印協助程式類別來格式化要列印的內容。

列印頁面子集時，有數種方法可以在預覽列印中顯示內容。 不論您選擇使用哪一種方法在預覽列印中顯示頁面範圍，列印的輸出都必須只包含選取的頁面。

-   不論是否指定頁面範圍，在預覽列印中都顯示所有頁面，讓使用者自己知道實際要印出的是哪些頁面。
-   在預覽列印中只顯示使用者的頁面範圍內所選取的頁面，並在每次使用者變更頁面範圍時更新顯示。
-   在預覽列印中顯示所有頁面，但是以灰色呈現不在使用者所選頁面範圍內的頁面。

## <a name="related-topics"></a>相關主題

* [列印的設計指導方針](https://msdn.microsoft.com/library/windows/apps/Hh868178)
* [//2015 建置影片：開發在 Windows 10 中列印的 app](https://channel9.msdn.com/Events/Build/2015/2-94)
* [UWP 列印範例](http://go.microsoft.com/fwlink/p/?LinkId=619984)
