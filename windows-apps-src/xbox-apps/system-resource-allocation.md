---
title: 適用於 Xbox One 上 UWP 應用程式和遊戲的系統資源
description: Xbox 上的 UWP 系統資源
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.assetid: 12e87019-4315-424e-b73c-426d565deef9
ms.localizationpriority: medium
ms.openlocfilehash: a78d669902876cdb05a24cee421b0f95a71de31a
ms.sourcegitcommit: bac5574a1f47a5b38c984a5482272c9e49a9c91e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/18/2019
ms.locfileid: "71100837"
---
# <a name="system-resources-for-uwp-apps-and-games-on-xbox-one"></a>適用於 Xbox One 上 UWP 應用程式和遊戲的系統資源

在 Xbox One 上執行的 UWP 應用程式會與系統及其他應用程式共用資源。 UWP on Xbox One 應用程式的可用資源，取決於您提交為 app 或 Xbox Live 創作者計畫遊戲而定。

* 在前景執行時最大可用記憶體：
    * 均1 GB
    * 遊戲5 GB

在背景執行之 App 的最大可用記憶體是 128 MB。 背景模式僅適用於並行應用程式，例如背景音樂播放程式。  遊戲將暫時停用，並在背景中終止。


超過這些限制會導致記憶體配置失敗。 如需監視記憶體使用的詳細資訊，請參閱 [MemoryManager 類別](https://docs.microsoft.com/uwp/api/windows.system.memorymanager)參考。

> [!NOTE]
> 在 Visual Studio 偵錯工具中執行應用程式或遊戲時，不適用這些記憶體條件約束。 只有不是在偵錯模式中執行時，才適用此限制。

* CPU
    * 應用程式：視在系統上執行的應用程式和遊戲數目而定，共用 2-4 個 CPU 核心。
    * 遊戲4個獨佔和2個共用 CPU 核心。

* GPU
    * 應用程式：視在系統上執行的應用程式和遊戲數目而定，共用 45% 的 GPU。
    * 遊戲：可用 GPU 週期的完整存取。

* DirectX 支援
    * 均DirectX 11 功能層級10。
    * 遊戲DirectX 12 和 DirectX 11 功能層級10。

* 所有應用程式和遊戲必須以 x64 架構為目標，以便開發或提交到 Microsoft Store 供 Xbox 使用。  

對於**應用程式開發**，相較於標準電腦，資源可能有限且會因系統上執行的應用程式和遊戲數目而異。

對於 **「遊戲開發」** ，Xbox One (如同其他遊戲主機) 是需要特定硬體型開發套件來存取其完整功能的特殊化硬體。 如果您正在處理需要存取 Xbox One 硬體的最大潛力的遊戲，請考慮向 [ID@Xbox](https://www.xbox.com/Developers/id) 計畫註冊來存取該 Xbox One 開發套件。

## <a name="see-also"></a>另請參閱
- [在 Xbox One UWP](index.md)
- [開始使用 Xbox Live 創作者計畫](https://docs.microsoft.com/gaming/xbox-live/get-started-with-creators/creators-program)
- [Xbox One 上的 DirectX 和 UWP](https://walbourn.github.io/)