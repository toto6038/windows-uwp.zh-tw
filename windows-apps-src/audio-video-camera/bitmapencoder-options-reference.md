---
author: laurenhughes
ms.assetid: 98BD79B3-F420-43C5-98D3-52EBDDB479A0
description: "本文列出可搭配 BitmapEncoder 使用的編碼選項。"
title: "BitmapEncoder 選項參考"
translationtype: Human Translation
ms.sourcegitcommit: fd5b52a1d431b9396a4b162077d4f8d6246cd597
ms.openlocfilehash: 4edf119d0f8830fec9ece34f1a7eb8f3ffbe67dd

---

# BitmapEncoder 選項參考

\[ 針對 Windows10 上的 UWP app 更新。 如需 Windows8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

本文列出可搭配 [**BitmapEncoder**](https://msdn.microsoft.com/library/windows/apps/br226206) 使用的編碼選項。 編碼選項是由它的名稱和值所定義，其中的名稱是一個字串，而值則是特定資料類型 ([**Windows.Foundation.PropertyType**](https://msdn.microsoft.com/library/windows/apps/br225871))。 如需使用影像的資訊，請參閱[建立、編輯和儲存點陣圖影像](imaging.md)。

| 名稱                    | PropertyType | 使用方式註解                                                                                        | 有效格式 |
|-------------------------|--------------|----------------------------------------------------------------------------------------------------|---------------|
| ImageQuality            | single       | 有效值範圍從 0 到 1.0。 值越高，品質越高                                 | JPEG、JPEG-XR |
| CompressionQuality      | single       | 有效值範圍從 0 到 1.0。 值越高，效率越高，壓縮配置越慢 | TIFF          |
| Lossless                | 布林值      | 如果設為 true，就會略過 ImageQuality 選項                                        | JPEG-XR       |
| InterlaceOption         | 布林值      | 是否交錯影像                                                                    | PNG           |
| FilterOption            | uint8        | 使用 [**PngFilterMode**](https://msdn.microsoft.com/library/windows/apps/br226389) 列舉                                | PNG           |
| TiffCompressionMethod   | uint8        | 使用 [**TiffCompressionMode**](https://msdn.microsoft.com/library/windows/apps/br226399) 列舉                    | TIFF          |
| Luminance               | uint32Array  | 含有照度量化常數的 64 個元素的陣列                               | JPEG          |
| Chrominance             | uint32Array  | 含有色度量化常數的 64 個元素的陣列                             | JPEG          |
| JpegYCrCbSubsampling    | uint8        | 使用 [**JpegSubsamplingMode**](https://msdn.microsoft.com/library/windows/apps/br226386) 列舉                    | JPEG          |
| SuppressApp0            | 布林值      | 是否抑制建立 App0 中繼資料區塊                                        | JPEG          |
| EnableV5Header32bppBGRA | 布林值      | 是否編碼為支援 Alpha 的版本 5 BMP                                         | BMP           |

 

## 相關主題

* [建立、編輯和儲存點陣圖影像](imaging.md)
* [支援的轉碼器](supported-codecs.md)

 







<!--HONumber=Nov16_HO1-->


