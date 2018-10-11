---
author: Mtoepke
title: 適用於 Xbox One 上 UWP 應用程式和遊戲的系統資源
description: Xbox 上的 UWP 系統資源
ms.author: mstahl
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.assetid: 12e87019-4315-424e-b73c-426d565deef9
ms.localizationpriority: medium
ms.openlocfilehash: 8d6ebe8e3344f5276939471d7ac569ae83d48311
ms.sourcegitcommit: 933e71a31989f8063b020746fdd16e9da94a44c4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/11/2018
ms.locfileid: "4529918"
---
# <a name="system-resources-for-uwp-apps-and-games-on-xbox-one"></a>適用於 Xbox One 上 UWP 應用程式和遊戲的系統資源

在 Xbox One 上執行的 UWP 應用程式會與系統及其他應用程式共用資源。 UWP on Xbox One 應用程式的可用資源，取決於您提交為 app 或 Xbox Live 創作者計畫遊戲而定。

* 在前景執行時最大可用記憶體：
    * 應用程式：1 GB
    * 遊戲：5 GB

在背景執行之 App 的最大可用記憶體是 128 MB。 背景模式僅適用於並行應用程式，例如背景音樂播放程式。  遊戲將暫時停用，並在背景中終止。

超過這些限制會導致記憶體配置失敗。 如需監視記憶體使用的詳細資訊，請參閱 [MemoryManager 類別](https://msdn.microsoft.com/library/windows/apps/windows.system.memorymanager.aspx)參考。
    
    > [!NOTE]
    > When running your app or game from the Visual Studio debugger, these memory constraints do not apply. This limit is only applicable when not running in debugging mode.

* CPU
    * 應用程式：視在系統上執行的應用程式和遊戲數目而定，共用 2-4 個 CPU 核心。
    * 遊戲：4 個專屬與 2 個共用 CPU 核心。

* GPU
    * 應用程式：視在系統上執行的應用程式和遊戲數目而定，共用 45% 的 GPU。
    * 遊戲：可用 GPU 週期的完整存取。

* DirectX 支援
    * 應用程式：DirectX 11 功能層級 10。
    * 遊戲：DirectX 12 和 DirectX 11 功能層級 10。

* 所有應用程式和遊戲必須以 x64 架構為目標，以便開發或提交到 Microsoft Store 供 Xbox 使用。  

對於**應用程式開發**，相較於標準電腦，資源可能有限且會因系統上執行的應用程式和遊戲數目而異。

對於 **「遊戲開發」**，Xbox One (如同其他遊戲主機) 是需要特定硬體型開發套件來存取其完整功能的特殊化硬體。 如果您正在處理需要存取 Xbox One 硬體的最大潛力的遊戲，請考慮向 [ID@Xbox](http://www.xbox.com/Developers/id) 計畫註冊來存取該 Xbox One 開發套件。


如需稍多一些關於 Xbox One 上 UWP app 系統資源的詳細資訊，請觀看這部影片的第一部分。
</br>
</br>
<iframe src="https://mva.microsoft.com/en-US/training-courses-embed/developing-xbox-one-applications-16860/Video-What-s-Unique--vk0fOPf9C_2006218965" width="636" height="480" allowFullScreen frameBorder="0"></iframe>

## <a name="see-also"></a>請參閱
- [Xbox One 上的 UWP](index.md)
- [開始使用 Xbox Live 創作者計畫](../xbox-live/get-started-with-creators/get-started-with-xbox-live-creators.md)
- [DirectX 與 Xbox One 上的 UWP](https://blogs.msdn.microsoft.com/chuckw/2017/12/15/directx-and-uwp-on-xbox-one/)

