---
title: 使用條碼掃描器碼制
description: 使用 UWP 條碼掃描器 Api 來處理條碼條碼符號表示，而不需要手動設定掃描器。
ms.date: 08/29/2018
ms.topic: article
keywords: windows 10, uwp, 服務點, pos
ms.localizationpriority: medium
ms.openlocfilehash: 96014dfeb0b160d9cb94afef5ba4251b3c8bf8aa
ms.sourcegitcommit: 5d34eb13c7b840c05e5394910a22fa394097dc36
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/28/2020
ms.locfileid: "89054378"
---
# <a name="working-with-symbologies"></a>使用碼制
[條碼碼制](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies)是資料到特定條碼格式的對應。 某些常見的條碼符號表示包括 UPC、程式碼128、QR 代碼等等。  通用 Windows 平臺條碼掃描器 Api 可讓應用程式控制掃描器處理這些條碼符號表示的方式，而不需要手動設定掃描器。 

## <a name="determine-which-symbologies-are-supported"></a>判斷支援的碼制 
因為您的應用程式可能搭配多個製造商的不同條碼掃描器機型使用，您可能想要查詢掃描器以判斷其支援碼制的清單。  如果您的應用程式需要所有掃描器可能都不支援的特定碼制，或者您必須啟用掃描器上手動或以程式設計方式停用的碼制，這非常有用。

若您取得 [BarcodeScanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner) 物件的方式是使用 [BarcodeScanner.FromIdAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.fromidasync)，呼叫 [GetSupportedSymbologiesAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.getsupportedsymbologiesasync#Windows_Devices_PointOfService_BarcodeScanner_GetSupportedSymbologiesAsync) 以取得裝置所支援碼制的清單。

下列範例會取得條碼掃描器的支援條碼符號表示清單，並將它們顯示在文字區塊中：

```cs
private void DisplaySupportedSymbologies(BarcodeScanner barcodeScanner, TextBlock textBlock) 
{
    var supportedSymbologies = await barcodeScanner.GetSupportedSymbologiesAsync();

    foreach (uint item in supportedSymbologies)
    {
        string symbology = BarcodeSymbologies.GetName(item);
        textBlock.Text += (symbology + "\n");
    }
}
```

## <a name="determine-if-a-specific-symbology-is-supported"></a>判斷是否支援特定碼制
若要判斷掃描器是否支援特定的符號，您可以呼叫 [IsSymbologySupportedAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.issymbologysupportedasync#Windows_Devices_PointOfService_BarcodeScanner_IsSymbologySupportedAsync_System_UInt32_)。

下列範例會檢查條碼掃描器是否支援 **Code32** 的條碼：

```cs
bool symbologySupported = await barcodeScanner.IsSymbologySupportedAsync(BarcodeSymbologies.Code32);
```

## <a name="change-which-symbologies-are-recognized"></a>變更可辨識的條碼符號表示
有時候，您可能想要使用條碼掃描器支援的碼制子集。  這特別適合封鎖您不想在您的應用程式中使用的碼制。 例如，為確保使用者掃描正確的條碼，您可以在取得項目 SKU 時限制掃描到 UPC 或 EAN，以及在取得序號時限制掃描到 Code 128。

一旦您知道掃描器支援的碼制，您可以設定要其辨識的碼制。  這可以在您使用[ClaimScannerAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.claimscannerasync#Windows_Devices_PointOfService_BarcodeScanner_ClaimScannerAsync)建立[ClaimedBarcodeScanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner)物件之後完成。 您可以呼叫 [SetActiveSymbologiesAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.setactivesymbologiesasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_SetActiveSymbologiesAsync_Windows_Foundation_Collections_IIterable_System_UInt32__) 以啟用特定組的碼制，而這些從清單省略的碼制已停用。

下列範例會將宣告的條碼掃描器的主動式條碼符號表示設定為 [Code39](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies.code39#Windows_Devices_PointOfService_BarcodeSymbologies_Code39) 和 [Code39Ex](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies.code39ex)：

```cs
private async void SetSymbologies(ClaimedBarcodeScanner claimedBarcodeScanner) 
{
    var symbologies = new List<uint>{ BarcodeSymbologies.Code39, BarcodeSymbologies.Code39Ex };
    await claimedBarcodeScanner.SetActiveSymbologiesAsync(symbologies);
}
```

## <a name="barcode-symbology-attributes"></a>條碼條碼屬性
不同的條碼條碼符號表示可以有不同的屬性，例如支援多個解碼長度、將檢查數位傳送至主機做為原始資料的一部分，以及檢查數位驗證。 使用 [BarcodeSymbologyAttributes](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes) 類別，您可以取得並設定指定 [ClaimedBarcodeScanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner) 和條碼條碼的這些屬性。

您可以使用 [GetSymbologyAttributesAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.getsymbologyattributesasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_GetSymbologyAttributesAsync_System_UInt32_)取得指定之條碼的屬性。 下列程式碼片段會取得適用于 **ClaimedBarcodeScanner**之 Upca 表示的屬性。

```cs
BarcodeSymbologyAttributes barcodeSymbologyAttributes = 
    await claimedBarcodeScanner.GetSymbologyAttributesAsync(BarcodeSymbologies.Upca);
```

當您完成修改屬性並準備設定這些屬性時，您可以呼叫 [SetSymbologyAttributesAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.setsymbologyattributesasync)。 這個方法會傳回 **布林**值，如果已成功設定屬性，則為 **true** 。

```cs
bool success = await claimedBarcodeScanner.SetSymbologyAttributesAsync(
    BarcodeSymbologies.Upca, barcodeSymbologyAttributes);
```

### <a name="restrict-scan-data-by-data-length"></a>依資料長度限制掃描資料
某些碼制的長度可變，例如 Code 39 或 Code 128。  這些條碼符號表示的條碼可位於彼此附近，其中包含特定長度的不同資料。 設定特定長度的資料，可以防止無效的掃描。

設定解碼長度之前，請檢查條碼的表示是否可使用 [IsDecodeLengthSupported](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.isdecodelengthsupported#Windows_Devices_PointOfService_BarcodeSymbologyAttributes_IsDecodeLengthSupported)來支援多個長度。 知道支援之後，您就可以設定[BarcodeSymbologyDecodeLengthKind](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologydecodelengthkind)類型的[DecodeLengthKind](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.decodelengthkind#Windows_Devices_PointOfService_BarcodeSymbologyAttributes_DecodeLengthKind)。 這個屬性可以是下列其中一個值：

* **AnyLength**：解碼任何數位的長度。
* **離散**：解碼 [DecodeLength1](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.decodelength1) 或 [DecodeLength2](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.decodelength2) 單一位元組字元的長度。
* **範圍**：在 **DecodeLength1** 和 **DecodeLength2** 單一位元組字元之間解碼長度。 **DecodeLength1**和**DecodeLength2**的順序並不重要 (可能會比其他) 更高或更低。

最後，您可以設定 **DecodeLength1** 和 **DecodeLength2** 的值，以控制您需要的資料長度。

下列程式碼片段示範如何設定解碼長度：

```cs
private async Task<bool> SetDecodeLength(
    ClaimedBarcodeScanner scanner,
    uint symbology, 
    BarcodeSymbologyDecodeLengthKind kind, 
    uint decodeLength1, 
    uint decodeLength2)
{
    bool success = false;
    BarcodeSymbologyAttributes attributes = await scanner.GetSymbologyAttributesAsync(symbology);

    if (attributes.IsDecodeLengthSupported)
    {
        attributes.DecodeLengthKind = kind;
        attributes.DecodeLength1 = decodeLength1;
        attributes.DecodeLength2 = decodeLength2;
        success = await scanner.SetSymbologyAttributesAsync(symbology, attributes);
    }

    return success;
}
```

### <a name="check-digit-transmission"></a>檢查數位傳輸

您可以在 [條碼] 上設定的另一個屬性是，檢查數位是否會以原始資料的一部分傳送至主機。 設定此選項之前，請確定該符號支援使用 [IsCheckDigitTransmissionSupported](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.ischeckdigittransmissionsupported)進行檢查數位傳輸。 然後，設定是否使用 [IsCheckDigitTransmissionEnabled](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.ischeckdigittransmissionenabled)啟用檢查數位傳輸。

下列程式碼片段將示範設定檢查位數傳輸：

```cs
private async Task<bool> SetCheckDigitTransmission(ClaimedBarcodeScanner scanner, uint symbology, bool isEnabled)
{
    bool success = false;
    BarcodeSymbologyAttributes attributes = await scanner.GetSymbologyAttributesAsync(symbology);

    if (attributes.IsCheckDigitTransmissionSupported)
    {
        attributes.IsCheckDigitTransmissionEnabled = isEnabled;
        success = await scanner.SetSymbologyAttributesAsync(symbology, attributes);
    }

    return success;
}
```

### <a name="check-digit-validation"></a>檢查數位驗證

您也可以設定是否要驗證條碼檢查數位。 設定此選項之前，請確定該符號支援使用 [IsCheckDigitValidationSupported](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.ischeckdigitvalidationsupported)進行檢查數位驗證。 然後，設定是否使用 [IsCheckDigitValidationEnabled](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.ischeckdigitvalidationenabled)啟用檢查數位驗證。

下列程式碼片段將示範設定檢查位數驗證：

```cs
private async Task<bool> SetCheckDigitValidation(ClaimedBarcodeScanner scanner, uint symbology, bool isEnabled)
{
    bool success = false;
    BarcodeSymbologyAttributes attributes = await scanner.GetSymbologyAttributesAsync(symbology);

    if (attributes.IsCheckDigitValidationSupported)
    {
        attributes.IsCheckDigitValidationEnabled = isEnabled;
        success = await scanner.SetSymbologyAttributesAsync(symbology, attributes);
    }

    return success;
}
```

[!INCLUDE [feedback](./includes/pos-feedback.md)]

## <a name="see-also"></a>另請參閱

* [條碼掃描器](pos-barcodescanner.md)
* [BarcodeSymbologies 類別](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies)
* [BarcodeScanner 類別](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner)
* [ClaimedBarcodeScanner 類別](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner)
* [BarcodeSymbologyAttributes 類別](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes)
* [BarcodeSymbologyDecodeLengthKind 列舉](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologydecodelengthkind)