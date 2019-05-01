---
title: 使用條碼掃描器碼制
description: 本文內容涵蓋有關條碼掃描器碼制的相關資訊。
ms.date: 08/29/2018
ms.topic: article
keywords: windows 10, uwp, 服務點, pos
ms.localizationpriority: medium
ms.openlocfilehash: ee78ffbc49fdcb7f8e87844dea1e2ce29297e9f3
ms.sourcegitcommit: fca0132794ec187e90b2ebdad862f22d9f6c0db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/24/2019
ms.locfileid: "63816693"
---
# <a name="working-with-symbologies"></a>使用碼制
[條碼碼制](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies)是資料到特定條碼格式的對應。 一些常見的象徵意義包括 UPC、 程式碼 128、 QR 代碼，等等。  通用 Windows 平台的條碼掃描器的 Api 可讓應用程式控制掃描器處理這些象徵意義不需要手動設定掃描器的方式。 

## <a name="determine-which-symbologies-are-supported"></a>判斷支援的碼制 
因為您的應用程式可能搭配多個製造商的不同條碼掃描器機型使用，您可能想要查詢掃描器以判斷其支援碼制的清單。  如果您的應用程式需要所有掃描器可能都不支援的特定碼制，或者您必須啟用掃描器上手動或以程式設計方式停用的碼制，這非常有用。

若您取得 [BarcodeScanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner) 物件的方式是使用 [BarcodeScanner.FromIdAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.fromidasync)，呼叫 [GetSupportedSymbologiesAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.getsupportedsymbologiesasync#Windows_Devices_PointOfService_BarcodeScanner_GetSupportedSymbologiesAsync) 以取得裝置所支援碼制的清單。

下列範例取得一份支援的象徵意義的條碼掃描器，並顯示在文字區塊：

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
若要判斷掃描器是否支援特定的象徵意義，您可以呼叫[IsSymbologySupportedAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.issymbologysupportedasync#Windows_Devices_PointOfService_BarcodeScanner_IsSymbologySupportedAsync_System_UInt32_)。

下列範例會檢查是否要支援的條碼掃描器**Code32**象徵意義：

```cs
bool symbologySupported = await barcodeScanner.IsSymbologySupportedAsync(BarcodeSymbologies.Code32);
```

## <a name="change-which-symbologies-are-recognized"></a>變更辨識的象徵意義
有時候，您可能想要使用條碼掃描器支援的碼制子集。  這特別適合封鎖您不想在您的應用程式中使用的碼制。 例如，為確保使用者掃描正確的條碼，您可以在取得項目 SKU 時限制掃描到 UPC 或 EAN，以及在取得序號時限制掃描到 Code 128。

一旦您知道掃描器支援的碼制，您可以設定要其辨識的碼制。  您已建立之後可以完成這[ClaimedBarcodeScanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner)物件使用[ClaimScannerAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.claimscannerasync#Windows_Devices_PointOfService_BarcodeScanner_ClaimScannerAsync)。 您可以呼叫 [SetActiveSymbologiesAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.setactivesymbologiesasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_SetActiveSymbologiesAsync_Windows_Foundation_Collections_IIterable_System_UInt32__) 以啟用特定組的碼制，而這些從清單省略的碼制已停用。

下列範例會設定已領取的條碼掃描器的使用中的象徵意義[Code39](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies.code39#Windows_Devices_PointOfService_BarcodeSymbologies_Code39)並[Code39Ex](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies.code39ex):

```cs
private async void SetSymbologies(ClaimedBarcodeScanner claimedBarcodeScanner) 
{
    var symbologies = new List<uint>{ BarcodeSymbologies.Code39, BarcodeSymbologies.Code39Ex };
    await claimedBarcodeScanner.SetActiveSymbologiesAsync(symbologies);
}
```

## <a name="barcode-symbology-attributes"></a>條碼象徵意義屬性
不同的條碼象徵意義可以有不同的屬性，例如支援多個解碼一部分的未經處理的資料傳輸至主機的核取數字的長度，並檢查數字驗證。 具有[BarcodeSymbologyAttributes](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes)類別，您可以取得和設定這些屬性的給定[ClaimedBarcodeScanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner)和條碼象徵意義。

您可以取得與指定的象徵意義的屬性[GetSymbologyAttributesAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.getsymbologyattributesasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_GetSymbologyAttributesAsync_System_UInt32_)。 下列程式碼片段會取得 Upca 象徵意義的屬性**ClaimedBarcodeScanner**。

```cs
BarcodeSymbologyAttributes barcodeSymbologyAttributes = 
    await claimedBarcodeScanner.GetSymbologyAttributesAsync(BarcodeSymbologies.Upca);
```

當您完成修改屬性和已準備好進行設定時，您可以呼叫[SetSymbologyAttributesAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.setsymbologyattributesasync)。 這個方法會傳回**bool**，即 **，則為 true**如果已成功設定屬性。

```cs
bool success = await claimedBarcodeScanner.SetSymbologyAttributesAsync(
    BarcodeSymbologies.Upca, barcodeSymbologyAttributes);
```

### <a name="restrict-scan-data-by-data-length"></a>限制掃描資料的資料長度
某些碼制的長度可變，例如 Code 39 或 Code 128。  條碼的這些象徵意義可以是放置於鄰近位置包含不同的特定長度的資料。 設定特定長度的資料，可以防止無效的掃描。

設定解碼長度，之前，先檢查 條碼象徵意義是否支援使用多個長度[IsDecodeLengthSupported](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.isdecodelengthsupported#Windows_Devices_PointOfService_BarcodeSymbologyAttributes_IsDecodeLengthSupported)。 一旦您知道它受支援，您可以設定[DecodeLengthKind](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.decodelengthkind#Windows_Devices_PointOfService_BarcodeSymbologyAttributes_DecodeLengthKind)，這是型別的[BarcodeSymbologyDecodeLengthKind](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologydecodelengthkind)。 這個屬性可以是下列值之一：

* **AnyLength**:解碼任意數目的長度。
* **離散**:解碼的長度[DecodeLength1](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.decodelength1)或是[DecodeLength2](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.decodelength2)單一位元組字元。
* **範圍**:解碼之間的長度**DecodeLength1**並**DecodeLength2**單一位元組字元。 順序**DecodeLength1**並**DecodeLength2**執行的問題 （可以是高於或低於其他）。

最後，您可以在這裡設定的值**DecodeLength1**並**DecodeLength2**來控制您所需要的資料長度。

下列程式碼片段將示範如何設定解碼長度：

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

您可以設定的象徵意義的另一個屬性是是否檢查數字將會傳送至主應用程式做為未經處理資料的一部分。 之前，請確定象徵意義支援檢查以確認使用的數字傳輸[IsCheckDigitTransmissionSupported](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.ischeckdigittransmissionsupported)。 然後，設定是否啟用檢查數字傳輸[IsCheckDigitTransmissionEnabled](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.ischeckdigittransmissionenabled)。

下列程式碼片段會示範設定核取數字傳輸：

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

您也可以設定是否將驗證條碼檢查碼。 之前，請確定象徵意義支援檢查以確認使用的數字驗證[IsCheckDigitValidationSupported](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.ischeckdigitvalidationsupported)。 然後，設定是否已啟用檢查數字驗證[IsCheckDigitValidationEnabled](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.ischeckdigitvalidationenabled)。

下列程式碼片段示範如何設定核取數字驗證：

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