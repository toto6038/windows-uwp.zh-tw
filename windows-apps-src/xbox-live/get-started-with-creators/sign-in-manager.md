---
title: 使用 unity SignInManager 登入
description: Unity 外掛程式登入管理員的概觀
ms.date: 05/08/2018
ms.topic: get-started-article
keywords: xbox live、 xbox、 遊戲、 uwp、 windows 10，其中一個 xbox、 unity
ms.openlocfilehash: e6d066fe7792912f8918cb139d45ff05d105feaa
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57641723"
---
# <a name="scripting-sign-in"></a>登入指令碼

若要將登入新增至您自己自訂的遊戲物件，您必須編寫到 GameObject。 讓我們假設 PlayerAuthentication prefab 容納不下您的遊戲和您想要有自己的登入面板，這篇文章會引導您將登入邏輯新增至您的標題的基本步驟。

## <a name="sign-in-with-the-signinmanager"></a>使用 SignInManager 登入

Xbox Live Unity 外掛程式包含的指令碼`SignInManager`下的檔案路徑**資產 >> XboxLive >> 指令碼 >> SignInManager.cs**。 「 管理員 」 的單一類別可以呼叫表單任何位置中您的標題所指的項目的*執行個體*的`SignInManager`。 這*執行個體*不會不需要初始化，您可以使用 it 遊戲開始時立即。 您可以存取所有公用屬性和函式的參考*執行個體*做為`SignInManager.Instance`。

`SignInManager`包含所有必要的程式碼，用於管理您的標題的驗證，包括登入、 登出，以及取得使用者的相關資訊的播放程式以登入。

### <a name="calls-and-results"></a>呼叫和結果

`SignInManager`有三個非同步共同常式函式`SignInPlayer(int playerNumber)`， `SignOutPlayer(int playerNumber)`，和`SwitchUser(int playerNumber)`，該觸發程序事件的函式收集呼叫的結果，並採取適當動作。 您可以將對應的函式新增至您的指令碼，並將它們指派給`SignInManager.Instance`的回撥清單。 事件的函式`OnPlayerSignIn(int playerNumber, UnityAction<XboxLiveUser, XboxLiveAuthStatus, string> callback)`， `OnPlayerSignOut(int playerNumber, UnityAction<XboxLiveUser, XboxLiveAuthStatus, string> callback)`， `OnAnyPlayerSignIn(UnityAction<XboxLiveUser, XboxLiveAuthStatus, string> callback)`，和`OnAnyPlayerSignOut(UnityAction<XboxLiveUser, XboxLiveAuthStatus, string> callback)`。 每個上事件函式會接聽其名稱中所述的事件。 您可以加入自己的函式的管理員在回撥清單中項目的`Start()`為下列程式碼的函式。

```csharp
using Microsoft.Xbox.Services;
using Microsoft.Xbox.Services.Client;

void Start () {
    try
    {
        SignInManager.Instance.OnPlayerSignOut(this.playerNumber, this.OnPlayerSignOut);
        SignInManager.Instance.OnPlayerSignIn(this.playerNumber, this.OnPlayerSignIn);
    }
    catch (Exception ex)
    {
        Debug.LogWarning(ex.Message);
    }

}
```

此程式碼片段會新增此 GameObject playerNumber 相關聯的播放程式的登入和登出的接聽程式。 此 GameObject`OnPlayerSignIn`函式時將會呼叫`SignInManager`偵測到的登入嘗試已完成且其`OnPlayerSignOut`SignInManager 偵測到登出時，就會呼叫函式。傳回的型別和參數，以符合 SignInManager 所呼叫的函式類型，必須具有事件中的函式程式 GameObject。 同時`OnPlayerSignIn`並`OnPlayerSignOut`是 void 函式需要`XboxLiveUser`， `XboxLiveAuthStatus`，並為其參數的字串。 您的函式的殼層可能如下所示：

```csharp
using Microsoft.Xbox.Services;
using Microsoft.Xbox.Services.Client;

private void OnPlayerSignIn(XboxLiveUser xboxLiveUserParam, XboxLiveAuthStatus authStatus, string errorMessage)
{
}

private void OnPlayerSignOut(XboxLiveUser xboxLiveUserParam, XboxLiveAuthStatus authStatus, string errorMessage)
{
}
```

在這兩個函式檢查`XboxLiveAuthStatus`，確定您呼叫`SignInManager.Instance`已順利完成。 在成功呼叫`XboxLiveUser`將會`XboxLiveUser`，，已登入我們放大`SignInManager`。 當呼叫不成功`errorMessage`字串將會包含失敗原因的詳細資訊。

新增數行程式碼來檢查有成功的呼叫會導致程式碼，如下所示：

```csharp
using Microsoft.Xbox.Services;
using Microsoft.Xbox.Services.Client;

private void OnPlayerSignIn(XboxLiveUser xboxLiveUserParam, XboxLiveAuthStatus authStatus, string errorMessage)
{
    if(authStatus == XboxLiveAuthStatus.Succeeded)
    {
        this.xboxLiveUser = xboxLiveUserParam; //store the xboxLiveUser SignedIn
        this.signedIn = true;
    }
    else
    {
        if (XboxLiveServicesSettings.Instance.DebugLogsOn)
        {
            Debug.LogError(errorMessage); //Log the error message in case of unsuccessful call. 
        }
    }
}

private void OnPlayerSignOut(XboxLiveUser xboxLiveUserParam, XboxLiveAuthStatus authStatus, string errorMessage)
{
    if (authStatus == XboxLiveAuthStatus.Succeeded)
    {
        this.xboxLiveUser = null;
        this.signedIn = false;
    }
    else
    {
        if (XboxLiveServicesSettings.Instance.DebugLogsOn)
        {
            Debug.LogError(errorMessage);
        }
    }
}
```

藉由呼叫登入，並擷取結果產生的事件，您可以處理登入和登出您的標題。

## <a name="get-signed-in-player-information"></a>登入播放器資訊

除了登入服務的玩家 SignInManager 會追蹤的所有登入的使用者。 會在電腦上這受限於單一登入播放程式，並在 Xbox 上是限制為 16。 您可以檢查如何接近限制您是否藉由比較的結果`SignInManager.Instance.GetCurrentNumberOfPlayers()`結果`SignInManager.Instance.GetMaximumNumberOfPlayers()`。 SignInManager 已登入的玩家以該播放程式編製索引的字典*playerNumber*。 您可以使用這個可存取播放程式的一些基本資訊擷取其相關聯`XboxLiveUser`。

```csharp
if (SignInManager.Instance.GetPlayer(this.playerNumber).IsSignedIn) // If there is a player signed in for this gameObjects player number
            {
                this.displayedGamertag = SignInManager.Instance.GetPlayer(this.playerNumber).Gamertag; // Set that users gamertag to the gamertag displayed
            }
```

這一小段的程式碼會檢查以查看是否有針對此 GameObject 登入的 player 的數字位置播放程式，並將儲存登入若要顯示該使用者玩家代號。 雖然`XboxLiveUser`包含帶正負號中使用者的玩家代號和 Xbox 使用者識別碼 (xuid)，之後您必須呼叫其他服務，例如`SocialManager`gamerpic 等玩家分數的存取資訊。

## <a name="destroying-your-sign-in-gameobject"></a>終結您登入 GameObject

當使用其中一個的 Xbox Live 外掛程式管理員的遊戲物件的終結就像`SignInManager`或`SocialManager`，通常是當載入新場景，務必要移除任何函式新增至管理員 的事件接聽程式的清單。 在本文的範例程式碼中，我們需要移除的函式，我們將新增至登入和登出的事件接聽程式。我們會移除這些函式，從`SignInManager`在`OnDestroy()`我們 GameObject 函式。

```csharp
private void OnDestroy()
{
    if (SignInManager.Instance != null)
    {
        SignInManager.Instance.RemoveCallbackFromPlayer(this.PlayerNumber, this.OnPlayerSignOut);
        SignInManager.Instance.RemoveCallbackFromPlayer(this.PlayerNumber, this.OnPlayerSignIn);
    }
```

這段程式碼將會移除此 GameObject 相關聯的播放程式的登入和登出的回呼函式。

## <a name="testing-you-code-in-visual-studio"></a>Visual Studio 中測試您的程式碼

除了[建置 Visual Studio 中的遊戲所需的步驟](configure-xbox-live-in-unity.md#build-and-test-the-project)中所列[設定 Unity 在 Xbox Live 標題](configure-xbox-live-in-unity.md)文章，還有額外的步驟，才能測試您在正確的遊戲Visual Studio。 您必須更新 package.appxmanifest.xml 檔案的屬性。 請這樣做：

1. 搜尋方案總管，package.appxmanifest.xml 檔案
2. 以滑鼠右鍵按一下檔案，然後選擇 檢視程式碼
3. 底下`<Properties><\/Properties>`區段中，新增下面這行: ' < uap:SupportedUsers > 多個 <\/uap:SupportedUsers >。
4. 部署至您的 Xbox 的遊戲，藉由從 Visual Studio 中啟動遠端偵錯組建。 您可以找到指示來設定您在 Xbox 上的標題[設定您在 Xbox 開發環境上的 UWP](../../xbox-apps/development-environment-setup.md)文章。

> [!NOTE]
> 組態已變更的部分可能看起來像它讓多人，但仍然必須在單一播放器案例中執行您的遊戲。

## <a name="policies-and-limitations"></a>原則和限制

有幾個登入原則和項目的，您可能要考慮開發您的登入使用經驗時的限制。

- 項目的初始登入之後，您必須保持登入的至少一名玩家。 `SignInManager`會擲回錯誤，並會放棄通話，如果您嘗試登入使用者的最新的登入。 務必也請注意，您無法呼叫`SignInManager.Instance.SwitchUser(int playerNumber)`上最後一個登入播放程式會嘗試登出再登入新的播放程式的播放程式。

- 電腦可以只有登入一位使用者一次，主控台可能會一次登入最多 16 個播放器。

- 標題實際上沒有登出 OS 從播放程式的權限，因為此 SignOut 可能無法如預期運作。 SignInManager in 和 out 而言標題，使用者可以登，但無法登出任何人都從標題會部署到電腦。

- 多個使用者登入時才可用在一個 Xbox 主機上。

- 來賓帳戶不適用於 Xbox Live 創作者計劃標題。