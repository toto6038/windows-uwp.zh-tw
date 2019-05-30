---
ms.assetid: 88132B6F-FB50-4B03-BC21-233988746230
title: 自訂預覽列印 UI
description: 本節說明如何在預覽列印 UI 中自訂列印選項和設定。
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10、 uwp、 列印
ms.localizationpriority: medium
ms.openlocfilehash: 68f8f990209a66a8677afbd1913c95bfd2fce187
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/29/2019
ms.locfileid: "66370296"
---
# <a name="customize-the-print-preview-ui"></a>自訂預覽列印 UI



**重要的 Api**

-   [**Windows.Graphics.Printing**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Printing)
-   [**Windows.UI.Xaml.Printing**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Printing)
-   [**PrintManager**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Printing.PrintManager)

本節說明如何在預覽列印 UI 中自訂列印選項和設定。 如需列印的詳細資訊，請參閱[從您的 app 進行列印](print-from-your-app.md)。

**祕訣**  大部分的此主題中的範例以列印範例為基礎。 若要查看完整程式碼，請從 GitHub 的 [Windows-universal-samples 儲存機制](https://go.microsoft.com/fwlink/p/?LinkId=619979)下載[通用 Windows 平台 (UWP) 列印範例](https://go.microsoft.com/fwlink/p/?LinkId=619984)。

 

## <a name="customize-print-options"></a>自訂列印選項

根據預設，預覽列印 UI 會顯示 [**ColorMode**](https://docs.microsoft.com/uwp/api/windows.graphics.printing.standardprinttaskoptions.colormode)、[**Copies**](https://docs.microsoft.com/uwp/api/windows.graphics.printing.standardprinttaskoptions.copies) 和 [**Orientation**](https://docs.microsoft.com/uwp/api/windows.graphics.printing.standardprinttaskoptions.orientation) 選項。 除了上述選項，還有數個其他常見的印表機選項，讓您可以新增到預覽列印 UI：

-   [**Binding**](https://docs.microsoft.com/uwp/api/windows.graphics.printing.standardprinttaskoptions.binding)
-   [**定序**](https://docs.microsoft.com/uwp/api/windows.graphics.printing.standardprinttaskoptions.collation)
-   [**Duplex**](https://docs.microsoft.com/uwp/api/windows.graphics.printing.standardprinttaskoptions.duplex)
-   [**HolePunch**](https://docs.microsoft.com/uwp/api/windows.graphics.printing.standardprinttaskoptions.holepunch)
-   [**InputBin**](https://docs.microsoft.com/uwp/api/windows.graphics.printing.standardprinttaskoptions.inputbin)
-   [**MediaSize**](https://docs.microsoft.com/uwp/api/windows.graphics.printing.standardprinttaskoptions.mediasize)
-   [**MediaType**](https://docs.microsoft.com/uwp/api/windows.graphics.printing.standardprinttaskoptions.mediatype)
-   [**NUp**](https://docs.microsoft.com/uwp/api/windows.graphics.printing.standardprinttaskoptions.nup)
-   [**PrintQuality**](https://docs.microsoft.com/uwp/api/windows.graphics.printing.standardprinttaskoptions.printquality)
-   [**Staple**](https://docs.microsoft.com/uwp/api/windows.graphics.printing.standardprinttaskoptions.staple)

這些選項會在 [**StandardPrintTaskOptions**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Printing.StandardPrintTaskOptions) 類別中定義。 您可以在預覽列印 UI 中顯示的選項清單中新增或移除選項。 您也可以變更選項出現的順序，以及設定對使用者顯示的預設設定。

不過，您使用這個方法所做的修改只會影響預覽列印 UI。 只要點選預覽列印 UI 中的 [更多設定]  連結，使用者就可以存取印表機支援的所有選項。

**附註**  您的應用程式，可指定要顯示的任何列印選項，但只會受到所選印表機會顯示在預覽列印 UI 中。 列印 UI 不會顯示選定的印表機不支援的選項。

 

### <a name="define-the-options-to-display"></a>定義要顯示的選項

App 的畫面載入時，會登錄列印協定。 該登錄包含定義 [**PrintTaskRequested**](https://msdn.microsoft.com/library/windows/apps/br206597) 事件處理常式。 自訂預覽列印 UI 中所顯示選項的程式碼會新增到 **PrintTaskRequested** 事件處理常式中。

修改 [**PrintTaskRequested**](https://msdn.microsoft.com/library/windows/apps/br206597) 事件處理常式以包含 [**printTask.options**](https://docs.microsoft.com/uwp/api/windows.graphics.printing.printtask.options) 指示，這些指示可用來設定您要在預覽列印 UI 中顯示的列印設定。對於要顯示自訂列印選項清單的 app 畫面，請覆寫協助程式類別中的 **PrintTaskRequested** 事件處理常式以加入程式碼，這個程式碼會指定列印畫面時要顯示的選項。

``` csharp
protected override void PrintTaskRequested(PrintManager sender, PrintTaskRequestedEventArgs e)
{
   PrintTask printTask = null;
   printTask = e.Request.CreatePrintTask("C# Printing SDK Sample", sourceRequestedArgs =>
   {
         IList<string> displayedOptions = printTask.Options.DisplayedOptions;

         // Choose the printer options to be shown.
         // The order in which the options are appended determines the order in which they appear in the UI
         displayedOptions.Clear();
         displayedOptions.Add(Windows.Graphics.Printing.StandardPrintTaskOptions.Copies);
         displayedOptions.Add(Windows.Graphics.Printing.StandardPrintTaskOptions.Orientation);
         displayedOptions.Add(Windows.Graphics.Printing.StandardPrintTaskOptions.MediaSize);
         displayedOptions.Add(Windows.Graphics.Printing.StandardPrintTaskOptions.Collation);
         displayedOptions.Add(Windows.Graphics.Printing.StandardPrintTaskOptions.Duplex);

         // Preset the default value of the printer option
         printTask.Options.MediaSize = PrintMediaSize.NorthAmericaLegal;

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

         sourceRequestedArgs.SetSource(printDocumentSource);
   });
}
```

**重要**  呼叫[ **displayedOptions.clear**](https://docs.microsoft.com/uwp/api/windows.graphics.printing.printtaskoptions.displayedoptions)（） 則會從 預覽列印 UI 中，移除所有的列印選項包括**更多設定**連結。 務必在預覽列印 UI 上附加要顯示的選項。

### <a name="specify-default-options"></a>指定預設選項

您也可以在預覽列印 UI 中設定選項的預設值。 下列這行程式碼來自上一個範例，會設定 [**MediaSize**](https://docs.microsoft.com/uwp/api/windows.graphics.printing.standardprinttaskoptions.mediasize) 選項的預設值。

``` csharp
         // Preset the default value of the printer option
         printTask.Options.MediaSize = PrintMediaSize.NorthAmericaLegal;
```         

## <a name="add-new-print-options"></a>新增列印選項

本節顯示如何建立新的列印選項、定義選項支援的值清單，以及將選項新增至預覽列印。 如同上一節，在 [**PrintTaskRequested**](https://msdn.microsoft.com/library/windows/apps/br206597) 事件處理常式中加入新的列印選項。

首先，取得 [**PrintTaskOptionDetails**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Printing.OptionDetails.PrintTaskOptionDetails) 物件。 此物件用來將新列印選項加入至預覽列印 UI。 然後清除預覽列印 UI 中顯示的選項清單，並且新增當使用者想要從 app 列印時所要顯示的選項。 接著，建立新的列印選項並初始化選項值的清單。 最後，加入新選項並指派 **OptionChanged** 事件的處理常式。

``` csharp
protected override void PrintTaskRequested(PrintManager sender, PrintTaskRequestedEventArgs e)
{
   PrintTask printTask = null;
   printTask = e.Request.CreatePrintTask("C# Printing SDK Sample", sourceRequestedArgs =>
   {
         PrintTaskOptionDetails printDetailedOptions = PrintTaskOptionDetails.GetFromPrintTaskOptions(printTask.Options);
         IList<string> displayedOptions = printDetailedOptions.DisplayedOptions;

         // Choose the printer options to be shown.
         // The order in which the options are appended determines the order in which they appear in the UI
         displayedOptions.Clear();

         displayedOptions.Add(Windows.Graphics.Printing.StandardPrintTaskOptions.Copies);
         displayedOptions.Add(Windows.Graphics.Printing.StandardPrintTaskOptions.Orientation);
         displayedOptions.Add(Windows.Graphics.Printing.StandardPrintTaskOptions.ColorMode);

         // Create a new list option
         PrintCustomItemListOptionDetails pageFormat = printDetailedOptions.CreateItemListOption("PageContent", "Pictures");
         pageFormat.AddItem("PicturesText", "Pictures and text");
         pageFormat.AddItem("PicturesOnly", "Pictures only");
         pageFormat.AddItem("TextOnly", "Text only");

         // Add the custom option to the option list
         displayedOptions.Add("PageContent");

         printDetailedOptions.OptionChanged += printDetailedOptions_OptionChanged;

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

         sourceRequestedArgs.SetSource(printDocumentSource);
   });
}
```

這些選項會依照附加的順序出現在預覽列印 UI 中，第一個選項會在視窗的頂端顯示。 在這個範例中，自訂選項會最後附加，如此該選項就會出現在選項清單的底部。 不過，您可以將它放到清單中的任意位置；自訂列印選項不一定要最後新增。

當使用者變更自訂選項中選取的選項時，更新預覽列印影像。 呼叫 [**InvalidatePreview**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.printing.printdocument.invalidatepreview) 方法以在預覽列印 UI 中重新繪製影像，如下所示。

``` csharp
async void printDetailedOptions_OptionChanged(PrintTaskOptionDetails sender, PrintTaskOptionChangedEventArgs args)
{
   // Listen for PageContent changes
   string optionId = args.OptionId as string;
   if (string.IsNullOrEmpty(optionId))
   {
         return;
   }

   if (optionId == "PageContent")
   {
         await scenarioPage.Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal, () =>
         {
            printDocument.InvalidatePreview();
         });
   }
}
```

## <a name="related-topics"></a>相關主題

* [列印的設計方針](https://docs.microsoft.com/windows/uwp/devices-sensors/printing-and-scanning)
* [Build 2015 影片：開發 Windows 10 中列印的應用程式](https://channel9.msdn.com/Events/Build/2015/2-94)
* [UWP 列印範例](https://go.microsoft.com/fwlink/p/?LinkId=619984)
