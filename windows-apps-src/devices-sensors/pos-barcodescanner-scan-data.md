---
title: 取得並了解條碼資料
description: 瞭解如何從 BarcodeScannerReport 物件中的條碼掃描器取得資料，並瞭解其格式和內容。
ms.date: 08/29/2018
ms.topic: article
keywords: windows 10, uwp, 服務點, pos
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 70226b45a50e0d340c3de316902033d5563106d2
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89163282"
---
# <a name="obtain-and-understand-barcode-data"></a>取得並了解條碼資料

設定條碼掃描器之後，您當然需要一種方法來瞭解您掃描的資料。 當您掃描條碼時，會引發 [DataReceived](/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.datareceived) 事件。 [ClaimedBarcodeScanner](/uwp/api/windows.devices.pointofservice.claimedbarcodescanner)應該訂閱此事件。 **DataReceived**事件會傳遞[BarcodeScannerDataReceivedEventArgs](/uwp/api/windows.devices.pointofservice.barcodescannerdatareceivedeventargs)物件，您可以用它來存取條碼資料。

## <a name="subscribe-to-the-datareceived-event"></a>訂閱 DataReceived 事件

**ClaimedBarcodeScanner**之後，請讓它訂閱**DataReceived**事件：

```cs
claimedBarcodeScanner.DataReceived += ClaimedBarcodeScanner_DataReceived;
```

系統會將 **ClaimedBarcodeScanner** 和 **BarcodeScannerDataReceivedEventArgs** 物件傳遞給事件處理常式。 您可以透過這個物件的 [報表](/uwp/api/windows.devices.pointofservice.barcodescannerdatareceivedeventargs.report#Windows_Devices_PointOfService_BarcodeScannerDataReceivedEventArgs_Report) 屬性（類型為 [BarcodeScannerReport](/uwp/api/windows.devices.pointofservice.barcodescannerreport)）來存取條碼資料。

```cs
private async void ClaimedBarcodeScanner_DataReceived(ClaimedBarcodeScanner sender, BarcodeScannerDataReceivedEventArgs args)
{
    // Parse the data
}
```

## <a name="get-the-data"></a>取得資料

**BarcodeScannerReport**之後，您就可以存取和剖析條碼資料。 **BarcodeScannerReport** 有三個屬性：

* [ScanData](/uwp/api/windows.devices.pointofservice.barcodescannerreport.scandata)：完整的原始條碼資料。
* [ScanDataLabel](/uwp/api/windows.devices.pointofservice.barcodescannerreport.scandatalabel)：已解碼的條碼標籤，不包括標頭、總和檢查碼和其他資訊。
* [ScanDataType](/uwp/api/windows.devices.pointofservice.barcodescannerreport.scandatatype)：已解碼的條碼標籤類型。 可能的值是在 [BarcodeSymbologies](/uwp/api/windows.devices.pointofservice.barcodesymbologies) 類別中定義。

如果您想要存取 **ScanDataLabel** 或 **ScanDataType**，您必須先將 [IsDecodeDataEnabled](/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.isdecodedataenabled#Windows_Devices_PointOfService_ClaimedBarcodeScanner_IsDecodeDataEnabled) 設定為 **true**。

```cs
claimedBarcodeScanner.IsDecodeDataEnabled = true;
```

### <a name="get-the-scan-data-type"></a>取得掃描資料類型

取得已解碼的條碼標籤類型相當簡單， &mdash; 只是在**ScanDataType**上呼叫[GetName](/uwp/api/windows.devices.pointofservice.barcodesymbologies.getname) 。

```cs
private string GetSymbology(BarcodeScannerDataReceivedEventArgs args)
{
    return BarcodeSymbologies.GetName(args.Report.ScanDataType);
}
```

### <a name="get-the-scan-data-label"></a>取得掃描資料標籤

若要取得已解碼的條碼標籤，您必須注意幾件事。 只有特定資料類型包含編碼的文字，因此您應該先檢查是否可以將此符號轉換成字串，然後將我們從 **ScanDataLabel** 取得的緩衝區轉換成編碼的 utf-8 字串。

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

### <a name="get-the-raw-scan-data"></a>取得原始掃描資料

為了從條碼取得完整的原始資料，我們只會將從 **ScanData** 取得的緩衝區轉換成字串。

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

一般而言，這些資料是從掃描器提供的格式。 但是會移除訊息標頭和結尾資訊，因為它們不包含應用程式的實用資訊，而且可能是掃描器特定的資訊。

常見的標頭資訊是首碼字元 (例如 STX 字元) 。 一般的結尾資訊是結束字元字元 (例如 ETX 或 CR 字元) 以及掃描器產生的區塊檢查字元（如果有的話）。

如果掃描器傳回了一個表示式的符號字元，則此屬性應該包含一個引號字元 (例如，適用于 UPC 的 **a**) 。 如果標籤中有標籤並由掃描器傳回，它也應該包含檢查數位。  (請注意，視掃描器的設定而定，這兩個條碼字元和檢查數位可能不會出現。 如果有的話，掃描器會傳回它們，但如果不存在，則不會產生或計算這些專案。 ) 

某些貨品可能會標示補充條碼。 此條碼通常會放在主要條碼的右邊，而且是由其他兩個或五個字元的資訊所組成。 如果掃描器讀取同時包含主要和補充條碼的商品，則會將補充字元附加至主要字元，並將結果以一個標籤的形式傳遞至應用程式。  (請注意，掃描器可能會支援啟用或停用補充程式碼讀取的設定。 ) 

某些貨品可能標示了多個標籤，有時稱為 *multisymbol 卷* 標或階層式 *卷*標。 這些條碼通常會垂直排列，而且可能是相同或不同的條碼。 如果掃描器讀取包含多個標籤的商品，每個條碼都會以個別標籤的形式傳遞至應用程式。 這是必要的，因為目前缺少這些條碼類型的標準化。 其中一個無法根據個別的條碼資料來判斷所有變化。 因此，應用程式必須根據傳回的資料來判斷何時已讀取多個標籤條碼。  (請注意，掃描器可能會或可能不支援讀取多個標籤。 ) 

在 **DataReceived** 事件引發至應用程式之前，會先設定這個值。

[!INCLUDE [feedback](./includes/pos-feedback.md)]

## <a name="see-also"></a>另請參閱
* [條碼掃描器](pos-barcodescanner.md)
* [ClaimedBarcodeScanner 類別](/uwp/api/windows.devices.pointofservice.barcodesymbologies.getname)
* [BarcodeScannerDataReceivedEventArgs 類別](/uwp/api/windows.devices.pointofservice.barcodescannerdatareceivedeventargs)
* [BarcodeScannerReport 類別](/uwp/api/windows.devices.pointofservice.barcodescannerreport)
* [BarcodeSymbologies 類別](/uwp/api/windows.devices.pointofservice.barcodesymbologies)