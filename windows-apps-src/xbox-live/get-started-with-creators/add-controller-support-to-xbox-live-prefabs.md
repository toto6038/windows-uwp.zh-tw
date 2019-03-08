---
title: 將控制器支援新增至 Xbox Live Prefabs
description: 將控制器支援新增至使用 Xbox Live Unity 外掛程式的 Xbox Live Prefabs
ms.assetid: ''
ms.date: 07/14/2017
ms.topic: article
keywords: xbox live、 xbox、 遊戲、 uwp、 windows 10，其中一個 xbox、 unity、 控制站支援
ms.localizationpriority: medium
ms.openlocfilehash: 4d32ec62b8beec10256ed9a695866c2fd9bdd03e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57622873"
---
# <a name="add-controller-support-to-xbox-live-prefabs"></a>將控制器支援新增至 Xbox Live prefabs

> [!IMPORTANT]
> Xbox Live Unity 外掛程式不支援的成就或線上多人遊戲，並建議僅用於[Xbox Live 創作者計劃](../developer-program-overview.md)成員。

所有的 Xbox Live Unity 外掛程式 Prefabs 偵測器中支援指定的控制器輸入。

比方說，假設您有遊戲物件呼叫`UserProfile1`根據`UserProfile`prefab。 如果您想要將繫結到播放程式 1 此遊戲物件，並讓他們登入`A`按鈕在其 Xbox 控制器上，只要撰寫`joystick 1 button 0`在`Input Controller Button`偵測器中的欄位。

  ![在 使用者設定檔 Prefab 的控制站支援](../images/unity/controller-support-example.png)

## <a name="all-prefab-controller-input-fields"></a>所有 Prefab 的控制站的輸入的欄位
### <a name="userprofile-prefab"></a>UserProfile prefab
- **輸入的控制器按鈕：** 新增並登入 Xbox Live 的使用者。

### <a name="social-prefab"></a>社交 prefab
- **切換篩選控制器按鈕：** 切換篩選，以顯示 'All' 朋友或 「 線上 」 狀態的朋友。

### <a name="leaderboard-prefab"></a>排行榜 prefab
- **第一個控制器按鈕：** 會採用第一頁的排行榜項目播放程式。
- **最後一個控制器按鈕：** 會採用最後一頁的排行榜項目播放程式。
- **下一步 的 控制器 按鈕：** 會在下一個排行榜項目頁面播放程式。
- **Prev 控制器按鈕：** 會採用前一頁的排行榜項目播放程式。
- **重新整理控制器按鈕：** 重新整理 [排行榜] 檢視。


### <a name="game-save-ui-prefab"></a>遊戲儲存 UI prefab
- **產生新的控制器按鈕：** 產生新的整數儲存的資料。
- **儲存資料 [控制器] 按鈕：** 將目前的資料儲存到連接的儲存體。
- **載入資料控制器按鈕：** 載入目前連接的儲存體中儲存的資料。
- **取得資訊 [控制器] 按鈕：** 擷取連接的儲存體中儲存容器的相關資訊。
- **刪除容器控制器按鈕：** 從連接的儲存體中刪除已儲存的容器

## <a name="xbox-controller-button-mappings"></a>Xbox 控制器按鈕對應

在 Unity Xbox 控制器按鈕對應，請參閱這[Unity 控制器 Wiki 頁面](https://wiki.unity3d.com/index.php?title=Xbox360Controller)。
