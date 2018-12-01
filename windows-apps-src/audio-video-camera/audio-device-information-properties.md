---
ms.assetid: 3b75d881-bdcf-402b-a330-23cd29d68e53
description: 本文列出與音訊裝置有關的 DeviceInformation 屬性
title: 音訊裝置資訊屬性
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 443bcc3c0280aca85de31d8c9f3704302432cb76
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/30/2018
ms.locfileid: "8350356"
---
# <a name="audio-device-information-properties"></a>音訊裝置資訊屬性

本文列出與音訊裝置有關的裝置資訊屬性 在 Windows 中，每個硬體裝置都有提供裝置相關詳細資訊的相關聯 [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393) 屬性，您可以在需要裝置相關特定資訊或建置裝置選取器時使用。 如需有關在 Windows 上列舉裝置的一般資訊，請參閱[列舉裝置](../devices-sensors/enumerate-devices.md)和[裝置資訊屬性](../devices-sensors/device-information-properties.md)。


|名稱|類型|描述|
|------------------------------------------------------------|------------|------------------------------------------------------|
|**System.Devices.AudioDevice.Microphone.SensitivityInDbfs**|雙聲道|指定相對於滿量程 (dBFS) 單位的麥克風靈敏度 (分貝)。|
|**System.Devices.AudioDevice.Microphone.SignalToNoiseRatioInDb**|雙聲道|指定以分貝 (dB) 單位測量的麥克風信噪比 (SNR)。|
|**System.Devices.AudioDevice.SpeechProcessingSupported**|布林值|指示音訊裝置是支援語音處理。|
|**System.Devices.AudioDevice.RawProcessingSupported**|布林值|指示音訊裝置是支援原始處理。|
|**System.Devices.MicrophoneArray.Geometry**|不帶正負號的字元[]|麥克風陣列的幾何資料|

## <a name="related-topics"></a>相關主題

* [列舉裝置](../devices-sensors/enumerate-devices.md)
* [裝置資訊屬性](../devices-sensors/device-information-properties.md)
* [建置裝置選取器](../devices-sensors/build-a-device-selector.md)
* [媒體播放](media-playback.md)




