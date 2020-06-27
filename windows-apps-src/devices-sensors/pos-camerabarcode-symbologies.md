---
title: 相機條碼掃描器碼制
description: 相機條碼掃描器支援的碼制
ms.date: 05/02/2018
ms.topic: article
keywords: windows 10, uwp, 服務點, pos
ms.localizationpriority: medium
ms.openlocfilehash: 481d10f2fea076f45124a3c75819dfe6494300bf
ms.sourcegitcommit: 48e047a581fcfcc9a4084d65a78b89f2c01cf4f3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/26/2020
ms.locfileid: "85448398"
---
# <a name="symbologies"></a>碼制

本主題提供 Windows 10 隨附之軟體條碼解碼器所支援的每個碼制的範例條碼包括：UPC/EAN、Code 39、Code 128、Interleaved 2 of 5、Databar Omnidirectional、Databar Stacked、QR 代碼和 GS1DWCode。

Windows 10 使用與軟體解碼器結合的標準鏡頭相機來產生條碼掃描器。 本文是指軟體解碼器支援的條碼符號表示。 具有內建硬體解碼器的專用條碼掃描器裝置可能支援其他條碼符號表示，如需詳細資訊，請洽詢條碼掃描器製造商。 除非另有指定，否則 Windows 10 組建17134或更新版本的所有版本都支援列出的條碼符號表示。

使用[GetSupportedSymbologiesAsync](/uwp/api/windows.devices.pointofservice.barcodescanner.getsupportedsymbologiesasync)來判斷條碼掃描器支援的特定條碼符號表示。

> [!NOTE]
> Windows 10 內建的軟體解碼器由[*Digimarc Corporation*](https://www.digimarc.com/)提供。

## <a name="1d-symbologies"></a>1D 碼制

### <a name="code-39"></a>Code 39
![範例條碼 - Code 39](images/pos/sample-barcode-code39.png)

### <a name="code-128"></a>Code 128
![範例條碼 - Code 128](images/pos/sample-barcode-code128.png)

### <a name="databar-omnidirectional"></a>Databar Omnidirectional
![範例條碼 - Databar Omnidirectional](images/pos/sample-barcode-databar-omnidirectional.png) 
### <a name="databar-stacked"></a>Databar Stacked
![範例條碼 - Databar Stacked](images/pos/sample-barcode-databar-stacked.png)

### <a name="ean-8"></a>EAN-8
![範例條碼 - EAN-8](images/pos/sample-barcode-ean8.png)

### <a name="ean-13"></a>EAN-13
![範例條碼 - EAN-13](images/pos/sample-barcode-ean13.png)

### <a name="interleaved-2-of-5"></a>Interleaved 2 of 5
![範例條碼 - Interleaved 2 of 5](images/pos/sample-barcode-interleaved-2-of-5.png)

### <a name="upc-a"></a>UPC-A
![範例條碼 - UPC A](images/pos/sample-barcode-upca.png)

### <a name="upc-e"></a>UPC-E
![範例條碼 - UPC E](images/pos/sample-barcode-upce.png)

## <a name="2d-symbologies"></a>2D 碼制
### <a name="qr-code"></a>QR 代碼
![範例條碼 - QR 代碼](images/pos/sample-barcode-qrcode.png)

## <a name="digital-watermark"></a>數位浮水印
### <a name="gs1-dwcode"></a>GS1-DWCode

使用您的相機條碼掃描器應用程式掃描以下包裝的影像，以查看 GS1DWCode 的動作。  影像編碼為 UPCA 856107006854。  如需 GS1DWCode 功能的詳細資訊，請瀏覽 http://www.digimarc.com。

![範例條碼 - GS1DWCode](images/pos/Rice-Box-V7.jpg)

## <a name="see-also"></a>另請參閱

### <a name="samples"></a>範例

- [條碼掃描器範例](https://github.com/microsoft/Windows-universal-samples/tree/master/Samples/BarcodeScanner)
