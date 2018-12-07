---
description: 本文說明如何使用剪貼簿在通用 Windows 平台 (UWP) App 中支援複製和貼上。
title: 複製並貼上
ms.assetid: E882DC15-E12D-4420-B49D-F495BB484BEE
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: ba498a9bc6541f45f01e74b1ea90f4f9a1bfde90
ms.sourcegitcommit: a3dc929858415b933943bba5aa7487ffa721899f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/07/2018
ms.locfileid: "8799987"
---
# <a name="copy-and-paste"></a>複製並貼上

本文說明如何使用剪貼簿在通用 Windows 平台 (UWP) App 中支援複製和貼上。 複製和貼上是在 App 間 (或是 App 內) 交換資料的傳統方式，而且幾乎每個 App 在某種程度上都能支援剪貼簿作業。

## <a name="check-for-built-in-clipboard-support"></a>檢查內建式剪貼簿支援

在很多情況下，您不需要撰寫程式碼就可以支援剪貼簿作業。 許多您可用來建立 app 的預設 XAML 控制項已經支援剪貼簿作業。 

## <a name="get-set-up"></a>開始設定

首先，在您的 App 中包含 [**Windows.ApplicationModel.DataTransfer**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer) 命名空間。 接著，新增 [**DataPackage**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.DataPackage) 物件的執行個體。 這個物件包含使用者想要複製的資料以及您想要包含的任何屬性 (例如描述)。

<!-- For some reason, the snippets in this file are all inline in the WDCML topic. Suggest moving to VS project with rest of snippets. -->
```cs
DataPackage dataPackage = new DataPackage();
```

<!-- AuthenticateAsync-->

## <a name="copy-and-cut"></a>複製和剪下

複製和剪下 (也稱為*移動*) 的作用幾乎相同。 使用 [**RequestedOperation**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.DataPackage.RequestedOperation) 屬性選擇您想執行的作業。

```cs
// copy 
dataPackage.RequestedOperation = DataPackageOperation.Copy;
// or cut
dataPackage.RequestedOperation = DataPackageOperation.Move;
```
## <a name="drag-and-drop"></a>拖放

接下來，您可以將使用者選取的資料新增至 [**DataPackage**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.DataPackage) 物件。 如果 **DataPackage** 類別支援此資料，就可以使用 **DataPackage** 物件的其中一種對應方法。 以下是新增文字的方式：

```cs
dataPackage.SetText("Hello World!");
```

最後一個步驟是呼叫靜態 [**SetContent**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.Clipboard.SetContent(Windows.ApplicationModel.DataTransfer.DataPackage)) 方法，將 [**DataPackage**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.DataPackage) 新增到剪貼簿。

```cs
Clipboard.SetContent(dataPackage);
```
## <a name="paste"></a>貼上

若要取得剪貼簿的內容，請呼叫靜態 [**GetContent**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.Clipboard.GetContent) 方法。 這個方法會傳回包含內容的 [**DataPackageView**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.DataPackageView)。 這個物件與 [**DataPackage**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.DataPackage) 物件幾乎完全一樣，不同的是它的內容是唯讀的。 有了該物件，您便可以使用 [**AvailableFormats**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.DataPackageView.AvailableFormats) 或 [**Contains**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.DataPackageView.Contains(System.String)) 方法來識別可用的格式。 接著，您可以呼叫對應的 [**DataPackageView**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.DataPackageView) 方法來取得資料。

```cs
async void OutputClipboardText()
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

## <a name="track-changes-to-the-clipboard"></a>追蹤剪貼簿的變更

除了複製和貼上命令，您可能也想要追蹤剪貼簿的變更。 處理剪貼簿的 [**ContentChanged**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.Clipboard.ContentChanged) 事件即可執行這個動作。

```cs
Clipboard.ContentChanged += async (s, e) => 
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

## <a name="see-also"></a>另請參閱

* [App 間通訊](index.md)
* [DataTransfer](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.aspx)
* [DataPackage](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.datapackage.aspx)
* [DataPackageView](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.datapackageview.aspx)
* [DataPackagePropertySet]( https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.datapackagepropertyset.aspx)
* [DataRequest](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.datarequest.aspx) 
* [DataRequested]( https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.datatransfermanager.datarequested.aspx)
* [FailWithDisplayText](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.datarequest.failwithdisplaytext.aspx)
* [ShowShareUi](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.datatransfermanager.showshareui.aspx)
* [RequestedOperation](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.datapackage.requestedoperation.aspx) 
* [ControlsList](https://msdn.microsoft.com/library/windows/apps/xaml/mt185406.aspx)
* [SetContent](https://msdn.microsoft.com/library/windows/apps/xaml/windows.applicationmodel.datatransfer.clipboard.setcontent.aspx)
* [GetContent](https://msdn.microsoft.com/library/windows/apps/xaml/windows.applicationmodel.datatransfer.clipboard.getcontent.aspx)
* [AvailableFormats](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.datapackageview.availableformats.aspx)
* [包含](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.datapackageview.contains.aspx)
* [ContentChanged](https://msdn.microsoft.com/library/windows/apps/xaml/windows.applicationmodel.datatransfer.clipboard.contentchanged.aspx)

