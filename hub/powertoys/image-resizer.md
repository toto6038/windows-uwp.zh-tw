---
title: Windows 10 的 Powertoy 影像調整工具公用程式
description: 用於大量影像調整大小的 Windows shell 擴充功能
ms.date: 12/02/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 486eb90b8515ec8422e8a475c9f03ced070dafa1
ms.sourcegitcommit: 46a7e9db64e17a645ee6e888f62a9b04632c56af
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/17/2020
ms.locfileid: "97618579"
---
# <a name="image-resizer-utility"></a>影像調整工具公用程式

影像調整器是適用于大量影像調整大小的 Windows shell 擴充功能。 安裝 Powertoy 之後，以滑鼠右鍵按一下檔案總管中的一或多個選取的影像檔，然後從功能表中選取 [ **調整圖片大小** ]。

![影像調整尺寸示範](../images/powertoys-resize-images.gif)

## <a name="drag-and-drop"></a>拖放

影像調整器也可讓您 **使用滑鼠右鍵** 拖曳選取的檔案，以調整影像大小。 這可讓您快速地將調整大小的圖片儲存在另一個資料夾中。

![影像調整的拖放示範](../images/powertoys-resize-drag-drop.gif)

## <a name="settings"></a>設定

在 [Powertoy 影像調整] 索引標籤中，您可以設定下列設定。

![Powertoy 影像調整大小設定功能表](../images/powertoys-imageresize-settings.png)

### <a name="sizes"></a>大小

加入新的預設大小。 每個大小都可以設定為填滿、適合或延展。 要用於調整大小的維度也可以設定為公分、釐米、Percent 和圖元。

#### <a name="fill-vs-fit-vs-stretch"></a>填滿 vs Stretch （& a）

- **填滿：** 使用影像填滿整個指定的大小。 依比例調整影像。 視需要裁剪影像。
- **符合：** 將整個影像納入指定的大小。 依比例調整影像。 不會裁剪影像。
- **Stretch：** 使用影像填滿整個指定的大小。 視需要伸展影像 disproportionally。 不裁剪影像

您可以交換指定大小的寬度和高度，以符合目前影像 (縱向/橫向) 的方向。 若要一律使用指定的寬度和高度，取消勾選： **忽略圖片的方向**。

![影像調整的設定](../images/powertoys-resize-settings.gif)

### <a name="fallback-encoding"></a>回溯編碼

當檔案無法以其原始格式儲存時，就會使用 fallback 編碼器。 例如，Windows 中繼檔 ( .wmf) 影像格式具有可讀取影像的解碼器，但沒有任何編碼器可寫入新的映射。 在此情況下，影像無法以其原始格式儲存。 影像調整，可讓您指定 fallback 編碼器將使用的格式： PNG、JPEG、TIFF、BMP、GIF 或 WMPhoto 設定。 *這不是檔案類型轉換工具，但僅適用于不支援的檔案格式。*

### <a name="file"></a>檔案

您可以使用下列參數修改已調整大小之影像的檔案名：

- `%1`：原始檔案名
- `%2`： Powertoy 影像調整設定中所設定的大小名稱 () 
- `%3`：選取的寬度
- `%4`：選取的高度
- `%5`：實際高度
- `%6`：實際寬度

例如，在檔案上將檔案名格式設定為：， `%1 (%2)` `example.png` 然後選取 [檔案 `Small` 大小] 設定，就會產生檔案名 `example (Small).png` 。

將檔案的格式設定為 `%1_%4` `example.jpg` ，然後選取 [大小] 設定， `Medium 1366 x 768px` 就會產生檔案名： `example_768.jpg` 。

您也可以選擇在調整大小的影像上保留原始的 *上次修改* 日期。

### <a name="auto-widthheight"></a>自動寬度/高度

您可以讓高度或寬度維持空白。 這會接受指定的維度，並將另一個維度的「鎖定」為與原始影像外觀比例比例的值。

### <a name="sub-directories"></a>子目錄

您可以指定檔案名格式的目錄，將調整大小的影像分組為子目錄。 例如，的值會將 `%2\%1` 調整大小的影像儲存至 `Small\Sample.jpg`
