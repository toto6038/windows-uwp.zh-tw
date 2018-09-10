---
author: eliotcowley
title: 取得，並了解條碼資料
description: 了解如何取得和您掃描條碼資料解譯。
ms.author: elcowle
ms.date: 08/29/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 服務點, pos
ms.localizationpriority: medium
ms.openlocfilehash: 0992ea54092063ba53f23871599905e58f1b456e
ms.sourcegitcommit: f5cf806a595969ecbb018c3f7eea86c7a34940f6
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2018
ms.locfileid: "3825122"
---
# <a name="obtain-and-understand-barcode-data"></a>取得，並了解條碼資料

一旦您已經設定好您的條碼掃描器，您文字串需要了解資料掃描的一種。 當您掃描條碼時，會引發[DataReceived](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.datareceived)事件。 這個事件[Claimscannerasyc](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner)應該訂閱。 **DataReceived**事件會傳遞的[BarcodeScannerDataReceivedEventArgs](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerdatareceivedeventargs)物件，您可以用來存取的條碼資料。

## <a name="subscribe-to-the-datareceived-event"></a>訂閱 DataReceived 事件

一旦您有**Claimscannerasyc**時，會有其訂閱**DataReceived**事件：

```cs
claimedBarcodeScanner.DataReceived += ClaimedBarcodeScanner_DataReceived;
```

**Claimscannerasyc**和**BarcodeScannerDataReceivedEventArgs**物件，將會被傳遞的事件處理常式。 您可以透過此物件的[報告](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerdatareceivedeventargs.report#Windows_Devices_PointOfService_BarcodeScannerDataReceivedEventArgs_Report)屬性的類型[BarcodeScannerReport](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerreport)存取的條碼資料。

```cs
private async void ClaimedBarcodeScanner_DataReceived(ClaimedBarcodeScanner sender, BarcodeScannerDataReceivedEventArgs args)
{
    // Parse the data
}
```

## <a name="get-the-data"></a>取得資料

一旦您有**BarcodeScannerReport**時，您可以存取及剖析條碼資料。 **BarcodeScannerReport**有三個屬性：

* [ScanData](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerreport.scandata)： 完整、 原始條碼資料。
* [ScanDataLabel](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerreport.scandatalabel)： 解碼的條碼標籤，其中還不包括標頭、 總和檢查碼，與其他資訊。
* [ScanDataType](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerreport.scandatatype)： 解碼的條碼標籤類型。 可能的值是[BarcodeSymbologies](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies)類別中定義。

如果您想要存取**ScanDataLabel**或**ScanDataType**，您必須先設定[IsDecodeDataEnabled](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.isdecodedataenabled#Windows_Devices_PointOfService_ClaimedBarcodeScanner_IsDecodeDataEnabled)設**為 true**。

```cs
claimedBarcodeScanner.IsDecodeDataEnabled = true;
```

### <a name="get-the-scan-data-type"></a>取得掃描的資料類型

取得解碼的條碼標籤類型，是相當簡單&mdash;我們只需呼叫[GetName](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies.getname) **ScanDataType**上。

```cs
private string GetSymbology(BarcodeScannerDataReceivedEventArgs args)
{
    return BarcodeSymbologies.GetName(args.Report.ScanDataType);
}
```

### <a name="get-the-scan-data-label"></a>取得掃描資料標籤

若要取得的解碼的條碼標籤，有您需要注意的幾件事。 只有特定資料類型包含編碼的文字，因此您應該先檢查碼制，是否可以轉換為字串，，然後將轉換我們前往**ScanDataLabel**從 utf-8 編碼的字串的緩衝區。

```cs
private string GetDataLabel(BarcodeScannerDataReceivedEventArgs args)
{
    uint scanDataType = args.Report.ScanDataType;

    // Only certain data types contain encoded text.
    // To keep this simple, we'll just decode a few of them.
    if (args.Report.ScanDataLabel == null)
    {
        return "No data";
    }

    // This is not an exhaustive list of symbologies that can be converted to a string.
    else if (scanDataType == BarcodeSymbologies.Upca ||
        scanDataType == BarcodeSymbologies.UpcaAdd2 ||
        scanDataType == BarcodeSymbologies.UpcaAdd5 ||
        scanDataType == BarcodeSymbologies.Upce ||
        scanDataType == BarcodeSymbologies.UpceAdd2 ||
        scanDataType == BarcodeSymbologies.UpceAdd5 ||
        scanDataType == BarcodeSymbologies.Ean8 ||
        scanDataType == BarcodeSymbologies.TfStd)
    {
        // The UPC, EAN8, and 2 of 5 families encode the digits 0..9
        // which are then sent to the app in a UTF8 string (like "01234").
        return CryptographicBuffer.ConvertBinaryToString(BinaryStringEncoding.Utf8, args.Report.ScanDataLabel);
    }

    // Some other symbologies (typically 2-D symbologies) contain binary data that
    // should not be converted to text.
    else
    {
        return "Decoded data unavailable.";
    }
}
```

### <a name="get-the-raw-scan-data"></a>取得原始的掃描資料

若要取得完整，原始從條碼，我們只需轉換的資料緩衝區我們取得從**ScanData**為字串。

```cs
private string GetRawData(BarcodeScannerDataReceivedEventArgs args)
{
    // Get the full, raw barcode data.
    if (args.Report.ScanData == null)
    {
        return "No data";
    }

    // Just to show that we have the raw data, we'll print the value of the bytes.
    else
    {
        return CryptographicBuffer.ConvertBinaryToString(BinaryStringEncoding.Utf8, args.Report.ScanData);
    }
}
```

這些資料是，在一般情況下，格式為從掃描器。 郵件標頭和預告片資訊會移除，不過，因為它們不包含應用程式的實用資訊，且很可能會掃描器專屬。

常見的標頭資訊是一個前置詞字元 （例如 STX 字元）。 一般的預告片資訊是結束點字元 （例如 ETX 或 CR 字元） 和區塊核取字元如果其中一個由掃描器所產生。

如果其中一個傳回的掃描器，這個屬性應包含的碼制字元 (例如， **A**適用於 UPC A)。 它是否出現在標籤中，則也應該包含檢查數字，並傳回的掃描器。 （請注意，同時碼制字元及檢查數字可能或可能不會出現，掃描器設定而定。 如果，掃描器會傳回它們記憶，，但將不產生或計算它們是否不存在。)

某些商品可能會使用補充條碼標記。 這個條碼右側的主要的條碼，通常會放置，且都包含其他兩個或五個字元的資訊。 如果掃描器讀取商品，其中包含主要與補充條碼、 補充的字元會附加到主要的字元，且結果都會傳送到應用程式成為一個標籤。 （請注意，掃描器可能會支援的設定，啟用或停用閱讀的補充代碼）。

某些商品可能會有多個標籤，有時也稱為*multisymbol 標籤*或*階層式的標籤*標記。 這些條碼會通常會垂直排列，且可能相同或不同的碼制。 如果掃描器讀取商品包含多個標籤，每個條碼都會傳送到個別的標籤的應用程式。 這是因為目前的這些條碼類型標準化缺少必要。 其中一個是不可以判斷根據個別條碼資料，而所有的變化。 因此，應用程式必須以判斷當多個標籤條碼已讀取根據傳回的資料。 （請注意，掃描器可能，或可能不支援多個標籤中的讀取）。

在**DataReceived**引發事件後的應用程式之前設定此值。

[!INCLUDE [feedback](./includes/pos-feedback.md)]

## <a name="see-also"></a>請參閱
* [條碼掃描器](pos-barcodescanner.md)
* [Claimscannerasyc 類別](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies.getname)
* [BarcodeScannerDataReceivedEventArgs 類別](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerdatareceivedeventargs)
* [BarcodeScannerReport 類別](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerreport)
* [BarcodeSymbologies 類別](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies)