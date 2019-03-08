---
title: 多使用者支援新增至您的 Unity 遊戲
description: 多使用者支援新增至您使用 Xbox Live Unity 外掛程式的 Unity 遊戲
ms.assetid: ''
ms.date: 07/14/2017
ms.topic: article
keywords: xbox live、 xbox、 遊戲、 uwp、 windows 10，其中一個 xbox、 unity、 多重使用者
ms.localizationpriority: medium
ms.openlocfilehash: 483a0e966be756de483bf7e2ab8458647397687b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57613733"
---
# <a name="add-multi-user-support-to-your-unity-game"></a>多使用者支援新增至您的 Unity 遊戲
> [!IMPORTANT]
> Xbox Live Unity 外掛程式不支援的成就或線上多人遊戲，並建議僅用於[Xbox Live 創作者計劃](../developer-program-overview.md)成員。

Xbox Live Unity 外掛程式的情況下，多個本機的播放程式現在支援多使用者支援。 每個播放程式可以使用自己的 Xbox Live 帳戶，統計資料和登入。 您的遊戲也可以啟用來賓，取得沒有要播放的 Xbox Live 帳戶的使用者。 在 xbox 上才支援這項功能。

本教學課程將逐步引導您了解如何將多使用者支援新增至不同的 Xbox Live Prefabs。

> [!IMPORTANT]
> 在 xbox 上才支援多使用者案例。 這項功能不適用於電腦。

## <a name="prerequisites"></a>必要條件
您必須遵循[開始使用](configure-xbox-live-in-unity.md)教學課程，以啟用及設定 Xbox Live。

## <a name="adding-and-signing-in-multiple-xbox-live-users"></a>新增並登入 Xbox Live 的多個使用者

1. 從**資產** > **Xbox Live** > **Prefabs**資料夾中，拖曳到多個場景**XboxLiveUser**因為 prefab 的執行個體將會播放程式。 本教學課程中，將會因此兩個兩名玩家**XboxLiveUser**會加入至場景的執行個體。 我們會以其作為**XboxLiveUser**並**XboxLiveUser2**為了方便起見。

2. 若要正確新增使用者並登入，新增兩個執行個體**UserProfile** prefab 至場景，一個用於每個**XboxLiveUser**。 請確定您具有執行個體的`XboxLiveServices`prefab 場景上。 此外，請確定將這兩個**UserProfile**相隔場景方向，以辨別這兩者上的物件。 由於這些 prefabs 使用 Unity Eventsystem，請確定您具有執行個體`EventSystem`場景上。

    ![階層的多使用者支援，在 Xbox Live Unity 外掛程式教學課程 」 專案](../images/unity/MUA-Tutorial-Hierarchy.png)

    ![遊戲的 Scene of Xbox Live 的 Unity 外掛程式教學課程專案中的多使用者支援](../images/unity/MUA-Tutorial-GameScene.png)

3. 指派的一個執行個體**XboxLiveUser** prefabs 您擁有的每個場景**UserProfile**物件。

    ![UserProfile prefab 的多使用者支援](../images/unity/user-profile-for-mua.png)

4. Xbox One 裝置上只支援多使用者應用程式，因為新增到的控制器支援**UserProfile**物件是必要。 在每一**UserProfile**物件，有一個欄位，稱為`InputControllerButton`其中您可以指定搖桿和按鈕的數字每**UserProfile**應接聽。

本教學課程中，我們將使用`joystick 1 button 0`for **UserProfile**玩家 1 指派給並`joystick 2 button 0`Player 2 和第二個**UserProfile**遊戲物件。 這會將指派`A`按鈕互動**UserProfile**的兩個控制站。

> [!Note]
> 如需進一步瞭解 Xbox Live Unity 外掛程式中的 Xbox 控制器支援的詳細資訊，請參閱：[將控制器支援新增至 Xbox Live Prefabs](add-controller-support-to-xbox-live-prefabs.md)

5. 在編輯器中執行的場景，並已正確設定每個按鈕，以確定所有項目上執行的叫用。

    ![在 Unity 編輯器中測試多使用者支援](../images/unity/run-example-mua.png)

## <a name="building-and-testing-the-uwp"></a>建置和測試 UWP

1. 下列步驟概述之後[使用 Unity 開發 Creators 標題](configure-xbox-live-in-unity.md)教學課程中，在 Visual Studio 中開啟匯出的 UWP 方案。

2. 在您的遊戲的 UWP 專案，以滑鼠右鍵按一下`package.appxmanifest.xml`檔案，然後選擇**檢視程式碼**。

3. 在下`<Properties></Properties>`區段中，新增下列可讓應用程式的多使用者輸入： `<uap:SupportedUsers>multiple</uap:SupportedUsers>`

4. 若要在 Xbox 上進行測試，請遵循文件，網址[設定您在 Xbox 開發環境上的 UWP](https://docs.microsoft.com/en-us/windows/uwp/xbox-apps/development-environment-setup)教學課程。

## <a name="using-the-other-xbox-live-prefabs-with-multiple-users"></a>使用多個使用者的其他 Xbox Live Prefabs

在 **範例**資料夾的外掛程式，還有**MultiUserExample**場景，會顯示如何使用不同的 prefabs **XboxLiveUser**顯示特定的執行個體每個使用者的資訊。
