---
title: 取得並了解條碼資料
description: 了解如何取得和解譯您掃描的條碼資料。
ms.date: 08/29/2018
ms.topic: article
keywords: windows 10, uwp, 服務點, pos
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: ece246ffd369ee21c089598f07b2566424757f55
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57605963"
---
# <a name="obtain-and-understand-barcode-data"></a>取得並了解條碼資料

一旦您已設定您的條碼掃描器，您必須了解您掃描的資料的方式。 當您掃描的條碼[DataReceived](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.datareceived)就會引發事件。 [ClaimedBarcodeScanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner)應該訂閱此事件。 **DataReceived**事件會傳遞[BarcodeScannerDataReceivedEventArgs](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerdatareceivedeventargs)物件，可用來存取條碼資料。

## <a name="subscribe-to-the-datareceived-event"></a>訂閱 DataReceived 事件

一旦**ClaimedBarcodeScanner**，讓它訂閱**DataReceived**事件：

```cs
claimedBarcodeScanner.DataReceived += ClaimedBarcodeScanner_DataReceived;
```

將傳遞的事件處理常式**ClaimedBarcodeScanner**並**BarcodeScannerDataReceivedEventArgs**物件。 您可以透過此物件來存取條碼資料[報表](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerdatareceivedeventargs.report#Windows_Devices_PointOfService_BarcodeScannerDataReceivedEventArgs_Report)屬性，這是型別的[BarcodeScannerReport](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerreport)。

```cs
private async void ClaimedBarcodeScanner_DataReceived(ClaimedBarcodeScanner sender, BarcodeScannerDataReceivedEventArgs args)
{
    // Parse the data
}
```

## <a name="get-the-data"></a>取得資料

一旦**BarcodeScannerReport**，您可以存取並剖析條碼資料。 **BarcodeScannerReport**有三個屬性：

* [ScanData](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerreport.scandata):完整的未經處理的條碼資料。
* [ScanDataLabel](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerreport.scandatalabel):已解碼的條碼標籤，不包括標頭、 總和檢查碼和其他的其他資訊。
* [ScanDataType](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerreport.scandatatype):已解碼的條碼標籤型別。 可能的值會定義於[BarcodeSymbologies](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies)類別。

如果您想要存取其中一個**ScanDataLabel**或**ScanDataType**，您必須先設定[IsDecodeDataEnabled](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.isdecodedataenabled#Windows_Devices_PointOfService_ClaimedBarcodeScanner_IsDecodeDataEnabled)至**true**。

```cs
claimedBarcodeScanner.IsDecodeDataEnabled = true;
```

### <a name="get-the-scan-data-type"></a>取得掃描的資料類型

取得已解碼的條碼標籤型別是相當繁瑣&mdash;只要呼叫[GetName](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies.getname)上**ScanDataType**。

```cs
private string GetSymbology(BarcodeScannerDataReceivedEventArgs args)
{
    return BarcodeSymbologies.GetName(args.Report.ScanDataType);
}
```

### <a name="get-the-scan-data-label"></a>取得掃描的資料標籤

若要取得已解碼的條碼標籤，有幾件事，您一定要留意。 只有特定資料類型會包含已編碼的文字，因此您應該先檢查是否象徵意義可以轉換成字串，，，然後將轉換我們從取得緩衝區**ScanDataLabel**編碼的 utf-8 字串。

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

### <a name="get-the-raw-scan-data"></a>取得未經處理的掃描的資料

若要取得完整的未經處理資料，從條碼，我們只要將轉換我們從取得緩衝區**ScanData**轉換為字串。

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

這些資料是，在一般情況下，傳遞從掃描器的格式。 訊息標頭和結尾資訊會移除，不過，因為它們不包含應用程式的實用資訊，可能會隨著掃描程式而不同。

常見的標頭資訊是前置詞字元 （例如 STX 字元）。 常見的結尾資訊是結束字元的字元 （例如 ETX 或 CR 字元） 和區塊檢查字元，如果其中一個產生的掃描器。

這個屬性應該包含的象徵意義的字元，如果其中一個由掃描器 (例如**A**的 UPC 的)。 它是否出現在標籤中，則也應該包括檢查位數，並傳回由掃描器。 （請注意，象徵意義的字元和檢查數字可能會或可能不存在，掃描器設定而定。 如果掃描器也會傳回它們存在，但無法產生或計算它們，如果它們不存在。)

某些商品可能已標記與補充的條碼。 此條碼通常會位於主要的條碼，右邊，並且包含其他兩個或五個字元的資訊。 如果掃描器讀取商品，包含主要和補充的條碼，補充字元會附加到主要的字元，而且結果會傳遞到應用程式做為一個標籤。 （請注意，掃描器可能支援的組態，啟用或停用的補充程式碼讀取）。

某些商品可能會標記為具有多個標籤，有時也稱為*multisymbol 標籤*或是*分層標籤*。 這些條碼通常是垂直排列，而且可能是相同或不同的象徵意義。 如果掃描器可讀取包含多個標籤的商品，每個條碼會傳遞至應用程式做為個別的標籤。 這是因為目前缺乏標準化這些條碼類型的必要。 一個不能夠判斷個別的條碼資料為基礎的所有變化。 因此，應用程式必須判斷當多個標籤條碼讀取根據傳回的資料。 （請注意，掃描器可能或可能不支援讀取多個標籤）。

此值設定為之前**DataReceived**對應用程式引發的事件。

[!INCLUDE [feedback](./includes/pos-feedback.md)]

## <a name="see-also"></a>請參閱
* [條碼掃描器](pos-barcodescanner.md)
* [ClaimedBarcodeScanner 類別](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies.getname)
* [BarcodeScannerDataReceivedEventArgs 類別](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerdatareceivedeventargs)
* [BarcodeScannerReport 類別](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerreport)
* [BarcodeSymbologies 類別](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies)
