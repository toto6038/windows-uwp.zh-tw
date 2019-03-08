---
title: 建立磚集區
description: 應用程式可以在每一個 Direct3D 裝置建立一個或多個磚集區。 每個圖格的集區的大小總計僅限於使用 Direct3D 11 的資源大小限制，亦即大約是 1/4 的圖形處理單元 (GPU) 的 RAM。
ms.assetid: BD51EDD3-4AD3-4733-B014-DD77B9D743BB
keywords:
- 建立磚集區
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 5ce3824ab2d435b42df9586a6c229b68db10a0c9
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57612803"
---
# <a name="tile-pool-creation"></a>建立磚集區


應用程式可以在每一個 Direct3D 裝置建立一個或多個磚集區。 每個圖格的集區的大小總計僅限於使用 Direct3D 11 的資源大小限制，亦即大約是 1/4 的圖形處理單元 (GPU) 的 RAM。

磚集區由 64 KB 磚所組成，但作業系統 (顯示驅動程式) 會在幕後將整個集區作為一或多個配置來管理，但應用程式看不到分解過程。 串流資源藉由指向磚集區中的磚來定義內容。 您能藉由將磚指向**NULL**，以取消對應串流資源中的磚。 這類非對應的磚有讀取或寫入行為的規則；請參閱[磚集區資源的危險追蹤](hazard-tracking-versus-tile-pool-resources.md) (英文)。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相關主題


[對應是到並排顯示集區](mappings-are-into-a-tile-pool.md)

 

 




