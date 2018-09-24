---
author: PatrickFarley
ms.assetid: 88132B6F-FB50-4B03-BC21-233988746230
title: 自訂預覽列印 UI
description: 本節說明如何在預覽列印 UI 中自訂列印選項和設定。
ms.author: pafarley
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10，uwp 列印
ms.localizationpriority: medium
ms.openlocfilehash: fe4086cc87699083304594eb4ccc8e7bb137b19f
ms.sourcegitcommit: 194ab5aa395226580753869c6b66fce88be83522
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/24/2018
ms.locfileid: "4155561"
---
# <a name="customize-the-print-preview-ui"></a>自訂預覽列印 UI



**重要 API**

-   [**Windows.Graphics.Printing**](https://msdn.microsoft.com/library/windows/apps/BR226489)
-   [**Windows.UI.Xaml.Printing**](https://msdn.microsoft.com/library/windows/apps/BR243325)
-   [**PrintManager**](https://msdn.microsoft.com/library/windows/apps/BR226426)

本節說明如何在預覽列印 UI 中自訂列印選項和設定。 如需列印的詳細資訊，請參閱[從您的 app 進行列印](print-from-your-app.md)。

**提示**：本主題中大部分的範例是以列印範例為基礎。 若要查看完整程式碼，請從 GitHub 的 [Windows-universal-samples 儲存機制](http://go.microsoft.com/fwlink/p/?LinkId=619979)下載[通用 Windows 平台 (UWP) 列印範例](http://go.microsoft.com/fwlink/p/?LinkId=619984)。

 

## <a name="customize-print-options"></a>自訂列印選項

根據預設，預覽列印 UI 會顯示 [**ColorMode**](https://msdn.microsoft.com/library/windows/apps/BR226478)、[**Copies**](https://msdn.microsoft.com/library/windows/apps/BR226479) 和 [**Orientation**](https://msdn.microsoft.com/library/windows/apps/BR226486) 選項。 除了上述選項，還有數個其他常見的印表機選項，讓您可以新增到預覽列印 UI：

-   [**Binding**](https://msdn.microsoft.com/library/windows/apps/BR226476)
-   [**Collation**](https://msdn.microsoft.com/library/windows/apps/BR226477)
-   [**Duplex**](https://msdn.microsoft.com/library/windows/apps/BR226480)
-   [**HolePunch**](https://msdn.microsoft.com/library/windows/apps/BR226481)
-   [**InputBin**](https://msdn.microsoft.com/library/windows/apps/BR226482)
-   [**MediaSize**](https://msdn.microsoft.com/library/windows/apps/BR226483)
-   [**MediaType**](https://msdn.microsoft.com/library/windows/apps/BR226484)
-   [**NUp**](https://msdn.microsoft.com/library/windows/apps/BR226485)
-   [**PrintQuality**](https://msdn.microsoft.com/library/windows/apps/BR226487)
-   [**Staple**](https://msdn.microsoft.com/library/windows/apps/BR226488)

這些選項會在 [**StandardPrintTaskOptions**](https://msdn.microsoft.com/library/windows/apps/BR226475) 類別中定義。 您可以在預覽列印 UI 中顯示的選項清單中新增或移除選項。 您也可以變更選項出現的順序，以及設定對使用者顯示的預設設定。

不過，您使用這個方法所做的修改只會影響預覽列印 UI。 只要點選預覽列印 UI 中的 **\[更多設定\]** 連結，使用者就可以存取印表機支援的所有選項。

**注意**：雖然您的應用程式可以指定要顯示的任何列印選項，但是只有選定的印表機支援的選項才會在預覽列印 UI 中顯示。 列印 UI 不會顯示選定的印表機不支援的選項。

 

### <a name="define-the-options-to-display"></a>定義要顯示的選項

App 的畫面載入時，會登錄列印協定。 該登錄包含定義 [**PrintTaskRequested**](https://msdn.microsoft.com/library/windows/apps/br206597) 事件處理常式。 自訂預覽列印 UI 中所顯示選項的程式碼會新增到 **PrintTaskRequested** 事件處理常式中。

修改 [**PrintTaskRequested**](https://msdn.microsoft.com/library/windows/apps/br206597) 事件處理常式以包含 [**printTask.options**](https://msdn.microsoft.com/library/windows/apps/BR226469) 指示，這些指示可用來設定您要在預覽列印 UI 中顯示的列印設定。對於要顯示自訂列印選項清單的 app 畫面，請覆寫協助程式類別中的 **PrintTaskRequested** 事件處理常式以加入程式碼，這個程式碼會指定列印畫面時要顯示的選項。

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

**重要**：呼叫 [**displayedOptions.clear**](https://msdn.microsoft.com/library/windows/apps/BR226453)() 會移除預覽列印 UI 中的所有列印選項，包括 **\[更多設定\]** 連結。 務必在預覽列印 UI 上附加要顯示的選項。

### <a name="specify-default-options"></a>指定預設選項

您也可以在預覽列印 UI 中設定選項的預設值。 下列這行程式碼來自上一個範例，會設定 [**MediaSize**](https://msdn.microsoft.com/library/windows/apps/BR226483) 選項的預設值。

``` csharp
         // Preset the default value of the printer option
         printTask.Options.MediaSize = PrintMediaSize.NorthAmericaLegal;
```         

## <a name="add-new-print-options"></a>新增列印選項

本節顯示如何建立新的列印選項、定義選項支援的值清單，以及將選項新增至預覽列印。 如同上一節，在 [**PrintTaskRequested**](https://msdn.microsoft.com/library/windows/apps/br206597) 事件處理常式中加入新的列印選項。

首先，取得 [**PrintTaskOptionDetails**](https://msdn.microsoft.com/library/windows/apps/Hh701256) 物件。 此物件用來將新列印選項加入至預覽列印 UI。 然後清除預覽列印 UI 中顯示的選項清單，並且新增當使用者想要從 app 列印時所要顯示的選項。 接著，建立新的列印選項並初始化選項值的清單。 最後，加入新選項並指派 **OptionChanged** 事件的處理常式。

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

當使用者變更自訂選項中選取的選項時，更新預覽列印影像。 呼叫 [**InvalidatePreview**](https://msdn.microsoft.com/library/windows/apps/Hh702146) 方法以在預覽列印 UI 中重新繪製影像，如下所示。

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

* [列印的設計指導方針](https://msdn.microsoft.com/library/windows/apps/Hh868178)
* [//2015 建置影片：開發在 Windows 10 中列印的 app](https://channel9.msdn.com/Events/Build/2015/2-94)
* [UWP 列印範例](http://go.microsoft.com/fwlink/p/?LinkId=619984)
