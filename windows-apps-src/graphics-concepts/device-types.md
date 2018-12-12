---
title: 裝置類型
description: Direct3D 裝置類型包括硬體抽象層 (hal) 裝置和軟體模擬轉譯器。
ms.assetid: 64084B23-10C0-4541-8E93-FB323385D2F0
keywords:
- 裝置類型
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 5ddb1dc0e42f88cf65464841388b9addfb4b5748
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/11/2018
ms.locfileid: "8935922"
---
# <a name="device-types"></a>裝置類型


Direct3D 裝置類型包括硬體抽象層 (hal) 裝置和軟體模擬轉譯器。

## <a name="span-idhaldevicespanspan-idhaldevicespanspan-idhaldevicespanhal-device"></a><span id="HAL_Device"></span><span id="hal_device"></span><span id="HAL_DEVICE"></span>HAL 裝置


主要裝置類型是 hal 裝置，其支援硬體加速點陣化，以及硬體和軟體頂點處理。 如果您執行應用程式所在的電腦配備了支援 Direct3D 顯示卡，您的應用程式應該會使用它執行 Direct3D 作業。 Direct3D hal 裝置會實作硬體中的所有或部分轉換、光源和點陣化模組。

應用程式不會直接存取圖形卡。 它們會呼叫 Direct3D 功能和方法。 Direct3D 會透過 hal 存取硬體。 如果您的應用程式執行所在的電腦支援 hal，它使用 hal 裝置將可獲得最佳效能。

## <a name="span-idreferencedevicespanspan-idreferencedevicespanspan-idreferencedevicespanreference-device"></a><span id="Reference_Device"></span><span id="reference_device"></span><span id="REFERENCE_DEVICE"></span>參照裝置


Direct3D 支援額外的裝置類型，稱為參照裝置或軟體模擬轉譯器。 與軟體裝置不同的是，軟體模擬轉譯器支援每一項 Direct3D 功能。 此裝置主要用於偵錯，因此只在已安裝 DirectX SDK 的電腦上提供。 由於這些功能實作的目的在於精確性，而非速度，並且在軟體中實作，因此結果不會非常快。 軟體模擬轉譯器確實會盡可能利用特殊 CPU 指令，但主要不是用於零售應用程式。 僅針對功能測試或展示目的使用軟體模擬轉譯器。

## <a name="span-idhalvsrefspanspan-idhalvsrefspanspan-idhalvsrefspanhal-vs-ref-devices"></a><span id="HAL_vs_REF"></span><span id="hal_vs_ref"></span><span id="HAL_VS_REF"></span>HAL 與參照裝置比較


HAL (硬體抽象層) 裝置和 REF (軟體模擬轉譯器) 裝置是 Direct3D 裝置的主要兩種類型；第一種是以硬體支援為基礎，速度非常快，但可能不會提供全面支援；第二種則不會使用硬體加速，因此非常慢，但是保證支援完整的 Direct3D 功能，並採取正確的方式。 一般來刷，您只會需要使用 HAL 裝置，但是如果您使用某些圖形卡不支援的進階功能，則您可能需要改用 REF。

其他可能想要使用 REF 的情況是，如果 HAL 裝置產生奇怪的結果，也就是說，您確定程式碼是正確的，但結果卻不如預期。 REF 裝置保證會正確運作，因此您可能想要在 REF 裝置上測試應用程式，並查看奇怪的情形是否繼續發生。 如果未繼續發生，表示 (a) 您的應用程式假設圖形卡支援它應該不支援的項目，或 (b) 是驅動程式錯誤。 如果使用 REF 裝置仍然無法運作，表示是應用程式錯誤。

## <a name="span-idhardwarevssoftwarespanspan-idhardwarevssoftwarespanspan-idhardwarevssoftwarespanhardware-vs-software-vertex-processing"></a><span id="Hardware_vs_Software"></span><span id="hardware_vs_software"></span><span id="HARDWARE_VS_SOFTWARE"></span>硬體與軟體頂點處理比較


硬體與軟體頂點處理僅適用於 HAL 裝置。 當您透過管線推送頂點時，它們需要轉換 (輪流以世界、檢視、投影矩陣) 和照亮 (藉由 D3D 內建的照明) - 此處理階段稱為 T&L (代表轉換與光源)。 硬體頂點處理表示是在硬體內完成 (如果硬體支援的話)；因此，軟體頂點處理是在軟體內完成。 一般方式是先嘗試建立硬體 T&L 裝置，如果失敗則嘗試混合，再失敗則嘗試軟體。 (如果軟體失敗，則放棄並在有錯誤的情況下結束)。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相關主題


[裝置](devices.md)

 

 




