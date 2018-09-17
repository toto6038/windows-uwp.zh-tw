---
author: TerryWarwick
title: 使用條碼掃描器碼制
description: 本文內容涵蓋有關條碼掃描器碼制的相關資訊。
ms.author: jken
ms.date: 08/29/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 服務點, pos
ms.localizationpriority: medium
ms.openlocfilehash: 8bd1dffe4da7b3725ef7716fe9cf28bdf8eaf34f
ms.sourcegitcommit: 9e2c34a5ed3134aeca7eb9490f05b20eb9a3e5df
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/17/2018
ms.locfileid: "3987497"
---
# <a name="working-with-symbologies"></a>使用碼制
[條碼碼制](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies)是資料到特定條碼格式的對應。 一些常見的碼制包含 UPC、 Code 128、 QR 代碼等等。  通用 Windows 平台條碼掃描器 Api 可讓應用程式控制掃描器如何處理這些碼制，而不需要手動設定掃描器。 

## <a name="determine-which-symbologies-are-supported"></a>判斷支援的碼制 
因為您的應用程式可能搭配多個製造商的不同條碼掃描器機型使用，您可能想要查詢掃描器以判斷其支援碼制的清單。  如果您的應用程式需要所有掃描器可能都不支援的特定碼制，或者您必須啟用掃描器上手動或以程式設計方式停用的碼制，這非常有用。

若您取得 [BarcodeScanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner) 物件的方式是使用 [BarcodeScanner.FromIdAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.fromidasync)，呼叫 [GetSupportedSymbologiesAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.getsupportedsymbologiesasync#Windows_Devices_PointOfService_BarcodeScanner_GetSupportedSymbologiesAsync) 以取得裝置所支援碼制的清單。

下列範例會取得的條碼掃描器支援的碼制的清單，並顯示這些文字區塊中：

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
若要判斷掃描器是否支援特定碼制，您可以呼叫[IsSymbologySupportedAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.issymbologysupportedasync#Windows_Devices_PointOfService_BarcodeScanner_IsSymbologySupportedAsync_System_UInt32_)。

下列範例會檢查條碼掃描器是否支援**Code32**碼制︰

```cs
bool symbologySupported = await barcodeScanner.IsSymbologySupportedAsync(BarcodeSymbologies.Code32);
```

## <a name="change-which-symbologies-are-recognized"></a>變更所辨識的碼制
有時候，您可能想要使用條碼掃描器支援的碼制子集。  這特別適合封鎖您不想在您的應用程式中使用的碼制。 例如，為確保使用者掃描正確的條碼，您可以在取得項目 SKU 時限制掃描到 UPC 或 EAN，以及在取得序號時限制掃描到 Code 128。

一旦您知道掃描器支援的碼制，您可以設定要其辨識的碼制。  這可以在建立[Claimscannerasyc](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner)物件，使用[ClaimScannerAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.claimscannerasync#Windows_Devices_PointOfService_BarcodeScanner_ClaimScannerAsync)之後完成。 您可以呼叫 [SetActiveSymbologiesAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.setactivesymbologiesasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_SetActiveSymbologiesAsync_Windows_Foundation_Collections_IIterable_System_UInt32__) 以啟用特定組的碼制，而這些從清單省略的碼制已停用。

下列範例會設定作用中的已宣告的條碼掃描器碼制[Code39](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies.code39#Windows_Devices_PointOfService_BarcodeSymbologies_Code39)和[Code39Ex](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies.code39ex):

```cs
private async void SetSymbologies(ClaimedBarcodeScanner claimedBarcodeScanner) 
{
    var symbologies = new List<uint>{ BarcodeSymbologies.Code39, BarcodeSymbologies.Code39Ex };
    await claimedBarcodeScanner.SetActiveSymbologiesAsync(symbologies);
}
```

## <a name="barcode-symbology-attributes"></a>條碼碼制屬性
不同條碼碼制可以有不同的屬性，例如支援多個解碼長度，傳輸到主機檢查數字做為一部分的原始的資料，並檢查數字驗證。 使用[BarcodeSymbologyAttributes](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes)類別中，您可以取得並設定這些屬性，指定[Claimscannerasyc](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner)和條碼碼制。

您可以取得與[GetSymbologyAttributesAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.getsymbologyattributesasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_GetSymbologyAttributesAsync_System_UInt32_)的特定碼制的屬性。 下列程式碼片段會取得**Claimscannerasyc**Upca 碼制的屬性。

```cs
BarcodeSymbologyAttributes barcodeSymbologyAttributes = 
    await claimedBarcodeScanner.GetSymbologyAttributesAsync(BarcodeSymbologies.Upca);
```

當您完成修改的屬性，並準備好，將其設定時，您可以呼叫[SetSymbologyAttributesAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.setsymbologyattributesasync)。 這個方法會傳回**布林值**，這是 **，則為 true** ，如果屬性已成功設定。

```cs
bool success = await claimedBarcodeScanner.SetSymbologyAttributesAsync(
    BarcodeSymbologies.Upca, barcodeSymbologyAttributes);
```

### <a name="restrict-scan-data-by-data-length"></a>限制掃描資料長度
某些碼制的長度可變，例如 Code 39 或 Code 128。  這些碼制的條碼可能位在包含特定長度的不同資料彼此相近。 設定特定長度的資料，可以防止無效的掃描。

設定解碼長度之前, 先檢查是否條碼碼制支援[IsDecodeLengthSupported](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.isdecodelengthsupported#Windows_Devices_PointOfService_BarcodeSymbologyAttributes_IsDecodeLengthSupported)與多個長度。 一旦您知道支援，您可以設定[DecodeLengthKind](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.decodelengthkind#Windows_Devices_PointOfService_BarcodeSymbologyAttributes_DecodeLengthKind)，也就是類型[BarcodeSymbologyDecodeLengthKind](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologydecodelengthkind)。 這個屬性可以是任何的下列值：

* **AnyLength**： 解碼任何數目的長度。
* **不連續**： 解碼[DecodeLength1](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.decodelength1)或[DecodeLength2](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.decodelength2)支援單位元組字元的長度。
* **範圍**： 解碼之間**DecodeLength1**和**DecodeLength2**支援單位元組字元的長度。 **DecodeLength1**和**DecodeLength2**不要相等 （無論是可以是較高或較低，比其他） 的順序。

最後，您可以設定**DecodeLength1**和**DecodeLength2** ，來控制長度的資料，您需要的值。

下列程式碼片段會示範如何設定解碼長度：

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

### <a name="check-digit-transmission"></a>檢查數字傳輸

您可以設定在特定符號學的另一個屬性是是否檢查數字會傳輸到主機的原始資料的一部分。 之前此設定，請確定的碼制支援檢查數字傳輸， [IsCheckDigitTransmissionSupported](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.ischeckdigittransmissionsupported)。 然後，設定是否已使用[IsCheckDigitTransmissionEnabled](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.ischeckdigittransmissionenabled)啟用時檢查數字傳輸。

下列程式碼片段示範設定檢查數字傳輸：

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

### <a name="check-digit-validation"></a>檢查數字驗證

您也可以設定是否條碼檢查數字將會進行驗證。 之前此設定，請確定的碼制支援檢查數字驗證使用[IsCheckDigitValidationSupported](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.ischeckdigitvalidationsupported)。 然後，設定是否已使用[IsCheckDigitValidationEnabled](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.ischeckdigitvalidationenabled)啟用時檢查數字驗證。

下列程式碼片段示範設定檢查數字驗證：

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

## <a name="see-also"></a>請參閱

* [條碼掃描器](pos-barcodescanner.md)
* [BarcodeSymbologies 類別](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies)
* [BarcodeScanner 類別](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner)
* [Claimscannerasyc 類別](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner)
* [BarcodeSymbologyAttributes 類別](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes)
* [BarcodeSymbologyDecodeLengthKind 列舉](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologydecodelengthkind)