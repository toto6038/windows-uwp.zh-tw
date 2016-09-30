---
author: Mtoepke
title: "適用於 Xbox One 上 UWP 應用程式和遊戲的系統資源"
description: "Xbox 上的 UWP 系統資源"
area: Xbox
translationtype: Human Translation
ms.sourcegitcommit: 3de603aec1dd4d4e716acbbb3daa52a306dfa403
ms.openlocfilehash: 5c5947239e16d883511e56c62f5267568c3d5feb

---

# 適用於 Xbox One 上 UWP 應用程式和遊戲的系統資源

在 Xbox One 上執行的 UWP 應用程式和遊戲會與系統及其他應用程式共用資源。 因此，UWP 應用程式和遊戲將可存取下列資源︰

* 在這個預覽版本中，在前景執行之 app 的最大可用記憶體是 1 GB。
    * 在背景執行之 app 的最大可用記憶體是 128 MB。
    * 超過這些記憶體需求的 app 將會發生記憶體配置失敗。 如需監視記憶體使用的詳細資訊，請參閱 [MemoryManager 類別](https://msdn.microsoft.com/library/windows/apps/windows.system.memorymanager.aspx)參考。
    * **注意**&nbsp;&nbsp;從 Visual Studio 偵錯工具執行您的應用程式或遊戲時，不適用這些記憶體限制。 只有不是在偵錯模式中執行時，才適用此限制。

* 視在系統上執行的應用程式和遊戲數目而定，共用 2-4 個 CPU 核心。

* 視在系統上執行的應用程式和遊戲數目而定，共用 45% 的 GPU。

* Xbox One 上的 UWP 支援 DirectX 11 功能層級 10。 此時不支援 DirectX 12。 

對於「應用程式開發」****，請務必牢記，相較於標準電腦，可用的資源可能受限。

對於「遊戲開發」****，請務必牢記，Xbox One (如同其他遊戲主機) 是需要特定硬體型開發套件來存取其完整功能的特殊化硬體。 如果您正在處理需要存取 Xbox One 硬體的最大潛力的遊戲，請向 [ID@Xbox](http://www.xbox.com/Developers/id) 計畫註冊來存取該 Xbox One 開發套件 (內含 DirectX 12 支援)。



<!--HONumber=Jul16_HO2-->


