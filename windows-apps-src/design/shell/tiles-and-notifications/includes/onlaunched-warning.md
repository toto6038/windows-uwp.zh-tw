---
ms.openlocfilehash: 9508a15492b2b2d6a27d042ddea323f142eb8103
ms.sourcegitcommit: 5e718720d1032a7089dea46a7c5aefa6cda3385f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/12/2021
ms.locfileid: "104804311"
---
> [!IMPORTANT]
> 就像您的 **OnLaunched** 程式碼一樣，您必須初始化畫面並啟用視窗。 **如果使用者按一下您的快顯通知，並不會呼叫 OnLaunched**，即使您的 App 已關閉，然後正在首次啟動，也不會。 我們經常建議將 **OnLaunched** 和 **OnActivated** 結合成 `OnLaunchedOrActivated` 方法，因為兩者都需要進行相同的初始設定。