---
description: 本文說明如何使用剪貼簿在通用 Windows 平台 (UWP) App 中支援複製和貼上。
title: 複製和貼上
ms.assetid: E882DC15-E12D-4420-B49D-F495BB484BEE
author: awkoren
---
#複製和貼上

\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


本文說明如何使用剪貼簿在通用 Windows 平台 (UWP) App 中支援複製和貼上。 複製和貼上是在 App 間 (或是 App 內) 交換資料的傳統方式，而且幾乎每個 App 在某種程度上都能支援剪貼簿作業。

## 檢查內建式剪貼簿支援


在很多情況下，您不需要撰寫程式碼就可以支援剪貼簿作業。 許多您可用來建立 app 的預設 XAML 控制項已經支援剪貼簿作業。 如需可用控制項的詳細資訊，請參閱[控制項清單][ControlsList]。

## 開始設定

首先，在您的 App 中包含 [**Windows.ApplicationModel.DataTransfer**][DataTransfer] 命名空間。 然後，新增 [**DataPackage**][DataPackage] 物件的執行個體。 這個物件包含使用者想要複製的資料以及您想要包含的任何屬性 (例如描述)。

<!-- For some reason, the snippets in this file are all inline in the WDCML topic. Suggest moving to VS project with rest of snippets. -->
```cs
DataPackage dataPackage = new DataPackage();
```

## 複製和剪下

複製和剪下 (也稱為移動) 的作用幾乎相同。 選擇您想使用 [**DataPackage.RequestedOperation**][RequestedOperation] 屬性的作業。

```cs
// copy 
dataPackage.RequestedOperation = DataPackageOperation.Copy;
// or cut
dataPackage.RequestedOperation = DataPackageOperation.Move;
```

接下來，您可以將使用者選取的資料新增至 [**DataPackage**][DataPackage] 物件。 如果 **DataPackage** 類別支援此資料，就可以使用 **DataPackage** 物件的其中一種對應方法。 以下是新增文字的方式：

```cs
dataPackage.SetText("Hello World!");
```

最後一個步驟是呼叫靜態 [**Clipboard.SetContent**][SetContent] 方法，將 [**DataPackage**][DataPackage] 新增到剪貼簿。

```cs
Clipboard.SetContent(dataPackage);
```
## 貼上

若要取得剪貼簿的內容，請呼叫靜態 [**Clipboard.GetContent**[GetContent] 方法。 這個方法會傳回包含內容的 [**DataPackageView**][DataPackageView]。 這個物件與 [**DataPackage**][DataPackage] 物件幾乎完全一樣，不同的是它的內容是唯讀的。 使用該物件，就可以使用 [**AvailableFormats**][AvailableFormats] 或 [**Contains**][Contains] 方法來識別可用的格式。 接著，再呼叫對應的 **DataPackageView** 方法來取得資料。

```cs
DataPackageView dataPackageView = Clipboard.GetContent();
if (dataPackageView.Contains(StandardDataFormats.Text))
{
    string text = await dataPackageView.GetTextAsync();
    // To output the text from this example, you need a TextBlock control
    TextOutput.Text = "Clipboard now contains: " + text;
}
```

## 追蹤剪貼簿的變更

除了複製和貼上命令，您可能也想要追蹤剪貼簿的變更。 處理剪貼簿的 [**Clipboard.ContentChanged**][ContentChanged] 事件來執行這個動作。

```cs
Clipboard.ContentChanged += (s, e) => 
{
    DataPackageView dataPackageView = Clipboard.GetContent();
    if (dataPackageView.Contains(StandardDataFormats.Text))
    {
        string text = await dataPackageView.GetTextAsync();
        // To output the text from this example, you need a TextBlock control
        TextOutput.Text = "Clipboard now contains: " + text;
    }
}
```

<!-- LINKS --> 
[DataTransfer]: https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.datatransfer.aspx 
[DataPackage]: https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.datatransfer.datapackage.aspx 
[DataPackageView]: https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.datatransfer.datapackageview.aspx
[DataPackagePropertySet]: https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.datatransfer.datapackagepropertyset.aspx 
[DataRequest]: https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.datatransfer.datarequest.aspx 
[DataRequested]: https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.datatransfer.datatransfermanager.datarequested.aspx 
[FailWithDisplayText]: https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.datatransfer.datarequest.failwithdisplaytext.aspx
[ShowShareUi]: https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.datatransfer.datatransfermanager.showshareui.aspx
[RequestedOperation]: https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.datatransfer.datapackage.requestedoperation.aspx 
[ControlsList]: https://msdn.microsoft.com/en-us/library/windows/apps/xaml/mt185406.aspx 
[SetContent]: https://msdn.microsoft.com/en-us/library/windows/apps/xaml/windows.applicationmodel.datatransfer.clipboard.setcontent.aspx 
[GetContent]: https://msdn.microsoft.com/en-us/library/windows/apps/xaml/windows.applicationmodel.datatransfer.clipboard.getcontent.aspx
[AvailableFormats]: https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.datatransfer.datapackageview.availableformats.aspx 
[包含]: https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.datatransfer.datapackageview.contains.aspx
[ContentChanged]: https://msdn.microsoft.com/en-us/library/windows/apps/xaml/windows.applicationmodel.datatransfer.clipboard.contentchanged.aspx 



<!--HONumber=May16_HO2-->


