---
author: TylerMSFT
title: 實驗 API
description: 了解實驗 API
ms.author: twhitney
ms.date: 11/13/2017
ms.topic: article
keywords: Windows 10, uwp, 實驗, API
ms.localizationpriority: medium
ms.openlocfilehash: fe5fa437c5a1e564be07b7277de0f190d6eab862
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2018
ms.locfileid: "5763511"
---
# <a name="experimental-apis"></a>實驗 API

實驗 API 是設計的早期階段，可能隨擁有者納入意見反應並加入其他案例的支援而改變。

這些 API 會在外部使用 [Windows 測試人員 SDK](https://www.microsoft.com/en-us/software-download/windowsinsiderpreviewSDK) 製作為正式發行前小眾測試版，讓開發人員能夠試用並提供意見反應，然後才會變成正式平台。 當它們還是正式發行前小眾測試版時，無法保證其穩定性或做出承諾。

## <a name="consuming-experimental-apis"></a>使用實驗 API
Intellisense 會讓您知道 API 是否為實驗性。 當您使用實驗 API 時，也會收到編譯器警告，像是「...供評估之用，而且有可能在未來更新中變更或移除」。

這些警告有助於保護您，避免在生產專案中製造實驗 API 的相依性。 若是實驗專案，您可以忽略或停用這些警告。

根據預設，這些 API 會在執行階段停用，呼叫它們將會導致執行階段例外狀況。 這是另一項保護機制，有助於防止意外的相依性及大量散發使用實驗 API 的 app。

若要將這些 API 供實驗用，請在目標裝置上使用 [Windows 裝置入口網站 (WDP)](https://docs.microsoft.com/en-us/windows/uwp/debug-test-perf/device-portal) 功能外掛程式，以啟用對應您要呼叫之 API 的功能。

特定實驗 API 的文件由擁有之小組自行決定是否提供。

## <a name="providing-feedback"></a>提供意見反應

如果您已試用過實驗 API 並想要提供意見反應，請使用 [Windows 意見反應中樞](https://support.microsoft.com/en-us/help/4021566/windows-10-send-feedback-to-microsoft-with-feedback-hub-app)中的 **\[開發人員平台\]** 類別。