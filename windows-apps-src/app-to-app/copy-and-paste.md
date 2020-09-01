---
description: 本文說明如何使用剪貼簿在通用 Windows 平台 (UWP) App 中支援複製和貼上。
title: 複製和貼上
ms.assetid: E882DC15-E12D-4420-B49D-F495BB484BEE
ms.date: 12/19/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 117c09eb2dd0f24a330060da9b9766cb33e90d58
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175762"
---
# <a name="copy-and-paste"></a>複製和貼上

本文說明如何使用剪貼簿在通用 Windows 平台 (UWP) App 中支援複製和貼上。 複製和貼上是在 App 間 (或是 App 內) 交換資料的傳統方式，而且幾乎每個 App 在某種程度上都能支援剪貼簿作業。 如需示範數種不同複製和貼上案例的完整程式碼範例，請參閱 [剪貼簿範例](https://github.com/microsoft/Windows-universal-samples/tree/master/Samples/Clipboard)。

## <a name="check-for-built-in-clipboard-support"></a>檢查內建式剪貼簿支援

在很多情況下，您不需要撰寫程式碼就可以支援剪貼簿作業。 許多您可用來建立 app 的預設 XAML 控制項已經支援剪貼簿作業。 

## <a name="get-set-up"></a>開始設定

首先，在您的 App 中包含 [**Windows.ApplicationModel.DataTransfer**](/uwp/api/Windows.ApplicationModel.DataTransfer) 命名空間。 接著，新增 [**DataPackage**](/uwp/api/Windows.ApplicationModel.DataTransfer.DataPackage) 物件的執行個體。 這個物件包含使用者想要複製的資料以及您想要包含的任何屬性 (例如描述)。

```cs
DataPackage dataPackage = new DataPackage();
```

<!-- AuthenticateAsync-->

## <a name="copy-and-cut"></a>複製和剪下

複製和剪下 (也稱為*移動*) 的作用幾乎相同。 使用 [**RequestedOperation**](/uwp/api/windows.applicationmodel.datatransfer.datapackage.requestedoperation) 屬性選擇您想執行的作業。

```cs
// copy 
dataPackage.RequestedOperation = DataPackageOperation.Copy;
// or cut
dataPackage.RequestedOperation = DataPackageOperation.Move;
```

## <a name="set-the-copied-content"></a>設定複製的內容

接下來，您可以將使用者選取的資料新增至 [**DataPackage**](/uwp/api/Windows.ApplicationModel.DataTransfer.DataPackage) 物件。 如果**DataPackage**類別支援這項資料，您可以使用**DataPackage**物件的其中一個對應[方法](/uwp/api/windows.applicationmodel.datatransfer.datapackage#methods)。 以下說明如何使用 [**SetText**](/uwp/api/windows.applicationmodel.datatransfer.datapackage.settext) 方法來新增文字：

```cs
dataPackage.SetText("Hello World!");
```

最後一個步驟是呼叫靜態 [**SetContent**](/uwp/api/windows.applicationmodel.datatransfer.clipboard.setcontent) 方法，將 [**DataPackage**](/uwp/api/Windows.ApplicationModel.DataTransfer.DataPackage) 新增到剪貼簿。

```cs
Clipboard.SetContent(dataPackage);
```

## <a name="paste"></a>貼上

若要取得剪貼簿的內容，請呼叫靜態 [**GetContent**](/uwp/api/windows.applicationmodel.datatransfer.clipboard.getcontent) 方法。 這個方法會傳回包含內容的 [**DataPackageView**](/uwp/api/Windows.ApplicationModel.DataTransfer.DataPackageView)。 這個物件與 [**DataPackage**](/uwp/api/Windows.ApplicationModel.DataTransfer.DataPackage) 物件幾乎完全一樣，不同的是它的內容是唯讀的。 有了該物件，您便可以使用 [**AvailableFormats**](/uwp/api/windows.applicationmodel.datatransfer.datapackageview.availableformats) 或 [**Contains**](/uwp/api/windows.applicationmodel.datatransfer.datapackageview.contains) 方法來識別可用的格式。 接著，您可以呼叫對應的 [**DataPackageView**](/uwp/api/Windows.ApplicationModel.DataTransfer.DataPackageView) 方法來取得資料。

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

除了複製和貼上命令，您可能也想要追蹤剪貼簿的變更。 處理剪貼簿的 [**ContentChanged**](/uwp/api/windows.applicationmodel.datatransfer.clipboard.contentchanged) 事件即可執行這個動作。

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

* [剪貼簿範例](https://github.com/microsoft/Windows-universal-samples/tree/master/Samples/Clipboard)
* [應用程式間通訊](index.md)
* [DataTransfer](/uwp/api/windows.applicationmodel.datatransfer)
* [DataPackage](/uwp/api/windows.applicationmodel.datatransfer.datapackage)
* [DataPackageView](/uwp/api/windows.applicationmodel.datatransfer.datapackageview)
* [DataPackagePropertySet]( /uwp/api/Windows.ApplicationModel.DataTransfer.DataPackagePropertySet)
* [DataRequest](/uwp/api/windows.applicationmodel.datatransfer.datarequest) 
* [DataRequested]( /uwp/api/Windows.ApplicationModel.DataTransfer.DataTransferManager)
* [FailWithDisplayText](/uwp/api/windows.applicationmodel.datatransfer.datarequest.failwithdisplaytext)
* [ShowShareUi](/uwp/api/windows.applicationmodel.datatransfer.datatransfermanager.showshareui)
* [RequestedOperation](/uwp/api/windows.applicationmodel.datatransfer.datapackage.requestedoperation) 
* [ControlsList](../design/controls-and-patterns/index.md)
* [SetContent](/uwp/api/windows.applicationmodel.datatransfer.clipboard.setcontent)
* [GetContent](/uwp/api/windows.applicationmodel.datatransfer.clipboard.getcontent)
* [AvailableFormats](/uwp/api/windows.applicationmodel.datatransfer.datapackageview.availableformats)
* [Contains](/uwp/api/windows.applicationmodel.datatransfer.datapackageview.contains)
* [ContentChanged](/uwp/api/windows.applicationmodel.datatransfer.clipboard.contentchanged)