---
author: TerryWarwick
title: 相機條碼掃描器系統需求
description: 本文列出從 UWP app 使用相機條碼掃描器的需求。
ms.author: jken
ms.date: 05/1/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 服務點, pos
ms.localizationpriority: medium
ms.openlocfilehash: f0dcbe28107bb8c6918e4e0ac63698f1f9692005
ms.sourcegitcommit: ab92c3e0dd294a36e7f65cf82522ec621699db87
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/03/2018
ms.locfileid: "1833099"
---
# <a name="camera-barcode-scanner-system-requirements"></a>相機條碼掃描器系統需求
從 Windows 10 版本 1803 開始，您可以從通用 Windows 應用程式透過標準攝影機鏡頭讀取條碼。

## <a name="supported-windows-editions"></a>支援的 Windows 版本
- Windows 10 專業版 S 模式
- Windows 10 專業版
- Windows 10 企業版
- Windows 10 IoT 核心版


## <a name="webcam-requirements"></a>網路攝影機需求
| 分類      | 建議           | 註解 |
| ------------- | ------------------------ | -------- |
| 對焦         | 自動對焦               | 不建議使用固定焦點 |
| 解析度    | 1920 x 1440 或更高    | 我們正解析度為 1920 x 1440 或更高的數位相機的最佳使用體驗。  如果條碼列印夠大的話，部分解析度較低的相機可讀取標準條碼。 元素較細的條碼可能需要解析度較高的相機。 |
|
