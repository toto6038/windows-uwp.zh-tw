---
ms.assetid: 98BD79B3-F420-43C5-98D3-52EBDDB479A0
description: 本文列出可搭配 BitmapEncoder 使用的編碼選項。
title: BitmapEncoder 選項參考
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 34e2ac1531b496f076abbe8e23ffa1c0cb318373
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/29/2019
ms.locfileid: "66359047"
---
# <a name="bitmapencoder-options-reference"></a>BitmapEncoder 選項參考


本文列出可搭配 [**BitmapEncoder**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.BitmapEncoder) 使用的編碼選項。 編碼選項是由它的名稱和值所定義，其中的名稱是一個字串，而值則是特定資料類型 ([**Windows.Foundation.PropertyType**](https://docs.microsoft.com/uwp/api/Windows.Foundation.PropertyType))。 如需使用影像的資訊，請參閱[建立、編輯和儲存點陣圖影像](imaging.md)。

| 名稱                    | PropertyType | 使用方式註解                                                                                        | 有效格式 |
|-------------------------|--------------|----------------------------------------------------------------------------------------------------|---------------|
| ImageQuality            | single       | 有效值範圍從 0 到 1.0。 值越高，品質越高                                 | JPEG、JPEG-XR |
| CompressionQuality      | single       | 有效值範圍從 0 到 1.0。 值越高，效率越高，壓縮配置越慢 | TIFF          |
| Lossless                | boolean      | 如果設為 true，就會略過 ImageQuality 選項                                        | JPEG-XR       |
| InterlaceOption         | boolean      | 是否交錯影像                                                                    | PNG           |
| FilterOption            | uint8        | 使用 [**PngFilterMode**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.PngFilterMode) 列舉                                | PNG           |
| TiffCompressionMethod   | uint8        | 使用 [**TiffCompressionMode**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.TiffCompressionMode) 列舉                    | TIFF          |
| Luminance               | uint32Array  | 含有照度量化常數的 64 個元素的陣列                               | JPEG          |
| Chrominance             | uint32Array  | 含有色度量化常數的 64 個元素的陣列                             | JPEG          |
| JpegYCrCbSubsampling    | uint8        | 使用 [**JpegSubsamplingMode**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.JpegSubsamplingMode) 列舉                    | JPEG          |
| SuppressApp0            | boolean      | 是否抑制建立 App0 中繼資料區塊                                        | JPEG          |
| EnableV5Header32bppBGRA | boolean      | 是否編碼為支援 Alpha 的版本 5 BMP                                         | BMP           |

 

## <a name="related-topics"></a>相關主題

* [建立、編輯和儲存點陣圖影像](imaging.md)
* [支援的轉碼器](supported-codecs.md)

 




