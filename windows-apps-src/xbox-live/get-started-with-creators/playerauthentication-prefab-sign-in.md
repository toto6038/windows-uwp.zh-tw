---
title: 使用 PlayerAuthentication Prefab 登入
description: Unity 外掛程式 PlayerAuthentication Prefab 的概觀
ms.date: 05/08/2018
ms.topic: get-started-article
keywords: xbox live、 xbox、 遊戲、 uwp、 windows 10，其中一個 xbox、 unity
ms.openlocfilehash: ea161ff801e2004569d88d53c78ae963e91b4ce6
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57605733"
---
# <a name="easy-sign-in-with-the-playerauthentication-prefab"></a>輕鬆使用登入 PlayerAuthentication Prefab

PlayerAuthentication prefab 是最簡單的方式將 Xbox Live 驗證新增至您的標題。 它只需要三個簡單的步驟，從新的場景時移至登入頁面。

1. PlayerAuthentication prefab 拖曳至場景
2. XboxLiveServices prefab 拖曳至場景
3. EventSystem 加入的場景 （就技術上而言 PlayerAuthentication 會建立一個，如果您 EventSystem 不存在，但將它加入就很好的習慣。）

這樣就大功告成了。 您現在可以登玩家 XboxLive 到在您的標題按一下 PlayerAuthentication prefab，場景中。 在 Unity 中測試場景，依序按一下 [播放] 按鈕會導致您 prefab 產生假的資料，這是因為 Unity 播放器無法連接到 Xbox Live 服務。 若要查看實際的登入，您必須建置專案以在 Visual Studio 在本機執行。 如果已設定您的標題在合作夥伴中心，而且您已授權的 Microsoft 帳戶/玩家代號來登入您的標題，則您將能夠登入您已獲授權的帳戶，在 Visual Studio 組建中的其中一個。

PlayerAuthentication prefab 的指令碼有一些設定值，您可以從其檢視偵測器中操作。

![PlayerAuthentication 偵測器螢幕擷取畫面](../images/unity/playerauthentication_prefab_inspector.JPG)

* 播放程式數目-規定的播放程式連結到 [登入] 面板
* 佈景主題-變更當使用者是登入或登出的登入面板的色彩配置。此設定會有淡或暗的選項。
* 可讓控制器輸入-此核取方塊可讓玩家使用登入和登出使用 PlayerAuthentication prefab Xbox 控制器。
* 搖桿號碼-規定的控制站可以登入我們外使用 prefab。
* 登入按鈕-下拉式這可讓您選擇哪一個按鈕，在 Xbox 控制器上的將使用者登入。
* 登出的按鈕-下拉式這可讓您選擇哪一個按鈕，在 Xbox 控制器上的將使用者登出。

## <a name="multiplayer-scenario"></a>多人遊戲案例

除了單一播放器登入，您也可以使用多個 PlayerAuthentication prefabs Xbox One 主控台標題上實作本機的多人遊戲。 藉由新增 prefab 的多個執行個體，並變更每個播放程式數目屬性，可以登入多個使用者，您的標題。

> [!WARNING]
> 登入多個的玩家代號不是允許在 Windows 10 電腦上。 若要登入多個使用者，您必須測試您在上一個 Xbox 的遊戲。

建立場景，可讓多人遊戲才稍微更難使用 PlayerAuthentication prefab。

1. 拖曳場景 PlayerAuthentication prefab 的執行個體
2. 請檢查**啟用控制站輸入**方塊 prefab 的偵測器中。
3. 請確定**播放程式數目**並**搖桿數目**設為 1。
4. 指派**登入 按鈕**從下拉式清單。
5. 指派**簽章 [Out] 按鈕**從下拉式清單。
6. 拖曳*第二個*光芒 PlayerAuthentication prefab 的執行個體。
7. 請檢查**啟用控制站輸入**方塊 prefab 的偵測器中。
8. 請確定**播放程式數目**並**搖桿數目**設為 2。
9. 指派**登入 按鈕**從下拉式清單。
10. 指派**簽章 [Out] 按鈕**從下拉式清單。
11. XboxLiveServices prefab 拖曳至場景
12. 新增至場景的 EventSystem

請檢查 prefabs 是按下播放，在 Unity 播放器中的運作正常，然後按一下 prefabs。 它們會傳回預期為 Unity 播放器無法連接到 Xbox Live 的假資料。 與兩個執行個體設定為不同的玩家、 搖桿您準備好要建置 Visual Studio 中的遊戲，PlayerAuthentication prefab 它可以正確地測試 Xbox 主控台上。 一旦建立您的遊戲，請在您必須啟用您的遊戲的多使用者支援的 Visual Studio 中開啟方案檔案。
請這樣做：

1. 搜尋方案總管，package.appxmanifest.xml 檔案
2. 以滑鼠右鍵按一下檔案，然後選擇 檢視程式碼
3. 底下`<Properties><\/Properties>`區段中，新增下面這行: ' < uap:SupportedUsers > 多個 <\/uap:SupportedUsers >。
4. 部署至您的 Xbox 的遊戲，藉由從 Visual Studio 中啟動遠端偵錯組建。 您可以找到指示來設定您在 Xbox 上的標題[設定您在 Xbox 開發環境上的 UWP](../../xbox-apps/development-environment-setup.md)文章。

> [!NOTE]
> 組態已變更的部分可能看起來像它讓多人，但仍然必須在單一播放器案例中執行您的遊戲。

一旦 PlayerAuthentication prefab 運作方式，您可以深入了解指令碼登入與發行項[登入的登入管理員，在 Unity 中](sign-in-manager.md)。